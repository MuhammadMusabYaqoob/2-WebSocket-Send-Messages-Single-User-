# WebSocket Communication with Node.js

## Overview

In this practice project, I learned how to establish a WebSocket connection between a single client (Browser) and a single server (Node.js).

The goal is to understand how a client connects to a server, how both sides send messages, and how they receive messages through the same WebSocket connection.

## Technologies Used

- Node.js
- JavaScript
- WebSocket (`ws` package)
- HTML

## 1. Project Setup

Initialize the Node.js project:

```bash
npm init -y
```

Install the WebSocket package:

```bash
npm install ws
```

Set the following in `package.json` to use ES modules:

```json
"type": "module"
```

## 2. Project Structure

```text
Real-Time-Communication/
│
├── webSocket.js
├── index.html
├── package.json
└── node_modules/
```

- `webSocket.js` contains the WebSocket server.
- `index.html` contains the browser client.

## 3. Understanding WebSocket Connection (Step by Step)

For this practice, we have one server and one client.

We will follow the execution flow from creating the server to exchanging messages between the client and server.

### Step 1: Create the WebSocket Server

**Server — webSocket.js**

```js
import { WebSocketServer } from "ws";

// Step 1: Creating WebSocket Server and waiting for clients
const server = new WebSocketServer({ port: 3000 });
```

**Explanation:**

- Import `WebSocketServer` from the `ws` package.
- Create a WebSocket server.
- The server starts listening on port `3000`.
- It waits for clients to request a connection.

At this point, the server is running, but no client is connected yet.

### Step 2: Create a WebSocket Client and Request a Connection

**Client — index.html**

```js
// Step 2: Creating a WebSocket client and requesting a connection
const socket = new WebSocket("ws://localhost:3000");
```

**Explanation:**

- `WebSocket()` is a built-in browser JavaScript API.
- `ws://` specifies the WebSocket protocol.
- `localhost` refers to the local machine.
- `3000` is the port where our server is listening.

This line initiates a connection request from the client to the server.

### Step 3: Client Sends a Message After the Connection Opens

**Client — index.html**

```js
// Step 3: When the WebSocket connection is successfully established,
// the client sends a message to the server.
socket.onopen = () => {
    socket.send("Hi I am Client");
};
```

**Explanation:**

- `onopen` executes when the WebSocket connection is successfully established.
- `socket.send()` sends a message from the client to the server.
- The client sends `"Hi I am Client"`.

### Step 4: Server Detects the Client and Sends a Message

**Server — webSocket.js**

```js
// Step 4: When a client successfully connects,
// the server detects the connection and sends a welcome message.
server.on("connection", (socket) => {

    console.log("Client Found");

    // Server sends a message to this client
    socket.send("Server: Hi");

});
```

**Explanation:**

- `server.on("connection", ...)` listens for new client connections.
- When the client successfully connects, the callback executes.
- The `socket` represents the connection with that particular client.
- `console.log()` prints `"Client Found"` in the server terminal.
- `socket.send()` sends `"Server: Hi"` to the client.

### Step 5: Server Receives and Prints the Client's Message

**Server — webSocket.js**

```js
// Step 5: Server listens for messages from the client
// and prints the received message in the terminal.
socket.on("message", (message) => {

    console.log(message.toString());

});
```

**Explanation:**

- `socket.on("message", ...)` listens for incoming messages from the client.
- When the client sends a message, this callback executes.
- `message.toString()` converts the received message into a readable string.
- `console.log()` prints the message in the VS Code terminal.

### Step 6: Client Receives the Server's Message

**Client — index.html**

```js
// Step 6: When the client receives a message from the server,
// execute this function.
socket.onmessage = (event) => {

    console.log("Server says:", event.data);

};
```

**Explanation:**

- `onmessage` listens for incoming messages from the server.
- `event.data` contains the actual message sent by the server.
- `console.log()` displays the received message in the browser console.

---

## 4. Complete Code

### Server — webSocket.js

```js
import { WebSocketServer } from "ws";

// Step 1: Creating WebSocket Server and waiting for clients
const server = new WebSocketServer({ port: 3000 });

// Step 4: When a client successfully connects,
// the server detects the connection and sends a welcome message.
server.on("connection", (socket) => {

    console.log("Client Found");

    // Server sends a message to this client
    socket.send("Server: Hi");

    // Step 5: Server listens for messages from the client
    // and prints the received message in the terminal.
    socket.on("message", (message) => {

        console.log(message.toString());

    });

});
```

### Client — index.html

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>WebSocket Client</title>
</head>

<body>

    <h1>WebSocket Client</h1>

    <script>

        // Step 2: Creating a WebSocket client and requesting
        // a connection with the server running on port 3000.
        const socket = new WebSocket("ws://localhost:3000");

        // Step 3: When the WebSocket connection is successfully
        // established, the client sends a message to the server.
        socket.onopen = () => {
            socket.send("Hi I am Client");
        };

        // Step 6: When the client receives a message from the server,
        // execute this function.
        socket.onmessage = (event) => {
            console.log("Server says:", event.data);
        };

    </script>

</body>

</html>
```

---

## 5. Complete Communication Flow

We have one client and one server communicating through a single WebSocket connection.

| Step | Side | Action |
|---|---|---|
| 1 | Server | Creates WebSocket server on port 3000 |
| 2 | Client | Requests a connection with the server |
| 3 | Client | Sends `"Hi I am Client"` after connection opens |
| 4 | Server | Detects the client and sends `"Server: Hi"` |
| 5 | Server | Receives and prints `"Hi I am Client"` |
| 6 | Client | Receives and prints `"Server: Hi"` |

### Visual Communication Flow

```text
             SERVER (Node.js)                 CLIENT (Browser)
                    |                                |
             Step 1: Start                           |
             Listen on port 3000                     |
                    |                                |
                    | <------------------------------|
                    |              Step 2            |
                    |        Request connection      |
                    |                                |
                    | <------------------------------|
                    |              Step 3            |
                    |        "Hi I am Client"        |
                    |                                |
             Step 4: Detect client                   |
             Send "Server: Hi" --------------------->|
                    |                                |
             Step 5: Receive and                     |
             print client message                   |
                    |                                |
                    |              Step 6            |
                    |        Receive "Server: Hi"    |
                    |                                |
```

## 6. Expected Output

**VS Code Terminal (Server):**

```text
Client Found
Hi I am Client
```

**Browser Console (Client):**

```text
Server says: Server: Hi
```

## 7. Important Concepts Learned

| Concept | Meaning |
|---|---|
| `WebSocketServer` | Creates a WebSocket server in Node.js |
| `new WebSocket()` | Creates a browser client and initiates a connection |
| `server` | Represents the WebSocket server |
| `socket` | Represents a connection between one client and the server |
| `connection` | Server event that detects a new client |
| `onopen` | Client event that runs when the connection opens |
| `send()` | Sends a message through the connection |
| `message` | Server event that receives a client message |
| `onmessage` | Client event that receives a server message |

**Important:** Both the client and server have their own `socket` objects representing the same connection. Either side can send and receive messages.

## Project Status

Completed:
- Created a WebSocket server.
- Created a browser WebSocket client.
- Established a connection between one client and one server.
- Sent a message from client to server.
- Sent a message from server to client.
- Received messages on both sides.

## Next Practice: Multiple WebSocket Clients

In this project, I learned how to establish a WebSocket connection between one client and one server.

In the next practice project, I will explore how to connect multiple clients (users) to a single WebSocket server and manage their individual connections.

**Next Repository:** [Multiple WebSocket Clients](https://github.com/MuhammadMusabYaqoob/WebSocket---Multiple-Users)

Topics to explore:
- Connecting multiple clients to one WebSocket server.
- Managing multiple client socket connections.
- Sending messages to specific clients.
- Broadcasting messages to all connected clients.
- Handling client disconnections.
