---
title: ASP.NET Core gRPC для разработчиков WCF — gRPC для разработчиков WCF
description: Подлежит написанию
author: markrendle
ms.date: 09/02/2019
ms.openlocfilehash: 7d02d7914aed39613b4a41da55515df80abddfe8
ms.sourcegitcommit: 55f438d4d00a34b9aca9eedaac3f85590bb11565
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71182751"
---
# <a name="aspnet-core-grpc-for-wcf-developers"></a>ASP.NET Core gRPC для разработчиков WCF

![изображение обложки](./media/cover.png)

ИЗДАТЕЛЬ

Подразделение Microsoft Developer Division, команды разработки .NET и Visual Studio

Подразделение корпорации Майкрософт

One Microsoft Way

Redmond, Washington 98052-6399

© Корпорация Майкрософт (Microsoft Corporation), 2019.

Все права защищены. Запрещается полное или частичное воспроизведение или передача настоящей книги в любом виде или любыми средствами без письменного разрешения издателя.

Эта книга предоставляется на условиях "как есть" и выражает взгляды и мнения автора. Взгляды, мнения и сведения, содержащиеся в этой книге, включая URL-адреса и другие ссылки на веб-сайты, могут изменяться без уведомления.

Некоторые приведенные в книге примеры служат только для иллюстрации и являются вымышленными. Все совпадения с реальными наименованиями, людьми и любыми другими предметами являются непреднамеренными и случайными.

Microsoft и товарные знаки, перечисленные на странице "Товарные знаки" на сайте https://www.microsoft.com, являются товарными знаками группы компаний Майкрософт.

Логотип Docker с изображением кита является зарегистрированным товарным знаком Docker, Inc. Используется с разрешения.

Все другие наименования и логотипы являются собственностью своих законных владельцев.

Автор:

> **Марк Рендл (Mark Rendle)**  — главный технический директор — [Visual Recode](https://visualrecode.com)
>
> **Миранда Стайнер (Miranda Steiner)**  — технический писатель

Редакторы:

> **Майра Вензел (Maira Wenzel)**  — старший разработчик содержимого — корпорация Майкрософт

## <a name="introduction"></a>Вступление

TODO

## <a name="purpose"></a>Цель

TODO

## <a name="who-should-use-this-guide"></a>Кому необходимо это руководство

**ОБНОВИТЕ ЭТО**

Это руководство предназначено для разработчиков WCF, руководителей отделов разработки и архитекторов, заинтересованных в переносе решений WCF на базе .NET 4 и более ранних версий в ASP.NET Core 3.0 с использованием служб gRPC.

## <a name="how-you-can-use-this-guide"></a>Как использовать это руководство

**ОБНОВИТЕ ЭТО**

Это краткое введение в создание служб gRPC в ASP.NET Core 3.0 с определенной ссылкой на WCF как на аналогичную платформу. В нем объясняются принципы gRPC, связывающие каждую из представленных концепций с эквивалентными функциями в WCF, и даются рекомендации по переносу существующего приложения WCF в gRPC. Оно также полезно для разработчиков, имеющих опыт работы с WCF и желающих изучать gRPC для создания новых служб. Этот пример приложения можно использовать в качестве шаблона для собственных проектов, и вы можете свободно копировать и повторно использовать код из книги или ее примеров.

При необходимости вы можете порекомендовать это руководство членам своей команды, чтобы и они были в курсе всех важных аспектов. Если все заинтересованные лица будут использовать общий набор терминологии и придерживаться основополагающих принципов, архитектурные модели и практики будут применяться более последовательно.

## <a name="references"></a>Ссылки

- **Веб-сайт gRPC**  
  <https://grpc.io>
- **Выбор между .NET Core и .NET Framework для серверных приложений**  
  <https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server>

>[!div class="step-by-step"]
>[Вперед](introduction.md)