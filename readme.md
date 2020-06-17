# repro-NuGetAuth - demo steps

cd \temp

git clone https://github.com/rrelyea/repro-nugetAuth auth

cd auth

```
Auth.sln has 3 projects.
-rrelyea.authTestApp, which has a packagereference to:
- rrelyea.classlib1, which has a project ref to:
- rrelyea.classlib2
```

There is a local [nuget.config](nuget.config) file pointing to a authenticated azure artifacts feed, also customizes the global package folder to be in \Packages\

show [failure image](FailureToRestoreDueToAuth.png)

run clear.cmd -- to delete assets file, packages from local machine

launch 'devenv c:\temp\auth\auth.sln /server

connect via client

F12 on Class1 in Program.cs in rrelyea-authTest project

Expand the top line in code, to show location of assembly (packages folder!)

# Next Steps

- Merge latest NuGet (eta today)

- dotnet.exe/nuget.exe/msbuild.exe running in VS Terminal or in middle of builds
  - msbuild.exe /restore
    - integrated windows auth doesn't work w/o interaction, unlike normal CLIs and AACP on a client. (expensive to solve)
    - device flow prompts once per feed, where it should be able to use the same creds, in most cases, if same AAD Tenant. [artifacts-credprovider#195](https://github.com/microsoft/artifacts-credprovider/issues/195)
  - nuget.exe
    - NuGet.exe should detect execution on codespace, and send canPrompt to plugin as false. [nuget/home#9687](https://github.com/NuGet/Home/issues/9687) 
  - dotnet.exe
    - make install of AACP (dotnet core flavor) installable easily [client.engineering#360](https://github.com/NuGet/Client.Engineering/issues/360)
  - all
    - [nuget/home#9688](https://github.com/NuGet/Home/issues/9688) improve error experience with NUXXXX errors when auth fails, or auth fails and no cred provider is there, etc..

- Test and design VS Auth flow when the codespace login account is different than the Azure Artifacts feed account.