# How to Run Locally - Office Energy System

## Prerequisites

- Python 3.8+ installed
- Web browser (Chrome, Firefox, or Edge)
- Internet connection (for CDN resources)

## Step 1: Install Python Dependencies

```bash
# Install required packages
pip install flask flask-cors tensorflow numpy
```

## Step 2: Start ML Backend Server

The dashboard uses a Python backend server for real TinyML model inference.

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

**Note**: The ML backend server is located in this project's `wokwi/` folder. It serves models for all projects in the workspace.

**Keep this terminal window open** - the server must be running for the dashboard to use real ML models.

## Step 3: Start Dashboard Server

Open a **new terminal window** and run:

```bash
cd d:\kapil
python serve_dashboard.py
```

**Expected Output:**
```
============================================================
Dashboard Server - Running on http://localhost:8000
============================================================
```

## Step 4: Open Dashboard in Browser

Open your web browser and navigate to:

```
http://localhost:8000/office_energy_system/demo_dashboard_web/index.html
```

## Step 5: Verify ML Model is Working

1. Open browser Developer Tools (Press `F12`)
2. Go to the **Console** tab
3. You should see one of these messages:
   - ✅ **Best**: `✓ ML Backend server available - using real TinyML models`
   - ✅ **Good**: `✓ TensorFlow.js model loaded successfully - using real ML inference`
   - ⚠️ **Fallback**: `Could not load TensorFlow.js model, using heuristic fallback`

## What You'll See

The dashboard displays:
- **Real-time sensor data**: Occupancy, motion, light, temperature, energy consumption
- **ML classification**: Optimal, Efficient, Warning, or Critical state
- **6 ThingSpeak-style charts**: One for each data field
- **Probability bars**: Showing confidence for each classification

## Troubleshooting

### Dashboard shows "heuristic fallback"
- ✅ Check that `ml_backend_server.py` is running on port 5000
- ✅ Verify the server shows "Models loaded: 7" (or more)
- ✅ Check browser console for connection errors
- ✅ Verify `office_energy_system` is in the list of available projects

### Charts not updating
- ✅ Refresh the browser page
- ✅ Check browser console for JavaScript errors
- ✅ Verify both servers are running

### Port already in use
- Change port in `serve_dashboard.py` (line with `PORT = 8000`)
- Or change port in `ml_backend_server.py` (line with `port=5000`)

## Stopping the Servers

Press `Ctrl+C` in each terminal window to stop the servers.

