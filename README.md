# Introduction to Bioimage Analysis: 3D Segmentation Workshop

Welcome to the Bioimage Analysis workshop! This 4-hour practical introduces 3D image segmentation and quantification using Napari, CellPose, and Python-based image analysis tools.

## Workshop Overview

This workshop is divided into two sections, each beginning with a theoretical lecture:

### Section 1: Segmentation Fundamentals (2 hours)
- **Theoretical Lecture** (30 minutes): Principles of image segmentation, thresholding methods, and morphological operations
- **Practical: Napari-based Segmentation** (90 minutes): Hands-on segmentation using Napari with the segmentation assistant
  - See [Napari Segmentation Guide](./segmentation_napari.md) for detailed instructions

### Section 2: Advanced Segmentation Methods (2 hours)
- **Theoretical Lecture** (30 minutes): Deep learning-based segmentation and CellPose overview
- **Practical: CellPose Segmentation** (90 minutes): Practical application of CellPose for automated cell segmentation
  - See [CellPose Segmentation Guide](./segmentation_cellpose.md) for detailed instructions

## Lecture Materials

Download the presentation slides and additional resources from **[Zenodo](https://zenodo.org/records/18717816)**.

## Environment Setup

### Prerequisites

Install [Pixi](https://pixi.sh/) - a package manager for Python environments.

### Installation

1. Clone or download this repository
2. Open a terminal in the repository directory
3. Install the environment:
   ```bash
   pixi install
   ```

### Download data

Downloaded the dataset with `pixi run download-data`.

## Running the Workshop

### Start Napari with the Assistant Plugin

```bash
pixi run napari
```

This launches Napari with the segmentation assistant panel that guides you through the workflow.

## Workflow Steps

### 1. Open a 3D Stack

- Launch Napari using `pixi run napari`
- Use `File → Open File` to load your 3D image (e.g., `Lund.tif`)
- Toggle between 2D and 3D views using the button in the lower left panel
- Use the axis order button to create orthogonal views

### 2. Background Removal

The assistant suggests preprocessing operations in the appropriate order:
- Select `Remove Background → White Top Hat`
- Set radius to 10 pixels (adjust based on your image)
- This removes uneven illumination and background

### 3. Binarization

Convert the intensity image to a binary mask:
- Select `Binarize → Threshold Yen`
- Make sure to select the **Result of White top-hat image** as input
- This creates a binary mask of foreground objects

### 4. Connected Component Labeling

Convert binary mask to individual labeled objects:
- Select `Label → Connected Component Labeling`
- Select the **Result of Threshold image** as input
- Enable `exclude on edges` option to remove partial objects touching borders
- This assigns unique labels to each connected region

### 5. Refine Labels (Optional but Recommended)

Objects may be stuck together. Refine them using morphological operations:

**Step 5a: Erosion**
- Select the binary image from step 3
- Go to `Process Labels → Binary Erosion`
- Set radius to 3 pixels (adjust as needed)
- This shrinks objects and separates touching regions

**Step 5b: Re-label**
- Perform connected component labeling again on the eroded image
- This creates labels with improved separation

**Step 5c: Dilation**
- Select `Process Labels → Expand Labels`
- Set radius to 3 pixels (match the erosion radius)
- This expands the eroded regions back to approximate original size

### 6. Measure Morphological Properties

Quantify your segmented objects:
- Select `Tools → Measure Tables → Object Features/Properties`
- Choose the **Result of Expanded Labels image** as input
- Select which features to measure:
  - Basic: Area, Eccentricity, Solidity
  - Intensity: Mean, Median, Standard deviation (from raw image)
  - Geometry: Equivalent diameter, Centroid, Moments
- Export results as CSV for downstream analysis

### 7. Visualize Measurements

Visualize measurements as colormaps on your labels:
- In the results table, double-click any column header
- A new layer appears with labels colored by that measurement value
- Adjust colormaps to highlight interesting features
- Useful for:
  - Spotting unusually sized objects
  - Determining filtering thresholds
  - Validating measurement correctness

## Sample Data

Download example images from Zenodo:
- **2D image**: `MAX_Lund.tif` - Maximum intensity projection for 2D analysis
- **3D image**: `Lund.tif` - Full 3D stack for this workshop

https://zenodo.org/records/17986091

Place these files in a local folder and open them in Napari.

## Complete Workflow Summary

```
Raw 3D Image
    ↓
Background Removal (White Top Hat)
    ↓
Binarization (Threshold Yen)
    ↓
Connected Component Labeling
    ↓
Label Refinement (Erosion → Re-label → Dilation)
    ↓
Morphological Measurement
    ↓
Visualization & Filtering
```

## Tips & Tricks

- **2D/3D Toggle**: Click the button in the lower-left corner to switch between 2D slicing and 3D rendering
- **Orthogonal Views**: Click the axis order button to see XY, XZ, and YZ planes simultaneously
- **Adjusting Parameters**: Parameters like erosion radius should be tuned based on your image
  - Larger radius = more aggressive erosion (better separation, but may lose small objects)
  - Smaller radius = conservative erosion (keeps more detail, but may not separate adequately)
- **Memory**: Working with large 3D stacks can be memory-intensive. Consider downsampling or working with subsets if needed

## Troubleshooting

### Napari won't launch
- Ensure all dependencies are properly installed: `pixi install`
- Try clearing the pixi cache: `pixi clean cache`

### Bad segmentation
- Try different preprocessing methods:
  - Use different thresholding methods (Otsu, Li, Isodata)
  - Adjust erosion/dilation parameters
  - Increase/decrease White Top Hat radius
  - Apply Gaussian smoothing before thresholding (available in assistant)

### Memory issues with large stacks
- Open only the region of interest (ROI)
- Downsampling spatial dimensions
- Process slices individually and recombine

## Further Learning

- [Napari Documentation](https://napari.org)
- [scikit-image Filters](https://scikit-image.org/docs/stable/api/skimage.filters.html)
- [napari-assistant](https://github.com/napari-assistant/napari-assistant)
- [Full Light-Sheet Workshop](https://bruvellu.github.io/light-sheet-image-analysis-workshop-2026/)

## References

- Cuenca, M., et al. - Practical segmentation workshop
- Data from: https://zenodo.org/records/17986091
