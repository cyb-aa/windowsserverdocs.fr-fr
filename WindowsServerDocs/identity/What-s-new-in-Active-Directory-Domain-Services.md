---
ms.assetid: 9a06cd41-426f-4cb9-89cf-f5be730e0b79
title: 'Quelle & #39; est nouveau dans les Services de domaine Active Directory'
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: 
ms.suite: na
ms.technology: active-directory-domain-services
ms.tgt_pltfrm: na
ms.topic: article
author: Femila
ms.author: billmath
ms.date: 05/31/2017
ms.openlocfilehash: 56716e2e0d633a19e9c6e02bc829874bbe863833
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="what39s-new-in-active-directory-domain-services"></a>Quelle & #39; est nouveau dans les Services de domaine Active Directory 

>S’applique à: Windows Server2016

Les nouvelles fonctionnalités suivantes dans les Services de domaine Active Directory (AD DS) améliorent la capacité des organisations à sécuriser les environnements Active Directory et les aider à migrer vers les déploiements de cloud uniquement et les déploiements hybrides, où certains services et applications sont hébergés dans le cloud et d’autres sont hébergés en local. Les améliorations incluent:  
  
-   [Gestion de l’accès privilégié](https://technet.microsoft.com/library/mt150258.aspx   
)  
  
- [L’extension des capacités du cloud pour les appareils Windows 10 par le biais d’Azure Active Directory Join](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-overview/)   
  
- [Connexion des appareils joints au domaine à Azure AD pour Windows 10 expériences](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-devices-group-policy/)   
  
- [Activer Microsoft Passport for Work dans votre organisation](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-passport-deployment/)    
  
-  [Désapprobation des niveaux fonctionnels de Service de réplication de fichiers (FRS) et Windows Server 2003](ad-ds/active-directory-functional-levels.md)  
  
  
## <a name="BKMK_PAM"></a>Gestion de l’accès privilégié  
Gestion d’accès privilégié (PAM) permet d’atténuer sécurité problèmes pour les environnements Active Directory qui sont provoquées par des informations d’identification des techniques de vol ces pass-the-hash, «spear phishing» et des types d’attaques similaires. Il fournit une nouvelle solution d’accès d’administration qui est configurée à l’aide de Microsoft Identity Manager (MIM). PAM présente:  
  
-   Une nouvelle forêt bastion Active Directory, qui est configurée par MIM. La forêt bastion a une relation d’approbation PAM spéciale avec une forêt existante. Il fournit un environnement Active Directory qui est connu pour être exempts de toute activité malveillante et d’isolation par rapport à une forêt existante pour l’utilisation des comptes privilégiés.  
  
-   Nouveaux processus dans MIM pour demander des privilèges d’administration, ainsi que de nouveaux workflows basés sur l’approbation des demandes.  
  
-   Nouveaux clichés principaux de sécurité (groupes) qui sont configurés dans la forêt bastion par MIM en réponse aux demandes de privilèges d’administration. Les principaux de sécurité de clichés instantanés ont un attribut qui référence le SID d’un groupe d’administration dans une forêt existante. Cela permet au groupe de clichés instantanés pour accéder aux ressources dans une forêt existante sans modifier les listes de contrôle d’accès (ACL).  
  
-   Une fonctionnalité liens arrivant à expiration, ce qui permet d’appartenance temporaires dans un groupe de clichés instantanés. Un utilisateur peut être ajouté au groupe pour juste assez temps requis pour effectuer une tâche d’administration. L’appartenance temporaires est exprimée en durée de vie (TTL) valeur propagé à une durée de vie du ticket Kerberos.  
  
    > [!NOTE]  
    > Liens arrivant à expiration sont disponibles sur tous les attributs liés. Mais l’attribut lié membre/memberOf relation entre un groupe et d’un utilisateur est le seul exemple où une solution complète comme PAM est préconfigurée pour utiliser la fonctionnalité liens arrivant à expiration.  
  
-   Améliorations du KDC sont intégrées aux contrôleurs de domaine Active Directory pour limiter la durée de vie du ticket Kerberos à la valeur la plus basse possible de durée de vie (TTL) dans les cas où un utilisateur possède plusieurs appartenances temporaires dans des groupes d’administration. Par exemple, si vous sont ajoutés à un groupe de temps A, lorsque vous ouvrez une session, la durée de vie Kerberos ticket-granting ticket (TGT ticket) est égale à la fois avoir restant dans le groupe A. Si vous êtes également un membre d’un autre groupe temporaires B, qui a une durée de vie inférieure à un groupe, la durée de vie du ticket TGT est égale au temps que restant dans le groupe B.  
  
-   Nouvelles fonctionnalités de surveillance pour vous aider à facilement identifient qui a demandé l’accès, les accès a été accordé et les activités qui ont été effectuées.  
  
**Configuration requise**  
  
-   Microsoft Identity Manager  
  
-   Niveau fonctionnel de Windows Server 2012 R2 ou une version ultérieure de forêt Active Directory.  
  
## <a name="BKMK_AzureADJoin"></a>Jonction à Azure AD  
Jonction d’Active Directory Azure améliore les expériences d’identité d’entreprise, les entreprises et les clients EDU - avec des fonctionnalités améliorées pour les appareils personnels et d’entreprise.  
  
Avantages:  
  
-   **Disponibilité des paramètres modernes** sur les appareils Windows corp. Services d’oxygène ne nécessitent plus un compte Microsoft personnel: elles s’exécutent désormais désactiver les comptes existants de travail des utilisateurs pour garantir la conformité. Services d’oxygène fonctionnent sur des ordinateurs joints à un domaine Windows locaux, et les ordinateurs et périphériques qui sont «joint» à votre client Azure AD («domaine cloud»). Ces paramètres sont les suivantes:  
  
    -   Itinérance ou les personnalisations, les paramètres d’accessibilité et les informations d’identification  
  
    -   Sauvegarde et restauration  
  
    -   Accès à MicrosoftStore avec un compte professionnel  
  
    -   Notifications et vignettes dynamiques  
  
-   **Accéder aux ressources de l’organisation** sur les périphériques mobiles (téléphones, phablettes) qui ne peuvent pas être joints à un domaine Windows, qu’ils soient appartenant à l’entreprise ou BYOD  
  
-   **Authentification unique sur** à Office 365 et autres applications d’organisation, les sites Web et les ressources.  
  
-   **Sur les appareils BYOD**, ajouter un compte professionnel (à partir d’un domaine local ou Azure AD) à un appareil personnel et profiter de l’authentification unique pour les ressources, via des applications et sur le web, de travail de manière permet de garantir la conformité avec les nouvelles fonctionnalités telles que l’attestation conditionnelle contrôle de compte et de contrôle d’intégrité de l’appareil.  
  
-   **Intégration de MDM** vous permet de l’inscription automatique des appareils à votre GPM (Intune ou tiers)  
  
-   **Configurer le mode «plein écran» et partagés périphériques** pour plusieurs utilisateurs dans votre organisation  
  
-   **Expérience du développeur** vous permet de créer des applications prenant en charge pour l’entreprise et personnelles contextes avec une pile de programmation partagé.  
  
-   **Création d’images** option vous permet de choisir entre autoriser vos utilisateurs et d’acquisition d’images pour configurer les appareils appartenant à corp directement au cours de l’expérience de première utilisation.  
  
Pour plus d’informations, voir [Windows 10 pour l’entreprise: façons d’utiliser des périphériques pour le travail](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-windows10-devices-overview/?rnd=1).  
  
## <a name="BKMK_IDLocker"></a>Microsoft Passport  
Microsoft Passport est une nouvelle clé de l’authentification basée sur l’approche organisations et les consommateurs, ce qui permet d’aller au-delà des mots de passe. Cette forme d’authentification s’appuie sur la violation, le vol et les informations d’identification résistant hameçonnage.  
  
L’utilisateur ouvre une session sur l’appareil avec une biométrique ou code confidentiel de session qui est lié à un certificat ou une paire de clés asymétrique. Les fournisseurs d’identité (IDP) valide que l’utilisateur en mappant la clé publique de l’utilisateur à IDLocker et fournit des journaux sur informations par le biais d’une heure mot de passe (OTP), Phonefactor ou un mécanisme de notification différents.  
  
Pour plus d’informations, voir [authentification des identités sans mot de passe par le biais de Microsoft Passport](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-passport/)  
  
## <a name="BKMK_FRSDeprecation"></a>Désapprobation des niveaux fonctionnels de Service de réplication de fichiers (FRS) et Windows Server 2003  
Bien que le Service de réplication de fichiers (FRS) et les niveaux fonctionnels de Windows Server 2003 ont été déconseillées dans les versions précédentes de Windows Server, il porte extensible que le système d’exploitation Windows Server 2003 n’est plus pris en charge. Par conséquent, n’importe quel contrôleur de domaine qui exécute Windows Server 2003 doit être supprimé du domaine. Le niveau fonctionnel du domaine et de forêt doit être déclenché au moins Windows Server 2008 pour empêcher un contrôleur de domaine qui exécute une version antérieure de Windows Server d’être ajouté à l’environnement.  
  
Aux niveaux fonctionnels de domaine plus élevés et Windows Server 2008, la réplication du Service de fichiers distribués (DFS) est utilisée pour répliquer le contenu du dossier SYSVOL entre les contrôleurs de domaine. Si vous créez un nouveau domaine au niveau fonctionnel de domaine Windows Server 2008 ou version ultérieure, la réplication DFS est automatiquement utilisée pour répliquer SYSVOL. Si vous avez créé le domaine à un niveau fonctionnel inférieur, vous devez migrer à partir de l’aide de FRS vers la réplication DFS pour SYSVOL. Pour les étapes de migration, vous pouvez soit suivre le [procédures sur TechNet](https://technet.microsoft.com/library/dd640019(v=WS.10).aspx) ou vous pouvez faire référence à la [simplifié des étapes sur le blog File Cabinet équipe de stockage](http://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx).  
  
Les Windows Server 2003 domaine et forêt niveaux fonctionnels de continuent à être pris en charge, mais les organisations doivent augmenter le niveau fonctionnel de Windows Server 2008 (ou version ultérieure si possible) pour assurer la compatibilité de la réplication SYSVOL et prendre en charge à l’avenir. En outre, il existe les nombreux autres avantages et fonctionnalités disponibles aux niveaux fonctionnels supérieurs plus élevés. Consultez les ressources suivantes pour plus d’informations:  
  
-   [Présentation de domaine Active Directory (AD DS) les niveaux fonctionnels des Services](ad-ds/active-directory-functional-levels.md)  
  
-   [Augmenter le niveau fonctionnel du domaine](https://technet.microsoft.com/library/cc753104.aspx)  
  
-   [Augmenter le niveau fonctionnel de forêt](https://technet.microsoft.com/library/cc730985.aspx)  
  
