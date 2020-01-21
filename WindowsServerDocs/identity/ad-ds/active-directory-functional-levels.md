---
ms.assetid: f964d056-11bf-4d9b-b5ab-dceaad8bfbc3
title: Niveaux fonctionnels de Windows Server 2016
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 10/29/2018
ms.topic: article
ms.prod: windows-server
ms.custom: it-pro
ms.reviewer: maheshu
ms.technology: identity-adds
ms.openlocfilehash: 53793fc62b1bc1444c567f92c9f18642245fded9
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75948183"
---
# <a name="forest-and-domain-functional-levels"></a>Niveaux fonctionnels de domaine et de forêt

>S'applique à : Windows Server

Les niveaux fonctionnels déterminent les capacités de domaine ou de forêt Active Directory Domain Services (AD DS). Ils déterminent également quels systèmes d’exploitation Windows Server vous pouvez exécuter sur des contrôleurs de domaine dans le domaine ou la forêt. Toutefois, les niveaux fonctionnels n’affectent pas les systèmes d’exploitation que vous pouvez exécuter sur des postes de travail et les serveurs membres qui sont joints au domaine ou à la forêt.

Lorsque vous déployez AD DS, définissez les niveaux fonctionnels du domaine et de la forêt sur la valeur la plus élevée prise en charge par votre environnement. Vous pourrez ainsi utiliser autant de fonctionnalités AD DS que possible. Lorsque vous déployez une nouvelle forêt, vous êtes invité à définir le niveau fonctionnel de celle-ci, puis le niveau fonctionnel du domaine. Vous pouvez définir le niveau fonctionnel du domaine sur une valeur supérieure au niveau fonctionnel de la forêt, mais vous ne pouvez pas définir le niveau fonctionnel du domaine sur une valeur inférieure au niveau fonctionnel de la forêt.

Avec la fin de vie de Windows 2003, les contrôleurs de domaine Windows 2003 doivent être mis à jour vers Windows Server 2008, 2008R2, 2012, 2012R2, 2016 ou 2019. Par conséquent, tous les contrôleurs de domaine qui exécutent Windows Server 2003 doivent être supprimés du domaine.

Aux niveaux fonctionnels de domaine Windows Server 2008 et supérieurs, la réplication DFS (Distributed File Service) est utilisée pour répliquer le contenu du dossier SYSVOL entre les contrôleurs de domaine. Si vous créez un domaine au niveau fonctionnel de domaine Windows Server 2008 ou supérieur, la réplication DFS est automatiquement utilisée pour répliquer SYSVOL. Si vous avez créé le domaine à un niveau fonctionnel inférieur, vous devez passer de l’utilisation de FRS à la réplication DFS pour SYSVOL. Pour connaître les étapes de migration, reportez-vous aux [procédures sur TechNet](https://technet.microsoft.com/library/dd640019(v=WS.10).aspx) ou à l’[ensemble de procédures simplifiées sur le blog Storage Team File Cabinet](https://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx).

## <a name="windows-server-2019"></a>Windows Server 2019

Aucun nouveau niveau fonctionnel de domaine ou de forêt n’est ajouté dans cette version.

La configuration requise minimale pour ajouter un contrôleur de domaine Windows Server 2019 est le niveau fonctionnel Windows Server 2008. Le domaine doit également utiliser DFS-R comme moteur pour répliquer SYSVOL.

## <a name="windows-server-2016"></a>Windows Server 2016

Système d’exploitation de contrôleur de domaine pris en charge :

* Windows Server 2019
* Windows Server 2016

### <a name="windows-server-2016-forest-functional-level-features"></a>Fonctionnalités du niveau fonctionnel de forêt Windows Server 2016

* Toutes les fonctionnalités du niveau fonctionnel de forêt Windows Server 2012 R2, plus les fonctionnalités suivantes sont disponibles :
   * [La gestion des accès privilégiés (Privileged Access Management, PAM) utilisant Microsoft Identity Manager (MIM)](https://docs.microsoft.com/windows-server/identity/whats-new-active-directory-domain-services#a-namebkmkpamaprivileged-access-management)

### <a name="windows-server-2016-domain-functional-level-features"></a>Fonctionnalités du niveau fonctionnel de domaine Windows Server 2016

* Toutes les fonctionnalités Active Directory par défaut, toutes les fonctionnalités du niveau fonctionnel de domaine Windows Server 2012 R2, plus les fonctionnalités suivantes :
   * Les contrôleurs de domaine peuvent prendre en charge le déroulement automatique des NTLM et autres secrets basés sur un mot de passe sur un compte d’utilisateur configuré pour exiger l’authentification PKI. Cette configuration est également appelée « Carte à puce requise pour l’ouverture de session interactive »
   * Les contrôleurs de domaine peuvent prendre en charge l’autorisation de réseau NTLM lorsqu’un utilisateur est limité à certains appareils associés à un domaine.
   * Les clients Kerberos qui sont authentifiés avec l’extension PKInit Freshness obtiennent le SID d’identité de clé publique actualisé.

    Pour plus d’informations, voir [Nouveautés de l’authentification Kerberos](https://docs.microsoft.com/windows-server/security/kerberos/whats-new-in-kerberos-authentication) et [Nouveautés de la protection des informations d’identification](https://docs.microsoft.com/windows-server/security/credentials-protection-and-management/whats-new-in-credential-protection)

## <a name="windows-server-2012r2"></a>Windows Server 2012 R2

Système d’exploitation de contrôleur de domaine pris en charge :

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2

### <a name="windows-server-2012r2-forest-functional-level-features"></a>Fonctionnalités du niveau fonctionnel de forêt Windows Server 2012 R2

* Toutes les fonctionnalités du niveau fonctionnel de forêt Windows Server 2012, mais pas de fonctionnalités supplémentaires.

### <a name="windows-server-2012r2-domain-functional-level-features"></a>Fonctionnalités du niveau de domaine Windows Server 2012 R2

* Toutes les fonctionnalités Active Directory par défaut, toutes les fonctionnalités du niveau fonctionnel du domaine Windows Server 2012, plus les fonctionnalités suivantes :
   * Protections côté contrôleur de domaine pour les utilisateurs protégés. Les utilisateurs protégés qui s’authentifient auprès d’un domaine Windows Server 2012 R2 ne peuvent plus effectuer les opérations suivantes :
      * s’authentifier avec l’authentification NTLM ;
      * utiliser les suites de chiffrement DES ou RC4 dans la pré-authentification Kerberos ;
      * être délégués en utilisant la délégation non contrainte ou contrainte ;
      * renouveler les tickets TGT utilisateur au-delà de la durée de vie initiale de 4 heures.
   * Stratégies d'authentification
      * Nouvelles stratégies Active Directory basées sur une forêt qui peuvent être appliquées à des comptes dans les domaines Windows Server 2012 R2 pour contrôler les hôtes à partir desquels un compte peut se connecter et appliquer des conditions de contrôle d’accès pour l’authentification auprès de services s’exécutant en tant que compte.
   * Silos de stratégies d'authentification
      * Nouvel objet Active Directory basé sur une forêt, qui peut créer une relation entre des comptes d’utilisateurs, de services gérés et d’ordinateurs permettant de classer les comptes pour les stratégies d’authentification ou pour l’isolation de l’authentification.

## <a name="windows-server-2012"></a>Windows Server 2012

Système d’exploitation de contrôleur de domaine pris en charge :

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012

### <a name="windows-server-2012-forest-functional-level-features"></a>Fonctionnalités du niveau fonctionnel de forêt Windows Server 2012

* Toutes les fonctionnalités du niveau fonctionnel de forêt Windows Server 2008 R2, mais pas de fonctionnalités supplémentaires.

### <a name="windows-server-2012-domain-functional-level-features"></a>Fonctionnalités du niveau fonctionnel de domaine Windows Server 2012

* Toutes les fonctionnalités Active Directory par défaut, toutes les fonctionnalités du niveau fonctionnel de domaine Windows Server 2008 R2, plus les fonctionnalités suivantes :
   * La prise en charge du centre de distribution de clés pour les revendications, l’authentification composée et la stratégie de modèles d’administration du centre de distribution de clés du blindage Kerberos possède deux paramètres (Toujours fournir des revendications et Rejeter les demandes d’authentification non blindées) qui nécessitent le niveau fonctionnel de domaine Windows Server 2012. Pour plus d’informations, voir [Nouveautés de l’authentification Kerberos](https://technet.microsoft.com/library/hh831747.aspx).

## <a name="windows-server-2008r2"></a>Windows Server 2008 R2

Système d’exploitation de contrôleur de domaine pris en charge :

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2

### <a name="windows-server-2008r2-forest-functional-level-features"></a>Fonctionnalités du niveau fonctionnel de forêt Windows Server 2008 R2

* Toutes les fonctionnalités du niveau fonctionnel de forêt Windows Server 2003, plus les fonctionnalités suivantes :
   * La corbeille Active Directory offre la possibilité de restaurer des objets supprimés dans leur intégralité pendant que les services de domaine Active Directory sont en cours d’exécution.

### <a name="windows-server-2008r2-domain-functional-level-features"></a>Fonctionnalités du niveau fonctionnel de domaine Windows Server 2008 R2

* Toutes les fonctionnalités Active Directory par défaut, toutes les fonctionnalités du niveau fonctionnel de domaine Windows Server 2008, plus les fonctionnalités suivantes :
   * L’assurance du mécanisme d’authentification stocke des informations sur le type de méthode d’ouverture de session (carte à puce ou nom d’utilisateur/mot de passe) utilisé pour authentifier les utilisateurs d’un domaine à l’intérieur du jeton Kerberos de chaque utilisateur. Lorsque cette fonctionnalité est activée dans un environnement réseau ayant déployé une infrastructure de gestion des identités fédérées, telle que les services AD FS (Active Directory Federation Services), les informations contenues dans le jeton peuvent être extraites chaque fois qu’un utilisateur tente d’accéder à une application prenant en charge les revendications, développée de façon à déterminer l’autorisation en fonction de la méthode d’ouverture de session d’un utilisateur.
   * Gestion automatique des noms principaux de service pour les services en cours d’exécution sur un ordinateur particulier dans le contexte d’un compte de service géré lorsque le nom ou le nom d’hôte DNS du compte d’ordinateur change. Pour plus d’informations sur les Comptes de service administrés, consultez [Guide pas à pas des comptes de service](https://go.microsoft.com/fwlink/?LinkId=180401).

## <a name="windows-server-2008"></a>Windows Server 2008

Système d’exploitation de contrôleur de domaine pris en charge :

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008

### <a name="windows-server-2008-forest-functional-level-features"></a>Fonctionnalités du niveau fonctionnel de forêt Windows Server 2008

* Toutes les fonctionnalités du niveau fonctionnel de forêt Windows Server 2003, mais aucune fonctionnalité supplémentaire n’est disponible. 

### <a name="windows-server-2008-domain-functional-level-features"></a>Fonctionnalités du niveau fonctionnel de domaine Windows Server 2008

* Toutes les fonctionnalités AD DS par défaut, toutes les fonctionnalités du niveau fonctionnel de domaine Windows Server 2003, plus les fonctionnalités suivantes sont disponibles  :
  * Prise en charge de la réplication du système de fichiers DFS (Distributed File System) pour le volume système Windows Server 2003 (SYSVOL)
    * La prise en charge de la réplication DFS offre une réplication plus fiable et plus granulaire du contenu de SYSVOL.

      > [!NOTE]
      > À partir de Windows Server 2012 R2, le Service de réplication de fichiers (FRS) est déconseillé. Un nouveau domaine créé sur un contrôleur de domaine qui exécute au moins Windows Server 2012 R2 doit être défini sur le niveau fonctionnel de domaine Windows Server 2008 ou supérieur.

  * Les espaces de noms DFS de domaine exécutés en mode Windows Server 2008, qui prend en charge l’énumération basée sur l’accès et une extensibilité accrue. Les espace de noms de domaine en mode Windows Server 2008 exigent également que la forêt utilise le niveau fonctionnel de forêt Windows Server 2003. Pour plus d’informations, consultez [Choisir un type d’espace de noms](https://go.microsoft.com/fwlink/?LinkId=180400).
  * Prise en charge d’Advanced Encryption Standard (AES 128 et AES 256) pour le protocole Kerberos. Pour que les tickets TGT soient émis à l’aide d’AES, le niveau fonctionnel de domaine doit être Windows Server 2008 ou supérieur et le mot de passe de domaine doit être modifié. 
    * Pour plus d’informations, voir [Améliorations liées à Kerberos](https://technet.microsoft.com/library/cc749438(ws.10).aspx).

      > [!NOTE]
      >Des erreurs d’authentification peuvent se produire sur un contrôleur de domaine lorsque le niveau fonctionnel de domaine a été élevé à Windows Server 2008 ou supérieur si le contrôleur de domaine a déjà répliqué le changement DFL mais n’a pas encore actualisé le mot de passe krbtgt. Dans ce cas, un redémarrage du service KDC sur le contrôleur de domaine déclenchera une actualisation en mémoire du nouveau mot de passe krbtgt et résoudra les erreurs d’authentification associées.

  * L’élément [Dernière ouverture de session interactive](https://go.microsoft.com/fwlink/?LinkId=180387) affiche les informations suivantes :
     * le nombre total de tentatives d’ouverture de session ayant échoué sur un serveur Windows Server 2008 ou un poste de travail Windows Vista associé au domaine ;
     * le nombre total de tentatives d’ouverture de session ayant échoué après une ouverture de session réussie sur un serveur Windows Server 2008 ou un poste de travail Windows Vista ;
     * l’heure de la dernière tentative d’ouverture de session ayant échoué sur un serveur Windows Server 2008 ou un poste de travail Windows Vista ;
     * l’heure de la dernière tentative d’ouverture de session réussie sur un serveur Windows Server 2008 ou un poste de travail Windows Vista.
  * Les stratégies de mot de passe affinées vous permettent de spécifier les stratégies de mot de passe et de verrouillage de compte pour les utilisateurs et les groupes de sécurité globaux d’un domaine. Pour plus d’informations, voir le [guide pas à pas relatif à la configuration des stratégies de verrouillage de compte et de mot de passe affinées](https://go.microsoft.com/fwlink/?LinkID=91477).
  * Bureaux virtuels personnels
     * Pour utiliser la nouvelle fonctionnalité disponible sous l’onglet Bureau virtuel personnel de la boîte de dialogue Propriétés du compte d’utilisateur dans Utilisateurs et ordinateurs Active Directory, votre schéma AD DS doit être étendu pour Windows Server 2008 R2 (version de l’objet de schéma = 47). Pour plus d’informations, voir le [Guide pas à pas de déploiement des bureaux virtuels personnels via les connexions aux programmes RemoteApp et aux services Bureau à distance](https://go.microsoft.com/fwlink/?LinkId=183552).

## <a name="windows-server-2003"></a>Windows Server 2003

Système d’exploitation de contrôleur de domaine pris en charge :

* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003

### <a name="windows-server-2003-forest-functional-level-features"></a>Fonctionnalités du niveau fonctionnel de forêt Windows Server 2003

* Toutes les fonctionnalités AD DS par défaut, ainsi que les suivantes, sont disponibles :
   * Approbation de forêt
   * changement de nom de domaine ;
   * Réplication de valeurs liées
      - La réplication de valeurs liées vous permet de modifier l’appartenance de groupe de façon à stocker et répliquer des valeurs pour chaque membre au lieu de répliquer l’ensemble de l’appartenance comme une seule unité. Le fait de stocker et de répliquer les valeurs de chaque membre utilise moins de bande passante réseau et moins de cycles de processeur au cours de la réplication. Cela vous empêche également de perdre des mises à jour lorsque vous ajoutez ou supprimez plusieurs membres simultanément auprès de différents contrôleurs de domaine.
   * Possibilité de déployer un contrôleur de domaine en lecture seule (RODC)
   * Extensibilité et algorithmes du vérificateur de cohérence des données améliorés
      - Le générateur de topologie intersite (ISTG) utilise des algorithmes améliorés qui s’adaptent pour prendre en charge des forêts contenant un nombre de sites supérieur à celui pouvant être pris en charge par le niveau fonctionnel de forêt Windows 2000. L’algorithme d’élection amélioré du générateur de topologie intersite propose un mécanisme de sélection d’ISTG au niveau fonctionnel de la forêt Windows 2000 moins contraignant.
   * Possibilité de créer des instances de la classe auxiliaire dynamique appelée **dynamicObject** dans une partition d’annuaire de domaine.
   * Possibilité de convertir une instance d’objet **inetOrgPerson** en instance d’objet **User** et d’effectuer la conversion en sens inverse.
   * Possibilité de créer des instances de nouveaux types de groupes pour prendre en charge l’autorisation basée sur les rôles. 
      - Ces types sont appelés groupes de base d’application et groupes de requête LDAP.
   * désactivation et redéfinition des attributs et classes du schéma. Les attributs suivants peuvent être réutilisés : ldapDisplayName, schemaIdGuid, OID et mapiID.
   * Les espaces de noms DFS de domaine exécutés en mode Windows Server 2008, qui prend en charge l’énumération basée sur l’accès et une extensibilité accrue. Pour plus d’informations, consultez [Choisir un type d’espace de noms](https://go.microsoft.com/fwlink/?LinkId=180400).

### <a name="windows-server-2003-domain-functional-level-features"></a>Fonctionnalités du niveau fonctionnel de domaine Windows Server 2003

* Toutes les fonctionnalités AD DS par défaut, toutes les fonctionnalités du niveau fonctionnel de domaine natif Windows 2000 ainsi que les fonctionnalités suivantes sont disponibles :
   * L’outil de gestion de domaine, Netdom.exe, qui vous permet de renommer les contrôleurs de domaine
   * Mises à jour relatives à l’horodatage d’ouverture de session
      * L’attribut **lastLogonTimestamp** est mis à jour avec l’heure de la dernière ouverture de session de l’utilisateur ou ordinateur. Cet attribut est répliqué au sein du domaine. ;
   * Possibilité de définir l’attribut **userPassword** comme mot de passe effectif pour l’objet **inetOrgPerson** et les objets utilisateur
   * Possibilité de rediriger les conteneurs Utilisateurs et ordinateurs
      * Par défaut, deux conteneurs connus sont fournis pour héberger les comptes d’ordinateurs et d’utilisateurs, à savoir cn=Computers,<domain root> et cn=Users,<domain root>. Cette fonctionnalité permet de définir un nouvel emplacement connu pour ces comptes.
   * Possibilité pour le Gestionnaire d’autorisations de stocker ses stratégies d’autorisation dans les services de domaine Active Directory (AD DS)
   * Délégation contrainte
      * La délégation contrainte permet aux applications de tirer parti de la délégation sécurisée des informations d’identification de l’utilisateur au moyen du protocole d’authentification Kerberos
      * Vous pouvez limiter la délégation à certains services de destination.
   * Authentification sélective
      * L’authentification sélective vous permet de spécifier les utilisateurs et les groupes d’une forêt approuvée autorisés à s’authentifier auprès des serveurs de ressources d’une forêt d’approbation.

## <a name="windows-2000"></a>Windows 2000

Système d’exploitation de contrôleur de domaine pris en charge :

* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003
* Windows 2000

### <a name="windows-2000-native-forest-functional-level-features"></a>Fonctionnalités du niveau fonctionnel de forêt natif Windows 2000

* Toutes les fonctionnalités AD DS par défaut sont disponibles.

### <a name="windows-2000-native-domain-functional-level-features"></a>Fonctionnalités du niveau fonctionnel de domaine natif Windows 2000

* Toutes les fonctionnalités AD DS par défaut et les fonctionnalités d’annuaire suivantes sont disponibles, notamment :
   * groupes universels pour les groupes de distribution et les groupes de sécurité ;
   * imbrication de groupes ;
   * conversion de groupe, qui permet la conversion entre groupes de sécurité et de distribution
   * historique SID.

## <a name="next-steps"></a>Étapes suivantes

* [Augmenter le niveau fonctionnel de domaine](https://technet.microsoft.com/library/cc753104.aspx)  
* [Augmenter le niveau fonctionnel de forêt](https://technet.microsoft.com/library/cc730985.aspx)
