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

sequenceDiagram
    autonumber
    actor U as Telegram User
    participant T as Telegram Bot
    participant R as JobRunner V2
    participant Q as Job Queue
    participant W as Worker(1..N)
    participant F as ChatFunctions.js
    participant WA as Baileys Socket
    participant X as WhatsApp Target

    U->>T: /xthieves 6281234567890
    T->>T: Cek role & WA_READY
    T->>T: Normalisasi nomor -> JID (62…@s.whatsapp.net)
    T->>R: enqueue({ funcName: 'promoSafe', jid })
    R->>Q: Push job ke queue (stats.enqueued++)
    Note right of Q: Menunggu worker siap

    W->>Q: Ambil job
    W->>W: Rate-limit + jitter + (optional) idlePing()
    W->>F: Call promoSafe(sock, jid)
    F->>WA: sock.sendMessage(jid, payload)
    WA-->>X: Kirim pesan ke target

    WA-->>F: Sukses / Error
    alt Sukses
      F-->>W: OK
      W->>R: stats.succeeded++ & catat recent job
    else Error & bisa retry
      W->>W: backoff (exponential)
      W->>Q: Re-enqueue (stats.retried++)
      Note right of W: Drop kalau > MAX_RETRY (stats.failed++)
    end

    U->>T: /dash
    T->>R: getStats()
    R-->>T: Snapshot (queue, workers, enq/ok/retry/fail, uptime, recent jobs, errors)
    T-->>U: Kirim caption (block code) sebagai dashboard
