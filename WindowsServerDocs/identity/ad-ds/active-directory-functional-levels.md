---
ms.assetid: f964d056-11bf-4d9b-b5ab-dceaad8bfbc3
title: Niveaux fonctionnels de Windows Server 2016
description: 
author: MicrosoftGuyJFlo
ms.author: joflore
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.custom: it-pro
ms.reviewer: maheshu
ms.technology: identity-adds
ms.openlocfilehash: a39955cf088ce7ce8bef20c70b83d49c6d508497
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="forest-and-domain-functional-levels"></a>Niveaux fonctionnels de forêt et du domaine

>S’applique à: Windows Server

À la fin de la durée de vie de Windows 2003, Windows 2003 des contrôleurs de domaine (DC) doivent être mis à jour vers Windows Server 2008, 2012 ou 2016. Par conséquent, n’importe quel contrôleur de domaine qui exécute Windows Server 2003 doit être supprimé du domaine. Le niveau fonctionnel du domaine et de forêt doit être déclenché au moins Windows Server 2008 pour empêcher un contrôleur de domaine qui exécute une version antérieure de Windows Server d’être ajouté à l’environnement.

Nous recommandons que les clients mettre à jour leur niveau fonctionnel du domaine (DFL) et le niveau fonctionnel de forêt (FFL) dans le cadre de ce, dans la mesure où le niveau fonctionnel du domaine 2003 et FFL sont déconseillés dans Windows Server 2016 et ils seront plus être pris en charge dans les futures versions.

Pour les clients qui ont besoin de temps supplémentaire pour évaluer le déplacement de leur niveau fonctionnel du domaine et le FFL de 2003, le niveau fonctionnel du domaine 2003 et FFL continueront à être pris en charge avec Windows 10 et Windows Server 2016 fourni tous les contrôleurs de domaine dans le domaine et la forêt sont soit sur Windows Server 2008, 2008 R2, 2012, 2012 R2, ou 2016.

Aux niveaux fonctionnels de domaine plus élevés et Windows Server 2008, la réplication du Service de fichiers distribués (DFS) est utilisée pour répliquer le contenu du dossier SYSVOL entre les contrôleurs de domaine. Si vous créez un nouveau domaine au niveau fonctionnel de domaine Windows Server 2008 ou version ultérieure, la réplication DFS est automatiquement utilisée pour répliquer SYSVOL. Si vous avez créé le domaine à un niveau fonctionnel inférieur, vous devez migrer à partir de l’aide de FRS vers la réplication DFS pour SYSVOL. Pour les étapes de migration, vous pouvez soit suivre le [procédures sur TechNet](https://technet.microsoft.com/library/dd640019(v=WS.10).aspx) ou vous pouvez faire référence à la [simplifié des étapes sur le blog File Cabinet équipe de stockage](http://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx).

Les Windows Server 2003 domaine et forêt niveaux fonctionnels de continuent à être pris en charge, mais les organisations doivent augmenter le niveau fonctionnel de Windows Server 2008 (ou version ultérieure si possible) pour assurer la compatibilité de la réplication SYSVOL et prendre en charge à l’avenir. En outre, il existe les nombreux autres avantages et fonctionnalités disponibles aux niveaux fonctionnels supérieurs plus élevés. Consultez les ressources suivantes pour plus d’informations:

## <a name="windows-server-2016"></a>Windows Server2016

Prise en charge du système d’exploitation de contrôleur de domaine:

* Windows Server2016

### <a name="windows-server-2016-forest-functional-level-features"></a>Fonctionnalités au niveau fonctionnelles de forêt Windows Server 2016

* Toutes les fonctionnalités qui sont disponibles sur le niveau fonctionnel de la forêt Windows Server 2012 R2 et les fonctionnalités suivantes sont disponibles:
    * [Gestion des accès privilégiés (PAM) à l’aide de Microsoft Identity Manager (MIM)] (https://docs.microsoft.com/windows-server/identity/whats-new-active-directory-domain-services#a-namebkmkpamaprivileged-access-management)

### <a name="windows-server-2016-domain-functional-level-features"></a>Fonctionnalités au niveau fonctionnelles du domaine Windows Server 2016

* Toutes les fonctionnalités d’Active Directory par défaut, toutes les fonctionnalités de Windows Server 2012 R2 niveau fonctionnel du domaine, plus les fonctionnalités suivantes:
    * Contrôleurs de domaine peuvent prendre en charge propagée secrets NTLM publique clé uniquement d’un utilisateur. 
    * Contrôleurs de domaine peuvent prendre en charge réseau permettant NTLM lorsqu’un utilisateur est limité à des appareils joints au domaine. 
    * Les clients Kerberos avec succès l’authentification avec l’Extension de rafraîchissement du PKInit Obtient l’identité de clé publique nouvelle SID.

    Pour plus d’informations, voir [What ' s New in Kerberos Authentication](https://docs.microsoft.com/windows-server/security/kerberos/whats-new-in-kerberos-authentication) et [Nouveautés de la Protection des informations d’identification](https://docs.microsoft.com/windows-server/security/credentials-protection-and-management/whats-new-in-credential-protection)

## <a name="windows-server-2012r2"></a>Windows de Windows Server 2012 R2

Prise en charge du système d’exploitation de contrôleur de domaine:

* Windows Server2016
* Windows Server2012R2

### <a name="windows-server-2012r2-forest-functional-level-features"></a>Fonctionnalités au niveau fonctionnelles de Windows Server 2012 R2 forêt

* Toutes les fonctionnalités qui sont disponibles sur le serveur Windows Server 2012 des fonctionnalités de niveau, mais d’autres non fonctionnelles de forêt.

### <a name="windows-server-2012r2-domain-functional-level-features"></a>Fonctionnalités au niveau fonctionnelles de Windows Server 2012 R2 domaine

* Toutes les fonctionnalités d’Active Directory par défaut, toutes les fonctionnalités du niveau fonctionnel du domaine Windows Server 2012, plus les fonctionnalités suivantes:
    * Protections côté contrôleur de domaine pour les utilisateurs protégés. Protégé par l’authentification des utilisateurs à un domaine ne peut plus Windows Server 2012 R2:
        * S’authentifier avec l’authentification NTLM
        * Utiliser les suites de chiffrement DES ou RC4 dans la pré-authentification Kerberos
        * Être délégués avec délégation non contrainte ou contrainte
        * Renouveler les tickets utilisateur (TGT) au-delà de la durée de vie initiale de 4 heures
    * Stratégies d’authentification
        * Nouvelle forêt Active Directory stratégies qui peuvent être appliquées aux comptes dans des domaines Windows Server 2012 R2 pour contrôler les hôtes un compte peut ouverture de session à partir d’et appliquer des conditions de contrôle d’accès pour l’authentification aux services en cours d’exécution en tant que compte.
    * Silos de stratégies d’authentification
        * Nouvelle forêt Active Directory objet, qui peut créer une relation entre les comptes d’utilisateurs, service géré et l’ordinateur, pour être utilisées pour classer les comptes pour les stratégies d’authentification ou pour l’isolation de l’authentification.

## <a name="windows-server-2012"></a>Windows Server2012

Prise en charge du système d’exploitation de contrôleur de domaine:

* Windows Server2016
* Windows Server2012R2
* Windows Server2012

### <a name="windows-server-2012-forest-functional-level-features"></a>Fonctionnalités au niveau fonctionnelles de forêt Windows Server 2012

* Toutes les fonctionnalités qui sont disponibles sur le serveur Windows Server 2008 R2 des fonctionnalités de niveau, mais d’autres non fonctionnelles de forêt.

### <a name="windows-server-2012-domain-functional-level-features"></a>Fonctionnalités au niveau fonctionnelles du domaine Windows Server 2012

* Toutes les fonctionnalités d’Active Directory par défaut, toutes les fonctionnalités de Windows Server 2008 R2 niveau fonctionnel du domaine, plus les fonctionnalités suivantes:
    * La prise en charge des revendications, l’authentification composée et Kerberos blindage de stratégie de modèle d’administration KDC a deux paramètres (toujours fournir des revendications et rejeter les demandes d’authentification non blindées) qui nécessitent le niveau fonctionnel du domaine Windows Server 2012. Pour plus d’informations, voir [What ' s New in Kerberos Authentication](https://technet.microsoft.com/en-us/library/hh831747.aspx)

## <a name="windows-server-2008r2"></a>Windows Server 2008 R2

Prise en charge du système d’exploitation de contrôleur de domaine:

* Windows Server2016
* Windows Server2012R2
* Windows Server2012
* Windows Server2008R2

### <a name="windows-server-2008r2-forest-functional-level-features"></a>Fonctionnalités au niveau fonctionnelles de Windows Server 2008 R2 forêt

* Toutes les fonctionnalités qui sont disponibles sur le serveur Windows Server 2003 de la forêt au niveau fonctionnel, plus les fonctionnalités suivantes:
    * La Corbeille Active Directory, qui offre la possibilité de restaurer des objets supprimés dans leur intégralité pendant que les services AD DS est en cours d’exécution.

### <a name="windows-server-2008r2-domain-functional-level-features"></a>Fonctionnalités au niveau fonctionnelles de Windows Server 2008 R2 domaine

* Toutes les fonctionnalités d’Active Directory par défaut, toutes les fonctionnalités du niveau fonctionnel du domaine Windows Server 2008, plus les fonctionnalités suivantes:
    * Assurance du mécanisme d’authentification, qui stocke des informations sur le type de méthode d’ouverture de session (carte à puce ou nom d’utilisateur/mot de passe) qui est utilisé pour authentifier les utilisateurs du domaine à l’intérieur du jeton Kerberos de chaque utilisateur. Lorsque cette fonctionnalité est activée dans un environnement réseau qui a déployé une infrastructure de gestion des identités fédérées, tels que les Services de fédération Active Directory (AD FS), les informations contenues dans le jeton peuvent être extraites chaque fois qu’un utilisateur tente d’accéder à n’importe quelle application prenant en charge les revendications qui a été développée pour déterminer l’autorisation basée sur la méthode d’ouverture de session d’un utilisateur.
    * Gestion automatique des SPN pour les services en cours d’exécution sur un ordinateur particulier dans le contexte d’un compte de Service géré lorsque le nom DNS ou le nom d’hôte de modifications du compte d’ordinateur. Pour plus d’informations sur les comptes de Service administrés, voir [Guide pas à pas de comptes de Service](https://go.microsoft.com/fwlink/?LinkId=180401).

## <a name="windows-server-2008"></a>Windows Server2008

Prise en charge du système d’exploitation de contrôleur de domaine:

* Windows Server2016
* Windows Server2012R2
* Windows Server2012
* Windows Server2008R2
* Windows Server2008

### <a name="windows-server-2008-forest-functional-level-features"></a>Fonctionnalités au niveau fonctionnelles de forêt Windows Server 2008

* Toutes les fonctionnalités qui sont disponibles sur le niveau fonctionnel de forêt Windows Server 2003, mais pas de fonctionnalités supplémentaires sont disponibles. 

### <a name="windows-server-2008-domain-functional-level-features"></a>Fonctionnalités au niveau fonctionnelles du domaine Windows Server 2008

* Tous les de la valeur par défaut les services AD DS fonctionnalités, toutes les fonctionnalités à partir du niveau fonctionnel du domaine Windows Server 2003, et les fonctionnalités suivantes sont disponibles:
    * Support de réplication DFS (File System) distribuée pour le Volume du système Windows Server 2003 (SYSVOL)
        * Prise en charge de la réplication DFS offre une réplication plus fiable et granulaire du contenu de SYSVOL.
        [!NOTE]>
        >À compter de Windows Server 2012 R2, le Service de réplication de fichiers (FRS) est déconseillée. Un nouveau domaine est créé sur un contrôleur de domaine qui exécute au moins Windows Server 2012 R2 doit être définie sur le niveau fonctionnel de domaine Windows Server 2008 ou une version ultérieure.

    * Domaine espaces de noms DFS en cours d’exécution en Mode Windows Server 2008, qui inclut la prise en charge de l’énumération basée sur l’accès et une extensibilité accrue. Espaces de noms basés sur un domaine en mode Windows Server 2008 requièrent également la forêt à utiliser le niveau fonctionnel de forêt Windows Server 2003. Pour plus d’informations, voir [choisir un Namespace Type](https://go.microsoft.com/fwlink/?LinkId=180400).
    * Encryption Standard (AES 128 et AES 256) prise en charge avancée pour le protocole Kerberos. Dans l’ordre des tickets TGT à être émis à l’aide de AES, le niveau fonctionnel du domaine doit être Windows Server 2008 ou version supérieure, et le mot de passe de domaine doit être modifié. 
        * Pour plus d’informations, voir [améliorations liées à Kerberos](https://technet.microsoft.com/library/cc749438(ws.10).aspx).
        [!NOTE]>
        >Erreurs d’authentification peuvent se produire sur un contrôleur de domaine une fois le niveau fonctionnel du domaine est déclenché pour Windows Server 2008 ou version ultérieure si le contrôleur de domaine a déjà répliqué la modification du niveau fonctionnel du domaine, mais le mot de passe krbtgt n’a pas encore été actualisé. Dans ce cas, un redémarrage du service KDC sur le contrôleur de domaine déclenche une actualisation en mémoire du nouveau mot de passe krbtgt et résoudre les erreurs d’authentification associés.

    * [La dernière ouverture de session Interactive](https://go.microsoft.com/fwlink/?LinkId=180387) informations affichent les informations suivantes:
        * Le nombre total de tentatives de connexion à un serveur Windows Server 2008 joints au domaine ou une station de travail Windows Vista
        * Le nombre total de tentatives d’ouverture de session ayant échoué après une ouverture de session réussie sur un serveur Windows Server 2008 ou une station de travail Windows Vista
        * L’heure de la dernière tentative d’ouverture de session ayant échoué sur un ordinateur Windows Server 2008 ou une station de travail Windows Vista
        * Tentative de l’heure de la dernière ouverture de session réussie sur un serveur Windows Server 2008 ou une station de travail Windows Vista
    * Les stratégies de mot de passe affinées permettent de spécifier des stratégies de verrouillage de compte et de mot de passe pour les utilisateurs et groupes de sécurité globaux dans un domaine. Pour plus d’informations, voir [Guide pas à pas pour la Configuration de stratégie de verrouillage de compte et de mot de passe affinées](https://go.microsoft.com/fwlink/?LinkID=91477).
    * Bureaux virtuels personnels
        * Pour utiliser les fonctionnalités ajoutées fournies par l’onglet Bureau virtuel personnel dans la boîte de dialogue Propriétés de compte d’utilisateur dans Active Directory Users and Computers, votre schéma AD DS doit être étendu pour Windows Server 2008 R2 (version de l’objet schéma = 47). Pour plus d’informations, voir [déploiement de bureaux virtuels personnels à l’aide de RemoteApp et Guide pas à pas de connexion Bureau](https://go.microsoft.com/fwlink/?LinkId=183552).

## <a name="windows-server-2003"></a>Windows Server2003

Prise en charge du système d’exploitation de contrôleur de domaine:

* Windows Server2012R2
* Windows Server2012
* Windows Server2008R2
* Windows Server2008
* Windows Server2003

### <a name="windows-server-2003-forest-functional-level-features"></a>Fonctionnalités au niveau fonctionnelles de forêt Windows Server 2003

* Toutes les fonctionnalités par défaut les services AD DS et les fonctionnalités suivantes sont disponibles:
    * Approbation de forêt
    * Changement de nom de domaine
    * Réplication de valeurs liées
        - Réplication de valeurs liées permet de modifier l’appartenance au groupe pour stocker et répliquer les valeurs pour chaque membre au lieu de répliquer l’ensemble de l’appartenance comme une seule unité. Stocker et répliquer les valeurs de membres individuels utilise moins de bande passante réseau et moins de cycles processeur lors de la réplication et vous empêche de perdre des mises à jour lorsque vous ajoutez ou supprimez des membres de plusieurs simultanément sur différents contrôleurs de domaine.
    * La possibilité de déployer un contrôleur de domaine en lecture seule (RODC)
    * Algorithmes du vérificateur de cohérence de connaissances (KCC) et l’extensibilité améliorée
        - Le Générateur de topologie intersite (ISTG) utilise des algorithmes améliorés qui s’adaptent pour prendre en charge des forêts avec un plus grand nombre de sites que les services AD DS peut prendre en charge au niveau fonctionnel de forêt Windows 2000. L’algorithme d’élection ISTG amélioré est un mécanisme moins intrusif pour la sélection d’ISTG au niveau fonctionnel de forêt Windows 2000.
    * La possibilité de créer des instances de la classe auxiliaire dynamique appelée **dynamicObject** dans une partition d’annuaire de domaine
    * Possibilité de convertir un **inetOrgPerson** instance d’objet dans un **utilisateur** instance d’objet et pour effectuer la conversion dans la direction opposée
    * La possibilité de créer des instances de nouveaux types de groupe pour prendre en charge d’autorisation basée sur un rôle. 
        - Ces types sont appelés groupes de base d’application et les groupes de requêtes LDAP.
    * Désactivation et redéfinition des attributs et classes du schéma. Les attributs suivants peuvent être réutilisées: ldapDisplayName, schemaIdGuid, l’OID et mapiID.
    * Domaine espaces de noms DFS en cours d’exécution en Mode Windows Server 2008, qui inclut la prise en charge de l’énumération basée sur l’accès et une extensibilité accrue. Pour plus d’informations, voir [choisir un Namespace Type](https://go.microsoft.com/fwlink/?LinkId=180400).

### <a name="windows-server-2003-domain-functional-level-features"></a>Fonctionnalités de niveau fonctionnelles du domaine Windows Server 2003

* Toutes les fonctionnalités par défaut les services AD DS, toutes les fonctionnalités qui sont disponibles sur le niveau fonctionnel de domaine en mode natif de Windows 2000 et les fonctionnalités suivantes sont disponibles:
    * L’outil de gestion de domaine, Netdom.exe, ce qui rend possible pour vous permet de renommer des contrôleurs de domaine
    * Les mises à jour et l’heure d’ouverture de session
        * Le **lastLogonTimestamp** attribut est mis à jour avec la dernière ouverture de session de l’utilisateur ou l’ordinateur. Cet attribut est répliqué au sein du domaine.
    * La possibilité de définir le **userPassword** en tant que le mot de passe de l’attribut sur **inetOrgPerson** et les objets utilisateur
    * La possibilité de rediriger les utilisateurs et ordinateurs des conteneurs
        * Par défaut, deux conteneurs connus sont fournis pour héberger les comptes d’ordinateur et utilisateur, à savoir, cn = Computers,<domain root> et cn = Users,<domain root>. Cette fonctionnalité permet la définition d’un nouvel emplacement connu pour ces comptes.
    * La possibilité de gestionnaire d’autorisations de stocker ses stratégies d’autorisation dans les services AD DS
    * Délégation contrainte
        * La délégation contrainte permet aux applications de tirer parti de la délégation sécurisée des informations d’identification de l’utilisateur au moyen de l’authentification Kerberos.
        * Vous pouvez limiter la délégation aux services de destination spécifique.
    * Authentification sélective
        * Facilite l’authentification sélective qu'il est possible que vous pouvez spécifier les utilisateurs et groupes d’une forêt approuvée autorisés à s’authentifier auprès des serveurs de ressources dans une forêt d’approbation.

## <a name="windows-2000"></a>Windows2000

Prise en charge du système d’exploitation de contrôleur de domaine:

* Windows Server2008R2
* Windows Server2008
* Windows Server2003
* Windows2000

### <a name="windows-2000-native-forest-functional-level-features"></a>Fonctionnalités du niveau fonctionnelles Windows 2000 natif de forêt

* Toutes les fonctionnalités du service d’annuaire Active Directory par défaut sont disponibles.

### <a name="windows-2000-native-domain-functional-level-features"></a>Fonctionnalités du niveau fonctionnelles Windows 2000 natif

* Toutes les fonctionnalités par défaut les services AD DS et les fonctionnalités de répertoire suivantes sont disponibles, y compris:
    * Groupes universels pour les groupes de distribution et de sécurité.
    * Imbrication de groupes
    * Conversion de groupe, ce qui permet la conversion entre les groupes de sécurité et de distribution
    * Historique de sécurité (SID) d’identificateur

## <a name="next-steps"></a>Étapes suivantes

* [Augmenter le niveau fonctionnel du domaine](https://technet.microsoft.com/library/cc753104.aspx)  
* [Augmenter le niveau fonctionnel de forêt](https://technet.microsoft.com/library/cc730985.aspx)
