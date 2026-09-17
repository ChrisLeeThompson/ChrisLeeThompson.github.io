---
title: ASV Spin Mill Angle Calculator
breadcrumbs: false
version: "3.2.0"
repo: ASV_Spin_Mill_Angle_Calculator
---

Releases on GitHub: [ASV Spin Mill Angle Calculator](https://github.com/ChrisLeeThompson/ASV_Spin_Mill_Angle_Calculator/releases)

Latest version: {{< version-badge >}}

{{< callout type="info" >}}
This script is experimental. Many of the features are still being explored and tested.
{{< /callout >}}

## Introduction

The ASV Spin Mill Angle Calculator is a Python UI that helps set up the stage positions for a Thermo Fisher Scientific (TFS) [Auto Slice and View (ASV)](https://www.thermofisher.com/us/en/home/electron-microscopy/products/software-em-3d-vis/auto-slice-view-4-software.html) Spin Mill project on a [Hydra Bio](https://www.thermofisher.com/us/en/home/electron-microscopy/products/dualbeam-fib-sem-microscopes/hydra-bio-plasma-fib.html) SEM-FIB microscope.

A Spin Mill project has two kinds of stage positions: FIB milling positions (the spin mill positions) and SEM imaging positions. The ASV Spin Mill Angle Calculator UI includes tools for setting up both.

### Motivation

Some Spin Mill projects show a "wavy" (or "rippling") pattern in their SEM images. Two factors appear to contribute to the pattern: inconsistent FIB milling angles between the milling positions, and too high an electron dose at the sample surface.

Inconsistent milling angles are relatively simple to address. The spin mill positions need to be set up with the same milling angle at each position, which the standard setup does not guarantee (see [Consistent Milling Angles](#consistent-milling-angles)).

Lowering the electron dose is often more challenging. The dose can be lowered by reducing the SEM landing energy, the beam current, or both. Landing energy is usually the largest contributor to the wavy pattern. Lowering either setting, however, also lowers the signal-to-noise ratio of the images. Stage bias, when available, helps keep the signal-to-noise ratio high at lower landing energies and beam currents, but it is sensitive to the planarity of the sample relative to the SEM. To get the most from stage bias, each SEM imaging site, or region of interest (ROI), should be set up as close to perpendicular to the SEM as possible. Perpendicular ROIs are preferred even when stage bias is not used (see [SEM Imaging Positions](#sem-imaging-positions)).

This leaves two issues with the standard Spin Mill project setup: setting up the spin mill positions with a consistent milling angle, and positioning each ROI perpendicular to the SEM. The ASV Spin Mill Angle Calculator was created to help with both and, perhaps, reduce the wavy pattern in spin mill images.

### Consistent Milling Angles

A Spin Mill project uses at least three FIB milling positions per slice. The stage moves to each position in turn, and a FIB pattern runs there for a set duration. Milling at all of the positions completes one slice.

A milling angle is specified when the project is set up. The ASV 5 Define Spin Mill Position activity, a guided workflow, uses the same user-specified milling angle (and therefore the same stage tilt angle) for each position. This works if the sample surface is ideal, that is, parallel to the FIB at a stage tilt of -38° and perpendicular to the SEM at a stage tilt of 0°.

Most sample surfaces, however, are slightly tilted or non-planar relative to the FIB and SEM, so a constant stage tilt does not give the same milling angle at every position.

The FIB Angle Calc and Position Alignment pages help set up the milling positions with a consistent milling angle.

### SEM Imaging Positions

SEM imaging positions in a Spin Mill project are typically set up with the sample surface perpendicular to the SEM. A perpendicular ROI removes the need for dynamic focus and mitigates image distortions when stage bias is used.

The SEM Angle Calc page helps position the ROIs perpendicular to the SEM.

The following sections cover installing and running the script, and describe each page in the UI.

## Requirements

The script's requirements differ from the standard list under [Requirements](/scripts/#requirements) on the [Scripts](/scripts/) page. The script requires:

* PySide6 6.7.1+
* Python 3.11+
* NumPy 2.2.5+
* OpenCV 4.8.1+
* [TFS AutoScript 4.14+](https://www.thermofisher.com/us/en/home/electron-microscopy/products/software-em-3d-vis/autoscript-4-software.html) --- required only for the Position Alignment page, which connects to the microscope

The Python packages are all included with the AutoScript 4.14 environment, so no additional dependencies are needed if the script is run with AutoScript. The FIB Angle Calc and SEM Angle Calc pages, and simulation mode, do not need AutoScript.

The script was developed and tested with the AutoScript 4.14 and ASV 5.13.

## Installation

To install, follow the general steps under [Installing A Script](/scripts/#installing-a-script) on the [Scripts](/scripts/) page.

To use the Position Alignment page, which connects to the microscope, install the script on the Hydra Bio Support PC (SPC) or Microscope PC (MPC). For the other pages, the script can be installed on any compatible PC.

If the script is run outside the AutoScript environment, install the required packages into a fresh virtual environment from the `requirements.txt` file in the script's root directory:

```
pip install -r requirements.txt
```

### Updating the Script

Replace the script folder with the new release. Settings are stored in the Windows registry, so they persist between sessions and updates.

## Running the Script

The script's main file is `asv_spin_mill_angle_calculator.py`. See [Running A Script](/scripts/#running-a-script) on the [Scripts](/scripts/) page for how to run it with the AutoScript Python interpreter or the AutoScript Runner application.

### Simulation Mode

For testing and training, the Position Alignment page can be run with a simulated microscope. Simulation is never selected automatically; it is enabled only by an environment variable or a flag in `alignment_config.py`. When it is enabled, the Connect To Microscope switch connects to the simulated microscope and the connection status is marked "(sim)". The UI is fully navigable in simulation mode, but it cannot control a real microscope.

To enable simulation mode for a single launch, set the `ASV_SIMULATED_MICROSCOPE` environment variable to `1` before starting the script:

```
set ASV_SIMULATED_MICROSCOPE=1
python asv_spin_mill_angle_calculator.py
```

To force simulation mode on every launch, set `DEV_FORCE_SIMULATION` to `True` in `alignment_config.py` and restart the script.

{{< filetree/container >}}
    {{< filetree/folder name="ASV_Spin_Mill_Angle_Calculator-x.x.x" >}}
        {{< filetree/folder name="asv_spin_mill_angle_calc" >}}
            {{< filetree/file name="alignment_config.py" >}}
        {{< /filetree/folder >}}
    {{< /filetree/folder >}}
{{< /filetree/container >}}

```python {filename="alignment_config.py"}
DEV_FORCE_SIMULATION: bool = True
```

## Position Alignment Page

{{< img src="/asv_spin_mill_calculator/asv_spin_mill_calculator_position_alignment_1.png" caption="ASV Spin Mill Angle Calculator Position Alignment page." alt="ASV Spin Mill Angle Calculator Position Alignment page" width="90%" >}}

The Position Alignment page is shown when the application launches.

This page provides an automated spin mill position alignment tool for use with the ASV Define Spin Mill Position activity.

{{< callout type="info" >}}
The automated spin mill position alignment uses an ellipse finder algorithm, so it requires a clearly visible area of interest (AOI) circle (ellipse from the FIB view at low milling angles) on the sample surface. Run the Create AOI Marker activity in ASV to create one.
{{< /callout >}}

Follow these steps to use the automated spin mill position alignment.

{{% steps %}}

### Connect To Microscope

Turn on the Connect To Microscope switch (technically, this connects to the AutoScript server.)

### Set Parameters

Set the target milling angle, the AOI diameter, and the number of spin mill positions. Use the same values as in the Spin Mill project.

### Define Spin Mill Position Activity

Start the Define Spin Mill Position activity in ASV. The activity is in the Sample Preparation step.

The Define Spin Mill Position activity in ASV includes a guided workflow for setting up the spin mill positions. When ASV has moved the stage to a spin mill position and is ready to store the position, it displays a dialog.

{{< img src="/asv_spin_mill_calculator/asv_spin_mill_calculator_asv_dialog.png" caption="ASV Define SpinMill position dialog." alt="ASV Spin Mill Angle Calculator ASV Define SpinMill dialog" width="60%" >}}

### Start Position Alignment

{{< img src="/asv_spin_mill_calculator/asv_spin_mill_calculator_position_alignment_2.png" caption="Position Alignment page with the Start button enabled (connected to microscope)." alt="ASV Spin Mill Angle Calculator Position Alignment page enabled" width="90%" >}}

While the Define SpinMill position dialog is open, click the Start button on the Position Alignment page.

A dialog warns that the script is about to control the microscope. Check the Do not show this again box to skip the dialog for the rest of the session.

When ready, click the OK button to begin the automated position alignment.

{{< img src="/asv_spin_mill_calculator/asv_spin_mill_calculator_position_alignment_dialog.png" caption="Start position alignment dialog." alt="ASV Spin Mill Angle Calculator position alignment dialog" width="60%" >}}

### Position Alignment (Ellipse Finder)

{{< img src="/asv_spin_mill_calculator/asv_spin_mill_calculator_position_alignment_moving.png" caption="Position alignment in progress." alt="ASV Spin Mill Angle Calculator position alignment in progress" width="90%" >}}

While the alignment runs, the FIB View panel's border turns blue and shows FIB images during the process. The Status Log panel reports the alignment status.

{{< img src="/asv_spin_mill_calculator/asv_spin_mill_calculator_position_alignment_confirm.png" caption="Ellipse pattern matched and position ready for confirmation." alt="ASV Spin Mill Angle Calculator ellipse found" width="90%" >}}

If the position alignment procedure finds the ellipse (the pattern match is within the match criteria), the FIB View panel border color changes to green, the aligned milling angle is displayed in the status bar, and the Update and Confirm buttons are enabled.

If the match is satisfactory, click the Confirm button to save the position in the Spin Mill Positions panel. To override the aligned position, adjust it manually in the Microscope Control software, click the Update button to capture the new position, then click Confirm.

Finally, click the OK button in the ASV Define SpinMill position dialog. ASV saves the current stage position, beam shift, and scan rotation, then moves to the next spin mill position.

Repeat steps 3 to 5 for each spin mill position that ASV defines.

{{% /steps %}}

### Position Alignment Results

{{< img src="/asv_spin_mill_calculator/asv_spin_mill_calculator_position_alignment_5sites.png" caption="Position alignment complete for all five spin mill positions." alt="ASV Spin Mill Angle Calculator position alignment complete" width="90%" >}}

Each saved spin mill position is listed in the Spin Mill Positions panel. Click the chevron to the left of a position to update it or drive to it (similar to the controls in ASV). Scroll the panel to the right for more details, including the calculated milling angle.

The average milling angle for all spin mill positions is shown in the upper-right corner of the Spin Mill Positions panel.

The Calculated SEM Positions panel shows SEM positions, derived from the saved spin mill positions, at which the sample is nearly perpendicular to the SEM. Use them when setting up the SEM steps in the ASV Spin Mill project.

## FIB Angle Calc Page

{{< img src="/asv_spin_mill_calculator/asv_spin_mill_calculator_fib_calc_1.png" caption="The FIB Angle Calc page." alt="ASV Spin Mill Angle Calculator FIB Angle Calc page" width="90%" >}}

The FIB Angle Calc page includes a milling angle calculator and two cartoon panels.

During the Define Spin Mill Position activity in ASV, and when an AOI ellipse is visible, an ellipse annotation tool can be used in the Microscope Control software to measure the width and height of the ellipse. Enter the target milling angle, AOI diameter, and measured ellipse height, and the page displays the calculated milling angle.

The 2D View panel shows a side view of the milling angle relative to the AOI circle/ellipse, the SEM, and the FIB. The FIB View panel shows a cartoon of the AOI circle as it appears from the FIB's perspective. At low milling angles, the circle appears elliptical.

## SEM Angle Calc Page

{{< img src="/asv_spin_mill_calculator/asv_spin_mill_calculator_sem_calc_1.png" caption="The SEM Angle Calc page." alt="ASV Spin Mill Angle Calculator SEM Angle Calc page" width="90%" >}}

The SEM Angle Calc page calculates SEM positions from the metadata of spin mill position images saved by the ASV Define Spin Mill Position activity.

Image logging must be enabled first: in ASV Settings, open the Logging page and check Log Spin Mill Position Images.

The images are saved under the ASV project directory:

{{< filetree/container >}}
    {{< filetree/folder name="Spin Mill Project" >}}
        {{< filetree/folder name="ImageLogs" >}}
            {{< filetree/folder name="Site Name" >}}
                {{< filetree/folder name="Sample Preparation" >}}
                    {{< filetree/folder name="Define Spin Mill Position" >}}
                        {{< filetree/folder name="AutoSliceAndView.Services.Services.Positioning.SpinMillPositioningService" >}}
                        {{< /filetree/folder >}}
                    {{< /filetree/folder >}}
                {{< /filetree/folder >}}
            {{< /filetree/folder >}}
        {{< /filetree/folder >}}
    {{< /filetree/folder >}}
{{< /filetree/container >}}

To calculate SEM positions, set the target milling angle, click the Load Spin Mill Position Images button, and select the images corresponding to the spin mill positions. The SEM positions are calculated as soon as the images load. The Spin Mill Positions panel displays metadata from each image; use the horizontal scrollbar to see more.

The Calculated SEM Positions panel displays the calculated and measured positions. Use them to set up sites in the ASV Spin Mill project that are nearly perpendicular to the SEM.

The Results panel displays details of the calculations.

Below is an example of results calculated from five spin mill position images.

{{< img src="/asv_spin_mill_calculator/asv_spin_mill_calculator_sem_calc_2.png" caption="Example of calculated SEM positions derived from five spin mill position images." alt="ASV Spin Mill Angle Calculator SEM Angle Calc page results" width="90%" >}}

## Settings Page

{{< img src="/asv_spin_mill_calculator/asv_spin_mill_calculator_settings_1.png" caption="The Settings page." alt="ASV Spin Mill Angle Calculator Settings page" width="90%" >}}

All application parameters are in the `alignment_config.py` file, in the `asv_spin_mill_angle_calc` directory under the script root.

The Settings page exposes a few of the main parameters so they can be adjusted without editing the file or restarting the application.