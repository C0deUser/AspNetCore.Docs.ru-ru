---
title: Начало работы с ASP.NET Core Блазор
author: guardrex
description: Приступите к работе с Блазор, создав приложение Блазор с любыми инструментами по своему усмотрению.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2019
uid: blazor/get-started
ms.openlocfilehash: 1358a2e92af9d9104e565718692b1ca1940b9d9e
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/13/2019
ms.locfileid: "68993407"
---
# <a name="get-started-with-aspnet-core-blazor"></a>Начало работы с ASP.NET Core Блазор

Авторы: [Дэниэл Рот (Daniel Roth)](https://github.com/danroth27) и [Люк Лэтем (Luke Latham)](https://github.com/guardrex)

Начало работы с Блазор:

1. Установите последнюю версию [пакета SDK для предварительной версии .NET Core 3,0](https://dotnet.microsoft.com/download/dotnet-core/3.0) .

1. Установите шаблоны Блазор, выполнив следующую команду в командной оболочке:

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.0.0-preview8.19405.7
   ```

1. Следуйте указаниям по выбору инструментов:

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   1 \. Установите последнюю предварительную версию [Visual Studio](https://visualstudio.com/vs/preview) с рабочей нагрузкой **ASP.NET и веб-разработка** .

   2 \. Создайте новый проект.

   3 \. Выберите **приложение блазор**. Выберите **Далее**.

   4 \. В поле **Имя проекта** укажите имя проекта или оставьте имя по умолчанию. Убедитесь, что запись **расположения** указана правильно, или укажите расположение для проекта. Выберите **Создать**.

   5 \. Для интерфейса Блазор на стороне клиента выберите шаблон **приложения блазор для сборки** . Для интерфейса Блазор на стороне сервера выберите шаблон серверное **приложение блазор** . Выберите **Создать**. Сведения о двух моделях размещения Блазор: на стороне сервера и на клиенте см. в разделе <xref:blazor/hosting-models>.

   6 \. Нажмите клавишу **F5** для запуска приложения.

   > [!NOTE]
   > Если вы установили расширение Visual Studio Блазор для предыдущей предварительной версии ASP.NET Core Блазор (Предварительная версия 6 или более ранняя версия), вы можете удалить расширение. Установка шаблонов Блазор в командной оболочке теперь достаточно для отображения шаблонов в Visual Studio.

   # <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

   1 \. Установите [Visual Studio Code](https://code.visualstudio.com/).

   2 \. Установите последнюю версию [ C# для расширения Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).

   3 \. Для интерфейса Блазор на стороне клиента выполните следующую команду в командной оболочке:

      ```console
      dotnet new blazorwasm -o WebApplication1
      ```

      Для интерфейса Блазор на стороне сервера выполните следующую команду в командной оболочке:

      ```console
      dotnet new blazorserver -o WebApplication1
      ```

      Сведения о двух моделях размещения Блазор: на стороне сервера и на клиенте см. в разделе <xref:blazor/hosting-models>.

   4 \. Откройте папку *WebApplication1* в Visual Studio Code.

   5 \. Для проекта на стороне сервера Блазор среда IDE запрашивает добавление ресурсов для сборки и отладки проекта. Выберите ответ **Да**.

   6 \. При использовании приложения Блазор на стороне сервера запустите приложение с помощью отладчика Visual Studio Code. Если используется клиентское приложение блазор, выполните `dotnet run` из папки проекта приложения.

   7 \. В браузере перейдите на адрес `https://localhost:5001`.

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor server-side experience, select the **Blazor Server App** template. For a Blazor client-side experience, select the **Blazor WebAssembly App** template. Select **Next**. For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli/)

   Для интерфейса Блазор на стороне клиента выполните следующие команды в командной оболочке:

   ```console
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Для интерфейса Блазор на стороне сервера выполните следующие команды в командной оболочке:

   ```console
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Сведения о двух моделях размещения Блазор: на стороне сервера и на клиенте см. в разделе <xref:blazor/hosting-models>.

   В браузере перейдите на адрес `https://localhost:5001`.

   ---

Из вкладок боковой панели доступны несколько страниц:

* Корневая
* Счетчик
* Выборка данных

На странице Counter нажмите кнопку **Click me** (Щелкните здесь), чтобы увеличить значение счетчика без обновления страницы. Увеличение счетчика на веб-странице обычно требует написания JavaScript, но компоненты Razor предоставляют более эффективный подход с помощью C#.

*Pages/Counter.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

Запрос `/counter` в браузере, как указано `@page` в директиве вверху, приводит к тому, что `Counter` компонент визуализирует свое содержимое. Компоненты подготавливаются к просмотру в памяти представления дерева подготовки, которое затем может использоваться для обновления пользовательского интерфейса в гибком и удобном виде.

При каждом выборе кнопки « **Click Me** »:

* `onclick` Событие срабатывает.
* вызывается метод `IncrementCount` .
* `currentCount` Значение увеличивается.
* Компонент снова готовится к просмотру.

Среда выполнения сравнивает новое содержимое с предыдущим содержимым и применяет только измененное содержимое к модель DOM (DOM).

Добавьте компонент в другой компонент с помощью синтаксиса HTML. Например, добавьте `Counter` компонент в домашнюю страницу приложения, `<Counter />` добавив элемент в `Index` компонент.

*Pages/Index.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

Запустите приложение. Домашняя страница имеет собственный счетчик, `Counter` предоставляемый компонентом.

Параметры компонента указываются с помощью атрибутов или дочернего [содержимого](xref:blazor/components#child-content), которые позволяют задавать свойства дочернего компонента. Чтобы добавить параметр в `Counter` компонент, обновите `@code` блок компонента:

* Добавьте открытое свойство для `IncrementAmount` `[Parameter]` с атрибутом.
* Изменение метод `IncrementCount`, чтобы он использовал `IncrementAmount` при увеличении значения `currentCount`.

*Pages/Counter.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

`IncrementAmount` Укажите вэлементе`<Counter>`компонента,используяатрибут. `Index`

*Pages/Index.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

Запустите приложение. Компонент имеет собственный счетчик, увеличивающийся на десять при каждом нажатии кнопки « **Click Me** ». `Index` Компонент `Counter` (*Counter. Razor*) по- `/counter` своему по-в то же время увеличивается на единицу.

## <a name="next-steps"></a>Следующие шаги

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:signalr/introduction>