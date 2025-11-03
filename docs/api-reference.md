# ⚙️ API Reference

Namespace: `Kaze.AdvancedQueue.API`  
Class: `AdvancedQueueAPI`  
Type: `public static class`

---

### 🧩 `bool UpdatePriority(ulong steamID, int level = 1)`
Adds, updates, or removes a player's queue priority.  
If the player does not exist, they are created automatically.  
If `level <= 0`, the player is automatically removed from the priority list.

```csharp
AdvancedQueueAPI.UpdatePriority(76561198000000000, 3);
```

---

### 🧩 `bool RemovePriority(ulong steamID)`
Removes a player's priority record completely.

```csharp
AdvancedQueueAPI.RemovePriority(76561198000000000);
```

---

### 🧩 `bool IsPriority(ulong steamID)`
Checks whether a player currently has a priority entry in memory.

```csharp
bool hasPriority = AdvancedQueueAPI.IsPriority(76561198000000000);
```

---

### 🧩 `int GetPriorityLevel(ulong steamID)`
Gets a player's current priority level.  
Returns `0` if the player has no priority or has been removed.

```csharp
int prio = AdvancedQueueAPI.GetPriorityLevel(76561198000000000);
```

---

### 🧩 `PriorityPlayer[] GetAll()`
Retrieves all cached priority entries from memory.

```csharp
foreach (var p in AdvancedQueueAPI.GetAll())
{
    Logger.Log($"Player {p.SteamID} → Priority {p.PriorityLevel}");
}
```

---

## 🧠 Notes
- All API methods are **thread-safe** and can be used anywhere server-side.  
- Changes are saved automatically through **LiteDB** when the plugin unloads.  
- The API is static — no initialization is required if the main plugin is active.  
- Calling `UpdatePriority(steamID, 0)` will automatically remove the player from the queue.

---

## 🧩 Example Integration Recap
```csharp
ulong id = player.playerID.steamID.m_SteamID;
AdvancedQueueAPI.UpdatePriority(id, 3);
Logger.Log($"Player {id} priority: {AdvancedQueueAPI.GetPriorityLevel(id)}");
```
