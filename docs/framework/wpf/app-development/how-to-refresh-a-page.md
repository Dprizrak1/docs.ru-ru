---
title: Практическое руководство. Обновление страницы
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- pages [WPF], refreshing
- refreshing pages [WPF]
ms.assetid: 06dd1bbd-81c4-40ad-ac0d-7a5b326b1465
ms.openlocfilehash: 71a71c91384a8905413358d023531afec23f2d41
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61947672"
---
# <a name="how-to-refresh-a-page"></a>Практическое руководство. Обновление страницы
В этом примере показан способ вызова <xref:System.Windows.Navigation.NavigationWindow.Refresh%2A> метод для обновления текущего содержимого в <xref:System.Windows.Navigation.NavigationWindow>.  
  
## <a name="example"></a>Пример  
 <xref:System.Windows.Navigation.NavigationWindow.Refresh%2A> обновляет текущее содержимое в <xref:System.Windows.Navigation.NavigationWindow> для перезагрузки из источника.  
  
 [!code-csharp[HOWTONavigationSnippets#NavigateRefreshCODE](~/samples/snippets/csharp/VS_Snippets_Wpf/HOWTONavigationSnippets/CSharp/MainWindow.xaml.cs#navigaterefreshcode)]
 [!code-vb[HOWTONavigationSnippets#NavigateRefreshCODE](~/samples/snippets/visualbasic/VS_Snippets_Wpf/HOWTONavigationSnippets/visualbasic/mainwindow.xaml.vb#navigaterefreshcode)]
