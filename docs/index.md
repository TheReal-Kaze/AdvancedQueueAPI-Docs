# 🧩 Kaze.AdvancedQueue API

Welcome to the official documentation for **Kaze.AdvancedQueue**,  
a modular queue management system for Unturned.  
This system lets developers or admins manage player priorities dynamically across servers.

---

## 🧰 Command Usage (In-Game)

If you don’t use the API directly, **Kaze.AdvancedQueue** also provides simple in-game commands for managing priorities.

### 🔹 Add or Update a Player’s Priority
```csharp
/addpriority <steamid64> <priority>
```

**Example:**
```csharp
/addpriority 76561198000000000 5
```

Adds or updates a player’s queue priority level.  
If the player already has one, it’s overwritten.  

---

### 🔹 Remove a Player’s Priority
```csharp
/delpriority <steamid64>
```

**Example:**
```csharp
/delpriority 76561198000000000
```

Removes a player from the priority list.

---

### 🔸 Command Source Code (for reference)
```csharp
using Rocket.API;
using System.Collections.Generic;
using Kaze.AdvancedQueue.Managers;

namespace Kaze.AdvancedQueue.Commands
{
    public class AddPriorityCmd : IRocketCommand
    {
        public AllowedCaller AllowedCaller => AllowedCaller.Both;
        public string Name => "addpriority";
        public string Help => "Adds or updates a player's queue priority by SteamID.";
        public string Syntax => "/addpriority <steamid> <priority>";
        public List<string> Aliases => new() { "setpriority", "priorityadd" };
        public List<string> Permissions => new() { "advancedqueue.addpriority" };

        public void Execute(IRocketPlayer caller, string[] command)
        {
            if (command.Length < 2)
            {
                Logger.Log("Usage: /addpriority <steamid> <priority>");
                return;
            }

            if (!ulong.TryParse(command[0], out var steamID))
            {
                Logger.Log("Invalid SteamID.");
                return;
            }

            if (!int.TryParse(command[1], out var priority))
            {
                Logger.Log("Invalid priority value.");
                return;
            }

            var manager = PriorityManager.Instance;
            if (manager == null)
            {
                Logger.Log("[AQueue] PriorityManager not initialized.");
                return;
            }

            if (manager.IsPriority(steamID))
            {
                var player = manager.GetPriorityPlayer(steamID);
                if (player != null)
                {
                    player.PriorityLevel = priority;
                    Logger.Log($"[AQueue] Updated {steamID} to priority {priority}.");
                }
                else
                {
                    Logger.Log($"[AQueue] Failed to find cached player for update.");
                }
            }
            else
            {
                manager.UpdatePriority(steamID, priority);
                Logger.Log($"[AQueue] Added {steamID} with priority {priority}.");
            }
        }
    }
}
```

```csharp
using Kaze.AdvancedQueue.Managers;
using Rocket.API;
using System.Collections.Generic;

namespace Kaze.AdvancedQueue.Commands
{
    public class DelPriorityCmd : IRocketCommand
    {
        public AllowedCaller AllowedCaller => AllowedCaller.Both;
        public string Name => "delpriority";
        public string Help => "Deletes a player's queue priority by SteamID.";
        public string Syntax => "/delpriority <steamid64>";
        public List<string> Aliases => new() { "removepriority", "prioritydel" };
        public List<string> Permissions => new() { "advancedqueue.delpriority" };

        public void Execute(IRocketPlayer caller, string[] command)
        {
            if (command.Length < 1)
            {
                Logger.Log("Usage: /delpriority <steamid64>");
                return;
            }

            if (!ulong.TryParse(command[0], out var steamID))
            {
                Logger.Log("Invalid SteamID64.");
                return;
            }

            var manager = PriorityManager.Instance;
            if (manager == null)
            {
                Logger.Log("[AQueue] PriorityManager not initialized.");
                return;
            }

            if (!manager.IsPriority(steamID))
            {
                Logger.Log($"[AQueue] SteamID {steamID} is not in the priority list.");
                return;
            }

            if (manager.RemovePriority(steamID))
                Logger.Log($"[AQueue] Removed SteamID {steamID} from priority list.");
            else
                Logger.Log($"[AQueue] Failed to remove SteamID {steamID} from database.");
        }
    }
}
```

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
AdvancedQueueAPI.AddPriority(steamID, 3);
```

---

## 📘 Example Usage
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
        Provider.onEnemyConnected -= null;
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
The API can be used directly by other developers or through in-game commands to manage ranks, priority slots, or reserved access systems.

---

(Thanks to ChatGPT for the doc)
