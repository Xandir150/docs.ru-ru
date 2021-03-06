---
title: Установка F#
description: 'Узнайте, как установить F # различными способами.'
ms.date: 12/20/2019
ms.openlocfilehash: 302e04f7cf3271516dff88d9d5f18f620b6ede80
ms.sourcegitcommit: 67ebdb695fd017d79d9f1f7f35d145042d5a37f7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/20/2020
ms.locfileid: "92224283"
---
# <a name="install-f"></a>Установка F\#

F # можно установить несколькими способами в зависимости от среды.

## <a name="install-f-with-visual-studio"></a>Установка F # с помощью Visual Studio

1. Если вы скачиваете [Visual Studio](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) впервые, сначала будет установлена Visual Studio Installer. Установите соответствующий выпуск Visual Studio из установщика.

   Если у вас уже установлена среда Visual Studio, выберите пункт **изменить** рядом с выпуском, в который нужно добавить F #.

2. На странице рабочие нагрузки выберите рабочую нагрузку **ASP.NET and Web Development** , которая включает в себя поддержку F # и .NET Core для проектов ASP.NET Core.

3. Выберите **изменить** в правом нижнем углу, чтобы установить все, что вы выбрали.

   Затем можно открыть Visual Studio с помощью F #, выбрав **запустить** в Visual Studio Installer.

## <a name="install-f-with-visual-studio-code"></a>Установка F # с Visual Studio Code

1. Убедитесь, что на вашем пути установлен и доступен [Git](https://git-scm.com/download) . Чтобы проверить правильность установки, введите `git --version` в командной строке и нажмите клавишу **Ввод**.

2. Установите [пакет SDK для .NET Core](https://dotnet.microsoft.com/download) и [Visual Studio Code](https://code.visualstudio.com).

3. Щелкните значок расширения и выполните поиск по запросу "Ionide":

   Единственным подключаемым модулем, необходимым для поддержки F # в Visual Studio Code, является [Ionide-FSharp](https://marketplace.visualstudio.com/items?itemName=Ionide.Ionide-fsharp). Однако можно также установить [Ionide-ПОДдельные](https://marketplace.visualstudio.com/items?itemName=Ionide.Ionide-FAKE) , чтобы получить [поддельную](https://fake.build/) поддержку и [Ionide-пакет](https://marketplace.visualstudio.com/items?itemName=Ionide.Ionide-Paket) , чтобы получить поддержку [пакет](https://fsprojects.github.io/Paket/) . Поддельные и пакет — это дополнительные инструменты сообщества F # для создания проектов и управления зависимостями соответственно.

## <a name="install-f-with-visual-studio-for-mac"></a>Установка F # с Visual Studio для Mac

F # устанавливается по умолчанию в [Visual Studio для Mac](https://visualstudio.microsoft.com/vs/mac/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link), независимо от выбранной конфигурации.

После завершения установки нажмите кнопку **запустить Visual Studio**. Вы также можете открыть Visual Studio с помощью Finder в macOS.

## <a name="install-f-on-a-build-server"></a>Установка F # на сервере сборки

Если вы используете .NET Core или .NET Framework с помощью пакета SDK для .NET, необходимо просто установить пакет SDK для .NET на сервере сборки. Он содержит все необходимое.

Если вы используете .NET Framework и вы **не** используете пакет SDK для .NET, необходимо установить [Visual Studio Build Tools SKU](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=BuildTools&rel=16) на сервере Windows Server. В установщике выберите **.NET Desktop Build Tools**, а затем выберите компонент **компилятора F #** в правой части меню установщика.
