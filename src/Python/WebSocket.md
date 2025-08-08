# WebSocket Fundamentals & Flask Tutorial

## 1. Understanding WebSockets

**WebSockets** provide a persistent, bidirectional communication channel between the client and server over a single TCP connection.

### Key Features

- **Full-duplex communication**: Both client and server can send messages at any time.
- **Low latency**: Avoids repeated HTTP requests (polling).
- **Event-driven**: Data is pushed when ready.
- **Used in**: Chat apps, live notifications, multiplayer games, collaborative apps.

## 2. WebSockets vs HTTP

| Feature           | HTTP (REST) | WebSocket |
|-------------------|-------------|-----------|
| Communication     | Request/Response | Full-Duplex |
| Connection        | Short-lived | Persistent |
| Latency           | Higher (polling) | Low |
| Data Flow         | Client initiates | Either can initiate |
| Best For          | CRUD APIs | Real-time updates |

## 3. How WebSockets Work

1. Client sends a **WebSocket handshake request** (over HTTP).
2. Server responds with a **WebSocket handshake acceptance**.
3. HTTP connection is **upgraded** to WebSocket protocol (`ws://` or `wss://`).
4. Messages are exchanged using **frames** until the connection closes.

## 4. WebSocket with Flask

Flask does not natively support WebSockets, but we can use **Flask-SocketIO**.

### Installation

```bash
pip install flask-socketio
```

## 5. Example: Simple Chat Application

### Server Code (Flask + SocketIO)

```python
#!/usr/bin/env python3
from flask import Flask, render_template
from flask_socketio import SocketIO, send

app = Flask(__name__)
app.config["SECRET_KEY"] = "secret!"
socketio = SocketIO(app)

@app.route("/")
def index():
    return render_template("chat.html")

@socketio.on("message")
def handle_message(msg):
    print(f"Received message: {msg}")
    send(msg, broadcast=True)  # Send to all clients

if __name__ == "__main__":
    socketio.run(app, debug=True)
```

### Client Code (`templates/chat.html`)

```html
<!DOCTYPE html>
<html>
<head>
    <title>WebSocket Chat</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/socket.io/4.0.1/socket.io.js"></script>
</head>
<body>
    <h1>Flask-SocketIO Chat</h1>
    <input id="message" autocomplete="off">
    <button onclick="sendMessage()">Send</button>
    <ul id="messages"></ul>

    <script>
        var socket = io();

        socket.on("message", function(msg) {
            var li = document.createElement("li");
            li.textContent = msg;
            document.getElementById("messages").appendChild(li);
        });

        function sendMessage() {
            var msg = document.getElementById("message").value;
            socket.send(msg);
            document.getElementById("message").value = "";
        }
    </script>
</body>
</html>
```

## 6. Running the App

1. Save the Python code in `app.py`.
2. Save HTML in `templates/chat.html`.
3. Run:

```bash
python app.py
```

4. Visit `http://localhost:5000` in multiple browser tabs and send messages.

## 7. Secure WebSockets

- Use **`wss://`** for TLS encryption.
- Restrict allowed origins with `cors_allowed_origins` in `SocketIO()`.
- Authenticate users before opening the WebSocket connection.

## 8. Common Use Cases

- Chat applications
- Live notifications
- Stock market tickers
- Multiplayer games
- IoT device communication

## 9. References

- [Flask-SocketIO Documentation](https://flask-socketio.readthedocs.io/)
- [MDN WebSocket Guide](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API)

---

