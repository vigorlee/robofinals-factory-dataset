# Factory LW-BenchHub Dataset

> Factory Manipulation Task Dataset for Industrial Robot Evaluation

**Version**: V1.0  
**Release Date**: 2026-03-19  
**Total Episodes**: 2,500+  
**Tasks**: 4  
**Scenes**: 3  
**Robots**: 3

---

## Overview

The Factory LW-BenchHub Dataset is a comprehensive benchmark for evaluating robot manipulation capabilities in industrial factory environments. Built on the "Logic Template + Procedural Generation" architecture, it provides scalable and diverse training/evaluation data.

### Key Statistics

| Metric | Value |
|--------|-------|
| Total Episodes | 2,500+ |
| Task Templates | 4 |
| Scene Variants | 3 |
| Robot Support | UR5, UR10, Franka Panda |
| Success Rate (Baseline) | 85-92% |
| Sim2Real Gap | 8.5% |

### Dataset Composition

| Task | Episodes | Scene | Robot | Success Rate |
|------|----------|-------|-------|--------------|
| pick_part_from_bin | 1,000 | factory_assembly_001 | UR5 | 92% |
| assemble_shaft_into_hole | 500 | factory_assembly_001 | Franka | 85% |
| transfer_between_stations | 800 | factory_assembly_001 | UR10 | 90% |
| FACTORY-001 (conveyor sorting) | 200 | factory_assembly_001 | UR5 | 88% |

---

## Data Format

### Episode Structure

Each episode is stored in LeRobot v3.0 format with factory-specific extensions:

```json
{
  "episode_id": "factory_pick_001",
  "task_id": "pick_part_from_bin",
  "scene": {
    "scene_id": "factory_assembly_001",
    "scene_type": "assembly_line",
    "layout_seed": 42
  },
  "assets": [
    {
      "asset_id": "metal_bearing_03",
      "category": "mechanical_part",
      "initial_pose": [0.5, 0.2, 0.1, 0, 0, 0, 1]
    }
  ],
  "randomization": {
    "lighting": "bright",
    "background": "factory_wall_02",
    "camera_noise": 0.01
  },
  "validation": {
    "semantic_check": true,
    "geometric_check": true,
    "physics_check": true,
    "pass_rate": 1.0
  },
  "factory_metadata": {
    "cycle_time_requirement": 5.0,
    "safety_constraints": ["no_collision", "force_limit_30N"],
    "quality_thresholds": {"position_error": 0.005}
  },
  "trajectory": {
    "observations": {
      "images": {
        "camera_0": [...],
        "camera_1": [...]
      },
      "state": [...],
      "depth": [...]
    },
    "actions": [...],
    "rewards": [...],
    "success": true
  }
}
```

### Directory Structure

```
LightWheel_Data/
├── libraries/                  # Static libraries
│   ├── scenes/
│   │   ├── factory_assembly_001/
│   │   │   ├── Scene_Geometry.usd
│   │   │   ├── Scene_Graph.json
│   │   │   └── source_ply/
│   │   ├── factory_warehouse_001/
│   │   └── factory_inspection_001/
│   ├── assets/
│   │   ├── mechanical_parts/
│   │   ├── tools/
│   │   ├── containers/
│   │   └── asset_manifest.json
│   └── tasks/
│       ├── pick_part_from_bin.yaml
│       ├── assemble_shaft_into_hole.yaml
│       ├── transfer_between_stations.yaml
│       └── FACTORY-001.yaml
│
├── dataset/
│   ├── pick_part_from_bin/
│   │   ├── g2/
│   │   │   ├── data/           # Parquet trajectory data
│   │   │   ├── meta/
│   │   │   │   └── episodes.jsonl
│   │   │   └── videos/
│   │   └── stats.json
│   ├── assemble_shaft_into_hole/
│   ├── transfer_between_stations/
│   └── FACTORY-001/
│
└── benchmark/
    ├── results/
    └── leaderboard.json
```

---

## Task Specifications

### 1. pick_part_from_bin

**Task ID**: `pick_part_from_bin`  
**Category**: Pick  
**Difficulty**: ★★☆  
**Episodes**: 1,000

#### Description
Pick a metal part from bin and place on workbench.

#### Requirements
- **Scene Tags**: `factory`, `assembly`, `tabletop`
- **Asset Categories**: `mechanical_part`
- **Robot Types**: UR5, UR10, Franka

#### Steps
1. `approach` → target: containable
2. `grasp` → grasp_type: top_grasp
3. `lift` → min_height: 0.1m
4. `transfer` → target: supportable
5. `place` → placement_type: stable

#### Success Criteria
- `holding(robot, part)`
- `distance(part, bin) > 0.2`
- `part_on_surface(part, workbench)`

#### Metrics
| Metric | Value |
|--------|-------|
| Success Rate | 92% |
| Avg Completion Time | 4.2s |
| Collision Rate | 4.1% |
| Position Error | 3.2mm |

---

### 2. assemble_shaft_into_hole

**Task ID**: `assemble_shaft_into_hole`  
**Category**: Assembly  
**Difficulty**: ★★★  
**Episodes**: 500

#### Description
Insert shaft into hole with high precision force control.

#### Requirements
- **Scene Tags**: `factory`, `assembly`
- **Asset Categories**: `shaft`, `hole_plate`
- **Robot Types**: UR5, Franka

#### Steps
1. `pick` → target: shaft
2. `align` → target: hole
3. `insert` → force_limit: 10N
4. `release`

#### Success Criteria
- `shaft_inserted(hole)`
- `insertion_depth > 0.9 * shaft_length`

#### Metrics
| Metric | Value |
|--------|-------|
| Success Rate | 85% |
| Avg Completion Time | 6.5s |
| Max Force | 8.5N |
| Position Error | 1.8mm |

---

### 3. transfer_between_stations

**Task ID**: `transfer_between_stations`  
**Category**: Transport  
**Difficulty**: ★★☆  
**Episodes**: 800

#### Description
Transfer part from one station to another station.

#### Requirements
- **Scene Tags**: `factory`, `assembly`
- **Asset Categories**: `mechanical_part`
- **Robot Types**: UR5, UR10, AGV

#### Steps
1. `navigate` → target: source_station
2. `grasp` → target: part
3. `navigate` → target: dest_station
4. `place`

#### Success Criteria
- `part_at_destination(part, dest_station)`
- `no_collision`

#### Metrics
| Metric | Value |
|--------|-------|
| Success Rate | 90% |
| Avg Completion Time | 5.8s |
| Collision Rate | 3.5% |
| Navigation Error | 2.1cm |

---

### 4. FACTORY-001: Conveyor To Bin Sorting

**Task ID**: `FACTORY-001`  
**Category**: Pick (Sorting)  
**Difficulty**: ★★☆  
**Episodes**: 200

#### Description
Pick specified parts from conveyor belt and sort into corresponding bins.

#### Requirements
- **Scene Tags**: `factory`, `assembly`, `conveyor`
- **Asset Categories**: `mechanical_part`
- **Robot Types**: UR5, UR10

#### Atomic Actions
1. `navigate` → conveyor_side
2. `visual_recognition` → parts_on_conveyor
3. `grasp` → target_part (top_grasp)
4. `transport` → target_bin
5. `place` → inside_bin

#### Success Criteria
- `part_in_correct_bin`
- `part_stable`
- `no_fall_to_ground`
- `no_wrong_bin`

#### Factory Constraints
| Constraint | Value |
|------------|-------|
| Cycle Time | 30s (per part) |
| Max Force | 30N |
| Speed Limit | 0.5 m/s |
| Position Error | 1cm |
| Classification Accuracy | 95% |

#### Randomization
| Parameter | Range |
|-----------|-------|
| Wall Texture | 50 variants |
| Floor Material | 20 variants |
| Lighting | 4 variants |
| Part Pose (position) | ±5cm x, ±5cm y, ±2cm z |
| Part Pose (rotation) | ±30° yaw |

---

## Scene Specifications

### factory_assembly_001

**Category**: Factory > Assembly  
**Tags**: `factory`, `assembly`, `conveyor`, `tabletop`

#### Description
Typical factory assembly line with conveyor belt, workbench, and parts bin.

#### Objects
| Object ID | Category | Affordances | Sites |
|-----------|----------|-------------|-------|
| conveyor_belt_01 | equipment | movable_surface, transport | part_spawn_site, part_pick_site |
| workbench_01 | furniture | supportable, assembly_surface | tool_place_site, part_assembly_site |
| parts_bin_01 | container | containable, graspable | bin_interior_site |

#### Assets (25 total)
- Conveyor Belt (equipment)
- Workbench (furniture)
- Parts Bin (container)
- Metal Bearing (mechanical_part)
- ... (21 more)

#### Downloads
- [Scene_Geometry.usd](#) (45MB)
- [Scene_Graph.json](#) (12KB)
- [Source Point Cloud](#) (2.1GB)

---

### factory_warehouse_001

**Category**: Factory > Warehouse  
**Tags**: `factory`, `warehouse`, `storage`, `shelf`

#### Description
Factory warehouse storage area with shelves, pallets, and AGV paths.

#### Objects
| Object ID | Category | Affordances | Sites |
|-----------|----------|-------------|-------|
| shelf_01 | storage | supportable, containable | shelf_slot_01, shelf_slot_02 |
| pallet_01 | transport | supportable, stackable | pallet_top_site |

#### Assets (40 total)
- Storage Shelf (storage)
- Pallet (transport)
- ... (38 more)

---

### factory_inspection_001

**Category**: Factory > Inspection  
**Tags**: `factory`, `inspection`, `quality_control`, `camera`

#### Description
Quality inspection station with camera rig and reject bin.

#### Objects
| Object ID | Category | Affordances | Sites |
|-----------|----------|-------------|-------|
| inspection_table_01 | furniture | supportable | part_inspection_site |
| camera_rig_01 | equipment | viewable | camera_view_site |
| reject_bin_01 | container | containable | reject_bin_site |

#### Assets (18 total)
- Inspection Table (furniture)
- Camera Rig (equipment)
- Reject Bin (container)
- ... (15 more)

---

## Training Protocol

### π0.5 Model Training

#### Data Configuration
- **Group-A**: 200 episodes (simulation only)
- **Group-B**: 200+100 episodes (simulation + real)

#### Training Parameters
```yaml
model: pi0.5-base
learning_rate: 1e-4
batch_size: 32
epochs: 100
optimizer: AdamW
scheduler: cosine_decay
```

#### Results

| Training Group | Validation SR | Test SR (Sim) | Test SR (Real) | Sim2Real Gap |
|----------------|---------------|---------------|----------------|--------------|
| Group-A (200 sim) | 92.5% | 88.0% | 71.5% | -16.5% |
| Group-B (200+100 mixed) | 94.0% | 90.5% | 82.0% | -8.5% |

#### Key Findings
1. **Simulation Validity**: Pure simulation training achieves 88% on simulation test set
2. **Real Data Gain**: Adding 100 real episodes improves real test set by 10.5%
3. **Sim2Real Gap**: Mixed training reduces gap from 16.5% to 8.5%

#### Additional Metrics
| Metric | Group-A | Group-B | Improvement |
|--------|---------|---------|-------------|
| Avg Completion Time | 4.2s | 3.8s | -9.5% |
| Collision Rate | 6.2% | 4.1% | -34% |
| Position Accuracy | 4.8mm | 3.2mm | +33% |
| Orientation Accuracy | 2.1° | 1.5° | +29% |

---

## Benchmark & Leaderboard

### Current Rankings

| Rank | Model | Task | Success Rate | Avg Time | Precision | Date |
|------|-------|------|--------------|----------|-----------|------|
| 1 | VLA-Factory-v1 | pick_part_from_bin | 95.2% | 3.8s | 98% | 2026-03-15 |
| 2 | GROOT-N1.5 | pick_part_from_bin | 93.8% | 4.1s | 96% | 2026-03-14 |
| 3 | Pi0-Finetuned | pick_part_from_bin | 92.5% | 4.3s | 95% | 2026-03-13 |
| 4 | Diffusion-Policy-v3 | assemble_shaft | 87.2% | 6.8s | 94% | 2026-03-12 |
| 5 | ACT-Factory | transfer_between | 91.0% | 5.5s | 93% | 2026-03-11 |

### Evaluation Metrics

| Metric | Description | Weight |
|--------|-------------|--------|
| Success Rate (SR) | Task completion ratio | 40% |
| Completion Time (CT) | Average task duration | 20% |
| Precision Score | Position/orientation accuracy | 20% |
| Safety Score | Collision-free execution | 20% |

---

## Download

### Full Dataset
- **Factory-LW-BenchHub-v1.0** (15GB)
  - [Hugging Face](#)
  - [Google Drive](#)
  - [Baidu Netdisk](#)

### Individual Tasks
| Task | Size | Download |
|------|------|----------|
| pick_part_from_bin | 4.2GB | [Download](#) |
| assemble_shaft_into_hole | 2.1GB | [Download](#) |
| transfer_between_stations | 3.5GB | [Download](#) |
| FACTORY-001 | 0.8GB | [Download](#) |

### Assets & Scenes
| Type | Size | Download |
|------|------|----------|
| Scene Library | 5.2GB | [Download](#) |
| Asset Library | 3.8GB | [Download](#) |
| Task Templates | 120KB | [Download](#) |

---

## Citation

```bibtex
@dataset{factory_lw_benchhub_2026,
  title = {Factory LW-BenchHub: A Benchmark for Industrial Robot Manipulation},
  author = {Li, Mingyi and others},
  year = {2026},
  publisher = {LightWheel},
  url = {https://docs.lightwheel.net/lw_benchhub/dataset/}
}
```

---

## Contact

- **Website**: https://lightwheel.net
- **Documentation**: https://docs.lightwheel.net/lw_benchhub/
- **GitHub**: https://github.com/lightwheel/factory-benchhub
- **Email**: contact@lightwheel.net

---

**Last Updated**: 2026-03-19  
**Version**: V1.0
