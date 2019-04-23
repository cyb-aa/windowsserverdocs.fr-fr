---
title: Évitez d’installer RemoteFX sur un ordinateur qui est configuré comme un contrôleur de domaine Active Directory
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: da58694e-91f6-45d8-a599-18966db165f4
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 9e1c642e3f36b5fe25f34bb417a83b8510adcc02
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832560"
---
# <a name="avoid-installing-remotefx-on-a-computer-that-is-configured-as-an-active-directory-domain-controller"></a>Évitez d’installer RemoteFX sur un ordinateur qui est configuré comme un contrôleur de domaine Active Directory

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Erreur|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>**Problème**  
*RemoteFX est installé sur un contrôleur de domaine.*  
  
## <a name="impact"></a>**Impact**  
*Les ordinateurs virtuels configurés pour RemoteFX ne peut pas être utilisés sur ces ordinateurs.*  
  
## <a name="resolution"></a>**Résolution**  
*Décidez si vous souhaitez que ce serveur pour être configuré soit avec RemoteFX pour Hyper-V ou comme un contrôleur de domaine Active Directory, puis reconfigurer le serveur en fonction des besoins.*  
  


