---
title: 'Метод ICorProfilerInfo7:: GetInMemorySymbolsLength'
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo7.GetInMemorySymbolsLength
api_location:
- mscorwks.dll
- icorprof.idl
api_type:
- COM
ms.assetid: d62c4a4c-8a62-45aa-8f01-a8387cf36159
ms.openlocfilehash: 46ffa5cb4fac6988240d32cb1939cc25bdf0a412
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/24/2020
ms.locfileid: "95686076"
---
# <a name="icorprofilerinfo7getinmemorysymbolslength-method"></a>Метод ICorProfilerInfo7:: GetInMemorySymbolsLength

[Поддерживается в .NET Framework 4.6.1 и более поздних версиях.]  
  
 Возвращает длину потока символов в памяти.  
  
## <a name="syntax"></a>Синтаксис  
  
```cpp  
HRESULT GetInMemorySymbolsLength(  
        [in] ModuleID moduleId,  
        [out] DWORD* pCountSymbolBytes  
);  
```  
  
## <a name="parameters"></a>Параметры  

 `moduleId`  
 окне Идентификатор модуля, содержащего поток в памяти.  
  
 пкаунтсимболбитес  
 заполняет Указатель на значение, `DWORD` которое при возврате метода содержит длину потока в байтах.  
  
## <a name="return-value"></a>Возвращаемое значение  

 Метод возвращает значение `S_OK` , если длина потока памяти может быть определена, даже если она равна нулю (0).  
  
 Метод возвращает значение, `CORPROF_E_MODULE_IS_DYNAMIC` Если метод был создан с помощью <xref:System.Reflection.Emit?displayProperty=nameWithType> .  
  
## <a name="remarks"></a>Комментарии  

 Если модуль содержит символы в памяти, длина потока помещается в `pCountSymbolBytes` . Если у модуля нет символов в памяти, то `*pCountSymbolBytes = 0` .  
  
> [!NOTE]
> Текущая реализация не поддерживает отражение. Emit. Если модуль был создан с помощью отражения. Emit, метод возвращает значение `CORPROF_E_MODULE_IS_DYNAMIC` .  
  
## <a name="requirements"></a>Требования  

 **Платформы:** см. раздел [Требования к системе](../../get-started/system-requirements.md).  
  
 **Заголовок:** CorProf.idl, CorProf.h  
  
 **Библиотека:** CorGuids.lib  
  
 **.NET Framework версии:**[!INCLUDE[net_current_v461plus](../../../../includes/net-current-v461plus-md.md)]  
  
## <a name="see-also"></a>См. также раздел

- [Интерфейс ICorProfilerInfo7](icorprofilerinfo7-interface.md)
