# IRIS – Indicații Radioimagistice Structurate

A progressive web application (PWA) providing clinical decision support for medical imaging recommendations in Romanian.

**IRIS** is an interactive guide based on radiological imaging guidelines, offering:
- **1,655+ clinical recommendations** across 14 major chapters and 796 clinical situations
- **Evidence-based guidance** with graded recommendations (A, B, C levels)
- **Radiation dose information** with comparative assessments
- **Offline access** via service worker technology
- **Searchable synonyms** supporting technical terminology lookups
- **Mobile-first design** with light/dark theme support

## Features

### Clinical Decision Support
- Browse recommendations by chapter and clinical situation
- Filter by examination type and modality (X-ray, CT, MRI, Ultrasound, etc.)
- View indication grades and supporting comments
- Compare radiation dose levels

### Accessibility
- Fully responsive design for desktop, tablet, and mobile
- Progressive Web App – installable and works offline
- Dark and light theme options
- Print-friendly layout
- Multi-language synonyms for term expansion

### Data
The application contains structured data organized as:
```
chapters (1) ──> subchapters (n) ──> situations (n) ──> recommendations (n)
```

See [`data/SCHEMA.md`](data/SCHEMA.md) for complete data structure documentation.

## Quick Start

### Online
- **Desktop/Web**: Open `Iris.dc.html` or access the hosted version
- **Mobile/PWA**: Use `pwa/index.html` for the optimized PWA experience

### Offline
The PWA registers a service worker (`pwa/sw.js`) that enables:
- Complete offline access after first load
- Cached data and assets
- Installation as a standalone app

### Data Conversion
To regenerate JSON data from a CSV source:
1. Open `tools/csv-to-json.html` in a browser
2. Load a CSV file with the standard schema
3. Download the regenerated JSON package

## File Structure

```
IRIS/
├── Iris.dc.html              # Desktop/web app (single-file version)
├── Iris-offline.html         # Offline-capable version
├── support.js                # Shared utility functions
│
├── pwa/                       # Progressive Web App
│   ├── index.html            # Main PWA entry point
│   ├── manifest.json         # PWA manifest
│   ├── sw.js                 # Service worker
│   ├── support.js            # PWA utilities
│   ├── data/                 # PWA data assets
│   └── icon-*.png            # App icons
│
├── data/                      # Core application data
│   ├── iris-data.js          # Nested guidelines (single payload)
│   ├── chapters.json
│   ├── subchapters.json
│   ├── situations.json
│   ├── recommendations.json
│   ├── guidelines.json
│   ├── legend.json           # Modality, indication, dose codes
│   ├── synonyms.json         # Search term expansion
│   ├── SCHEMA.md             # Data structure documentation
│   └── source/               # Source CSV data
│
└── tools/
    └── csv-to-json.html      # Data conversion utility
```

## Data Schema

Each recommendation includes:
- **Situation**: Clinical presentation (e.g., "Suspected pulmonary embolism")
- **Modality**: Imaging type (X-ray, CT, MRI, Ultrasound, etc.)
- **Examination**: Specific exam name
- **Indication**: Recommendation status (7 levels)
- **Grade**: Evidence level (A/B/C)
- **Comments**: Clinical rationale and notes
- **Other Info**: Additional guidance
- **Radiation Dose**: Min/max dose scale (0–4) with natural background comparison

See [`data/SCHEMA.md`](data/SCHEMA.md) for complete field documentation.

## Technologies

- **HTML5 / CSS3 / JavaScript** – Core application
- **Service Workers** – Offline functionality
- **Web Manifest** – PWA installation
- **Custom Web Components** – UI architecture
- **JSON Data Format** – Normalized and denormalized structures

## License

See [LICENSE](LICENSE) file for details.

## Credits

Based on radiological imaging guidelines. Data sourced from `Ghid indicații radioimagistice`.

---

**For more information**: Open the application and explore the interactive guide, or refer to [`data/SCHEMA.md`](data/SCHEMA.md) for technical details about the data structure.
