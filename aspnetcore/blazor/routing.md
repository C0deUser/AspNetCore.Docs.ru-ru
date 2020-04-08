---
title: Маршрутизация ASP.NET Core Blazor
author: guardrex
description: Узнайте, как маршрутизировать запросы в приложениях и использовать компонент NavLink.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/17/2020
no-loc:
- Blazor
- SignalR
uid: blazor/routing
ms.openlocfilehash: 87579c88a37e0258921e199db2b5d8c7627f5499
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2020
ms.locfileid: "80218899"
---
# <a name="aspnet-core-blazor-routing"></a><span data-ttu-id="eedb8-103">Маршрутизация ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="eedb8-103">ASP.NET Core Blazor routing</span></span>

<span data-ttu-id="eedb8-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="eedb8-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="eedb8-105">Узнайте, как маршрутизировать запросы и использовать компонент `NavLink` для создания навигационных ссылок в приложениях Blazor.</span><span class="sxs-lookup"><span data-stu-id="eedb8-105">Learn how to route requests and how to use the `NavLink` component to create navigation links in Blazor apps.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="eedb8-106">Интеграция маршрутизации конечных точек ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eedb8-106">ASP.NET Core endpoint routing integration</span></span>

<span data-ttu-id="eedb8-107">Blazor Server интегрирован в [маршрутизацию конечных точек ASP.NET Core](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="eedb8-107">Blazor Server is integrated into [ASP.NET Core Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="eedb8-108">Приложение ASP.NET Core настроено для приема входящих подключений для интерактивных компонентов с помощью `MapBlazorHub` в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="eedb8-108">An ASP.NET Core app is configured to accept incoming connections for interactive components with `MapBlazorHub` in `Startup.Configure`:</span></span>

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

<span data-ttu-id="eedb8-109">Наиболее типичная конфигурация — маршрутизация всех запросов на страницу Razor, которая выступает в качестве узла для серверной части приложения Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="eedb8-109">The most typical configuration is to route all requests to a Razor page, which acts as the host for the server-side part of the Blazor Server app.</span></span> <span data-ttu-id="eedb8-110">По соглашению страница *узла* обычно называется *_Host.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="eedb8-110">By convention, the *host* page is usually named *_Host.cshtml*.</span></span> <span data-ttu-id="eedb8-111">Маршрут, указанный в файле узла, называется *резервным маршрутом*, так как он работает с низким приоритетом в соответствии с правилами маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="eedb8-111">The route specified in the host file is called a *fallback route* because it operates with a low priority in route matching.</span></span> <span data-ttu-id="eedb8-112">Резервный маршрут рассматривается, если другие маршруты не сопоставляются.</span><span class="sxs-lookup"><span data-stu-id="eedb8-112">The fallback route is considered when other routes don't match.</span></span> <span data-ttu-id="eedb8-113">Это позволяет приложению использовать другие контроллеры и страницы, не мешая работе с приложением Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="eedb8-113">This allows the app to use others controllers and pages without interfering with the Blazor Server app.</span></span>

## <a name="route-templates"></a><span data-ttu-id="eedb8-114">Шаблоны маршрутов</span><span class="sxs-lookup"><span data-stu-id="eedb8-114">Route templates</span></span>

<span data-ttu-id="eedb8-115">Компонент `Router` позволяет выполнять маршрутизацию для каждого компонента с указанным маршрутом.</span><span class="sxs-lookup"><span data-stu-id="eedb8-115">The `Router` component enables routing to each component with a specified route.</span></span> <span data-ttu-id="eedb8-116">Компонент `Router` находится в файле *App.razor*.</span><span class="sxs-lookup"><span data-stu-id="eedb8-116">The `Router` component appears in the *App.razor* file:</span></span>

```razor
<Router AppAssembly="typeof(Startup).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <p>Sorry, there's nothing at this address.</p>
    </NotFound>
</Router>
```

<span data-ttu-id="eedb8-117">При компиляции файла *.razor* с директивой `@page` созданный класс предоставляет атрибут <xref:Microsoft.AspNetCore.Components.RouteAttribute> для указания шаблона маршрута.</span><span class="sxs-lookup"><span data-stu-id="eedb8-117">When a *.razor* file with an `@page` directive is compiled, the generated class is provided a <xref:Microsoft.AspNetCore.Components.RouteAttribute> specifying the route template.</span></span>

<span data-ttu-id="eedb8-118">В среде выполнения компонент `RouteView` выполняет следующие операции:</span><span class="sxs-lookup"><span data-stu-id="eedb8-118">At runtime, the `RouteView` component:</span></span>

* <span data-ttu-id="eedb8-119">получает `RouteData` от `Router` вместе с всеми необходимыми параметрами;</span><span class="sxs-lookup"><span data-stu-id="eedb8-119">Receives the `RouteData` from the `Router` along with any desired parameters.</span></span>
* <span data-ttu-id="eedb8-120">визуализирует указанный компонент с его макетом (или необязательным макетом по умолчанию), используя указанные параметры.</span><span class="sxs-lookup"><span data-stu-id="eedb8-120">Renders the specified component with its layout (or an optional default layout) using the specified parameters.</span></span>

<span data-ttu-id="eedb8-121">При необходимости можно указать параметр `DefaultLayout` с классом макета, который будет использоваться для компонентов, которые не задают макет.</span><span class="sxs-lookup"><span data-stu-id="eedb8-121">You can optionally specify a `DefaultLayout` parameter with a layout class to use for components that don't specify a layout.</span></span> <span data-ttu-id="eedb8-122">Шаблоны Blazor по умолчанию определяют компонент `MainLayout`.</span><span class="sxs-lookup"><span data-stu-id="eedb8-122">The default Blazor templates specify the `MainLayout` component.</span></span> <span data-ttu-id="eedb8-123">Файл *MainLayout.razor* находится в папке *Shared* проекта шаблона.</span><span class="sxs-lookup"><span data-stu-id="eedb8-123">*MainLayout.razor* is in the template project's *Shared* folder.</span></span> <span data-ttu-id="eedb8-124">Дополнительные сведения о макетах см. в разделе <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="eedb8-124">For more information on layouts, see <xref:blazor/layouts>.</span></span>

<span data-ttu-id="eedb8-125">К компоненту можно применить несколько шаблонов маршрутов.</span><span class="sxs-lookup"><span data-stu-id="eedb8-125">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="eedb8-126">Следующий компонент отвечает на запросы `/BlazorRoute` и `/DifferentBlazorRoute`.</span><span class="sxs-lookup"><span data-stu-id="eedb8-126">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

```razor
@page "/BlazorRoute"
@page "/DifferentBlazorRoute"

<h1>Blazor routing</h1>
```

> [!IMPORTANT]
> <span data-ttu-id="eedb8-127">Для правильного разрешения URL-адресов приложение должно включать тег `<base>` в файл *wwwroot/index.html* (Blazor WebAssembly) или файл *Pages/_Host.cshtml* (Blazor Server) с базовым путем к приложению, указанным в атрибуте `href` (`<base href="/">`).</span><span class="sxs-lookup"><span data-stu-id="eedb8-127">For URLs to resolve correctly, the app must include a `<base>` tag in its *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server) with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="eedb8-128">Дополнительные сведения см. в разделе <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="eedb8-128">For more information, see <xref:host-and-deploy/blazor/index#app-base-path>.</span></span>

## <a name="provide-custom-content-when-content-isnt-found"></a><span data-ttu-id="eedb8-129">Предоставление пользовательского содержимого, когда содержимое не найдено</span><span class="sxs-lookup"><span data-stu-id="eedb8-129">Provide custom content when content isn't found</span></span>

<span data-ttu-id="eedb8-130">Компонент `Router` позволяет приложению указать пользовательское содержимое, если содержимое для запрошенного маршрута не найдено.</span><span class="sxs-lookup"><span data-stu-id="eedb8-130">The `Router` component allows the app to specify custom content if content isn't found for the requested route.</span></span>

<span data-ttu-id="eedb8-131">В файле *App.razor* задайте пользовательское содержимое в параметре шаблона `NotFound` компонента `Router`.</span><span class="sxs-lookup"><span data-stu-id="eedb8-131">In the *App.razor* file, set custom content in the `NotFound` template parameter of the `Router` component:</span></span>

```razor
<Router AppAssembly="typeof(Startup).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <h1>Sorry</h1>
        <p>Sorry, there's nothing at this address.</p> b
    </NotFound>
</Router>
```

<span data-ttu-id="eedb8-132">Содержимое тегов `<NotFound>` может включать произвольные элементы, например другие интерактивные компоненты.</span><span class="sxs-lookup"><span data-stu-id="eedb8-132">The content of `<NotFound>` tags can include arbitrary items, such as other interactive components.</span></span> <span data-ttu-id="eedb8-133">Сведения о применении макета по умолчанию к содержимому `NotFound` см. в разделе <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="eedb8-133">To apply a default layout to `NotFound` content, see <xref:blazor/layouts>.</span></span>

## <a name="route-to-components-from-multiple-assemblies"></a><span data-ttu-id="eedb8-134">Маршрутизация к компонентам из нескольких сборок</span><span class="sxs-lookup"><span data-stu-id="eedb8-134">Route to components from multiple assemblies</span></span>

<span data-ttu-id="eedb8-135">Используйте параметр `AdditionalAssemblies`, чтобы указать дополнительные сборки для компонента `Router`, которые следует учитывать при поиске маршрутизируемых компонентов.</span><span class="sxs-lookup"><span data-stu-id="eedb8-135">Use the `AdditionalAssemblies` parameter to specify additional assemblies for the `Router` component to consider when searching for routable components.</span></span> <span data-ttu-id="eedb8-136">Указанные сборки рассматриваются в дополнение к сборке, указанной в `AppAssembly`.</span><span class="sxs-lookup"><span data-stu-id="eedb8-136">Specified assemblies are considered in addition to the `AppAssembly`-specified assembly.</span></span> <span data-ttu-id="eedb8-137">В следующем примере `Component1` представляет собой маршрутизируемый компонент, определенный в упоминаемой библиотеке классов.</span><span class="sxs-lookup"><span data-stu-id="eedb8-137">In the following example, `Component1` is a routable component defined in a referenced class library.</span></span> <span data-ttu-id="eedb8-138">В следующем примере `AdditionalAssemblies` приводится поддержка маршрутизации для `Component1`.</span><span class="sxs-lookup"><span data-stu-id="eedb8-138">The following `AdditionalAssemblies` example results in routing support for `Component1`:</span></span>

```razor
<Router
    AppAssembly="typeof(Program).Assembly"
    AdditionalAssemblies="new[] { typeof(Component1).Assembly }">
    ...
</Router>
```

## <a name="route-parameters"></a><span data-ttu-id="eedb8-139">Параметры маршрута</span><span class="sxs-lookup"><span data-stu-id="eedb8-139">Route parameters</span></span>

<span data-ttu-id="eedb8-140">Маршрутизатор использует параметры маршрута для заполнения соответствующих параметров компонента с тем же именем (без учета регистра).</span><span class="sxs-lookup"><span data-stu-id="eedb8-140">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

```razor
@page "/RouteParameter"
@page "/RouteParameter/{text}"

<h1>Blazor is @Text!</h1>

@code {
    [Parameter]
    public string Text { get; set; }

    protected override void OnInitialized()
    {
        Text = Text ?? "fantastic";
    }
}
```

<span data-ttu-id="eedb8-141">Необязательные параметры не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="eedb8-141">Optional parameters aren't supported.</span></span> <span data-ttu-id="eedb8-142">В предыдущем примере применяются две директивы `@page`.</span><span class="sxs-lookup"><span data-stu-id="eedb8-142">Two `@page` directives are applied in the previous example.</span></span> <span data-ttu-id="eedb8-143">Первая позволяет переходить к компоненту без параметра.</span><span class="sxs-lookup"><span data-stu-id="eedb8-143">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="eedb8-144">Вторая директива `@page` принимает параметр маршрута `{text}` и присваивает значение свойству `Text`.</span><span class="sxs-lookup"><span data-stu-id="eedb8-144">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="eedb8-145">Ограничения маршрута</span><span class="sxs-lookup"><span data-stu-id="eedb8-145">Route constraints</span></span>

<span data-ttu-id="eedb8-146">Ограничение маршрута применяет сопоставление типов в сегменте маршрута к компоненту.</span><span class="sxs-lookup"><span data-stu-id="eedb8-146">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="eedb8-147">В следующем примере маршрут к компоненту `Users` соответствует только в следующих случаях:</span><span class="sxs-lookup"><span data-stu-id="eedb8-147">In the following example, the route to the `Users` component only matches if:</span></span>

* <span data-ttu-id="eedb8-148">в URL-адресе запроса имеется сегмент маршрута `Id`;</span><span class="sxs-lookup"><span data-stu-id="eedb8-148">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="eedb8-149">сегмент `Id` является целым числом (`int`).</span><span class="sxs-lookup"><span data-stu-id="eedb8-149">The `Id` segment is an integer (`int`).</span></span>

[!code-razor[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

<span data-ttu-id="eedb8-150">Доступны ограничения маршрутов, приведенные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="eedb8-150">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="eedb8-151">Сведения об ограничениях маршрута, соответствующих инвариантному языку и региональным параметрам, см. в предупреждении внизу таблицы.</span><span class="sxs-lookup"><span data-stu-id="eedb8-151">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="eedb8-152">Ограничение</span><span class="sxs-lookup"><span data-stu-id="eedb8-152">Constraint</span></span> | <span data-ttu-id="eedb8-153">Пример</span><span class="sxs-lookup"><span data-stu-id="eedb8-153">Example</span></span>           | <span data-ttu-id="eedb8-154">Примеры совпадений</span><span class="sxs-lookup"><span data-stu-id="eedb8-154">Example Matches</span></span>                                                                  | <span data-ttu-id="eedb8-155">Инвариант</span><span class="sxs-lookup"><span data-stu-id="eedb8-155">Invariant</span></span><br><span data-ttu-id="eedb8-156">culture</span><span class="sxs-lookup"><span data-stu-id="eedb8-156">culture</span></span><br><span data-ttu-id="eedb8-157">соответствие</span><span class="sxs-lookup"><span data-stu-id="eedb8-157">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="eedb8-158">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="eedb8-158">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="eedb8-159">нет</span><span class="sxs-lookup"><span data-stu-id="eedb8-159">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="eedb8-160">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="eedb8-160">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="eedb8-161">Да</span><span class="sxs-lookup"><span data-stu-id="eedb8-161">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="eedb8-162">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="eedb8-162">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="eedb8-163">Да</span><span class="sxs-lookup"><span data-stu-id="eedb8-163">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="eedb8-164">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="eedb8-164">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="eedb8-165">Да</span><span class="sxs-lookup"><span data-stu-id="eedb8-165">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="eedb8-166">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="eedb8-166">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="eedb8-167">Да</span><span class="sxs-lookup"><span data-stu-id="eedb8-167">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="eedb8-168">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="eedb8-168">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="eedb8-169">нет</span><span class="sxs-lookup"><span data-stu-id="eedb8-169">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="eedb8-170">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="eedb8-170">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="eedb8-171">Да</span><span class="sxs-lookup"><span data-stu-id="eedb8-171">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="eedb8-172">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="eedb8-172">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="eedb8-173">Да</span><span class="sxs-lookup"><span data-stu-id="eedb8-173">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="eedb8-174">Ограничения маршрута, которые проверяют URL-адрес и могут быть преобразованы в тип CLR (например, `int` или `DateTime`), всегда используют инвариантные язык и региональные параметры.</span><span class="sxs-lookup"><span data-stu-id="eedb8-174">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="eedb8-175">Эти ограничения предполагают, что URL-адрес является нелокализуемым.</span><span class="sxs-lookup"><span data-stu-id="eedb8-175">These constraints assume that the URL is non-localizable.</span></span>

### <a name="routing-with-urls-that-contain-dots"></a><span data-ttu-id="eedb8-176">Маршрутизация URL-адресов, содержащих точки</span><span class="sxs-lookup"><span data-stu-id="eedb8-176">Routing with URLs that contain dots</span></span>

<span data-ttu-id="eedb8-177">В приложениях Blazor Server маршрутом по умолчанию в *_Host.cshtml* является `/` (`@page "/"`).</span><span class="sxs-lookup"><span data-stu-id="eedb8-177">In Blazor Server apps, the default route in *_Host.cshtml* is `/` (`@page "/"`).</span></span> <span data-ttu-id="eedb8-178">URL-адрес запроса, содержащий точку (`.`), не соответствует маршруту по умолчанию, так как такой адрес считается запросом файла.</span><span class="sxs-lookup"><span data-stu-id="eedb8-178">A request URL that contains a dot (`.`) isn't matched by the default route because the URL appears to request a file.</span></span> <span data-ttu-id="eedb8-179">Приложение Blazor возвращает ответ *404 — не найдено* для статического файла, который не существует.</span><span class="sxs-lookup"><span data-stu-id="eedb8-179">A Blazor app returns a *404 - Not Found* response for a static file that doesn't exist.</span></span> <span data-ttu-id="eedb8-180">Чтобы использовать маршруты, содержащие точку, добавьте в файл *_Host.cshtml* следующий шаблон маршрута.</span><span class="sxs-lookup"><span data-stu-id="eedb8-180">To use routes that contain a dot, configure *_Host.cshtml* with the following route template:</span></span>

```cshtml
@page "/{**path}"
```

<span data-ttu-id="eedb8-181">Шаблон `"/{**path}"` включает в себя следующие элементы:</span><span class="sxs-lookup"><span data-stu-id="eedb8-181">The `"/{**path}"` template includes:</span></span>

* <span data-ttu-id="eedb8-182">двойная *универсальная* звездочка (`**`) для захвата пути через границы нескольких папок без кодирования косой чертой (`/`);</span><span class="sxs-lookup"><span data-stu-id="eedb8-182">Double-asterisk *catch-all* syntax (`**`) to capture the path across multiple folder boundaries without encoding forward slashes (`/`).</span></span>
* <span data-ttu-id="eedb8-183">имя параметра маршрута `path`.</span><span class="sxs-lookup"><span data-stu-id="eedb8-183">`path` route parameter name.</span></span>

> [!NOTE]
> <span data-ttu-id="eedb8-184">*Универсальный* параметр синтаксиса (`*`/`**`) **не** поддерживается в компонентах Razor ( *.razor*).</span><span class="sxs-lookup"><span data-stu-id="eedb8-184">*Catch-all* parameter syntax (`*`/`**`) is **not** supported in Razor components (*.razor*).</span></span>

<span data-ttu-id="eedb8-185">Дополнительные сведения см. в разделе <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="eedb8-185">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="eedb8-186">Компонент NavLink</span><span class="sxs-lookup"><span data-stu-id="eedb8-186">NavLink component</span></span>

<span data-ttu-id="eedb8-187">Используйте при создании ссылок навигации компонент `NavLink` вместо HTML-элементов гиперссылок (`<a>`).</span><span class="sxs-lookup"><span data-stu-id="eedb8-187">Use a `NavLink` component in place of HTML hyperlink elements (`<a>`) when creating navigation links.</span></span> <span data-ttu-id="eedb8-188">Компонент `NavLink` ведет себя как элемент `<a>`, за исключением того, что он переключает класс CSS `active` в зависимости от того, соответствует ли его `href` текущему URL-адресу.</span><span class="sxs-lookup"><span data-stu-id="eedb8-188">A `NavLink` component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="eedb8-189">Класс `active` помогает пользователю понять, какая страница является активной страницей среди отображаемых ссылок навигации.</span><span class="sxs-lookup"><span data-stu-id="eedb8-189">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="eedb8-190">Следующий компонент `NavMenu` создает панель навигации [Bootstrap](https://getbootstrap.com/docs/), которая демонстрирует использование компонентов `NavLink`.</span><span class="sxs-lookup"><span data-stu-id="eedb8-190">The following `NavMenu` component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use `NavLink` components:</span></span>

[!code-razor[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

<span data-ttu-id="eedb8-191">Существует два параметра `NavLinkMatch`, которые можно назначить атрибуту `Match` элемента `<NavLink>`:</span><span class="sxs-lookup"><span data-stu-id="eedb8-191">There are two `NavLinkMatch` options that you can assign to the `Match` attribute of the `<NavLink>` element:</span></span>

* <span data-ttu-id="eedb8-192">`NavLinkMatch.All` &ndash; `NavLink` активен, если он соответствует всему текущему URL-адресу;</span><span class="sxs-lookup"><span data-stu-id="eedb8-192">`NavLinkMatch.All` &ndash; The `NavLink` is active when it matches the entire current URL.</span></span>
* <span data-ttu-id="eedb8-193">`NavLinkMatch.Prefix` (*по умолчанию*) &ndash; `NavLink` активен, если он соответствует любому префиксу текущего URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="eedb8-193">`NavLinkMatch.Prefix` (*default*) &ndash; The `NavLink` is active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="eedb8-194">В предыдущем примере `NavLink` `href=""` элемента Home соответствует домашнему URL-адресу и получает только класс CSS `active` в URL-адресе базового пути приложения по умолчанию (например, `https://localhost:5001/`).</span><span class="sxs-lookup"><span data-stu-id="eedb8-194">In the preceding example, the Home `NavLink` `href=""` matches the home URL and only receives the `active` CSS class at the app's default base path URL (for example, `https://localhost:5001/`).</span></span> <span data-ttu-id="eedb8-195">Второй `NavLink` получает класс `active`, когда пользователь посещает любой URL-адрес с префиксом `MyComponent` (например, `https://localhost:5001/MyComponent` и `https://localhost:5001/MyComponent/AnotherSegment`).</span><span class="sxs-lookup"><span data-stu-id="eedb8-195">The second `NavLink` receives the `active` class when the user visits any URL with a `MyComponent` prefix (for example, `https://localhost:5001/MyComponent` and `https://localhost:5001/MyComponent/AnotherSegment`).</span></span>

<span data-ttu-id="eedb8-196">Дополнительные атрибуты компонента `NavLink` передаются в отображаемый тег привязки.</span><span class="sxs-lookup"><span data-stu-id="eedb8-196">Additional `NavLink` component attributes are passed through to the rendered anchor tag.</span></span> <span data-ttu-id="eedb8-197">В следующем примере компонент `NavLink` включает атрибут `target`.</span><span class="sxs-lookup"><span data-stu-id="eedb8-197">In the following example, the `NavLink` component includes the `target` attribute:</span></span>

```razor
<NavLink href="my-page" target="_blank">My page</NavLink>
```

<span data-ttu-id="eedb8-198">Отобразится следующая разметка HTML.</span><span class="sxs-lookup"><span data-stu-id="eedb8-198">The following HTML markup is rendered:</span></span>

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a><span data-ttu-id="eedb8-199">URI и вспомогательные инструменты состояния навигации</span><span class="sxs-lookup"><span data-stu-id="eedb8-199">URI and navigation state helpers</span></span>

<span data-ttu-id="eedb8-200">Используйте <xref:Microsoft.AspNetCore.Components.NavigationManager> для работы с URI и навигацией в коде C#.</span><span class="sxs-lookup"><span data-stu-id="eedb8-200">Use <xref:Microsoft.AspNetCore.Components.NavigationManager> to work with URIs and navigation in C# code.</span></span> <span data-ttu-id="eedb8-201">`NavigationManager` предоставляет события и методы, приведенные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="eedb8-201">`NavigationManager` provides the event and methods shown in the following table.</span></span>

| <span data-ttu-id="eedb8-202">Участник</span><span class="sxs-lookup"><span data-stu-id="eedb8-202">Member</span></span> | <span data-ttu-id="eedb8-203">Description</span><span class="sxs-lookup"><span data-stu-id="eedb8-203">Description</span></span> |
| ------ | ----------- |
| <span data-ttu-id="eedb8-204">URI</span><span class="sxs-lookup"><span data-stu-id="eedb8-204">Uri</span></span> | <span data-ttu-id="eedb8-205">Возвращает текущий абсолютный URI.</span><span class="sxs-lookup"><span data-stu-id="eedb8-205">Gets the current absolute URI.</span></span> |
| <span data-ttu-id="eedb8-206">BaseUri</span><span class="sxs-lookup"><span data-stu-id="eedb8-206">BaseUri</span></span> | <span data-ttu-id="eedb8-207">Получает базовый URI (с завершающей косой чертой), который можно добавить в начало относительных путей URI для получения абсолютного URI.</span><span class="sxs-lookup"><span data-stu-id="eedb8-207">Gets the base URI (with a trailing slash) that can be prepended to relative URI paths to produce an absolute URI.</span></span> <span data-ttu-id="eedb8-208">Как правило, `BaseUri` соответствует атрибуту `href` элемента документа `<base>` в *wwwroot/index.html* (Blazor WebAssembly) или *Pages/_Host.cshtml* (Blazor Server).</span><span class="sxs-lookup"><span data-stu-id="eedb8-208">Typically, `BaseUri` corresponds to the `href` attribute on the document's `<base>` element in *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server).</span></span> |
| <span data-ttu-id="eedb8-209">NavigateTo</span><span class="sxs-lookup"><span data-stu-id="eedb8-209">NavigateTo</span></span> | <span data-ttu-id="eedb8-210">Переходит по указанному URI.</span><span class="sxs-lookup"><span data-stu-id="eedb8-210">Navigates to the specified URI.</span></span> <span data-ttu-id="eedb8-211">Если значение `forceLoad` равно `true`:</span><span class="sxs-lookup"><span data-stu-id="eedb8-211">If `forceLoad` is `true`:</span></span><ul><li><span data-ttu-id="eedb8-212">маршрутизация на стороне клиента обходится;</span><span class="sxs-lookup"><span data-stu-id="eedb8-212">Client-side routing is bypassed.</span></span></li><li><span data-ttu-id="eedb8-213">браузер принуждается к загрузке новой страницы с сервера, независимо от того, обрабатывается ли универсальный код ресурса клиентским маршрутизатором.</span><span class="sxs-lookup"><span data-stu-id="eedb8-213">The browser is forced to load the new page from the server, whether or not the URI is normally handled by the client-side router.</span></span></li></ul> |
| <span data-ttu-id="eedb8-214">LocationChanged</span><span class="sxs-lookup"><span data-stu-id="eedb8-214">LocationChanged</span></span> | <span data-ttu-id="eedb8-215">Событие, возникающее при изменении расположения навигации.</span><span class="sxs-lookup"><span data-stu-id="eedb8-215">An event that fires when the navigation location has changed.</span></span> |
| <span data-ttu-id="eedb8-216">ToAbsoluteUri</span><span class="sxs-lookup"><span data-stu-id="eedb8-216">ToAbsoluteUri</span></span> | <span data-ttu-id="eedb8-217">Преобразует относительный URI в абсолютный.</span><span class="sxs-lookup"><span data-stu-id="eedb8-217">Converts a relative URI into an absolute URI.</span></span> |
| <span data-ttu-id="eedb8-218"><span style="word-break:normal;word-wrap:normal">ToBaseRelativePath</span></span><span class="sxs-lookup"><span data-stu-id="eedb8-218"><span style="word-break:normal;word-wrap:normal">ToBaseRelativePath</span></span></span> | <span data-ttu-id="eedb8-219">Если указан базовый URI (например, URI, возвращенный ранее `GetBaseUri`), преобразует абсолютный URI в URI относительно базового префикса.</span><span class="sxs-lookup"><span data-stu-id="eedb8-219">Given a base URI (for example, a URI previously returned by `GetBaseUri`), converts an absolute URI into a URI relative to the base URI prefix.</span></span> |

<span data-ttu-id="eedb8-220">Следующий компонент при нажатии кнопки переходит к компоненту приложения `Counter`.</span><span class="sxs-lookup"><span data-stu-id="eedb8-220">The following component navigates to the app's `Counter` component when the button is selected:</span></span>

```razor
@page "/navigate"
@inject NavigationManager NavigationManager

<h1>Navigate in Code Example</h1>

<button class="btn btn-primary" @onclick="NavigateToCounterComponent">
    Navigate to the Counter component
</button>

@code {
    private void NavigateToCounterComponent()
    {
        NavigationManager.NavigateTo("counter");
    }
}
```

<span data-ttu-id="eedb8-221">Следующий компонент обрабатывает событие изменения расположения.</span><span class="sxs-lookup"><span data-stu-id="eedb8-221">The following component handles a location changed event.</span></span> <span data-ttu-id="eedb8-222">При вызове `HandleLocationChanged` платформой метод `Dispose` отсоединяется.</span><span class="sxs-lookup"><span data-stu-id="eedb8-222">The `HandleLocationChanged` method is unhooked when `Dispose` is called by the framework.</span></span> <span data-ttu-id="eedb8-223">После отсоединения метода становится возможной сборка мусора для компонента.</span><span class="sxs-lookup"><span data-stu-id="eedb8-223">Unhooking the method permits garbage collection of the component.</span></span>

```razor
@implement IDisposable
@inject NavigationManager NavigationManager

...

protected override void OnInitialized()
{
    NavigationManager.LocationChanged += HandleLocationChanged;
}

private void HandleLocationChanged(object sender, LocationChangedEventArgs e)
{
    ...
}

public void Dispose()
{
    NavigationManager.LocationChanged -= HandleLocationChanged;
}
```

<span data-ttu-id="eedb8-224">В <xref:Microsoft.AspNetCore.Components.Routing.LocationChangedEventArgs> содержатся следующие сведения о событии.</span><span class="sxs-lookup"><span data-stu-id="eedb8-224"><xref:Microsoft.AspNetCore.Components.Routing.LocationChangedEventArgs> provides the following information about the event:</span></span>

* <span data-ttu-id="eedb8-225"><xref:Microsoft.AspNetCore.Components.Routing.LocationChangedEventArgs.Location> &ndash; URL-адрес нового расположения.</span><span class="sxs-lookup"><span data-stu-id="eedb8-225"><xref:Microsoft.AspNetCore.Components.Routing.LocationChangedEventArgs.Location> &ndash; The URL of the new location.</span></span>
* <span data-ttu-id="eedb8-226"><xref:Microsoft.AspNetCore.Components.Routing.LocationChangedEventArgs.IsNavigationIntercepted> &ndash; Если `true`, Blazor перехватывает навигацию из браузера.</span><span class="sxs-lookup"><span data-stu-id="eedb8-226"><xref:Microsoft.AspNetCore.Components.Routing.LocationChangedEventArgs.IsNavigationIntercepted> &ndash; If `true`, Blazor intercepted the navigation from the browser.</span></span> <span data-ttu-id="eedb8-227">Если `false`, [NavigationManager.NavigateTo](xref:Microsoft.AspNetCore.Components.NavigationManager.NavigateTo%2A) приводит к переходу.</span><span class="sxs-lookup"><span data-stu-id="eedb8-227">If `false`, [NavigationManager.NavigateTo](xref:Microsoft.AspNetCore.Components.NavigationManager.NavigateTo%2A) caused the navigation to occur.</span></span>

<span data-ttu-id="eedb8-228">Дополнительные сведения об удалении компонентов см. в разделе <xref:blazor/lifecycle#component-disposal-with-idisposable>.</span><span class="sxs-lookup"><span data-stu-id="eedb8-228">For more information on component disposal, see <xref:blazor/lifecycle#component-disposal-with-idisposable>.</span></span>
