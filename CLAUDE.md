# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a Claude Code plugin marketplace for life sciences tools. It provides:
- **MCP Servers**: Remote and local servers connecting to PubMed, BioRender, Benchling, Synapse, 10x Genomics, and Wiley Scholar Gateway
- **Skills**: Domain-specific workflows (currently single-cell RNA-seq QC)

## Architecture

```
life-science/
├── .claude-plugin/marketplace.json  # Marketplace manifest listing all plugins
├── <plugin-name>/
│   └── .claude-plugin/plugin.json   # Individual plugin configuration
└── single-cell-rna-qc/
    ├── SKILL.md                     # Skill definition and usage
    ├── scripts/                     # Python implementation
    │   ├── qc_analysis.py           # Main entry point (complete pipeline)
    │   ├── qc_core.py               # Modular QC utility functions
    │   └── qc_plotting.py           # Visualization functions
    └── references/                  # Reference documentation
```

### Plugin Types

1. **Remote MCP Servers** (no local code): pubmed, biorender, synapse, wiley-scholar-gateway
   - Configuration in `plugin.json` points to external HTTP endpoints

2. **Local MCP Servers** (MCPB format): benchling, 10x-genomics
   - Configuration points to `.mcpb` bundle URLs

3. **Skills**: single-cell-rna-qc
   - Contains Python scripts + SKILL.md defining the workflow

## Single-Cell RNA-seq QC Skill

The skill follows scverse best practices for quality control. Key components:

**Entry Point**: `single-cell-rna-qc/scripts/qc_analysis.py`
```bash
python3 scripts/qc_analysis.py input.h5ad  # or .h5 for 10X format
```

**Modular Functions** (in `qc_core.py`):
- `calculate_qc_metrics()` - Calculates MT/ribo/Hb percentages
- `detect_outliers_mad()` - MAD-based outlier detection
- `apply_hard_threshold()` - Hard cutoff filtering
- `filter_cells()` / `filter_genes()` - Apply filters

**Python Dependencies**: anndata, scanpy, scipy, matplotlib, seaborn, numpy

**Default MAD Thresholds**: counts=5, genes=5, MT%=3, hard MT threshold=8%

## Releases

Releases are automated via `.github/workflows/release.yml`. Push a `v*` tag to create a draft release with the single-cell-rna-qc skill zipped.
