.. _settings:

ESP-IDF Settings
================

:link_to_translation:`zh_CN:[中文]`

This extension contributes the following settings that can be later updated in ``settings.json`` or from VS Code Settings Preference Menu by:

- Navigate to **View** > **Command Palette**.

- Type **Preferences: Open Settings (UI)** and select the command to open the setting management window.

.. note::

    Please note that ``~``, ``%VARNAME%`` and ``$VARNAME`` are not recognized when set on ANY of this extension configuration settings. Instead, you can set any environment variable in the path using ``${env:VARNAME}``, such as ``${env:HOME}``, or refer to other configuration parameter path with ``${config:SETTINGID}``, such as ``${config:idf.espIdfPath}``.

The **idf.saveScope** allows you to specify where to save settings when using commands such as **Set Espressif Device Target** and other commands. Possible values are Global (User Settings), Workspace and WorkspaceFolder. Use the **Select where to Save Configuration Settings** command to choose where to save settings when using this extension commands.

.. note::

    All settings can be applied to Global (User Settings), Workspace and WorkspaceFolder unless Scope is specified.

ESP-IDF Specific Settings
-------------------------

These are the configuration settings that ESP-IDF extension contributes to your Visual Studio Code editor settings.

.. list-table::
    :widths: 10 20
    :header-rows: 1

    * - Setting ID
      - Description
    * - idf.buildPath
      - Custom build directory name for extension commands (Default: \${workspaceFolder}/build)
    * - idf.buildPathWin
      - Custom build directory name for extension commands in Windows (Default: \${workspaceFolder}\\build)
    * - idf.sdkconfigFilePath
      - Absolute path for the sdkconfig file
    * - idf.sdkconfigDefaults
      - List of sdkconfig default values for initial build configuration
    * - idf.cmakeCompilerArgs
      - Arguments for CMake compilation task
    * - idf.customExtraVars
      - Variables to be added to system environment variables
    * - idf.gitPath
      - Path to the Git executable
    * - idf.gitPathWin
      - Path to the Git executable in Windows
    * - idf.enableCCache
      - Enable CCache in build task (make sure CCache is in PATH)
    * - idf.enableIdfComponentManager
      - Enable IDF Component manager in build command
    * - idf.espIdfPath
      - Path to locate ESP-IDF framework (IDF_PATH)
    * - idf.espIdfPathWin
      - Path to locate ESP-IDF framework in Windows (IDF_PATH)
    * - idf.ninjaArgs
      - Arguments for Ninja build task
    * - idf.pythonInstallPath
      - System Python absolute path used to compute ESP-IDF Python virtual environment
    * - idf.toolsPath
      - Path to locate ESP-IDF Tools (IDF_TOOLS_PATH)
    * - idf.toolsPathWin
      - Path to locate ESP-IDF Tools in Windows (IDF_TOOLS_PATH)

This is how the extension uses them:

1. **idf.customExtraVars** stores any custom environment variable such as OPENOCD_SCRIPTS, which is the openOCD scripts directory used in OpenOCD server startup. These variables are loaded to this extension command's process environment variables, choosing the extension variable if available, else extension commands will try to use what is already in your system PATH. **This doesn't modify your system environment outside Visual Studio Code.**
2. **idf.espIdfPath** (or **idf.espIdfPathWin** in Windows) is used to store ESP-IDF directory path within our extension. We override Visual Studio Code process IDF_PATH if this value is available. **This doesn't modify your system environment outside Visual Studio Code.**. It is also used to compute the list of ESP-IDF tools to add to environment variable PATH and the Python virtual environment path together from **idf.toolsPath** and **idf.pythonInstallPath**.
3. **idf.pythonInstallPath** is the system Python absolute path used to compute ESP-IDF Python virtual environment from **idf.toolsPath** and **idf.espIdfPath** where ESP-IDF Python packages will be installed and used.
4. **idf.gitPath** (or **idf.gitPathWin** in Windows) is used in the extension to clone ESP-IDF master version or the additional supported frameworks such as ESP-ADF, ESP-MDF and Arduino-ESP32.
5. **idf.toolsPath** (or **idf.toolsPathWin** in Windows) is used to compute the list of ESP-IDF tools to add to environment variable PATH and the Python virtual environment path together from **idf.pythonInstallPath** and **idf.espIdfPath**.

.. note::

    From Visual Studio Code extension context, we can't modify your system PATH or any other environment variable. We use a modified process environment in all of this extension tasks and child processes which should not affect any other system process or extension. Please review the content of **idf.customExtraVars** in case you have issues with other extensions.

Board/Chip Specific Settings
----------------------------

These settings are specific to the ESP32 Chip/Board.

.. list-table::
    :widths: 25 75
    :header-rows: 1

    * - Setting
      - Description
    * - **idf.flashBaudRate**
      - Flash Baud Rate
    * - **idf.monitorBaudRate**
      - Monitor Baud Rate (Empty by default to use SDKConfig ``CONFIG_ESP_CONSOLE_UART_BAUDRATE``)
    * - **idf.openOcdConfigs**
      - Configuration files for OpenOCD, relative to ``OPENOCD_SCRIPTS`` folder
    * - **idf.openOcdLaunchArgs**
      - Launch arguments for OpenOCD before ``idf.openOcdDebugLevel`` and ``idf.openOcdConfigs``
    * - **idf.openOcdDebugLevel**
      - Set OpenOCD Debug Level (0-4) Default: 2
    * - **idf.port**
      - Path of selected device port
    * - **idf.monitorPort**
      - Optional Path of selected device port for monitor. If undefined, will use **idf.port** instead as monitor port.
    * - **idf.portWin**
      - Path of selected device port in Windows
    * - **idf.enableSerialPortChipIdRequest**
      - Enable detecting the chip ID and show on serial port selection list
    * - **idf.useSerialPortVendorProductFilter**
      - Enable use of ``idf.usbSerialPortFilters`` list to filter serial port devices list
    * - **idf.usbSerialPortFilters**
      - USB productID and vendorID list to filter known Espressif devices
    * - **openocd.jtag.command.force_unix_path_separator**
      - Forced to use ``/`` instead of ``\\`` as path separator for Win32 based OS
    * - **idf.svdFilePath**
      - SVD file absolute path to resolve chip debug peripheral tree view
    * - **idf.jtagFlashCommandExtraArgs**
      - OpenOCD JTAG flash extra arguments. Default is ["verify", "reset"].

This is how the extension uses them:

1. **idf.flashBaudRate** is the baud rate value used for the **ESP-IDF: Flash your Project** command and `Debugging <https://docs.espressif.com/projects/vscode-esp-idf-extension/en/latest/debugproject.html>`_.
2. **idf.monitorBaudRate** is the ESP-IDF Monitor baud rate value and fallback from your project's sdkconfig ``CONFIG_ESPTOOLPY_MONITOR_BAUD`` (idf.py monitor' baud rate). You can override this value by setting the ``IDF_MONITOR_BAUD`` or ``MONITORBAUD`` environment variables, or by configuring it through **idf.customExtraVars** setting of the extension.
3. **idf.openOcdConfigs** stores an string array of relative paths to OpenOCD script configuration files, which are used with OpenOCD server. (e.g.，``["interface/ftdi/esp32_devkitj_v1.cfg", "board/esp32-wrover.cfg"]``). More information can be found in `OpenOCD JTAG Target Configuration <https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-guides/jtag-debugging/tips-and-quirks.html#jtag-debugging-tip-openocd-configure-target>`_.
4. **idf.port** (or **idf.portWin** in Windows) is used as the serial port value for the extension commands.
5. **idf.openOcdDebugLevel** is the log level for OpenOCD server output from 0 to 4.
6. **idf.openOcdLaunchArgs** is the launch arguments string array for OpenOCD. The resulting OpenOCD launch command looks like this: ``openocd -d${idf.openOcdDebugLevel} -f ${idf.openOcdConfigs} ${idf.openOcdLaunchArgs}``.
7. **idf.jtagFlashCommandExtraArgs** is used for OpenOCD JTAG flash task. Please review `Upload application for debugging <https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-guides/jtag-debugging/index.html#upload-application-for-debugging>`.

.. note::

    * When using the command **ESP-IDF: Set Espressif Device Target**, it will override the current sdkconfig IDF_TARGET with selected Espressif chip, and it will also update **idf.openOcdConfigs** with its default OpenOCD configuration files.
    * To customize the **idf.openOcdConfigs** alone, you can use the **ESP-IDF: Select OpenOCD Board Configuration** or modify your ``settings.json`` directly.

Code Coverage Specific Settings
-------------------------------

These settings are used to configure the code coverage colors.

.. list-table::
    :widths: 25 75
    :header-rows: 1

    * - Setting ID
      - Description
    * - **idf.coveredLightTheme**
      - Background color for covered lines in light theme for gcov coverage
    * - **idf.coveredDarkTheme**
      - Background color for covered lines in dark theme for gcov coverage
    * - **idf.partialLightTheme**
      - Background color for partially covered lines in light theme for gcov coverage
    * - **idf.partialDarkTheme**
      - Background color for partially covered lines in dark theme for gcov coverage
    * - **idf.uncoveredLightTheme**
      - Background color for uncovered lines in light theme for gcov coverage
    * - **idf.uncoveredDarkTheme**
      - Background color for uncovered lines in dark theme for gcov coverage


Extension Behaviour Settings
----------------------------

.. list-table::
    :widths: 25 75
    :header-rows: 1

    * - Setting ID
      - Description
    * - **idf.enableUpdateSrcsToCMakeListsFile**
      - Enable updating source files in ``CMakeLists.txt`` (default ``true``)
    * - **idf.flashType**
      - Preferred flash method. DFU, UART or JTAG
    * - **idf.launchMonitorOnDebugSession**
      - Launch ESP-IDF Monitor along with ESP-IDF debug session
    * - **idf.notificationMode**
      - ESP-IDF extension notifications and output focus mode. (default ``All``)
    * - **idf.showOnboardingOnInit**
      - Show ESP-IDF configuration window on extension activation
    * - **idf.saveScope**
      - Where to save extension settings
    * - **idf.saveBeforeBuild**
      - Save all the edited files before building (default ``true``)
    * - **idf.useIDFKconfigStyle**
      - Enable style validation for Kconfig files
    * - **idf.telemetry**
      - Enable telemetry
    * - **idf.deleteComponentsOnFullClean**
      - Delete ``managed_components`` on **Full Clean Project** command (default ``false``)
    * - **idf.monitorNoReset**
      - Enable no-reset flag to IDF Monitor (default ``false``)
    * - **idf.monitorEnableTimestamps**
      - Enable timestamps in IDF Monitor (default ``false``)
    * - **idf.monitorCustomTimestampFormat**
      - Custom timestamp format in IDF Monitor
    * - **idf.monitorStartDelayBeforeDebug**
      - Delay to start debug session after IDF monitor execution
    * - **idf.enableStatusBar**
      - Show or hide the extension status bar items
    * - **idf.enableSizeTaskAfterBuildTask**
      - Enable IDF Size Task to be executed after IDF Build Task
    * - **idf.customTerminalExecutable**
      - Absolute path to shell terminal executable to use (default to VS Code Terminal)
    * - **idf.customTerminalExecutableArgs**
      - Shell arguments for idf.customTerminalExecutable


Custom Tasks for Build and Flash Tasks
--------------------------------------

.. list-table::
    :widths: 25 75
    :header-rows: 1

    * - Setting ID
      - Description
    * - **idf.customTask**
      - Custom task to execute with **ESP-IDF: Execute Custom Task**
    * - **idf.preBuildTask**
      - Command string to execute before build task
    * - **idf.postBuildTask**
      - Command string to execute after build task
    * - **idf.preFlashTask**
      - Command string to execute before flash task
    * - **idf.postFlashTask**
      - Command string to execute after flash task


QEMU Specific Settings
----------------------

.. list-table::
    :widths: 25 75
    :header-rows: 1

    * - Setting ID
      - Description
    * - **idf.qemuDebugMonitor**
      - Enable QEMU Monitor on debug session
    * - **idf.qemuExtraArgs**
      - QEMU extra arguments


Log Tracing Specific Settings
-----------------------------

.. list-table::
    :widths: 25 75
    :header-rows: 1

    * - Setting
      - Description
    * - **trace.poll_period**
      - poll_period will be set for the apptrace
    * - **trace.trace_size**
      - trace_size will set for the apptrace
    * - **trace.stop_tmo**
      - stop_tmo will be set for the apptrace
    * - **trace.wait4halt**
      - wait4halt will be set for the apptrace
    * - **trace.skip_size**
      - skip_size will be set for the apptrace


Other Frameworks' Specific Settings
-----------------------------------

These settings support additional frameworks together with ESP-IDF:

.. list-table::
    :widths: 25 75
    :header-rows: 1

    * - Setting ID
      - Description
    * - **idf.espAdfPath**
      - Path to locate ESP-ADF framework (ADF_PATH)
    * - **idf.espAdfPathWin**
      - Path to locate ESP-ADF framework in Windows (ADF_PATH)
    * - **idf.espMdfPath**
      - Path to locate ESP-MDF framework (MDF_PATH)
    * - **idf.espMdfPathWin**
      - Path to locate ESP-MDF framework in Windows (MDF_PATH)
    * - **idf.espMatterPath**
      - Path to locate ESP-Matter framework (ESP_MATTER_PATH)
    * - **idf.espRainmakerPath**
      - Path to locate ESP-Rainmaker framework in Windows (RMAKER_PATH)
    * - **idf.espRainmakerPathWin**
      - Path to locate ESP-Rainmaker framework in Windows (RMAKER_PATH)
    * - **idf.sbomFilePath**
      - Path to create ESP-IDF SBOM report


Use of Environment Variables in ESP-IDF ``settings.json`` and ``tasks.json``
----------------------------------------------------------------------------

Environment (env) variables and other ESP-IDF settings (config) can be referenced in ESP-IDF settings using the syntax ``${env:VARNAME}`` and ``${config:ESPIDFSETTING}``, respectively.

For example, to use ``"~/esp/esp-idf"``, set the value of **idf.espIdfPath** to ``"${env:HOME}/esp/esp-idf"``.
