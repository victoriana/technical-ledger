**Start Date:** Nov 15, 2026  
**End Date:** Jan 7```markdown
**End Date:** Jan 7, 2027  
**Goal:** Build an agentic Physical AI coach that perceives human movement via IMU sensors, reasons about kinematic metrics using LangGraph, and delivers behavioral coaching interventions to improve the athlete's performance.

## Hardware Inventory (Procured)

| Component | Qty | Status | Role in System |
| :--- | :--- | :--- | :--- |
| Adafruit BNO055 Breakout | 1 | ✅ In hand | Sensor: Converts physics to quaternions (on-chip fusion) |
| ESP32-WROOM-32 Dev Boards | 3 | ✅ In hand | Broadcaster/Edge Compute: I2C read, timestamping, Wi-Fi UDP |
| 3.7V 500mAh LiPo (JST-PH) | 4 | ✅ In hand | Power source (502530 form factor, waist-pouch friendly) |
| TP4056 Charging Modules | 3 | ✅ In hand | Battery management: Voltage/current regulation + protection |
| Micro-USB Data Cables | 2 | ✅ In hand | Flashing/debug station (data-capable, not charge-only) |
| Soldering Kit (80W LCD) | 1 | ✅ In hand | Final assembly only (fine conical tip for TP4056 pads) |
| Athletic Belt / Waist Pouch | 1 | ✅ In hand | Rigid mounting — sensor must not rotate independently of torso |
| 3M VHB Tape + Cohesive Wrap | 1 kit | ✅ In hand | Rigidity during spins is the #1 data-quality risk |
| Dot2Dance Turn-board |1 | ✅ In hand | To physically do the moves/pirouette |

**Critical Constraint:** The BNO055 is a single unit with no spares. It stays sealed in its anti-static bag until Week 3, after all software pipelines are validated against synthetic data.

**Architecture Decision:** Direct Wi-Fi (SoftAP mode). Waist ESP32 creates its own network; PC connects directly. No gateway node required. No router purchase needed yet.

---

## Week 1 (Nov 15–21): Rust Core & Ownership

- [ ] Install Rust toolchain (rustup, cargo, rust-analyzer in VS Code)
- [ ] Complete `rustlings` exercises: variables, functions, control flow
- [ ] Deep dive into ownership, borrowing, lifetimes — write small programs that deliberately break reference rules to understand errors
- [ ] Define `Quaternion` struct and `ImuState` enum; implement pattern matching on sensor states
- [ ] Implement error handling (`Result`, `Option`, `?`) in a CLI parser for IMU-shaped JSON
- [ ] Create a `Sensor` trait with `read()` and `calibrate()` methods; implement for `FakeSensor`
- [ ] Build a moving-average filter using pure Rust iterators (no raw loops)
- [ ] Write summary notes in `technical-ledger` documenting key Rust concepts learned

## Week 2 (Nov 22–28): ROS2 Integration & Synthetic Data

- [ ] Set up Docker devcontainer with ROS2 Jazzy + Rust + `rclrs`; verify `colcon build` succeeds
- [ ] Write Python talker/listener nodes to confirm understanding of pub/sub fundamentals
- [ ] Port talker/listener to Rust using `rclrs`; publish synthetic `sensor_msgs/Imu`
- [ ] Create second Rust subscriber node; confirm end-to-end pub/sub loop inside container
- [ ] Experiment with QoS profiles: compare `Reliable` vs `BestEffort` at 100Hz stream rates
- [ ] Build synthetic IMU generator in Rust: constant-rate spin + Gaussian noise + bias drift shaped as pirouette profiles (yaw ramps through 360° crossings + injected wobble)
- [ ] Implement EMA bias calibration for accel + gyro; validate against known ground truth
- [ ] Write unit tests for calibration module with convergence assertions within tolerance

## Week 3 (Nov 29–Dec 5): Hardware Bringup (Bench Only, No Soldering)

- [ ] Flash ESP32 #1 via USB with SoftAP firmware creating `AcroSpin_Waist` network
- [ ] Wire BNO055 on jumper leads (SDA→GPIO21, SCL→GPIO22, 3V3, GND) — breadboard only
- [ ] Multimeter-check TP4056 polarity against battery lead before first connection
- [ ] Verify quaternion output over USB serial before removing cable
- [ ] Test wireless path: connect laptop to `AcroSpin_Waist`, receive UDP packets in Rust listener
- [ ] Validate full power chain: LiPo → TP4056 → ESP32 VIN → 3V3 pin → BNO055
- [ ] Mount to waist belt temporarily with cohesive wrap; perform 5 test spins while streaming
- [ ] Document discrepancies between bench data and field data

## Week 4 (Dec 6–12): Kinematic Metrics (Tier 1)

- [ ] Define serial frame format: timestamp, quat wxyz, gyro xyz, sequence number
- [ ] Implement zero-copy parser in Rust; add fuzzed/malformed frame unit tests
- [ ] Add sequence-gap detection to identify packet loss during spins
- [ ] Bind bridge to ROS2 publisher; confirm `/imu/waist` topic live from real hardware
- [ ] Implement Rotation Count: yaw 360°-crossing counter; field-test with 10 slow pirouettes
- [ ] Implement Peak Angular Velocity + Smoothness (std-dev of yaw rate per 360° segment)
- [ ] Implement Settling Time + Posture (pitch/roll deviation from vertical)
- [ ] Compare real vs synthetic streams side-by-side; document discrepancies in README

## Week 5 (Dec 13–19): Agentic Layer — Perception & Reasoning

- [ ] Design state machine diagram before writing code: Perceive → Plan → Act → Evaluate
- [ ] Set up persistent session memory (SQLite or JSON): athlete profile, baselines, prior scores
- [ ] Build perception node: subscribe to ROS2 metrics topics → format agent context window
- [ ] Build reasoning node: LLM call outputs structured coaching decision object (not free text)
- [ ] Populate strategy bank with escalating physical interventions per metric:
  - Low Smoothness → Attempt 1: "Slow your entry to preserve momentum" → Attempt 2: "Engage core earlier and keep arms tight to ribs" → Attempt 3: "Reduce arm range of motion entirely"
  - High Settling Time → Attempt 1: "Focus on a fixed horizon point before stopping" → Attempt 2: "Land on balls of feet with knees slightly bent" → Attempt 3: "Hold final position for 3 seconds to prove stability"
  - High Pitch/Roll Deviation → Attempt 1: "Keep head directly over hips, look straight ahead" → Attempt 2: "Tighten glutes to maintain straight vertical axis" → Attempt 3: "Imagine a string pulling you up from the crown of your head"
- [ ] Track whether each intervention was followed and whether score improved to determine if escalation is needed
- [ ] If advice failed, escalate to next strategy automatically rather than repeating

## Week 6 (Dec 20–26): Evaluation Loop & Guardrails

- [ ] Build evaluation node: compute score deltas per attempt; did intervention improve outcome?
- [ ] Feed evaluation results into memory; update baseline on statistically significant improvement
- [ ] Implement guardrails: LLM rate limiting, output schema validation, timeout/failure handling
- [ ] Wire full agent loop against real single-node field data from waist-mounted trials
- [ ] Perform 8 clean pirouette trials; log raw quaternions, Tier 1 metrics, and agent decisions simultaneously
- [ ] Architecture review day: identify gaps, simplify where possible
- [ ] REST days protected (Dec 24–25)

## Week 7 (Dec 27–Jan 2): Production Hardening & Demo Capture

- [ ] GitHub Actions pipeline: lint, unit tests, integration tests, Docker image build — green on every push
- [ ] Version agent prompts + calibration params; implement rollback to last-known-good config
- [ ] Structured logging with correlation IDs across ROS2 nodes and agent steps
- [ ] Write final README: full-lifecycle framing, architecture diagram, Motion Primitive protocol section
- [ ] Motion Primitive Capture Session: barefoot Pirouette en Tendu on SRTP with waist node mounted; record 5–8 clean trials
- [ ] Screen-capture live quaternion plots, Tier 1 metric scores, and agent decisions simultaneously
- [ ] Film tight-crop B-roll of rig on waist and belt (prioritize gear visibility over face)
- [ ] Edit YouTube demo: interleave screen capture with B-roll; voiceover follows Perceive → Reason → Act → Evaluate loop
- [ ] Upload as unlisted; embed at top of both READMEs

## Week 8 (Jan 3–7): Portfolio Polish & Positioning

- [ ] Update GitHub profile: pin acro-spin, ros2-rust-workspace, technical-ledger
- [ ] Update One-Page Resume: add Physical AI, Rust/ROS2, and agentic system design sections
- [ ] Submit PR to `ros2/rclrs`: minimal Rust example demonstrating QoS profiles for high-rate sensor streams (BestEffort + Volatile), with README note explaining why these settings matter for IMU data
- [ ] Final polish pass on all READMEs including License section removal where inaccurate
- [ ] Draft STAR stories aloud from this project: pivot moment, hardest bug, staff-mindset decision
- [ ] Rest day Jan 1 (New Year's Day)

---

## Scope Rules

- If behind schedule, cut scope (drop one metric or simplify graph). Never extend hours. Consistency beats intensity.
- BNO055 remains sealed until Week 3. Any soldering before synthetic pipeline passes is prohibited.
- Do not revisit lit-flow-flash agency work until acro-spin demo is published.
- Job search activities remain separate from this plan entirely.
```The previous response completed the full 8-week plan markdown block, ending with the closing fence and the scope rules section. There is no remaining content to continue.
