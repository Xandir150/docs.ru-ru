---
title: Метод IMetaDataEmit::SetClassLayout
ms.date: 03/30/2017
api_name:
- IMetaDataEmit.SetClassLayout
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit::SetClassLayout
helpviewer_keywords:
- IMetaDataEmit::SetClassLayout method [.NET Framework metadata]
- SetClassLayout method [.NET Framework metadata]
ms.assetid: 2576c449-388d-4434-a0e1-9f53991e11b6
topic_type:
- apiref
ms.openlocfilehash: ee10907fb7f5d90db1bdce845272cd3de38e35a6
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/24/2020
ms.locfileid: "95718700"
---
# <a name="imetadataemitsetclasslayout-method"></a>Метод IMetaDataEmit::SetClassLayout

Завершает компоновку полей для класса, который был определен при предыдущем вызове [метода дефинетипедеф](imetadataemit-definetypedef-method.md).  
  
## <a name="syntax"></a>Синтаксис  
  
```cpp  
HRESULT SetClassLayout (  
    [in]  mdTypeDef           td,
    [in]  DWORD               dwPackSize,
    [in]  COR_FIELD_OFFSET    rFieldOffsets[],
    [in]  ULONG               ulClassSize
);  
```  
  
## <a name="parameters"></a>Параметры  

 `td`  
 окне `mdTypeDef` Токен, указывающий класс для размещения.  
  
 `dwPackSize`  
 окне Размер упаковки: 1, 2, 4, 8 или 16 байт. Размер упаковки — это число байтов между смежными полями.  
  
 `rFieldOffsets`  
 окне Массив структур [COR_FIELD_OFFSET](cor-field-offset-structure.md) , каждый из которых задает поле класса и смещение поля в пределах класса. Завершите массив с помощью `mdTokenNil` .  
  
 `ulClassSize`  
 окне Размер класса в байтах.  
  
## <a name="remarks"></a>Комментарии  

 Класс изначально определяется путем вызова метода [IMetaDataEmit::D ефинетипедеф](imetadataemit-definetypedef-method.md) и указания одного из трех макетов для полей класса: Automatic, последовательный или явный. Как правило, вы используете автоматическую разметку и позволяете среде выполнения выбрать лучший способ размещения полей.  
  
 Однако может потребоваться, чтобы поля были размещены в соответствии с расположением, используемым неуправляемым кодом. В этом случае выберите последовательный или явный макет и вызов `SetClassLayout` , чтобы завершить компоновку полей:  
  
- Последовательный макет: укажите размер упаковки. Поле выстраивается в соответствии с его естественным размером или упаковочным размером, в зависимости от того, что приводит к уменьшению смещения поля. Задайте `rFieldOffsets` и `ulClassSize` равным нулю.  
  
- Явный макет. либо укажите смещение каждого поля, либо укажите размер класса и размер упаковки.  
  
## <a name="requirements"></a>Требования  

 **Платформы:** см. раздел [Требования к системе](../../get-started/system-requirements.md).  
  
 **Заголовок:** COR. h  
  
 **Библиотека:** Используется в качестве ресурса в MSCorEE.dll  
  
 **.NET Framework версии:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>См. также раздел

- [Интерфейс IMetaDataEmit](imetadataemit-interface.md)
- [Интерфейс IMetaDataEmit2](imetadataemit2-interface.md)
