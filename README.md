# Nuget integration with gitlab Packages Registry

This time we're using gitlab [Packages Registry](https://docs.gitlab.com/ee/user/packages/nuget_repository/#use-the-gitlab-endpoint-for-nuget-packages) to publish on gitlab our Nuget Packages by .NET CLI using Project Level Access.

## Nuget Packages used privately

First of all, we need a [Api Personal Access Token](https://gitlab.com/-/profile/personal_access_tokens) for gitlab.

Generate it, save it, because you may not find the token again. The token name is just for you to remaber, so use a name related to your project.

## Create a package using .NET CLI

$ `dotnet new classlib Luuby`

And add some nice description to it:
```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>net5.0</TargetFramework>
     <PackageId>Lubby</PackageId>
    <Version>1.0.0</Version>
    <Authors>Keisy Daniel Barcellos</Authors>
    <Company>UUDI</Company>
    <Description>
      Some nice Description here.
    </Description>
  </PropertyGroup>
  
</Project>
```

## Create a nuget.config

Nuget.config is Nothing but a xml file to configure your package.

Pay attention that the tag `<?xml version="1.0" encoding="utf-8"?>` has to be the first line of the config.

You may use your project Id as the only way to target it, so, donot change the url https://gitlab.com/api/v4/projects/27687525/packages/nuget/index.json beyond the number. 

Paste your Token on <add key="ClearTextPassword" value="YOUR_TOKEN_HERE" />

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <packageSources>
        <clear />
        <add key="gitlab" value="https://gitlab.com/api/v4/projects/276875/packages/nuget/index.json" />
    </packageSources>
    <packageSourceCredentials>
        <gitlab>
            <add key="Username" value="KeisyNKK" />
            <add key="ClearTextPassword" value="sWm9awdD5yBK7wqxJY" />
        </gitlab>
    </packageSourceCredentials>
</configuration>
```

All set, go create your package file.

## Create a Package using .NET CLI

Now, on the root:

$ `dotnet pack`

If it works fine you may see the following output:

Microsoft (R) Build Engine version 16.10.1+2fd48ab73 for .NET  <br>
Copyright (C) Microsoft Corporation. All rights reserved.<br>

  Determining projects to restore...
  Restored /home/keisy/Github/keisyd/nuget-samples/Lubby/Lubby.csproj (in 57 ms).<br>
  Lubby -> /home/keisy/Github/keisyd/nuget-samples/Lubby/bin/Debug/net5.0/Lubby.dll<br>
  Successfully created package '/home/keisy/Github/keisyd/nuget-samples/Lubby/bin/Debug/Lubby.1.0.0.nupkg'.<br>

If so, to publish you just:

`dotnet nuget push /home/keisy/Github/keisyd/nuget-samples/Lubby/bin/Debug/Lubby.1.0.0.nupkg --source gitlab`

And it can take around 10min to get published.