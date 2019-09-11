---
ms.assetid: f964d056-11bf-4d9b-b5ab-dceaad8bfbc3
title: Niveaux fonctionnels de Windows Server 2016
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 10/29/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.custom: it-pro
ms.reviewer: maheshu
ms.technology: identity-adds
ms.openlocfilehash: c1e2108084b03fabbf7c6a18c2ecbcaf3cbd1dd9
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868255"
---
# <a name="forest-and-domain-functional-levels"></a>Niveaux fonctionnels de forêt et de domaine

>S'applique à : Windows Server

Les niveaux fonctionnels déterminent les fonctionnalités de forêt ou de domaine de Active Directory Domain Services (AD DS) disponibles. Ils déterminent également les systèmes d’exploitation Windows Server que vous pouvez exécuter sur les contrôleurs de domaine dans le domaine ou la forêt. Toutefois, les niveaux fonctionnels n’affectent pas les systèmes d’exploitation que vous pouvez exécuter sur les stations de travail et les serveurs membres qui sont joints au domaine ou à la forêt.

Lorsque vous déployez AD DS, définissez les niveaux fonctionnels du domaine et de la forêt sur la valeur la plus élevée que votre environnement peut prendre en charge. De cette façon, vous pouvez utiliser autant de fonctionnalités AD DS que possible. Lorsque vous déployez une nouvelle forêt, vous êtes invité à définir le niveau fonctionnel de la forêt, puis à définir le niveau fonctionnel du domaine. Vous pouvez définir le niveau fonctionnel du domaine sur une valeur supérieure au niveau fonctionnel de la forêt, mais vous ne pouvez pas définir le niveau fonctionnel du domaine sur une valeur inférieure au niveau fonctionnel de la forêt.

Avec la fin de vie de Windows 2003, les contrôleurs de domaine Windows 2003 doivent être mis à jour vers Windows Server 2008, 2008R2, 2012, 2012 R2, 2016 ou 2019. Par conséquent, tous les contrôleurs de domaine qui exécutent Windows Server 2003 doivent être supprimés du domaine.

Au niveau fonctionnel de domaine Windows Server 2008 et versions ultérieures, la réplication DFS (Distributed file service) est utilisée pour répliquer le contenu du dossier SYSVOL entre les contrôleurs de domaine. Si vous créez un nouveau domaine au niveau fonctionnel de domaine Windows Server 2008 ou supérieur, réplication DFS est automatiquement utilisé pour répliquer SYSVOL. Si vous avez créé le domaine à un niveau fonctionnel inférieur, vous devez migrer à partir de à l’aide de FRS vers la réplication DFS pour SYSVOL. Pour les étapes de migration, vous pouvez suivre les [procédures sur TechNet](https://technet.microsoft.com/library/dd640019(v=WS.10).aspx) ou vous pouvez consulter l' [ensemble d’étapes rationalisé sur le blog de l’équipe de stockage file cabinet](http://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx).

## <a name="windows-server-2019"></a>Windows Server 2019

Aucun nouveau niveau fonctionnel de forêt ou de domaine n’a été ajouté dans cette version.

La configuration minimale requise pour ajouter un contrôleur de domaine Windows Server 2019 est un niveau fonctionnel Windows Server 2008. Le domaine doit également utiliser DFS-R comme moteur pour répliquer SYSVOL.

## <a name="windows-server-2016"></a>Windows Server 2016

Système d’exploitation du contrôleur de domaine pris en charge :

* Windows Server 2019
* Windows Server 2016

### <a name="windows-server-2016-forest-functional-level-features"></a>Fonctionnalités de niveau fonctionnel de la forêt Windows Server 2016

* Toutes les fonctionnalités disponibles au niveau fonctionnel de la forêt Windows Server 2012 R2 et les fonctionnalités suivantes sont disponibles :
   * [Privileged Access Management (PAM) à l’aide d’Microsoft Identity Manager (MIM)](https://docs.microsoft.com/windows-server/identity/whats-new-active-directory-domain-services#a-namebkmkpamaprivileged-access-management)

### <a name="windows-server-2016-domain-functional-level-features"></a>Fonctionnalités de niveau fonctionnel de domaine Windows Server 2016

* Toutes les fonctionnalités de Active Directory par défaut, toutes les fonctionnalités du niveau fonctionnel du domaine Windows Server 2012 R2, ainsi que les fonctionnalités suivantes :
   * Les contrôleurs de clés peuvent prendre en charge le déroulement automatique des secrets NTLM et d’autres secrets basés sur un mot de passe sur un compte d’utilisateur configuré pour exiger l’authentification PKI. Cette configuration est également connue sous le nom de « carte à puce requise pour l’ouverture de session interactive ».
   * Les contrôleurs de domaine peuvent prendre en charge l’autorisation du réseau NTLM lorsqu’un utilisateur est limité à des appareils spécifiques joints à un domaine.
   * Les clients Kerberos qui s’authentifient avec succès avec l’extension d’actualisation de PKInit obtiendront le SID d’identité de clé publique actualisé.

    Pour plus d’informations, consultez [Nouveautés de l’authentification Kerberos](https://docs.microsoft.com/windows-server/security/kerberos/whats-new-in-kerberos-authentication) et [Nouveautés de la protection des informations d’identification](https://docs.microsoft.com/windows-server/security/credentials-protection-and-management/whats-new-in-credential-protection)

## <a name="windows-server-2012r2"></a>Windows Server 2012 R2

Système d’exploitation du contrôleur de domaine pris en charge :

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2

### <a name="windows-server-2012r2-forest-functional-level-features"></a>Fonctionnalités de niveau fonctionnel de la forêt Windows Server 2012 R2

* Toutes les fonctionnalités disponibles au niveau fonctionnel de la forêt Windows Server 2012, mais aucune fonctionnalité supplémentaire.

### <a name="windows-server-2012r2-domain-functional-level-features"></a>Fonctionnalités du niveau fonctionnel du domaine Windows Server 2012 R2

* Toutes les fonctionnalités de Active Directory par défaut, toutes les fonctionnalités du niveau fonctionnel de domaine Windows Server 2012, ainsi que les fonctionnalités suivantes :
   * Protections côté DC pour les utilisateurs protégés. Les utilisateurs protégés qui s’authentifient auprès d’un domaine Windows Server 2012 R2 ne peuvent plus :
      * S’authentifier avec l’authentification NTLM
      * Utiliser les suites de chiffrement DES ou RC4 dans la pré-authentification Kerberos
      * Être délégué avec délégation non contrainte ou contrainte
      * Renouveler les tickets d’utilisateur (TGT) au-delà de la durée de vie initiale de 4 heures
   * Stratégies d'authentification
      * Nouvelles stratégies de Active Directory basées sur les forêts qui peuvent être appliquées aux comptes dans les domaines Windows Server 2012 R2 pour contrôler les hôtes auxquels un compte peut se connecter et appliquer des conditions de contrôle d’accès pour l’authentification aux services s’exécutant en tant que compte.
   * Silos de stratégies d'authentification
      * Nouvel objet de Active Directory basé sur une forêt, qui peut créer une relation entre l’utilisateur, le service géré et l’ordinateur, les comptes à utiliser pour classer les comptes pour les stratégies d’authentification ou pour l’isolation d’authentification.

## <a name="windows-server-2012"></a>Windows Server 2012

Système d’exploitation du contrôleur de domaine pris en charge :

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012

### <a name="windows-server-2012-forest-functional-level-features"></a>Fonctionnalités de niveau fonctionnel de la forêt Windows Server 2012

* Toutes les fonctionnalités disponibles au niveau fonctionnel de la forêt Windows Server 2008 R2, mais aucune fonctionnalité supplémentaire.

### <a name="windows-server-2012-domain-functional-level-features"></a>Fonctionnalités de niveau fonctionnel de domaine Windows Server 2012

* Toutes les fonctionnalités de Active Directory par défaut, toutes les fonctionnalités du niveau fonctionnel du domaine Windows Server 2008R2, ainsi que les fonctionnalités suivantes :
   * La prise en charge du KDC pour les revendications, l’authentification composée et le blindage Kerberos stratégie de modèle d’administration KDC a deux paramètres (toujours fournir des revendications et rejeter les demandes d’authentification non blindées) qui nécessitent le niveau fonctionnel de domaine Windows Server 2012. Pour plus d’informations, consultez [Nouveautés de l’authentification Kerberos](https://technet.microsoft.com/library/hh831747.aspx)

## <a name="windows-server-2008r2"></a>2008R2 de Windows Server

Système d’exploitation du contrôleur de domaine pris en charge :

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2

### <a name="windows-server-2008r2-forest-functional-level-features"></a>Fonctionnalités de niveau fonctionnel de la forêt Windows Server 2008R2

* Toutes les fonctionnalités du niveau fonctionnel de forêt Windows Server 2003, plus les fonctionnalités suivantes :
   * La corbeille Active Directory offre la possibilité de restaurer des objets supprimés dans leur intégralité pendant que les services de domaine Active Directory sont en cours d’exécution.

### <a name="windows-server-2008r2-domain-functional-level-features"></a>Fonctionnalités du niveau fonctionnel du domaine Windows Server 2008R2

* Toutes les fonctionnalités de Active Directory par défaut, toutes les fonctionnalités du niveau fonctionnel de domaine Windows Server 2008, ainsi que les fonctionnalités suivantes :
   * Assurance du mécanisme d’authentification, qui contient des informations sur le type de méthode d’ouverture de session (carte à puce ou nom d’utilisateur/mot de passe) utilisé pour authentifier les utilisateurs du domaine au sein du jeton Kerberos de chaque utilisateur. Lorsque cette fonctionnalité est activée dans un environnement réseau qui a déployé une infrastructure de gestion des identités fédérées, telle que Services ADFS (AD FS), les informations contenues dans le jeton peuvent ensuite être extraites chaque fois qu’un utilisateur tente d’accéder à application prenant en charge les revendications qui a été développée pour déterminer l’autorisation en fonction de la méthode d’ouverture de session d’un utilisateur.
   * Gestion automatique des noms principaux de service pour les services qui s’exécutent sur un ordinateur particulier dans le contexte d’un compte de service administré lorsque le nom ou le nom d’hôte DNS du compte d’ordinateur change. Pour plus d’informations sur les comptes de service administrés, consultez le [Guide pas à pas des comptes de service](https://go.microsoft.com/fwlink/?LinkId=180401).

## <a name="windows-server-2008"></a>Windows Server 2008

Système d’exploitation du contrôleur de domaine pris en charge :

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008

### <a name="windows-server-2008-forest-functional-level-features"></a>Fonctionnalités de niveau fonctionnel de la forêt Windows Server 2008

* Toutes les fonctionnalités disponibles au niveau fonctionnel de la forêt Windows Server 2003, mais aucune fonctionnalité supplémentaire n’est disponible. 

### <a name="windows-server-2008-domain-functional-level-features"></a>Fonctionnalités de niveau fonctionnel de domaine Windows Server 2008

* Toutes les fonctionnalités de AD DS par défaut, toutes les fonctionnalités du niveau fonctionnel de domaine Windows Server 2003 et les fonctionnalités suivantes sont disponibles :
  * Prise en charge de la réplication système de fichiers DFS (DFS) pour le volume système Windows Server 2003 (SYSVOL)
    * La prise en charge de la réplication DFS offre une réplication plus robuste et plus détaillée du contenu SYSVOL.

      > [!NOTE]
      > À partir de Windows Server 2012 R2, le service de réplication de fichiers (FRS) est déconseillé. Un nouveau domaine créé sur un contrôleur de domaine qui exécute au moins Windows Server 2012 R2 doit être défini au niveau fonctionnel du domaine Windows Server 2008 ou supérieur.

  * Espaces de noms DFS basés sur un domaine qui s’exécutent en mode Windows Server 2008, ce qui comprend la prise en charge de l’énumération basée sur l’accès et une évolutivité accrue. Les espaces de noms basés sur un domaine en mode Windows Server 2008 nécessitent également que la forêt utilise le niveau fonctionnel de la forêt Windows Server 2003. Pour plus d’informations, consultez [choisir un type d’espace de noms](https://go.microsoft.com/fwlink/?LinkId=180400).
  * Prise en charge d’Advanced Encryption Standard (AES 128 et AES 256) pour le protocole Kerberos. Pour que les TGT soient émis à l’aide d’AES, le niveau fonctionnel de domaine doit être Windows Server 2008 ou une version ultérieure et le mot de passe de domaine doit être modifié. 
    * Pour plus d’informations, consultez [améliorations de Kerberos](https://technet.microsoft.com/library/cc749438(ws.10).aspx).

      > [!NOTE]
      >Des erreurs d’authentification peuvent se produire sur un contrôleur de domaine une fois que le niveau fonctionnel du domaine est élevé à Windows Server 2008 ou version ultérieure si le contrôleur de domaine a déjà répliqué la modification DFL mais n’a pas encore actualisé le mot de passe krbtgt. Dans ce cas, un redémarrage du service KDC sur le contrôleur de domaine déclenche une actualisation en mémoire du nouveau mot de passe krbtgt et résout les erreurs d’authentification associées.

  * [Dernière ouverture de session interactive](https://go.microsoft.com/fwlink/?LinkId=180387) Les informations affichent les informations suivantes :
     * Nombre total de tentatives de connexion ayant échoué sur un serveur Windows Server 2008 joint à un domaine ou une station de travail Windows Vista
     * Nombre total d’échecs de tentative de connexion après une ouverture de session réussie sur un serveur Windows Server 2008 ou une station de travail Windows Vista
     * Heure de la dernière tentative d’ouverture de session ayant échoué sur un serveur Windows Server 2008 ou une station de travail Windows Vista
     * Heure de la dernière tentative d’ouverture de session réussie sur un serveur Windows Server 2008 ou une station de travail Windows Vista
  * Les stratégies de mot de passe affinées vous permettent de spécifier des stratégies de verrouillage de compte et de mot de passe pour les utilisateurs et les groupes de sécurité globaux d’un domaine. Pour plus d’informations, consultez le [Guide pas à pas pour la configuration des stratégies de verrouillage de compte et de mot de passe affinées](https://go.microsoft.com/fwlink/?LinkID=91477).
  * Bureaux virtuels personnels
     * Pour utiliser la fonctionnalité ajoutée fournie par l’onglet bureau virtuel personnel de la boîte de dialogue Propriétés du compte d’utilisateur dans Active Directory utilisateurs et ordinateurs, votre schéma de AD DS doit être étendu pour Windows Server 2008 R2 (Schema Object version = 47). Pour plus d’informations, consultez [Guide pas à pas de déploiement de bureaux virtuels personnels à l’aide de connexions aux programmes RemoteApp et aux services Bureau à distance](https://go.microsoft.com/fwlink/?LinkId=183552).

## <a name="windows-server-2003"></a>Windows Server 2003

Système d’exploitation du contrôleur de domaine pris en charge :

* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003

### <a name="windows-server-2003-forest-functional-level-features"></a>Fonctionnalités de niveau fonctionnel de la forêt Windows Server 2003

* Toutes les fonctionnalités de AD DS par défaut et les fonctionnalités suivantes sont disponibles :
   * Approbation de forêt
   * changement de nom de domaine ;
   * Réplication de valeurs liées
      - La réplication de valeurs liées vous permet de modifier l’appartenance à un groupe pour stocker et répliquer les valeurs de chaque membre au lieu de répliquer l’ensemble de l’appartenance en tant qu’unité unique. Le stockage et la réplication des valeurs de chaque membre utilisent moins de bande passante réseau et moins de cycles processeur pendant la réplication, et vous empêchent de perdre des mises à jour lorsque vous ajoutez ou supprimez plusieurs membres simultanément sur des contrôleurs de domaine différents.
   * Possibilité de déployer un contrôleur de domaine en lecture seule (RODC)
   * Amélioration des algorithmes KCC (Knowledge Consistency Checker) et de l’évolutivité
      - Le générateur de topologie intersite (ISTG) utilise des algorithmes améliorés qui s’adaptent pour prendre en charge les forêts avec un plus grand nombre de sites que AD DS peut prendre en charge au niveau fonctionnel de la forêt Windows 2000. L’algorithme d’élection ISTG amélioré est un mécanisme moins intrusif pour le choix de l’ISTG au niveau fonctionnel de la forêt Windows 2000.
   * Possibilité de créer des instances de la classe auxiliaire dynamique nommée **DynamicObject** dans une partition d’annuaire de domaine
   * Possibilité de convertir une instance d’objet **InetOrgPerson** en instance d’objet **utilisateur** et d’effectuer la conversion dans la direction opposée
   * Possibilité de créer des instances de nouveaux types de groupes pour prendre en charge l’autorisation basée sur les rôles. 
      - Ces types sont appelés groupes de base d’applications et groupes de requêtes LDAP.
   * désactivation et redéfinition des attributs et classes du schéma. Les attributs suivants peuvent être réutilisés : ldapDisplayName, schemaIdGuid, OID et mapiID.
   * Espaces de noms DFS basés sur un domaine qui s’exécutent en mode Windows Server 2008, ce qui comprend la prise en charge de l’énumération basée sur l’accès et une évolutivité accrue. Pour plus d’informations, consultez [choisir un type d’espace de noms](https://go.microsoft.com/fwlink/?LinkId=180400).

### <a name="windows-server-2003-domain-functional-level-features"></a>Fonctionnalités de niveau fonctionnel de domaine Windows Server 2003

* Toutes les fonctionnalités de AD DS par défaut, toutes les fonctionnalités disponibles au niveau fonctionnel de domaine natif Windows 2000 et les fonctionnalités suivantes sont disponibles :
   * L’outil de gestion de domaine, Netdom. exe, qui vous permet de renommer les contrôleurs de domaine
   * Mises à jour de l’horodatage de connexion
      * (L’attribut **lastLogonTimestamp** est mis à jour avec l’heure de la dernière ouverture de session de l’utilisateur ou ordinateur. Cet attribut est répliqué au sein du domaine.) ;
   * Possibilité de définir l’attribut **userPassword** comme mot de passe effectif sur les objets **InetOrgPerson** et utilisateur
   * Possibilité de rediriger les conteneurs utilisateurs et ordinateurs
      * Par défaut, deux conteneurs connus sont fournis pour héberger des comptes d’ordinateur et d’utilisateur, à savoir CN =<domain root> Computers, et CN<domain root>= Users,. Cette fonctionnalité permet de définir un nouvel emplacement connu pour ces comptes.
   * La possibilité pour le gestionnaire d’autorisations de stocker ses stratégies d’autorisation dans AD DS
   * Délégation contrainte
      * La délégation avec restriction permet aux applications de tirer parti de la délégation sécurisée des informations d’identification de l’utilisateur au moyen de l’authentification Kerberos.
      * Vous pouvez limiter la délégation à des services de destination spécifiques uniquement.
   * Authentification sélective
      * L’authentification sélective vous permet de spécifier les utilisateurs et les groupes d’une forêt approuvée autorisés à s’authentifier auprès des serveurs de ressources d’une forêt d’approbation.

## <a name="windows-2000"></a>Windows 2000

Système d’exploitation du contrôleur de domaine pris en charge :

* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003
* Windows 2000

### <a name="windows-2000-native-forest-functional-level-features"></a>Fonctionnalités de niveau fonctionnel de la forêt native Windows 2000

* Toutes les fonctionnalités de AD DS par défaut sont disponibles.

### <a name="windows-2000-native-domain-functional-level-features"></a>Fonctionnalités de niveau fonctionnel de domaine natif de Windows 2000

* Toutes les fonctionnalités de AD DS par défaut et les fonctionnalités d’annuaire suivantes sont disponibles, notamment :
   * Groupes universels pour les groupes de distribution et de sécurité.
   * Imbrication de groupes
   * Conversion de groupe, qui permet la conversion entre les groupes de sécurité et de distribution
   * Historique des identificateurs de sécurité (SID)

## <a name="next-steps"></a>Étapes suivantes

* [Augmenter le niveau fonctionnel du domaine](https://technet.microsoft.com/library/cc753104.aspx)  
* [Augmenter le niveau fonctionnel de la forêt](https://technet.microsoft.com/library/cc730985.aspx)
