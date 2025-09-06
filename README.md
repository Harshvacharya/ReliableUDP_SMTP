# ReliableUDP_SMTP

## Overview
A two-part systems project implemented in C:

1. **ReliableUDP** — a library and demo implementing **Go-Back-N**–style reliable data transfer over UDP (window-based flow control, loss handling).  
2. **SMTP** — a concurrent, TCP-based **mail server and client** implementing core SMTP (RFC 5321) and POP3 (RFC 1939) workflows for sending and retrieving email.

Repo layout:

```
ReliableUDP_SMTP/
├─ ReliableUDP/   # Go-Back-N over UDP: library + test programs
├─ SMTP/          # TCP-based SMTP/POP3 server & client
└─ README.md
```

## ReliableUDP (Go-Back-N)
### Highlights
- Sliding window, sequence numbers, ACK/timeout logic.
- Packet loss/duplication tolerance on top of UDP.
- Suitable as a teaching/demo library for transport-layer reliability.

### Build
```bash
cd ReliableUDP
make
```

### Run (example workflow)
```bash
# in one terminal
./server_demo <port>

# in another terminal
./client_demo <server_ip> <port> <file_to_send>
```

## SMTP (server & client)
### Highlights
- **SMTP server/client** for sending mail; **POP3** server/client for retrieval.
- Concurrent server using threads.
- Demonstrates protocol parsing, session state, and multi-user handling.

### Build
```bash
cd SMTP
make
```

### Run (example workflow)
```bash
# Start the SMTP server
./smtp_server <listen_port>

# Send mail with client
./smtp_client <server_ip> <port>   --from alice@example.com   --to bob@example.com   --subject "Hello"   --data "Hi from ReliableUDP_SMTP!"

# Start the POP3 server (if in repo) and fetch with a POP3 client
./pop3_server <listen_port>
./pop3_client <server_ip> <port> --user bob --pass secret
```

## Testing
- The ReliableUDP component can be tested with different window sizes and simulated loss rates.  
- SMTP/POP3 can be tested end-to-end by sending, storing, and retrieving messages with multiple clients.

## Implementation Notes
- **ReliableUDP:** timers for retransmission, cumulative ACKs, and sender window advancement.
- **SMTP/POP3:** core command sets like `HELO`, `MAIL FROM`, `RCPT TO`, `DATA`, `USER`, `PASS`, `LIST`, `RETR`, etc.

## Security & Environment
- Designed for **local/demo environments**.  
- Add TLS and stronger authentication if used on a real network.

