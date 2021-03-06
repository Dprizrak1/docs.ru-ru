---
title: Обработка XML-файла
ms.date: 07/20/2015
helpviewer_keywords:
- XML comments [Visual Basic], parsing [Visual Basic]
ms.assetid: 78a15cd0-7708-4e79-85d1-c154b7a14a8c
ms.openlocfilehash: 4230fd88b4b60c631135f5b7fb15f4b6272b5351
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/22/2019
ms.locfileid: "74347305"
---
# <a name="processing-the-xml-file-visual-basic"></a>Обработка XML-файла (Visual Basic)
Компилятор создает строку идентификатора для каждой конструкции в коде, помеченной для создания документации. (Дополнительные сведения о том, как пометить код, см. в разделе [теги XML-комментариев](../../../visual-basic/language-reference/xmldoc/index.md).) Строка идентификатора уникально идентифицирует конструкцию. Программы, обрабатывающие XML-файл, могут использовать строку идентификатора для идентификации соответствующего элемента .NET Framework метаданных или отражения.  
  
 XML-файл не является иерархическим представлением кода; Это плоский список с созданным ИДЕНТИФИКАТОРом для каждого элемента.  
  
 Компилятор соблюдает следующие правила при формировании строк идентификаторов.  
  
- Пробелы в строку не вставляются.  
  
- Первая часть строки идентификатора определяет тип идентифицируемого члена. Это один символ, за которым следует двоеточие. Используются следующие типы элементов.  
  
|Символ|Описание|  
|---|---|  
|в|namespace<br /><br /> Нельзя добавлять комментарии к документации в пространство имен, но можно сделать CREF-ссылки на них, где это поддерживается.|  
|T|Тип: `Class`, `Module`, `Interface`, `Structure`, `Enum`, `Delegate`|  
|F|поле: `Dim`|  
|P|свойство: `Property` (включая свойства по умолчанию)|  
|M|метод: `Sub`, `Function`, `Declare`, `Operator`|  
|E|событие: `Event`|  
|!|текст ошибки<br /><br /> Остальная часть строки предоставляет сведения об ошибке. Компилятор Visual Basic создает сведения об ошибках для ссылок, которые не удается разрешить.|  
  
- Вторая часть `String` — полное имя элемента, начиная с корня пространства имен. Имя элемента, включающий его тип (s) и пространство имен разделяются точками. Если имя элемента содержит точки, они заменяются знаком номера (#). Предполагается, что ни один элемент не имеет знака номера непосредственно в имени. Например, полное имя конструктора `String` будет `System.String.#ctor`.  
  
- Для свойств и методов, имеющих аргументы, список этих аргументов заключается в круглые скобки и указывается в конце. Если аргументы не используются, скобки отсутствуют. Аргументы разделяются запятыми. Кодировка каждого аргумента соответствует непосредственно их кодированию в сигнатуре .NET Framework.  
  
## <a name="example"></a>Пример  
 В следующем коде показано, как создаются строки идентификатора для класса и его членов.  
  
 [!code-vb[VbVbcnXmlDocComments#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnXmlDocComments/VB/Class1.vb#10)]  
  
## <a name="see-also"></a>См. также

- [-doc](../../../visual-basic/reference/command-line-compiler/doc.md)
- [Практическое руководство. Создание XML-документации](../../../visual-basic/programming-guide/program-structure/how-to-create-xml-documentation.md)
