---
title: Authentification par certificat est recommandée pour la réplication
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: d931cc57-414f-4bdf-9ebd-08fd5e22b19d
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 5bd131b48b009b43b6379f72f370b4179dea5a03
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873060"
---
# <a name="certificate-based-authentication-is-recommended-for-replication"></a>Authentification par certificat est recommandée pour la réplication

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>**Problème**  
*Un ou plusieurs ordinateurs virtuels sélectionnés pour la réplication sont configurés pour l’authentification Kerberos.*  
  
## <a name="impact"></a>**Impact**  
*Le trafic réseau de réplication à partir du serveur principal sur le serveur de réplication n’est pas chiffré. Cela affecte les ordinateurs virtuels suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>**Résolution**  
*Si une autre méthode est utilisée pour effectuer un chiffrement, vous pouvez l’ignorer. Sinon, modifiez les paramètres de machine virtuelle pour choisir l’authentification par certificat.*  
  


