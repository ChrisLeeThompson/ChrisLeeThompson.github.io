---
title: ATC Project Explorer
breadcrumbs: false
version: "1.4.2"
repo: ATC_Project_Explorer
---

Releases on GitHub: [ATC Project Explorer](https://github.com/ChrisLeeThompson/ATC_Project_Explorer/releases)

Latest version: {{< version-badge >}}

## Introduction

AutoTEM Cryo (ATC) is a Thermo Fisher Scientific (TFS) software application that is used to automatically prepare cryo lamellae. AutoTEM Cryo is most often used with the TFS Aquilos 2, Hydra Bio, and Arctis microscopes.

ATC Project Explorer is a Python UI that parses, processes, and displays information from AutoTEM Cryo project files. The UI includes plots with project data, such as mean durations, and an image viewer to browse images logged during lamellae production.

The project information can be helpful when developing ATC templates, troubleshooting issues with lamellae production, and understanding more about how ATC makes lamellae.

## Requirements

The script's requirements differ from the standard list under [Requirements](/scripts/#requirements) on the [Scripts](/scripts/) page. The script requires:

* PySide6 6.7.1+
* Python 3.11+
* Matplotlib 3.8.1+
* NumPy 2.2.5+
* Pillow 10.1+
* tifffile 2025.3.13+ (optional; used for TIFF metadata in image overlays and the site position montage)

AutoScript is not required to run this script, but all of these packages are included with the AutoScript 4.14 environment, so no additional dependencies are needed if the script is run with AutoScript.

The script was tested with ATC 2.4.6.

## Installation

To install, follow the general steps under [Installing A Script](/scripts/#installing-a-script) on the [Scripts](/scripts/) page. The script does not connect to a microscope, so it can be installed on any compatible PC.

## Running the Script

The script's main file is `atc_project_explorer.py`. See [Running A Script](/scripts/#running-a-script) on the [Scripts](/scripts/) page for how to run it with the AutoScript Python interpreter or the AutoScript Runner application.

## Loading an ATC Project

{{< img src="/atc_project_explorer/atc_project_explorer_blank_ui.png" caption="The ATC Project Explorer window before a project is loaded." alt="ATC Project Explorer initial window." width="90%" >}}

In this context, an ATC project is an entire ATC project directory.

There are two methods for loading a project. One is to click the Load ATC Project button and select the project directory from the Select ATC Project Directory window. The other method is to drag and drop a project directory onto Catbug (the icon in the upper-right corner of the window).

The script validates the project directory by checking that the `ProjectData.dat` and `Statistics.txt` files are present.

When a project is loaded, the script parses the project files. The parsed data is written to a `_temp_consolidated_metadata.json` file, which is saved in a new `_temp_output_JSON_file` directory in the project root directory.

The `_temp_consolidated_metadata.json` file can be loaded by clicking the Load Metadata File button or dragging and dropping the file onto Catbug. Loading a project from the `.json` file is much faster than loading the project directory because the data is already parsed.

If you load the `.json` file without the project directory, project images may not be available in the UI as only their relative paths are saved in the file.

## Global Project Data

{{< img src="/atc_project_explorer/atc_project_explorer_global_page_1.png" caption="The Global Project Data page." alt="ATC Project Explorer Global Project Data page." width="90%" >}}

When project data is loaded, the Global Project Data page is displayed first.

On the right side of the window, in the Site Selection groupbox, use the combobox to select the Global Project Data page or a lamella site in the project. The Previous and Next buttons can be used to move through the sites, and the Open In New Window button will open the selected site in a new window.

The project title and currently selected site are shown at the top of the UI. Click the project title to open its directory.

The Global Project Data page has four main panels/groupboxes. They are:

1. Global Statistics
2. Site Previews
3. Site Durations
4. Relative Site Positions

### Global Statistics

{{< img src="/atc_project_explorer/atc_project_explorer_global_statistics_1.png" caption="Global Statistics calculated from all lamella sites." alt="ATC Project Explorer Global Statistics" width="60%" >}}

The Global Statistics groupbox shows statistical data calculated from all selected lamella sites in the project. The sites included in the calculations can be selected in the Site Durations groupbox.

### Site Previews

{{< img src="/atc_project_explorer/atc_project_explorer_site_preview_2.png" caption="Site Previews with preview cards for each site in the project." alt="ATC Project Explorer Site Previews" width="90%" >}}

The Site Previews groupbox displays a card for each lamella site in the project. Click a card to open its lamella site page.

### Site Durations

{{< img src="/atc_project_explorer/atc_project_explorer_site_durations_1.png" caption="Site durations for all sites in the project." alt="ATC Project Explorer Site Durations" width="90%" >}}

The Site Durations plot displays the total duration for each site and the durations for each recipe in a site.

ATC has three main recipes when creating a lamella. These are Preparation, Milling, and Thinning. The lamella placement duration is separated from the Preparation duration as this is one of the few activities that is not automated.

Hover the mouse cursor over a recipe section to see its duration, and click any of the recipe durations or lamella site names to open the associated lamella page.

The checkboxes to the left of the site names can be checked or unchecked to add or remove the sites from the global statistics calculations.

### Relative Site Positions

{{< img src="/atc_project_explorer/atc_project_explorer_rel_site_positions_2.png" caption="The Relative Site Positions plot on the Global Project Data page." alt="ATC Project Explorer Relative Site Positions" width="90%" >}}

The Relative Site Positions plot displays images (when available) from the tileset created when setting up the lamella sites. The lamella positions are overlaid on the images to provide an overview of their relative positions. Click a lamella site to open its page.

## Lamella Site Pages

{{< img src="/atc_project_explorer/atc_project_explorer_lamella_page_1.png" caption="The lamella site page." alt="ATC Project Explorer lamella site page." width="90%" >}}

The lamella site page (or selected site page) has six main panels/groupboxes. They are:

1. Site Statistics
2. Site Preview
3. Activity Durations
4. Pattern Viewer
5. Image Viewer
6. Site Parameters

### Site Statistics

{{< img src="/atc_project_explorer/atc_project_explorer_site_statistics.png" caption="Information and statistics for a selected site." alt="ATC Project Explorer Site Statistics" width="60%" >}}

The Site Statistics groupbox shows information about the site, including statistical information from the site's activities.

### Site Preview

{{< img src="/atc_project_explorer/atc_project_explorer_site_preview_3.png" caption="Selected site preview showing the lamella evaluation image and the last FIB image acquired after polishing." alt="ATC Project Explorer Site Preview" width="90%" >}}

When available, the lamella evaluation image and last polish image are shown to preview the lamella site. Click a preview image to open it in the Image Viewer groupbox.

### Activity Durations

{{< img src="/atc_project_explorer/atc_project_explorer_activity_durations.png" caption="The Activity Durations plot for a selected site." alt="ATC Project Explorer Activity Durations plot" width="90%" >}}

The Activity Durations plot shows the durations for each of the site's recipes and activities.

### Pattern Viewer

{{< img src="/atc_project_explorer/atc_project_explorer_pattern_viewer.png" caption="The Pattern Viewer groupbox." alt="ATC Project Explorer Pattern Viewer" width="90%" >}}

The Pattern Viewer shows the relative sizes and positions of the site's milling patterns, along with information about them.

The collapsible left-side panel shows data related to the selected pattern(s). The right-side buttons can be used to select patterns associated with the site's activities. Patterns can also be selected by clicking them in the graphics area.

The mouse scroll-wheel can be used to zoom in and out of the graphic, and the Reset view button (bottom-right corner) will reset the view.

### Image Viewer

{{< img src="/atc_project_explorer/atc_project_explorer_image_viewer_5.png" caption="The Image Viewer groupbox." alt="ATC Project Explorer Image Viewer" width="90%" >}}

The Image Viewer shows images from the site's logged image directories, such as the PrecisePositioningLogImages directory. This directory can be helpful as the images show the entire story of the lamella from preparation to final polishing.

The Select Image Directory combobox, in the collapsible left-side panel, lists the site's image directories. Each image from a selected directory will be displayed in the image area.

Click the image filename to open its containing directory. Check the Show Graphics checkbox to show available graphics overlaid on the image, such as pattern positions derived from the metadata. The current image number out of the total number of images in the directory is displayed to the right of the Show Graphics checkbox.

The mouse scroll-wheel can be used to zoom in and out of the image. The Previous and Next buttons can be used to cycle through the images, and the zoom value is preserved between images. Click the Reset view button in the bottom-right corner to reset the view.

The opacity sliders can be used to reveal the previous or next image.

{{< img src="/atc_project_explorer/atc_project_explorer_image_viewer_3.png" caption="Image Viewer with opacity adjusted to show the next image with overlaid milling pattern graphics." alt="ATC Project Explorer Image Viewer and opacity slider." width="90%" >}}

### Site Parameters

{{< img src="/atc_project_explorer/atc_project_explorer_site_parameters.png" caption="The Site Parameters groupbox." alt="ATC Project Explorer Site Parameters groupbox" width="90%" >}}

The Site Parameters groupbox shows searchable metadata parsed from the project's `ProjectData.dat` file for the selected lamella site. Recipe-specific data is displayed in its own labeled card.

The information that is shown represents the state of the lamella parameters at the time the lamella site was completed. It does not reflect dynamic changes made while the lamella was being processed --- the data is static, as written to the `ProjectData.dat` file.

## Config File

In the script directory, there is a `config_files` directory. Within this directory is the config file `ATCProjectExplorerConfig.json`.

At the top of the config file, there are a few parameters that can be edited, such as the name of the temporary metadata file. By default, the metadata file is minified. To make it more human-readable, set the `MinifyExportedJSON` parameter to `false`.