# 9-Week Execution Plan: Physical AI Portfolio (Nov 8, 2026 – Jan 7, 2027)

**Cadence:** Sunday–Friday, 4 hrs/day focused engineering. Saturday = hard rest.
**Working days:** ~48 | **Total hours:** ~192
**Holidays protected:** Thanksgiving (11/26), Christmas Eve/Day (12/24–25), New Year's Day (1/1), GA Filk (1/8–9)

---

## Phase 1 — Rust Foundation & ROS2 Basics (Weeks 1–2)

### Week 1 (Nov 8–14): Rust Language Core
| Day | Deliverable |
|---|---|
| Sun 11/8 | Toolchain install; `rustlings` — variables, functions, control flow |
| Mon 11/9 | Ownership + borrowing exercises (highest-friction concept; allocate full block) |
| Tue 11/10 | Structs, enums, pattern matching; CLI parsing IMU-shaped JSON |
| Wed 11/11 | Error handling (`Result`, `Option`, `?`); refactor CLI for graceful failure |
| Thu 11/12 | Generics + traits; map ROS2 message types to Rust structs via `rclrs` |
| Fri 11/13 | Collections + iterators; implement moving-average filter in pure Rust |

### Week 2 (Nov 15–21): ROS2 Integration Layer
| Day | Deliverable |
|---|---|
| Sun 11/15 | Docker devcontainer: ROS2 Jazzy + Rust + `rclrs`; verify `colcon build` |
| Mon 11/16 | ROS2 fundamentals: nodes/topics/pub-sub (Python reference implementation first) |
| Tue 11/17 | Port talker/listener to Rust; publish synthetic `sensor_msgs/Imu` |
| Wed 11/18 | Second Rust node subscribes; confirm end-to-end pub/sub loop |
| Thu 11/19 | QoS profile analysis: reliability vs best-effort tradeoffs for sensor streams |
| Fri 11/20 | Timestamp alignment utility; document multi-node clock skew implications |

**Phase Gate:** Two Rust ROS2 nodes exchanging IMU-shaped messages inside devcontainer.

---

## Phase 2 — Sensor Fusion Core (Weeks 3–5)

### Week 3 (Nov 22–28): Synthetic Data & Calibration
| Day | Deliverable |
|---|---|
| Sun 11/22 | Synthetic IMU generator: constant-rate spin + Gaussian noise + bias drift @ 100Hz |
| Mon 11/23 | EMA bias calibration for accel + gyro; validate against known ground truth |
| Tue 11/24 | Unit tests for calibration module; convergence assertions within tolerance |
| Wed 11/25 | Light day (Thanksgiving week): doc comments, module structure cleanup |
| Thu 11/26 | **REST — protected holiday** |
| Fri 11/27 | Integrate calibration into ROS2 pipeline; topics carry calibrated data |

### Week 4 (Nov 29–Dec 5): Madgwick AHRS Implementation
| Day | Deliverable |
|---|---|
| Sun 11/29 | Study Madgwick algorithm; understand gradient-descent beta parameter (no code) |
| Mon 11/30 | Implement update step in pure Rust; quaternion state as fixed arrays |
| Tue 12/1 | Validate against synthetic spin; tune beta until angular error < threshold |
| Wed 12/2 | Refactor as reusable struct; configurable sample rate + beta |
| Thu 12/3 | Bind filter to ROS2 subscriber; publish `/imu/waist/quaternion` |
| Fri 12/4 | Extend to all six nodes; verify six concurrent quaternion topics |

### Week 5 (Dec 6–12): Kinematic Metrics & ZUPT Logic
| Day | Deliverable |
|---|---|
| Sun 12/6 | Define metric specs: axis drift, tuck efficiency, spotting stability, landing impact |
| Mon 12/7 | Axis drift from waist quaternion → dedicated output topic |
| Tue 12/8 | Tuck efficiency: wristlet radius magnitude vs waist angular velocity |
| Wed 12/9 | Spotting stability: head yaw ≈ 0 while waist yaw high during spin phase |
| Thu 12/10 | Contact-state detection (grounded/flight) via waistband accel signature → ZUPT trigger |
| Fri 12/11 | Full-pipeline integration test on synthetic acro-spin sequence; plot all metrics |

**Phase Gate:** Six-node fusion producing four quantitative kinematic metrics from simulated data.

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
| Fri 12/18 | Wire graph end-to-end; run full cycle on simulated data |

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

## Phase 4 — Production Hardening (Week 8)

### Week 8 (Dec 27–Jan 2): CI/CD, Observability, Demo
| Day | Deliverable |
|---|---|
| Sun 12/27 | GitHub Actions: lint, unit tests, integration tests, Docker image build — green on every push |
| Mon 12/28 | Versioning for agent prompts + calibration params; rollback to last-known-good config |
| Tue 12/29 | Structured logging with correlation IDs across ROS2 nodes and agent steps |
| Wed 12/30 | Final README with full-lifecycle framing + architecture diagram |
| Thu 12/31 | Record 3-minute screen-capture demo: simulated spin → agent feedback → improved metrics |
| Fri 1/1 | **REST — New Year's Day** |
| Sat 1/2 | Final polish pass; pin repos; update public profiles |

**Phase Gate:** Reproducible build, automated tests, versioned configs, published demo artifact.

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

## Definition of Done (by Jan 7, 2027)

- [ ] `ros2-rust-workspace`: six-node Rust/ROS2 fusion pipeline, tested, containerized
- [ ] `acro-spin`: full-lifecycle agentic system (perceive → reason → act → evaluate → adapt)
- [ ] CI/CD green on both repos; versioned configs with rollback path
- [ ] Architecture diagrams + READMEs written in systems-engineering language
- [ ] Screen-captured demo available for recruiters
- [ ] 10+ direct outreach conversations initiated across GitHub, Atlanta hybrid, and remote targets
- [ ] Three STAR stories rehearsed and defensible under pressure
