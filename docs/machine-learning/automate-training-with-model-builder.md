---
title: Построитель моделей и принципы его работы
description: Сведения об использовании построителя моделей ML.NET для автоматического обучения модели машинного обучения
ms.date: 01/07/2020
ms.custom: overview, mlnet-tooling
ms.openlocfilehash: cff4601843ec9ca7201ea7dbdbfbcfa18f50e46e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/15/2020
ms.locfileid: "79397809"
---
# <a name="what-is-model-builder-and-how-does-it-work"></a>Построитель моделей и принципы его работы

Построитель моделей ML.NET — это интуитивно понятное графическое расширение Visual Studio для создания, обучения и развертывания пользовательских моделей машинного обучения.

Построитель моделей использует автоматизированное машинное обучение (AutoML) для анализа различных алгоритмов и параметров машинного обучения и выбора наиболее подходящих из них для конкретного сценария.

Для работы с построителем моделей не нужно быть экспертом в области машинного обучения. Все, что требуется, — это данные и проблема, которую нужно решить. Построитель моделей генерирует код для добавления модели в приложение .NET.

![Анимация: пользовательский интерфейс расширения "Построитель моделей" для Visual Studio](media/ml-dotnet-model-builder.gif)

> [!NOTE]
> Построитель моделей в настоящее время находится на этапе предварительной версии.

## <a name="scenarios"></a>Сценарии

Построитель моделей может создавать модели машинного обучения для приложения на основе множества различных сценариев.

Сценарий описывает тип прогноза, который нужно сделать с использованием данных. Пример:

- прогнозирование будущего объема продаж продукта на основе исторических данных по продажам;
- классификация тональности как положительной или отрицательной на основе отзывов клиентов;
- определение того, является ли банковская операция мошеннической;
- направление проблем, с которыми обращаются клиенты, на рассмотрение соответствующим сотрудникам компании.

### <a name="which-machine-learning-scenario-is-right-for-me"></a>Выбор подходящего сценария машинного обучения

В построителе моделей необходимо выбрать сценарий. Тип сценария зависит от того, какой прогноз вы пытаетесь сделать.

#### <a name="predict-a-category-when-there-are-only-two-categories"></a>Прогнозирование категории (при наличии только двух категорий)

Двоичная классификация служит для разделения данных на две категории (да или нет, успех или неудача, истина или ложь, положительные или отрицательные).

![Схема с примерами двоичной классификации, включая обнаружение мошенничества, устранение рисков и блокировку приложений](media/binary-classification-examples.png)

Анализ тональности можно использовать для определения положительной или отрицательной тональности отзыва клиента. Это пример такой задачи машинного обучения, как двоичная классификация.

Если ваш сценарий требует классификации на две категории, вы можете использовать этот шаблон со своим набором данных.

#### <a name="predict-a-category-when-there-are-three-or-more-categories"></a>Прогнозирование категории (при наличии трех или более категорий)

Многоклассовая классификация служит для разделения данных на три или более категорий.

![Примеры многоклассовой классификации, включая классификацию документов и продуктов, маршрутизацию запросов в службу поддержки и определение приоритетности клиентских проблем](media/multiclass-classification-examples.png)

С помощью классификации проблем можно разделять проблемы, о которых сообщают клиенты (например, на GitHub), на категории исходя из их названий и описаний. Это пример такой задачи машинного обучения, как многоклассовая классификация.

Шаблон многоклассовой классификации можно использовать для сценария, который предполагает разделение данных на три или более категорий.

#### <a name="predict-a-number"></a>Прогнозирование числа

Регрессия служит для прогнозирования числовых значений.

![Схема с примерами регрессии, включая прогнозирование цен, прогнозирование продаж и прогнозное обслуживание](media/regression-examples.png)

С помощью прогнозирования цен можно прогнозировать цены на недвижимость исходя из местоположения, площади и других характеристик объекта недвижимости. Это пример такой задачи машинного обучения, как регрессия.

Шаблон прогнозирования цен можно использовать для сценария, который предполагает прогнозирование числовых значений на основе собственного набора данных.

#### <a name="classify-images-into-categories"></a>Классификация изображений по категориям

Этот сценарий является особым случаем многоклассовой классификации, при котором входные данные, которые нужно классифицировать, представляют собой набор изображений.

Классификация изображений помогает идентифицировать изображения разных категорий. Например, различные виды рельефа, животных или производственного брака.

Вы можете использовать шаблон классификации изображений для своего сценария, если у вас есть набор изображений и вы хотите классифицировать изображения по различным категориям.

#### <a name="custom-scenario"></a>Пользовательский сценарий

Пользовательский сценарий позволяет выбрать сценарий вручную.

## <a name="data"></a>Данные

После выбора сценария построитель моделей попросит вас предоставить набор данных. Данные используются для обучения, оценки и выбора оптимальной модели для сценария.

![Схема, демонстрирующая этапы работы построителя моделей](media/model-builder-steps.png)

Построитель моделей поддерживает наборы данных в форматах TSV, CSV, TXT, а также в формате базы данных SQL. При использовании TXT-файла добавьте в него строку заголовка и разделите столбцы с помощью символов `,`, `;` или `/t`.

При использовании набора данных, состоящего из изображений, поддерживаются файлы с такими расширениями: `.jpg` и `.png`.

Дополнительные сведения см. в статье [Загрузка данных для обучения в построитель моделей](how-to-guides/load-data-model-builder.md).

### <a name="choose-the-output-to-predict-label"></a>Выбор результата для прогнозирования (метка)

Набор данных — это таблица, строки которой содержат образцы для обучения, а столбцы — атрибуты. В каждой строке есть:

- **метка** (атрибут, который нужно спрогнозировать);
- **признаки** (атрибуты, исходя из которых прогнозируется метка).

В сценарии прогнозирования цен на недвижимость признаками могут быть:

- площадь дома;
- количество спален и ванных комнат;
- почтовый индекс.

Метка — это историческая цена на дом данной площади, с данным количеством спален и ванных комнат, а также почтовым индексом.

![Таблица цен на недвижимость, содержащая признаки (площадь, количество комнат, почтовый индекс) и метку (цена)](media/model-builder-data.png)

### <a name="example-datasets"></a>Примеры наборов данных

Если у вас нет собственного набора данных, можно воспользоваться одним из следующих:

|Сценарий|Задача машинного обучения|Данные|Метка|Функции|
|-|-|-|-|-|
|Прогнозирование цен|регрессия|[данные по платам за такси](https://github.com/dotnet/machinelearning-samples/blob/master/datasets/taxi-fare-train.csv)|Плата|Время поездки, расстояние|
|Обнаружение аномалий|двоичная классификация|[данные по продажам продуктов](https://github.com/dotnet/machinelearning-samples/blob/master/samples/csharp/getting-started/AnomalyDetection_Sales/SpikeDetection/Data/product-sales.csv)|Продажи продукта|Месяц|
|Анализ тональности|двоичная классификация|[данные по комментариям на веб-сайте](https://raw.githubusercontent.com/dotnet/machinelearning/master/test/data/wikipedia-detox-250-line-data.tsv)|Метка (0 — отрицательная тональность, 1 — положительная)|Комментарий, год|
|Обнаружение мошенничества|двоичная классификация|[данные по кредитным картам](https://github.com/dotnet/machinelearning-samples/blob/master/samples/csharp/getting-started/BinaryClassification_CreditCardFraudDetection/CreditCardFraudDetection.Trainer/assets/input/creditcardfraud-dataset.zip)|Класс (1 — мошенничество, 0 — нет)|Сумма, V1–V28 (анонимизированные признаки)|
|Классификация текста|многоклассовая классификация|[Данные по проблемам на GitHub](https://github.com/dotnet/machinelearning-samples/blob/master/samples/csharp/end-to-end-apps/MulticlassClassification-GitHubLabeler/GitHubLabeler/Data/corefx-issues-train.tsv)|Область|Название, описание|
|Классификация изображений|многоклассовая классификация|[Изображения с цветами](http://download.tensorflow.org/example_images/flower_photos.tgz)|Вид цветов: маргаритки, одуванчики, розы, подсолнухи, тюльпаны|Данные изображения|

## <a name="train"></a>Обучение

После выбора сценария, данных и метки построитель моделей обучает модель.

### <a name="what-is-training"></a>Сведения об обучении

Обучение — это автоматический процесс, посредством которого построитель моделей учит модель отвечать на вопросы в рамках сценария. После обучения модель может делать прогнозы на основе новых входных данных. Например, если модель прогнозирует цены на недвижимость и на рынке появляется новый дом, она может спрогнозировать цену его продажи.

Так как построитель моделей использует автоматизированное машинное обучение (AutoML), во время обучения вводить данные или производить настройку не нужно.

### <a name="how-long-should-i-train-for"></a>Требуемая длительность обучения

С помощью AutoML построитель моделей изучает несколько моделей, определяя самую эффективную.

Более длительное время обучения позволяет AutoML исследовать больше моделей с более широким диапазоном параметров.

В приведенной ниже таблице указано обобщенное среднее время, необходимое для получения хорошей эффективности для набора примеров наборов данных на локальном компьютере.

|Размер набора данных|Среднее время обучения|
|------------|---------------------|
|0–10 МБ|10 с|
|10–100 МБ|10 мин|
|100–500 МБ|30 мин|
|500 МБ — 1 ГБ|60 мин|
|Более 1 ГБ|Более 3 часов|

Это ориентировочные числа. Точная продолжительность обучения зависит от:

- количества признаков (столбцов), используемых в качестве входных данных для модели;
- типа столбцов;
- задачи машинного обучения;
- производительности ЦП, диска и памяти компьютера, используемого для обучения.

## <a name="evaluate"></a>Оценка

Оценка — это процесс измерения эффективности модели. Построитель моделей использует обученную модель для составления прогнозов на основе новых данных с последующим измерением их точности.

Построитель моделей разделяет обучающие данные на набор для обучения и тестовый набор. Обучающие данные (80 %) служат для обучения модели, а тестовые (20 %) резервируются для ее оценки.

### <a name="how-do-i-understand-my-model-performance"></a>Как определить эффективность модели?

Сценарий сопоставляется с задачей машинного обучения. Каждая задача машинного обучения имеет собственный набор метрик оценки.

#### <a name="regression-for-example-price-prediction"></a>Регрессия (например, прогнозирование цены)

Метрика по умолчанию для регрессии — коэффициент детерминации, для которого допустимо значение в диапазоне от 0 до 1. 1 — наилучшее из возможных значений. Чем ближе значение коэффициента детерминации к 1, тем выше эффективность вашей модели.

Другие метрики, такие как абсолютная потеря, квадратичная потеря и среднеквадратичная потеря, являются дополнительными. Их можно использовать для анализа модели и сравнения ее с другими моделями регрессии.

#### <a name="binary-classification-for-example-sentiment-analysis"></a>Двоичная классификация (например, анализ тональности)

Метрика по умолчанию для задач классификации — точность. Точность — это доля правильных прогнозов, которые модель делает на основе тестового набора данных. Чем ближе она к 100 % или 1,0, тем лучше.

Чтобы модель считалась приемлемой, другие метрики, например площадь под кривой, которая представляет собой отношение доли истинно положительных результатов к доле ложноположительных результатов, должны превышать 0,50.

Дополнительные метрики, такие как показатель F1, можно использовать для контроля баланса между точностью и полнотой.

#### <a name="multi-class-classification-for-example-issue-classification-image-classification"></a>Многоклассовая классификация (например, классификация неполадок, классификация изображений)

Метрика по умолчанию для многоклассовой классификации — микроточность. Чем ближе значение микроточности к 100 % или 1,0, тем лучше.

Еще одна важная метрика для многоклассовой классификации — макроточность. Подобно микроточности, чем ближе ее значение к 1,0, тем лучше. Ниже приведены лучшие определения этих двух типов точности:

- Микроточность — как часто входящий запрос передается подходящей команде сотрудников?
- Макроточность — как часто входящий запрос подходит для команды в среднем случае?

### <a name="more-information-on-evaluation-metrics"></a>Дополнительные сведения о метриках оценки

Дополнительные сведения см. в статье [Метрики оценки модели](resources/metrics.md).

## <a name="improve"></a>Улучшение

Если точность модели недостаточно высока, можно сделать следующее:

- Увеличить время обучения. Чем дольше длится обучение, тем больше алгоритмов и параметров может опробовать система машинного обучения.

- Добавить больше данных. Иногда для подготовки качественной модели машинного обучения недостаточно данных.

- Сбалансировать данные. Для задач классификации обучающий набор должен быть сбалансирован по всем категориям. Например, если имеются четыре класса для 100 образцов, причем первые два класса (тег1 и тег2) используются для 90 записей, а другие два (тег3 и тег4) — для остальных 10 записей, несбалансированность данных может сказаться на правильности прогнозирования классов тег3 и тег4.

## <a name="code"></a>Код

После этапа оценки построитель моделей предоставляет файл модели и код, с помощью которого можно добавить модель в свое приложение. Модели ML.NET сохраняются как ZIP-файлы. Код для загрузки и использования модели добавляется в решение как новый проект. Кроме того, построитель моделей добавляет для примера консольное приложение, которое можно запустить, чтобы увидеть модель в действии.

Построитель моделей также выводит код, использовавшийся для создания модели, чтобы можно было понять, как именно она формировалась. Код обучения модели можно использовать для повторного обучения на основе новых данных.

## <a name="whats-next"></a>Что дальше?

[Установите](how-to-guides/install-model-builder.md) расширение "Построитель моделей" для Visual Studio

Попробуйте [сценарий прогнозирования цен или любой сценарий регрессии](tutorials/predict-prices-with-model-builder.md)
