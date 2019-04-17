---
title: "Activer l’énumération basée sur l’accès pour un espace de noms"
description: "Cet article décrit comment activer l’énumération basée sur l’accès pour un espace de noms."
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 8c7ff3bde0a88c7c64cb5b1b6e656adab0e14452
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="enable-access-based-enumeration-on-a-namespace"></a>Activer l’énumération basée sur l’accès pour un espace de noms

> S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2, WindowsServer2008

L’énumération basée sur l’accès masque les fichiers et les dossiers auxquels les utilisateurs ne sont pas autorisés à accéder. Par défaut, cette fonctionnalité n’est pas activée pour les espaces de nomsDFS. Vous pouvez activer l’énumération basée sur l’accès des dossiersDFS à l’aide de GestionDFS. Pour contrôler l’énumération basée sur l’accès des fichiers et des dossiers dans les cibles de dossier, vous devez activer l’énumération basée sur l’accès pour chaque dossier partagé à l’aide de Gestion du partage et du stockage.

Pour activer l’énumération basée sur l’accès pour un espace de noms, tous les serveurs d’espaces de noms doivent exécuter WindowsServer2008 ou version ultérieure. En outre, les espaces de noms basés sur un domaine doivent utiliser le mode WindowsServer2008. Pour plus d’informations sur la configuration requise par le mode WindowsServer2008, voir [Choisir un type d’espace de noms](choose-a-namespace-type.md).

Dans certains environnements, l’activation de l’énumération basée sur l’accès peut entraîner une utilisation élevée du processeur sur le serveur et des temps de réponse lents pour les utilisateurs.

> [!NOTE]
> Si vous augmentez le niveau fonctionnel de domaine à WindowsServer2008 alors qu’il existe des espaces de noms basés sur un domaine, le composant logiciel enfichable Gestion DFS vous permettra d’activer l’énumération basée sur l’accès pour ces espaces de noms. Toutefois, vous n’aurez pas la possibilité de modifier les autorisations pour cacher des dossiers à des groupes ou utilisateurs, à moins de migrer les espaces de noms vers le mode WindowsServer2008. Pour plus d’informations, voir [Migrer un espace de noms basé sur un domaine vers le mode WindowsServer2008](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md).


Pour utiliser l’énumération basée sur l’accès avec des espaces de nomsDFS, vous devez effectuer les étapes suivantes:

-   Activer l’énumération basée sur l’accès pour un espace de noms
-   Contrôler les utilisateurs et les groupes qui peuvent afficher des dossiersDFS individuels


> [!WARNING]
> L’énumération basée sur l’accès n’empêche pas les utilisateurs d’obtenir une référence vers une cible de dossier s’ils connaissent déjà le chemin d’accèsDFS. Seules les autorisations de partage ou les autorisations du système de fichiers NTFS de la cible de dossier (dossier partagé) peuvent empêcher les utilisateurs d’accéder à une cible de dossier. Les autorisations de dossiers DFS permettent uniquement d’afficher ou de masquer les dossiersDFS, pas de contrôler l’accès, rendant l’accès en lecture la seule autorisation pertinente du niveau de dossiersDFS. Pour plus d’informations, voir [Utilisation d’autorisations héritées avec l’énumération basée sur l’accès](https://technet.microsoft.com/library/dd834874(v=ws.11).aspx)

<br />
Vous pouvez activer l’énumération basée sur l’accès pour un espace de noms à l’aide de l’interface Windows ou d’une ligne de commande.

## <a name="to-enable-access-based-enumeration-by-using-the-windows-interface"></a>Pour activer l’énumération basée sur l’accès à l’aide de l’interface Windows

1.  Dans l’arborescence de la console, sous le nœud **Espaces de noms**, cliquez avec le bouton droit sur l’espace de noms de domaine approprié, puis cliquez sur **Propriétés**.

2.  Cliquez sur l’onglet **Avancé**, puis activez la case à cocher **Activer l’énumération basée sur l’accès pour cet espace de noms**.

## <a name="to-enable-access-based-enumeration-by-using-a-command-line"></a>Pour activer l’énumération basée sur l’accès à l’aide d’une ligne de commande

1.  Ouvrez une fenêtre d’invite de commandes sur un serveur sur lequel est installé le service de rôle **Système de fichiersDFS** ou la fonctionnalité **Outils du système de fichiersDFS**.

2.  Tapez la commande suivante, où *<namespace\_root>* est la racine de l’espace de noms:

    ```  
    dfsutil property abe enable \\ <namespace_root>
    ```

> [!TIP]
> Pour gérer l’énumération basée sur l’accès pour un espace de noms à l’aide de WindowsPowerShell, utilisez les [applets de commande Set-DfsnRoot](https://technet.microsoft.com/library/jj884281.aspx), [Grant-DfsnAccess](https://technet.microsoft.com/library/jj884272.aspx) et [Revoke-DfsnAccess](https://technet.microsoft.com/library/jj884273.aspx). Le module WindowsPowerShell DFSN a été introduit dans WindowsServer2012.

Vous pouvez contrôler les utilisateurs et les groupes qui peuvent afficher des dossiersDFS à l’aide de l’interface Windows ou d’une ligne de commande.

## <a name="to-control-folder-visibility-by-using-the-windows-interface"></a>Pour contrôler la visibilité des dossiers à l’aide de l’interface Windows

1.  Dans l’arborescence de la console, sous le nœud **Espaces de noms**, recherchez le dossier avec cibles dont vous voulez contrôler la visibilité, cliquez dessus avec le bouton droit, puis cliquez sur **Propriétés**.

2.  Cliquez sur l’onglet **Avancé**.

3.  Cliquez sur **Définir des autorisations d’affichage explicites sur le dossierDFS**, puis **Configurer les autorisations d’affichage**.

4.  Ajoutez ou supprimez des groupes ou des utilisateurs en cliquant sur **Ajouter** ou **Supprimer**.

5.  Pour autoriser les utilisateurs à voir le dossierDFS, sélectionnez le groupe ou l’utilisateur, puis activez la case à cocher **Autoriser**.

    Pour masquer le dossier à un groupe ou à un utilisateur, sélectionnez le groupe ou l’utilisateur, puis activez la case à cocher **Refuser**.

## <a name="to-control-folder-visibility-by-using-a-command-line"></a>Pour contrôler la visibilité des dossiers à l’aide de la ligne de commande

1.  Ouvrez une fenêtre d’invite de commandes sur un serveur sur lequel est installé le service de rôle **Système de fichiersDFS** ou la fonctionnalité **Outils du système de fichiersDFS**.

2.  Tapez la commande suivante, où *&lt;DFSPath&gt;* est le chemin d’accès du dossier DFS (lien), *<DOMAIN\\Account>* est le nom du compte d’utilisateur ou de groupe et *(...)* est remplacé par des entrées de contrôle d’accès supplémentaires:

    ```
    dfsutil property sd grant <DFSPath> DOMAIN\Account:R (...) Protect Replace
    ```

    Par exemple, pour remplacer les autorisations existantes par des autorisations qui accordent aux groupes Admins du domaine et CONTOSO\\Trainers un accès en lecture (R) au dossier \\contoso.office\public\training, tapez la commande suivante:

   ```
   dfsutil property sd grant \\contoso.office\public\training "CONTOSO\Domain Admins":R CONTOSO\Trainers:R Protect Replace 
   ```

3. Pour effectuer des tâches supplémentaires à partir de l’invite de commandes, utilisez les commandes suivantes:


| Commande | Description |
|---|---|
|[Dfsutil property sd deny](https://msdn.microsoft.com/library/dd759150(v=ws.11).aspx)|Refuse à un groupe ou un utilisateur la possibilité d’afficher le dossier.|
|[Dfsutil property sd reset](https://msdn.microsoft.com/library/dd759150(v=ws.11).aspx) |Supprime toutes les autorisations du dossier.|
|[Dfsutil property sd revoke](https://msdn.microsoft.com/library/dd759150(v=ws.11).aspx)| Supprime une entrée de contrôle d’accès d’un groupe ou d’un utilisateur du dossier. |

## <a name="see-also"></a>Articles associés

-   [Créer un espace de nomsDFS](create-a-dfs-namespace.md)
-   [Déléguer les autorisations de gestion pour les espaces de nomsDFS](delegate-management-permissions-for-dfs-namespaces.md)
-   [Installation du système DFS](https://technet.microsoft.com/library/cc731089(v=ws.11).aspx)
-   [Utilisation d’autorisations héritées avec l’énumération basée sur l’accès](using-inherited-permissions-with-access-based-enumeration.md)