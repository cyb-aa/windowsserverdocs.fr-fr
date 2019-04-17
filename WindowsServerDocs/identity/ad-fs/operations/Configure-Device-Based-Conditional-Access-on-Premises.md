---
ms.assetid: 35de490f-c506-4b73-840c-b239b72decc2
title: "Configurer l’accès conditionnel basé sur l’appareil local"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/11/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 47afd0c6963bd8b8b4dde82650cf807c1954b40b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="configure-on-premises-conditional-access-using-registered-devices"></a>Configurer On-Premises l’accès conditionnel à l’aide d’appareils inscrits

>S’applique à: Windows Server2016, Windows Server2012R2  

Le document suivant vous guide dans l’installation et configuration de l’accès conditionnel local avec les appareils inscrits.

![Accès conditionnel](media/Using-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

## <a name="infrastructure-pre-requisites"></a>Composants d’infrastructure requis
Les conditions-par suivantes sont nécessaires avant de commencer avec un accès conditionnel local. 

|Configuration requise|Description
|-----|-----
|Un abonnement Azure AD avec Azure AD Premium | Pour activer l’écriture de périphérique pour sur l’accès conditionnel local - [une version d’évaluation gratuite est correctement](https://azure.microsoft.com/en-us/trial/get-started-active-directory/)  
|Abonnement Intune|Requis uniquement pour l’intégration de GPM pour les scénarios de conformité d’appareil -[une version d’évaluation gratuite est correctement](https://portal.office.com/Signup/Signup.aspx?OfferId=40BE278A-DFD1-470a-9EF7-9F2596EA7FF9&dl=INTUNE_A&ali=1#0)
|Azure AD Connect|Novembre2015 QFE ou version ultérieure.  Obtenez la dernière version [ici](https://www.microsoft.com/en-us/download/details.aspx?id=47594).  
|Windows Server2016|Build10586 ou version ultérieure pour ADFS  
|Schéma ActiveDirectory de Windows Server2016|Niveau de schéma 85 ou une version ultérieure est requis.
|Contrôleur de domaine Windows Server2016|Cette fonctionnalité est uniquement requis pour les déploiements d’approbation de la clé Hello pour entreprises.  Vous trouverez des informations supplémentaires à [ici](https://aka.ms/whfbdocs).  
|Client Windows10|Build10586 ou version plus récente, joints au domaine ci-dessus est requis pour la jonction de domaine Windows10 et MicrosoftPassport pour les scénarios de travail uniquement  
|Compte d’utilisateur AD Azure avec les licences Azure AD Premium attribué|Pour inscrire l’appareil  


 
## <a name="upgrade-your-active-directory-schema"></a>Mettre à niveau votre schéma ActiveDirectory
Pour utiliser l’accès conditionnel local avec les appareils inscrits, vous devez tout d’abord mettre à niveau votre schéma ActiveDirectory.  Les conditions suivantes doivent être remplies:
    - Le schéma doit être 85 ou version ultérieure
    - Cette fonctionnalité est uniquement requis pour la forêt qui n’est joint à ADFS

> [!NOTE]
> Si vous avez installé Azure AD Connect avant la mise à niveau vers la version de schéma (niveau 85 ou supérieure) dans Windows Server2016, vous devrez exécuter à nouveau l’installation d’Azure AD Connect et actualiser les locaux sur schéma ActiveDirectory pour vous assurer de la règle de synchronisation pour l’attribut msDS-KeyCredentialLink est configuré.

### <a name="verify-your-schema-level"></a>Vérifier votre niveau de schéma
Pour vérifier votre niveau de schéma, procédez comme suit:

1.  Vous pouvez utiliser ADSIEdit ou LDP et se connecter pour le contexte de nommage de schéma.  
2.  À l’aide d’ADSIEdit, avec le bouton droit sur «CN = Schema, CN = Configuration, DC =<domain>, DC =<com> et sélectionnez Propriétés.  Domaine Relpace et les parties com avec vos informations de la forêt.
3.  Sous l’éditeur d’attribut localiser l’attribut objectVersion et elle vous indiquera, votre version.  

![Éditeur ADSI](media/Configure-Device-Based-Conditional-Access-on-Premises/adsiedit.png)  

Vous pouvez également utiliser l’applet de commande PowerShell suivante (remplacez l’objet avec vos informations de contexte de nommage de schéma):

``` powershell
Get-ADObject "cn=schema,cn=configuration,dc=domain,dc=local" -Property objectVersion
    
```

![PowerShell](media/Configure-Device-Based-Conditional-Access-on-Premises/pshell1.png) 

Pour plus d’informations sur la mise à niveau, voir [mise à niveau des contrôleurs de domaine vers Windows Server2016](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2016.md). 

## <a name="enable-azure-ad-device-registration"></a>Activer DRS Azure AD  
Pour configurer ce scénario, vous devez configurer la fonctionnalité d’inscription d’appareil dans Azure AD.  

Pour ce faire, suivez les étapes sous [paramétrage de votre organisation Azure AD Join](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-setup/)  

## <a name="setup-ad-fs"></a>Programme d’installation ADFS  
1. Créer l’un [nouvelle batterie ADFS2016](https://technet.microsoft.com/library/dn486775.aspx).   
2.  Ou [migrer](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md) une batterie de serveurs à ADFS2016depuis ADFS2012R2  
4. Déployer [Azure AD Connect](https://azure.microsoft.com/documentation/articles/active-directory-aadconnectfed-whatis/) utilisant le chemin d’accès personnalisé pour se connecter de ADFS à Azure AD.  

## <a name="configure-device-write-back-and-device-authentication"></a>Configurer l’écriture de périphérique précédent et l’authentification des appareils  
> [!NOTE]
> Si vous avez exécuté Azure AD Connect à l’aide de la configuration rapide, les objets ActiveDirectory appropriés ont été créés pour vous.  Toutefois, dans la plupart des scénarios ADFS, Azure AD Connect a été exécuté avec des paramètres personnalisés pour configurer ADFS, par conséquent, les étapes ci-dessous sont nécessaires.  

### <a name="create-ad-objects-for-ad-fs-device-authentication"></a>Créer des objets ActiveDirectory pour l’authentification des appareils ADFS  
Si votre batterie ADFS n’est pas déjà configuré pour l’authentification des appareils (vous pouvez le constater dans la console de gestion ADFS sous Service -> inscription de l’appareil), procédez comme suit pour créer les objets de domaine ActiveDirectory corrects et la configuration.  

![Inscription de l’appareil](media/Configure-Device-Based-Conditional-Access-on-Premises/device1.png)

>Remarque: La ci-dessous commandes requièrent des outils d’administration ActiveDirectory, par conséquent, si votre serveur de fédération n’est pas également un contrôleur de domaine, tout d’abord installer les outils à l’aide de l’étape1 ci-dessous.  Dans le cas contraire, vous pouvez ignorer l’étape1.  

1.  Exécuter le **Ajout de rôles et fonctionnalités** fonctionnalité Assistant et sélectionnez-le **Remote Server Administration Tools** -> **outils d’Administration de rôles** -> **outils ADDS et ADLDS** -> choisir les deux **module ActiveDirectory pour Windows PowerShell** et **outils ADDS**.

![Inscription de l’appareil](media/Configure-Device-Based-Conditional-Access-on-Premises/device2.png)
  
2.  Sur votre serveur principal ADFS, assurez-vous que vous êtes connecté en tant qu’utilisateur de domaine ActiveDirectory avec des privilèges d’administrateur d’entreprise (EA) et que vous ouvrez une invite powershell avec élévation de privilèges.  Ensuite, exécutez les commandes PowerShell suivantes:  
    
    `Import-module activedirectory`  
    `PS C:\> Initialize-ADDeviceRegistration -ServiceAccountName "<your service account>" ` 
3.  Dans la fenêtre contextuelle, appuyez sur Oui.

>Remarque: Si votre service ADFS est configuré pour utiliser un compte de service administré de groupe, entrez le nom du compte au format «forme domaine\nom_compte$»

![Inscription de l’appareil](media/Configure-Device-Based-Conditional-Access-on-Premises/device3.png)  

Le PSH ci-dessus crée les objets suivants:  


- Conteneur RegisteredDevices sous la partition de domaine ActiveDirectory  
- Conteneur de Service d’inscription de périphérique et d’objet sous Configuration--> services--> device Registration Configuration  
- Conteneur de gestion distribuée de clés Service d’inscription de périphérique et d’objet sous Configuration--> services--> device Registration Configuration  

![Inscription de l’appareil](media/Configure-Device-Based-Conditional-Access-on-Premises/device4.png)  

4.  Après cela, vous verrez un message de réussite.

![Inscription de l’appareil](media/Configure-Device-Based-Conditional-Access-on-Premises/device5.png) 

###        <a name="create-service-connection-point-scp-in-ad"></a>Créer un Point de connexion de Service (SCP) dans ActiveDirectory  
Si vous prévoyez d’utiliser Joindre un domaine Windows10 (avec l’inscription automatique à Azure AD) comme décrit ici, exécutez les commandes suivantes pour créer un point de connexion de service dans ADDS  
1.  Ouvrez Windows PowerShell et exécutez la commande suivante:
    
    `PS C:>Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1" ` 

>Remarque: si nécessaire, copiez le fichier AdSyncPrep.psm1 à partir de votre serveur Azure AD Connect.  Ce fichier se trouve dans le programme Files\MicrosoftAzure ActiveDirectory Connect\AdPrep

![Inscription de l’appareil](media/Configure-Device-Based-Conditional-Access-on-Premises/device6.png)   

2. Fournir vos informations d’identification d’administrateur général Azure AD  

    `PS C:>$aadAdminCred = Get-Credential`

![Inscription de l’appareil](media/Configure-Device-Based-Conditional-Access-on-Premises/device7.png) 

3.  Exécutez la commande PowerShell suivante 

    `PS C:>Initialize-ADSyncDomainJoinedComputerSync -AdConnectorAccount [AD connector account name] -AzureADCredentials $aadAdminCred ` 

Où [nom de compte d’ActiveDirectory connector] est le nom du compte que vous avez configuré dans Azure AD Connect lors de l’ajout de votre site répertoire ADDS.
  
Les commandes ci-dessus activent Windows10aux clients de trouver la bonne domaine Azure AD pour joindre en créant l’objet serviceConnectionpoint dans ADDS.  

### <a name="prepare-ad-for-device-write-back"></a>Préparer ActiveDirectory pour l’écriture de périphérique précédent   
Pour vérifier des conteneurs et objets de domaine ActiveDirectory sont dans l’état correct d’écriture à l’arrière de périphériques à partir d’Azure AD, procédez comme suit.

1.  Ouvrez Windows PowerShell et exécutez la commande suivante:  

    `PS C:>Initialize-ADSyncDeviceWriteBack -DomainName <AD DS domain name> -AdConnectorAccount [AD connector account name] ` 

Où [nom de compte d’ActiveDirectory connector] est le nom du compte que vous avez configuré dans Azure AD Connect lors de l’ajout de votre site répertoire ADDS au format forme domaine\nom_compte  

La commande ci-dessus crée les objets suivants pour l’écriture d’appareil à ADDS, s’ils n’existent pas déjà et permet d’accéder au nom du compte de connecteur ActiveDirectory spécifié  

- Conteneur RegisteredDevices dans la partition de domaine ActiveDirectory  
- Conteneur de Service d’inscription de périphérique et d’objet sous Configuration--> services--> device Registration Configuration  

### <a name="enable-device-write-back-in-azure-ad-connect"></a>Activer l’écriture de l’appareil dans Azure AD se connecter  
Si vous n’avez pas fait donc avant, activer l’écriture d’appareil dans Azure AD Connect en exécutant l’Assistant une deuxième fois et en sélectionnant **«Personnaliser les Options de synchronisation»**, puis cochez la case pour l’écriture de périphérique précédent et en sélectionnant la forêt dans laquelle vous avez exécuté les applets de commande ci-dessus  

### <a name="configure-device-authentication-in-ad-fs"></a>Configurer l’authentification des appareils dans ADFS  
À l’aide d’une fenêtre de commande PowerShell avec élévation de privilèges, configurer la stratégie de ADFS en exécutant la commande suivante  

`PS C:>Set-AdfsGlobalAuthenticationPolicy -DeviceAuthenticationEnabled $true -DeviceAuthenticationMethod All` 

### <a name="check-your-configuration"></a>Vérifiez votre configuration.  
Pour référence, voici une liste complète des périphériques, des conteneurs et autorisations requises pour l’authentification et le périphérique en écriture différée pour utiliser les services ADDS
 


- Objet de type ms-DS-DeviceContainer à CN = RegisteredDevices, DC =&lt;domaine&gt;        
    - Accès en lecture au compte de service ADFS   
    - Accès en lecture/écriture pour la compte du connecteur ActiveDirectory de synchronisation Azure AD Connect</br></br>

- Le conteneur CN = Device Registration Configuration, CN = Services, CN = Configuration, DC =&lt;domaine&gt;  
- Conteneur Device Registration Service de gestion distribuée de clés sous le conteneur ci-dessus

![Inscription de l’appareil](media/Configure-Device-Based-Conditional-Access-on-Premises/device8.png) 
 


- Objet de serviceConnectionpoint type à CN =&lt;guid&gt;, CN = d’inscription de l’appareil

- Configuration, CN = Services, CN = Configuration, DC =&lt;domaine&gt;  
 - Accès en lecture/écriture pour le nom de compte de connecteur ActiveDirectory spécifié sur le nouvel objet</br></br> 


- Objet de type msDS-DeviceRegistrationServiceContainer à CN = Services d’inscription de périphérique, CN = Device Registration Configuration, CN = Services, CN = Configuration, DC = et ltdomain >  


- Objet de type msDS-DeviceRegistrationService dans le conteneur ci-dessus  

### <a name="see-it-work"></a>Voir comment il fonctionne  
Pour évaluer les nouvelles revendications et les stratégies, d’abord inscrire un appareil.  Par exemple, vous pouvez un ordinateur Windows10 à l’aide de l’application paramètres sous système -> sur Azure AD Join, ou vous pouvez définir la jonction de domaine Windows10 avec l’inscription automatique de l’appareil en suivant les étapes supplémentaires [ici](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/).  Pour plus d’informations sur la jonction de Windows10, les périphériques mobiles, consultez le document [ici](https://technet.microsoft.com/itpro/windows/manage/join-windows-10-mobile-to-azure-active-directory).  

Pour une évaluation plus simple, connectez-vous à ADFS à l’aide d’une application de test qui affiche une liste de revendications. Vous ne pourrez pas voir les nouvelles revendications notamment isManaged, isCompliant et trusttype.  Si vous activez MicrosoftPassport pour le travail, vous verrez également le prt de revendication.  
 

## <a name="configure-additional-scenarios"></a>Configurer des scénarios supplémentaires  
### <a name="automatic-registration-for-windows-10-domain-joined-computers"></a>Ordinateurs de l’inscription pour Windows10joints au domaine automatique  
Pour activer l’inscription automatique de l’appareil pour Windows10 domaine joint ordinateurs, suivez les étapes1 et 2 [ici](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/).   
Cela vous permettra d’effectuer les opérations suivantes:  

1. Assurez-vous que votre point de connexion de service dans ADDS existe et qu’il dispose des autorisations appropriées (nous avons créé cet objet ci-dessus, mais il ne pas conseillé de vérifier).  
2. Assurez-vous de QU'ADFS est correctement configuré.  
3. Assurez-vous que votre système ADFS a les points de terminaison corrects est activés et configurés des règles de revendication   
4. Configurer les paramètres de stratégie de groupe requis pour l’inscription automatique de l’appareil d’ordinateurs appartenant à un domaine   

### <a name="microsoft-passport-for-work"></a>MicrosoftPassport for Work   
Pour plus d’informations sur l’activation de Windows10 avec MicrosoftPassport for Work, consultez [activer MicrosoftPassport for Work dans votre organisation.](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-passport-deployment/)  

### <a name="automatic-mdm-enrollment"></a>Inscription GPM automatique   
Pour activer l’inscription automatique des appareils inscrits afin que vous pouvez utiliser la revendication isCompliant dans votre stratégie de contrôle d’accès, suivez les étapes [ici.](https://blogs.technet.microsoft.com/ad/2015/08/14/windows-10-azure-ad-and-microsoft-intune-automatic-mdm-enrollment-powered-by-the-cloud/)  

## <a name="troubleshooting"></a>Résolution des problèmes  
1.  Si vous obtenez une erreur `Initialize-ADDeviceRegistration`qui se plaint sur un objet déjà existant dans un état incorrect, tel que «l’objet du drs service a été trouvée sans tous les attributs requis», vous pouvez avoir exécuté Azure AD Connect powershell commandes précédemment et ont une configuration partielle dans ADDS.  Essayez de supprimer manuellement les objets sous **CN = Device Registration Configuration, CN = Services, CN = Configuration, DC =&lt;domaine&gt;** et essayer à nouveau.  
2.  Pour Windows10 domaine joint les clients  
    1. Pour vérifier que l’authentification d’appareil fonctionne, connectez-vous au domaine joint à un client en tant qu’un compte d’utilisateur de test. Pour déclencher rapidement d’approvisionnement, verrouiller et déverrouiller le bureau au moins une fois.   
    2. Instructions pour vérifier les informations d’identification de la clé stk lier sur l’objet ADDS (synchronisation toujours a-t-il exécuter deux fois)  
3.  Si vous obtenez une erreur lors de la tentative d’inscription d’un ordinateur Windows que l’appareil a été déjà inscrit, mais vous ne parvenez pas ou avez désinscrit déjà de l’appareil, peut avoir un fragment de configuration de l’inscription de périphérique dans le Registre.  Pour examiner et supprimer cela, procédez comme suit:  
    1. Sur l’ordinateur Windows, ouvrez Regedit et accédez à **HKLM\Software\Microsoft\Enrollments**   
    2. Sous cette clé, il y aura plusieurs sous-clés sous la forme GUID.  Naviguez jusqu'à la sous-clé qui possède des valeurs ~ 17dans celui-ci et «EnrollmentType» de «6» [GPM joint] ou «13» (joints à Azure AD)  
    3. Modifier **EnrollmentType** à **0** 
    4. Essayez à nouveau de l’inscription de périphérique ou l’enregistrement  

### <a name="related-articles"></a>Articles connexes  
* [Sécurisation de l’accès à Office 365 et d’autres applications connectées à Azure ActiveDirectory](https://azure.microsoft.com/en-us/documentation/articles/active-directory-conditional-access/)  
* [Stratégies d’accès conditionnel appareil pour les services Office 365](https://azure.microsoft.com/en-us/documentation/articles/active-directory-conditional-access-device-policies/)  
* [Configuration d’accès conditionnel local à l’aide d’inscription de l’appareil Azure ActiveDirectory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-device-registration-on-premises-setup)  
* [Connecter des appareils joints au domaine à Azure AD pour des expériences Windows10](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-devices-group-policy/)  
