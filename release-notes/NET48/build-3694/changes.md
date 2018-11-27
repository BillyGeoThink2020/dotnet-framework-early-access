# .NET Framework Build 3694 Release Notes
Following changes are included in .NET Framework 4.8 build 3694. You can view the full list of [.NET Framework 4.8 Changes](/release-notes/NET48/dotnet-48-changes.md).

.NET Framework release notes describe product improvements grouped by product area. Each change includes a Microsoft-internal VSTS bug ID, the primary binary that was updated and whether the change was a bug or a feature.

## ASP.NET
* Fixed an issue with ValidationContext.MemberName when using custom DataAnnotation.ValidationAttribute. [563497, System.web.dll, Bug, Build:3694]
* Fixed ArgumentOutOfRangeException in MemoryCache when using change monitors with non-existent files east of GMT. [684136, System.Web.dll, Bug, Build:3694]
* Fix handling of multi-value HTTP headers that may affect multipart data processing. [684397, System.Web.dll, Bug, Build:3694]


## BCL
* Fixed default settings used by RsaProtectedConfigurationProvider (use AES instead of 3DES, RSA is now using 2048bit key, OAEP is on by default), fixed encryption with OAEP so that it writes correct metadata. [549418, System.Configuration.dll, Bug, Build:3694]
* Add API to obtain certificate thumbprints with a caller-specified digest algorithm. [700365, mscorlib.dll, Feature, Build:3694]


## CLR
* Fixed an issue with the GC where frequent LOH under high memory pressure may result in premature OOM errors. [657730, clr.dll, Bug, Build:3694]
* Process corrupting exceptions in exception filter (like access violation) now result in aborting the current process. [110375, clr.dll, Bug, Build:3694]
* .NET now integrates with antimalware providers to scan assemblies loaded from byte arrays. [576558, clr.dll, Feature, Build:3694]
* Fixed an issue with missing Win32 resources in ReadyToRun images. [624174, crossgen.exe, Bug, Build:3694]
* Fixed an issue with ngen createpdb where passing in a long output folder could cause a crash. [627712, ngen.exe, Bug, Build:3694]
* Fixed a crossgen failure when compiling assemblies with no Win32 resources into ReadyToRun images. [722265, coreclr.dll, Bug, Build:3694]


## Networking
* Fix a race condition that would sometimes cause all of the connections to a server to stop sending HTTP requests. [499777, System.dll, Bug, Build:3694]
* Added the ability via the config file to specify opt-in performance caching of default credentials for Windows authentication for HTTP and SMTP APIs. [514209, System.dll, Bug, Build:3694]
* Added retry timer for PAC file discovery after failure. [567511, System.dll, Bug, Build:3694]


## SQL
* This change provides an AppContext switch for making the default value of TransparentNetworkIPResolution false in SqlClient connection strings. [710778, System.Data.dll, Bug, Build:3694]


## WCF
* ServiceHealthBehavior is a new service behavior (IServiceBehavior) for WCF services that exposes a “?health” endpoint.  This new behavior allows one to monitor the internal state of their WCF service and obtain specific HTTP response codes during those times that listeners become faulted, throttle capacities are reached, etc.  One can extend this behavior to further scope the health of their WCF services to match their own custom business rules and needs. [620852, System.Servicemodel.dll, Feature, Build:3694]
* Fixed a System.AccessViolationException due to accessing disposed X509Certificate2 instance in a rare race condition. The fix is to defer the service certificate cleanup to GC. The impacted scenario is WCF NetTcp bindings using reliable sessions with certificate authentication. Customer can opt-in to the fix by adding the follow AppSetting to the config file. Without this AppSetting set to true, existing services won’t be affected by this code change. :
  <appSettings>
    <add key=""wcf:deferSslStreamServerCertificateCleanup"" value=""true""/>
  </appSettings> [695709, System.Servicemodel.dll, Bug, Build:3694]
* Fixed a race condition with IIS hosted net.tcp services when the portsharing service is restarted which resulted in the service being unavailable. [695877, System.ServiceModel.WasHosting.dll, Bug, Build:3694]


## Windows Forms
* Fixed ToolStrip and MenuStrip control accessible hierarchy of inner menu/tool items. Enabled support of UI Automation notifications for ToolStrip and MenuStrip controls.[497307, System.Windows.Forms.dll, Bug, Build:3694]

  In order for the application to benefit from these changes, the application should be recompiled to target .NET framework 4.8 or the application should explicitly opt-in into all accessibility app context switches in the app.config file. For example:
  ```<?xml version=""1.0"" encoding=""utf-8""?>
  <configuration>
    <runtime>
      <!-- AppContextSwitchOverrides values are in the form of 'key1=true|false;key2=true|false  -->
      <!-- Enabling newer accessibility features (e.g. UseLegacyAccessibilityFeatures.2=false) requires all older accessibility features to be enabled (e.g. UseLegacyAccessibilityFeatures=false) -->
      <AppContextSwitchOverrides value=""Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false;Switch.UseLegacyAccessibilityFeatures.3=false""/>
    </runtime>
  </configuration>
* The keyboard tooltips feature is "opt-in" now - it is no longer switched on implicitly when app targets .NET 4.8. "Switch.UseLegacyAccessibilityFeatures.3=false", which is a default value for .NET 4.8 apps, is still required by the feature. [686499, System.Windows.Forms.dll, Bug, Build:3694]
App.config file content example with enabled keyboard tooltips for apps targeting .NET 4.7.2 or older:
  ```<?xml version=""1.0"" encoding=""utf-8""?>
  <configuration>
    ...
    <runtime>
      <!-- Enabling newer accessibility features (e.g. UseLegacyAccessibilityFeatures.2=false) requires all older accessibility features to be enabled (e.g. UseLegacyAccessibilityFeatures=false) -->
      <AppContextSwitchOverrides value=""Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false;Switch.UseLegacyAccessibilityFeatures.3=false;Switch.System.Windows.Forms.UseLegacyToolTipDisplay=false""/>
    </runtime>
  </configuration>
  App.config file content example with enabled keyboard tooltips for apps targeting .NET 4.8:
  <?xml version=""1.0"" encoding=""utf-8""?>
  <configuration>
    ...
    <runtime>
      <AppContextSwitchOverrides value=""Switch.System.Windows.Forms.UseLegacyToolTipDisplay=false""/>
    </runtime>
  </configuration>


## WorkFlow
* Previously Visual Studio builds of C# projects would create 3 temporary files and not clean them. With this change, the files are only created for C# projects that contain XAML files and utilize the XamlAppDef build action and those files are deleted with the Clean task. [392996, System.Workflow.Runtime.dll, Bug, Build:3694]


## WPF
* Added support for ControllerFor UIAutomation property [503411, PresentationCore.dll, UIAutomationTypes.dll, Feature, Build:3694]
* Fixed an issue where focus loops inside a WPF UserControl instead of breaking out of it under some hosting scenarios. [542626, PresentationCore.dll, PresentationFramework.dll, WindowFormsIntegration.dll, Bug, Build:3694]
* Fixed an issue where dragging from a WPF application and dropping into an application that fails causes a crash in WPF. [685894, PresentationFramework.dll, Bug, Build:3694]
* Fixed a crash when using "live sorting" and NewItemPlaceholderPosition.AtBeginning at the same time. [387603, PresentationFramework.dll, Bug, Build:3694]
* Added support for SizeOfSet and PositionInSet UIAutomation properties, this change also provides defaults for some controls. [488213, PresentationCore.dll, PresentationFramework.dll, System.Windows.Controls.Ribbon.dll, UIAutomationClient.dll, UIAutomationTypes.dll, Feature, Build:3694]
* Scrolling panels now honor the system setting for mouse wheel to "scroll by screen". [586801, PresentationFramework.dll, System.Windows.Controls.Ribbon.dll, Bug, Build:3694]
* Tooltips now show underneath controls when keyboard focused, Ctrl+Shift+F10 dismisses/reshows tooltips. [614397, PresentationFramework.dll, System.Windows.Controls.Ribbon.dll, Feature, Build:3694]
