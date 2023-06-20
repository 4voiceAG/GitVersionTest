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
