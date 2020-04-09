---
title: Comptes d’utilisateur MultiPoint services
description: En savoir plus sur les comptes d’utilisateur dans MultiPoint services, en particulier sur le type à utiliser pour différents scénarios
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 7f3c6ce5-9b7c-45a0-83c5-3f9b9f5f48d4
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 0933eb856182239ef6940079f7d6c9700a868e60
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858882"
---
# <a name="example-scenarios-multipoint-services-user-accounts"></a>Exemples de scénarios : comptes d’utilisateur MultiPoint Services
Que devez-vous faire pour implémenter le scénario de compte d’utilisateur que vous avez choisi pour votre environnement MultiPoint services ? Les tableaux suivants décrivent chaque tâche à effectuer pour configurer les comptes d’utilisateur et préparer les stations pour les comptes d’utilisateurs partagés ou individuels sur un ordinateur MultiPoint autonome ou sur des serveurs en réseau dans un groupe de travail ou un domaine Active Directory. Choisissez le scénario qui s’applique à votre environnement. Suivez ensuite les liens dans le tableau pour terminer chaque tâche de configuration requise.  
  
> [!NOTE]  
> Si vous n’avez pas encore décidé comment configurer vos comptes d’utilisateur, consultez [planifier des comptes d’utilisateur pour votre environnement multipoint services](Plan-user-accounts-for-your-MultiPoint-services-environment.md) pour plus d’informations sur la façon dont chaque choix affecte les utilisateurs.  
  
## <a name="single-multipoint-services-computer-in-a-stand-alone-environment-no-network"></a>Un seul ordinateur MultiPoint services dans un environnement autonome (aucun réseau)  
  
|||  
|-|-|  
|**Mes utilisateurs n’ont pas besoin de se connecter.** Les stations peuvent être accessibles à toute personne qui les parcourt. Ils n’ont pas besoin d’une expérience de bureau Windows individuelle qui comprend des dossiers privés pour le stockage des données ou des postes de travail personnalisés.|1. Créez un seul compte d’utilisateur local (pour obtenir des instructions, consultez [créer des comptes d’utilisateurs locaux](Create-local-user-accounts.md).)<br />2. [autoriser un compte à avoir plusieurs sessions](Allow-one-account-to-have-multiple-sessions.md)<br />3. [configurer les stations pour une ouverture de session automatique](Configure-stations-for-automatic-logon.md)|  
|**Mes utilisateurs peuvent tous partager la même ouverture de session utilisateur.** Ils n’ont pas besoin d’une expérience de bureau Windows individuelle qui comprend des dossiers privés pour le stockage des données ou des postes de travail personnalisés.|1. Créez un seul compte d’utilisateur local (pour obtenir des instructions, consultez [créer des comptes d’utilisateurs locaux](Create-local-user-accounts.md).)<br />2. [autoriser un compte à avoir plusieurs sessions](Allow-one-account-to-have-multiple-sessions.md)|  
|**Mes utilisateurs doivent avoir leur propre expérience de bureau Windows.**|Créez un compte d’utilisateur local pour chaque utilisateur (pour obtenir des instructions, consultez [créer des comptes d’utilisateurs locaux](Create-local-user-accounts.md).)|  
  
## <a name="multiple-multipoint-services-computers-on-a-network-but-with-no-domain"></a>Plusieurs ordinateurs MultiPoint services sur un réseau, mais sans domaine  
  
|||  
|-|-|  
|**Mes utilisateurs n’ont pas besoin de se connecter.** Les stations peuvent être accessibles à toute personne qui les parcourt. Ils n’ont pas besoin d’une expérience de bureau Windows individuelle qui comprend des dossiers privés pour le stockage des données ou des postes de travail personnalisés.|1. Créez un seul compte d’utilisateur local sur chaque serveur. (Pour obtenir des instructions, consultez [créer des comptes d’utilisateurs locaux](Create-local-user-accounts.md).)<br />2. [autoriser un compte à avoir plusieurs sessions](Allow-one-account-to-have-multiple-sessions.md) sur chaque serveur<br />3. [configurer les stations pour une ouverture de session automatique](Configure-stations-for-automatic-logon.md) sur chaque serveur|  
|**Mes utilisateurs peuvent tous partager la même ouverture de session utilisateur.** Ils n’ont pas besoin d’une expérience de bureau Windows individuelle qui comprend des dossiers privés pour le stockage des données ou des postes de travail personnalisés.|1. Créez un seul compte d’utilisateur local sur chaque serveur. (Pour obtenir des instructions, consultez [créer des comptes d’utilisateurs locaux](Create-local-user-accounts.md).)<br />2. [autoriser un compte à avoir plusieurs sessions](Allow-one-account-to-have-multiple-sessions.md) sur chaque serveur.|  
|**Mes utilisateurs doivent avoir leur propre expérience de bureau Windows.**<p>**option de -   A** -mes utilisateurs utilisent toujours les stations locales connectées au même ordinateur multipoint services.<br />Option de -   **B** -mes utilisateurs utiliseront des stations locales sur plusieurs ordinateurs multipoint services.<br />-   **option C** -mes utilisateurs utiliseront des clients distants sur le réseau local.|-   **option A** -créez un compte d’utilisateur local unique sur chaque serveur pour les utilisateurs de ce serveur. (Pour obtenir des instructions, consultez [créer des comptes d’utilisateurs locaux](Create-local-user-accounts.md).)<br />-   **option B** -créer des comptes d’utilisateurs locaux pour chaque utilisateur sur chaque serveur. **Remarque :** Cela signifie que chaque utilisateur aura un profil sur chaque serveur. En d’autres termes, s’ils enregistrent un fichier dans mes documents lors de la connexion à la station de serveur A, ils ne voient pas le fichier lors de la connexion à la station du serveur B. (Pour obtenir des instructions, consultez [créer des comptes d’utilisateurs locaux](Create-local-user-accounts.md).)<br />-   **option C** -assigner chaque utilisateur à un ordinateur multipoint services spécifique. Créez des comptes d’utilisateur locaux pour les utilisateurs affectés sur chaque serveur. (Pour obtenir des instructions, consultez [créer des comptes d’utilisateurs locaux](Create-local-user-accounts.md).)|  
  
## <a name="one-or-more-multipoint-services-computers-in-a-domain-network-environment"></a>Un ou plusieurs ordinateurs MultiPoint services dans un environnement de réseau de domaine  
  
|||  
|-|-|  
|**Mes utilisateurs n’ont pas besoin de se connecter.** Les stations peuvent être accessibles à toute personne qui les parcourt. Ils n’ont pas besoin d’une expérience de bureau Windows individuelle qui comprend des dossiers privés pour le stockage des données ou des postes de travail personnalisés.|1. Créez un compte de domaine pour vous connecter aux serveurs.<br />2. [autoriser un compte à avoir plusieurs sessions](Allow-one-account-to-have-multiple-sessions.md) sur chaque serveur.<br />3. [configurez les stations pour une ouverture de session automatique](Configure-stations-for-automatic-logon.md) sur chaque serveur.|  
|**Mes utilisateurs peuvent tous partager la même ouverture de session utilisateur.** Ils n’ont pas besoin d’une expérience de bureau Windows individuelle qui comprend des dossiers privés pour le stockage des données ou des postes de travail personnalisés.|1. Créez un compte de domaine pour un groupe ou pour chaque utilisateur.<br />2. [autoriser un compte à avoir plusieurs sessions](Allow-one-account-to-have-multiple-sessions.md) sur chaque serveur.|  
|**Mes utilisateurs doivent avoir leur propre expérience de bureau Windows.**<p>-   **option A** -tout utilisateur disposant d’un compte de domaine peut utiliser l’ordinateur multipoint services.<br />-   **option B** -je souhaite limiter les comptes de domaine qui peuvent accéder au serveur.|**option de -   a** -aucune configuration n’est requise. Par défaut, tous les utilisateurs du domaine ont accès à n’importe quel ordinateur MultiPoint services sur le réseau.<br />-   **option B** -limiter l’accès des comptes d’utilisateur de domaine à l’ordinateur multipoint services. Pour obtenir des instructions, consultez [limiter l’accès des utilisateurs au serveur](limit-users--access-to-the-server-in-multipoint-services.md).|  
|**Je souhaite utiliser des comptes d’utilisateurs locaux et les gérer séparément de mes comptes de domaine.** Par exemple, vous souhaitez que quelqu’un gère les services MultiPoint, mais pas le domaine, ou que vous ne souhaitiez pas octroyer des comptes de domaine à tous les utilisateurs de MultiPoint services.|Créez un ou plusieurs comptes d’utilisateurs locaux sur chaque serveur. (Pour obtenir des instructions, consultez [créer des comptes d’utilisateurs locaux](Create-local-user-accounts.md).)<p>**Remarque :** Cela signifie que chaque compte d’utilisateur aura un profil sur chaque serveur. En d’autres termes, s’ils enregistrent un fichier dans mes documents lors de la connexion à la station de serveur A, ils ne voient pas le fichier lors de la connexion à la station du serveur B.|  