---
title: Команда dotnet aspnet-codegenerator
author: rick-anderson
description: Команда dotnet aspnet-codegenerator формирует шаблоны для проектов ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/04/2019
uid: fundamentals/tools/dotnet-aspnet-codegenerator
ms.openlocfilehash: 1043a578f66d5bb57f4a81e9fe21afa5e3c37cb8
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2020
ms.locfileid: "78649858"
---
# <a name="dotnet-aspnet-codegenerator"></a><span data-ttu-id="c996d-103">dotnet aspnet-codegenerator</span><span class="sxs-lookup"><span data-stu-id="c996d-103">dotnet aspnet-codegenerator</span></span>

<span data-ttu-id="c996d-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c996d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c996d-105">Команда `dotnet aspnet-codegenerator` запускает подсистему формирования шаблонов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c996d-105">`dotnet aspnet-codegenerator` - Runs the ASP.NET Core scaffolding engine.</span></span> <span data-ttu-id="c996d-106">Команда `dotnet aspnet-codegenerator` нужна только для командной строки. Она не требуется для формирования шаблонов в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c996d-106">`dotnet aspnet-codegenerator` is only required to scaffold from the command line, it's not needed to use scaffolding with Visual Studio.</span></span>

<span data-ttu-id="c996d-107">Эта статья относится к [пакету SDK для .NET Core 2.1](https://dotnet.microsoft.com/download/dotnet-core/2.1) и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="c996d-107">This article applies to [.NET Core 2.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/2.1) and later.</span></span>

## <a name="installing-aspnet-codegenerator"></a><span data-ttu-id="c996d-108">Установка aspnet-codegenerator</span><span class="sxs-lookup"><span data-stu-id="c996d-108">Installing aspnet-codegenerator</span></span>

<span data-ttu-id="c996d-109">`dotnet-aspnet-codegenerator` — [глобальное средство](/dotnet/core/tools/global-tools), которое нужно установить.</span><span class="sxs-lookup"><span data-stu-id="c996d-109">`dotnet-aspnet-codegenerator` is a [global tool](/dotnet/core/tools/global-tools) that must be installed.</span></span> <span data-ttu-id="c996d-110">Следующая команда позволяет установить последнюю стабильную версию средства `dotnet-aspnet-codegenerator`:</span><span class="sxs-lookup"><span data-stu-id="c996d-110">The following command installs the latest stable version of the `dotnet-aspnet-codegenerator` tool:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="c996d-111">Приведенная ниже команда позволяет обновить `dotnet-aspnet-codegenerator` до последней стабильной версии, доступной из установленных пакетов SDK для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="c996d-111">The following command updates `dotnet-aspnet-codegenerator` to the latest stable version available from the installed .NET Core SDKs:</span></span>

```dotnetcli
dotnet tool update -g dotnet-aspnet-codegenerator
```

## <a name="synopsis"></a><span data-ttu-id="c996d-112">Краткий обзор</span><span class="sxs-lookup"><span data-stu-id="c996d-112">Synopsis</span></span>

```
dotnet aspnet-codegenerator [arguments] [-p|--project] [-n|--nuget-package-dir] [-c|--configuration] [-tfm|--target-framework] [-b|--build-base-path] [--no-build] 
dotnet aspnet-codegenerator [-h|--help]
```

## <a name="description"></a><span data-ttu-id="c996d-113">Description</span><span class="sxs-lookup"><span data-stu-id="c996d-113">Description</span></span>

<span data-ttu-id="c996d-114">Глобальная команда `dotnet aspnet-codegenerator` запускает генератор кода и подсистему формирования шаблонов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c996d-114">The `dotnet aspnet-codegenerator` global command runs the ASP.NET Core code generator and scaffolding engine.</span></span>

## <a name="arguments"></a><span data-ttu-id="c996d-115">Аргументы</span><span class="sxs-lookup"><span data-stu-id="c996d-115">Arguments</span></span>

`generator`

<span data-ttu-id="c996d-116">Запускаемый генератор кода.</span><span class="sxs-lookup"><span data-stu-id="c996d-116">The code generator to run.</span></span> <span data-ttu-id="c996d-117">Доступны такие генераторы:</span><span class="sxs-lookup"><span data-stu-id="c996d-117">The following generators are available:</span></span>

| <span data-ttu-id="c996d-118">Generator</span><span class="sxs-lookup"><span data-stu-id="c996d-118">Generator</span></span> | <span data-ttu-id="c996d-119">Операция</span><span class="sxs-lookup"><span data-stu-id="c996d-119">Operation</span></span> |
| ----------------- | ------------ | 
| <span data-ttu-id="c996d-120">площадь</span><span class="sxs-lookup"><span data-stu-id="c996d-120">area</span></span>      | [<span data-ttu-id="c996d-121">Формирует шаблон области</span><span class="sxs-lookup"><span data-stu-id="c996d-121">Scaffolds an Area</span></span>](/aspnet/core/mvc/controllers/areas) |
  <span data-ttu-id="c996d-122">controller</span><span class="sxs-lookup"><span data-stu-id="c996d-122">controller</span></span>| [<span data-ttu-id="c996d-123">Формирует шаблон контроллера</span><span class="sxs-lookup"><span data-stu-id="c996d-123">Scaffolds a controller</span></span>](/aspnet/core/tutorials/first-mvc-app/adding-model) |
  <span data-ttu-id="c996d-124">identity</span><span class="sxs-lookup"><span data-stu-id="c996d-124">identity</span></span>  | [<span data-ttu-id="c996d-125">Формирует шаблон удостоверения</span><span class="sxs-lookup"><span data-stu-id="c996d-125">Scaffolds Identity</span></span>](/aspnet/core/security/authentication/scaffold-identity) |
  <span data-ttu-id="c996d-126">razorpage</span><span class="sxs-lookup"><span data-stu-id="c996d-126">razorpage</span></span> | [<span data-ttu-id="c996d-127">Формирует шаблоны страниц Razor Pages</span><span class="sxs-lookup"><span data-stu-id="c996d-127">Scaffolds Razor Pages</span></span>](/aspnet/core/tutorials/razor-pages/model) |
  <span data-ttu-id="c996d-128">представление</span><span class="sxs-lookup"><span data-stu-id="c996d-128">view</span></span>      | [<span data-ttu-id="c996d-129">Формирует шаблон представления</span><span class="sxs-lookup"><span data-stu-id="c996d-129">Scaffolds a view</span></span>](/aspnet/core/mvc/views/overview) |

## <a name="options"></a><span data-ttu-id="c996d-130">Параметры</span><span class="sxs-lookup"><span data-stu-id="c996d-130">Options</span></span>

`-n|--nuget-package-dir`

<span data-ttu-id="c996d-131">Позволяет указать каталог пакета NuGet.</span><span class="sxs-lookup"><span data-stu-id="c996d-131">Specifies the NuGet package directory.</span></span>

`-c|--configuration {Debug|Release}`

<span data-ttu-id="c996d-132">Определяет конфигурацию сборки.</span><span class="sxs-lookup"><span data-stu-id="c996d-132">Defines the build configuration.</span></span> <span data-ttu-id="c996d-133">Значение по умолчанию — `Debug`.</span><span class="sxs-lookup"><span data-stu-id="c996d-133">The default value is `Debug`.</span></span>

`-tfm|--target-framework`

<span data-ttu-id="c996d-134">[Целевая платформа](/dotnet/standard/frameworks) для использования.</span><span class="sxs-lookup"><span data-stu-id="c996d-134">Target [Framework](/dotnet/standard/frameworks) to use.</span></span> <span data-ttu-id="c996d-135">Например, `net46`.</span><span class="sxs-lookup"><span data-stu-id="c996d-135">For example, `net46`.</span></span>

`-b|--build-base-path`

<span data-ttu-id="c996d-136">Базовый путь сборки.</span><span class="sxs-lookup"><span data-stu-id="c996d-136">The build base path.</span></span>

`-h|--help`

<span data-ttu-id="c996d-137">Выводит краткую справку по команде.</span><span class="sxs-lookup"><span data-stu-id="c996d-137">Prints out a short help for the command.</span></span>

`--no-build`

<span data-ttu-id="c996d-138">Не выполняет сборку проекта перед запуском.</span><span class="sxs-lookup"><span data-stu-id="c996d-138">Doesn't build the project before running.</span></span> <span data-ttu-id="c996d-139">Он также неявно задает флаг `--no-restore`.</span><span class="sxs-lookup"><span data-stu-id="c996d-139">It also implicitly sets the `--no-restore` flag.</span></span>

`-p|--project <PATH>`

<span data-ttu-id="c996d-140">Задает путь к запускаемому файлу проекта (имя папки или полный путь).</span><span class="sxs-lookup"><span data-stu-id="c996d-140">Specifies the path of the project file to run (folder name or full path).</span></span> <span data-ttu-id="c996d-141">Если значение не задано, по умолчанию используется текущий каталог.</span><span class="sxs-lookup"><span data-stu-id="c996d-141">If not specified, it defaults to the current directory.</span></span>

## <a name="generator-options"></a><span data-ttu-id="c996d-142">Параметры генератора</span><span class="sxs-lookup"><span data-stu-id="c996d-142">Generator options</span></span>

<span data-ttu-id="c996d-143">В следующих разделах подробно описаны параметры, доступные для поддерживаемых генераторов.</span><span class="sxs-lookup"><span data-stu-id="c996d-143">The following sections detail the options available for the supported generators:</span></span>

* <span data-ttu-id="c996d-144">Область</span><span class="sxs-lookup"><span data-stu-id="c996d-144">Area</span></span>
* <span data-ttu-id="c996d-145">Контроллер</span><span class="sxs-lookup"><span data-stu-id="c996d-145">Controller</span></span>
* <span data-ttu-id="c996d-146">Удостоверение</span><span class="sxs-lookup"><span data-stu-id="c996d-146">Identity</span></span>  
* <span data-ttu-id="c996d-147">Razorpage</span><span class="sxs-lookup"><span data-stu-id="c996d-147">Razorpage</span></span>
* <span data-ttu-id="c996d-148">Представление</span><span class="sxs-lookup"><span data-stu-id="c996d-148">View</span></span>

<a name="area"></a>

### <a name="area-options"></a><span data-ttu-id="c996d-149">Параметры области</span><span class="sxs-lookup"><span data-stu-id="c996d-149">Area options</span></span>

<span data-ttu-id="c996d-150">Это средство предназначено для веб-проектов ASP.NET Core с контроллерами и представлениями.</span><span class="sxs-lookup"><span data-stu-id="c996d-150">This tool is intended for ASP.NET Core web projects with controllers and views.</span></span> <span data-ttu-id="c996d-151">Оно не предназначено для приложений Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="c996d-151">It's not intended for Razor Pages apps.</span></span>

<span data-ttu-id="c996d-152">Использование: `dotnet aspnet-codegenerator area AreaNameToGenerate`</span><span class="sxs-lookup"><span data-stu-id="c996d-152">Usage: `dotnet aspnet-codegenerator area AreaNameToGenerate`</span></span>

<span data-ttu-id="c996d-153">Приведенная выше команда создает такие папки:</span><span class="sxs-lookup"><span data-stu-id="c996d-153">The preceding command generates the following folders:</span></span>

* <span data-ttu-id="c996d-154">*Области*</span><span class="sxs-lookup"><span data-stu-id="c996d-154">*Areas*</span></span>
  * <span data-ttu-id="c996d-155">*AreaNameToGenerate*</span><span class="sxs-lookup"><span data-stu-id="c996d-155">*AreaNameToGenerate*</span></span>
    * <span data-ttu-id="c996d-156">*Контроллеры*</span><span class="sxs-lookup"><span data-stu-id="c996d-156">*Controllers*</span></span>
    * <span data-ttu-id="c996d-157">*Data*</span><span class="sxs-lookup"><span data-stu-id="c996d-157">*Data*</span></span>
    * <span data-ttu-id="c996d-158">*Модели*</span><span class="sxs-lookup"><span data-stu-id="c996d-158">*Models*</span></span>
    * <span data-ttu-id="c996d-159">*Представления*</span><span class="sxs-lookup"><span data-stu-id="c996d-159">*Views*</span></span>

<a name="ctl"></a>

### <a name="controller-options"></a><span data-ttu-id="c996d-160">Параметры контроллера</span><span class="sxs-lookup"><span data-stu-id="c996d-160">Controller options</span></span>

<span data-ttu-id="c996d-161">В таблице ниже перечислены параметры для `aspnet-codegenerator` `controller` и `razorpage`.</span><span class="sxs-lookup"><span data-stu-id="c996d-161">The following table lists options for  `aspnet-codegenerator` `controller` and `razorpage`:</span></span>

[!INCLUDE [aspnet-codegenerator-args-md.md](~/includes/aspnet-codegenerator-args-md.md)]

<span data-ttu-id="c996d-162">В таблице ниже перечислены параметры только для `aspnet-codegenerator controller`.</span><span class="sxs-lookup"><span data-stu-id="c996d-162">The following table lists options unique to  `aspnet-codegenerator controller`:</span></span>

| <span data-ttu-id="c996d-163">Параметр</span><span class="sxs-lookup"><span data-stu-id="c996d-163">Option</span></span>               | <span data-ttu-id="c996d-164">Description</span><span class="sxs-lookup"><span data-stu-id="c996d-164">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="c996d-165">--controllerName или -name</span><span class="sxs-lookup"><span data-stu-id="c996d-165">--controllerName or -name</span></span> | <span data-ttu-id="c996d-166">Имя контроллера.</span><span class="sxs-lookup"><span data-stu-id="c996d-166">Name of the controller.</span></span> |
| <span data-ttu-id="c996d-167">--useAsyncActions или -async</span><span class="sxs-lookup"><span data-stu-id="c996d-167">--useAsyncActions or -async</span></span> | <span data-ttu-id="c996d-168">Создание асинхронных действий контроллера.</span><span class="sxs-lookup"><span data-stu-id="c996d-168">Generate async controller actions.</span></span> |
| <span data-ttu-id="c996d-169">--noViews или -nv</span><span class="sxs-lookup"><span data-stu-id="c996d-169">--noViews or -nv</span></span> | <span data-ttu-id="c996d-170">Представления **не** создаются.</span><span class="sxs-lookup"><span data-stu-id="c996d-170">Generate **no** views.</span></span> |
| <span data-ttu-id="c996d-171">--restWithNoViews или -api</span><span class="sxs-lookup"><span data-stu-id="c996d-171">--restWithNoViews or -api</span></span>  | <span data-ttu-id="c996d-172">Создание контроллера с API в стиле REST.</span><span class="sxs-lookup"><span data-stu-id="c996d-172">Generate a Controller with REST style API.</span></span> <span data-ttu-id="c996d-173">Используется `noViews`, а все параметры, связанные с представлением, игнорируются.</span><span class="sxs-lookup"><span data-stu-id="c996d-173">`noViews` is assumed and any view related options are ignored.</span></span> |
| <span data-ttu-id="c996d-174">--readWriteActions или -actions</span><span class="sxs-lookup"><span data-stu-id="c996d-174">--readWriteActions or -actions</span></span> | <span data-ttu-id="c996d-175">Создание контроллера с действиями чтения и записи без модели.</span><span class="sxs-lookup"><span data-stu-id="c996d-175">Generate controller with read/write actions without a model.</span></span> |

<span data-ttu-id="c996d-176">Чтобы получить справку по команде `-h`, используйте параметр `aspnet-codegenerator controller`.</span><span class="sxs-lookup"><span data-stu-id="c996d-176">Use the `-h` switch for help on the `aspnet-codegenerator controller` command:</span></span>

```dotnetcli
dotnet aspnet-codegenerator controller -h
```

<span data-ttu-id="c996d-177">Пример [ см. в разделе ](/aspnet/core/tutorials/razor-pages/model)Создание модели фильма`dotnet aspnet-codegenerator controller`.</span><span class="sxs-lookup"><span data-stu-id="c996d-177">See [Scaffold the movie model](/aspnet/core/tutorials/razor-pages/model) for an example of `dotnet aspnet-codegenerator controller`.</span></span>

### <a name="razorpage"></a><span data-ttu-id="c996d-178">Razorpage</span><span class="sxs-lookup"><span data-stu-id="c996d-178">Razorpage</span></span>

<a name="rp"></a>

<span data-ttu-id="c996d-179">Шаблоны для страниц Razor Pages можно формировать по отдельности. Для этого нужно указать имя новой страницы и используемый шаблон.</span><span class="sxs-lookup"><span data-stu-id="c996d-179">Razor Pages can be individually scaffolded by specifying the name of the new page and the template to use.</span></span> <span data-ttu-id="c996d-180">Поддерживаются такие шаблоны:</span><span class="sxs-lookup"><span data-stu-id="c996d-180">The supported templates are:</span></span>

* `Empty`
* `Create`
* `Edit`
* `Delete`
* `Details`
* `List`

<span data-ttu-id="c996d-181">Например, в следующей команде используется шаблон Edit для создания *MyEdit.cshtml* и *MyEdit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="c996d-181">For example, the following command uses the Edit template to generate *MyEdit.cshtml* and *MyEdit.cshtml.cs*:</span></span>

```dotnetcli
dotnet aspnet-codegenerator razorpage MyEdit Edit -m Movie -dc RazorPagesMovieContext -outDir Pages/Movies
```

<span data-ttu-id="c996d-182">Создаются следующие шаблоны, а шаблон и имя созданного файла обычно не указываются:</span><span class="sxs-lookup"><span data-stu-id="c996d-182">Typically, the template and generated file name is not specified, and the following templates are created:</span></span>

* `Create`
* `Edit`
* `Delete`
* `Details`
* `List`

<span data-ttu-id="c996d-183">В таблице ниже перечислены параметры для `aspnet-codegenerator` `razorpage` и `controller`.</span><span class="sxs-lookup"><span data-stu-id="c996d-183">The following table lists options for  `aspnet-codegenerator` `razorpage` and `controller`:</span></span>

[!INCLUDE [aspnet-codegenerator-args-md.md](~/includes/aspnet-codegenerator-args-md.md)]

<span data-ttu-id="c996d-184">В таблице ниже перечислены параметры только для `aspnet-codegenerator razorpage`.</span><span class="sxs-lookup"><span data-stu-id="c996d-184">The following table lists options unique to  `aspnet-codegenerator razorpage`:</span></span>

| <span data-ttu-id="c996d-185">Параметр</span><span class="sxs-lookup"><span data-stu-id="c996d-185">Option</span></span>               | <span data-ttu-id="c996d-186">Description</span><span class="sxs-lookup"><span data-stu-id="c996d-186">Description</span></span>|
| ----------------- | ------------ |
|   <span data-ttu-id="c996d-187">--namespaceName или -namespace</span><span class="sxs-lookup"><span data-stu-id="c996d-187">--namespaceName or -namespace</span></span> | <span data-ttu-id="c996d-188">Имя пространства имен, используемого для созданной модели PageModel.</span><span class="sxs-lookup"><span data-stu-id="c996d-188">The name of the namespace to use for the generated PageModel</span></span> |
| <span data-ttu-id="c996d-189">--partialView или -partial</span><span class="sxs-lookup"><span data-stu-id="c996d-189">--partialView or -partial</span></span> | <span data-ttu-id="c996d-190">Создание частичного представления.</span><span class="sxs-lookup"><span data-stu-id="c996d-190">Generate a partial view.</span></span> <span data-ttu-id="c996d-191">Если указан этот параметр, параметры макета -l и -udl игнорируются.</span><span class="sxs-lookup"><span data-stu-id="c996d-191">Layout options -l and -udl are ignored if this is specified.</span></span> |
| <span data-ttu-id="c996d-192">--noPageModel или -npm</span><span class="sxs-lookup"><span data-stu-id="c996d-192">--noPageModel or -npm</span></span> | <span data-ttu-id="c996d-193">Параметр, при использовании которого не создается класс PageModel для пустого шаблона.</span><span class="sxs-lookup"><span data-stu-id="c996d-193">Switch to not generate a PageModel class for Empty template</span></span> |

<span data-ttu-id="c996d-194">Чтобы получить справку по команде `-h`, используйте параметр `aspnet-codegenerator razorpage`.</span><span class="sxs-lookup"><span data-stu-id="c996d-194">Use the `-h` switch for help on the `aspnet-codegenerator razorpage` command:</span></span>

```dotnetcli
dotnet aspnet-codegenerator razorpage -h
```

<span data-ttu-id="c996d-195">Пример [ см. в разделе ](/aspnet/core/tutorials/razor-pages/model)Создание модели фильма`dotnet aspnet-codegenerator razorpage`.</span><span class="sxs-lookup"><span data-stu-id="c996d-195">See [Scaffold the movie model](/aspnet/core/tutorials/razor-pages/model) for an example of `dotnet aspnet-codegenerator razorpage`.</span></span>

### <a name="identity"></a><span data-ttu-id="c996d-196">Удостоверение</span><span class="sxs-lookup"><span data-stu-id="c996d-196">Identity</span></span>

<span data-ttu-id="c996d-197">См. статью [Удостоверение шаблона](/aspnet/core/security/authentication/scaffold-identity).</span><span class="sxs-lookup"><span data-stu-id="c996d-197">See [Scaffold Identity](/aspnet/core/security/authentication/scaffold-identity)</span></span>
