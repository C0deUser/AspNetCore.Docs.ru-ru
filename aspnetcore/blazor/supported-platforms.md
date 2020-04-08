---
title: Поддерживаемые платформы ASP.NET Core Blazor
author: guardrex
description: Сведения о поддерживаемых платформах для ASP.NET Core Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/supported-platforms
ms.openlocfilehash: 505974280b5c96ec2bcae42c6e076ab67a15bb07
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2020
ms.locfileid: "78647110"
---
# <a name="aspnet-core-blazor-supported-platforms"></a>Поддерживаемые платформы ASP.NET Core Blazor

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="browser-requirements"></a>Требования к браузерам

### <a name="blazor-webassembly"></a>Blazor WebAssembly

| Браузер                          | Версия               |
| -------------------------------- | :-------------------: |
| Microsoft Edge                   | Текущий               |
| Mozilla Firefox                  | Текущий               |
| Google Chrome, включая Android | Текущий               |
| Safari, включая iOS            | Текущий               |
| Microsoft Internet Explorer      | Не поддерживается&dagger; |

&dagger;Microsoft Internet Explorer не поддерживает [WebAssembly](https://webassembly.org).

### <a name="blazor-server"></a>Blazor Server

| Браузер                          | Версия    |
| -------------------------------- | :--------: |
| Microsoft Edge                   | Текущий    |
| Mozilla Firefox                  | Текущий    |
| Google Chrome, включая Android | Текущий    |
| Safari, включая iOS            | Текущий    |
| Microsoft Internet Explorer      | 11&dagger; |

&dagger;Требуются дополнительные заполнения (например, обещания можно добавить с помощью пакета [Polyfill.io](https://polyfill.io/v3/)).

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:blazor/hosting-models>
