---
title: Évitez d’installer RemoteFX sur un ordinateur configuré en tant que contrôleur de domaine Active Directory
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: da58694e-91f6-45d8-a599-18966db165f4
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 67fd8e2568691b7e9be4b46e30b64bf44558d6d0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366470"
---
# <a name="avoid-installing-remotefx-on-a-computer-that-is-configured-as-an-active-directory-domain-controller"></a>Évitez d’installer RemoteFX sur un ordinateur configuré en tant que contrôleur de domaine Active Directory

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Error|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>**Problème**  
*RemoteFX est installé sur un contrôleur de domaine.*  
  
## <a name="impact"></a>**Impact**  
*Les ordinateurs virtuels configurés pour RemoteFX ne peuvent pas être utilisés sur ces ordinateurs.*  
  
## <a name="resolution"></a>**Résolution**  
*Déterminez si vous souhaitez que ce serveur soit configuré avec RemoteFX pour Hyper-V ou en tant que contrôleur de domaine Active Directory, puis reconfigurez le serveur en fonction des besoins.*  
  


