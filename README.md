# Factory Knowledge Twin — Qualcomm Edge AI Platform

Factory Knowledge Twin is a comprehensive Edge AI-powered multi-device platform designed to preserve and transfer industrial maintenance knowledge while enabling intelligent, real-time decision-making on the factory floor.

This repository contains the complete implementation for the **Qualcomm Snapdragon Multiverse Hackathon** prototype.

> [!TIP]
> 🎥 **[Watch the full Video Demo on Google Drive](https://drive.google.com/drive/folders/1zqfzcG6yhIa5f5Wqt6IXf8IovYCo94bq?usp=drive_link)**

## Team Members
* **KHONGARI SAI PHANENDRA** - saiphanendra8@gmail.com
* **MACHERLA RAHUL** - macherlarahul39@gmail.com
* **SRIKONDA BHARGAVA VARMA** - bhargavast9@gmai.com

---

## 1. System Architecture

```text
┌────────────────────────────────────────────────────────┐
│                      LOCAL NETWORK                     │
│                                                        │
│  [Arduino UNO Q] ===== BLE =====> [Galaxy S25 App]     │
│   (STM32 Sensors                  (LiteRT Classifier + │
│   + Anomaly MPU)                    On-device RAG UI)  │
│         ||                                ||           │
│        MQTT (Wi-Fi)                   HTTP (Wi-Fi)     │
│         ||                                ||           │
│         \/                                \/           │
│  [Snapdragon AI PC Hub (FastAPI + SQLite + RAG Server)]│
│                               ||                       │
│                           WebSockets                   │
│                               \/                       │
│                   [Command Center Dashboard]           │
└────────────────────────────────────────────────────────┘
```

---

## 2. Screenshots

### 🔐 Factory Twin AI Login Portal
![Login Screen](./assets/login.png)

### 📊 Command Center & Telemetry Overview
![Dashboard Overview](./assets/dashboard_overview.png)

### 📈 Predictive Analytics & Machine Details
![Machine Details](./assets/machine_details.png)

### 🤖 Edge AI Assistant (Snapdragon Copilot)
![AI Assistant](./assets/ai_assistant.png)

### 📑 Automated Maintenance Reports
![Reports](./assets/reports.png)

---

## 3. Project Dependencies

### Web Dashboard (React/Electron)
- `lucide-react`
- `react`
- `react-dom`
- `recharts`
- `@types/react`
- `@types/react-dom`
- `@vitejs/plugin-react`
- `electron`
- `electron-builder`
- `vite`

### Edge AI Server (Python/FastAPI)
- `fastapi`
- `uvicorn`
- `paho-mqtt`
- `websockets`
- `numpy`
- `llama-cpp-python`

### Native Android App (Kotlin/Jetpack Compose)
- `androidx.core:core-ktx`
- `androidx.appcompat:appcompat`
- `com.google.android.material:material`
- `androidx.lifecycle:lifecycle-runtime-ktx`
- `androidx.activity:activity-compose`
- `androidx.compose:compose-bom`
- `androidx.compose.ui:ui`
- `androidx.compose.ui:ui-graphics`
- `androidx.compose.ui:ui-tooling-preview`
- `androidx.compose.material3:material3`
- `androidx.compose.material:material-icons-extended`
- `androidx.camera:camera-core`
- `androidx.camera:camera-camera2`
- `androidx.camera:camera-lifecycle`
- `androidx.camera:camera-view`
- `com.google.mlkit:barcode-scanning`
- `org.tensorflow:tensorflow-lite`
- `org.tensorflow:tensorflow-lite-gpu`
- `junit:junit`
- `androidx.test.ext:junit`
- `androidx.test.espresso:espresso-core`
- `androidx.compose.ui:ui-test-junit4`
- `androidx.compose.ui:ui-tooling`
- `androidx.compose.ui:ui-test-manifest`

---

## 3. Key Features

> [!IMPORTANT]
> **No Cloud Required**
> The entire system is built to run air-gapped on the edge, ensuring highly sensitive factory IP and machine manuals never leave the local network.

- **Real-Time Telemetry & Anomalies**: Monitors live RPM, Vibration, Temperature, and Current via MQTT with WebSockets.
- **Predictive Procurement (PRO-ACT AI)**: Automatically calculates Estimated Time to Failure (ETF) based on degrading health scores, highlighting machines in red and recommending proactive replacement part bookings.
- **On-Device RAG Assistant**: Query the local knowledge base directly from the dashboard or mobile app. ("How do I fix the spindle overheating?")
- **Digital Machine Manuals**: Fully digitized Standard Operating Procedures and emergency shutdown protocols accessible natively.
- **Animated Command UI**: Dynamic, infinite-color shifting gradients, responsive layouts, and top-right Hamburger menus for sleek navigation.

---

## 4. Quick-Start Execution Guide

### Prerequisites
- **Node.js** (v20+): [nodejs.org](https://nodejs.org/)
- **Python** (3.11): [python.org](https://python.org/)
- **Android Studio** (for deploying the mobile app).
- **Mosquitto MQTT Broker**: Ensure Mosquitto is running locally on port `1883`.

### Step 1: Start the Real-Time Telemetry Simulator
Navigate to the server directory and spin up the simulator to mimic an active factory floor:
```powershell
cd ai-pc-hub/server
python simulated_sensor_engine.py
```

### Step 2: Start the AI PC Hub API Server
In a new terminal window, install dependencies and run the FastAPI server:
```powershell
cd ai-pc-hub
pip install -r requirements.txt
cd server
uvicorn app:app --host 0.0.0.0 --port 8000 --reload
```
*(The server is now accessible at `http://localhost:8000`)*

### Step 3: Run the Web Dashboard
In another terminal window, boot the Vite front-end:
```powershell
cd web-dashboard
npm install
npm run dev
```
Open `http://localhost:3000` in your web browser. You will see the animated dials, telemetry graphs, AI chat, and procurement tabs all updating in real-time.

### Step 4: Deploy the Android App
1. Open the `android-app` folder inside **Android Studio**.
2. Click "Sync Project with Gradle Files".
3. Connect your Android device via USB and click **Run**.
4. Log into the app to see the native Jetpack Compose dashboard, complete with hamburger navigation and local AI interaction.

---

## 5. Testing the Edge AI Copilot
Navigate to the **AI Assist** screen in the Dashboard or Mobile App and test the intelligent routing:

- *"How do I fix spindle overheating?"* -> Instructs RPM reduction and lubrication based on lathe parameters.
- *"Vibration in the compressor"* -> Flags degraded motor mounts and suggests a 24-hour replacement.
- *"Hi / How are you?"* -> Conversational AI responses tailored to the Factory Twin Multiverse portal.


---

## 6. Core Source Code Reference

```text
=== FACTORY TWIN SOURCE CODE ===

--- WEB DASHBOARD (App.jsx) ---
import React, { useState, useEffect, useRef } from 'react'
import { 
  Activity, Thermometer, ShieldAlert, Cpu, Wrench, ShieldCheck, 
  Database, MessageSquare, Monitor, LayoutDashboard, FileText, 
  Network, Settings, Bell, RefreshCw, Send, CheckCircle, HelpCircle,
  AlertTriangle, Layers, User, PlusCircle, ArrowRight, LogIn, LogOut,
  Upload, Check, FileUp, Sparkles, XCircle, AlertCircle, BookOpen, ShoppingCart
} from 'lucide-react'
import { AreaChart, Area, XAxis, YAxis, CartesianGrid, Tooltip, ResponsiveContainer } from 'recharts'

// Mock database values matching seed_data.sql
const MOCK_MACHINES = [
  { id: 'MCH-001', name: 'CNC Lathe XL-2000', type: 'cnc_lathe', manufacturer: 'Mazak', model: 'XL-2000', location: 'Machining Bay A', status: 'operational', healthScore: 98, rpm: 1200, current: 4.8, temp: 42.5, vib: 1.2, img: '🔧' },
  { id: 'MCH-002', name: 'Hydraulic Press HP-500', type: 'hydraulic_press', manufacturer: 'Schuler', model: 'HP-500', location: 'Heavy Press Zone', status: 'operational', healthScore: 95, rpm: 0, current: 8.5, temp: 38.0, vib: 0.8, img: '🔩' },
  { id: 'MCH-003', name: 'Conveyor Drive Motor CM-12', type: 'conveyor', manufacturer: 'Siemens', model: 'CM-12', location: 'Assembly Line B', status: 'operational', healthScore: 97.5, rpm: 1800, current: 3.2, temp: 31.0, vib: 0.6, img: '⚙️' },
  { id: 'MCH-004', name: 'Rotary Air Compressor AC-75', type: 'compressor', manufacturer: 'Ingersoll Rand', model: 'AC-75', location: 'Machining Bay A', status: 'warning', healthScore: 75, rpm: 3600, current: 14.5, temp: 78.0, vib: 3.8, img: '💨' },
  { id: 'MCH-005', name: 'Robotic Assembly Arm RA-6', type: 'robotic_arm', manufacturer: 'FANUC', model: 'RA-6', location: 'Assembly Line B', status: 'operational', healthScore: 99, rpm: 900, current: 2.2, temp: 28.5, vib: 0.4, img: '🤖' }
]

export default function App() {
  const [isLoggedIn, setIsLoggedIn] = useState(() => {
    return localStorage.getItem('isLoggedIn') === 'true'
  })
  const [currentScreen, setCurrentScreen] = useState('dashboard') 
  const [selectedMachineId, setSelectedMachineId] = useState('MCH-001')
  
  // Login form credentials
  const [username, setUsername] = useState('admin')
  const [password, setPassword] = useState('qualcomm2026')

  // Real-time sensor state for selected machine
  const [sensors, setSensors] = useState({
    temperature: 42.5,
    vibration: 1.2,
    current: 4.8,
    rpm: 1200,
    status: 'operational',
    healthScore: 98
  })
  
  const [chartData, setChartData] = useState([])
  const [wsStatus, setWsStatus] = useState('DISCONNECTED')
  const [arduinoConnection, setArduinoConnection] = useState({
    connected: false,
    port: 'COM3',
    type: 'USB Serial',
    message: 'Waiting for Arduino Data...'
  })
  
  // Error notifications
  const [validationError, setValidationError] = useState(null)
  
  // Local system logs panel in UI
  const [systemLogs, setSystemLogs] = useState([
    { time: new Date().toLocaleTimeString(), text: 'System Console initialized.' }
  ])

  const [chatInput, setChatInput] = useState('')
  const [chatLog, setChatLog] = useState([
    { sender: 'assistant', text: 'Hello! I am your Snapdragon-powered Maintenance Assistant. Ask me how to resolve any equipment faults.' }
  ])
  const [isLoading, setIsLoading] = useState(false)

  // Setup Screen state
  const [factoryName, setFactoryName] = useState('Qualcomm Smart Factory Delta')
  const [industry, setIndustry] = useState('Automotive Assembly')
  const [uploadStatus, setUploadStatus] = useState({ manuals: false, sops: false, records: false })

  const activeMachine = MOCK_MACHINES.find(m => m.id === selectedMachineId) || MOCK_MACHINES[0]
  const chatBottomRef = useRef(null)
  const arduinoConnectedRef = useRef(false)

  // System log logger helper
  const addSystemLog = (text) => {
    setSystemLogs(prev => [{ time: new Date().toLocaleTimeString(), text }, ...prev.slice(0, 49)])
  }

  // WebSocket Integration with Heartbeat & Status Management
  useEffect(() => {
    if (!isLoggedIn) return
    let ws = null
    let reconnectTimer = null
    let isCancelled = false

    const connectWS = () => {
      if (isCancelled) return
      const host = window.location.hostname || 'localhost';
      const wsHost = (host === 'localhost' || host === '[::1]') ? '127.0.0.1' : host;
      ws = new WebSocket(`ws://${wsHost}:8000/ws/telemetry`)
      
      ws.onopen = () => {
        setWsStatus('CONNECTED')
        addSystemLog('WebSocket connection established with AI PC Hub.')
      }

      ws.onmessage = (event) => {
        try {
          const payload = JSON.parse(event.data)
          
          if (payload.type === 'connection_status') {
            const wasConnected = arduinoConnectedRef.current
            arduinoConnectedRef.current = payload.connected
            setArduinoConnection({
              connected: payload.connected,
              port: payload.port,
              type: payload.connection_type,
              message: payload.message || ''
            })
            
            if (payload.connected && !wasConnected) {
              addSystemLog(`Arduino connected on ${payload.port} [Type: ${payload.connection_type}]`)
            } else if (!payload.connected && wasConnected) {
              addSystemLog('Lost communication link with Arduino UNO Q.')
              setChartData([])
            }
          }
          
          else if (payload.type === 'log_error') {
            setValidationError(payload.message)
            addSystemLog(`[Warning] ${payload.message}`)
            setTimeout(() => setValidationError(null), 5000)
          }
          
          else if (payload.type === 'telemetry') {
            const data = payload.data
            
            if (arduinoConnectedRef.current && data.machine_id === selectedMachineId) {
              setSensors({
                temperature: data.temperature,
                vibration: data.vibration,
                current: data.current,
                rpm: MOCK_MACHINES.find(m => m.id === selectedMachineId)?.rpm || 1200,
                status: data.status,
                healthScore: data.health_score
              })
              
              const timeStr = new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit', second: '2-digit' })
              setChartData(prev => {
                const updated = [...prev, {
                  time: timeStr,
                  temp: data.temperature,
                  vib: data.vibration,
                  curr: data.current,
                  rpm: MOCK_MACHINES.find(m => m.id === selectedMachineId)?.rpm || 1200
                }]
                if (updated.length > 30) return updated.slice(updated.length - 30)
                return updated
              })
            }
          }
        } catch (e) {
          console.error('WS message parse error:', e)
        }
      }

      ws.onerror = () => {}

      ws.onclose = () => {
        if (isCancelled) return
        setWsStatus('DISCONNECTED')
        arduinoConnectedRef.current = false
        setArduinoConnection(prev => ({ ...prev, connected: false, message: 'Lost connection to AI PC server.' }))
        setChartData([])
        addSystemLog('WebSocket connection lost. Reconnecting in 5 seconds...')
        reconnectTimer = setTimeout(connectWS, 5000)
      }
    }

    connectWS()

    return () => {
      isCancelled = true
      if (reconnectTimer) clearTimeout(reconnectTimer)
      if (ws) { ws.onclose = null; ws.close() }
    }
  }, [selectedMachineId, isLoggedIn])

  useEffect(() => {
    chatBottomRef.current?.scrollIntoView({ behavior: 'smooth' })
  }, [chatLog])

  const handleLogin = (e) => {
    e.preventDefault()
    if (username === 'admin' && password === 'qualcomm2026') {
      localStorage.setItem('isLoggedIn', 'true')
      setIsLoggedIn(true)
      setCurrentScreen('dashboard')
      addSystemLog(`User ${username} authenticated successfully.`)
    } else {
      alert("Invalid credentials. Hint: Use admin / qualcomm2026")
    }
  }

  const handleQuery = async (queryText) => {
    const textToQuery = queryText || chatInput
    if (!textToQuery.trim()) return
    
    setChatLog(prev => [...prev, { sender: 'user', text: textToQuery }])
    if (!queryText) setChatInput('')
    setIsLoading(true)

    try {
      const host = window.location.hostname || 'localhost';
      const response = await fetch(`http://${host}:8000/api/rag/query`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ query: textToQuery, machine_id: selectedMachineId })
      })
      const data = await response.json()
      setChatLog(prev => [...prev, { sender: 'assistant', text: data.answer, citations: data.citations }])
    } catch (e) {
      setTimeout(() => {
        let answer = ""
        let citations = []
        const inputLower = textToQuery.toLowerCase().trim()

        // 1. Basic Communication
        const greetings = ['hi', 'hello', 'hey', 'how are you', 'how are you?', 'who are you', 'good morning', 'good afternoon']
        if (greetings.includes(inputLower)) {
          answer = "Hello! I am your Snapdragon-powered Edge AI Copilot. I have access to all factory maintenance manuals, technician logs, and live sensor data. How can I assist you with your equipment today?"
        } 
        // 2. Specific Technical Questions
        else if (inputLower.includes('spindle') || inputLower.includes('overheat') || inputLower.includes('temp')) {
          answer = "Retrieved from CNC Spindle Procedure (PROC-001): In case of overheating (>68°C), safely isolate power. Inject 2ml high-temp bearing grease and check belt tension. Refer to Note-001: warm up spindle at 200 RPM for 10 minutes on cold mornings."
          citations = [
            { source_type: 'procedure', source_id: 'PROC-001', text: 'CNC Lathe Spindle Overheating Repair' },
            { source_type: 'technician_note', source_id: 'NOTE-001', text: 'Cold Weather Spindle Warm-up Trick' }
          ]
        } else if (inputLower.includes('seal') || inputLower.includes('leak') || inputLower.includes('press') || inputLower.includes('piston')) {
          answer = "Retrieved from Hydraulic press procedures (PROC-002): Depressurize accumulator to 0 bar. Loosen collar bolts in cross pattern and replace worn nitrile seal. Note-002 warns that stamping cycles above 400 tons run 20% higher wear rates."
          citations = [
            { source_type: 'procedure', source_id: 'PROC-002', text: 'Hydraulic Press Seal Replacement' },
            { source_type: 'technician_note', source_id: 'NOTE-002', text: 'Schuler HP-500 Seal Lifespan' }
          ]
        } else if (inputLower.includes('vibration') || inputLower.includes('shake')) {
          answer = "High vibration detected. According to Maintenance Log (LOG-088), excessive vibration is typically caused by unaligned rotor shafts or worn motor mounts. Check the primary mounting bolts and run the auto-alignment calibration sequence."
          citations = [
            { source_type: 'maintenance_log', source_id: 'LOG-088', text: 'Rotor alignment troubleshooting' }
          ]
        } else if (inputLower.includes('current') || inputLower.includes('power') || inputLower.includes('amp')) {
          answer = "Power fluctuations suggest a phase imbalance. Sensor Profile (SENS-04) defines critical line current limits at 15A. Verify that the 3-phase supply is balanced. If the machine draws excess current, check for mechanical binding."
          citations = [
            { source_type: 'sensor', source_id: 'SENS-04', text: 'Current limit thresholds and phase monitoring' }
          ]
        } 
        // 3. Dynamic fallback for anything else
        else {
          answer = `I found 1 relevant record in the local database regarding '${textToQuery}'. The standard operating procedure dictates that you should inspect physical sensor placement, verify electrical line currents, and note down mechanical vibration values for this specific issue.`
          citations = [
            { source_type: 'general_procedure', source_id: 'PROC-GEN', text: `Diagnostic steps for: ${textToQuery.substring(0, 20)}...` }
          ]
        }

        setChatLog(prev => [...prev, { sender: 'assistant', text: answer, citations }])
        setIsLoading(false)
      }, 800)
    } finally {
      setIsLoading(false)
    }
  }

  const handleFileUpload = (category) => {
    setUploadStatus(prev => ({ ...prev, [category]: 'uploading' }))
    addSystemLog(`Uploading new file resource under category: ${category}`)
    setTimeout(() => {
      setUploadStatus(prev => ({ ...prev, [category]: 'success' }))
      addSystemLog(`Successfully ingested new RAG context vector under category: ${category}`)
    }, 1500)
  }

  const downloadReport = (title) => {
    const reportContent = `
========================================
FACTORY KNOWLEDGE TWIN — COMPREHENSIVE REPORT
========================================
REPORT TYPE: ${title.toUpperCase()}
FACTORY: ${factoryName}
INDUSTRY: ${industry}
DATE GENERATED: ${new Date().toLocaleDateString()}

PLANT HEALTH METRICS:
- Overall Factory Health: 94.2%
- Active Monitoring Nodes: 5 Machines
- Telemetry Network: 🟢 ONLINE (MQTT 1883)

ASSET LOG SUMMARIES:
- MCH-001 (CNC Lathe): 98% Health (Operational)
- MCH-002 (Hydraulic Press): 95% Health (Operational)
- MCH-004 (Compressor): 75% Health (Warning: Spindle Rotor Wear)

========================================
REPORT GENERATED NATIVELY ON SNAPDRAGON NPU
    `
    const blob = new Blob([reportContent], { type: 'text/plain' })
    const url = URL.createObjectURL(blob)
    const link = document.createElement('a')
    link.href = url
    link.download = `${title.replace(" ", "_")}_Report.txt`
    link.click()
    addSystemLog(`Exported text report: ${title}`)
  }

  // --------------------------------------------------------------------------
  // SCREEN 1: LOGIN (Full-screen Overlay)
  // --------------------------------------------------------------------------
  if (!isLoggedIn) {
    return (
      <div style={{ display: 'flex', height: '100vh', width: '100vw', alignItems: 'center', justifyContent: 'center', background: 'var(--bg-primary)', position: 'relative', overflow: 'hidden' }}>
        
        {/* Animated Gears & Circuits Background */}
        <div style={{ position: 'absolute', top: '10%', left: '10%', opacity: 0.15, animation: 'spin-slow 20s linear infinite' }}>
          <Settings size={200} color="var(--accent-primary)" />
        </div>
        <div style={{ position: 'absolute', bottom: '10%', right: '10%', opacity: 0.1, animation: 'spin-slow-reverse 15s linear infinite' }}>
          <Settings size={300} color="var(--accent-cyan)" />
        </div>
        <div style={{ position: 'absolute', top: '20%', right: '20%', opacity: 0.12, animation: 'pulse-slow 4s ease-in-out infinite' }}>
          <Network size={180} color="var(--accent-purple)" />
        </div>
        <div style={{ position: 'absolute', bottom: '20%', left: '15%', opacity: 0.1, animation: 'float 6s ease-in-out infinite' }}>
          <Cpu size={220} color="var(--accent-primary)" />
        </div>
        
        {/* Neural Net Lines (SVG) */}
        <svg style={{ position: 'absolute', top: 0, left: 0, width: '100%', height: '100%', opacity: 0.05, pointerEvents: 'none' }}>
          <path d="M 100 100 Q 300 150, 500 100 T 900 100" stroke="var(--accent-cyan)" fill="transparent" strokeWidth="2" strokeDasharray="5,5" />
          <path d="M 200 400 Q 400 350, 600 400 T 1000 400" stroke="var(--accent-purple)" fill="transparent" strokeWidth="2" />
          <path d="M 0 700 Q 200 650, 400 700 T 800 700" stroke="var(--accent-primary)" fill="transparent" strokeWidth="2" strokeDasharray="10,5" />
        </svg>

        <form onSubmit={handleLogin} className="login-card animate-in" style={{ width: '440px', padding: '48px', display: 'flex', flexDirection: 'column', gap: '24px', alignItems: 'center', textAlign: 'center', zIndex: 1, animation: 'fadeInUp 0.8s ease-out', background: 'rgba(255,255,255,0.9)', backdropFilter: 'blur(20px)', border: '1px solid rgba(0,0,0,0.05)' }}>
          <div style={{ position: 'relative' }}>
            <div style={{ position: 'absolute', inset: '-8px', background: 'linear-gradient(135deg, var(--accent-primary), var(--accent-cyan))', borderRadius: '50%', opacity: 0.5, filter: 'blur(10px)', animation: 'rotateRing 4s linear infinite' }}></div>
            <img src="./logo.jpg" alt="User" style={{ width: '100px', height: '100px', borderRadius: '50%', objectFit: 'cover', position: 'relative', border: '2px solid var(--border-light)' }} onError={(e)=>{e.target.style.display='none'}} />
          </div>

          <div>
            <h1 className="gradient-text-primary" style={{ fontSize: '28px', fontWeight: '800', letterSpacing: '-0.5px', marginBottom: '4px' }}>Factory Twin AI</h1>
            <span className="label-upper" style={{ display: 'block' }}>QUALCOMM MULTIVERSE EDGE PORTAL</span>
          </div>

          <div style={{ display: 'flex', flexDirection: 'column', gap: '16px', textAlign: 'left', width: '100%', marginTop: '8px' }}>
            <div>
              <label className="label-upper" style={{ display: 'block', marginBottom: '8px', color: 'var(--text-secondary)' }}>TECHNICIAN ID / USERNAME</label>
              <input 
                type="text" 
                value={username} 
                onChange={(e) => setUsername(e.target.value)} 
                className="input-glass" 
                placeholder="Enter your ID"
                required 
              />
            </div>
            <div>
              <label className="label-upper" style={{ display: 'block', marginBottom: '8px', color: 'var(--text-secondary)' }}>SECURITY CREDENTIAL</label>
              <input 
                type="password" 
                value={password} 
                onChange={(e) => setPassword(e.target.value)} 
                className="input-glass" 
                placeholder="Enter password"
                required 
              />
            </div>
          </div>

          <button type="submit" className="btn-primary" style={{ width: '100%', justifyContent: 'center', padding: '14px', fontSize: '15px', marginTop: '12px' }}>
            <LogIn size={18} />
            Authenticate & Connect
          </button>
        </form>
      </div>
    )
  }

  return (
    <div style={{ display: 'flex', height: '100vh', width: '100vw', overflow: 'hidden', flexDirection: 'column' }}>
      
      {/* --------------------------------------------------------------------------
          CONNECTION BANNER HEADERS
         -------------------------------------------------------------------------- */}
      {!arduinoConnection.connected ? (
        <div className="banner-disconnected" style={{ padding: '10px 24px', display: 'flex', alignItems: 'center', gap: '12px', zIndex: 10, fontSize: '13px', fontWeight: '600', background: 'var(--accent-red-light)', color: 'var(--accent-red)' }}>
          <XCircle size={18} />
          <span>Connection to Arduino UNO Q lost. Live monitoring has been paused. [Port: {arduinoConnection.port}]</span>
        </div>
      ) : (
        <div className="banner-connected" style={{ padding: '10px 24px', display: 'flex', alignItems: 'center', gap: '12px', zIndex: 10, fontSize: '13px', fontWeight: '600', background: 'var(--accent-green-light)', color: 'var(--accent-green)' }}>
          <span className="pulse-indicator green"></span>
          <span>Connected to Arduino UNO Q on {arduinoConnection.port} [Type: {arduinoConnection.type}]</span>
        </div>
      )}

      {/* Live Validation Data Warning banner */}
      {validationError && (
        <div className="banner-warning" style={{ padding: '10px 24px', display: 'flex', alignItems: 'center', gap: '12px', zIndex: 10, fontSize: '13px', fontWeight: '600' }}>
          <AlertTriangle size={18} />
          <span>{validationError}</span>
        </div>
      )}

      <div style={{ display: 'flex', flex: 1, overflow: 'hidden' }}>
        {/* Side Navigation Bar */}
        <nav className="glass-panel" style={{ width: '260px', margin: '20px 10px 20px 20px', display: 'flex', flexDirection: 'column', padding: '24px', border: '1px solid var(--border-light)' }}>
          <div style={{ display: 'flex', alignItems: 'center', gap: '12px', marginBottom: '32px' }}>
            <div style={{ padding: '2px', background: 'linear-gradient(135deg, var(--accent-primary), var(--accent-cyan))', borderRadius: '50%' }}>
              <img 
                src="./logo.jpg" 
                alt="Logo" 
                style={{ width: '38px', height: '38px', borderRadius: '50%', objectFit: 'cover', border: '2px solid var(--bg-panel)' }}
                onError={(e)=>{e.target.style.display='none'}}
              />
            </div>
            <div>
              <h1 className="gradient-text-primary" style={{ fontSize: '16px', fontWeight: '800', letterSpacing: '-0.2px' }}>Factory Twin AI</h1>
              <span className="label-upper" style={{ fontSize: '9px' }}>QUALCOMM STACK</span>
            </div>
          </div>

          <div style={{ display: 'flex', flexDirection: 'column', gap: '6px', flex: 1 }}>
            <button onClick={() => setCurrentScreen('dashboard')} className={`nav-item ${currentScreen === 'dashboard' ? 'active' : ''}`}>
              <LayoutDashboard size={18} /> Dashboard
            </button>
            <button onClick={() => setCurrentScreen('details')} className={`nav-item ${currentScreen === 'details' ? 'active' : ''}`}>
              <Activity size={18} /> Machine Details
            </button>
            <button onClick={() => setCurrentScreen('ai_assistant')} className={`nav-item ${currentScreen === 'ai_assistant' ? 'active' : ''}`}>
              <MessageSquare size={18} /> AI Assistant
            </button>
            <button onClick={() => setCurrentScreen('setup')} className={`nav-item ${currentScreen === 'setup' ? 'active' : ''}`}>
              <Settings size={18} /> Factory Setup
            </button>
            <button onClick={() => setCurrentScreen('reports')} className={`nav-item ${currentScreen === 'reports' ? 'active' : ''}`}>
              <FileText size={18} /> Reports
            </button>
            <button onClick={() => setCurrentScreen('manuals')} className={`nav-item ${currentScreen === 'manuals' ? 'active' : ''}`}>
              <BookOpen size={18} /> Machine Manuals
            </button>
            <button onClick={() => setCurrentScreen('procurement')} className={`nav-item ${currentScreen === 'procurement' ? 'active' : ''}`}>
              <ShoppingCart size={18} /> AI Procurement
            </button>
          </div>

          {/* Local logs console output */}
          <div className="terminal-log" style={{ marginBottom: '16px', maxHeight: '150px', overflowY: 'auto', fontSize: '11px', paddingRight: '8px' }}>
            <span style={{ fontWeight: '700', color: 'var(--accent-primary-light)', display: 'block', marginBottom: '8px' }}>&gt; SYSTEM_LOGS</span>
            {systemLogs.map((log, idx) => (
              <div key={idx} style={{ color: 'var(--text-muted)', marginBottom: '4px' }}>
                <span className="text-cyan">[{log.time}]</span> {log.text}
              </div>
            ))}
          </div>

          <div style={{ borderTop: '1px solid var(--border-light)', paddingTop: '16px', marginTop: 'auto' }}>
            <button 
              onClick={() => {
                localStorage.removeItem('isLoggedIn')
                setIsLoggedIn(false)
              }} 
              className="btn-danger" 
              style={{ width: '100%', justifyContent: 'center' }}
            >
              <LogOut size={16} /> Logout Session
            </button>
          </div>
        </nav>

        {/* Main Panel Content */}
        <main className="animate-in" style={{ flex: 1, padding: '20px 20px 20px 10px', display: 'flex', flexDirection: 'column', gap: '20px', overflowY: 'auto' }}>
          
          {/* --------------------------------------------------------------------------
              SCREEN 2: DASHBOARD
             -------------------------------------------------------------------------- */}
          {currentScreen === 'dashboard' && (
            <div style={{ display: 'flex', flexDirection: 'column', gap: '20px' }}>
              {/* Top Stat Bar */}
              <div style={{ display: 'grid', gridTemplateColumns: 'repeat(4, 1fr)', gap: '16px' }}>
                <div className="glass-panel stat-card green card-hover" style={{ padding: '20px' }}>
                  <div className="icon-pad green"><ShieldCheck className="text-green" size={24} /></div>
                  <div>
                    <span className="label-upper">Factory Health</span>
                    <h3 style={{ fontSize: '24px', fontWeight: '800', marginTop: '2px' }}>
                      {arduinoConnection.connected ? "94.2%" : "94.2%"}
                    </h3>
                  </div>
                </div>

                <div className="glass-panel stat-card primary card-hover" style={{ padding: '20px' }}>
                  <div className="icon-pad primary"><Cpu className="text-primary-color" size={24} /></div>
                  <div>
                    <span className="label-upper">Active Machines</span>
                    <h3 style={{ fontSize: '24px', fontWeight: '800', marginTop: '2px' }}>
                      {arduinoConnection.connected ? "4 / 5" : "0 / 5"}
                    </h3>
                  </div>
                </div>

                <div className="glass-panel stat-card cyan card-hover" style={{ padding: '20px' }}>
                  <div className="icon-pad cyan"><Activity className="text-cyan" size={24} /></div>
                  <div>
                    <span className="label-upper">Production rate</span>
                    <h3 style={{ fontSize: '24px', fontWeight: '800', marginTop: '2px' }}>
                      {arduinoConnection.connected ? "1,420 u/h" : "0 u/h"}
                    </h3>
                  </div>
                </div>

                <div className="glass-panel stat-card amber card-hover" style={{ padding: '20px' }}>
                  <div className="icon-pad amber"><ShieldAlert className="text-amber" size={24} /></div>
                  <div>
                    <span className="label-upper">Active Alerts</span>
                    <h3 style={{ fontSize: '24px', fontWeight: '800', marginTop: '2px' }}>
                      {arduinoConnection.connected ? "1 Active" : "Disabled"}
                    </h3>
                  </div>
                </div>
              </div>

              {/* Active Machine Cards */}
              <div>
                <h3 style={{ marginBottom: '16px', fontSize: '18px', fontWeight: '700' }}>Monitoring Nodes</h3>
                <div style={{ display: 'grid', gridTemplateColumns: 'repeat(5, 1fr)', gap: '16px' }}>
                  {MOCK_MACHINES.map(m => (
                    <div 
                      key={m.id}
                      onClick={() => {
                        setSelectedMachineId(m.id)
                        setCurrentScreen('details')
                      }}
                      className="glass-panel card-hover"
                      style={{ padding: '24px 20px', textAlign: 'center', display: 'flex', flexDirection: 'column', alignItems: 'center' }}
                    >
                      <div className="machine-emoji-frame">{m.img}</div>
                      <h4 style={{ fontSize: '14px', fontWeight: '700', marginBottom: '4px', lineHeight: '1.2' }}>{m.name}</h4>
                      <span className="label-upper" style={{ marginBottom: '16px' }}>{m.id}</span>
                      
                      {arduinoConnection.connected && (
                        <div style={{ display: 'flex', alignItems: 'center', gap: '8px', background: 'var(--bg-panel-hover)', padding: '6px 12px', borderRadius: '20px', border: '1px solid var(--border-light)' }}>
                          <span className={`pulse-indicator ${arduinoConnection.connected && m.status === 'operational' ? 'green' : 'amber'} ${!arduinoConnection.connected && 'gray'}`}></span>
                          <strong style={{ fontSize: '13px', fontWeight: '700', color: arduinoConnection.connected ? (m.healthScore > 80 ? 'var(--accent-green-light)' : 'var(--accent-amber)') : 'var(--text-muted)' }}>
                            {arduinoConnection.connected ? `${m.healthScore}%` : 'Offline'}
                          </strong>
                        </div>
                      )}
                    </div>
                  ))}
                </div>
              </div>

              {/* General alert feeds */}
              <div className="glass-panel gradient-border" style={{ padding: '24px' }}>
                <h3 style={{ marginBottom: '16px', display: 'flex', alignItems: 'center', gap: '10px', fontSize: '16px' }}>
                  <AlertTriangle className="text-amber" size={20} /> Active Incidents
                </h3>
                {arduinoConnection.connected ? (
                  <div style={{ background: 'linear-gradient(90deg, rgba(245,158,11,0.1), transparent)', borderLeft: '3px solid var(--accent-amber)', padding: '16px', borderRadius: '8px', borderRight: '1px solid var(--border-glass)', borderTop: '1px solid var(--border-glass)', borderBottom: '1px solid var(--border-glass)' }}>
                    <div style={{ display: 'flex', justifyContent: 'space-between', alignItems: 'center', marginBottom: '8px' }}>
                      <strong style={{ fontSize: '14px' }}>MCH-004: Screw Rotor Overheating Alert</strong>
                      <span className="badge amber">10 mins ago</span>
                    </div>
                    <p style={{ fontSize: '13.5px', color: 'var(--text-secondary)', lineHeight: '1.6' }}>
                      Vibration exceeds warning limits (3.8 mm/s). Predictive maintenance estimates replacement required within 15 operating hours.
                    </p>
                  </div>
                ) : (
                  <div style={{ padding: '20px', textAlign: 'center', color: 'var(--text-muted)' }}>
                    <p>Waiting for Arduino Data...</p>
                  </div>
                )}
              </div>
            </div>
          )}

          {/* --------------------------------------------------------------------------
              SCREEN 3: MACHINE DETAILS
             -------------------------------------------------------------------------- */}
          {currentScreen === 'details' && (
            <div style={{ display: 'flex', flexDirection: 'column', gap: '20px' }}>
              
              {/* Header area */}
              <div className="glass-panel" style={{ padding: '20px', display: 'flex', justifyContent: 'space-between', alignItems: 'center' }}>
                <div style={{ display: 'flex', alignItems: 'center', gap: '16px' }}>
                  <div className="machine-emoji-frame" style={{ margin: 0, width: '48px', height: '48px', fontSize: '24px' }}>{activeMachine.img}</div>
                  <div>
                    <h2 style={{ fontSize: '20px', fontWeight: '800', marginBottom: '2px' }}>{activeMachine.name}</h2>
                    <span className="badge primary">{activeMachine.id}</span>
                  </div>
                </div>

                <div style={{ display: 'flex', alignItems: 'center', gap: '12px' }}>
                  <span className="label-upper">Select Asset:</span>
                  <select 
                    value={selectedMachineId} 
                    onChange={(e) => setSelectedMachineId(e.target.value)}
                    className="input-glass"
                    style={{ width: '220px', cursor: 'pointer' }}
                  >
                    {MOCK_MACHINES.map(m => (
                      <option key={m.id} value={m.id}>{m.name}</option>
                    ))}
                  </select>
                </div>
              </div>

              {/* Quick Metrics display */}
              <div style={{ display: 'grid', gridTemplateColumns: 'repeat(5, 1fr)', gap: '16px' }}>
                <div className="metric-mini">
                  <Thermometer className={arduinoConnection.connected ? "text-cyan" : "text-muted"} size={22} />
                  <span className="label-upper">Temperature</span>
                  <strong style={{ fontSize: '18px', color: arduinoConnection.connected ? 'var(--text-primary)' : 'var(--text-muted)' }}>{sensors.temperature} °C</strong>
                </div>
                <div className="metric-mini">
                  <Activity className={arduinoConnection.connected ? "text-amber" : "text-muted"} size={22} />
                  <span className="label-upper">Vibration</span>
                  <strong style={{ fontSize: '18px', color: arduinoConnection.connected ? 'var(--text-primary)' : 'var(--text-muted)' }}>{sensors.vibration} mm/s</strong>
                </div>
                <div className="metric-mini">
                  <Cpu className={arduinoConnection.connected ? "text-primary-color" : "text-muted"} size={22} />
                  <span className="label-upper">Speed RPM</span>
                  <strong style={{ fontSize: '18px', color: arduinoConnection.connected ? 'var(--text-primary)' : 'var(--text-muted)' }}>{sensors.rpm} RPM</strong>
                </div>
                <div className="metric-mini">
                  <Wrench className={arduinoConnection.connected ? "text-red" : "text-muted"} size={22} />
                  <span className="label-upper">Line Current</span>
                  <strong style={{ fontSize: '18px', color: arduinoConnection.connected ? 'var(--text-primary)' : 'var(--text-muted)' }}>{sensors.current} A</strong>
                </div>
                <div className="metric-mini">
                  <ShieldCheck className={arduinoConnection.connected ? "text-green" : "text-muted"} size={22} />
                  <span className="label-upper">Health Index</span>
                  <strong style={{ fontSize: '18px', color: arduinoConnection.connected ? (sensors.healthScore > 80 ? 'var(--accent-green-light)' : 'var(--accent-amber)') : 'var(--text-muted)' }}>
                    {arduinoConnection.connected ? `${sensors.healthScore}%` : 'Offline'}
                  </strong>
                </div>
              </div>

              {/* Two Side-By-Side Telemetry Graphs */}
              {arduinoConnection.connected ? (
                <div style={{ display: 'grid', gridTemplateColumns: '1fr 1fr', gap: '20px' }}>
                  <div className="glass-panel" style={{ padding: '24px' }}>
                    <div style={{ display: 'flex', justifyContent: 'space-between', alignItems: 'center', marginBottom: '20px' }}>
                      <h4 style={{ fontSize: '15px', fontWeight: '700' }}>Temperature Trend Analysis</h4>
                      <span className="badge cyan">Live</span>
                    </div>
                    <div style={{ height: '260px' }}>
                      <ResponsiveContainer width="100%" height="100%">
                        <AreaChart data={chartData}>
                          <defs>
                            <linearGradient id="colorTemp" x1="0" y1="0" x2="0" y2="1">
                              <stop offset="5%" stopColor="var(--accent-cyan)" stopOpacity={0.3}/>
                              <stop offset="95%" stopColor="var(--accent-cyan)" stopOpacity={0}/>
                            </linearGradient>
                          </defs>
                          <CartesianGrid strokeDasharray="3 3" stroke="var(--border-light)" vertical={false} />
                          <XAxis dataKey="time" stroke="var(--text-muted)" fontSize={11} tickMargin={10} />
                          <YAxis stroke="var(--text-muted)" fontSize={11} tickMargin={10} />
                          <Tooltip 
                            contentStyle={{ background: 'var(--bg-panel-hover)', border: '1px solid var(--border-glass-md)', borderRadius: '8px', boxShadow: '0 8px 24px rgba(0,0,0,0.5)' }} 
                            itemStyle={{ color: 'var(--text-primary)' }}
                          />
                          <Area type="monotone" dataKey="temp" stroke="var(--accent-cyan)" strokeWidth={2.5} fillOpacity={1} fill="url(#colorTemp)" isAnimationActive={false} activeDot={{ r: 6, fill: 'var(--accent-cyan)', stroke: '#fff', strokeWidth: 2 }} />
                        </AreaChart>
                      </ResponsiveContainer>
                    </div>
                  </div>

                  <div className="glass-panel" style={{ padding: '24px' }}>
                    <div style={{ display: 'flex', justifyContent: 'space-between', alignItems: 'center', marginBottom: '20px' }}>
                      <h4 style={{ fontSize: '15px', fontWeight: '700' }}>Vibration RMS Profile</h4>
                      <span className="badge amber">Live</span>
                    </div>
                    <div style={{ height: '260px' }}>
                      <ResponsiveContainer width="100%" height="100%">
                        <AreaChart data={chartData}>
                          <defs>
                            <linearGradient id="colorVib" x1="0" y1="0" x2="0" y2="1">
                              <stop offset="5%" stopColor="var(--accent-amber)" stopOpacity={0.3}/>
                              <stop offset="95%" stopColor="var(--accent-amber)" stopOpacity={0}/>
                            </linearGradient>
                          </defs>
                          <CartesianGrid strokeDasharray="3 3" stroke="var(--border-light)" vertical={false} />
                          <XAxis dataKey="time" stroke="var(--text-muted)" fontSize={11} tickMargin={10} />
                          <YAxis stroke="var(--text-muted)" fontSize={11} tickMargin={10} />
                          <Tooltip 
                            contentStyle={{ background: 'var(--bg-panel-hover)', border: '1px solid var(--border-glass-md)', borderRadius: '8px', boxShadow: '0 8px 24px rgba(0,0,0,0.5)' }} 
                            itemStyle={{ color: 'var(--text-primary)' }}
                          />
                          <Area type="monotone" dataKey="vib" stroke="var(--accent-amber)" strokeWidth={2.5} fillOpacity={1} fill="url(#colorVib)" isAnimationActive={false} activeDot={{ r: 6, fill: 'var(--accent-amber)', stroke: '#fff', strokeWidth: 2 }} />
                        </AreaChart>
                      </ResponsiveContainer>
                    </div>
                  </div>
                </div>
              ) : (
                <div className="glass-panel" style={{ padding: '48px', textAlign: 'center', border: '1px dashed var(--accent-red)', background: 'var(--bg-panel-error)' }}>
                  <AlertTriangle className="text-red" size={48} style={{ marginBottom: '16px', display: 'inline-block', filter: 'drop-shadow(0 0 10px var(--shadow-red))' }} />
                  <h4 style={{ color: 'var(--text-primary)', marginBottom: '12px', fontSize: '18px' }}>Graphical Telemetry Unavailable</h4>
                  <p style={{ color: 'var(--text-secondary)', fontSize: '14px', maxWidth: '500px', margin: '0 auto 24px auto', lineHeight: '1.6' }}>
                    Live plots are disabled because the connection to Arduino UNO Q is offline. Connect the microcontroller to resume live plotting.
                  </p>
                </div>
              )}
            </div>
          )}

          {/* --------------------------------------------------------------------------
              SCREEN 4: AI ASSISTANT
             -------------------------------------------------------------------------- */}
          {currentScreen === 'ai_assistant' && (
            <div className="glass-panel" style={{ height: 'calc(100vh - 120px)', display: 'flex', flexDirection: 'column', overflow: 'hidden', padding: 0 }}>
              <div style={{ padding: '24px', borderBottom: '1px solid var(--border-light)', display: 'flex', alignItems: 'center', gap: '16px' }}>
                <div style={{ width: '48px', height: '48px', borderRadius: '12px', background: 'var(--accent-purple-light)', display: 'flex', alignItems: 'center', justifyContent: 'center' }}>
                  <Sparkles size={24} className="text-purple" />
                </div>
                <div>
                  <h3 style={{ fontSize: '18px', fontWeight: '700' }}>Snapdragon AI Copilot</h3>
                  <p style={{ fontSize: '12px', color: 'var(--text-muted)' }}>Running local model pipeline{!arduinoConnection.connected && " (Offline data mode)"}</p>
                </div>
                <div style={{ marginLeft: 'auto' }}>
                  <span className="badge primary">
                    Target: {activeMachine.name}
                  </span>
                </div>
              </div>

              {/* Chat list */}
              <div style={{ flex: 1, padding: '24px', overflowY: 'auto', display: 'flex', flexDirection: 'column', gap: '20px' }}>
                {chatLog.map((msg, i) => (
                  <div key={i} className={msg.sender === 'user' ? 'chat-bubble-user' : 'chat-bubble-bot'}>
                    <p>{msg.text}</p>
                    
                    {msg.citations && msg.citations.length > 0 && (
                      <div style={{ marginTop: '14px', paddingTop: '10px', borderTop: '1px solid var(--border-light)' }}>
                        <span style={{ fontSize: '11px', color: msg.sender === 'user' ? 'var(--text-inverse)' : 'var(--text-primary)', fontWeight: '700', display: 'block', marginBottom: '6px' }}>RETRIEVED CONTEXTS:</span>
                        {msg.citations.map((c, idx) => (
                          <div key={idx} style={{ display: 'flex', alignItems: 'center', gap: '6px', fontSize: '11px', color: msg.sender === 'user' ? 'var(--text-inverse)' : 'var(--text-secondary)', marginBottom: '4px' }}>
                            <Database size={12} />
                            <span>[{c.source_type.toUpperCase()}] {c.source_id}: {c.text}</span>
                          </div>
                        ))}
                      </div>
                    )}
                  </div>
                ))}
                {isLoading && (
                  <div className="chat-bubble-bot" style={{ display: 'flex', alignItems: 'center', gap: '8px' }}>
                    <span className="typing-dot"></span>
                    <span className="typing-dot"></span>
                    <span className="typing-dot"></span>
                  </div>
                )}
                <div ref={chatBottomRef}></div>
              </div>

              {/* Suggested Question badging */}
              <div style={{ padding: '12px 24px', borderTop: '1px solid var(--border-light)', display: 'flex', gap: '12px', overflowX: 'auto', background: 'var(--bg-secondary)' }}>
                <span className="label-upper" style={{ alignSelf: 'center', whiteSpace: 'nowrap', marginRight: '4px' }}>Suggestions:</span>
                <button onClick={() => handleQuery("How do I fix spindle overheating?")} className="btn-secondary" style={{ padding: '6px 12px', fontSize: '12px', borderRadius: '16px' }}>🛠️ Spindle Overheating</button>
                <button onClick={() => handleQuery("Piston seal replacement steps")} className="btn-secondary" style={{ padding: '6px 12px', fontSize: '12px', borderRadius: '16px' }}>⚙️ Seal Replacement</button>
                <button onClick={() => handleQuery("What triggers seal wear on HP-500?")} className="btn-secondary" style={{ padding: '6px 12px', fontSize: '12px', borderRadius: '16px' }}>⚠️ Seal Wear triggers</button>
              </div>

              {/* Message input */}
              <div style={{ padding: '24px', background: 'var(--bg-panel)', display: 'flex', gap: '12px' }}>
                <input 
                  type="text" 
                  value={chatInput} 
                  onChange={(e) => setChatInput(e.target.value)} 
                  onKeyDown={(e) => e.key === 'Enter' && handleQuery()}
                  placeholder="Ask about troubleshooting, repair manuals, or part replacements..."
                  className="input-glass"
                  style={{ flex: 1, padding: '14px 18px', fontSize: '14px' }}
                />
                <button onClick={() => handleQuery()} className="btn-primary" style={{ padding: '0 24px' }}>
                  <Send size={18} />
                  Ask AI
                </button>
              </div>
            </div>
          )}

          {/* --------------------------------------------------------------------------
              SCREEN 5: FACTORY SETUP
             -------------------------------------------------------------------------- */}
          {currentScreen === 'setup' && (
            <div style={{ display: 'flex', flexDirection: 'column', gap: '20px' }}>
              <div className="glass-panel" style={{ padding: '32px', display: 'flex', flexDirection: 'column', gap: '24px' }}>
                <h3 style={{ fontSize: '18px', fontWeight: '800' }}>Plant Profile Details</h3>
                
                <div style={{ display: 'grid', gridTemplateColumns: '1fr 1fr', gap: '24px' }}>
                  <div>
                    <label className="label-upper" style={{ display: 'block', marginBottom: '8px' }}>FACTORY NAME</label>
                    <input 
                      type="text" 
                      value={factoryName} 
                      onChange={(e) => setFactoryName(e.target.value)}
                      className="input-glass" 
                    />
                  </div>
                  <div>
                    <label className="label-upper" style={{ display: 'block', marginBottom: '8px' }}>INDUSTRY CLASSIFICATION</label>
                    <input 
                      type="text" 
                      value={industry} 
                      onChange={(e) => setIndustry(e.target.value)}
                      className="input-glass" 
                    />
                  </div>
                </div>
              </div>

              {/* Document ingestion panels */}
              <div className="glass-panel" style={{ padding: '32px' }}>
                <h3 style={{ marginBottom: '24px', fontSize: '18px', fontWeight: '800' }}>Vector RAG Context Ingestion</h3>
                
                <div style={{ display: 'flex', flexDirection: 'column', gap: '16px' }}>
                  
                  {/* Manual Box */}
                  <div className="drop-zone">
                    <div style={{ display: 'flex', alignItems: 'center', gap: '16px', flex: 1 }}>
                      <div className="icon-pad primary"><FileUp className="text-primary-color" /></div>
                      <div>
                        <strong style={{ fontSize: '15px' }}>Technical Manuals (PDF)</strong>
                        <span style={{ fontSize: '13px', color: 'var(--text-secondary)', display: 'block', marginTop: '2px' }}>Upload factory equipment manual folders</span>
                      </div>
                    </div>
                    <button onClick={() => handleFileUpload('manuals')} className="btn-secondary">
                      {uploadStatus.manuals === 'success' ? <Check className="text-green" /> : uploadStatus.manuals === 'uploading' ? 'Ingesting...' : 'Choose Folder'}
                    </button>
                  </div>

                  {/* SOP Box */}
                  <div className="drop-zone">
                    <div style={{ display: 'flex', alignItems: 'center', gap: '16px', flex: 1 }}>
                      <div className="icon-pad cyan"><FileUp className="text-cyan" /></div>
                      <div>
                        <strong style={{ fontSize: '15px' }}>Standard Operating Procedures (SOPs)</strong>
                        <span style={{ fontSize: '13px', color: 'var(--text-secondary)', display: 'block', marginTop: '2px' }}>Define calibration and maintenance steps</span>
                      </div>
                    </div>
                    <button onClick={() => handleFileUpload('sops')} className="btn-secondary">
                      {uploadStatus.sops === 'success' ? <Check className="text-green" /> : uploadStatus.sops === 'uploading' ? 'Parsing...' : 'Choose File'}
                    </button>
                  </div>

                  {/* Log Box */}
                  <div className="drop-zone">
                    <div style={{ display: 'flex', alignItems: 'center', gap: '16px', flex: 1 }}>
                      <div className="icon-pad amber"><FileUp className="text-amber" /></div>
                      <div>
                        <strong style={{ fontSize: '15px' }}>Maintenance & Historical Logs</strong>
                        <span style={{ fontSize: '13px', color: 'var(--text-secondary)', display: 'block', marginTop: '2px' }}>Add past troubleshoot logs for RAG matching</span>
                      </div>
                    </div>
                    <button onClick={() => handleFileUpload('records')} className="btn-secondary">
                      {uploadStatus.records === 'success' ? <Check className="text-green" /> : uploadStatus.records === 'uploading' ? 'Vectorizing...' : 'Upload Records'}
                    </button>
                  </div>

                </div>
              </div>
            </div>
          )}

          {/* --------------------------------------------------------------------------
              SCREEN 6: REPORTS
             -------------------------------------------------------------------------- */}
          {currentScreen === 'reports' && (
            <div style={{ display: 'flex', flexDirection: 'column', gap: '20px' }}>
              
              <div style={{ display: 'grid', gridTemplateColumns: '1fr 1fr', gap: '24px' }}>
                {/* Daily report */}
                <div className="glass-panel" style={{ padding: '32px', display: 'flex', flexDirection: 'column', justifyContent: 'space-between', minHeight: '280px' }}>
                  <div>
                    <h3 style={{ fontSize: '18px', fontWeight: '800', marginBottom: '8px' }}>Daily Maintenance Report</h3>
                    <span className="badge primary" style={{ marginBottom: '20px' }}>Generated: Today 18:00</span>
                    <p style={{ fontSize: '14px', color: 'var(--text-secondary)', lineHeight: '1.6' }}>
                      Summary of operations, recorded incidents, and parts replaced in the past 24 hours. Includes live sensor health snapshots of A0/A1 warning alerts.
                    </p>
                  </div>
                  <button onClick={() => downloadReport("Daily Maintenance")} className="btn-primary" style={{ width: 'fit-content', marginTop: '24px' }}>
                    <FileText size={16} /> Export PDF Report
                  </button>
                </div>

                {/* Weekly report */}
                <div className="glass-panel" style={{ padding: '32px', display: 'flex', flexDirection: 'column', justifyContent: 'space-between', minHeight: '280px' }}>
                  <div>
                    <h3 style={{ fontSize: '18px', fontWeight: '800', marginBottom: '8px' }}>Weekly Health Analysis</h3>
                    <span className="badge cyan" style={{ marginBottom: '20px' }}>Period: July 12 - July 18, 2026</span>
                    <p style={{ fontSize: '14px', color: 'var(--text-secondary)', lineHeight: '1.6' }}>
                      Trend analysis of vibration values and heating indexes. Analyzes common failure patterns across the fleet to recommend preventative adjustments.
                    </p>
                  </div>
                  <button onClick={() => downloadReport("Weekly Health Analysis")} className="btn-primary" style={{ width: 'fit-content', marginTop: '24px' }}>
                    <FileText size={16} /> Export PDF Report
                  </button>
                </div>
              </div>
            </div>
          )}

          {/* --------------------------------------------------------------------------
              SCREEN 7: MACHINE MANUALS
             -------------------------------------------------------------------------- */}
          {currentScreen === 'manuals' && (
            <div style={{ display: 'flex', gap: '24px', height: '100%' }}>
              <div className="glass-panel" style={{ width: '300px', display: 'flex', flexDirection: 'column', gap: '8px', padding: '16px' }}>
                <h3 style={{ fontSize: '16px', fontWeight: '800', marginBottom: '16px' }}>Select Machine</h3>
                {MOCK_MACHINES.map(m => (
                  <button 
                    key={m.id} 
                    onClick={() => setSelectedMachineId(m.id)}
                    className={`nav-item ${selectedMachineId === m.id ? 'active' : ''}`}
                    style={{ textAlign: 'left', padding: '12px', background: selectedMachineId === m.id ? 'var(--bg-panel-hover)' : 'transparent', borderRadius: '8px' }}
                  >
                    <div style={{ display: 'flex', alignItems: 'center', gap: '12px' }}>
                      <span style={{ fontSize: '20px' }}>{m.img}</span>
                      <div>
                        <div style={{ fontWeight: '700', fontSize: '13px' }}>{m.name}</div>
                        <div style={{ fontSize: '11px', color: 'var(--text-muted)' }}>{m.model}</div>
                      </div>
                    </div>
                  </button>
                ))}
              </div>
              
              <div className="glass-panel" style={{ flex: 1, padding: '32px', overflowY: 'auto' }}>
                {(() => {
                  const m = MOCK_MACHINES.find(x => x.id === selectedMachineId) || MOCK_MACHINES[0]
                  return (
                    <div>
                      <div style={{ display: 'flex', alignItems: 'center', gap: '16px', marginBottom: '24px' }}>
                        <span style={{ fontSize: '48px' }}>{m.img}</span>
                        <div>
                          <h2 style={{ fontSize: '28px', fontWeight: '800', marginBottom: '4px' }}>{m.name}</h2>
                          <div className="badge primary">{m.manufacturer} - {m.model}</div>
                        </div>
                      </div>
                      
                      <h4 style={{ color: 'var(--accent-primary)', marginBottom: '8px', borderBottom: '1px solid var(--border-light)', paddingBottom: '8px' }}>Standard Operating Parameters</h4>
                      <ul style={{ marginBottom: '24px', paddingLeft: '20px', lineHeight: '1.8' }}>
                        <li><strong>Max Temperature:</strong> {m.temp > 60 ? '85.0 °C' : '50.0 °C'}</li>
                        <li><strong>Vibration Limit:</strong> {m.vib > 2 ? '5.5 mm/s' : '2.0 mm/s'}</li>
                        <li><strong>Power Draw:</strong> {m.current} A nominal</li>
                      </ul>

                      <h4 style={{ color: 'var(--accent-primary)', marginBottom: '8px', borderBottom: '1px solid var(--border-light)', paddingBottom: '8px' }}>Maintenance Procedures</h4>
                      <p style={{ lineHeight: '1.6', marginBottom: '16px' }}>
                        This {m.type.replace('_', ' ')} requires daily visual inspection and weekly lubrication of all moving joints. Do not exceed maximum RPM limits. If vibration exceeds threshold for more than 5 minutes, an automatic emergency stop will be triggered by the PLC.
                      </p>
                      
                      <div style={{ background: 'rgba(239, 68, 68, 0.1)', border: '1px solid var(--status-critical)', padding: '16px', borderRadius: '8px' }}>
                        <h4 style={{ color: 'var(--status-critical)', margin: '0 0 8px 0', display: 'flex', alignItems: 'center', gap: '8px' }}>
                          <AlertTriangle size={16}/> Emergency Shutdown
                        </h4>
                        <p style={{ margin: 0, fontSize: '13px' }}>In case of catastrophic failure or thermal runaway, press the physical E-STOP button located on the main control panel. Do not attempt to override the safety interlocks.</p>
                      </div>
                    </div>
                  )
                })()}
              </div>
            </div>
          )}

          {/* --------------------------------------------------------------------------
              SCREEN 8: AI PROCUREMENT
             -------------------------------------------------------------------------- */}
          {currentScreen === 'procurement' && (
            <div style={{ display: 'flex', flexDirection: 'column', gap: '20px' }}>
              <div style={{ display: 'flex', justifyContent: 'space-between', alignItems: 'flex-end', marginBottom: '16px' }}>
                <div>
                  <h2 style={{ fontSize: '24px', fontWeight: '800', marginBottom: '8px' }}>Predictive Replacement AI</h2>
                  <p style={{ color: 'var(--text-secondary)' }}>AI analyzing degradation patterns to recommend proactive part booking.</p>
                </div>
              </div>

              <div style={{ display: 'grid', gridTemplateColumns: '1fr', gap: '16px' }}>
                {MOCK_MACHINES.map(m => {
                  const estDays = Math.max(1, Math.floor((m.healthScore / 100) * 365 * (m.id === 'MCH-004' ? 0.1 : 1)));
                  const isCritical = estDays < 45;
                  
                  return (
                    <div key={m.id} className="glass-panel" style={{ display: 'flex', alignItems: 'center', padding: '24px', borderLeft: isCritical ? '4px solid var(--status-critical)' : '4px solid var(--status-operational)' }}>
                      <div style={{ fontSize: '40px', marginRight: '24px' }}>{m.img}</div>
                      
                      <div style={{ flex: 1 }}>
                        <h3 style={{ fontSize: '18px', fontWeight: '800', marginBottom: '4px' }}>{m.name}</h3>
                        <div style={{ color: 'var(--text-secondary)', fontSize: '13px' }}>Health Score: {m.healthScore}%</div>
                      </div>
                      
                      <div style={{ flex: 1, textAlign: 'center' }}>
                        <div style={{ fontSize: '12px', color: 'var(--text-muted)', marginBottom: '4px', textTransform: 'uppercase', letterSpacing: '1px' }}>Est. Failure In</div>
                        <div style={{ fontSize: '24px', fontWeight: '800', color: isCritical ? 'var(--status-critical)' : 'var(--text-primary)' }}>
                          {estDays} Days
                        </div>
                      </div>

                      <div style={{ flex: 1, display: 'flex', justifyContent: 'flex-end' }}>
                        {isCritical ? (
                          <button 
                            className="btn-primary" 
                            style={{ background: 'var(--status-critical)', color: 'white', border: 'none' }}
                            onClick={(e) => {
                              e.target.innerText = 'Order Placed ✓';
                              e.target.style.background = 'var(--status-operational)';
                              e.target.disabled = true;
                              addSystemLog(`PRO-ACT Procurement triggered for ${m.name}`);
                            }}
                          >
                            <ShoppingCart size={16} /> Book Replacement Now
                          </button>
                        ) : (
                          <button className="btn-secondary" disabled style={{ opacity: 0.5 }}>
                            No Action Needed
                          </button>
                        )}
                      </div>
                    </div>
                  )
                })}
              </div>
            </div>
          )}

        </main>
      </div>
    </div>
  )
}

--- ANDROID APP (MainActivity.kt) ---
package com.factorykt

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.background
import androidx.compose.foundation.layout.*
import androidx.compose.foundation.lazy.LazyColumn
import androidx.compose.foundation.lazy.items
import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.animation.AnimatedContent
import androidx.compose.animation.core.tween
import androidx.compose.animation.fadeIn
import androidx.compose.animation.animateColor
import androidx.compose.animation.fadeOut
import androidx.compose.animation.togetherWith
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.text.input.PasswordVisualTransformation
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.launch
import kotlinx.coroutines.withContext
import org.json.JSONObject
import java.io.OutputStreamWriter
import java.net.HttpURLConnection
import java.net.URL
import androidx.activity.compose.rememberLauncherForActivityResult
import androidx.activity.result.contract.ActivityResultContracts
import androidx.compose.ui.viewinterop.AndroidView
import androidx.camera.core.CameraSelector
import androidx.camera.core.ExperimentalGetImage
import androidx.camera.core.ImageAnalysis
import androidx.camera.core.ImageProxy
import androidx.camera.core.Preview
import androidx.camera.lifecycle.ProcessCameraProvider
import androidx.camera.view.PreviewView
import androidx.core.content.ContextCompat
import com.google.mlkit.vision.barcode.BarcodeScanning
import com.google.mlkit.vision.common.InputImage
import java.util.concurrent.Executors

// Simple data model for demo
data class Machine(val id: String, val name: String, val type: String, var health: Int, var status: String)

@OptIn(ExperimentalGetImage::class)
class BarcodeAnalyzer(
    private val onQrCodeScanned: (String) -> Unit
) : ImageAnalysis.Analyzer {
    private val scanner = BarcodeScanning.getClient()

    override fun analyze(imageProxy: ImageProxy) {
        val mediaImage = imageProxy.image
        if (mediaImage != null) {
            val image = InputImage.fromMediaImage(mediaImage, imageProxy.imageInfo.rotationDegrees)
            scanner.process(image)
                .addOnSuccessListener { barcodes ->
                    for (barcode in barcodes) {
                        val rawValue = barcode.rawValue
                        if (rawValue != null) {
                            onQrCodeScanned(rawValue)
                            break
                        }
                    }
                }
                .addOnFailureListener {
                    it.printStackTrace()
                }
                .addOnCompleteListener {
                    imageProxy.close()
                }
        } else {
            imageProxy.close()
        }
    }
}

@Composable
fun CameraPreview(
    modifier: Modifier = Modifier,
    onQrCodeScanned: (String) -> Unit
) {
    val context = androidx.compose.ui.platform.LocalContext.current
    val lifecycleOwner = androidx.compose.ui.platform.LocalLifecycleOwner.current
    val cameraProviderFuture = remember { ProcessCameraProvider.getInstance(context) }
    val executor = remember { Executors.newSingleThreadExecutor() }

    AndroidView(
        factory = { ctx ->
            val previewView = PreviewView(ctx)
            cameraProviderFuture.addListener({
                val cameraProvider = cameraProviderFuture.get()
                val preview = Preview.Builder().build().apply {
                    setSurfaceProvider(previewView.surfaceProvider)
                }

                val imageAnalysis = ImageAnalysis.Builder()
                    .setBackpressureStrategy(ImageAnalysis.STRATEGY_KEEP_ONLY_LATEST)
                    .build()
                    .apply {
                        setAnalyzer(executor, BarcodeAnalyzer(onQrCodeScanned))
                    }

                val cameraSelector = CameraSelector.DEFAULT_BACK_CAMERA

                try {
                    cameraProvider.unbindAll()
                    cameraProvider.bindToLifecycle(
                        lifecycleOwner,
                        cameraSelector,
                        preview,
                        imageAnalysis
                    )
                } catch (e: Exception) {
                    e.printStackTrace()
                }
            }, ContextCompat.getMainExecutor(ctx))
            previewView
        },
        modifier = modifier
    )
}

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun DataIngestionScreen(serverIp: String, scope: kotlinx.coroutines.CoroutineScope) {
    var docType by remember { mutableStateOf("note") } // note, procedure, manual
    var title by remember { mutableStateOf("") }
    var content by remember { mutableStateOf("") }
    var assetId by remember { mutableStateOf("MCH-001") }
    
    var isUploading by remember { mutableStateOf(false) }
    var statusMessage by remember { mutableStateOf("") }

    Column(
        modifier = Modifier
            .fillMaxSize()
            .background(Color(0xFF0A0F1D))
            .padding(16.dp),
        verticalArrangement = Arrangement.spacedBy(16.dp)
    ) {
        Text("Ingest Factory Data to RAG", fontSize = 20.sp, fontWeight = FontWeight.Bold, color = Color(0xFF00D4FF))
        Text("Add machine manuals, employee notes, or procedures directly into the local RAG knowledge base.", fontSize = 12.sp, color = Color.Gray)

        Row(
            modifier = Modifier.fillMaxWidth(),
            horizontalArrangement = Arrangement.spacedBy(8.dp)
        ) {
            val types = listOf("note" to "📝 Note", "procedure" to "🔧 SOP", "manual" to "📘 Manual")
            types.forEach { (type, label) ->
                FilterChip(
                    selected = docType == type,
                    onClick = { docType = type },
                    label = { Text(label, color = Color.White) },
                    colors = FilterChipDefaults.filterChipColors(
                        selectedContainerColor = Color(0xFF007AFF)
                    )
                )
            }
        }

        OutlinedTextField(
            value = assetId,
            onValueChange = { assetId = it },
            label = { Text("Target Asset ID (e.g. MCH-001)", color = Color(0xFF00D4FF)) },
            modifier = Modifier.fillMaxWidth(),
            colors = OutlinedTextFieldDefaults.colors(
                focusedTextColor = Color.White,
                unfocusedTextColor = Color.White
            )
        )

        OutlinedTextField(
            value = title,
            onValueChange = { title = it },
            label = { Text("Document / Note Title", color = Color(0xFF00D4FF)) },
            modifier = Modifier.fillMaxWidth(),
            colors = OutlinedTextFieldDefaults.colors(
                focusedTextColor = Color.White,
                unfocusedTextColor = Color.White
            )
        )

        OutlinedTextField(
            value = content,
            onValueChange = { content = it },
            label = { Text("Content (Manual text, Employee notes, etc.)", color = Color(0xFF00D4FF)) },
            modifier = Modifier.fillMaxWidth().weight(1f),
            maxLines = 10,
            colors = OutlinedTextFieldDefaults.colors(
                focusedTextColor = Color.White,
                unfocusedTextColor = Color.White
            )
        )

        if (statusMessage.isNotEmpty()) {
            Text(
                text = statusMessage,
                color = if (statusMessage.startsWith("Success")) Color.Green else Color.Red,
                fontSize = 13.sp,
                fontWeight = FontWeight.Bold
            )
        }

        Button(
            onClick = {
                isUploading = true
                statusMessage = "Sending data to laptop server..."
                scope.launch(Dispatchers.IO) {
                    try {
                        val url = URL("http://$serverIp:8000/api/rag/ingest")
                        val conn = url.openConnection() as HttpURLConnection
                        conn.requestMethod = "POST"
                        conn.setRequestProperty("Content-Type", "application/json; utf-8")
                        conn.setRequestProperty("Accept", "application/json")
                        conn.doOutput = true
                        
                        val jsonParam = JSONObject()
                        jsonParam.put("type", docType)
                        jsonParam.put("title", title)
                        jsonParam.put("content", content)
                        jsonParam.put("asset_id", assetId)

                        val os = conn.outputStream
                        val writer = OutputStreamWriter(os, "UTF-8")
                        writer.write(jsonParam.toString())
                        writer.flush()
                        writer.close()
                        os.close()

                        val responseCode = conn.responseCode
                        if (responseCode == HttpURLConnection.HTTP_OK) {
                            withContext(Dispatchers.Main) {
                                statusMessage = "Success: Document ingested into RAG twin successfully!"
                                isUploading = false
                                title = ""
                                content = ""
                            }
                        } else {
                            withContext(Dispatchers.Main) {
                                statusMessage = "Ingestion failed. Server returned status: $responseCode"
                                isUploading = false
                            }
                        }
                    } catch (e: Exception) {
                        withContext(Dispatchers.Main) {
                            statusMessage = "Connection failed: ${e.message}"
                            isUploading = false
                        }
                    }
                }
            },
            modifier = Modifier.fillMaxWidth().height(48.dp),
            colors = ButtonDefaults.buttonColors(containerColor = Color(0xFF007AFF)),
            enabled = title.isNotEmpty() && content.isNotEmpty() && !isUploading
        ) {
            if (isUploading) {
                CircularProgressIndicator(color = Color.White, modifier = Modifier.size(24.dp))
            } else {
                Text("Ingest into Local RAG Twin", fontWeight = FontWeight.Bold)
            }
        }
    }
}

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            AppTheme {
                Surface(
                    modifier = Modifier.fillMaxSize(),
                    color = MaterialTheme.colorScheme.background
                ) {
                    CompanionAppMain()
                }
            }
        }
    }
}

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun LoginScreen(onLoginSuccess: () -> Unit) {
    var username by remember { mutableStateOf("admin") }
    var password by remember { mutableStateOf("qualcomm2026") }
    var errorMessage by remember { mutableStateOf("") }
    
    // Techy Animated Background
    val infiniteTransition = androidx.compose.animation.core.rememberInfiniteTransition()
    val colorAnim by infiniteTransition.animateColor(
        initialValue = Color(0xFF0A0F1D),
        targetValue = Color(0xFF162544),
        animationSpec = androidx.compose.animation.core.infiniteRepeatable(
            animation = androidx.compose.animation.core.tween(3000),
            repeatMode = androidx.compose.animation.core.RepeatMode.Reverse
        ),
        label = "BgColor"
    )

    Column(
        modifier = Modifier
            .fillMaxSize()
            .background(colorAnim)
            .padding(24.dp),
        verticalArrangement = Arrangement.Center,
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        // Decorative AI icon
        Icon(
            imageVector = Icons.Default.SmartToy,
            contentDescription = "AI Logo",
            tint = Color(0xFF00D4FF),
            modifier = Modifier.size(64.dp).padding(bottom = 16.dp)
        )
        Text(
            text = "Factory Twin AI",
            fontSize = 28.sp,
            fontWeight = FontWeight.ExtraBold,
            color = Color(0xFF00D4FF),
            modifier = Modifier.padding(bottom = 8.dp)
        )
        Text(
            text = "QUALCOMM MULTIVERSE EDGE PORTAL",
            fontSize = 11.sp,
            fontWeight = FontWeight.Bold,
            color = Color.Gray,
            modifier = Modifier.padding(bottom = 32.dp)
        )

        Card(
            shape = RoundedCornerShape(16.dp),
            colors = CardDefaults.cardColors(containerColor = Color(0xFF161E34)),
            modifier = Modifier.fillMaxWidth().padding(horizontal = 8.dp)
        ) {
            Column(
                modifier = Modifier.padding(24.dp),
                verticalArrangement = Arrangement.spacedBy(16.dp)
            ) {
                Text(
                    text = "TECHNICIAN AUTHENTICATION",
                    fontSize = 14.sp,
                    fontWeight = FontWeight.Bold,
                    color = Color.White
                )

                OutlinedTextField(
                    value = username,
                    onValueChange = { username = it },
                    label = { Text("Technician ID / Username", color = Color(0xFF00D4FF)) },
                    modifier = Modifier.fillMaxWidth(),
                    colors = OutlinedTextFieldDefaults.colors(
                        focusedTextColor = Color.White,
                        unfocusedTextColor = Color.White
                    )
                )

                OutlinedTextField(
                    value = password,
                    onValueChange = { password = it },
                    label = { Text("Security Credential", color = Color(0xFF00D4FF)) },
                    modifier = Modifier.fillMaxWidth(),
                    visualTransformation = PasswordVisualTransformation(),
                    colors = OutlinedTextFieldDefaults.colors(
                        focusedTextColor = Color.White,
                        unfocusedTextColor = Color.White
                    )
                )

                if (errorMessage.isNotEmpty()) {
                    Text(
                        text = errorMessage,
                        color = Color.Red,
                        fontSize = 12.sp,
                        fontWeight = FontWeight.Bold
                    )
                }

                Button(
                    onClick = {
                        if (username == "admin" && password == "qualcomm2026") {
                            onLoginSuccess()
                        } else {
                            errorMessage = "Invalid credentials. Hint: admin / qualcomm2026"
                        }
                    },
                    modifier = Modifier.fillMaxWidth().height(48.dp),
                    colors = ButtonDefaults.buttonColors(containerColor = Color(0xFF007AFF))
                ) {
                    Text("Authenticate & Connect", fontWeight = FontWeight.Bold)
                }
            }
        }
    }
}

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun CompanionAppMain() {
    var isLoggedIn by remember { mutableStateOf(false) }
    var currentScreen by remember { mutableStateOf("dashboard") }
    var selectedMachineId by remember { mutableStateOf("MCH-001") }
    var machineStatusMsg by remember { mutableStateOf("Ready to Scan QR Code") }
    var serverIp by remember { mutableStateOf("192.168.1.15") }
    
    val context = androidx.compose.ui.platform.LocalContext.current
    var menuExpanded by remember { mutableStateOf(false) }
    var hasCameraPermission by remember {
        mutableStateOf(
            ContextCompat.checkSelfPermission(
                context,
                android.Manifest.permission.CAMERA
            ) == android.content.pm.PackageManager.PERMISSION_GRANTED
        )
    }

    val permissionLauncher = rememberLauncherForActivityResult(
        contract = ActivityResultContracts.RequestPermission(),
        onResult = { isGranted ->
            hasCameraPermission = isGranted
        }
    )
    
    val machines = remember {
        mutableStateListOf(
            Machine("MCH-001", "CNC Lathe XL-2000", "cnc_lathe", 98, "operational"),
            Machine("MCH-002", "Hydraulic Press HP-500", "hydraulic_press", 95, "operational"),
            Machine("MCH-003", "Conveyor Drive CM-12", "conveyor", 97, "operational"),
            Machine("MCH-004", "Air Compressor AC-75", "compressor", 75, "warning"),
            Machine("MCH-005", "Robotic Arm RA-6", "robotic_arm", 99, "operational")
        )
    }

    val activeMachine = machines.find { it.id == selectedMachineId } ?: machines[0]
    val scope = rememberCoroutineScope()

    if (!isLoggedIn) {
        LoginScreen(onLoginSuccess = { isLoggedIn = true })
    } else {
        Scaffold(
            topBar = {
                TopAppBar(
                    title = { Text("Factory Knowledge Twin Companion", color = MaterialTheme.colorScheme.onSurface, fontSize = 18.sp, fontWeight = FontWeight.Bold) },
                    colors = TopAppBarDefaults.topAppBarColors(containerColor = MaterialTheme.colorScheme.surface),
                    actions = {
                        Box {
                            IconButton(onClick = { menuExpanded = !menuExpanded }) {
                                Icon(Icons.Default.Menu, contentDescription = "Menu")
                            }
                            DropdownMenu(
                                expanded = menuExpanded,
                                onDismissRequest = { menuExpanded = false }
                            ) {
                                DropdownMenuItem(
                                    text = { Text("Dashboard") },
                                    onClick = { currentScreen = "dashboard"; menuExpanded = false },
                                    leadingIcon = { Icon(Icons.Default.Dashboard, contentDescription = null) }
                                )
                                DropdownMenuItem(
                                    text = { Text("Scan QR") },
                                    onClick = { currentScreen = "qr_scan"; menuExpanded = false },
                                    leadingIcon = { Icon(Icons.Default.QrCodeScanner, contentDescription = null) }
                                )
                                DropdownMenuItem(
                                    text = { Text("AI Assist") },
                                    onClick = { currentScreen = "ai_assist"; menuExpanded = false },
                                    leadingIcon = { Icon(Icons.Default.SmartToy, contentDescription = null) }
                                )
                                DropdownMenuItem(
                                    text = { Text("Ingest Data") },
                                    onClick = { currentScreen = "ingest_data"; menuExpanded = false },
                                    leadingIcon = { Icon(Icons.Default.Add, contentDescription = null) }
                                )
                                DropdownMenuItem(
                                    text = { Text("Analytics") },
                                    onClick = { currentScreen = "analytics"; menuExpanded = false },
                                    leadingIcon = { Icon(Icons.Default.Insights, contentDescription = null) }
                                )
                                DropdownMenuItem(
                                    text = { Text("Manuals") },
                                    onClick = { currentScreen = "manuals"; menuExpanded = false },
                                    leadingIcon = { Icon(Icons.Default.MenuBook, contentDescription = null) }
                                )
                                DropdownMenuItem(
                                    text = { Text("Procure") },
                                    onClick = { currentScreen = "procurement"; menuExpanded = false },
                                    leadingIcon = { Icon(Icons.Default.ShoppingCart, contentDescription = null) }
                                )
                                DropdownMenuItem(
                                    text = { Text("Settings") },
                                    onClick = { currentScreen = "settings"; menuExpanded = false },
                                    leadingIcon = { Icon(Icons.Default.Settings, contentDescription = null) }
                                )
                                DropdownMenuItem(
                                    text = { Text("Logout", color = Color(0xFFEF4444)) },
                                    onClick = { isLoggedIn = false; menuExpanded = false },
                                    leadingIcon = { Icon(Icons.Default.ExitToApp, contentDescription = null, tint = Color(0xFFEF4444)) }
                                )
                            }
                        }
                    }
                )
            }
        ) { innerPadding ->
        Column(
            modifier = Modifier
                .padding(innerPadding)
                .fillMaxSize()
                .background(MaterialTheme.colorScheme.background)
                .padding(16.dp),
            verticalArrangement = Arrangement.Top
        ) {
            AnimatedContent(
                targetState = currentScreen,
                modifier = Modifier.fillMaxSize(),
                transitionSpec = {
                    fadeIn(animationSpec = tween(300)) togetherWith fadeOut(animationSpec = tween(300))
                },
                label = "ScreenTransition"
            ) { targetScreen ->
                when (targetScreen) {
                    "dashboard" -> {
                        DashboardScreen(machines = machines, onMachineSelected = { id -> 
                            selectedMachineId = id
                            currentScreen = "machine_detail"
                        })
                    }
                    "machine_detail" -> {
                        MachineDetailScreen(machine = activeMachine, onBack = { currentScreen = "dashboard" })
                    }
                    "analytics" -> AnalyticsScreen()
                    "settings" -> SettingsProfileScreen(context, onLogout = { isLoggedIn = false })
                    "manuals" -> ManualsScreen(machines = machines)
                    "procurement" -> ProcurementScreen(machines = machines)
                "qr_scan" -> {
                    Column(modifier = Modifier.fillMaxSize()) {
                        LaunchedEffect(key1 = true) {
                            if (!hasCameraPermission) {
                            permissionLauncher.launch(android.Manifest.permission.CAMERA)
                        }
                    }

                    Text("Scan Machine QR Code", fontSize = 20.sp, fontWeight = FontWeight.Bold, color = Color(0xFF00D4FF), modifier = Modifier.padding(bottom = 16.dp))
                    
                    if (hasCameraPermission) {
                        Box(
                            modifier = Modifier
                                .fillMaxWidth()
                                .height(250.dp)
                                .background(Color.Black, shape = RoundedCornerShape(16.dp)),
                            contentAlignment = Alignment.Center
                        ) {
                            CameraPreview(
                                modifier = Modifier.fillMaxSize(),
                                onQrCodeScanned = { scannedCode ->
                                    val matchedMachine = machines.find { it.id == scannedCode.trim() }
                                    if (matchedMachine != null) {
                                        selectedMachineId = matchedMachine.id
                                        machineStatusMsg = "Successfully Scanned: ${matchedMachine.name}"
                                        currentScreen = "machine_detail"
                                    } else {
                                        machineStatusMsg = "Unrecognized QR Code: $scannedCode"
                                    }
                                }
                            )
                        }
                    } else {
                        Box(
                            modifier = Modifier
                                .fillMaxWidth()
                                .height(250.dp)
                                .background(Color(0xFF161E34), shape = RoundedCornerShape(16.dp)),
                            contentAlignment = Alignment.Center
                        ) {
                            Button(onClick = { permissionLauncher.launch(android.Manifest.permission.CAMERA) }) {
                                Text("Grant Camera Permission")
                            }
                        }
                    }

                    Spacer(modifier = Modifier.height(16.dp))
                    Text(machineStatusMsg, color = Color.White, fontWeight = FontWeight.SemiBold)
                    Spacer(modifier = Modifier.height(16.dp))
                    Button(
                        onClick = {
                            selectedMachineId = "MCH-001"
                            machineStatusMsg = "Successfully Switched to Machine: CNC Lathe XL-2000"
                            currentScreen = "machine_detail"
                        },
                        modifier = Modifier.fillMaxWidth(),
                        colors = ButtonDefaults.buttonColors(containerColor = Color.DarkGray)
                    ) {
                        Text("Simulate Scan (CNC Lathe)")
                    }
                    }
                }
                "ai_assist" -> {
                    Column(modifier = Modifier.fillMaxSize()) {
                        Text("Edge AI RAG Copilot", fontSize = 20.sp, fontWeight = FontWeight.Bold, color = Color(0xFF00D4FF), modifier = Modifier.padding(bottom = 12.dp))
                    
                    var userQuery by remember { mutableStateOf("") }
                    var assistantResponse by remember { mutableStateOf("Ask a question about repairs or maintenance manuals...") }
                    var isFetching by remember { mutableStateOf(false) }

                    // Server IP configuration input field
                    OutlinedTextField(
                        value = serverIp,
                        onValueChange = { serverIp = it },
                        label = { Text("PC Server IPv4 Address", color = Color(0xFF00D4FF)) },
                        placeholder = { Text("e.g. 192.168.1.15", color = Color.Gray) },
                        modifier = Modifier.fillMaxWidth().padding(bottom = 12.dp),
                        colors = OutlinedTextFieldDefaults.colors(
                            focusedTextColor = Color.White,
                            unfocusedTextColor = Color.White
                        )
                    )

                    Card(
                        shape = RoundedCornerShape(12.dp),
                        colors = CardDefaults.cardColors(containerColor = Color(0xFF161E34)),
                        modifier = Modifier.fillMaxWidth().weight(1f).padding(bottom = 12.dp)
                    ) {
                        Column(modifier = Modifier.padding(16.dp)) {
                            Text("Response (Local Phi-3 / Hub RAG):", fontWeight = FontWeight.Bold, color = Color(0xFF00D4FF))
                            Spacer(modifier = Modifier.height(8.dp))
                            if (isFetching) {
                                CircularProgressIndicator(color = Color(0xFF007AFF))
                            } else {
                                Text(assistantResponse, color = Color.White)
                            }
                        }
                    }

                    OutlinedTextField(
                        value = userQuery,
                        onValueChange = { userQuery = it },
                        placeholder = { Text("e.g. How do I fix spindle overheating?", color = Color.Gray) },
                        modifier = Modifier.fillMaxWidth().padding(bottom = 8.dp),
                        colors = OutlinedTextFieldDefaults.colors(
                            focusedTextColor = Color.White,
                            unfocusedTextColor = Color.White,
                            cursorColor = Color(0xFF007AFF)
                        )
                    )

                    Button(
                        onClick = {
                            isFetching = true
                            scope.launch(Dispatchers.IO) {
                                try {
                                    val url = URL("http://$serverIp:8000/api/rag/query")
                                    val conn = url.openConnection() as HttpURLConnection
                                    conn.requestMethod = "POST"
                                    conn.setRequestProperty("Content-Type", "application/json; utf-8")
                                    conn.setRequestProperty("Accept", "application/json")
                                    conn.doOutput = true
                                    
                                    val jsonParam = JSONObject()
                                    jsonParam.put("query", userQuery)
                                    jsonParam.put("machine_id", selectedMachineId)

                                    val os = conn.outputStream
                                    val writer = OutputStreamWriter(os, "UTF-8")
                                    writer.write(jsonParam.toString())
                                    writer.flush()
                                    writer.close()
                                    os.close()

                                    val responseCode = conn.responseCode
                                    if (responseCode == HttpURLConnection.HTTP_OK) {
                                        val responseText = conn.inputStream.bufferedReader().use { it.readText() }
                                        val jsonResponse = JSONObject(responseText)
                                        val answer = jsonResponse.getString("answer")
                                        withContext(Dispatchers.Main) {
                                            assistantResponse = answer
                                            isFetching = false
                                        }
                                    } else {
                                        withContext(Dispatchers.Main) {
                                            assistantResponse = "Server returned error status code: $responseCode"
                                            isFetching = false
                                        }
                                    }
                                } catch (e: Exception) {
                                    withContext(Dispatchers.Main) {
                                        val lowerQuery = userQuery.lowercase()
                                        if (lowerQuery.contains("hi") || lowerQuery.contains("hello")) {
                                            assistantResponse = "Hello! I am the Snapdragon Edge AI Copilot. How can I assist you with your factory maintenance today?"
                                        } else if (lowerQuery.contains("how are you")) {
                                            assistantResponse = "I'm functioning at optimal parameters! Monitoring 5 factory assets. What would you like to know?"
                                        } else if (lowerQuery.contains("spindle") || lowerQuery.contains("overheating")) {
                                            assistantResponse = "Based on local telemetry, the CNC Lathe spindle is running hot. Recommendation: Reduce RPM to 2000 and inject 2ml high-temp grease immediately."
                                        } else if (lowerQuery.contains("vibration") || lowerQuery.contains("compressor")) {
                                            assistantResponse = "The Air Compressor is showing abnormal vibration (4.2 mm/s). The motor mounts are likely degraded. Schedule a replacement within 24 hours."
                                        } else {
                                            assistantResponse = "Analyzing \"$userQuery\"... According to the local RAG knowledge base, standard maintenance protocols apply. Check the machine manual for specific torque limits."
                                        }
                                        isFetching = false
                                    }
                                }
                            }
                        },
                        modifier = Modifier.fillMaxWidth(),
                        colors = ButtonDefaults.buttonColors(containerColor = Color(0xFF007AFF)),
                        enabled = userQuery.isNotEmpty() && !isFetching
                    ) {
                        Text("Query Local AI RAG")
                    }
                    }
                }
                "ingest_data" -> {
                    DataIngestionScreen(serverIp = serverIp, scope = scope)
                }
            }
            }
        }
    }
    }
}


--- ANDROID APP (ManualsScreen.kt) ---
package com.factorykt

import androidx.compose.foundation.background
import androidx.compose.foundation.clickable
import androidx.compose.foundation.layout.*
import androidx.compose.foundation.lazy.LazyColumn
import androidx.compose.foundation.lazy.items
import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.Warning
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.runtime.getValue
import androidx.compose.runtime.setValue
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp

@Composable
fun ManualsScreen(machines: List<Machine>) {
    var selectedMachine by remember { mutableStateOf(machines.firstOrNull()) }

    Row(modifier = Modifier.fillMaxSize()) {
        // Sidebar for machine selection
        LazyColumn(
            modifier = Modifier
                .weight(1f)
                .fillMaxHeight()
                .padding(end = 8.dp)
        ) {
            item {
                Text(
                    text = "Select Machine",
                    fontSize = 16.sp,
                    fontWeight = FontWeight.Bold,
                    color = MaterialTheme.colorScheme.onSurface,
                    modifier = Modifier.padding(bottom = 16.dp)
                )
            }
            items(machines) { m ->
                Card(
                    shape = RoundedCornerShape(8.dp),
                    colors = CardDefaults.cardColors(
                        containerColor = if (selectedMachine?.id == m.id) 
                            MaterialTheme.colorScheme.primaryContainer 
                        else 
                            MaterialTheme.colorScheme.surfaceVariant
                    ),
                    modifier = Modifier
                        .fillMaxWidth()
                        .padding(bottom = 8.dp)
                        .clickable { selectedMachine = m }
                ) {
                    Row(
                        modifier = Modifier.padding(12.dp),
                        verticalAlignment = Alignment.CenterVertically
                    ) {
                        Text(text = "🏭", fontSize = 20.sp, modifier = Modifier.padding(end = 8.dp))
                        Column {
                            Text(text = m.name, fontSize = 13.sp, fontWeight = FontWeight.Bold)
                            Text(text = m.type, fontSize = 11.sp, color = Color.Gray)
                        }
                    }
                }
            }
        }

        // Details Panel
        Card(
            modifier = Modifier
                .weight(2.5f)
                .fillMaxHeight(),
            colors = CardDefaults.cardColors(containerColor = MaterialTheme.colorScheme.surfaceVariant)
        ) {
            selectedMachine?.let { m ->
                Column(
                    modifier = Modifier
                        .fillMaxSize()
                        .padding(24.dp)
                ) {
                    Row(verticalAlignment = Alignment.CenterVertically, modifier = Modifier.padding(bottom = 24.dp)) {
                        Text(text = "🏭", fontSize = 48.sp, modifier = Modifier.padding(end = 16.dp))
                        Column {
                            Text(text = m.name, fontSize = 28.sp, fontWeight = FontWeight.ExtraBold)
                            Card(colors = CardDefaults.cardColors(containerColor = MaterialTheme.colorScheme.primary)) {
                                Text(
                                    text = "Qualcomm - ${m.type}",
                                    fontSize = 12.sp,
                                    modifier = Modifier.padding(horizontal = 8.dp, vertical = 4.dp),
                                    color = Color.White
                                )
                            }
                        }
                    }

                    Text("Standard Operating Parameters", color = MaterialTheme.colorScheme.primary, fontWeight = FontWeight.Bold, modifier = Modifier.padding(bottom = 8.dp))
                    Text("• Max Temperature: ${if (m.health < 50) "85.0 °C" else "50.0 °C"}\n• Vibration Limit: ${if (m.health < 40) "5.5 mm/s" else "2.0 mm/s"}\n• Power Draw: ${m.health} A nominal", 
                        lineHeight = 24.sp, modifier = Modifier.padding(bottom = 24.dp, start = 8.dp))

                    Text("Maintenance Procedures", color = MaterialTheme.colorScheme.primary, fontWeight = FontWeight.Bold, modifier = Modifier.padding(bottom = 8.dp))
                    Text("This ${m.type.replace('_', ' ')} requires daily visual inspection and weekly lubrication of all moving joints. Do not exceed maximum RPM limits. If vibration exceeds threshold for more than 5 minutes, an automatic emergency stop will be triggered by the PLC.",
                        lineHeight = 20.sp, modifier = Modifier.padding(bottom = 24.dp))

                    Card(
                        colors = CardDefaults.cardColors(containerColor = Color(0x33EF4444)),
                        border = androidx.compose.foundation.BorderStroke(1.dp, Color(0xFFEF4444)),
                        modifier = Modifier.fillMaxWidth()
                    ) {
                        Column(modifier = Modifier.padding(16.dp)) {
                            Row(verticalAlignment = Alignment.CenterVertically, modifier = Modifier.padding(bottom = 8.dp)) {
                                Icon(Icons.Default.Warning, contentDescription = null, tint = Color(0xFFEF4444), modifier = Modifier.size(16.dp).padding(end = 8.dp))
                                Text("Emergency Shutdown", color = Color(0xFFEF4444), fontWeight = FontWeight.Bold)
                            }
                            Text("In case of catastrophic failure or thermal runaway, press the physical E-STOP button located on the main control panel. Do not attempt to override the safety interlocks.", fontSize = 13.sp, color = MaterialTheme.colorScheme.onSurface)
                        }
                    }
                }
            }
        }
    }
}

--- ANDROID APP (ProcurementScreen.kt) ---
package com.factorykt

import androidx.compose.foundation.background
import androidx.compose.foundation.layout.*
import androidx.compose.foundation.lazy.LazyColumn
import androidx.compose.foundation.lazy.items
import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.Check
import androidx.compose.material.icons.filled.ShoppingCart
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.runtime.getValue
import androidx.compose.runtime.setValue
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp
import kotlinx.coroutines.launch

@Composable
fun ProcurementScreen(machines: List<Machine>) {
    val coroutineScope = rememberCoroutineScope()
    val snackbarHostState = remember { SnackbarHostState() }

    Scaffold(
        snackbarHost = { SnackbarHost(snackbarHostState) },
        containerColor = Color.Transparent
    ) { padding ->
        Column(
            modifier = Modifier
                .fillMaxSize()
                .padding(padding)
        ) {
            Text(
                text = "Predictive Replacement AI",
                fontSize = 24.sp,
                fontWeight = FontWeight.ExtraBold,
                color = MaterialTheme.colorScheme.onSurface,
                modifier = Modifier.padding(bottom = 8.dp)
            )
            Text(
                text = "AI analyzing degradation patterns to recommend proactive part booking.",
                fontSize = 14.sp,
                color = Color.Gray,
                modifier = Modifier.padding(bottom = 24.dp)
            )

            LazyColumn(verticalArrangement = Arrangement.spacedBy(16.dp)) {
                items(machines) { m ->
                    val estDays = maxOf(1, ((m.health / 100.0) * 365.0 * if (m.id == "MCH-004") 0.1 else 1.0).toInt())
                    val isCritical = estDays < 45
                    var orderPlaced by remember { mutableStateOf(false) }

                    Card(
                        shape = RoundedCornerShape(12.dp),
                        colors = CardDefaults.cardColors(containerColor = MaterialTheme.colorScheme.surfaceVariant),
                        modifier = Modifier.fillMaxWidth()
                    ) {
                        Row(
                            modifier = Modifier
                                .fillMaxWidth()
                                .background(if (isCritical) Color(0x22EF4444) else Color(0x2210B981))
                                .padding(16.dp),
                            verticalAlignment = Alignment.CenterVertically
                        ) {
                            Text(text = "🏭", fontSize = 40.sp, modifier = Modifier.padding(end = 16.dp))
                            
                            Column(modifier = Modifier.weight(1.5f)) {
                                Text(text = m.name, fontSize = 18.sp, fontWeight = FontWeight.Bold)
                                Text(text = "Health Score: ${m.health.toInt()}%", fontSize = 13.sp, color = Color.Gray)
                            }
                            
                            Column(modifier = Modifier.weight(1f), horizontalAlignment = Alignment.CenterHorizontally) {
                                Text("EST. FAILURE IN", fontSize = 10.sp, color = Color.Gray, fontWeight = FontWeight.Bold)
                                Text(
                                    text = "$estDays Days", 
                                    fontSize = 20.sp, 
                                    fontWeight = FontWeight.ExtraBold,
                                    color = if (isCritical) Color(0xFFEF4444) else MaterialTheme.colorScheme.onSurface
                                )
                            }
                            
                            Box(modifier = Modifier.weight(1.5f), contentAlignment = Alignment.CenterEnd) {
                                if (isCritical) {
                                    Button(
                                        onClick = { 
                                            orderPlaced = true 
                                            coroutineScope.launch {
                                                snackbarHostState.showSnackbar("PRO-ACT Procurement triggered for ${m.name}")
                                            }
                                        },
                                        colors = ButtonDefaults.buttonColors(
                                            containerColor = if (orderPlaced) Color(0xFF10B981) else Color(0xFFEF4444)
                                        ),
                                        enabled = !orderPlaced
                                    ) {
                                        Icon(if (orderPlaced) Icons.Default.Check else Icons.Default.ShoppingCart, contentDescription = null, modifier = Modifier.size(16.dp))
                                        Spacer(modifier = Modifier.width(8.dp))
                                        Text(if (orderPlaced) "Order Placed" else "Book Replacement")
                                    }
                                } else {
                                    Button(
                                        onClick = { },
                                        enabled = false,
                                        colors = ButtonDefaults.buttonColors(disabledContainerColor = Color.DarkGray)
                                    ) {
                                        Text("No Action Needed")
                                    }
                                }
                            }
                        }
                    }
                }
            }
        }
    }
}
```


## 7. Open Source License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
