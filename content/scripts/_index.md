---
title: Scripts
weight: 2
sidebar:
    open: false
breadcrumbs: false
---

These scripts are developed from experience with Thermo Fisher Scientific (TFS) SEM-FIB microscopes, and they are designed to be used with TFS microscopes and software. 

The scripts are intended to be helpful additions to an electron microscopist's toolkit.

{{< callout type="info" >}}
All of the scripts are in development (some are more in a state of development than others). Check this page and the [Script Menu](#script-menu) for updates from time to time.
{{< /callout >}}

## Acknowledgements

These scripts benefited from support and helpful feedback from colleagues (TFS and St. Jude Children's Research Hospital), friends, and other users. Thank you for everyone's assistance! Thank you, Arthur Alves De Melo, for your helpful ideas and assistance with the ASV Project Explorer and ATC Project Explorer.

Most of the scripts started as simple user interfaces as I learned Python and PySide. As I continue to learn Python, I have recently started to work with Anthropic's Claude to rapidly update the original scripts and build new ones.

## Catbug

{{< columns >}}
    {{< column >}}
        The [Bravest Warriors](https://www.youtube.com/user/BravestWarriors) character, Catbug, is featured in these scripts. Perhaps Catbug helps to brighten your day, or brings a smile! Catbug was introduced in Bravest Warriors [season 1, episode 11](https://share.google/56Qc40XuCUYUuRYv9).

        Catbug and Bravest Warriors are copyrighted by Frederator Networks, Inc. The character was created by writer and director Breehn Burns and designed by artist Bob Flynn.
    {{< /column >}}
    {{< column >}}
        {{< img src="/catbug/catbug_color_2.svg" width="60%" >}}
    {{< /column >}}
{{< /columns >}}

## Requirements

All of the scripts were developed with Python packages included in the [TFS AutoScript 4.14](https://www.thermofisher.com/us/en/home/electron-microscopy/products/software-em-3d-vis/autoscript-4-software.html) environment. If a script is run with AutoScript, no additional dependencies are necessary.

Unless noted otherwise on a script's page, a script requires the following:

* [TFS AutoScript 4.14+](https://www.thermofisher.com/us/en/home/electron-microscopy/products/software-em-3d-vis/autoscript-4-software.html) --- required for any function that connects to the microscope
* PySide6 6.7.1+
* Python 3.11+

The scripts are developed and tested in the AutoScript 4.14 Python environment (Python 3.11.14, PySide6 6.7.1). Package versions listed on the script pages are the versions that environment includes.

Many of the scripts are designed to connect to a TFS microscope, and those scripts require AutoScript to function properly. Some of the scripts do not connect to a microscope at all, or connect for only some of their functionality. For these scripts, the minimum Python package requirements are listed on the script's page.

## Installing A Script

Each script is installed the same way:

1. Download the latest version from the GitHub releases link at the top of the script's page.
2. Extract the contents of the zip archive.
3. Copy the script directory to your desired location.

Scripts that connect to a TFS microscope are best installed on the microscope's Support PC (SPC) or Microscope PC (MPC). Which PC to use depends on the type of AutoScript installation; see the AutoScript Reference Manual for more information.

Scripts that do not connect to a microscope can be installed on any PC that meets the requirements listed on the script's page.

## Running A Script

A script can be run with a compatible Python interpreter, the AutoScript Python interpreter, or the AutoScript Runner application. Each script is started from its main `.py` file, which is named on the script's page. The main `.py` file will be in the script's root directory and have the same name as the script name.

### AutoScript Python Interpreter

Run the script's main `.py` file using the AutoScript Python interpreter. If `.py` files are executed using the interpreter by default, you can double-click the file to start the script.

The AutoScript Python interpreter is usually located in the AutoScript directory on the SPC or MPC: `C:\Program Files\Enthought\Python\envs\AutoScript`.

### AutoScript Runner

The AutoScript Runner application can also be used to run a script. Open the Runner, navigate to the script root directory, select the script's main `.py` file, and click Run to start the script.

## Script Menu

{{< cards cols="2" >}}
    {{< card image="/aquilos2_cryo_utilities/aquilos2_utilities_cryo_prep_page_small.png" method="Fill" options="600x480 webp q80 TopLeft" imageStyle="width:100%" link="/scripts/aquilos2_cryo_utilities/" title="Aquilos 2 Cryo Utilities" alt="Aquilos 2 Cryo Utilities card" >}}
    {{< card image="/arctis_angle_calculator/arctis_angle_calculator_1.png" method="Fill" options="600x480 webp q80 TopLeft" imageStyle="width:100%" link="/scripts/arctis_angle_calculator/" title="Arctis Angle Calculator" alt="Arctis Angle Calculator card" >}}
    {{< card image="/asv_project_explorer/asv_project_explorer_slice_link_2.png" method="Fill" options="600x480 webp q80 TopLeft" 
    imageStyle="width:100%" link="/scripts/asv_project_explorer/" title="ASV Project Explorer" alt="ASV Project Explorer card" >}}
    {{< card image="/asv_spin_mill_calculator/asv_spin_mill_calculator_fib_calc_1.png" method="Fill" options="600x480 webp q80 TopLeft" imageStyle="width:100%" link="/scripts/asv_spin_mill_angle_calculator/" title="ASV Spin Mill Angle Calculator" alt="ASV Spin Mill Angle Calculator card" >}}
    {{< card image="/atc_project_explorer/atc_project_explorer_lamella_page_1.png" method="Fill" options="600x480 webp q80 TopRight" imageStyle="width:100%" link="/scripts/atc_project_explorer/" title="ATC Project Explorer" alt="ATC Project Explorer card" >}}
    {{< card image="/atc_monitor/atc_monitor_ui_pattern_1_start.png" method="Fill" options="600x480 webp q80 TopRight" imageStyle="width:100%" link="/scripts/atc_monitor/" title="ATC Monitor" alt="ATC Monitor card" >}}
    {{< card image="/hydra_bio_cryo_utilities/hydra_bio_utilities_cryo_prep_page_small.png" method="Fill" options="600x480 webp q80 TopLeft" imageStyle="width:100%" link="/scripts/hydra_bio_cryo_utilities/" title="Hydra Bio Cryo Utilities" alt="Hydra Bio Cryo Utilities card" >}}
    {{< card image="/hydra_nsr_cryo_utilities/hydra_nsr_utilities_cryo_prep_page_small.png" method="Fill" options="600x480 webp q80 TopLeft" imageStyle="width:100%" link="/scripts/hydra_nsr_cryo_utilities/" title="Hydra NSR Cryo Utilities" alt="Hydra NSR Cryo Utilities card" >}}
    {{< card image="/iflm_slice_interpolator/iflm_slice_interpolator_card.png" method="Fill" options="600x480 webp q80 TopCenter" imageStyle="width=100%" link="/scripts/iflm_slice_interpolator/" title="iFLM Slice Interpolator" alt="iFLM Slice Interpolator card" >}}
    {{< card image="/tfs_label_maker/tfs_label_maker_2.png" method="Fill" options="600x480 webp q80 TopLeft" imageStyle="width:100%" link="/scripts/tfs_label_maker/" title="TFS Label Maker" alt="TFS Label Maker card" >}}
{{< /cards >}}
