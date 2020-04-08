---
title: DevOps с ASP.NET Core и Azure
author: CamSoper
description: Рекомендации по созданию сквозного решения конвейера DevOps для приложения ASP.NET Core, размещенного в Azure.
ms.author: casoper
ms.date: 08/07/2018
ms.custom: mvc, seodec18
uid: azure/devops/index
ms.openlocfilehash: f45bb2a5dd4b3d1a820085ede7ce3219045ed80b
ms.sourcegitcommit: 72792e349458190b4158fcbacb87caf3fc605268
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2020
ms.locfileid: "78644248"
---
# <a name="devops-with-aspnet-core-and-azure"></a><span data-ttu-id="c62db-103">DevOps с ASP.NET Core и Azure</span><span class="sxs-lookup"><span data-stu-id="c62db-103">DevOps with ASP.NET Core and Azure</span></span>

<span data-ttu-id="c62db-104">[![Изображение обложки](./media/cover-large.png)](https://aka.ms/devopsbook)</span><span class="sxs-lookup"><span data-stu-id="c62db-104">[![Cover Image](./media/cover-large.png)](https://aka.ms/devopsbook)</span></span>

<span data-ttu-id="c62db-105">Авторы [Кэм Сопер (Cam Soper)](https://twitter.com/camsoper) и [Скотт Эдди (Scott Addie)](https://twitter.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="c62db-105">By [Cam Soper](https://twitter.com/camsoper) and [Scott Addie](https://twitter.com/scottaddie)</span></span>

<span data-ttu-id="c62db-106">Это руководство доступно как [загружаемая электронная книга в формате PDF](https://aka.ms/devopsbook).</span><span class="sxs-lookup"><span data-stu-id="c62db-106">This guide is available as a [downloadable PDF e-book](https://aka.ms/devopsbook).</span></span>

## <a name="welcome"></a><span data-ttu-id="c62db-107">Экран приветствия</span><span class="sxs-lookup"><span data-stu-id="c62db-107">Welcome</span></span> 

<span data-ttu-id="c62db-108">Руководство по жизненному циклу разработки Azure для .NET</span><span class="sxs-lookup"><span data-stu-id="c62db-108">Welcome to the Azure Development Lifecycle guide for .NET!</span></span> <span data-ttu-id="c62db-109">В этом руководстве предоставляются основные сведения по созданию жизненного цикла разработки в Azure с помощью инструментов и процессов .NET.</span><span class="sxs-lookup"><span data-stu-id="c62db-109">This guide introduces the basic concepts of building a development lifecycle around Azure using .NET tools and processes.</span></span> <span data-ttu-id="c62db-110">После его прохождения вы сможете наиболее эффективно использовать цепочку инструментов DevOps.</span><span class="sxs-lookup"><span data-stu-id="c62db-110">After finishing this guide, you'll reap the benefits of a mature DevOps toolchain.</span></span>

## <a name="who-this-guide-is-for"></a><span data-ttu-id="c62db-111">Для кого предназначено это руководство</span><span class="sxs-lookup"><span data-stu-id="c62db-111">Who this guide is for</span></span>

<span data-ttu-id="c62db-112">Вы должны быть опытным разработчиком ASP.NET Core (уровень 200–300).</span><span class="sxs-lookup"><span data-stu-id="c62db-112">You should be an experienced ASP.NET Core developer (200-300 level).</span></span> <span data-ttu-id="c62db-113">Вам не нужно ничего знать об Azure, так как эти сведения есть во введении.</span><span class="sxs-lookup"><span data-stu-id="c62db-113">You don't need to know anything about Azure, as we'll cover that in this introduction.</span></span> <span data-ttu-id="c62db-114">Это руководство может также оказаться полезным для инженеров DevOps, которые преимущественно работают с операциями, а не занимаются разработкой.</span><span class="sxs-lookup"><span data-stu-id="c62db-114">This guide may also be useful for DevOps engineers who are more focused on operations than development.</span></span>

<span data-ttu-id="c62db-115">Это руководство предназначено для разработчиков Windows.</span><span class="sxs-lookup"><span data-stu-id="c62db-115">This guide targets Windows developers.</span></span> <span data-ttu-id="c62db-116">Linux и macOS также полностью поддерживаются в .NET Core.</span><span class="sxs-lookup"><span data-stu-id="c62db-116">However, Linux and macOS are fully supported by .NET Core.</span></span> <span data-ttu-id="c62db-117">Чтобы адаптировать это руководство для Linux или macOS, смотрите сноски, в которых приводятся характерные отличия.</span><span class="sxs-lookup"><span data-stu-id="c62db-117">To adapt this guide for Linux/macOS, watch for callouts for Linux/macOS differences.</span></span>

## <a name="what-this-guide-doesnt-cover"></a><span data-ttu-id="c62db-118">Темы, которые выходят за рамки этого руководства</span><span class="sxs-lookup"><span data-stu-id="c62db-118">What this guide doesn't cover</span></span>

<span data-ttu-id="c62db-119">В этом руководстве приводятся рекомендации для разработчиков .NET по сквозному непрерывному развертыванию.</span><span class="sxs-lookup"><span data-stu-id="c62db-119">This guide is focused on an end-to-end continuous deployment experience for .NET developers.</span></span> <span data-ttu-id="c62db-120">Это не исчерпывающее руководство по Azure, и в нем не рассматриваются подробно API .NET для служб Azure.</span><span class="sxs-lookup"><span data-stu-id="c62db-120">It's not an exhaustive guide to all things Azure, and it doesn't focus extensively on .NET APIs for Azure services.</span></span> <span data-ttu-id="c62db-121">Основное внимание уделяется непрерывной интеграции, развертыванию, мониторингу и отладке.</span><span class="sxs-lookup"><span data-stu-id="c62db-121">The emphasis is all around continuous integration, deployment, monitoring, and debugging.</span></span> <span data-ttu-id="c62db-122">В конце руководства предлагаются рекомендации по дальнейшим действиям.</span><span class="sxs-lookup"><span data-stu-id="c62db-122">Near the end of the guide, recommendations for next steps are offered.</span></span> <span data-ttu-id="c62db-123">В число предложений входят службы платформы Azure, полезные для разработчиков ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c62db-123">Included in the suggestions are Azure platform services that are useful to ASP.NET Core developers.</span></span>

## <a name="whats-in-this-guide"></a><span data-ttu-id="c62db-124">Содержание руководства</span><span class="sxs-lookup"><span data-stu-id="c62db-124">What's in this guide</span></span>

### <a name="tools-and-downloads"></a>[<span data-ttu-id="c62db-125">Инструменты и файлы для скачивания</span><span class="sxs-lookup"><span data-stu-id="c62db-125">Tools and downloads</span></span>](xref:azure/devops/tools-and-downloads)

<span data-ttu-id="c62db-126">Вы узнаете, где получить инструменты, используемые в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="c62db-126">Learn where to acquire the tools used in this guide.</span></span>

### <a name="deploy-to-app-service"></a>[<span data-ttu-id="c62db-127">Развертывание в службу приложений</span><span class="sxs-lookup"><span data-stu-id="c62db-127">Deploy to App Service</span></span>](xref:azure/devops/deploy-to-app-service)

<span data-ttu-id="c62db-128">Разные способы развертывания приложения ASP.NET Core в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="c62db-128">Learn the various methods for deploying an ASP.NET Core app to Azure App Service.</span></span>

### <a name="continuous-integration-and-deployment"></a>[<span data-ttu-id="c62db-129">Непрерывная интеграция и развертывание</span><span class="sxs-lookup"><span data-stu-id="c62db-129">Continuous integration and deployment</span></span>](xref:azure/devops/cicd)

<span data-ttu-id="c62db-130">Создание решения сквозной непрерывной интеграции и развертывания для приложения ASP.NET Core с помощью GitHub, Azure DevOps Services и Azure.</span><span class="sxs-lookup"><span data-stu-id="c62db-130">Build an end-to-end continuous integration and deployment solution for your ASP.NET Core app with GitHub, Azure DevOps Services, and Azure.</span></span>

### <a name="monitor-and-debug"></a>[<span data-ttu-id="c62db-131">Мониторинг и отладка</span><span class="sxs-lookup"><span data-stu-id="c62db-131">Monitor and debug</span></span>](xref:azure/devops/monitor)

<span data-ttu-id="c62db-132">Мониторинг, устранение неполадок и настройка приложения с помощью инструментов Azure.</span><span class="sxs-lookup"><span data-stu-id="c62db-132">Use Azure's tools to monitor, troubleshoot, and tune your application.</span></span>

### <a name="next-steps"></a>[<span data-ttu-id="c62db-133">Дальнейшие действия</span><span class="sxs-lookup"><span data-stu-id="c62db-133">Next steps</span></span>](xref:azure/devops/next-steps)

<span data-ttu-id="c62db-134">Другие способы изучения Azure для разработчиков ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c62db-134">Other learning paths for the ASP.NET Core developer learning Azure.</span></span>

## <a name="additional-introductory-reading"></a><span data-ttu-id="c62db-135">Дополнительные справочные материалы</span><span class="sxs-lookup"><span data-stu-id="c62db-135">Additional introductory reading</span></span>

<span data-ttu-id="c62db-136">Если это ваш первый опыт работы с облачными вычислениями, в этой статье рассматриваются основы.</span><span class="sxs-lookup"><span data-stu-id="c62db-136">If this is your first exposure to cloud computing, these articles explain the basics.</span></span>

* [<span data-ttu-id="c62db-137">Что такое облачные вычисления?</span><span class="sxs-lookup"><span data-stu-id="c62db-137">What is Cloud Computing?</span></span>](https://azure.microsoft.com/overview/what-is-cloud-computing/)
* [<span data-ttu-id="c62db-138">Примеры облачных вычислений</span><span class="sxs-lookup"><span data-stu-id="c62db-138">Examples of Cloud Computing</span></span>](https://azure.microsoft.com/overview/examples-of-cloud-computing/)
* [<span data-ttu-id="c62db-139">Что такое IaaS?</span><span class="sxs-lookup"><span data-stu-id="c62db-139">What is IaaS?</span></span>](https://azure.microsoft.com/overview/what-is-iaas/)
* [<span data-ttu-id="c62db-140">Что такое PaaS?</span><span class="sxs-lookup"><span data-stu-id="c62db-140">What is PaaS?</span></span>](https://azure.microsoft.com/overview/what-is-paas/)
