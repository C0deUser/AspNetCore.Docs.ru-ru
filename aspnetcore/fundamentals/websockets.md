---
title: Поддержка WebSockets в ASP.NET Core
author: rick-anderson
description: Сведения о начале работы с WebSocket в ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: fundamentals/websockets
ms.openlocfilehash: a8040003374906fd93e12c9fde44c4a5ccc2cf37
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2020
ms.locfileid: "78645088"
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="33c5e-103">Поддержка WebSockets в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="33c5e-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="33c5e-104">Авторы: [Tom Dykstra](https://github.com/tdykstra) (Том Дикстра) и [Andrew Stanton-Nurse](https://github.com/anurse) (Эндрю Стэнтон-Нёрс)</span><span class="sxs-lookup"><span data-stu-id="33c5e-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="33c5e-105">Эта статья описывает начало работы с WebSocket в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="33c5e-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="33c5e-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) — это протокол, предоставляющий сохраняемые двусторонние каналы связи по TCP-подключениям.</span><span class="sxs-lookup"><span data-stu-id="33c5e-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="33c5e-107">Он используется в приложениях, где нужна быстрая связь в режиме реального времени, например в чатах, панелях мониторинга и играх.</span><span class="sxs-lookup"><span data-stu-id="33c5e-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="33c5e-108">[Просмотреть или скачать пример кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="33c5e-108">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="33c5e-109">[Процедура выполнения](#sample-app).</span><span class="sxs-lookup"><span data-stu-id="33c5e-109">[How to run](#sample-app).</span></span>

## <a name="signalr"></a><span data-ttu-id="33c5e-110">SignalR</span><span class="sxs-lookup"><span data-stu-id="33c5e-110">SignalR</span></span>

<span data-ttu-id="33c5e-111">[ASP.NET Core SignalR](xref:signalr/introduction) — это библиотека, которая упрощает добавление веб-функций в приложения в режиме реального времени.</span><span class="sxs-lookup"><span data-stu-id="33c5e-111">[ASP.NET Core SignalR](xref:signalr/introduction) is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="33c5e-112">Она использует WebSocket, когда это возможно.</span><span class="sxs-lookup"><span data-stu-id="33c5e-112">It uses WebSockets whenever possible.</span></span>

<span data-ttu-id="33c5e-113">Для большинства приложений рекомендуется использовать SignalR вместо прямых соединений WebSocket.</span><span class="sxs-lookup"><span data-stu-id="33c5e-113">For most applications, we recommend SignalR over raw WebSockets.</span></span> <span data-ttu-id="33c5e-114">Служба SignalR обеспечивает резервный транспорт для сред, в которых соединения WebSocket недоступны.</span><span class="sxs-lookup"><span data-stu-id="33c5e-114">SignalR provides transport fallback for environments where WebSockets is not available.</span></span> <span data-ttu-id="33c5e-115">Она также предоставляет простую модель приложений на основе удаленного вызова процедур.</span><span class="sxs-lookup"><span data-stu-id="33c5e-115">It also provides a simple remote procedure call app model.</span></span> <span data-ttu-id="33c5e-116">В большинстве сценариев SignalR по производительности не уступает прямым соединениям WebSocket.</span><span class="sxs-lookup"><span data-stu-id="33c5e-116">And in most scenarios, SignalR has no significant performance disadvantage compared to using raw WebSockets.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="33c5e-117">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="33c5e-117">Prerequisites</span></span>

* <span data-ttu-id="33c5e-118">ASP.NET Core 1.1 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="33c5e-118">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="33c5e-119">Любая ОС с поддержкой ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="33c5e-119">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="33c5e-120">Windows 7/Windows Server 2008 или более поздних версий</span><span class="sxs-lookup"><span data-stu-id="33c5e-120">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="33c5e-121">Linux</span><span class="sxs-lookup"><span data-stu-id="33c5e-121">Linux</span></span>
  * <span data-ttu-id="33c5e-122">macOS</span><span class="sxs-lookup"><span data-stu-id="33c5e-122">macOS</span></span>
  
* <span data-ttu-id="33c5e-123">Если приложение выполняется в Windows с IIS:</span><span class="sxs-lookup"><span data-stu-id="33c5e-123">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="33c5e-124">Windows 8/Windows Server 2012 или более поздних версий</span><span class="sxs-lookup"><span data-stu-id="33c5e-124">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="33c5e-125">IIS 8/IIS 8 Express</span><span class="sxs-lookup"><span data-stu-id="33c5e-125">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="33c5e-126">Необходимо включить WebSockets (см. раздел [Поддержка IIS и IIS Express](#iisiis-express-support)).</span><span class="sxs-lookup"><span data-stu-id="33c5e-126">WebSockets must be enabled (See the [IIS/IIS Express support](#iisiis-express-support) section.).</span></span>
  
* <span data-ttu-id="33c5e-127">Если приложение выполняется в [HTTP.sys](xref:fundamentals/servers/httpsys):</span><span class="sxs-lookup"><span data-stu-id="33c5e-127">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="33c5e-128">Windows 8/Windows Server 2012 или более поздних версий</span><span class="sxs-lookup"><span data-stu-id="33c5e-128">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="33c5e-129">Список поддерживаемых обозревателей см. на странице https://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="33c5e-129">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

::: moniker range="< aspnetcore-2.1"

## <a name="nuget-package"></a><span data-ttu-id="33c5e-130">Пакет NuGet</span><span class="sxs-lookup"><span data-stu-id="33c5e-130">NuGet package</span></span>

<span data-ttu-id="33c5e-131">Установка пакета [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/).</span><span class="sxs-lookup"><span data-stu-id="33c5e-131">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>

::: moniker-end

## <a name="configure-the-middleware"></a><span data-ttu-id="33c5e-132">Настройка ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="33c5e-132">Configure the middleware</span></span>


<span data-ttu-id="33c5e-133">Добавьте ПО промежуточного слоя WebSocket в метод `Configure` класса `Startup`:</span><span class="sxs-lookup"><span data-stu-id="33c5e-133">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="33c5e-134">Можно настроить следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="33c5e-134">The following settings can be configured:</span></span>

* <span data-ttu-id="33c5e-135">`KeepAliveInterval` — как часто нужно отправлять клиенту кадры проверки связи, чтобы прокси-серверы удерживали соединение открытым.</span><span class="sxs-lookup"><span data-stu-id="33c5e-135">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="33c5e-136">Значение по умолчанию — две минуты.</span><span class="sxs-lookup"><span data-stu-id="33c5e-136">The default is two minutes.</span></span>
* <span data-ttu-id="33c5e-137">`ReceiveBufferSize` — размер буфера, используемого для получения данных.</span><span class="sxs-lookup"><span data-stu-id="33c5e-137">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="33c5e-138">Опытные пользователи могут изменить этот параметр для настройки производительности с учетом размера данных.</span><span class="sxs-lookup"><span data-stu-id="33c5e-138">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="33c5e-139">Значение по умолчанию — 4 КБ.</span><span class="sxs-lookup"><span data-stu-id="33c5e-139">The default is 4 KB.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="33c5e-140">Можно настроить следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="33c5e-140">The following settings can be configured:</span></span>

* <span data-ttu-id="33c5e-141">`KeepAliveInterval` — как часто нужно отправлять клиенту кадры проверки связи, чтобы прокси-серверы удерживали соединение открытым.</span><span class="sxs-lookup"><span data-stu-id="33c5e-141">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="33c5e-142">Значение по умолчанию — две минуты.</span><span class="sxs-lookup"><span data-stu-id="33c5e-142">The default is two minutes.</span></span>
* <span data-ttu-id="33c5e-143"><xref:Microsoft.AspNetCore.Builder.WebSocketOptions.ReceiveBufferSize> — размер буфера, используемого для получения данных.</span><span class="sxs-lookup"><span data-stu-id="33c5e-143"><xref:Microsoft.AspNetCore.Builder.WebSocketOptions.ReceiveBufferSize> - The size of the buffer used to receive data.</span></span> <span data-ttu-id="33c5e-144">Опытные пользователи могут изменить этот параметр для настройки производительности с учетом размера данных.</span><span class="sxs-lookup"><span data-stu-id="33c5e-144">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="33c5e-145">Значение по умолчанию — 4 КБ.</span><span class="sxs-lookup"><span data-stu-id="33c5e-145">The default is 4 KB.</span></span>
* <span data-ttu-id="33c5e-146">`AllowedOrigins` — список допустимых значений заголовка Origin для запросов WebSocket.</span><span class="sxs-lookup"><span data-stu-id="33c5e-146">`AllowedOrigins` - A list of allowed Origin header values for WebSocket requests.</span></span> <span data-ttu-id="33c5e-147">По умолчанию разрешены все источники.</span><span class="sxs-lookup"><span data-stu-id="33c5e-147">By default, all origins are allowed.</span></span> <span data-ttu-id="33c5e-148">Дополнительные сведения см. в разделе "Ограничения для источников WebSocket" ниже.</span><span class="sxs-lookup"><span data-stu-id="33c5e-148">See "WebSocket origin restriction" below for details.</span></span>

::: moniker-end

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

## <a name="accept-websocket-requests"></a><span data-ttu-id="33c5e-149">Принятие запросов WebSocket</span><span class="sxs-lookup"><span data-stu-id="33c5e-149">Accept WebSocket requests</span></span>

<span data-ttu-id="33c5e-150">На более позднем этапе жизненного цикла запроса (например, далее в методе `Configure` или методе действия) проверьте, относится ли запрос к WebSocket, и примите его.</span><span class="sxs-lookup"><span data-stu-id="33c5e-150">Somewhere later in the request life cycle (later in the `Configure` method or in an action method, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="33c5e-151">Следующий пример взят из дальнейшей части метода `Configure`:</span><span class="sxs-lookup"><span data-stu-id="33c5e-151">The following example is from later in the `Configure` method:</span></span>

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="33c5e-152">Запрос WebSocket может поступить по любому URL-адресу, но этот пример кода принимает только запросы для `/ws`.</span><span class="sxs-lookup"><span data-stu-id="33c5e-152">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

<span data-ttu-id="33c5e-153">При использовании WebSocket вам **необходимо** поддерживать работу конвейера ПО промежуточного слоя, пока соединение активно.</span><span class="sxs-lookup"><span data-stu-id="33c5e-153">When using a WebSocket, you **must** keep the middleware pipeline running for the duration of the connection.</span></span> <span data-ttu-id="33c5e-154">При попытке отправить или получить сообщение WebSocket после того, как конвейер ПО промежуточного слоя завершил работу, вы можете получить исключение такого вида:</span><span class="sxs-lookup"><span data-stu-id="33c5e-154">If you attempt to send or receive a WebSocket message after the middleware pipeline ends, you may get an exception like the following:</span></span>

```
System.Net.WebSockets.WebSocketException (0x80004005): The remote party closed the WebSocket connection without completing the close handshake. ---> System.ObjectDisposedException: Cannot write to the response body, the response has completed.
Object name: 'HttpResponseStream'.
```

<span data-ttu-id="33c5e-155">Если вы используете фоновую службу для записи данных в WebSocket, убедитесь, что конвейер ПО промежуточного слоя продолжает работу.</span><span class="sxs-lookup"><span data-stu-id="33c5e-155">If you're using a background service to write data to a WebSocket, make sure you keep the middleware pipeline running.</span></span> <span data-ttu-id="33c5e-156">Это можно сделать с помощью <xref:System.Threading.Tasks.TaskCompletionSource%601>.</span><span class="sxs-lookup"><span data-stu-id="33c5e-156">Do this by using a <xref:System.Threading.Tasks.TaskCompletionSource%601>.</span></span> <span data-ttu-id="33c5e-157">Передайте `TaskCompletionSource` фоновой службе, а после завершения работы с WebSocket вызовите <xref:System.Threading.Tasks.TaskCompletionSource%601.TrySetResult%2A> с ее помощью.</span><span class="sxs-lookup"><span data-stu-id="33c5e-157">Pass the `TaskCompletionSource` to your background service and have it call <xref:System.Threading.Tasks.TaskCompletionSource%601.TrySetResult%2A> when you finish with the WebSocket.</span></span> <span data-ttu-id="33c5e-158">Затем используйте конструкцию `await` для ожидания свойства <xref:System.Threading.Tasks.TaskCompletionSource%601.Task> во время запроса, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="33c5e-158">Then `await` the <xref:System.Threading.Tasks.TaskCompletionSource%601.Task> property during the request, as shown in the following example:</span></span>

```csharp
app.Use(async (context, next) => {
    var socket = await context.WebSockets.AcceptWebSocketAsync();
    var socketFinishedTcs = new TaskCompletionSource<object>();

    BackgroundSocketProcessor.AddSocket(socket, socketFinishedTcs); 

    await socketFinishedTcs.Task;
});
```
<span data-ttu-id="33c5e-159">Исключение закрытия WebSocket может также возникнуть, если метод действия возвращает управление слишком быстро.</span><span class="sxs-lookup"><span data-stu-id="33c5e-159">The WebSocket closed exception can also happen if you return too soon from an action method.</span></span> <span data-ttu-id="33c5e-160">Если вы принимаете сокет в методе действия, дождитесь завершения выполнения кода, в котором используется сокет, прежде чем возвращать управление из метода действия.</span><span class="sxs-lookup"><span data-stu-id="33c5e-160">If you accept a socket in an action method, wait for the code that uses the socket to complete before returning from the action method.</span></span>

<span data-ttu-id="33c5e-161">Никогда не используйте `Task.Wait()`, `Task.Result` или аналогичные блокирующие вызовы для ожидания выполнения сокета, так как это может привести к серьезным проблемам потокового выполнения.</span><span class="sxs-lookup"><span data-stu-id="33c5e-161">Never use `Task.Wait()`, `Task.Result`, or similar blocking calls to wait for the socket to complete, as that can cause serious threading issues.</span></span> <span data-ttu-id="33c5e-162">Всегда используйте `await`.</span><span class="sxs-lookup"><span data-stu-id="33c5e-162">Always use `await`.</span></span>

## <a name="send-and-receive-messages"></a><span data-ttu-id="33c5e-163">Отправка и получение сообщений</span><span class="sxs-lookup"><span data-stu-id="33c5e-163">Send and receive messages</span></span>

<span data-ttu-id="33c5e-164">Метод `AcceptWebSocketAsync` обновляет TCP-соединение до соединения WebSocket и предоставляет объект [WebSocket](/dotnet/core/api/system.net.websockets.websocket).</span><span class="sxs-lookup"><span data-stu-id="33c5e-164">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="33c5e-165">Используйте объект `WebSocket` для отправки и получения сообщений.</span><span class="sxs-lookup"><span data-stu-id="33c5e-165">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="33c5e-166">Приведенный выше код, который принимает запрос WebSocket, передает объект `WebSocket` в метод `Echo`.</span><span class="sxs-lookup"><span data-stu-id="33c5e-166">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="33c5e-167">Код принимает сообщение и сразу отправляет такое же сообщение обратно.</span><span class="sxs-lookup"><span data-stu-id="33c5e-167">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="33c5e-168">Сообщения отправляются и получаются циклически, пока клиент не закроет подключение:</span><span class="sxs-lookup"><span data-stu-id="33c5e-168">Messages are sent and received in a loop until the client closes the connection:</span></span>

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=Echo)]

<span data-ttu-id="33c5e-169">Если вы принимаете подключение WebSocket до начала этого цикла, конвейер ПО промежуточного слоя завершается.</span><span class="sxs-lookup"><span data-stu-id="33c5e-169">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="33c5e-170">После закрытия сокета конвейер развертывается.</span><span class="sxs-lookup"><span data-stu-id="33c5e-170">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="33c5e-171">То есть запрос перестает перемещаться по конвейеру после принятия WebSocket.</span><span class="sxs-lookup"><span data-stu-id="33c5e-171">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="33c5e-172">После завершения цикла и закрытия сокета запрос возвращается в конвейер.</span><span class="sxs-lookup"><span data-stu-id="33c5e-172">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="handle-client-disconnects"></a><span data-ttu-id="33c5e-173">Обработка отключений клиента</span><span class="sxs-lookup"><span data-stu-id="33c5e-173">Handle client disconnects</span></span>

<span data-ttu-id="33c5e-174">При отключении клиента из-за потери связи сервер не получает сведения автоматически.</span><span class="sxs-lookup"><span data-stu-id="33c5e-174">The server is not automatically informed when the client disconnects due to loss of connectivity.</span></span> <span data-ttu-id="33c5e-175">Сервер получает сообщение об отключении, только если клиент отправляет его. В случае потери подключения к Интернету это невозможно.</span><span class="sxs-lookup"><span data-stu-id="33c5e-175">The server receives a disconnect message only if the client sends it, which can't be done if the internet connection is lost.</span></span> <span data-ttu-id="33c5e-176">Если при этом вы хотите выполнить определенное действие, установите время ожидания сигнала клиента с определенным интервалом.</span><span class="sxs-lookup"><span data-stu-id="33c5e-176">If you want to take some action when that happens, set a timeout after nothing is received from the client within a certain time window.</span></span>

<span data-ttu-id="33c5e-177">Если клиент не всегда отправляет сообщения, а вы не хотите ожидать истечения времени ожидания только потому, что подключение не используется, тогда клиент может использовать таймер для отправки сообщения проверки связи каждые X секунд.</span><span class="sxs-lookup"><span data-stu-id="33c5e-177">If the client isn't always sending messages and you don't want to timeout just because the connection goes idle, have the client use a timer to send a ping message every X seconds.</span></span> <span data-ttu-id="33c5e-178">Если сообщение не было получено на сервере в течение 2\*X секунд после предыдущего, завершите подключение и сообщите о том, что клиент отключен.</span><span class="sxs-lookup"><span data-stu-id="33c5e-178">On the server, if a message hasn't arrived within 2\*X seconds after the previous one, terminate the connection and report that the client disconnected.</span></span> <span data-ttu-id="33c5e-179">Подождите вдвое дольше ожидаемого временного интервала, чтобы оставить дополнительное время на задержки в сети при отправке сообщения проверки связи.</span><span class="sxs-lookup"><span data-stu-id="33c5e-179">Wait for twice the expected time interval to leave extra time for network delays that might hold up the ping message.</span></span>

## <a name="websocket-origin-restriction"></a><span data-ttu-id="33c5e-180">Ограничения для источников WebSocket</span><span class="sxs-lookup"><span data-stu-id="33c5e-180">WebSocket origin restriction</span></span>

<span data-ttu-id="33c5e-181">Варианты защиты, предоставляемые CORS, не применяются к WebSocket.</span><span class="sxs-lookup"><span data-stu-id="33c5e-181">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="33c5e-182">Браузеры **не** поддерживают следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="33c5e-182">Browsers do **not**:</span></span>

* <span data-ttu-id="33c5e-183">выполнение предварительных запросов CORS;</span><span class="sxs-lookup"><span data-stu-id="33c5e-183">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="33c5e-184">использование ограничений, указанных в заголовках `Access-Control`, при выполнении запросов WebSocket.</span><span class="sxs-lookup"><span data-stu-id="33c5e-184">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="33c5e-185">Однако браузеры отправляют заголовок `Origin` при выпуске запросов WebSocket.</span><span class="sxs-lookup"><span data-stu-id="33c5e-185">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="33c5e-186">Приложения должны быть настроены для проверки этих заголовков, чтобы использовались только WebSocket из ожидаемых источников.</span><span class="sxs-lookup"><span data-stu-id="33c5e-186">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="33c5e-187">Если вы размещаете сервер по адресу "https://server.com", а клиент — по адресу "https://client.com", добавьте "https://client.com" в список `AllowedOrigins` подлежащих проверке WebSocket.</span><span class="sxs-lookup"><span data-stu-id="33c5e-187">If you're hosting your server on "https://server.com" and hosting your client on "https://client.com", add "https://client.com" to the `AllowedOrigins` list for WebSockets to verify.</span></span>

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptionsAO&highlight=6-7)]

> [!NOTE]
> <span data-ttu-id="33c5e-188">Заголовок `Origin` контролируется клиентом и, как и заголовок `Referer`, может быть подделан.</span><span class="sxs-lookup"><span data-stu-id="33c5e-188">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="33c5e-189">**Не** используйте эти заголовки в качестве механизма проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="33c5e-189">Do **not** use these headers as an authentication mechanism.</span></span>

::: moniker-end

## <a name="iisiis-express-support"></a><span data-ttu-id="33c5e-190">Поддержка служб IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="33c5e-190">IIS/IIS Express support</span></span>

<span data-ttu-id="33c5e-191">Windows Server 2012 или более поздней версии и Windows 8 или более поздней версии с IIS и IIS Express 8 или более поздней версии поддерживают протокол WebSocket.</span><span class="sxs-lookup"><span data-stu-id="33c5e-191">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="33c5e-192">Соединения WebSockets всегда включены при использовании IIS Express.</span><span class="sxs-lookup"><span data-stu-id="33c5e-192">WebSockets are always enabled when using IIS Express.</span></span>

### <a name="enabling-websockets-on-iis"></a><span data-ttu-id="33c5e-193">Включение WebSockets в службах IIS</span><span class="sxs-lookup"><span data-stu-id="33c5e-193">Enabling WebSockets on IIS</span></span>

<span data-ttu-id="33c5e-194">Чтобы включить поддержку протокола WebSocket в Windows Server 2012 или более поздней версии:</span><span class="sxs-lookup"><span data-stu-id="33c5e-194">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="33c5e-195">Эти действия не требуется выполнять при использовании IIS Express</span><span class="sxs-lookup"><span data-stu-id="33c5e-195">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="33c5e-196">В меню **Управление** запустите мастер **Добавить роли и компоненты** или в окне **Диспетчер серверов** щелкните соответствующую ссылку.</span><span class="sxs-lookup"><span data-stu-id="33c5e-196">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="33c5e-197">Выберите **Установка ролей или компонентов**.</span><span class="sxs-lookup"><span data-stu-id="33c5e-197">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="33c5e-198">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="33c5e-198">Select **Next**.</span></span>
1. <span data-ttu-id="33c5e-199">Выберите подходящий сервер (по умолчанию выбирается локальный сервер).</span><span class="sxs-lookup"><span data-stu-id="33c5e-199">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="33c5e-200">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="33c5e-200">Select **Next**.</span></span>
1. <span data-ttu-id="33c5e-201">Разверните **Веб-сервер (IIS)** в дереве **Роли**, разверните **Веб-сервер**, а затем **Разработка приложений**.</span><span class="sxs-lookup"><span data-stu-id="33c5e-201">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="33c5e-202">Выберите **протокол WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="33c5e-202">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="33c5e-203">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="33c5e-203">Select **Next**.</span></span>
1. <span data-ttu-id="33c5e-204">Если дополнительные функции не требуются, нажмите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="33c5e-204">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="33c5e-205">Нажмите кнопку **Установить**.</span><span class="sxs-lookup"><span data-stu-id="33c5e-205">Select **Install**.</span></span>
1. <span data-ttu-id="33c5e-206">По завершении установки выберите **Закрыть**, чтобы выйти из мастера.</span><span class="sxs-lookup"><span data-stu-id="33c5e-206">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="33c5e-207">Чтобы включить поддержку протокола WebSocket в Windows 8 или более поздней версии:</span><span class="sxs-lookup"><span data-stu-id="33c5e-207">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="33c5e-208">Эти действия не требуется выполнять при использовании IIS Express</span><span class="sxs-lookup"><span data-stu-id="33c5e-208">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="33c5e-209">Последовательно выберите **Панель управления** > **Программы** > **Программы и компоненты** > **Включение или отключение компонентов Windows** (в левой части экрана).</span><span class="sxs-lookup"><span data-stu-id="33c5e-209">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="33c5e-210">Откройте следующие узлы: **IIS** > **Службы Интернета** > **Компоненты разработки приложений**.</span><span class="sxs-lookup"><span data-stu-id="33c5e-210">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="33c5e-211">Выберите компонент **Протокол WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="33c5e-211">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="33c5e-212">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="33c5e-212">Select **OK**.</span></span>

### <a name="disable-websocket-when-using-socketio-on-nodejs"></a><span data-ttu-id="33c5e-213">Отключите WebSocket при использовании socket.io на Node.js</span><span class="sxs-lookup"><span data-stu-id="33c5e-213">Disable WebSocket when using socket.io on Node.js</span></span>

<span data-ttu-id="33c5e-214">Если используется поддержка WebSocket в [socket.io](https://socket.io/) на [Node.js](https://nodejs.org/), отключите модуль WebSocket IIS по умолчанию с помощью элемента `webSocket` в *web.config* или *applicationHost.config*. Если не выполнить этот шаг, модуль IIS WebSocket попытается обработать соединение WebSocket, а не Node.js и приложение.</span><span class="sxs-lookup"><span data-stu-id="33c5e-214">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="sample-app"></a><span data-ttu-id="33c5e-215">Пример приложения</span><span class="sxs-lookup"><span data-stu-id="33c5e-215">Sample app</span></span>

<span data-ttu-id="33c5e-216">[Пример приложения](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) в этой статье — это эхо-приложение.</span><span class="sxs-lookup"><span data-stu-id="33c5e-216">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) that accompanies this article is an echo app.</span></span> <span data-ttu-id="33c5e-217">Оно имеет веб-страницу, которая устанавливает соединения WebSocket, а сервер перенаправляет все полученные сообщения обратно клиенту.</span><span class="sxs-lookup"><span data-stu-id="33c5e-217">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="33c5e-218">Запустите приложение из командной строки (оно не предназначено для запуска из Visual Studio с IIS Express) и перейдите по адресу http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="33c5e-218">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="33c5e-219">В верхнем левом углу веб-страницы отображается состояние подключения:</span><span class="sxs-lookup"><span data-stu-id="33c5e-219">The web page shows the connection status in the upper left:</span></span>

![Начальное состояние веб-страницы](websockets/_static/start.png)

<span data-ttu-id="33c5e-221">Выберите **Connect** (Подключить), чтобы отправить запрос WebSocket на показанный URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="33c5e-221">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="33c5e-222">Введите тестовое сообщение и выберите **Send** (Отправить).</span><span class="sxs-lookup"><span data-stu-id="33c5e-222">Enter a test message and select **Send**.</span></span> <span data-ttu-id="33c5e-223">После этого выберите **Close Socket** (Закрыть сокет).</span><span class="sxs-lookup"><span data-stu-id="33c5e-223">When done, select **Close Socket**.</span></span> <span data-ttu-id="33c5e-224">В разделе **Communication Log** (Журнал связи) выводится каждое выполняемое действие открытия, отправки и закрытия.</span><span class="sxs-lookup"><span data-stu-id="33c5e-224">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Начальное состояние веб-страницы](websockets/_static/end.png)

