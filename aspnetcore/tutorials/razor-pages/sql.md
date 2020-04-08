---
title: Работа с базой данных и ASP.NET Core
author: rick-anderson
description: В этой статье описывается работа с базой данных и ASP.NET Core.
ms.author: riande
ms.date: 7/22/2019
uid: tutorials/razor-pages/sql
ms.openlocfilehash: b5acb573f8fa39e5300ecdb359113d8697d78934
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2020
ms.locfileid: "78649516"
---
# <a name="work-with-a-database-and-aspnet-core"></a><span data-ttu-id="f8547-103">Работа с базой данных и ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f8547-103">Work with a database and ASP.NET Core</span></span>

<span data-ttu-id="f8547-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Джо Одетт](https://twitter.com/joeaudette) (Joe Audette)</span><span class="sxs-lookup"><span data-stu-id="f8547-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="f8547-105">Объект `RazorPagesMovieContext` обрабатывает задачу подключения к базе данных и сопоставления объектов `Movie` с записями базы данных.</span><span class="sxs-lookup"><span data-stu-id="f8547-105">The `RazorPagesMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="f8547-106">Контекст базы данных регистрируется с помощью контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection) в методе `ConfigureServices` в файле *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="f8547-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in *Startup.cs*:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="f8547-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8547-107">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="f8547-108">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="f8547-108">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

---

<span data-ttu-id="f8547-109">Система [конфигурации](xref:fundamentals/configuration/index) ASP.NET Core считывает `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="f8547-109">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="f8547-110">Для разработки на локальном уровне она получает строку подключения из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="f8547-110">For local development, it gets the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="f8547-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8547-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f8547-112">Значение имени для базы данных (`Database={Database name}`) будет отличаться для созданного кода.</span><span class="sxs-lookup"><span data-stu-id="f8547-112">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="f8547-113">Значение имени является произвольным.</span><span class="sxs-lookup"><span data-stu-id="f8547-113">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie30/appsettings.json?highlight=10-12)]

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="f8547-114">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="f8547-114">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

---

<span data-ttu-id="f8547-115">Если приложение развертывается на тестовом или рабочем сервере, можно задать строку подключения к реальному серверу базы данных с помощью переменной среды.</span><span class="sxs-lookup"><span data-stu-id="f8547-115">When the app is deployed to a test or production server, an environment variable can be used to set the connection string to a real database server.</span></span> <span data-ttu-id="f8547-116">Дополнительные сведения см. в статье [Конфигурация](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="f8547-116">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="f8547-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8547-117">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="f8547-118">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="f8547-118">SQL Server Express LocalDB</span></span>

<span data-ttu-id="f8547-119">LocalDB — это упрощенная версия ядра СУБД SQL Server Express, предназначенная для разработки программ.</span><span class="sxs-lookup"><span data-stu-id="f8547-119">LocalDB is a lightweight version of the SQL Server Express database engine that's targeted for program development.</span></span> <span data-ttu-id="f8547-120">LocalDB запускается по запросу в пользовательском режиме, поэтому настройки не слишком сложны.</span><span class="sxs-lookup"><span data-stu-id="f8547-120">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="f8547-121">По умолчанию база данных LocalDB создает файлы `*.mdf` в каталоге `C:\Users\<user>\`.</span><span class="sxs-lookup"><span data-stu-id="f8547-121">By default, LocalDB database creates `*.mdf` files in the `C:\Users\<user>\` directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="f8547-122">В меню **Вид** откройте **обозреватель объектов SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="f8547-122">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Меню "Вид"](sql/_static/ssox.png)

* <span data-ttu-id="f8547-124">Щелкните правой кнопкой мыши таблицу `Movie` и выберите пункт **Конструктор представлений**.</span><span class="sxs-lookup"><span data-stu-id="f8547-124">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Контекстные меню, открытые на таблице Movie](sql/_static/design.png)

  ![Таблицы Movie, открытые в конструкторе](sql/_static/dv.png)

<span data-ttu-id="f8547-127">Обратите внимание на значок с изображением ключа рядом с `ID`.</span><span class="sxs-lookup"><span data-stu-id="f8547-127">Note the key icon next to `ID`.</span></span> <span data-ttu-id="f8547-128">По умолчанию EF создает свойство с именем `ID` для первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="f8547-128">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="f8547-129">Щелкните правой кнопкой мыши таблицу `Movie` и выберите пункт **Просмотреть данные**.</span><span class="sxs-lookup"><span data-stu-id="f8547-129">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Открытая таблица Movie с отображением данных](sql/_static/vd22.png)

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="f8547-131">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="f8547-131">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---

## <a name="seed-the-database"></a><span data-ttu-id="f8547-132">Заполнение базы данных</span><span class="sxs-lookup"><span data-stu-id="f8547-132">Seed the database</span></span>

<span data-ttu-id="f8547-133">Создайте класс `SeedData` в папке *Models* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="f8547-133">Create a new class named `SeedData` in the *Models* folder with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="f8547-134">Если в базе данных есть фильмы, возвращается инициализатор заполнения и фильмы не добавляются.</span><span class="sxs-lookup"><span data-stu-id="f8547-134">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="f8547-135">Добавление инициализатора заполнения</span><span class="sxs-lookup"><span data-stu-id="f8547-135">Add the seed initializer</span></span>

<span data-ttu-id="f8547-136">В файле *Program.cs* измените метод `Main`, чтобы реализовать следующее:</span><span class="sxs-lookup"><span data-stu-id="f8547-136">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="f8547-137">Получение экземпляра контекста базы данных из контейнера внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="f8547-137">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="f8547-138">Вызов метода инициализации с передачей ему контекста.</span><span class="sxs-lookup"><span data-stu-id="f8547-138">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="f8547-139">Высвобождение контекста после завершения работы метода заполнения.</span><span class="sxs-lookup"><span data-stu-id="f8547-139">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="f8547-140">В следующем примере кода показан обновленный файл *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="f8547-140">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Program.cs)]

<span data-ttu-id="f8547-141">Если `Update-Database` не выполнялось, возникает следующее исключение:</span><span class="sxs-lookup"><span data-stu-id="f8547-141">The following exception occurs when `Update-Database` has not been run:</span></span>

> `SqlException: Cannot open database "RazorPagesMovieContext-" requested by the login. The login failed.`
> `Login failed for user 'user name'.`

### <a name="test-the-app"></a><span data-ttu-id="f8547-142">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="f8547-142">Test the app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="f8547-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8547-143">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f8547-144">Удалите все записи из базы данных.</span><span class="sxs-lookup"><span data-stu-id="f8547-144">Delete all the records in the DB.</span></span> <span data-ttu-id="f8547-145">Это можно сделать с помощью ссылок удаления в браузере или из [SSOX](xref:tutorials/razor-pages/new-field#ssox).</span><span class="sxs-lookup"><span data-stu-id="f8547-145">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="f8547-146">Необходимо выполнить инициализацию (вызывать методы в классе `Startup`), чтобы запустить метод заполнения.</span><span class="sxs-lookup"><span data-stu-id="f8547-146">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="f8547-147">Для этого следует остановить и перезапустить IIS Express.</span><span class="sxs-lookup"><span data-stu-id="f8547-147">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="f8547-148">Воспользуйтесь одним из перечисленных ниже подходов.</span><span class="sxs-lookup"><span data-stu-id="f8547-148">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="f8547-149">Щелкните правой кнопкой мыши значок IIS Express в области уведомлений и выберите **Выход** или **Остановить сайт**.</span><span class="sxs-lookup"><span data-stu-id="f8547-149">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Значок IIS Express в области уведомлений](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Контекстное меню](sql/_static/stopIIS.png)

    * <span data-ttu-id="f8547-152">Если среда Visual Studio была запущена в режиме без отладки, нажмите клавишу F5 для запуска в режиме отладки.</span><span class="sxs-lookup"><span data-stu-id="f8547-152">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="f8547-153">Если среда Visual Studio была запущена в режиме отладки, остановите отладчик и нажмите клавишу F5.</span><span class="sxs-lookup"><span data-stu-id="f8547-153">If you were running VS in debug mode, stop the debugger and press F5.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="f8547-154">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="f8547-154">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="f8547-155">Удалите все записи в базе данных для запуска метода заполнения.</span><span class="sxs-lookup"><span data-stu-id="f8547-155">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="f8547-156">Остановите и запустите приложение, чтобы начать заполнение базы данных.</span><span class="sxs-lookup"><span data-stu-id="f8547-156">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="f8547-157">В приложении будут отображены данные.</span><span class="sxs-lookup"><span data-stu-id="f8547-157">The app shows the seeded data.</span></span>

---

<span data-ttu-id="f8547-158">В следующем руководстве будет улучшено представление данных.</span><span class="sxs-lookup"><span data-stu-id="f8547-158">The next tutorial will improve the presentation of the data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f8547-159">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="f8547-159">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f8547-160">[Предыдущая тема. Шаблонные страницы Razor](xref:tutorials/razor-pages/page)
> [Следующая тема. Обновление страниц](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="f8547-160">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="f8547-161">Объект `RazorPagesMovieContext` обрабатывает задачу подключения к базе данных и сопоставления объектов `Movie` с записями базы данных.</span><span class="sxs-lookup"><span data-stu-id="f8547-161">The `RazorPagesMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="f8547-162">Контекст базы данных регистрируется с помощью контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection) в методе `ConfigureServices` в файле *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="f8547-162">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in *Startup.cs*:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="f8547-163">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8547-163">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="f8547-164">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="f8547-164">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

---

<span data-ttu-id="f8547-165">Дополнительные сведения о методах, которые используются в `ConfigureServices`, см.:</span><span class="sxs-lookup"><span data-stu-id="f8547-165">For more information on the methods used in `ConfigureServices`, see:</span></span>

* <span data-ttu-id="f8547-166">[Общий регламент по защите данных (GDPR), принятый в ЕС, в ASP.NET Core](xref:security/gdpr) для `CookiePolicyOptions`.</span><span class="sxs-lookup"><span data-stu-id="f8547-166">[EU General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr) for `CookiePolicyOptions`.</span></span>
* [<span data-ttu-id="f8547-167">SetCompatibilityVersion</span><span class="sxs-lookup"><span data-stu-id="f8547-167">SetCompatibilityVersion</span></span>](xref:mvc/compatibility-version)

<span data-ttu-id="f8547-168">Система [конфигурации](xref:fundamentals/configuration/index) ASP.NET Core считывает `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="f8547-168">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="f8547-169">Для разработки на локальном уровне она получает строку подключения из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="f8547-169">For local development, it gets the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="f8547-170">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8547-170">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f8547-171">Значение имени для базы данных (`Database={Database name}`) будет отличаться для созданного кода.</span><span class="sxs-lookup"><span data-stu-id="f8547-171">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="f8547-172">Значение имени является произвольным.</span><span class="sxs-lookup"><span data-stu-id="f8547-172">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie22/appsettings.json)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="f8547-173">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f8547-173">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="f8547-174">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="f8547-174">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

---

<span data-ttu-id="f8547-175">Если приложение развертывается на тестовом или рабочем сервере, можно задать строку подключения к реальному серверу базы данных с помощью переменной среды.</span><span class="sxs-lookup"><span data-stu-id="f8547-175">When the app is deployed to a test or production server, an environment variable can be used to set the connection string to a real database server.</span></span> <span data-ttu-id="f8547-176">Дополнительные сведения см. в статье [Конфигурация](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="f8547-176">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="f8547-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8547-177">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="f8547-178">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="f8547-178">SQL Server Express LocalDB</span></span>

<span data-ttu-id="f8547-179">LocalDB — это упрощенная версия ядра СУБД SQL Server Express, предназначенная для разработки программ.</span><span class="sxs-lookup"><span data-stu-id="f8547-179">LocalDB is a lightweight version of the SQL Server Express database engine that's targeted for program development.</span></span> <span data-ttu-id="f8547-180">LocalDB запускается по запросу в пользовательском режиме, поэтому настройки не слишком сложны.</span><span class="sxs-lookup"><span data-stu-id="f8547-180">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="f8547-181">По умолчанию база данных LocalDB создает файлы `*.mdf` в каталоге `C:/Users/<user/>`.</span><span class="sxs-lookup"><span data-stu-id="f8547-181">By default, LocalDB database creates `*.mdf` files in the `C:/Users/<user/>` directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="f8547-182">В меню **Вид** откройте **обозреватель объектов SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="f8547-182">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Меню "Вид"](sql/_static/ssox.png)

* <span data-ttu-id="f8547-184">Щелкните правой кнопкой мыши таблицу `Movie` и выберите пункт **Конструктор представлений**.</span><span class="sxs-lookup"><span data-stu-id="f8547-184">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Контекстное меню, открытое на таблице Movie](sql/_static/design.png)

  ![Таблица Movie, открытая в конструкторе](sql/_static/dv.png)

<span data-ttu-id="f8547-187">Обратите внимание на значок с изображением ключа рядом с `ID`.</span><span class="sxs-lookup"><span data-stu-id="f8547-187">Note the key icon next to `ID`.</span></span> <span data-ttu-id="f8547-188">По умолчанию EF создает свойство с именем `ID` для первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="f8547-188">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="f8547-189">Щелкните правой кнопкой мыши таблицу `Movie` и выберите пункт **Просмотреть данные**.</span><span class="sxs-lookup"><span data-stu-id="f8547-189">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Открытая таблица Movie с отображением данных](sql/_static/vd22.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="f8547-191">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f8547-191">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="f8547-192">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="f8547-192">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---

## <a name="seed-the-database"></a><span data-ttu-id="f8547-193">Заполнение базы данных</span><span class="sxs-lookup"><span data-stu-id="f8547-193">Seed the database</span></span>

<span data-ttu-id="f8547-194">Создайте класс `SeedData` в папке *Models* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="f8547-194">Create a new class named `SeedData` in the *Models* folder with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="f8547-195">Если в базе данных есть фильмы, возвращается инициализатор заполнения и фильмы не добавляются.</span><span class="sxs-lookup"><span data-stu-id="f8547-195">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="f8547-196">Добавление инициализатора заполнения</span><span class="sxs-lookup"><span data-stu-id="f8547-196">Add the seed initializer</span></span>

<span data-ttu-id="f8547-197">В файле *Program.cs* измените метод `Main`, чтобы реализовать следующее:</span><span class="sxs-lookup"><span data-stu-id="f8547-197">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="f8547-198">Получение экземпляра контекста базы данных из контейнера внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="f8547-198">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="f8547-199">Вызов метода инициализации с передачей ему контекста.</span><span class="sxs-lookup"><span data-stu-id="f8547-199">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="f8547-200">Высвобождение контекста после завершения работы метода заполнения.</span><span class="sxs-lookup"><span data-stu-id="f8547-200">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="f8547-201">В следующем примере кода показан обновленный файл *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="f8547-201">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Program.cs)]

<span data-ttu-id="f8547-202">Рабочее приложение не вызывает `Database.Migrate`.</span><span class="sxs-lookup"><span data-stu-id="f8547-202">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="f8547-203">Он добавляется в предыдущем коде, чтобы предотвратить следующее исключение, если `Update-Database` не был запущен.</span><span class="sxs-lookup"><span data-stu-id="f8547-203">It's added to the preceding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="f8547-204">SqlException: не удается открыть базу данных RazorPagesMovieContext-21, запрошенную при входе.</span><span class="sxs-lookup"><span data-stu-id="f8547-204">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="f8547-205">Не удалось выполнить вход.</span><span class="sxs-lookup"><span data-stu-id="f8547-205">The login failed.</span></span>
<span data-ttu-id="f8547-206">Сбой при входе в систему пользователя user name.</span><span class="sxs-lookup"><span data-stu-id="f8547-206">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="f8547-207">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="f8547-207">Test the app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="f8547-208">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8547-208">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f8547-209">Удалите все записи из базы данных.</span><span class="sxs-lookup"><span data-stu-id="f8547-209">Delete all the records in the DB.</span></span> <span data-ttu-id="f8547-210">Это можно сделать с помощью ссылок удаления в браузере или из [SSOX](xref:tutorials/razor-pages/new-field#ssox).</span><span class="sxs-lookup"><span data-stu-id="f8547-210">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="f8547-211">Необходимо выполнить инициализацию (вызывать методы в классе `Startup`), чтобы запустить метод заполнения.</span><span class="sxs-lookup"><span data-stu-id="f8547-211">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="f8547-212">Для этого следует остановить и перезапустить IIS Express.</span><span class="sxs-lookup"><span data-stu-id="f8547-212">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="f8547-213">Воспользуйтесь одним из перечисленных ниже подходов.</span><span class="sxs-lookup"><span data-stu-id="f8547-213">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="f8547-214">Щелкните правой кнопкой мыши значок IIS Express в области уведомлений и выберите **Выйти** или **Остановить сайт**.</span><span class="sxs-lookup"><span data-stu-id="f8547-214">Right-click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Значок IIS Express в области уведомлений](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Контекстное меню](sql/_static/stopIIS.png)

    * <span data-ttu-id="f8547-217">Если среда Visual Studio была запущена в режиме без отладки, нажмите клавишу F5 для запуска в режиме отладки.</span><span class="sxs-lookup"><span data-stu-id="f8547-217">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="f8547-218">Если среда Visual Studio была запущена в режиме отладки, остановите отладчик и нажмите клавишу F5.</span><span class="sxs-lookup"><span data-stu-id="f8547-218">If you were running VS in debug mode, stop the debugger and press F5.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="f8547-219">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f8547-219">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="f8547-220">Удалите все записи в базе данных для запуска метода заполнения.</span><span class="sxs-lookup"><span data-stu-id="f8547-220">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="f8547-221">Остановите и запустите приложение, чтобы начать заполнение базы данных.</span><span class="sxs-lookup"><span data-stu-id="f8547-221">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="f8547-222">В приложении будут отображены данные.</span><span class="sxs-lookup"><span data-stu-id="f8547-222">The app shows the seeded data.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="f8547-223">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="f8547-223">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="f8547-224">Удалите все записи в базе данных для запуска метода заполнения.</span><span class="sxs-lookup"><span data-stu-id="f8547-224">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="f8547-225">Остановите и запустите приложение, чтобы начать заполнение базы данных.</span><span class="sxs-lookup"><span data-stu-id="f8547-225">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="f8547-226">В приложении будут отображены данные.</span><span class="sxs-lookup"><span data-stu-id="f8547-226">The app shows the seeded data.</span></span>

---

<span data-ttu-id="f8547-227">В приложении отображаются заполненные данные.</span><span class="sxs-lookup"><span data-stu-id="f8547-227">The app shows the seeded data:</span></span>

![Приложение Movie с данными по фильмам, открытое в Chrome](sql/_static/m55.png)

<span data-ttu-id="f8547-229">В следующем учебнике будет улучшено представление данных.</span><span class="sxs-lookup"><span data-stu-id="f8547-229">The next tutorial will clean up the presentation of the data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f8547-230">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="f8547-230">Additional resources</span></span>

* [<span data-ttu-id="f8547-231">Версия руководства на YouTube</span><span class="sxs-lookup"><span data-stu-id="f8547-231">YouTube version of this tutorial</span></span>](https://youtu.be/A_5ff11sDHY)

> [!div class="step-by-step"]
> <span data-ttu-id="f8547-232">[Предыдущая тема. Шаблонные страницы Razor](xref:tutorials/razor-pages/page)
> [Следующая тема. Обновление страниц](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="f8547-232">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>

::: moniker-end
