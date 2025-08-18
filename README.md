# 🤖 Ovalium AI Agent V2

AI Agent promosi Telegram → WhatsApp berbasis **Baileys** + **Node.js** + **node-telegram-bot-api**.  
Didesain modular dengan JobRunner V2 (queue, worker, retry, stats), support multi-session WhatsApp, auto-proxy idle, dan integrasi dashboard via command Telegram.

---

## ✨ Fitur Utama
- 🚀 **Multi WhatsApp Session** (sinkron dengan `initializeWhatsAppConnections`)
- 📬 **JobRunner V2**
  - Queue & worker paralel
  - Retry + backoff
  - Rate-limit + jitter (anti spam)
  - Mini dashboard dengan `/dash`
- 💬 **Chat Functions Modular**
  - `promoSafe`
  - `productInfo`
  - `followUp`
- 🔒 Role-based Command (`isAdmin`, `isPremium`, `isSupervip`)
- 🌐 Support Auto Proxy Idle (opsional)

---

## 🔄 Arsitektur & Alur Kerja

```mermaid
flowchart TD
    A[Telegram User / Command] -->|/xthieves /dash| B(Telegram Bot)
    B --> C{JobRunner V2}
    C -->|enqueue| D[Job Queue]
    D --> E[Worker 1..N]
    E -->|call| F[ChatFunctions.js]
    F -->|sendMessage| G[WhatsApp Socket (Baileys)]
    G --> H[Target User di WhatsApp]

    C --> I[/Stats & Monitoring/]
    I -->|/dash| B
