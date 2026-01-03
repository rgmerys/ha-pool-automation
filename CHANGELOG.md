# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.1] - 2026-01-02

### 🐛 Fixed

#### Critical Bug: Season Detection Across Year Boundaries
- **Problem:** Season sensor showed "Fuera de temporada" when it should show "Verano" for seasons crossing New Year (e.g., Nov 2025 - Mar 2026)
- **Cause:** Template was incorrectly replacing years in configured dates
- **Solution:** Removed year replacement logic, now uses exact configured dates
- **Impact:** Season detection now works correctly for any date range, including those that cross calendar years

#### Critical Bug: False Manual Mode Detection on Automatic Schedule
- **Problem:** Pump switched from Automatic to Manual mode every minute, even when controlled by automation
- **Cause:** Manual detection triggered on ALL state changes, including those from the control automation itself
- **Solution:** 
  - Added `input_boolean.piscina_control_automatico_activo` flag to track when automation is in control
  - Added 5-second delays after pump state changes before clearing the flag
  - Manual detection now only triggers when flag is OFF (not controlled by automation)
- **Impact:** Pump stays in Automatic mode during scheduled operations, only switches to Manual when actually controlled manually (Alexa, UI, app, physical button)

### ✨ Added

#### Smart Return to Automatic on Manual Shutdown
- **Feature:** When pump is in Manual mode and you turn it off (via Alexa, UI, app), it automatically returns to Automatic mode
- **Benefit:** More intuitive workflow - turn on manually when needed, turn off when done, system takes back control
- **Automation:** `[Piscina] Volver a Automático al apagar manualmente`

#### Control Activity Flag
- **Helper:** `input_boolean.piscina_control_automatico_activo` (hidden from users)
- **Purpose:** Internal flag to distinguish automation-controlled changes from manual changes
- **Usage:** Prevents false detection of manual control during scheduled operations

### 🔧 Changed

#### Manual Detection Logic
- **Before:** Used `context.user_id` to detect manual changes (only worked from Home Assistant UI)
- **After:** Uses control activity flag (works from Alexa, Google Home, Sonoff app, physical buttons, etc.)
- **Improvement:** More reliable detection across all control methods

#### Season Template Performance
- **Before:** Complex year replacement calculations on every evaluation
- **After:** Direct date comparison without year manipulation
- **Improvement:** Simpler logic, easier to understand and maintain

### 📚 Documentation

#### Enhanced Troubleshooting Guide
- Added detailed solution for season detection issues
- Added explanation of manual mode detection loop problem
- Included test templates to verify fixes
- Documented limitations of different control methods

#### Updated README
- Added information about the control activity flag
- Clarified how manual override detection works
- Updated technical notes section

---

## [1.0.0] - 2026-01-01

### 🎉 Initial Release

Complete pool automation package for Home Assistant.

#### Features

- **Three Operating Modes:**
  - OFF: Pump always off
  - Manual: Full manual control with automatic timeout
  - Automatic: Schedule-based operation with summer/winter seasons

- **Seasonal Schedules:**
  - Different pump schedules for summer vs off-season
  - Automatic season detection
  - Auto-update dates to next year when season ends

- **Smart Manual Mode:**
  - Auto-detects manual pump control
  - Returns to Automatic after configurable timeout (default: 24h)
  - Real-time countdown display

- **Automatic Pool Lighting:**
  - Triggers at sunset + configurable offset
  - Configurable duration
  - Easy enable/disable toggle

- **Energy Monitoring:**
  - Daily, monthly, and yearly consumption tracking
  - Real-time power, voltage, and amperage display
  - Integration with Home Assistant Energy Dashboard

#### Components

- Input helpers (select, boolean, datetime, number)
- Template sensors for season detection and manual mode tracking
- Comprehensive automation suite
- Utility meters for energy tracking
- Dashboard example configuration

#### Documentation

- Complete README in English and Spanish
- Troubleshooting guide
- Contributing guidelines
- Issue templates
- Installation instructions

---

## Release Links

- [1.0.1] - https://github.com/rgmerys/ha-pool-automation/releases/tag/v1.0.1
- [1.0.0] - https://github.com/rgmerys/ha-pool-automation/releases/tag/v1.0.0
