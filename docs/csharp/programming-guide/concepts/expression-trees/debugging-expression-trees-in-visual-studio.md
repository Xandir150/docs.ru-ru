---
title: Отладка деревьев выражений в Visual Studio (C#)
description: Изучите свойство DebugView в Visual Studio. Узнайте, как использовать это свойство для анализа структуры и содержимого деревьев выражений.
ms.date: 07/20/2015
ms.assetid: 1369fa25-0fbd-4b92-98d0-8df79c49c27a
ms.openlocfilehash: 5dcf56f96ecdbfdc3f4cb171fdb30b96456b59c9
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2020
ms.locfileid: "91154136"
---
# <a name="debugging-expression-trees-in-visual-studio-c"></a>Отладка деревьев выражений в Visual Studio (C#)

При отладке приложений можно анализировать структуру и содержимое деревьев выражений. Чтобы получить краткий обзор структуры дерева выражения, вы можете использовать свойство `DebugView`, которое представляет деревья выражений, [используя специальный синтаксис](debugview-syntax.md). (Обратите внимание, что `DebugView` доступен только в режиме отладки.)  

![Снимок экрана: DebugView дерева выражений в отладчике VS](media/debugging-expression-trees-in-visual-studio/debugview-expression-tree.png)

Так как `DebugView` представляет собой строку, можно использовать [встроенный визуализатор текста](/visualstudio/debugger/view-strings-visualizer#open-a-string-visualizer) для его просмотра по нескольким строкам, выбрав **визуализатор текста** из меню со значком лупы рядом с меткой `DebugView`.

 ![Снимок экрана: визуализатор текста, примененный к результатам DebugView](media/debugging-expression-trees-in-visual-studio/string-visualizer-debugview.png)

Или вы можете установить и использовать [пользовательский визуализатор](/visualstudio/debugger/create-custom-visualizers-of-data) для деревьев выражений, например:

- [Выражения для чтения](https://github.com/agileobjects/ReadableExpressions) ([лицензия MIT](https://github.com/agileobjects/ReadableExpressions/blob/master/LICENSE.md), доступная в [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=vs-publisher-1232914.ReadableExpressionsVisualizers)) отображают дерево выражения в виде кода C# с темой и различными вариантами отрисовки:

  ![Снимок экрана: визуализатор доступных для чтения выражений](media/debugging-expression-trees-in-visual-studio/readable-expressions-visualizer.png)

- [Визуализатор дерева выражения](https://github.com/zspitz/ExpressionTreeVisualizer/blob/master/README.md) ([лицензия MIT](https://github.com/zspitz/ExpressionTreeVisualizer/blob/master/LICENSE)) — древовидное представление дерева выражения и его узлов:

  ![Снимок экрана: визуализатор дерева выражений](media/debugging-expression-trees-in-visual-studio/expression-tree-visualizer.png)

### <a name="to-open-a-visualizer-for-an-expression-tree"></a>Открытие визуализатора дерева выражения  
  
1. Щелкните значок лупы рядом с деревом выражения в окне **Подсказки по данным**, **Контрольные значения**, **Видимые** или **Локальные**.  

    Отображается список доступных визуализаторов:

    ![Снимок экрана: открытие визуализаторов из Visual Studio](media/debugging-expression-trees-in-visual-studio/expression-tree-visualizers.png)

2. Щелкните визуализатор, который необходимо использовать.  
  
## <a name="see-also"></a>См. также

- [Деревья выражений (C#)](./index.md)
- [Отладка в Visual Studio](/visualstudio/debugger/debugger-feature-tour)
- [Создание настраиваемых визуализаторов](/visualstudio/debugger/create-custom-visualizers-of-data)
- [Синтаксис `DebugView`](debugview-syntax.md)
