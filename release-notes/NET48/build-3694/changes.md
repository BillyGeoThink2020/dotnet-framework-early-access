# .NET Framework Build 3677 Release Notes
Following changes are included in .NET Framework 4.8 build 3677. You can view the full list of [.NET Framework 4.8 Changes](/release-notes/NET48/dotnet-48-changes.md) (all changes, including those in build 3677 and builds upto current date).

.NET Framework release notes describe product improvements grouped by product area. Each change includes a Microsoft-internal VSTS bug ID, the primary binary that was updated and whether the change was a bug or a feature.

## BCL

* Fixed default settings used by RsaProtectedConfigurationProvider (use AES instead of 3DES, RSA is now using 2048bit key, OAEP is on by default), fixed encryption with OAEP so that it writes correct metadata. [549418, System.Configuration.dll, Bug, Build:3677 ]
* Add API to obtain certificate thumbprints with a caller-specified digest algorithm. [700365, mscorlib.dll, Feature, Build:3677]


## CLR
* Fixed an issue with the GC where frequent LOH under high memory pressure may result in premature OOM errors. [657730, clr.dll, Bug, Build:3677]


## Windows Forms
* Fixed ToolStrip and MenuStrip control accessible hierarchy of inner menu/tool items. Enabled support of UI Automation notifications for ToolStrip and MenuStrip controls.[497307, System.Windows.Forms.dll, Bug, Build:3677]

  In order for the application to benefit from these changes, the application should be recompiled to target .NET framework 4.8 or the application should explicitly opt-in into all accessibility app context switches in the app.config file. For example:
  ```<?xml version=""1.0"" encoding=""utf-8""?>
  <configuration>
    <runtime>
      <!-- AppContextSwitchOverrides values are in the form of 'key1=true|false;key2=true|false  -->
      <!-- Enabling newer accessibility features (e.g. UseLegacyAccessibilityFeatures.2=false) requires all older accessibility features to be enabled (e.g. UseLegacyAccessibilityFeatures=false) -->
      <AppContextSwitchOverrides value=""Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false;Switch.UseLegacyAccessibilityFeatures.3=false""/>
    </runtime>
  </configuration>


## WPF

* Added support for ControllerFor UIAutomation property [503411, PresentationCore.dll, UIAutomationTypes.dll, Feature, Build:3677]
* Fixed an issue where focus loops inside a WPF UserControl instead of breaking out of it under some hosting scenarios. [542626, PresentationCore.dll, PresentationFramework.dll, WindowFormsIntegration.dll, Bug, Build:3677]
* Fixed an issue where dragging from a WPF application and dropping into an application that fails causes a crash in WPF. [685894, PresentationFramework.dll, Bug, Build:3677]
