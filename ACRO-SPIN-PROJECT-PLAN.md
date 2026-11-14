# ACRO-SPIN PROJECT PLAN

**Status:** Active background project, no fixed dates. Progress is measured in checkpoints, not weeks.
**Goal:** Build an agentic Physical AI coach that perceives human movement via IMU sensors, reasons about kinematic metrics using LangGraph, and delivers behavioral coaching interventions to improve the athlete's performance.

## Priority Order

1. Job search (W2 role with health benefits)
2. `lit-flow-flash` (agentic upgrade and showcase)
3. acro-spin (this plan)

acro-spin gets time only after the first two have had their daily blocks. Suggested cap: a few hours per week. When a W2 role starts, the plan keeps going at whatever pace life allows.

## Hardware Inventory (Procured, stored)

| Component | Qty | Role in System |
| :--- | :--- | :--- |
| Adafruit BNO055 Breakout | 1 | Sensor: quaternions via on-chip fusion |
| ESP32-WROOM-32 Dev Boards | 3 | Edge compute: I2C read, timestamping, Wi-Fi UDP |
| 3.7V 500mAh LiPo (JST-PH) | 4 | Power (502530 form factor, waist-pouch friendly) |
| TP4056 Charging Modules | 3 | Battery management and protection |
| Micro-USB Data Cables | 2 | Flashing/debug (data-capable) |
| Soldering Kit (80W LCD) | 1 | Final assembly only |
| Athletic Belt / Waist Pouch | 1 | Rigid mounting |
| 3M VHB Tape + Cohesive Wrap | 1 kit | Rigidity during spins (#1 data-quality risk) |
| Dot2Dance Turn-board | 1 | To physically do the pirouettes |

**Returns:** Amazon return decision on **Sat, Jan 16, 2027** (return window closes Jan 31, 2027); leaning toward keeping everything. Adafruit BNO055: keeping, no return.

**Simulate First:** Everything that can be simulated is simulated and passing before anything physical is touched.

**Hardware Gate:** The BNO055 is a single unit with no spares. It stays sealed in its anti-static bag until **Checkpoint D** passes (metrics, agent loop, and the simulated waist node all working in software). No soldering before then.

**Architecture Decision:** Direct Wi-Fi (SoftAP). The waist ESP32 creates its own network and the PC connects directly. No gateway or router needed yet.

## How This Thread Works

- Work top to bottom. Tasks are deliberately unnumbered so they can be reordered, swapped, added, or dropped freely.
- Phases can be paused at any checkpoint. Do not skip a checkpoint's requirements.
- A task can be cut or simplified if it isn't earning its place. Never extend hours to finish one.
- Swap-in candidate: the CI task in Phase 7 can move up next to the toolchain setup if you want a green pipeline from the start.
- Log each completed task with a one-line note in `technical-ledger`.
- Task wording is the identifier. If a task is dropped, delete the line; there is nothing to renumber.

---

## Phase 0: Housekeeping

- [ ] Fix the End Date line (remove the stray fragment); replace with "No fixed end date"
- [ ] Keep the Priority Order section above as the plan's scope rule
- [ ] Update the repo description to reflect "active, background, unscheduled"

## Phase 1: Rust Core & Ownership

- [ ] Install Rust toolchain (rustup, cargo, rust-analyzer in VS Code)
- [ ] Complete `rustlings` exercises: variables, functions, control flow
- [ ] Ownership, borrowing, lifetimes: write small programs that deliberately break reference rules to understand the errors
- [ ] Define `Quaternion` struct and `ImuState` enum; pattern match on sensor states
- [ ] Error handling (`Result`, `Option`, `?`) in a CLI parser for IMU-shaped JSON
- [ ] `Sensor` trait with `read()` and `calibrate()`; implement for `FakeSensor`
- [ ] Moving-average filter using pure Rust iterators (no raw loops)
- [ ] Summary notes in `technical-ledger` on key Rust concepts learned

**Checkpoint A:** Small Rust IMU toolkit with tests. Safe stopping point.

## Phase 2: ROS 2 Integration & Synthetic Data

- [ ] Docker devcontainer with ROS 2 Jazzy + Rust + `rclrs`; `colcon build` succeeds
- [ ] Python talker/listener nodes (pub/sub fundamentals)
- [ ] Port talker/listener to Rust with `rclrs`; publish synthetic `sensor_msgs/Imu`
- [ ] Second Rust subscriber node; confirm end-to-end pub/sub inside the container
- [ ] QoS experiments: `Reliable` vs `BestEffort` at 100 Hz
- [ ] Synthetic IMU generator in Rust: constant-rate spin + Gaussian noise + bias drift shaped as pirouette profiles (yaw ramps through 360° crossings + injected wobble)
- [ ] EMA bias calibration for accel and gyro; validate against known ground truth
- [ ] Unit tests for the calibration module with convergence assertions

**Checkpoint B:** Containerized ROS 2 + Rust pipeline streaming synthetic IMU data.

## Phase 3: Kinematic Metrics on Synthetic Data

- [ ] Define frame format: timestamp, quaternion wxyz, gyro xyz, sequence number
- [ ] Zero-copy parser in Rust; fuzzed/malformed-frame unit tests
- [ ] Sequence-gap detection (packet loss)
- [ ] Rotation Count: yaw 360°-crossing counter
- [ ] Peak Angular Velocity + Smoothness (std-dev of yaw rate per 360° segment)
- [ ] Settling Time + Posture (pitch/roll deviation from vertical)
- [ ] Validate all metrics against synthetic ground truth

**Checkpoint C:** Metrics correct on synthetic data.

## Phase 4: Agentic Layer on Synthetic Data

- [ ] State machine diagram before code: Perceive → Plan → Act → Evaluate
- [ ] Persistent session memory (SQLite or JSON): athlete profile, baselines, prior scores
- [ ] Perception node: subscribe to metrics topics → format the agent context window
- [ ] Reasoning node: LLM call outputs a structured coaching decision object (not free text)
- [ ] Strategy bank with escalating interventions per metric:
  - Low Smoothness → "Slow your entry to preserve momentum" → "Engage core earlier and keep arms tight to ribs" → "Reduce arm range of motion entirely"
  - High Settling Time → "Focus on a fixed horizon point before stopping" → "Land on balls of feet with knees slightly bent" → "Hold final position for 3 seconds to prove stability"
  - High Pitch/Roll Deviation → "Keep head directly over hips, look straight ahead" → "Tighten glutes to maintain straight vertical axis" → "Imagine a string pulling you up from the crown of your head"
- [ ] Track whether each intervention was followed and whether the score improved; escalate automatically rather than repeating failed advice
- [ ] Evaluation node: score deltas per attempt (did the intervention improve the outcome?)
- [ ] Feed evaluation results into memory; update the baseline only on statistically significant improvement
- [ ] Guardrails: LLM rate limiting, output schema validation, timeout/failure handling
- [ ] Run the full loop on synthetic trials with a simulated athlete who responds to (or ignores) advice
- [ ] Build a software stand-in for the waist node: a Rust program that sends the real frame format over UDP with the same timing as the planned ESP32 firmware
- [ ] Inject faults into the simulated node: packet loss, reordering, duplicate and corrupted frames, clock drift, dropouts mid-spin
- [ ] Run the Rust listener, ROS 2 bridge, metrics, and agent loop end to end against the simulated node over a real UDP socket
- [ ] Write the ESP32 SoftAP firmware and exercise whatever can be tested off-device (frame packing, sequence numbering, timestamping) before flashing

**Checkpoint D (Hardware Gate):** Agent loop demonstrably improving synthetic athlete scores, and the full pipeline surviving the simulated node's faults, with no hardware required. Unlocks the BNO055. Write a short README section and a screen recording. This is the job-relevant showcase.

## Phase 5: Hardware Bring-up (Bench Only, No Soldering)

*Requires Checkpoint D. Anything that fails here is compared against the simulation first, to tell a hardware problem from a software one.*

- [ ] Flash ESP32 #1 via USB with SoftAP firmware creating the `AcroSpin_Waist` network
- [ ] Wire BNO055 on jumper leads (SDA→GPIO21, SCL→GPIO22, 3V3, GND), breadboard only
- [ ] Multimeter-check TP4056 polarity against the battery lead before first connection
- [ ] Verify quaternion output over USB serial before removing the cable
- [ ] Wireless path: connect laptop to `AcroSpin_Waist`, receive UDP packets in a Rust listener
- [ ] Validate the power chain: LiPo → TP4056 → ESP32 VIN → 3V3 → BNO055
- [ ] Mount to the waist belt with cohesive wrap; stream during 5 test spins
- [ ] Document discrepancies between bench and field data

**Checkpoint E:** Real IMU data streaming wirelessly.

## Phase 6: Real Data

- [ ] Bind the bridge to a ROS 2 publisher; confirm `/imu/waist` is live from real hardware
- [ ] Field-test Rotation Count with 10 slow pirouettes
- [ ] Compare real vs synthetic streams side by side; document discrepancies in the README
- [ ] Wire the agent loop to real single-node field data
- [ ] Perform 8 clean pirouette trials; log raw quaternions, Tier 1 metrics, and agent decisions together

**Checkpoint F:** Agent loop running on real athlete data.

## Phase 7: Production Hardening

- [ ] GitHub Actions: lint, unit tests, integration tests, Docker image build, green on every push
- [ ] Version agent prompts and calibration params; rollback to last-known-good config
- [ ] Structured logging with correlation IDs across ROS 2 nodes and agent steps
- [ ] Architecture review: identify gaps, simplify where possible

## Phase 8: Showcase & Positioning

- [ ] Final README: full-lifecycle framing, architecture diagram, Motion Primitive protocol section
- [ ] Capture session: barefoot Pirouette en Tendu on the turn-board with the waist node mounted; record 5-8 clean trials
- [ ] Screen-capture live quaternion plots, Tier 1 metric scores, and agent decisions together
- [ ] Film tight-crop B-roll of the rig on the waist and belt (gear visibility over face)
- [ ] Edit a YouTube demo: interleave screen capture with B-roll; voiceover follows Perceive → Reason → Act → Evaluate
- [ ] Upload as unlisted; embed at the top of the READMEs
- [ ] Update the GitHub profile and one-page resume with Rust/ROS 2 and agentic system design (accurately: self-directed project, not professional robotics work)
- [ ] Final polish pass on all READMEs (remove License sections where inaccurate)
- [ ] Draft STAR stories aloud: pivot moment, hardest bug, staff-mindset decision
