---
title: 'Управление ресурсами: Ключевое слово use'
description: Сведения о F# ключевом слове "use" и функции "using", которые могут управлять инициализацией и освобождением ресурсов.
ms.date: 05/16/2016
ms.openlocfilehash: 5e0838401bee02050343b2f6dcc646a8dc8b4dc0
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68627261"
---
# <a name="resource-management-the-use-keyword"></a>Управление ресурсами: Ключевое слово use

В этом разделе описывается ключевое `use` слово `using` и функция, которые могут управлять инициализацией и освобождением ресурсов.

## <a name="resources"></a>Ресурсы

Термин *ресурс* используется более чем одним способом. Да, ресурсы могут быть данными, используемыми приложением, например строками, графикой и т. д., но в этом контексте *ресурсы* относятся к программному обеспечению или ресурсам операционной системы, таким как контексты графических устройств, дескрипторы файлов, сеть и база данных соединения, объекты параллелизма, такие как дескрипторы ожидания и т. д. Использование этих ресурсов приложениями предполагает приобретение ресурса из операционной системы или другого поставщика ресурсов, за которым следует более поздний выпуск ресурса в пул, чтобы его можно было предоставить другому приложению. Проблемы возникают, когда приложения не освобождают ресурсы в общий пул.

## <a name="managing-resources"></a>Управление ресурсами

Чтобы эффективно и ответственно управлять ресурсами в приложении, необходимо быстро освобождать ресурсы и предсказуемым образом. .NET Framework помогает сделать это, предоставляя `System.IDisposable` интерфейс. Тип, реализующий `System.IDisposable` , `System.IDisposable.Dispose` имеет метод, который правильно освобождает ресурсы. Хорошо написанные приложения гарантируют `System.IDisposable.Dispose` , что он вызывается немедленно, когда любой объект, содержащий ограниченный ресурс, больше не нужен. К счастью, большинство языков .NET предоставляют поддержку для упрощения и F# не являются исключением. Существует две полезные языковые конструкции, поддерживающие шаблон удаления: `use` привязка `using` и функция.

## <a name="use-binding"></a>использовать привязку

Ключевое слово имеет форму, похожую на эту `let` привязку: `use`

использовать*выражение* *значения* = 

Он предоставляет те же функциональные возможности, `let` что и привязка, но добавляет `Dispose` вызов к значению, когда значение выходит за пределы области. Обратите внимание, что компилятор вставляет проверку значения на null, поэтому, если значение равно `null`, `Dispose` вызов метода не выполняется.

В следующем примере показано, как автоматически закрыть файл с помощью `use` ключевого слова.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6301.fs)]

> [!NOTE]
> Можно использовать `use` в вычислительных выражениях, в этом случае используется настроенная версия `use` выражения. Дополнительные сведения см. в разделе [последовательности](sequences.md), [асинхронные рабочие процессы](asynchronous-workflows.md)и [вычислительные выражения](computation-expressions.md).

## <a name="using-function"></a>Использование функции

`using` Функция имеет следующую форму:

`using`(*expression1*) *Function-или-лямбда*

В выражении expression1 создает объект, который должен быть удален. `using` Результат *expression1* (объект, который должен быть удален) становится аргументом, *значением*, *функцией или лямбда*-выражением, представляющим собой либо функцию, которая ожидает один оставшийся аргумент типа, совпадающий со значением, полученным  *expression1*или лямбда-выражение, которое принимает аргумент этого типа. В конце выполнения функции среда выполнения вызывает `Dispose` и освобождает ресурсы (если значение не равно `null`), в этом случае попытка вызова Dispose не выполняется.

В следующем примере показано `using` выражение с лямбда-выражением.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6302.fs)]

В следующем примере показано `using` выражение с функцией.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6303.fs)]

Обратите внимание, что функция может быть функцией, к которой уже применены некоторые аргументы. Это действие представлено в следующем примере кода: Он создает файл, содержащий строку `XYZ`.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6304.fs)]

`using` Функция`use` и привязка являются практически эквивалентными способами выполнения одной и той же задачи. Ключевое слово обеспечивает больший контроль над вызовом метода when `Dispose`. `using` `using`При использовании `Dispose` метод вызывается в конце функции или лямбда-выражения `use` ; при использовании ключевого слова `Dispose` метод вызывается в конце содержащего его блока кода. Как правило, предпочтительнее использовать `use` вместо `using` функции.

## <a name="see-also"></a>См. также

- [Справочник по языку F#](index.md)
