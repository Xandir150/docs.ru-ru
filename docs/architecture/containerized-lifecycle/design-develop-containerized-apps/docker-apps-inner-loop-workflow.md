---
title: Рабочий процесс внутреннего цикла разработки для приложений Docker
description: Сведения о рабочем процессе "внутреннего цикла" при разработке приложений Docker.
ms.date: 02/15/2019
ms.openlocfilehash: ce573546f61b98c2f93e998203497fa949e9efe8
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68673981"
---
# <a name="inner-loop-development-workflow-for-docker-apps"></a><span data-ttu-id="d1e66-103">Рабочий процесс внутреннего цикла разработки для приложений Docker</span><span class="sxs-lookup"><span data-stu-id="d1e66-103">Inner-loop development workflow for Docker apps</span></span>

<span data-ttu-id="d1e66-104">Рабочий процесс внутреннего цикла, охватывающий весь цикл DevOps, начинается на компьютере каждого разработчика, где разработчик локально пишет код приложения на предпочитаемых языках и платформах, а затем тестирует его (рис. 4-21).</span><span class="sxs-lookup"><span data-stu-id="d1e66-104">Before triggering the outer-loop workflow spanning the entire DevOps cycle, it all begins on each developer's machine, coding the app itself, using their preferred languages or platforms, and testing it locally (Figure 4-21).</span></span> <span data-ttu-id="d1e66-105">Независимо от выбранного языка и платформы этот рабочий процесс имеет одну общую черту:</span><span class="sxs-lookup"><span data-stu-id="d1e66-105">But in every case, you'll have an important point in common, no matter what language, framework, or platforms you choose.</span></span> <span data-ttu-id="d1e66-106">вы всегда разрабатываете и тестируете контейнеры Docker, но делаете это локально.</span><span class="sxs-lookup"><span data-stu-id="d1e66-106">In this specific workflow, you're always developing and testing Docker containers, but locally.</span></span>

![Шаг 1. Написание кода, запуск и отладка](./media/image18.png)

<span data-ttu-id="d1e66-108">**Рис. 4-21**.</span><span class="sxs-lookup"><span data-stu-id="d1e66-108">**Figure 4-21**.</span></span> <span data-ttu-id="d1e66-109">Контекст внутреннего цикла разработки</span><span class="sxs-lookup"><span data-stu-id="d1e66-109">Inner-loop development context</span></span>

<span data-ttu-id="d1e66-110">В каждый контейнер или экземпляр образа Docker входят следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="d1e66-110">The container or instance of a Docker image will contain these components:</span></span>

- <span data-ttu-id="d1e66-111">выбранная операционная система (например, Windows или дистрибутив Linux);</span><span class="sxs-lookup"><span data-stu-id="d1e66-111">An operating system selection (for example, a Linux distribution or Windows)</span></span>

- <span data-ttu-id="d1e66-112">файлы, добавленные разработчиком (например, двоичные файлы приложения);</span><span class="sxs-lookup"><span data-stu-id="d1e66-112">Files added by the developer (for example, app binaries)</span></span>

- <span data-ttu-id="d1e66-113">конфигурация (например, параметры среды и зависимости);</span><span class="sxs-lookup"><span data-stu-id="d1e66-113">Configuration (for example, environment settings and dependencies)</span></span>

- <span data-ttu-id="d1e66-114">инструкции касательно того, какие процессы должны выполняться Docker.</span><span class="sxs-lookup"><span data-stu-id="d1e66-114">Instructions for what processes to run by Docker</span></span>

<span data-ttu-id="d1e66-115">Рабочий процесс внутреннего цикла разработки на основе Docker можно настроить как процесс (как описано в следующем разделе).</span><span class="sxs-lookup"><span data-stu-id="d1e66-115">You can set up the inner-loop development workflow that utilizes Docker as the process (described in the next section).</span></span> <span data-ttu-id="d1e66-116">Учтите, что начальные этапы настройки среды здесь не рассматриваются, так как они выполняются только один раз.</span><span class="sxs-lookup"><span data-stu-id="d1e66-116">Consider that the initial steps to set up the environment are not included, because you only need to do it once.</span></span>

## <a name="building-a-single-app-within-a-docker-container-using-visual-studio-code-and-docker-cli"></a><span data-ttu-id="d1e66-117">Создание одного приложения в контейнере Docker с помощью Visual Studio Code и интерфейса командной строки (CLI) Docker</span><span class="sxs-lookup"><span data-stu-id="d1e66-117">Building a single app within a Docker container using Visual Studio Code and Docker CLI</span></span>

<span data-ttu-id="d1e66-118">Приложение состоит из ваших собственных служб и дополнительных библиотек (зависимостей).</span><span class="sxs-lookup"><span data-stu-id="d1e66-118">Apps are made up from your own services plus additional libraries (dependencies).</span></span>

<span data-ttu-id="d1e66-119">На рис. 4-22 показаны основные шаги, которые обычно необходимо выполнить при сборке приложения Docker, после чего приводится подробное описание каждого из них.</span><span class="sxs-lookup"><span data-stu-id="d1e66-119">Figure 4-22 shows the basic steps that you usually need to carry out when building a Docker app, followed by detailed descriptions of each step.</span></span>

![Обзор рабочего процесса: шаг 1 — написание кода; шаг 2 — написание файлов Dockerfile; шаг 3 — создание образов, определенных в файлах Dockerfile; шаг 4 — определение служб в файле docker-compose; шаг 5 — запуск контейнеров или скомпонованных приложений; шаг 6 — тестирование приложений; шаг 7 — начало внешнего цикла (конвейеры CI/CD) или продолжение разработки.](./media/image19.png)

<span data-ttu-id="d1e66-121">**Рис. 4-22**.</span><span class="sxs-lookup"><span data-stu-id="d1e66-121">**Figure 4-22**.</span></span> <span data-ttu-id="d1e66-122">Высокоуровневое представление рабочего процесса для жизненного цикла контейнерных приложений Docker, создаваемых с помощью CLI Docker</span><span class="sxs-lookup"><span data-stu-id="d1e66-122">High-level workflow for the life cycle for Docker containerized applications using Docker CLI</span></span>

### <a name="step-1-start-coding-in-visual-studio-code-and-create-your-initial-appservice-baseline"></a><span data-ttu-id="d1e66-123">Шаг 1. Начало программирования в Visual Studio Code и создание первого приложения или базовой службы</span><span class="sxs-lookup"><span data-stu-id="d1e66-123">Step 1: Start coding in Visual Studio Code and create your initial app/service baseline</span></span>

<span data-ttu-id="d1e66-124">Разработка приложения Docker аналогична разработке приложения без Docker.</span><span class="sxs-lookup"><span data-stu-id="d1e66-124">The way you develop your application is similar to the way you do it without Docker.</span></span> <span data-ttu-id="d1e66-125">Разница заключается в том, что при разработке развертывание и тестирование приложения или служб, работающих в контейнерах Docker, выполняется в локальной среде (например, в Windows или виртуальной машине Linux).</span><span class="sxs-lookup"><span data-stu-id="d1e66-125">The difference is that while developing, you're deploying and testing your application or services running within Docker containers placed in your local environment (like a Linux VM or Windows).</span></span>

<span data-ttu-id="d1e66-126">**Настройка локальной среды**</span><span class="sxs-lookup"><span data-stu-id="d1e66-126">**Setting up your local environment**</span></span>

<span data-ttu-id="d1e66-127">С помощью последних версий Docker для Mac и Windows разрабатывать приложения Docker стало еще легче. Настройка очень проста.</span><span class="sxs-lookup"><span data-stu-id="d1e66-127">With the latest versions of Docker for Mac and Windows, it's easier than ever to develop Docker applications, and the setup is straightforward.</span></span>

> <span data-ttu-id="d1e66-128">[ИНФОРМАЦИЯ!]</span><span class="sxs-lookup"><span data-stu-id="d1e66-128">[!INFORMATION]</span></span>
>
> <span data-ttu-id="d1e66-129">Инструкции по настройке Docker для Windows см. на странице <https://docs.docker.com/docker-for-windows/>.</span><span class="sxs-lookup"><span data-stu-id="d1e66-129">For instructions on setting up Docker for Windows, go to <https://docs.docker.com/docker-for-windows/>.</span></span>
>
><span data-ttu-id="d1e66-130">Инструкции по настройке Docker для Mac см. на странице <https://docs.docker.com/docker-for-mac/>.</span><span class="sxs-lookup"><span data-stu-id="d1e66-130">For instructions on setting up Docker for Mac, go to <https://docs.docker.com/docker-for-mac/>.</span></span>

<span data-ttu-id="d1e66-131">Кроме того, вам потребуется редактор кода для разработки приложения с помощью CLI Docker.</span><span class="sxs-lookup"><span data-stu-id="d1e66-131">In addition, you'll need a code editor so that you can actually develop your application while using Docker CLI.</span></span>

<span data-ttu-id="d1e66-132">Корпорация Майкрософт предлагает Visual Studio Code, простой редактор кода, который поддерживается в Mac, Windows и Linux. Он предоставляет технологию IntelliSense с [поддержкой множества языков](https://code.visualstudio.com/docs/languages/overview) (JavaScript, .NET, Go, Java, Ruby, Python и большинства современных языков), возможность [отладки](https://code.visualstudio.com/Docs/editor/debugging), [интеграцию с Git](https://code.visualstudio.com/Docs/editor/versioncontrol) и [поддержку расширений](https://code.visualstudio.com/docs/extensions/overview).</span><span class="sxs-lookup"><span data-stu-id="d1e66-132">Microsoft provides Visual Studio Code, which is a lightweight code editor that's supported on Mac, Windows, and Linux, and provides IntelliSense with [support for many languages](https://code.visualstudio.com/docs/languages/overview) (JavaScript, .NET, Go, Java, Ruby, Python, and most modern languages), [debugging](https://code.visualstudio.com/Docs/editor/debugging), [integration with Git](https://code.visualstudio.com/Docs/editor/versioncontrol) and [extensions support](https://code.visualstudio.com/docs/extensions/overview).</span></span> <span data-ttu-id="d1e66-133">Это средство отлично подойдет разработчикам, использующим Mac и Linux.</span><span class="sxs-lookup"><span data-stu-id="d1e66-133">This editor is a great fit for Mac and Linux developers.</span></span> <span data-ttu-id="d1e66-134">В Windows можно также использовать полнофункциональную среду Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d1e66-134">In Windows, you also can use the full Visual Studio application.</span></span>

> <span data-ttu-id="d1e66-135">[ИНФОРМАЦИЯ!]</span><span class="sxs-lookup"><span data-stu-id="d1e66-135">[!INFORMATION]</span></span>
>
> <span data-ttu-id="d1e66-136">Инструкции по установке Visual Studio Code для Windows, Mac или Linux см. на странице <https://code.visualstudio.com/docs/setup/setup-overview/>.</span><span class="sxs-lookup"><span data-stu-id="d1e66-136">For instructions on installing Visual Studio Code for Windows, Mac, or Linux, go to <https://code.visualstudio.com/docs/setup/setup-overview/>.</span></span>
>
> <span data-ttu-id="d1e66-137">Инструкции по настройке Docker для Mac см. на странице <https://docs.docker.com/docker-for-mac/>.</span><span class="sxs-lookup"><span data-stu-id="d1e66-137">For instructions on setting up Docker for Mac, go to <https://docs.docker.com/docker-for-mac/>.</span></span>

<span data-ttu-id="d1e66-138">Вы можете использовать CLI Docker и писать код в любом редакторе кода, однако Visual Studio Code в сочетании с расширением Docker упрощает создание файлов `Dockerfile` и `docker-compose.yml`.</span><span class="sxs-lookup"><span data-stu-id="d1e66-138">You can work with Docker CLI and write your code using any code editor, but using Visual Studio Code with the Docker extension makes it easy to author `Dockerfile` and `docker-compose.yml` files in your workspace.</span></span> <span data-ttu-id="d1e66-139">Вы также можете выполнять задачи и скрипты из интегрированной среды разработки Visual Studio Code для выполнения команд Docker на базе CLI Docker.</span><span class="sxs-lookup"><span data-stu-id="d1e66-139">You can also run tasks and scripts from the Visual Studio Code IDE to execute Docker commands using the Docker CLI underneath.</span></span>

<span data-ttu-id="d1e66-140">Расширение Docker для VS Code предоставляет следующие возможности:</span><span class="sxs-lookup"><span data-stu-id="d1e66-140">The Docker extension for VS Code provides the following features:</span></span>

- <span data-ttu-id="d1e66-141">автоматическое создание файлов `Dockerfile` и `docker-compose.yml`;</span><span class="sxs-lookup"><span data-stu-id="d1e66-141">Automatic `Dockerfile` and `docker-compose.yml` file generation</span></span>

- <span data-ttu-id="d1e66-142">выделение синтаксических конструкций и подсказки при наведении для файлов `docker-compose.yml` и `Dockerfile`;</span><span class="sxs-lookup"><span data-stu-id="d1e66-142">Syntax highlighting and hover tips for `docker-compose.yml` and `Dockerfile` files</span></span>

- <span data-ttu-id="d1e66-143">IntelliSense (варианты завершения) для файлов `Dockerfile` и `docker-compose.yml`;</span><span class="sxs-lookup"><span data-stu-id="d1e66-143">IntelliSense (completions) for `Dockerfile` and `docker-compose.yml` files</span></span>

- <span data-ttu-id="d1e66-144">статический анализ кода (ошибки и предупреждения) для файлов `Dockerfile`;</span><span class="sxs-lookup"><span data-stu-id="d1e66-144">Linting (errors and warnings) for `Dockerfile` files</span></span>

- <span data-ttu-id="d1e66-145">интеграция палитры команд (F1) с наиболее часто используемыми командами Docker;</span><span class="sxs-lookup"><span data-stu-id="d1e66-145">Command Palette (F1) integration for the most common Docker commands</span></span>

- <span data-ttu-id="d1e66-146">интеграция в обозреватель для управления образами и контейнерами;</span><span class="sxs-lookup"><span data-stu-id="d1e66-146">Explorer integration for managing Images and Containers</span></span>

- <span data-ttu-id="d1e66-147">развертывание образов из DockerHub и реестров контейнеров Azure в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="d1e66-147">Deploy images from DockerHub and Azure Container Registries to Azure App Service</span></span>

<span data-ttu-id="d1e66-148">Чтобы установить расширение Docker, нажмите клавиши CTRL+SHIFT+P, введите `ext install` и выполните команду "Установить расширение", чтобы открыть список расширений в Marketplace.</span><span class="sxs-lookup"><span data-stu-id="d1e66-148">To install the Docker extension, press Ctrl+Shift+P, type `ext install`, and then run the Install Extension command to bring up the Marketplace extension list.</span></span> <span data-ttu-id="d1e66-149">Затем введите **docker**, чтобы отфильтровать результаты, и выберите расширение поддержки Docker, как показано на рис. 4-23.</span><span class="sxs-lookup"><span data-stu-id="d1e66-149">Next, type **docker** to filter the results, and then select the Docker Support extension, as depicted in Figure 4-23.</span></span>

![Расширение Docker для VS Code.](./media/image20.png)

<span data-ttu-id="d1e66-151">**Рис. 4-23**.</span><span class="sxs-lookup"><span data-stu-id="d1e66-151">**Figure 4-23**.</span></span> <span data-ttu-id="d1e66-152">Установка расширения Docker в Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d1e66-152">Installing the Docker Extension in Visual Studio Code</span></span>

### <a name="step-2-create-a-dockerfile-related-to-an-existing-image-plain-os-or-dev-environments-like-net-core-nodejs-and-ruby"></a><span data-ttu-id="d1e66-153">Шаг 2. Создание файла Dockerfile, связанного с существующим образом (обычная ОС или среды разработки, такие как .NET Core, Node.js и Ruby)</span><span class="sxs-lookup"><span data-stu-id="d1e66-153">Step 2: Create a DockerFile related to an existing image (plain OS or dev environments like .NET Core, Node.js, and Ruby)</span></span>

<span data-ttu-id="d1e66-154">Для каждого собираемого образа и каждого развертываемого контейнера требуется файл `DockerFile`.</span><span class="sxs-lookup"><span data-stu-id="d1e66-154">You'll need a `DockerFile` per custom image to be built and per container to be deployed.</span></span> <span data-ttu-id="d1e66-155">Если в приложении имеется только одна пользовательская служба, необходим один файл `DockerFile`.</span><span class="sxs-lookup"><span data-stu-id="d1e66-155">If your app is made up of a single custom service, you'll need a single `DockerFile`.</span></span> <span data-ttu-id="d1e66-156">Но если приложение состоит из нескольких служб (как в архитектуре на основе микрослужб), потребуется по одному файлу `Dockerfile` для каждой службы.</span><span class="sxs-lookup"><span data-stu-id="d1e66-156">But if your app is composed of multiple services (as in a microservices architecture), you'll need one `Dockerfile` per service.</span></span>

<span data-ttu-id="d1e66-157">Файл `DockerFile` обычно находится в корневой папке приложения или службы и содержит команды, которые требуются Docker для настройки и запуска приложения или службы.</span><span class="sxs-lookup"><span data-stu-id="d1e66-157">The `DockerFile` is commonly placed in the root folder of your app or service and contains the required commands so that Docker knows how to set up and run that app or service.</span></span> <span data-ttu-id="d1e66-158">Вы можете создать файл `DockerFile` самостоятельно и добавить его в проект вместе с кодом (node.js, .NET Core и т. д.) или, если у вас нет опыта работы со средой, воспользоваться приведенным ниже советом.</span><span class="sxs-lookup"><span data-stu-id="d1e66-158">You can create your `DockerFile` and add it to your project along with your code (node.js, .NET Core, etc.), or, if you're new to the environment, take a look at the following Tip.</span></span>

> [!TIP]
>
> <span data-ttu-id="d1e66-159">При использовании файлов `Dockerfile` и `docker-compose.yml`, связанных с контейнерами Docker, можно следовать указаниям, которые предоставляются расширением Docker.</span><span class="sxs-lookup"><span data-stu-id="d1e66-159">You can use the Docker extension to guide you when using the `Dockerfile` and `docker-compose.yml` files related to your Docker containers.</span></span> <span data-ttu-id="d1e66-160">Вероятно, в дальнейшем вы будете создавать эти файлы, не прибегая к помощи данного средства, но поначалу оно позволяет ускорить обучение.</span><span class="sxs-lookup"><span data-stu-id="d1e66-160">Eventually, you'll probably write these kinds of files without this tool, but using the Docker extension is a good starting point that will accelerate your learning curve.</span></span>

<span data-ttu-id="d1e66-161">На рисунке 4-24 показано, как добавляется файл docker-compose с помощью расширения Docker для VS Code.</span><span class="sxs-lookup"><span data-stu-id="d1e66-161">In Figure 4-24, you can see how a docker-compose file is added by using the Docker Extension for VS Code.</span></span>

![Консоль расширения Docker для VS Code.](./media/image24.png)

<span data-ttu-id="d1e66-163">**Рис. 4-24**.</span><span class="sxs-lookup"><span data-stu-id="d1e66-163">**Figure 4-24**.</span></span> <span data-ttu-id="d1e66-164">Добавление файлов Docker с помощью **команды добавления файлов Docker в рабочую область**</span><span class="sxs-lookup"><span data-stu-id="d1e66-164">Docker files added using the **Add Docker files to Workspace command**</span></span>

<span data-ttu-id="d1e66-165">При добавлении файла Dockerfile указывается базовый образ Docker, который необходимо использовать (например, `FROM mcr.microsoft.com/dotnet/core/aspnet`).</span><span class="sxs-lookup"><span data-stu-id="d1e66-165">When you add a DockerFile, you specify what base Docker image you’ll be using (like using `FROM mcr.microsoft.com/dotnet/core/aspnet`).</span></span> <span data-ttu-id="d1e66-166">Обычно пользовательский образ создается на основе базового образа, полученного из официального репозитория в [реестре Docker Hub](https://hub.docker.com/) (например, [образа для .NET Core](https://hub.docker.com/_/microsoft-dotnet-core/) или [Node.js](https://hub.docker.com/_/node/)).</span><span class="sxs-lookup"><span data-stu-id="d1e66-166">You'll usually build your custom image on top of a base image that you get from any official repository at the [Docker Hub registry](https://hub.docker.com/) (like an [image for .NET Core](https://hub.docker.com/_/microsoft-dotnet-core/) or the one [for Node.js](https://hub.docker.com/_/node/)).</span></span>

<span data-ttu-id="d1e66-167">***Использование существующего официального образа Docker***</span><span class="sxs-lookup"><span data-stu-id="d1e66-167">***Use an existing official Docker image***</span></span>

<span data-ttu-id="d1e66-168">Использование официального репозитория стека языка с номером версии гарантирует, что на всех компьютерах (включая компьютеры для разработки, тестирования и работы) будут доступны одни и те же функции языка.</span><span class="sxs-lookup"><span data-stu-id="d1e66-168">Using an official repository of a language stack with a version number ensures that the same language features are available on all machines (including development, testing, and production).</span></span>

<span data-ttu-id="d1e66-169">Вот пример файла Dockerfile для контейнера .NET Core:</span><span class="sxs-lookup"><span data-stu-id="d1e66-169">The following is a sample DockerFile for a .NET Core container:</span></span>

```Dockerfile
# Base Docker image to use  
FROM mcr.microsoft.com/dotnet/core/aspnet:2.2
  
# Set the Working Directory and files to be copied to the image  
ARG source  
WORKDIR /app  
COPY ${source:-bin/Release/PublishOutput} .  
  
# Configure the listening port to 80 (Internal/Secured port within Docker host)  
EXPOSE 80  
  
# Application entry point  
ENTRYPOINT ["dotnet", "MyCustomMicroservice.dll"]
```

<span data-ttu-id="d1e66-170">В этом случае образ основан на версии 2.2 официального образа Docker ASP.NET Core (мультиархитектурного, для Linux и Windows), что следует из строки `FROM mcr.microsoft.com/dotnet/core/aspnet:2.2`.</span><span class="sxs-lookup"><span data-stu-id="d1e66-170">In this case, the image is based on version 2.2 of the official ASP.NET Core Docker image (multi-arch for Linux and Windows), as per the line `FROM mcr.microsoft.com/dotnet/core/aspnet:2.2`.</span></span> <span data-ttu-id="d1e66-171">(Дополнительные сведения по этой теме см. на страницах [Образ Docker ASP.NET Core](https://hub.docker.com/_/microsoft-dotnet-core-aspnet/) и [Образ Docker .NET Core](https://hub.docker.com/_/microsoft-dotnet-core/).)</span><span class="sxs-lookup"><span data-stu-id="d1e66-171">(For more information about this topic, see the [ASP.NET Core Docker Image](https://hub.docker.com/_/microsoft-dotnet-core-aspnet/) page and the [.NET Core Docker Image](https://hub.docker.com/_/microsoft-dotnet-core/) page).</span></span>

<span data-ttu-id="d1e66-172">Кроме того, в файле Dockerfile можно указать, что средство Docker должно прослушивать порт TCP, который будет использоваться во время выполнения (например, порт 80).</span><span class="sxs-lookup"><span data-stu-id="d1e66-172">In the DockerFile, you can also instruct Docker to listen to the TCP port that you'll use at runtime (such as port 80).</span></span>

<span data-ttu-id="d1e66-173">В Dockerfile можно задать дополнительные параметры конфигурации, в зависимости от используемого языка и платформы.</span><span class="sxs-lookup"><span data-stu-id="d1e66-173">You can specify additional configuration settings in the Dockerfile, depending on the language and framework you're using.</span></span> <span data-ttu-id="d1e66-174">Например, строка `ENTRYPOINT` со значением `["dotnet", "MySingleContainerWebApp.dll"]` указывает Docker запускать приложение .NET Core.</span><span class="sxs-lookup"><span data-stu-id="d1e66-174">For instance, the `ENTRYPOINT` line with `["dotnet", "MySingleContainerWebApp.dll"]` tells Docker to run a .NET Core application.</span></span> <span data-ttu-id="d1e66-175">Если для создания и запуска приложения .NET используется пакет SDK и .NET Core CLI (`dotnet CLI`), этот параметр будет другим.</span><span class="sxs-lookup"><span data-stu-id="d1e66-175">If you're using the SDK and the .NET Core CLI (`dotnet CLI`) to build and run the .NET application, this setting would be different.</span></span> <span data-ttu-id="d1e66-176">Ключевой момент здесь заключается в том, что строка ENTRYPOINT и другие параметры зависят от языка и платформы, выбранных для приложения.</span><span class="sxs-lookup"><span data-stu-id="d1e66-176">The key point here is that the ENTRYPOINT line and other settings depend on the language and platform you choose for your application.</span></span>

> <span data-ttu-id="d1e66-177">[ИНФОРМАЦИЯ!]</span><span class="sxs-lookup"><span data-stu-id="d1e66-177">[!INFORMATION]</span></span>
>
> <span data-ttu-id="d1e66-178">Дополнительные сведения о создании образов Docker для приложений .NET Core см. на странице <https://docs.microsoft.com/dotnet/core/docker/building-net-docker-images>.</span><span class="sxs-lookup"><span data-stu-id="d1e66-178">For more information about building Docker images for .NET Core applications, go to <https://docs.microsoft.com/dotnet/core/docker/building-net-docker-images>.</span></span>
>
> <span data-ttu-id="d1e66-179">Дополнительные сведения о создании собственных образов см. на странице <https://docs.docker.com/engine/tutorials/dockerimages/>.</span><span class="sxs-lookup"><span data-stu-id="d1e66-179">To learn more about building your own images, go to <https://docs.docker.com/engine/tutorials/dockerimages/>.</span></span>

<span data-ttu-id="d1e66-180">**Использование репозиториев мультиархитектурных образов**</span><span class="sxs-lookup"><span data-stu-id="d1e66-180">**Use multi-arch image repositories**</span></span>

<span data-ttu-id="d1e66-181">В репозитории могут содержаться варианты одного и того же образа для разных платформ, например образ Linux и образ Windows.</span><span class="sxs-lookup"><span data-stu-id="d1e66-181">A single image name in a repo can contain platform variants, such as a Linux image and a Windows image.</span></span> <span data-ttu-id="d1e66-182">Это позволяет поставщикам, таким как Майкрософт, которые создают базовые образы, создать один репозиторий для охвата нескольких платформ (т. е. Windows и Linux).</span><span class="sxs-lookup"><span data-stu-id="d1e66-182">This feature allows vendors like Microsoft (base image creators) to create a single repo to cover multiple platforms (that is, Linux and Windows).</span></span> <span data-ttu-id="d1e66-183">Например, репозиторий [dotnet/core/aspnet](https://hub.docker.com/_/microsoft-dotnet-core-aspnet/) в реестре Docker Hub обеспечивает поддержку Linux и Windows Nano Server при использовании одного и того же имени образа.</span><span class="sxs-lookup"><span data-stu-id="d1e66-183">For example, the [dotnet/core/aspnet](https://hub.docker.com/_/microsoft-dotnet-core-aspnet/) repository available in the Docker Hub registry provides support for Linux and Windows Nano Server by using the same image name.</span></span>

<span data-ttu-id="d1e66-184">При запросе образа [dotnet/core/aspnet](https://hub.docker.com/_/microsoft-dotnet-core-aspnet/) с узла Windows извлекается вариант для Windows, а при запросе образа с тем же именем с узла Linux — вариант для Linux.</span><span class="sxs-lookup"><span data-stu-id="d1e66-184">Pulling the [dotnet/core/aspnet](https://hub.docker.com/_/microsoft-dotnet-core-aspnet/) image from a Windows host pulls the Windows variant, whereas pulling the same image name from a Linux host pulls the Linux variant.</span></span>

<span data-ttu-id="d1e66-185">***Создание базового образа с нуля***</span><span class="sxs-lookup"><span data-stu-id="d1e66-185">***Create your base image from scratch***</span></span>

<span data-ttu-id="d1e66-186">Вы можете создать собственный базовый образ Docker с нуля, как описано в этой [статье](https://docs.docker.com/engine/userguide/eng-image/baseimages/).</span><span class="sxs-lookup"><span data-stu-id="d1e66-186">You can create your own Docker base image from scratch as explained in this [article](https://docs.docker.com/engine/userguide/eng-image/baseimages/) from Docker.</span></span> <span data-ttu-id="d1e66-187">Этот сценарий, вероятно, будет не самым лучшим для тех, кто только начинает работать с Docker, но если вы хотите задать определенные биты базового образа, это можно сделать.</span><span class="sxs-lookup"><span data-stu-id="d1e66-187">This scenario is probably not the best for you if you're just starting with Docker, but if you want to set the specific bits of your own base image, you can do it.</span></span>

### <a name="step-3-create-your-custom-docker-images-embedding-your-service-in-it"></a><span data-ttu-id="d1e66-188">Шаг 3. Создание пользовательских образов Docker и внедрение в них собственных служб</span><span class="sxs-lookup"><span data-stu-id="d1e66-188">Step 3: Create your custom Docker images embedding your service in it</span></span>

<span data-ttu-id="d1e66-189">Для каждой пользовательской службы в приложении необходимо создать связанный образ.</span><span class="sxs-lookup"><span data-stu-id="d1e66-189">For each custom service that comprises your app, you'll need to create a related image.</span></span> <span data-ttu-id="d1e66-190">Если приложение состоит из одной службы или веб-приложения, достаточно одного образа.</span><span class="sxs-lookup"><span data-stu-id="d1e66-190">If your app is made up of a single service or web app, you'll need just a single image.</span></span>

> [!NOTE]
>
> <span data-ttu-id="d1e66-191">В рамках "рабочего процесса внешнего цикла DevOps" образы создаются автоматическим процессом сборки при отправке исходного кода в репозиторий Git (непрерывная интеграция), поэтому образы будут создаваться в этой глобальной среде из вашего исходного кода.</span><span class="sxs-lookup"><span data-stu-id="d1e66-191">When taking into account the "outer-loop DevOps workflow", the images will be created by an automated build process whenever you push your source code to a Git repository (Continuous Integration), so the images will be created in that global environment from your source code.</span></span>
>
> <span data-ttu-id="d1e66-192">Однако перед переходом к этому внешнему циклу необходимо убедиться в том, что приложение Docker действительно работает правильно, чтобы в систему управления версиями (Git и т. д.) не передавался неправильно работающий код.</span><span class="sxs-lookup"><span data-stu-id="d1e66-192">But before we consider going to that outer-loop route, we need to ensure that the Docker application is actually working properly so that they don't push code that might not work properly to the source control system (Git, etc.).</span></span>
>
> <span data-ttu-id="d1e66-193">Поэтому каждый разработчик должен вначале полностью выполнить внутренний цикл, чтобы провести тестирование локально, прежде чем отправлять в систему управления версиями полностью готовую функцию или изменение.</span><span class="sxs-lookup"><span data-stu-id="d1e66-193">Therefore, each developer first needs to do the entire inner-loop process to test locally and continue developing until they want to push a complete feature or change to the source control system.</span></span>

<span data-ttu-id="d1e66-194">Чтобы создать образ в локальной среде с помощью Dockerfile, можно использовать команду docker build, как показано на рисунке 4-25 (для приложений, состоящих из нескольких контейнеров или служб, можно также выполнить команду `docker-compose up --build`).</span><span class="sxs-lookup"><span data-stu-id="d1e66-194">To create an image in your local environment and using the DockerFile, you can use the docker build command, as demonstrated in Figure 4-25 (you also can run `docker-compose up --build` for applications composed by several containers/services).</span></span>

![Выходные данные команды docker-compose build с ходом скачивания образов.](./media/image25.png)

<span data-ttu-id="d1e66-196">**Рис. 4-25**.</span><span class="sxs-lookup"><span data-stu-id="d1e66-196">**Figure 4-25**.</span></span> <span data-ttu-id="d1e66-197">Выполнение команды docker build</span><span class="sxs-lookup"><span data-stu-id="d1e66-197">Running docker build</span></span>

<span data-ttu-id="d1e66-198">При необходимости вместо непосредственного выполнения команды `docker build` из папки проекта можно сначала создать развертываемую папку с нужными библиотеками .NET, выполнив команду `dotnet publish`, а затем использовать команду `docker build`.</span><span class="sxs-lookup"><span data-stu-id="d1e66-198">Optionally, instead of directly running `docker build` from the project folder, you first can generate a deployable folder with the .NET libraries needed by using the run `dotnet publish` command, and then run `docker build`.</span></span>

<span data-ttu-id="d1e66-199">В этом примере создается образ Docker с именем `cesardl/netcore-webapi-microservice-docker:first` (`:first` — это тег, например определенная версия).</span><span class="sxs-lookup"><span data-stu-id="d1e66-199">This example creates a Docker image with the name `cesardl/netcore-webapi-microservice-docker:first` (`:first` is a tag, like a specific version).</span></span> <span data-ttu-id="d1e66-200">Этот шаг можно выполнить для каждого пользовательского образа, который требуется создать для составного приложения Docker с несколькими контейнерами.</span><span class="sxs-lookup"><span data-stu-id="d1e66-200">You can take this step for each custom image you need to create for your composed Docker application with several containers.</span></span>

<span data-ttu-id="d1e66-201">Вы можете найти образы, имеющиеся в локальном репозитории (на компьютере разработки), с помощью команды docker images, как показано на рисунке 4-26.</span><span class="sxs-lookup"><span data-stu-id="d1e66-201">You can find the existing images in your local repository (your development machine) by using the docker images command, as illustrated in Figure 4-26.</span></span>

![Выходные данные команды docker images в консоли со списком существующих образов.](./media/image26.png)

<span data-ttu-id="d1e66-203">**Рис. 4-26**.</span><span class="sxs-lookup"><span data-stu-id="d1e66-203">**Figure 4-26**.</span></span> <span data-ttu-id="d1e66-204">Просмотр существующих образов с помощью команды docker images</span><span class="sxs-lookup"><span data-stu-id="d1e66-204">Viewing existing images using docker images</span></span>

### <a name="step-4-define-your-services-in-docker-composeyml-when-building-a-composed-docker-app-with-multiple-services"></a><span data-ttu-id="d1e66-205">Шаг 4. Определение служб в файле docker-compose.yml при сборке составного приложения Docker с несколькими службами</span><span class="sxs-lookup"><span data-stu-id="d1e66-205">Step 4: Define your services in docker-compose.yml when building a composed Docker app with multiple services</span></span>

<span data-ttu-id="d1e66-206">В файле `docker-compose.yml` можно задать ряд связанных служб для развертывания в качестве составного приложения с помощью команд развертывания, описанных в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="d1e66-206">With the `docker-compose.yml` file, you can define a set of related services to be deployed as a composed application with the deployment commands explained in the next step section.</span></span>

<span data-ttu-id="d1e66-207">Создайте этот файл в основной или корневой папке решения. Его содержимое должно быть аналогично приведенному в следующем файле `docker-compose.yml`:</span><span class="sxs-lookup"><span data-stu-id="d1e66-207">Create that file in your main or root solution folder; it should have content similar to that shown in this `docker-compose.yml` file:</span></span>

```yml
version: '3.4'
services:
  web:
    build: .
    ports:
     - "81:80"
    volumes:
     - .:/code
    depends_on:
     - redis
  redis:
    image: redis
```

<span data-ttu-id="d1e66-208">В данном случае в этом файле определены две службы: веб-служба (ваша пользовательская служба) и служба Redis (популярная служба кэширования).</span><span class="sxs-lookup"><span data-stu-id="d1e66-208">In this particular case, this file defines two services: the web service (your custom service) and the redis service (a popular cache service).</span></span> <span data-ttu-id="d1e66-209">Каждая служба развертывается как контейнер, поэтому для каждой из них требуется определенный образ Docker.</span><span class="sxs-lookup"><span data-stu-id="d1e66-209">Each service will be deployed as a container, so we need to use a concrete Docker image for each.</span></span> <span data-ttu-id="d1e66-210">Для данной веб-службы образ должен выполнять следующие действия:</span><span class="sxs-lookup"><span data-stu-id="d1e66-210">For this particular web service, the image will need to do the following:</span></span>

- <span data-ttu-id="d1e66-211">выполнять сборку из файла Dockerfile в текущем каталоге;</span><span class="sxs-lookup"><span data-stu-id="d1e66-211">Build from the DockerFile in the current directory</span></span>

- <span data-ttu-id="d1e66-212">переадресовывать предоставленный порт 80 в контейнере на внешний порт 81 на компьютере узла;</span><span class="sxs-lookup"><span data-stu-id="d1e66-212">Forward the exposed port 80 on the container to port 81 on the host machine</span></span>

- <span data-ttu-id="d1e66-213">подключать каталог проекта в узле к /code в контейнере, чтобы код можно было изменять, не перестраивая образ;</span><span class="sxs-lookup"><span data-stu-id="d1e66-213">Mount the project directory on the host to /code within the container, making it possible for you to modify the code without having to rebuild the image</span></span>

- <span data-ttu-id="d1e66-214">связывать веб-службу со службой Redis.</span><span class="sxs-lookup"><span data-stu-id="d1e66-214">Link the web service to the redis service</span></span>

<span data-ttu-id="d1e66-215">Служба Redis использует [последний общедоступный образ Redis](https://hub.docker.com/_/redis/), извлеченный из реестра Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="d1e66-215">The redis service uses the [latest public redis image](https://hub.docker.com/_/redis/) pulled from the Docker Hub registry.</span></span> <span data-ttu-id="d1e66-216">[Redis](https://redis.io/) — это популярная система кэширования для серверных приложений.</span><span class="sxs-lookup"><span data-stu-id="d1e66-216">[redis](https://redis.io/) is a popular cache system for server-side applications.</span></span>

### <a name="step-5-build-and-run-your-docker-app"></a><span data-ttu-id="d1e66-217">Шаг 5. Сборка и запуск приложения Docker</span><span class="sxs-lookup"><span data-stu-id="d1e66-217">Step 5: Build and run your Docker app</span></span>

<span data-ttu-id="d1e66-218">Если в приложении имеется только один контейнер, его можно запустить путем развертывания на узле Docker (в виртуальной машине или на физическом сервере).</span><span class="sxs-lookup"><span data-stu-id="d1e66-218">If your app has only a single container, you just need to run it by deploying it to your Docker Host (VM or physical server).</span></span> <span data-ttu-id="d1e66-219">Однако если приложение состоит из нескольких служб, его также необходимо *скомпоновать*.</span><span class="sxs-lookup"><span data-stu-id="d1e66-219">However, if your app is made up of multiple services, you need to *compose it*, too.</span></span> <span data-ttu-id="d1e66-220">Давайте рассмотрим разные варианты.</span><span class="sxs-lookup"><span data-stu-id="d1e66-220">Let's see the different options.</span></span>

<span data-ttu-id="d1e66-221">***Вариант А. Запуск одного контейнера или службы***</span><span class="sxs-lookup"><span data-stu-id="d1e66-221">***Option A: Run a single container or service***</span></span>

<span data-ttu-id="d1e66-222">Образ Docker можно запустить с помощью команды docker run, как показано в этом примере:</span><span class="sxs-lookup"><span data-stu-id="d1e66-222">You can run the Docker image by using the docker run command, as shown here:</span></span>

```console
docker run -t -d -p 80:5000 cesardl/netcore-webapi-microservice-docker:first
```

<span data-ttu-id="d1e66-223">В случае с этим развертыванием запросы, отправляемые на порт 80, перенаправляются на внутренний порт 5000.</span><span class="sxs-lookup"><span data-stu-id="d1e66-223">For this particular deployment, we'll be redirecting requests sent to port 80 to the internal port 5000.</span></span> <span data-ttu-id="d1e66-224">Теперь приложение ожидает передачи данных через внешний порт 80 на уровне узла.</span><span class="sxs-lookup"><span data-stu-id="d1e66-224">Now the application is listening on the external port 80 at the host level.</span></span>

<span data-ttu-id="d1e66-225">***Вариант Б. Компоновка и запуск многоконтейнерного приложения***</span><span class="sxs-lookup"><span data-stu-id="d1e66-225">***Option B: Compose and run a multiple-container application***</span></span>

<span data-ttu-id="d1e66-226">В большинстве корпоративных сценариев приложение Docker будет состоять из нескольких служб.</span><span class="sxs-lookup"><span data-stu-id="d1e66-226">In most enterprise scenarios, a Docker application will be composed of multiple services.</span></span> <span data-ttu-id="d1e66-227">В таких случаях можно выполнить команду `docker-compose up` (рис. 4-27), которая использует ранее созданный файл docker-compose.yml.</span><span class="sxs-lookup"><span data-stu-id="d1e66-227">For these cases, you can run the `docker-compose up` command (Figure 4-27), which will use the docker-compose.yml file that you might have created previously.</span></span> <span data-ttu-id="d1e66-228">В результате ее выполнения развертывается скомпонованное приложение со всеми связанными контейнерами.</span><span class="sxs-lookup"><span data-stu-id="d1e66-228">Running this command deploys a composed application with all of its related containers.</span></span>

![Выходные данные команды docker-compose up в консоли.](./media/image27.png)

<span data-ttu-id="d1e66-230">**Рис. 4-27**.</span><span class="sxs-lookup"><span data-stu-id="d1e66-230">**Figure 4-27**.</span></span> <span data-ttu-id="d1e66-231">Результаты выполнения команды docker-compose up</span><span class="sxs-lookup"><span data-stu-id="d1e66-231">Results of running the "docker-compose up" command</span></span>

<span data-ttu-id="d1e66-232">После выполнения команды `docker-compose up` приложение и связанные с ним контейнеры развертываются в узле Docker, как показано в представлении виртуальной машины на рисунке 4-28.</span><span class="sxs-lookup"><span data-stu-id="d1e66-232">After you run `docker-compose up`, you deploy your application and its related container(s) into your Docker Host, as illustrated in Figure 4-28, in the VM representation.</span></span>

![Виртуальная машина, в которой выполняются многоконтейнерные приложения.](./media/image28.png)

<span data-ttu-id="d1e66-234">**Рис. 4-28**.</span><span class="sxs-lookup"><span data-stu-id="d1e66-234">**Figure 4-28**.</span></span> <span data-ttu-id="d1e66-235">Виртуальная машина с развернутыми контейнерами Docker</span><span class="sxs-lookup"><span data-stu-id="d1e66-235">VM with Docker containers deployed</span></span>

### <a name="step-6-test-your-docker-application-locally-in-your-local-cd-vm"></a><span data-ttu-id="d1e66-236">Шаг 6. Тестирование приложения Docker (в локальной виртуальной машине для непрерывного развертывания)</span><span class="sxs-lookup"><span data-stu-id="d1e66-236">Step 6: Test your Docker application (locally, in your local CD VM)</span></span>

<span data-ttu-id="d1e66-237">Этот шаг будет зависеть от того, что делает ваше приложение.</span><span class="sxs-lookup"><span data-stu-id="d1e66-237">This step will vary depending on what your app is doing.</span></span>

<span data-ttu-id="d1e66-238">В случае простого веб-API Hello World на основе .NET Core, развернутого в виде единственного контейнера или службы, для доступа к службе достаточно указать TCP-порт из файла Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="d1e66-238">In a simple .NET Core Web API "Hello World" deployed as a single container or service, you'd just need to access the service by providing the TCP port specified in the DockerFile.</span></span>

<span data-ttu-id="d1e66-239">Если localhost не включен, то для перехода к службе определите IP-адрес компьютера с помощью следующей команды:</span><span class="sxs-lookup"><span data-stu-id="d1e66-239">If localhost is not turned on, to navigate to your service, find the IP address for the machine by using this command:</span></span>

```console
docker-machine {IP} {YOUR-CONTAINER-NAME}
```

<span data-ttu-id="d1e66-240">В узле Docker откройте браузер и перейдите на этот сайт. Вы должны увидеть работающее приложение или службу, как показано на рисунке 4-29.</span><span class="sxs-lookup"><span data-stu-id="d1e66-240">On the Docker host, open a browser and navigate to that site; you should see your app/service running, as demonstrated in Figure 4-29.</span></span>

![Ответ от localhost/API/values в браузере.](./media/image29.png)

<span data-ttu-id="d1e66-242">**Рис. 4-29**.</span><span class="sxs-lookup"><span data-stu-id="d1e66-242">**Figure 4-29**.</span></span> <span data-ttu-id="d1e66-243">Локальное тестирование приложения Docker с помощью localhost</span><span class="sxs-lookup"><span data-stu-id="d1e66-243">Testing your Docker application locally using localhost</span></span>

<span data-ttu-id="d1e66-244">Обратите внимание на то, что используется порт 80, однако внутренние запросы перенаправляются на порт 5000, поскольку именно так было выполнено развертывание с помощью команды `docker run`, как описывалось ранее.</span><span class="sxs-lookup"><span data-stu-id="d1e66-244">Note that it's using port 80, but internally it's being redirected to port 5000, because that's how it was deployed with `docker run`, as explained earlier.</span></span>

<span data-ttu-id="d1e66-245">Вы можете выполнить тестирование с помощью команды CURL из терминала.</span><span class="sxs-lookup"><span data-stu-id="d1e66-245">You can test this by using CURL from the terminal.</span></span> <span data-ttu-id="d1e66-246">Для Docker в Windows IP-адресом по умолчанию является 10.0.75.1, как показано на рисунке 4-30.</span><span class="sxs-lookup"><span data-stu-id="d1e66-246">In a Docker installation on Windows, the default IP is 10.0.75.1, as depicted in Figure 4-30.</span></span>

![Выходные данные команды curl, выполненной применительно к http://10.0.75.1/API/values, в консоли](./media/image30.png)

<span data-ttu-id="d1e66-248">**Рис. 4-30**.</span><span class="sxs-lookup"><span data-stu-id="d1e66-248">**Figure 4-30**.</span></span> <span data-ttu-id="d1e66-249">Локальное тестирование приложения Docker с помощью CURL</span><span class="sxs-lookup"><span data-stu-id="d1e66-249">Testing a Docker application locally by using CURL</span></span>

<span data-ttu-id="d1e66-250">**Отладка контейнера, запущенного в Docker**</span><span class="sxs-lookup"><span data-stu-id="d1e66-250">**Debugging a container running on Docker**</span></span>

<span data-ttu-id="d1e66-251">Visual Studio Code поддерживает отладку Docker при использовании Node.js и других платформ, таких как контейнеры .NET Core.</span><span class="sxs-lookup"><span data-stu-id="d1e66-251">Visual Studio Code supports debugging Docker if you're using Node.js and other platforms like .NET Core containers.</span></span>

<span data-ttu-id="d1e66-252">Вы также можете отлаживать контейнеры .NET Core или .NET Framework в Docker с помощью Visual Studio для Windows или Mac, как описано в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="d1e66-252">You also can debug .NET Core or .NET Framework containers in Docker when using Visual Studio for Windows or Mac, as described in the next section.</span></span>

> <span data-ttu-id="d1e66-253">[ИНФОРМАЦИЯ!]</span><span class="sxs-lookup"><span data-stu-id="d1e66-253">[!INFORMATION]</span></span>
>
> <span data-ttu-id="d1e66-254">Дополнительные сведения об отладке контейнеров Docker Node.js см. на страницах <https://blog.docker.com/2016/07/live-debugging-docker/> и <https://blogs.msdn.microsoft.com/user_ed/2016/02/27/visual-studio-code-new-features-13-big-debugging-updates-rich-object-hover-conditional-breakpoints-node-js-mono-more/>.</span><span class="sxs-lookup"><span data-stu-id="d1e66-254">To learn more about debugging Node.js Docker containers, go to <https://blog.docker.com/2016/07/live-debugging-docker/> and <https://blogs.msdn.microsoft.com/user_ed/2016/02/27/visual-studio-code-new-features-13-big-debugging-updates-rich-object-hover-conditional-breakpoints-node-js-mono-more/>.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="d1e66-255">[Назад](docker-apps-development-environment.md)
>[Вперед](visual-studio-tools-for-docker.md)</span><span class="sxs-lookup"><span data-stu-id="d1e66-255">[Previous](docker-apps-development-environment.md)
[Next](visual-studio-tools-for-docker.md)</span></span>