# Maui.AppStoreInfo

[![Nuget package](https://img.shields.io/nuget/vpre/Oscore.Maui.AppStoreInfo)](https://www.nuget.org/packages/Oscore.Maui.AppStoreInfo/)
[![CI/CD](https://github.com/oscoreio/Maui.AppStoreInfo/actions/workflows/dotnet.yml/badge.svg?branch=main)](https://github.com/oscoreio/Maui.AppStoreInfo/actions/workflows/dotnet.yml)
[![License: MIT](https://img.shields.io/github/license/oscoreio/Maui.AppStoreInfo)](https://github.com/oscoreio/Maui.AppStoreInfo/blob/main/LICENSE)

Allows you to check the information in App stores(for example the latest published version)
and suggest actions to the user based on this.

### Supported Platforms
| Platform | Minimum Version Supported             |
|----------|---------------------------------------|
| iOS      | 12.2+                                 |
| macOS    | 15+                                   |
| Android  | 5.0 (API 21)                          |
| Windows  | 11 and 10 version 1809+ (build 17763) |
> [!NOTE]  
> Since Android doesn't provide an official API, there is no support for this other than opening a store page. It is recommended to use [Android In-App Updates](https://github.com/oscoreio/Maui.Android.InAppUpdates) if you need to check for updates.

# Usage
- Add NuGet package to your project:
```xml
<PackageReference Include="Oscore.Maui.AppStoreInfo" Version="1.1.0" />
```
- Add the following to your `MauiProgram.cs` `CreateMauiApp` method:
```diff
builder
    .UseMauiApp<App>()
+   .UseAppStoreInfo(options =>
+   {
+       options.CountryCode = "gb"; // Optional, default is "us"
+       options.PackageName = "com.companyname.appname"; // Optional, default is AppInfo.Current.PackageName
+       options.CurrentVersion = new Version(1, 0, 0); // Optional, default is AppInfo.Current.Version
+   })
    .ConfigureFonts(fonts =>
    {
        fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
        fonts.AddFont("OpenSans-Semibold.ttf", "OpenSansSemibold");
    });
```
- Use the `AppStoreInfo.Current` class or `IAppStoreInfo` from DI to check the latest version and suggest actions to the user:
```csharp
if (!await AppStoreInfo.Current.IsUsingLatestVersionAsync())
{
    await AppStoreInfo.Current.OpenApplicationInStoreAsync();
}

// This is all based on the information provided by the following method
var information = await AppStoreInfo.Current.GetInformationAsync();
            
await DisplayAlert(
    "App Store Information",
    $"Title: {information.Title}\n" +
    $"Description: {information.Description}\n" +
    $"Latest Version: {information.LatestVersion}\n" +
    $"External Store Uri: {information.ExternalStoreUri}\n" +
    $"Internal Store Uri: {information.InternalStoreUri}\n" +
    $"Release Notes: {information.ReleaseNotes}\n" +
    $"Application Size: {information.ApplicationSizeInBytes/1024/1024} MB\n",
    "OK");
```

# Links
- https://github.com/edsnider/latestversionplugin/
- https://learn.microsoft.com/en-us/dotnet/maui/platform-integration/appmodel/app-information?view=net-maui-8.0&tabs=android
- https://stackoverflow.com/questions/49072305/official-api-for-grabbing-app-version-on-google-playstore/58590547#58590547
- https://stackoverflow.com/questions/60043944/does-ios-has-in-app-updates-like-feature-as-of-android
