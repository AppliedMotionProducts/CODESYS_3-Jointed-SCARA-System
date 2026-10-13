# 🤖 3-Jointed SCARA Robot — CODESYS SoftMotion

A complete motion-control reference project for a **3-jointed SCARA robotic manipulator** built entirely on Applied Motion Products hardware and programmed in **CODESYS SoftMotion**. Demonstrates forward/inverse kinematic transformations, coordinated multi-axis motion, and real-world integration between a motion controller PLC and heterogeneous servo/stepper drives on a single bus.

<p align="center">
  <img width="600" alt="3-jointed SCARA system" src="https://github.com/user-attachments/assets/0d6d7746-bcc3-4e72-90a5-81c0de5b7053" />
</p>

<p align="center">
  <img alt="CODESYS" src="https://img.shields.io/badge/-CODESYS-EA1B22?style=for-the-badge&logoColor=white" />
  <img alt="SoftMotion" src="https://img.shields.io/badge/-SoftMotion-003366?style=for-the-badge&logoColor=white" />
  <img alt="EtherCAT" src="https://img.shields.io/badge/-EtherCAT-7F2A2A?style=for-the-badge&logoColor=white" />
  <img alt="SCARA" src="https://img.shields.io/badge/-SCARA%20Kinematics-0A66C2?style=for-the-badge&logoColor=white" />
  <img alt="License" src="https://img.shields.io/badge/License-Apache%202.0-blue.svg?style=for-the-badge" />
</p>

---

## 🎯 What this project does

- Implements a **3-jointed SCARA** kinematic chain in CODESYS using the built-in SoftMotion [`SMC_TRAFOF_THREEJOINTEDSCARA`](https://content.helpme-codesys.com/en/CODESYS%20SoftMotion/_sm_trafo_pou_three_jointed_scara.html) transformation function block
- Coordinates **three axes** (one M5 EtherCAT servo + two AK StepServo drives) under one motion controller
- Streams synchronized setpoints through CODESYS SoftMotion → cyclic Cartesian motion in XYZ space
- Demonstrates mixed-drive integration: a servo on Joint 1 for high-performance base rotation, closed-loop steppers on Joints 2 & 3 for precise and economical arm actuation

Reference application for engineers building pick-and-place, assembly, inspection, or dispensing cells using Applied Motion hardware.

---

## 🔧 Hardware (Bill of Materials)

| Component | Product | Role | Link |
|---|---|---|---|
| 🧠 **Motion Controller** | AMP **MCA6** CODESYS Motion PLC | Runs the SCARA kinematics and SoftMotion runtime | [applied-motion.com/mca6](https://www.applied-motion.com/s/mca6-met2) |
| ⚙️ **Joint 1 Drive** | **M5 EtherCAT Servo Drive + Motor** | Base rotation (high-dynamic performance) | [applied-motion.com/m5-servos](https://www.applied-motion.com/s/m5-servo-motor-and-drive-series) |
| ⚙️ **Joint 2 Drive** | **AK Series StepServo Drive** | Arm segment (closed-loop stepper) | [applied-motion.com/ak-series](https://www.applied-motion.com/s/ak-series) |
| ⚙️ **Joint 3 Drive** | **AK Series StepServo Drive** | End segment (closed-loop stepper) | [applied-motion.com/ak-series](https://www.applied-motion.com/s/ak-series) |

All drives share a single **EtherCAT fieldbus** connected to the MCA6, giving the SoftMotion kernel direct cyclic access to each axis.

---

## 📐 Kinematics overview

The project uses CODESYS SoftMotion's canonical three-jointed SCARA transformation. In short:

- **Forward kinematics** → given joint angles `(θ₁, θ₂, θ₃)`, compute end-effector `(X, Y, Z)`
- **Inverse kinematics** → given target `(X, Y, Z)`, solve for `(θ₁, θ₂, θ₃)`
- SoftMotion handles both internally. The application only commands Cartesian setpoints; the kinematics POU translates to per-axis targets each cycle.

🔗 Reference: [SoftMotion — Three-Jointed SCARA Transformation](https://content.helpme-codesys.com/en/CODESYS%20SoftMotion/_sm_trafo_pou_three_jointed_scara.html)

---

## 📂 Repository contents

| File | Purpose |
|---|---|
| [`ARM_Demo_RevA.project`](ARM_Demo_RevA.project) | The complete CODESYS project — PLC program, SoftMotion configuration, kinematics POU, HMI visualization |
| [`README.md`](README.md) | This file |
| [`LICENSE`](LICENSE) | Apache-2.0 license |

---

## 🚀 Getting started

### Prerequisites

- **CODESYS Development System V3.5 SP19** or newer (free download from [store.codesys.com](https://store.codesys.com))
- **CODESYS SoftMotion** add-on package
- **CODESYS EtherCAT Master** package
- AMP MCA6 PLC (or any CODESYS-compatible motion controller) + the three drives listed above
- EtherCAT cabling in daisy-chain order: MCA6 → M5 → AK₁ → AK₂

### Open the project

1. Clone or download this repository
2. Launch CODESYS, open **`ARM_Demo_RevA.project`**
3. Confirm the EtherCAT device tree matches your physical hardware; adjust slave addresses if needed
4. Build the project (**F11**) and download to the MCA6
5. Switch to RUN, open the HMI visualization, and jog the robot in Cartesian space

### Tuning

- Joint limits and kinematic parameters (segment lengths, offsets) are defined as constants in the kinematics POU — update them to match your mechanical build
- Default motion profile velocities are conservative. Increase them in SoftMotion's axis configuration once you've validated your envelope

---

## 📚 References

- [CODESYS SoftMotion — SCARA transformations](https://content.helpme-codesys.com/en/CODESYS%20SoftMotion/_sm_trafo_pou_three_jointed_scara.html)
- [AMP MCA6 CODESYS Motion PLC](https://www.applied-motion.com/s/mca6-met2)
- [AMP M5 EtherCAT Servo Drives](https://www.applied-motion.com/s/m5-servo-motor-and-drive-series)
- [AMP AK Series StepServo Drives](https://www.applied-motion.com/s/ak-series)
- [CODESYS Store (free dev system download)](https://store.codesys.com)

---

## 📝 License

Apache License 2.0 — see [LICENSE](LICENSE). You're free to use, modify, and redistribute this reference project (including commercially); keep the copyright notice and NOTICE preserved on redistribution.

---

## 💬 Questions or feedback?

Open an issue in this repo, or reach out through:
- **Website:** [applied-motion.com](https://www.applied-motion.com)
- **GitHub organization:** [@AppliedMotionProducts](https://github.com/AppliedMotionProducts)

Pull requests welcome — especially additional kinematic configurations (Delta, 6-axis articulated), end-effector attachments, or language ports of the HMI.
