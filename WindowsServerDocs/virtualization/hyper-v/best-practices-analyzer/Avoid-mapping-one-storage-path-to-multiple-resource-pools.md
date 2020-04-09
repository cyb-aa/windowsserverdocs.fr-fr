---
title: Évitez de mapper un chemin de stockage à plusieurs pools de ressources
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 24992453-762b-4892-9a50-55d237b9b7f2
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 53ef04a2dde875b26dd109075a2cfa4484ebd5cd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857752"
---
# <a name="avoid-mapping-one-storage-path-to-multiple-resource-pools"></a>Évitez de mapper un chemin de stockage à plusieurs pools de ressources

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Opérations|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.
  
## <a name="issue"></a>**Problème**  
*Un chemin d’accès au fichier de stockage est mappé à plusieurs pools de ressources.*  
  
## <a name="impact"></a>**Effet**  
*Pour le type de pool de stockage spécifié, les pools parents et enfants suivants partagent le même chemin de stockage :*  
  
\<liste des pools >  
  
## <a name="resolution"></a>**Résolution**  
*Utilisez Windows PowerShell pour reconfigurer les pools de ressources de stockage de sorte que plusieurs pools n’utilisent pas le même chemin de stockage.*  
  


