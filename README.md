# A temporary solution for Microsoft WinUI issue [#8810](https://github.com/microsoft/microsoft-ui-xaml/issues/8810)

## Issue Background 

See [#8810](https://github.com/microsoft/microsoft-ui-xaml/issues/8810)

## Solution 1

#### This sample solution overview

* Reference:  
```xml
<ItemGroup>
  <PackageReference Include="CommunityToolkit.WinUI.Controls.SettingsControls" Version="8.0.240109" />
  <PackageReference Include="CommunityToolkit.WinUI.UI.Controls.DataGrid" Version="7.1.2" />
  <PackageReference Include="Microsoft.WindowsAppSDK" Version="1.5.240311000" />
  <PackageReference Include="Microsoft.Windows.SDK.BuildTools" Version="10.0.22621.3233" />
</ItemGroup>
```

* Using SDK introduced control `SelectorBar` in MainWindow

```xaml
<SelectorBar x:Name="SelectorBar1">
    <SelectorBarItem x:Name="SelectorBarItemRecent" Text="Recent" Icon="Clock" />
    <SelectorBarItem x:Name="SelectorBarItemShared" Text="Shared" Icon="Share" />
    <SelectorBarItem x:Name="SelectorBarItemFavorites" Text="Favorites" Icon="Favorite" />
</SelectorBar>
```


#### Steps of solution

* First, add a `XamlCodeGenerationControlFlags` markup in `<PropertyGroup` of project file.
 
```xaml
  <PropertyGroup>
    <OutputType>WinExe</OutputType>
    <TargetFramework>net6.0-windows10.0.19041.0</TargetFramework>
    <TargetPlatformMinVersion>10.0.17763.0</TargetPlatformMinVersion>
    ...

    <!--Temporary solution for issue 8810-->
    <XamlCodeGenerationControlFlags>DoNotGenerateOtherProviders</XamlCodeGenerationControlFlags>
	  
  </PropertyGroup>
```
* Then, execute `Clean solution`, and then `Build solution`
* Thrid, Add below code to App.xaml.cs file
```C#
  public App()
  {
      this.InitializeComponent();

      // Temporary solution for issue 8810
      this.AddOtherProvider(new Microsoft.UI.Xaml.XamlTypeInfo.XamlControlsXamlMetaDataProvider());
      this.AddOtherProvider(new CommunityToolkit.WinUI.UI.Controls.CommunityToolkit_WinUI_UI_Controls_DataGrid_XamlTypeInfo.XamlMetaDataProvider());
      this.AddOtherProvider(new CommunityToolkit.WinUI.Controls.SettingsControlsRns.CommunityToolkit_WinUI_Controls_SettingsControls_XamlTypeInfo.XamlMetaDataProvider());
  }
```
