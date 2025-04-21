# 05 - TCP and UDP

## TCP (Transmission Control Protocol)

### Characteristics:

- **Connection-oriented**: A connection must be established before data can be sent.
- **Reliable**: Ensures delivery of packets in the correct order without loss or duplication.
- **Error Checking**: Includes mechanisms for retransmission, flow control, and congestion control.
- **Slower** than UDP due to overhead of maintaining state and reliability.

---

### TCP 3-Way Handshake

1. **SYN (Synchronize)**: Client sends a TCP packet with SYN flag to server.
2. **SYN-ACK (Synchronize-Acknowledge)**: Server responds with SYN and ACK.
3. **ACK (Acknowledge)**: Client sends final ACK, and the connection is established.

Once the handshake completes, data transfer begins.

---

### TCP Connection Termination

- Connection ends with a **4-step teardown** using FIN and ACK flags.
  - One side sends FIN → other responds with ACK.
  - Then reverse happens for the other direction.

---

### Use Cases for TCP

- Web browsing (HTTP/HTTPS)
- Email (SMTP, IMAP)
- File transfers (FTP)
- Remote login (SSH)

---

## UDP (User Datagram Protocol)

### Characteristics:

- **Connectionless**: No handshake or setup.
- **Unreliable**: No guarantees of delivery, ordering, or duplication protection.
- **Faster** than TCP due to minimal overhead.
- Suitable for applications where speed is more critical than reliability.

---

### How UDP Works

- Each UDP packet (datagram) is sent independently.
- The receiver does not send acknowledgements.
- No guarantee that packets arrive in order—or at all.

---

### Use Cases for UDP

- Real-time applications (voice/video calls, gaming, streaming).
- DNS lookups.
- IoT sensors sending frequent small bursts of data.
