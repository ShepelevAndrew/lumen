# 🔌 SSE for ASP.NET Core

A lightweight, configurable **Server-Sent Events (SSE)** library for ASP.NET Core.  
Easily add real-time, uni-directional server-to-client messaging without WebSockets.

---

## ✨ Features

- ✅ Simple integration with minimal APIs
- 🧩 Middleware-based connection handling
- 🧠 Supports multiple clients & devices per user
- 🔄 Built-in ping/keep-alive
- 🔧 Fully configurable endpoints and connection logic
- 📦 .NET 6/7/8 compatible

---

## 📦 Installation

Install via NuGet:

```bash
dotnet add package Lumen.SSE
```

---

## 🚀 Usage

#### 1. Configure services

```csharp
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddSse();
```

#### 2. Configure middleware

```csharp
var app = builder.Build();
app.UseSse();
```

#### 3. Send events

```csharp
app.MapPost("/send-message", async (
    Message message,
    ISseBuilder sse)
    => await sse.Build()
        .SetEventName("new_message")
        .SetData(message)
        .SendToAllClientsAsync(clientId));
```

### 📡 Client-Side (JavaScript)

```js
const sseUrl = `https://localhost:7287/sse/connection`;
const eventSource = new EventSource(sseUrl);

eventSource.onopen = () => {
    console.log("✅ Connected to SSE server");
};

eventSource.addEventListener("new_message", (event) => {
    const parsedData = JSON.parse(event.data);
    console.log("📨 New Message:", parsedData);
});

eventSource.onerror = (err) => {
    console.error("🚨 SSE connection error:", err);
};
```

---

## 🔍 SSE vs WebSockets

| Feature              | **SSE (Server-Sent Events)**                           | **WebSockets**                                      |
|---------------------|---------------------------------------------------------|-----------------------------------------------------|
| **Direction**        | Server ➜ Client (one-way)                              | Bi-directional (Client ⬄ Server)                    |
| **Protocol**         | HTTP/1.1 (works with HTTP/2 partial support)           | Custom over TCP (upgrades from HTTP)                |
| **Browser Support**  | Widely supported, except for IE/old Edge               | Widely supported                                    |
| **Complexity**       | Simple to implement                                     | More complex (stateful, needs connection management)|
| **Reconnection**     | Built-in automatic reconnection                        | Manual reconnection logic                           |
| **Use Case**         | Notifications, live feeds, updates                     | Chat apps, multiplayer games, complex interaction   |
| **Proxy-Friendly**   | Yes                                                    | Sometimes blocked by firewalls/proxies              |

🟢 **Use SSE** when:
- You only need **server-to-client** updates
- You want a **simple**, **scalable**, and **HTTP-friendly** solution

🟡 **Use WebSockets** when:
- You need **real-time** two-way communication
- The client should also be able to **push** events to the server

---

💡 **Why choose Lumen (SSE)?**
- Zero dependencies
- High performance for broadcasting updates
- Great for lightweight, read-only real-time applications

---

## 💬 Questions? Feedback?

Feel free to [open an issue](https://github.com/your-repo/issues) or contact the maintainer.

---
