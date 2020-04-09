---
title: L’authentification basée sur les certificats est recommandée pour la réplication
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: d931cc57-414f-4bdf-9ebd-08fd5e22b19d
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: c5e356792ba93d21d9ce130a46e1d60336c99844
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857672"
---
# <a name="certificate-based-authentication-is-recommended-for-replication"></a>L’authentification basée sur les certificats est recommandée pour la réplication

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>**Problème**  
*Une ou plusieurs machines virtuelles sélectionnées pour la réplication sont configurées pour l’authentification Kerberos.*  
  
## <a name="impact"></a>**Effet**  
*Le trafic réseau de réplication entre le serveur principal et le serveur de réplication n’est pas chiffré. Cela a un impact sur les machines virtuelles suivantes :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>**Résolution**  
*Si une autre méthode est utilisée pour effectuer le chiffrement, vous pouvez ignorer cette option. Dans le cas contraire, modifiez les paramètres de l’ordinateur virtuel pour choisir l’authentification basée sur les certificats.*  
  


