---
ms.assetid: 6a852428-c1ec-4703-b3b3-a4bfdf8cbb9d
title: Nouveautés de Active Directory Domain Services dans Windows Server 2016
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: a82f45772e5e35afffc632de2b40c02c75b5e5e4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856282"
---
# <a name="whats-new-in-active-directory-domain-services-for-windows-server-2016"></a>Nouveautés des services Active Directory Domain Services (AD DS) pour Windows Server 2016

>S’applique à Windows Server 2016

Les nouvelles fonctionnalités suivantes de Active Directory Domain Services (AD DS) améliorent la capacité des organisations à sécuriser les environnements Active Directory et à les aider à migrer vers des déploiements dans le Cloud uniquement et des déploiements hybrides, où certains services et applications sont hébergés dans le Cloud, et d’autres sont hébergés localement. Les améliorations sont les suivantes :  
  
- [Privileged Access Management](https://docs.microsoft.com/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services)  
  
- [Extension des fonctionnalités du Cloud aux appareils Windows 10 via Azure Active Directory Join](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-overview/)
  
- [Connexion d’appareils joints à un domaine à des Azure AD pour les expériences Windows 10](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/)
  
- [Activer les Microsoft Passport for Work dans votre organisation](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)
  
- [Désapprobation des niveaux fonctionnels du service de réplication de fichiers (FRS) et de Windows Server 2003](ad-ds/active-directory-functional-levels.md)  
  
## <a name="privileged-access-management"></a>Privileged Access Management

Privileged Access Management (PAM) permet de limiter les problèmes de sécurité pour les environnements Active Directory qui sont causés par des techniques de vol d’informations d’identification telles que les attaques Pass-The-hash, les tentatives de hameçonnage et des types similaires d’attaques. Il fournit une nouvelle solution d’accès administratif configurée à l’aide d’Microsoft Identity Manager (MIM). PAM introduit les éléments suivants :  
  
- Nouvelle forêt bastion Active Directory, approvisionnée par MIM. La forêt bastion a une approbation PAM spéciale avec une forêt existante. Il fournit un nouvel environnement de Active Directory connu comme étant exempt de toute activité malveillante, et isolation d’une forêt existante pour l’utilisation de comptes privilégiés.  
  
- Nouveaux processus dans MIM pour demander des privilèges d’administrateur, ainsi que de nouveaux flux de travail basés sur l’approbation des demandes.  
  
- Nouveaux principaux de sécurité Shadow (groupes) approvisionnés dans la forêt bastion par MIM en réponse aux demandes de privilèges d’administration. Les principaux de sécurité Shadow ont un attribut qui fait référence au SID d’un groupe d’administration dans une forêt existante. Cela permet au groupe Shadow d’accéder aux ressources d’une forêt existante sans modifier les listes de contrôle d’accès (ACL).  
  
- Une fonctionnalité de liens arrivant à expiration, qui permet d’appartenir au temps dans un groupe Shadow. Un utilisateur peut être ajouté au groupe pour le temps nécessaire à l’exécution d’une tâche d’administration. L’appartenance liée à la durée est exprimée par une valeur de durée de vie (TTL) qui est propagée à une durée de vie de ticket Kerberos.  
  
    > [!NOTE]  
    > Les liens arrivant à expiration sont disponibles sur tous les attributs liés. Toutefois, la relation d’attribut lié membre/memberOf entre un groupe et un utilisateur est le seul exemple dans lequel une solution complète comme PAM est préconfigurée pour utiliser la fonctionnalité des liens arrivant à expiration.  
  
- Les améliorations du KDC sont intégrées à Active Directory contrôleurs de domaine pour limiter la durée de vie des tickets Kerberos à la valeur de durée de vie (TTL) la plus basse possible dans les cas où un utilisateur dispose de plusieurs appartenances dans des groupes d’administration. Par exemple, si vous êtes ajouté à un groupe de temps A, lorsque vous vous connectez, la durée de vie du ticket TGT (Ticket-Granting Ticket) Kerberos est égale à la durée restante dans le groupe A. Si vous êtes également membre d’un autre groupe B lié à la durée, qui a une durée de vie inférieure à celle du groupe A, la durée de vie du ticket TGT est égale au temps restant dans le groupe B.  
  
- Nouvelles fonctionnalités de surveillance pour vous aider à identifier facilement les personnes qui ont demandé l’accès, l’accès accordé et les activités qui ont été effectuées.  

### <a name="requirements-for-privileged-access-management"></a>Configuration requise pour Privileged Access Management
  
- Microsoft Identity Manager  
  
- Active Directory niveau fonctionnel de la forêt de Windows Server 2012 R2 ou version ultérieure.  
  
## <a name="azure-ad-join"></a>Joindre Azure AD

Azure Active Directory Join améliore les expériences d’identité pour les clients Enterprise, Business et EDU, avec des fonctionnalités améliorées pour les appareils d’entreprise et personnels.  
  
Avantages :  
  
- **Disponibilité des paramètres modernes** sur les appareils Windows appartenant à l’entreprise. Les services d’oxygène n’ont plus besoin d’un compte Microsoft personnel : ils exécutent maintenant les comptes professionnels existants des utilisateurs pour garantir leur conformité. Les services d’oxygène fonctionnent sur les PC qui sont joints à un domaine Windows local, ainsi que sur les PC et les appareils qui sont « joints » à votre locataire Azure AD (« domaine Cloud »). Ces paramètres sont les suivants :  

   - Itinérance ou personnalisation, paramètres d’accessibilité et informations d’identification  
   - Sauvegarde et restauration  
   - Accès à Microsoft Store avec un compte professionnel  
   - Vignettes et notifications dynamiques  
  
- **Accédez aux ressources organisationnelles** sur les appareils mobiles (téléphones, phablets) qui ne peuvent pas être joints à un domaine Windows, qu’ils appartiennent à l’entreprise ou à BYOD  
- **Authentification unique** sur Office 365 et d’autres applications organisationnelles, sites Web et ressources.  
- **Sur les appareils BYOD**, ajoutez un compte professionnel (à partir d’un domaine local ou Azure AD) à un appareil personnel et profitez de l’authentification unique aux ressources de travail, via des applications et sur le Web, de manière à garantir la conformité avec les nouvelles fonctionnalités telles que le contrôle de compte conditionnel et l’attestation intégrité de l’appareil.  
- L' **intégration MDM** vous permet d’inscrire automatiquement des appareils à votre MDM (Intune ou tiers)  
- **Configurer le mode plein écran et les appareils partagés** pour plusieurs utilisateurs de votre organisation  
- L' **expérience des développeurs** vous permet de créer des applications qui s’appuient sur des contextes d’entreprise et personnels avec une pile de programmation partagée.  
- L’option d' **acquisition d’images** vous permet de choisir entre la création d’images et de permettre à vos utilisateurs de configurer des appareils d’entreprise directement lors de la première exécution.  
  
Pour plus d’informations, consultez [Introduction à la gestion des appareils dans Azure Active Directory](https://docs.microsoft.com/azure/active-directory/devices/overview).  
  
## <a name="windows-hello-for-business"></a>Windows Hello Entreprise

Windows Hello entreprise est une approche d’authentification basée sur les clés qui s’appuie sur les organisations et les consommateurs, qui vont au-delà des mots de passe. Cette forme d’authentification s’appuie sur la violation, le vol et les informations d’identification résistantes au hameçonnage.  
  
L’utilisateur se connecte à l’appareil avec des informations de connexion biométrique ou code confidentiel qui sont liées à un certificat ou à une paire de clés asymétriques. Les fournisseurs d’identité (fournisseurs) valident l’utilisateur en mappant la clé publique de l’utilisateur à IDLocker et fournissent des informations de connexion via un mot de passe à usage unique (OTP), un téléphone ou un autre mécanisme de notification.  
  
Pour plus d’informations, consultez [Windows Hello entreprise](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification)  
  
## <a name="deprecation-of-file-replication-service-frs-and-windows-server-2003-functional-levels"></a>Désapprobation des niveaux fonctionnels du service de réplication de fichiers (FRS) et de Windows Server 2003

Bien que les niveaux fonctionnels du service de réplication de fichiers (FRS) et de Windows Server 2003 étaient déconseillés dans les versions précédentes de Windows Server, la répétition du fait que le système d’exploitation Windows Server 2003 n’est plus pris en charge. Par conséquent, tous les contrôleurs de domaine qui exécutent Windows Server 2003 doivent être supprimés du domaine. Le niveau fonctionnel du domaine et de la forêt doit atteindre au moins Windows Server 2008 pour empêcher l’ajout d’un contrôleur de domaine qui exécute une version antérieure de Windows Server à l’environnement.

Aux niveaux fonctionnels de domaine Windows Server 2008 et supérieurs, la réplication DFS (Distributed File Service) est utilisée pour répliquer le contenu du dossier SYSVOL entre les contrôleurs de domaine. Si vous créez un domaine au niveau fonctionnel de domaine Windows Server 2008 ou supérieur, la réplication DFS est automatiquement utilisée pour répliquer SYSVOL. Si vous avez créé le domaine à un niveau fonctionnel inférieur, vous devez passer de l’utilisation de FRS à la réplication DFS pour SYSVOL. Pour les étapes de migration, vous pouvez suivre [ces étapes](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640019\(v=ws.10\)) ou vous pouvez vous reporter à l' [ensemble d’étapes rationalisé sur le blog de l’équipe de stockage file cabinet](https://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx).  
  
Les niveaux fonctionnels de domaine et de forêt de Windows Server 2003 continuent d’être pris en charge, mais les organisations doivent élever le niveau fonctionnel à Windows Server 2008 (ou une version ultérieure si possible) pour garantir la compatibilité et la prise en charge de la réplication SYSVOL à l’avenir. En outre, il existe de nombreux autres avantages et fonctionnalités disponibles à des niveaux fonctionnels plus élevés. Pour plus d'informations, consultez les ressources suivantes :  

- [Fonctionnement des niveaux fonctionnels de Active Directory Domain Services (AD DS)](ad-ds/active-directory-functional-levels.md)  
- [Augmenter le niveau fonctionnel de domaine](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753104\(v=ws.11\))  
- [Augmenter le niveau fonctionnel de forêt](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc730985\(v=ws.11\))  
