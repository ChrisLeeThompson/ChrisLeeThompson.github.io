---
title: Arctis Angle Calculator
breadcrumbs: false
version: "1.2.3"
repo: Arctis_Angle_Calculator
---

Releases on GitHub: [Arctis Angle Calculator](https://github.com/ChrisLeeThompson/Arctis_Angle_Calculator/releases)

Latest version: {{< version-badge >}}

## Introduction

The Arctis Angle Calculator is a Python UI for calculating and visualizing the stage tilt and milling angles of a [Thermo Fisher Scientific (TFS) Arctis](https://www.thermofisher.com/us/en/home/electron-microscopy/products/dualbeam-fib-sem-microscopes/arctis-cryo-pfib.html) SEM-FIB microscope.

The following sections cover installing and running the script, and briefly describe the UI and its features.

## Requirements

The script has the standard requirements --- AutoScript, PySide, and Python --- listed under [Requirements](/scripts/#requirements) on the [Scripts](/scripts/) page. AutoScript is required for any function that connects to the microscope.

## Installation

To install, follow the general steps under [Installing A Script](/scripts/#installing-a-script) on the [Scripts](/scripts/) page.

The script can be used with or without a connection to an Arctis. To get or set the stage tilt angle, the script must be connected to the microscope; in that case, it is best installed on the Arctis Support PC (SPC) or the Microscope PC (MPC). Without a microscope connection, the script can be installed on any compatible PC.

## Running the Script

The script's main file is `arctis_angle_calculator.py`. See [Running A Script](/scripts/#running-a-script) on the [Scripts](/scripts/) page for how to run it with the AutoScript Python interpreter or the AutoScript Runner application.

## Overview of the UI and Features

The UI consists of three sections: controls, stage graphics, and sample graphics.

{{< img src="/arctis_angle_calculator/arctis_angle_calculator_1.png" caption="The Arctis Angle Calculator UI." alt="Arctis Angle Calculator UI" width="90%" >}}

### Controls

{{< img src="/arctis_angle_calculator/arctis_angle_calculator_top_panel_controls.png" caption="Controls (top groupbox)." alt="Top groupbox controls"  >}}

The Controls section includes the Milling Angle and Alpha Tilt (stage tilt) spinboxes, Get and Go To buttons, a Connect To Microscope switch, and a Back Of Grid (BOG) Milling Angle switch. The Milling Angle and Alpha Tilt spinboxes are used to set the target alpha tilt of the stage and tilt the graphics in the other two groupboxes.

The Connect To Microscope switch is used to connect to an AutoScript server installed on an Arctis MPC. When the script is connected to the server, the Get and Go To buttons are enabled. The Get button retrieves the current stage tilt angle, and the Go To button tilts the stage to the angle set in the Alpha Tilt spinbox.

The milling angle is calculated as `38° + alpha tilt`. The BOG Milling Angle switch changes this calculation for back-of-grid milling: when the switch is on and the alpha tilt is below -128°, the milling angle is instead calculated as `-180° - (38° + alpha tilt)`. With the switch off, or at any tilt of -128° or above, the standard calculation applies. The 38° offset is because the FIB is 38° from the stage plane at 0° alpha tilt.

The alpha tilt range is -190° to +10°. With BOG switched on, typing a milling angle into the Milling Angle spinbox also resolves to the back-of-grid alpha tilt where one exists.

{{< img src="/arctis_angle_calculator/arctis_angle_calculator_3.png" caption="Back Of Grid (BOG) Milling Angle switched off." alt="BOG Milling Angle switched off." width="90%" >}}

{{< img src="/arctis_angle_calculator/arctis_angle_calculator_4.png" caption="Back Of Grid (BOG) Milling Angle switched on." alt="BOG Milling Angle switched on." width="90%" >}}

### Stage Graphics

The Stage Graphics panel shows a 2D diagram of a cross-section of a grid and sample (the sample is blue) clipped into an AutoGrid. Each label in the diagram (SEM, FIB, GIS, and iFLM) can be clicked to tilt the stage graphic to its associated angle. The mouse scroll-wheel can also be used to tilt the stage graphic.

The positions of the SEM, FIB, GIS, and iFLM are intended to show their positions and angles relative to the grid/stage. Note the sputter coater is not shown.

{{< callout type="info" >}}
The GIS position is shown in 2D. The graphic ignores the angle between the GIS and the FIB along the z-axis, which points out of the screen.
{{< /callout >}}

{{< img src="/arctis_angle_calculator/arctis_angle_calculator_left-panel.png" caption="Stage graphics (left groupbox)." alt="Stage graphics groupbox" width="90%" >}}

### Sample Graphics

The Sample Graphics panel shows a 2D cartoon representing a zoomed portion of a sample on the grid. The tilt of the sample cartoon is linked to the alpha/stage tilt angle.

{{< img src="/arctis_angle_calculator/arctis_angle_calculator_right-panel.png" caption="Sample graphics (right groupbox)." alt="Sample graphics groupbox" width="90%" >}}

Named after the construction tool, the Add Chalk Line button draws a chalk line onto the sample graphic along the FIB line. This can be helpful for visualizing FIB workflows and the angles of the SEM, FIB, and GIS relative to the FIB milling angle(s). Multiple chalk lines can be drawn. Click the Remove Last button to remove the most recently drawn chalk line; it is disabled when there are no chalk lines to remove.

When there is a stage tilt at which a chalk line becomes perpendicular or parallel to the SEM, FIB, or GIS, a chip is displayed next to the associated label. Clicking a chip moves the graphic to that tilt position; hovering over it shows the tilt angle in a tooltip.

{{< img src="/arctis_angle_calculator/arctis_angle_calculator_right-panel_full.png" caption="Stage tilted for a standard 15° milling angle, with a chalk line drawn." alt="Stage tilted for a standard 15° milling angle, with a chalk line drawn." width="90%" >}}

{{< img src="/arctis_angle_calculator/arctis_angle_calculator_sample_parallel-to-SEM.png" caption="15° milling angle chalk line tilted parallel with the SEM." alt="15° milling angle parallel with the SEM." width="90%" >}}

{{< img src="/arctis_angle_calculator/arctis_angle_calculator_sample_perpendicular-to-SEM.png" caption="15° milling angle chalk line tilted perpendicular to the SEM." alt="15° milling angle perpendicular to the SEM." width="90%" >}}