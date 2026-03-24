# Debugging Plugins

To debug any plugins we have to first change some settings in Visual Studio and the Plugin Profiler before we can start debugging.

**Visual Studio Settings**

Navigate to Visual Studio's Options (Tools -> Options). In the options window please select "Debugging" from the left tree. Now adjust the settings according to this list:
- Uncheck "Enable Just My Code"
- Uncheck "Step over property evaluation and operators"
- Check "Enable property evaluation and other implicit calls" and all options beneath it
- Check "Enable source server support" and all boxes beneath it

**Symbol Server Support** (only applicable for version 1 of the BizApps Core Accelerator)
> Symbol Server Support is only available to users with an Visual Studio Enterprise license and an Avanade account

All symbols of DLLs which are being build from the BizApps Core Accelerator's backend code are being pushed to the AzDO project's symbol server. 

Navigate to Visual Studio's Options (Tools -> Options). In the options window please select "Debugging" and then "Symbols" in the tree.

- Click on the left most button in the top right corner with the tooltip "New Azure DevOps Symbol Server Location"
- Select id
- Select innersource.visualstudio.com
- Click button "Connect"

Now if you're debugging you can step into the framework code. Visual Studio will download the appropriate symbols and will step into it.

**Plugin Profiler**

Before you can debug your plugins you have to build the plugin locally in "Debug" mode and then update the assembly once before you execute the profiled message again.

Now you can replay past executions (preferable using the persisted requests -> see while installing the plugin profiler). In the "Replace plugin execution" window you can select these past persisted executions using the down arrow button in the window.

Then you select the assembly which you've used to update the Dataverse tenant. 

**<span style="color:red">!!!!IMPORTANT!!!!</span>**
Before clicking on "Start execution" make sure you've adjusted the Isolation Mode. Please select the tab "Settings" in the open "Replay Plugin Execution" window and select the Isolation Mode = None. This has to be done every time you reopen the window. This option won't be saved. If you've missed changing it and run the execution you will get a SecurityException
