---
ms.assetid: f0cbdd78-f5ae-47ff-b5d3-96faf4940f4a
title: Configuration des ID de connexion alternatif
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 11/14/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 615faf4153949aa4ad989f017068d1809fca26b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820870"
---
# <a name="configuring-alternate-login-id"></a>Configuration des ID de connexion alternatif

>S'applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

## <a name="what-is-alternate-login-id"></a>Nouveautés d’ID de connexion alternatif ?
Dans la plupart des scénarios, les utilisateurs utiliser leur UPN (nom d’utilisateur Principal) pour vous connecter à leurs comptes. Toutefois, dans certains environnements en raison des stratégies d’entreprise ou les dépendances de line-of-business application en local, les utilisateurs peuvent utiliser une autre forme de la connexion. 

>[!NOTE]
>Il recommandée par Microsoft sont des meilleures pratiques pour faire correspondre des UPN à l’adresse SMTP principale. Cet article aborde le faible pourcentage de clients qui ne peut pas corriger l’UPN de faire pour correspondre.

Par exemple, ils peuvent être à l’aide de leur id d’e-mail pour la connexion et qui peut être différent de leur UPN. Il s’agit en particulier une occurrence courante dans les scénarios où leur UPN est non routable. Prenons un utilisateur Jane Doe avec UPN jdoe@contoso.local et adresse de messagerie jdoe@contoso.com. Jane n’est peut-être pas encore prenant en charge de l’UPN qu’elle a toujours utilisé son id de courrier électronique pour la connexion. L’utilisation de toute autre méthode de connexion au lieu de l’UPN constitue une autre ID. Pour plus d’informations sur la façon dont l’UPN est crée, consultez [remplissage de UserPrincipalName Azure AD](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-userprincipalname).

Active Directory Federation Services (ADFS) permet aux applications fédérées à l’aide d’AD FS pour se connecter en utilisant un autre code. Cela permet aux administrateurs de spécifier une alternative à la valeur par défaut UPN à utiliser pour la connexion. AD FS prend déjà en charge à l’aide de n’importe quel écran de l’identificateur d’utilisateur qui est acceptée par les Services de domaine Active Directory (AD DS). Lorsque configuré pour l’ID de l’autre, AD FS permet aux utilisateurs de se connecter à l’aide de la valeur d’ID de substitution configurée, par exemple, id d’e-mail. À l’aide de l’ID alternatif vous permet d’adopter des fournisseurs SaaS, telles qu’Office 365 sans modifier vos UPN locaux. Il vous permet également de prendre en charge des applications line of business service avec les identités de consommateur allocation.

## <a name="alternate-id-in-azure-ad"></a>Id secondaire dans Azure AD
Une organisation peut avoir à utiliser un autre ID dans les scénarios suivants :
1.  Le nom de domaine local est non routable, par ex. Contoso.local et par conséquent, le nom d’utilisateur principal par défaut est non routable (jdoe@contoso.local). UPN existant ne sont pas modifiables en raison des dépendances des applications locales ou des stratégies d’entreprise. Azure AD et Office 365 nécessitent tous les suffixes de domaine associés au répertoire Azure AD pour être entièrement internet routable. 
2.  L’UPN local n’est pas identique à l’adresse e-mail utilisateur et pour vous connecter à Office 365, les utilisateurs utiliser adresse de messagerie et UPN ne peut pas être utilisé en raison de contraintes organisationnelles.
Dans les scénarios mentionnés ci-dessus, autre ID avec AD FS permet aux utilisateurs pour se connecter à Azure AD sans modifier vos UPN locaux. 

## <a name="end-user-experience-with-alternate-login-id"></a>Utilisateur final avec l’ID de connexion de substitution
L’expérience utilisateur varie en fonction de la méthode d’authentification utilisée avec l’id de connexion de substitution.  Actuellement là trois façons différentes dans lesquels à l’aide des id de connexion de substitution peut être effectuée.  Celles-ci sont les suivantes :

- **Authentification standard (hérité)**-utilise le protocole d’authentification de base.
- **L’authentification moderne** -apporte basée sur Active Directory Authentication Library ADAL connectez-vous aux applications. Ainsi, les fonctionnalités de connexion telles que l’authentification multifacteur (MFA), basée sur SAML des fournisseurs d’identité tiers avec les applications clientes Office, carte à puce et authentification par certificat.
- **L’authentification moderne hybride** - fournit tous les avantages de l’authentification moderne et fournit aux utilisateurs la possibilité d’accéder aux applications locales à l’aide des jetons d’autorisation obtenus à partir du cloud.

>[!NOTE]
> Pour la meilleure expérience possible, Microsoft recommande vivement l’authentification moderne hybride.



## <a name="configure-alternate-logon-id"></a>Configurer l’ID d’ouverture de session secondaire
Il est recommandé à l’aide d’Azure AD à l’aide d’Azure AD se connecter nous connecter pour configurer l’ID d’ouverture de session secondaire pour votre environnement.

- Pour la nouvelle configuration d’Azure AD Connect, consultez se connecter à Azure AD pour obtenir des instructions détaillées sur la façon de configurer la batterie de serveurs FS ID et AD autre.
- Pour les installations existantes de Azure AD Connect, consultez Modification de la méthode d’authentification utilisateur pour obtenir des instructions sur la modification de méthode de connexion à AD FS

Lorsqu’Azure AD Connect est fourni plus d’informations sur l’environnement AD FS, il vérifie automatiquement la présence de la base de connaissances de droite sur vos services AD FS et configure AD FS pour l’ID secondaire, y compris toutes les règles de revendication approprié nécessaire pour l’approbation de fédération Azure AD. Il n’existe aucune étape supplémentaire requise extérieur configuration Assistant autre ID.

>[!NOTE]
> Microsoft recommande d’utiliser Azure AD Connect pour configurer l’ID de connexion de remplacement.

### <a name="manually-configure-alternate-id"></a>Configurer manuellement les ID de substitution
Pour configurer l’ID de connexion secondaire, vous devez effectuer les tâches suivantes : Configurer vos approbations de fournisseur de revendications AD FS pour activer l’ID de connexion alternatif

1.  Si vous avez Server 2012 R2, assurez-vous de qu'avoir KB2919355 installé sur tous les serveurs AD FS. Vous pouvez l’obtenir via Windows Update Services ou téléchargez-la directement. 

2.  Mettre à jour la configuration AD FS en exécutant l’applet de commande PowerShell suivante sur un des serveurs de fédération de votre batterie de serveurs (si vous avez une batterie de serveurs WID, vous devez exécuter cette commande sur le serveur principal AD FS dans votre batterie de serveurs) :

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID <attribute> -LookupForests <forest domain>
```

**AlternateLoginID** est le nom LDAP de l’attribut que vous souhaitez utiliser pour la connexion.

**LookupForests** est la liste de forêt DNS appartenant à vos utilisateurs.

Pour activer la fonctionnalité d’ID de connexion de substitution, vous devez configurer des paramètres AlternateLoginID - et - LookupForests avec une valeur non null, valide.

Dans l’exemple suivant, vous activez fonctionnalités des ID de connexion de substitution tels que les utilisateurs disposant de comptes dans les forêts contoso.com et fabrikam.com peuvent vous connecter à AD FS les applications compatibles avec leur attribut de « messagerie ».

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID mail -LookupForests contoso.com,fabrikam.com
```

3.  Pour désactiver cette fonctionnalité, définissez la valeur pour les deux paramètres avoir la valeur null.

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID $NULL -LookupForests $NULL
```

## <a name="hybrid-modern-authentication-with-alternate-id"></a>Authentification moderne hybride avec l’ID de l’autre

>[!IMPORTANT]
>Les éléments suivants a uniquement été testé avec AD FS et les fournisseurs d’identité tiers pas 3e.

### <a name="exchange-and-skype-for-business"></a>Exchange et Skype pour entreprises
Si vous utilisez un autre id de connexion avec Exchange et Skype pour entreprises, l’expérience utilisateur varie selon si vous utilisez la zone de mémoire haute.

>[!NOTE]
>Pour optimiser l’expérience utilisateur final, Microsoft recommande d’utiliser l’authentification moderne hybride.

ou plus d’informations, consultez [vue d’ensemble de l’authentification moderne hybride](https://support.office.com/en-us/article/Hybrid-Modern-Authentication-overview-and-prerequisites-for-using-it-with-on-premises-Skype-for-Business-and-Exchange-servers-ef753b32-7251-4c9e-b442-1a5aec14e58d)

### <a name="pre-requisites-for-exchange-and-skype-for-business"></a>Conditions préalables pour Exchange et Skype pour entreprises
Voici les composants requis pour réaliser l’authentification unique avec l’ID de substitution.

- Exchange Online doit avoir l’authentification moderne activée.
- Skype pour entreprise (SFB) Online doit avoir l’authentification moderne activée.
- Exchange sur site doit avoir l’authentification moderne activée.  Exchange 2013 CU19 ou Exchange 2016 CU18 et est requis sur tous les serveurs Exchange. Aucun Exchange 2010 dans l’environnement.
- Skype for Business sur site doit avoir l’authentification moderne activée.
- Vous devez utiliser Exchange et Skype les clients qui ont une authentification moderne activée. Tous les serveurs doivent être en cours d’exécution SFB Server 2015 CU5.
- Skype pour les Clients d’entreprise qui sont capables de l’authentification moderne
   - iOS, Android, Windows Phone
   - SFB 2016 (agent de gestion est activé par défaut, mais assurez-vous qu’il n’a pas été désactivé.)
   - SFB 2013 (agent de gestion est désactivée par défaut, assurez-vous donc MA a été activée.)
   - Poste de travail SFB Mac
- Clients Exchange qui sont capables de l’authentification moderne et prennent en charge AltID regkeys
    - Office Professionnel Plus 2016 uniquement





#### <a name="supported-office-version"></a>Version d’Office prise en charge

Configuration de votre annuaire pour l’authentification unique avec l’id alternatif-id à l’aide de remplacement peut provoquer des invites supplémentaires pour l’authentification si ces configurations supplémentaires ne sont pas terminées. Consultez l’article pour l’impact possible sur l’expérience utilisateur avec l’id de l’autre.

Avec une configuration supplémentaire, l’expérience utilisateur est sensiblement améliorée, et vous pouvez obtenir près de zéro invites pour l’authentification des utilisateurs de l’id de l’autre dans votre organisation.

##### <a name="step-1-update-to-required-office-version"></a>Étape 1. Mettre à jour vers la version d’office requise
Version d’Office 1712 (ne build aucun 8827.2148) et versions ultérieures ont mis à jour la logique d’authentification pour gérer le scénario de l’id de l’autre. Pour tirer parti de la nouvelle logique, les ordinateurs clients doivent être mis à jour vers la version d’office 1712 (ne build aucun 8827.2148) et versions ultérieures.

##### <a name="step-2-configure-registry-for-impacted-users-using-group-policy"></a>Étape 2. Configurez le Registre pour les utilisateurs affectés à l’aide de la stratégie de groupe
Les applications office s’appuient sur les informations envoyées par l’administrateur d’annuaire pour identifier l’environnement de l’id de l’autre. Les clés de Registre suivantes doivent être configurés pour aider les applications office à authentifier l’utilisateur avec l’id de l’autre sans afficher les invites supplémentaires

|Clé de Registre à ajouter|Valeur, type et nom de clé de Registre de données|Windows 7/8|Windows 10|Description|
|-----|-----|-----|-----|-----|
|HKEY_CURRENT_USER\Software\Microsoft\AuthN|DomainHint</br>REG_SZ</br>contoso.com|Obligatoire|Obligatoire|La valeur de cette clé de Registre est un nom de domaine personnalisé vérifié dans le client de l’organisation. Par exemple, Contoso corp peut fournir une valeur de Contoso.com dans cette clé de Registre si Contoso.com est un des noms de domaine personnalisé vérifié dans le locataire Contoso.onmicrosoft.com.|
HKEY_CURRENT_USER\Software\Microsoft\Office\16.0\Common\Identity|EnableAlternateIdSupport</br>REG_DWORD</br>1|Requis pour Outlook 2016 ProPlus|Requis pour Outlook 2016 ProPlus|La valeur de cette clé de Registre peut être 1 / 0 pour indiquer à l’application Outlook si elle doit s’engager à la logique d’authentification alternatif-id améliorée.|
HKEY_CURRENT_USER\SOFTWARE\Microsoft\Office\16.0\Common\Identity|DisableADALatopWAMOverride</br>REG_DWORD</br>1|Non applicable|Obligatoire.|Cela garantit qu’Office n’utilise pas WAM alt-id n’est pas pris en charge par WAM.|
HKEY_CURRENT_USER\SOFTWARE\Microsoft\Office\16.0\Common\Identity|DisableAADWAM</br>REG_DWORD</br>1|Non applicable|Obligatoire.|Cela garantit qu’Office n’utilise pas WAM alt-id n’est pas pris en charge par WAM.|
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\ZoneMap\Domains\contoso.com\sts|&#42;</br>REG_DWORD</br>1|Obligatoire|Obligatoire|Cette clé de Registre peut être utilisé pour définir le STS comme une Zone de confiance dans les paramètres d’internet. Déploiement d’ADFS standard recommande d’y ajouter l’espace de noms ADFS pour la Zone Intranet Local pour Internet Explorer|

## <a name="new-authentication-flow-after-additional-configuration"></a>Nouveau flux d’authentification après une configuration supplémentaire

![Flux d’authentification](media/Configure-Alternate-Login-ID/alt1a.png)

1. a: Utilisateur est configuré dans Azure AD à l’aide de l’id de remplacement
   </br>b : Administrateur d’annuaire transmet les paramètres de clé de Registre requis sur les ordinateurs clients concernés
2. Utilisateur s’authentifie sur l’ordinateur local et ouvre une application office
3. Application Office prend les informations d’identification de session locale
4. Application Office s’authentifie auprès d’Azure AD à l’aide d’indication de domaine transmise par l’administrateur et les informations d’identification locales
5. Azure AD authentifie correctement l’utilisateur en dirigeant vers le domaine de fédération de corriger et d’émettre un jeton

## <a name="applications-and-user-experience-after-the-additional-configuration"></a>Applications et une expérience utilisateur après la configuration supplémentaire

### <a name="non-exchange-and-skype-for-business-clients"></a>Non-Exchange et Skype pour les Clients d’entreprise
|Client|Instruction de prise en charge|Notes|
| ----- | -----|-----|
|Microsoft Teams|Prise en charge|<li>Microsoft Teams prend en charge AD FS (SAML-P, WS-Fed, WS-Trust et OAuth) et l’authentification moderne.</li><li> Core Microsoft Teams telles que des fonctionnalités de canaux, les conversations et les fichiers ne fonctionne pas avec l’ID de connexion de substitution.</li><li>applications tierces 1er et 3e doivent être surveillées séparément par le client. Il s’agit, car chaque application possède ses propres protocoles d’authentification prise en charge.</li>|     
|OneDrive Entreprise|Prise en charge - clé de Registre côté client recommandé |Avec un autre ID configuré, vous consultez que l’UPN local est prérempli dans le champ de vérification. Cette opération doit être modifié à l’autre identité qui est utilisée. Nous vous recommandons d’utiliser la clé de Registre côté client indiquée dans cet article : Office 2013 et Lync 2013 invitent régulièrement les informations d’identification pour SharePoint Online, OneDrive et Lync Online.|
|Client OneDrive entreprise Mobile|Prise en charge|| 
|Page de l’activation de Office 365 Pro Plus|Prise en charge - clé de Registre côté client recommandé|Avec un autre ID configuré, vous consultez que l’UPN local est prérempli dans le champ de vérification. Cette opération doit être modifié à l’autre identité qui est utilisée. Nous vous recommandons d’utiliser la clé de Registre côté client indiquée dans cet article : Office 2013 et Lync 2013 invitent régulièrement les informations d’identification pour SharePoint Online, OneDrive et Lync Online.|

### <a name="exchange-and-skype-for-business-clients"></a>Exchange et Skype pour les Clients d’entreprise

|Client|Instruction de prise en charge - avec la zone de mémoire haute|Instruction de prise en charge - sans zone de mémoire haute|
| ----- |----- | ----- |
|Outlook|Invites pris en charge, ne supplémentaires|Prise en charge</br></br>Avec **l’authentification moderne** pour Exchange Online : Prise en charge</br></br>Avec **authentification régulière** pour Exchange Online : Prise en charge avec les mises en garde suivantes :</br><li>Vous devez être sur un ordinateur joint au domaine et connecté au réseau d’entreprise </li><li>Vous pouvez uniquement utiliser un ID secondaire dans les environnements qui n’autorisent pas l’accès externe pour les utilisateurs de boîtes aux lettres. Cela signifie que les utilisateurs peuvent uniquement s’authentifier à leur boîte aux lettres dans une méthode prise en charge lorsqu’elles sont connectées et joint au réseau d’entreprise, sur un réseau privé virtuel ou par le biais de machines d’un accès Direct, mais vous obtenez deux invites supplémentaires lorsque vous configurez votre profil Outlook.| 
|Dossiers publics hybride|Prise en charge, supplémentaire n’est demandé.|Avec **l’authentification moderne** pour Exchange Online : Prise en charge</br></br>Avec **authentification régulière** pour Exchange Online : Non prise en charge</br></br><li>Dossiers publics hybride ne sont pas en mesure de développer si autre ID sont utilisé et doit donc pas être utilisé dès aujourd'hui avec les méthodes d’authentification standard.|
|Cross-site délégation|Consultez [configurer d’Exchange pour prendre en charge les autorisations déléguées de boîtes aux lettres dans un déploiement hybride](https://technet.microsoft.com/library/mt784505.aspx)|Consultez [configurer d’Exchange pour prendre en charge les autorisations déléguées de boîtes aux lettres dans un déploiement hybride](https://technet.microsoft.com/library/mt784505.aspx)|
|Accès aux boîtes aux lettres d’archives (boîte aux lettres en local - archive dans le cloud)|Invites pris en charge, ne supplémentaires|Prise en charge : les utilisateurs obtiennent une invite supplémentaire pour les informations d’identification lorsqu’ils accèdent à l’archive, ils doivent fournir leur ID secondaire lorsque vous y êtes invité.| 
|Outlook Web Access|Prise en charge|Prise en charge|
|Applications mobiles d’Outlook pour Android, IOS et Windows Phone|Prise en charge|Prise en charge|
|Skype for Business / Lync|Prise en charge, sans invite supplémentaire|Prise en charge (sauf indication contraire), mais il existe un risque de confusion de l’utilisateur.</br></br>Sur les clients mobiles, un Id secondaire est pris en charge uniquement si adresse SIP = adresse de messagerie = ID secondaire.</br></br> Les utilisateurs devront peut-être connectez-vous à deux reprises pour le Skype pour Business desktop client, tout d’abord à l’aide de l’UPN local, puis en utilisant l’ID secondaire. (Notez que l’adresse « connexion » est en réalité l’adresse SIP qui ne peut pas être identique à « nom d’utilisateur », mais est souvent). Lorsque tout d’abord invité à entrer un nom d’utilisateur, l’utilisateur doit entrer l’UPN, même si elle est incorrectement rempli au préalable avec l’adresse ID secondaire ou SIP. Une fois que l’utilisateur clique sur se connecter à l’aide de l’UPN, l’utilisateur nom invite réapparaît, cette fois prérempli avec l’UPN. Cette fois, l’utilisateur doit le remplacer par l’autre ID et cliquez sur se connecter terminer le processus de connexion. Sur les clients mobiles, les utilisateurs doivent entrer l’ID d’utilisateur local dans la page Avancé, à l’aide du style de SAM (DOMAINE\nom_utilisateur), non format UPN.</br></br>Après la connexion réussie, Skype entreprise ou Lync « Exchange doit vos informations d’identification », en revanche, si vous devez fournir les informations d’identification qui sont valides pour où se trouve la boîte aux lettres. Si la boîte aux lettres se trouve dans le cloud, que vous devez fournir l’ID secondaire. Si la boîte aux lettres est en local, vous devez fournir l’UPN en local.| 
 
## <a name="additional-details--considerations"></a>Considérations sur les & détails supplémentaires

-   La fonctionnalité d’ID de connexion de substitution est disponible pour les environnements fédérés avec AD FS déployé.  Il n’est pas pris en charge dans les scénarios suivants :
    -   Non routables domaines (par exemple, Contoso.local) qui ne peut pas être vérifiés par Azure AD.
    -   Géré les environnements qui n’ont pas d’AD FS déployé.


-   Lorsque l’option est activée, la fonctionnalité d’ID de connexion de substitution est uniquement disponible pour l’authentification du nom d’utilisateur/mot de passe sur tous les protocoles d’authentification de nom/mot de passe d’utilisateur pris en charge par AD FS (SAML-P, WS-Fed, WS-Trust et OAuth).


-   Si l’authentification intégrée de Windows (WIA) est effectuée (par exemple, lorsque utilisateurs essaient d’accéder à une application d’entreprise sur un ordinateur joint au domaine à partir de l’intranet et administrateur AD FS a configuré la stratégie d’authentification pour utiliser WIA pour intranet) et est utilisé UPN pour l’authentification. Si vous avez configuré les règles de revendication pour les parties de confiance pour la fonctionnalité d’ID de connexion de substitution, il se peut que vous devez vous assurer que ces règles sont toujours valides dans le cas de WIA.

-   Lorsque activé, la fonctionnalité d’ID de connexion de substitution requiert au moins un serveur de catalogue global pour être accessibles depuis le serveur AD FS pour chaque forêt de comptes d’utilisateur AD FS prend en charge. Échec d’atteindre un serveur de catalogue global dans la forêt de comptes d’utilisateur entraîne dans AD FS pour utiliser UPN en secours. Par défaut tous les contrôleurs de domaine sont des serveurs de catalogue global.

-   Si activé, si le serveur AD FS ne trouve plus d’un objet utilisateur avec la même valeur d’ID de connexion de substitution spécifiée à travers toutes les forêts de comptes d’utilisateur configuré, la connexion échoue.

-   Lors de la fonctionnalité d’ID de connexion de substitution est activée, AD FS essaie d’abord authentifier l’utilisateur final avec l’ID de connexion de substitution et puis se rabattront pour utiliser l’UPN s’il ne trouve pas un compte qui peut être identifié par l’ID de connexion de substitution. Vous devez vous assurer qu’aucun conflit entre l’ID de connexion de substitution et de l’UPN si vous souhaitez toujours prendre en charge l’ouverture de session UPN. Par exemple, UPN définissant l’attribut de messagerie avec les autres blocs l’autre utilisateur de se connecter avec son UPN.

-   Si une des forêts qui est configuré par l’administrateur est arrêté, AD FS continue à rechercher le compte d’utilisateur avec l’ID de connexion de substitution dans d’autres forêts qui sont configurés. Si le serveur AD FS recherche un utilisateur unique d’objets entre les forêts qui lui a recherché, un utilisateur se connecte avec succès.

-   Vous pouvez souhaiter également personnaliser la page de connexion AD FS pour donner aux utilisateurs finaux certain indicateur sur l’ID de connexion de substitution. Vous pouvez le faire soit en ajoutant la description de la page de connexion personnalisé (pour plus d’informations, consultez [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx) ou en personnalisant la chaîne de « Connectez-vous avec un compte professionnel » au-dessus de champ de nom d’utilisateur (pour plus d’informations plus d’informations, consultez [Advanced Customization of AD FS Sign-in Pages](https://technet.microsoft.com/library/dn636121.aspx).

-   Le nouveau type de revendication qui contient la valeur d’ID de connexion de substitution est **http:schemas.microsoft.com/ws/2013/11/alternateloginid**

## <a name="events-and-performance-counters"></a>Événements et compteurs de performances
Les compteurs de performances suivants ont été ajoutées pour mesurer les performances des serveurs AD FS lors de l’ID de connexion de substitution est activé :

-   Autre authentifications d’Id de connexion : nombre d’authentifications effectuées à l’aide des ID de connexion alternatif

-   Autre Id de connexion authentifications/s : le nombre d’authentifications effectuées à l’aide des ID de connexion de substitution par seconde

-   Nombre moyen de latence de recherche pour l’ID de connexion alternatif : moyenne de latence de recherche pour les forêts un administrateur a configuré pour l’ID de connexion de substitution

Voici les différents cas d’erreur et impact correspondant sur l’expérience d’un utilisateur de connexion avec les événements consignés par AD FS :



**Cas d’erreur**|**Impact sur l’expérience de connexion**|**Événement**|
---------|---------|---------
Impossible d’obtenir une valeur de SAMAccountName pour l’objet utilisateur|Échec de connexion|ID d’événement 364 avec le message d’exception MSIS8012 : Impossible de trouver le samAccountName de l’utilisateur : '{0}'.|
L’attribut CanonicalName n’est pas accessible|Échec de connexion|ID d’événement 364 avec le message d’exception MSIS8013 : CanonicalName : '{0}» de l’utilisateur : «{1}» est dans un format incorrect.|
Plusieurs objets utilisateur sont trouvent dans une forêts|Échec de connexion|ID d’événement 364 avec le message d’exception MSIS8015 : Trouvé plusieurs comptes d’utilisateur avec l’identité «{0}« dans la forêt »{1}» avec des identités : {2}|
Plusieurs objets utilisateur sont trouvent dans plusieurs forêts|Échec de connexion|ID d’événement 364 avec le message d’exception MSIS8014 : Trouvé plusieurs comptes d’utilisateur avec l’identité «{0}» dans les forêts : {1}|

## <a name="see-also"></a>Voir aussi
[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md)


