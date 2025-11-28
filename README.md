# Peer-to-peer Communication (Improved README)

This repository provides a simple and reliable way to transfer files (images, videos, PDFs, etc.) over **WebRTC Data Channels**.

## Background

Over the past few months, I have been working with WebRTC live streams and one-to-one chats. However, when it came to file transfers, the system often crashed.

### Why Crashes Happened

- `dataChannel.bufferedAmount()` gets full, causing the system to stop responding.

### Solution

Implement a listener to detect when the buffered amount drops below the safe limit (`16KiB`).

```js
if (dataChannel.bufferedAmount > dataChannel.bufferedAmountLowThreshold) {
  dataChannel.onbufferedamountlow = () => {
    dataChannel.onbufferedamountlow = null;
    send();
  };
  return;
}
```

---

## Run Locally

Clone the project:

```bash
git clone https://github.com/SevenSquare-Tech/local-network-file-transfer.git
```

Go to the project directory:

```bash
cd local-network-file-transfer
```

Install dependencies:

```bash
npm install
```

Start the server:

```bash
npm start
```

Default server runs at **http://localhost:3000**.

---

## Establishing Connection

To establish a peer-to-peer connection between two users:

Assume: 👨‍🎓 George and 🙋‍♀️ Anna  
George initiates the connection.

1. **George:** Open `http://localhost:3000` after running `npm start`.
2. **Anna:** Do the same on her machine.
3. **George:** Click **Create Local Offer**, copy the generated text, and send it to Anna via WhatsApp/Telegram (acting as a temporary signaling method).
4. **Anna:** Paste George’s offer into **Remote Offer**, click **Connect**, then copy her generated **Local Offer** and send it back to George.
5. **George:** Paste Anna’s text into **Remote Offer**, click **Connect**.

If successful, the status will show **Connected to peer**.

> ⚡ In future updates, WebSockets will replace manual text sharing for signaling.

---

## Sending Files

Once connected, file transfer works both ways:

- Click **Choose File** and select any file to send.
- Transfer speed depends on both users’ network speed.
- If behind a **NAT**, speeds may be slower because the included TURN servers are basic.

---

## Protocols Used

- **SDP (Session Description Protocol)**

---
