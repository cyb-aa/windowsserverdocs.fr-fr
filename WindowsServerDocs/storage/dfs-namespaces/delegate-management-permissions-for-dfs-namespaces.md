---
title: Déléguer les autorisations de gestion pour les espaces de noms DFS
description: Cet article décrit comment déléguer des autorisations de gestion pour les espaces de noms DFS et détaille les groupes autorisés par défaut à réaliser des tâches liées aux espaces de noms
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 5bf23498c95d4b44d5c17aecd216921dc70819a3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402214"
---
# <a name="delegate-management-permissions-for-dfs-namespaces"></a>Déléguer les autorisations de gestion pour les espaces de noms DFS

> S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008

Le tableau suivant présente les groupes autorisés par défaut à réaliser les tâches de base liées aux espaces de noms et les procédures permettant de déléguer l’exécution de ces tâches :

|Tâche | Groupes autorisés par défaut à effectuer cette tâche | Méthode de délégation |
|---|---|---|
|Créer un espace de noms basé sur un domaine|Groupe Admins du domaine dans le domaine où l’espace de noms est configuré|Cliquez avec le bouton droit sur le nœud **Espaces de noms** dans l’arborescence de la console, puis cliquez sur **Déléguer les autorisations de gestion**. Vous pouvez également utiliser [Set-DfsnRoot GrantAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot) et [Set-DfsnRoot RevokeAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot). Applets de commande Windows PowerShell (introduits dans Windows Server 2012). Vous devez également ajouter l’utilisateur au groupe Administrateurs local sur le serveur d’espaces de noms.|
|Ajouter un serveur d’espaces de noms dans un espace de noms basé sur un domaine|Groupe Admins du domaine dans le domaine où l’espace de noms est configuré| Cliquez avec le bouton droit sur l’espace de noms basé sur un domaine dans l’arborescence de la console, puis cliquez sur **Déléguer les autorisations de gestion**. Vous pouvez également utiliser [Set-DfsnRoot GrantAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot) et [Set-DfsnRoot RevokeAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot). Applets de commande Windows PowerShell (introduits dans Windows Server 2012). Vous devez également ajouter l’utilisateur au groupe Administrateurs local sur le serveur d’espaces de noms à ajouter.|
|Gérer un espace de noms basé sur un domaine|Groupe Administrateurs local sur chaque serveur d’espaces de noms| Cliquez avec le bouton droit sur l’espace de noms basé sur un domaine dans l’arborescence de la console, puis cliquez sur **Déléguer les autorisations de gestion**. |
|Créer un espace de noms autonome|Groupe Administrateurs local sur le serveur d’espaces de noms.| Ajoutez l’utilisateur au groupe Administrateurs local sur le serveur d’espaces de noms. |
|Gérer un espace de noms autonome*|Groupe Administrateurs local sur le serveur d’espaces de noms.| Cliquez avec le bouton droit sur l’espace de noms autonome dans l’arborescence de la console, puis cliquez sur **Déléguer les autorisations de gestion**. Vous pouvez également utiliser [Set-DfsnRoot GrantAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot) et [Set-DfsnRoot RevokeAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot). Applets de commande Windows PowerShell (introduits dans Windows Server 2012).|
|Créer un groupe de réplication ou activer la réplication DFS pour un dossier|Groupe Admins du domaine dans le domaine où l’espace de noms est configuré| Cliquez avec le bouton droit sur le nœud Réplication dans l’arborescence de la console, puis cliquez sur **Déléguer les autorisations de gestion**. |

<br />

les autorisations de gestion \*Delegating pour gérer un espace de noms autonome n’autorisent pas l’utilisateur à afficher et à gérer la sécurité à l’aide de l’onglet **délégation** , sauf si l’utilisateur est membre du groupe Administrateurs local sur le serveur d’espaces de noms. Ce problème est dû au fait que le composant logiciel enfichable Gestion DFS ne peut pas récupérer les listes de contrôle d’accès discrétionnaire (DACL) de l’espace de noms autonome à partir du Registre. Pour permettre au composant logiciel enfichable d’afficher les informations de délégation, vous devez suivre les étapes de l’article de la base de connaissances Microsoft<sup>®</sup> : @NO__T 0KB314837 : Gestion de l’accès à distance au registre @ no__t-0