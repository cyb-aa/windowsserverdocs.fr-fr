---
ms.assetid: f0cbdd78-f5ae-47ff-b5d3-96faf4940f4a
title: Configuration des ID de connexion alternatif
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 11/14/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 12c47f98af24331b25355178370cc4cd28c0aa10
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358054"
---
# <a name="configuring-alternate-login-id"></a>Configuration des ID de connexion alternatif


## <a name="what-is-alternate-login-id"></a>Qu’est-ce que l’ID de connexion de substitution ?
Dans la plupart des scénarios, les utilisateurs utilisent leur UPN (nom d’utilisateur principal) pour se connecter à leurs comptes. Toutefois, dans certains environnements en raison des stratégies d’entreprise ou des dépendances d’application métier locales, les utilisateurs peuvent utiliser une autre forme de connexion. 

>[!NOTE]
>Les meilleures pratiques recommandées par Microsoft doivent correspondre au nom UPN et à l’adresse SMTP principale. Cet article traite du petit pourcentage de clients qui ne peuvent pas mettre à l’échelle les UPN pour les faire correspondre.

Par exemple, ils peuvent utiliser leur ID d’adresse de messagerie pour la connexion et peuvent être différents de leur UPN. Ceci est particulièrement fréquent dans les scénarios où leur UPN n’est pas routable. Prenons l’exemple d’un utilisateur Jane jdoe@contoso.local Doe avec un jdoe@contoso.comUPN et une adresse de messagerie. Jane n’a peut-être pas la même connaissance de l’UPN car il a toujours utilisé son ID de messagerie pour la connexion. L’utilisation de toute autre méthode de connexion au lieu de l’UPN constitue un autre ID. Pour plus d’informations sur la création de l’UPN, consultez [Azure ad le remplissage userPrincipalName](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-userprincipalname).

Services ADFS (AD FS) permet aux applications fédérées d’utiliser AD FS pour se connecter à l’aide d’un autre ID. Cela permet aux administrateurs de spécifier une alternative à l’UPN par défaut à utiliser pour la connexion. AD FS prend déjà en charge l’utilisation d’une forme d’identificateur d’utilisateur acceptée par Active Directory Domain Services (AD DS). Lorsqu’il est configuré pour un autre ID, AD FS permet aux utilisateurs de se connecter à l’aide de la valeur ID de remplacement configurée, par exemple, l’ID de messagerie. L’utilisation de l’ID secondaire vous permet d’adopter des fournisseurs SaaS, comme Office 365 sans modifier vos UPN locaux. Elle vous permet également de prendre en charge des applications de service métier avec des identités approvisionnées par les consommateurs.

## <a name="alternate-id-in-azure-ad"></a>Autre ID dans Azure AD
Une organisation peut avoir à utiliser un autre ID dans les scénarios suivants :
1. Le nom de domaine local n’est pas routable, par exemple Contoso. local et, par conséquent, le nom d’utilisateur principal par défaut n’est pasjdoe@contoso.localroutable (). Impossible de modifier l’UPN existant en raison de dépendances d’application locales ou de stratégies d’entreprise. Azure AD et Office 365 requièrent que tous les suffixes de domaine associés à Azure AD Directory soient entièrement routés sur Internet. 
2. L’UPN local est différent de l’adresse de messagerie de l’utilisateur et pour se connecter à Office 365, les utilisateurs utilisent l’adresse de messagerie et l’UPN ne peuvent pas être utilisés en raison de contraintes organisationnelles.
   Dans les scénarios mentionnés ci-dessus, l’ID alternatif avec AD FS permet aux utilisateurs de se connecter à Azure AD sans modifier vos UPN locaux. 

## <a name="end-user-experience-with-alternate-login-id"></a>Expérience de l’utilisateur final avec un ID de connexion de substitution
L’expérience de l’utilisateur final varie en fonction de la méthode d’authentification utilisée avec l’ID de connexion de substitution.  Il existe actuellement trois façons différentes d’utiliser l’ID de connexion de substitution.  Celles-ci sont les suivantes :

- **Authentification normale (héritée)** : utilise le protocole d’authentification de base.
- **L’authentification moderne** : permet aux applications de se connecter à Bibliothèque D’AUTHENTIFICATION Active Directory (Adal). Cela permet des fonctionnalités de connexion telles que les Multi-Factor Authentication (MFA), les fournisseurs d’identité tiers basés sur SAML avec les applications clientes Office, l’authentification par carte à puce et par certificat.
- **Authentification moderne hybride** : fournit tous les avantages de l’authentification moderne et offre aux utilisateurs la possibilité d’accéder aux applications locales à l’aide de jetons d’autorisation obtenus à partir du Cloud.

>[!NOTE]
> Pour une expérience optimale, Microsoft recommande fortement l’authentification hybride moderne.



## <a name="configure-alternate-logon-id"></a>Configurer un ID de connexion de substitution
À l’aide de Azure AD Connect nous vous recommandons d’utiliser Azure AD Connect pour configurer un autre ID de connexion pour votre environnement.

- Pour obtenir une nouvelle configuration de Azure AD Connect, consultez se connecter à Azure AD pour obtenir des instructions détaillées sur la configuration d’un ID de substitution et d’AD FS batterie de serveurs.
- Pour les installations de Azure AD Connect existantes, consultez Modification de la méthode de connexion utilisateur pour obtenir des instructions sur la modification de la méthode de connexion en AD FS

Lorsque Azure AD Connect contient des détails sur AD FS environnement, il vérifie automatiquement la présence de la base de connaissances appropriée sur votre AD FS et configure AD FS pour un autre ID, y compris toutes les règles de revendication appropriées pour Azure AD approbation de Fédération. Aucune étape supplémentaire n’est requise en dehors de l’Assistant pour configurer un autre ID.

>[!NOTE]
> Microsoft recommande d’utiliser Azure AD Connect pour configurer un ID de connexion de substitution.

### <a name="manually-configure-alternate-id"></a>Configurer manuellement un autre ID
Pour configurer un ID de connexion de substitution, vous devez effectuer les tâches suivantes : Configurer vos approbations de fournisseur de revendications AD FS pour activer l’ID de connexion de substitution

1.  Si vous avez un serveur 2012 R2, assurez-vous que KB2919355 est installé sur tous les serveurs AD FS. Vous pouvez l’utiliser via Windows Update Services ou le télécharger directement. 

2.  Mettez à jour la configuration de AD FS en exécutant l’applet de commande PowerShell suivante sur l’un des serveurs de Fédération de votre batterie de serveurs (si vous avez une batterie de serveurs WID, vous devez exécuter cette commande sur le serveur de AD FS principal de votre batterie de serveurs) :

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID <attribute> -LookupForests <forest domain>
```

**AlternateLoginID** est le nom LDAP de l’attribut que vous souhaitez utiliser pour la connexion.

**LookupForests** est la liste des DNS de forêt auxquels vos utilisateurs appartiennent.

Pour activer la fonctionnalité d’ID de connexion de substitution, vous devez configurer les paramètres-AlternateLoginID et-LookupForests avec une valeur non null, valide.

Dans l’exemple suivant, vous activez d’autres fonctionnalités d’ID de connexion, de sorte que vos utilisateurs disposant de comptes dans les forêts contoso.com et fabrikam.com peuvent se connecter aux applications AD FS avec leur attribut « mail ».

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID mail -LookupForests contoso.com,fabrikam.com
```

3. Pour désactiver cette fonctionnalité, définissez la valeur des deux paramètres sur null.

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID $NULL -LookupForests $NULL
```

## <a name="hybrid-modern-authentication-with-alternate-id"></a>Authentification moderne hybride avec un autre ID

>[!IMPORTANT]
>Les éléments suivants ont été testés uniquement par rapport aux fournisseurs d’identité tiers AD FS et non tiers.

### <a name="exchange-and-skype-for-business"></a>Exchange et Skype entreprise
Si vous utilisez un ID de connexion de substitution avec Exchange et Skype entreprise, l’expérience utilisateur varie selon que vous utilisez ou non HMA.

>[!NOTE]
>Pour une expérience utilisateur optimale, Microsoft recommande l’utilisation de l’authentification hybride moderne.

ou plus d’informations, consultez [vue d’ensemble de l’authentification moderne hybride](https://support.office.com/en-us/article/Hybrid-Modern-Authentication-overview-and-prerequisites-for-using-it-with-on-premises-Skype-for-Business-and-Exchange-servers-ef753b32-7251-4c9e-b442-1a5aec14e58d)

### <a name="pre-requisites-for-exchange-and-skype-for-business"></a>Conditions préalables pour Exchange et Skype entreprise
Voici les conditions préalables pour obtenir l’authentification unique avec un autre ID.

- L’authentification moderne doit être activée sur Exchange Online.
- L’authentification moderne doit être activée sur Skype for Business (SFB) Online.
- L’authentification moderne doit être activée sur Exchange sur site.  Exchange 2013 CU19 ou Exchange 2016 CU18 et up est requis sur tous les serveurs Exchange. Aucun Exchange 2010 dans l’environnement.
- Skype entreprise local doit avoir une authentification moderne activée.
- Vous devez utiliser des clients Exchange et Skype sur lesquels l’authentification moderne est activée. Tous les serveurs doivent exécuter SFB Server 2015 CU5.
- Clients Skype entreprise prenant en charge l’authentification moderne
   - iOS, Android, Windows Phone
   - SFB 2016 (MA est activé par défaut, mais assurez-vous qu’il n’a pas été désactivé.)
   - SFB 2013 (MA est désactivée par défaut, vérifiez que la MA a été activée.)
   - SFB Mac Desktop
- Les clients Exchange qui sont compatibles avec l’authentification moderne et prennent en charge AltID REGKEYS
    - Office Pro plus 2016 uniquement





#### <a name="supported-office-version"></a>Version d’Office prise en charge

La configuration de votre annuaire pour l’authentification unique avec un autre ID à l’aide d’un autre ID peut entraîner des invites supplémentaires pour l’authentification si ces configurations supplémentaires ne sont pas terminées. Reportez-vous à l’article pour connaître l’impact possible sur l’expérience utilisateur avec l’ID de substitution.

Avec la configuration supplémentaire suivante, l’expérience utilisateur est considérablement améliorée et vous pouvez obtenir des invites presque zéro pour l’authentification pour les autres utilisateurs de votre organisation.

##### <a name="step-1-update-to-required-office-version"></a>Étape 1. Mise à jour vers la version d’Office requise
Office version 1712 (Build no 8827,2148) et versions ultérieures ont mis à jour la logique d’authentification pour gérer le scénario d’ID secondaire. Pour tirer parti de la nouvelle logique, les ordinateurs clients doivent être mis à jour vers Office version 1712 (Build no 8827,2148) et versions ultérieures.

##### <a name="step-2-update-to-required-windows-version"></a>Étape 2. Mise à jour vers la version requise de Windows
Windows version 1709 et versions ultérieures ont mis à jour la logique d’authentification pour gérer le scénario d’ID secondaire. Pour tirer parti de la nouvelle logique, les ordinateurs clients doivent être mis à jour vers Windows version 1709 et versions ultérieures.

##### <a name="step-3-configure-registry-for-impacted-users-using-group-policy"></a>Étape 3. Configurer le registre pour les utilisateurs affectés à l’aide d’une stratégie de groupe
Les applications Office s’appuient sur les informations transmises par l’administrateur d’annuaire pour identifier l’environnement de substitution d’ID. Les clés de Registre suivantes doivent être configurées pour aider les applications Office à authentifier l’utilisateur avec un autre ID sans afficher d’invites supplémentaires.

|RegKey à ajouter|Nom, type et valeur des données de la RegKey|Windows 7/8|Windows 10|Description|
|-----|-----|-----|-----|-----|
|HKEY_CURRENT_USER\Software\Microsoft\AuthN|DomainHint</br>REG_SZ</br>contoso.com|Obligatoire|Obligatoire|La valeur de cette RegKey est un nom de domaine personnalisé vérifié dans le locataire de l’organisation. Par exemple, contoso Corp peut fournir la valeur Contoso.com dans cette RegKey si Contoso.com est l’un des noms de domaine personnalisés vérifiés dans le Contoso.onmicrosoft.com du locataire.|
HKEY_CURRENT_USER\Software\Microsoft\Office\16.0\Common\Identity|EnableAlternateIdSupport</br>REG_DWORD</br>1|Requis pour Outlook 2016 ProPlus|Requis pour Outlook 2016 ProPlus|La valeur de cette RegKey peut être 1/0 pour indiquer à l’application Outlook s’il doit faire appel à la logique améliorée d’authentification de l’ID de remplacement.|
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\ZoneMap\Domains\contoso.com\sts|&#42;</br>REG_DWORD</br>1|Obligatoire|Obligatoire|Vous pouvez utiliser cette RegKey pour définir le STS comme zone approuvée dans les paramètres Internet. Le déploiement ADFS standard recommande l’ajout de l’espace de noms ADFS à la zone Intranet local pour Internet Explorer|

## <a name="new-authentication-flow-after-additional-configuration"></a>Nouveau workflow d’authentification après une configuration supplémentaire

![Workflow d’authentification](media/Configure-Alternate-Login-ID/alt1a.png)

1. Un L’utilisateur est approvisionné dans Azure AD à l’aide d’un autre ID
   </br>p L’administrateur de l’annuaire pousse les paramètres de RegKey requis sur les ordinateurs clients concernés
2. L’utilisateur s’authentifie sur l’ordinateur local et ouvre une application Office
3. L’application Office prend les informations d’identification de session locale
4. L’application Office s’authentifie auprès d’Azure AD à l’aide de l’indicateur de domaine envoyé par l’administrateur et les informations d’identification locales
5. Azure AD authentifie correctement l’utilisateur en dirigeant pour corriger le domaine de Fédération et émettre un jeton

## <a name="applications-and-user-experience-after-the-additional-configuration"></a>Applications et expérience utilisateur après la configuration supplémentaire

### <a name="non-exchange-and-skype-for-business-clients"></a>Clients non-Exchange et Skype entreprise

|Client|Support (instruction)|Notes|
| ----- | -----|-----|
|Microsoft Teams|Prise en charge|<li>Microsoft teams prend en charge les AD FS (SAML-P, WS-FED, WS-Trust et OAuth) et l’authentification moderne.</li><li> Les fonctionnalités principales de Microsoft, telles que les canaux, les conversations et les fonctionnalités de fichiers, fonctionnent avec un ID de connexion de substitution.</li><li>les applications de première et tierces doivent être examinées séparément par le client. Cela est dû au fait que chaque application possède ses propres protocoles d’authentification de prise en charge.</li>|     
|OneDrive Entreprise|Prise en charge : clé de Registre côté client recommandée |Avec l’ID secondaire configuré, vous voyez que l’UPN local est prérempli dans le champ vérification. Cela doit être remplacé par l’identité qui est utilisée. Nous vous recommandons d’utiliser la clé de Registre côté client indiquée dans cet article : Office 2013 et Lync 2013 demandent périodiquement des informations d’identification pour SharePoint Online, OneDrive et Lync Online.|
|Client mobile OneDrive entreprise|Prise en charge|| 
|Page d’activation d’Office 365 Pro plus|Prise en charge : clé de Registre côté client recommandée|Avec l’ID secondaire configuré, vous voyez que l’UPN local est prérempli dans le champ vérification. Cela doit être remplacé par l’identité qui est utilisée. Nous vous recommandons d’utiliser la clé de Registre côté client indiquée dans cet article : Office 2013 et Lync 2013 demandent périodiquement des informations d’identification pour SharePoint Online, OneDrive et Lync Online.|

### <a name="exchange-and-skype-for-business-clients"></a>Clients Exchange et Skype entreprise

|Client|Support Statement-with HMA|Déclaration de support-sans HMA|
| ----- |----- | ----- |
|Outlook|Pris en charge, aucune invite supplémentaire|Prise en charge</br></br>Avec **l’authentification moderne** pour Exchange Online : Prise en charge</br></br>Avec **l’authentification régulière** pour Exchange Online : Pris en charge avec les avertissements suivants :</br><li>Vous devez être sur un ordinateur joint à un domaine et être connecté au réseau d’entreprise </li><li>Vous pouvez utiliser uniquement un autre ID dans les environnements qui n’autorisent pas l’accès externe pour les utilisateurs de boîtes aux lettres. Cela signifie que les utilisateurs peuvent uniquement s’authentifier sur leur boîte aux lettres de manière prise en charge lorsqu’ils sont connectés et joints au réseau d’entreprise, sur un VPN ou connectés via des ordinateurs à accès direct, mais vous recevez quelques invites supplémentaires lors de la configuration de votre profil Outlook.| 
|Dossiers publics hybrides|Pris en charge, aucune invite supplémentaire.|Avec **l’authentification moderne** pour Exchange Online : Prise en charge</br></br>Avec **l’authentification régulière** pour Exchange Online : Non prise en charge</br></br><li>Les dossiers publics hybrides ne peuvent pas être étendus si des ID de substitution sont utilisés et ne doivent donc pas être utilisés aujourd’hui avec les méthodes d’authentification standard.|
|Délégation entre locaux|Consultez [configurer Exchange pour prendre en charge les autorisations déléguées de boîte aux lettres dans un déploiement hybride](https://technet.microsoft.com/library/mt784505.aspx)|Consultez [configurer Exchange pour prendre en charge les autorisations déléguées de boîte aux lettres dans un déploiement hybride](https://technet.microsoft.com/library/mt784505.aspx)|
|Archiver l’accès aux boîtes aux lettres (boîte aux lettres locale-Archive dans le Cloud)|Pris en charge, aucune invite supplémentaire|Pris en charge : les utilisateurs reçoivent une invite supplémentaire pour les informations d’identification lors de l’accès à l’archive. ils doivent fournir leur autre ID lorsque vous y êtes invité.| 
|Outlook Web Access|Prise en charge|Prise en charge|
|Outlook Mobile Apps pour Android, IOS et Windows Phone|Prise en charge|Prise en charge|
|Skype entreprise/Lync|Pris en charge, sans invite supplémentaire|Pris en charge (sauf indication contraire), mais il existe un risque de confusion pour l’utilisateur.</br></br>Sur les clients mobiles, l’ID alternatif est pris en charge uniquement si adresse SIP = adresse de messagerie = autre ID.</br></br> Il se peut que les utilisateurs doivent se connecter deux fois au client de bureau Skype entreprise, en utilisant d’abord l’UPN local, puis l’ID secondaire. (Notez que l’adresse de connexion est en fait l’adresse SIP qui peut ne pas être la même que le « nom d’utilisateur », bien que c’est souvent le cas). Quand vous êtes invité à entrer un nom d’utilisateur pour la première fois, l’utilisateur doit entrer l’UPN, même s’il est pré-rempli de manière incorrecte avec l’ID ou l’adresse SIP de remplacement. Une fois que l’utilisateur a cliqué sur se connecter avec l’UPN, l’invite de nom d’utilisateur réapparaît et cette fois prérempli avec l’UPN. Cette fois-ci, l’utilisateur doit remplacer ceci par l’ID de substitution, puis cliquer sur se connecter pour terminer le processus de connexion. Sur les clients mobiles, les utilisateurs doivent entrer l’ID d’utilisateur local dans la page avancé, en utilisant le format de type SAM (domaine\nom_utilisateur), et non le format UPN.</br></br>Après une connexion réussie, si Skype entreprise ou Lync indique « Exchange a besoin de vos informations d’identification », vous devez fournir les informations d’identification valides pour l’emplacement de la boîte aux lettres. Si la boîte aux lettres se trouve dans le Cloud, vous devez fournir l’ID de remplacement. Si la boîte aux lettres est locale, vous devez fournir l’UPN local.| 

## <a name="additional-details--considerations"></a>Détails supplémentaires & considérations

-   La fonctionnalité de l’ID de connexion de substitution est disponible pour les environnements fédérés avec AD FS déployée.  Il n’est pas pris en charge dans les scénarios suivants :
    -   Domaines non routables (par exemple, contoso. local) qui ne peuvent pas être vérifiés par Azure AD.
    -   Environnements gérés qui n’ont pas AD FS déployés.


-   Lorsque cette fonctionnalité est activée, l’ID de connexion de substitution est disponible uniquement pour l’authentification par nom d’utilisateur/mot de passe sur tous les protocoles d’authentification par nom d’utilisateur/mot de passe pris en charge par AD FS (SAML-P, WS-FED, WS-Trust et OAuth).


-   Lorsque l’authentification intégrée Windows (WIA) est exécutée (par exemple, lorsque les utilisateurs essaient d’accéder à une application d’entreprise sur un ordinateur appartenant à un domaine à partir d’un intranet et AD FS administrateur a configuré la stratégie d’authentification pour utiliser WIA pour l’intranet), UPN IsUsed pour l’authentification. Si vous avez configuré des règles de revendication pour les parties de confiance pour la fonctionnalité ID de connexion de substitution, vous devez vous assurer que ces règles sont toujours valides dans le cas WIA.

-   Lorsque cette fonctionnalité est activée, l’ID de connexion de substitution requiert qu’au moins un serveur de catalogue global soit accessible à partir du serveur de AD FS pour chaque forêt de compte d’utilisateur prise en charge par AD FS. Si vous ne parvenez pas à atteindre un serveur de catalogue global dans la forêt de comptes d’utilisateur, AD FS revenir à l’utilisation de l’UPN. Par défaut, tous les contrôleurs de domaine sont des serveurs de catalogue global.

-   Si cette option est activée, si le serveur AD FS trouve plusieurs objets utilisateur avec la même valeur d’ID de connexion que celle spécifiée dans toutes les forêts de comptes d’utilisateurs configurées, la connexion échoue.

-   Lorsque la fonctionnalité ID de connexion de substitution est activée, AD FS tente d’abord d’authentifier l’utilisateur final avec un ID de connexion de substitution, puis de revenir à l’utilisation de l’UPN s’il ne trouve pas de compte pouvant être identifié par l’ID de connexion de substitution. Vous devez vérifier qu’il n’y a pas de conflit entre l’ID de connexion de substitution et l’UPN si vous souhaitez toujours prendre en charge la connexion UPN. Par exemple, la définition de l’attribut de messagerie de l’autre avec l’UPN de l’autre empêche l’autre utilisateur de se connecter avec son UPN.

-   Si l’une des forêts configurée par l’administrateur est en cours, AD FS continue à rechercher le compte d’utilisateur avec un ID de connexion de substitution dans d’autres forêts configurées. Si AD FS serveur trouve un objet utilisateur unique dans les forêts dans lesquelles il a effectué une recherche, un utilisateur se connecte avec succès.

-   Vous pouvez en outre personnaliser la page de connexion AD FS pour fournir aux utilisateurs finaux des indications sur l’ID de connexion de substitution. Vous pouvez le faire en ajoutant la description de la page de connexion personnalisée (pour plus d’informations, consultez [Personnalisation des pages de connexion AD FS](https://technet.microsoft.com/library/dn280950.aspx) ou personnalisation de la chaîne « se connecter avec le compte de l’organisation » au-dessus du champ nom d’utilisateur (pour plus d’informations, consultez [ Personnalisation avancée des pages de connexion AD FS](https://technet.microsoft.com/library/dn636121.aspx).

-   Le nouveau type de revendication qui contient la valeur de l’ID de connexion de remplacement est **http :schemas. Microsoft. com/ws/2013/11/alternateloginid**

## <a name="events-and-performance-counters"></a>Événements et compteurs de performances
Les compteurs de performances suivants ont été ajoutés pour mesurer les performances des serveurs AD FS lorsque l’ID de connexion de substitution est activé :

-   Authentifications d’ID de connexion de substitution : nombre d’authentifications effectuées à l’aide d’un ID de connexion de substitution

-   Authentifications d’ID de connexion de substitution/s : nombre d’authentifications effectuées à l’aide d’un ID de connexion de substitution par seconde

-   Latence de recherche moyenne pour l’ID de connexion de substitution : latence de recherche moyenne sur les forêts qu’un administrateur a configurées pour l’ID de connexion de substitution

Voici les différents cas d’erreur et l’impact correspondant sur l’expérience de connexion d’un utilisateur avec des événements consignés par AD FS :



|                       **Cas d’erreur**                        | **Impact sur l’expérience de connexion** |                                                              **Événement**                                                              |
|--------------------------------------------------------------|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| Impossible d’obtenir une valeur pour SAMAccountName pour l’objet utilisateur |          Échec de la connexion           |                  ID d’événement 364 avec le message d’exception MSIS8012 : SamAccountName introuvable pour l’utilisateur : '{0}'.                   |
|        L’attribut CanonicalName n’est pas accessible         |          Échec de la connexion           |               ID d’événement 364 avec le message d’exception MSIS8013 : CanonicalName : '{0}'de l’utilisateur : '{1}'est dans un format incorrect.                |
|        Plusieurs objets utilisateur se trouvent dans une forêt        |          Échec de la connexion           | ID d’événement 364 avec le message d’exception MSIS8015 : Plusieurs comptes d’utilisateur avec l’identité{0}« » ont été{1}trouvés dans la forêt « » avec des identités :{2} |
|   Plusieurs objets utilisateur sont trouvés dans plusieurs forêts    |          Échec de la connexion           |           ID d’événement 364 avec le message d’exception MSIS8014 : Plusieurs comptes d’utilisateur avec l’identité{0}' 'ont été trouvés dans les forêts :{1}            |

## <a name="see-also"></a>Voir aussi
[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md)


