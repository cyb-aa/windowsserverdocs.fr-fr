---
title: Évitez d’activer les ordinateurs virtuels configurés avec des cartes Fibre Channel virtuels pour permettre des migrations en direct lorsqu’il existe des chemins d’accès moins Fibre Channel d’unités logiques (LUN) sur la destination que sur la source
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 6ff69d5cb09133a806c2a2df3446713264a4e892
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849550"
---
# <a name="avoid-enabling-virtual-machines-configured-with-virtual-fibre-channel-adapters-to-allow-live-migrations-when-there-are-fewer-paths-to-fibre-channel-logical-units-luns-on-the-destination-than-on-the-source"></a>Évitez d’activer les ordinateurs virtuels configurés avec des cartes Fibre Channel virtuels pour permettre des migrations en direct lorsqu’il existe des chemins d’accès moins Fibre Channel d’unités logiques (LUN) sur la destination que sur la source

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
*Un ou plusieurs machines virtuelles ont la propriété AllowReducedFcRedunancy définie dans le fournisseur WMI de virtualisation.*  
  
## <a name="impact"></a>**Impact**  
*Migration dynamique des machines virtuelles suivantes peut-être entraîner une perte de données ou d’interruption d’e/s de stockage :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>**Résolution**  
*Envisagez d’effacement de la propriété AllowReducedFcRedundancy WMI sur les ordinateurs virtuels affectés. Lorsque cette propriété est désactivée, vous pouvez effectuer une migration dynamique sur les ordinateurs virtuels configurés avec les cartes Fibre Channel virtuelles uniquement lorsque le nombre de chemins d’accès à Fibre Channel sur la destination est le même ou supérieur au nombre de chemins d’accès sur la source. Ces vérifications permettent d’empêcher la perte de données ou l’interruption d’e/s sur le stockage.* 