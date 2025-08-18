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
