# ⚙️ API Reference

**Namespace:** `Kaze.AdvancedQueue.API`  
**Class:** `AdvancedQueueAPI`  
**Type:** `public static class`

---

## 🔗 Back to Overview

For general information, in-game commands, features, and integration examples, see:  
➡ [Kaze.AdvancedQueue API](README.md)

---

## 🧩 `bool UpdatePriority(ulong steamID, int level = 1)`

Adds, updates, or removes a player's queue priority.  
If the player does not exist, they are created automatically.  
If `level <= 0`, the player is automatically removed from the priority list.

**Example:**

    AdvancedQueueAPI.UpdatePriority(76561198000000000, 3);

---

## 🧩 `bool RemovePriority(ulong steamID)`

Removes a player's priority record completely.

**Example:**

    AdvancedQueueAPI.RemovePriority(76561198000000000);

---

## 🧩 `bool IsPriority(ulong steamID)`

Checks whether a player currently has a priority entry in memory.

**Example:**

    bool hasPriority = AdvancedQueueAPI.IsPriority(76561198000000000);

---

## 🧩 `int GetPriorityLevel(ulong steamID)`

Gets a player's current priority level.  
Returns `0` if the player has no priority or has been removed.

**Example:**

    int prio = AdvancedQueueAPI.GetPriorityLevel(76561198000000000);

---

## 🧩 `PriorityPlayer? GetPriorityPlayer(ulong steamID)`

Retrieves the full `PriorityPlayer` entry associated with the given SteamID.  
Returns `null` if the player has no priority entry.

**Example:**

    PriorityPlayer? data = AdvancedQueueAPI.GetPriorityPlayer(steamID);
    if (data != null)
    {
        Logger.Log($"Priority for {data.SteamID}: {data.PriorityLevel}");
    }

---

## 🧩 `PriorityPlayer[] GetAll()`

Retrieves all cached priority entries from memory.

**Example:**

    foreach (var p in AdvancedQueueAPI.GetAll())
    {
        Logger.Log($"Player {p.SteamID} → Priority {p.PriorityLevel}");
    }

---

## 🧩 `event Action OnPreOrder`

Event fired **before** the internal queue is reordered (i.e. before Unturned selects the next player from the pending list).  

This allows external plugins to:

- Hook into the queue process  
- Log or monitor reorder operations  
- Apply custom logic or adjustments before selection  

**Example:**

    AdvancedQueueAPI.OnPreOrder += () =>
    {
        Logger.Log("AdvancedQueue: queue is being reordered...");
    };

---

## 🧠 Notes

- All API methods are designed to be **thread-safe** and can be called from any server-side context.  
- Priority data is persisted automatically via **LiteDB** when the main plugin saves or unloads.  
- The API is static — no manual initialization is required, as long as the core plugin is active.  
- Calling `UpdatePriority(steamID, 0)` is equivalent to removing the player from the priority list.  
- `GetPriorityLevel(steamID)` always returns an integer (0 if no entry).  
- `OnPreOrder` is invoked once every time the queue order is recomputed.

---

## 🧩 Example Integration Recap

**Minimal usage inside any plugin:**

    ulong id = player.playerID.steamID.m_SteamID;
    AdvancedQueueAPI.UpdatePriority(id, 3);
    Logger.Log($"Player {id} priority: {AdvancedQueueAPI.GetPriorityLevel(id)}");
