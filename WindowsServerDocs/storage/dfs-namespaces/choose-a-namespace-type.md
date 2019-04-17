---
title: "Choisir un type d’espace de noms"
description: "Cet article explique comment choisir un type d’espace de noms."
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: eb471b5bf4e12a05b36973eb1ea5350469f6acd5
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="choose-a-namespace-type"></a>Choisir un type d’espace de noms

> S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2, WindowsServer2008

Quand vous créez un espace de noms, vous devez choisir l’un des deux types d’espaces de noms: un espace de noms autonome ou un espace de noms basé sur un domaine. En outre, si vous choisissez un espace de noms basé sur un domaine, vous devez choisir un mode d’espace de noms: le mode Windows2000 Server ou le mode WindowsServer2008.

## <a name="choosing-a-namespace-type"></a>Choix d’un type d’espace de noms

Choisissez un espace de noms autonome si l’une des conditions suivantes s’applique à votre environnement:

-   Votre organisation n’utilise pas ActiveDirectory DomainServices (ADDS).
-   Vous voulez augmenter la disponibilité de l’espace de noms en utilisant un cluster de basculement.
-   Vous devez créer un espace de noms unique avec plus de 5000dossiers DFS dans un domaine qui ne satisfait pas les prérequis pour un espace de noms basé sur un domaine (mode WindowsServer2008), comme expliqué plus loin dans cette rubrique.

> [!NOTE]
> Pour vérifier la taille d’un espace de noms, cliquez avec le bouton droit sur l’espace de noms dans l’arborescence de la console Gestion du système de fichiers distribués (DFS), cliquez sur **Propriétés**, puis consultez la taille de l’espace de noms dans la boîte de dialogue **Propriétés de l’espace de noms**. Pour plus d’informations sur l’extensibilité des espaces de nomsDFS, consultez le site web de Microsoft [Services de fichiers](https://technet.microsoft.com/library/cc771548.aspx).

Choisissez un espace de noms basé sur un domaine si l’une des conditions suivantes s’applique à votre environnement:

-   Vous voulez garantir la disponibilité de l’espace de noms en utilisant plusieurs serveurs d’espaces de noms.
-   Vous voulez masquer aux utilisateurs le nom du serveur d’espace de noms. Cela facilite le remplacement du serveur d’espace de noms ou la migration de l’espace de noms vers un autre serveur.

## <a name="choosing-a-domain-based-namespace-mode"></a>Choix d’un mode d’espace de noms basé sur un domaine

Si vous choisissez un espace de noms basé sur un domaine, vous devez choisir d’utiliser le mode Windows2000 Server ou le mode WindowsServer2008. Le mode WindowsServer2008 inclut la prise en charge de l’énumération basée sur l’accès et une extensibilité accrue. L’espace de noms basé sur un domaine introduit dans Windows2000 Server est maintenant appelé «espace de noms basé sur un domaine (mode Windows2000 Server)».

Pour utiliser le mode Windows Server2008, le domaine et l’espace de noms doivent satisfaire les conditions minimales suivantes:

-   La forêt utilise le niveau fonctionnel de forêt WindowsServer2003 ou un niveau supérieur.
-   Le domaine utilise le niveau fonctionnel de domaine Windows Server2008 ou un niveau supérieur.
-   Tous les serveurs d’espaces de noms exécutent WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2 ou WindowsServer2008.

Choisissez le mode WindowsServer2008, si votre environnement le prend en charge, quand vous créez des espaces de noms de domaine basés sur un domaine. Ce mode fournit des fonctionnalités et une évolutivité supplémentaires, et élimine également le besoin éventuel de migrer un espace de noms à partir du mode Windows2000Server.

Pour plus d’informations sur la migration d’un espace de noms vers le mode WindowsServer2008, consultez [Migrer un espace de noms basé sur un domaine vers le mode WindowsServer2008](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md).

Si votre environnement ne prend pas en charge les espaces de noms basés sur un domaine en mode WindowsServer2008, utilisez le mode Windows2000Server existant pour l’espace de noms.

## <a name="comparing-namespace-types-and-modes"></a>Comparaison des types et des modes d’espace de noms

Les caractéristiques de chaque type ou mode d’espace de noms sont décrites dans le tableau suivant.

|Caractéristique|Espace de noms autonome|Espace de noms basé sur un domaine (mode Windows2000Server) |Espace de noms basé sur un domaine (mode WindowsServer2008) | 
|---|---|---|---|
|Chemin de l’espace de noms|\\\ *NomServeur\NomRacine* |\\\ *NomDomaineNetBIOS\NomRacine* <br />\\\ *NomDomaineDNS\NomRacine*|\\\ *NomDomaineNetBIOS\NomRacine* <br /> \\\ *NomDomaineDNS\NomRacine*|
|Emplacement de stockage des informations d’espace de noms|Dans le Registre et dans un cache mémoire sur le serveur d’espaces de noms|Dans ADDS et dans un cache mémoire sur chaque serveur d’espaces de noms|Dans ADDS et dans un cache mémoire sur chaque serveur d’espaces de noms|
|Recommandations relatives à la taille de l’espace de noms|L’espace de noms peut contenir plus de 5000dossiers avec cibles. La limite recommandée est de 50000dossiers avec cibles|La taille de l’objet espace de noms dans ADDS doit être inférieure à 5mégaoctets (Mo) pour maintenir la compatibilité avec les contrôleurs de domaine qui n’exécutent pas WindowsServer2008. Cela correspond à un maximum d’environ 5000dossiers avec cibles.|L’espace de noms peut contenir plus de 5000dossiers avec cibles. La limite recommandée est de 50000dossiers avec cibles |
|Niveau fonctionnel de forêt ADDS minimal|ADDS n’est pas requis|Windows2000|WindowsServer2003|
|Niveau fonctionnel de domaine ADDS minimal|ADDS n’est pas requis|Windows2000 mixte|WindowsServer2008|
|Serveurs d’espaces de noms pris en charge|Windows2000 Server|Windows2000 Server|WindowsServer2008|
|Prise en charge de l’énumération basée sur l’accès (si elle est activée)|Oui, requiert un serveur d’espaces de noms WindowsServer2008|Non|Oui|
|Méthodes prises en charge pour garantir la disponibilité de l’espace de noms|Créer un espace de noms autonome sur un cluster de basculement|Utiliser plusieurs serveurs d’espaces de noms pour héberger l’espace de noms. (Les serveurs d’espaces de noms doivent se trouver dans le même domaine.)|Utiliser plusieurs serveurs d’espaces de noms pour héberger l’espace de noms. (Les serveurs d’espaces de noms doivent se trouver dans le même domaine.)|
|Prise en charge permettant d’utiliser la réplicationDFS pour répliquer des cibles de dossier|Existante en cas de jonction à un domaine ADDS|Existante|Existante|

## <a name="see-also"></a>Articles associés

-   [Déploiement d’espaces de nomsDFS](deploying-dfs-namespaces.md)
-   [Migrer un espace de noms basé sur un domaine vers le mode WindowsServer2008](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md)


