# LAN Quiz application

A cross-platform web application for hosting and participating in interactive quizzes within a local area network (LAN). No internet connection required — designed to run fully offline on a standard Windows laptop.

Built as a semester project (Year 2) with further development planned for a bachelor's thesis.

---

## 📅 Project Status

This repository is part of a **Year 2 Semester Project**. The current phase focuses on architecture design, documentation, and planning.

Full implementation is planned for the **Bachelor's Thesis (Year 3)**, which will include:
- Complete backend and frontend implementation
- Load testing with real participant groups
- Expanded game modes and quiz types

---

## 🎯 Motivation

Interactive quiz platforms like Kahoot or Quizizz require a stable internet connection and external servers. This makes them unreliable or unusable in environments such as summer camps, fieldwork locations, or classrooms with poor connectivity.

LAN Quiz solves this by running entirely on the host's laptop, accessible to participants via a local WiFi network — no accounts, no internet, no external dependencies.

---

## ✨ Features

- 🧠 **Multiple game modes** — Buzzer, Kahoot-style, and Jeopardy-style (Risk Mode)
- 📱 **Mobile-friendly player view** — participants join via browser on their phones
- 📺 **Projector host view** — questions, answers, timer, and leaderboard displayed for the whole room
- 🌐 **Offline LAN operation** — no internet required at runtime
- 📶 **Two connection modes** — join existing WiFi or let the laptop create its own hotspot
- 🔗 **QR code joining** — one scan connects and opens the app
- 📝 **Quiz editor** — create and manage quizzes in advance, import/export via JSON
- 💾 **Persistent storage** — SQLite database, all quizzes saved between sessions

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────┐
│                   Host Laptop                       │
│                                                     │
│  ┌─────────────┐        ┌────────────────────────┐  │
│  │  Spring Boot│◄──────►│  React (Host View)     │  │
│  │  Backend    │  REST  │  Projector screen      │  │
│  │             │  + WS  └────────────────────────┘  │
│  │  SQLite DB  │        ┌────────────────────────┐  │
│  │  (single    │◄──────►│  React (Quiz Editor)   │  │
│  │   file)     │        └────────────────────────┘  │
│  └──────┬──────┘                                    │
│         │ WebSocket (STOMP)                         │
└─────────┼───────────────────────────────────────────┘
          │ LAN / Hotspot
          ▼
┌─────────────────┐
│  Participant    │
│  Phone Browser  │
│                 │
│  React (Player  │
│  View)          │
│  - answer btns  │
│  - buzzer btn   │
└─────────────────┘
```

### Tech Stack

| Layer | Technology | Reason |
|---|---|---|
| Backend | Java + Spring Boot | Embedded Tomcat, no installation needed, supervisor's domain |
| Database | SQLite | Single portable file, persists between restarts |
| Frontend | React | PWA-friendly, works in mobile browsers without installation |
| Real-time | WebSocket (STOMP) | Low latency, minimal payload for older devices |

---

## 🎮 Game Modes

### Buzzer Mode
First participant to tap the button earns the right to answer out loud.
Host confirms correctness and awards points manually.

### Kahoot Mode
All participants answer within a time limit via colored buttons on their phones.
Score formula: `score = maxPoints × (1 - t/T) × isCorrect`

### Risk Mode *(TBA)*
A Jeopardy-style board of questions grouped by category and point value.
Correct answer = points gained. Wrong answer = points lost.
Details and rules TBA.

---

## 🌐 Network Setup

Two connection modes, selected manually at startup:

```
How will participants connect?
  ◉ They are already on the same network as me   → generates QR with current IP
  ○ I will create a hotspot for them              → starts hotspot, then generates QR
```

**Mode A — Existing network:** Host and participants are on the same WiFi. QR encodes `http://<laptop-ip>:8080`.

**Mode B — Laptop hotspot:** App starts a Windows Mobile Hotspot via PowerShell. QR contains both network credentials and server URL — one scan connects the phone to the hotspot and opens the app.

---

## 🗄️ Data Model

> TBA

---

## 🚀 Getting Started

> TBA

---

## 👤 Author

> Ivan Shulha / FMFI UK / 2026
