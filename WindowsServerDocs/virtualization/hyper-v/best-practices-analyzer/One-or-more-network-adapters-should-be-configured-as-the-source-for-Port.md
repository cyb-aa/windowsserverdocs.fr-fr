---
title: Une ou plusieurs cartes réseau doivent être configurées comme source pour la mise en miroir des ports
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 147fd00f-1440-44d1-94e3-3a8af63aa7ed
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: b2beb64629f95b04ddf1fa3f43634759899554c5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861862"
---
# <a name="one-or-more-network-adapters-should-be-configured-as-the-source-for-port-mirroring"></a>Une ou plusieurs cartes réseau doivent être configurées comme source pour la mise en miroir des ports

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
*Une ou plusieurs machines virtuelles ont une carte réseau configurée comme destination pour la mise en miroir des ports, mais il n’existe aucune source correspondante sur le commutateur virtuel.*  
  
## <a name="impact"></a>**Effet**  
*La mise en miroir des ports ne fonctionnera pas correctement pour les commutateurs virtuels et les ordinateurs virtuels suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>**Résolution**  
*Utilisez Windows PowerShell ou le Gestionnaire Hyper-V pour terminer ou corriger la configuration de la mise en miroir des ports.*  
  


