# UE Performance Analyzer

**Robust CSV analyzer for Unreal Engine profiler exports with statistical performance metrics**

[![Python](https://img.shields.io/badge/Python-3.7+-blue.svg)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

## Overview

This tool provides **scientifically rigorous performance analysis** for Unreal Engine CSV profiler exports. It addresses critical data integrity issues in UE performance measurement by implementing robust CSV parsing and calculating statistically valid performance metrics.

**Author:** Marvin Schubert  
**Version:** 1.0.0  
**Date:** September 2025

## Scientific Background

### Problem Statement

Unreal Engine's built-in profiler generates CSV exports with **variable column counts** (typically 372-376 columns), causing standard pandas CSV parsers to skip "malformed" lines. This results in **massive data loss** (up to 90% of frames) and renders statistical analysis invalid.

### Solution Approach

This tool implements a **robust parsing algorithm** that:
1. **Handles variable column counts** through manual line-by-line processing
2. **Normalizes row lengths** to match header specifications
3. **Preserves data integrity** by loading complete datasets (1600+ frames vs. 12-154 with standard methods)

### Statistical Methodology

The tool calculates performance metrics according to established standards:

- **95th Percentile (p95)**: `numpy.percentile(data, 95)` - Critical metric for frame consistency analysis
- **Arithmetic Mean**: `pandas.Series.mean()` - Central tendency measurement
- **Frame Rate**: `1000 / frametime_ms` - Standard FPS calculation from frame timing data

## Features

- 🛡️ **Robust CSV Processing**: Handles UE's inconsistent column count exports
- 📊 **Statistical Analysis**: Calculates p95 percentiles and means for performance metrics
- 🔍 **Flexible Column Detection**: Automatically maps varying UE column names to metrics
- 📈 **Automated Parsing**: Extracts scene/variant/run information from filenames
- 📋 **Professional Reporting**: Generates formatted Excel reports with German locale
- ⚡ **High Data Integrity**: Loads 10x+ more data compared to standard CSV parsers

## Performance Metrics

### Frame Timing Metrics
- **Frametime Mean** (ms): Average frame rendering time
- **Frametime p95** (ms): 95th percentile frame time (frame consistency indicator)
- **FPS Mean**: Average frames per second

### GPU Performance Metrics
- **GPU Time Mean** (ms): Average GPU processing time
- **GPU Time p95** (ms): 95th percentile GPU time
- **Draw Calls**: Average number of draw calls per frame
- **Primitives**: Average number of rendered primitives

### Memory Metrics
- **Local VRAM** (MB): GPU memory usage
- **Shader Memory** (MB): Shader compilation memory usage

## Installation

### Prerequisites
```bash
pip install pandas numpy openpyxl
```

### Dependencies
- **Python 3.7+**
- **pandas ≥ 2.0**: DataFrame operations and statistical calculations
- **numpy ≥ 1.20**: Percentile calculations and numerical operations
- **openpyxl ≥ 3.1**: Excel file generation and formatting

## Usage

### 1. Data Preparation
```bash
# Create input directory
mkdir messungen

# Place UE CSV exports in the directory
# Expected naming pattern: EXP_[Scene]_[Variant]_Messung_[Run].csv
# Example: EXP_1_A_Messung_1.csv
```

### 2. Execution
```bash
python messung_auswertung.py
```

### 3. Output
The tool generates:
- **Excel report**: `messungen_auswertung.xlsx`
- **Separate worksheets** for each scene/variant combination
- **Statistical summaries** with p95 and mean values
- **German-formatted numbers** (comma as decimal separator)

## Input Data Format

### CSV File Requirements
- **Source**: Unreal Engine Profiler → Export to CSV
- **Naming Convention**: `EXP_[SceneNumber]_[A|B]_Messung_[RunNumber].csv`
- **Content**: Performance profiling data with frame timing, GPU metrics, draw calls, etc.

### Supported Column Variations
The tool automatically detects these column name patterns:
- **Frame Time**: `FrameTime (ms)`, `Frame Time (ms)`, `FrameTime`, etc.
- **GPU Time**: `GPU (ms)`, `GPUTime (ms)`, `GPU Time (ms)`, etc.
- **Draw Calls**: `Draw Calls`, `RHI Draw Calls`, `DrawCalls`, etc.
- **Memory**: `RHI GPU Memory (MB)`, `LocalUsedMB`, etc.

## Scientific Validation

### Data Integrity Verification
```
Standard pandas parsing: 12-154 frames loaded (< 10% of data)
Robust parsing method: 1,677-1,683 frames loaded (> 99% of data)
```

### Statistical Significance
- **Minimum sample size**: 400+ frames per measurement
- **p95 calculation**: Based on complete dataset for valid percentile estimation
- **Error handling**: Graceful handling of missing or corrupted data points

## Output Example

```
Processing 3 CSV files...
    Robustly loaded: 1677/1677 lines (Separator: ',', 0 skipped)
  ✓ EXP_1_A_Messung_1.csv -> Scene 1, Variant A, Run 1
    ✓ All metrics found
    
Excel report created: messungen_auswertung.xlsx
```

### Excel Report Structure
| Metric | Run 1 | Run 2 | Run 3 |
|--------|-------|-------|-------|
| N | 1.677 | 1.677 | 1.683 |
| Frametime Ø [ms] | 11,947 | 11,942 | 11,903 |
| Frametime p95 [ms] | 13,426 | 13,331 | 13,542 |
| FPS Ø [#] | 83,702 | 83,738 | 84,011 |

## Technical Implementation

### Architecture
- **Modular design**: Separate functions for parsing, analysis, and reporting
- **Error handling**: Comprehensive exception management with fallback methods
- **Type safety**: Full type hints for maintainability
- **Documentation**: Extensive docstrings following scientific documentation standards

### Algorithm Details
1. **Header Detection**: Scans first 200 lines for frame-related keywords
2. **Separator Detection**: Auto-detects comma vs. semicolon separators
3. **Column Normalization**: Extends/truncates rows to match header length
4. **Statistical Processing**: Converts to numeric with robust error handling
5. **Report Generation**: Creates formatted Excel with proper styling

## Contributing

Contributions are welcome! Please ensure:
- **Scientific rigor** in statistical calculations
- **Data integrity** preservation in any parsing modifications
- **Comprehensive testing** with various UE export formats
- **Clear documentation** of changes and their scientific rationale

## License

MIT License - see [LICENSE](LICENSE) file for details.

## Citation

If you use this tool in academic research, please cite:

```bibtex
@software{schubert2025ue_analyzer,
  author = {Schubert, Marvin},
  title = {UE Performance Analyzer: Robust CSV Analysis for Unreal Engine Profiler Data},
  year = {2025},
  version = {1.0.0},
  url = {https://github.com/username/ue-performance-analyzer}
}
```

## Contact

**Marvin Schubert**  
For questions regarding scientific methodology or technical implementation.

---

*This tool was developed as part of a Bachelor's thesis research project focused on performance analysis methodologies for real-time rendering systems.*
