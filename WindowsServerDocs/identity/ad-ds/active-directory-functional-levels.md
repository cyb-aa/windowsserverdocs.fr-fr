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
ms.openlocfilehash: ea56c718394d145a36145d32e5769661a62efd56
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841000"
---
# <a name="forest-and-domain-functional-levels"></a>Niveaux fonctionnels de forêt et du domaine

>S'applique à : Windows Server

Niveaux fonctionnels de déterminent les fonctionnalités disponibles de domaine ou forêt de Services de domaine Active Directory (AD DS). Elles déterminent également quels systèmes d’exploitation de Windows Server que vous pouvez exécuter sur les contrôleurs de domaine dans le domaine ou la forêt. Toutefois, les niveaux fonctionnels n’affectent pas les systèmes d’exploitation que vous pouvez exécuter sur les stations de travail et les serveurs membres qui sont joints au domaine ou forêt.

Lorsque vous déployez les services AD DS, définir les niveaux fonctionnels de domaine et de forêt à la valeur la plus élevée que votre environnement peut prendre en charge. De cette façon, vous pouvez utiliser les fonctionnalités AD DS autant que possible. Lorsque vous déployez une nouvelle forêt, vous êtes invité pour définir le niveau fonctionnel de forêt, puis définissez le niveau fonctionnel du domaine. Vous pouvez définir le niveau fonctionnel du domaine à une valeur qui est plus élevée que le niveau fonctionnel de forêt, mais vous ne peut pas définir le niveau fonctionnel du domaine à une valeur inférieure à celle du niveau fonctionnel de forêt.

Avec la fin de vie de Windows 2003, Windows 2003 des contrôleurs de domaine (DC) doivent être mis à jour vers Windows Server 2008, 2008 R2, 2012, 2012 R2, 2016 ou 2019. Par conséquent, n’importe quel contrôleur de domaine qui exécute Windows Server 2003 doit être supprimé du domaine.

Au plus élevés niveaux fonctionnels de domaine et à Windows Server 2008, la réplication du Service de fichiers distribués (DFS, Distributed File System) est utilisée pour répliquer le contenu du dossier SYSVOL entre les contrôleurs de domaine. Si vous créez un nouveau domaine de niveau fonctionnel de domaine Windows Server 2008 ou version ultérieure, la réplication DFS est automatiquement utilisée pour répliquer SYSVOL. Si vous avez créé le domaine à un niveau fonctionnel inférieur, vous devez migrer à partir de l’utilisation de FRS pour la réplication DFS pour SYSVOL. Pour les étapes de migration, vous pouvez soit suivre les [procédures sur TechNet](https://technet.microsoft.com/library/dd640019(v=WS.10).aspx) ou vous pouvez faire référence à la [rationalisé l’ensemble d’étapes sur le blog de l’équipe stockage fichier CAB du fichier](http://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx).

## <a name="windows-server-2019"></a>Windows Server 2019

Il n’existe aucune nouvelle forêt ou les niveaux fonctionnels de domaine ajoutées dans cette version.

La configuration minimale requise pour ajouter un contrôleur de domaine Windows Server 2019 est un niveau fonctionnel de Windows Server 2008 R2.

## <a name="windows-server-2016"></a>Windows Server 2016

Prise en charge du système d’exploitation du contrôleur de domaine :

* Windows Server 2019
* Windows Server 2016

### <a name="windows-server-2016-forest-functional-level-features"></a>Fonctionnalités au niveau fonctionnelles de forêt Windows Server 2016

* Toutes les fonctionnalités qui sont disponibles pour le niveau fonctionnel de la forêt Windows Server 2012 R2 et les fonctionnalités suivantes, sont disponibles :
   * [Gestion des accès privilégiés (PAM) à l’aide de Microsoft Identity Manager (MIM)](https://docs.microsoft.com/windows-server/identity/whats-new-active-directory-domain-services#a-namebkmkpamaprivileged-access-management)

### <a name="windows-server-2016-domain-functional-level-features"></a>Fonctionnalités au niveau fonctionnelles du domaine Windows Server 2016

* Toutes les fonctionnalités d’Active Directory par défaut, toutes les fonctionnalités de Windows Server 2012 R2 niveau fonctionnel du domaine, ainsi que les fonctionnalités suivantes :
   * Contrôleurs de domaine peuvent prendre en charge la restauration automatique de NTLM et autres secrets en fonction du mot de passe sur un compte d’utilisateur configuré pour exiger une authentification de l’infrastructure à clé publique. Cette configuration est également appelé « Carte à puce requise pour l’ouverture de session interactive »
   * Contrôleurs de domaine peuvent prendre en charge réseau autorisant NTLM lorsqu’un utilisateur est limité à des appareils joints au domaine.
   * Les clients Kerberos avec succès l’authentification avec l’Extension de fraîcheur PKInit obtiennent l’identité de clé publique fraîche SID.

    Pour plus d’informations, consultez [What ' s New in Kerberos Authentication](https://docs.microsoft.com/windows-server/security/kerberos/whats-new-in-kerberos-authentication) et [Nouveautés de la Protection des informations d’identification](https://docs.microsoft.com/windows-server/security/credentials-protection-and-management/whats-new-in-credential-protection)

## <a name="windows-server-2012r2"></a>Windows Server 2012 R2

Prise en charge du système d’exploitation du contrôleur de domaine :

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2

### <a name="windows-server-2012r2-forest-functional-level-features"></a>Fonctionnalités au niveau fonctionnelles de Windows Server 2012 R2 forêt

* Toutes les fonctionnalités qui sont disponibles dans Windows Server 2012 des fonctionnalités de niveau, mais aucune supplémentaires fonctionnelles de forêt.

### <a name="windows-server-2012r2-domain-functional-level-features"></a>Fonctionnalités au niveau fonctionnelles de Windows Server 2012 R2 domaine

* Toutes les fonctionnalités d’Active Directory par défaut, toutes les fonctionnalités du niveau fonctionnel du domaine Windows Server 2012, ainsi que les fonctionnalités suivantes :
   * Protections côté contrôleur de domaine pour les utilisateurs protégés. Protégé par l’authentification des utilisateurs à un domaine ne peut plus Windows Server 2012 R2 :
      * Authentifier avec l’authentification NTLM
      * Utilisez les suites de chiffrement DES ou RC4 dans la pré-authentification Kerberos
      * Être délégués avec délégation non contrainte ou contrainte
      * Renouveler les tickets utilisateur (TGT) au-delà de la durée de vie initiale de 4 heures
   * Stratégies d'authentification
      * Nouvelle forêt Active Directory stratégies qui peuvent être appliquées aux comptes dans des domaines Windows Server 2012 R2 pour contrôler les hôtes un compte peut l’authentification à partir d’et appliquer des conditions de contrôle d’accès pour l’authentification pour les services qui s’exécutent en tant que compte.
   * Silos de stratégies d'authentification
      * Nouvelle forêt Active Directory objet, ce qui peut créer une relation entre les comptes d’utilisateur, service géré et ordinateur, à utiliser pour classer les comptes pour les stratégies d’authentification ou pour l’isolation de l’authentification.

## <a name="windows-server-2012"></a>Windows Server 2012

Prise en charge du système d’exploitation du contrôleur de domaine :

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012

### <a name="windows-server-2012-forest-functional-level-features"></a>Fonctionnalités au niveau fonctionnelles de forêt Windows Server 2012

* Toutes les fonctionnalités qui sont disponibles dans Windows Server 2008 R2 des fonctionnalités de niveau, mais aucune supplémentaires fonctionnelles de forêt.

### <a name="windows-server-2012-domain-functional-level-features"></a>Fonctionnalités au niveau fonctionnelles du domaine Windows Server 2012

* Toutes les fonctionnalités d’Active Directory par défaut, toutes les fonctionnalités de Windows Server 2008 R2 niveau fonctionnel du domaine, ainsi que les fonctionnalités suivantes :
   * La prise en charge des revendications, l’authentification composée et Kerberos blindage KDC d’administration de stratégie de modèle a deux paramètres (toujours fournir des revendications et rejeter les demandes d’authentification non blindées) qui nécessitent le niveau fonctionnel du domaine Windows Server 2012. Pour plus d’informations, consultez [What ' s New in Kerberos Authentication](https://technet.microsoft.com/library/hh831747.aspx)

## <a name="windows-server-2008r2"></a>Windows Server 2008R2

Prise en charge du système d’exploitation du contrôleur de domaine :

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2

### <a name="windows-server-2008r2-forest-functional-level-features"></a>Fonctionnalités au niveau fonctionnelles de Windows Server 2008 R2 forêt

* Toutes les fonctionnalités du niveau fonctionnel de forêt Windows Server 2003, plus les fonctionnalités suivantes :
   * La corbeille Active Directory offre la possibilité de restaurer des objets supprimés dans leur intégralité pendant que les services de domaine Active Directory sont en cours d’exécution.

### <a name="windows-server-2008r2-domain-functional-level-features"></a>Fonctionnalités au niveau fonctionnelles de Windows Server 2008 R2 domaine

* Toutes les fonctionnalités d’Active Directory par défaut, toutes les fonctionnalités du niveau fonctionnel du domaine de Windows Server 2008, ainsi que les fonctionnalités suivantes :
   * L’assurance du mécanisme d’authentification stocke des informations sur le type de méthode d’ouverture de session (carte à puce ou nom d’utilisateur/mot de passe) utilisé pour authentifier les utilisateurs d’un domaine à l’intérieur du jeton Kerberos de chaque utilisateur. Lorsque cette fonctionnalité est activée dans un environnement réseau qui a déployé une infrastructure de gestion fédérée des identités, telles que Active Directory Federation Services (ADFS), les informations contenues dans le jeton peuvent ensuite être extraites chaque fois qu’un utilisateur tente d’accéder à tous application prenant en charge les revendications qui a été développée pour déterminer l’autorisation basée sur une méthode de l’utilisateur d’ouverture de session.
   * Gestion automatique des SPN pour les services qui s’exécutent sur un ordinateur particulier dans le contexte d’un compte de Service géré lorsque le nom DNS ou le nom d’hôte des modifications de compte d’ordinateur. Pour plus d’informations sur les comptes de Service administrés, consultez [Guide pas à pas de comptes de Service](https://go.microsoft.com/fwlink/?LinkId=180401).

## <a name="windows-server-2008"></a>Windows Server 2008

Prise en charge du système d’exploitation du contrôleur de domaine :

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008

### <a name="windows-server-2008-forest-functional-level-features"></a>Fonctionnalités au niveau fonctionnelles de forêt Windows Server 2008

* Toutes les fonctionnalités qui sont disponibles pour le niveau fonctionnel de forêt Windows Server 2003, mais pas de fonctionnalités supplémentaires sont disponibles. 

### <a name="windows-server-2008-domain-functional-level-features"></a>Fonctionnalités au niveau fonctionnelles du domaine Windows Server 2008

* Tous de la valeur par défaut AD DS les fonctionnalités, toutes les fonctionnalités à partir du niveau fonctionnel du domaine Windows Server 2003, et les fonctionnalités suivantes sont disponibles :
   * Support de réplication distribués DFS (File System) pour le Volume du système Windows Server 2003 (SYSVOL)
      * Prise en charge de la réplication DFS fournit une réplication plus fiable et granulaire du contenu de SYSVOL.
        [!NOTE]>
        >À compter de Windows Server 2012 R2, Service de réplication de fichiers (FRS) est déconseillée. Un nouveau domaine est créé sur un contrôleur de domaine qui s’exécute au moins Windows Server 2012 R2 doit être définie sur le niveau fonctionnel de domaine Windows Server 2008 ou version ultérieure.

   * Basés sur domaine espaces de noms DFS en cours d’exécution en Mode Windows Server 2008, qui inclut la prise en charge de l’énumération basée sur l’accès et une meilleure évolutivité. Espaces de noms basés sur le domaine en mode Windows Server 2008 requièrent également la forêt à utiliser le niveau fonctionnel de forêt Windows Server 2003. Pour plus d’informations, consultez [choisir un Type de Namespace](https://go.microsoft.com/fwlink/?LinkId=180400).
   * Advanced Encryption Standard (AES 128 et AES 256) prise en charge le protocole Kerberos. Dans l’ordre pour les tickets TGT doit être émis à l’aide d’AES, le niveau fonctionnel du domaine doit être Windows Server 2008 ou version ultérieure et le mot de passe de domaine doit être modifiée. 
      * Pour plus d’informations, consultez [améliorations liées à Kerberos](https://technet.microsoft.com/library/cc749438(ws.10).aspx).
        [!NOTE]>
        >Erreurs d’authentification peuvent se produire sur un contrôleur de domaine après le déclenchement de niveau fonctionnel du domaine vers Windows Server 2008 ou version ultérieure si le contrôleur de domaine a déjà répliqué la modification du niveau fonctionnel du domaine, mais le mot de passe krbtgt n’a pas encore actualisé. Dans ce cas, un redémarrage du service KDC sur le contrôleur de domaine déclenche une actualisation en mémoire du nouveau mot de passe krbtgt et résoudre les erreurs d’authentification associées.

   * [Dernière ouverture de session Interactive](https://go.microsoft.com/fwlink/?LinkId=180387) informations affichent les informations suivantes :
      * Le nombre total de tentatives de connexion à un serveur Windows Server 2008 joints au domaine ou une station de travail Windows Vista
      * Le nombre total de tentatives de connexion après une ouverture de session pour un serveur Windows Server 2008 ou une station de travail Windows Vista
      * L’heure de la dernière tentative d’ouverture de session ayant échoué à un serveur Windows Server 2008 ou une station de travail Windows Vista
      * L’heure de la dernière ouverture de session réussie tentative sur un serveur Windows Server 2008 ou une station de travail Windows Vista
   * Stratégies de mot de passe affinées permettent de spécifier des stratégies de verrouillage de compte et mot de passe pour les utilisateurs et groupes de sécurité globaux dans un domaine. Pour plus d’informations, consultez [Guide pas à pas pour la Configuration de stratégie de verrouillage de compte et de mot de passe affinées](https://go.microsoft.com/fwlink/?LinkID=91477).
   * Bureaux virtuels personnels
      * Pour utiliser les fonctionnalités fournies par l’onglet Bureau virtuel personnel dans la boîte de dialogue Propriétés du compte utilisateur dans Active Directory Users and Computers, votre schéma AD DS doit être étendu pour Windows Server 2008 R2 (version de l’objet schéma = 47). Pour plus d’informations, consultez [déploiement de bureaux virtuels personnels à l’aide de RemoteApp et Guide pas à pas de connexion Bureau](https://go.microsoft.com/fwlink/?LinkId=183552).

## <a name="windows-server-2003"></a>Windows Server 2003

Prise en charge du système d’exploitation du contrôleur de domaine :

* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003

### <a name="windows-server-2003-forest-functional-level-features"></a>Fonctionnalités au niveau fonctionnelles de forêt Windows Server 2003

* Toutes les fonctionnalités par défaut AD DS et les fonctionnalités suivantes, sont disponibles :
   * Approbation de forêt
   * changement de nom de domaine ;
   * Réplication de valeurs liées
      - Réplication de valeurs liées rend possible de modifier l’appartenance au groupe pour stocker et répliquer des valeurs pour les membres individuels au lieu de répliquer l’ensemble de l’appartenance comme une seule unité. Stocker et répliquer les valeurs des membres individuels utilise moins de bande passante réseau et moins de cycles processeur lors de la réplication et vous empêche de perdre des mises à jour lorsque vous ajoutez ou supprimez plusieurs membres simultanément à différents contrôleurs de domaine.
   * La possibilité de déployer un contrôleur de domaine en lecture seule (RODC)
   * Algorithmes du vérificateur de cohérence des connaissances (KCC) et l’évolutivité améliorées
      - Le Générateur de topologie intersite (ISTG) utilise des algorithmes améliorés qui s’adaptent pour prendre en charge des forêts avec un plus grand nombre de sites que les services AD DS peut prendre en charge au niveau fonctionnel de forêt Windows 2000. L’algorithme d’élection ISTG améliorée est un mécanisme moins intrusive pour la sélection d’ISTG au niveau fonctionnel de forêt Windows 2000.
   * La possibilité de créer des instances de la classe auxiliaire dynamique appelée **dynamicObject** dans une partition d’annuaire de domaine
   * La possibilité de convertir un **inetOrgPerson** instance d’objet dans un **utilisateur** instance d’objet et pour terminer la conversion dans la direction opposée
   * La possibilité de créer des instances de nouveaux types de groupe pour prendre en charge d’autorisation en fonction du rôle. 
      - Ces types sont appelés groupes de base d’application et les groupes de requêtes LDAP.
   * désactivation et redéfinition des attributs et classes du schéma. Les attributs suivants peuvent être réutilisées : ldapDisplayName, schemaIdGuid, OID et mapiID.
   * Basés sur domaine espaces de noms DFS en cours d’exécution en Mode Windows Server 2008, qui inclut la prise en charge de l’énumération basée sur l’accès et une meilleure évolutivité. Pour plus d’informations, consultez [choisir un Type de Namespace](https://go.microsoft.com/fwlink/?LinkId=180400).

### <a name="windows-server-2003-domain-functional-level-features"></a>Fonctionnalités au niveau fonctionnelles du domaine Windows Server 2003

* Toutes les fonctionnalités par défaut les services AD DS, toutes les fonctionnalités qui sont disponibles pour le niveau fonctionnel de domaine en mode natif de Windows 2000 et les fonctionnalités suivantes sont disponibles :
   * L’outil de gestion de domaine, Netdom.exe, ce qui permet de renommer les contrôleurs de domaine
   * Met à jour de la date et heure d’ouverture de session
      * (L’attribut **lastLogonTimestamp** est mis à jour avec l’heure de la dernière ouverture de session de l’utilisateur ou ordinateur. Cet attribut est répliqué au sein du domaine.) ;
   * La possibilité de définir le **userPassword** d’attribut en tant que le mot de passe efficace sur **inetOrgPerson** et les objets utilisateur
   * La possibilité de rediriger les utilisateurs et ordinateurs conteneurs
      * Par défaut, deux conteneurs connus sont fournis pour héberger les comptes d’ordinateur et utilisateur, à savoir, cn = Computers,<domain root> et cn = Users,<domain root>. Cette fonctionnalité permet la définition d’un nouvel emplacement connu pour ces comptes.
   * La possibilité de gestionnaire d’autorisations de stocker ses stratégies d’autorisation dans AD DS
   * Délégation contrainte
      * La délégation contrainte rend possible pour les applications tirer parti de la délégation sécurisée des informations d’identification de l’utilisateur au moyen de l’authentification Kerberos.
      * Vous pouvez limiter la délégation aux services de destination spécifique.
   * Authentification sélective
      * Rend l’authentification sélective qu'il est possible que vous devez spécifier les utilisateurs et groupes d’une forêt approuvée autorisés à s’authentifier auprès des serveurs de ressources dans une forêt d’approbation.

## <a name="windows-2000"></a>Windows 2000

Prise en charge du système d’exploitation du contrôleur de domaine :

* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003
* Windows 2000

### <a name="windows-2000-native-forest-functional-level-features"></a>Fonctionnalités au niveau fonctionnelles de forêt natif Windows 2000

* Toutes les fonctionnalités par défaut AD DS sont disponibles.

### <a name="windows-2000-native-domain-functional-level-features"></a>Fonctionnalités au niveau fonctionnelles du domaine en mode natif Windows 2000

* Toutes les fonctionnalités par défaut AD DS et les fonctionnalités de répertoire suivantes sont notamment disponibles :
   * Groupes universels pour les groupes de distribution et de sécurité.
   * Imbrication de groupes
   * Conversion de groupe, ce qui permet la conversion entre les groupes de sécurité et de distribution
   * Historique de sécurité (SID) d’identificateur

## <a name="next-steps"></a>Étapes suivantes

* [Augmenter le niveau fonctionnel de domaine](https://technet.microsoft.com/library/cc753104.aspx)  
* [Augmenter le niveau fonctionnel de forêt](https://technet.microsoft.com/library/cc730985.aspx)
