# .NET Framework Build 3707 Release Notes
Following changes are included in .NET Framework 4.8 build 3707. You can view the full list of [.NET Framework 4.8 Changes](/release-notes/NET48/dotnet-48-changes.md).

.NET Framework release notes describe product improvements grouped by product area. Each change includes a Microsoft-internal VSTS bug ID, the primary binary that was updated and whether the change was a bug or a feature.

## BCL
* Fixed an IndexOutOfRangeException thrown when asynchronously reading a process output with less than a character's worth of bytes is read at the beginning of a line. [724219, System.dll, Bug, Build:3707]
* Mitigate compatibility breaks seen in some System.Data.SqlClient usage scenarios. [727701, System.Configuration.dll, Bug, Build:3707]

## CLR
* Fixed ability to handle process corrupted state exceptions stemming from Marshal.PtrToStructure on x86. [381677, clr.dll, Bug, Build:3707]
* Fixed intermittent access violation errors when Server GC interacts with type-forwarded value types implemented in mscorlib or other domain-neutral assemblies during garbage collection. [425626, clr.dll, Bug, Build:3707]
* Enabled a very obscure and uncommon usage pattern where FX Closure Walks are too expensive in the default domain. Introduced DeferFxClosureWalk (opt-in) switch that when set does the following things: 1) Disable DisableFxClosureWalk switch 2) When in LoaderOptimization.MultiHost, all assemblies are assumed sharable in the default domain and the closure walk is deferred as long as possible. This solution will provide good default domain performance and correctness. [485894, clr.dll, Bug, Build:3707]
* Fixed crashes that can occur when NGen'ed facade assemblies are loaded as multidomain-sharable.  [602785, clr.dll, Bug, Build:3707]
* Improved Monitor’s lock acquisition performance and scalability under a perpetual lock convoy. [602844, clr.dll, Bug, Build:3707]
* Fixed an issue that previously caused Ngen to run out of memory every time it executed due to registry corruption. [702519, mscorsvc.dll / mscorsvw.exe, Bug, Build:3707]
* Triggering a System.Threading.ThreadAbortException when System.Diagnostics.Debugger.s_triggerThreadAbortExceptionForDebugger is set to true. [732816, mscorlib.dll, Bug, Build:3707]
* Fix a crash on COM interop and properly return the hresult for the OOM. [733492, clr.dll, Bug, Build:3707]

## WCF
* Fixed broken WCF document links in the tracing log that were broken due to MSDN doc location change. [712450, System.ServiceModel.dll, Bug, Build:3707]

## Windows Forms
* Added per monitor DPI awareness support to ToolStrip and ToolStrip Items. [378542, System.Windows.Forms.dll, Bug, Build:3707]
* Fixed accessibility information about ComboBox DataGridView cell, including expanded/collapsed state of this cell. [657355, System.Windows.Forms.dll, Bug, Build:3707]
* Fixed providing accessibility information about ComboBox selected item: item selection is announced and accessible info is presented even the DropDownList is not opened and user selects the items using arrow keys. In ordr=er for the application to benefit from these changes, the application should explicitly opt-in into all accessibility app context switches in the app.config file. [703373, System.Windows.Forms.dll, Bug, Build:3707]
  For example:
  ```<?xml version=""1.0"" encoding=""utf-8""?>
  <configuration>
    <runtime>
      <!-- AppContextSwitchOverrides values are in the form of 'key1=true|false;key2=true|false  -->
      <!-- Enabling newer accessibility features (e.g. UseLegacyAccessibilityFeatures.2=false) requires all older accessibility features to be enabled (e.g. UseLegacyAccessibilityFeatures=false) -->
      <AppContextSwitchOverrides value=""Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false;Switch.UseLegacyAccessibilityFeatures.3=false""/>
    </runtime>
  </configuration>
* Added per monitor DPI awareness support to the PropertyGrid. [719232, System.Windows.Forms.dll, Bug, Build:3707]

## WorkFlow
* Prevented vulnerabilities based on misuse of serialized ActivitySurrogateSelector.ObjectSurrogates. [726199, System.Workflow.ComponentModel.dll, Bug, Build:3707]

## WPF
* Improved the performance of rebuilding the automation tree of an items control (ListBox, DataGrid, etc.) in which grouping is enabled. [104559, PresentationCore.dll, PresentationFramework.dll, Bug, Build:3707]
* Fixed the behavior of automation Grid pattern in a ListView. [401080, PresentationFramework.dll, Bug, Build:3707]
* Fixed the automation tree exposed for a plain ItemsControl (as opposed to a derived class like ListBox, DataGrid, etc.). [410007, PresentationCore.dll, PresentationFramework.dll, Bug, Build:3707]
* Fixed an infinite loop that can arise when setting the height of ListBox (or other ItemsControl) to zero. [448747, PresentationFramework.dll, Bug, Build:3707]
* Fixed a reliability problem to reduce crashes with rapid window size changes when running in Software Rendering mode. [691364, wpfgfx_v0400.dll, Bug, Build:3707]
* Fixed a crash arising when Visual Studio's diagnostic tools are enabled to debug an app that has a ResourceDictionary whose Source points to invalid XAML. [727642, PresentationFramework.dll, Bug, Build:3707]