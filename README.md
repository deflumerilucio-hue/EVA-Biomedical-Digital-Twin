# Multimodal Space Telemetry & Biomechanical Monitoring System
**Author:** Lucio De Flumeri  
**Program:** Master's Degree in Biomedical and Aerospace Engineering (LM-21)  
**Status:** Public Portfolio Showcase

---

## 1. Project Overview
This repository hosts the architectural framework of an advanced multimodal telemedical system designed for real-time biomechanical and psycho-physical monitoring. Originally developed to support postural and motor rehabilitation, the system's core architecture is tailored for **human spaceflight applications**—specifically targeting the monitoring and rehabilitation of astronauts recovering from microgravity-induced musculoskeletal deconditioning.

The infrastructure bridges **Computer Vision** and **Computational Mechanics** through a decoupled multi-process architecture communicating via local **UDP protocols** at 30 FPS.

---

## 2. System Architecture & Data Flow

```text
[Webcam Feed]
      │
      ▼
[Python Vision Module (OpenCV / MediaPipe)]
      │
      │ UDP Stream @ 30 FPS
      ▼
[MATLAB HMI & FEM Engine]
      │
      ├── Trunk Kinematics (L1 Strain)
      │
      ├── Facial Micro-Expressions (Stress Index)
      │
      ├── 1D Non-Linear Viscoelastic FEM Model
      │
      └── Real-Time 3D Structural Rendering
```

### Module 1: Python Virtual Sensor (`sensore_multimodale.py`)

- **Frameworks:** OpenCV, MediaPipe Holistic, Python Sockets.
- **Functionality:** Operates as a real-time virtual sensor extracting skeletal and facial landmarks.

#### Trunk Kinematics → Strain ($\epsilon$)

Tracks shoulder and hip vertical coordinates to compute relative torso compression against an upright baseline.

A physiological clamping mechanism is applied to estimate microstructural strain on the L1 lumbar vertebra.

#### Facial Stress Index

Isolates inner eyebrow landmarks using the MediaPipe face mesh to quantify acute psychological or physical stress through Euclidean distance variations.

The resulting signal is mapped to a standardized **0–100 stress index**.

#### Communication

Streams lightweight comma-separated telemetry payloads:

```text
strain,stress_index
```

via a non-blocking local UDP socket:

```text
127.0.0.1:5006
```

The architecture is designed to minimize communication overhead and support low-latency real-time transmission.

---

### Module 2: MATLAB HMI & Structural Solver (`AeroSport_HMI.mlapp`)

- **Environment:** MATLAB App Designer
- **Numerical Engine:** Custom FEM Solvers

#### Process Orchestration

Spawns and manages the background Python vision process using system hooks, maintaining a clear separation between computer-vision processing and numerical computation.

#### 1D Non-Linear FEM Engine

Implements predictor-corrector iterations for viscoelastic material behavior and continuous damage mechanics ($d$).

The solver evaluates equilibrium and non-equilibrium stress components while accounting for trabecular bone degradation under compressive loading conditions.

#### Dynamic Visualization

Features:

- Sliding-window telemetry buffers
- Memory-managed real-time data handling
- Live numerical monitoring
- Real-time 3D cylinder deformation
- Synchronization between incoming kinematic telemetry and structural simulation

---

## 3. Key Engineering Highlights

### Multi-Process Decoupling

Asynchronous separation between computationally intensive AI/computer-vision inference in Python and numerical finite-element computation in MATLAB.

This architecture provides clear module boundaries and facilitates independent development, testing, and optimization.

### Robust Error Handling

Algorithmic constraints are applied to:

- Time-step variations ($\Delta t$)
- Strain limits
- Telemetry validation
- Numerical state variables

These mechanisms reduce the risk of matrix instability, numerical overflow, and unrealistic real-time states.

### Systems Engineering Approach

The project follows a systems-oriented architecture based on:

- Functional decomposition
- Explicit interface definition
- Modular software boundaries
- Real-time telemetry management
- Computational separation of concerns
- Numerical stability considerations
- Memory-managed data streams

---

## 4. Technology Stack

| Domain | Technology |
|---|---|
| Computer Vision | Python, OpenCV, MediaPipe |
| Communication | UDP / Python Sockets |
| Numerical Computing | MATLAB |
| GUI | MATLAB App Designer |
| Structural Modeling | Custom FEM Solver |
| Biomechanics | Musculoskeletal / Trabecular Bone Modeling |
| Real-Time Processing | Multi-process architecture |
| Data Handling | Sliding-window telemetry buffers |
| Hardware Interface | Webcam / Virtual Sensors |

---

## 5. System-Level Concept

```text
                    ┌─────────────────────┐
                    │     HUMAN USER      │
                    │  / ASTRONAUT MODEL  │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │    WEBCAM SENSOR    │
                    └──────────┬──────────┘
                               │
                               ▼
              ┌────────────────────────────────┐
              │     PYTHON VIRTUAL SENSOR      │
              │                                │
              │  OpenCV + MediaPipe Holistic  │
              │                                │
              │  • Skeletal Landmarks         │
              │  • Facial Landmarks            │
              │  • Kinematic Extraction        │
              │  • Stress Index                │
              └───────────────┬────────────────┘
                              │
                              │ UDP @ 30 FPS
                              ▼
              ┌────────────────────────────────┐
              │       MATLAB INTERFACE         │
              │                                │
              │       AeroSport_HMI            │
              └───────────────┬────────────────┘
                              │
                ┌─────────────┴─────────────┐
                ▼                           ▼
      ┌──────────────────┐       ┌────────────────────┐
      │ TELEMETRY ENGINE │       │   FEM SOLVER       │
      │                  │       │                    │
      │ Strain           │       │ Non-Linear Model   │
      │ Stress Index     │       │ Viscoelasticity    │
      │ Time Series      │       │ Damage Mechanics   │
      └────────┬─────────┘       └─────────┬──────────┘
               │                           │
               └─────────────┬─────────────┘
                             ▼
                  ┌──────────────────────┐
                  │ REAL-TIME 3D MODEL   │
                  │                      │
                  │ Structural Response  │
                  │ Telemetry Feedback   │
                  └──────────────────────┘
```

---

## 6. Human Spaceflight Application

The architecture can be conceptually extended to **human spaceflight biomedical monitoring**.

Potential applications include:

- Monitoring musculoskeletal deconditioning during and after microgravity exposure
- Real-time biomechanical assessment
- Astronaut rehabilitation
- Wearable physiological sensing
- Crew health telemetry
- Human-machine interfaces
- Biomedical digital twins
- Structural response monitoring
- AI-assisted anomaly detection
- Ground-based astronaut rehabilitation systems

The current implementation is a **research and portfolio prototype**, not a flight-certified medical or aerospace system.

---

## 7. Systems Engineering Perspective

The project is intentionally structured around a systems engineering philosophy.

### Input

```text
Human / Sensor Data
        ↓
Kinematic & Physiological Parameters
```

### Processing

```text
Data Acquisition
        ↓
Signal Extraction
        ↓
Telemetry Transmission
        ↓
Numerical Modeling
        ↓
Structural / Biomechanical Analysis
```

### Output

```text
Real-Time Telemetry
        +
Biomechanical State Estimation
        +
Structural Response
        ↓
Human-Machine Interface
```

The architecture demonstrates the integration of heterogeneous engineering domains within a single real-time system.

---

## 8. Future Development

Potential future iterations include:

- Integration of wearable IMUs
- ECG / EMG acquisition
- SpO₂ and heart-rate monitoring
- Multi-sensor fusion
- Embedded hardware implementation
- Python-based numerical backend
- ROS 2 integration
- Real-time physiological telemetry
- Machine-learning-based anomaly detection
- Digital-twin architecture
- Hardware-in-the-loop testing
- Automated requirements verification
- Fault detection and isolation
- Aerospace-grade communication interfaces

---

## 9. Repository Structure

```text
.
├── sensore_multimodale.py
├── AeroSport_HMI.mlapp
├── README.md
│
├── /models
│   └── biomechanical_models
│
├── /data
│   └── sample_telemetry
│
├── /docs
│   └── system_architecture
│
└── /assets
    └── figures
```

---

## 10. Engineering Value

This project demonstrates practical experience in:

- **Systems Engineering**
- **Biomedical Engineering**
- **Aerospace-oriented system architecture**
- **Computer Vision**
- **Biomechanical modeling**
- **Finite Element Methods**
- **Real-time telemetry**
- **Multi-process software architecture**
- **Numerical simulation**
- **Human-machine interfaces**
- **Sensor integration**
- **Python ↔ MATLAB interoperability**

The main engineering objective is the integration of heterogeneous computational modules into a coherent real-time monitoring architecture.

---

## 11. Portfolio Disclaimer

This repository is intended as a **technical portfolio and systems engineering demonstration**.

It does not represent a flight-certified aerospace or medical system.

Advanced proprietary algorithms, research data, and restricted implementation details are intentionally omitted from the public repository.

---

## 12. Author

**Lucio De Flumeri**

Biomedical & Aerospace Engineering  
Systems Engineering • Human Spaceflight • Biomedical Systems • Computational Biomechanics

---
```
