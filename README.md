# Unveiling the Hidden World: Real-World Camera Calibration and Uncertainty Analysis

This repository contains the Python notebook and associated files for a computer vision project focused on calibrating real-world cameras. The core of this project is to determine a camera's precise 3D geographic location (latitude, longitude, altitude) and intrinsic properties (calibration matrix K) from 2D image data.

A key differentiator of this project is its two-part structure: first, a rigorous uncertainty analysis using synthetic data, and second, a practical application to find the location of live webcams.

###  companion Blog Post

For a detailed narrative, insights into the methodology, and a full discussion of the project's challenges and findings, please read the companion article on Medium:

**[Unveiling the Hidden World: Calibrating Real-World Cameras and Understanding Their...](https://medium.com/@anaswara.raghuthaman/blog-post-unveiling-the-hidden-world-calibrating-real-world-cameras-and-understanding-their-511e475f8cba)**

---

## Table of Contents
* [Core Objectives](#core-objectives)
* [Tools & Technologies](#tools--technologies)
* [Project Methodology](#project-methodology)
    * [Part 1: Uncertainty Analysis with Synthetic Data](#part-1-uncertainty-analysis-with-synthetic-data)
    * [Part 2: Real-World Camera Calibration](#part-2-real-world-camera-calibration)
* [How to Use This Repository](#how-to-use-this-repository)
* [Expected Outputs](#expected-outputs)

---

## Core Objectives

* **Estimate Camera Pose:** Determine the camera's 3D position (latitude, longitude, altitude), orientation, and its intrinsic calibration matrix (K).
* **Uncertainty Analysis:** Quantify the reliability and robustness of the camera parameter estimations using statistical methods.
* **Real-World Application:** Apply these calibration techniques to determine the 3D locations of live webcams.

## Tools & Technologies

* **Python Libraries:** `pandas`, `matplotlib`, `pyproj`, `pymap3d`, `numpy`, `cv2` (OpenCV).
* **Geospatial Software:** [Google Earth Pro](https://earth.google.com/) (for manual 3D coordinate extraction from real-world scenes).

## Project Methodology

This project is divided into two main parts:

### Part 1: Uncertainty Analysis with Synthetic Data

This section uses a pre-provided dataset of 3D-to-2D point correspondences to perform an in-depth uncertainty analysis and validate the model's stability.

* **1a) Leave-One-Out (LOO) Analysis:**
    * **Objective:** To assess the sensitivity of the estimated camera location to individual data points.
    * **Method:** The calibration is run 'N' times, each time excluding one of the 'N' data points.
    * **Output:** A 3D scatter plot of the 'N' recovered camera locations, visually illustrating the estimation's stability.

* **1b) Noise Analysis:**
    * **Objective:** To evaluate the robustness of the camera parameters (location and focal length) under simulated real-world noise.
    * **Method:** The calibration is run 100 times. In each iteration, Gaussian zero-mean noise is added to both the 3D coordinates (std dev 1m) and 2D pixel locations (std dev 1px).
    * **Output:** A 3D scatter plot of the 100 recovered camera locations and a histogram of the estimated focal lengths.

### Part 2: Real-World Camera Calibration

This section applies the validated methodology to determine the 3D geographic locations of live webcams.

* **Methodology:**
    1.  **Select Webcams:** Identify live streams (e.g., [YouTube](https://www.youtube.com/watch?v=5maZNtsWzeY), NPS webcams).
    2.  **Landmark Identification:** Find distinct, static features visible in the webcam and on satellite imagery.
    3.  **3D Coordinate Extraction:** Use Google Earth Pro to find the precise latitude, longitude, and absolute altitude of these landmarks.
    4.  **2D Coordinate Extraction:** Record the corresponding (x, y) pixel coordinates for each landmark from the webcam image.
    5.  **Data Compilation:** Aggregate the 3D and 2D points into a structured CSV file (e.g., `bostonharbor.csv`).
    6.  **Calibration & Validation:** Run the Python calibration script on the real-world data and generate a **Reprojection Plot** to visually validate the accuracy by projecting the 3D points back onto the image.
    7.  **Refinement:** Iteratively refine landmark selection based on the reprojection error.

## How to Use This Repository

1.  **Clone the repository:**
    ```bash
    git clone [YOUR_REPOSITORY_URL]
    ```
2.  **Install dependencies:**
    ```bash
    pip install pandas matplotlib pyproj pymap3d numpy opencv-python
    ```
3.  **Run the notebook:**
    * Open `calibrate.ipynb` (or your notebook's name) in a Jupyter environment or Google Colab.
4.  **Part 1 (Uncertainty Analysis):**
    * Ensure the synthetic data CSV is present.
    * Run the cells in the "Part 1" section to generate the LOO and noise analysis plots.
5.  **Part 2 (Real-World Calibration):**
    * Follow the methodology in "Part 2" to create your own CSV file.
    * Update the notebook to load your CSV file.
    * Run the calibration and validation cells to find the camera's location and see the reprojection plot.

## Expected Outputs
* A 3D point cloud visualizing the Leave-One-Out (LOO) analysis.
* A 3D point cloud and histogram for the Noise Analysis.
* For Part 2, a table of collected 3D/2D coordinates, the final recovered 3D GPS location (lat, long, alt) for the webcam, and a reprojection plot showing the calibration accuracy.
