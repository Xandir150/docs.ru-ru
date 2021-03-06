---
title: Практическое руководство. Предоставление контрактов SOAP- и веб-клиентам
description: Сведения о том, как сделать конечную точку сервера WFC доступной для клиентов SOAP и не-SOAP. По умолчанию конечные точки доступны только для клиентов SOAP.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: bb765a48-12f2-430d-a54d-6f0c20f2a23a
ms.openlocfilehash: b1bdb7af51e0e2795c36865058fbeb34a716e3e2
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246978"
---
# <a name="how-to-expose-a-contract-to-soap-and-web-clients"></a>Практическое руководство. Предоставление контрактов SOAP- и веб-клиентам

По умолчанию Windows Communication Foundation (WCF) делает конечные точки доступными только для клиентов SOAP. В [процедуре создания базовой веб-службы HTTP WCF](how-to-create-a-basic-wcf-web-http-service.md)конечная точка становится доступной для клиентов, не относящихся к протоколу SOAP. Иногда может потребоваться сделать один и тот же контракт доступным обоими способами: в качестве сетевой конечной точки и в качестве конечной точки SOAP. В данном разделе приводится пример того, как это сделать.

## <a name="to-define-the-service-contract"></a>Определение контракта службы

1. Определите контракт службы с помощью интерфейса, помеченного <xref:System.ServiceModel.ServiceContractAttribute> <xref:System.ServiceModel.Web.WebInvokeAttribute> атрибутом, и <xref:System.ServiceModel.Web.WebGetAttribute> атрибутов, как показано в следующем коде:

    [!code-csharp[htSoapWeb#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/htsoapweb/cs/program.cs#0)]
    [!code-vb[htSoapWeb#0](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htsoapweb/vb/program.vb#0)]

    > [!NOTE]
    > По умолчанию атрибут <xref:System.ServiceModel.Web.WebInvokeAttribute> сопоставляет с операцией вызовы POST. Однако можно указать метод для сопоставления с операцией, задав параметр «method=». В методе <xref:System.ServiceModel.Web.WebGetAttribute> отсутствует параметр «method=», при этом метод сопоставляет с операцией службы только вызовы GET.

2. Реализуйте контракт службы, как показано в следующем коде:

     [!code-csharp[htSoapWeb#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/htsoapweb/cs/program.cs#1)]
     [!code-vb[htSoapWeb#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htsoapweb/vb/program.vb#1)]

## <a name="to-host-the-service"></a>Размещение службы

1. Создайте <xref:System.ServiceModel.ServiceHost> объект, как показано в следующем коде:

     [!code-csharp[htSoapWeb#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/htsoapweb/cs/program.cs#2)]
     [!code-vb[htSoapWeb#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htsoapweb/vb/program.vb#2)]

2. Добавьте <xref:System.ServiceModel.Description.ServiceEndpoint> с помощью <xref:System.ServiceModel.BasicHttpBinding> для КОНЕЧНОЙ точки SOAP, как показано в следующем коде:

     [!code-csharp[htSoapWeb#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/htsoapweb/cs/program.cs#3)]
     [!code-vb[htSoapWeb#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htsoapweb/vb/program.vb#3)]

3. Добавьте <xref:System.ServiceModel.Description.ServiceEndpoint> с помощью <xref:System.ServiceModel.WebHttpBinding> для конечной точки, отличной от SOAP, и добавьте в <xref:System.ServiceModel.Description.WebHttpBehavior> конечную точку, как показано в следующем коде:

     [!code-csharp[htSoapWeb#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/htsoapweb/cs/program.cs#4)]
     [!code-vb[htSoapWeb#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htsoapweb/vb/program.vb#4)]

4. Вызовите `Open()` в <xref:System.ServiceModel.ServiceHost> экземпляре, чтобы открыть узел службы, как показано в следующем коде:

     [!code-csharp[htSoapWeb#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/htsoapweb/cs/program.cs#5)]
     [!code-vb[htSoapWeb#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htsoapweb/vb/program.vb#5)]

## <a name="to-call-service-operations-mapped-to-get-in-internet-explorer"></a>Вызов операций службы, сопоставленных с операцией GET, в Internet Explorer

1. Откройте Internet Explorer и введите " `http://localhost:8000/Web/EchoWithGet?s=Hello, world!` " и нажмите клавишу ВВОД. URL-адрес содержит базовый адрес службы ( `http://localhost:8000/` ), относительный адрес конечной точки (""), операцию службы для вызова ("ечовисжет") и вопросительный знак, за которым следует список именованных параметров, разделенных амперсандом (&).

## <a name="to-call-service-operations-on-the-web-endpoint-in-code"></a>Вызов операций службы в сетевой конечной точке в коде

1. Создайте экземпляр класса <xref:System.ServiceModel.Web.WebChannelFactory%601> в блоке `using`, как показано в следующем коде.

     [!code-csharp[htSoapWeb#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/htsoapweb/cs/program.cs#6)]
     [!code-vb[htSoapWeb#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htsoapweb/vb/program.vb#6)]

> [!NOTE]
> Метод `Close()` автоматически вызывается для канала в конце блока `using`.

1. Создайте канал и вызовите службу, как показано в следующем коде.

     [!code-csharp[htSoapWeb#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/htsoapweb/cs/program.cs#8)]
     [!code-vb[htSoapWeb#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htsoapweb/vb/program.vb#8)]

## <a name="to-call-service-operations-on-the-soap-endpoint"></a>Вызов операций службы в конечной точке SOAP

1. Создайте экземпляр класса <xref:System.ServiceModel.ChannelFactory> в блоке `using`, как показано в следующем коде.

    [!code-csharp[htSoapWeb#10](../../../../samples/snippets/csharp/VS_Snippets_CFX/htsoapweb/cs/program.cs#10)]
    [!code-vb[htSoapWeb#10](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htsoapweb/vb/program.vb#10)]

2. Создайте канал и вызовите службу, как показано в следующем коде.

    [!code-csharp[htSoapWeb#11](../../../../samples/snippets/csharp/VS_Snippets_CFX/htsoapweb/cs/program.cs#11)]
    [!code-vb[htSoapWeb#11](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htsoapweb/vb/program.vb#11)]

## <a name="to-close-the-service-host"></a>Закрытие узла службы

1. Закройте ведущее приложение службы, как показано в следующем коде.

    [!code-csharp[htSoapWeb#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/htsoapweb/cs/program.cs#9)]
    [!code-vb[htSoapWeb#9](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htsoapweb/vb/program.vb#9)]

## <a name="example"></a>Пример

Ниже приведен полный листинг кода для этого раздела:

[!code-csharp[htSoapWeb#13](../../../../samples/snippets/csharp/VS_Snippets_CFX/htsoapweb/cs/program.cs#13)]
[!code-vb[htSoapWeb#13](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htsoapweb/vb/program.vb#13)]

## <a name="compiling-the-code"></a>Компиляция кода

 При компиляции Service.cs обращается к файлам System.ServiceModel.dll и System.ServiceModel.Web.dll.

## <a name="see-also"></a>См. также

- <xref:System.ServiceModel.WebHttpBinding>
- <xref:System.ServiceModel.Web.WebGetAttribute>
- <xref:System.ServiceModel.Web.WebInvokeAttribute>
- <xref:System.ServiceModel.Web.WebServiceHost>
- <xref:System.ServiceModel.ChannelFactory>
- <xref:System.ServiceModel.Description.WebHttpBehavior>
- [Модель веб-программирования HTTP WCF](wcf-web-http-programming-model.md)
