# OCR-SAMPLES 

A handwritten math OCR pipeline that takes an image of algebra problems, detects each line, recognizes the expressions using a transformer model, and solves the equations automatically.

---

## Overview

This project runs entirely in Google Colab. Given a photo or scan of handwritten math exercises, it:

1. Preprocesses the image (grayscale, contrast enhancement, sharpening, upscaling)
2. Detects and splits individual lines using row density analysis
3. Recognizes each line with Microsoft's TrOCR model
4. Cleans and parses the recognized text
5. Solves the equations using SymPy

---

## Pipeline

```
Image Input
    │
    ▼
Preprocessing
  - Convert to grayscale
  - Enhance contrast & sharpness
  - Upscale for better OCR accuracy
    │
    ▼
Line Detection
  - Compute row pixel density
  - Find valleys below threshold (< 0.02)
  - Split image into individual line crops
    │
    ▼
OCR (TrOCR)
  - microsoft/trocr-large-handwritten
  - Each line image → raw text string
    │
    ▼
Equation Solver (SymPy)
  - Clean & normalize OCR output
  - Parse left/right sides of equation
  - Solve for x
    │
    ▼
Solutions Output
  - Solution 1: x = [21/2]
  - Solution 2: x = [38/3]
  - Solution 3: x = [9/5]
```

---

## Requirements

```
transformers
torch
Pillow
numpy
matplotlib
sympy
```

Install with:

```bash
pip install transformers torch Pillow numpy matplotlib sympy
```

---

## Usage

Open `OCR_MATH_ORIENTED.ipynb` in Google Colab and run all cells. The notebook will prompt you to upload an image of handwritten math problems.

**Expected input format:**
- Each line should contain one equation
- Format: `[number]: [expression] = [value]`  
- Example: `1: 2x + 6 = 27`

---

## Model

Uses [`microsoft/trocr-large-handwritten`](https://huggingface.co/microsoft/trocr-large-handwritten) from HuggingFace — a Vision Encoder-Decoder model fine-tuned on handwritten text recognition.

> No HuggingFace token is required to run this project, but setting one up is recommended to avoid rate limiting.

---

## Example Output

Given a handwritten image with:
```
1: 2x + 6 = 27
2: 3x - 8 = 30
3: 5x - 9 = 6
```

The pipeline outputs:
```
Solution 1: x = [21/2]
Solution 2: x = [38/3]
Solution 3: x = [9/5]
```

---

## Limitations

- Works best with clearly written, single-variable linear equations
- OCR accuracy depends on handwriting quality and image resolution
- Complex expressions (fractions, exponents) may not be parsed correctly
- Currently supports equations in the form `ax + b = c`

---

## File Structure

```
OCR-SAMPLES/
├── OCR_MATH_ORIENTED.ipynb   # Main Colab notebook
└── README.md
```
