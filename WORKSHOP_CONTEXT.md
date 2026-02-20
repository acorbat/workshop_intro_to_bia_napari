# Workshop Context: Introduction to Bioimage Analysis

## Overview

This workshop introduces fundamental concepts in bioimage analysis, focusing on 3D image segmentation and quantification using Napari and Python-based tools. Participants will learn practical image analysis techniques through hands-on experience with real bioimage data.

## Learning Objectives

By the end of this workshop, you will be able to:

### Image Visualization
- Load and visualize 3D image stacks in Napari
- Navigate between 2D and 3D viewing modes
- Create orthogonal projections for better spatial understanding
- Adjust display settings (brightness, contrast, colormaps)

### Image Preprocessing
- Understand background removal and its importance
- Apply White Top Hat filtering for uneven illumination correction
- Identify and remove noise using smoothing filters
- Evaluate the impact of preprocessing on segmentation results

### Image Segmentation
- Understand thresholding concepts and methods
- Apply global and local thresholding techniques
- Create binary masks from intensity images
- Compare different thresholding algorithms (Yen, Otsu, Li, Isodata)

### Label Generation & Refinement
- Generate connected component labels from binary masks
- Understand connectivity and its impact on labeling
- Apply morphological operations (erosion, dilation, fill holes)
- Refine segmentation using iterative refinement workflows
- Handle touching/overlapping objects through morphological refinement

### Quantification
- Extract morphological measurements from segmented objects
- Understand region properties (area, perimeter, circularity, ellipse fit)
- Measure intensity features from original data
- Export results for downstream analysis

### Visualization & Analysis
- Create colormapped visualizations of measurements
- Interpret measurement distributions
- Filter objects based on morphological criteria
- Prepare data for statistical analysis

## Workshop Structure

### Part 1: Introduction (30 minutes)
- Overview of bioimage analysis workflow
- Introduction to Napari interface
- Demonstration of segmentation pipeline

### Part 2: Hands-on Practical (90 minutes)
- Load and visualize 3D sample image
- Execute preprocessing steps
- Perform segmentation workflow
- Refine and measure results
- Explore and visualize measurements

### Part 3: Advanced Topics (30 minutes)
- Troubleshooting common segmentation issues
- Adjusting parameters for your own images
- Introduction to automation and batch processing
- Resources for further learning

## Key Concepts

### Segmentation Pipeline

```
Raw Image
    ↓
Preprocessing: Remove background and noise
    ↓ 
Binarization: Convert to foreground/background
    ↓
Labeling: Assign unique IDs to objects
    ↓
Refinement: Improve label quality through morphology
    ↓
Quantification: Extract morphological and intensity measurements
    ↓
Analysis: Statistical analysis and visualization
```

### White Top Hat Filtering

- Removes uneven background illumination (shading)
- Isolates small objects while suppressing larger background features
- Radius parameter should be larger than largest background structure but smaller than smallest foreground objects

### Thresholding

- **Global thresholding**: Uses single threshold for entire image
  - Fast but fails with uneven illumination
  - Best for high-contrast images

- **Local thresholding**: Computes threshold for neighborhood around each pixel
  - Adapts to local intensity variations
  - Requires larger computational cost
  - Better for images with shading or uneven lighting

### Connected Component Labeling

- Converts binary mask into integer label image
- Each connected foreground region receives unique label (1, 2, 3, ...)
- Connectivity can be 4-neighbor or 8-neighbor (2D) or 6/26-neighbor (3D)
- Foundation for object-based analysis

### Morphological Operations

- **Erosion**: Removes small objects and separates touching regions
  - Shrinks all foreground pixels by radius
  - Effective at breaking weak connections

- **Dilation**: Expands remaining objects back toward original size
  - Grows foreground regions by radius
  - Restores approximate original shape after erosion

- **Erosion-Dilation cycle (Opening)**: Removes small noise while preserving larger structures

### Region Properties

Typical measurements extracted from segmented objects:

**Geometric:**
- Area: Number of pixels in object
- Perimeter: Boundary length
- Circularity: How round the object is (1.0 = perfect circle)
- Eccentricity: Elongation of best-fit ellipse
- Solidity: How "filled" the object is

**Positional:**
- Centroid: Center of mass position
- Bounding box: Smallest rectangle containing object
- Orientation: Angle of major axis

**Intensity (from original image):**
- Mean, median, standard deviation
- Min/max intensity
- Integrated intensity

## Tools & Libraries

### Napari
- Interactive image viewer with advanced visualization
- Plugin architecture for extensions
- **napari-assistant**: Workflow recommendation tool that suggests next processing steps

### Python Libraries
- **scikit-image**: Image processing algorithms (filters, morphology, segmentation)
- **scipy**: Scientific computing including morphological operations
- **pandas**: Data manipulation (for measurement tables)
- **imageio/tifffile**: Reading/writing image files

## Practical Workflow Example

Working with `Lund.tif` (light-sheet confocal image of biological structure):

1. **Visualization**: Open in Napari, inspect in 3D
2. **Preprocessing**: Apply White Top Hat (r=10) to remove background
3. **Thresholding**: Use Yen method to create binary mask
4. **Labeling**: Connected component labeling on binary image
5. **Refinement**: Erode (r=3) → Re-label → Dilate (r=3) to separate touching objects
6. **Measurement**: Extract area, volume, shape, and intensity features
7. **Analysis**: Filter by size, visualize distribution, export for further analysis

## Learning Outcomes

### Knowledge
- Understanding of image segmentation principles
- Familiarity with preprocessing, thresholding, and labeling strategies
- Knowledge of morphological operations and their effects

### Skills
- Practical ability to perform segmentation in Napari
- Parameter optimization for different image types
- Result visualization and quality assessment
- Data export for downstream analysis

### Competence
- Ability to segment simple 3D biological images
- Capacity to troubleshoot common segmentation issues
- Understanding of when/how to apply preprocessing and refinement
- Foundation for learning advanced techniques (machine learning segmentation, tracking, etc.)

## Next Steps After the Workshop

### Immediate Applications
- Apply workflow to your own image data
- Adjust parameters for your specific imaging conditions
- Experiment with different preprocessing and thresholding methods

### Further Learning
- Explore advanced segmentation (machine learning-based)
- Learn advanced quantification techniques
- Study cell/object tracking in time-lapse data
- Investigate classification of segmented objects

### Automation
- Script repetitive workflows using Python
- Batch process multiple images
- Create reproducible analysis pipelines

## Resources

- **Napari Tutorials**: https://napari.org/stable/tutorials/
- **scikit-image Gallery**: https://scikit-image.org/docs/stable/auto_examples/
- **Bioimage Analysis Course**: https://bruvellu.github.io/light-sheet-image-analysis-workshop-2026/
- **ImageJ/Fiji Segmentation Guide**: https://imagej.net/
- **Python for Bioimaging**: Community books and tutorials

## Contact & Support

For questions during the workshop:
- Refer to the README.md for step-by-step instructions
- Check the Troubleshooting section for common issues
- Consult Napari/scikit-image documentation
- Explore the references and learning resources provided

---

**Workshop Version**: 0.1.0  
**Last Updated**: February 2026
