---
title: 安裝 Windows 專案
description: 舊版 Xamarin.Forms 方案 （或 macOS 上建立那些） 並不會通用 Windows 平台專案中，且因此這篇文章說明如何將新的 UWP 專案加入至現有的 Xamarin.Forms 方案。
ms.prod: xamarin
ms.assetid: A0774D2E-6994-4D91-84E8-DAB66FC92320
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/10/2018
ms.openlocfilehash: 81cad7b178948ebfcf6f424be9ab62d6b6be5b92
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242633"
---
# <a name="setup-windows-projects"></a>安裝 Windows 專案

_將新的 Windows 專案加入至現有的 Xamarin.Forms 方案_

舊版 Xamarin.Forms 方案 （或建立 macOS 上） 不會有通用 Windows 平台 (UWP) 應用程式專案。 因此，您必須手動加入 UWP 專案以建置的 Windows 10 (UWP) 應用程式。

## <a name="add-a-universal-windows-platform-app"></a>新增通用 Windows 平台應用程式

您應該執行**Visual Studio 2017**上**Windows 10**建置 UWP 應用程式。 如需通用 Windows 平台的詳細資訊，請參閱[簡介通用 Windows 平台](/windows/uwp/get-started/universal-application-platform-guide/)。

UWP 可用 Xamarin.Forms 2.1 和更新版本，而且 Xamarin.Forms.Maps 都支援在 Xamarin.Forms 2.2 和更新版本。

請檢查<a href="#troubleshooting">疑難排解</a>一節以取得實用秘訣。

請遵循這些指示來加入將在 Windows 10 手機、 平板電腦和桌上型電腦執行的 UWP 應用程式：

 1 . 以滑鼠右鍵按一下方案，然後選取**新增 > 新的專案...** 並加入**空白應用程式 (通用 Windows)** 專案：

  ![](universal-images/add-wu.png "加入新的專案 對話方塊")

 2 . 在**新的通用 Windows 平台專案**] 對話方塊中，選取 [Windows 10 上執行應用程式的最小值和目標版本：

  ![](universal-images/target-version.png "新的通用 Windows 平台專案對話方塊")

 3。 UWP 專案上按一下滑鼠右鍵，然後選取**管理 NuGet 封裝...** 並加入**Xamarin.Forms**封裝。 確定方案中的其他專案也會更新成 Xamarin.Forms 封裝的相同版本。

 4 . 請確定將建立新的 UWP 專案**建置 > 組態管理員**視窗 （這可能不是依預設）。 刻度**建置**和**部署**方塊通用專案：

  [![](universal-images/configuration-sml.png "組態管理員視窗")](universal-images/configuration.png#lightbox "組態管理員視窗")

 5。 以滑鼠右鍵按一下專案，然後選取**新增 > 參考**並建立 Xamarin.Forms 應用程式專案 （.NET 標準或共用專案） 的參考。

  ![](universal-images/addref-sml.png "參考管理員 對話方塊")

 6。 在 UWP 專案中，編輯**App.xaml.cs**包含`Init`方法呼叫內`OnLaunched`列 52 解決方法：

```csharp
// under this line
rootFrame.NavigationFailed += OnNavigationFailed;
// add this line
Xamarin.Forms.Forms.Init (e); // requires the `e` parameter
```

 7。 在 UWP 專案中，編輯**MainPage.xaml**藉由移除`Grid`內含`Page`項目。

 8 . 在**MainPage.xaml**，加入新`xmlns`項目`Xamarin.Forms.Platform.UWP`:

```csharp
xmlns:forms="using:Xamarin.Forms.Platform.UWP"
```

 第 9。 在**MainPage.xaml**，變更根`<Page`元素`<forms:WindowsPage`:

```xaml
<forms:WindowsPage
...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
...
</forms:WindowsPage>
```

 10。 在 UWP 專案中，編輯**MainPage.xaml.cs**移除`: Page`繼承規範的類別名稱 (因為它現在會繼承`WindowsPage`因為上一個步驟中所做的變更):

```csharp
public sealed partial class MainPage  // REMOVE ": Page"
```

 11。 在**MainPage.xaml.cs**，新增`LoadApplication`呼叫中`MainPage`啟動 Xamarin.Forms 應用程式的建構函式：

```csharp
// below this existing line
this.InitializeComponent();
// add this line
LoadApplication(new YOUR_NAMESPACE.App());
```

<!--
11 . Double-click **Package.appxmanifest** to set these capabilities
  that are often required:

  Capabilities set:

  * Internet (Client)
  * Location
-->

12。 新增任何本機資源 （例如。 影像檔） 從現有的平台專案所需。

## <a name="troubleshooting"></a>疑難排解

<a name="target-invocation-exception" />

### <a name="target-invocation-exception-when-using-compile-with-net-native-tool-chain"></a>「 目標引動過程例外狀況 」 使用 「 使用.NET 原生工具鏈編譯 」 時

在 UWP 應用程式正在參考多個組件 （例如第三個合作對象控制文件庫，或將應用程式本身會分成多個程式庫），如果 Xamarin.Forms 可能無法從這些組件 （例如自訂轉譯器） 載入的物件。

這在使用時可能會發生**使用.NET Native 工具鏈編譯**這是用於 UWP 應用程式中的選項**屬性 > 建置 > 一般**專案的視窗。

您可以使用的 UWP 特定多載來修正這個`Forms.Init`呼叫中**App.xaml.cs**如下列程式碼所示 (您應該取代`ClassInOtherAssembly`與實際類別參考您的程式碼):

```csharp
// You'll need to add `using System.Reflection;`
List<Assembly> assembliesToInclude = new List<Assembly>();

// Now, add in all the assemblies your app uses
assembliesToInclude.Add(typeof (ClassInOtherAssembly).GetTypeInfo().Assembly);

// Also do this for all your other 3rd party libraries
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// replaces Xamarin.Forms.Forms.Init(e);
```

加入每個應用程式所參考的組件的參考。

#### <a name="dependency-services-and-net-native-compilation"></a>相依性服務和.NET 原生編譯

使用.NET 原生編譯的發行組建可能會失敗，用來解析外部的主要應用程式可執行檔 （例如，在個別的專案或程式庫） 所定義的相依性服務。

使用`DependencyService.Register<T>()`手動註冊相依性服務類別的方法。 根據上述範例中，加入 register 方法如下：

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
Xamarin.Forms.DependencyService.Register<ClassInOtherAssembly>(); // add this
```
