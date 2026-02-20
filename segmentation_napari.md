# Napari-Based 3D Segmentation Guide

This practical guide covers the complete workflow for 3D image segmentation using Napari and the Napari Assistant plugin.

## Learning Objectives

By the end of this section, you will be able to:
- Load and visualize 3D image stacks in Napari
- Use the Napari Assistant for guided segmentation
- Apply preprocessing techniques (background removal, smoothing)
- Perform thresholding and binarization
- Create connected component labels
- Refine segmentation through morphological operations
- Extract and visualize morphological measurements

## Segmentation Pipeline Overview

The complete workflow follows these steps:

```
Raw 3D Image
    ↓
Preprocessing: Remove background and noise
    ↓ 
Binarization: Convert to foreground/background mask
    ↓
Labeling: Assign unique IDs to individual objects
    ↓
Refinement: Improve label quality through morphological operations
    ↓
Quantification: Extract morphological and intensity measurements
    ↓
Visualization: Analyze and explore results
```

## Running the Practical

### Start Napari with the Assistant Plugin

```bash
pixi run assistant
```

This launches Napari with the segmentation assistant panel that guides you through the workflow.

## Workflow Steps

## 3D segmentation in Napari

In this exercise we will:

- Use Napari to open a 3D image
- Use Napari assistant to visualize a workflow for 3D image segmentation and labelling
- Use region props to quantify morphological parameters and make colorcoded plots

These steps form a complete workflow:  
**raw image → pre-processing → segmentation → cleanup → labeling → measurement → filtering**

We’ll use `Lund.tif` as the example image https://zenodo.org/records/17986091.

Requirements:
- Everything you need is in the toml file in the Pixi/napari-assistant folder https://github.com/cuenca-mb/pixi-napari-assistant

### 0. Open Napari assistant using Pixi
In the terminal, go to the directory Pixi/napari-assistant and run:

`pixi run assistant`

### 1. Open a 3D stack

Drag and drop the file or

`File → Open File`

![Brightness/Contrast histogram](images/Napari-load.png)

We will be able to see and explore the stack, and we can change to a 3D rendering with the option `Toogle 2D/3D view` in the lower left button pannel. We can also make orthogonal views by clicking the button to the right `Change order of the visible axis`.

In the right pannel we will see the Assistant plugin, where it suggests operations in the appropriate order. The amount of operations and options depends on your installed plugins. Some of them are redundant.

### 2. Remove background, binarization and labeling

Select `Remove Background → White top hat  → radius = 10`

![Brightness/Contrast histogram](images/Napari-bkg.png)

Then select `Binarize → Threshold Yen`, making sure to select the Result of White top-hat image.

![Brightness/Contrast histogram](images/Napari-thresh.png)

I recommend looking at the result in 3D.

![Brightness/Contrast histogram](images/Napari-3D.png)

Finally, we can select `Label → Connected component labeling`, make sure to select the Result of Threshold image. We can additionally select the `exclude on edges option`.

![Brightness/Contrast histogram](images/Napari-labels.png)

Some of them are stuck together. Let's try and fix that.

### 3. Fix labels

Let's select again the previous layer Result of Threshold. Then select `Process labels → Binary erosion → radius = 3`. This will reduce the objects of the binary segmentation.

![Brightness/Contrast histogram](images/Napari-erode.png)

Now let's recreate the labels `Label → Connected component labeling`, make sure to select the Result of Binary Erosion.

Then `Process labels → Expand Labels → radius = 3`. Explore the labels in 3D.

![Brightness/Contrast histogram](images/Napari-extend.png)

Now we can accurately measure morphological features of these labels. You can close the assistant pannel now.

### 4. Measure morphological properties

Select `Tools → Measure Tables → Object Features/Properties`. Here make sure to select the Result of Expanded Labels image. You can select different features, includding intensity features extracted from the raw data. After running a table should appear which can be exported in csv format. 

![Brightness/Contrast histogram](images/Napari-table.png)

![Brightness/Contrast histogram](images/Napari-table2.png)

by double clicking any of the columns of this table, a new layer image will appear with colorcoded labels indicating the value of the selected measurement. Colormaps can be adjusted for preference.

![Brightness/Contrast histogram](images/Napari-colorcoded.png)

### 5. Saving the workflow

Now you can select `Generate Code...` and export the workflow you just build into a Jupyter notebook.


## Tips and Troubleshooting

### Segmentation Quality Issues

**Problem**: Objects are not clearly separated or background is noisy
- **Solution**: Adjust the radius in Step 2 (White Top Hat). Larger radius removes larger background features
- Try different thresholding methods in Step 3
- Increase erosion radius in Step 5a to further separate touching objects

**Problem**: Small objects are disappearing
- **Solution**: Use a smaller erosion radius in Step 5a
- Consider filtering by size instead of using erosion (see Advanced section)

**Problem**: Uneven segmentation quality across the image
- **Solution**: Try local thresholding instead of global (if available in your Napari version)
- Consider preprocessing with Gaussian blur before white top hat

### Advanced Techniques

**Size-based filtering**: Remove objects smaller than a certain size
- Can be more effective than morphological opening for some images
- Provides finer control over object selection

**Batch processing**: Apply the same segmentation to multiple images
- Napari Assistant can save/load parameter configurations
- Useful for processing image series consistently

## Data Requirements

Example image: `Lund.tif` (available at https://zenodo.org/records/17986091)

Requirements:
- Napari (main visualization tool)
- Napari Assistant plugin (simplifies workflow)
- Region Props plugin (for measurements)
- Setup via Pixi: `pixi install` in repository directory

## References

- [Napari Documentation](https://napari.org/)
- [Scikit-image Morphology](https://scikit-image.org/docs/stable/api/skimage.morphology.html)
- [Connected Component Labeling](https://en.wikipedia.org/wiki/Connected-component_labeling)
- [Mathematical Morphology](https://en.wikipedia.org/wiki/Mathematical_morphology)
