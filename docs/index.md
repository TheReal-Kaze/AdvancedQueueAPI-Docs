# 🧩 Kaze.AdvancedQueue API

Welcome to the official documentation for **Kaze.AdvancedQueue**,  
a modular queue management system for Unturned.  
This API lets developers manage player priorities dynamically across servers.

---

## 🚀 Overview

`Kaze.AdvancedQueue.API` is a static C# API built around the internal `PriorityManager`.  
It provides simple, thread-safe access to queue priority operations.

### 🔧 Import the namespace
```csharp
using Kaze.AdvancedQueue.API;
```
You can then access the API directly using its static methods:
```csharp
AdvancedQueueAPI.AddPriority(steamID, 3);
```

---

## 📘 Example Usage

Below is a practical example of assigning, reading, and removing queue priorities for players:
```csharp
using Kaze.AdvancedQueue.API;
using Rocket.Core.Logging;

public void Example(ulong steamID)
{
    // Add or update the player's priority
    AdvancedQueueAPI.AddPriority(steamID, 3);

    // Read their priority
    int prio = AdvancedQueueAPI.GetPriorityLevel(steamID);
    Logger.Log($"Player {steamID} now has priority level {prio}.");

    // Remove their priority by setting it to 0
    AdvancedQueueAPI.AddPriority(steamID, 0);
}
```

---

## 📂 API Features

- Add or update priority levels dynamically  
- Remove players automatically when priority ≤ 0  
- Read player priorities at runtime  
- Retrieve all cached priority entries  
- Persistent via LiteDB

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
            AdvancedQueueAPI.AddPriority(id, 5);
            Logger.Log($"[QueueDemo] {id} now has priority {AdvancedQueueAPI.GetPriorityLevel(id)}.");
        };

        Logger.Log($"{Name} loaded and hooked into AdvancedQueue API.");
    }

    protected override void Unload()
    {
        Provider.onEnemyConnected -= null; // Replace with your event handler variable if stored
        Logger.Log($"{Name} has been unloaded!");
    }
}
```

---

## 📖 API Reference
➡ [View the full API reference](api-reference.md)

---

## 🧱 About

**Kaze.AdvancedQueue** is designed to make queue management modular, persistent, and easily extendable.  
The API can be used directly by other developers to integrate priority systems, rank-based access, or server slot extensions.
