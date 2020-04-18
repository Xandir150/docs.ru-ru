---
title: Определение ресурсов XAML
description: Узнайте о ресурсах XAML в WPF для .NET Core. Поймите типы ресурсов XAML и узнайте, как определить ресурсы XAML.
author: thraka
ms.author: adegeo
ms.date: 08/21/2019
ms.openlocfilehash: b278bb92afc308578d60e347871e0150b26a95db
ms.sourcegitcommit: f8c36054eab877de4d40a705aacafa2552ce70e9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/31/2019
ms.locfileid: "81432572"
---
# <a name="overview-of-xaml-resources"></a>Обзор ресурсов XAML

Ресурс — это объект, который можно повторно использовать в разных местах вашего приложения. Примерами ресурсов являются кисти и стили. В этом обзоре описывается, как использовать ресурсы в языке разметки приложений (XAML). Вы также можете создавать и получать доступ к ресурсам с помощью кода.

> [!NOTE]
> Ресурсы XAML, описанные в этой статье, отличаются от *ресурсов приложения,* которые обычно добавляются в приложение, такие как содержимое, данные или встроенные файлы.

<!-- TODO: File redirect from docs\framework\wpf\advanced\xaml-resources.md -->

[!INCLUDE [desktop guide under construction](../../../includes/desktop-guide-preview-note.md)]

## <a name="using-resources-in-xaml"></a>Использование ресурсов в XAML

Следующий пример определяет <xref:System.Windows.Media.SolidColorBrush> ресурс на корневом элементе страницы. Затем в примере ссылается ресурс и он использует его <xref:System.Windows.Shapes.Ellipse>для <xref:System.Windows.Controls.TextBlock>набора свойств <xref:System.Windows.Controls.Button>нескольких элементов ребенка, включая a, a и a.

[!code-xaml[FEResourceSH_snip#XAML](~/samples/snippets/csharp/VS_Snippets_Wpf/FEResourceSH_snip/CS/page1.xaml#xaml)]

Каждый элемент уровня<xref:System.Windows.FrameworkElement> <xref:System.Windows.FrameworkContentElement>фреймворка (или) имеет свойство, <xref:System.Windows.FrameworkElement.Resources%2A> которое является типом, <xref:System.Windows.ResourceDictionary> содержащим определенные ресурсы. Вы можете определить ресурсы по <xref:System.Windows.Controls.Button>любому элементу, например Однако ресурсы чаще всего определяются на <xref:System.Windows.Controls.Page> корневом элементе, который находится в примере.

Каждый ресурс в словаре ресурсов должен иметь уникальный ключ. При определении ресурсов в разметке вы назначаете уникальный ключ через [директиву x:Key.](../xaml-services/xkey-directive.md) Как правило, ключ является строкой. Однако можно задать его как другой тип объекта с помощью соответствующих расширений разметки. Нестрокие для ресурсов используются определенными областями функций в WPF, в частности для стилей, ресурсов компонентов и укладки данных.

Можно использовать определенный ресурс с синтаксисом расширения разметки ресурсов, определяющим ключевое имя ресурса. Например, используйте ресурс в качестве значения свойства на другом элементе.

[!code-xaml[FEResourceSH_snip#KeyNameUsage](~/samples/snippets/csharp/VS_Snippets_Wpf/FEResourceSH_snip/CS/page2.xaml#keynameusage)]

В предыдущем примере, когда загрузчик XAML <xref:System.Windows.Controls.Control.Background%2A> обрабатывает <xref:System.Windows.Controls.Button>значение `{StaticResource MyBrush}` для свойства на, логика поиска <xref:System.Windows.Controls.Button> ресурсов сначала проверяет словарь ресурсов для элемента. Если <xref:System.Windows.Controls.Button> нет определения ключа `MyBrush` ресурса (в этом примере он не делает; его коллекция ресурсов пуста), <xref:System.Windows.Controls.Button>поиск <xref:System.Windows.Controls.Page>следующий проверяет родительский элемент , который находится . Если вы определяете <xref:System.Windows.Controls.Page> ресурс на корневом элементе, <xref:System.Windows.Controls.Page> все элементы логического дерева могут получить к нему доступ. И можно повторно использовать тот же ресурс для настройки значения любого свойства, которое принимает тот же тип, который представляет ресурс. В предыдущем примере `MyBrush` один и тот же <xref:System.Windows.Controls.Control.Background%2A> ресурс <xref:System.Windows.Controls.Button>устанавливает <xref:System.Windows.Shapes.Shape.Fill%2A> два <xref:System.Windows.Shapes.Rectangle>разных свойства: и .

## <a name="static-and-dynamic-resources"></a>Статические и динамические ресурсы

Ресурс можно отосвазать как статический или динамический. Ссылки создаются с помощью либо [расширения StaticResource Markup,](../../framework/wpf/advanced/staticresource-markup-extension.md) либо [расширения DynamicResource Markup.](../../framework/wpf/advanced/dynamicresource-markup-extension.md) Расширение разметки — это функция XAML, которая позволяет указать ссылку на объект, имея процесс расширения разметки строки атрибута и вернуть объект погрузчику XAML. Для получения дополнительной информации о поведении расширения разметки смотрите [расширения разметки и WPF XAML](../../framework/wpf/advanced/markup-extensions-and-wpf-xaml.md).

При использовании расширения разметки обычно предоставляется один или несколько параметров в строке, которые обрабатываются этим конкретным расширением разметки. [Расширение StaticResource Markup](../../framework/wpf/advanced/staticresource-markup-extension.md) обрабатывает ключ, просматривая значение этого ключа во всех доступных словарях ресурсов. Обработка происходит во время нагрузки, когда процесс загрузки должен назначить значение свойства. Вместо этого [расширение DynamicResource Markup](../../framework/wpf/advanced/dynamicresource-markup-extension.md) обрабатывает ключ, создавая выражение, и это выражение остается неоцененным до тех пор, пока приложение не заработает, после чего выражение будет оценено и обеспечивает значение.

Приведенные ниже соображения могут повлиять на выбор использования ссылки на статический или динамический ресурс.

- При определении общей конструкции создания ресурсов для приложения (на странице, в приложении, в свободном XAML или в сборке только для ресурсов) следует:

- Функциональность приложения. Обновляется ли обновление ресурсов в режиме реального времени частью требований к приложению?
- Соответствующее поведение подстановки этого ссылочного типа ресурса.
- Конкретное свойство или тип ресурса и собственное поведение этих типов.

## <a name="static-resources"></a>Статические ресурсы

Ссылки на статические ресурсы лучше всего использовать в указанных ниже случаях.

- Дизайн приложения концентрирует большую часть своих ресурсов в словарях ресурсов на странице или уровне приложения. Статические ссылки ресурсов не переоцениваются на основе поведения времени выполнения, например перезагрузки страницы. Таким образом, может быть некоторое преимущество производительности, чтобы избежать большого числа динамических ссылок ресурсов, когда они не являются необходимыми на основе вашего ресурса и дизайна приложения.

- Вы устанавливаете значение свойства, которое не <xref:System.Windows.DependencyObject> на <xref:System.Windows.Freezable>или .

- Вы создаете словарь ресурсов, который будет компилирован в DLL и упакован как часть приложения или совместно между приложениями.

- Вы создаете тему для пользовательского управления и определяете ресурсы, которые используются в темах. В этом случае обычно не требуется динамическое поведение поиска ссылки ресурса; вместо этого требуется поведение ссылки статического ресурса, чтобы поиск был предсказуемым и автономным для темы. С динамической ссылкой ресурса, даже ссылка в теме остается неоцененной до времени выполнения. и есть шанс, что, когда тема применяется, некоторые местные элементы будут переопределять ключ, что ваша тема пытается ссылаться, и локальный элемент будет падать до самой темы в поиске. Если это произойдет, ваша тема не будет вести себя, как ожидалось.

- Вы используете ресурсы для установки большого количества свойств зависимостей. Свойства зависимости имеют эффективное кэширование значений, включенное системой свойств, поэтому, если вы предоставляете значение для свойства зависимости, которое может быть оценено во время загрузки, свойство зависимости не обязано проверять переоцененное выражение и может вернуть последнее эффективное значение. Этот метод может обеспечить выигрыш в производительности.

- Вы хотите изменить базовый ресурс для всех потребителей, или вы хотите сохранить отдельные экземпляры для каждого потребителя с помощью [x:Shared Attribute](../xaml-services/xshared-attribute.md).

### <a name="static-resource-lookup-behavior"></a>Поведение подстановки статического ресурса

Ниже описывается процесс поиска, который автоматически происходит, когда на статический ресурс ссылается свойство или элемент:

01. Процесс подстановки ищет запрошенный ключ в словаре ресурсов, определенном элементом, который устанавливает это свойство.

01. Затем процесс поиска пересекает логическое дерево вверх к родительскому элементу и его словарю ресурсов. Этот процесс продолжается до тех пор, пока не будет достигнут корневой элемент.

01. Ресурсы приложений проверяются. Ресурсы приложения — это те ресурсы в <xref:System.Windows.Application> словаре ресурсов, который определяется объектом для приложения WPF.

Ссылки на статический ресурс из словаря ресурсов должны указывать на ресурс, уже определенный лексически до ссылки на ресурс. Опережающие ссылки не могут быть разрешены ссылкой на статический ресурс. По этой причине спроектируйте структуру словаря ресурсов таким образом, чтобы ресурсы определялись в начале или вблизи каждого соответствующего словаря ресурсов.

Статический поиск ресурсов может распространяться на темы или на системные ресурсы, но этот поиск поддерживается только потому, что загрузчик XAML откладывает запрос. Отсрочка необходима для того, чтобы тема выполнения в момент загрузки страницы была правильно применима к приложению. Тем не менее, статические ссылки ресурсов на ключи, которые, как известно, существуют только в темах или как системные ресурсы не рекомендуется, потому что такие ссылки не переоцениваются, если тема изменена пользователем в режиме реального времени. Ссылки на динамические ресурсы более надежны при запросе темы или системных ресурсов. Исключением является случай, когда элемент темы сам запрашивает другой ресурс. Такие ссылки должны быть статическими по причинам, упомянутым выше.

Поведение исключения, если не найдена ссылка статического ресурса, изменяется. Если ресурс был задержан, то исключение возникнет во время выполнения. Если ресурс не был задержан, исключение возникнет во время загрузки.

## <a name="dynamic-resources"></a>Динамические ресурсы

Динамические ресурсы работают лучше всего, когда:

- Значение ресурса, включая системные ресурсы, или ресурсы, которые в противном случае пользовательские наборы, зависит от условий, которые не известны до времени выполнения. Например, можно создать значения сеттера, которые относятся <xref:System.Windows.SystemFonts>к <xref:System.Windows.SystemParameters>свойствам системы как подверженные, <xref:System.Windows.SystemColors>или. Эти значения являются действительно динамическими, так как они в конечном счете берутся из среды выполнения пользователя и операционной системы. Кроме того, возможны темы уровня приложения, которые могут изменяться, когда при доступе к ресурсу уровня страницы также необходимо отследить изменения.

- Вы создаете или ссылаетесь на тематические стили для пользовательского управления.

- Вы намерены настроить содержимое приложения <xref:System.Windows.ResourceDictionary> в течение всего срока службы приложения.

- Когда имеется сложная структура ресурсов с взаимозависимостями, где могут потребоваться опережающие ссылки. Статические ссылки на ресурсы не поддерживают передние ссылки, но динамические ссылки на ресурсы поддерживают их, поскольку ресурс не нуждается в оценке до времени выполнения, и поэтому передние ссылки не являются соответствующей концепцией.

- Вы ссылаетесь на ресурс, который является большим с точки зрения компиляции или рабочего набора, и ресурс может быть использован не сразу, когда страница загружается. Статические ссылки на ресурсы всегда загружаются с XAML при загрузке страницы. Тем не менее, динамическая ссылка ресурса не загружается до тех пор, пока она не будет использована.

- Вы создаете стиль, в котором значения сеттера могут исходить от других значений, на которые влияют темы или другие настройки пользователя.

- Ресурсы применяются к элементам, которые могут быть восстановлены в логическом дереве в течение жизни приложения. Изменение родительского элемента также потенциально изменяет область подстановки ресурса, так что, если необходим пересчет ресурса в новой области элемента с измененным порядком наследования, всегда следует использовать ссылку на динамический ресурс.

### <a name="dynamic-resource-lookup-behavior"></a>Поведение подстановки динамического ресурса

Поведение поиска ресурсов для динамической ссылки ресурса параллели <xref:System.Windows.FrameworkElement.FindResource%2A> поведение <xref:System.Windows.FrameworkElement.SetResourceReference%2A>поиска в коде, если вы звоните или:

01. Проверка поиска запрашиваемого ключа в словаре ресурса, определяемом элементом, который устанавливает свойство:

    - Если элемент определяет <xref:System.Windows.FrameworkElement.Style%2A> свойство, <xref:System.Windows.FrameworkElement.Style?displayProperty=fullName> элемент имеет <xref:System.Windows.Style.Resources> свой словарь проверен.

    - Если элемент определяет <xref:System.Windows.Controls.Control.Template%2A> свойство, <xref:System.Windows.FrameworkTemplate.Resources?displayProperty=fullName> проверяется словарь элемента.

01. Поиск пересекает логическое дерево вверх к родительскому элементу и его словарю ресурсов. Этот процесс продолжается до тех пор, пока не будет достигнут корневой элемент.

01. Ресурсы приложений проверяются. Ресурсы приложений являются теми ресурсами в <xref:System.Windows.Application> словаре ресурсов, которые определяются объектом для приложения WPF.

01. Словарь тематиного ресурса проверяется на активную тему. Если тема изменяется во время выполнения, значение будет повторно вычислено.

01. Проверяются системные ресурсы.

Поведение исключения (если таковое имеется) варьируется.

- Если ресурс был запрошен <xref:System.Windows.FrameworkElement.FindResource%2A> вызовом и не найден, выбрасывается исключение.

- Если ресурс был запрошен <xref:System.Windows.FrameworkElement.TryFindResource%2A> вызовом и не найден, не выбрасывается исключение, а возвращенное значение . `null` Если набор свойств не принимает, `null`то все еще возможно, что более глубокое исключение будет брошено, в зависимости от индивидуального свойства устанавливается.

- Если ресурс был запрошен динамической ссылкой ресурса в XAML и не найден, то поведение зависит от общей системы свойств. Общее поведение, как будто не операция настройки свойства произошла на уровне, на котором существует ресурс. Например, если вы попытаетесь установить фон на отдельном элементе кнопки с помощью ресурса, который не может быть оценен, то никакие результаты набора значений не могут быть получены от других участников системы свойств и приоритета значения. Например, фоновое значение может по-прежнему исходить от локально определенного стиля кнопок или от стиля темы. Для свойств, не определяемых стилями тем, эффективное значение после неудачной оценки ресурсов может исходить от значения по умолчанию в метаданных свойств.

### <a name="restrictions"></a>Ограничения

Для ссылок на динамические ресурсы есть некоторые важные ограничения. По крайней мере одно из следующих условий должно быть правдой:

- Свойство устанавливается должно быть <xref:System.Windows.FrameworkElement> свойство на или <xref:System.Windows.FrameworkContentElement>. Это свойство должно <xref:System.Windows.DependencyProperty>быть поддержано .

- Ссылка предназначена для `StyleSetter`значения в пределах .

- Имущество, устанавливаемое должно быть <xref:System.Windows.Freezable> свойством на том, <xref:System.Windows.FrameworkElement> что <xref:System.Windows.FrameworkContentElement> предоставляется <xref:System.Windows.Setter> как стоимость либо имущества, или значение.

Поскольку набор свойств должен <xref:System.Windows.DependencyProperty> <xref:System.Windows.Freezable> быть свойством или свойством, большинство изменений свойств может распространяться на uI, поскольку изменение свойства (измененная динамическая ценность ресурса) признается системой свойств. Большинство элементов управления включают логику, которая <xref:System.Windows.DependencyProperty> заставит другой макет элемента управления, если изменение и свойство может повлиять на макет. Однако не все свойства, которые имеют [расширение DynamicResource Markup,](../../framework/wpf/advanced/dynamicresource-markup-extension.md) поскольку их стоимость гарантированно обеспечивают обновления в режиме реального времени в uI. Эта функциональность по-прежнему может варьироваться в зависимости от свойства, а также в зависимости от типа, который владеет собственностью, или даже логической структуры вашего приложения.

## <a name="styles-datatemplates-and-implicit-keys"></a>Стили, данныешаблоны и неявные клавиши

Хотя все элементы в элементе <xref:System.Windows.ResourceDictionary> должны иметь ключ, это не `x:Key`означает, что все ресурсы должны иметь явный . Некоторые типы объектов поддерживают неявный ключ, если он определен как ресурс, где значение ключа связано со значением другого свойства. Этот тип ключа известен как неявный `x:Key` ключ, в то время как атрибут является явным ключом. Любой неявный ключ можно перезаписать, указав явный ключ.

Одним из важных сценариев <xref:System.Windows.Style>для ресурсов является определение . В самом <xref:System.Windows.Style> деле, почти всегда определяется как запись в словаре ресурса, потому что стили по своей сути предназначены для повторного использования. Для получения дополнительной информации о стилях, [см Стиль и Templating](styles-templates-overview.md).

С помощью неявного ключа можно создавать стили для элементов управления либо ссылаться на них. Стили темы, определяющие внешний вид элемента управления по умолчанию, используют этот неявный ключ. С точки зрения запроса, неявным <xref:System.Type> ключом является сам контроль. С точки зрения определения ресурсов, неявным ключом является <xref:System.Windows.Style.TargetType%2A> стиль. Поэтому, если вы создаете темы для пользовательского управления или создаете стили, которые взаимодействуют с <xref:System.Windows.Style>существующими стилями тем, вам не нужно указывать [директиву x:Key](../xaml-services/xkey-directive.md) для этого. А если требуется использовать стили из тем, то вообще не нужно задавать стиль. Например, работает следующее определение стиля, даже если <xref:System.Windows.Style> у ресурса нет ключа:

[!code-xaml[FEResourceSH_snip#ImplicitStyle](~/samples/snippets/csharp/VS_Snippets_Wpf/FEResourceSH_snip/CS/page2.xaml#implicitstyle)]

Этот стиль действительно имеет ключ: `typeof(System.Windows.Controls.Button)`неявный ключ . В разметке можно <xref:System.Windows.Style.TargetType%2A> указать непосредственно в качестве имени типа (или вы можете дополнительно использовать [«x:Type...»](../xaml-services/xtype-markup-extension.md) вернуть <xref:System.Type>.

С помощью механизмов стиля темы по умолчанию, используемых WPF, этот стиль применяется как стиль выполнения <xref:System.Windows.Controls.Button> страницы, даже если <xref:System.Windows.Controls.Button> сам по себе не пытается указать свое <xref:System.Windows.FrameworkElement.Style%2A> свойство или конкретную ссылку ресурса на стиль. Ваш стиль, определяемый на странице, находится ранее в последовательности поиска, чем стиль словаря темы, используя тот же ключ, что стиль словаря темы. Вы можете `<Button>Hello</Button>` просто указать в любом месте <xref:System.Windows.Style.TargetType%2A> страницы, и стиль, который вы определили `Button` с будет применяться к этой кнопке. Если вы хотите, вы все еще можете явно ключ <xref:System.Windows.Style.TargetType%2A> стиль с тем же значением типа, как для ясности в разметке, но это необязательно.

Неявные ключи для стилей <xref:System.Windows.FrameworkElement.OverridesDefaultStyle%2A> `true`не применяются на элементе управления, если это . (Также обратите <xref:System.Windows.FrameworkElement.OverridesDefaultStyle%2A> внимание, что может быть установлен как часть родного поведения для класса управления, а не явно на экземпляре элемента управления.) Кроме того, для поддержки неявных ключей для <xref:System.Windows.FrameworkElement.DefaultStyleKey%2A> сценариев производных классов элемент управления должен переопределять (все существующие элементы управления, предоставляемые в рамках WPF, включают этот переопределение). Для получения дополнительной информации о стилях, темах и дизайне управления [см.](../../framework/wpf/controls/guidelines-for-designing-stylable-controls.md)

<xref:System.Windows.DataTemplate>также имеет неявный ключ. Неявным <xref:System.Windows.DataTemplate> ключом <xref:System.Windows.DataTemplate.DataType%2A> для a является стоимость недвижимости. <xref:System.Windows.DataTemplate.DataType%2A>также может быть указано как имя типа, а не явно с помощью [«x:Type...».](../xaml-services/xtype-markup-extension.md) Для получения подробной информации [см.](../../framework/wpf/data/data-templating-overview.md)

## <a name="see-also"></a>См. также

- <xref:System.Windows.ResourceDictionary>
- [Ресурсы приложений](../../framework/wpf/advanced/optimizing-performance-application-resources.md)
- [Ресурсы и код](../../framework/wpf/advanced/resources-and-code.md)
- [Определение и ссылка ресурса](../../framework/wpf/advanced/how-to-define-and-reference-a-resource.md)
- [Обзор управления приложениями](../../framework/wpf/app-development/application-management-overview.md)
- [x:Расширение разметки типа](../xaml-services/xtype-markup-extension.md)
- [Расширение разметки StaticResource](../../framework/wpf/advanced/staticresource-markup-extension.md)
- [Расширение разметки DynamicResource](../../framework/wpf/advanced/dynamicresource-markup-extension.md)