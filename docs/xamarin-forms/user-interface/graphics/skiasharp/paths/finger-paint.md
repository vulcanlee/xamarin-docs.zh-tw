---
title: 在 SkiaSharp 手指繪製
description: 本文說明如何使用手指來繪製 SkiaSharp 畫布的 Xamarin.Forms 應用程式，並示範此範例程式碼。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 56929D74-8F2C-44C6-90E6-3FBABCDC0A4B
author: charlespetzold
ms.author: chape
ms.date: 04/05/2017
ms.openlocfilehash: b0f28cd3e8a928a6da3169dee96ec089178a64e2
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615817"
---
# <a name="finger-painting-in-skiasharp"></a>在 SkiaSharp 手指繪製

_您可以使用手指來在畫布上繪製。_

`SKPath`物件可以持續更新並顯示。 這項功能可讓進行互動式的繪圖，例如使用手指繪製的程式中的路徑。

![](finger-paint-images/fingerpaintsample.png "在手指繪製練習")

在 Xamarin.Forms 中的觸控支援不允許在畫面上，追蹤個人的手指，因此提供額外的觸控支援開發 Xamarin.Forms 觸控追蹤效果。 這種效果文件中所述[**叫用事件影響**](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)。 範例程式[**觸控追蹤效果示範**](https://developer.xamarin.com/samples/xamarin-forms/Effects/TouchTrackingEffectDemos/)包含使用 SkiaSharp，包括手指繪製的計劃的兩個頁面。

[ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)解決方案包含此觸控追蹤事件。 .NET Standard 程式庫專案包含`TouchEffect`類別，`TouchActionType`列舉型別`TouchActionEventHandler`委派，而`TouchActionEventArgs`類別。 每個平台專案包含`TouchEffect`類別，該平台; iOS 專案也包含`TouchRecognizer`類別。

**手指繪製**頁面**SkiaSharpFormsDemos**是指畫的簡化的實作。 不允許選取色彩或描邊寬度、 不其任何方法可清除畫布和當然，您無法儲存您的作品。

[ **FingerPaintPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/FingerPaintPage.xaml)檔案置於`SKCanvasView`在單一儲存格`Grid`並附加`TouchEffect`所`Grid`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Paths.FingerPaintPage"
             Title="Finger Paint">

    <Grid BackgroundColor="White">
        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface" />
        <Grid.Effects>
            <tt:TouchEffect Capture="True"
                            TouchAction="OnTouchEffectAction" />
        </Grid.Effects>
    </Grid>
</ContentPage>
```

附加`TouchEffect`直接`SKCanvasView`下所有平台無法運作。

[ **FingerPaintPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/LinesAndPaths/FingerPaintPage.xaml.cs)程式碼後置檔案會定義兩個集合，用於儲存`SKPath`物件，以及`SKPaint`呈現這些路徑的物件：

```csharp
public partial class FingerPaintPage : ContentPage
{
    Dictionary<long, SKPath> inProgressPaths = new Dictionary<long, SKPath>();
    List<SKPath> completedPaths = new List<SKPath>();

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = SKStrokeCap.Round,
        StrokeJoin = SKStrokeJoin.Round
    };

    public FingerPaintPage()
    {
        InitializeComponent();
    }
    ...
}
```

如同名稱所暗示，`inProgressPaths`字典會儲存目前由一個或多根手指繪製的路徑。 字典索引鍵是隨附於觸控事件的觸控式識別碼。 `completedPaths`欄位是已完成時手指繪製提昇的路徑，從螢幕的路徑的集合。

`TouchAction`處理常式來管理這兩個集合。 當第一次在手指觸控的畫面中，新`SKPath`新增至`inProgressPaths`。 當該手指移動時，其他的點會新增至路徑中。 手指發行時，路徑會轉移至`completedPaths`集合。 您可以同時繪製多個根手指。 其中一個路徑或集合，每次變更後`SKCanvasView`無效：

```csharp
public partial class FingerPaintPage : ContentPage
{
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (!inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = new SKPath();
                    path.MoveTo(ConvertToPixel(args.Location));
                    inProgressPaths.Add(args.Id, path);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Moved:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = inProgressPaths[args.Id];
                    path.LineTo(ConvertToPixel(args.Location));
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    completedPaths.Add(inProgressPaths[args.Id]);
                    inProgressPaths.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Cancelled:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    inProgressPaths.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;
        }
    }
    ...
    SKPoint ConvertToPixel(Point pt)
    {
        return new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                           (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));
    }
}
```

隨附的觸控追蹤事件的點是 Xamarin.Forms 座標。這些必須轉換成 SkiaSharp 座標，也就是像素為單位。 這是目的`ConvertToPixel`方法。

`PaintSurface`處理常式就只是呈現這兩個集合的路徑。 稍早的完整的路徑會出現在進行中的路徑底下：

```csharp
public partial class FingerPaintPage : ContentPage
{
    ,,,
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKCanvas canvas = args.Surface.Canvas;
        canvas.Clear();

        foreach (SKPath path in completedPaths)
        {
            canvas.DrawPath(path, paint);
        }

        foreach (SKPath path in inProgressPaths.Values)
        {
            canvas.DrawPath(path, paint);
        }
    }
    ...
}
```

您的聰明才智只受限於您的手指繪畫：

[![](finger-paint-images/fingerpaint-small.png "手指繪製頁面的三個螢幕擷取畫面")](finger-paint-images/fingerpaint-large.png#lightbox "手指繪製頁面的三個螢幕擷取畫面")


## <a name="related-links"></a>相關連結

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos （範例）](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [追蹤觸控效果示範 （範例）](https://developer.xamarin.com/samples/xamarin-forms/Effects/TouchTrackingEffectDemos/)
- [叫用事件的效果](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
