# Inland Water Body Detection: A Student-Friendly Machine Learning Project


## Table of Contents
- [Introduction](#introduction)
- [Methods](#methods)
  - [What is NDWI?](#what-is-ndwi)
  - [What is K-Means Clustering?](#what-is-k-means-clustering)
- [Study Locations](#study-locations)
- [How to Set Up](#how-to-set-up)
- [How to Use the Code](#how-to-use-the-code)
- [Results & Evaluation](#results--evaluation)
- [Environmental Impact](#environmental-impact)
- [License](#license)
- [Credits](#credits)

---

## Introduction
This project explores how satellite imagery can be used to identify and monitor inland water bodies like lakes, rivers, wetlands, and ponds. Monitoring these water features is essential for effective environmental protection, early flood warning systems, and responsible water resource planning.

We utilize data from the **Sentinel-2** satellites, which provide detailed, multi-spectral images of Earth's surface. Our detection method is based on two techniques:

- **NDWI (Normalized Difference Water Index)** — a standard method in remote sensing for detecting water by analyzing how light is reflected from the Earth's surface.
- **K-Means Clustering** — an unsupervised machine learning algorithm that can group pixels based on similarities, aided by NDWI to enhance accuracy.

By combining both methods, we improve water body detection in environments with complex features like murky waters, dense urban zones, and small, fragmented lakes.

---

## Methods

### What is NDWI?
NDWI (Normalized Difference Water Index) is a widely used indicator for identifying water bodies using satellite imagery. It compares the reflectance in the green and near-infrared (NIR) bands. The formula is:

```python
NDWI = (Green - NIR) / (Green + NIR)
```

In Sentinel-2 imagery:
- **Green = Band 3 (560nm)**
- **NIR = Band 8 (842nm)**

Water typically reflects more in the green band and less in the NIR band, so high NDWI values suggest water. We use a threshold between **0.0 and 0.3** to adapt to different conditions like turbidity or terrain.

### What is K-Means Clustering?
K-Means is a clustering algorithm that groups data points based on feature similarity. In satellite imagery, we use this technique to group pixels with similar reflectance values.

For this project, each pixel is represented using:
- Band 2 (Blue)
- Band 3 (Green)
- Band 4 (Red)
- Band 8 (NIR)
- NDWI

We use **K=2**, creating two groups: water and non-water. The group with the higher average NDWI is identified as water. This method is particularly helpful when NDWI alone isn't sufficient, such as in urban or turbid areas.

---

## Study Locations
To test our methods, we selected two diverse coastal areas in China:

1. **Locate1: Qiongzhou Strait / Beibu Gulf**
   - Characterized by high sediment concentrations and minimal agriculture. Standard NDWI struggles here due to turbid water reflectance.

2. **Locate2: Haikou Coastal Zone**
   - A densely built-up coastal area with many small lakes and shadows from urban structures, which can confuse water detection algorithms.

These contrasting regions help validate the strengths and weaknesses of both methods under real-world conditions.

---

## How to Set Up
Make sure Python and the following libraries are installed:

```bash
pip install rasterio 
opencv-python 
numpy 
scikit-learn 
matplotlib 
seaborn
```



---

## How to Use the Code

Our script follows this general workflow:

1. **Read Bands** — Load Bands B02, B03, B04, B08 from Sentinel-2.
2. **Compute NDWI** — Use Green and NIR to calculate NDWI.
3. **Create Water Masks** — Apply thresholds and save results as TIFF and JPEG.
4. **Enhance RGB Image** — Use histogram equalization to boost visibility.
5. **Run K-Means Clustering** — Identify water clusters using spectral data and NDWI.

### NDWI Sample Code
```python
ndwi = (green - nir) / (green + nir)
```

### Compare Output Images
Visualize results side by side:
```python
plt.subplot(1, 3, 1)
plt.imshow(rgb_image)
plt.title("RGB Image")

plt.subplot(1, 3, 2)
plt.imshow(ndwi_mask, cmap='gray')
plt.title("NDWI Mask")

plt.subplot(1, 3, 3)
plt.imshow(kmeans_result, cmap='gray')
plt.title("K-Means Result")
plt.show()
```

This helps highlight how both methods perform across different scenarios.

---

## Results & Evaluation
To assess method performance, we use:
- **Accuracy** — Proportion of all correctly predicted pixels.
- **Recall** — Proportion of actual water pixels correctly identified.
- **Kappa Score** — Measures reliability beyond random chance.

### Locate1
- **Recall**: 0.9994
- **Kappa**: 0.5183
- **Accuracy**: 0.7796

### Locate2
- **Recall**: 0.9971
- **Kappa**: 0.9572
- **Accuracy**: 0.9794

K-Means outperforms NDWI alone in complex environments like urban zones. Confusion matrix visualizations are included (`figure/confusion_matrix.png`) for deeper insights.

---

## Environmental Impact

### Code Efficiency
- Efficient use of Rasterio, NumPy, and OpenCV
- Minimal memory usage with vectorized operations
- Avoids unnecessary file operations where possible

### Real-World Benefits
- Enables large-scale water monitoring
- Supports disaster response and land-use planning
- Contributes to environmental sustainability through better data

### Future Improvements
- Use `MiniBatchKMeans` for faster processing
- Cache intermediate results to reduce I/O
- Deploy on cloud services using renewable energy

Even small efficiency tweaks can make remote sensing projects more environmentally sustainable.

---

## References
*Unsupervised Learning - GEOL0069 Guide Book*. (n.d.). https://cpomucl.github.io/GEOL0069-AI4EO/Chapter1%3AUnsupervised_Learning_Methods.html
AI/Machine Learning Implementation - GEOL0069 Guide Book*. (n.d.). https://cpomucl.github.io/GEOL0069-AI4EO/Chapter_1_AI_Algorithms.html
Chew, C., Shah, R., Zuffada, C., & Hajj, G. (2023). Unsupervised machine learning for GNSS reflectometry inland water body detection. Remote Sensing, *15*(12), Article 3206. https://doi.org/10.3390/rs15123206
Liu, Y., Zhang, H., Wang, X., & Chen, J. (2025). The application of remote sensing technology in inland water quality monitoring and water environment science: Recent progress and perspectives. Remote Sensing, *17*(4), Article 667. https://doi.org/10.3390/rs17040667
Rodríguez-López, L., Alvarez, D., Bustos Usta, D., et al. (2024). Chlorophyll-a detection algorithms at different depths using in situ, meteorological, and remote sensing data in a Chilean lake. Remote Sensing, *16*(4), Article 647. https://doi.org/10.3390/rs16040647
