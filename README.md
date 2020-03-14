# TypeScriptBuildRepro
Repro for a typescript build error. Works when building from VS19 but fails when building from "dotnet build"

If I open this solution in Visual Studio 2019, it builds without any issues. A app.js file is generated for the app.ts file, as expected.

If I try to build this project from the command line using "dotnet build", it fails with the following result:

``` bash
C:\Users\kelps\Source\Repos\TypeScriptBuildRepro>dotnet build
Microsoft (R) Build Engine version 16.4.0+e901037fe for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 275,61 ms for C:\Users\kelps\Source\Repos\TypeScriptBuildRepro\TestTypeScript\TestTypeScript.csproj.
C:\Users\kelps\.nuget\packages\microsoft.typescript.msbuild\3.8.3\tools\Microsoft.TypeScript.targets(311,5): error : The build task could not find node.exe which is required to run the TypeScript compiler. Please install Node and ensure that the system path contains its location. [C:\Users\kelps\Source\Repos\TypeScriptBuildRepro\TestTypeScript\TestTypeScript.csproj]

Build FAILED.

C:\Users\kelps\.nuget\packages\microsoft.typescript.msbuild\3.8.3\tools\Microsoft.TypeScript.targets(311,5): error : The build task could not find node.exe which is required to run the TypeScript compiler. Please install Node and ensure that the system path contains its location. [C:\Users\kelps\Source\Repos\TypeScriptBuildRepro\TestTypeScript\TestTypeScript.csproj]
    0 Warning(s)
    1 Error(s)

Time Elapsed 00:00:02.08
```

Looks like the TypeScript nuget package is doing somethign wrong when trying to locate node. It seems to be depending on node being installed locally and it is not recognizing the path when running from the "dotnet" command line.

FYI, the [WebCompiler](https://github.com/madskristensen/WebCompiler) nuget package (from @madskristensen) doesn't seem to suffer from this, even though is also used node for all it's compilations. I am not sure, but I believe Mads myght be embedding a stand-alone/portable node with his package.

The actual solution I am working uses WebCompiler to compile and minify .less files to .css without this issue, both in VS and from "dotnet build", even when running CI in Azure Pipelines.
