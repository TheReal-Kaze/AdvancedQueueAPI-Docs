# ⚙️ API Reference

Namespace: `Kaze.AdvancedQueue.API`  
Class: `AdvancedQueueAPI`  
Type: `public static class`

---

### 🧩 `bool AddPriority(ulong steamID, int level = 1)`
Adds or updates a player's queue priority.  
If the player already exists, their priority is overwritten.  
If `level <= 0`, the player is automatically removed.

```csharp
AdvancedQueueAPI.AddPriority(76561198000000000, 3);
```

---

### 🧩 `bool UpdatePriority(ulong steamID, int level = 1)`
Forces a priority level update for an existing player.  
If the player doesn’t exist, they are added automatically.

```csharp
AdvancedQueueAPI.UpdatePriority(76561198000000000, 5);
```

---

### 🧩 `bool RemovePriority(ulong steamID)`
Removes a player's priority record.

```csharp
AdvancedQueueAPI.RemovePriority(76561198000000000);
```

---

### 🧩 `bool IsPriority(ulong steamID)`
Checks whether a player currently has a priority entry.

```csharp
bool hasPriority = AdvancedQueueAPI.IsPriority(76561198000000000);
```

---

### 🧩 `int GetPriorityLevel(ulong steamID)`
Gets a player's current priority level. Returns `0` if none exists.

```csharp
int prio = AdvancedQueueAPI.GetPriorityLevel(76561198000000000);
```

---

### 🧩 `IEnumerable<PriorityPlayer> GetAll()`
Retrieves all known priority entries from memory.

```csharp
foreach (var p in AdvancedQueueAPI.GetAll())
{
    Logger.Log($"Player {p.SteamID} → Priority {p.PriorityLevel}");
}
```

---

## 🧠 Notes
- All API methods are **thread-safe** and can be used server-wide.  
- Changes persist automatically through **LiteDB** when the main plugin unloads.  
- The API is static — no initialization required if the main plugin is active.  
- Adding a player with level `0` automatically removes them from the queue list.

---

## 🧩 Example Integration Recap
```csharp
ulong id = player.playerID.steamID.m_SteamID;
AdvancedQueueAPI.AddPriority(id, 3);
Logger.Log($"Player {id} priority: {AdvancedQueueAPI.GetPriorityLevel(id)}");
```
