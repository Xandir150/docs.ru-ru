---
title: Функция Dir должна первый раз вызываться с аргументом PathName
ms.date: 07/20/2015
f1_keywords:
- vbrDIR_IllegalCall
ms.assetid: 7b5d149f-be91-4ac3-8262-86a360894e7d
ms.openlocfilehash: 7f8191b0ef5452fcf6f3a20c3e0f28ca84ef9c4a
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/17/2020
ms.locfileid: "92163432"
---
# <a name="dir-function-must-first-be-called-with-a-pathname-argument"></a>Функция Dir должна первый раз вызываться с аргументом PathName

Начальный вызов `Dir` функции не включает `PathName` аргумент. Первый вызов `Dir` должен включать `PathName` , но последующие вызовы `Dir` не обязательно должны включать параметры для получения следующего элемента.

## <a name="to-correct-this-error"></a>Исправление ошибки

- Укажите `PathName` аргумент в вызове функции.

## <a name="see-also"></a>См. также

- <xref:Microsoft.VisualBasic.FileSystem.Dir%2A>
