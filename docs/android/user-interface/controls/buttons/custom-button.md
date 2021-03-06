---
title: 自訂按鈕
ms.prod: xamarin
ms.assetid: C523D41E-5855-248D-079D-6B12B74B7617
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: e6fc3fe4c3cb89d74188557615f58cc8e34f5991
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/04/2018
ms.locfileid: "30766565"
---
# <a name="custom-button"></a>自訂按鈕

在本節中，您將建立按鈕使用自訂映像，而不是文字，使用[ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) widget 和定義三個不同的映像，以使用不同的按鈕狀態的 XML 檔案。 按下按鈕時，就會顯示簡短訊息。

以滑鼠右鍵按一下並下載的三個圖像下方，然後將其複製到**資源/drawable**專案的目錄。 這些將用於不同按鈕狀態。

 [![狀態為 normal 的 Android 綠色圖示](custom-button-images/android-normal.png)](custom-button-images/android-normal.png#lightbox) [![橙色 Android 焦點狀態圖示](custom-button-images/android-focused.png)](custom-button-images/android-focused.png#lightbox) [![黃色 Android 已按下狀態圖示](custom-button-images/android-pressed.png)](custom-button-images/android-pressed.png#lightbox)

建立新的檔案中**資源/drawable**名為目錄**android_button.xml**。 插入下列 XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@drawable/android_pressed"
          android:state_pressed="true" />
    <item android:drawable="@drawable/android_focused"
          android:state_focused="true" />
    <item android:drawable="@drawable/android_normal" />
</selector>
```

這會定義單一 drawable 資源，這將變更其按鍵的目前狀態為基礎的映像。 第一個`<item>`定義**android_pressed.png**做為影像餂鈕蒢 （啟動）; 第二個`<item>`定義**android_focused.png**做為影像時（[] 按鈕使用的軌跡或方向鍵反白顯示） 時，按鈕是已取得焦點;第三個`<item>`定義**android_normal.png**做為正常的狀態 （當按下都已取得焦點）。 此 XML 檔現在代表單一 drawable 資源和參考時[ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/)槲剢褁，顯示的影像就會根據在變更這三種狀態。


> [!NOTE]
> 順序`<item>`項目是很重要。 此 drawable 參考時， `<item>`s 會周遊中順序來判斷哪一個是適用於目前的按鈕狀態。
> 「 標準 」 影像是最後一個項目，因為它套用的時，才會在條件`android:state_pressed`和`android:state_focused`都評估為 false。

開啟**Resources/layout/Main.axml**檔案，然後加入[ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/)項目：

```xml
<Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:padding="10dp"
        android:background="@drawable/android_button" />
```

`android:background`屬性會指定要用於按鈕背景 drawable 資源 (即在儲存時**Resources/drawable/android.xml**，被參考為`@drawable/android`)。 這會取代整個系統的按鈕所用的一般背景影像。 為了讓 drawable 按鈕狀態為基礎的映像變更，將映像必須套用至背景。

若要讓執行某個動作時按下按鈕，加入下列程式碼的結尾[ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle/)方法：

```csharp
Button button = FindViewById<Button>(Resource.Id.button);

button.Click += (o, e) => {
    Toast.MakeText (this, "Beep Boop", ToastLength.Short).Show ();
};
```

這會擷取[ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/)在版面配置，然後加入[ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/)時顯示訊息[ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/)按下。

現在執行應用程式。


*此頁面上的部分是修改依據工作建立及 Android 的開放原始碼專案所共用，並依據條款中所述來使用*
[*Creative Commons 2.5 Attribution 授權*](http://creativecommons.org/licenses/by/2.5/).
