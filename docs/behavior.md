# Chat Application – Behavioral Specification

This document defines the **behavioral rules** of the chat application.
It intentionally avoids implementation, architecture, or technology details.
The goal is to clearly describe **how the system behaves from a user and system perspective**.

---

## 1. User Identity & Authentication

### 1.1 User Identity
- Every user has a stable, internal `userId` that never changes.
- `userId` is the primary identifier used internally across the system.
- Usernames are mutable and used only for display purposes.

### 1.2 Mobile Number
- Mobile number is mandatory at signup.
- Mobile number is:
  - Used for user discovery
  - Used for primary authentication (OTP-based)
- Mobile number can be changed after verification.

### 1.3 Email
- Email is **optional** at signup.
- Email can be added later for recovery and reliability.
- When email is added, it must be verified via OTP.
- Email is:
  - Used as a secondary OTP delivery channel
  - Used for account recovery
- Email is **not searchable** by other users.

### 1.4 Login Behavior
- Login is OTP-based.
- OTP is sent to:
  - Mobile number (always)
  - Email (only if email is added and verified)
- The same OTP may be delivered via both channels.
- If a user does not add an email, they accept the risk of mobile-only access.

---

## 2. User Discovery

- Any logged-in user can search for another user using **exact-match mobile number**.
- If the user exists:
  - They appear in search results
  - Their public profile can be viewed
- There are no message requests or approval flows.

---

## 3. Private Chats

### 3.1 Private Chat Definition
- A private chat exists between **exactly two users**.
- There can be **only one private chat per user pair**.

### 3.2 Chat Creation
- Opening a chat UI does **not** create a chat.
- A private chat is created **only when the first message is sent**.
- If no message is ever sent, no chat exists.

### 3.3 First Message Flow
1. User A searches User B and opens chat UI.
2. User A sends the first message.
3. The system:
   - Creates a private chat
   - Stores the message
   - Delivers the message to User B
4. User B:
   - Receives a notification
   - Sees the new private chat
   - Can reply immediately

### 3.4 Ongoing Messaging
- Once created, both users can freely message each other.
- Messages are delivered in real time.

---

## 4. Blocking Behavior (Silent Block)

### 4.1 Blocking Rules
- Either user in a private chat can block the other.
- Blocking is **asymmetric**.
- Blocking applies **only to private chats**.

### 4.2 Behavior After Blocking
When User A blocks User B:

**User A**
- Does not receive messages from User B.
- Receives no notifications from User B.
- Chat remains visible and marked as blocked.

**User B**
- Can send messages.
- Messages appear sent on the client.
- Receives no error or notification.
- Messages are silently dropped server-side.

### 4.3 Message Handling
- Messages sent during a block:
  - Are **not stored**
  - Are **not delivered**
  - Are **never delivered later**, even if unblocked

### 4.4 Unblocking
- When User A unblocks User B:
  - Normal messaging resumes
  - Messages sent during the block are permanently lost

---

## 5. Groups

### 5.1 Group Creation
- Any user can create a group.
- Minimum group size is **2 users**.
- A group exists immediately upon creation, even before any messages are sent.

### 5.2 Group Types

#### 5.2.1 Normal Group
- One or more admins exist.
- Admin powers:
  - Add users
  - Remove users
  - Assign admin role
  - Edit group profile

**Admin Rules**
- Multiple admins are allowed.
- There is always at least one admin.
- If the last admin leaves:
  - Admin rights transfer to the next-joined member.

---

#### 5.2.2 Democratic Group
- All members have equal privileges.
- No permanent admin hierarchy.
- Polling is required **only for removing users**.
- All other actions (adding users, editing profile) do not require polling.

---

## 6. Group Membership & Messaging

### 6.1 Adding Users
- Users can be added to groups without their consent.

### 6.2 Messaging
- All group members receive group messages.
- Blocking does **not** apply inside groups.
- Even if two users have blocked each other privately:
  - They still receive each other’s messages in shared groups.

---

## 7. Polling (Democratic Groups Only)

- Polling is used **only** for removing a user from the group.
- Polling is group-wide.
- No polling is used for:
  - Adding users
  - Editing group profile
  - Messaging

---

## 8. Scope Notes

- Voice and video calling are out of scope.
- Message requests and spam prevention are out of scope.
- Account recovery beyond email fallback is out of scope.
- Moderation and reporting are out of scope.

These decisions are intentional to keep the system:

- Low-cost
- Serverless-friendly
- Focused on real-time messaging behavior
