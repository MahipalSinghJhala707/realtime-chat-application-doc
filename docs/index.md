# Chat Application – Overview

## Purpose
This project is a real-time chat application built as a **learning-focused, production-inspired system**.
The primary goal is to understand and implement real-world backend concepts such as:

- Real-time messaging
- End-to-end encryption
- Authorization rules
- Group governance
- Low-cost, serverless-friendly design

This is not intended to be a feature-complete WhatsApp or Telegram clone.

---

## Target Users
- A small number of authenticated users (personal use, friends, known contacts)
- Users are identified primarily by mobile number
- The system is not designed for public or anonymous usage

---

## Core Principles
- **Behavior-first design**: system behavior is defined before implementation
- **Privacy-first**: messages are end-to-end encrypted
- **Low operational cost**: suitable for long-term personal hosting
- **Minimal scope**: only essential features are included
- **Real-world realism**: trade-offs are intentional and documented

---

## What the System Does
- Enables private one-to-one messaging
- Supports group messaging with two group models:
  - Admin-based groups
  - Democratic groups with polling for user removal
- Allows silent blocking in private chats
- Encrypts all messages end-to-end
- Uses mobile number–based discovery

---

## What the System Does NOT Do
- Voice or video calling
- Public user discovery
- Message requests or spam prevention
- Content moderation
- Server-side message inspection
- Backup or cross-device message recovery

---

## Constraints
- Serverless-first mindset
- Minimal ongoing cost
- Small, controlled user base
- No dependence on server-side message content

---

## Scope Philosophy
This project intentionally prioritizes **clarity and correctness of behavior** over feature breadth.
Some real-world complexities are deferred or excluded to keep the system understandable and maintainable.
