---
title: "Android.Support.v7.AppCompat-沒有資源找到符合指定名稱： attr ' android: actionModeShareDrawable'"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5814069C-FC43-41DE-B5A5-024D05E59929
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 07655587642c3e1aa94d035e76f6f6758340546d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/04/2018
ms.locfileid: "30774892"
---
# <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawable"></a>Android.Support.v7.AppCompat-沒有資源找到符合指定名稱： attr ' android: actionModeShareDrawable'

1. 請確定您下載最新的額外項目，以及 Android 5.0 (API 21) SDK 透過 Android SDK Manager 中。

2. 請確定您具有設定為 21 compileSdkVersion 編譯您的應用程式。 您可以選擇為 21 以及設定 targetSdkVersion。

3. 如果您需要的先前版本，例如 API 19，請下載 Nuget 頁面上找到的個別版本：

[https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)

*請注意*： 如果您手動安裝此透過封裝管理員主控台，請確定您也可以安裝相同版本的 Xamarin.Android.Support.v4

[https://www.nuget.org/packages/Xamarin.Android.Support.v4/](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)

堆疊溢位的參考： [http://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro](http://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro)

## <a name="see-also"></a>另請參閱

- [我應該安裝哪一個 Android SDK 套件？](~/android/troubleshooting/questions/install-android-sdk-packages.md)

