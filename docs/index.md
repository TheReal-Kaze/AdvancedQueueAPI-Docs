# 🧩 Kaze.AdvancedQueue API Overview

Welcome to the official documentation for **Kaze.AdvancedQueue**,  
a modular and extensible queue management system for *Unturned*.  

This system allows servers and developers to manage dynamic player priorities,  
implement reserved slots, VIP handling, admin bypasses, and cross-server logic.

---

## 🔗 Full API Reference

For the detailed method list (UpdatePriority, RemovePriority, events, models, etc.), see:  
➡ **[AdvancedQueueAPI Reference](api-reference.md)**

---

# 🧰 In-Game Commands

Kaze.AdvancedQueue provides simple commands for administrators who prefer not to interact directly with the C# API.

---

## 🔹 Add or Update a Player’s Priority

```
/addpriority <steamid64> <priority>
```

**Example:**

```
/addpriority 76561198000000000 5
```

Adds or updates the priority. If the player already has a priority entry, it is overwritten.

**Aliases:**  
`/setpriority`  
`/priorityadd`

---

## 🔹 Remove a Player’s Priority

```
/delpriority <steamid64>
```

**Example:**

```
/delpriority 76561198000000000
```

Removes the player from the priority list completely.

**Aliases:**  
`/removepriority`  
`/prioritydel`

---

# 🚀 API Overview

The namespace to include in your plugin:

```csharp
using Kaze.AdvancedQueue.API;
```

The entire API is exposed through the static class:

```
AdvancedQueueAPI
```

There is **no initialization needed**—the plugin automatically assigns  
its internal manager to the API at startup.

You can access all operations using static methods like:

```csharp
AdvancedQueueAPI.UpdatePriority(steamID, 3);
```

---

# 📘 Basic Example Usage

```csharp
using Kaze.AdvancedQueue.API;
using Rocket.Core.Logging;

public void Example(ulong steamID)
{
    // Add or update the player's priority level
    AdvancedQueueAPI.UpdatePriority(steamID, 3);

    // Read the player's priority
    int prio = AdvancedQueueAPI.GetPriorityLevel(steamID);
    Logger.Log($"Player {steamID} now has priority {prio}.");

    // Remove a player's priority by setting it to 0
    AdvancedQueueAPI.UpdatePriority(steamID, 0);
}
```

---

# 📂 Features

Kaze.AdvancedQueue provides:

- Dynamic runtime priority management  
- Automatic removal when priority <= 0  
- Priority and player data persistence through **LiteDB**  
- Ability to retrieve all priority values  
- Queue sorting integration via **Harmony patching**  
- Admin bypass support  
- Extra slot handling  
- New: **Pre-order event** exposed by the API  

---

# ⚙️ Example Integration (Rocket Plugin)

Below is a simple RocketMod plugin showing how to use the API during player connection.

```csharp
using Kaze.AdvancedQueue.API;
using Rocket.Core.Logging;
using Rocket.Core.Plugins;
using SDG.Unturned;

public class QueueDemoPlugin : RocketPlugin
{
    protected override void Load()
    {
        // Log each queue reordering moment
        AdvancedQueueAPI.OnPreOrder += () =>
        {
            Logger.Log("[QueueDemo] Queue is being reordered...");
        };

        // Give default priority to new players
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

# 🧱 About Kaze.AdvancedQueue

Kaze.AdvancedQueue was designed to offer:

- A modular queue system  
- Persistence without configuration  
- Integration points for developers  
- Compatibility with other plugins  
- High performance priority sorting  
- A clean and intuitive C# API  

It is suitable for:

- VIP tiers  
- Donator ranks  
- Admin bypass logic  
- Reserved slots  
- Multi-server networks  
- Custom gameplay queue systems  

---

*(Documentation generated with ❤️ by ChatGPT)*
