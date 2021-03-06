---
title: 部署停用 Configuration Manager 中的核取方塊
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: aaf675cd-d885-4dac-9754-77dbcaea3be9
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: ab825ba4d28ca8768e5c633fc3779828638a498d
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/09/2018
ms.locfileid: "33919510"
---
# <a name="deploy-checkboxes-disabled-in-configuration-manager"></a>部署停用 Configuration Manager 中的核取方塊

因為 Xamarin 3.5 Xamarin.iOS 專案部署自動每當您按下**啟動**工具列按鈕或挑選**偵錯 > 開始偵錯**功能表項目。 您仍然需要設定所需的 Xamarin.iOS 應用程式專案，**啟始專案**任何的命令之前執行之前。

因為這個緣故，**部署**核取方塊，已刻意停用 Xamarin.iOS 專案 Visual Studio 組態管理員：

![](deploy-checkboxes-images/configuration.png "顯示停用 Xamarin 3.5 Xamarin.iOS 專案 '部署' 核取方塊的 visual Studio 組態管理員")

這項變更可以解決當 Xamarin.iOS 應用程式專案未設定為部署舊版 Xamarin （3.3 或更早版本） 中出現錯誤：

![](deploy-checkboxes-images/error.png "錯誤對話方塊： 專案 iPhoneApp1 需要它才能啟動部署。請確認已部署在 方案組態管理員 」 中選取專案。")
