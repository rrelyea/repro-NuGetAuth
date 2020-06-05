# repro-NuGetAuth

Auth.sln has 3 projects.
-rrelyea.authTestApp, which has a packagereference to:
- rrelyea.classlib1, which has a project ref to:
- rrelyea.classlib2

There is a local nuget.config file pointing to a authenticated azure artifacts feed.

## How to repro
- In VS, open auth.sln, and build (which will do a restore).
- In dev command prompt, run `msbuild /r auth.sln /p:NuGetInteractive=true`, which will restore and then build.

## Clean and try again
To clean up and try your authenticated scenario again:

Make sure the assets file is deleted, or restore won't need to run:

`rd src\rrelyea.authTestApp\obj -r`

Clear NuGet's http cache and global package folder (customized to be in .packages\ via nuget.config):

`dotnet nuget locals -c all`

If using AACP (azure artifacts credential provider), make sure to remove the sessionTokenCache, if you are trying to test from scratch:

`del %LocalAppData%\MicrosoftCredentialProvider\SessionTokenCache.dat`