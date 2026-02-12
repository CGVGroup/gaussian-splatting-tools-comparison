# OpenSplat — How-To (Training and Export)

This document describes the workflow used for the **OpenSplat** implementation, including:

- environment setup and installation (Windows)  
- required dependencies (LibTorch + OpenCV)  
- dataset preparation (images + COLMAP)  
- training  
- export for downstream inspection and cleaning  

---

## References

- **Official OpenSplat repository**:  
  https://github.com/pierotofy/OpenSplat

---

## 1. Prerequisites

OpenSplat is a C++/CUDA project and requires the following dependencies:

- NVIDIA GPU with **CUDA** support  
- CUDA Toolkit installed  
- **LibTorch (C++ distribution of PyTorch)**  
- **OpenCV**  
- Visual Studio (recommended: VS2019 for CUDA 11.x compatibility)  
- CMake  

---

## 2. Installation Order (Windows Setup Used)

In this setup, dependencies were installed in the following order:

1. Install **CUDA Toolkit**
2. Download and extract **LibTorch**
3. Download and build/install **OpenCV**
4. Clone and build **OpenSplat**

LibTorch and OpenCV must be available before compiling OpenSplat, as described in the official repository.

---

## 3. Windows Project Structure (Used in This Setup)

The working directory was structured as follows:

```text
C:\Users\ernes\OpenSplat_Project\
│
├── libtorch
├── opencv
└── OpenSplat
```

- `libtorch` → extracted LibTorch distribution  
- `opencv` → OpenCV installation/build  
- `OpenSplat` → compiled OpenSplat project (contains `opensplat.exe`)  

The executable `opensplat.exe` was run from inside the `OpenSplat` directory.

---

## 4. Dataset Requirements

OpenSplat expects a dataset folder containing:

- input images  
- COLMAP reconstruction data  

Expected structure:

```text
<dataset_folder>
|---images
|   |---<image 0>
|   |---<image 1>
|   |---...
|---sparse
    |---0
        |---cameras.bin
        |---images.bin
        |---points3D.bin
```

---

## 5. Training (Used in Benchmark)

Training was executed directly via the OpenSplat executable.

The following command was used:

```bash
opensplat.exe -n 30000 -s 5000 -d 2 -o "C:\Users\ernes\OpenSplat_Project\OpenSplat\results\video-interno-1.ply" "C:\Users\ernes\OpenSplat_Project\OpenSplat\data\video-interno-1-Copia"
```

### Parameter description

- `-n 30000` → number of optimization iterations (30,000 steps)  
- `-s 5000` → checkpoint / save interval (every 5,000 steps)  
- `-d 2` → image downsampling factor  
- `-o <output.ply>` → output path for exported `.ply` file  
- `<dataset_folder>` → input dataset directory (images + COLMAP data)  

---

### 5.1 Downsampling Factor

The `-d` parameter controls image downsampling.

Example values:

- `-d 1` → original resolution  
- `-d 2` → resolution divided by 2 per dimension (1/4 total pixels)  
- `-d 4` → resolution divided by 4 per dimension  
- `-d 8` → resolution divided by 8 per dimension  

In this benchmark, `-d 2` was used to reduce GPU memory usage while preserving sufficient detail.

---

## 6. Export

The final reconstruction was exported directly as a `.ply` file using the `-o` flag.

The exported `.ply` files were later inspected and cleaned using **SuperSplat** for qualitative evaluation and post-processing analysis.

---

## 7. Tested Configuration (To Be Completed)

The following software stack was used (exact versions to be documented):

- CUDA Toolkit: ______  
- LibTorch: ______  
- OpenCV: ______  
- Visual Studio: ______  

These versions will be specified for full reproducibility.

