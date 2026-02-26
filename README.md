# Gaussian Splatting Tools Comparison

- Author: Ernesta Maria Sichetti

This repository provides a structured overview and experimental analysis of Gaussian Splatting training pipelines and visualization environments.

The project is organized around two complementary dimensions:

- **Overview** — descriptive documentation of selected Gaussian Splatting tools
- **Analysis** — controlled experimental evaluation of selected open-source trainers and XR viewers

---

## Repository Structure

The repository is organized into four main sections:

### `overview/`

Descriptive overview of Gaussian Splatting tools.

Includes:

- `trainers.md` — Overview of open-source and commercial Gaussian Splatting trainers  
- `viewers.md` — Overview of desktop, web-based, and XR Gaussian Splatting viewers  

This section provides contextual and comparative descriptions of tools within the ecosystem.

Commercial trainers and desktop/web-based viewers are documented at overview level only and are **not included in the controlled experimental analysis**.

---

### `analysis/`

Controlled experimental evaluation conducted under fixed and reproducible conditions.

This section combines structured visual analysis and quantitative measurements.

#### `analysis/trainers/`

Technical benchmark and visual analysis of selected **open-source trainers**, including:

- indoor and outdoor benchmarks
- quantitative metrics (model size, Gaussian count, training time)
- raw vs cleaned reconstruction quantitative evaluation
- structured visual analysis before and after post-processing
- cross-scenario comparison (indoor vs outdoor)

#### `analysis/viewers/`

Visual and performance analysis of XR viewers using the cleaned reconstructions produced in the trainer benchmark.

Includes:

- qualitative XR visual inspection  
- cross-viewer comparison  
- runtime performance measurements (FPS and frame timing) conducted on the selected XR viewer  

This section evaluates how cleaned reconstructions behave in immersive environments.

---

### `how-to/`

Reproducibility and execution guides.

Divided into:

- `how-to/trainers/` — Setup and execution instructions for each open-source trainer included in the benchmark  
- `how-to/viewers/` — Setup and execution instructions for each XR viewer evaluated  

Each file contains:

- requirements
- installation notes
- documentation links

---

### `media/`

Supplementary visual material supporting the analyses.

---

## Methodological Scope

The repository distinguishes clearly between:

### Tool Overview

An overview of selected Gaussian Splatting tools, including:

- open-source trainers
- commercial trainers
- desktop viewers
- web-based viewers
- XR viewers

This section does not include controlled benchmarking.

### Experimental Analysis

A structured evaluation limited to:

- selected open-source training pipelines
- two Unity-based XR viewers

All experimental analyses are conducted using:

- fixed datasets
- consistent optimization settings
- standardized cleaning workflow
- identical hardware configuration
- exported `.ply` files for inspection and XR loading

---

## Experimental Workflow

The evaluation follows a structured pipeline:

Training → Cleaning → Structural Analysis → XR Visualization → Runtime Evaluation

This separation enables independent assessment of:

- reconstruction structure and density
- post-processing impact
- immersive rendering stability
- runtime performance behavior

---

## How to Navigate the Repository

- Tool overview → `overview/`
- Trainer benchmark and visual analysis → `analysis/trainers/`
- XR viewer visual and performance analysis → `analysis/viewers/`
- Reproduction guides → `how-to/`

---

## Purpose

This repository provides a structured framework to examine selected Gaussian Splatting tools across different stages of the workflow.

It integrates descriptive overviews and experimental analyses to connect tool documentation , reconstruction inspection and post-processing, immersive visualization and runtime behavior.

---
