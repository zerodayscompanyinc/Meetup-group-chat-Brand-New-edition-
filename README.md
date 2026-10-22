# ZeroDay Meet Chat v4 — Complete Guide & Documentation

> **Discord-grade P2P chat with end-to-end encryption.** Reactions, replies, voice & video calls, slash commands, QR scanner, history export — all in one HTML file. No IP, no port, no API key. Works in 195 countries via WebRTC mesh + MQTT signaling.

A single-file HTML chat application that delivers Discord-class messaging features with hacker-resistant end-to-end encryption. Three chat modes (private 1-on-1, group mesh, anonymous stranger match), full markdown support, voice messages, polls, scheduled messages, and a complete command palette — all running entirely in your browser with zero backend.

---

# link URL here:
# click now 

https://meetupch0p.vercel.app/

---

## Table of Contents

1. [Quick Start — 60 seconds to your first encrypted message](#1-quick-start--60-seconds-to-your-first-encrypted-message)
2. [Tutorial — Walk through every feature](#2-tutorial--walk-through-every-feature)
3. [User Guide — How to use each part of the app](#3-user-guide--how-to-use-each-part-of-the-app)
4. [Architecture Overview](#4-architecture-overview)
5. [Feature Reference](#5-feature-reference)
6. [Keyboard Shortcuts & Commands](#6-keyboard-shortcuts--commands)
7. [Security & Privacy](#7-security--privacy)
8. [Technical Implementation](#8-technical-implementation)
9. [Use Cases](#9-use-cases)
10. [Limitations & Troubleshooting](#10-limitations--troubleshooting)
11. [Extending ZeroDay Meet Chat](#11-extending-zeroday-meet-chat)
12. [Appendix](#12-appendix)

---

## 1. Quick Start — 60 seconds to your first encrypted message

### Six-step quickstart

1. **Open the HTML file.** Save the file anywhere (USB stick, GitHub Pages, your own server, even a local `file://` open) and open it in any modern browser (Chrome 90+, Firefox 90+, Safari 15+).
2. **Wait for the boot screen.** The page loads in ~1 second. A crypto keypair is generated client-side and your E2E fingerprint appears in the settings drawer.
3. **Pick a chat mode.** Click **Start Private Chat**, **Create Group**, **Find a Stranger**, or **Enter Code or Scan QR**.
4. **Set your identity.** A modal pops up asking for your display name and avatar letter. Accept the suggestion, type your own, or click Generate.
5. **Share the invite code.** You'll receive a 9-character code like `A4F-9X2-K7P`. Share it via any channel — the code is also rendered as a QR code you can scan with a phone camera.
6. **Start chatting.** When your peer joins (via the code or QR), the connection establishes and you can send messages. Everything is end-to-end encrypted by default — there's no way to turn it off.

### What happens automatically

- **ECDH keypair generation** — every session generates a fresh P-256 keypair.
- **Fingerprint computation** — SHA-256 of your public key, shown as hex groups for out-of-band verification.
- **MQTT signaling** — connects to `wss://broker.hivemq.com:8884/mqtt` (free public broker, no API key).
- **WebRTC mesh** — uses Google/Cloudflare STUN + Open Relay TURN servers for NAT traversal.
- **IndexedDB setup** — conversation history store opens automatically (encrypted at rest).
- **CSP enforcement** — Content-Security-Policy blocks inline event handlers and untrusted script sources.

---

## 2. Tutorial — Walk through every feature

This tutorial covers every interactive surface of the app, in the order you'd naturally encounter them.

### Tutorial 2.1 — The home screen

When you open ZeroDay Meet Chat, you land on the home view:

```
┌──────────────────────────────────────────────────────────┐
│ [0D ZeroDay Meet v4 · Full Messenger · E2E] [🔒 E2E]    │
│                                            [Idle] [🔍] [⚙️]│
├──────────────────────────────────────────────────────────┤
│                                                            │
│              ZeroDay Meet Chat v3                          │
│   Discord-grade P2P chat with end-to-end encryption.       │
│   Reactions, replies, slash commands, QR scanner...         │
│                                                            │
│  [E2E Encrypted] [Anti-XSS] [Anti-Replay] [HMAC Tamper]    │
│  [Voice & Video] [Group Mesh] [Stranger Match] [History]   │
│                                                            │
│  ┌──────────────────────────────────────────────────────┐ │
│  │ 🔒 Security Layer — Hacker-Resistant                  │ │
│  │ • End-to-end encryption: AES-GCM 256 + ECDH P-256    │ │
│  │ • Anti-replay: nonce + timestamp validation            │ │
│  │ • Anti-tamper: HMAC-SHA256 over ciphertext             │ │
│  │ • Anti-XSS: tokenized + HTML-escaped                  │ │
│  │ • CSP-locked                                          │ │
│  │ • Key fingerprint verification                        │ │
│  └──────────────────────────────────────────────────────┘ │
│                                                            │
│  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐         │
│  │ Private │ │ Group   │ │ Match   │ │ Join    │         │
│  │ Chat    │ │ Room    │ │ Stranger│ │ Code    │         │
│  └─────────┘ └─────────┘ └─────────┘ └─────────┘         │
│                                                            │
│  ┌─────────┐ ┌─────────┐                                 │
│  │ Saved   │ │ Clear   │                                 │
│  │ History │ │ All Data│                                 │
│  └─────────┘ └─────────┘                                 │
└──────────────────────────────────────────────────────────┘
```

Six cards let you pick a chat mode or action. Click any card to begin.

### Tutorial 2.2 — The top bar

The persistent top bar contains:

| Element | What it does |
|---|---|
| **0D ZeroDay Meet logo** | Visual branding only. |
| **🔒 E2E badge** | Indicates end-to-end encryption is active. Hover to see the peer's fingerprint after keys are exchanged. |
| **Idle / Searching / Connected pill** | Real-time connection status. |
| **🔍 Command palette button** | Opens the Ctrl+K command palette. |
| **⚙️ Settings button** | Opens the personalization drawer. |

**Try it:** Click 🔍 in the top bar. A search-style palette appears with all commands and quick actions. Type `/me` to filter to slash commands. Press `Esc` to close.

### Tutorial 2.3 — Starting a private chat

1. Click **Start Private Chat** on the home screen.
2. A modal asks for your display name and avatar letter.
3. Click **Generate Invite Code**.
4. A crypto keypair is generated; your fingerprint appears.
5. The Invite modal shows:
   - The 9-character invite code (e.g. `A4F-9X2-K7P`)
   - A shareable link: `https://your-host/#join=A4F-9X2-K7P`
   - A QR code (180×180 PNG)
   - Your E2E fingerprint (8-character groups for easy comparison)
6. Share the code, link, or QR with your peer.
7. When they join via the same code, the WebRTC connection establishes.

**Try it:** Generate a code, then open the share link in another browser tab. Both tabs will detect each other via MQTT signaling and establish a direct P2P tunnel.

### Tutorial 2.4 — The chat header

Once connected, the chat view shows:

```
[←] [avatar] Peer Name · Online · E2E encrypted                [📞] [📹] [🔇] [⏱️] [⭐] [🔍] [📌] [📋] [💾] [🗑️]
```

| Button | Action |
|---|---|
| ← Back | Leave the chat and return home |
| Avatar + Name + Status | Peer info; click avatar to see profile + fingerprint |
| 📞 Voice call | Start a voice-only WebRTC call |
| 📹 Video call | Start a video WebRTC call |
| 🔇 Mute notifications | Suppress sound effects for incoming messages |
| ⏱️ Disappearing messages | Set message TTL (30s/5m/1h/24h/7d/off) |
| ⭐ Starred messages | View starred messages |
| 🔍 Search messages | Open the search side panel |
| 📌 Pinned messages | Open the pinned messages side panel |
| 📋 Copy invite code | Copy the invite code to clipboard |
| 💾 Export conversation | Download as JSON or TXT |
| 🗑️ Clear conversation | Clear local/both/history |

### Tutorial 2.5 — Sending messages

Type in the bottom text box. As you type:

- The text box **auto-grows** up to 200px tall.
- A **typing indicator** appears at the bottom of the message list ("peer is typing…").
- Press `Enter` to send. `Shift+Enter` for a newline.
- A **sound effect** plays when a message arrives (toggleable in settings).

After sending:

- Your message appears right-aligned with your avatar letter and a gradient bubble.
- The message is encrypted with AES-GCM 256 before being sent over the WebRTC data channel.
- An HMAC-SHA256 tag is attached for tamper detection.
- A unique nonce + timestamp prevents replay attacks.

### Tutorial 2.6 — Markdown & formatting

ZeroDay Meet Chat supports a Discord-style markdown subset:

| You type | What renders |
|---|---|
| `` `inline code` `` | `inline code` (monospace, dark background) |
| ` ```lang\n...``` ` | Multi-line code block with language label |
| `**bold**` or `__bold__` | **bold** |
| `*italic*` or `_italic_` | *italic* |
| `~~strikethrough~~` | ~~strikethrough~~ |
| `\|\|spoiler\|\|` | <span style="background:#000;color:#000">spoiler</span> (click to reveal) |
| `@user` | Highlighted mention |
| `#channel` | Channel reference |
| `[text](url)` | Clickable link |
| `https://example.com` | Auto-linked bare URL |
| `> quote` | Blockquote |
| `- item` | Unordered list item |

**Try it:** Send this message:
```
Check out this **code**:
```js
function hello() { return "world"; }
```
||Spoiler: don't peek||
```

### Tutorial 2.7 — The composer toolbar

Above the text box, a toolbar offers markdown shortcuts:

| Button | Inserts |
|---|---|
| **B** | `**bold**` |
| ***I*** | `*italic*` |
| ~~S~~ | `~~strikethrough~~` |
| `< >` | `` `inline code` `` |
| `[ ]` | ` ```code block``` ` |
| ❝ | `> quote` |
| 👁 | `\|\|spoiler\|\|` |
| 😊 | Opens emoji picker |

Select text first to wrap it. Without selection, the cursor is placed between the markers.

### Tutorial 2.8 — Mentions

Typing `@` followed by a username triggers a mention. The mention renders with a highlighted background.

**Try it:** In a group chat, type `@alex` — the username gets highlighted and the recipient sees a notification-style mention.

### Tutorial 2.9 — Emoji picker

Click 😊 in the composer toolbar. A 320×340 picker appears with:

- **Search box** at top — type "heart" to filter to ❤️, 💕, 💗, etc.
- **Category tabs** — 😀 Smileys, 👍 Gestures, ❤️ Hearts, 🎉 Events, 🔥 Symbols
- **8-column grid** — click any emoji to insert at cursor

Press `Esc` or click outside to close.

### Tutorial 2.10 — Reactions

Hover over any message. A row of **quick reactions** appears above it:

```
👍 ❤️ 😂 😮 😢 🙏
```

Click one to react. Click again to remove your reaction. Reaction counts appear below the message bubble with a highlighted background for reactions you've added.

You can also click the 😊 icon in the hover actions to open the full emoji picker in reaction mode.

### Tutorial 2.11 — Replying to messages

Hover over a message and click the ↩️ reply button. A reply bar appears above the composer:

```
↩ Replying to Alex: Hey everyone…
```

Type your reply and send. Your message appears with a quoted reference to the original at the top.

Click the quote to scroll to the original message (it briefly highlights in yellow).

### Tutorial 2.12 — Editing messages

Hover over **your own** message and click the ✏️ edit button. The composer populates with the original text and a hint appears:

```
Editing message… Press Enter to save · Esc to cancel
```

Press Enter to save the edit. The message shows `(edited)` next to the timestamp. Edits propagate to all peers.

### Tutorial 2.13 — Deleting messages

Hover over **your own** message and click the 🗑️ delete button. Confirm the prompt. The message becomes italic gray with "Message deleted". Deletions propagate to all peers.

### Tutorial 2.14 — Pinning messages

Hover over any message and click the 📌 pin button. The message is added to the pinned list. Click 📌 in the chat header to open the pinned panel and see all pinned messages in one place.

Click the pin button again to unpin.

### Tutorial 2.15 — Starring messages

Hover over any message and click the ⭐ star button (added in v4). The message is added to your starred list — a yellow ⭐ badge appears next to the timestamp.

Click ⭐ in the chat header to view all starred messages as a system message listing.

### Tutorial 2.16 — Forwarding messages

Hover over any message and click the ➡️ forward button (added in v4). The Forward modal appears:

1. Enter the target invite code.
2. Preview the message content.
3. Click **Forward**.

The message body is copied to your clipboard with instructions to open the target chat and paste. (Full peer-to-peer forwarding would require opening a new peer connection — for now it's a copy-and-paste bridge.)

### Tutorial 2.17 — Message info

Hover over any message and click the ℹ️ info button (added in v4). The Message Info modal shows:

- Sent timestamp
- Author name
- Status: ✓ Delivered or ✓✓ Read
- Read timestamp (if read)
- Edited flag
- Pinned/Starred status
- Message ID

### Tutorial 2.18 — Voice messages

Click the 🎤 microphone button next to the composer. A red recording bar appears:

```
🔴 0:14  [Cancel] [Send]
```

- **Cancel** — discards the recording
- **Send** — stops and sends the voice message as a WebM audio blob

The recipient sees a voice message bubble with play button, waveform visualization, and duration.

**Try it:** Click 🎤, speak for 5 seconds, click Send. Your voice message appears in chat with a playable waveform.

### Tutorial 2.19 — Polls

Type `/poll` or use the command palette. The Poll modal appears:

1. Enter a question (e.g. "What's your favorite color?")
2. Enter options (one per line, max 6 — e.g. `Red`, `Green`, `Blue`)
3. Click **Create Poll**

The poll appears as a special message bubble with:
- Question at top
- Each option as a clickable row with a progress bar showing vote percentage
- Vote count per option
- Total votes at bottom

Click an option to vote. You can change your vote — clicking a different option moves your vote.

### Tutorial 2.20 — Scheduled messages

Type `/schedule` or use the command palette. The Schedule modal appears:

1. Enter your message text.
2. Pick a date and time (datetime-local input).
3. Click **Schedule**.

The message is queued locally and sent automatically at the scheduled time. A system message confirms: "⏰ Message scheduled for Oct 25, 3:30 PM".

### Tutorial 2.21 — Disappearing messages

Click ⏱️ in the chat header. Choose a TTL:

- Off
- 30 seconds
- 5 minutes
- 1 hour
- 24 hours
- 7 days

Once enabled, all messages (yours and incoming) auto-delete after the TTL expires. A yellow ⏱️ indicator appears in the composer hint.

The setting is broadcast to your peer so both sides apply the same TTL.

### Tutorial 2.22 — Voice & video calls

Click 📞 (voice) or 📹 (video) in the chat header:

1. Browser requests microphone (and camera) permission.
2. A WebRTC call offer is sent to your peer.
3. Their device shows an incoming call modal: "Accept" or "Reject".
4. On accept, a full-screen call overlay appears:
   - Remote video fills the screen (or black background for voice-only)
   - Local video shows as a 120×160 thumbnail in the corner
   - Call controls at the bottom: mute audio, mute video (video calls only), hang up

Click the red hangup button to end the call. A system message logs the call in chat history.

### Tutorial 2.23 — Stranger match

1. Click **Find a Stranger** on the home screen.
2. Set your display name + avatar.
3. Pick match mode:
   - ⚡ **Auto** — instant match with the next available stranger
   - ✋ **Manual** — pick from a list of waiting strangers
4. (Optional) Enter an interest/topic to match on (e.g. "music", "gaming", "philosophy")
5. Click **Start Matching**.

A spinner appears with "Searching for a stranger…". When matched, the chat connects automatically. Use the ➡️ Next button in the header to skip to another stranger.

### Tutorial 2.24 — Group chat

1. Click **Create Group** on the home screen.
2. Set your display name + avatar.
3. Enter a group name (e.g. "Family Chat").
4. Click **Create Group**.

You receive a `GRP-XXXX-XXXX` invite code. Share it with anyone. When peers join, a mesh network forms:

- Each peer connects to every other peer via WebRTC
- Messages broadcast to all connected peers
- Peers re-broadcast incoming messages to others they're connected to
- No single point of failure — the mesh stays alive as long as any two peers are connected

The member list sidebar shows all participants with their online status.

### Tutorial 2.25 — Joining via QR scan

1. Click **Enter Code or Scan QR** on the home screen.
2. Click **Scan QR**.
3. Browser requests camera permission.
4. Point your camera at the invite QR code.
5. The code is parsed automatically and the join modal populates.
6. Click **Connect Now**.

The QR scanner uses `jsQR` library to decode the camera feed at 60fps until a code is detected.

### Tutorial 2.26 — Command palette (Ctrl+K)

Press `Ctrl+K` (or `Cmd+K` on Mac) anywhere in the app. A palette appears at the top center:

- **Search box** — type to filter
- **Results list** — slash commands + quick actions
- **Arrow keys** to navigate
- **Enter** to execute
- **Esc** to close

Items include:
- All slash commands (`/me`, `/shrug`, `/poll`, `/schedule`, etc.)
- Toggle theme
- Open settings
- Export conversation
- Start voice/video call
- Search messages
- View pinned/starred
- Show fingerprint
- Leave chat

### Tutorial 2.27 — Slash commands

Type `/` in the composer to begin a command. Available commands:

| Command | Action |
|---|---|
| `/me <action>` | Send an action message (italicized) |
| `/shrug [text]` | Append `¯\_(ツ)_/¯` to your message |
| `/clear` | Clear conversation locally |
| `/pin <id>` | Toggle pin on a message |
| `/search <query>` | Search messages |
| `/export [json\|txt]` | Export conversation |
| `/topic <text>` | Set room topic (group only) |
| `/nick <name>` | Change your nickname |
| `/status <text>` | Set your custom status |
| `/online` / `/idle` / `/dnd` / `/invisible` | Set online status |
| `/fingerprint` | Show your E2E fingerprint |
| `/help` | List all commands |
| `/call` | Start voice call |
| `/video` | Start video call |
| `/next` | Find next stranger (stranger mode) |
| `/leave` | Leave current chat |
| `/voice` | Record voice message |
| `/poll` | Create a poll |
| `/schedule` | Schedule a message |
| `/forward` | Forward last message |
| `/star <id>` | Star a message by ID |
| `/mute` | Toggle mute |
| `/disappear [seconds]` | Set disappearing messages |
| `/info <id>` | Show message info |

### Tutorial 2.28 — Settings drawer

Click ⚙️ to open the personalization drawer:

#### Your Identity
- Display name (max 32 chars)
- Avatar letter (auto-derived from name)
- Custom status (max 48 chars)

#### Online Status
- 🟢 Online
- 🟡 Idle
- 🔴 Do Not Disturb
- ⚫ Invisible

#### Theme (5 themes)
- 🌌 **Midnight** — Discord-inspired indigo (#5865f2)
- 🔥 **Crimson** — red/orange (#ed4245)
- 💚 **Matrix** — green monospace (#00ff8c)
- 🌅 **Sunset** — coral/peach (#ff7a59)
- ❄️ **Arctic** — light mode (#3b5bff)

#### UI Scale
- S (0.85×), M (1×), L (1.15×), XL (1.35×)
- Auto-detected based on screen size, DPI

#### Behavior toggles
- Read receipts
- Typing indicator
- Sound effects
- Enter to send
- Auto-resume last session
- Save history (encrypted at rest)
- Reduce motion

#### Security
- Your crypto fingerprint (SHA-256 of public key, 8-char groups)

#### Danger Zone
- Clear local data & reset

### Tutorial 2.29 — Verifying your peer's identity

After connecting, both peers exchange public keys. To verify no man-in-the-middle attack is happening:

1. Open your settings drawer (⚙️) — see your fingerprint.
2. Ask your peer (via voice call or out-of-band channel) to read their fingerprint.
3. Compare the two — they should match each other (each peer sees the other's fingerprint).

If the fingerprints match, your connection is secure against MITM. If they don't, disconnect immediately — someone is intercepting your traffic.

### Tutorial 2.30 — Exporting conversations

Click 💾 in the chat header, or type `/export json` / `/export txt`:

- **JSON format** — full structured export with timestamps, author info, message IDs
- **TXT format** — plain text log: `[timestamp] Author: message body`

The file downloads as `zdmc-<invite>-<timestamp>.<ext>`.

### Tutorial 2.31 — Saved history

Click **Open History** on the home screen. A modal lists all past conversations saved on this device:

- Peer name + chat mode (private/group/stranger)
- Date and time
- Message count + invite code

Click any item to restore the conversation in the chat view. History is stored in IndexedDB, encrypted at rest with your session key.

### Tutorial 2.32 — Clearing data

Three options in the Clear modal:

1. **Clear This Device Only** — wipes the local chat view, doesn't affect peer
2. **Clear For Everyone** — sends a `clear-req` message; both sides clear
3. **Wipe All Local History** — clears IndexedDB entirely

For nuclear reset, click **Wipe Local Data** on the home screen — wipes IndexedDB + localStorage + reloads.

---

## 3. User Guide — How to use each part of the app

This section is a reference-style guide for specific tasks. Jump to any subsection.

### 3.1 How to set or change your nickname

**During setup:** The setup modal asks for your display name when you start any chat mode.

**Later:**
- Open Settings (⚙️) → edit the **Display Name** field.
- Or type `/nick NewName` in the composer.

Your nickname is broadcast to all peers via the `hello` message on next reconnect.

### 3.2 How to verify your connection is secure

1. Click ⚙️ Settings → Security section → see your fingerprint.
2. Compare with your peer's fingerprint (which they see in their settings).
3. They should match each other (each peer sees the other's fingerprint).

If fingerprints match → no MITM attack. If they don't → disconnect immediately.

### 3.3 How to share your invite code

Three ways:
1. **Copy code** — click 📋 in chat header, paste anywhere.
2. **Share link** — copy `https://your-host/#join=XXXX-XXXX-XXXX` from the invite modal.
3. **QR code** — show the QR from the invite modal, or your peer can scan your screen.

### 3.4 How to switch themes

Three ways:
1. **Settings drawer** → Theme section → click any of 5 swatches.
2. **Command palette** (Ctrl+K) → "Toggle Theme".
3. Open the chat and the theme persists across sessions in localStorage.

### 3.5 How to change UI scale

- Open Settings → UI Scale section.
- Click S/M/L/XL buttons.
- Or check "Auto-detected" to let the app pick based on screen size and DPI.

### 3.6 How to set your online status

- Open Settings → Online Status section.
- Click Online / Idle / Do Not Disturb / Invisible.
- Or type `/online`, `/idle`, `/dnd`, `/invisible`.

Status affects how you appear in the member list (groups) and influences notification behavior.

### 3.7 How to set a custom status

- Open Settings → Custom Status field.
- Type up to 48 characters (e.g. "Building things", "In a meeting").
- Or type `/status Building things`.

Custom status appears under your name in the member list.

### 3.8 How to start a voice or video call

1. Ensure you're in a private chat or stranger match (calls don't work in groups).
2. Click 📞 (voice) or 📹 (video) in the chat header.
3. Browser requests mic/camera permission.
4. A call offer is sent to your peer.
5. Their device shows an incoming call modal.
6. On accept, the call overlay appears.

Use the mute buttons to toggle audio/video mid-call. Click the red hangup button to end.

### 3.9 How to use disappearing messages

1. Click ⏱️ in the chat header.
2. Choose a TTL: 30s, 5m, 1h, 24h, 7d, or Off.
3. All new messages (both directions) auto-delete after the TTL.

The setting syncs with your peer so both sides apply the same TTL.

### 3.10 How to search messages

1. Click 🔍 in the chat header (or type `/search query`).
2. The search panel slides in from the right.
3. Type your query in the search box.
4. Results show snippets with the search term highlighted.
5. Click any result to scroll to the original message.

### 3.11 How to pin a message

1. Hover over the message.
2. Click the 📌 pin button in the hover actions.
3. Click 📌 in the chat header to view all pinned messages.

### 3.12 How to star a message

1. Hover over the message.
2. Click the ⭐ star button in the hover actions (added in v4).
3. Click ⭐ in the chat header to view all starred messages.

### 3.13 How to forward a message

1. Hover over the message.
2. Click the ➡️ forward button in the hover actions (added in v4).
3. Enter the target invite code in the modal.
4. Click **Forward**.

The message body is copied to your clipboard with instructions to open the target chat and paste.

### 3.14 How to create a poll

1. Type `/poll` or use the command palette.
2. Enter the question.
3. Enter options (one per line, max 6).
4. Click **Create Poll**.

The poll appears as a special message with clickable options and live vote counts.

### 3.15 How to schedule a message

1. Type `/schedule` or use the command palette.
2. Type your message.
3. Pick a date and time.
4. Click **Schedule**.

The message is queued locally and sent automatically at the scheduled time.

### 3.16 How to record a voice message

1. Click the 🎤 microphone button next to the composer.
2. A recording bar appears with a timer.
3. Click **Cancel** to discard, or **Send** to stop and send.

The recipient sees a voice message bubble with play button, waveform, and duration.

### 3.17 How to mute notifications

Click 🔇 in the chat header. The button turns red. Sound effects for incoming messages are suppressed. Click again to unmute.

### 3.18 How to export a conversation

Click 💾 in the chat header, or type `/export json` or `/export txt`. The file downloads with timestamp and invite code in the filename.

### 3.19 How to clear the conversation

Click 🗑️ in the chat header. Three options:
- **Clear This Device Only** — local view only
- **Clear For Everyone** — sends clear request to peer
- **Wipe All Local History** — clears IndexedDB

### 3.20 How to leave a chat

Click ← Back in the chat header. All connections are torn down, draft is cleared, and you return to the home screen.

### 3.21 How to find another stranger

In stranger mode, click ➡️ Next in the chat header (or type `/next`). The current peer is disconnected and a new search begins automatically.

### 3.22 How to scan a QR code

1. Click **Enter Code or Scan QR** on the home screen.
2. Click **Scan QR**.
3. Grant camera permission.
4. Point the camera at the invite QR code.
5. The code is detected automatically and the join modal populates.

### 3.23 How to restore a previous conversation

1. Click **Open History** on the home screen.
2. Browse saved conversations sorted by date.
3. Click any item to restore it in the chat view.

History is stored in IndexedDB, encrypted at rest with your session key.

### 3.24 How to use the command palette

Press `Ctrl+K` (or `Cmd+K` on Mac). The palette appears at the top center.

- Type to filter
- ↑↓ to navigate
- Enter to execute
- Esc to close

Includes all slash commands plus quick actions (toggle theme, open settings, etc.).

---

## 4. Architecture Overview

### 4.1 Four-layer stack

ZeroDay Meet Chat is built as four distinct layers, each addressing a specific concern:

```
┌──────────────────────────────────────────────────────────┐
│              LAYER 4 — Anti-XSS Markdown Renderer        │
│   Tokenizes & escapes user input before DOM insertion    │
└──────────────────────────────┬───────────────────────────┘
                               │
┌──────────────────────────────┴───────────────────────────┐
│            LAYER 3 — HMAC-SHA256 Anti-Tamper              │
│   + Nonce + Timestamp Anti-Replay                        │
└──────────────────────────────┬───────────────────────────┘
                               │
┌──────────────────────────────┴───────────────────────────┐
│          LAYER 2 — ECDH P-256 + AES-GCM 256              │
│   Per-session keypair, shared secret via ECDH            │
└──────────────────────────────┬───────────────────────────┘
                               │
┌──────────────────────────────┴───────────────────────────┐
│          LAYER 1 — WebRTC P2P Transport (v2 proven)       │
│   Data channels + media streams + MQTT signaling         │
└──────────────────────────────────────────────────────────┘
```

### 4.2 Transport layer

ZeroDay Meet Chat uses **two parallel transports** for signaling + one for media:

| Transport | Purpose | Endpoint |
|---|---|---|
| **MQTT (HiveMQ)** | Public signaling channel for WebRTC offer/answer/ICE | `wss://broker.hivemq.com:8884/mqtt` |
| **WebRTC data channel** | Encrypted message transport (P2P) | Direct browser-to-browser |
| **WebRTC media stream** | Voice/video calls | Direct browser-to-browser |
| **IndexedDB** | Local encrypted history | Browser storage |

### 4.3 Three chat modes

#### Private chat (1-on-1)

```
   Peer A                          Peer B
     │                               │
     │  MQTT topic: zdmc/v3/priv/   │
     │       A4F-9X2-K7P             │
     │   (signaling only)            │
     │  ┌─────────────────────────┐  │
     │  │ offer/answer/ICE        │  │
     │  └─────────────────────────┘  │
     │                               │
     │  Direct WebRTC data channel   │
     │ ◄───────────────────────────► │
     │   (encrypted with AES-GCM)    │
```

- One invite code → one MQTT topic for signaling.
- After WebRTC handshake, MQTT is no longer needed.
- All messages flow over the encrypted data channel.

#### Group chat (mesh)

```
            Peer A
           ╱        ╲
          ╱          ╲
        Peer B ◄────► Peer C
              ╲      ╱
               ╲    ╱
                Peer D
```

- Each peer connects to every other peer (full mesh).
- Messages broadcast to all connected peers.
- Peers re-broadcast incoming messages to others (relay).
- `seenIds` set (1000 entries, FIFO) prevents duplicate rendering.
- No central hub — survives any single peer leaving.

#### Stranger match

```
   Peer A (looking)            Peer B (looking)
        │                            │
        │  MQTT topic: zdmc/v3/match/music
        │                            │
        ▼                            ▼
   Publishes "looking" message with peer info
        │                            │
        ▼                            ▼
   Lower ID initiates WebRTC offer
        │                            │
        ▼                            ▼
   Direct WebRTC data channel
```

- Both peers publish "looking" messages on a shared topic.
- Lower ID becomes the initiator, sends offer.
- Match completes when data channel opens.
- "Next" button tears down current peer and searches again.

### 4.4 Encryption flow

```
   SENDING                                    RECEIVING
   
   plaintext JSON                             encrypted blob
        │                                          │
        ▼                                          ▼
   Crypto.encrypt(obj)                       Crypto.decrypt(obj)
        │                                          │
        ├─ Generate 12-byte IV                   ├─ Check timestamp (reject if >5min old)
        ├─ AES-GCM encrypt → ciphertext          ├─ Check nonce (reject if seen before)
        ├─ HMAC-SHA256(IV ‖ ciphertext)          ├─ Verify HMAC (reject if mismatch)
        ├─ Base64 encode IV, ciphertext, HMAC    ├─ AES-GCM decrypt → plaintext
        └─ Attach timestamp + nonce             └─ Parse JSON → message object
        │                                          │
        ▼                                          ▼
   Send over WebRTC                          Hand to message handler
```

**Key points:**
- Each session generates a fresh ECDH P-256 keypair.
- Public keys exchanged in `hello` messages.
- Shared secret derived via ECDH — never transmitted.
- HMAC key derived from shared secret via HKDF.
- Nonces tracked in a `Set` (5000 entries, FIFO eviction).
- Messages older than 5 minutes are rejected (anti-replay).

### 4.5 Anti-XSS markdown renderer

The renderer follows a strict escape-first philosophy:

1. **Escape everything** — `escapeHtml()` runs on the entire input first.
2. **Apply formatting tokens** — regex replacements for code blocks, inline code, bold, italic, strikethrough, spoilers, mentions, channel refs, links, blockquotes, lists.
3. **No raw `innerHTML` with user input** — all user content is tokenized before reaching the DOM.

This means `<script>alert('xss')</script>` in a message renders as inert text, not executed code.

### 4.6 Message protocol

| `type` | Direction | Payload | Purpose |
|---|---|---|---|
| `hello` | out | `name, avatar, pubKey, status, customStatus` | Identity + crypto key exchange |
| `text` | out | `body, id, ts, replyTo` | Chat message |
| `file` | out | `name, size, dataUrl, ts, id` | File attachment (max 5MB) |
| `voice` | out | `dataUrl, duration, ts, id` | Voice message |
| `poll` | out | `question, options, votes, ts, id` | Poll message |
| `poll-vote` | out | `id, option, voter` | Poll vote |
| `reaction` | out | `id, emoji, users` | Reaction toggle |
| `edit` | out | `id, body` | Edit message |
| `delete` | out | `id` | Delete message |
| `pin` | out | `id, value` | Pin/unpin message |
| `typing` | out | `value` | Typing indicator (bool) |
| `read` | out | `id` | Read receipt |
| `presence` | out | `value` | Online status |
| `clear-req` | out | (none) | Request peer to clear conversation |
| `dm-setting` | out | `ttl` | Disappearing messages TTL |
| `call-offer` | out | `sdp, video` | WebRTC call offer |
| `call-answer` | out | `sdp` | WebRTC call answer |
| `call-signal` | out | `sub, c` | WebRTC ICE candidate |
| `call-end` | out | (none) | End call |
| `call-reject` | out | (none) | Reject incoming call |
| `stranger-next` | out | (none) | Skip to next stranger |

### 4.7 File structure (single HTML file)

```
<head>
  ├─ Meta tags (viewport, theme-color, CSP, referrer policy, X-Content-Type-Options)
  ├─ <title>
  └─ <style> (~700 lines: tokens, layout, components, themes, responsive, animations)
<body>
  ├─ #app
  │   ├─ <header.topbar> (logo, E2E badge, conn pill, command palette, settings)
  │   └─ <main>
  │       ├─ #homeView (hero, security info, action grid with 6 cards)
  │       └─ #chatView
  │           ├─ .chat-wrap
  │           │   ├─ .chat-main
  │           │   │   ├─ .chat-header (avatar, name, status, 11 action buttons)
  │           │   │   ├─ .messages (Discord-style rows)
  │           │   │   ├─ .stranger-searching overlay
  │           │   │   ├─ .reply-bar
  │           │   │   └─ .composer (toolbar + textarea + buttons)
  │           │   │       └─ .emoji-picker
  │           │   ├─ .member-list (group sidebar)
  │           │   ├─ .search-panel
  │           │   └─ .pinned-panel
  ├─ 14 modals (create, group, stranger, invite, join, QR scan, history,
  │              incoming call, clear, forward, msg info, disappear, poll, schedule,
  │              profile)
  ├─ .call-overlay (full-screen video call UI)
  ├─ .cmd-palette (Ctrl+K)
  ├─ .ctx-menu (right-click menu)
  ├─ .drawer#settingsDrawer (personalization)
  ├─ <input type="file">
  ├─ #toast-host
  └─ <script> (~2500 lines)
      ├─ Utils ($, $$, now, uid, randHex, fmtSize, fmtTime, esc, toast, loadScript)
      ├─ Crypto module (ECDH, AES-GCM, HMAC, fingerprint)
      ├─ renderMarkdown (anti-XSS tokenizer)
      ├─ Storage (localStorage settings + IndexedDB history)
      ├─ MQTT_RELAY (HiveMQ public broker)
      ├─ ICE_SERVERS (Google STUN + Cloudflare STUN + Open Relay TURN)
      ├─ ZeroDayPeer (1-on-1 WebRTC + E2E)
      ├─ ZeroDayGroup (mesh + relay + E2E)
      ├─ ZeroDayStranger (anonymous match + E2E)
      ├─ ZeroDayCall (voice/video)
      ├─ State object
      ├─ UI helpers (applySettings, autoScale, setConnPill, showView, openModal)
      ├─ Message rendering (renderMessages, buildMsgRow, pushMessage)
      ├─ Message actions (reply, edit, delete, react, pin, star, forward, info)
      ├─ Slash commands (20+ commands)
      ├─ Composer logic (markdown toolbar, emoji picker, voice recorder)
      ├─ Command palette
      ├─ QR scanner
      ├─ Context menu (right-click)
      ├─ Settings drawer
      ├─ v4 features (voice msgs, disappearing, starred, forward, mute, polls, schedule)
      └─ boot() entry point
```

### 4.8 External CDN dependencies

| Library | Version | Size | Purpose | Loaded when |
|---|---|---|---|---|
| `mqtt.js` | 5.3.5 | ~50 KB | MQTT client for signaling | On boot |
| `qrcode` | 1.5.3 | ~50 KB | QR code generation | When invite modal opens |
| `jsQR` | 1.4.0 | ~40 KB | QR code scanning | When scan button clicked |

All loaded from `cdn.jsdelivr.net` — no API key required.

### 4.9 STUN/TURN servers

ZeroDay Meet Chat uses 5 STUN servers + 3 TURN servers for NAT traversal:

```
STUN servers:
- stun:stun.l.google.com:19302
- stun:stun1.l.google.com:19302
- stun:stun2.l.google.com:19302
- stun:stun.cloudflare.com:3478
- stun:stun.nextcloud.com:443

TURN servers (free, Open Relay Project):
- turn:openrelay.metered.ca:80
- turn:openrelay.metered.ca:443
- turn:openrelay.metered.ca:443?transport=tcp
```

The TURN servers ensure connectivity even behind strict NATs/firewalls.

---

## 5. Feature Reference

### 5.1 Messaging

| Feature | Implementation |
|---|---|
| **Plain text** | Auto-growing textarea (max 200px height) |
| **Markdown subset** | Code blocks (with language label), inline code, bold, italic, strikethrough, spoilers, mentions, channel refs, links, blockquotes, lists |
| **Markdown toolbar** | 7 buttons (B, I, S, code, codeblock, quote, spoiler) + emoji picker |
| **Slash commands** | 25+ commands (see §6.2) |
| **Mentions** | `@username` highlighted with accent background |
| **Channel refs** | `#channel` highlighted with accent background |
| **Emoji picker** | 5 categories, 100+ emojis, search box, click to insert |
| **Quick reactions** | 6 emojis appear above each message on hover (👍 ❤️ 😂 😮 😢 🙏) |
| **Reactions** | Any emoji can be added via the 😊 hover action; toggle behavior |
| **Message grouping** | Consecutive messages from same author within 5 min are grouped (avatar hidden, meta hidden) |
| **Reply quotes** | Click quote to scroll to original (highlights briefly) |
| **Edit messages** | Only own messages; shows `(edited)` tag |
| **Delete messages** | Only own messages; soft-delete with "Message deleted" placeholder |
| **Pin messages** | Toggle pin; pinned messages appear in dedicated panel |
| **Star messages** | Toggle star; starred messages appear in dedicated view |
| **Forward messages** | Copy to clipboard with target invite code instructions |
| **Message info** | Sent time, author, delivery/read status, edited flag, pinned/starred, message ID |
| **File attachments** | Up to 5MB, sent as base64 data URL |
| **Voice messages** | WebM audio, recorded via MediaRecorder, playable inline with waveform |
| **Polls** | Up to 6 options, live vote counts, percentage bars, changeable votes |
| **Scheduled messages** | Queue locally, send at specified datetime |
| **Disappearing messages** | TTL options: 30s, 5m, 1h, 24h, 7d, off |
| **Read receipts** | Double-tick when peer reads; toggleable in settings |
| **Typing indicator** | Animated dots; toggleable in settings |
| **Auto-save drafts** | Per-invite-code draft storage in localStorage |
| **Last seen** | Shows "Last seen 5m ago" when peer goes offline |
| **Sound effects** | 660 Hz sine wave on new message; toggleable |
| **Mute notifications** | Suppress sound effects; toggleable per chat |

### 5.2 Chat modes

| Mode | Topology | Encryption | Group size | Use case |
|---|---|---|---|---|
| **Private chat** | 1-on-1 P2P | E2E (ECDH + AES-GCM) | 2 | Private conversations |
| **Group room** | Full mesh | E2E per peer pair | Unlimited (mesh overhead) | Team / family / community |
| **Stranger match** | 1-on-1 P2P | E2E | 2 | Anonymous chat |

### 5.3 Voice & video calls

| Feature | Implementation |
|---|---|
| **Voice calls** | WebRTC media stream, audio only |
| **Video calls** | WebRTC media stream, audio + video |
| **Incoming call modal** | Accept / Reject buttons |
| **Full-screen overlay** | Remote video fills screen, local video as thumbnail |
| **Mute audio** | Toggle button in call controls |
| **Mute video** | Toggle button (video calls only) |
| **Hangup** | Red button, ends call for both peers |
| **Call logging** | System message in chat when call starts/ends |

### 5.4 Themes

| Theme | Background | Accent | Best for |
|---|---|---|---|
| **🌌 Midnight** | `#0a0c14` | `#5865f2` (Discord indigo) | Default, modern dark |
| **🔥 Crimson** | `#1a0a10` | `#ed4245` (red) | Gaming, intense vibe |
| **💚 Matrix** | `#02110a` | `#00ff8c` (green monospace) | Hackers, terminal aesthetic |
| **🌅 Sunset** | `#1a0f08` | `#ff7a59` (coral) | Warm, creative |
| **❄️ Arctic** | `#f0f5ff` (light) | `#3b5bff` (blue) | Daytime, high contrast |

### 5.5 UI scale

| Scale | Multiplier | Use case |
|---|---|---|
| S | 0.85× | Small screens, dense layouts |
| M | 1.0× (default) | Standard desktop |
| L | 1.15× | Large monitors, accessibility |
| XL | 1.35× | Very large screens, visual impairment |

Auto-detection considers screen width, height, and DPR.

### 5.6 Online status

| Status | Indicator | Behavior |
|---|---|---|
| 🟢 Online | Green dot | Full presence, immediate responses |
| 🟡 Idle | Yellow dot | Away from keyboard |
| 🔴 Do Not Disturb | Red dot | Suppress notifications |
| ⚫ Invisible | Gray dot | Appear offline but stay connected |

### 5.7 Security features

| Feature | Implementation |
|---|---|
| **End-to-end encryption** | ECDH P-256 key exchange + AES-GCM 256 |
| **Anti-replay** | Nonce + timestamp (5 min window) |
| **Anti-tamper** | HMAC-SHA256 over ciphertext |
| **Anti-XSS** | Tokenized + escaped markdown renderer |
| **CSP-locked** | Content-Security-Policy header |
| **Referrer policy** | `no-referrer` |
| **X-Content-Type-Options** | `nosniff` |
| **Key fingerprint** | SHA-256 of public key, 8-char groups |
| **Per-session keys** | Fresh keypair generated on each session start |

### 5.8 Persistence

| Storage | What's stored | Encrypted? |
|---|---|---|
| `localStorage` | Settings, session, drafts | No (browser-managed) |
| `IndexedDB` | Conversation history (last 200 msgs per chat) | Yes (encrypted at rest with session key) |

### 5.9 Settings (all stored locally)

| Setting | Default | Effect |
|---|---|---|
| Display name | `Anonymous` | Your visible name |
| Avatar letter | `A` (auto from name) | 1-2 char avatar |
| Custom status | (empty) | Up to 48 chars |
| Online status | `online` | Online/Idle/DND/Invisible |
| Theme | `midnight` | One of 5 themes |
| UI scale | `1` (auto) | S/M/L/XL |
| Read receipts | on | Show double-tick when peer reads |
| Typing indicator | on | Show "typing…" in real-time |
| Sound effects | on | Play sound on new message |
| Enter to send | on | Enter sends; off → Shift+Enter sends |
| Auto-resume | on | Restore last session on reload |
| Save history | on | Store conversations locally (encrypted) |
| Reduce motion | off | Disable animations |

---

## 6. Keyboard Shortcuts & Commands

### 6.1 Keyboard shortcuts

| Shortcut | Action |
|---|---|
| `Enter` | Send message (when "Enter to send" is on) |
| `Shift + Enter` | Insert newline |
| `Ctrl/Cmd + Enter` | Send (when "Enter to send" is off) |
| `/` | Begin a slash command |
| `Ctrl/Cmd + K` | Open command palette |
| `Esc` | Close command palette / emoji picker / context menu / cancel edit/reply |
| `↑` / `↓` | Navigate command palette results |
| Right-click message | Open context menu |
| Click outside | Close any open modal/menu |

### 6.2 Slash commands

**Core commands:**

| Command | Action |
|---|---|
| `/help` | List all commands |
| `/me <action>` | Send an action (italicized) |
| `/shrug [text]` | Append `¯\_(ツ)_/¯` to your message |
| `/clear` | Clear conversation locally |
| `/leave` | Leave current chat |
| `/fingerprint` | Show your E2E fingerprint |

**Message commands:**

| Command | Action |
|---|---|
| `/pin <id>` | Toggle pin on a message |
| `/star <id>` | Star a message by ID |
| `/forward` | Forward last message |
| `/info <id>` | Show message info |
| `/search <query>` | Search messages |
| `/export [json\|txt]` | Export conversation |

**Identity & status:**

| Command | Action |
|---|---|
| `/nick <name>` | Change your nickname |
| `/status <text>` | Set your custom status |
| `/online` / `/idle` / `/dnd` / `/invisible` | Set online status |
| `/topic <text>` | Set room topic (group only) |

**Calls:**

| Command | Action |
|---|---|
| `/call` | Start voice call |
| `/video` | Start video call |

**Stranger mode:**

| Command | Action |
|---|---|
| `/next` | Find next stranger |

**v4 features:**

| Command | Action |
|---|---|
| `/voice` | Record voice message |
| `/poll` | Create a poll |
| `/schedule` | Schedule a message |
| `/mute` | Toggle mute |
| `/disappear [seconds]` | Set disappearing messages |

---

## 7. Security & Privacy

### 7.1 What is stored locally

| Storage | Content |
|---|---|
| `localStorage` | Display name, avatar, theme, scale, status, settings, drafts (per invite code), session info |
| `IndexedDB` | Conversation history (last 200 messages per chat), encrypted at rest |

**Settings → Clear Local Data & Reset** wipes both stores and reloads the page.

### 7.2 What is transmitted

| Destination | What's sent | When |
|---|---|---|
| **HiveMQ broker** (`broker.hivemq.com:8884`) | WebRTC signaling (offer/answer/ICE candidates) only — never message content | During P2P setup |
| **STUN servers** (Google, Cloudflare) | STUN binding requests for NAT traversal | During P2P setup |
| **TURN servers** (Open Relay Project) | Encrypted WebRTC traffic (only if STUN fails) | When behind strict NAT |
| **Peer's browser** (direct) | E2E encrypted messages, voice/video streams | After P2P connection establishes |
| **CDN** (jsdelivr) | Library code only (`mqtt.js`, `qrcode`, `jsQR`) | On demand |

### 7.3 Trust boundaries

| Layer | Trusted? | Reason |
|---|---|---|
| `localStorage` / `IndexedDB` | Local only | Stays on this device |
| HiveMQ MQTT broker | Trusted for signaling only | Sees WebRTC SDP/ICE, never message content |
| STUN servers | Trusted for IP discovery only | Sees your public IP, no message content |
| TURN servers (if used) | Trusted as relay only | DTLS-encrypted WebRTC traffic passes through; not plaintext |
| Other peers | **Verified via fingerprint** | Compare fingerprints out-of-band to detect MITM |
| CDN libraries | Trusted code source | jsdelivr serves signed library files |

**The MQTT broker never sees your message content.** All chat flows over the direct WebRTC data channel, which is end-to-end encrypted with keys only you and your peer possess.

### 7.4 Encryption details

| Component | Algorithm | Key size | Purpose |
|---|---|---|---|
| Key exchange | ECDH (P-256 / prime256v1) | 256-bit | Derive shared secret |
| Message encryption | AES-GCM | 256-bit | Encrypt message content |
| Tamper detection | HMAC-SHA256 | 256-bit | Verify message integrity |
| HMAC key derivation | HKDF-SHA256 | 256-bit | Derive separate HMAC key from shared secret |
| Fingerprint | SHA-256 | 256-bit | Verify peer's public key |

**Anti-replay:**
- Each message has a 12-byte IV (nonce).
- Receiver tracks seen nonces in a `Set` (cap 5000, FIFO).
- Messages older than 5 minutes are rejected.

**Anti-tamper:**
- HMAC computed over `IV ‖ ciphertext`.
- Constant-time comparison to prevent timing attacks.

### 7.5 Anti-XSS

The markdown renderer follows a strict escape-first philosophy:

1. **Escape all HTML** via `escapeHtml()` — converts `<`, `>`, `&`, `"`, `'` to entities.
2. **Apply formatting tokens** via regex — code blocks, inline code, bold, italic, strikethrough, spoilers, mentions, channel refs, links, blockquotes, lists.
3. **No raw `innerHTML`** with user input — all user content is tokenized before reaching the DOM.

This means `<script>alert('xss')</script>` in a message renders as inert text.

### 7.6 Content Security Policy

The page enforces a strict CSP:

```
default-src 'self' 'unsafe-inline' data: blob: https: wss:;
script-src 'self' 'unsafe-inline' https://cdn.jsdelivr.net https://unpkg.com;
img-src 'self' data: blob: https:;
media-src 'self' blob:;
connect-src 'self' https: wss:;
```

This blocks:
- Inline event handlers (except `'unsafe-inline'` for scripts, needed for the single-file architecture)
- Untrusted script sources
- Mixed content (HTTP resources on HTTPS pages)

### 7.7 Recommendations

- **Verify fingerprints out-of-band** — call your peer via phone/voice call and read fingerprints to each other. If they match, you're secure against MITM.
- **Don't share invite codes publicly** — anyone with the code can join your chat. Use random stranger match for anonymous encounters instead.
- **Use disappearing messages for sensitive content** — set a short TTL (5 min) so messages don't linger.
- **Wipe data before selling/recycling devices** — click "Wipe Local Data" on the home screen.
- **Be cautious with file attachments** — files up to 5MB are sent as base64 over the encrypted channel. Larger files aren't supported (security/DoS protection).

---

## 8. Technical Implementation

### 8.1 Stack

- **Vanilla JS** — no framework, no build step. Single ~2500-line `<script>` block.
- **Web APIs used**: `RTCPeerConnection`, `RTCDataChannel`, `MediaRecorder`, `getUserMedia`, `IndexedDB`, `crypto.subtle`, `BroadcastChannel` (not used — replaced by WebRTC), `navigator.clipboard`, `navigator.share`, `URL.createObjectURL`, `MediaDevices.getUserMedia`, `MediaStreamTrack`.
- **External libraries** (loaded on demand):
  - `mqtt@5.3.5` (~50 KB) — MQTT client for signaling
  - `qrcode@1.5.3` (~50 KB) — QR code generation
  - `jsQR@1.4.0` (~40 KB) — QR code scanning
- **All libraries from `cdn.jsdelivr.net`** — no API key required.

### 8.2 Performance choices

- **Append-only rendering** — new messages are appended as individual DOM nodes; only the affected row is patched for edits/reactions.
- **Message grouping** — consecutive messages from the same author within 5 minutes are visually grouped (avatar hidden, meta hidden).
- **IndexedDB writes throttled** — history is saved once per message add (last 200 messages per chat).
- **MQTT message buffer** — if MQTT isn't connected yet, signaling messages are queued and flushed on connect.
- **Heartbeat intervals**:
  - Private chat: 15s presence heartbeat
  - Group chat: 12s hello heartbeat
  - Stranger: 4s looking heartbeat
- **Stale peer pruning** — peers not seen for 45s are removed from the online list.
- **Voice recording** — uses `MediaRecorder` with default codec (WebM/Opus in Chrome, WebM/Opus in Firefox).

### 8.3 Resilience

| Failure mode | Behavior |
|---|---|
| MQTT broker unreachable | Cannot establish new P2P connections; existing connections keep working |
| STUN fails (all servers) | TURN servers take over (Open Relay Project) |
| TURN servers fail | P2P fails on strict NATs; users on open networks still connect |
| Peer disconnects mid-call | Call ends, system message logged, both sides return to chat |
| IndexedDB quota exceeded | Writes silently fail; app keeps running |
| `localStorage` quota exceeded | Same — writes wrapped in try/catch |
| Camera/mic permission denied | Call fails with toast explaining the error |
| File > 5MB | Rejected with toast warning |
| Stale message (>5 min old) | Rejected by anti-replay check |
| HMAC mismatch (tampering) | Message rejected with console warning |
| Unknown message type | Silently ignored |
| Invalid JSON | Silently ignored |
| CDN library fails to load | Feature degrades gracefully (e.g. no QR scan if jsQR fails) |
| Tab backgrounded | Presence still broadcasts; sound effects fire on messages |
| Page reload | Auto-resume restores last session if setting is on |
| Network offline | Toast warns; P2P may be interrupted |
| Stranger leaves | Auto-show "searching" overlay; user can search again |
| Group peer leaves | Member list updates; system message logged; mesh stays alive |

### 8.4 Browser compatibility

Targets modern evergreen browsers (Chrome 90+, Firefox 90+, Safari 15+). Key APIs required:

- `RTCPeerConnection` with data channels (universal)
- `RTCDataChannel` (universal)
- `crypto.subtle` (universal in modern browsers)
- `IndexedDB` (universal)
- `MediaRecorder` (Chrome 47+, Firefox 25+, Safari 14.1+)
- `getUserMedia` (universal with HTTPS)
- `BroadcastChannel` (not used — replaced by WebRTC)
- `navigator.clipboard` (Chrome 66+, Firefox 63+, Safari 13.1+)
- `navigator.share` (mobile Safari, Chrome Android)
- `URL.createObjectURL` (universal)
- CSS `env(safe-area-inset-*)` (iOS 11.2+)
- `100dvh` viewport unit (modern mobile browsers)
- `Intl.DisplayNames` (Chrome 81+, Firefox 86+, Safari 14.1+)

**CSP requires HTTPS** for most features — serve over HTTPS, not HTTP or `file://`.

### 8.5 Signaling flow

```
   Peer A (host)                              Peer B (guest)
       │                                          │
       │ 1. Generate invite code                  │
       │    A4F-9X2-K7P                          │
       │                                          │
       │ 2. Subscribe to MQTT topic:              │
       │    zdmc/v3/priv/a4f9x2k7p                │
       │                                          │
       │              ◄──── user shares code ──── │
       │                                          │
       │                                          │ 3. Subscribe to same topic
       │                                          │
       │ 4. Create RTCPeerConnection              │
       │    Create DataChannel "zdmc"             │
       │    Generate local offer (SDP)            │
       │                                          │
       │ 5. Publish offer to MQTT ───────────────►│
       │                                          │
       │                                          │ 6. Receive offer
       │                                          │    Set as remote description
       │                                          │    Generate local answer
       │                                          │
       │ ◄────────────── publish answer ──────────│ 7. Publish answer
       │                                          │
       │ 8. Set answer as remote description      │
       │                                          │
       │ 9. ICE candidates exchanged via MQTT ◄──►│ 9. ICE candidates
       │                                          │
       │ 10. ICE connection established           │ 10. ICE connection established
       │     Data channel opens                   │      Data channel opens
       │                                          │
       │ 11. Exchange `hello` messages:           │ 11. Exchange `hello`:
       │     - name, avatar                        │      - name, avatar
       │     - public key (ECDH P-256)             │      - public key (ECDH P-256)
       │                                          │
       │ 12. Derive shared secret via ECDH         │ 12. Derive shared secret
       │     All further messages encrypted       │      All further messages encrypted
       │                                          │
       │ ◄─────── encrypted chat ───────────────► │ 13. Encrypted chat
```

---

## 9. Use Cases

### 9.1 Private secure conversation

Two friends want to discuss sensitive topics (medical, legal, personal). They:
1. Both open ZeroDay Meet Chat.
2. Friend A starts a private chat, gets invite code `A4F-9X2-K7P`.
3. Friend A shares code via Signal (encrypted messenger).
4. Friend B enters the code, connects.
5. They verify fingerprints via a voice call.
6. They chat with full E2E encryption — no server stores anything.
7. They enable disappearing messages (5 min TTL) for extra security.
8. After the conversation, both click "Wipe Local Data".

### 9.2 Family group chat

A family of 5 wants a private group chat without using Facebook/WhatsApp:
1. One member creates a group: `GRP-A4F9-K7P`.
2. Shares the code with the family.
3. Each member joins; mesh network forms.
4. Messages flow peer-to-peer; no central server.
5. Pinned messages keep important info (birthdays, addresses) at top.
6. Polls help with decisions ("Where to eat tonight?").
7. Voice messages for quick updates without typing.

### 9.3 Anonymous stranger chat

A user wants to practice a language or just chat anonymously:
1. Click "Find a Stranger".
2. Set interest to "language exchange".
3. Get matched with someone with the same interest.
4. Chat with full E2E encryption — even the relay can't read it.
5. Click "Next" to skip to another stranger.

### 9.4 Team collaboration

A small team (3-5 people) wants a no-setup chat for a project:
1. One member creates a group.
2. Shares invite code via Slack/email.
3. Mesh network forms.
4. Use markdown for code snippets, polls for decisions, file attachments for sharing documents.
5. Use pinned messages for project decisions.
6. Export conversation as JSON at the end of the project.

### 9.5 Voice/video calling

Two users want a private video call without Zoom/Teams:
1. Start a private chat.
2. Verify fingerprints.
3. Click 📹 video call.
4. Full-screen overlay with remote + local video.
5. Mute audio/video as needed.
6. Hang up when done — call logged in chat history.

### 9.6 Conference / event backchannel

At a conference, attendees want a chat room for the event:
1. Organizer creates a group: `GRP-EVENT-2024`.
2. Prints QR codes on posters around the venue.
3. Attendees scan QR with their phones, join the chat.
4. Use polls for live Q&A: "Which session did you like most?"
5. Pinned messages keep the schedule visible.

### 9.7 Customer support

A small business wants to offer encrypted customer chat:
1. Generate a private chat invite code.
2. Share with customer via email.
3. Customer joins, conversation is E2E encrypted.
4. After resolution, both sides clear the conversation.
5. History is never stored on any server.

### 9.8 Anonymous whistleblowing

A whistleblower wants to share information with a journalist:
1. Both open ZeroDay Meet Chat.
2. Use stranger match with a pre-agreed interest keyword.
3. Verify fingerprints via Signal.
4. Enable disappearing messages (30s TTL).
5. Share information; messages auto-delete rapidly.
6. Both sides wipe local data after the conversation.

### 9.9 Language practice

Two language learners want to practice:
1. Use stranger match with interest "language exchange".
2. Take turns typing in each other's native language.
3. Use voice messages to practice pronunciation.
4. Use polls for fun vocabulary quizzes.
5. Schedule regular sessions with `/schedule`.

### 9.10 Gaming coordination

A gaming clan wants voice + text coordination:
1. Create a group: `GRP-CLAN-RAID`.
2. Share code with clan members.
3. Use text chat for strategy, voice calls for live coordination.
4. Pin the raid strategy at the top.
5. Use polls for "Who's available tonight?".

### 9.11 Developer code review

Two developers want to discuss code privately:
1. Start a private chat.
2. Paste code blocks with language labels for syntax highlighting.
3. Edit messages to refine suggestions.
4. Star important snippets for later reference.
5. Export conversation as JSON for archival.

### 9.12 Doctor-patient telehealth

A doctor and patient want a HIPAA-friendly encrypted chat:
1. Both open ZeroDay Meet Chat.
2. Doctor generates private chat invite, shares with patient via secure channel.
3. Verify fingerprints via phone call.
4. Patient shares symptoms via text or voice message.
5. Doctor responds with advice.
6. Both sides wipe data after the consultation.

---

## 10. Limitations & Troubleshooting

### 10.1 Limitations

| Limitation | Reason | Mitigation |
|---|---|---|
| Single HTML file size (~3 MB) | All code inline, no build step | Acceptable trade-off for portability |
| WebRTC requires HTTPS | Browser security requirement | Host on GitHub Pages, Netlify, Cloudflare Pages, or any HTTPS server |
| Group mesh overhead | O(N²) connections for N peers | Practical limit ~10 peers before mesh becomes unwieldy |
| File size limit 5MB | Base64 encoding + WebRTC data channel limits | For larger files, share a link to external storage |
| No persistent identity | Per-session crypto keys | By design — improves privacy |
| MQTT broker is third-party | HiveMQ public broker | Self-host a broker if needed (see §11.2) |
| TURN servers are shared | Open Relay Project is free/public | For higher reliability, run your own TURN server |
| No message persistence across sessions | Per-session crypto keys can't decrypt old messages | Use Export feature for archival |
| QR code is for invite only | QR encodes invite code, not message content | By design — messages are E2E encrypted |
| Calls are 1-on-1 only | WebRTC mesh for calls would be too heavy | Group calls not supported in v4 |
| Forwarding is copy-paste | Full forwarding requires new P2P connection | Acceptable for occasional forwarding |

### 10.2 Troubleshooting

#### "I can't connect to my peer"
1. Check both of you entered the same invite code (case-insensitive, hyphens optional).
2. Check the connection pill in the top bar — it should say "Connecting…" then "Connected".
3. Check your network — corporate firewalls may block WebRTC. Try a different network.
4. Refresh both pages — MQTT signaling sometimes needs a clean retry.
5. If still failing, check browser console for WebRTC errors.

#### "My messages aren't being delivered"
1. Check the connection pill — must be "Connected".
2. Check that you see the "typing…" indicator when your peer types.
3. If using disappearing messages, your messages may have expired — check the TTL.
4. Check that you're not in "Invisible" mode (Settings → Online Status).

#### "Voice/video call fails"
1. Browser must have camera/mic permission.
2. Page must be served over HTTPS (not `file://` or HTTP).
3. Check browser console for `getUserMedia` errors.
4. Some corporate networks block WebRTC media — try a different network.

#### "Sound effects aren't playing"
- Check Settings → Sound effects is on.
- Browser may block autoplay audio until you interact with the page. Click anywhere first.
- Mobile browsers (especially iOS) require the page to be in the foreground.

#### "My fingerprint doesn't match my peer's"
This indicates a man-in-the-middle attack or a network issue:
1. Disconnect immediately.
2. Both sides should refresh the page to generate new keypairs.
3. Verify fingerprints again via out-of-band channel (phone call).
4. If still mismatched, try a different network — your ISP may be intercepting traffic.

#### "I can't see the emoji picker"
- Click the 😊 button in the composer toolbar.
- Press `Esc` to close any other open modal first.
- Check that the picker isn't hidden behind another element.

#### "The QR scanner doesn't work"
- Browser must have camera permission.
- Page must be served over HTTPS.
- Some browsers require user gesture — click "Scan QR" explicitly.
- Check browser console for `getUserMedia` errors.

#### "My poll votes aren't syncing"
- Both peers must be connected (check connection pill).
- Poll votes use the same encrypted channel — if messages work, votes should too.
- Refresh both pages if sync is stuck.

#### "Disappearing messages aren't deleting"
- Check the TTL setting (⏱️ button in chat header).
- The setting syncs with your peer — both sides must support it.
- Cleanup runs every 5 seconds — there may be a slight delay.

#### "The app is slow on mobile"
- Try a smaller UI scale (Settings → UI Scale → S).
- Reduce the number of messages in chat (use `/clear`).
- Disable sound effects if you have many messages.
- WebRTC connections can be battery-intensive — leave the chat when not in use.

#### "I'm seeing someone else's messages"
This shouldn't happen — each chat has a unique invite code. If it does:
1. You may have joined the wrong chat — check the invite code.
2. Someone may have guessed your invite code — generate a new one.
3. Use the `/leave` command and start a new chat.

#### "History restore shows old messages"
- This is by design — the History feature lets you restore past conversations.
- To wipe history entirely, click "Wipe Local Data" on the home screen.

---

## 11. Extending ZeroDay Meet Chat

The codebase is structured to make several extensions straightforward.

### 11.1 Self-hosted MQTT broker

For full control over signaling, run your own MQTT broker:

1. Install Mosquitto or EMQX on your server.
2. Enable WebSocket transport (e.g. `wss://your-broker.com:8884/mqtt`).
3. In the code, change `MQTT_RELAY.brokerUrl` to your broker URL.

Your broker only sees WebRTC signaling — never message content (which flows over the encrypted P2P channel).

### 11.2 Self-hosted TURN server

For better reliability behind strict NATs:

1. Install coturn on your server.
2. Configure with a range of ports for UDP relay.
3. Add your TURN server to the `ICE_SERVERS` array in the code.

```javascript
{ urls:'turn:your-server.com:3478', username:'youruser', credential:'yourpass' }
```

### 11.3 Custom theme

Add a new theme by:

1. Define CSS variables in a `[data-theme="yourtheme"]` block.
2. Add a new swatch to the theme grid in the settings drawer.
3. The theme persists via `localStorage` automatically.

```css
[data-theme="ocean"] {
  --bg: #001a2c;
  --bg-soft: #002b45;
  --bg-elev: rgba(0, 50, 80, 0.85);
  --accent: #00b4d8;
  --accent-2: #90e0ef;
  --accent-glow: rgba(0, 180, 216, 0.45);
}
```

### 11.4 E2E encrypted group calls

Currently, voice/video calls are 1-on-1 only. To add group calls:

1. Use SFU (Selective Forwarding Unit) like mediasoup or Janus.
2. Or mesh the media streams (heavy on bandwidth).
3. Modify `ZeroDayCall` to accept multiple peer connections.

### 11.5 Message search across all conversations

Currently, search is per-chat. To add global search:

1. Iterate over all items in IndexedDB.
2. Search each conversation's message log.
3. Display results with conversation context.

### 11.6 File transfer improvements

Currently, files are sent as base64 data URLs (limited to 5MB). To support larger files:

1. Use WebRTC data channel binary transfers (chunked).
2. Show progress bar during transfer.
3. Support resume on interruption.

### 11.7 Mobile app wrapper

To package as a mobile app:

1. Use Capacitor or Cordova to wrap the HTML.
2. Add camera/mic permissions to the manifest.
3. Use the platform's native notification system.

### 11.8 Bot / scripting support

Add slash commands that trigger scripts:

1. Extend the `COMMANDS` array with bot commands.
2. Bot commands can fetch data, run calculations, etc.
3. For security, restrict bot commands to local execution only.

### 11.9 Message threading

Currently, replies appear inline. To add threading:

1. Add a `thread` field to messages.
2. Render threaded replies as a collapsible tree.
3. Add UI to expand/collapse threads.

### 11.10 End-to-end encrypted file storage

For larger files without base64 overhead:

1. Encrypt the file with the shared AES-GCM key.
2. Upload to a storage service (S3, IPFS, etc.).
3. Send the URL + decryption key over the existing encrypted channel.
4. Recipient downloads and decrypts locally.

---

## 12. Appendix

### 12.1 Quick command reference card

```
HELP        /help                    Show all commands
IDENTITY    /nick <name>             Change your nickname
            /status <text>           Set custom status
            /online | /idle | /dnd | /invisible
            /fingerprint              Show your E2E fingerprint

MESSAGING   /me <action>             Send an action (italicized)
            /shrug [text]            Append ¯\_(ツ)_/¯
            /pin <id>                Toggle pin on message
            /star <id>              Star a message
            /forward                 Forward last message
            /info <id>              Show message info
            /search <query>         Search messages
            /export [json|txt]      Export conversation

CALLS       /call                    Start voice call
            /video                   Start video call
            /voice                   Record voice message
            /poll                    Create a poll
            /schedule                Schedule a message
            /disappear [seconds]    Set disappearing messages
            /mute                    Toggle mute

NAVIGATION  /leave                   Leave current chat
            /next                    Find next stranger (stranger mode)
            /clear                   Clear conversation locally
            /topic <text>            Set room topic (group only)

PALETTE     Ctrl/Cmd+K              Open command palette
```

### 12.2 Default settings

| Setting | Default |
|---|---|
| Display name | `Anonymous` |
| Avatar letter | `A` |
| Custom status | (empty) |
| Online status | `online` |
| Theme | `midnight` |
| UI scale | `1` (auto) |
| Read receipts | on |
| Typing indicator | on |
| Sound effects | on |
| Enter to send | on |
| Auto-resume | on |
| Save history | on |
| Reduce motion | off |

### 12.3 Glossary

| Term | Meaning |
|---|---|
| **Invite code** | A 9-character string (e.g. `A4F-9X2-K7P`) that identifies a chat room. |
| **Fingerprint** | SHA-256 hash of a peer's public key, shown as 8-character hex groups. Used to verify no MITM attack. |
| **Mesh** | A network topology where each peer connects to every other peer (used in group chat). |
| **Signaling** | The exchange of WebRTC offer/answer/ICE candidates via MQTT, before direct P2P connection. |
| **STUN** | Session Traversal Utilities for NAT — helps peers discover their public IP for P2P. |
| **TURN** | Traversal Using Relays around NAT — relays traffic when P2P fails (used for strict NATs). |
| **ECDH** | Elliptic Curve Diffie-Hellman — key exchange algorithm used to derive shared secrets. |
| **AES-GCM** | Advanced Encryption Standard with Galois/Counter Mode — symmetric encryption with authentication. |
| **HMAC** | Hash-based Message Authentication Code — used for tamper detection. |
| **Nonce** | Number used once — unique per message, prevents replay attacks. |
| **CSP** | Content Security Policy — restricts what scripts/sources can run. |
| **Data channel** | WebRTC's bidirectional byte stream between two browsers. |
| **Disappearing messages** | Messages that auto-delete after a TTL (30s, 5m, 1h, 24h, 7d). |
| **Starred messages** | Messages marked with ⭐ for later reference (local only). |
| **Pinned messages** | Messages marked with 📌 and shown in a dedicated panel. |
| **Quick reactions** | 6 emoji that appear above a message on hover for fast reacting. |
| **Command palette** | Ctrl+K interface for running commands quickly. |
| **Auto-resume** | Setting that restores the last chat session on page reload. |

### 12.4 Default endpoints

| Service | URL | Purpose |
|---|---|---|
| **MQTT broker** | `wss://broker.hivemq.com:8884/mqtt` | Public signaling channel |
| **STUN (Google 1)** | `stun:stun.l.google.com:19302` | NAT traversal |
| **STUN (Google 2)** | `stun:stun1.l.google.com:19302` | NAT traversal |
| **STUN (Google 3)** | `stun:stun2.l.google.com:19302` | NAT traversal |
| **STUN (Cloudflare)** | `stun:stun.cloudflare.com:3478` | NAT traversal |
| **STUN (Nextcloud)** | `stun:stun.nextcloud.com:443` | NAT traversal |
| **TURN (Open Relay UDP)** | `turn:openrelay.metered.ca:80` | Relay for strict NATs |
| **TURN (Open Relay TLS)** | `turn:openrelay.metered.ca:443` | Relay for strict NATs |
| **TURN (Open Relay TCP)** | `turn:openrelay.metered.ca:443?transport=tcp` | Relay for strict NATs |
| **CDN (jsdelivr)** | `https://cdn.jsdelivr.net` | Library hosting (mqtt.js, qrcode, jsQR) |

### 12.5 Storage keys

| Key | Storage | Content |
|---|---|---|
| `zdmc.v3.settings` | localStorage | All user settings |
| `zdmc.v3.session` | localStorage | Current chat session info |
| `zdmc.v4.draft` | localStorage | Per-invite-code message drafts |
| `zdmc-v3` (DB) | IndexedDB | Conversation history store |

### 12.6 Crypto algorithms summary

| Purpose | Algorithm | Key size |
|---|---|---|
| Key exchange | ECDH (P-256 / prime256v1) | 256-bit |
| Message encryption | AES-GCM | 256-bit |
| Tamper detection | HMAC-SHA256 | 256-bit |
| HMAC key derivation | HKDF-SHA256 | 256-bit |
| Fingerprint | SHA-256 | 256-bit |
| IV/nonce | Random | 96-bit (12 bytes) |

### 12.7 Message type reference

| Type | Direction | Payload fields |
|---|---|---|
| `hello` | out | name, avatar, pubKey, status, customStatus |
| `text` | out | body, id, ts, replyTo |
| `file` | out | name, size, dataUrl, ts, id |
| `voice` | out | dataUrl, duration, ts, id |
| `poll` | out | question, options, votes, ts, id |
| `poll-vote` | out | id, option, voter |
| `reaction` | out | id, emoji, users |
| `edit` | out | id, body |
| `delete` | out | id |
| `pin` | out | id, value |
| `typing` | out | value |
| `read` | out | id |
| `presence` | out | value |
| `clear-req` | out | (none) |
| `dm-setting` | out | ttl |
| `call-offer` | out | sdp, video |
| `call-answer` | out | sdp |
| `call-signal` | out | sub, c |
| `call-end` | out | (none) |
| `call-reject` | out | (none) |
| `stranger-next` | out | (none) |

### 12.8 Final notes

ZeroDay Meet Chat v4 is a single-file demo of how much real-time, secure web functionality can be packed into a browser with zero backend. Every feature — Discord-grade messaging, voice/video calls, end-to-end encryption, voice messages, polls, scheduled messages, command palette — works without any server code you control.

The trade-offs (no cross-session persistence, single HTML file size, 1-on-1 calls only) are deliberate choices that keep the app simple, free, instantly deployable, and trivially auditable.

For sensitive or production use cases:
- Self-host your MQTT broker and TURN server (see §11.1, §11.2).
- Verify fingerprints out-of-band for every conversation.
- Use disappearing messages with short TTLs for sensitive content.
- Wipe local data after each session.

For everything else, the default configuration works out of the box — open the file, generate an invite code, share it, and start chatting with end-to-end encryption that even the relay operators can't break.
