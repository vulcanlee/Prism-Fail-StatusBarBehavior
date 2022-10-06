# Prism.Maui cannot work with ComminityToolkit's StatusBarBehavior

Today, I see this [Announcing the .NET MAUI Community Toolkit v1.3](https://devblogs.microsoft.com/dotnet/announcing-the-dotnet-maui-community-toolkit-v13/) blog, then I create a MAUI Project by Prism.Maui Template and upgrade Prism.DryIoc.Maui

## Using Maui Template to create App and can work with StatusBarBehavior

* Open Visual Studio 2022
* New Project
* Select [.NET MAUI Application] template
* Set project name is mauiTemplate
* Create this Project
* Install [CommunityToolkit.Maui] NuGet package into this project

  > the version is 1.3
* Open [MauiProgram.cs] file, after `.UseMauiApp<App>()` , then add `.UseMauiCommunityToolkit()`
* Open [MainPage.xaml] 
  * Add new namespace : `xmlns:toolkit="http://schemas.microsoft.com/dotnet/2022/maui/toolkit"`
  * before `<ScrollView>` element, add following XAML

```XML
<ContentPage.Behaviors>
    <toolkit:StatusBarBehavior StatusBarColor="Fuchsia" StatusBarStyle="LightContent" />
</ContentPage.Behaviors>
```

* Runing on Android emulator, it work fine.

  ![](Maui%20Template%20working%20fine.png)

## Using Prism.Maui to create App and can not work with StatusBarBehavior

* Open Visual Studio 2022
* New Project
* Select [Prism.NET MAUI App (Dan Siegel)] template
* Set project name is prismTemplate
* Create this Project
* Install [CommunityToolkit.Maui] NuGet package into this project

  > the version is 1.3
* Upgrade [Prism.DryIoc.Maui] NuGet package to version 8.1.273-pre
* Open [MauiProgram.cs] file, find `.UsePrismApp<App>(PrismStartup.Configure)` , then replace with following C# 

```csharp
.UseMauiApp<App>()
.UseMauiCommunityToolkit()
.UsePrism(prism=>
{
    prism.OnAppStart("MainPage");
    prism.RegisterTypes(containerRegistry =>
    {
        containerRegistry.RegisterForNavigation<MainPage>()
                        .RegisterInstance(SemanticScreenReader.Default);
    });
})
```

* Open [Views] > [MainPage.xaml] 
  * Add new namespace : `xmlns:toolkit="http://schemas.microsoft.com/dotnet/2022/maui/toolkit"`
  * before `<ScrollView>` element, add following XAML

```XML
<ContentPage.Behaviors>
    <toolkit:StatusBarBehavior StatusBarColor="Fuchsia" StatusBarStyle="LightContent" />
</ContentPage.Behaviors>
```

* Runing on Android emulator, it got exception : System.NotImplementedException: 'Either set MainPage or override CreateWindow.'

* I remark the `<ContentPage.Behaviors> ... </ContentPage.Behaviors>` XAML and re-run this project on Android emulator,

  ![](Prism%20Template.png)

