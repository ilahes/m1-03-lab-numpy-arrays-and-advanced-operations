![logo_ironhack_blue 7](https://user-images.githubusercontent.com/23629340/40541063-a07a0a8a-601a-11e8-91b5-2f13e4e6b441.png)

# Lab | NumPy Arrays and Advanced Operations

## Overview

This lab guides you through numerical analysis using NumPy arrays, vectorized operations, and advanced features: broadcasting, partial sorting, and controlled randomness. You will work in two parts. First, you will clean and summarize synthetic sensor data using arrays and vectorization. Then, you will work with simulated scenario data using broadcasting, sorting, partitioning, and reproducible random generation. The emphasis is on correct reasoning about shapes, reproducibility, and building a single coherent workflow from basics to advanced operations.

## Learning Goals

By the end of the lab, you should be able to:

- Create and inspect NumPy arrays and apply vectorized transformations.
- Use boolean masks for filtering and compute summary statistics along the correct axis.
- Apply broadcasting across compatible shapes and use sorting and partitioning to rank results.
- Generate reproducible random samples and validate outcomes with shape and value checks.
- Build validation reports and verify that array operations produce expected shapes and values.

## Setup and Context

You will generate synthetic data directly in code: first sensor-style readings, then simulation-style outcomes. Use a fixed random seed so that results are reproducible. Work in a single notebook named `m1-03-numpy-arrays-and-advanced-operations-lab.ipynb`.

## Requirements

- Fork this repository to your own GitHub account.
- Clone your fork to your machine.
- Make sure you can open and run Jupyter notebooks (for example via Jupyter Lab or VS Code).

## Getting Started

- Create a new notebook and name it `m1-03-numpy-arrays-and-advanced-operations-lab.ipynb`.
- Set a fixed random seed exactly as requested so your results are reproducible.
- Before you submit, restart your kernel and run the notebook **top to bottom**.

---

## Part 1: Sensor Data and Vectorization

### Task 1: Generate a reproducible sensor matrix

Create a 2D NumPy array named `readings` with shape `(360, 4)` to represent six hours of minute-level readings from four sensors. Use a fixed seed and generate values that roughly follow a normal distribution around 50 with some variation. After creating the array, print its shape, dtype, and the first three rows to confirm the structure.

### Task 2: Apply vectorized transformations

Create a new array `scaled_readings` that rescales the original readings to a 0–1 range using vectorized operations. Do not use loops. Then create a second array `centered_readings` by subtracting the column means from the original readings. Verify both arrays have the same shape as the original.

### Task 3: Filter outliers with boolean masks

Define outliers as any reading above 80 or below 20. Build a boolean mask that identifies outliers across the full matrix. Use the mask to compute two values: the count of outlier readings and the percentage of total readings that are outliers. Then create a cleaned array `cleaned_readings` where outliers are replaced with `np.nan`. Confirm that the number of `np.nan` values matches your outlier count.

### Task 4: Compute summaries by sensor and over time

Compute the mean and standard deviation for each sensor using axis-based aggregation. Store the results in two 1D arrays named `sensor_means` and `sensor_stds`. Then compute the average reading at each time step across sensors and store it in a 1D array named `time_means`. Validate shapes: the sensor summaries should have length 4, and the time summary should have length 360.

### Task 5: Build a validation report (Part 1)

Create a short report dictionary with keys `total_readings`, `outlier_count`, `outlier_percent`, `sensor_means`, and `sensor_stds`. Convert this report into a readable string and print it in the notebook. Include at least one simple validation, such as confirming that `total_readings` equals `readings.size`.

---

## Part 2: Simulation, Broadcasting, and Randomness

### Task 6: Create a reproducible simulation matrix

Initialize a random number generator with a fixed seed. Create a 2D array named `sim` with shape `(1000, 6)` representing 1000 trials for 6 scenarios. Use a normal distribution with mean 0 and standard deviation 1. After creating the array, print its shape, dtype, and the first three rows to confirm the structure.

### Task 7: Apply broadcasting for scenario adjustments

Create a 1D array named `scenario_shift` of length 6 containing small offsets such as `[-0.3, -0.1, 0.0, 0.1, 0.2, 0.4]`. Use broadcasting to add these offsets to `sim` and store the result in `adjusted_sim`. Verify that `adjusted_sim` has the same shape as `sim` and that the mean of each column changes in the direction of the offsets.

### Task 8: Rank scenarios with sorting and partitioning

Compute the mean outcome per scenario from `adjusted_sim` and store it in a 1D array named `scenario_means`. Use `np.argsort` to get the ranking of scenarios from lowest to highest. Then use `np.partition` to identify the top two scenario means without fully sorting the array. Confirm that the top two values from `partition` match the two largest values from a full sort.

### Task 9: Controlled randomness and reproducibility

Generate a second simulation matrix `sim_2` using the same seed and confirm that it matches `sim` exactly. Then generate a third matrix `sim_3` with a different seed and confirm that it differs. Include a short check such as comparing equality counts or using `np.allclose` to verify the difference.

### Task 10: Build a validation report (Part 2)

Create a small report dictionary with keys `shape`, `scenario_means`, `top_two_indices`, and `top_two_values`. Convert this report into a readable string and print it. Include at least one validation statement, such as confirming that `scenario_means` has length 6 and that the `top_two_indices` list has length 2.

---

## Common Pitfalls and Debugging Notes

- **Axes and shapes:** Mixing up axes when computing summaries is a frequent mistake. If your sensor or scenario means have the wrong length, check whether you aggregated along the correct axis. Use new variable names for each transformation step to keep the workflow clear.
- **Outlier filtering:** Ensure your boolean mask uses elementwise comparisons and combines conditions with `|` rather than `or`. If your outlier count seems off, inspect a small subset of the data and the mask together.
- **Broadcasting:** Errors often come from mismatched shapes. If a broadcast fails, print the shapes of both arrays and ensure the smaller array can expand along the correct dimension.
- **Sorting vs partitioning:** Remember that `partition` does not fully sort the array; only the selected positions are guaranteed. Do not assume the rest of the array is ordered.
- **Reproducibility:** If results do not match when reusing a seed, reinitialize the generator with the same seed before creating a new array. Unintentionally modifying the original array when you meant to create a new one can also cause confusion—prefer explicit copies when needed.

## Submission

### What to submit

Submit the following files:

- The notebook file `m1-03-numpy-arrays-and-advanced-operations-lab.ipynb`

### Definition of done (checklist)

Before you submit, make sure:

- [ ] The notebook runs **top to bottom** without errors after a kernel restart.
- [ ] All requested arrays/variables exist with the expected shapes (for example `(360, 4)`, `(1000, 6)`, length 4, length 360, length 6).
- [ ] You used vectorized NumPy operations (no loops where explicitly disallowed).
- [ ] Your validation checks are included (shape checks, `readings.size`, reproducibility comparisons, partition vs sort consistency).
- [ ] The final notebook is saved and included in your git commit.

### How to submit (Git workflow)

- When you’re done, make sure all changes are saved, then run:

```bash
git add .
git commit -m "Solved m1-03 lab"
git push -u origin HEAD
```

- Make a pull request.
- Paste the link to your pull request in the Student Portal.

## Evaluation Criteria

Your work will be evaluated on correctness, clarity, and structure. Correctness means your array operations produce the expected shapes and values, and your validation checks are logically consistent. Clarity means your code is readable and uses descriptive variable names. Structure means the notebook runs cleanly from top to bottom with Part 1 and Part 2 completed in order. The overall expectation is a single, professional NumPy workflow that uses vectorized operations, broadcasting, sorting, partitioning, and controlled randomness appropriately.
