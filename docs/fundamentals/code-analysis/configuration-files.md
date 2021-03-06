---
title: Файлы конфигурации для правил анализа кода
description: Сведения о различных файлах конфигурации для настройки правил анализа кода.
ms.date: 09/24/2020
ms.topic: conceptual
no-loc:
- EditorConfig
ms.openlocfilehash: 0d64df42ffb1763afed3e883c4f043755e158489
ms.sourcegitcommit: 635a0ff775d2447a81ef7233a599b8f88b162e5d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97633992"
---
# <a name="configuration-files-for-code-analysis-rules"></a>Файлы конфигурации для правил анализа кода

Правила анализа кода имеют различные [Параметры конфигурации](configuration-options.md). Эти параметры указываются в виде пар "ключ-значение" в одном из следующих файлов конфигурации анализатора:

- [EditorConfig](#editorconfig) файл: параметры конфигурации на основе файлов или папок.
- [Глобальный файл анализерконфиг](#global-analyzerconfig) : параметры конфигурации на уровне проекта.

## EditorConfig

[EditorConfig](/visualstudio/ide/create-portable-custom-editor-options) файлы используются для предоставления **параметров, которые применяются к конкретным исходным файлам или папкам**. Параметры размещаются в разделе Заголовки разделов, чтобы найти соответствующие файлы и папки. Добавьте запись для каждого правила, которое необходимо настроить, и разместите его в соответствующем разделе расширения файла, например `[*.cs]` .

```ini
[*.cs]
<option_name> = <option_value>
```

В приведенном выше примере `[*.cs]` — это заголовок раздела editorconfig для выбора всех файлов C# с `.cs` расширением файла в текущей папке, включая вложенные папки. Последующая запись `<option_name> = <option_value>` — это параметр анализатора, который будет применяться ко всем файлам C#.

EditorConfigСоглашения о файлах можно применить к папке, проекту или всему репозиторию, поместив файл в соответствующий каталог. Эти параметры применяются при выполнении анализа во время сборки, а также при редактировании кода в Visual Studio.

Если у вас есть файл *. editorconfig* для параметров редактора, таких как размер отступа или необходимость обрезать пробелы в конце, можно поместить параметры конфигурации анализа кода в тот же файл.

> [!TIP]
> Visual Studio предоставляет шаблон элемента *. editorconfig* , который упрощает добавление одного из этих файлов в проект. Дополнительные сведения см. [в разделе Добавление EditorConfig файла в проект](/visualstudio/ide/create-portable-custom-editor-options#add-an-editorconfig-file-to-a-project).

### <a name="example"></a>Пример

Ниже приведен пример EditorConfig файла для настройки параметров и серьезности правила.

```ini
# Remove the line below if you want to inherit .editorconfig settings from higher directories
root = true

# C# files
[*.cs]

#### Core EditorConfig Options ####

# Indentation and spacing
indent_size = 4
indent_style = space
tab_width = 4

#### .NET Coding Conventions ####

# this. and Me. preferences
dotnet_style_qualification_for_method = true:warning

#### Diagnostic configuration ####

# CA1000: Do not declare static members on generic types
dotnet_diagnostic.CA1000.severity = warning
```

## <a name="global-analyzerconfig"></a>Глобальная Анализерконфиг

Начиная с пакета SDK для .NET 5 (который поддерживается в Visual Studio 2019 версии 16,8 и более поздних версиях), можно также настроить параметры анализатора с помощью глобальных файлов _анализерконфиг_ . Эти файлы используются для предоставления **параметров, которые применяются ко всем исходным файлам в проекте**, независимо от их имен или путей к файлам.

В отличие от [EditorConfig](#editorconfig) файлов глобальные файлы конфигурации нельзя использовать для настройки параметров стиля редактора для IDE, таких как размер отступа или необходимость обрезки конечных пробелов. Вместо этого они предназначены исключительно для указания параметров конфигурации анализатора уровня проекта.

### <a name="format"></a>Формат

В отличие от EditorConfig файлов, которые должны иметь заголовки разделов, например `[*.cs]` , для обнаружения подходящих файлов и папок, глобальные файлы анализерконфиг не имеют заголовков разделов. Вместо этого им требуется запись верхнего уровня формы, `is_global = true` чтобы отличать их от обычных EditorConfig файлов. Это означает, что все параметры в файле применяются ко всему проекту. Пример:

```ini
is_global = true
<option_name> = <option_value>
```

### <a name="naming"></a>Именование

В отличие от EditorConfig файлов, которые должны называться `.editorconfig` , глобальные файлы конфигурации не обязательно должны иметь определенное имя или расширение. Однако если вы назначите эти файлы как, `.globalconfig` они будут неявно применены ко всем проектам C# и Visual Basic в текущей папке, включая вложенные папки. В противном случае необходимо явно добавить `GlobalAnalyzerConfigFiles` элемент в файл проекта MSBuild:

```xml
<ItemGroup>
  <GlobalAnalyzerConfigFiles Include="<path_to_global_analyzer_config>" />
</ItemGroup>
```

> [!NOTE]
> Запись верхнего уровня требуется, `is_global = true` даже если файл называется `.globalconfig` .

### <a name="example"></a>Пример

Ниже приведен пример глобального файла Анализерконфиг для настройки параметров и серьезности правил на уровне проекта.

```ini
# Top level entry required to mark this as a global AnalyzerConfig file
is_global = true

# NOTE: No section headers for configuration entries

#### .NET Coding Conventions ####

# this. and Me. preferences
dotnet_style_qualification_for_method = true:warning

#### Diagnostic configuration ####

# CA1000: Do not declare static members on generic types
dotnet_diagnostic.CA1000.severity = warning
```

## <a name="precedence"></a>Приоритет

Оба EditorConfig файла и глобальные файлы анализерконфиг задают пару "ключ-значение" для каждого параметра. Конфликты возникают, когда имеется несколько записей с одинаковым ключом, но разными значениями.

### <a name="general-options"></a>Общие параметры

При возникновении конфликтов между параметрами для разрешения конфликтов используются следующие правила приоритета.

- Конфликтующие записи в одном файле конфигурации: запись, которая появляется позже в файле WINS. Это справедливо для конфликтующих записей в одном EditorConfig файле, а также в одном глобальном файле анализерконфиг.

- Конфликтующие записи в двух EditorConfig файлах: запись в EditorConfig файле, которая находится глубже в файловой системе и, следовательно, имеет более длинный путь к файлу, WINS.

- Конфликтующие записи в двух глобальных файлах Анализерконфиг: выводится предупреждение компилятора, и обе записи игнорируются.

- Конфликтующие записи в EditorConfig файле и глобальном файле анализерконфиг: запись в EditorConfig файле WINS.

### <a name="severity-options"></a>Параметры серьезности

Предыдущие [Общие правила приоритета](#general-options) применяются ко всем параметрам, указанным в файлах конфигурации. Для параметров [конфигурации с уровнем серьезности](configuration-options.md#severity-level) применяются следующие дополнительные правила приоритета.

- Параметры серьезности, указанные в командной строке как параметры компилятора ( `/nowarn` или `/warnaserror` ), всегда переопределяют параметры [конфигурации серьезности](configuration-options.md#severity-level) , указанные в EditorConfig и глобальные файлы анализерконфиг.

- Параметры серьезности также можно указать с помощью [RuleSet](/visualstudio/code-quality/using-rule-sets-to-group-code-analysis-rules) -файла. Однако файлы набора правил не рекомендуются в пользу EditorConfig глобальных файлов анализерконфиг. Рекомендуется [преобразовать RuleSet Files в эквивалентный EditorConfig файл](/visualstudio/code-quality/use-roslyn-analyzers#convert-an-existing-ruleset-file-to-editorconfig-file). Приоритет для конфликтующих записей серьезности из файла набора правил и EditorConfig глобальных файлов анализерконфиг не _определен_.

- Сведения о правилах приоритета для связанных параметров серьезности с различными ключами, например при указании различных уровней серьезности для одного правила и категории, к которой относится правило, см. в разделе [Параметры конфигурации для анализа кода](configuration-options.md#precedence).

## <a name="see-also"></a>См. также раздел

- [EditorConfig Общая ошибка проектирования VS Анализерконфиг](https://github.com/dotnet/roslyn/issues/47707)
