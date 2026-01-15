# How to Integrate ThingSpeak - Office Energy System

## Overview

ThingSpeak is a cloud platform for IoT data visualization. This guide shows how to set up ThingSpeak for the Office Energy System project.

## Step 1: Create ThingSpeak Account

1. Go to https://thingspeak.com
2. Click **"Sign Up"** (free account)
3. Complete registration and verify email

## Step 2: Create a New Channel

1. After logging in, click **"Channels"** → **"My Channels"**
2. Click **"New Channel"**
3. Fill in channel details:
   - **Name**: `Office Energy System`
   - **Description**: `Multi-room energy optimization and monitoring`

## Step 3: Configure Fields

Configure **6 fields** for your data:

| Field Number | Field Name | Description | Type |
|--------------|------------|-------------|------|
| **Field 1** | Occupancy Score | Room occupancy (0-1) | Numeric |
| **Field 2** | Motion Detection | Motion level (0-1) | Numeric |
| **Field 3** | Light Level | Light intensity (0-1) | Numeric |
| **Field 4** | Temperature | Temperature in °C | Numeric |
| **Field 5** | Energy Consumption | Power in Watts | Numeric |
| **Field 6** | Efficiency State | 0=Optimal, 1=Efficient, 2=Warning, 3=Critical | Numeric |

**To configure:**
1. Check **"Field 1"** checkbox
2. Enter **"Occupancy Score"** as the name
3. Repeat for all 6 fields
4. Click **"Save Channel"**

## Step 4: Get API Keys

1. Go to your channel page
2. Click **"API Keys"** tab
3. Copy the **"Write API Key"** (you'll need this)

## Step 5: Configure Code

### For Firmware

1. Run `idf.py menuconfig`
2. Navigate to: `Office Energy System` → `Use ThingSpeak cloud platform`
3. Enable **"Use ThingSpeak cloud platform"**
4. Enter your **"ThingSpeak Write API Key"**
5. Save and exit

## Step 6: Set Up Charts

### Chart 1: Occupancy Score (Field 1)

1. Go to your channel page
2. Click **"Add Visualizations"** → **"Add Widget"**
3. Select **"Line Chart"**
4. Configure:
   - **Name**: `Occupancy Score`
   - **Field**: Select **"Field 1"**
   - **Y-axis Label**: `Score`
   - **Y-axis Min**: `0`
   - **Y-axis Max**: `1.0`
   - **Line Color**: Blue
5. Click **"Save"**

### Chart 2: Motion Detection (Field 2)

1. Add **"Line Chart"**
2. Configure:
   - **Name**: `Motion Detection`
   - **Field**: Select **"Field 2"**
   - **Y-axis Min**: `0`
   - **Y-axis Max**: `1.0`
   - **Line Color**: Green
3. Click **"Save"**

### Chart 3: Light Level (Field 3)

1. Add **"Line Chart"**
2. Configure:
   - **Name**: `Light Level`
   - **Field**: Select **"Field 3"**
   - **Y-axis Min**: `0`
   - **Y-axis Max**: `1.0`
   - **Line Color**: Yellow
3. Click **"Save"**

### Chart 4: Temperature (Field 4)

1. Add **"Line Chart"**
2. Configure:
   - **Name**: `Temperature`
   - **Field**: Select **"Field 4"**
   - **Y-axis Label**: `°C`
   - **Y-axis Min**: `15`
   - **Y-axis Max**: `30`
   - **Line Color**: Red
3. Click **"Save"**

### Chart 5: Energy Consumption (Field 5)

1. Add **"Line Chart"**
2. Configure:
   - **Name**: `Energy Consumption`
   - **Field**: Select **"Field 5"**
   - **Y-axis Label**: `Watts`
   - **Y-axis Min**: `0`
   - **Y-axis Max**: `1000`
   - **Line Color**: Orange
3. Click **"Save"**

### Chart 6: Efficiency State (Field 6)

1. Add **"Line Chart"** or **"Gauge"**
2. Configure:
   - **Name**: `Efficiency State`
   - **Field**: Select **"Field 6"**
   - **Y-axis Min**: `0`
   - **Y-axis Max**: `3`
   - **Line Color**: Purple
   - **Legend**: 
     - 0 = Optimal
     - 1 = Efficient
     - 2 = Warning
     - 3 = Critical
3. Click **"Save"**

## Step 7: Test Data Upload

1. Flash firmware to ESP32-S3
2. Monitor serial output
3. Check ThingSpeak channel for updates
4. You should see data appearing in charts

## Step 8: View Real-Time Data

1. Go to your ThingSpeak channel
2. Charts update automatically every 15 seconds (free tier limit)
3. Use **"Private View"** for personal monitoring

## ThingSpeak Free Tier Limits

- **Update Rate**: 15 seconds between updates (minimum)
- **Data Retention**: 1 year
- **Channels**: Unlimited
- **Fields per Channel**: 8 fields

## Data Field Mapping Reference

| Code Field | ThingSpeak Field | Description |
|------------|------------------|-------------|
| `occupancy` | Field 1 | Occupancy score (0-1) |
| `motion` | Field 2 | Motion detection (0-1) |
| `light_level` | Field 3 | Light intensity (0-1) |
| `temperature` | Field 4 | Temperature in °C |
| `energy_consumption` | Field 5 | Power in Watts |
| `efficiency_state` | Field 6 | 0=Optimal, 1=Efficient, 2=Warning, 3=Critical |

## Troubleshooting

### "ThingSpeak update failed"
- ✅ Verify API key is correct (Write API Key)
- ✅ Check channel is active
- ✅ Verify internet connection
- ✅ Free tier: Wait 15 seconds between updates

### "No data appearing"
- ✅ Check Serial Monitor for upload status
- ✅ Verify field numbers match (Field 1-6)
- ✅ Refresh ThingSpeak page

### "Charts not updating"
- ✅ ThingSpeak free tier updates every 15 seconds minimum
- ✅ Verify data is being sent
- ✅ Clear browser cache and refresh

