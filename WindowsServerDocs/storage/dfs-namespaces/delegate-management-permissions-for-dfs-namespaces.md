---
title: "Déléguer les autorisations de gestion pour les espaces de nomsDFS"
description: "Cet article décrit comment déléguer des autorisations de gestion pour les espaces de noms DFS et détaille les groupes autorisés par défaut à réaliser des tâches liées aux espaces de noms"
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e584b49639a83e4ab1da142a999741ae4ac7ff84
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="delegate-management-permissions-for-dfs-namespaces"></a>Déléguer les autorisations de gestion pour les espaces de nomsDFS

> S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2, WindowsServer2008

Le tableau suivant présente les groupes autorisés par défaut à réaliser les tâches de base liées aux espaces de noms et les procédures permettant de déléguer l’exécution de ces tâches:

|Tâche | Groupes autorisés par défaut à effectuer cette tâche | Méthode de délégation |
|---|---|---|
|Créer un espace de noms basé sur un domaine|Groupe Admins du domaine dans le domaine où l’espace de noms est configuré|Cliquez avec le bouton droit sur le nœud **Espaces de noms** dans l’arborescence de la console, puis cliquez sur **Déléguer les autorisations de gestion**. Vous pouvez également utiliser [Set-DfsnRoot GrantAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot) et [Set-DfsnRoot RevokeAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot). Applets de commande WindowsPowerShell (introduits dans WindowsServer2012). Vous devez également ajouter l’utilisateur au groupe Administrateurs local sur le serveur d’espaces de noms.|
|Ajouter un serveur d’espaces de noms dans un espace de noms basé sur un domaine|Groupe Admins du domaine dans le domaine où l’espace de noms est configuré| Cliquez avec le bouton droit sur l’espace de noms basé sur un domaine dans l’arborescence de la console, puis cliquez sur **Déléguer les autorisations de gestion**. Vous pouvez également utiliser [Set-DfsnRoot GrantAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot) et [Set-DfsnRoot RevokeAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot). Applets de commande WindowsPowerShell (introduits dans WindowsServer2012). Vous devez également ajouter l’utilisateur au groupe Administrateurs local sur le serveur d’espaces de noms à ajouter.|
|Gérer un espace de noms basé sur un domaine|Groupe Administrateurs local sur chaque serveur d’espaces de noms| Cliquez avec le bouton droit sur l’espace de noms basé sur un domaine dans l’arborescence de la console, puis cliquez sur **Déléguer les autorisations de gestion**. |
|Créer un espace de noms autonome|Groupe Administrateurs local sur le serveur d’espaces de noms.| Ajoutez l’utilisateur au groupe Administrateurs local sur le serveur d’espaces de noms. |
|Gérer un espace de noms autonome*|Groupe Administrateurs local sur le serveur d’espaces de noms.| Cliquez avec le bouton droit sur l’espace de noms autonome dans l’arborescence de la console, puis cliquez sur **Déléguer les autorisations de gestion**. Vous pouvez également utiliser [Set-DfsnRoot GrantAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot) et [Set-DfsnRoot RevokeAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot). Applets de commande WindowsPowerShell (introduits dans WindowsServer2012).|
|Créer un groupe de réplication ou activer la réplicationDFS pour un dossier|Groupe Admins du domaine dans le domaine où l’espace de noms est configuré| Cliquez avec le bouton droit sur le nœud Réplication dans l’arborescence de la console, puis cliquez sur **Déléguer les autorisations de gestion**. |

<br />

\*La délégation des autorisations de gestion pour un espace de noms autonome n’accorde pas à l’utilisateur l’autorisation d’afficher et de gérer la sécurité par le biais de l’onglet **Délégation**, sauf si l’utilisateur est membre du groupe Administrateurs local sur le serveur d’espace de noms. Ce problème est dû au fait que le composant logiciel enfichable GestionDFS ne peut pas récupérer les listes de contrôle d’accès discrétionnaire (DACL) de l’espace de noms autonome à partir du Registre. Pour permettre au composant logiciel enfichable d’afficher les informations de délégation, suivez la procédure décrite dans l’article de la Base de connaissances Microsoft<sup>®</sup> [KB314837: Comment faire pour gérer un accès distant au Registre](http://go.microsoft.com/fwlink?linkid=46803)