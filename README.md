## 📡 Networking Multiplayer Mode for Google Genie

### 🧠 Core Idea

Build a multiplayer extension for Google Genie using a **spherical world model** (like a snowball). Each player interacts with the same shared universe, and their perspective determines what part of the world they "see." Genie handles the video output generation per player by considering player movement, nearby players, and cooperative dynamics.

---

### 🌍 World Model: Snowball as a Spatial Anchor

* Use a **world sphere (snowball)** as the central environment.
* Each player's **init image** is generated based on their current **perspective of the sphere** (e.g., like rotating a globe to a spawn point).
* **Spawn logic** is tied to **keypoints** or regions on the sphere surface.
* The sphere allows:

  * 360° movement
  * Global positioning (lat/lon or quaternion-based)
  * Relative orientation for determining camera/view vectors

---

### 👥 Player State & Syncing

Each player maintains the following state:

```json
{
  "id": "player_id",
  "position": [x, y, z],
  "rotation": [rx, ry, rz],
  "last_action": "move_left | attack | interact | idle",
  "init_image": "<base64-encoded>",
  "view_vector": [vx, vy, vz]
}
```

* **Position & rotation** are computed relative to the **center of the sphere**.
* **init\_image** is kept or updated for temporal consistency in AI context.
* All states are synchronized periodically with the server.

---

### 📤 Action Pipeline

#### 1. **Player Input**

Each client sends:

* Current action
* Position and rotation
* Previous frame's image or init image
* Optional: player intentions (co-op hints, gaze direction)

#### 2. **Server Aggregation**

* Server receives all player data.
* For each player, compute:

  * Nearby players within **render distance**
  * Their positions, actions, and view angles

#### 3. **AI Context Assembly**

The AI (Genie) receives a custom prompt context per player:

* The player’s own state
* The init image or last frame
* Actions and positions of **visible nearby players**
* Optional: world keypoint metadata (e.g., areas of interest)

#### 4. **AI Output**

* Genie decides what to **render/display** based on visibility, cooperation, and logic.
* It can **animate nearby players** in the video frame.
* Genie returns:

  * **Next frame/video**
  * Optional: embedded signals (e.g., next action suggestion, perspective shift)

#### 5. **Server Update**

* Server updates global state with the player's next position and action.
* All players continue in sync within the same **elastic, persistent universe**.

---

### ⚠️ Challenges & Notes

* **Latency**: Needs interpolation/prediction to stay smooth.
* **Render Distance**: Keeps compute reasonable; players far away are ignored.
* **AI Hallucination**: Long-term consistency must be managed via embedding cache or memory chaining.
* **World Model Scaling**: Spatial indexing (e.g., Octree or ElasticSearch-based KNN) needed for large player counts.

