# How to use local NuGet packages

Updated: 2025-10-22

You can use local nuget packages by setting up a local feed (sometimes called store).

We will be using .NET 10 with the dotnet CLI for this.

## Create a NuGet package:

1. Create a class library with `dotnet new classlib`

2. Add the functionality that you want to that class

3. Edit the .csproj file and change the following:
		
```xml
<PropertyGroup>
	<TargetFramework>net10.0</TargetFramework> <!-- change to your correct version here-->
	<PackageId>MyPackageId</PackageId> <!-- change to an appropriate name later -->
	<Version>1.0.0</Version>
	<Authors>Author</Authors>
	<Company>Company</Company>
	<Product>Product</Product>
</PropertyGroup>

```

4. run `dotnet pack`

5. You can now find your .nupkg package with a directory containing all the necessary .dll files in ./bin/Release/

## Create a local feed

1. In another directory, create a new project: `dotnet new console`

2. In your new project run: `dotnet new nugetconfig`
	
3. Run `mkdir packages; mkdir ./packages/myPackage`

4. Copy your .nupkg package INCLUDING the directory containing the .dll files to ./packages/myPackage
	
5. `dotnet nuget add source ./packages`

6. Add your package with `dotnet add package MyPackageId`

7. NOTE! Any externally referenced .dll libraries need to be referenced in your new project. For example if the class library (i.e. nuget package) contains a reference to 'MyOtherLibrary', then you also need to add:
```xml
<Reference Include="MyOtherLibrary"><HintPath>.\MyOtherLibrary.dll</HintPath></Reference>
```
To your item Group.

When you add your package, and if you run `dotnet restore; dotnet build`, the MyOtherLibrary.dll
SHOULD be copied over automatically. You STILL need to add the reference in the .csproj file.

Make sure that the path is accurate.

8. Run `dotnet restore; dotnet build`
	
