---
title: Владение данными в каждой микрослужбе
description: Владение данными в каждой микрослужбе является одним из ключевых моментов микрослужб. Каждая микрослужба должна быть единственным владельцем своей базы данных. Другие микрослужбы не должны иметь к ней доступ. Разумеется, все экземпляры микрослужбы подключаются к одной и той же базе данных высокой доступности.
ms.date: 09/20/2018
ms.openlocfilehash: ccb12451cd7cd44938e09d171eb29e614786f469
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68673181"
---
# <a name="data-sovereignty-per-microservice"></a><span data-ttu-id="eac3a-105">Владение данными в каждой микрослужбе</span><span class="sxs-lookup"><span data-stu-id="eac3a-105">Data sovereignty per microservice</span></span>

<span data-ttu-id="eac3a-106">Важное правило архитектуры микрослужб состоит в том, что каждая микрослужба должна быть владельцем своих данных и логики предметной области.</span><span class="sxs-lookup"><span data-stu-id="eac3a-106">An important rule for microservices architecture is that each microservice must own its domain data and logic.</span></span> <span data-ttu-id="eac3a-107">Так же как полнофункциональное приложение является владельцем своих логики и данных, так и каждая микрослужба должна быть владельцем своей логики и данных в рамках автономного жизненного цикла, причем развертывание производится независимо для каждой микрослужбы.</span><span class="sxs-lookup"><span data-stu-id="eac3a-107">Just as a full application owns its logic and data, so must each microservice own its logic and data under an autonomous lifecycle, with independent deployment per microservice.</span></span>

<span data-ttu-id="eac3a-108">Это означает, что концептуальная модель предметной области будет различаться для разных подсистем и микрослужб.</span><span class="sxs-lookup"><span data-stu-id="eac3a-108">This means that the conceptual model of the domain will differ between subsystems or microservices.</span></span> <span data-ttu-id="eac3a-109">Для примера можно взять корпоративное приложение, в котором каждая из подсистем управления отношениями с клиентами (CRM), финансовых транзакций и поддержки клиентов обращается к уникальным атрибутам и данным сущностей и использует собственный ограниченный контекст.</span><span class="sxs-lookup"><span data-stu-id="eac3a-109">Consider enterprise applications, where customer relationship management (CRM) applications, transactional purchase subsystems, and customer support subsystems each call on unique customer entity attributes and data, and where each employs a different Bounded Context (BC).</span></span>

<span data-ttu-id="eac3a-110">Аналогичный принцип принят в [проблемно-ориентированном проектировании (DDD)](https://en.wikipedia.org/wiki/Domain-driven_design), в котором каждый [ограниченный контекст](https://martinfowler.com/bliki/BoundedContext.html) либо автономная подсистема или служба должны быть владельцем своей модели предметной области (состоящей из данных и логики или поведения).</span><span class="sxs-lookup"><span data-stu-id="eac3a-110">This principle is similar in [Domain-driven design (DDD)](https://en.wikipedia.org/wiki/Domain-driven_design), where each [Bounded Context](https://martinfowler.com/bliki/BoundedContext.html) or autonomous subsystem or service must own its domain model (data plus logic and behavior).</span></span> <span data-ttu-id="eac3a-111">Каждый ограниченный контекст DDD сопоставлен с одной бизнес-микрослужбой (одной или несколькими службами).</span><span class="sxs-lookup"><span data-stu-id="eac3a-111">Each DDD Bounded Context correlates to one business microservice (one or several services).</span></span> <span data-ttu-id="eac3a-112">Более подробно о шаблоне ограниченного контекста мы поговорим в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="eac3a-112">This point about the Bounded Context pattern is expanded in the next section.</span></span>

<span data-ttu-id="eac3a-113">С другой стороны, традиционный подход (на основе единых данных), применяемый во многих приложениях, предполагает наличие одной централизованной базы данных или небольшого количества баз данных.</span><span class="sxs-lookup"><span data-stu-id="eac3a-113">On the other hand, the traditional (monolithic data) approach used in many applications is to have a single centralized database or just a few databases.</span></span> <span data-ttu-id="eac3a-114">Часто это нормализованная база данных SQL, которая используется для всего приложения и всех его внутренних подсистем, как показано на рис. 4-7.</span><span class="sxs-lookup"><span data-stu-id="eac3a-114">This is often a normalized SQL database that's used for the whole application and all its internal subsystems, as shown in Figure 4-7.</span></span>

![В традиционном подходе имеется отдельная база данных, совместно используемая всеми службами, как правило, в многоуровневой архитектуре.](./media/image7.png)

<span data-ttu-id="eac3a-117">**Рис. 4-7**.</span><span class="sxs-lookup"><span data-stu-id="eac3a-117">**Figure 4-7**.</span></span> <span data-ttu-id="eac3a-118">Сравнение владения данными: единая база данных и микрослужбы</span><span class="sxs-lookup"><span data-stu-id="eac3a-118">Data sovereignty comparison: monolithic database versus microservices</span></span>

<span data-ttu-id="eac3a-119">Подход на основе централизованной базы данных изначально выглядит более простым и позволяющим повторно использовать сущности в разных подсистемах для обеспечения согласованности.</span><span class="sxs-lookup"><span data-stu-id="eac3a-119">The centralized database approach initially looks simpler and seems to enable reuse of entities in different subsystems to make everything consistent.</span></span> <span data-ttu-id="eac3a-120">Но в действительности вы в итоге получаете огромные таблицы, обслуживающие множество разных подсистем и содержащие атрибуты и столбцы, которые в большинстве случаев не нужны.</span><span class="sxs-lookup"><span data-stu-id="eac3a-120">But the reality is you end up with huge tables that serve many different subsystems, and that include attributes and columns that aren't needed in most cases.</span></span> <span data-ttu-id="eac3a-121">Это все равно что пытаться использовать одну и ту же карту для небольшого пешего похода, суточной поездки на автомобиле и изучения географии.</span><span class="sxs-lookup"><span data-stu-id="eac3a-121">It's like trying to use the same physical map for hiking a short trail, taking a day-long car trip, and learning geography.</span></span>

<span data-ttu-id="eac3a-122">Монолитное приложение, обычно использующее одну реляционную базу данных, имеет два важных преимущества: [транзакции ACID](https://en.wikipedia.org/wiki/ACID) и язык SQL; они работают для всех таблиц и данных, относящихся к приложению.</span><span class="sxs-lookup"><span data-stu-id="eac3a-122">A monolithic application with typically a single relational database has two important benefits: [ACID transactions](https://en.wikipedia.org/wiki/ACID) and the SQL language, both working across all the tables and data related to your application.</span></span> <span data-ttu-id="eac3a-123">Такой подход позволяет легко писать запросы, объединяющие данные из нескольких таблиц.</span><span class="sxs-lookup"><span data-stu-id="eac3a-123">This approach provides a way to easily write a query that combines data from multiple tables.</span></span>

<span data-ttu-id="eac3a-124">При переходе на архитектуру микрослужб доступ к данным становится гораздо сложнее.</span><span class="sxs-lookup"><span data-stu-id="eac3a-124">However, data access becomes much more complex when you move to a microservices architecture.</span></span> <span data-ttu-id="eac3a-125">Однако даже когда транзакции, обладающие свойствами атомарности, согласованности, изолированности и долговечности, могут или должны использоваться в микрослужбе или ограниченном контексте, данные, принадлежащие микрослужбе, являются частными для нее, и доступ к ним возможен только посредством интерфейса API микрослужбы.</span><span class="sxs-lookup"><span data-stu-id="eac3a-125">But even when ACID transactions can or should be used within a microservice or Bounded Context, the data owned by each microservice is private to that microservice and can only be accessed via its microservice API.</span></span> <span data-ttu-id="eac3a-126">Инкапсуляция данных делает микрослужбы слабосвязанными, благодаря чему они могут изменяться независимо друг от друга.</span><span class="sxs-lookup"><span data-stu-id="eac3a-126">Encapsulating the data ensures that the microservices are loosely coupled and can evolve independently of one another.</span></span> <span data-ttu-id="eac3a-127">Если бы несколько служб обращались к одним и тем же данным, изменения схемы требовали бы согласованного изменения всех служб.</span><span class="sxs-lookup"><span data-stu-id="eac3a-127">If multiple services were accessing the same data, schema updates would require coordinated updates to all the services.</span></span> <span data-ttu-id="eac3a-128">При этом автономность жизненного цикла микрослужб нарушалась бы.</span><span class="sxs-lookup"><span data-stu-id="eac3a-128">This would break the microservice lifecycle autonomy.</span></span> <span data-ttu-id="eac3a-129">Однако распределенные структуры данных означают невозможность выполнения транзакции, обладающей свойствами атомарности, согласованности, изолированности и долговечности, в рамках нескольких микрослужб.</span><span class="sxs-lookup"><span data-stu-id="eac3a-129">But distributed data structures mean that you can't make a single ACID transaction across microservices.</span></span> <span data-ttu-id="eac3a-130">Из этого, в свою очередь, следует, что если бизнес-процесс охватывает несколько микрослужб, необходимо обеспечивать итоговую согласованность.</span><span class="sxs-lookup"><span data-stu-id="eac3a-130">This in turn means you must use eventual consistency when a business process spans multiple microservices.</span></span> <span data-ttu-id="eac3a-131">Это гораздо сложнее в реализации, чем простые соединения SQL, поскольку невозможно создать ограничения целостности или использовать распределенные транзакции между отдельными базами данных, как мы объясним позднее.</span><span class="sxs-lookup"><span data-stu-id="eac3a-131">This is much harder to implement than simple SQL joins, because you can't create integrity constraints or use distributed transactions between separate databases, as we'll explain later on.</span></span> <span data-ttu-id="eac3a-132">Аналогичным образом многие другие возможности реляционных баз данных недоступны в рамках нескольких микрослужб.</span><span class="sxs-lookup"><span data-stu-id="eac3a-132">Similarly, many other relational database features aren't available across multiple microservices.</span></span>

<span data-ttu-id="eac3a-133">Если разбираться дальше, микрослужбы часто используют базы данных разных *типов*.</span><span class="sxs-lookup"><span data-stu-id="eac3a-133">Going even further, different microservices often use different *kinds* of databases.</span></span> <span data-ttu-id="eac3a-134">Современные приложения хранят и обрабатывают разнообразные типы данных, и реляционная база данных — не всегда лучший выбор.</span><span class="sxs-lookup"><span data-stu-id="eac3a-134">Modern applications store and process diverse kinds of data, and a relational database isn't always the best choice.</span></span> <span data-ttu-id="eac3a-135">В некоторых ситуациях база данных NoSQL, например Azure CosmosDB или MongoDB, может иметь более удобную модель данных и обеспечивать более высокую производительность и масштабируемость по сравнению с базой данных SQL, например SQL Server или базой данных SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="eac3a-135">For some use cases, a NoSQL database such as Azure CosmosDB or MongoDB might have a more convenient data model and offer better performance and scalability than a SQL database like SQL Server or Azure SQL Database.</span></span> <span data-ttu-id="eac3a-136">В других случаях реляционная база данных по-прежнему является оптимальным решением.</span><span class="sxs-lookup"><span data-stu-id="eac3a-136">In other cases, a relational database is still the best approach.</span></span> <span data-ttu-id="eac3a-137">По этой причине в приложениях на основе микрослужб часто используется сочетание баз данных SQL и NoSQL. Такой подход иногда называют [разнородным хранением данных](https://martinfowler.com/bliki/PolyglotPersistence.html).</span><span class="sxs-lookup"><span data-stu-id="eac3a-137">Therefore, microservices-based applications often use a mixture of SQL and NoSQL databases, which is sometimes called the [polyglot persistence](https://martinfowler.com/bliki/PolyglotPersistence.html) approach.</span></span>

<span data-ttu-id="eac3a-138">Секционированная архитектура разнородного хранения данных имеет много преимуществ.</span><span class="sxs-lookup"><span data-stu-id="eac3a-138">A partitioned, polyglot-persistent architecture for data storage has many benefits.</span></span> <span data-ttu-id="eac3a-139">К ним относятся слабая связь между службами, более высокая производительность и масштабируемость, снижение затрат и лучшая управляемость.</span><span class="sxs-lookup"><span data-stu-id="eac3a-139">These include loosely coupled services and better performance, scalability, costs, and manageability.</span></span> <span data-ttu-id="eac3a-140">Однако может возникнуть ряд проблем с распределенным управлением данными, которые рассмотрены в разделе [Определение границ модели предметной области](identify-microservice-domain-model-boundaries.md) далее в этой главе.</span><span class="sxs-lookup"><span data-stu-id="eac3a-140">However, it can introduce some distributed data management challenges, as explained in "[Identifying domain-model boundaries](identify-microservice-domain-model-boundaries.md)" later in this chapter.</span></span>

## <a name="the-relationship-between-microservices-and-the-bounded-context-pattern"></a><span data-ttu-id="eac3a-141">Связь между микрослужбами и шаблоном ограниченного контекста</span><span class="sxs-lookup"><span data-stu-id="eac3a-141">The relationship between microservices and the Bounded Context pattern</span></span>

<span data-ttu-id="eac3a-142">Концепция микрослужбы является производной от [шаблона ограниченного контекста](https://martinfowler.com/bliki/BoundedContext.html) в [проблемно-ориентированном проектировании (DDD)](https://en.wikipedia.org/wiki/Domain-driven_design).</span><span class="sxs-lookup"><span data-stu-id="eac3a-142">The concept of microservice derives from the [Bounded Context (BC) pattern](https://martinfowler.com/bliki/BoundedContext.html) in [domain-driven design (DDD)](https://en.wikipedia.org/wiki/Domain-driven_design).</span></span> <span data-ttu-id="eac3a-143">В DDD большие модели разделяются на несколько ограниченных контекстов с явно определенными границами между ними.</span><span class="sxs-lookup"><span data-stu-id="eac3a-143">DDD deals with large models by dividing them into multiple BCs and being explicit about their boundaries.</span></span> <span data-ttu-id="eac3a-144">Каждый ограниченный контекст должен иметь собственную модель и базу данных, так же как каждая микрослужба является владельцем связанных с ней данных.</span><span class="sxs-lookup"><span data-stu-id="eac3a-144">Each BC must have its own model and database; likewise, each microservice owns its related data.</span></span> <span data-ttu-id="eac3a-145">Кроме того, каждый ограниченный контекст обычно имеет собственный [единый язык](https://martinfowler.com/bliki/UbiquitousLanguage.html), упрощающий взаимодействие между разработчиками ПО и экспертами в предметных областях.</span><span class="sxs-lookup"><span data-stu-id="eac3a-145">In addition, each BC usually has its own [ubiquitous language](https://martinfowler.com/bliki/UbiquitousLanguage.html) to help communication between software developers and domain experts.</span></span>

<span data-ttu-id="eac3a-146">Термины единого языка (в основном это сущности предметной области) могут иметь разные имена в разных ограниченных контекстах, даже если разные сущности предметной области имеют одинаковый уникальный идентификатор (по которому сущность считывается из хранилища).</span><span class="sxs-lookup"><span data-stu-id="eac3a-146">Those terms (mainly domain entities) in the ubiquitous language can have different names in different Bounded Contexts, even when different domain entities share the same identity (that is, the unique ID that's used to read the entity from storage).</span></span> <span data-ttu-id="eac3a-147">Например, в ограниченном контексте профиля пользователя сущность предметной области "Пользователь" может иметь тот же идентификатор, что и сущность предметной области "Покупатель" в ограниченном контексте размещения заказов.</span><span class="sxs-lookup"><span data-stu-id="eac3a-147">For instance, in a user-profile Bounded Context, the User domain entity might share identity with the Buyer domain entity in the ordering Bounded Context.</span></span>

<span data-ttu-id="eac3a-148">Таким образом, микрослужба похожа на ограниченный контекст, но она также определена как распределенная служба.</span><span class="sxs-lookup"><span data-stu-id="eac3a-148">A microservice is therefore like a Bounded Context, but it also specifies that it's a distributed service.</span></span> <span data-ttu-id="eac3a-149">Она создается как отдельный процесс для каждого ограниченного контекста и должна использовать упомянутые ранее распределенные протоколы, такие как HTTP/HTTPS, WebSockets или [AMQP](https://en.wikipedia.org/wiki/Advanced_Message_Queuing_Protocol).</span><span class="sxs-lookup"><span data-stu-id="eac3a-149">It's built as a separate process for each Bounded Context, and it must use the distributed protocols noted earlier, like HTTP/HTTPS, WebSockets, or [AMQP](https://en.wikipedia.org/wiki/Advanced_Message_Queuing_Protocol).</span></span> <span data-ttu-id="eac3a-150">В свою очередь, шаблон ограниченного контекста не указывает, является ли ограниченный контекст распределенной службой или просто логической границей (например, подсистемой общего назначения) в пределах монолитного приложения.</span><span class="sxs-lookup"><span data-stu-id="eac3a-150">The Bounded Context pattern, however, doesn't specify whether the Bounded Context is a distributed service or if it's simply a logical boundary (such as a generic subsystem) within a monolithic-deployment application.</span></span>

<span data-ttu-id="eac3a-151">Важно отметить, что сначала желательно определить службу для каждого ограниченного контекста.</span><span class="sxs-lookup"><span data-stu-id="eac3a-151">It's important to highlight that defining a service for each Bounded Context is a good place to start.</span></span> <span data-ttu-id="eac3a-152">Однако проектирование необязательно ограничивать этим.</span><span class="sxs-lookup"><span data-stu-id="eac3a-152">But you don't have to constrain your design to it.</span></span> <span data-ttu-id="eac3a-153">Иногда следует спроектировать ограниченный контекст или бизнес-микрослужбу, которые состоят из нескольких физических служб.</span><span class="sxs-lookup"><span data-stu-id="eac3a-153">Sometimes you must design a Bounded Context or business microservice composed of several physical services.</span></span> <span data-ttu-id="eac3a-154">Но, в конечном счете, оба шаблона (ограниченный контекст и микрослужба) тесно связны друг с другом.</span><span class="sxs-lookup"><span data-stu-id="eac3a-154">But ultimately, both patterns -Bounded Context and microservice- are closely related.</span></span>

<span data-ttu-id="eac3a-155">В DDD преимущества микрослужб реализуются путем определения реальных границ в форме распределенных микрослужб.</span><span class="sxs-lookup"><span data-stu-id="eac3a-155">DDD benefits from microservices by getting real boundaries in the form of distributed microservices.</span></span> <span data-ttu-id="eac3a-156">Однако в ограниченном контексте также могут требоваться такие возможности, как отсутствие общей модели для микрослужб.</span><span class="sxs-lookup"><span data-stu-id="eac3a-156">But ideas like not sharing the model between microservices are what you also want in a Bounded Context.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="eac3a-157">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="eac3a-157">Additional resources</span></span>

- <span data-ttu-id="eac3a-158">**Крис Ричардсон (Chris Richardson). Шаблон: база данных для каждой службы** </span><span class="sxs-lookup"><span data-stu-id="eac3a-158">**Chris Richardson. Pattern: Database per service** </span></span>\
  <https://microservices.io/patterns/data/database-per-service.html>

- <span data-ttu-id="eac3a-159">**Мартин Фоулер (Martin Fowler). BoundedContext** </span><span class="sxs-lookup"><span data-stu-id="eac3a-159">**Martin Fowler. BoundedContext** </span></span>\
  <https://martinfowler.com/bliki/BoundedContext.html>

- <span data-ttu-id="eac3a-160">**Мартин Фоулер (Martin Fowler). PolyglotPersistence** </span><span class="sxs-lookup"><span data-stu-id="eac3a-160">**Martin Fowler. PolyglotPersistence** </span></span>\
  <https://martinfowler.com/bliki/PolyglotPersistence.html>

- <span data-ttu-id="eac3a-161">**Альберто Брандолини (Alberto Brandolini). Стратегическое предметно-ориентированное проектирование с сопоставлением контекста** </span><span class="sxs-lookup"><span data-stu-id="eac3a-161">**Alberto Brandolini. Strategic Domain Driven Design with Context Mapping** </span></span>\
  <https://www.infoq.com/articles/ddd-contextmapping>

>[!div class="step-by-step"]
><span data-ttu-id="eac3a-162">[Назад](microservices-architecture.md)
>[Вперед](logical-versus-physical-architecture.md)</span><span class="sxs-lookup"><span data-stu-id="eac3a-162">[Previous](microservices-architecture.md)
[Next](logical-versus-physical-architecture.md)</span></span>