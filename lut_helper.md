# Lookup Table (LUT) helpers
This documentation explains how to use the Python scripts in the soft-posit-cpp repo(in the `LUT_helpers` folder) to generate lookup tables (LUT) for the various mathematical functions. 
In this doc I will walk you through the function $2^x$ using the softposit library's posit<8> format, and how to integrate the generated LUT into other projects (e.g., C/C++ code).

## 1. Overview
Purpose: Efficiently compute $2^x$ for all possible posit<8> input values via a precomputed LUT.

Input: All 256 possible posit<8> bit patterns.

Output: A C-style array representing the LUT, mapping each input to its corresponding output in posit<8> format.

## 2. Prerequisites
Python 3.x

softposit Python library

NumPy

Install dependencies:

```
pip install numpy softposit
```

## 3. How the Script Works
Iterates over all 256 possible posit<8> bit patterns.

Converts each bit pattern to a posit8 number.

Handles special cases: NaR (Not a Real), infinities, and values that would result in overflows.

Computes $2^x$ for each valid input and clamps very small inputs, infinite values.

Encodes the result back into posit8 format.

Outputs a C-style array for easy inclusion in embedded or high-performance code.

## 4. Running the Script
Make sure you are in the director `LUT_helpers` and run:
```
python exp2_lut.py > exp2_posit8_lut.h
This will generate a header file (exp2_posit8_lut.h) containing the LUT.
```

## 5. Example Output
The output will look like:

```c
// Lookup Table for exp2(x) = 2^x using posit<8>

    64,   64,   65,   65,   65,   66,   66,   67,   67,   67,   68,   68,   68,   69,   69,   70,   
    70,   70,   71,   71,   72,   72,   73,   73,   73,   74,   74,   75,   75,   76,   76,   77,   
    ...
```
Each value corresponds to the output for a given 8-bit input.

## 6. Using the LUT in C/C++
a. Include the LUT

Copy the generated array into your C/C++ project, for example:

```c
const uint8_t exp2_posit8_lut[256] = {
    // ... contents from the generated file ...
};
```
b. Lookup Usage

To compute  $2^x$ for a given posit<8> input:
```c
c
#include <stdint.h>

// Assume input is a uint8_t representing posit<8>
uint8_t input = ...; // your posit<8> value
uint8_t output = exp2_posit8_lut[input];

// output is the posit<8> encoding of exp2(x) into uint8_t
```
