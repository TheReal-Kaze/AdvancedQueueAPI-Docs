# 🧩 Kaze.AdvancedQueue API

Welcome to the official documentation for **Kaze.AdvancedQueue**,  
a modular queue management system for *Unturned*.  
This system lets developers and server admins manage player priorities dynamically across servers.

---

## 🧰 Command Usage (In-Game)

If you don’t use the API directly, **Kaze.AdvancedQueue** also provides simple in-game commands for managing priorities.

### 🔹 Add or Update a Player’s Priority
```plaintext
/addpriority <steamid64> <priority>
```

**Example:**
```plaintext
/addpriority 76561198000000000 5
```

Adds or updates a player’s queue priority level.  
If the player already has one, it’s overwritten.  

**Aliases:**  
`/setpriority`, `/priorityadd`

---

### 🔹 Remove a Player’s Priority
```plaintext
/delpriority <steamid64>
```

**Example:**
```plaintext
/delpriority 76561198000000000
```

Removes a player from the priority list.

**Aliases:**  
`/removepriority`, `/prioritydel`

---

## 🚀 API Overview

`Kaze.AdvancedQueue.API` is a static C# API built around the internal `PriorityManager`.  
It provides simple, thread-safe access to queue priority operations.

### 🔧 Import the namespace
```csharp
using Kaze.AdvancedQueue.API;
```

You can then access the API directly using its static methods:
```csharp
AdvancedQueueAPI.UpdatePriority(steamID, 3);
```

---

## 📘 Example Usage
```csharp
using Kaze.AdvancedQueue.API;
using Rocket.Core.Logging;

public void Example(ulong steamID)
{
    // Add or update the player's priority
    AdvancedQueueAPI.UpdatePriority(steamID, 3);

    // Read their priority
    int prio = AdvancedQueueAPI.GetPriorityLevel(steamID);
    Logger.Log($"Player {steamID} now has priority level {prio}.");

    // Remove their priority by setting it to 0
    AdvancedQueueAPI.UpdatePriority(steamID, 0);
}
```

---

## 📂 API Features
- Add or update priority levels dynamically  
- Remove players automatically when priority ≤ 0  
- Read player priorities at runtime  
- Retrieve all cached priority entries  
- Persistent via **LiteDB**

---

## ⚙️ Example Integration (Rocket plugin)
```csharp
using Kaze.AdvancedQueue.API;
using Rocket.Core.Logging;
using Rocket.Core.Plugins;
using SDG.Unturned;

public class QueueDemoPlugin : RocketPlugin
{
    protected override void Load()
    {
        Provider.onEnemyConnected += player =>
        {
            ulong id = player.playerID.steamID.m_SteamID;
            AdvancedQueueAPI.UpdatePriority(id, 5);
            Logger.Log($"[QueueDemo] {id} now has priority {AdvancedQueueAPI.GetPriorityLevel(id)}.");
        };

        Logger.Log($"{Name} loaded and hooked into AdvancedQueue API.");
    }
}
```

---

## 📖 API Reference
➡ [View the full API reference](api-reference.md)

---

## 🧱 About

**Kaze.AdvancedQueue** is designed to make queue management modular, persistent, and easily extendable.  
The API can be used directly by other developers or through in-game commands to manage ranks, priority slots, or reserved-access systems.

---

*(Documentation generated with ❤️ & ChatGPT)*
