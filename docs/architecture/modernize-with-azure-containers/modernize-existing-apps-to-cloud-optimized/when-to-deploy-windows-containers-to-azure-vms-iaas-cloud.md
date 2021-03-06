---
title: Когда следует развертывать контейнеры Windows на виртуальных машинах Azure (облачная среда IaaS)
description: Модернизация существующих приложений .NET с помощью облака Azure и контейнеров Windows | Когда следует развертывать контейнеры Windows на виртуальных машинах Azure (облачная среда IaaS)
ms.date: 12/21/2020
ms.openlocfilehash: 64ba53fa56227266ee0e61a128d18373a2dbbc93
ms.sourcegitcommit: 5d9cee27d9ffe8f5670e5f663434511e81b8ac38
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/08/2021
ms.locfileid: "98025098"
---
# <a name="when-to-deploy-windows-containers-to-azure-vms-iaas-cloud"></a>Когда следует развертывать контейнеры Windows на виртуальных машинах Azure (облачная среда IaaS)

Если ваша организация использует виртуальные машины Azure, даже если вы также пользуетесь контейнерами Windows, вы по-прежнему работаете с IaaS. Таким образом, когда вам необходимо развернуть несколько виртуальных машин в инфраструктуре с распределением нагрузки, вы будете сталкиваться с операциями с инфраструктурой, исправлениями ОС виртуальных машин и сложностью инфраструктуры для высокомасштабируемых приложений. Ниже приведены основные сценарии использования контейнеров Windows на виртуальной машине Azure.

- **Среда разработки и тестирования**. Виртуальная машина в облаке идеально подходит для разработки и тестирования в облаке. Вы можете быстро создать или остановить среду в зависимости от своих потребностей.

- **Потребности в небольшой и средней масштабируемости** . В сценариях, где вам может потребоваться всего несколько виртуальных машин для рабочей среды, вы можете управлять небольшим количеством виртуальных машин, пока вам не нужно будет перейти к более сложным средам PaaS, таким как оркестраторы.

- **Рабочая среда с существующими инструментами развертывания**. Возможно, вы переходите из локальной среды, где вы вложили средства в инструменты для реализации сложных развертываний на виртуальных машинах или серверах без операционной системы (например, Puppet или аналогичные средства). Чтобы перейти в облако с минимальными изменениями в процедурах развертывания в рабочей среде, вы можете продолжать использовать эти средства для развертывания на виртуальных машинах Azure. Однако для улучшения процесса развертывания в качестве единицы развертывания необходимо использовать контейнеры Windows.

>[!div class="step-by-step"]
>[Назад](when-to-deploy-windows-containers-in-your-on-premises-iaas-vm-infrastructure.md)
>[Вперед](when-to-deploy-windows-containers-to-azure-container-instances-ACI.md)
