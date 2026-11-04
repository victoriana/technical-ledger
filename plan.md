# 9-Week Execution Plan: Physical AI Portfolio (Nov 8, 2026 – Jan 7, 2027)

**Cadence:** Sunday–Friday, 4 hrs/day focused engineering. Saturday = hard rest.
**Working days:** ~48 | **Total hours:** ~192
**Holidays protected:** Thanksgiving (11/26), Christmas Eve/Day (12/24–25), New Year's Day (1/1), GA Filk (1/8–9)
**Critical Goals:** Secure at least one upstream ROS 2 / `rclrs` contribution; validate sensor fusion against physical hardware on SRTP using Pirouette en Tendu as CVM; produce professional YouTube demo.

---

## Hardware Procurement (Pre-Work)

| Date | Action | Source |
|---|---|---|
| Sun 11/8 | Order sensor/processing parts (see list below) | Adafruit + Amazon Prime |
| Mon 11/9 | Order **Koretch Portable Dance Floor (32-inch)** — *Standardized Rotational Test Platform (SRTP)* | Amazon Prime |
| Wed 11/11 | Verify delivery of all components | — |
| Fri 11/13 | Gather fitting materials (compression sleeves, 3M VHB tape, cohesive wrap) | Pharmacy / Amazon |

### Purchase List (~$215 total)
-   6x BNO055 Breakout Boards (Adafruit #1749 or SparkFun SEN-13284)
-   6x ESP32-WROOM-32 Dev Boards (Amazon, DOIT or Espressif brand)
-   6x 3.7V 300mAh LiPo Batteries w/ JST-PH connector (Amazon)
-   6x TP4056 Charging Modules (Amazon)
-   3x Ankle Compression Sleeves + 3x Wrist Support Sleeves (pharmacy/Amazon)
-   1x Wide Elastic Athletic Belt (for waist mount)
-   1x Rigid Plastic Headband + 3M VHB Double-Sided Tape + Cohesive Bandage Wrap
-   1x Koretch Portable Dance Floor (32-inch, Round) — *SRTP*
-   USB-C cables for flashing/debugging

---

## Phase 1 — Rust Foundation & ROS2 Basics (Weeks 1–2)

### Week 1 (Nov 8–14): Rust Language Core
| Day | Deliverable |
|---|---|
| Sun 11/8 | Toolchain install; `rustlings` — variables, functions, control flow; **ORDER HARDWARE** |
| Mon 11/9 | Ownership + borrowing exercises (highest-friction concept; allocate full block) |
| Tue 11/10 | Structs, enums, pattern matching; CLI parsing IMU-shaped JSON |
| Wed 11/11 | Error handling (`Result`, `Option`, `?`); refactor CLI; **ORDER SRTP** |
| Thu 11/12 | Generics + traits; map ROS2 message types to Rust structs via `rclrs` |
| Fri 11/13 | Collections + iterators; moving-average filter in pure Rust; **GATHER FITTING MATERIALS** |

### Week 2 (Nov 15–21): ROS2 Integration Layer
| Day | Deliverable |
|---|---|
| Sun 11/15 | Docker devcontainer: ROS2 Jazzy + Rust + `rclrs`; verify `colcon build` |
| Mon 11/16 | ROS2 fundamentals: nodes/topics/pub-sub (Python reference implementation first) |
| Tue 11/17 | Port talker/listener to Rust; publish synthetic `sensor_msgs/Imu` |
| Wed 11/18 | Second Rust node subscribes; confirm end-to-end pub/sub loop |
| Thu 11/19 | QoS profile analysis: reliability vs best-effort tradeoffs for sensor streams |
| Fri 11/20 | **ROS2 Contribution Sprint #1:** Review `rclrs`/`ros2_tutorials` docs for gaps. Submit PR. |

**Phase Gate:** Two Rust ROS2 nodes exchanging IMU-shaped messages inside devcontainer. All hardware on desk. SRTP ordered.

---

## Phase 2 — Sensor Fusion Core + Hardware Bringup (Weeks 3–5)

### Week 3 (Nov 22–28): Synthetic Data, Calibration & Hardware Assembly
| Day | Deliverable |
|---|---|
| Sun 11/22 | Synthetic IMU generator: constant-rate spin + Gaussian noise + bias drift @ 100Hz |
| Mon 11/23 | EMA bias calibration for accel + gyro; validate against known ground truth |
| Tue 11/24 | Unit tests for calibration module; convergence assertions within tolerance |
| Wed 11/25 | Light day (Thanksgiving week): doc comments, module cleanup |
| Thu 11/26 | **REST — protected holiday** |
| Fri 11/27 | **Hardware Assembly Day:** Flash firmware onto 1 ESP32+BNO055 unit; verify quaternion output over serial. Mount using compression sleeve hack. |

### Week 4 (Nov 29–Dec 5): Madgwick AHRS + Single Node Real Data
| Day | Deliverable |
|---|---|
| Sun 11/29 | Study Madgwick algorithm; understand gradient-descent beta parameter (no code) |
| Mon 11/30 | Implement update step in pure Rust; quaternion state as fixed arrays |
| Tue 12/1 | Validate against synthetic spin; tune beta until angular error < threshold |
| Wed 12/2 | Refactor as reusable struct; configurable sample rate + beta |
| Thu 12/3 | Bind filter to ROS2 subscriber; publish `/imu/waist/quaternion` from **REAL single-node data** |
| Fri 12/4 | Compare real vs synthetic quaternion streams side-by-side; document discrepancies |

### Week 5 (Dec 6–12): Kinematic Metrics & Multi-Node Expansion
| Day | Deliverable |
|---|---|
| Sun 12/6 | Define metric specs: axis drift, tuck efficiency, spotting stability, landing impact |
| Mon 12/7 | Axis drift from waist quaternion → dedicated output topic |
| Tue 12/8 | Tuck efficiency: wristlet radius magnitude vs waist angular velocity |
| Wed 12/9 | Spotting stability: head yaw ≈ 0 while waist yaw high during spin phase |
| Thu 12/10 | Contact-state detection (grounded/flight) via waistband accel signature → ZUPT trigger |
| Fri 12/11 | **Flash + mount remaining 5 nodes.** Run 2-node simultaneous capture (waist + one ankle). Verify timestamp alignment across physical devices. |

**Phase Gate:** Six-node fusion producing four quantitative kinematic metrics. At least two physical nodes streaming concurrently alongside synthetic sources.

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
| Fri 12/18 | **ROS2 Contribution Sprint #2:** Identify missing feature/bug in `rclrs`. Submit PR. Wire agent against **real multi-node data** (all 6 mounted, performing slow controlled spins). |

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

### Week 8 (Dec 27–Jan 2): CI/CD, Observability, CVM Capture, YouTube Demo
| Day | Deliverable |
|---|---|
| Sun 12/27 | GitHub Actions: lint, unit tests, integration tests, Docker image build — green on every push |
| Mon 12/28 | Versioning for agent prompts + calibration params; rollback to last-known-good config |
| Tue 12/29 | Structured logging with correlation IDs across ROS2 nodes and agent steps |
| Wed 12/30 | Final README with full-lifecycle framing + architecture diagram + CVM protocol section |
| Thu 12/31 | **CVM Capture Session:** Perform barefoot Pirouette en Tendu on SRTP. Record 3–5 clean trials. Screen-capture live quaternion plots, metric scores, and agent decisions simultaneously. Film tight-crop B-roll of rig on waist/wrists/head/ankles (face optional; prioritize gear visibility). |
| Fri 1/1 | **REST — New Year's Day** |
| Sat 1/2 | **YouTube Demo Edit:** Interleave screen capture with B-roll. Voiceover script follows Perceive → Reason → Act → Evaluate loop. Upload as unlisted. Embed at top of both READMEs. Final polish pass; pin repos; update public profiles. |

**Phase Gate:** Reproducible build, automated tests, versioned configs, published demo artifact using real CVM data on SRTP.

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
| Thu 1/7 | 45-min mock interview: technical grilling on Acro Spins architecture + DevOps-for-AI questions; finalize applications |

---

## Operating Rules

1. **Saturday is a hard stop.** No commits, no guilt. Recovery compounds across the nine weeks.
2. **Missed day → skip, don't double up.** Momentum protects the schedule better than catch-up sessions.
3. **Stuck >90 minutes?** Log what was tried, move to next task, return fresh next day.
4. **Friday self-review (15 min):** What landed, what slipped, adjust following week's tasks accordingly.
5. **Holiday protection is non-negotiable.** Burnout in week 5 costs more than any single feature.
6. **Hardware fallback:** If a physical node fails irrecoverably, run it in simulation mode with recorded data from a working node. Do not let hardware debugging consume >2 hours on any single day.

## Definition of Done (by Jan 7, 2027)

- [ ] `ros2-rust-workspace`: six-node Rust/ROS2 fusion pipeline, tested, containerized
- [ ] `acro-spin`: full-lifecycle agentic system (perceive → reason → act → evaluate → adapt)
- [ ] **Upstream Contribution:** At least one merged PR (or active discussion) in `rclrs` or `ros2`
- [ ] **Hardware Validation:** Minimum 4 of 6 physical nodes producing stable quaternion streams used in final demo
- [ ] **CVM Executed:** Barefoot Pirouette en Tendu captured on SRTP with synchronized sensor + screen data
- [ ] **YouTube Demo:** Unlisted video embedded in both READMEs showing full lifecycle loop with real hardware footage
- [ ] CI/CD green on both repos; versioned configs with rollback path
- [ ] Architecture diagrams + READMEs written in systems-engineering language
- [ ] 10+ direct outreach conversations initiated across GitHub, Atlanta hybrid, and remote targets
- [ ] Three STAR stories rehearsed and defensible under pressure
