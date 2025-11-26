# CKD-EPI eGFR Calculator

> Active docs: the primary documentation is `QUICK_REFERENCE.md`. Other documentation created during recent development has been archived to `docs_backup/` to reduce repository clutter.

A Progressive Web Application (PWA) for calculating estimated Glomerular Filtration Rate (eGFR) using the CKD-EPI formula. The application supports both Russian and English languages and features a responsive design suitable for desktop and mobile devices.

## 📋 Overview

This calculator helps healthcare professionals and patients estimate kidney function using the CKD-EPI (Chronic Kidney Disease Epidemiology Collaboration) formula. It provides:

- **Real-time eGFR calculations** based on patient parameters
- **Bilingual support** (Russian and English)
- **Calculation history** with import/export functionality
- **Visual charts** tracking eGFR trends over time
- **CKD stage classification** with color-coded visualization
- **Progressive Web App** capability for offline access

## 🎯 Key Features

### 1. **Calculator Tab**
- **Date of Calculation**: Select the date when the calculation is performed
- **Serum Creatinine**: Input in either mg/dL or μmol/L with unit conversion
- **Results Display**: Shows calculated eGFR value in mL/min/1.73m²
- **Unit Toggle**: Switch between mg/dL and μmol/L (conversion factor: 88.4)

### 2. **Settings Tab**
- **Date of Birth**: Required for age calculation
- **Sex**: Select Female or Male (affects formula coefficients)
- **Race**: Non-Black or Black (applies race factor of 1.012 for Black individuals)
- **Auto-save**: All settings are automatically saved to browser localStorage

### 3. **History Tab**
- **Calculation History Table**: Displays all previous calculations sorted by date
- **Data Management**: Delete individual entries with confirmation dialog
- **Export Data**: Download history as JSON file (format: GFR-DD-MM-YYYY-HHMM.json)
- **Import Data**: Upload previously exported JSON files with validation

### 4. **Visual Analytics**
- **eGFR Chart**: Line graph showing historical data and predicted decline
- **CKD Stage Zones**: Color-coded background zones for each CKD stage
- **Trend Lines**: 
  - Blue line: Historical data points
  - Orange dashed line: Linear regression projection (10-year forecast)
- **Legend**: Visual reference for CKD stages (1-5)

## 🔬 Medical Background

### CKD-EPI Formula

The CKD-EPI formula calculates eGFR based on:

```
eGFR = 142 × (Scr/κ)^α × 0.9938^Age × Race Factor
```

Where:
- **Scr**: Serum creatinine (mg/dL)
- **κ** (kappa):
  - 0.7 for females
  - 0.9 for males
- **α** (alpha):
  - -0.241 if Scr ≤ κ (females) or ≤ 0.9 (males)
  - -1.2 if Scr > κ (females) or > 0.9 (males)
- **Race Factor**:
  - 1.0 for Non-Black individuals
  - 1.012 for Black individuals
- **Age**: Patient age in years at time of calculation

### CKD Stages

| Stage | eGFR (mL/min/1.73m²) | Kidney Function | Color |
|-------|----------------------|-----------------|-------|
| 1 | ≥90 | Normal | Green (#10b981) |
| 2 | 60-89 | Mild decrease | Light Green (#22c55e) |
| 3a | 45-59 | Mild to moderate decrease | Yellow (#eab308) |
| 3b | 30-44 | Moderate to severe decrease | Orange (#f59e0b) |
| 4 | 15-29 | Severe decrease | Dark Orange (#f97316) |
| 5 | <15 | Kidney failure | Red (#ef4444) |

## 📁 Project Structure

```
egfr-dev/
├── index.html          # Main application file (PWA, all-in-one)
├── manifest.json       # PWA manifest configuration
├── icon-192.png        # App icon (192×192 pixels)
├── icon-512.png        # App icon (512×512 pixels)
├── sw.js              # Service Worker (referenced, not included)
└── README.md          # This documentation
```

## 🛠️ Technology Stack

- **Frontend Framework**: HTML5 + CSS3 (Tailwind CSS)
- **Charting**: Chart.js v3 with date-fns adapter
- **Styling**: Tailwind CSS (CDN)
- **Storage**: Browser localStorage API
- **PWA Features**: Web App Manifest, Service Worker
- **Internationalization**: Custom translation object system

## 🔧 Installation & Deployment

### Local Development
1. Clone or download the project files
2. Ensure `index.html`, `manifest.json`, and icon files are in the same directory
3. Create or implement `sw.js` Service Worker for PWA functionality
4. Serve via HTTPS (required for PWA)

### Deployment Requirements
- HTTPS endpoint (PWA requirement)
- Web server capable of serving static files
- Service Worker file (`sw.js`) implementation

### Browser Support
- Modern browsers with ES6 support
- PWA support: Chrome, Edge, Firefox, Safari (partial)
- Minimum API requirements:
  - localStorage
  - fetch API
  - Promise support
  - ES6 classes/arrow functions

## 📊 Data Storage

### localStorage Schema

```json
{
  "creatinineCalculatorData": {
    "dob": "YYYY-MM-DD",
    "sex": "female|male",
    "race": "non-black|black",
    "history": [
      {
        "date": "YYYY-MM-DD",
        "egfr": 85.5,
        "creatinine": 0.95,
        "units": "mg/dL|µmol/L",
        "age": 45
      }
    ]
  }
}
```

### Export/Import Format

Exported JSON files contain complete patient data:
- Patient demographics (DOB, sex, race)
- Complete calculation history
- Timestamps for all entries

## 🌐 Internationalization

The application supports full bilingual interface:

### Supported Languages
- **Russian (RU)**: Default language
- **English (EN)**: Alternative language

### Language Switch
- Toggle in the top-right corner of the main interface
- All UI elements update dynamically
- Selection persists in user session

### Translation Keys
- Over 40 translation keys for complete UI coverage
- Accessible via `data-lang-key` HTML attributes
- Managed in JavaScript `translations` object

## 📱 PWA Features

### Installation
- "Add to Home Screen" support on mobile devices
- Standalone app mode (full-screen experience)
- Custom app icon and splash screen

### Offline Capability
- Service Worker registration for offline support
- Local data persistence via localStorage
- No backend server required

### Configuration (manifest.json)
- App name: Калькулятор СКФ CKD-EPI
- Display: Standalone
- Theme color: #3b82f6 (blue)
- Categories: health, medical, productivity

## 🎨 UI/UX Components

### Tab System
- **Calculator**: Input patient data and calculate eGFR
- **Settings**: Manage patient demographics
- **History**: View and manage calculation records

### Modals
- **Confirmation Modal**: Delete entry confirmation
- **Import Warning Modal**: Data import confirmation with risk warning

### Message Boxes
- **Success Messages**: Green background (#10b981)
- **Error Messages**: Red background (#dc2626)
- **Info Messages**: Blue background (#3b82f6)

### Unit Converter
- Real-time conversion: mg/dL ↔ μmol/L
- Conversion factor: 88.4
- Label updates automatically

## 📈 Chart Features

### Visualization Elements
1. **Historical Data Points**: Blue line connecting actual calculations
2. **Extrapolation**: Orange dashed line showing 10-year prediction
3. **CKD Stage Zones**: Semi-transparent colored background areas
4. **Threshold Lines**: Dashed lines at critical thresholds:
   - eGFR = 30 (orange)
   - eGFR = 15 (red)
   - eGFR = 5 (dark red)

### Chart Calculations
- **Linear Regression**: Calculates trend slope and intercept
- **Extrapolation**: Projects eGFR decline for next 10 years
- **Date Scale**: X-axis displays yearly intervals
- **Value Range**: Y-axis from 0 to 150 mL/min/1.73m²

## ⚠️ Important Notes

### Clinical Disclaimer
- **Not for clinical diagnosis**: Results should not be used solely for medical diagnosis
- **Expected Decline projection**: Simplified mathematical model, not medically validated
- **Professional consultation**: Always consult healthcare providers for medical decisions
- **Age restriction**: Calculator designed for individuals 18+ years old

### Data Privacy
- All calculations stored locally in browser (no cloud sync)
- No personal data sent to external servers
- User responsible for data backup via export feature
- Data persists until manually cleared or localStorage deleted

## 🔄 Workflow

### Typical Usage Flow
1. **Initial Setup**
   - Enter Date of Birth in Settings tab
   - Select Sex and Race
   - Data auto-saves

2. **Calculate eGFR**
   - Enter calculation date
   - Input serum creatinine value (with unit selection)
   - Click "Calculate eGFR"
   - View results

3. **Track Progress**
   - View History tab for all calculations
   - Monitor eGFR trend in Chart section
   - Delete outdated entries if needed

4. **Data Management**
   - Export data for backup: History tab → Export Data
   - Import previous data: History tab → Import Data
   - Select JSON file and confirm import

## 🐛 Troubleshooting

### Common Issues

**Calculation won't work**
- Verify Date of Birth is entered in Settings
- Ensure all fields contain valid values
- Check that age is 18 or older

**Settings not saving**
- Clear browser cache and localStorage
- Check browser privacy/incognito mode settings
- Verify localStorage is enabled

**Chart not displaying**
- Minimum 1 calculation required in history
- Check browser JavaScript console for errors
- Try refreshing the page

**Import fails**
- Verify JSON file format from export
- Ensure file contains required fields: dob, sex, race, history
- Check for file corruption

## 🔐 Security Considerations

- No external API calls (fully client-side)
- No authentication required (local data only)
- HTTPS recommended for PWA deployment
- localStorage vulnerable if device is compromised
- Recommended to export and backup important data regularly

## 📝 Development Notes

### Code Organization
- Single HTML file with embedded CSS and JavaScript
- Modular function structure for easy maintenance
- Event-driven architecture for user interactions
- Plugin system for Chart.js customization

### Key Functions
- `calculateEGFRFromCreatinine()`: Core eGFR calculation
- `linearRegression()`: Trend analysis and projection
- `updateChart()`: Chart rendering and updates
- `importData()` / `exportData()`: Data I/O operations

### Customization Points
- Translation object for new languages
- CKD stage colors and thresholds in chart plugins
- Formula coefficients (currently CKD-EPI)
- Chart styling and dimensions

## 📞 Support

For issues or questions:
1. Check the troubleshooting section above
2. Review the code comments in index.html
3. Verify your browser supports required APIs
4. Ensure HTTPS is enabled for PWA features

## 📄 License

Please refer to the project's LICENSE file for licensing information.

---

**Version**: 1.0  
**Last Updated**: November 2025  
**Language**: Russian (Primary), English (Secondary)
