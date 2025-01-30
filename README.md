# ArcGIS Pro Visual Studio Code Debugger Extension

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

[Button Icon]: https://img.shields.io/badge/Beta-Download_v1.1.12-blue?style=for-the-badge&logoColor=white
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

Debug your scripts on your own machine as you develop your script tool.

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

Debug your script tool while it is running on another machine (of your coworker, for example). It may be easier to debug this way than attempting to reproduce the conditions in which the script is failing on your own machine.

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

1. **Start the SSH service:**  
    _On the remote machine_
    1. Open the PowerShell as administrator
    2. Run `Start-Service sshd`  

    **NOTE:** When you no longer need the SSH service, run `Stop-Service sshd`.  

    **NOTE:** You can run `Get-Service -Name sshd` to confirm service is running or stopped.  

    **NOTE:** Keep the PowerShell open, you will use it in the next step.
2. **Find the hostname or IP address of your remote machine:**  
    _On the remote machine_
    1. Run the command `hostname` and note the name  
    -or-  
    2. Run the command `ipconfig` and note the IPv4 address (formatted `X.X.X.X`, where each `X` is a number between 0 and 255).  

    **NOTE:** You will use either the host name or the IP address to make an SSH connection from your local to your remote machine.  
3. **Set up a remote listening port for debugpy in your ArcGIS Pro session:**  
    _On the remote machine_  
    1. Open your ArcGIS Pro project (.aprx)  
    2. Open the Python Window  
    3. Run the following code:
    ``` python
    import debugpy
    debugpy.configure(python=r"<path\to\active-env\python.exe>")
    debugpy.listen(<port>)
    ```  

    **NOTE:** Replace `<path\to\active-env\python.exe>` with the path to the active Python interpreter.  

    **NOTE:** Replace `<port>` with an open port, for example, try `5678`.  

    **NOTE:** If successful, you should see something like `('123.0.0.4', 5678)` printed to the output. **IMPORTANT:** This is not the same as the IP address obtained earlier.  
4. **Open VSC** _on the local machine_
5. **Select the ArcGIS Pro Debugger configuration type:**  
    _On the local machine_  
    1. Press `Ctrl+Shift+P` to open the command palette  
    2. Select **ArcGIS Pro Debugger: Select Configuration Type**  
    3. Choose **Attach using Port**  

    **NOTE:** The default configuration is **Attach using Process ID**, this is recommended for local debugging. For remote debugging, **Attach using Port** is recommended.
6. **Add an SSH host (skip if this was already done previously):**  
    _On the local machine_  
    1. Press `Ctrl+Shift+P` to open the command palette  
    2. Select **ArcGIS Pro Debugger: Remote Development**  
    3. Select **Add New SSH Host...**
    4. Enter the host name or IP address noted earlier and press enter
    5. Choose an option for how SSH host configuration is stored (select first option if unsure)  

    **NOTE:** Once connected, you will see SSH: <hostname/IP> on the lower left of the status bar. You can now open and edit scripts remotely.  

    **NOTE:** Alternatively, you can also click on the ArcGIS Pro Debugger in the status bar to perform this action.  
7. **Make an SSH connection:**  
    _On the local machine_  
    1. Press `Ctrl+Shift+P` to open the command palette  
    2. Select **ArcGIS Pro Debugger: Remote Development**  
    3. Select the remote machine from the list  
    4. Select the remote machine operating system from the list  

    **NOTE:** A separate VSC instance may open, continue there with all steps going forward  
    5. Enter the password and press enter  

    **NOTE:** This instance (window) of VSC is now accessing the remote machine.  
8. **Ensure all necessary components are installed on remote machine**  
    _On the local machine_  
    1. Click the Extensions tab and make sure the ArcGIS Pro Debugger extension components are installed on the SSH host  

    **NOTE:** From the Extensions tab you will see extensions installed on the remote machine. This tab will also show you extensions that are missing on the remote machine, make sure the required extensions are installed on the remote machine before proceeding (refer to the list of required extensions provided earlier). For the ArcGIS Pro Debugger extension item in the list, you may have to click on the **Install in SSH:<hostname/IP>** to install required components.  
7. **Open the remote project folder**:  
    _On the local machine_  
    **NOTE: You may skip this step if you already have a folder opened  
    1. From the menu select File > Open Folder. You can alternatively do this from the **Explorer** tab.  

    **NOTE:** The drop-down list shows folders on the remote machine  
    2. Select the project folder containing the scripts you want to debug 
    3. If prompted, re-enter the password  

    **NOTE:** From the **Explorer** tab you will see the directory tree of the remote machine. You can now open and edit scripts remotely.  

    **NOTE:** This will generate a `.vscode` folder in that workspace (if it is not already there). This folder will be populated with settings injected 
by the extension.  

    **NOTE:** You must have read/write-permission to this workspace. You may need to start VSC as administrator.  
8. **Open your script**:  
    _On the local machine_  
    1. In VSC, open the `.py` file associated with your `.atbx`, or the `.pyt` file, that you wish to debug.  

    **NOTE:** For script tools (`.atbx`), some additional setup might be required _on the remote machine_ to [debug execution code](#debug-script-tool-execution-code) or [validation code](#debug-script-tool-validation-code). No additional setup is required for python toolboxes (`.pyt`).  
9. **Set Python interpreter**:  
    _On the local machine_  
    1. Press `Ctrl+Shift+P` to open the command palette
    2. Choose Python: Select Interpreter
    3. Select the currently active ArcGIS Python interpreter (of the remote machine) from the dropdown  

    **NOTE:** You can alternatively click the Python icon in the bottom right of VSC to select the interpreter.  

    **NOTE:** It could be `arcgispro-py3` or a clone of it. Use the Package Manager in ArcGIS Pro _on the remote machine_ to determine the active environment.  
10. **Open the Debug Console**: 
    1. From the menu choose **View > Debug Console**.  

    **NOTE:** The debug console opens. Debug messages from the ArcGIS Pro debugger extension will appear here.  

    **NOTE:** Alternatively, you can also click on the **ArcGIS Pro Debugger** in the status bar to perform this action.  
12. **Attach to a running ArcGIS Pro instance using a debug port**:  
    _On the local machine_  
    1. Press `Ctrl+Shift+P` to open the command palette
    2. Run the command **ArcGIS Pro Debugger: Attach Debugger to ArcGIS Pro**
    3. Enter the port number you chose in an earlier step  
    4. If you have multiple workspaces open in the Explorer tab, select the current workspace from the dropdown list.  

    **NOTE:** You should begin to see messages appear in the debug console. Once successfully attached, the debug toolbar should appear with a socket icon indicating that you are attached and debugging.  

    **NOTE:** Once attached, the **Debug Toolbar** should appear with a socket icon indicating that you are attached and debugging.  

    **NOTE:** Alternatively, you can also click on the **ArcGIS Pro Debugger** in the status bar to perform this action.  

    **NOTE:** If you have not opened a script tool, attaching will fail (more precisely, if the Python environment has not been initialized, there is no Python.exe process to attach to, see https://pro.arcgis.com/en/pro-app/latest/arcpy/get-started/activate-an-environment.htm)
13. **Run your script tool in ArcGIS Pro**:  
    _On the remote machine_  
    1. Open the tool you are debugging  
    2. Run the tool

    **NOTE:** Execution of the tool will stop when a breakpoint is hit, in the UI _on the remote machine_ it will appear to have paused.  
14. **Debugging your script**:  
    _On the local machine_  
    1. The execution will pause at the breakpoints set in VSC, allowing you to debug the script.  

    **Note**: For `.pyt` scripts, specifically within the `execute` function, you will need `arcpy.SetupDebugger()` to trigger breakpoints. Injecting these will automatically be set up and torn down by the debugger when connecting and disconnecting.  


## Debug Script Tool Execution Code

If the script tool's execution code is embedded, you must first export the script from the toolbox to a Python file (`.py`) before you can debug it. Follow these steps to debug the script tool's execution code:

1. Open the script tool properties dialog and navigate to the **Execution** tab.
2. If the execution script is embedded, click the **Export script to file** button.
3. Continue debugging using the exported Python script file.
4. When you are done debugging, don't forget to embed the script again if it was previously embedded.

## Debug Script Tool Validation Code

Python code in the script tool validation is embedded in the tool and needs to be copied to an external Python file (.py) to debug it. You can then open the Python file in your IDE and set breakpoints, attach the IDE to ArcGIS Pro, and run your script tool. Upon completion of code modification, copy the contents of the Python file back into the tool validation.

1. Open the script tool properties dialog and navigate to the **Validation** tab.
2. Copy the contents of the code editor into a Python file (validation_code.py in the sample code).
3. Replace the contents of the code editor in the **Tool Properties** dialog **Validation** tab with the sample code below:  
    In the following example, validation_code.py (contents not shown) contains the `ToolValidator` class and is saved to the same directory as the toolbox.  
    ``` python
    import os
    import sys
    import validation_code

    sys.path.append(os.getcwd())  # directory containing module must be added to `sys.path`

    # the following code will reload the val.py module if it's modified
    if 'validation_code' in sys.modules:
        import importlib
        importlib.reload(validation_code)

    # ToolValidator should exist at the global scope
    ToolValidator = validation_code.ToolValidator
    ```
4. Continue debugging using the validation_code.py file.
5. When you are done debugging, replace the contents of the code editor in the tool properties dialog Validation tab with those of your Python file (validation_code.py in the sample code).
