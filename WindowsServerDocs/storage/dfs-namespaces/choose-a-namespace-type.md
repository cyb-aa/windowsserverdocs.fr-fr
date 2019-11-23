---
title: Choisir un type d’espace de noms
description: Cet article explique comment choisir un type d’espace de noms.
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: becaab1b4c35492200ad3d75d5f31829d85e10f8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402201"
---
# <a name="choose-a-namespace-type"></a>Choisir un type d’espace de noms

> S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008

Quand vous créez un espace de noms, vous devez choisir l’un des deux types d’espaces de noms : un espace de noms autonome ou un espace de noms basé sur un domaine. En outre, si vous choisissez un espace de noms basé sur un domaine, vous devez choisir un mode d’espace de noms : mode serveur Windows 2000 ou mode Windows Server 2008.

## <a name="choosing-a-namespace-type"></a>Choix d’un type d’espace de noms

Choisissez un espace de noms autonome si l’une des conditions suivantes s’applique à votre environnement :

-   Votre organisation n’utilise pas de Active Directory Domain Services (AD DS).
-   Vous voulez augmenter la disponibilité de l’espace de noms en utilisant un cluster de basculement.
-   Vous devez créer un espace de noms unique contenant plus de 5 000 dossiers DFS dans un domaine qui ne répond pas aux conditions requises pour un espace de noms basé sur un domaine (mode Windows Server 2008), comme décrit plus loin dans cette rubrique.

> [!NOTE]
> Pour vérifier la taille d’un espace de noms, cliquez avec le bouton droit sur l’espace de noms dans l’arborescence de la console Gestion du système de fichiers distribués (DFS), cliquez sur **Propriétés**, puis consultez la taille de l’espace de noms dans la boîte de dialogue **Propriétés de l’espace de noms**. Pour plus d’informations sur l’extensibilité des espaces de noms DFS, consultez le site web de Microsoft [Services de fichiers](https://technet.microsoft.com/library/cc771548.aspx).

Choisissez un espace de noms basé sur un domaine si l’une des conditions suivantes s’applique à votre environnement :

-   Vous voulez garantir la disponibilité de l’espace de noms en utilisant plusieurs serveurs d’espaces de noms.
-   Vous voulez masquer aux utilisateurs le nom du serveur d’espace de noms. Cela facilite le remplacement du serveur d’espace de noms ou la migration de l’espace de noms vers un autre serveur.

## <a name="choosing-a-domain-based-namespace-mode"></a>Choix d’un mode d’espace de noms basé sur un domaine

Si vous choisissez un espace de noms basé sur un domaine, vous devez choisir s’il faut utiliser le mode serveur Windows 2000 ou le mode Windows Server 2008. Le mode Windows Server 2008 prend en charge l’énumération basée sur l’accès et l’évolutivité accrue. L’espace de noms basé sur un domaine introduit dans le serveur Windows 2000 est maintenant appelé « espace de noms basé sur un domaine (mode serveur Windows 2000) ».

Pour utiliser le mode Windows Server 2008, le domaine et l’espace de noms doivent respecter la configuration minimale suivante :

-   La forêt utilise le niveau fonctionnel de forêt Windows Server 2003 ou un niveau supérieur.
-   Le domaine utilise le niveau fonctionnel de domaine Windows Server 2008 ou version ultérieure.
-   Tous les serveurs d’espaces de noms exécutent Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008.

Si votre environnement le prend en charge, choisissez le mode Windows Server 2008 quand vous créez des espaces de noms basés sur un domaine. Ce mode fournit des fonctionnalités et une extensibilité supplémentaires, et élimine la nécessité éventuelle de migrer un espace de noms à partir du mode serveur Windows 2000.

Pour plus d’informations sur la migration d’un espace de noms vers le mode Windows Server 2008, consultez [migrer un espace de noms basé sur un domaine vers le mode Windows server 2008](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md).

Si votre environnement ne prend pas en charge les espaces de noms basés sur un domaine en mode Windows Server 2008, utilisez le mode serveur Windows 2000 existant pour l’espace de noms.

## <a name="comparing-namespace-types-and-modes"></a>Comparaison des types et des modes d’espace de noms

Les caractéristiques de chaque type ou mode d’espace de noms sont décrites dans le tableau suivant.

|Caractéristique|Espace de noms autonome|Espace de noms basé sur un domaine (mode Windows 2000 Server) |Espace de noms basé sur un domaine (mode Windows Server 2008) | 
|---|---|---|---|
|Chemin de l’espace de noms|\\\ *ServerName\RootName* |\\\ *NetBIOSDomainName\RootName* <br />\\\ *DNSDomainName\RootName*|\\\ *NetBIOSDomainName\RootName* <br /> \\\ *DNSDomainName\RootName*|
|Emplacement de stockage des informations d’espace de noms|Dans le Registre et dans un cache mémoire sur le serveur d’espaces de noms|Dans AD DS et dans un cache mémoire sur chaque serveur d’espaces de noms|Dans AD DS et dans un cache mémoire sur chaque serveur d’espaces de noms|
|Recommandations relatives à la taille de l’espace de noms|L’espace de noms peut contenir plus de 5 000 dossiers avec cibles. La limite recommandée est de 50 000 dossiers avec cibles|La taille de l’objet espace de noms dans AD DS doit être inférieure à 5 mégaoctets (Mo) pour maintenir la compatibilité avec les contrôleurs de domaine qui n’exécutent pas Windows Server 2008. Cela correspond à un maximum d’environ 5 000 dossiers avec cibles.|L’espace de noms peut contenir plus de 5 000 dossiers avec cibles. La limite recommandée est de 50 000 dossiers avec cibles |
|Niveau fonctionnel de forêt AD DS minimal|AD DS n’est pas requis|Windows 2000|Windows Server 2003|
|Niveau fonctionnel de domaine AD DS minimal|AD DS n’est pas requis|Windows 2000 mixte|Windows Server 2008|
|Serveurs d’espaces de noms pris en charge|Windows 2000 Server|Windows 2000 Server|Windows Server 2008|
|Prise en charge de l’énumération basée sur l’accès (si elle est activée)|Oui, requiert un serveur d’espaces de noms Windows Server 2008|Non|Oui|
|Méthodes prises en charge pour garantir la disponibilité de l’espace de noms|Créer un espace de noms autonome sur un cluster de basculement|Utiliser plusieurs serveurs d’espaces de noms pour héberger l’espace de noms. (Les serveurs d’espaces de noms doivent se trouver dans le même domaine.)|Utiliser plusieurs serveurs d’espaces de noms pour héberger l’espace de noms. (Les serveurs d’espaces de noms doivent se trouver dans le même domaine.)|
|Prise en charge permettant d’utiliser la réplication DFS pour répliquer des cibles de dossier|Existante en cas de jonction à un domaine AD DS|Prise en charge|Prise en charge|

## <a name="see-also"></a>Voir également

-   [Déploiement d’espaces de noms DFS](deploying-dfs-namespaces.md)
-   [Migrer un espace de noms basé sur un domaine vers le mode Windows Server 2008](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md)


