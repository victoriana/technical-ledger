# 9-Week Execution Plan: Physical AI Portfolio (Nov 8, 2026 – Jan 7, 2027)

**Cadence:** Sunday–Friday, 4 hrs/day focused engineering. Saturday = hard rest.
**Working days:** ~48 | **Total hours:** ~192
**Holidays protected:** Thanksgiving (11/26), Christmas Eve/Day (12/24–25), New Year's Day (1/1), GA Filk (1/8–9)
**Critical Goals:** Secure at least one upstream ROS 2 / `rclrs` contribution; validate sensor ingestion against physical hardware on SRTP using Pirouette en Tendu as Motion Primitive; produce professional YouTube demo.

---

## Hardware Procurement (Pre-Work)

| Date | Action | Source |
|---|---|---|
| Mon 11/9 | **In-person:** Gather fitting materials (waist belt/pouch, tape, strap). **Online:** Order electronics. | Amazon / Adafruit |
| Thu 11/12 | Verify delivery of all components | — |

### Purchase List (~$108 total)

Scope decision: **single waist-mounted node** for field validation. Multi-node kinematics move to simulation + bench testing with spare boards. This cut hardware cost from ~$354 to ~$108 while preserving every Tier 1 rubric metric.

| Item | Qty | Est. Cost | Buy Where | Notes |
|---|---|---|---|---|
| **BNO055 Breakout Board** | 1 | $34.95 | Adafruit | 9-DOF, 100Hz onboard fusion (Path A). Waist node only. |
| **ESP32-WROOM-32 Dev Boards** | 3 (1 pack) | $16.99 | Amazon | 1 waist node + 2 spares. Spares exercise `esp32-bridge` N=2 path on the bench. |
| **3.7V 500mAh LiPo (JST-PH)** | 4 (1 pack) | $24.99 | Amazon | 1 in use, 3 spares. 502530 form factor: 35×30×5mm, ~10g — waist-pouch friendly. |
| **TP4056 Charging Modules (w/ protection)** | 3 (1 pack) | $5.99 | Amazon | Type-C, 4-pad B+/B−/OUT+/OUT−. 1 in use, 2 spares. |
| **Micro-USB Data Cables** | 2 (1 pack) | $6.59 | Amazon | 1 flashing/debug station, 1 spare. Must be data-capable, not charge-only. |
| **Soldering Kit (80W LCD, adjustable)** | 1 | $18.98 | Amazon | Fine conical tip required for TP4056 pads. Includes desolder pump + flux. |
| **Athletic Belt or Waist Pouch** | 1 | ~$10 | In person | Rigid mounting. Sensor must not rotate independently of torso. |
| **3M VHB Tape + Cohesive Wrap** | 1 kit | ~$10 | In person | Rigidity during spins is the #1 data-quality risk. |
| **Total Hardware** | | **~$108** | | *SRTP (Koretch Turnboard) already owned. i.e. The Dot2Dance Turnboard* |

**Mounting Note:** Waist placement was chosen because pirouettes are fundamentally a yaw rotation about the vertical axis, and yaw is the BNO055's most stable axis (its pitch axis has a known gimbal-lock limitation near ±90°, which is irrelevant for an upright torso).

**Standardization Note:** BNO055 exclusively. It works reliably with ESP32-WROOM-32 over I2C and performs Bosch's sensor fusion onboard, so the Rust pipeline ingests fused quaternions directly rather than re-running Madgwick on already-fused data.

**Fusion Path Note:** `imu-fusion` (Madgwick AHRS) remains in the workspace as a hardware-agnostic primitive and is exercised via synthetic raw-data tests. It is bypassed for the BNO055 deployment.

---

## Phase 1 — Rust Foundation & ROS2 Basics (Weeks 1–2)

### Week 1 (Nov 8–14): Rust Language Core
| Day | Deliverable |
|---|---|
| Sun 11/8 | Toolchain install; `rustlings` — variables, functions, control flow |
| Mon 11/9 | **ORDER HARDWARE + GATHER FITTING MATERIALS**; ownership + borrowing exercises |
| Tue 11/10 | Structs, enums, pattern matching; CLI parsing IMU-shaped JSON |
| Wed 11/11 | Error handling (`Result`, `Option`, `?`); refactor CLI |
| Thu 11/12 | Generics + traits; map ROS2 message types to Rust structs via `rclrs` |
| Fri 11/13 | Collections + iterators; moving-average filter in pure Rust |

### Week 2 (Nov 15–21): ROS2 Integration Layer
| Day | Deliverable |
|---|---|
| Sun 11/15 | Docker devcontainer: ROS2 Jazzy + Rust + `rclrs`; verify `colcon build` |
| Mon 11/16 | ROS2 fundamentals: nodes/topics/pub-sub (Python reference implementation first) |
| Tue 11/17 | Port talker/listener to Rust; publish synthetic `sensor_msgs/Imu` |
| Wed 11/18 | Second Rust node subscribes; confirm end-to-end pub/sub loop |
| Thu 11/19 | QoS profile analysis: reliability vs best-effort tradeoffs for sensor streams |
| Fri 11/20 | **ROS2 Contribution Sprint #1:** Review `rclrs`/`ros2_tutorials` docs for gaps. Submit PR. |

**Phase Gate:** Two Rust ROS2 nodes exchanging IMU-shaped messages inside devcontainer. Hardware on desk. SRTP on floor.

---

## Phase 2 — Ingestion Core + Hardware Bringup (Weeks 3–5)

### Week 3 (Nov 22–28): Synthetic Data, Calibration & Single-Node Assembly
| Day | Deliverable |
|---|---|
| Sun 11/22 | Synthetic IMU generator: constant-rate spin + Gaussian noise + bias drift @ 100Hz, shaped as pirouette profiles (yaw ramps through 360° crossings + injected wobble) |
| Mon 11/23 | EMA bias calibration for accel + gyro; validate against known ground truth |
| Tue 11/24 | Unit tests for calibration module; convergence assertions within tolerance |
| Wed 11/25 | Light day (Thanksgiving week): doc comments, module cleanup |
| Thu 11/26 | **REST — protected holiday** |
| Fri 11/27 | **Hardware Assembly Day:** Flash ESP32 #1. Wire BNO055 on jumper leads (SDA→GPIO21, SCL→GPIO22, 3V3, GND) and verify quaternion output over USB serial *before* soldering anything. Multimeter-check TP4056 polarity against battery lead. Mount to waist belt. |

### Week 4 (Nov 29–Dec 5): Real Data Ingestion + Tier 1 Metrics
| Day | Deliverable |
|---|---|
| Sun 11/29 | `esp32-bridge`: define serial frame format (timestamp, quat wxyz, gyro xyz, seq); implement zero-copy parser |
| Mon 11/30 | Parser unit tests with fuzzed/malformed frames; sequence-gap detection |
| Tue 12/1 | Bind bridge to ROS2 publisher; `/imu/waist` live from **REAL single-node data** |
| Wed 12/2 | Rotation Count: yaw 360°-crossing counter. Field-test with 10 slow pirouettes; verify count matches intent |
| Thu 12/3 | Peak Angular Velocity + Smoothness (std-dev of yaw rate per 360° segment) |
| Fri 12/4 | Settling Time + Posture (pitch/roll deviation from vertical). Compare real vs synthetic streams side-by-side; document discrepancies |

### Week 5 (Dec 6–12): Multi-Node Path Validation (Bench + Sim)
| Day | Deliverable |
|---|---|
| Sun 12/6 | Define Tier 2 metric specs (Alignment, Symmetry) as interfaces only — no hardware required yet |
| Mon 12/7 | Axis drift from waist quaternion → dedicated output topic |
| Tue 12/8 | Contact-state detection (grounded/flight) via waist accel signature → ZUPT trigger |
| Wed 12/9 | **Bench multi-node test:** flash ESP32 #2, run two simultaneous serial streams through `esp32-bridge`. Confirm N≥2 path and per-port namespace isolation |
| Thu 12/10 | Exercise `time-sync` across the two physical ports; assert alignment within tolerance |
| Fri 12/11 | Publish simulated proximal/distal/base streams alongside the live waist node. Document that Tier 2 metrics are interface-complete but hardware-gated |

**Phase Gate:** One physical node streaming stable quaternions into ROS 2, producing all five Tier 1 metrics. Two physical ports validated concurrently in `esp32-bridge`. Tier 2 metrics spec'd and simulated.

---

## Phase 3 — Agentic Layer (Weeks 6–7)

### Week 6 (Dec 13–19): LangGraph Architecture & Memory
| Day | Deliverable |
|---|---|
| Sun 12/13 | Design state machine: perceive → plan → act → evaluate (diagram before code) |
| Mon 12/14 | Persistent session memory (SQLite/JSON): athlete profile, baselines, prior scores |
| Tue 12/15 | Perception node: subscribe ROS2 metrics → format agent context window |
| Wed 12/16 | Reasoning node: LLM call outputs structured coaching decision object (not free text) |
| Thu 12/17 | Action node: publish ROS2 feedback topic + write annotated telemetry to disk |
| Fri 12/18 | **ROS2 Contribution Sprint #2:** Identify missing feature/bug in `rclrs`. Submit PR. Wire agent against **real single-node field data** from waist-mounted trials. |

### Week 7 (Dec 20–26): Evaluation Loop & Guardrails
| Day | Deliverable |
|---|---|
| Sun 12/20 | Evaluation node: compute score deltas per attempt; did intervention improve outcome? |
| Mon 12/21 | Feed evaluation into memory; update baseline on statistically significant improvement |
| Tue 12/22 | Guardrails: LLM rate limiting, output schema validation, timeout/failure handling |
| Wed 12/23 | Light day: architecture review, gap identification |
| Thu 12/24 | **REST — protected holiday** |
| Fri 12/25 | **REST — protected holiday** |

**Scope rule:** If behind schedule, cut scope (drop one metric or simplify graph). Never extend hours. Consistency beats intensity.

---

## Phase 4 — Production Hardening & Demo (Week 8)

### Week 8 (Dec 27–Jan 2): CI/CD, Observability, Motion Primitive Capture, YouTube Demo
| Day | Deliverable |
|---|---|
| Sun 12/27 | GitHub Actions: lint, unit tests, integration tests, Docker image build — green on every push |
| Mon 12/28 | Versioning for agent prompts + calibration params; rollback to last-known-good config |
| Tue 12/29 | Structured logging with correlation IDs across ROS2 nodes and agent steps |
| Wed 12/30 | Final README with full-lifecycle framing + architecture diagram + Motion Primitive protocol section |
| Thu 12/31 | **Motion Primitive Capture Session:** Perform barefoot Pirouette en Tendu on SRTP with waist node mounted. Record 5–8 clean trials (more trials compensate for single-sensor coverage). Screen-capture live quaternion plots, Tier 1 metric scores, and agent decisions simultaneously. Film tight-crop B-roll of rig on waist and belt (face optional; prioritize gear visibility). |
| Fri 1/1 | **REST — New Year's Day** |
| Sat 1/2 | **YouTube Demo Edit:** Interleave screen capture with B-roll. Voiceover script follows Perceive → Reason → Act → Evaluate loop. Upload as unlisted. Embed at top of both READMEs. Final polish pass; pin repos; update public profiles. |

**Phase Gate:** Reproducible build, automated tests, versioned configs, published demo artifact using real Motion Primitive data on SRTP.

---

## Phase 5 — Launch (Week 9)

### Week 9 (Jan 3–7): Positioning & Direct Outreach
Target mix: **GitHub roles** + **Atlanta hybrid** + **national remote**, pursued through both company portals and direct recruiter contact.

| Day | Deliverable |
|---|---|
| Sun 1/3 | Resume rewrite: impact-language bullets, quantified outcomes, Physical AI framing |
| Mon 1/4 | Rehearse three STAR stories aloud (pivot, hardest bug, staff-mindset); record and review |
| Tue 1/5 | Build target list (~15 orgs): GitHub, Anduril, GTRI, Greenzie, Mujin, Eureka, remote Physical AI startups; note each org's recent AI/robotics announcements |
| Wed 1/6 | First outreach batch: 5 personalized recruiter/hiring-manager notes + 3 portal applications with tailored cover notes |
| Thu 1/7 | 45-min mock interview: technical grilling on Acro Spin's architecture + DevOps-for-AI questions; finalize applications |

---

## Operating Rules

1. **Saturday is a hard stop.** No commits, no guilt. Recovery compounds across the nine weeks.
2. **Missed day → skip, don't double up.** Momentum protects the schedule better than catch-up sessions.
3. **Stuck >90 minutes?** Log what was tried, move to next task, return fresh next day.
4. **Friday self-review (15 min):** What landed, what slipped, adjust following week's tasks accordingly.
5. **Holiday protection is non-negotiable.** Burnout in week 5 costs more than any single feature.
6. **Hardware fallback:** If the physical node fails irrecoverably, run it in simulation mode with recorded data captured before failure. Do not let hardware debugging consume >2 hours on any single day.
7. **Soldering safety:** Multimeter-verify battery polarity against every TP4056 board before first connection. Reversed polarity kills the module instantly — this is why 2 spares were bought.
8. **Data-quality gate:** Before trusting any metric, confirm the sensor does not shift on the belt mid-spin. Slippage produces phantom yaw that looks like real rotation.

## Definition of Done (by Jan 7, 2027)

- [ ] `ros2-rust-workspace`: Rust/ROS2 ingestion pipeline, tested, containerized, validated N=1 and N=2
- [ ] `acro-spin`: full-lifecycle agentic system (perceive → reason → act → evaluate → adapt)
- [ ] **Upstream Contribution:** At least one merged PR (or active discussion) in `rclrs` or `ros2`
- [ ] **Hardware Validation:** Physical waist node producing stable quaternion streams used in final demo, plus two concurrent ports validated in `esp32-bridge`
- [ ] **Tier 1 Metrics Live:** All five single-node metrics computed from real field data
- [ ] **Motion Primitive Executed:** Barefoot Pirouette en Tendu captured on SRTP with synchronized sensor + screen data
- [ ] **YouTube Demo:** Unlisted video embedded in both READMEs showing full lifecycle loop with real hardware footage
- [ ] CI/CD green on both repos; versioned configs with rollback path
- [ ] Architecture diagrams + READMEs written in systems-engineering language
- [ ] 10+ direct outreach conversations initiated across GitHub, Atlanta hybrid, and remote targets
- [ ] Three STAR stories rehearsed and defensible under pressure
