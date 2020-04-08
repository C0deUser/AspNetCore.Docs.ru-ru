---
title: Внедрение зависимостей Blazor в ASP.NET Core
author: guardrex
description: Сведения о том, как приложения Blazor могут внедрять службы в компоненты.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/20/2020
no-loc:
- Blazor
- SignalR
uid: blazor/dependency-injection
ms.openlocfilehash: 4cdde9ee8c9fd9adf00894a067d32965b180e5ec
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2020
ms.locfileid: "78646696"
---
# <a name="aspnet-core-blazor-dependency-injection"></a><span data-ttu-id="62fb0-103">Внедрение зависимостей Blazor в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="62fb0-103">ASP.NET Core Blazor dependency injection</span></span>

<span data-ttu-id="62fb0-104">Авторы: [Райнер Стропек](https://www.timecockpit.com) (Rainer Stropek) и [Майк Роусос](https://github.com/mjrousos) (Mike Rousos)</span><span class="sxs-lookup"><span data-stu-id="62fb0-104">By [Rainer Stropek](https://www.timecockpit.com) and [Mike Rousos](https://github.com/mjrousos)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="62fb0-105">Blazor поддерживает [внедрение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="62fb0-105">Blazor supports [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="62fb0-106">Приложения могут использовать встроенные службы, внедряя их в компоненты.</span><span class="sxs-lookup"><span data-stu-id="62fb0-106">Apps can use built-in services by injecting them into components.</span></span> <span data-ttu-id="62fb0-107">Приложения также могут определять и регистрировать пользовательские службы и предоставлять к ним доступ в рамках всего приложения с помощью внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="62fb0-107">Apps can also define and register custom services and make them available throughout the app via DI.</span></span>

<span data-ttu-id="62fb0-108">Внедрение зависимостей — это методика доступа к службам, настроенным в центральном расположении.</span><span class="sxs-lookup"><span data-stu-id="62fb0-108">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="62fb0-109">Она может пригодиться в приложениях Blazor для следующих задач:</span><span class="sxs-lookup"><span data-stu-id="62fb0-109">This can be useful in Blazor apps to:</span></span>

* <span data-ttu-id="62fb0-110">Совместное использование одного экземпляра класса службы во множестве компонентов, называемого *одноэлементной* службой.</span><span class="sxs-lookup"><span data-stu-id="62fb0-110">Share a single instance of a service class across many components, known as a *singleton* service.</span></span>
* <span data-ttu-id="62fb0-111">Отделение компонентов от конкретных классов служб с помощью абстракций ссылок.</span><span class="sxs-lookup"><span data-stu-id="62fb0-111">Decouple components from concrete service classes by using reference abstractions.</span></span> <span data-ttu-id="62fb0-112">Например, рассмотрим интерфейс `IDataAccess` для доступа к данным в приложении.</span><span class="sxs-lookup"><span data-stu-id="62fb0-112">For example, consider an interface `IDataAccess` for accessing data in the app.</span></span> <span data-ttu-id="62fb0-113">Этот интерфейс реализуется конкретным классом `DataAccess` и регистрируется в качестве службы в контейнере службы приложения.</span><span class="sxs-lookup"><span data-stu-id="62fb0-113">The interface is implemented by a concrete `DataAccess` class and registered as a service in the app's service container.</span></span> <span data-ttu-id="62fb0-114">Когда компонент использует внедрение зависимостей для получения реализации `IDataAccess`, этот компонент не связывается с конкретным типом.</span><span class="sxs-lookup"><span data-stu-id="62fb0-114">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="62fb0-115">Реализация может быть переключена, например, для реализации макета в модульных тестах.</span><span class="sxs-lookup"><span data-stu-id="62fb0-115">The implementation can be swapped, perhaps for a mock implementation in unit tests.</span></span>

## <a name="default-services"></a><span data-ttu-id="62fb0-116">Службы по умолчанию</span><span class="sxs-lookup"><span data-stu-id="62fb0-116">Default services</span></span>

<span data-ttu-id="62fb0-117">Службы по умолчанию автоматически добавляются в коллекцию служб приложения.</span><span class="sxs-lookup"><span data-stu-id="62fb0-117">Default services are automatically added to the app's service collection.</span></span>

| <span data-ttu-id="62fb0-118">Служба</span><span class="sxs-lookup"><span data-stu-id="62fb0-118">Service</span></span> | <span data-ttu-id="62fb0-119">Срок действия</span><span class="sxs-lookup"><span data-stu-id="62fb0-119">Lifetime</span></span> | <span data-ttu-id="62fb0-120">Description</span><span class="sxs-lookup"><span data-stu-id="62fb0-120">Description</span></span> |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="62fb0-121">Одноэлементный</span><span class="sxs-lookup"><span data-stu-id="62fb0-121">Singleton</span></span> | <span data-ttu-id="62fb0-122">Предоставляет методы для отправки HTTP-запросов и получения HTTP-ответов от ресурса с заданным URI.</span><span class="sxs-lookup"><span data-stu-id="62fb0-122">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI.</span></span><br><br><span data-ttu-id="62fb0-123">Экземпляр `HttpClient` в приложении Blazor WebAssembly использует браузер для обработки HTTP-трафика в фоновом режиме.</span><span class="sxs-lookup"><span data-stu-id="62fb0-123">The instance of `HttpClient` in a Blazor WebAssembly app uses the browser for handling the HTTP traffic in the background.</span></span><br><br><span data-ttu-id="62fb0-124">Приложения Blazor Server не включают `HttpClient`, по умолчанию настроенный в качестве службы.</span><span class="sxs-lookup"><span data-stu-id="62fb0-124">Blazor Server apps don't include an `HttpClient` configured as a service by default.</span></span> <span data-ttu-id="62fb0-125">Предоставьте `HttpClient` приложению Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="62fb0-125">Provide an `HttpClient` to a Blazor Server app.</span></span><br><br><span data-ttu-id="62fb0-126">Дополнительные сведения см. в разделе <xref:blazor/call-web-api>.</span><span class="sxs-lookup"><span data-stu-id="62fb0-126">For more information, see <xref:blazor/call-web-api>.</span></span> |
| `IJSRuntime` | <span data-ttu-id="62fb0-127">Одноэлементный (Blazor WebAssembly)</span><span class="sxs-lookup"><span data-stu-id="62fb0-127">Singleton (Blazor WebAssembly)</span></span><br><span data-ttu-id="62fb0-128">С заданной областью (Blazor Server)</span><span class="sxs-lookup"><span data-stu-id="62fb0-128">Scoped (Blazor Server)</span></span> | <span data-ttu-id="62fb0-129">Представляет экземпляр среды выполнения JavaScript, в которую отправляются вызовы JavaScript.</span><span class="sxs-lookup"><span data-stu-id="62fb0-129">Represents an instance of a JavaScript runtime where JavaScript calls are dispatched.</span></span> <span data-ttu-id="62fb0-130">Дополнительные сведения см. в разделе <xref:blazor/call-javascript-from-dotnet>.</span><span class="sxs-lookup"><span data-stu-id="62fb0-130">For more information, see <xref:blazor/call-javascript-from-dotnet>.</span></span> |
| `NavigationManager` | <span data-ttu-id="62fb0-131">Одноэлементный (Blazor WebAssembly)</span><span class="sxs-lookup"><span data-stu-id="62fb0-131">Singleton (Blazor WebAssembly)</span></span><br><span data-ttu-id="62fb0-132">С заданной областью (Blazor Server)</span><span class="sxs-lookup"><span data-stu-id="62fb0-132">Scoped (Blazor Server)</span></span> | <span data-ttu-id="62fb0-133">Содержит вспомогательные методы для работы с URI и состоянием навигации.</span><span class="sxs-lookup"><span data-stu-id="62fb0-133">Contains helpers for working with URIs and navigation state.</span></span> <span data-ttu-id="62fb0-134">Дополнительные сведения см. в разделе [URI и вспомогательные инструменты состояния навигации](xref:blazor/routing#uri-and-navigation-state-helpers).</span><span class="sxs-lookup"><span data-stu-id="62fb0-134">For more information, see [URI and navigation state helpers](xref:blazor/routing#uri-and-navigation-state-helpers).</span></span> |

<span data-ttu-id="62fb0-135">Пользовательский поставщик услуг не предоставляет перечисленные в таблице службы по умолчанию автоматически.</span><span class="sxs-lookup"><span data-stu-id="62fb0-135">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="62fb0-136">Если вы используете пользовательский поставщик услуг и нуждаетесь в какой-либо из служб, указанных в таблице, добавьте необходимые службы в новый поставщик услуг.</span><span class="sxs-lookup"><span data-stu-id="62fb0-136">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="add-services-to-an-app"></a><span data-ttu-id="62fb0-137">Добавление служб в приложение</span><span class="sxs-lookup"><span data-stu-id="62fb0-137">Add services to an app</span></span>

### <a name="blazor-webassembly"></a><span data-ttu-id="62fb0-138">Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="62fb0-138">Blazor WebAssembly</span></span>

<span data-ttu-id="62fb0-139">Настройте службы для коллекции служб приложения в методе `Main` файла *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="62fb0-139">Configure services for the app's service collection in the `Main` method of *Program.cs*.</span></span> <span data-ttu-id="62fb0-140">В следующем примере реализация `MyDependency` зарегистрирована для `IMyDependency`:</span><span class="sxs-lookup"><span data-stu-id="62fb0-140">In the following example, the `MyDependency` implementation is registered for `IMyDependency`:</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddSingleton<IMyDependency, MyDependency>();
        builder.RootComponents.Add<App>("app");

        await builder.Build().RunAsync();
    }
}
```

<span data-ttu-id="62fb0-141">После сборки узла доступ к службам можно получить из корневой области внедрения зависимостей до отрисовки каких-либо компонентов.</span><span class="sxs-lookup"><span data-stu-id="62fb0-141">Once the host is built, services can be accessed from the root DI scope before any components are rendered.</span></span> <span data-ttu-id="62fb0-142">Это может быть удобно для запуска логики инициализации перед отрисовкой содержимого:</span><span class="sxs-lookup"><span data-stu-id="62fb0-142">This can be useful for running initialization logic before rendering content:</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddSingleton<WeatherService>();
        builder.RootComponents.Add<App>("app");

        var host = builder.Build();

        var weatherService = host.Services.GetRequiredService<WeatherService>();
        await weatherService.InitializeWeatherAsync();

        await host.RunAsync();
    }
}
```

<span data-ttu-id="62fb0-143">Узел также предоставляет центральный экземпляр конфигурации для приложения.</span><span class="sxs-lookup"><span data-stu-id="62fb0-143">The host also provides a central configuration instance for the app.</span></span> <span data-ttu-id="62fb0-144">Основываясь на предыдущем примере, URL-адрес службы погоды передается из источника конфигурации по умолчанию (например, *appsettings.json*) в `InitializeWeatherAsync`:</span><span class="sxs-lookup"><span data-stu-id="62fb0-144">Building on the preceding example, the weather service's URL is passed from a default configuration source (for example, *appsettings.json*) to `InitializeWeatherAsync`:</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddSingleton<WeatherService>();
        builder.RootComponents.Add<App>("app");

        var host = builder.Build();

        var weatherService = host.Services.GetRequiredService<WeatherService>();
        await weatherService.InitializeWeatherAsync(
            host.Configuration["WeatherServiceUrl"]);

        await host.RunAsync();
    }
}
```

### <a name="blazor-server"></a><span data-ttu-id="62fb0-145">Blazor Server</span><span class="sxs-lookup"><span data-stu-id="62fb0-145">Blazor Server</span></span>

<span data-ttu-id="62fb0-146">После создания приложения изучите метод `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="62fb0-146">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="62fb0-147">В метод `ConfigureServices` передается <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, представляющий собой список объектов дескриптора службы (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span><span class="sxs-lookup"><span data-stu-id="62fb0-147">The `ConfigureServices` method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of service descriptor objects (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span></span> <span data-ttu-id="62fb0-148">Службы добавляются путем предоставления дескрипторов служб в коллекцию служб.</span><span class="sxs-lookup"><span data-stu-id="62fb0-148">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="62fb0-149">В следующем примере показана концепция с интерфейсом `IDataAccess` и его конкретной реализацией `DataAccess`.</span><span class="sxs-lookup"><span data-stu-id="62fb0-149">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

### <a name="service-lifetime"></a><span data-ttu-id="62fb0-150">Время существования службы</span><span class="sxs-lookup"><span data-stu-id="62fb0-150">Service lifetime</span></span>

<span data-ttu-id="62fb0-151">Для служб можно настроить параметры времени существования, указанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="62fb0-151">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="62fb0-152">Срок действия</span><span class="sxs-lookup"><span data-stu-id="62fb0-152">Lifetime</span></span> | <span data-ttu-id="62fb0-153">Description</span><span class="sxs-lookup"><span data-stu-id="62fb0-153">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | <span data-ttu-id="62fb0-154">Сейчас в приложениях Blazor WebAssembly концепция областей внедрения зависимостей отсутствует.</span><span class="sxs-lookup"><span data-stu-id="62fb0-154">Blazor WebAssembly apps don't currently have a concept of DI scopes.</span></span> <span data-ttu-id="62fb0-155">Службы, зарегистрированные как `Scoped`, работают аналогично службам `Singleton`.</span><span class="sxs-lookup"><span data-stu-id="62fb0-155">`Scoped`-registered services behave like `Singleton` services.</span></span> <span data-ttu-id="62fb0-156">Однако модель размещения сервера Blazor поддерживает время существования `Scoped`.</span><span class="sxs-lookup"><span data-stu-id="62fb0-156">However, the Blazor Server hosting model supports the `Scoped` lifetime.</span></span> <span data-ttu-id="62fb0-157">В приложениях Blazor Server регистрация службы с заданной областью ограничивается *соединением*.</span><span class="sxs-lookup"><span data-stu-id="62fb0-157">In Blazor Server apps, a scoped service registration is scoped to the *connection*.</span></span> <span data-ttu-id="62fb0-158">По этой причине использование служб с заданной областью предпочтительно для служб, которые должны быть ограничены текущим пользователем, даже если текущим намерением является запуск на стороне клиента в браузере.</span><span class="sxs-lookup"><span data-stu-id="62fb0-158">For this reason, using scoped services is preferred for services that should be scoped to the current user, even if the current intent is to run client-side in the browser.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | <span data-ttu-id="62fb0-159">Система внедрения зависимостей создает *один экземпляр* службы.</span><span class="sxs-lookup"><span data-stu-id="62fb0-159">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="62fb0-160">Все компоненты, для которых необходима служба `Singleton`, получают экземпляр той же службы.</span><span class="sxs-lookup"><span data-stu-id="62fb0-160">All components requiring a `Singleton` service receive an instance of the same service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | <span data-ttu-id="62fb0-161">Каждый раз, когда компонент получает экземпляр службы `Transient` из контейнера службы, он получает *новый экземпляр* этой службы.</span><span class="sxs-lookup"><span data-stu-id="62fb0-161">Whenever a component obtains an instance of a `Transient` service from the service container, it receives a *new instance* of the service.</span></span> |

<span data-ttu-id="62fb0-162">Система внедрения зависимостей основана на системе внедрения зависимостей в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="62fb0-162">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="62fb0-163">Дополнительные сведения см. в разделе <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="62fb0-163">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="62fb0-164">Запрос службы в компоненте</span><span class="sxs-lookup"><span data-stu-id="62fb0-164">Request a service in a component</span></span>

<span data-ttu-id="62fb0-165">После добавления служб в коллекцию служб внедрите их в компоненты, используя директиву [\@inject](xref:mvc/views/razor#inject) Razor.</span><span class="sxs-lookup"><span data-stu-id="62fb0-165">After services are added to the service collection, inject the services into the components using the [\@inject](xref:mvc/views/razor#inject) Razor directive.</span></span> <span data-ttu-id="62fb0-166">`@inject` имеет два параметра:</span><span class="sxs-lookup"><span data-stu-id="62fb0-166">`@inject` has two parameters:</span></span>

* <span data-ttu-id="62fb0-167">Тип &ndash; тип внедряемой службы.</span><span class="sxs-lookup"><span data-stu-id="62fb0-167">Type &ndash; The type of the service to inject.</span></span>
* <span data-ttu-id="62fb0-168">Свойство &ndash; имя свойства, получающего внедренную службу приложений.</span><span class="sxs-lookup"><span data-stu-id="62fb0-168">Property &ndash; The name of the property receiving the injected app service.</span></span> <span data-ttu-id="62fb0-169">Свойство не требуется создавать вручную.</span><span class="sxs-lookup"><span data-stu-id="62fb0-169">The property doesn't require manual creation.</span></span> <span data-ttu-id="62fb0-170">Его создает компилятор.</span><span class="sxs-lookup"><span data-stu-id="62fb0-170">The compiler creates the property.</span></span>

<span data-ttu-id="62fb0-171">Дополнительные сведения см. в разделе <xref:mvc/views/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="62fb0-171">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="62fb0-172">Используйте несколько инструкций `@inject` для внедрения различных служб.</span><span class="sxs-lookup"><span data-stu-id="62fb0-172">Use multiple `@inject` statements to inject different services.</span></span>

<span data-ttu-id="62fb0-173">В следующем примере показано, как использовать `@inject`.</span><span class="sxs-lookup"><span data-stu-id="62fb0-173">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="62fb0-174">Служба, реализующая `Services.IDataAccess`, внедряется в свойство `DataRepository` компонента.</span><span class="sxs-lookup"><span data-stu-id="62fb0-174">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="62fb0-175">Обратите внимание, что код использует только абстракцию `IDataAccess`:</span><span class="sxs-lookup"><span data-stu-id="62fb0-175">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-razor[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

<span data-ttu-id="62fb0-176">На внутреннем уровне создаваемое свойство (`DataRepository`) использует атрибут `InjectAttribute`.</span><span class="sxs-lookup"><span data-stu-id="62fb0-176">Internally, the generated property (`DataRepository`) uses the `InjectAttribute` attribute.</span></span> <span data-ttu-id="62fb0-177">Как правило, этот атрибут не используется напрямую.</span><span class="sxs-lookup"><span data-stu-id="62fb0-177">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="62fb0-178">Если базовый класс необходим для компонентов, а для базового класса также требуются обязательные свойства, добавьте `InjectAttribute` вручную:</span><span class="sxs-lookup"><span data-stu-id="62fb0-178">If a base class is required for components and injected properties are also required for the base class, manually add the `InjectAttribute`:</span></span>

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="62fb0-179">В компонентах, производных от базового класса, директива `@inject` не требуется.</span><span class="sxs-lookup"><span data-stu-id="62fb0-179">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="62fb0-180">`InjectAttribute` базового класса достаточно:</span><span class="sxs-lookup"><span data-stu-id="62fb0-180">The `InjectAttribute` of the base class is sufficient:</span></span>

```razor
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a><span data-ttu-id="62fb0-181">Использование внедрения зависимостей в службах</span><span class="sxs-lookup"><span data-stu-id="62fb0-181">Use DI in services</span></span>

<span data-ttu-id="62fb0-182">Для сложных служб могут потребоваться дополнительные службы.</span><span class="sxs-lookup"><span data-stu-id="62fb0-182">Complex services might require additional services.</span></span> <span data-ttu-id="62fb0-183">В предыдущем примере для `DataAccess` может потребоваться служба `HttpClient` по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="62fb0-183">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="62fb0-184">`@inject` (или `InjectAttribute`) невозможно использовать в службах.</span><span class="sxs-lookup"><span data-stu-id="62fb0-184">`@inject` (or the `InjectAttribute`) isn't available for use in services.</span></span> <span data-ttu-id="62fb0-185">Вместо этого следует использовать *внедрения конструктора*.</span><span class="sxs-lookup"><span data-stu-id="62fb0-185">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="62fb0-186">Необходимые службы добавляются путем добавления параметров в конструктор службы.</span><span class="sxs-lookup"><span data-stu-id="62fb0-186">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="62fb0-187">Когда система внедрения зависимостей создает службу, она распознает необходимые службы в конструкторе и предоставляет их соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="62fb0-187">When DI creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

```csharp
public class DataAccess : IDataAccess
{
    // The constructor receives an HttpClient via dependency
    // injection. HttpClient is a default service.
    public DataAccess(HttpClient client)
    {
        ...
    }
}
```

<span data-ttu-id="62fb0-188">Необходимые условия для внедрения конструктора:</span><span class="sxs-lookup"><span data-stu-id="62fb0-188">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="62fb0-189">Должен существовать один конструктор, аргументы которого могут использоваться системой внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="62fb0-189">One constructor must exist whose arguments can all be fulfilled by DI.</span></span> <span data-ttu-id="62fb0-190">Дополнительные параметры, не охваченные системой внедрения зависимостей, разрешены, если они указывают значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="62fb0-190">Additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="62fb0-191">Применимый конструктор должен быть *общедоступным*.</span><span class="sxs-lookup"><span data-stu-id="62fb0-191">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="62fb0-192">Должен существовать один подходящий конструктор.</span><span class="sxs-lookup"><span data-stu-id="62fb0-192">One applicable constructor must exist.</span></span> <span data-ttu-id="62fb0-193">В случае неоднозначности система внедрения зависимостей выдает исключение.</span><span class="sxs-lookup"><span data-stu-id="62fb0-193">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="utility-base-component-classes-to-manage-a-di-scope"></a><span data-ttu-id="62fb0-194">Служебные классы базовых компонентов служебной программы для управления областью внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="62fb0-194">Utility base component classes to manage a DI scope</span></span>

<span data-ttu-id="62fb0-195">В приложениях ASP.NET Core службы с заданной областью обычно ограничены текущим запросом.</span><span class="sxs-lookup"><span data-stu-id="62fb0-195">In ASP.NET Core apps, scoped services are typically scoped to the current request.</span></span> <span data-ttu-id="62fb0-196">По завершении запроса все временные службы или службы с заданной областью удаляются системой внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="62fb0-196">After the request completes, any scoped or transient services are disposed by the DI system.</span></span> <span data-ttu-id="62fb0-197">В приложениях Blazor Server область запроса существует на протяжении всего клиентского соединения, что может привести к тому, что временные службы или службы с заданной областью будут работать намного дольше, чем ожидалось.</span><span class="sxs-lookup"><span data-stu-id="62fb0-197">In Blazor Server apps, the request scope lasts for the duration of the client connection, which can result in transient and scoped services living much longer than expected.</span></span> <span data-ttu-id="62fb0-198">В приложениях Blazor WebAssembly службы, зарегистрированные с ограниченным временем существования, рассматриваются как одноэлементные, поэтому они существуют дольше, чем службы с заданной областью в типичных приложениях ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="62fb0-198">In Blazor WebAssembly apps, services registered with a scoped lifetime are treated as singletons, so they live longer than scoped services in typical ASP.NET Core apps.</span></span>

<span data-ttu-id="62fb0-199">Подход, ограничивающий время существования службы в приложениях Blazor, заключается в использовании типа `OwningComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="62fb0-199">An approach that limits a service lifetime in Blazor apps is use of the `OwningComponentBase` type.</span></span> <span data-ttu-id="62fb0-200">`OwningComponentBase` является абстрактным типом, производным от `ComponentBase`, который создает область внедрения зависимостей, соответствующую времени существования компонента.</span><span class="sxs-lookup"><span data-stu-id="62fb0-200">`OwningComponentBase` is an abstract type derived from `ComponentBase` that creates a DI scope corresponding to the lifetime of the component.</span></span> <span data-ttu-id="62fb0-201">Используя эту область, можно использовать службы внедрения зависимостей с ограниченным временем существования, заставляя их существовать так же долго, как и компонент.</span><span class="sxs-lookup"><span data-stu-id="62fb0-201">Using this scope, it's possible to use DI services with a scoped lifetime and have them live as long as the component.</span></span> <span data-ttu-id="62fb0-202">При удалении компонента также удаляются и службы из поставщика служб с заданной областью действия этого компонента.</span><span class="sxs-lookup"><span data-stu-id="62fb0-202">When the component is destroyed, services from the component's scoped service provider are disposed as well.</span></span> <span data-ttu-id="62fb0-203">Это может быть полезно для служб, которые:</span><span class="sxs-lookup"><span data-stu-id="62fb0-203">This can be useful for services that:</span></span>

* <span data-ttu-id="62fb0-204">требуется повторно использовать в компоненте, так как временное существование не подходит;</span><span class="sxs-lookup"><span data-stu-id="62fb0-204">Should be reused within a component, as the transient lifetime is inappropriate.</span></span>
* <span data-ttu-id="62fb0-205">не должны совместно использоваться компонентами, так как одноэлементное время существования не подходит.</span><span class="sxs-lookup"><span data-stu-id="62fb0-205">Shouldn't be shared across components, as the singleton lifetime is inappropriate.</span></span>

<span data-ttu-id="62fb0-206">Доступны две версии типа `OwningComponentBase`:</span><span class="sxs-lookup"><span data-stu-id="62fb0-206">Two versions of the `OwningComponentBase` type are available:</span></span>

* <span data-ttu-id="62fb0-207">`OwningComponentBase` является абстрактной высвобождаемой дочерней версией типа `ComponentBase` с защищенным свойством `ScopedServices` типа `IServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="62fb0-207">`OwningComponentBase` is an abstract, disposable child of the `ComponentBase` type with a protected `ScopedServices` property of type `IServiceProvider`.</span></span> <span data-ttu-id="62fb0-208">Этот поставщик можно использовать для разрешения служб, ограниченных временем существования компонента.</span><span class="sxs-lookup"><span data-stu-id="62fb0-208">This provider can be used to resolve services that are scoped to the lifetime of the component.</span></span>

  <span data-ttu-id="62fb0-209">Службы внедрения зависимостей, внедренные в компонент с помощью `@inject` или `InjectAttribute` (`[Inject]`), не создаются в области компонента.</span><span class="sxs-lookup"><span data-stu-id="62fb0-209">DI services injected into the component using `@inject` or the `InjectAttribute` (`[Inject]`) aren't created in the component's scope.</span></span> <span data-ttu-id="62fb0-210">Чтобы использовать область компонента, необходимо разрешить службы с помощью `ScopedServices.GetRequiredService` или `ScopedServices.GetService`.</span><span class="sxs-lookup"><span data-stu-id="62fb0-210">To use the component's scope, services must be resolved using `ScopedServices.GetRequiredService` or `ScopedServices.GetService`.</span></span> <span data-ttu-id="62fb0-211">Все службы, разрешенные с помощью поставщика `ScopedServices`, имеют свои зависимости из этой же области.</span><span class="sxs-lookup"><span data-stu-id="62fb0-211">Any services resolved using the `ScopedServices` provider have their dependencies provided from that same scope.</span></span>

  ```razor
  @page "/preferences"
  @using Microsoft.Extensions.DependencyInjection
  @inherits OwningComponentBase

  <h1>User (@UserService.Name)</h1>

  <ul>
      @foreach (var setting in SettingService.GetSettings())
      {
          <li>@setting.SettingName: @setting.SettingValue</li>
      }
  </ul>

  @code {
      private IUserService UserService { get; set; }
      private ISettingService SettingService { get; set; }

      protected override void OnInitialized()
      {
          UserService = ScopedServices.GetRequiredService<IUserService>();
          SettingService = ScopedServices.GetRequiredService<ISettingService>();
      }
  }
  ```

* <span data-ttu-id="62fb0-212">`OwningComponentBase<T>` является производным от `OwningComponentBase` и добавляет свойство `Service`, которое возвращает экземпляр `T` из поставщика внедрения зависимостей с заданной областью.</span><span class="sxs-lookup"><span data-stu-id="62fb0-212">`OwningComponentBase<T>` derives from `OwningComponentBase` and adds a property `Service` that returns an instance of `T` from the scoped DI provider.</span></span> <span data-ttu-id="62fb0-213">Этот тип удобен для доступа к службам с заданной областью без использования экземпляра `IServiceProvider` при наличии одной основной службы, которую приложение запрашивает из контейнера внедрения зависимостей с использованием области компонента.</span><span class="sxs-lookup"><span data-stu-id="62fb0-213">This type is a convenient way to access scoped services without using an instance of `IServiceProvider` when there's one primary service the app requires from the DI container using the component's scope.</span></span> <span data-ttu-id="62fb0-214">Свойство `ScopedServices` доступно, поэтому при необходимости приложение может получить службы других типов.</span><span class="sxs-lookup"><span data-stu-id="62fb0-214">The `ScopedServices` property is available, so the app can get services of other types, if necessary.</span></span>

  ```razor
  @page "/users"
  @attribute [Authorize]
  @inherits OwningComponentBase<AppDbContext>

  <h1>Users (@Service.Users.Count())</h1>

  <ul>
      @foreach (var user in Service.Users)
      {
          <li>@user.UserName</li>
      }
  </ul>
  ```

## <a name="use-of-entity-framework-dbcontext-from-di"></a><span data-ttu-id="62fb0-215">Использование DbContext Entity Framework из системы внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="62fb0-215">Use of Entity Framework DbContext from DI</span></span>

<span data-ttu-id="62fb0-216">Одним из распространенных типов службы, извлекаемым из системы внедрения зависимостей в веб-приложениях, являются объекты `DbContext` Entity Framework (EF).</span><span class="sxs-lookup"><span data-stu-id="62fb0-216">One common service type to retrieve from DI in web apps is Entity Framework (EF) `DbContext` objects.</span></span> <span data-ttu-id="62fb0-217">Регистрация служб EF с помощью `IServiceCollection.AddDbContext` добавляет `DbContext` в качестве службы с заданной областью по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="62fb0-217">Registering EF services using `IServiceCollection.AddDbContext` adds the `DbContext` as a scoped service by default.</span></span> <span data-ttu-id="62fb0-218">Регистрация в качестве службы с заданной областью может привести к проблемам в приложениях Blazor, так как это приводит к длительному существованию экземпляров `DbContext` и их совместному использованию в приложении.</span><span class="sxs-lookup"><span data-stu-id="62fb0-218">Registering as a scoped service can lead to problems in Blazor apps because it causes `DbContext` instances to be long-lived and shared across the app.</span></span> <span data-ttu-id="62fb0-219">`DbContext` не является потокобезопасным и не должен использоваться параллельно.</span><span class="sxs-lookup"><span data-stu-id="62fb0-219">`DbContext` isn't thread-safe and must not be used concurrently.</span></span>

<span data-ttu-id="62fb0-220">В зависимости от приложения использование `OwningComponentBase` для ограничения области `DbContext` одним компонентом *может* решить эту проблему.</span><span class="sxs-lookup"><span data-stu-id="62fb0-220">Depending on the app, using `OwningComponentBase` to limit the scope of a `DbContext` to a single component *may* solve the issue.</span></span> <span data-ttu-id="62fb0-221">Если компонент не использует `DbContext` параллельно, достаточно создать производный компонент от `OwningComponentBase` и извлечь `DbContext` из `ScopedServices`, так как это гарантирует следующее:</span><span class="sxs-lookup"><span data-stu-id="62fb0-221">If a component doesn't use a `DbContext` in parallel, deriving the component from `OwningComponentBase` and retrieving the `DbContext` from `ScopedServices` is sufficient because it ensures that:</span></span>

* <span data-ttu-id="62fb0-222">Отдельные компоненты не используют `DbContext` совместно.</span><span class="sxs-lookup"><span data-stu-id="62fb0-222">Separate components don't share a `DbContext`.</span></span>
* <span data-ttu-id="62fb0-223">`DbContext` существует столько же, сколько и зависящий от него компонент.</span><span class="sxs-lookup"><span data-stu-id="62fb0-223">The `DbContext` lives only as long as the component depending on it.</span></span>

<span data-ttu-id="62fb0-224">Если отдельный компонент может использовать `DbContext` параллельно (например, каждый раз, когда пользователь нажимает кнопку), даже использование `OwningComponentBase` не позволяет избежать проблем с параллельными операциями EF.</span><span class="sxs-lookup"><span data-stu-id="62fb0-224">If a single component might use a `DbContext` concurrently (for example, every time a user selects a button), even using `OwningComponentBase` doesn't avoid issues with concurrent EF operations.</span></span> <span data-ttu-id="62fb0-225">В этом случае используйте разные `DbContext` для каждой логической операции EF.</span><span class="sxs-lookup"><span data-stu-id="62fb0-225">In that case, use a different `DbContext` for each logical EF operation.</span></span> <span data-ttu-id="62fb0-226">Вы можете использовать один из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="62fb0-226">Use either of the following approaches:</span></span>

* <span data-ttu-id="62fb0-227">Создайте `DbContext` напрямую с использованием `DbContextOptions<TContext>` в качестве аргумента, который можно извлечь из системы внедрения зависимостей и является потокобезопасным.</span><span class="sxs-lookup"><span data-stu-id="62fb0-227">Create the `DbContext` directly using `DbContextOptions<TContext>` as an argument, which can be retrieved from DI and is thread safe.</span></span>

    ```razor
    @page "/example"
    @inject DbContextOptions<AppDbContext> DbContextOptions

    <ul>
        @foreach (var item in _data)
        {
            <li>@item</li>
        }
    </ul>

    <button @onclick="LoadData">Load Data</button>

    @code {
        private List<string> _data = new List<string>();

        private async Task LoadData()
        {
            _data = await GetAsync();
            StateHasChanged();
        }

        public async Task<List<string>> GetAsync()
        {
            using (var context = new AppDbContext(DbContextOptions))
            {
                return await context.Products.Select(p => p.Name).ToListAsync();
            }
        }
    }
    ```

* <span data-ttu-id="62fb0-228">Зарегистрируйте `DbContext` в контейнере службы с временным существованием:</span><span class="sxs-lookup"><span data-stu-id="62fb0-228">Register the `DbContext` in the service container with a transient lifetime:</span></span>
  * <span data-ttu-id="62fb0-229">При регистрации контекста используйте `ServiceLifetime.Transient`.</span><span class="sxs-lookup"><span data-stu-id="62fb0-229">When registering the context, use `ServiceLifetime.Transient`.</span></span> <span data-ttu-id="62fb0-230">Метод расширения `AddDbContext` принимает два необязательных параметра типа `ServiceLifetime`.</span><span class="sxs-lookup"><span data-stu-id="62fb0-230">The `AddDbContext` extension method takes two optional parameters of type `ServiceLifetime`.</span></span> <span data-ttu-id="62fb0-231">Чтобы использовать этот подход, только параметр `contextLifetime` должен иметь значение `ServiceLifetime.Transient`.</span><span class="sxs-lookup"><span data-stu-id="62fb0-231">To use this approach, only the `contextLifetime` parameter needs to be `ServiceLifetime.Transient`.</span></span> <span data-ttu-id="62fb0-232">`optionsLifetime` может иметь значение по умолчанию `ServiceLifetime.Scoped`.</span><span class="sxs-lookup"><span data-stu-id="62fb0-232">`optionsLifetime` can keep its default value of `ServiceLifetime.Scoped`.</span></span>

    ```csharp
    services.AddDbContext<AppDbContext>(options =>
         options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")),
         ServiceLifetime.Transient);
    ```  

  * <span data-ttu-id="62fb0-233">Временный объект `DbContext` можно внедрить как обычный (с помощью `@inject`) в компоненты, которые не будут выполнять несколько операций EF параллельно.</span><span class="sxs-lookup"><span data-stu-id="62fb0-233">The transient `DbContext` can be injected as normal (using `@inject`) into components that will not execute multiple EF operations in parallel.</span></span> <span data-ttu-id="62fb0-234">Те компоненты, которые могут выполнять несколько операций EF одновременно, могут запрашивать отдельные объекты `DbContext` для каждой параллельной операции с помощью `IServiceProvider.GetRequiredService`.</span><span class="sxs-lookup"><span data-stu-id="62fb0-234">Those that may perform multiple EF operations simultaneously can request separate `DbContext` objects for each parallel operation using `IServiceProvider.GetRequiredService`.</span></span>

    ```razor
    @page "/example"
    @using Microsoft.Extensions.DependencyInjection
    @inject IServiceProvider ServiceProvider

    <ul>
        @foreach (var item in _data)
        {
            <li>@item</li>
        }
    </ul>

    <button @onclick="LoadData">Load Data</button>

    @code {
        private List<string> _data = new List<string>();

        private async Task LoadData()
        {
            _data = await GetAsync();
            StateHasChanged();
        }

        public async Task<List<string>> GetAsync()
        {
            using (var context = ServiceProvider.GetRequiredService<AppDbContext>())
            {
                return await context.Products.Select(p => p.Name).ToListAsync();
            }
        }
    }
    ```

## <a name="additional-resources"></a><span data-ttu-id="62fb0-235">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="62fb0-235">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
