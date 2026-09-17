---
title: Hydra NSR Cryo Utilities
breadcrumbs: false
version: "3.0.6"
repo: Hydra_NSR_Cryo_Utilities
---

Releases on GitHub: [Hydra NSR Cryo Utilities](https://github.com/ChrisLeeThompson/Hydra_NSR_Cryo_Utilities/releases)

Latest version: {{< version-badge >}}

## Introduction

The Hydra NSR Cryo Utilities is a Python UI intended to support common cryo applications with a [Thermo Fisher Scientific (TFS) Hydra](https://www.thermofisher.com/us/en/home/electron-microscopy/products/dualbeam-fib-sem-microscopes/helios-hydra-dualbeam.html) SEM-FIB microscope.

NSR stands for "Non-standard Request". For life science cryo applications, there have historically been two Hydra NSR variants: one with a magnetron Pt sputter coater, and one with a gas injection system (GIS) equipped with a Pt microsputter coater target. This script supports the GIS microsputter variant. Because that coater differs from the Hydra Bio microsputter coater, the Sputter Coat activity in this UI differs from the one in the [Hydra Bio Cryo Utilities](/scripts/hydra_bio_cryo_utilities/).

Other than the Sputter Coat activity, the Hydra NSR Cryo Utilities UI is nearly identical to the Hydra Bio Cryo Utilities UI.

The following sections cover installing and running the script, and briefly describe each page in the UI.

## Requirements

The script has the standard requirements --- AutoScript, PySide, and Python --- listed under [Requirements](/scripts/#requirements) on the [Scripts](/scripts/) page. AutoScript is required for any function that connects to the microscope.

## Installation

To install, follow the general steps under [Installing A Script](/scripts/#installing-a-script) on the [Scripts](/scripts/) page. Because the script interacts with a Hydra NSR microscope, it is best installed on the Hydra NSR Support PC (SPC) and/or the Microscope PC (MPC).

The script can also be run in simulation mode, which requires no microscope. In this mode it can be installed on any PC with the required Python packages, but it cannot interact with a real microscope.

### Updating the Script

1. Download the [latest version](https://github.com/ChrisLeeThompson/Hydra_NSR_Cryo_Utilities/releases) from GitHub.
2. Copy the new files over the existing folder, overwriting when prompted. Do not delete the old folder first (it may contain template and session log files you want to keep).
    * You can copy template and session log files outside the script directory before replacing or updating the application folder.

An update preserves existing data (templates and session logs). Settings, stage positions, and the activity list are stored in the Windows registry rather than in the script directory.

If the default template file is deleted, a factory default version is recreated the next time the script launches.

## Running the Script

The script's main file is `hydra_nsr_cryo_utilities.py`. See [Running A Script](/scripts/#running-a-script) on the [Scripts](/scripts/) page for how to run it with the AutoScript Python interpreter or the AutoScript Runner application.

### Simulation Mode

For development and testing, the UI can be run in simulation mode, where default microscope parameters supply data to the functions without connecting to a microscope.

When the script cannot connect to a microscope, it defaults to simulation mode and displays "Microscope not available" in the status bar.

To run in simulation mode without editing files, launch the script with the `--simulation` flag.

To force simulation mode persistently, set the `DEV_FORCE_SIMULATION` boolean to `True` in the `defaults.py` file, then re-run the script.

{{< filetree/container >}}
    {{< filetree/folder name="Hydra_NSR_Cryo_Utilities-x.x.x" >}}
        {{< filetree/folder name="hydra_nsr_cu" >}}
            {{< filetree/file name="defaults.py" >}}
        {{< /filetree/folder >}}
    {{< /filetree/folder >}}
{{< /filetree/container >}}

```python {filename="defaults.py"}
DEV_FORCE_SIMULATION: bool = True
```

## Safety

The script checks the state of the microscope for some functions, but always be aware of the stage position when using any function or activity that moves the stage.

The `STAGE_SAFE_RADIAL_RANGE_M` parameter in `defaults.py` sets a safe radial range, 8 mm by default, measured from the center position of the stage (x=0, y=0).

When the stage is outside this range, the UI displays warnings for activities that require stage movement. These warnings do not prevent stage movement --- they can be acknowledged to continue --- and serve only as a reminder to check the stage position before proceeding.

{{< filetree/container >}}
    {{< filetree/folder name="Hydra_NSR_Cryo_Utilities-x.x.x" >}}
        {{< filetree/folder name="hydra_nsr_cu" >}}
            {{< filetree/file name="defaults.py" >}}
        {{< /filetree/folder >}}
    {{< /filetree/folder >}}
{{< /filetree/container >}}

```python {filename="defaults.py"}
STAGE_SAFE_RADIAL_RANGE_M: float = 8e-3   # 8 mm from chamber center
```

## RT Prep

The RT Prep page has two activities that prepare the Hydra NSR for cryo applications. They are intended to be run when the stage is at room temperature. These are:

1. GIS Purge
2. Home Stage

{{< img src="/hydra_nsr_cryo_utilities/hydra_nsr_utilities_rt_prep_page_1.png" caption="The RT Prep page." alt="The RT Prep page." width="90%" >}}

### GIS Purge

The GIS Purge activity purges the gas injection system (GIS) for a specified duration. This can help clear water and contamination from the GIS nozzle.

### Home Stage

{{< callout type="warning" >}}
The Hydra NSR microscope is often unaware of where the cryo stage and shuttle are positioned relative to objects that may be in the vacuum chamber, such as the integrated fluorescence microscope (iFLM). Ensure the stage is in a safe position before running the Home Stage activity.
{{< /callout >}}

The Home Stage activity homes/resets the stage with rotation.

## Cryo Prep

The Cryo Prep page has four activities intended to be run when the stage is at cryo temperatures. These are:

1. Stage Positions
2. Sputter Coat
3. GIS Deposition
4. Home Stage

{{< img src="/hydra_nsr_cryo_utilities/hydra_nsr_utilities_cryo_prep_page_1.png" caption="The Cryo Prep page with test stage positions saved in the Stage Positions activity." alt="Cryo Prep page with test stage positions in the Stage Positions activity." width="90%" >}}

### Stage Positions

The Stage Positions activity is modeled after the Stage module in the xT Microscope Control software. With xT Microscope Control, you can move the stage to a position, then use this activity to save the position in the list. Click a stage position in the list to show its raw stage coordinates. The Sputter Coat and GIS Deposition activities both require a position saved here.

The positions in the list are automatically saved between application launches.

To create and save a stage position to the list:

1. With xT Microscope Control, move the stage to the desired position (the default GIS deposition for a specified grid, for example).
2. Click the Add Position button.
3. Name the stage position.

### Sputter Coat

{{< img src="/hydra_nsr_cryo_utilities/hydra_nsr_utilities_cryo_prep_page_sputter_coat.png" caption="An expanded Sputter Coat activity." alt="Expanded NSR Sputter Coat activity" width="60%" >}}

Compared to the [Sputter Coat activity](/scripts/hydra_bio_cryo_utilities/#sputter-coat) in the [Hydra Bio Cryo Utilities](/scripts/hydra_bio_cryo_utilities/) script, the Hydra NSR Sputter Coat activity differs in the following ways:

1. The stage position for microsputter coating a grid must be selected. The Position combobox is populated with stage positions from the Stage Positions activity.
2. Parameters on the Settings page must be specified. These are:
    * Sputter Pattern File: the filename of the microsputter pattern file (`.ptf` file extension). The file must be located in the script's `pattern_files` directory. The microscope uses this pattern to mill into the microsputter target.
    * Sputter Coat HFW (µm): the horizontal field width (HFW) of the PFIB the microscope will use to run the pattern file.
    * Sputter Coat Port Name: the name of the GIS port associated with the microsputter coater/target. The default name for Hydra NSR cryo microscopes is `µCoater`.

The Sputter Coat activity uses the gas injection system equipped with a microsputter target to deposit a thin layer of Pt at the selected position (a specified grid, for example). It applies the ion species, PFIB current, pattern file, sputter coat HFW, and duration set for the activity.

When the activity is switched on, it automatically sets the Ion Species combobox to the microscope's current PFIB ion species. The ion species can also be changed for each Sputter Coat activity.

The Bulk Sputtering and Lamella Sputtering buttons set preset sputter durations. The default durations for each button can be set on the Settings page.

### GIS Deposition

The GIS Deposition activity drives the stage to a position selected from the Position combobox --- populated with the positions list from the Stage Positions activity --- then inserts the GIS, opens the valve for a set duration, and retracts the GIS.

The GIS port name is specified on the Settings page. The default is `Pt dep Cryo`.

### Home Stage

{{< callout type="warning" >}}
The Hydra NSR microscope is often unaware of where the cryo stage and shuttle are positioned relative to objects that may be in the vacuum chamber, such as the integrated fluorescence microscope (iFLM). Ensure the stage is in a safe position before running the Home Stage activity.
{{< /callout >}}

This is the same activity as on the RT Prep page. Homing is often unnecessary here, but it can be helpful for resetting the stage after the last GIS Deposition or Sputter Coat activity.

### Cryo Prep Activities List

Sputter Coat and GIS Deposition activities can be added or removed from the list of activities. They can also be reordered in the list (the Stage Positions and Home Stage activities cannot be reordered).

The activity list is preserved between application instances and can be saved as a template file for later use. Click the Save button to store the current list as a template, or the Load button to restore a saved one.

{{< img src="/hydra_nsr_cryo_utilities/hydra_nsr_utilities_cryo_prep_page_2.png" caption="The default Cryo Prep page activities list." alt="Default Cryo Prep page activities list." width="90%" >}}

## Stage/Scan

The Stage/Scan page has functions for manually driving the stage to regions of interest.

1. Rotate Stage 180° button
    * Rotates the stage 180° from its current position (compucentric and relative rotation). Depending on the options selected, the stage can tilt to 0° before rotating and then tilt to a specified angle once rotation is complete. The SEM and FIB can also be scan rotated 180° automatically after the rotation and tilt finish.
2. Scan Rotate SEM and FIB 0° and Scan Rotate SEM and FIB 180° buttons
    * Set the scan rotation of the SEM and FIB to 0° or 180° at the same time.
3. Stage Z slider
    * A clickable and draggable slider adjusts the z-height of the stage with Z-Y Link enabled. This is helpful when manually positioning a region of interest at the SEM-FIB coincidence point. The further the slider is moved from the center, the larger the z-height step change.
    * For example: move the stage in x and y to center the region of interest in the SEM view, then, while scanning with the FIB, adjust the z slider until the region is centered in the FIB view. Because Z-Y Link is enabled, the region should be nearly centered in both the SEM and FIB views.

{{< img src="/hydra_nsr_cryo_utilities/hydra_nsr_utilities_stage_scan_page_1.png" caption="The Stage/Scan page." alt="The Stage/Scan page." width="90%" >}}

The application window can be resized to fit a quadrant of the xT Microscope Control window. Enable this "compact mode" from the Settings page, or resize the window manually to enable the mode automatically.

Always On Top, also on the Settings page, can be used to overlay the window onto the xT Microscope Control window. This is helpful when using the Stage/Scan page with xT for repeated manual stage movements.

{{< columns >}}
    {{< column >}}
    {{< img src="/hydra_nsr_cryo_utilities/hydra_nsr_utilities_settings_compact_mode.png" caption="The Settings page with Always On Top and Compact Mode enabled." alt="Settings page with Always On Top and Compact Mode enabled" >}}
    {{< /column >}}
    {{< column >}}
    {{< img src="/hydra_nsr_cryo_utilities/hydra_nsr_utilities_stage_scan_page_compact_mode.png" caption="The Stage/Scan page with Compact Mode enabled." alt="Stage/Scan page in compact mode" >}}
    {{< /column >}}
{{< /columns >}}

## Lift-out Angle Calc

The Lift-out Angle Calc page has three calculators for cryo lift-out angles. They help determine the stage tilt angles needed to reach a desired milling angle for a lift-out site, assuming a pre-tilted AutoGrid shuttle is used.

The three types of lift-outs supported are:

1. Top-down: -70° stage rotation
2. Planar: -70° stage rotation
3. Planar: 110° stage rotation

{{< img src="/hydra_nsr_cryo_utilities/hydra_nsr_utilities_lift-out_angle_calc_page_1.png" caption="The Lift-out Angle Calc page." alt="Lift-out angle calc page." width="90%" >}}

## Milling Angle Calc

The Milling Angle Calc page provides a milling angle calculator and two interactive graphics to visualize stage tilt angles relative to the SEM, FIB, GIS, and milling positions. The calculator and graphics are not linked to the microscope; they are display only.

The top graphic shows a 35° AutoGrid shuttle and the bottom graphic shows a cartoon of a portion of a sample on a grid.

{{< callout type="info" >}}
The GIS position is shown in 2D. The graphic ignores the angle between the GIS and the FIB along the z-axis, which points out of the screen.
{{< /callout >}}

{{< img src="/hydra_nsr_cryo_utilities/hydra_nsr_utilities_milling_angle_calc_page_1.png" caption="The Milling Angle Calc page with the stage tilted to 7° (milling angle = 10°)." alt="Milling Angle Calc page with stage tilted to 7°." width="90%" >}}

The graphics are interactive: clicking an angle label tilts the stage graphic to that angle, and clicking the SEM, FIB, or GIS label tilts it to the angle associated with that position. For example, clicking the SEM label tilts the stage graphic to 35° (positioning the grid perpendicular to the SEM). Scrolling over either graphic --- with a mouse wheel or a touchpad --- also tilts it, 1° per step.

In the stage/shuttle tilt cartoon, at tilt angles greater than about 35°, the bottom of the shuttle does not align with the angle tick marks. This is because the shuttle graphic rotates about the point where the grid is located, which allows it to show a eucentric tilt of the shuttle without moving the shuttle along the plot's x-axis.

{{< img src="/hydra_bio_cryo_utilities/hydra_bio_utilities_milling_angle_calc_page_shuttle_7deg.png" caption="7° stage tilt (milling angle = 10°)." alt="7° stage tilt" width="90%" >}}

{{< img src="/aquilos2_cryo_utilities/aquilos2_utilities_milling_angle_calc_page_sample_chalk_7deg.png" caption="Chalk line drawn on the sample cartoon at a 7° stage tilt." alt="Chalk line at 7° stage tilt" width="90%" >}}

Named after the construction tool, the Add Chalk Line button draws a chalk line onto the graphic along the FIB line at the current tilt angle. Chalk lines represent FIB milling positions, and multiple lines can be drawn. This can be useful for sketching FIB workflows or for quickly determining approximate angles for the SEM or GIS relative to a FIB milling position or angle. Click the Remove Last button to remove the most recently drawn chalk line, or Clear All to remove every chalk line. Both are disabled when there are no chalk lines to remove.

When there is a stage tilt at which a chalk line becomes perpendicular or parallel to the SEM, FIB, or GIS, a chip is displayed next to the associated label. Clicking a chip moves the graphic to that tilt position; hovering over it shows the tilt angle in a tooltip.

{{< img src="/hydra_nsr_cryo_utilities/hydra_nsr_utilities_milling_angle_calc_page_2.png" caption="Tooltip for a perpendicular to SEM position relative to a 10° milling angle." alt="Tooltip for perpendicular to SEM for 10° milling angle." width="90%" >}}

### Edit GIS Position

The GIS position shown on the Milling Angle Calc page is at port 4 on the vacuum chamber.

If your Hydra NSR uses port 5 for the cryo GIS, a small change to the `millingAngleCalculations.js` file moves the GIS label in the shuttle and sample graphics to match.

{{< filetree/container >}}
    {{< filetree/folder name="Hydra_NSR_Cryo_Utilities-x.x.x" >}}
        {{< filetree/folder name="qml_resources" >}}
            {{< filetree/folder name="Js" >}}
                {{< filetree/file name="millingAngleCalculations.js" >}}
            {{< /filetree/folder >}}
        {{< /filetree/folder >}}
    {{< /filetree/folder >}}
{{< /filetree/container >}}

```javascript {filename="millingAngleCalculations.js"}
var GIS_ANGLE_FROM_SEM_DEG = 40.0
```

For port 5, change `GIS_ANGLE_FROM_SEM_DEG` from 40.0 to 73.5.

## Session Log

When activities are run from the RT Prep page or the Cryo Prep page, the session information is displayed on the Session Log page. The session data is saved to a `session_log.jsonl` file in the `session_logs` directory.

Notes can be added for each session to provide more context in the log.

The session log file can be cleared/deleted from the Settings page.

{{< img src="/hydra_nsr_cryo_utilities/hydra_nsr_utilities_session_log_page_1.png" caption="The Session Log page." alt="Session Log page" width="90%" >}}

## Settings

The Settings page provides access to some of the application's settings and parameters.

{{< img src="/hydra_nsr_cryo_utilities/hydra_nsr_utilities_settings_page_1.png" caption="The Settings page." alt="Settings page" width="90%" >}}

1. Always On Top
    * Keeps the application window on top of other windows.
2. Compact Mode
    * Resizes the application window to approximately the size of a quadrant in the xT Microscope Control software.
3. GIS Gas Port Name
    * This is a critical setting because it provides the GIS Deposition activity with the object name it needs to control the GIS. The default is the name of a common Hydra NSR GIS for cryo applications.
4. Sputter Pattern File
    * The name of the pattern file the microscope will use to mill into the microsputter target. The `.ptf` pattern file must be in the script's `pattern_files` directory. The default pattern file included with the script is `pattern_Sputtering.ptf`.
5. Sputter Coat HFW (µm)
    * The horizontal field width (HFW) for the PFIB when running the sputter pattern file. Note: the microscope will not run a pattern that extends beyond the current HFW.
6. Sputter Coat Port Name
    * The port name of the GIS equipped with the microsputter target. Look at the Gas Injection module in xT Microscope Control to see the GIS port names.
7. Bulk Sputtering Duration (s)
    * Sets the default duration for the Bulk Sputtering button in the Sputter Coat activity.
8. Lamella Sputtering Duration (s)
    * Sets the default duration for the Lamella Sputtering button in the Sputter Coat activity.
9. Restore PFIB Voltage and Current
    * Restores the original PFIB high voltage, beam current, and on/off state after all Cryo Prep activities have successfully completed.
10. Restore PFIB Ion Species
    * Restores the original ion species after all Cryo Prep activities have successfully completed.
11. Zero Tilt Before GIS Deposition
    * Adds a layer of safety when the stage is moved to GIS deposition positions. If you know the positions are safe to move to, unchecking this feature reduces the time required to reach them.
12. Move Stage To Original Position
    * Returns the stage to its original position after all of the RT Prep or Cryo Prep activities have successfully completed.
13. Reset RT Prep Parameters and Reset Cryo Prep Parameters
    * Reset their respective page activities to application defaults.
14. Session Log File Size
    * Displays the current log file size, so you know when the file should be manually copied and cleared.
15. Clear Session Log button
    * Deletes the `session_log.jsonl` file.
