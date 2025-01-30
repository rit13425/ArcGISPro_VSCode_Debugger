# ArcGIS Pro Visual Studio Code Debugger

## About the Extension

This extension allows debugging ArcGIS Pro script tools (.atbx, .pyt) using Visual Studio Code (VSC).

Our aim is to provide developers a seamless workflow to attach VSC to the Python process live in 
ArcGIS Pro. This integration allows developers to leverage VSC's native Python debugging experience while developing
script tools.

## Features
- Easily attach to ArcGIS Pro and stand-alone Python processes
- Debug ArcGIS Pro Script Tools (.atbx) and Python Toolboxes (.pyt)
- Debug code on the local machine or remotely on a different machine

## Download the Latest Version

> [!IMPORTANT]
> Requires ArcGIS Pro 3.5 or greater

[![Button Icon]][Button Link]

[Button Icon]: https://img.shields.io/badge/Download_v1.1.12-blue?style=for-the-badge&logoColor=white
[Button Link]: https://devtopia.esri.com/ArcGISPro/Python/files/58991/arcgis-pro-debugger-1.1.12.zip

## Installation

1. Download the latest ArcGIS Pro Debugger extension (.vsix):  

    **NOTE:** It will download a .zip archive
2. Unzip the downloaded archive
3. Now install the extension in Visual Studio Code:
    1. Open Visual Studio Code
    2. Open the Extensions pane
    3. Click on the three dots (...) in the top-right corner of the Extensions pane.  
    4. Select the Install from VSIX option    
    ![image](https://github.com/user-attachments/assets/80ec150f-f545-426a-912a-ac4298bc5bab)
    6. Navigate to the directory where the .vsix file is located
    7. Select the .vsix file and click Open
4. After the extension is installed, you may need to reload Visual Studio Code to activate the extension. You can do this by using the Command Palette (Ctrl+Shift+P or Cmd+Shift+P) and selecting Developer: Reload Window.  

    **Note:** You should see popups indicating the directory where ArcGIS Pro is installed and the Python interpreter path. Additionally, an ArcGIS Pro Debugger will appear in the status bar (right).  
    ![image](https://github.com/user-attachments/assets/dfebe000-e7f6-4b3b-8acb-0db263c616a7)

**Note:** After unzipping the .vsix file you can alternatively drag and drop it to the extension section of VSC to install it.

## Local Debugging

> [!IMPORTANT]  
> - Requires VSC and the following additional extensions:
>    - ArcGIS Pro Debugger
> - Requires ArcGIS Pro

1. **Open VSC**
2. **Open your project folder in VSC**:  
    1. From the menu select **File** > **Open Folder**  

    **NOTE:** This will generate a `.vscode` folder in that workspace (if it is not already there). This folder will be populated with settings injected by the extension.  

    **NOTE:** You must have read/write-permission to this workspace. You may need to start VSC as administrator.  
4. **Open your script tool in ArcGIS Pro**:
    1. Open your ArcGIS Pro project (.aprx)
    2. Open the tool you are debugging
5. **Open your script**: 
    1. In VSC, open the `.py` file associated with your `.atbx`, or the `.pyt` file, that you wish to debug.  

    **NOTE:** For script tools (`.atbx`), some additional setup might be required to [debug execution code](#debug-script-tool-execution-code) or [validation code](#debug-script-tool-validation-code). No additional setup is required for python toolboxes (`.pyt`).
3. **Set Python interpreter**:
    1. Press `Ctrl+Shift+P` to open the command palette
    2. Choose **Python: Select Interpreter**
    3. Select the currently active ArcGIS Python interpreter from the dropdown  

    **NOTE:** You can alternatively click the **Python** icon in the bottom right of VSC to select the interpreter.  

    **NOTE:** It could be `arcgispro-py3` or a clone of it. Use the **Package Manager** in ArcGIS Pro to determine the active environment.
6. **Open the Debug Console**: 
    1. From the menu choose **View > Debug Console**.  

    **NOTE:** The debug console opens. Debug messages from the ArcGIS Pro debugger extension will appear here.  
7. **Attach to a running ArcGIS Pro instance using process ID**:  
    1. Press `Ctrl+Shift+P` to open the command pallette
    2. Run the command **ArcGIS Pro Debugger: Attach Debugger to ArcGIS Pro**
    3. Select the ArcGIS Pro process from the dropdown  
    4. If you have multiple workspaces open in the Explorer tab, select the current workspace from the dropdown list.  

    **NOTE:** You will see attaching messages appear in the **Debugger Console**.  

    **NOTE:** Once attached, the **Debug Toolbar** should appear with a socket icon indicating that you are attached and debugging.  

    **NOTE:** If you have not opened a script tool, attaching will fail (more precisely, if the Python environment has not been initialized, there is no Python.exe process to attach to, see https://pro.arcgis.com/en/pro-app/latest/arcpy/get-started/activate-an-environment.htm)

    **NOTE:** Alternatively, you can also click on the **ArcGIS Pro Debugger** item in the status bar to perform this action.
8. **Debugging your script**:  
    1. Run the `.atbx` or `.pyt` script from ArcGIS Pro. The execution will pause at the breakpoints set in VSC, allowing you to debug the script.  

## Remote Debugging

You can debug your script tool while it is running on another machine (of your coworker, for example). This may be easier than attempting to reproduce the conditions in which the script is failing on your own machine.

> [!IMPORTANT]  
> _On the local (client, initiating) machine_
> - Requires VSC and the following additional extensions:
>    - ArcGIS Pro Debugger
> - Requires OpenSSH Client  
>
> _On the remote (server, target) machine_
> - Requires VSC and the following additional extensions:
>    - ArcGIS Pro Debugger
> - Requires OpenSSH Server
> - Requires ArcGIS Pro >= 3.4

## Debug Script Tool Execution Code

## Debug Script Tool Validation Code
