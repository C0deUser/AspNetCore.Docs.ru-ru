---
title: Компиляция файлов Razor в ASP.NET Core
author: rick-anderson
description: Узнайте, как происходит компиляция файлов Razor в приложении ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 4/8/2020
uid: mvc/views/view-compilation
ms.openlocfilehash: 7f329ffb4c63e8699663f49720145984bb8802fd
ms.sourcegitcommit: 9a46e78c79d167e5fa0cddf89c1ef584e5fe1779
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/09/2020
ms.locfileid: "80994611"
---
# <a name="razor-file-compilation-in-aspnet-core"></a>Компиляция файлов Razor в ASP.NET Core

Автор: [Рик Андерсон (Rick Anderson)](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

Файлы Razor с расширением *CSHTML* компилируются и во время сборки, и во время публикации с помощью [пакета SDK для Razor](xref:razor-pages/sdk). Компиляцию в среде выполнения при необходимости можно включить, настроив приложение.

## <a name="razor-compilation"></a>Компиляция Razor

Компиляция файлов Razor во время сборки и публикации включена по умолчанию с помощью пакета SDK для Razor. Если эта возможность включена, компиляция в среде выполнения дополняет компиляцию во время сборки, позволяя обновлять файлы Razor при редактировании.

## <a name="runtime-compilation"></a>компиляция среды выполнения.

Чтобы включить компиляцию в среде выполнения для всех сред и режимов конфигурации, сделайте следующее:

1. Установите пакет [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) NuGet.

1. обновите метод `Startup.ConfigureServices` в проекте, чтобы включить вызов `AddRazorRuntimeCompilation`. Пример:

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddRazorPages()
            .AddRazorRuntimeCompilation();

        // code omitted for brevity
    }
    ```

### <a name="conditionally-enable-runtime-compilation"></a>Условное включение компиляции в среде выполнения

Компиляцию в среде выполнения можно включить так, чтобы она была доступна только для локальной рабочей среды. Подобное условное включение гарантирует, что опубликованные выходные данные:

* используют скомпилированные представления;
* имеют меньший размер;
* не включают наблюдатели файлов в рабочей среде.

Чтобы включить компиляцию в среде выполнения для конкретной среды и конкретного режима конфигурации, сделайте следующее:

1. Условно сошлитесь на пакет [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) с учетом активного значения `Configuration`:

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation" Version="3.1.0" Condition="'$(Configuration)' == 'Debug'" />
    ```

1. обновите метод `Startup.ConfigureServices` в проекте, чтобы включить вызов `AddRazorRuntimeCompilation`. Условно выполните `AddRazorRuntimeCompilation`, чтобы он выполнялся только в режиме отладки, если для переменной `ASPNETCORE_ENVIRONMENT` задано значение `Development`:

    ```csharp
    public IWebHostEnvironment Env { get; set; }

    public void ConfigureServices(IServiceCollection services)
    {
        IMvcBuilder builder = services.AddRazorPages();

    #if DEBUG
        if (Env.IsDevelopment())
        {
            builder.AddRazorRuntimeCompilation();
        }
    #endif

        // code omitted for brevity
    }
    ```

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Файл Razor компилируется в среде выполнения при вызове связанной страницы Razor или представления MVC. Файлы Razor компилируются и во время сборки, и во время публикации с помощью [пакета SDK для Razor](xref:razor-pages/sdk).

## <a name="razor-compilation"></a>Компиляция Razor

Компиляция файлов Razor во время сборки и публикации включена по умолчанию с помощью пакета SDK для Razor. Редактирование файлов Razor после их обновления поддерживается во время сборки. По умолчанию только скомпилированные файлы *Views.dll*, а не *.cshtml*, и сборки, необходимые для компиляции файлов Razor, развертываются с приложением.

> [!IMPORTANT]
> Средство предварительной компиляции является нерекомендуемым и будет удалено в ASP.NET Core 3.0. Мы советуем перейти на [пакет SDK для Razor](xref:razor-pages/sdk).
>
> Пакет SDK для Razor применяется только в том случае, если в файле проекта не заданы свойства предварительной компиляции. Например, если в *CSPROJ*-файле для свойства `MvcRazorCompileOnPublish` установлено значение `true`, пакет SDK для Razor будет отключен.

## <a name="runtime-compilation"></a>компиляция среды выполнения.

Компиляция во время сборки дополняется компиляцией файлов Razor в среде выполнения. ASP.NET Core MVC будет перекомпилировать файлы Razor, когда содержимое файла *.cshtml* меняется.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>

::: moniker-end