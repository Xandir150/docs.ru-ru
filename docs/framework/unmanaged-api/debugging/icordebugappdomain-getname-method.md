---
title: Метод ICorDebugAppDomain::GetName
ms.date: 03/30/2017
api_name:
- ICorDebugAppDomain.GetName
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugAppDomain::GetName
helpviewer_keywords:
- ICorDebugAppDomain::GetName method [.NET Framework debugging]
- GetName method, ICorDebugAppDomain interface [.NET Framework debugging]
ms.assetid: 02c596d7-00b0-4e2c-856b-5425158fcefd
topic_type:
- apiref
ms.openlocfilehash: 501a4543940437bfe2a6cb0952aed8184107150c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/24/2020
ms.locfileid: "95672166"
---
# <a name="icordebugappdomaingetname-method"></a>Метод ICorDebugAppDomain::GetName

Возвращает имя домена приложения.  
  
## <a name="syntax"></a>Синтаксис  
  
```cpp  
HRESULT GetName (  
    [in]  ULONG32           cchName,  
    [out] ULONG32           *pcchName,  
    [out, size_is(cchName), length_is(*pcchName)]
         WCHAR              szName[]  
);  
```  
  
## <a name="parameters"></a>Параметры  

 `cchName`  
 [in] Размер массива `szName`. Присвойте этому параметру значение 0, чтобы перевести этот метод в режим запроса.  
  
 `pcchName`  
 заполняет Указатель на размер имени или числа символов, фактически возвращаемых в `szName` . В режиме запроса это значение позволяет вызывающему объекту понять, насколько большой размер буфера выделяется для имени.  
  
 `szName`  
 заполняет Массив, в котором хранится имя домена приложения.  
  
## <a name="remarks"></a>Комментарии  

 Отладчик вызывает `GetName` метод один раз, чтобы получить размер буфера, необходимого для имени. Отладчик выделяет буфер, а затем вызывает метод еще раз для заполнения буфера. Первый вызов для получения размера имени называется *режимом запроса*.  
  
## <a name="requirements"></a>Требования  

 **Платформы:** см. раздел [Требования к системе](../../get-started/system-requirements.md).  
  
 **Заголовок:** CorDebug.idl, CorDebug.h  
  
 **Библиотека:** CorGuids.lib  
  
 **.NET Framework версии:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
