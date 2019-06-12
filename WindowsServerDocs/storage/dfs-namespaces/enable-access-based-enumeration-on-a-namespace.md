---
title: Activer l’énumération basée sur l’accès pour un espace de noms
description: Cet article décrit comment activer l’énumération basée sur l’accès pour un espace de noms.
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 7e9a5b397127e9eb88352fb4d7bc28955023d4b7
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447217"
---
# <a name="enable-access-based-enumeration-on-a-namespace"></a>Activer l’énumération basée sur l’accès pour un espace de noms

> S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

L’énumération basée sur l’accès masque les fichiers et les dossiers auxquels les utilisateurs ne sont pas autorisés à accéder. Par défaut, cette fonctionnalité n’est pas activée pour les espaces de noms DFS. Vous pouvez activer l’énumération basée sur l’accès des dossiers DFS à l’aide de Gestion DFS. Pour contrôler l’énumération basée sur l’accès des fichiers et des dossiers dans les cibles de dossier, vous devez activer l’énumération basée sur l’accès pour chaque dossier partagé à l’aide de Gestion du partage et du stockage.

Pour activer l’énumération basée sur l’accès sur un espace de noms, tous les serveurs d’espace de noms doivent exécuter Windows Server 2008 ou version ultérieure. En outre, les espaces de noms de domaine doivent utiliser le mode de Windows Server 2008. Pour plus d’informations sur la configuration requise du mode Windows Server 2008, consultez [choisir un Type de Namespace](choose-a-namespace-type.md).

Dans certains environnements, l’activation de l’énumération basée sur l’accès peut entraîner une utilisation élevée du processeur sur le serveur et des temps de réponse lents pour les utilisateurs.

> [!NOTE]
> Si vous mettez à niveau le domaine fonctionnel niveau vers Windows Server 2008, bien qu’il existe existants basés sur domaine espaces de noms, la gestion DFS vous permettra d’activer l’accès en fonction d’énumération sur ces espaces de noms. Toutefois, vous ne serez pas en mesure de modifier les autorisations pour masquer des dossiers à partir des groupes ou utilisateurs, sauf si vous migrez les espaces de noms vers le mode Windows Server 2008. Pour plus d’informations, voir [Migrer un espace de noms basé sur un domaine vers le mode Windows Server 2008](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md).


Pour utiliser l’énumération basée sur l’accès avec des espaces de noms DFS, vous devez effectuer les étapes suivantes :

-   Activer l’énumération basée sur l’accès pour un espace de noms
-   Contrôler les utilisateurs et les groupes qui peuvent afficher des dossiers DFS individuels


> [!WARNING]
> L’énumération basée sur l’accès n’empêche pas les utilisateurs d’obtenir une référence vers une cible de dossier s’ils connaissent déjà le chemin d’accès DFS. Seules les autorisations de partage ou les autorisations du système de fichiers NTFS de la cible de dossier (dossier partagé) peuvent empêcher les utilisateurs d’accéder à une cible de dossier. Les autorisations de dossiers DFS permettent uniquement d’afficher ou de masquer les dossiers DFS, pas de contrôler l’accès, rendant l’accès en lecture la seule autorisation pertinente du niveau de dossiers DFS. Pour plus d’informations, voir [Utilisation d’autorisations héritées avec l’énumération basée sur l’accès](https://technet.microsoft.com/library/dd834874(v=ws.11).aspx)

<br />
Vous pouvez activer l’énumération basée sur l’accès pour un espace de noms à l’aide de l’interface Windows ou d’une ligne de commande.

## <a name="to-enable-access-based-enumeration-by-using-the-windows-interface"></a>Pour activer l’énumération basée sur l’accès à l’aide de l’interface Windows

1.  Dans l’arborescence de la console, sous le nœud **Espaces de noms**, cliquez avec le bouton droit sur l’espace de noms de domaine approprié, puis cliquez sur **Propriétés**.

2.  Cliquez sur l’onglet **Avancé**, puis activez la case à cocher **Activer l’énumération basée sur l’accès pour cet espace de noms**.

## <a name="to-enable-access-based-enumeration-by-using-a-command-line"></a>Pour activer l’énumération basée sur l’accès à l’aide d’une ligne de commande

1.  Ouvrez une fenêtre d’invite de commandes sur un serveur sur lequel est installé le service de rôle **Système de fichiers DFS** ou la fonctionnalité **Outils du système de fichiers DFS**.

2.  Tapez la commande suivante, où *< espace de noms\_racine >* est la racine de l’espace de noms :

    ```  
    dfsutil property abe enable \\ <namespace_root>
    ```

> [!TIP]
> Pour gérer l’énumération basée sur l’accès pour un espace de noms à l’aide de Windows PowerShell, utilisez les [applets de commande Set-DfsnRoot](https://technet.microsoft.com/library/jj884281.aspx), [Grant-DfsnAccess](https://technet.microsoft.com/library/jj884272.aspx) et [Revoke-DfsnAccess](https://technet.microsoft.com/library/jj884273.aspx). Le module Windows PowerShell DFSN a été introduit dans Windows Server 2012.

Vous pouvez contrôler les utilisateurs et les groupes qui peuvent afficher des dossiers DFS à l’aide de l’interface Windows ou d’une ligne de commande.

## <a name="to-control-folder-visibility-by-using-the-windows-interface"></a>Pour contrôler la visibilité des dossiers à l’aide de l’interface Windows

1.  Dans l’arborescence de la console, sous le nœud **Espaces de noms**, recherchez le dossier avec cibles dont vous voulez contrôler la visibilité, cliquez dessus avec le bouton droit, puis cliquez sur **Propriétés**.

2.  Cliquez sur l'onglet **Avancé**.

3.  Cliquez sur **Définir des autorisations d’affichage explicites sur le dossier DFS**, puis **Configurer les autorisations d’affichage**.

4.  Ajoutez ou supprimez des groupes ou des utilisateurs en cliquant sur **Ajouter** ou **Supprimer**.

5.  Pour autoriser les utilisateurs à voir le dossier DFS, sélectionnez le groupe ou l’utilisateur, puis activez la case à cocher **Autoriser**.

    Pour masquer le dossier à un groupe ou à un utilisateur, sélectionnez le groupe ou l’utilisateur, puis activez la case à cocher **Refuser**.

## <a name="to-control-folder-visibility-by-using-a-command-line"></a>Pour contrôler la visibilité des dossiers à l’aide de la ligne de commande

1. Ouvrez une fenêtre d’invite de commandes sur un serveur sur lequel est installé le service de rôle **Système de fichiers DFS** ou la fonctionnalité **Outils du système de fichiers DFS**.

2. Tapez la commande suivante, où *&lt;DFSPath&gt;* est le chemin d’accès du dossier DFS (lien), *< domaine\\compte >* est le nom du compte d’utilisateur ou de groupe et *(...)*  est remplacé par les autres entrées de contrôle d’accès (ACE) :

   ```
   dfsutil property sd grant <DFSPath> DOMAIN\Account:R (...) Protect Replace
   ```

   Par exemple, pour remplacer les autorisations existantes avec des autorisations qui permet les Admins du domaine CONTOSO\\formateurs groupes Read (R) l’accès à la \\contoso.office\public\training dossier, tapez la commande suivante :

   ```
   dfsutil property sd grant \\contoso.office\public\training "CONTOSO\Domain Admins":R CONTOSO\Trainers:R Protect Replace 
   ```

3. Pour effectuer des tâches supplémentaires à partir de l’invite de commandes, utilisez les commandes suivantes :


| Command | Description |
|---|---|
|[Propriété de Dfsutil sd refuser](https://msdn.microsoft.com/library/dd759150(v=ws.11).aspx)|Refuse à un groupe ou un utilisateur la possibilité d’afficher le dossier.|
|[Propriété de Dfsutil sd réinitialiser](https://msdn.microsoft.com/library/dd759150(v=ws.11).aspx) |Supprime toutes les autorisations du dossier.|
|[Revoke de Dfsutil propriété sd](https://msdn.microsoft.com/library/dd759150(v=ws.11).aspx)| Supprime une entrée de contrôle d’accès d’un groupe ou d’un utilisateur du dossier. |

## <a name="see-also"></a>Voir aussi

-   [Créer un espace de noms DFS](create-a-dfs-namespace.md)
-   [Déléguer les autorisations de gestion pour les espaces de noms DFS](delegate-management-permissions-for-dfs-namespaces.md)
-   [L’installation de DFS](https://technet.microsoft.com/library/cc731089(v=ws.11).aspx)
-   [À l’aide d’autorisations héritées avec l’énumération basée sur l’accès](using-inherited-permissions-with-access-based-enumeration.md)