# GitVersionTest
A repository to show/debug a missing *.deps.json file when referencing GitVersion.MSBuild

## Project setup
The project consists of a Blazor app that can be published to a folder.
For deployment, there's a Setup project that's supposed to put all files from the "PublishItemsOutputGroup" into an msi file.

If you publish the BlazorApp project, a 'BlazorApp.deps.json' file is being created in the output folder.

If you build the setup project, the 'BlazorApp.deps.json' file is part of the 'PublishItemsOutputGroup' and gets packaged into the .msi file.

## GitVersion reference
As soon as you add a NuGet package reference to GitVersion.MSBuild (v5.12.0 at the moment), the 'BlazorApp.deps.json' file is no longer 
part of the 'PublishItemsOutputGroup' and doesn't get packaged.

This leads to the target application being unable to use Windows-specific APIs (in the 'real' project that was running the BlazorApp as a Windows Service).

## Build outputs
### Without GitVersion reference
```
Neuerstellen gestartet...
1>------ Neues Erstellen gestartet: Projekt: BlazorApp, Konfiguration: Release x86 ------
"E:\Temp\GitVersionTest\BlazorApp\BlazorApp.csproj" wiederhergestellt (in "0,9 ms").
1>BlazorApp -> E:\Temp\GitVersionTest\BlazorApp\bin\x86\Release\net7.0-windows\BlazorApp.dll
------ Starting pre-build validation for project 'Setup' ------ 
------ Pre-build validation for project 'Setup' completed ------
2>------ Neues Erstellen gestartet: Projekt: Setup, Konfiguration: Release ------
Building file 'E:\Temp\GitVersionTest\Setup\Release\Setup.msi'...
Packaging file 'BlazorApp.exe'...
Packaging file 'appsettings.json'...
Packaging file 'BlazorApp.runtimeconfig.json'...
Packaging file 'BlazorApp.pdb'...
Packaging file 'appsettings.Development.json'...
Packaging file 'BlazorApp.dll'...
Packaging file 'BlazorApp.deps.json'...
========== Alle neu erstellen: 2 erfolgreich, 0 fehlgeschlagen, 0 übersprungen ==========
========== Neu erstellen wurde am 9:17 AM gestartet und dauerte 07,487 Sekunden ==========
```

### With GitVersion.MSBuild reference
```
Neuerstellen gestartet...
1>------ Neues Erstellen gestartet: Projekt: BlazorApp, Konfiguration: Release x86 ------
"E:\Temp\GitVersionTest\BlazorApp\BlazorApp.csproj" wiederhergestellt (in "2 ms").
1>BlazorApp -> E:\Temp\GitVersionTest\BlazorApp\bin\x86\Release\net7.0-windows\BlazorApp.dll
------ Starting pre-build validation for project 'Setup' ------ 
------ Pre-build validation for project 'Setup' completed ------
2>------ Neues Erstellen gestartet: Projekt: Setup, Konfiguration: Release ------
Building file 'E:\Temp\GitVersionTest\Setup\Release\Setup.msi'...
Packaging file 'BlazorApp.exe'...
Packaging file 'appsettings.json'...
Packaging file 'BlazorApp.runtimeconfig.json'...
Packaging file 'BlazorApp.pdb'...
Packaging file 'appsettings.Development.json'...
Packaging file 'BlazorApp.dll'...
========== Alle neu erstellen: 2 erfolgreich, 0 fehlgeschlagen, 0 übersprungen ==========
========== Neu erstellen wurde am 9:19 AM gestartet und dauerte 08,822 Sekunden ==========
```
