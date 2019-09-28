---
ms.assetid: 35de490f-c506-4b73-840c-b239b72decc2
title: Configurer l’accès conditionnel local basé sur un périphérique
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/11/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a7646144b591fd7327f881cb54489201140e9287
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358150"
---
# <a name="configure-on-premises-conditional-access-using-registered-devices"></a>Configurer l’accès conditionnel local à l’aide des appareils inscrits


Le document suivant vous guide tout au long de l’installation et de la configuration de l’accès conditionnel local avec les appareils inscrits.

![Accès conditionnel](media/Using-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

## <a name="infrastructure-pre-requisites"></a>Conditions préalables de l’infrastructure
Les éléments suivants sont nécessaires pour que vous puissiez commencer à utiliser l’accès conditionnel local. 

|Condition requise|Description
|-----|-----
|Un abonnement Azure AD avec Azure AD Premium | Pour activer la réécriture de l’appareil pour l’accès conditionnel local, [une version d’évaluation gratuite est correcte](https://azure.microsoft.com/trial/get-started-active-directory/)  
|Abonnement Intune|uniquement requis pour l’intégration MDM pour les scénarios de conformité des appareils :[un essai gratuit est parfait](https://portal.office.com/Signup/Signup.aspx?OfferId=40BE278A-DFD1-470a-9EF7-9F2596EA7FF9&dl=INTUNE_A&ali=1#0)
|Azure AD Connect|QFE 2015 novembre ou version ultérieure.  Téléchargez la dernière version [ici](https://www.microsoft.com/en-us/download/details.aspx?id=47594).  
|Windows Server 2016|Build 10586 ou version ultérieure pour AD FS  
|Schéma de Active Directory Windows Server 2016|Le niveau de schéma 85 ou une version ultérieure est requis.
|Contrôleur de domaine Windows Server 2016|Cela est requis uniquement pour les déploiements de confiance de clé Hello entreprise.  Des informations supplémentaires sont disponibles [ici](https://aka.ms/whfbdocs).  
|Client Windows 10|La build 10586 ou une version plus récente, jointe au domaine ci-dessus, est requise pour la jonction de domaine Windows 10 et les scénarios de Microsoft Passport for Work uniquement  
|Azure AD compte d’utilisateur avec Azure AD Premium licence affectée|Pour inscrire l’appareil  


 
## <a name="upgrade-your-active-directory-schema"></a>Mettre à niveau votre schéma Active Directory
Pour utiliser l’accès conditionnel local avec des appareils inscrits, vous devez d’abord mettre à niveau votre schéma AD.  Les conditions suivantes doivent être remplies :
    - Le schéma doit être version 85 ou ultérieure
    - Cela est requis uniquement pour la forêt à laquelle AD FS est joint

> [!NOTE]
> Si vous avez installé Azure AD Connect avant d’effectuer la mise à niveau vers la version de schéma (niveau 85 ou version ultérieure) dans Windows Server 2016, vous devez réexécuter l’installation Azure AD Connect et actualiser le schéma AD local pour vous assurer que la règle de synchronisation est activée pour msDS-KeyCredentialLink est configuré.

### <a name="verify-your-schema-level"></a>Vérifier votre niveau de schéma
Pour vérifier votre niveau de schéma, procédez comme suit :

1.  Vous pouvez utiliser ADSIEdit ou LDP et vous connecter au contexte d’appellation de schéma.  
2.  À l’aide d’ADSIEdit, cliquez avec le bouton droit sur «CN = Schema, CN = Configuration, DC = <domain>, DC = <com>, puis sélectionnez Propriétés.  Le domaine relpace et les parties com avec les informations de votre forêt.
3.  Dans l’éditeur d’attributs, recherchez l’attribut objectVersion et il vous indiquera votre version.  

![Éditeur ADSI](media/Configure-Device-Based-Conditional-Access-on-Premises/adsiedit.png)  

Vous pouvez également utiliser l’applet de commande PowerShell suivante (remplacez l’objet par vos informations de contexte de nommage de schéma) :

``` powershell
Get-ADObject "cn=schema,cn=configuration,dc=domain,dc=local" -Property objectVersion
    
```

![PowerShell](media/Configure-Device-Based-Conditional-Access-on-Premises/pshell1.png) 

Pour plus d’informations sur la mise à niveau, voir [mettre à niveau les contrôleurs de domaine vers Windows Server 2016](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2016.md). 

## <a name="enable-azure-ad-device-registration"></a>Activer Azure AD Device Registration  
Pour configurer ce scénario, vous devez configurer la fonctionnalité d’inscription de l’appareil dans Azure AD.  

Pour ce faire, suivez les étapes de la section [configuration de Azure ad JOIN dans votre organisation](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-setup/)  

## <a name="setup-ad-fs"></a>AD FS d’installation  
1. Créez une [nouvelle batterie de serveurs AD FS 2016](https://technet.microsoft.com/library/dn486775.aspx).   
2.  Ou [migrer](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md) une batterie de serveurs vers AD FS 2016 à partir de AD FS 2012 R2  
4. Déployez [Azure ad Connect](https://azure.microsoft.com/documentation/articles/active-directory-aadconnectfed-whatis/) à l’aide du chemin d’accès personnalisé pour vous connecter AD FS à Azure ad.  

## <a name="configure-device-write-back-and-device-authentication"></a>Configurer l’écriture différée des appareils et l’authentification des appareils  
> [!NOTE]
> Si vous avez exécuté Azure AD Connect à l’aide des paramètres Express, les objets AD corrects ont été créés pour vous.  Toutefois, dans la plupart des scénarios de AD FS, Azure AD Connect a été exécuté avec des paramètres personnalisés pour configurer AD FS, les étapes ci-dessous sont donc nécessaires.  

### <a name="create-ad-objects-for-ad-fs-device-authentication"></a>Créer des objets AD pour l’authentification d'appareil AD FS  
Si votre batterie de serveurs AD FS n’est pas déjà configurée pour l’authentification d'appareil (vous pouvez le voir dans la console de gestion AD FS sous Service -> Inscription d’appareil), effectuez les étapes suivantes pour créer les objets et la configuration AD DS appropriés.  

![Inscription de l’appareil](media/Configure-Device-Based-Conditional-Access-on-Premises/device1.png)

>Remarque : Les commandes ci-dessous exigent les outils d’administration Active Directory. Par conséquent, si votre serveur de fédération n’est pas également un contrôleur de domaine, commencez par installer les outils en suivant l’étape 1 ci-dessous.  Dans le cas contraire, vous pouvez ignorer cette étape.  

1.  Exécutez l'Assistant **Ajouter des rôles et des fonctionnalités**, puis sélectionnez la fonctionnalité **Outils d'administration de serveur distant** -> **Outils d'administration de rôles** -> **Outils AD DS et AD LDS** -> Choisissez les options **Module Active Directory pour Windows PowerShell** et **Outils AD DS**.

![Inscription de l’appareil](media/Configure-Device-Based-Conditional-Access-on-Premises/device2.png)
  
2. Sur votre serveur AD FS principal, assurez-vous que vous êtes connecté en tant qu’AD DS utilisateur avec des privilèges Enterprise admin (EA) et ouvrez une invite PowerShell avec élévation de privilèges.  Ensuite, exécutez les commandes PowerShell suivantes :  
    
   `Import-module activedirectory`  
   `PS C:\> Initialize-ADDeviceRegistration -ServiceAccountName "<your service account>" ` 
3. Dans la fenêtre contextuelle, cliquez sur Oui.

>Remarque : Si votre service AD FS est configuré pour utiliser un compte service administré de groupe (GMSA), entrez le nom du compte au format « domaine\nom_compte$ ».

![Inscription de l’appareil](media/Configure-Device-Based-Conditional-Access-on-Premises/device3.png)  

Le PSH ci-dessus crée les objets suivants :  


- Conteneur RegisteredDevices sous la partition de domaine AD  
- Conteneur et objet Device Registration Service sous Configuration --> Services --> Configuration de l’inscription de l’appareil  
- Conteneur et objet DKM Device Registration Service sous Configuration --> Services --> Configuration de l’inscription de l’appareil  

![Inscription de l’appareil](media/Configure-Device-Based-Conditional-Access-on-Premises/device4.png)  

4. Une fois cette étape effectuée, vous verrez un message d'opération terminée correctement.

![Inscription de l’appareil](media/Configure-Device-Based-Conditional-Access-on-Premises/device5.png) 

###        <a name="create-service-connection-point-scp-in-ad"></a>Créer un point de connexion de service (SCP) dans AD  
Si vous envisagez d’utiliser une jonction de domaine Windows 10 (avec l’inscription automatique auprès d'Azure AD) comme décrit ici, exécutez les commandes suivantes pour créer un point de connexion de service dans AD DS  
1.  Ouvrez Windows PowerShell, puis exécutez la commande suivante :
    
    `PS C:>Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1" ` 

>Remarque : si nécessaire, copiez le fichier AdSyncPrep. psm1 à partir de votre serveur Azure AD Connect.  Ce fichier se trouve dans Program Files\Microsoft Azure Active Directory Connect\AdPrep

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


- objet de type msDS-DeviceRegistrationServiceContainer sur CN = Device Registration Services, CN = Device Registration configuration, CN = Services, CN = Configuration, DC = & ltdomain >  


- objet de type msDS-DeviceRegistrationService dans le conteneur ci-dessus  

### <a name="see-it-work"></a>Voir le fonctionnement  
Pour évaluer les nouvelles revendications et les stratégies, commencez par inscrire un appareil.  Par exemple, vous pouvez Azure AD joindre un ordinateur Windows 10 à l’aide de l’application paramètres sous système-> à propos de, ou vous pouvez configurer la jonction de domaine Windows 10 avec l’inscription automatique de l’appareil en suivant les étapes supplémentaires [ici](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/).  Pour plus d’informations sur la jonction des appareils Windows 10 mobile, consultez le document [ici](https://technet.microsoft.com/itpro/windows/manage/join-windows-10-mobile-to-azure-active-directory).  

Pour une évaluation plus facile, connectez-vous à AD FS à l’aide d’une application de test qui affiche une liste de revendications. Vous pourrez voir les nouvelles revendications, notamment isManaged, isCompliant et TrustType.  Si vous activez Microsoft Passport pour le travail, vous verrez également la revendication PRT.  
 

## <a name="configure-additional-scenarios"></a>Configurer des scénarios supplémentaires  
### <a name="automatic-registration-for-windows-10-domain-joined-computers"></a>Inscription automatique pour les ordinateurs Windows 10 joints à un domaine  
Pour activer l’inscription automatique des appareils pour les ordinateurs Windows 10 joints à un domaine, suivez les étapes 1 et 2 [ici](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/).   
Cela vous permet d’obtenir les informations suivantes :  

1. Vérifiez que votre point de connexion de service dans AD DS existe et qu’il dispose des autorisations appropriées (nous avons créé cet objet ci-dessus, mais cela n’a pas d’impact sur la double vérification).  
2. Vérifier que AD FS est correctement configuré  
3. Vérifiez que les points de terminaison corrects sont activés pour votre système AD FS et que les règles de revendication sont configurées   
4. Configurer les paramètres de stratégie de groupe requis pour l’inscription automatique des ordinateurs joints à un domaine   

### <a name="microsoft-passport-for-work"></a>Microsoft Passport for Work   
Pour plus d’informations sur l’activation de Windows 10 avec Microsoft Passport for Work, consultez [activer Microsoft Passport for Work dans votre organisation.](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)  

### <a name="automatic-mdm-enrollment"></a>Inscription automatique à MDM   
Pour activer l’inscription automatique MDM des appareils inscrits afin que vous puissiez utiliser la revendication isCompliant dans votre stratégie de contrôle d’accès, suivez les étapes [ci-dessous.](https://blogs.technet.microsoft.com/ad/2015/08/14/windows-10-azure-ad-and-microsoft-intune-automatic-mdm-enrollment-powered-by-the-cloud/)  

## <a name="troubleshooting"></a>Résolution des problèmes  
1.  Si vous recevez une erreur sur `Initialize-ADDeviceRegistration` qui se compose d’un objet déjà existant dans un état incorrect, par exemple « l’objet de service DRS a été trouvé sans tous les attributs requis », vous avez peut-être exécuté Azure AD Connect commandes PowerShell précédemment et avez un configuration partielle dans AD DS.  Essayez de supprimer manuellement les objets sous **CN = Device Registration configuration, CN = Services, CN = Configuration, DC = &lt;domain @ no__t-2** , puis réessayez.  
2.  Pour les clients Windows 10 joints à un domaine  
    1. Pour vérifier que l’authentification de l’appareil fonctionne, connectez-vous au client joint au domaine en tant que compte d’utilisateur de test. Pour déclencher rapidement l’approvisionnement, verrouillez et déverrouillez le bureau au moins une fois.   
    2. Instructions pour vérifier le lien des informations d’identification de la clé STK sur l’objet AD DS (la synchronisation doit-elle toujours s’exécuter deux fois ?)  
3.  Si vous recevez une erreur lors de la tentative d’inscription d’un ordinateur Windows que l’appareil a déjà été inscrit, mais que vous ne parvenez pas ou que vous avez déjà annulé l’inscription de l’appareil, vous pouvez avoir un fragment de la configuration de l’inscription d’appareil dans le registre.  Pour examiner et supprimer ce processus, procédez comme suit :  
    1. Sur l’ordinateur Windows, Ouvrez Regedit et accédez à **HKLM\Software\Microsoft\Enrollments**   
    2. Sous cette clé, il y aura de nombreuses sous-clés dans le format GUID.  Accédez à la sous-clé qui contient environ 17 valeurs et contient « EnrollmentType » de « 6 » [joint MDM] ou « 13 » (Azure AD jointe)  
    3. Modifier **EnrollmentType** à **0** 
    4. Réessayez l’inscription ou l’inscription de l’appareil  

### <a name="related-articles"></a>Articles connexes  
* [Sécurisation de l’accès à Office 365 et à d’autres applications connectées à Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access/)  
* [Stratégies d’appareil d’accès conditionnel pour les services Office 365](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access-device-policies/)  
* [Configuration de l’accès conditionnel local à l’aide de Azure Active Directory Device Registration](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup)  
* [Connecter des appareils joints à un domaine à des Azure AD pour les expériences Windows 10](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/)  
