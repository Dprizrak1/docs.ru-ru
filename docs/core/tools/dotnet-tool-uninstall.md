---
title: Команда dotnet tool uninstall
description: Команда dotnet tool uninstall удаляет указанное средство .NET Core с компьютера.
ms.date: 02/14/2020
ms.openlocfilehash: 82799404c40baa3a39f4e2a5fdb414fb745ef448
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/14/2020
ms.locfileid: "78847837"
---
# <a name="dotnet-tool-uninstall"></a>dotnet tool uninstall

**Эта статья относится к следующему.** ✔️ SDK для .NET Core 2.1 и более поздних версий

## <a name="name"></a>name

`dotnet tool uninstall` — удаляет указанное [средство .NET Core](global-tools.md) с компьютера.

## <a name="synopsis"></a>Краткий обзор

```dotnetcli
dotnet tool uninstall <PACKAGE_NAME> <-g|--global>

dotnet tool uninstall <PACKAGE_NAME> <--tool-path>

dotnet tool uninstall <PACKAGE_NAME>

dotnet tool uninstall <-h|--help>
```

## <a name="description"></a>Описание

Команда `dotnet tool uninstall` позволяет удалить средства .NET Core с компьютера. Чтобы использовать команду, необходимо указать один из следующих параметров:

* Чтобы удалить глобальное средство, установленное в расположении по умолчанию, используйте параметр `--global`.
* Чтобы удалить глобальное средство, установленное в пользовательском расположении, используйте параметр `--tool-path`.
* Чтобы удалить локальный инструмент, пропустите параметры `--global` и `--tool-path`.

**Локальные средства доступны в пакете SDK для .NET Core, начиная с версии 3.0.**

## <a name="arguments"></a>Аргументы

- **`PACKAGE_NAME`**

  Имя или идентификатор пакета NuGet, который содержит удаляемое средство .NET Core. Найти имя пакета можно с помощью команды [dotnet tool list](dotnet-tool-list.md).

## <a name="options"></a>Параметры

- **`-g|--global`**

  Указывает, что средство будет удалено из установки на уровне пользователя. Не может использоваться вместе с параметром `--tool-path`. Пропуск `--global` и `--tool-path` означает, что удаляемое средство является локальным.

- **`-h|--help`**

  Выводит краткую справку по команде.

- **`--tool-path <PATH>`**

  Указывает место, откуда будет удалено средство. Путь может быть абсолютным или относительным. Не может использоваться вместе с параметром `--global`. Пропуск `--global` и `--tool-path` означает, что удаляемое средство является локальным.

## <a name="examples"></a>Примеры

- **`dotnet tool uninstall -g dotnetsay`**

  Удаляет глобальное средство [dotnetsay](https://www.nuget.org/packages/dotnetsay/).

- **`dotnet tool uninstall dotnetsay --tool-path c:\global-tools`**

  Удаляет глобальное средство [dotnetsay](https://www.nuget.org/packages/dotnetsay/) из определенного каталога Windows.

- **`dotnet tool uninstall dotnetsay --tool-path ~/bin`**

  Удаляет глобальное средство [dotnetsay](https://www.nuget.org/packages/dotnetsay/) из определенного каталога Linux/macOS.

- **`dotnet tool uninstall dotnetsay`**

  Удаляет локальный инструмент [dotnetsay](https://www.nuget.org/packages/dotnetsay/) из текущего каталога.

## <a name="see-also"></a>См. также

- [Средства .NET Core](global-tools.md)
- [Учебник. Установка и использование глобального средства .NET Core с помощью интерфейса командной строки .NET Core](global-tools-how-to-use.md)
- [Учебник. Установка и использование локального средства .NET Core с помощью интерфейса командной строки .NET Core](local-tools-how-to-use.md)
