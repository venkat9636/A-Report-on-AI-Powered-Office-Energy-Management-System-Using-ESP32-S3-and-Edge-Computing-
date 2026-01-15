# How to Run on Wokwi - Office Energy System

## Prerequisites

- Wokwi account (free at https://wokwi.com)
- Python 3.8+ installed (for ML backend server)
- Internet connection

## Step 1: Start ML Backend Server (Required for Real ML)

The Wokwi simulator uses a Python backend server for real TinyML model inference.

**Open a terminal and run:**
```bash
cd office_energy_system/wokwi
python ml_backend_server.py
```

**Expected Output:**
```
============================================================
TinyML Backend Server for Wokwi
============================================================
Models loaded: 7
Available projects: ['office_energy_system', ...] (and other projects)
Server starting on http://localhost:5000
```

**Keep this running** - Wokwi needs it to use real ML models.

**Note**: The ML backend server is located in this project's `wokwi/` folder. It serves models for all projects in the workspace.

## Step 2: Open Wokwi

1. Go to https://wokwi.com
2. Sign in or create a free account
3. Click **"New Project"** or **"Create New"**

## Step 3: Select ESP32 Board

1. In the project creation dialog, select **"ESP32"**
2. Click **"Create"**

## Step 4: Upload Code

**Note**: Office Energy System may not have a Wokwi .ino file yet. If not available:
1. Use the firmware code from `firmware/main/main.cpp`
2. Adapt it for Wokwi (add WiFi and HTTP client libraries)
3. Create a new `.ino` file following the ESP32 Arduino structure

## Step 5: Configure ThingSpeak API Key

1. In the code, find the ThingSpeak API key line
2. **Replace with your ThingSpeak Write API Key** (see ThingSpeak setup guide)

## Step 6: Configure ML Backend

Set the project name in ML backend requests:
```cpp
"project": "office_energy_system"
```

## Step 7: Add Components (Optional)

Wokwi automatically provides:
- ✅ WiFi connection (Wokwi-GUEST network)
- ✅ Serial Monitor
- ✅ Basic I/O pins

## Step 8: Run Simulation

1. Click the **green "Play" button** (▶) or press `F1`
2. Wait for "Connecting to WiFi..." message
3. Open **Serial Monitor** (click the monitor icon or press `Ctrl+Shift+M`)

## Step 9: Monitor Output

In Serial Monitor, you should see:
```
=== Office Energy System ===
Connecting to WiFi...
WiFi connected!
Occupancy: X% | Motion: X% | Energy: X W | State: X
```

## Step 10: Verify ML Backend is Working

Look for these messages in Serial Monitor:
- ✅ **Best**: `ML Backend result: X (using real TinyML model)`
- ⚠️ **Fallback**: `ML backend unavailable, using heuristic fallback`

## Step 11: View ThingSpeak Data

1. Go to your ThingSpeak channel
2. You should see data updating every 15 seconds
3. Check the 6 fields (see ThingSpeak integration guide)

## Troubleshooting

### "WiFi connection failed"
- ✅ Wokwi provides WiFi automatically - wait a few seconds
- ✅ Check Serial Monitor for connection status

### "ML backend unavailable"
- ✅ Verify `ml_backend_server.py` is running
- ✅ Check project name is "office_energy_system"
- ✅ Or disable ML backend to use heuristic

### "ThingSpeak update failed"
- ✅ Verify API key is correct
- ✅ Check ThingSpeak channel is active
- ✅ Free tier limit: 15 seconds between updates

## Stopping Simulation

Click the **red "Stop" button** (⏹) or press `Shift+F1`

