---
title: RecyclerView 組件和功能
description: RecyclerView 配置管理員配接器及檢視的持有者的概觀。
ms.prod: xamarin
ms.assetid: 54F999BE-2732-4BC7-A466-D17373961C48
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/13/2018
ms.openlocfilehash: 4d55124e9a02489d1f55e900c537939ff3450509
ms.sourcegitcommit: cb80df345795989528e9df78eea8a5b45d45f308
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/14/2018
ms.locfileid: "39038504"
---
# <a name="recyclerview-parts-and-functionality"></a>RecyclerView 組件和功能


`RecyclerView` 某些工作需要在內部 （例如捲動和檢視的回收），但它的控制代碼是基本上協調協助程式類別來顯示集合的管理員。 `RecyclerView` 在下列的協助程式類別的委派工作：

-   **`Adapter`** &ndash; 擴大項目配置 （具現化的版面配置檔案的內容） 和資料繫結至檢視，會顯示在`RecyclerView`。 配接器也會報告項目按一下事件。

-   **`LayoutManager`** &ndash; 量值，並將項目檢視內`RecyclerView`及管理檢視所回收的原則。

-   **`ViewHolder`** &ndash; 查閱，並將檢視的參考。 檢視持有者也有助於偵測項目檢視按一下。

-   **`ItemDecoration`** &ndash; 可讓應用程式來繪製項目、 重點提示、 和視覺化群組界限之間的分隔線的特定檢視中加入特殊的繪製和配置的位移。

-   **`ItemAnimator`** &ndash; 定義項目動作期間進行，或變更不會對配接器的動畫。

之間的關聯性`RecyclerView`， `LayoutManager`，和`Adapter`類別在下列圖表中所述：

![使用配接器，來存取資料集，在含有 LayoutManager RecyclerView 的圖表](parts-and-functionality-images/01-recyclerview-diagram.png)

如本圖所示`LayoutManager`可以視為之間的媒介`Adapter`而`RecyclerView`。 `LayoutManager`呼叫`Adapter`方法代表`RecyclerView`。 例如，`LayoutManager`呼叫`Adapter`方法來建立新的檢視中的特定項目位置的時候`RecyclerView`。 `Adapter`擴大該項目的版面配置，並建立`ViewHolder`（未顯示） 的執行個體以檢視該位置的快取參考。 當`LayoutManager`呼叫`Adapter`若要將特定的項目繫結至資料集，`Adapter`找出該項目的資料、 擷取資料集，並將它複製到相關聯的項目檢視。

當使用`RecyclerView`在您的應用程式中，若要建立下列類別的衍生型別是必要：

-   **`RecyclerView.Adapter`** &ndash; 提供從您的應用程式的資料集 （也就是針對您的應用程式） 的繫結，會顯示內的項目檢視`RecyclerView`。 配接器知道如何建立關聯中的每個項目檢視位置`RecyclerView`到資料來源中的特定位置。 此外，配接器處理每個個別項目檢視中之內容的配置，並建立每個檢視的檢視持有者。 配接器也會報告偵測到的項目檢視項目按一下事件。

-   **`RecyclerView.ViewHolder`** &ndash; 會快取項目的版面配置檔中檢視的參考，這樣不會不必要地重複資源查閱。 檢視持有者也會排列項目按一下 事件轉送到配接器，當使用者點選檢視持有人相關聯的項目檢視。

-   **`RecyclerView.LayoutManager`** &ndash; 將項目內`RecyclerView`。 您可以使用數個預先定義的配置管理員的其中一個，或者您可以實作自己的自訂版面配置管理員。
    `RecyclerView` 委派，配置管理員，所以您可以外掛在不同的版面配置管理員中，而不需要進行大量的配置原則會變更您的應用程式。

此外，您可以選擇性地擴充下列類別，若要變更的外觀與風格`RecyclerView`應用程式中：

-   **`RecyclerView.ItemDecoration`**
-   **`RecyclerView.ItemAnimator`**

如果您不會延伸`ItemDecoration`並`ItemAnimator`，`RecyclerView`會使用預設實作。 本指南不會說明如何建立自訂`ItemDecoration`並`ItemAnimator`類別，如需有關這些類別的詳細資訊，請參閱[RecyclerView.ItemDecoration](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemDecoration.html)和[RecyclerView.ItemAnimator](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemAnimator.html).


<a name="recycling" />

### <a name="how-view-recycling-works"></a>如何檢視 回收運作方式

`RecyclerView` 不會在您的資料來源中的每一個項目配置的項目檢視。 配置只適合螢幕大小的項目檢視次數而它會重複使用為使用者捲動這些項目配置中。 在使其看不先捲動檢視時，會經過回收的處理序，如下圖所示：

[![說明檢視回收的六個步驟的圖表](parts-and-functionality-images/02-view-recycling-sml.png)](parts-and-functionality-images/02-view-recycling.png#lightbox)

1.  當檢視捲動隱密，並且不會再顯示時，它會變成*丟棄檢視*。

2.  剪輯資料檢視會放在集區，而變得*回收檢視*。
    此集區是檢視，其中顯示相同的資料類型的快取。

3.  要顯示新的項目時，檢視是取自回收集區，以供重複使用。 此檢視必須重新繫結，配接器才會顯示，因為它會呼叫*中途檢視*。

4.  已變更的檢視就會回收： 配接器找出下一個項目，要顯示的資料，並將此資料複製到這個項目的檢視。 這些檢視的參考會從與回收檢視相關聯的檢視持有者擷取。

5.  回收的檢視新增至清單中的項目`RecyclerView`，即將螢幕上移。

6.  回收的檢視畫面上會因使用者捲動`RecyclerView`清單中下一個項目。 同時，另一個檢視使其看不捲動，並回收根據上述的步驟。

項目檢視重複使用，除了`RecyclerView`也會使用另一種效率最佳化： 檢視持有者。 A*檢視的持有者*是一個簡單的快取檢視參考的類別。 配接器會擴大為項目配置檔案中，每次它也會建立對應的檢視持有者。 檢視持有者會使用`FindViewById`擴大的項目配置檔案中取得檢視的參考。 這些參考會用來檢視新的資料載入，每次配置就會回收以顯示新的資料。
 


### <a name="the-layout-manager"></a>配置管理員

配置管理員會負責定位在項目`RecyclerView`顯示; 它決定展示型別 （清單或方格）、 （是否項目會顯示垂直或水平） 的方向，以及應該顯示方向的項目（在一般順序或以相反順序）。 配置管理員也會負責計算的大小和位置中的每個項目的**RecycleView**顯示。

配置管理員有額外的用途： 它會決定何時回收不再顯示給使用者的項目檢視原則。
由於配置管理員是了解哪些檢視會顯示 （與並不是），則為要決定當檢視可以回收的最佳位置。 若要回收的檢視，請配置管理員通常會呼叫配接器回收檢視的內容取代為不同的資料，如先前所述[檢視回收的運作方式](#recycling)。

您可以擴充`RecyclerView.LayoutManager`來建立您自己的配置管理員 中，或者您可以使用預先定義的配置管理員。 `RecyclerView` 提供下列預先定義的版面配置管理員：

-   **`LinearLayoutManager`** &ndash; 排列項目可以水平捲動的資料列或可垂直捲動的資料行中。

-   **`GridLayoutManager`** &ndash; 在方格中顯示項目。

-   **`StaggeredGridLayoutManager`** &ndash; 在交錯的方格中，其中某些項目有不同的高度和寬度，會顯示項目。

若要指定配置管理員，您所選的配置管理員具現化，並將它傳遞給`SetLayoutManager`方法。 請注意，您*必須*指定的配置管理員&ndash;`RecyclerView`不會選取預設的預先定義的配置管理員。

如需有關配置管理員的詳細資訊，請參閱[RecyclerView.LayoutManager 類別參考](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.LayoutManager.html)。


### <a name="the-view-holder"></a>檢視持有者

檢視持有者是您定義快取檢視參考的類別。 配接器會使用這些檢視參考繫結至其內容的每個檢視。 在每個項目`RecyclerView`具有相關聯的檢視持有者執行個體所快取的檢視參考，該項目。 若要建立檢視的持有者，請使用下列步驟來定義類別以包裝每個項目檢視一組正確：

1.  子類別`RecyclerView.ViewHolder`。
2.  實作，查閱，並將檢視參考的建構函式。
3.  實作配接器可用來存取這些參考的屬性。

詳細的範例`ViewHolder`實作所示[基本 RecyclerView 範例](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)。
如需詳細資訊`RecyclerView.ViewHolder`，請參閱 < [RecyclerView.ViewHolder 類別參考](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html)。


### <a name="the-adapter"></a>配接器

大部分的 「 處理的麻煩"`RecyclerView`整合程式碼會發生在配接器。 `RecyclerView` 會要求您提供衍生自配接器`RecyclerView.Adapter`以存取您的資料來源，並填入每個項目，以從資料來源的內容。
因為應用程式專屬資料來源，您必須實作配接器功能了解如何存取您的資料。 配接器從資料來源中擷取資訊，並將其載入至每個項目`RecyclerView`集合。

欞迶飹晱配接器如何將透過檢視持有者的資料來源中的內容對應至在每個資料列項目內的個別檢視`RecyclerView`:

[![說明資料來源連線至 ViewHolders 配接器的圖表](parts-and-functionality-images/03-recyclerviewer-adapter-sml.png)](parts-and-functionality-images/03-recyclerviewer-adapter.png#lightbox)

配接器載入每一個`RecyclerView`具有特定資料列項目的資料列。 資料列位置*P*，例如，配接器找出相關聯的資料位置*P*內的資料來源和複製這項資料給資料列項目位置*P*中`RecyclerView`集合。
在上述繪圖中，比方說，配接器使用的檢視持有者查閱的參考`ImageView`並`TextView`在該位置，因此不需要重複呼叫`FindViewById`這些檢視的使用者身分的捲動 集合和會重複使用檢視。

當您實作配接器時，您必須覆寫下列`RecyclerView.Adapter`方法：

-   **`OnCreateViewHolder`** &ndash; 具現化項目配置檔案並檢視持有者。

-   **`OnBindViewHolder`** &ndash; 載入其參考儲存在指定的檢視持有者檢視中位於指定位置的資料。

-   **`ItemCount`** &ndash; 傳回資料來源中的項目數目。

配置管理員呼叫這些方法，而它定位內的項目`RecyclerView`。 



### <a name="notifying-recyclerview-of-data-changes"></a>通知 RecyclerView 的資料變更

`RecyclerView` 不會自動更新其顯示時其資料的內容來源的變更;配接器必須通知`RecyclerView`資料集中的變更時。 資料集可以變更多種的方式;比方說，可以變更的項目內容，或可能會修改資料的整體結構。
`RecyclerView.Adapter` 提供許多您可以呼叫的方法，讓`RecyclerView`最有效率的方式回應資料變更：

-  **`NotifyItemChanged`** &ndash; 指定位置處的項目已變更的訊號。

-  **`NotifyItemRangeChanged`** &ndash; 指定的範圍內的位置的項目已變更的訊號。

-  **`NotifyItemInserted`** &ndash; 指定的位置中的項目有新插入的訊號。

-  **`NotifyItemRangeInserted`** &ndash; 指定的範圍內的位置的項目有新插入的訊號。

-  **`NotifyItemRemoved`** &ndash; 指定的位置中的項目已移除的訊號。

-  **`NotifyItemRangeRemoved`** &ndash; 指定的範圍內的位置的項目已移除的訊號。

-  **`NotifyDataSetChanged`** &ndash; 資料集已變更的訊號 （強制的完整更新）。

如果您完全了解如何您的資料集已變更，您可以呼叫適當的方法和更新版本，以重新整理`RecyclerView`最有效率的方式。 如果您不知道到底如何您的資料集已變更，您可以呼叫`NotifyDataSetChanged`，這是目前比較沒有效率因為`RecyclerView`必須重新整理會對使用者顯示的所有檢視。 如需這些方法的詳細資訊，請參閱[RecyclerView.Adapter](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html)。

在下一個主題中，[基本 RecyclerView 範例](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)，範例應用程式會實作以示範實際的程式碼範例的組件和上面所述的功能。


## <a name="related-links"></a>相關連結

- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [基本的 RecyclerView 範例](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [延伸 RecyclerView 範例](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
