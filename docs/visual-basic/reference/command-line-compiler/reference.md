---
title: -reference
ms.date: 03/13/2018
helpviewer_keywords:
- /reference compiler option [Visual Basic]
- r compiler option [Visual Basic]
- -reference compiler option [Visual Basic]
- /r compiler option [Visual Basic]
- reference compiler option [Visual Basic]
- -r compiler option [Visual Basic]
ms.assetid: 66bdfced-bbf6-43d1-a554-bc0990315737
ms.openlocfilehash: 35e02d1ad4409e754c2466f7d0ae7e68214772e6
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/07/2020
ms.locfileid: "75716703"
---
# <a name="-reference-visual-basic"></a>-Reference (Visual Basic)
Заставляет компилятор предоставлять сведения о типах в указанных сборках, доступных для компилируемого в данный момент проекта.  
  
## <a name="syntax"></a>Синтаксис  
  
```console  
-reference:fileList  
```

или

```console
-r:fileList  
```  
  
## <a name="arguments"></a>Arguments  
  
|Термин|Определение|  
|---|---|  
|`fileList`|Обязательное Список всех имен файлов сборки, разделенных запятыми. Если имя файла содержит пробел, заключите его в кавычки.|  
  
## <a name="remarks"></a>Заметки  
 Импортируемые файлы должны содержать метаданные сборки. За пределами сборки видны только открытые типы. Параметр [-addmodule](../../../visual-basic/reference/command-line-compiler/addmodule.md) импортирует метаданные из модуля.  
  
 При ссылке на сборку (сборку A), которая сама ссылается на другую сборку (сборка B), необходимо ссылаться на сборку б, если:  
  
- Тип из сборки A наследуется из типа или реализует интерфейс сборки Б.  
  
- Вызывается поле, свойство, событие или метод, имеющий тип возвращаемого значения или тип параметра из сборки Б.  
  
 Используйте параметр [-libpath](../../../visual-basic/reference/command-line-compiler/libpath.md) , чтобы указать каталог, в котором находится одна или несколько ссылок на сборки.  
  
 Чтобы компилятор распознал тип в сборке (а не в модуле), необходимо принудительно разрешить тип. Одним из примеров того, как это можно сделать, является определение экземпляра типа. Существуют другие способы разрешения имен типов в сборке для компилятора. Например, при наследовании от типа в сборке имя типа будет называться компилятором.  
  
 Файл ответов Vbc. rsp, который ссылается на часто используемые .NET Framework сборки, используется по умолчанию. Используйте `-noconfig`, если не нужно, чтобы компилятор использовал Vbc. rsp.  
  
 Краткой формой `-reference` является `-r`.  
  
## <a name="example"></a>Пример  
 Следующая команда компилирует исходный файл `Input.vb` и ссылочные сборки из `Metad1.dll` и `Metad2.dll` для создания `Out.exe`.  
  
```console
vbc -reference:metad1.dll,metad2.dll -out:out.exe input.vb  
```  
  
## <a name="see-also"></a>См. также:

- [Компилятор Visual Basic с интерфейсом командной строки](../../../visual-basic/reference/command-line-compiler/index.md)
- [-noconfig](../../../visual-basic/reference/command-line-compiler/noconfig.md)
- [-Target (Visual Basic)](../../../visual-basic/reference/command-line-compiler/target.md)
- [Public](../../../visual-basic/language-reference/modifiers/public.md)
- [Примеры командных строк компиляции](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
