---
title: Защищайте Blazor автономное приложение ASP.NET Core WebAssembly с помощью Azure Active Directory B2C
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-azure-active-directory-b2c
ms.openlocfilehash: 0734bad2d4281eb856783a362ef8c608a303c17a
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/08/2020
ms.locfileid: "80977058"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-azure-active-directory-b2c"></a><span data-ttu-id="81086-102">Защищайте Blazor автономное приложение ASP.NET Core WebAssembly с помощью Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="81086-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with Azure Active Directory B2C</span></span>

<span data-ttu-id="81086-103">[Хавьер Кальварро Нельсон](https://github.com/javiercn) и Люк [Лэйтам](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="81086-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="81086-104">Для создания Blazor автономного приложения WebAssembly, использующему Для проверки подлинности [Active Directory (AAD) B2C,](/azure/active-directory-b2c/overview) используется приложение Для получения подлинности:</span><span class="sxs-lookup"><span data-stu-id="81086-104">To create a Blazor WebAssembly standalone app that uses [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) for authentication:</span></span>

1. <span data-ttu-id="81086-105">Следуйте инструкциям по следующим темам для создания арендатора и регистрации веб-приложения на портале Azure:</span><span class="sxs-lookup"><span data-stu-id="81086-105">Follow the guidance in the following topics to create a tenant and register a web app in the Azure Portal:</span></span>

   * <span data-ttu-id="81086-106">[Создайте aAD B2C арендатора](/azure/active-directory-b2c/tutorial-create-tenant) &ndash; Запись следующая информация:</span><span class="sxs-lookup"><span data-stu-id="81086-106">[Create an AAD B2C tenant](/azure/active-directory-b2c/tutorial-create-tenant) &ndash; Record the following information:</span></span>

     <span data-ttu-id="81086-107">1\.</span><span class="sxs-lookup"><span data-stu-id="81086-107">1\.</span></span> <span data-ttu-id="81086-108">AAD B2C экземпляр (например, `https://contoso.b2clogin.com/`который включает в себя задний слэш)</span><span class="sxs-lookup"><span data-stu-id="81086-108">AAD B2C instance (for example, `https://contoso.b2clogin.com/`, which includes the trailing slash)</span></span><br>
     <span data-ttu-id="81086-109">2\.</span><span class="sxs-lookup"><span data-stu-id="81086-109">2\.</span></span> <span data-ttu-id="81086-110">AAD B2C Арендатор домена `contoso.onmicrosoft.com`(например, )</span><span class="sxs-lookup"><span data-stu-id="81086-110">AAD B2C Tenant domain (for example, `contoso.onmicrosoft.com`)</span></span>

   * <span data-ttu-id="81086-111">[Регистрация веб-приложения](/azure/active-directory-b2c/tutorial-register-applications) &ndash; Сделать следующие выборы во время регистрации приложения:</span><span class="sxs-lookup"><span data-stu-id="81086-111">[Register a web application](/azure/active-directory-b2c/tutorial-register-applications) &ndash; Make the following selections during app registration:</span></span>

     <span data-ttu-id="81086-112">1\.</span><span class="sxs-lookup"><span data-stu-id="81086-112">1\.</span></span> <span data-ttu-id="81086-113">Установите **Web App / Web API** для **Yes**.</span><span class="sxs-lookup"><span data-stu-id="81086-113">Set **Web App / Web API** to **Yes**.</span></span><br>
     <span data-ttu-id="81086-114">2\.</span><span class="sxs-lookup"><span data-stu-id="81086-114">2\.</span></span> <span data-ttu-id="81086-115">Установить **Разрешить неявный поток** **да**.</span><span class="sxs-lookup"><span data-stu-id="81086-115">Set **Allow implicit flow** to **Yes**.</span></span><br>
     <span data-ttu-id="81086-116">3\.</span><span class="sxs-lookup"><span data-stu-id="81086-116">3\.</span></span> <span data-ttu-id="81086-117">Добавить **URL-адрес ответа** `https://localhost:5001/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="81086-117">Add a **Reply URL** of `https://localhost:5001/authentication/login-callback`.</span></span>

     <span data-ttu-id="81086-118">Запись идентификатора приложения (например, `11111111-1111-1111-1111-111111111111`id клиента).</span><span class="sxs-lookup"><span data-stu-id="81086-118">Record the Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`).</span></span>

   * <span data-ttu-id="81086-119">[Создание пользовательских потоков](/azure/active-directory-b2c/tutorial-create-user-flows) &ndash; Создание регистрации и вступления в поток пользователя.</span><span class="sxs-lookup"><span data-stu-id="81086-119">[Create user flows](/azure/active-directory-b2c/tutorial-create-user-flows) &ndash; Create a sign-up and sign-in user flow.</span></span>

     <span data-ttu-id="81086-120">Как минимум, выберите атрибут **приложения Претензии** > **Display Name** для заполнения `context.User.Identity.Name` `LoginDisplay` компонента *(Общий/LoginDisplay.razor*).</span><span class="sxs-lookup"><span data-stu-id="81086-120">At a minimum, select the **Application claims** > **Display Name** user attribute to populate the `context.User.Identity.Name` in the `LoginDisplay` component (*Shared/LoginDisplay.razor*).</span></span>

     <span data-ttu-id="81086-121">Запись регистрации и входе в пользовательское имя, созданное `B2C_1_signupsignin`для приложения (например, ).</span><span class="sxs-lookup"><span data-stu-id="81086-121">Record the sign-up and sign-in user flow name created for the app (for example, `B2C_1_signupsignin`).</span></span>

1. <span data-ttu-id="81086-122">Замените заполнителей в следующей команде информацией, записанной ранее, и выполните команду в командном корпусе:</span><span class="sxs-lookup"><span data-stu-id="81086-122">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -au IndividualB2C --aad-b2c-instance "{AAD B2C INSTANCE}" --client-id "{CLIENT ID}" --domain "{DOMAIN}" -ssp "{SIGN UP OR SIGN IN POLICY}"
   ```

   <span data-ttu-id="81086-123">Чтобы указать местона вывода, которое создает папку проекта, если она не существует, включите опцию вывода в команду с путем (например, `-o BlazorSample`).</span><span class="sxs-lookup"><span data-stu-id="81086-123">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="81086-124">Имя папки также становится частью названия проекта.</span><span class="sxs-lookup"><span data-stu-id="81086-124">The folder name also becomes part of the project's name.</span></span>

## <a name="authentication-package"></a><span data-ttu-id="81086-125">Пакет аутентификации</span><span class="sxs-lookup"><span data-stu-id="81086-125">Authentication package</span></span>

<span data-ttu-id="81086-126">Когда приложение создается для использования индивидуальной`IndividualB2C`учетной записи B2C ( ), приложение`Microsoft.Authentication.WebAssembly.Msal`автоматически получает ссылку на пакет для [библиотеки подлинности Майкрософт](/azure/active-directory/develop/msal-overview) ( ).</span><span class="sxs-lookup"><span data-stu-id="81086-126">When an app is created to use an Individual B2C Account (`IndividualB2C`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="81086-127">Пакет предоставляет набор примитивов, которые помогают приложению аутентифицировать пользователей и получать токены для вызова защищенных AIS.</span><span class="sxs-lookup"><span data-stu-id="81086-127">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="81086-128">При добавлении аутентификации в приложение вручную добавьте пакет в файл проекта приложения:</span><span class="sxs-lookup"><span data-stu-id="81086-128">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="81086-129">Замените `{VERSION}` в предыдущем пакете ссылку на версию `Microsoft.AspNetCore.Blazor.Templates` пакета, показанную <xref:blazor/get-started> в статье.</span><span class="sxs-lookup"><span data-stu-id="81086-129">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="81086-130">Пакет `Microsoft.Authentication.WebAssembly.Msal` транзитно добавляет `Microsoft.AspNetCore.Components.WebAssembly.Authentication` пакет в приложение.</span><span class="sxs-lookup"><span data-stu-id="81086-130">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="81086-131">Поддержка службы аутентификации</span><span class="sxs-lookup"><span data-stu-id="81086-131">Authentication service support</span></span>

<span data-ttu-id="81086-132">Поддержка аутентификации пользователей регистрируется `AddMsalAuthentication` в сервисном `Microsoft.Authentication.WebAssembly.Msal` контейнере с методом расширения, предусмотренным пакетом.</span><span class="sxs-lookup"><span data-stu-id="81086-132">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="81086-133">Этот метод настраивает все службы, необходимые для взаимодействия приложения с поставщиком идентификационных данных (IP).</span><span class="sxs-lookup"><span data-stu-id="81086-133">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="81086-134">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="81086-134">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = 
        "{AAD B2C INSTANCE}{DOMAIN}/{SIGN UP OR SIGN IN POLICY}";
    authentication.ClientId = "{CLIENT ID}";
    authentication.ValidateAuthority = false;
});
```

<span data-ttu-id="81086-135">Метод `AddMsalAuthentication` принимает обратный вызов для настройки параметров, необходимых для проверки подлинности приложения.</span><span class="sxs-lookup"><span data-stu-id="81086-135">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="81086-136">Значения, необходимые для настройки приложения, можно получить из конфигурации Azure Portal AAD при регистрации приложения.</span><span class="sxs-lookup"><span data-stu-id="81086-136">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

## <a name="access-token-scopes"></a><span data-ttu-id="81086-137">Области маркеров доступа</span><span class="sxs-lookup"><span data-stu-id="81086-137">Access token scopes</span></span>

<span data-ttu-id="81086-138">Шаблон Blazor WebAssembly не настраивает приложение автоматически, чтобы запросить токен доступа для защищенного API.</span><span class="sxs-lookup"><span data-stu-id="81086-138">The Blazor WebAssembly template doesn't automatically configure the app to request an access token for a secure API.</span></span> <span data-ttu-id="81086-139">Чтобы предоставить токен как часть потока ввоза, добавьте область к области `MsalProviderOptions`маркеров доступа по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="81086-139">To provision a token as part of the sign-in flow, add the scope to the default access token scopes of the `MsalProviderOptions`:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

> [!NOTE]
> <span data-ttu-id="81086-140">Если портал Azure предоставляет область URI и **приложение выбрасывает необработанное исключение,** когда получает *401 несанкционированный* ответ от API, попробуйте использовать область URI, которая не включает схему и узел.</span><span class="sxs-lookup"><span data-stu-id="81086-140">If the Azure portal provides a scope URI and **the app throws an unhandled exception** when it receives a *401 Unauthorized* response from the API, try using a scope URI that doesn't include the scheme and host.</span></span> <span data-ttu-id="81086-141">Например, портал Azure может предоставить один из следующих форматов URI:</span><span class="sxs-lookup"><span data-stu-id="81086-141">For example, the Azure portal may provide one of the following scope URI formats:</span></span>
>
> * `https://{ORGANIZATION}.onmicrosoft.com/{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
> * `api://{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
>
> <span data-ttu-id="81086-142">Поставка области URI без схемы и хозяина:</span><span class="sxs-lookup"><span data-stu-id="81086-142">Supply the scope URI without the scheme and host:</span></span>
>
> ```csharp
> options.ProviderOptions.DefaultAccessTokenScopes.Add(
>     "{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}");
> ```

<span data-ttu-id="81086-143">Для получения дополнительной информации см. <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>.</span><span class="sxs-lookup"><span data-stu-id="81086-143">For more information, see <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>.</span></span>

## <a name="imports-file"></a><span data-ttu-id="81086-144">Файл импорта</span><span class="sxs-lookup"><span data-stu-id="81086-144">Imports file</span></span>

[!INCLUDE[](~/includes/blazor-security/imports-file-standalone.md)]

## <a name="index-page"></a><span data-ttu-id="81086-145">Страница индексации</span><span class="sxs-lookup"><span data-stu-id="81086-145">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

## <a name="app-component"></a><span data-ttu-id="81086-146">Компонент приложения</span><span class="sxs-lookup"><span data-stu-id="81086-146">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="81086-147">ПеренаправлениеКомпонентToLogin</span><span class="sxs-lookup"><span data-stu-id="81086-147">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="81086-148">Компонент LoginDisplay</span><span class="sxs-lookup"><span data-stu-id="81086-148">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="81086-149">Компонент аутентификации</span><span class="sxs-lookup"><span data-stu-id="81086-149">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="81086-150">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="81086-150">Additional resources</span></span>

* [<span data-ttu-id="81086-151">Запрос дополнительных токенов доступа</span><span class="sxs-lookup"><span data-stu-id="81086-151">Request additional access tokens</span></span>](xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens)
* <xref:security/authentication/azure-ad-b2c>
* [<span data-ttu-id="81086-152">Руководство по созданию клиента Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="81086-152">Tutorial: Create an Azure Active Directory B2C tenant</span></span>](/azure/active-directory-b2c/tutorial-create-tenant)
* [<span data-ttu-id="81086-153">Документация по платформе удостоверений Майкрософт</span><span class="sxs-lookup"><span data-stu-id="81086-153">Microsoft identity platform documentation</span></span>](/azure/active-directory/develop/)
