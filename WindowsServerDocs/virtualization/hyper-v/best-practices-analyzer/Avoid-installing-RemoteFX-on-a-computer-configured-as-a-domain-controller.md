---
title: Évitez d’installer RemoteFX sur un ordinateur configuré en tant que contrôleur de domaine Active Directory
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: da58694e-91f6-45d8-a599-18966db165f4
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 597dd26d0a317ca026cd94ab29138ce679d7c3b9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857762"
---
# <a name="avoid-installing-remotefx-on-a-computer-that-is-configured-as-an-active-directory-domain-controller"></a>Évitez d’installer RemoteFX sur un ordinateur configuré en tant que contrôleur de domaine Active Directory

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Error|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>**Problème**  
*RemoteFX est installé sur un contrôleur de domaine.*  
  
## <a name="impact"></a>**Effet**  
*Les ordinateurs virtuels configurés pour RemoteFX ne peuvent pas être utilisés sur ces ordinateurs.*  
  
## <a name="resolution"></a>**Résolution**  
*Déterminez si vous souhaitez que ce serveur soit configuré avec RemoteFX pour Hyper-V ou en tant que contrôleur de domaine Active Directory, puis reconfigurez le serveur en fonction des besoins.*  
  


