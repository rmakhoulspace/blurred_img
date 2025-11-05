# CUDA Image Blur (Naive & Tiled Implementations)

This project demonstrates GPU-accelerated **image blurring** using **CUDA C++**.  
It includes both a **naive per-pixel blur kernel** and an **experimental tiled shared-memory kernel**,  
using [LodePNG](https://github.com/lvandeve/lodepng) for image decoding and encoding.

---

## 🧠 Overview

The program loads an input PNG image, applies a box blur on the **RGBA** channels using CUDA,  
and saves the result as a new PNG file.

Two CUDA implementations are provided:

| Version | Description |
|----------|--------------|
| `blur_kernel.cu` | Naive implementation: each thread computes one pixel by directly reading from global memory. |
| `image_blur_tiled.cu` | Experimental tiled version using shared memory for better performance on large images. |

---

## 📁 Repository Structure

blurred_img/
├── blur_img.py
├── initial_img.png
├── blurred_size_2.png
├── tiled_blurred_size_5.png
└── README.md

## ⚙️ Build Instructions

Ensure that you have the **CUDA Toolkit** installed and `nvcc` available on your PATH.

### Compile the naive version:
```bash
nvcc -arch=sm_75 blur_kernel.cu lodepng.cpp -o blur_naive

Compile the tiled version:: nvcc -arch=sm_75 image_blur_tiled.cu lodepng.cpp -o blur_tiled

▶️ Run the Program
./blur_naive test.png blurred.png

🧮 Algorithm Summary

Each thread computes one output pixel:

For every color channel (R, G, B, A)

Averages pixels in a (2 * BLUR_SIZE + 1)² neighborhood

Writes the result to global memory

The tiled version preloads a shared-memory patch (with borders) to minimize global memory reads.
