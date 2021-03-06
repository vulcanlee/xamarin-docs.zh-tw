---
title: 建置 iOS 設計工具使用者介面
description: 本文件說明如何使用 Xamarin 設計工具，適用於 iOS 建置分鏡腳本和.xib 檔案的應用程式的使用者介面。 它會連結至討論工具的可用性、 其基本功能，可以設計控制項，並提供逐步解說及其使用的文件。
ms.prod: xamarin
ms.assetid: E35EFB69-EBBA-40E3-ADBE-CB8016F17127
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/31/2018
ms.openlocfilehash: a931373a6abba3084af3c7aefcdddc903ad1b577
ms.sourcegitcommit: 7a89735aed9ddf89c855fd33928915d72da40c2d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/19/2018
ms.locfileid: "36209228"
---
# <a name="building-user-interfaces-with-the-ios-designer"></a>建置 iOS 設計工具使用者介面

_適用於 iOS Xamarin 設計工具是完全整合適用於 Mac 和 Visual Studio 與 Visual Studio 的 iOS 分鏡腳本和介面產生器格式的視覺化設計工具。IOS 設計工具會維護與分鏡腳本和.xib 格式，完全相容，因此您可以在 Visual Studio for Mac 或除了 Xcode 的介面產生器的 Visual Studio 中編輯檔案。此外，適用於 iOS Xamarin 設計工具支援進階的功能，例如在設計階段在編輯器中呈現的自訂控制項。_

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![iOS 設計工具在 Visual Studio for Mac](images/designer-vsmac-sml.png "iOS 設計工具")](images/designer-vsmac.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![在 Visual Studio 中的設計工具 iOS](images/designer-vs.png "iOS 設計工具")](images/designer-vs.png#lightbox)

-----

## <a name="availability"></a>可用性

在 Visual Studio for Mac 和 Windows 上的 Visual Studio 2017 中使用 Xamarin 設計工具，適用於 iOS。

這些指南假設熟悉內容涵蓋[Xamarin.iOS 快速入門引導](~/ios/get-started/index.md)。

## <a name="ios-designer-basicsintroductionmd"></a>[iOS 設計工具基本概念](introduction.md)

本指南涵蓋 Xamarin iOS 設計工具的功能。 它涵蓋了設計工具的基本概念，其中顯示如何使用設計工具以視覺化方式配置控制項，以及如何編輯內容。

## <a name="designable-controls-overviewios-designable-controls-overviewmd"></a>[可以設計控制項概觀](ios-designable-controls-overview.md)

本指南會深入探討自訂控制項，如何建立和哪些需求必須符合在設計介面上呈現。 此外，它會示範如何偵錯時使用可以設計控制項可能會發生的常見問題。

## <a name="walkthrough---using-custom-controls-with-ios-designerios-designable-controls-walkthroughmd"></a>[逐步解說-iOS 設計工具搭配使用的自訂控制項](ios-designable-controls-walkthrough.md)

本文章提供逐步解說示範如何建立自訂控制項，並在 iOS 設計工具中使用它。 它會顯示如何讓控制項使用設計工具的工具箱中，因此它可以拖/放至檢視。 此外，它會顯示如何實作控制項，讓它正確呈現在設計階段與執行階段，以及如何建立可以在設計階段設定的屬性。

## <a name="auto-layout-with-the-xamarin-ios-designerdesigner-auto-layoutmd"></a>[使用 Xamarin iOS 設計工具的自動配置](designer-auto-layout.md)

本指南介紹 iOS 自動配置和新條件約束的工作流程 iOS 設計工具中可用。
