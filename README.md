# EduStats

**A browser-based student performance analysis tool for teachers and schools.**

EduStats turns class marks into useful statistical summaries, visualisations, and PDF reports. It is designed to make common classroom analysis tasks easier without requiring a hosted application backend.

[![Live Demo](https://img.shields.io/badge/Live%20Demo-Open%20EduStats-2f855a)](https://hamzaskr.github.io/EduStats/)
[![Source Code](https://img.shields.io/badge/Source-GitHub-181717?logo=github)](https://github.com/hamzaskr/EduStats)

## Why EduStats?

Teachers often have marks stored in spreadsheets but still need to calculate class statistics, identify students who may need additional support, understand performance patterns, and prepare reports. EduStats brings these tasks into one browser-based workflow:

1. Set up a class, term, and subjects.
2. Enter marks manually or import them from a CSV file.
3. Analyse student and class performance.
4. Explore charts and distributions.
5. Export individual or whole-class PDF reports.

## Features

### Class and subject setup

- Create a class analysis workspace with school, teacher, class, and term information.
- Add subjects and configure each subject's maximum mark and fail threshold.
- Use the default fail threshold of 40% of the subject's maximum mark, or adjust it.

### Marks and data handling

- Enter student marks in the browser.
- Import and export marks using CSV files.
- Handle different maximum marks across subjects through normalised class averages.
- Treat blank marks as missing rather than counting them as zero.
- Save term snapshots in IndexedDB, with a localStorage fallback for supported data.

### Statistical analysis

EduStats calculates a range of descriptive statistics and class-level indicators, including:

- Mean, median, and mode.
- Standard deviation.
- Quartiles and interquartile range (IQR).
- Outlier detection.
- Pass/fail counts and rates by subject.
- Class rankings and support-oriented sorting or filtering.

### Visualisations and reports

- Charts for mark distributions, cumulative frequency, subject performance, trends, and radar comparisons.
- Individual student PDF reports.
- Whole-class PDF reports.
- Light and dark themes, compact table mode, keyboard navigation, and reduced-motion support.

### Optional predictive analysis

The project includes an optional predictive-grade workflow. Model training is performed offline with Python. In the browser, predictive inference can use an ONNX model when the required model/runtime assets are available; otherwise, the application uses its documented local heuristic fallback. Predictions should be treated as indicative support signals, not definitive judgements about a student's ability or future results.

## Technology

- **Frontend:** HTML, CSS, and vanilla JavaScript.
- **Statistical processing:** JavaScript modules and Web Workers.
- **Charts:** Chart.js.
- **PDF generation:** jsPDF and jsPDF-AutoTable.
- **CSV processing:** Papa Parse.
- **Browser storage:** IndexedDB with a localStorage fallback.
- **Optional model workflow:** Python, scikit-learn, skl2onnx, and ONNX inference support.
- **Testing:** Node.js built-in test runner and project-specific smoke/stress checks.

## Run locally

EduStats is a static browser application. For local development, serve the repository through a local HTTP server rather than opening `index.html` directly. This helps browser workers and asset loading behave consistently.

### 1. Clone the repository

```bash
git clone https://github.com/hamzaskr/EduStats.git
cd EduStats
```

### 2. Start a local server

Using Python:

```bash
python -m http.server 8080
```

Open [http://localhost:8080](http://localhost:8080) in your browser.

You can use another static file server if preferred.

## Using CSV files

EduStats accepts a student data table with a header row. A simple example is:

```csv
Student ID,Full Name,Mathematics,English,Mean
G9B001,Aisha Mwangi,91,84,
G9B002,Brian Otieno,67,58,
```

The `Mean` column is optional; EduStats recalculates it. Blank subject marks are treated as missing values.

Optional subject threshold metadata can be included before the student table:

```csv
__EDUSTATS_THRESHOLD__,Mathematics,100,40
__EDUSTATS_THRESHOLD__,English,100,40
```

Each threshold row follows this format:

```text
__EDUSTATS_THRESHOLD__,<Subject>,<MaximumMark>,<FailMark>
```

## Development checks

The repository includes Node-based tests and smoke/stress checks. With Node.js installed, run these commands from the repository root:

```bash
npm test
npm run smoke
npm run test:stress
```

The optional model-training script is:

```bash
npm run train:model -- --data-dir generated-test-data --output models/edustats_v2.onnx
```

The model-training workflow requires the Python packages used by the training script, including `pandas`, `numpy`, `scikit-learn`, `skl2onnx`, and `onnxruntime`. The predictive feature is optional; the core application remains usable without a locally generated model asset.

## Data and privacy

EduStats is designed to work without an application backend. Class data and working state are stored in the browser rather than in a central EduStats account. This means:

- Data is associated with the browser and device where it was entered.
- It is not automatically synchronised between devices or browsers.
- Clearing browser storage can remove locally saved data, so keep exported copies of important records.
- The application can load external resources such as Google Fonts. Optional analytics are disabled by default and can be enabled through the application's settings.

Do not enter identifiable student information on a shared or untrusted device. Follow your school or institution's data-handling policies when working with student records.

## Project structure

```text
EduStats/
├── index.html              # Main application
├── analysis.worker.js      # Background analysis worker
├── js/core/                # Shared statistics, CSV, feature, and database modules
├── tests/                  # Unit and stress tests
├── tools/                  # Data-generation and model-training utilities
├── generated-test-data/    # Generated sample/test datasets
├── lib/                    # Self-hosted browser libraries
├── icons/                  # Application icons
└── .github/workflows/      # CI workflow
```

## Project status

EduStats is an actively developed personal project. The core workflow is available in the live application; predictive analysis is optional and depends on the availability of its model/runtime assets.

## Contributing

Suggestions, bug reports, and focused pull requests are welcome. For a change, please describe the problem it addresses, explain the implementation, and include relevant test results. When changing calculations, include examples or tests that make the statistical behaviour clear.

## License

EduStats is licensed for personal and non-commercial institutional use only. See the [`LICENSE`](LICENSE) file for the full terms. The license does not grant unrestricted commercial reuse.
