---
title: Метод ExportType
ms.date: 03/30/2017
api_name:
- IALink.ExportType
api_location:
- alink.dll
api_type:
- COM
f1_keywords:
- ExportType
helpviewer_keywords:
- ExportType method
ms.assetid: 91a7ce63-f5b8-4f16-b2c4-e1d0baa88944
topic_type:
- apiref
ms.openlocfilehash: b8db08b22765bac0ed2fe058db49d6882b8d22df
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/24/2020
ms.locfileid: "95684802"
---
# <a name="exporttype-method"></a>Метод ExportType

Указывает, что тип является экспортируемым.  
  
## <a name="syntax"></a>Синтаксис  
  
```cpp  
HRESULT ExportType(  
    mdAssembly      AssemblyID,  
    mdToken         FileToken,  
    mdTypeDef       TypeToken,  
    LPCWSTR         pszTypename,  
    DWORD           dwFlags,  
    mdExportedType* pType  
) PURE;  
```  
  
## <a name="parameters"></a>Параметры  

 `AssemblyID`  
 Идентификатор сборки, из которой необходимо выполнить экспорт.  
  
 `FileToken`  
 Маркер файла или идентификатор сборки файла, который определяет экспортируемый тип.  
  
 `TypeToken`  
 Токен типа, который должен быть сделан экспортируемым.  
  
 `pszTypename`  
 Полное имя типа, которое необходимо сделать экспортируемым.  
  
 `dwFlags`  
 `ComType` Флаги, такие как `tdPublic` или `tdNested` . Этот параметр может быть передан [методу дефиникспортедтипе](../metadata/imetadataassemblyemit-defineexportedtype-method.md).  
  
 `pType`  
 Получает токен для экспортируемого типа.  
  
## <a name="return-value"></a>Возвращаемое значение  

 Возвращает S_OK, если метод завершается с ошибкой.  
  
## <a name="requirements"></a>Требования  

 Требуется ALink. h  
  
## <a name="see-also"></a>См. также раздел

- [Интерфейс IALink](ialink-interface.md)
- [Интерфейс IALink2](ialink2-interface.md)
- [API ALink](index.md)
