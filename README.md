# Maya Production Pipeline — WORKSPACE

**A full in-house Maya production pipeline for Modelling, Surfacing, and Groom departments.**  
Built and maintained by [Bharti Chavan](https://mythosnorth.ca) · Vancouver, BC

> All tools are authored in Python with PyQt/PySide interfaces, designed around asset validation, metadata conventions, and QA-gating that protects downstream department workflows.

---

## Overview

This repository contains the WORKSPACE pipeline toolset — a suite of integrated Maya tools built to handle the full asset lifecycle across Modelling, Surfacing, and Groom departments. Each tool is designed to work as a standalone utility and as part of the connected pipeline.

Also included: **GFBuild**, a proprietary Maya plugin for procedural groom and foliage construction.

---

## Pipeline Tools — WORKSPACE

### 1. Look-Dev Surfacing Tool
A four-tab Maya interface covering the full surfacing workflow.

- Shader preset creation, assignment, and library management
- Per-asset texture path binding with validation
- Lookdev preview controls with Arnold IPR integration
- Batch shader assignment across asset hierarchies

### 2. Arnold Standin Exporter
Exports optimised Arnold standin (`.ass`) files for LOD and instancing workflows.

- Single and batch export modes
- LOD level configuration per asset
- Automatic output path resolution based on asset metadata
- Validation pass before export to catch naming and topology issues

### 3. Mesh & Texture Renamer
Enforces naming conventions across geometry and texture networks.

- Regex-based renaming with preview before commit
- Supports hierarchy renaming (shape nodes, transform nodes, shading groups)
- Texture file node batch rename with path preservation
- Undo-safe — full rollback on error

### 4. Universal Path Remapper
Remaps texture and file paths across a scene or asset batch — handles studio-to-studio handoffs, drive letter changes, and project root migrations.

- Pattern-based find/replace with regex support
- Dry-run mode — preview all changes before writing
- Logs all remapped paths to a session file
- Works across all Maya file node types (aiImage, file, mentalrayTexture, etc.)

### 5. Edge Loop Optimizer
Reduces edge loop density on production meshes while preserving silhouette and UV integrity.

- Two-step ring-then-loop selection using `polySelectSp` (accounts for Maya's non-spatial edge index ordering)
- Configurable reduction ratio per loop
- UV seam detection — skips loops that would break UV shells
- Non-destructive — original mesh preserved until commit

### 6. Paint Effects Parameter Copier
Copies Paint Effects brush parameters between strokes with full ramp sub-attribute support.

- Copies all brush attributes including ramp channels (`Position`, `FloatValue`, `Interp`)
- Handles the `brush.attrName[i].attrName_*` sub-attribute naming pattern
- Batch copy from source stroke to multiple targets
- Works with both attached and detached Paint Effects strokes

---

## GFBuild — Groom & Foliage Build Plugin

**GFBuild** is a Maya plugin for procedural groom and foliage construction, covering the full curve-to-strand pipeline.

### Modules

#### Extract Tab
- Curve extraction from geometry surface normals
- Guide curve generation with configurable density and distribution
- Curve-based guide connectivity for hair, fur, and grass simulation setups
- Outputs guide curves compatible with Xgen, Yeti, and nHair workflows

#### Build Tab
- **Tube generation** — polygon tubes along guide curves with configurable cross-section resolution
- **Card generation** — billboard cards oriented to guide curve tangents
- **Twist controls** — parallel transport frame implementation for twist-free tube deformation
- **Braid workflows** — *(in development)*

#### Simulation Integration
- nHair dynamic wire animation parameter setup direct from the UI
- XGen simulation parameter integration — connects guide curves to XGen's internal simulation controls for character interaction workflows
- Vellum-ready curve output for cloth-groom interaction setups

### Technical Notes
- Mesh generation uses `polyCreateFacet` / `polyUnite` (avoids `MFnMesh` `kInvalidParameter` errors on complex topology)
- Twist uses parallel transport frames to eliminate the DAG path mismatch issue with naive cross-product twist
- UI callbacks use `evalDeferred` for Maya UI rebuild safety

---

## Repository Structure

```
WORKSPACE/
├── surfacing/
│   ├── look_dev_tool.py
│   ├── standin_exporter.py
│   ├── mesh_texture_renamer.py
│   └── path_remapper.py
├── modelling/
│   ├── edge_loop_optimizer.py
│   └── paint_effects_copier.py
└── groom_foliage/
    └── GFBuild/
        ├── curve_tools.py
        ├── curve_extract_tools.py
        └── strand_builder.py
```

---

## Requirements

- Autodesk Maya (tested across recent production versions)
- Python 3 (Maya's bundled interpreter)
- PyQt5 or PySide2 / PySide6
- Arnold for Maya (for standin export and surfacing tools)

---

## Status

| Tool | Status |
|---|---|
| Look-Dev Surfacing Tool | ✅ Production ready |
| Arnold Standin Exporter | ✅ Production ready |
| Mesh & Texture Renamer | ✅ Production ready |
| Universal Path Remapper | ✅ Production ready |
| Edge Loop Optimizer | ✅ Production ready |
| Paint Effects Parameter Copier | ✅ Production ready |
| GFBuild — Extract Tab | ✅ Working |
| GFBuild — Build Tab (tubes, cards, twist) | ✅ Working |
| GFBuild — Braid | 🔧 In development |
| GFBuild — Simulation Integration | 🔧 In development |

---

## About

Built by **Bharti Chavan** — VFX artist and Pipeline TD with 10+ years of production experience across Netflix, Disney+, and feature film productions at Mainframe Studios, Double Negative, ICON Creative Studios, and Prana Studios.

- 🌐 [mythosnorth.ca](https://mythosnorth.ca)
- 🎨 [artstation.com/cbharti](https://artstation.com/cbharti)
- 📧 bchavanca@gmail.com
