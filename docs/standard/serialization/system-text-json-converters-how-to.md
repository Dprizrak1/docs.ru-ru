---
title: Написание пользовательских преобразователей для сериализации JSON — .NET
ms.date: 01/10/2020
no-loc:
- System.Text.Json
- Newtonsoft.Json
helpviewer_keywords:
- JSON serialization
- serializing objects
- serialization
- objects, serializing
- converters
ms.openlocfilehash: 310967f39c3aa7a46d79087bcbf0cb016f7d7284
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2020
ms.locfileid: "78159576"
---
# <a name="how-to-write-custom-converters-for-json-serialization-marshalling-in-net"></a>Написание пользовательских преобразователей для сериализации JSON (маршалирование) в .NET

В этой статье показано, как создать пользовательские преобразователи для классов сериализации JSON, предоставляемых в пространстве имен <xref:[!OP.NO-LOC(System.Text.Json)]>. Общие сведения о `[!OP.NO-LOC(System.Text.Json)]`см. [в статье сериализация и десериализация JSON в .NET](system-text-json-how-to.md).

*Преобразователь* — это класс, который преобразует объект или значение в JSON и обратно. Пространство имен `[!OP.NO-LOC(System.Text.Json)]` содержит встроенные преобразователи для большинства типов-примитивов, которые сопоставляются с примитивами JavaScript. Вы можете создавать пользовательские преобразователи:

* Переопределение поведения по умолчанию встроенного преобразователя. Например, может потребоваться, чтобы `DateTime` значения были представлены в формате мм/дд/гггг вместо формата ISO 8601-1:2019 по умолчанию.
* Для поддержки пользовательского типа значения. Например, структура `PhoneNumber`.

Можно также написать пользовательские преобразователи для настройки или расширения `[!OP.NO-LOC(System.Text.Json)]` с функциональностью, не входящей в текущий выпуск. Далее в этой статье рассматриваются следующие сценарии.

* [Десериализация выводимых типов в свойства объекта](#deserialize-inferred-types-to-object-properties).
* [Словарь поддержки с ключом, не являющимся строкой](#support-dictionary-with-non-string-key).
* [Поддержка полиморфизма](#support-polymorphic-deserialization).

## <a name="custom-converter-patterns"></a>Пользовательские шаблоны преобразователя

Существует два шаблона создания пользовательского преобразователя: базовый шаблон и шаблон фабрики. Шаблон фабрики предназначен для преобразователей, обрабатывающих `Enum` типов или открытых универсальных шаблонов. Базовый шаблон предназначен для неуниверсальных и закрытых универсальных типов.  Например, для преобразователей следующих типов требуется шаблон фабрики:

* `Dictionary<TKey, TValue>`
* `Enum`
* `List<T>`

Ниже приведены некоторые примеры типов, которые могут быть обработаны базовым шаблоном.

* `Dictionary<int, string>`
* `WeekdaysEnum`
* `List<DateTimeOffset>`
* `DateTime`
* `Int32`

Базовый шаблон создает класс, который может работать с одним типом. Шаблон фабрики создает класс, который определяет в среде выполнения, какой необходим конкретный тип, и динамически создает соответствующий преобразователь.

## <a name="sample-basic-converter"></a>Образец базового преобразователя

Следующий пример представляет собой преобразователь, который переопределяет сериализацию по умолчанию для существующего типа данных. Для `DateTimeOffset` свойств преобразователь использует формат мм/дд/гггг.

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/DateTimeOffsetConverter.cs)]

## <a name="sample-factory-pattern-converter"></a>Образец преобразователя шаблона фабрики

В следующем коде показан пользовательский преобразователь, работающий с `Dictionary<Enum,TValue>`. Код соответствует шаблону фабрики, так как первый параметр универсального типа является `Enum`, а второй — открытым. Метод `CanConvert` возвращает `true` только для `Dictionary` с двумя универсальными параметрами, первый из которых является типом `Enum`. Внутренний преобразователь получает существующий преобразователь для работы с любым типом, предоставленным во время выполнения для `TValue`.

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/DictionaryTKeyEnumTValueConverter.cs)]

Приведенный выше код аналогичен приведенному в [словаре поддержки с ключом, не являющимся строкой](#support-dictionary-with-non-string-key) далее в этой статье.

## <a name="steps-to-follow-the-basic-pattern"></a>Шаги по базовому шаблону

Ниже описано, как создать преобразователь, следуя базовому шаблону.

* Создайте класс, производный от <xref:[!OP.NO-LOC(System.Text.Json)].Serialization.JsonConverter%601>, где `T` — это тип для сериализации и десериализации.
* Переопределите метод `Read`, чтобы десериализовать входящий JSON и преобразовать его в тип `T`. Используйте <xref:[!OP.NO-LOC(System.Text.Json)].Utf8JsonReader>, передаваемый в метод для чтения JSON.
* Переопределите метод `Write`, чтобы сериализовать входящий объект типа `T`. Используйте <xref:[!OP.NO-LOC(System.Text.Json)].Utf8JsonWriter>, передаваемый в метод для записи JSON.
* Переопределяйте метод `CanConvert` только при необходимости. Реализация по умолчанию возвращает `true`, если тип для преобразования имеет тип `T`. Поэтому для преобразователей, поддерживающих только тип `T` не требуется переопределять этот метод. Пример преобразователя, в котором требуется переопределить этот метод, см. в подразделе « [полиморфизм десериализации](#support-polymorphic-deserialization) » далее в этой статье.

В качестве эталонных реализаций для написания пользовательских преобразователей можно обратиться к [исходному коду встроенных преобразователей](https://github.com/dotnet/runtime/tree/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/[!OP.NO-LOC(System.Text.Json)]/src/[!OP.NO-LOC(System/Text/Json)]/Serialization/Converters/) .

## <a name="steps-to-follow-the-factory-pattern"></a>Действия по шаблону фабрики

Ниже описано, как создать преобразователь, следуя шаблону фабрики.

* Создайте класс, наследующий от класса <xref:[!OP.NO-LOC(System.Text.Json)].Serialization.JsonConverterFactory>.
* Переопределите метод `CanConvert`, чтобы он возвращал значение true, если преобразуемый тип является одним, который может быть преобразован преобразователем. Например, если преобразователь предназначен для `List<T>` он может работать только с `List<int>`, `List<string>`и `List<DateTime>`.
* Переопределите метод `CreateConverter`, чтобы он возвращал экземпляр класса преобразователя, обрабатывающего тип для преобразования, который будет предоставлен во время выполнения.
* Создайте класс преобразователя, который создает экземпляр метода `CreateConverter`.

Шаблон фабрики необходим для открытых универсальных шаблонов, поскольку код для преобразования объекта в строку и из строки не совпадает для всех типов. Преобразователь для открытого универсального типа (например,`List<T>`) должен создать преобразователь для закрытого универсального типа (например,`List<DateTime>`) в фоновом режиме. Необходимо написать код для работы с каждым закрытым универсальным типом, который может обрабатываться преобразователем.

Тип `Enum` похож на открытый универсальный тип: преобразователь для `Enum` должен создать преобразователь для определенного `Enum` (например,`WeekdaysEnum`) в фоновом режиме.

## <a name="error-handling"></a>Обработка ошибок

Если необходимо создать исключение в коде обработки ошибок, рассмотрите возможность создания <xref:[!OP.NO-LOC(System.Text.Json)].JsonException> без сообщения. Этот тип исключения автоматически создает сообщение, содержащее путь к части JSON, вызвавшей ошибку. Например, инструкция `throw new JsonException();` выдает сообщение об ошибке, как в следующем примере:

```
Unhandled exception. [!OP.NO-LOC(System.Text.Json)].JsonException:
The JSON value could not be converted to System.Object.
Path: $.Date | LineNumber: 1 | BytePositionInLine: 37.
```

Если вы выдаете сообщение (например, `throw new JsonException("Error occurred")`, исключение по-прежнему предоставляет путь в свойстве <xref:[!OP.NO-LOC(System.Text.Json)].JsonException.Path>.

## <a name="register-a-custom-converter"></a>Регистрация пользовательского преобразователя

*Зарегистрируйте* пользовательский преобразователь, чтобы методы `Serialize` и `Deserialize` использовали его. Выберите один из следующих подходов:

* Добавьте экземпляр класса преобразователя в коллекцию <xref:[!OP.NO-LOC(System.Text.Json)].JsonSerializerOptions.Converters?displayProperty=nameWithType>.
* Примените атрибут [[жсонконвертер]](xref:[!OP.NO-LOC(System.Text.Json)].Serialization.JsonConverterAttribute) к свойствам, для которых требуется пользовательский преобразователь.
* Примените атрибут [[жсонконвертер]](xref:[!OP.NO-LOC(System.Text.Json)].Serialization.JsonConverterAttribute) к классу или структуре, представляющей пользовательский тип значения.

## <a name="registration-sample---converters-collection"></a>Пример регистрации — Коллекция преобразователей

Ниже приведен пример, который делает <xref:System.ComponentModel.DateTimeOffsetConverter> по умолчанию для свойств типа <xref:System.DateTimeOffset>.

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/RegisterConverterWithConvertersCollection.cs?name=SnippetSerialize)]

Предположим, что Вы сериализуете экземпляр следующего типа:

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecast.cs?name=SnippetWF)]

Ниже приведен пример выходных данных JSON, демонстрирующий использование пользовательского преобразователя.

```json
{
  "Date": "08/01/2019",
  "TemperatureCelsius": 25,
  "Summary": "Hot"
}
```

Следующий код использует тот же подход для десериализации с помощью пользовательского преобразователя `DateTimeOffset`:

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/RegisterConverterWithConvertersCollection.cs?name=SnippetDeserialize)]

## <a name="registration-sample---jsonconverter-on-a-property"></a>Пример регистрации-[Жсонконвертер] для свойства

Следующий код выбирает пользовательский преобразователь для свойства `Date`:

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecast.cs?name=SnippetWFWithConverterAttribute)]

Код для сериализации `WeatherForecastWithConverterAttribute` не требует использования `JsonSerializeOptions.Converters`:

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/RegisterConverterWithAttributeOnProperty.cs?name=SnippetSerialize)]

Код для десериализации также не требует использования `Converters`:

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/RegisterConverterWithAttributeOnProperty.cs?name=SnippetDeserialize)]

## <a name="registration-sample---jsonconverter-on-a-type"></a>Пример регистрации-[Жсонконвертер] для типа

Вот код, создающий структуру и применяющий к нему атрибут `[JsonConverter]`:

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/Temperature.cs)]

Вот пользовательский преобразователь для предыдущей структуры:

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/TemperatureConverter.cs)]

Атрибут `[JsonConvert]` в структуре регистрирует пользовательский преобразователь в качестве значения по умолчанию для свойств типа `Temperature`. Этот преобразователь автоматически используется в свойстве `TemperatureCelsius` следующего типа при сериализации или десериализации.

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecast.cs?name=SnippetWFWithTemperatureStruct)]

## <a name="converter-registration-precedence"></a>Приоритет регистрации преобразователя

Во время сериализации или десериализации выбирается преобразователь для каждого элемента JSON в следующем порядке, в котором в списке от наивысшего приоритета к наименьшему:

* `[JsonConverter]` применено к свойству.
* Преобразователь, добавленный в коллекцию `Converters`.
* `[JsonConverter]` применяется к пользовательскому типу значения или POCO.

Если в коллекции `Converters` зарегистрировано несколько пользовательских преобразователей, то используется первый преобразователь, возвращающий значение true для `CanConvert`.

Встроенный преобразователь выбирается только в том случае, если соответствующий пользовательский преобразователь не зарегистрирован.

## <a name="converter-samples-for-common-scenarios"></a>Примеры преобразователей для распространенных сценариев

В следующих разделах приведены примеры преобразователей, в которых рассматриваются некоторые распространенные сценарии, которые не обрабатывались встроенными функциями.

* [Десериализация выводимых типов в свойства объекта](#deserialize-inferred-types-to-object-properties)
* [Словарь поддержки с ключом, не являющимся строкой](#support-dictionary-with-non-string-key)
* [Поддержка полиморфизма](#support-polymorphic-deserialization)

### <a name="deserialize-inferred-types-to-object-properties"></a>Десериализация выводимых типов в свойства объекта

При десериализации свойства типа `object`создается объект `JsonElement`. Причина заключается в том, что десериализатор не знает, какой тип CLR создать, и не пытается угадать. Например, если свойство JSON имеет значение "true", десериализатор не определит, что значение является `Boolean`, а если у элемента есть "01/01/2019", десериализатор не определит, что это `DateTime`.

Вывод типа может быть неточным. Если десериализатор анализирует номер JSON, не имеющий десятичного разделителя, в качестве `long`, это может привести к проблемам вне диапазона, если значение первоначально было сериализовано как `ulong` или `BigInteger`. Анализ числа с десятичной запятой в качестве `double` может привести к потере точности, если это число было первоначально сериализовано как `decimal`.

В сценариях, требующих вывода типа, в следующем коде показан пользовательский преобразователь для `object` свойств. Код преобразует:

* `true` и `false` `Boolean`
* Числа без десятичного разделителя для `long`
* Числа с десятичным числом для `double`
* Даты для `DateTime`
* Строки для `string`
* Все остальное для `JsonElement`

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/ObjectToInferredTypesConverter.cs)]

Следующий код регистрирует преобразователь:

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/DeserializeInferredTypesToObject.cs?name=SnippetRegister)]

Ниже приведен пример типа с `object` свойствами.

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecast.cs?name=SnippetWFWithObjectProperties)]

Следующий пример JSON для десериализации содержит значения, которые будут десериализованы как `DateTime`, `long`и `string`:

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot",
}
```

Без пользовательского преобразователя десериализация помещает `JsonElement` в каждое свойство.

[Папка Unit Tests](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/tests/Serialization/) в пространстве имен `System.Text.Json.Serialization` содержит дополнительные примеры пользовательских преобразователей, которые обрабатывали десериализацию для `object` свойств.

### <a name="support-dictionary-with-non-string-key"></a>Словарь поддержки с ключом, не являющимся строкой

Встроенная поддержка коллекций словаря предназначена для `Dictionary<string, TValue>`. То есть ключ должен быть строкой. Для поддержки словаря с целым числом или другим типом в качестве ключа требуется пользовательский преобразователь.

В следующем коде показан пользовательский преобразователь, работающий с `Dictionary<Enum,TValue>`:

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/DictionaryTKeyEnumTValueConverter.cs)]

Следующий код регистрирует преобразователь:

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/RoundtripDictionaryTkeyEnumTValue.cs?name=SnippetRegister)]

Преобразователь может сериализовать и десериализовать свойство `TemperatureRanges` следующего класса, который использует следующие `Enum`:

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecast.cs?name=SnippetWFWithEnumDictionary)]

Выходные данные JSON из сериализации выглядят, как в следующем примере:

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot",
  "TemperatureRanges": {
    "Cold": 20,
    "Hot": 40
  }
}
```

В [папке Unit Tests](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/tests/Serialization/) в пространстве имен `System.Text.Json.Serialization` есть дополнительные примеры пользовательских преобразователей, обрабатывающих словари, не являющиеся строковыми.

### <a name="support-polymorphic-deserialization"></a>Поддержка полиморфизма

Встроенные функции предоставляют ограниченный диапазон [полиморфизмной сериализации](system-text-json-how-to.md#serialize-properties-of-derived-classes) , но не поддерживают десериализацию. Для десериализации требуется пользовательский преобразователь.

Предположим, например, что имеется `Person` абстрактный базовый класс с `Employee` и `Customer` производными классами. Полиморфизм десериализации означает, что во время разработки можно указать `Person` в качестве цели десериализации, а `Customer` и `Employee` объекты в JSON правильно десериализовать во время выполнения. Во время десериализации необходимо найти подсказки, которые указывают требуемый тип в JSON. В каждом сценарии доступны различные виды подкатегории. Например, может быть доступно свойство дискриминатора, или может потребоваться полагаться на присутствие или отсутствие конкретного свойства. В текущем выпуске `System.Text.Json` не предоставлены атрибуты для указания способов управления сценариями десериализации, поэтому требуются пользовательские преобразователи.

В следующем коде показан базовый класс, два производных класса и пользовательский преобразователь для них. Преобразователь использует свойство дискриминатора для преобразования в полиморфизм. Дискриминатор типа не находится в определениях классов, но создается во время сериализации и считывается во время десериализации.

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/Person.cs?name=SnippetPerson)]

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/PersonConverterWithTypeDiscriminator.cs)]

Следующий код регистрирует преобразователь:

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/RoundtripPolymorphic.cs?name=SnippetRegister)]

Преобразователь может десериализовать JSON, созданный с помощью того же преобразователя, для сериализации, например:

```json
[
  {
    "TypeDiscriminator": 1,
    "CreditLimit": 10000,
    "Name": "John"
  },
  {
    "TypeDiscriminator": 2,
    "OfficeNumber": "555-1234",
    "Name": "Nancy"
  }
]
```

Код преобразователя в предыдущем примере считывает и записывает каждое свойство вручную. Альтернативой является вызов `Deserialize` или `Serialize` для выполнения некоторой работы. Пример см. в [этой записи StackOverflow](https://stackoverflow.com/a/59744873/12509023).

## <a name="other-custom-converter-samples"></a>Другие примеры пользовательских преобразователей

В статье [Миграция из Newtonsoft.Json в System.Text.Json](system-text-json-migrate-from-newtonsoft-how-to.md) содержатся дополнительные образцы пользовательских преобразователей.

[Папка Unit Tests](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/tests/Serialization/) в исходном коде `System.Text.Json.Serialization` содержит другие примеры пользовательских преобразователей, например:

* [Преобразователь Int32, который преобразует значение NULL в 0 при десериализации](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/tests/Serialization/CustomConverterTests.NullValueType.cs)
* [Преобразователь Int32, который допускает как строковые, так и числовые значения при десериализации](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/tests/Serialization/CustomConverterTests.Int32.cs)
* [Преобразователь перечислений](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/tests/Serialization/CustomConverterTests.Enum.cs)
* [Перечисление\<преобразования >, принимающего внешние данные](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/tests/Serialization/CustomConverterTests.List.cs)
* [Преобразователь long [], который работает с разделенным запятыми списком чисел](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/tests/Serialization/CustomConverterTests.Array.cs)

Если необходимо сделать преобразователь, изменяющий поведение существующего встроенного преобразователя, можно получить [Исходный код существующего преобразователя](https://github.com/dotnet/runtime/tree/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/src/System/Text/Json/Serialization/Converters) , который будет служить отправной точкой для настройки.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Исходный код для встроенных преобразователей](https://github.com/dotnet/runtime/tree/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/src/System/Text/Json/Serialization/Converters)
* [Поддержка DateTime и DateTimeOffset в System.Text.Json](../datetime/system-text-json-support.md)
* [Обзор System.Text.Json](system-text-json-overview.md)
* [Использование System.Text.Json](system-text-json-how-to.md)
* [Переход с Newtonsoft.Json](system-text-json-migrate-from-newtonsoft-how-to.md)
* [Справочник по System.Text.Json API](xref:System.Text.Json)
* [System.Text.Json. Справочник по API сериализации](xref:System.Text.Json.Serialization)
<!-- * [System.Text.Json roadmap](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/roadmap/README.md)-->
