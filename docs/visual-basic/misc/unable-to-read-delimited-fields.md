---
title: Не удается прочитать поля с разделителями, поскольку двойные кавычки не являются допустимым разделителем, если параметр EscapeQuotes имеет значение True
ms.date: 07/20/2015
f1_keywords:
- vbrTextFieldParser_IllegalDelimiter
ms.assetid: ab8a0c3a-b89c-4617-9e31-7e81f5dca433
ms.openlocfilehash: 29682edb78b848897ac2999f2d84dc1a9c1a3367
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2020
ms.locfileid: "91100363"
---
# <a name="unable-to-read-delimited-fields-because-a-double-quote-is-not-a-legal-delimiter-when-escapequotes-is-set-to-true"></a>Не удается прочитать поля с разделителями, поскольку двойные кавычки не являются допустимым разделителем, если параметр EscapeQuotes имеет значение True

`TextFieldParser` не может читать из файла, поскольку в качестве разделителя указан знак кавычек ("), а `EscapeQuotes` имеет значение `True`.  
  
## <a name="to-correct-this-error"></a>Исправление ошибки  
  
- Присвойте свойству `EscapeQuotes` значение `False`.  
  
## <a name="see-also"></a>См. также

- [Метод TextFieldParser.SetDelimiters](xref:Microsoft.VisualBasic.FileIO.TextFieldParser.SetDelimiters%2A)
- [Свойство TextFieldParser.Delimiters](xref:Microsoft.VisualBasic.FileIO.TextFieldParser.Delimiters%2A)
- [Практическое руководство. Чтение из текстовых файлов с разделителями-запятыми](../developing-apps/programming/drives-directories-files/how-to-read-from-comma-delimited-text-files.md)
- [TextFieldParser Object](../language-reference/objects/textfieldparser-object.md)
