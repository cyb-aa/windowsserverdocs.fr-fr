---
ms.assetid: 35de490f-c506-4b73-840c-b239b72decc2
title: Configurer l’accès conditionnel local basé sur un périphérique
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/11/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: bcb6c415aae33b9742d7a7080ec169ca947098b9
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444998"
---
# <a name="configure-on-premises-conditional-access-using-registered-devices"></a>Configurer On-Premises l’accès conditionnel à l’aide d’appareils inscrits


Le document suivant vous guide dans l’installation et configuration de l’accès conditionnel en local avec les appareils inscrits.

![Accès conditionnel](media/Using-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

## <a name="infrastructure-pre-requisites"></a>Prérequis de l’infrastructure
Les préalables suivantes sont nécessaires avant de commencer avec un accès conditionnel en local. 

|Condition requise|Description
|-----|-----
|Un abonnement Azure AD avec Azure AD Premium | Pour activer l’écriture de périphérique pour revenir sur l’accès conditionnel local - [convient d’une version d’évaluation gratuite](https://azure.microsoft.com/trial/get-started-active-directory/)  
|Abonnement Intune|requis uniquement pour l’intégration de gestion des appareils mobiles pour les scénarios de conformité d’appareil -[convient d’une version d’évaluation gratuite](https://portal.office.com/Signup/Signup.aspx?OfferId=40BE278A-DFD1-470a-9EF7-9F2596EA7FF9&dl=INTUNE_A&ali=1#0)
|Azure AD Connect|Correctif QFE de novembre 2015 ou version ultérieure.  Obtenir la dernière version [ici](https://www.microsoft.com/en-us/download/details.aspx?id=47594).  
|Windows Server 2016|Build 10586 ou une version ultérieure pour AD FS  
|Schéma Active Directory Windows Server 2016|Niveau de schéma 85 ou une version ultérieure est requis.
|Contrôleur de domaine Windows Server 2016|Cela est uniquement requis pour les déploiements de confiance clé Hello For Business.  Vous trouverez des informations supplémentaires à [ici](https://aka.ms/whfbdocs).  
|Client Windows 10|Build 10586 ou plus récente, joints au domaine ci-dessus est requise pour la jonction de domaine Windows 10 et Microsoft Passport pour les scénarios de travail uniquement  
|Compte d’utilisateur Azure AD avec licence Azure AD Premium|Pour inscrire l’appareil  


 
## <a name="upgrade-your-active-directory-schema"></a>Mettre à niveau votre schéma Active Directory
Pour utiliser l’accès conditionnel en local avec les appareils inscrits, vous devez tout d’abord mettre à niveau votre schéma Active Directory.  Les conditions suivantes doivent être remplies :
    - Le schéma doit être 85 ou version ultérieure
    - Cela est uniquement nécessaire pour la forêt AD FS est jointe à

> [!NOTE]
> Si vous avez installé Azure AD Connect avant la mise à niveau vers la version de schéma (niveau 85 ou supérieur) dans Windows Server 2016, vous devez exécuter à nouveau l’installation d’Azure AD Connect et actualiser le site sur schéma Active Directory pour vous assurer de la règle de synchronisation pour msDS-KeyCredentialLink est configuré.

### <a name="verify-your-schema-level"></a>Vérifiez votre niveau de schéma
Pour vérifier votre niveau de schéma, procédez comme suit :

1.  Vous pouvez utiliser ADSIEdit ou LDP et vous connecter pour le contexte de nommage de schéma.  
2.  L’utilisation d’ADSIEdit, avec le bouton droit sur « CN = Schema, CN = Configuration, DC =<domain>, DC =<com> , puis sélectionnez Propriétés.  Relpace domaine et les parties de com avec les informations de votre forêt.
3.  Sous l’éditeur d’attributs localiser l’attribut objectVersion et il vous indiquera, votre version.  

![Éditeur ADSI](media/Configure-Device-Based-Conditional-Access-on-Premises/adsiedit.png)  

Vous pouvez également utiliser l’applet de commande PowerShell suivante (à remplacer l’objet avec vos informations de contexte de nommage de schéma) :

``` powershell
Get-ADObject "cn=schema,cn=configuration,dc=domain,dc=local" -Property objectVersion
    
```

![PowerShell](media/Configure-Device-Based-Conditional-Access-on-Premises/pshell1.png) 

Pour plus d’informations sur la mise à niveau, consultez [mise à niveau des contrôleurs de domaine vers Windows Server 2016](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2016.md). 

## <a name="enable-azure-ad-device-registration"></a>Activer Azure Active Directory Device Registration  
Pour configurer ce scénario, vous devez configurer la fonctionnalité d’inscription de l’appareil dans Azure AD.  

Pour ce faire, suivez les étapes sous [configuration d’Azure AD Join dans votre organisation](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-setup/)  

## <a name="setup-ad-fs"></a>Programme d’installation AD FS  
1. Créer l’un [nouvelle batterie de serveurs AD FS 2016](https://technet.microsoft.com/library/dn486775.aspx).   
2.  Ou [migrer](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md) une batterie de serveurs AD FS 2016 à partir d’AD FS 2012 R2  
4. Déployer [Azure AD Connect](https://azure.microsoft.com/documentation/articles/active-directory-aadconnectfed-whatis/) à l’aide du chemin d’accès personnalisé à connecter des services AD FS à Azure AD.  

## <a name="configure-device-write-back-and-device-authentication"></a>Configurer l’appareil écriture en différé et l’authentification des appareils  
> [!NOTE]
> Si vous avez exécuté Azure AD Connect à l’aide de paramètres Express, les objets AD corrects créés pour vous.  Toutefois, dans la plupart des scénarios AD FS, Azure AD Connect a été exécuté avec des paramètres personnalisés pour configurer AD FS, par conséquent, les étapes ci-dessous sont nécessaires.  

### <a name="create-ad-objects-for-ad-fs-device-authentication"></a>Créer des objets AD pour l’authentification d'appareil AD FS  
Si votre batterie de serveurs AD FS n’est pas déjà configurée pour l’authentification d'appareil (vous pouvez le voir dans la console de gestion AD FS sous Service -> Inscription d’appareil), effectuez les étapes suivantes pour créer les objets et la configuration AD DS appropriés.  

![Inscription de l’appareil](media/Configure-Device-Based-Conditional-Access-on-Premises/device1.png)

>Remarque: Les commandes ci-dessous exigent les outils d’administration Active Directory. Par conséquent, si votre serveur de fédération n’est pas également un contrôleur de domaine, commencez par installer les outils en suivant l’étape 1 ci-dessous.  Dans le cas contraire, vous pouvez ignorer cette étape.  

1.  Exécutez l'Assistant **Ajouter des rôles et des fonctionnalités**, puis sélectionnez la fonctionnalité **Outils d'administration de serveur distant** -> **Outils d'administration de rôles** -> **Outils AD DS et AD LDS** -> Choisissez les options **Module Active Directory pour Windows PowerShell** et **Outils AD DS**.

![Inscription de l’appareil](media/Configure-Device-Based-Conditional-Access-on-Premises/device2.png)
  
2. Sur votre serveur principal AD FS, vérifiez vous êtes connecté en tant qu’utilisateur de domaine Active Directory avec des privilèges d’administrateur d’entreprise (EA) et ouvrez une invite powershell avec élévation de privilèges.  Ensuite, exécutez les commandes PowerShell suivantes :  
    
   `Import-module activedirectory`  
   `PS C:\> Initialize-ADDeviceRegistration -ServiceAccountName "<your service account>" ` 
3. Dans la fenêtre contextuelle, appuyez sur Oui.

>Remarque: Si votre service AD FS est configuré pour utiliser un compte service administré de groupe (GMSA), entrez le nom du compte au format « domaine\nom_compte$ ».

![Inscription de l’appareil](media/Configure-Device-Based-Conditional-Access-on-Premises/device3.png)  

Le PSH ci-dessus crée les objets suivants :  


- Conteneur RegisteredDevices sous la partition de domaine AD  
- Conteneur et objet Device Registration Service sous Configuration --> Services --> Configuration de l’inscription de l’appareil  
- Conteneur et objet DKM Device Registration Service sous Configuration --> Services --> Configuration de l’inscription de l’appareil  

![Inscription de l’appareil](media/Configure-Device-Based-Conditional-Access-on-Premises/device4.png)  

4. Une fois cette étape effectuée, vous verrez un message d'opération terminée correctement.

![Inscription de l’appareil](media/Configure-Device-Based-Conditional-Access-on-Premises/device5.png) 

###        <a name="create-service-connection-point-scp-in-ad"></a>Créer un Point de connexion de Service (SCP) dans Active Directory  
Si vous envisagez d’utiliser une jonction de domaine Windows 10 (avec l’inscription automatique auprès d'Azure AD) comme décrit ici, exécutez les commandes suivantes pour créer un point de connexion de service dans AD DS  
1.  Ouvrez Windows PowerShell, puis exécutez la commande suivante :
    
    `PS C:>Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1" ` 

>Remarque : si nécessaire, copiez le fichier AdSyncPrep.psm1 à partir de votre serveur Azure AD Connect.  Ce fichier se trouve dans Program Files\Microsoft Azure Active Directory Connect\AdPrep

![Inscription de l’appareil](media/Configure-Device-Based-Conditional-Access-on-Premises/device6.png)   

2. Fournissez vos informations d’identification d’administrateur Azure AD  

    `PS C:>$aadAdminCred = Get-Credential`

![Inscription de l’appareil](media/Configure-Device-Based-Conditional-Access-on-Premises/device7.png) 

3. Exécutez la commande PowerShell suivante 

   `PS C:>Initialize-ADSyncDomainJoinedComputerSync -AdConnectorAccount [AD connector account name] -AzureADCredentials $aadAdminCred ` 

où [AD connector account name] est le nom du compte que vous avez configuré dans Azure AD Connect lors de l’ajout de votre annuaire AD DS local.
  
Les commandes ci-dessus permettent aux clients Windows 10 de trouver le domaine Azure AD à joindre approprié en créant l’objet serviceConnectionpoint dans AD DS.  

### <a name="prepare-ad-for-device-write-back"></a>Préparer Active Directory pour l’écriture différée des appareils   
Pour vérifier que les objets et conteneurs AD DS sont dans l’état correct pour l'écriture différée d'appareils à partir d’Azure AD, procédez comme suit.

1.  Ouvrez Windows PowerShell, puis exécutez la commande suivante :  

    `PS C:>Initialize-ADSyncDeviceWriteBack -DomainName <AD DS domain name> -AdConnectorAccount [AD connector account name] ` 

où [AD connector account name] est le nom du compte que vous avez configuré dans Azure AD Connect lors de l’ajout de votre annuaire AD DS local au format in domaine\nom_compte.  

La commande ci-dessus crée les objets suivants pour l’écriture différée d’appareil dans AD DS, s’ils n’existent pas déjà, et permet d’accéder au nom de compte de connecteur Active Directory spécifié  

- Conteneur RegisteredDevices dans la partition de domaine AD  
- Conteneur et objet Device Registration Service sous Configuration --> Services --> Configuration de l’inscription de l’appareil  

### <a name="enable-device-write-back-in-azure-ad-connect"></a>Activer l’écriture différée d’appareils dans Azure AD Connect  
Si ce n’est pas déjà fait, activez l’écriture différée d’appareils dans Azure AD Connect en exécutant l’Assistant une deuxième fois, puis en sélectionnant **« Personnalisation des options de synchronisation »** . Cochez ensuite la case correspondant à l’écriture différée d'appareils, puis sélectionnez la forêt dans laquelle vous avez exécuté les applets de commande ci-dessus  

### <a name="configure-device-authentication-in-ad-fs"></a>Configurer l’authentification des appareils dans AD FS  
À l’aide d’une fenêtre de commande PowerShell avec élévation de privilèges, configurez la stratégie AD FS en exécutant la commande suivante  

`PS C:>Set-AdfsGlobalAuthenticationPolicy -DeviceAuthenticationEnabled $true -DeviceAuthenticationMethod All` 

### <a name="check-your-configuration"></a>Vérifier votre configuration  
À titre de référence, voici une liste complète des appareils, conteneurs et autorisations AD DS exigés pour que l’authentification et l'écriture différée des appareils fonctionnent
 


- objet de type ms-DS-DeviceContainer at CN=RegisteredDevices,DC=&lt;domaine&gt;        
    - accès en lecture au compte de service AD FS   
    - accès en lecture/écriture au compte de connecteur AD de synchronisation Azure AD Connect</br></br>

- Conteneur CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=&lt;domaine&gt;  
- Conteneur DKM Device Registration Service sous le conteneur ci-dessus

![Inscription de l’appareil](media/Configure-Device-Based-Conditional-Access-on-Premises/device8.png) 
 


- objet de type serviceConnectionpoint at CN=&lt;guid&gt;, CN=Device Registration

- Configuration,CN=Services,CN=Configuration,DC=&lt;domaine&gt;  
  - accès en lecture/écriture au nom de compte de connecteur AD spécifié sur le nouvel objet</br></br> 


- objet de type msDS-DeviceRegistrationServiceContainer à CN = Services d’inscription de l’appareil, CN = Device Registration Configuration, CN = Services, CN = Configuration, DC = & ltdomain >  


- objet de type msDS-DeviceRegistrationService dans le conteneur ci-dessus  

### <a name="see-it-work"></a>Voir comment cela fonctionne  
Pour évaluer les nouvelles revendications et les stratégies, tout d’abord inscrire un appareil.  Par exemple, vous pouvez un ordinateur Windows 10 à l’aide de l’application paramètres sous système -> à propos de Azure AD Join, ou vous pouvez configurer la jonction de domaine Windows 10 avec l’inscription automatique des appareils en suivant les étapes supplémentaires [ici](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/).  Pour plus d’informations sur la jonction de Windows 10 appareils mobiles, consultez le document [ici](https://technet.microsoft.com/itpro/windows/manage/join-windows-10-mobile-to-azure-active-directory).  

Pour l’évaluation de la plus simple, connectez-vous à AD FS à l’aide d’une application de test qui affiche une liste de revendications. Vous serez en mesure de voir les nouvelles revendications notamment isManaged isCompliant et la propriété trusttype.  Si vous activez Microsoft Passport pour le travail, vous verrez également le prt de revendication.  
 

## <a name="configure-additional-scenarios"></a>Configurer des scénarios supplémentaires  
### <a name="automatic-registration-for-windows-10-domain-joined-computers"></a>Ordinateurs de l’inscription pour Windows 10 joints à un domaine automatique  
Pour activer l’inscription automatique pour le domaine de Windows 10 ordinateurs joints à un, suivez les étapes 1 et 2 [ici](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/).   
Cela vous aidera à effectuer les opérations suivantes :  

1. Assurez-vous que votre point de connexion de service dans AD DS existe et qu’il dispose des autorisations appropriées (nous avons créé cet objet ci-dessus, mais il ne nuire pas à vérifier).  
2. Vérifier QU'AD FS est correctement configuré.  
3. Assurez-vous que votre système AD FS a les points de terminaison corrects est activés et configurés des règles de revendication   
4. Configurer les paramètres de stratégie de groupe requis pour l’inscription automatique des ordinateurs joints au domaine   

### <a name="microsoft-passport-for-work"></a>Microsoft Passport for Work   
Pour plus d’informations sur l’activation de Windows 10 avec Microsoft Passport for Work, consultez [activer Microsoft Passport for Work dans votre organisation.](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)  

### <a name="automatic-mdm-enrollment"></a>Inscription MDM automatique   
Pour activer l’inscription MDM automatique des appareils enregistrés afin que vous puissiez utiliser la revendication isCompliant dans votre stratégie de contrôle d’accès, suivez les étapes [ici.](https://blogs.technet.microsoft.com/ad/2015/08/14/windows-10-azure-ad-and-microsoft-intune-automatic-mdm-enrollment-powered-by-the-cloud/)  

## <a name="troubleshooting"></a>Résolution des problèmes  
1.  Si vous obtenez une erreur sur `Initialize-ADDeviceRegistration` qui se plaint sur un objet déjà existant dans un état incorrect, tel que « l’objet de service de drs a été trouvé sans tous les attributs requis », vous pouvez avoir exécuté les commandes de powershell Azure AD Connect précédemment et a une configuration partielle dans AD DS.  Essayez de supprimer manuellement les objets sous **CN = Device Registration Configuration, CN = Services, CN = Configuration, DC =&lt;domaine&gt;**  , puis réessayez.  
2.  Domaine de Windows 10 clients appartenant à un  
    1. Pour vérifier que l’authentification d’appareil fonctionne, connectez-vous au client joint à un domaine sous un compte d’utilisateur de test. Pour déclencher rapidement le provisionnement, verrouiller et déverrouiller le bureau au moins une fois.   
    2. Instructions pour vérifier les informations d’identification de clé stk lier sur l’objet AD DS (synchronisation toujours doit-il exécuter deux fois ?)  
3.  Si vous obtenez une erreur lors de la tentative d’inscription d’un ordinateur Windows que l’appareil a été déjà inscrit, mais vous ne parvenez pas, ou avez désinscrit déjà de l’appareil, peut avoir un fragment de la configuration de l’inscription de périphérique dans le Registre.  Pour examiner et supprimer cela, utilisez les étapes suivantes :  
    1. Sur l’ordinateur Windows, ouvrez Regedit et accédez à **HKLM\Software\Microsoft\Enrollments**   
    2. Sous cette clé, il y aura plusieurs sous-clés sous la forme GUID.  Accédez à la sous-clé qui comporte les valeurs de ~ 17 et a « EnrollmentType » sur « 6 » [MDM joint] ou « 13 » (joints à Azure AD)  
    3. Modifier **EnrollmentType** à **0** 
    4. Réessayez l’inscription de l’appareil ou l’enregistrement  

### <a name="related-articles"></a>Articles connexes  
* [Sécurisation de l’accès à Office 365 et d’autres applications connectées à Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access/)  
* [Stratégies de périphérique d’accès conditionnel pour les services Office 365](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access-device-policies/)  
* [Configuration d’accès conditionnel en local à l’aide d’Azure Active Directory Device Registration](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup)  
* [Connecter des appareils joints au domaine à Azure AD pour des expériences Windows 10](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/)  
