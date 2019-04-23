---
title: Comptes d’utilisateur multiPoint Services
description: En savoir plus sur les comptes d’utilisateur dans MultiPoint Services, en particulier le type à utiliser pour différents scénarios
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7f3c6ce5-9b7c-45a0-83c5-3f9b9f5f48d4
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 31279f81d5af597b0b1f1729c953fefaf24a214f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855360"
---
# <a name="example-scenarios-multipoint-services-user-accounts"></a>Exemples de scénario : Comptes d’utilisateur multiPoint Services
Que devez-vous faire pour implémenter le scénario de compte d’utilisateur que vous avez choisi pour votre environnement MultiPoint Services ? Les tableaux suivants décrivent chaque tâche à effectuer pour configurer les comptes d’utilisateur et de préparer des stations pour les comptes d’utilisateur individuelle ou partagée sur un ordinateur MultiPoint autonome ou sur des serveurs en réseau dans un groupe de travail ou un domaine Active Directory. Choisissez le scénario qui s’applique à votre environnement. Puis suivez les liens dans la table pour effectuer chaque tâche de configuration requise.  
  
> [!NOTE]  
> Si vous n’avez pas encore décidé comment configurer vos comptes d’utilisateur, consultez [planifier des comptes d’utilisateur pour votre environnement MultiPoint Services](Plan-user-accounts-for-your-MultiPoint-services-environment.md) pour plus d’informations sur la façon dont chaque choix affecte les utilisateurs.  
  
## <a name="single-multipoint-services-computer-in-a-stand-alone-environment-no-network"></a>Ordinateur de Services MultiPoint d’unique dans un environnement autonome (aucun réseau)  
  
|||  
|-|-|  
|**Mes utilisateurs n’avez pas besoin de se connecter.** Les stations peuvent être disponibles à toute personne qui remonte à eux. Ils n’avez pas besoin d’une expérience du bureau Windows individuelle qui inclut des dossiers privés pour stocker des données ou des postes de travail personnalisés.|1.  Créer un compte d’utilisateur local (pour obtenir des instructions, consultez [créer des comptes d’utilisateurs locaux](Create-local-user-accounts.md).)<br />2.  [Autoriser plusieurs sessions sur un même compte](Allow-one-account-to-have-multiple-sessions.md)<br />3.  [Configurer des stations pour la connexion automatique](Configure-stations-for-automatic-logon.md)|  
|**Mes utilisateurs peuvent partager le même nom de connexion utilisateur.** Ils n’avez pas besoin d’une expérience du bureau Windows individuelle qui inclut des dossiers privés pour stocker des données ou des postes de travail personnalisés.|1.  Créer un compte d’utilisateur local (pour obtenir des instructions, consultez [créer des comptes d’utilisateurs locaux](Create-local-user-accounts.md).)<br />2.  [Autoriser plusieurs sessions sur un même compte](Allow-one-account-to-have-multiple-sessions.md)|  
|**Mes utilisateurs doivent avoir leur propre expérience utilisateur Windows individuel.**|Créer un compte d’utilisateur local pour chaque utilisateur (pour obtenir des instructions, consultez [créer des comptes d’utilisateurs locaux](Create-local-user-accounts.md).)|  
  
## <a name="multiple-multipoint-services-computers-on-a-network-but-with-no-domain"></a>Plusieurs ordinateurs de MultiPoint Services sur un réseau, mais avec aucun domaine.  
  
|||  
|-|-|  
|**Mes utilisateurs n’avez pas besoin de se connecter.** Les stations peuvent être disponibles à toute personne qui remonte à eux. Ils n’avez pas besoin d’une expérience du bureau Windows individuelle qui inclut des dossiers privés pour stocker des données ou des postes de travail personnalisés.|1.  Créer un compte d’utilisateur local sur chaque serveur. (Pour obtenir des instructions, consultez [créer des comptes d’utilisateurs locaux](Create-local-user-accounts.md).)<br />2.  [Autoriser plusieurs sessions sur un même compte](Allow-one-account-to-have-multiple-sessions.md) sur chaque serveur<br />3.  [Configurer des stations pour la connexion automatique](Configure-stations-for-automatic-logon.md) sur chaque serveur|  
|**Mes utilisateurs peuvent partager le même nom de connexion utilisateur.** Ils n’avez pas besoin d’une expérience du bureau Windows individuelle qui inclut des dossiers privés pour stocker des données ou des postes de travail personnalisés.|1.  Créer un compte d’utilisateur local sur chaque serveur. (Pour obtenir des instructions, consultez [créer des comptes d’utilisateurs locaux](Create-local-user-accounts.md).)<br />2.  [Autoriser plusieurs sessions sur un même compte](Allow-one-account-to-have-multiple-sessions.md) sur chaque serveur.|  
|**Mes utilisateurs doivent avoir leur propre expérience utilisateur Windows individuel.**<br /><br />-   **Option A** -mes utilisateurs utilisent toujours des stations locales connectées au même ordinateur MultiPoint Services.<br />-   **Option B** -mes utilisateurs utilisera des stations locales sur plusieurs ordinateurs de MultiPoint Services.<br />-   **Option C** -mes utilisateurs se serviront les clients distants sur le réseau local.|-   **Option A** -créer un compte d’utilisateur local sur chaque serveur pour les utilisateurs de ce serveur. (Pour obtenir des instructions, consultez [créer des comptes d’utilisateurs locaux](Create-local-user-accounts.md).)<br />-   **Option B** -créer des comptes d’utilisateur local pour chaque utilisateur sur chaque serveur. **Remarque :** Cela signifie que chaque utilisateur aura un profil sur chaque serveur. En d’autres termes, si ils enregistrent un fichier dans Mes Documents ouvert une session sur la station de serveur de A, ils verra pas le fichier lors de la connexion sur la station du serveur B. (Pour obtenir des instructions, consultez [créer des comptes d’utilisateurs locaux](Create-local-user-accounts.md).)<br />-   **Option C** -attribuer à chaque utilisateur à un ordinateur MultiPoint Services spécifique. Créer des comptes d’utilisateurs locaux pour les utilisateurs affectés sur chaque serveur. (Pour obtenir des instructions, consultez [créer des comptes d’utilisateurs locaux](Create-local-user-accounts.md).)|  
  
## <a name="one-or-more-multipoint-services-computers-in-a-domain-network-environment"></a>Un ou plusieurs ordinateurs de MultiPoint Services dans un environnement de réseau de domaine  
  
|||  
|-|-|  
|**Mes utilisateurs n’avez pas besoin de se connecter.** Les stations peuvent être disponibles à toute personne qui remonte à eux. Ils n’avez pas besoin d’une expérience du bureau Windows individuelle qui inclut des dossiers privés pour stocker des données ou des postes de travail personnalisés.|1.  Créer un compte de domaine pour se connecter les serveurs.<br />2.  [Autoriser plusieurs sessions sur un même compte](Allow-one-account-to-have-multiple-sessions.md) sur chaque serveur.<br />3.  [Configurer des stations pour la connexion automatique](Configure-stations-for-automatic-logon.md) sur chaque serveur.|  
|**Mes utilisateurs peuvent partager le même nom de connexion utilisateur.** Ils n’avez pas besoin d’une expérience du bureau Windows individuelle qui inclut des dossiers privés pour stocker des données ou des postes de travail personnalisés.|1.  Créer un compte de domaine pour un groupe ou pour chaque utilisateur.<br />2.  [Autoriser plusieurs sessions sur un même compte](Allow-one-account-to-have-multiple-sessions.md) sur chaque serveur.|  
|**Mes utilisateurs doivent avoir leur propre expérience utilisateur Windows individuel.**<br /><br />-   **Option A** -tout utilisateur avec un compte de domaine peut utiliser l’ordinateur MultiPoint Services.<br />-   **Option B** -je souhaite limiter les comptes de domaine peuvent accéder au serveur.|-   **Option A** -n’exige aucune configuration. Par défaut, tous les utilisateurs de domaine ont accès à n’importe quel ordinateur MultiPoint Services sur le réseau.<br />-   **Option B** -limiter l’accès des comptes d’utilisateur de domaine pour l’ordinateur MultiPoint Services. Pour obtenir des instructions, consultez [limiter l’accès des utilisateurs au serveur](limit-users--access-to-the-server-in-multipoint-services.md).|  
|**Je souhaite utiliser des comptes d’utilisateurs locaux et les gérer séparément à partir de mes comptes de domaine.** Par exemple, vous souhaitez qu’une personne à gérer MultiPoint Services mais pas le domaine, ou vous ne souhaitez pas permettre à des comptes de domaine à tous les utilisateurs MultiPoint Services.|Créer un ou plusieurs comptes d’utilisateur local sur chaque serveur. (Pour obtenir des instructions, consultez [créer des comptes d’utilisateurs locaux](Create-local-user-accounts.md).)<br /><br />**Remarque :** Cela signifie que chaque compte d’utilisateur aura un profil sur chaque serveur. En d’autres termes, si ils enregistrent un fichier dans Mes Documents ouvert une session sur la station de serveur de A, ils verra pas le fichier lors de la connexion sur la station du serveur B.|  