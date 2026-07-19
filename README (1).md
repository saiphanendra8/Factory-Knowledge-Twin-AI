# Factory Twin AI: Snapdragon-Powered Edge AI & RAG Copilot

An industrial-grade Dual-Twin (Web + Android mobile) predictive maintenance suite tailored for Qualcomm Smart Factory ecosystems. This application leverages real-time sensor processing via custom hardware loops alongside edge-compiled LLM retrieval frameworks (RAG) to dynamically predict manufacturing fault matrices, cross-examine procedural documents, and suggest parts procurement paths before a line failure happens.

---

## 🏗️ System Architecture Blueprint

```
                     +---------------------------------------+
                     |        Industrial Equipment Pool       |
                     |  [CNC Lathe, Hydraulic Press, etc.]   |
                     +---------------------------------------+
                                         |
                                         v (Physical Sensors)
                     +---------------------------------------+
                     |         Arduino UNO Q Node Loop       |
                     |      (High-Frequency Data Sampling)   |
                     +---------------------------------------+
                                         |
                                         v (USB Serial Interface / COM)
+-----------------------------------------------------------------------------------------+
| Qualcomm Snapdragon Edge AI PC Server Node                                              |
|                                                                                         |
|   +--------------------------+   +------------------------+   +---------------------+   |
|   | Telemetry Stream Engine  |-->| Data Sanitization Loop |-->| Local SQLite Matrix |   |
|   | (Multi-Threaded Listener)|   | (Safety & Validation)  |   | (Asset Registry)    |   |
|   +--------------------------+   +------------------------+   +---------------------+   |
|                 |                                                                       |
|                 v                                                                       |
|   +--------------------------+                                +---------------------+   |
|   | Async WebSocket Dispatch |                                | Native ONNX Embeds  |   |
|   | (Telemetry Broadcast)    |                                | & FAISS Vector Store|   |
|   +--------------------------+                                +---------------------+   |
|                 |                                                        |              |
|                 +--------------------------------+                       v              |
|                 |                                |            +---------------------+   |
|                 |                                +----------->| Edge LLM RAG Pipeline|   |
|                 |                                             | (Local Phi-3 State) |   |
+-----------------|---------------------------------------------+---------------------+---+
                  |                                                        ^
                  v (WS / Telemetry Stream)                                | (RAG REST API / JSON)
+---------------------------------------------+          +-----------------+-------------------+
| Multi-Twin Cross-Platform Client Stack      |          | Mobile Maintenance Edge Station    |
|                                             |          |                                     |
|   +-------------------------------------+   |          |   +-----------------------------+   |
|   | React 18 Web Management Dashboard   |   |          |   | Android Native App Client   |   |
|   | - Recharts Interactive Stream Line  |   |          |   | - Jetpack Compose Interface |   |
|   | - Plant Asset Health Matrix         |   |          |   | - CameraX ML Kit QR Scanner |   |
|   | - Vector Ingestion Portal (RAG upload) |          |   | - On-Site RAG Data Injector |   |
|   +-------------------------------------+   |          |   +-----------------------------+   |
+---------------------------------------------+          +-------------------------------------+
```

---

## 🔄 Core Process Flow Matrix

### 1. Telemetry Stream & Fault Verification Sequence
```
[Arduino Sensors] ---> (Serial Stream) ---> [Telemetry Engine]
                                                   |
                                                   v
                                        {Data Sanitization?}
                                         /              \
                                    (No) /                \ (Yes)
                                        v                  v
                            [Trigger Validation Err]   [Calculate Health Indices]
                                        |                  |
                                        v                  v
                            (Broadcast UI Alerts)     (Update State to WebSocket Loop)
                                                           |
                                                           v
                                                      [React Stream / Live Chart Render]
```

### 2. Snapdragon Local Edge RAG Retrieval Loop
```
[User Query Input] ---> [Context Retrieval Request] ---> [Extract Live Asset Sensor Data]
                                                                    |
                                                                    v
                                                        [FAISS Vector Store Scan]
                                                       (Manuals, Logs, Technician Notes)
                                                                    |
                                                                    v
                                                        [Construct Enriched Prompt]
                                                                    |
                                                                    v
                                                        [Local Phi-3 Edge Execution]
                                                                    |
                                                                    v
                                                      [Render Answer with Multi-Citations]
```

---

## 🛠️ Stack Dependencies & Core Engines

### Web Dashboard Stack
*   **React 18.3+** – Declarative component state-machine tree rendering.
*   **Lucide React** – Vector iconography representing technical metrics.
*   **Recharts** – Native SVG-driven mathematical path area plots for incoming streaming frames.
*   **Native WebSockets API** – Event-driven persistent telemetry connectivity.

### Mobile Android Client Stack
*   **Jetpack Compose Suite (3.x)** – Native declarative UI layer rendering.
*   **CameraX API** – Hardware-abstracted smartphone camera interface loop.
*   **Google ML Kit Barcode Vision** – Edge Neural Engine QR scanning decoding matrix.
*   **Kotlin Coroutines (Asynchronous Flow)** – Multi-threaded input/output blocking isolation.

---

## 📁 Repository Directory Structure

```
factory-twin-ai/
├── README.md                      # Judging Portal Operational Manifesto
├── web-dashboard/                 # Central Control Room Web Workspace
│   ├── public/                    # Manifests, favicons, static resources
│   └── src/
│       ├── App.jsx                # Web Dashboard Core Interface & WS Lifecycle
│       ├── main.jsx               # React DOM context bootstrap pipeline
│       └── index.css              # Glassmorphic component layout rules
├── mobile-android/                # On-Site Technician Handheald Workspace
│   └── src/main/java/com/factorykt/
│       ├── MainActivity.kt        # Application Lifecycle & View Entry-point
│       ├── ManualsScreen.kt       # Native Component: PDF-procedural Matrix
│       └── ProcurementScreen.kt   # Native Component: Predictive Part Booking
└── firmware/                      # Edge Hardware Source
    └── firmware.ino               # Arduino Sensor Loop & JSON Serializer
```

---

## 🚀 Quick Start Guide (Judging Panel Assessment)

Follow these clear steps to review the Factory Twin AI multi-client deployment locally:

### 1. Launching the Web Dashboard Control Center
```bash
# Navigate to the dashboard directory
cd web-dashboard

# Install necessary component assets
npm install

# Run the local Vite server instance
npm run dev
```
*   **Access Path:** Open `http://localhost:5173` inside your browser frame.
*   **Credentials:** Username: `admin` | Password: `qualcomm2026`.

### 2. Deploying the Mobile Technician Application
1. Import the `/mobile-android` repository node directly inside **Android Studio**.
2. Connect your mobile evaluation hardware device or spin up an **AVD Emulator instance** with back-camera routing active.
3. Configure the **PC Server IPv4 Address** on the screen toolbar to reference your machine's network IP (e.g., `192.168.1.XX`).
4. Click **Build & Run App**. Use the credentials `admin` / `qualcomm2026` to bypass security filters.

### 3. Testing Local RAG Intelligence Matrix
*   Navigate to the **AI Assistant** layout node on either client platform.
*   Input the question phrase: `How do I fix spindle overheating?` or `Piston seal replacement steps`.
*   Observe the response block: The system aggregates localized database attributes alongside contextual manuals and injects exact origin citations inline.
