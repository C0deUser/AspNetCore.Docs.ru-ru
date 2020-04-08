---
page_type: sample
description: В этом учебнике объясняется, как создать службу и клиента gRPC в ASP.NET Core.
languages:
- csharp
products:
- dotnet-core
- aspnet-core
- vs
urlFragment: create-grpc-client
ms.openlocfilehash: b9feb9eed62177358fffc0d7da582f625a431e32
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2020
ms.locfileid: "78648160"
---
# <a name="create-a-grpc-client-and-server-in-aspnet-core-30-using-visual-studio"></a><span data-ttu-id="123e3-102">Создание клиента и сервера gRPC в ASP.NET Core 3.0 с помощью Visual Studio</span><span class="sxs-lookup"><span data-stu-id="123e3-102">Create a gRPC client and server in ASP.NET Core 3.0 using Visual Studio</span></span>

<span data-ttu-id="123e3-103">В этом руководстве показано, как создать клиент [gRPC](https://grpc.io/docs/guides/) в .NET Core и сервер gRPC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="123e3-103">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="123e3-104">В итоге вы получите клиент gRPC, который взаимодействует со службой Greeter gRPC.</span><span class="sxs-lookup"><span data-stu-id="123e3-104">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="123e3-105">В этом учебнике рассмотрены следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="123e3-105">In this tutorial, you;</span></span>

* <span data-ttu-id="123e3-106">Создание сервера gRPC.</span><span class="sxs-lookup"><span data-stu-id="123e3-106">Create a gRPC Server.</span></span>
* <span data-ttu-id="123e3-107">Создание клиента gRPC.</span><span class="sxs-lookup"><span data-stu-id="123e3-107">Create a gRPC client.</span></span>
* <span data-ttu-id="123e3-108">Тестирование службы клиента gRPC с помощью службы Greeter gRPC.</span><span class="sxs-lookup"><span data-stu-id="123e3-108">Test the gRPC client service with the gRPC Greeter service.</span></span>

## <a name="create-a-grpc-service"></a><span data-ttu-id="123e3-109">Создание службы gRPC</span><span class="sxs-lookup"><span data-stu-id="123e3-109">Create a gRPC service</span></span>

* <span data-ttu-id="123e3-110">В меню **Файл** Visual Studio откройте **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="123e3-110">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="123e3-111">В диалоговом окне **Создать проект** выберите **Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="123e3-111">In the **Create a new project** dialog, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="123e3-112">Выберите **Далее**</span><span class="sxs-lookup"><span data-stu-id="123e3-112">Select **Next**</span></span>
* <span data-ttu-id="123e3-113">Присвойте проекту имя **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="123e3-113">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="123e3-114">Для проекта необходимо установить имя *GrpcGreeter*, чтобы при копировании и вставке кода совпадали пространства имен.</span><span class="sxs-lookup"><span data-stu-id="123e3-114">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="123e3-115">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="123e3-115">Select **Create**</span></span>
* <span data-ttu-id="123e3-116">в диалоговом окне **Создание веб-приложения ASP.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="123e3-116">In the **Create a new ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="123e3-117">В раскрывающемся меню выберите **.NET Core** и **ASP.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="123e3-117">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown menus.</span></span> 
  * <span data-ttu-id="123e3-118">Выберите шаблон **gRPC Service**.</span><span class="sxs-lookup"><span data-stu-id="123e3-118">Select the **gRPC Service** template.</span></span>
  * <span data-ttu-id="123e3-119">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="123e3-119">Select **Create**</span></span>

### <a name="run-the-service"></a><span data-ttu-id="123e3-120">Запуск службы</span><span class="sxs-lookup"><span data-stu-id="123e3-120">Run the service</span></span>

* <span data-ttu-id="123e3-121">Нажмите `Ctrl+F5`, чтобы запустить службу gRPC без отладчика.</span><span class="sxs-lookup"><span data-stu-id="123e3-121">Press `Ctrl+F5` to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="123e3-122">Служба запустится в командной строке Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="123e3-122">Visual Studio runs the service in a command prompt.</span></span>

<span data-ttu-id="123e3-123">В журналах отобразится служба, которая ожидает передачи данных через `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="123e3-123">The logs show the service listening on `https://localhost:5001`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

> [!NOTE]
> <span data-ttu-id="123e3-124">Шаблон gRPC настроен для использования [протокола TLS](https://tools.ietf.org/html/rfc5246).</span><span class="sxs-lookup"><span data-stu-id="123e3-124">The gRPC template is configured to use [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span> <span data-ttu-id="123e3-125">Клиенты gRPC должны использовать протокол HTTPS для обращения к серверу.</span><span class="sxs-lookup"><span data-stu-id="123e3-125">gRPC clients need to use HTTPS to call the server.</span></span>
>
> <span data-ttu-id="123e3-126">macOS не поддерживает ASP.NET Core gRPC с TLS.</span><span class="sxs-lookup"><span data-stu-id="123e3-126">macOS doesn't support ASP.NET Core gRPC with TLS.</span></span> <span data-ttu-id="123e3-127">Для успешного запуска служб gRPC в macOS требуется дополнительная настройка.</span><span class="sxs-lookup"><span data-stu-id="123e3-127">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="123e3-128">Дополнительные сведения см. в статье [gRPC и ASP.NET Core в macOS](xref:grpc/aspnetcore#grpc-and-aspnet-core-on-macos).</span><span class="sxs-lookup"><span data-stu-id="123e3-128">For more information, see [gRPC and ASP.NET Core on macOS](xref:grpc/aspnetcore#grpc-and-aspnet-core-on-macos).</span></span>

### <a name="examine-the-project-files"></a><span data-ttu-id="123e3-129">Анализ файлов проекта</span><span class="sxs-lookup"><span data-stu-id="123e3-129">Examine the project files</span></span>

<span data-ttu-id="123e3-130">Файлы проекта *GrpcGreeter*:</span><span class="sxs-lookup"><span data-stu-id="123e3-130">*GrpcGreeter* project files:</span></span>

* <span data-ttu-id="123e3-131">*greet.proto*: Файл *Protos/greet.proto* определяет службу gRPC `Greeter` и используется для создания ресурсов сервера gRPC.</span><span class="sxs-lookup"><span data-stu-id="123e3-131">*greet.proto*: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="123e3-132">Дополнительные сведения см. в разделе [Введение в gRPC](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="123e3-132">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="123e3-133">Папка *Services* содержит реализацию службы `Greeter`.</span><span class="sxs-lookup"><span data-stu-id="123e3-133">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="123e3-134">*appSettings.json*: содержит данные конфигурации, такие как протокол, используемый в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="123e3-134">*appSettings.json*: Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="123e3-135">Для получения дополнительной информации см. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="123e3-135">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="123e3-136">*Program.cs*: содержит точку входа для службы gRPC.</span><span class="sxs-lookup"><span data-stu-id="123e3-136">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="123e3-137">Для получения дополнительной информации см. <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="123e3-137">For more information, see <xref:fundamentals/host/generic-host>.</span></span>
* <span data-ttu-id="123e3-138">*Startup.cs*: содержит код, задающий поведение приложения.</span><span class="sxs-lookup"><span data-stu-id="123e3-138">*Startup.cs*: Contains code that configures app behavior.</span></span> <span data-ttu-id="123e3-139">Дополнительные сведения: [Запуск приложения](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="123e3-139">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="123e3-140">Создание клиента gRPC в консольном приложении .NET</span><span class="sxs-lookup"><span data-stu-id="123e3-140">Create the gRPC client in a .NET console app</span></span>

* <span data-ttu-id="123e3-141">Откройте еще один экземпляр Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="123e3-141">Open a second instance of Visual Studio.</span></span>
* <span data-ttu-id="123e3-142">Выберите **Файл** > **Создать** > **Проект** в меню.</span><span class="sxs-lookup"><span data-stu-id="123e3-142">Select **File** > **New** > **Project** from the menu bar.</span></span>
* <span data-ttu-id="123e3-143">В диалоговом окне **Создать проект** выберите **Консольное приложение (.NET Core)** .</span><span class="sxs-lookup"><span data-stu-id="123e3-143">In the **Create a new project** dialog, select **Console App (.NET Core)**.</span></span>
* <span data-ttu-id="123e3-144">Выберите **Далее**</span><span class="sxs-lookup"><span data-stu-id="123e3-144">Select **Next**</span></span>
* <span data-ttu-id="123e3-145">В текстовом поле **Имя** введите "GrpcGreeterClient".</span><span class="sxs-lookup"><span data-stu-id="123e3-145">In the **Name** text box, enter "GrpcGreeterClient".</span></span>
* <span data-ttu-id="123e3-146">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="123e3-146">Select **Create**.</span></span>

### <a name="add-required-packages"></a><span data-ttu-id="123e3-147">Добавление необходимых пакетов</span><span class="sxs-lookup"><span data-stu-id="123e3-147">Add required packages</span></span>

<span data-ttu-id="123e3-148">Для клиентского проекта gRPC требуются следующие пакеты:</span><span class="sxs-lookup"><span data-stu-id="123e3-148">The gRPC client project requires the following packages:</span></span>

* <span data-ttu-id="123e3-149">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), который содержит клиент .NET Core.</span><span class="sxs-lookup"><span data-stu-id="123e3-149">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), which contains the .NET Core client.</span></span>
* <span data-ttu-id="123e3-150">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), который содержит API сообщений protobuf для C#;</span><span class="sxs-lookup"><span data-stu-id="123e3-150">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="123e3-151">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), который содержит поддержку инструментов C# для файлов protobuf.</span><span class="sxs-lookup"><span data-stu-id="123e3-151">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="123e3-152">Пакет инструментов не требуется во время выполнения, поэтому зависимость помечается `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="123e3-152">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

<span data-ttu-id="123e3-153">Установка пакетов с помощью консоли диспетчера пакетов (PMC) или управления пакетами NuGet.</span><span class="sxs-lookup"><span data-stu-id="123e3-153">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Packages.</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="123e3-154">Установка пакетов с помощью консоли диспетчера пакетов</span><span class="sxs-lookup"><span data-stu-id="123e3-154">PMC option to install packages</span></span>

* <span data-ttu-id="123e3-155">В Visual Studio выберите пункты меню **Сервис** > **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="123e3-155">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="123e3-156">В **консоли диспетчера пакетов** перейдите в каталог, в котором содержится файл *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="123e3-156">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="123e3-157">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="123e3-157">Run the following commands:</span></span>

 ```powershell
Install-Package Grpc.Net.Client
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="123e3-158">Установка пакетов с помощью раздела управления пакетами NuGet</span><span class="sxs-lookup"><span data-stu-id="123e3-158">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="123e3-159">Щелкните правой кнопкой мыши проект в **обозревателе решений** > **Управление пакетами NuGet**</span><span class="sxs-lookup"><span data-stu-id="123e3-159">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="123e3-160">Выберите вкладку **Обзор**.</span><span class="sxs-lookup"><span data-stu-id="123e3-160">Select the **Browse** tab.</span></span>
* <span data-ttu-id="123e3-161">В поле поиска введите **Grpc.Net.Client**.</span><span class="sxs-lookup"><span data-stu-id="123e3-161">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="123e3-162">Выберите пакет **Grpc.Net.Client** на вкладке **Обзор** и нажмите кнопку **Установить**.</span><span class="sxs-lookup"><span data-stu-id="123e3-162">Select the **Grpc.Net.Client** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="123e3-163">Повторите все шаги для `Google.Protobuf` и `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="123e3-163">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

### <a name="add-greetproto"></a><span data-ttu-id="123e3-164">Добавление greet.proto</span><span class="sxs-lookup"><span data-stu-id="123e3-164">Add greet.proto</span></span>

* <span data-ttu-id="123e3-165">Создайте папку **Protos** в клиентском проекте gRPC.</span><span class="sxs-lookup"><span data-stu-id="123e3-165">Create a **Protos** folder in the gRPC client project.</span></span>
* <span data-ttu-id="123e3-166">Скопируйте файл **Protos\greet.proto** из службы Greeter gRPC в проект клиента gRPC.</span><span class="sxs-lookup"><span data-stu-id="123e3-166">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="123e3-167">Измените файл проекта *GrpcGreeterClient.csproj*:</span><span class="sxs-lookup"><span data-stu-id="123e3-167">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  <span data-ttu-id="123e3-168">Щелкните проект правой кнопкой мыши и выберите **Изменить файл проекта**.</span><span class="sxs-lookup"><span data-stu-id="123e3-168">Right-click the project and select **Edit Project File**.</span></span>

* <span data-ttu-id="123e3-169">Добавьте группу элементов с элементом `<Protobuf>`, ссылающимся на файл **greet.proto**:</span><span class="sxs-lookup"><span data-stu-id="123e3-169">Add an item group with a `<Protobuf>` element that refers to the **greet.proto** file:</span></span>

  ```xml
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a><span data-ttu-id="123e3-170">Создание клиента Greeter</span><span class="sxs-lookup"><span data-stu-id="123e3-170">Create the Greeter client</span></span>

<span data-ttu-id="123e3-171">Скомпилируйте проект, чтобы создать типы в пространстве имен `GrpcGreeter`.</span><span class="sxs-lookup"><span data-stu-id="123e3-171">Build the project to create the types in the `GrpcGreeter` namespace.</span></span> <span data-ttu-id="123e3-172">Типы `GrpcGreeter` создаются автоматически в процессе сборки.</span><span class="sxs-lookup"><span data-stu-id="123e3-172">The `GrpcGreeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="123e3-173">Добавьте в файл *Program.cs* клиента gRPC следующий код:</span><span class="sxs-lookup"><span data-stu-id="123e3-173">Update the gRPC client *Program.cs* file with the following code:</span></span>

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;
using GrpcGreeter;
using Grpc.Net.Client;

namespace GrpcGreeterClient
{
    class Program
    {
        static async Task Main(string[] args)
        {
            // The port number(5001) must match the port of the gRPC server.
            var channel = GrpcChannel.ForAddress("https://localhost:5001");
            var client = new Greeter.GreeterClient(channel);
            var reply = await client.SayHelloAsync(
                              new HelloRequest { Name = "GreeterClient" });
            Console.WriteLine("Greeting: " + reply.Message);
            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

<span data-ttu-id="123e3-174">файл *Program.cs* содержит точку входа и логику для клиента gRPC.</span><span class="sxs-lookup"><span data-stu-id="123e3-174">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="123e3-175">Клиент Greeter создается следующим образом:</span><span class="sxs-lookup"><span data-stu-id="123e3-175">The Greeter client is created by:</span></span>

* <span data-ttu-id="123e3-176">Создание экземпляра `GrpcChannel` со сведениями для создания подключения к службе gRPC.</span><span class="sxs-lookup"><span data-stu-id="123e3-176">Instantiating a `GrpcChannel` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="123e3-177">Использование `GrpcChannel` для создания клиента Greeter.</span><span class="sxs-lookup"><span data-stu-id="123e3-177">Using the `GrpcChannel` to construct the Greeter client.</span></span>

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="123e3-178">Тестирование клиента gRPC с помощью службы Greeter gRPC</span><span class="sxs-lookup"><span data-stu-id="123e3-178">Test the gRPC client with the gRPC Greeter service</span></span>

* <span data-ttu-id="123e3-179">В службе Greeter нажмите `Ctrl+F5` для запуска сервера без отладчика.</span><span class="sxs-lookup"><span data-stu-id="123e3-179">In the Greeter service, press `Ctrl+F5` to start the server without the debugger.</span></span>
* <span data-ttu-id="123e3-180">В проекте `GrpcGreeterClient` нажмите `Ctrl+F5` для запуска клиента без отладчика.</span><span class="sxs-lookup"><span data-stu-id="123e3-180">In the `GrpcGreeterClient` project, press `Ctrl+F5` to start the client without the debugger.</span></span>

<span data-ttu-id="123e3-181">Клиент отправляет приветствие в службу с сообщением, содержащим его имя "GreeterClient".</span><span class="sxs-lookup"><span data-stu-id="123e3-181">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="123e3-182">Служба отправляет сообщение "Hello GreeterClient" в качестве ответа.</span><span class="sxs-lookup"><span data-stu-id="123e3-182">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="123e3-183">Ответ "Hello GreeterClient" отображается в командной строке:</span><span class="sxs-lookup"><span data-stu-id="123e3-183">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="123e3-184">Служба gRPC записывает сведения об успешном вызове в журналы, что отображается в командной строке.</span><span class="sxs-lookup"><span data-stu-id="123e3-184">The gRPC service records the details of the successful call in the logs written to the command prompt.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\GH\aspnet\docs\4\Docs\aspnetcore\tutorials\grpc\grpc-start\sample\GrpcGreeter
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 POST https://localhost:5001/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 78.32260000000001ms 200 application/grpc
```

### <a name="docs-help--next-steps-for-grpc"></a><span data-ttu-id="123e3-185">Справочная документация и дальнейшие инструкции для gRPC</span><span class="sxs-lookup"><span data-stu-id="123e3-185">Docs help & next steps for gRPC</span></span>

* [<span data-ttu-id="123e3-186">Введение в gRPC на платформе ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="123e3-186">Introduction to gRPC on ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/grpc/index?view=aspnetcore-3.0)
* [<span data-ttu-id="123e3-187">Службы gRPC на языке C#</span><span class="sxs-lookup"><span data-stu-id="123e3-187">gRPC services with C#</span></span>](https://docs.microsoft.com/aspnet/core/grpc/basics?view=aspnetcore-3.0)
* [<span data-ttu-id="123e3-188">Миграция служб gRPC из C Core на ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="123e3-188">Migrating gRPC services from C-core to ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/grpc/migration?view=aspnetcore-3.0)
