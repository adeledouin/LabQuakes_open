# LabQuakes_open

**Multimodal analysis of avalanche dynamics in a laboratory granular fault**

*A. Douin, E. Saurety, V. Levy dit Vehel, L. Combe, L. Vanel, O. Cochet-Escartin & O. Ramos*  
*Institut Lumière Matière (ILM), UMR5306, Université Claude Bernard Lyon 1 – CNRS, Villeurbanne, France*

---

## Overview

This repository contains the data analysis pipeline associated with the paper:

> A. Douin et al., *Avalanches can increase stored energy in a granular fault.* arXiv:2609.14406 (2026).

The project studies the avalanche dynamics of a slowly sheared, compressed granular fault — a laboratory analogue of geological fault systems. The central result is that avalanches do not always relax the system: some events simultaneously dilate the granular layer and increase the elastic energy transmitted to the confining boundary. These anomalous events reorganise force chains beyond the shear band and produce a distinct acoustic signature.

The experiment provides simultaneous access to four coupled observables — torque, layer thickness, acoustic emission, and photoelastic force networks — yielding a uniquely rich multimodal dataset that reveals all four combinations of energy release or storage with contraction or dilation.

---

## Scientific context

Slowly sheared granular materials are widely used as laboratory analogues for crustal fault systems. Under quasi-static loading, mechanical energy accumulates gradually and is released intermittently through sudden structural reorganisations spanning a wide range of sizes — a dynamics that reproduces in detail the statistical laws of seismicity (Gutenberg–Richter distribution, Omori law, aftershock sequences).

The standard picture describes avalanches as relaxation events: the material releases stored elastic energy and compacts. This work challenges that picture by demonstrating the existence of a broader phenomenology — including events that **increase** stored energy and **dilate** the layer — and linking each avalanche type to a distinct signature in the force network and acoustic emission.

**Four event classes** are identified from the simultaneous response of torque (energy proxy) and layer thickness (dilation proxy):

| Class | Torque | Thickness | Physical interpretation |
|-------|--------|-----------|------------------------|
| Drop  | ↓ release | ↓ compaction | Classical relaxation event |
| Jump  | ↑ storage | ↑ dilation  | Anomalous energy-storing event |
| Q1    | ↓ release | ↑ dilation  | Intermediate regime |
| Q2    | ↑ storage | ↓ compaction | Intermediate regime |

The most striking finding is that **Jump** events reorganise force chains *beyond* the shear band — into the bulk of the granular layer — producing a qualitatively different acoustic response from classical Drop events.

---

## Installation

### Directory structure

```
~/LABQUAKES/
├── Git/
│   └── Project_Name/       ← code repositories
└── DATA/                   ← local data (or on external TB drive)
```

---

### PyCharm

Install the PyCharm IDE (free for Academic & Students):

```bash
wget https://download.jetbrains.com/python/pycharm-professional-2024.1.3.tar.gz
tar -xvf pycharm-professional-2024.1.3.tar.gz
```

Launch from terminal:

```bash
cd ~/pycharm-2024.1.3/bin/
./pycharm
```

---

### Anaconda

If not already installed, follow the official instructions at [https://docs.anaconda.com/free/anaconda/install/linux/](https://docs.anaconda.com/free/anaconda/install/linux/)

```bash
./Anaconda3-2025.06-0-Linux-x86_64.sh
```

---

### LaTeX (optional, for figure generation)

```bash
sudo apt install texlive-full
```

---

### Virtual environment setup

**1. Create a new conda environment in PyCharm:**

`Settings` → `Project: LabQuakes` → `Python Interpreter` → `Add Interpreter` → `Add Local Interpreter` → `Conda`

- Name: `project_name`
- Python version: `3.8`

> **Tip:** The active environment name should appear in your terminal prompt, e.g. `(LabQuakes) adele@anima:~$`. If not, you are running in the root OS environment.

**2. Activate the environment:**

```bash
conda activate env_name
```

**3. Install dependencies:**

```bash
pip install -r requirements.txt
```

If any package is missing:

```bash
pip install package_name
```

To save the current environment state:

```bash
pip freeze > requirements.txt
```

**4. Install specific package versions:**

```bash
pip uninstall opencv
pip install scikit_image==0.19.3        # or 0.17.2
pip install opencv-contrib-python==4.6.0.66
pip install imutils==0.5.4
pip install imageio==2.32.0
conda install scipy==1.10.1
```

**5. Install database dependencies:**

```bash
pip install duckdb
conda install -c conda-forge pytables
```

---

## Input data

### File naming convention

Raw input files follow the pattern:

```
signaltype_numerodata_ExpExpid.txt
```

Where:

- `Expid` — date of the experiment (e.g. `20231015`)
- `signaltype` — one of: `force`, `position`, `acoustic`, `signals_10k`, `signals_100k`
- `numerodata` — `1`, `2`, or empty

These files are the direct output of the MATLAB parser run after data acquisition.

### Local data structure

```
path_from_root_data/
└── DATA_Acoustics/
    └── Expid_DATA/
        ├── force_1_ExpExpid.txt
        ├── force_2_ExpExpid.txt
        ├── position_1_ExpExpid.txt
        ├── position_2_ExpExpid.txt
        ├── acoustic_1_ExpExpid.txt
        ├── acoustic_2_ExpExpid.txt
        ├── acoustic_3_ExpExpid.txt
        ├── acoustic_4_ExpExpid.txt
        ├── acoustic_5_ExpExpid.txt
        ├── acoustic_6_ExpExpid.txt
        ├── acoustic_7_ExpExpid.txt     ← or noise100k_ExpExpid.txt
        ├── signals_10k_ExpExpid.txt
        ├── signals_100k_ExpExpid.txt
        └── DATABASE/
            ├── Expid_cut_database.db
            ├── Expid_database.db
            └── Expid_force_database.db
```

Data paths are specified in `./Utils/path_from_root.py`. Raw data is imported from NAS.

---

## Configuration

**Update data and code paths** in the bash scripts before running:

```bash
#!/bin/bash

export PYTHONPATH="${PYTHONPATH}:~/LABQUAKES/Git/LabQuakes_up/"

source ~/anaconda3/etc/profile.d/conda.sh
conda activate LabQuakes

cd ~/LABQUAKES/Git/LabQuakes_up/Remote/
```

---

## Data availability

*Dataset to be deposited upon publication. This section will be updated with the OSF link and DOI.*

---

## Experimental system

The apparatus consists of a single monolayer of approximately 4,000 photoelastic disks (3D-printed in Durus White 430, 4 mm thick) confined between two concentric, fixed transparent acrylic cylinders (inner diameter 28 cm, outer 29 cm, gap 5 mm). The disks are bidisperse (diameters 6.4 mm and 7.0 mm in equal proportion) to prevent crystallisation.

The granular layer is bounded by two rough, 3D-printed rings composed of 99 half-cylinders. The lower ring is driven by a stepper motor reduced by a factor of 2200, producing quasi-static shear at a linear velocity of ~0.13 mm/h. The upper ring is free to move vertically but prevented from rotating, and compresses the layer under a dead load of ~20 kg.

**Four simultaneous measurement channels:**

- **Torque** — measured on the upper ring via a torque meter; proxy for elastic energy stored in the system
- **Layer thickness** — vertical displacement of the upper ring; proxy for dilation and compaction of the granular layer
- **Acoustic emission** — piezoelectric sensors detecting the acoustic waves generated by grain rearrangements
- **Photoelastic imaging** — images of the disk layer under circular polarised light, revealing the internal stress distribution and force chain network

The translucent and photoelastic properties of the Durus material allow visualisation of the stress state of each disk when the setup is placed between two circular polarisers.

---

## Relation to the LabQuakes project

This repository is part of the broader **LabQuakes** experimental programme led by O. Ramos at ILM Lyon, which uses this granular fault apparatus to study seismic-like dynamics over a wide range of scales. The same experimental platform was used in:

> Lherminier, S. et al. *Continuously sheared granular matter reproduces in detail seismicity laws.* **Physical Review Letters** 122, 218501 (2019).

The present work extends the analysis to the **multimodal** characterisation of individual avalanche types, going beyond the statistical description of event sizes to address the physical mechanisms that distinguish them.

A companion preprint from the same group addresses the predictability and memory effects in the same system:

> Duplat, K., Douin, A. et al. *OFC-like behavior in experimental granular piles.* arXiv:2609.14166 (2026).

---

## Related repositories

|Repository|Description|
|---|---|
|[LabQuakes_open](https://github.com/adeledouin/LabQuakes_open)|Data analysis pipeline for the LabQuakes experimental results (arXiv:2609.14406)|
|[KnitAnalyse_open](https://github.com/adeledouin/KnitAnalyse_open)|Analysis pipeline for seismic-like events in knitted fabrics|
|[KnitQuakesForecast_open](https://github.com/adeledouin/KnitQuakesForecast_open)|Prediction and KnitCity RL evaluation framework|

## Citing

If you use this code or dataset, please cite:

> A. Douin, E. Saurety, V. Levy dit Vehel, L. Combe, L. Vanel, O. Cochet-Escartin & O. Ramos.
> **Avalanches can increase stored energy in a granular fault.**
> arXiv:2609.14406 (2026). [https://arxiv.org/abs/2609.14406](https://arxiv.org/abs/2609.14406)

```bibtex
@article{douin2026labquakes,
  title   = {Avalanches can increase stored energy in a granular fault},
  author  = {Douin, Ad\`ele and Saurety, E. and Levy dit Vehel, V.
             and Combe, L. and Vanel, L. and Cochet-Escartin, O. and Ramos, O.},
  journal = {arXiv preprint},
  year    = {2026},
  eprint  = {2609.14406},
  url     = {https://arxiv.org/abs/2609.14406}
}
```

---

## License

This code is released under the **MIT License** (see `LICENSE`).
The associated dataset is released under **CC-BY 4.0** (upon deposit).
