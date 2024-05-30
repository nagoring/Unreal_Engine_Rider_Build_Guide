
# Unreal Engine with Rider: Build Error Troubleshooting Guide

This guide documents the steps taken to resolve build errors encountered when using Rider with Unreal Engine. The primary issues resolved include missing `UnrealBuildTool.dll` and an invalid `MinimumiOSVersion` setting.

## Steps to Resolve Build Errors

1. **Check .NET SDK Installation**
   Ensure the .NET SDK is correctly installed.
   ```sh
   dotnet --list-sdks
   ```
   Expected output:
   ```plaintext
   6.0.423 [C:\Program Files\dotnet\sdk]
   ```

2. **Set up `global.json`**
   Ensure the `global.json` file is present in the project root with the correct .NET SDK version.
   ```json
   {
     "sdk": {
       "version": "6.0.423"
     }
   }
   ```

3. **Verify Visual Studio Installation**
   Ensure necessary workloads are installed:
   - .NET desktop development
   - Game development with C++

4. **Configure Rider to Use Correct MSBuild Version**
   In Rider, navigate to `File > Settings > Build, Execution, Deployment > Toolset and Build` and ensure the correct MSBuild path is set under "Use MSBuild version."

5. **Restore NuGet Packages**
   In Rider, open `View > Tool Windows > NuGet` and click `Restore` to restore all necessary packages.

6. **Clean and Rebuild the Project**
   In Rider:
   - Select `Build > Clean Project`
   - Then, select `Build > Rebuild Project`

7. **Set Up Environment Variables**
   Ensure `UE_ENGINE_DIRECTORY` is set if needed:
   - Variable name: `UE_ENGINE_DIRECTORY`
   - Variable value: `D:\Applications\Epic Games\UE_5.3\`
   
   Alternatively, add the path directly to the system environment variables:
   - Variable value: `D:\Applications\Epic Games\UE_5.3\`

8. **Manually Build UnrealBuildTool.dll**
   Open a command prompt as Administrator and navigate to the Unreal Engine batch files directory:
   ```sh
   cd D:\Applications\Epic Games\UE_5.3\Engine\Build\BatchFiles
   ./RunUAT.bat BuildUBT
   ```
   ⚠︎ I did not perform this step. Do it if necessary.

9. **Verify the Location of UnrealBuildTool.dll**
    Ensure UnrealBuildTool.dll actually exists. If not, manually build it or repair the Unreal Engine installation.
    Open Epic Games Launcher.
    Locate Unreal Engine in the library.
    Click the `⋮` (three dots) and select `Verify`.

10. **Resolve Minimum iOS Version Error**
    Update `MinimumiOSVersion` in the project settings:
    - Navigate to `Edit > Project Settings > Platforms > iOS`
    - Set `Minimum iOS Version` to a valid value (e.g., `IOS_15`)

    Alternatively, update the `Config` files:
    ```ini
    [IOSRuntimeSettings]
    MinimumiOSVersion=IOS_15
    ```

## Example of a Successful Resolution
1. **Problem:** Build errors related to `UnrealBuildTool.dll` not found and `MinimumiOSVersion` being invalid.
2. **Solution:** Followed the above steps, corrected the `MinimumiOSVersion` to `IOS_15`.

With these steps, the build errors were successfully resolved and the project was built without issues.
