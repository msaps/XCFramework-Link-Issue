## Issue

When integrating an `xcframework` Framework into an Xcode workspace with a framework target which is dependent on the `xcframework` and then an application target which is dependent on the workspace framework, the application fails to build with a `No such module` error in the workspace framework.

The demo project in this repository has the following structure:

- Xcode Workspace
    - `MyApp` (iOS App target)
    - `MyFramework` (Framework target)
        - `OtherFramework.xcframework` (Pre-compiled `xcframework` that `MyFramework` depends on.)

The frameworks are linked as follows:
- `MyApp`
    - `MyFramework` (Embed & Sign)
    - `OtherFramework.xcframework` (Embed & Sign)
- `MyFramework`
    - `OtherFramework.xcframework` (Do Not Embed)

*Framework Search Paths are correctly configured as `$(SRCROOT)/../Frameworks`*.

**MyApp Project**
<p align="center">
    <img src="img/MyApp.png" width="890" alt="MyApp"/><br />
</p>

**MyFramework Project**
<p align="center">
    <img src="img/MyFramework.png" width="890" alt="MyApp"/><br />
</p>

## Observations
- Building the `MyFramework` scheme works absolutely fine.
- Building the `MyApp` scheme causes the `MyFramework` build to fail with a `No such module 'OtherFramework'` error.
- Removing the `OtherFramework` framework from the `MyApp` target allows the project to build successfully for the Simulator, but this fails on device as `OtherFramework` is not loaded.
- This build failure seems to disappear on the first build on a clean system (i.e. on CI this issue only seems to sporadically occur).
- Integrating normal `.framework` frameworks in this exact manner works fine.