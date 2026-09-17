---
title: iFLM Slice Interpolator
breadcrumbs: false
version: "2.2.2"
repo: iFLM_Slice_Interpolator
---

Releases on GitHub: [iFLM Slice Interpolator](https://github.com/ChrisLeeThompson/iFLM_Slice_Interpolator/releases)

Latest version: {{< version-badge >}}

{{< callout type="info" >}}
This script is experimental. Many of the features are still being explored and tested.
{{< /callout >}}

## Introduction

The Thermo Fisher Scientific (TFS) Integrated Fluorescence Microscope (iFLM) is featured on several TFS SEM-FIB microscopes (Aquilos 2, Hydra Bio, and Arctis). The iFLM software saves image stacks along with a `.tfs.xml` file. The `.tfs.xml` file includes metadata that TFS Maps software reads when the images are imported into Maps.

Before acquiring fluorescence images with the iFLM, the user specifies the slice thickness (the distance the objective moves between image acquisitions). The slice thickness is recorded in the `.tfs.xml` file that Maps reads.

To reduce energy dose to the sample and reduce acquisition time, slice thicknesses are often between 200 nm and 1 µm. The smaller the slice thickness, the more images are acquired — increasing both dose and acquisition time.

The iFLM Slice Interpolator is a Python UI that allows the user to take a set of iFLM images with a `.tfs.xml` file and interpolate the original images (creating virtual slices). The script outputs new interpolated images and a new `.tfs.xml` file that can be imported into Maps software. In addition to interpolation, the script processes the images to reduce background and sharpen features.

## Requirements

The script's requirements differ from the standard list under [Requirements](/scripts/#requirements) on the [Scripts](/scripts/) page. The script requires:

* PySide6 6.7.1+
* Python 3.11+
* NumPy 2.2.5+
* OpenCV 4.8.1+
* SciPy 1.15.3+
* tifffile 2025.3.13+

AutoScript is not required to run this script, but all of these packages are included with the AutoScript 4.14 environment, so no additional dependencies are needed if the script is run with AutoScript.

## Installation

To install, follow the general steps under [Installing A Script](/scripts/#installing-a-script) on the [Scripts](/scripts/) page. The script does not connect to a microscope, so it can be installed on any compatible PC.

## Running the Script

The script's main file is `iflm_slice_interpolator.py`. See [Running A Script](/scripts/#running-a-script) on the [Scripts](/scripts/) page for how to run it with the AutoScript Python interpreter or the AutoScript Runner application.

## Overview of the UI

{{< img src="/iflm_slice_interpolator/iflm_slice_interpolator_1.png" caption="The iFLM Slice Interpolator window." alt="iFLM Slice Interpolator window" width="40%" >}}

The iFLM Slice Interpolator application window contains five panels and three buttons. The following sections describe how to load iFLM image stacks, the panels in the application window, and how to load/import the interpolated images into TFS Maps software.

## Overview of the Workflow

The image processing workflow is as follows:

1. Load the `.tif` images via the `.tfs.xml` file. The image stacks are processed per wavelength.
2. Scan for hot pixels in the images (optional).
    * The entire stack of images is scanned to build a defect map, before anything is written to disk.
3. Filter pass, one slice at a time:
    * Hot pixel repair
    * Background subtraction
    * Unsharp masking
4. Interpolate images
5. Generate new `.tfs.xml` file.
6. Import the new `.tfs.xml` file into a Maps project

When processing is complete, a new `.tfs.xml` file is generated. The new file will be saved in the same directory as the original `.tfs.xml` file. The new file name is prefixed with `Interpolated_`.

A `processed_images` directory will also be created, containing all of the new interpolated images that Maps will import.

## Loading iFLM Images

{{< img src="/iflm_slice_interpolator/iflm_slice_interpolator_image_stack_origin.png" caption="The Image Stack Origin panel." alt="iFLM Slice Interpolator Image Stack Origin panel" width="60%" >}}

The iFLM software will save a `.tfs.xml` file along with a stack of images. There are two ways to load the `.tfs.xml` file associated with a stack of images. One is to click the Browse button in the Image Stack Origin panel. The other way is to drag and drop the `.tfs.xml` file onto Catbug (the icon in the Status panel).

## Background Subtraction Settings

{{< img src="/iflm_slice_interpolator/iflm_slice_interpolator_background-subtraction-settings_1.png" caption="The Background Subtraction Settings panel." alt="iFLM Slice Interpolator Background Subtraction Settings panel" width="50%" >}}

The Background Subtraction Settings panel contains a combobox to select a background subtraction algorithm and associated settings. The algorithms are:

1. Minimum Value
2. Gaussian Background
3. Rolling Background

By default, the background subtraction is performed for each image individually. When the Global Background Normalization checkbox is checked, the background is normalized for the entire stack of images.

## Image Filter Settings

{{< img src="/iflm_slice_interpolator/iflm_slice_interpolator_image-filter-settings_1.png" caption="The Image Filter Settings panel." alt="iFLM Slice Interpolator Image Filter Settings" width="50%" >}}

The Image Filter Settings panel contains options and parameters for performing image filtering. The Hot Pixel Filter removes hot pixels. The unsharp settings sharpen features after background subtraction.

Low unsharp amounts and low Gaussian sigma values are often ideal.

## Interpolation Settings

{{< img src="/iflm_slice_interpolator/iflm_slice_interpolator_interpolation-settings_1.png" caption="The Interpolation Settings panel." alt="iFLM Slice Interpolator Interpolation Settings" width="50%" >}}

The Interpolation Settings panel contains a method combobox and parameters for the slice interpolation process.

There are two interpolation methods: Linear Spline and Cubic Spline. Linear Spline is often sufficient; Cubic Spline requires significantly more processing time.

There are two interpolation factor values: 2 and 4. Interpolation factor 2 will divide the slice thickness in half. For example, an original 500 nm slice thickness will become 250 nm with interpolation factor 2.

## Status

{{< img src="/iflm_slice_interpolator/iflm_slice_interpolator_status_1.png" caption="The Status panel and main application buttons." alt="iFLM Slice Interpolator Status panel" width="50%" >}}

The Status panel will show the current status of image processing.

When a `.tfs.xml` file is loaded, the Start button is enabled. Click Start to begin processing and interpolating the images. Click the Stop button to stop processing.

The Delete Data button is enabled whenever interpolated output exists (including output from a previous run) and processing is not running. Click the Delete Data button to delete the new interpolated `.tfs.xml` file and the `processed_images` directory.

## Importing Interpolated Images in Maps

There are two ways to import the new interpolated images into Maps software. One is to click the Import Fluorescence Data icon in the upper-right corner of the Maps window. The icon is a file icon with a red arrow. The other way is to go to the File menu and select Import Images. This will open the Select images to import window. Change the file type to TFS File (*.tfs.xml). Select the new interpolated `.tfs.xml` file and click Open.

When browsing through the processed image stacks in Maps, you may observe a "flashing" effect. This is due to the image filtering and interpolation process. Non-interpolated images will appear slightly different compared to the interpolated images. The difference will be noticeable as you step through the stack of images.

