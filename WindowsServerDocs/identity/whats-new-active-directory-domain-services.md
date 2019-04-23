---
ms.assetid: 6a852428-c1ec-4703-b3b3-a4bfdf8cbb9d
title: Ce que&#39;s Nouveautés des Services de domaine Active Directory dans Windows Server 2016
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ffdeedfc2d818fe223c033aeecfe29d18365db7a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865540"
---
# <a name="whats-new-in-active-directory-domain-services-for-windows-server-2016"></a>Nouveautés dans Active Directory domaine Services pour Windows Server 2016

>S'applique à : Windows Server 2016

Les nouvelles fonctionnalités suivantes dans les Services de domaine Active Directory (AD DS) améliorent la capacité des organisations à sécuriser les environnements Active Directory et de les aider à migrer vers les déploiements cloud uniquement et les déploiements hybrides, où certains services et applications sont hébergé dans le cloud et d’autres sont hébergés en local. Les améliorations incluent :  
  
- [Gestion de l’accès privilégié](https://docs.microsoft.com/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services)  
  
- [Extension des fonctionnalités de cloud aux appareils Windows 10 via Azure Active Directory Join](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-overview/)
  
- [Expériences de connexion des appareils joints au domaine à Azure AD pour Windows 10](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/)
  
- [Activer Microsoft Passport for Work dans votre organisation](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)
  
- [Dépréciation des niveaux fonctionnels de Service de réplication de fichiers (FRS) et Windows Server 2003](ad-ds/active-directory-functional-levels.md)  
  
## <a name="privileged-access-management"></a>Gestion de l’accès privilégié

Gestion des accès privilégiés (PAM) permet d’atténuer sécurité problèmes relatifs à des environnements Active Directory qui sont provoquées par des informations d’identification des techniques de vol ces pass-the-hash, « spear phishing » et des types d’attaques similaires. Il fournit une solution d’accès administratif est configurée à l’aide de Microsoft Identity Manager (MIM). PAM est présente :  
  
- Une nouvelle forêt bastion Active Directory, qui est provisionné par MIM. La forêt bastion a une relation d’approbation PAM spéciale avec une forêt existante. Il fournit un nouvel environnement Active Directory qui est connu pour être libre de toute activité malveillante et d’isolation à partir d’une forêt existante pour l’utilisation des comptes privilégiés.  
  
- Dans MIM pour demander des privilèges d’administrateur, ainsi que de nouveaux flux de travail en fonction de l’approbation des demandes de nouveaux processus.  
  
- Nouveaux clichés instantanés principaux de sécurité (groupes) qui sont configurés dans la forêt bastion à MIM en réponse aux demandes de privilèges d’administrateur. Les principaux de sécurité de clichés instantanés ont un attribut qui référence le SID d’un groupe d’administration dans une forêt existante. Ainsi, le groupe d’ombres pour accéder aux ressources dans une forêt existante sans modifier les listes de contrôle d’accès (ACL).  
  
- Une fonctionnalité des liens qui arrive à expiration, ce qui permet l’appartenance limitée dans le temps dans un groupe d’ombres. Un utilisateur peut être ajouté au groupe pour juste le temps requis pour effectuer une tâche administrative. L’appartenance limitée dans le temps est exprimé par time-to-live (TTL) valeur qui est transmise à une durée de vie du ticket Kerberos.  
  
    > [!NOTE]  
    > Liens arrivant à expiration sont disponibles sur tous les attributs liés. Mais l’attribut lié membre/memberOf relation entre un groupe et un utilisateur est le seul exemple où une solution complète comme PAM est préconfigurée pour utiliser la fonctionnalité de liens qui arrive à expiration.  
  
- Améliorations du centre KDC sont intégrées aux contrôleurs de domaine Active Directory pour limiter la durée de vie de ticket Kerberos à la plus basse possible time-to-live (TTL) valeur au cas où un utilisateur possède plusieurs appartenances pendant une durée limitée dans les groupes d’administration. Par exemple, si vous êtes ajouté à un groupe de pendant une durée limitée de A, lorsque vous ouvrez une session, la durée de vie Kerberos ticket-granting ticket (TGT ticket) est égale à la fois avoir restant dans le groupe A. Si vous êtes également membre d’un autre groupe pendant une durée limitée B, qui a une durée de vie inférieure à celui de groupe A, la durée de vie du ticket TGT est égale au temps que restant dans le groupe B.  
  
- Nouvelles fonctionnalités de surveillance vous permettent de facilement identifient qui a demandé l’accès, quel est l’accès a été accordée, et les activités ont été effectuées.  

### <a name="requirements-for-privileged-access-management"></a>Configuration requise pour le privilège gestion des accès
  
- Gestionnaire d'identité Microsoft  
  
- Niveau fonctionnel de Windows Server 2012 R2 ou version ultérieure de forêt Active Directory.  
  
## <a name="azure-ad-join"></a>Joindre Azure AD

Azure Active Directory Join améliore les expériences d’identité pour enterprise, business et les clients EDU - avec des capacités améliorées pour les appareils personnels et professionnels.  
  
Avantages :  
  
- **Disponibilité des paramètres modernes** sur les appareils Windows-appartenant à l’entreprise. Les Services d’oxygène ne nécessitent plus un compte Microsoft personnel : elles s’exécutent désormais désactiver les comptes existants de travail des utilisateurs pour assurer la conformité. Les Services d’oxygène fonctionne sur les PC qui sont joints à un domaine Windows en local, et les PC et les appareils qui sont « joint » à votre locataire Azure AD (« domaine cloud »). Ces paramètres sont les suivants :  

   - Itinérance ou les personnalisations, les paramètres d’accessibilité et les informations d’identification  
   - Sauvegarde et restauration  
   - Accès à Microsoft Store avec un compte professionnel  
   - Notifications et les vignettes dynamiques  
  
- **Accéder aux ressources de l’organisation** sur des appareils mobiles (téléphones, les phablettes) qui ne peuvent pas être joints à un domaine Windows, qu’ils soient appartenant à l’entreprise ou BYOD  
- **L’authentification unique sur** à Office 365 et autres applications d’organisation, les sites Web et les ressources.  
- **Sur les appareils BYOD**, ajouter un compte professionnel (à partir d’un domaine local ou Azure AD) à un appareil personnels et profitez de l’authentification unique aux ressources professionnelles, via des applications et sur le web, d’une manière qui aide à garantit la conformité avec les nouvelles fonctionnalités telles que compte conditionnel Attestation de contrôle et l’intégrité de l’appareil.  
- **Intégration de la gestion des appareils mobiles** vous permet d’inscrire automatiquement des appareils à votre MDM (Intune ou tiers)  
- **Configurer le mode « plein écran » et d’appareils partagés** pour plusieurs utilisateurs de votre organisation  
- **Expérience de développement** vous permet de créer des applications destinées aux entreprises et des contextes personnels avec une pile de programmation partagé.  
- **Création d’images** option vous permet de choisir entre la création d’images et d’autoriser vos utilisateurs pour configurer des appareils appartenant à l’entreprise directement lors de l’expérience de première exécution.  
  
Pour plus d’informations, consultez [Introduction à la gestion des appareils dans Azure Active Directory](https://docs.microsoft.com/azure/active-directory/devices/overview).  
  
## <a name="windows-hello-for-business"></a>Windows Hello Entreprise

Windows Hello entreprise est une authentification basée sur la clé de l’approche les organisations et les consommateurs, qui va au-delà des mots de passe. Cette forme d’authentification s’appuie sur la violation, le vol et les informations d’identification résistant aux tentatives de phishing.  
  
L’utilisateur se connecte à l’appareil à un journal biométrique ou ÉPINGLER sur les informations liées à un certificat ou une paire de clés asymétrique. Les fournisseurs d’identité (IDP) valider l’utilisateur en mappant la clé publique de l’utilisateur à IDLocker et fournit un journal des informations via une seule fois mot de passe à usage unique, téléphone ou un mécanisme de notification différents.  
  
Pour plus d’informations, consultez [Windows Hello entreprise](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification)  
  
## <a name="deprecation-of-file-replication-service-frs-and-windows-server-2003-functional-levels"></a>Dépréciation des niveaux fonctionnels de Service de réplication de fichiers (FRS) et Windows Server 2003

Bien que le Service de réplication de fichiers (FRS) et les niveaux fonctionnels de Windows Server 2003 ont été déconseillées dans les versions précédentes de Windows Server, répétons-le que le système d’exploitation Windows Server 2003 n’est plus pris en charge. Par conséquent, n’importe quel contrôleur de domaine qui exécute Windows Server 2003 doit être supprimé du domaine. Le niveau fonctionnel du domaine et de forêt doit être déclenché au moins Windows Server 2008 pour empêcher un contrôleur de domaine qui exécute une version antérieure de Windows Server d’être ajouté à l’environnement.

Au plus élevés niveaux fonctionnels de domaine et à Windows Server 2008, la réplication du Service de fichiers distribués (DFS, Distributed File System) est utilisée pour répliquer le contenu du dossier SYSVOL entre les contrôleurs de domaine. Si vous créez un nouveau domaine de niveau fonctionnel de domaine Windows Server 2008 ou version ultérieure, la réplication DFS est automatiquement utilisée pour répliquer SYSVOL. Si vous avez créé le domaine à un niveau fonctionnel inférieur, vous devez migrer à partir de l’utilisation de FRS pour la réplication DFS pour SYSVOL. Pour les étapes de migration, vous pouvez soit suivre [suit](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640019\(v=ws.10\)) ou vous pouvez faire référence à la [rationalisé l’ensemble d’étapes sur le blog de l’équipe stockage fichier CAB du fichier](http://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx).  
  
Windows Server 2003 niveau du domaine et forêt fonctionnelle continue à être pris en charge, mais les organisations doivent augmenter le niveau fonctionnel vers Windows Server 2008 (ou version ultérieure si possible) pour garantir la compatibilité de la réplication SYSVOL et prendre en charge à l’avenir. En outre, il sont nombreux autres avantages et les fonctionnalités disponibles aux niveaux supérieurs fonctionnelles plus élevées. Pour plus d'informations, consultez les ressources suivantes :  

- [Présentation de domaine Active Directory (AD DS) des niveaux fonctionnels des Services](ad-ds/active-directory-functional-levels.md)  
- [Augmenter le niveau fonctionnel de domaine](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753104\(v=ws.11\))  
- [Augmenter le niveau fonctionnel de forêt](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc730985\(v=ws.11\))  
