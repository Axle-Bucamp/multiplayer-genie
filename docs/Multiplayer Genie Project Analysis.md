# Multiplayer Genie Project Analysis

## Repository Information
- **Repository**: https://github.com/Axle-Bucamp/multiplayer-genie
- **Description**: A multiplayer version of open Google Genie with docs and etc
- **Status**: Public repository with 1 star, 0 forks
- **Commits**: 5 commits total
- **Branches**: 1 (main)

## Project Overview
The project aims to build a **Networking Multiplayer Mode for Google Genie** using a spherical world model concept.

### Core Concept
- Uses a **spherical world model** (like a snowball) as the central environment
- Each player interacts with the same shared universe
- Player perspective determines what part of the world they "see"
- Genie handles video output generation per player considering movement, nearby players, and cooperative dynamics

### Key Features Described

#### World Model: Snowball as Spatial Anchor
- Central world sphere (snowball) as environment
- Player init images generated based on sphere perspective
- Spawn logic tied to keypoints/regions on sphere surface
- Supports 360° movement, global positioning, relative orientation

#### Player State & Syncing
Each player maintains:
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

#### Action Pipeline
1. **Player Input**: Action, position, rotation, previous frame
2. **Server Aggregation**: Compute nearby players within render distance
3. **AI Context Assembly**: Custom prompt context per player for Genie
4. **AI Output**: Genie renders next frame/video with nearby player animations
5. **Server Update**: Global state updates for persistent universe

### Identified Challenges
- **Latency**: Needs interpolation/prediction
- **Render Distance**: Compute optimization
- **AI Hallucination**: Long-term consistency management
- **World Model Scaling**: Spatial indexing for large player counts

## Current Repository Structure
- README.md (main documentation)
- TODO.md (project tasks)
- Recent commits mention "player agent pretrain on video"

## Files to Examine Next
- TODO.md - to understand current tasks and priorities
- Commit history - to understand development progress
- Any existing code files



## Comprehensive TODO Analysis (2025 Roadmap)

### Architecture Overview
- **Visualizer**: Genie (Open Genie GitHub) as world and event renderer
- **Backend**: Nakama (WebSocket, Go/TS) or FastAPI+Nginx+WebSocket+Pydantic (Python)
- **Infrastructure**: Kubernetes & Docker for deployment; Cloud-managed DBs and scalable node pools (GCP/AWS/Azure)
- **Dataset & AI Training**: Mario Bros. coop-inspired multiplayer traces, continuous model training & loss monitoring

### Critical Components (Priority: Critical)

#### 1. Genie Integration Interface
- **Type**: Protocol/API
- **Due Date**: October 20, 2025
- **Dependencies**: Data Serialization
- **Task**: Design REST/WebSocket API for bidirectional real-time communication with Genie visualizer
- **Challenges**: 
  - Managing prompt/context variability for synchronous multiplayer rendering
  - Handling API quotas and ensuring real-time responsiveness
  - Robust schema validation (Pydantic for FastAPI, Go structs/TypeScript for Nakama)

#### 2. AI Context Assembler
- **Type**: Backend Service
- **Due Date**: October 5, 2025
- **Dependencies**: Data Serialization, State Synchronization
- **Task**: Dynamically builds per-player Genie context from world state, nearby players, and metadata
- **Challenges**:
  - Preventing context overflow with spatial/semantic filtering
  - Temporal consistency for multiplayer AI rendering
  - Real-time dataset collection for later model refinement

#### 3. Player State Synchronizer
- **Type**: Server/Backend
- **Due Date**: September 20, 2025
- **Dependencies**: Connection Manager, State Synchronization
- **Task**: Collects, validates, serializes, and broadcasts player state with delta compression
- **Challenges**:
  - Reducing bandwidth via event coalescing/deltas
  - Network reliability and handling dropped packets/conflicting updates

#### 4. Spherical World Model Manager
- **Type**: Infrastructure
- **Due Date**: September 15, 2025
- **Dependencies**: State Synchronization
- **Task**: Central authority for snowball-style spherical coordinate management
- **Challenges**:
  - Geodesic grid or HEALPix for scalable proximity queries
  - Representation/transform consistency across backend/frontend

### High Priority Components

#### 5. Security Validator
- **Type**: Security
- **Due Date**: November 5, 2025
- **Dependencies**: Authentication, Connection Manager
- **Task**: Prevents cheating/exploitation by validating all player state changes
- **Challenges**:
  - Low-latency anomaly detection
  - False-positive avoidance for unusual but fair gameplay

#### 6. Frame Consistency Manager
- **Type**: Client
- **Due Date**: September 30, 2025
- **Dependencies**: None
- **Task**: Manages temporal coherence of AI output by buffering historical frames
- **Challenges**:
  - Client-side memory optimization for frame buffer
  - Major view/perspective change resets to prevent visual artifacts

#### 7. Proximity Detection System
- **Type**: Server
- **Due Date**: September 25, 2025
- **Dependencies**: State Synchronization
- **Task**: Geodesic sphere-based logic to determine render/update neighbors
- **Challenges**:
  - Great-circle distance calculations at scale
  - Subscription management for state-change broadcasts

#### 8. Dataset Collector & Training Pipeline
- **Type**: ML/AI Infrastructure
- **Due Date**: November 15, 2025
- **Dependencies**: Player State Synchronizer, AI Context Assembler
- **Task**: Instrument backend to log Mario Bros. coop-style multiplayer traces
- **Challenges**:
  - Scalable, cloud-native log collection and safe anonymization
  - Automated preprocessing on K8s jobs
  - Custom loss design (rewarding cooperation, penalizing instability)
  - Training on cloud GPU/TPUs with monitoring/dashboards

### Medium Priority Components

#### 9. World Event Broadcaster
- **Due Date**: October 25, 2025
- **Task**: Manages zone/global events ensuring multi-client consistency

#### 10. Spawn Point Manager
- **Due Date**: October 15, 2025
- **Task**: Calculates balanced, fair spawn points using current world/player distribution

#### 11. Latency Compensation System
- **Due Date**: October 10, 2025
- **Task**: Client prediction/interpolation of movement/actions

### Key Resources Referenced
- Open Genie GitHub Repository (Visualizer, API reference)
- Nakama Documentation (Multiplayer backend)
- Genie 3: A New Frontier for World Models (DeepMind Blog)
- Skybox AI Environment Generator
- **Player agent pretrain on video** (Key research reference)

### Process Best Practices Listed
- Modularize all services; clear API contracts
- Automate CI/CD, test, and deploys
- Instrument for detailed monitoring (Prometheus, Grafana)
- Enable dataset replay and rapid retraining via K8s pipelines
- Document each module with code, diagrams, and examples

