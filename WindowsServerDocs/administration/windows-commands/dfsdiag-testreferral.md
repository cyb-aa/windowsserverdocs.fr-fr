---
title: Dfsdiag TestReferral
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 877c60dc-e993-4bd5-87dd-e892e3f98a1a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cd1b87befa8a9cfda5ea27a4ce5a5105ea1a1009
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848020"
---
# <a name="dfsdiag-testreferral"></a>Dfsdiag TestReferral

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vérifie le système de fichiers distribués \(DFS\) références en effectuant les tests suivants :  
  
-   Lorsque vous utilisez le paramètre DFSpath sans arguments, cette commande valide le fait que la liste de référence inclut tous les domaines approuvés.  
  
-   Lorsque vous spécifiez un domaine, la commande effectue une vérification d’intégrité des contrôleurs de domaine \(dfsdiag \/testdcs\) et teste les associations de site et le cache du domaine de l’hôte local.  
  
-   Lorsque vous spécifiez un domaine et \\SYSvol ou \\NETLOGON, en plus d’effectuer le même contrôle d’intégrité vérifie, comme lorsque vous spécifiez un domaine, la commande vérifie que la durée de vie \(TTL\) de références de SYSvol ou NETLOGON correspond à la valeur par défaut de 900 secondes.  
  
-   Lorsque vous spécifiez une espace de noms racine, en plus d’effectuer le même contrôle d’intégrité vérifie, comme lorsque vous spécifiez un domaine, la commande effectue une vérification de configuration de DFS \(dfsdiag \/TestDFSConfig\) et un espace de noms contrôle d’intégrité \(dfsdiag \/TestDFSIntegrity\).  
  
-   Lorsque vous spécifiez un dossier DFS \(lien\), en plus d’effectuer les même contrôles d’intégrité comme lorsque vous spécifiez un espace de noms racine, la commande valide la configuration du site pour les dossiers cibles \(dfsdiag \/ testsites\) et valide l’association du site de l’hôte local.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
dfsdiag /TestReferral /DFSpath:<DFS path for getting referrals> [/Full]  
```  
  
### <a name="parameters"></a>Paramètres  
  
|Paramètre|Description|  
|-------|--------|  
|\/DFSpath :<path for getting referrals>|Ce chemin d’accès DFS peut être une des opérations suivantes :<br /><br />-   \(vide\): Domaines approuvés de tests.<br />-   \\\\Domaine : Références du contrôleur de domaine.<br />-   \\\\Domaine\\SYSvol : Références de SYSvol.<br />-   \\\\Domaine\\NETLOGON : Références de NETLOGON.<br />-   \\\\<Domain or server>\\<Namespace Root>: Références de racines Namespace.<br />-   \\\\<Domain or server>\\<Namespace root>\\<DFS folder>: Dossier DFS \(lien\) redirections.|  
|\/complet|Uniquement appliqué aux références de domaine et la racine. vérifie la cohérence des informations d’association de site entre le Registre et les Services de domaine active directory \(AD DS\).|  
  
## <a name="BKMK_Examples"></a>Exemples  
À TBD, tapez :  
  
```  
dfsdiag /TestReferral /DFSpath:\\Contoso.com\MyNamespace  
```  
  
À TBD, tapez :  
  
```  
dfsdiag /TestReferral /DFSpath:  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
  
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
  

