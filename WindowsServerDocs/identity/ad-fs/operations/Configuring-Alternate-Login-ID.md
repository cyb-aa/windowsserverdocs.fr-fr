---
ms.assetid: f0cbdd78-f5ae-47ff-b5d3-96faf4940f4a
title: La configuration des ID de connexion alternatif
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 12/04/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fb4c98ff1090a9d3c35654614a43e12db99d691d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="configuring-alternate-login-id"></a>La configuration des ID de connexion alternatif

>S’applique à: Windows Server2016, Windows Server2012R2

Applications d’ActiveDirectory Federation Services (ADFS) est activé à l’aide de n’importe quel écran de l’identificateur d’utilisateur qui est acceptée par les Services de domaine ActiveDirectory (ADDS), les utilisateurs peuvent se connecter. Ceux-ci incluent des noms d’utilisateur Principal (UPN) (johndoe@contoso.com) ou de domaine complet des noms de compte sam (contoso\johndoe ou contoso.com\johndoe #).

Dans certains environnements, en raison de la stratégie d’entreprise ou de dépendances d’applications métier de locale, les utilisateurs finaux peut uniquement être conscient de leur courrier électronique adresse et pas leur UPN ou sam nom de compte. Dans certains cas, l’UPN est également non routable (jdoe@contoso.local) et est utilisé uniquement pour l’authentification dans des applications sur le réseau d’entreprise.

Depuis des domaines non routable (par exemple). Propriété contoso.local) ne peut pas être vérifiée, Office 365 requiert une connexion utilisateur tous les ID pour être entièrement routable internet. Si le nom UPN local utilise un domaine non routable (par exemple). Contoso.local), ou l’UPN existant ne peut être modifié en raison des dépendances d’application locale, nous vous recommandons de configurer des ID de connexion de substitution. ID de connexion alternatif permet de configurer un journal de l’expérience où les utilisateurs peuvent se connecter avec un attribut autre que leur nom d’utilisateur principal, telles que courrier.

Un des avantages de cette fonctionnalité est qu’elle vous permet d’adopter des fournisseurs de SaaS, comme Office 365sans modifier votre UPN local. Il vous permet également de prendre en charge des applications métier de service avec des identités grand public dynamiquement.

> [!IMPORTANT]
> À l’aide d’un autre ID dans les environnements hybrides avec Exchange ou Skype entreprise est pris en charge mais pas recommandé. Le même jeu d’informations d’identification (par exemple, le nom d’utilisateur principal) sur site et en ligne fournit la meilleure expérience utilisateur dans un environnement hybride.  Microsoft recommande de clients modifier leurs noms UPN si possible pour éviter la nécessité d’ID de remplacement. À l’aide d’un autre ID avec Lync ou Skype entreprise requiert Lync Server2013 ou version ultérieure. Les clients qui utilisent un autre ID doivent prendre en compte l’activation [l’authentification moderne](https://support.office.com/article/using-office-365-modern-authentication-with-office-clients-776c0036-66fd-41cb-8928-5495c0f9168a) pour Exchange dans Office 365 pour une expérience utilisateur améliorée. En outre, les clients à l’aide de Skype entreprise avec des clients mobiles doivent vous assurer que l’adresse SIP est identique à l’utilisateur mail adresse (et un autre ID). 

Reportez-vous au tableau ci-dessous pour une expérience utilisateur avec un autre ID à l’aide de différents clients Office 365 avec certificat d’authentification (nécessite l’activation de l’authentification moderne), l’authentification moderne et d’authentification standard.

|**Types de clients**|**Informations supplémentaires**|**Prise en charge de la déclaration - authentification régulière et moderne**|**Description**|
|--------------------|------------------------------|------------------------------|------------------------------|
|Outlook|Authentification standard: Vous devez se trouver sur un ordinateur membre d’un domaine et connecté au réseau d’entreprise<br /><br />L’authentification moderne: pris en charge|Vous pouvez uniquement utiliser un autre ID dans les environnements qui ne permettent pas d’un accès externe pour les utilisateurs de boîte aux lettres. Cela signifie que les utilisateurs peuvent uniquement s’authentifier à y boîte aux lettres de manière pris en charge lorsqu’elles sont connectées et joint au réseau d’entreprise, sur un réseau privé virtuel ou via un accès Direct. Si vous choisissez de configurer l’authentification moderne (appelé ADAL), vous pouvez utiliser Outlook à partir d’un domaine joint ou connecté ordinateurs, mais vous obtenez deux invites supplémentaires lors de la configuration de votre profil Outlook.<br /><br />Reportez-vous à la première image au-dessous de la table pour une démonstration d’expérience utilisateur.|[Authentification moderne dans Office2013](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/)|
|Dossiers publics hybride|L’authentification standard: Pas pris en charge<br /><br />L’authentification moderne: pris en charge|Hybride des dossiers publics ne sera pas en mesure d’étendre si un autre ID sont utilisé et doit donc pas être utilisé aujourd'hui avec les méthodes d’authentification standard. Si vous voulez être en mesure d’utiliser des dossiers publics dans hybride, vous devez configurer l’authentification moderne (appelé ADAL).<br /><br />Reportez-vous à la première image au-dessous de la table pour une démonstration d’expérience utilisateur.|[Authentification moderne dans Office2013](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/)|
|Cross-site délégation|Non pris en charge|Actuellement cross-site les autorisations ne sont pas pris en charge dans une configuration hybride, mais ils également ne fonctionnera pas si vous utilisez AltID.||
|Accès aux boîtes aux lettres d’archives (boîte aux lettres local - archive dans le cloud)|Prise en charge|Les utilisateurs reçoivent une invite supplémentaire pour les informations d’identification lors de l’accès de l’archive, il devra fournir il autre numéro d’identification lorsque vous y êtes invité.<br /><br />Reportez-vous à la première image au-dessous de la table pour une démonstration d’expérience utilisateur.||
|Page d’activation Office 365 Pro Plus|Prise en charge - recommandé de clé de Registre côté client|Avec un autre ID configuré, vous verrez que le nom UPN local est prédéfinie dans le champ de vérification. Cela doit être modifié à l’autre identité qui est utilisée. Nous vous recommandons d’utiliser la clé de Registre côté client indiquée dans la colonne de lien.<br /><br />Voir la seconde image au-dessous de la table pour une démonstration d’expérience utilisateur.|[Office2013 et Lync2013 régulièrement demander les informations d’identification à SharePoint Online, OneDrive et Lync Online](https://support.microsoft.com/en-us/kb/2913639)|
|Équipes de Microsoft|Prise en charge|Teams Microsoft prend en charge ADFS (SAML-P, WS-chargées, WS-Trust et OAuth) et l’authentification moderne.<br/><br/> Core Teams Microsoft tels que les fonctionnalités de canaux, des conversations et des fichiers ne fonctionne pas avec les ID de connexion Altnernate. </br></br>Applications tierces 1eret 3edoivent être traités séparément par le client. Il s’agit, car chaque application a ses propres protocoles d’authentification prise en charge.| 
|Skype entreprise / Lync|Prise en charge (sauf exception notée), mais il existe un risque de confusion de l’utilisateur.  Sur les clients mobiles, un autre Id est pris en charge uniquement si adresse SIP = adresse de messagerie = code de remplacement.|Les utilisateurs devront peut-être connectez-vous deux fois pour le Skype pour Business desktop client, à l’aide de l’UPN local, puis en utilisant l’ID autre. (Notez que l’adresse «connexion» est réellement l’adresse SIP qui ne peut pas être le même comme «nom d’utilisateur», mais est souvent).  Lorsque tout d’abord invité à entrer un nom d’utilisateur, l’utilisateur doit entrer l’UPN, même si elle est prédéfinie incorrectement avec l’adresse d’un autre ID ou SIP. Une fois que l’utilisateur clique sur Connectez-vous avec le nom d’utilisateur principal, l’utilisateur réapparaît invite du nom, cette fois prérempli avec le nom d’utilisateur principal. Cette fois, l’utilisateur doit remplacer par l’autre ID et cliquez sur se connecter pour terminer la connexion dans le processus. Sur les clients mobiles, les utilisateurs doivent entrer l’ID d’utilisateur local dans la page Paramètres avancés, à l’aide du style de SAM (DOMAINE\nom_utilisateur), non format UPN.<br /><br />Après la connexion réussie, si Skype pour les entreprises ou Lync indique «Exchange a besoin de vos informations d’identification», vous devez fournir les informations d’identification qui sont valides pour où se trouve la boîte aux lettres. Si la boîte aux lettres est dans le cloud, que vous devez fournir l’ID autre. Si la boîte aux lettres est local, vous devez fournir le nom UPN local.|[Authentification moderne dans Office2013](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/)|
|Outlook Web Access|Prise en charge|||
|Applications mobiles Outlook pour Android, IOS et Windows Phone|Prise en charge|||
|OneDrive entreprise|Prise en charge - recommandé de clé de Registre côté client|Avec un autre ID configuré, vous verrez que le nom UPN local est prédéfinie dans le champ de vérification. Cela doit être modifié à l’autre identité qui est utilisée. Nous vous recommandons d’utiliser la clé de Registre côté client indiquée dans la colonne de lien.<br /><br />Voir la seconde image au-dessous de la table pour une démonstration d’expérience utilisateur.|[Office2013 et Lync2013 régulièrement demander les informations d’identification à SharePoint Online, OneDrive et Lync Online](https://support.microsoft.com/en-us/kb/2913639)|
|OneDrive pour le Client Mobile d’entreprise|Prise en charge|||

![Connexion de substitution](media/Configure-Alternate-Login-ID/ADFS_Alt_ID1.png)

![Connexion de substitution](media/Configure-Alternate-Login-ID/ADFS_Alt_ID2.png)

![Connexion de substitution](media/Configure-Alternate-Login-ID/ADFS_Alt_ID3.png)

Ci-dessous, les captures d’écran suivantes sont un exemple supplémentaire à l’aide de Skype entreprise.  Dans l’exemple les informations suivantes sont utilisées


- SIP:userA@contoso.com 
- NOM D’UTILISATEUR PRINCIPAL:userA@contoso.local
- Messagerie:userA@contoso.com
- AltId:userA@contoso.com 

Entrez l’adresse SIP dans le champ de connexion.

![Skype](media/Configure-Alternate-Login-ID/SkypeA.png)

![Skype](media/Configure-Alternate-Login-ID/SkypeB.png)

![Skype](media/Configure-Alternate-Login-ID/SkypeC.png)

## <a name="to-configure-alternate-login-id"></a>Pour configurer l’ID de connexion alternatif
Pour configurer un ID de connexion alternatif, vous devez effectuer les tâches suivantes:

Configurer vos approbations de fournisseur de revendications ADFS pour activer l’ID de connexion alternatif

1.  Installer [KB2919355](https://go.microsoft.com/fwlink/?LinkID=396590).  Vous pouvez obtenir via les Services de mise à jour de Windows ou le télécharger directement.

2.  Mettre à jour la configuration ADFS en exécutant l’applet de commande PowerShell suivante sur un des serveurs de fédération de votre batterie de serveurs (si vous disposez d’une batterie WID, vous devez exécuter cette commande sur le serveur ADFS principal dans votre batterie de serveurs):

    ```
    Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID <attribute> -LookupForests <forest domain>

    ```

    **AlternateLoginID** est le nom de l’attribut que vous souhaitez utiliser pour la connexion LDAP.

    **LookupForests** est la liste de forêt DNS appartenant à vos utilisateurs.

    Pour activer la fonctionnalité d’ID de connexion de substitution, vous devez configurer les paramètres AlternateLoginID - et - LookupForests avec une valeur non null, valide.

    Dans l’exemple suivant, vous activez le fonctionnalité ID de connexion de substitution telles que vos utilisateurs avec des comptes dans des forêts contoso.com et fabrikam.com peuvent vous connecter à des applications ActiveDirectory Federation Services-dont l’attribut de «courrier».

    ```
    Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID mail -LookupForests contoso.com,fabrikam.com
    ```

3.  Pour désactiver cette fonctionnalité, définissez la valeur pour les deux paramètres être null.

    ```
    Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID $NULL -LookupForests $NULL
    ```

4.  Pour activer l’ID de connexion alternatif avec Azure AD, aucune étape de configurations supplémentaires n’est nécessaires lorsque vous utilisez Azure AD Connect.   ID de remplacement peut être configuré directement à partir de l’Assistant.  Consultez l’identifiant de vos utilisateurs sous la section [se connecter à Azure AD](https://azure.microsoft.com/en-us/documentation/articles/active-directory-aadconnect-get-started-custom/#connect-to-azure-ad).

## <a name="additional-details--considerations"></a>Considérations et des détails supplémentaires

-   La fonctionnalité d’ID de connexion de substitution est uniquement disponible pour les environnements fédérés avec ADFS déployé.  Il n’est pas pris en charge dans les scénarios suivants:
    -   Non routable domaines (par exemple: Contoso.local) qui ne peut pas être vérifiés par Azure AD.
    -   Géré environnements qui n’ont pas ADFS déployé.


-   Lorsque activé, la fonctionnalité d’ID de connexion de substitution est uniquement disponible pour l’authentification du nom d’utilisateur/mot de passe sur tous les protocoles d’authentification de nom dans le mot de passe d’utilisateur pris en charge par ADFS (SAML-P, WS-chargées, WS-Trust et OAuth).


-   Lorsque l’authentification intégrée de Windows (WIA) est effectuée (par exemple, quand utilisateurs essaient d’accéder à une application sur un ordinateur joint au domaine d’entreprise à partir de l’intranet et administrateur ADFS a configuré la stratégie d’authentification pour utiliser WIA pour l’intranet), nom d’utilisateur principal sera utilisé pour l’authentification. Si vous avez configuré les règles de revendication pour les parties de confiance pour la fonctionnalité d’ID de connexion de substitution, vous devez vous assurer que ces règles sont toujours valides dans le cas de WIA.

-   Lorsque activé, la fonctionnalité d’ID de connexion de substitution nécessite au moins un serveur de catalogue global afin d’être accessible à partir du serveur ADFS pour chaque forêt de comptes d’utilisateur ADFS prend en charge. Échec d’atteindre le serveur de catalogue global dans la forêt de comptes d’utilisateur entraîne dans ADFS revenant pour utiliser le nom d’utilisateur principal. Par défaut, tous les contrôleurs de domaine sont des serveurs de catalogue global.

-   Si activé, si le serveur ADFS détecte plusieurs objets utilisateur avec la même valeur d’ID de connexion alternatif spécifiée sur toutes les forêts de comptes d’utilisateur configuré, elle échoue à la connexion.

-   Lorsque la fonctionnalité d’ID de connexion de substitution est activée, ADFS tentent de s’authentifier l’utilisateur final avec les ID de connexion alternatif tout d’abord et puis revenir pour utiliser le nom d’utilisateur principal si elle ne peut pas trouver un compte qui peut être identifié par l’ID de connexion de substitution. Vous devez vous assurer qu’aucun les conflits entre le ID de connexion alternatif et l’UPN si vous souhaitez toujours prendre en charge l’ouverture de session UPN. Par exemple, l’attribut de paramètre de messagerie avec l’autre nom d’utilisateur principal bloque les autres utilisateurs de se connecter avec son nom d’utilisateur principal.

-   Si une des forêts qui est configuré par l’administrateur est en panne, ADFS continuera à rechercher le compte d’utilisateur avec l’ID de connexion de substitution dans d’autres forêts qui sont configurés. Si le serveur ADFS détecte un utilisateur unique d’objets entre les forêts qui lui a recherché, un utilisateur se connecteront avec succès.

-   Vous voudrez peut-être plus personnaliser la page de connexion ADFS pour donner aux utilisateurs finaux des information sur l’ID de connexion de substitution. Vous pouvez le faire en ajoutant la description de la page de connexion personnalisée (pour plus d’informations, voir [personnalisation des Pages ADFS Sign-in](https://technet.microsoft.com/library/dn280950.aspx) ou personnaliser la chaîne de «ouvrir une session avec un compte d’organisation» ci-dessus champ de nom d’utilisateur (pour plus d’informations, voir [Advanced Customization of ADFS Sign-in Pages](https://technet.microsoft.com/library/dn636121.aspx).

-   Le nouveau type de revendication qui contient la valeur d’ID de connexion de substitution est **http:schemas.microsoft.com/ws/2013/11/alternateloginid**

## <a name="events-and-performance-counters"></a>Événements et les compteurs de performances
Les compteurs de performances suivants ont été ajoutés pour mesurer les performances des serveurs ADFS lors de l’ID de connexion alternatif est activé:

-   Autre authentifications d’Id de connexion: nombre d’authentifications effectuée à l’aide d’ID de connexion alternatif

-   Autre Id de connexion authentifications/s: le nombre d’authentifications effectuée à l’aide d’ID de connexion alternatif par seconde

-   Moyenne de latence de recherche pour un autre ID de connexion: latence de recherche de moyenne entre les forêts un administrateur a configuré pour l’ID de connexion alternatif

Voici les différents cas d’erreur et impact correspondant sur l’expérience de l’utilisateur de connexion avec les événements journalisés par ADFS:



**Cas d’erreur**|**Impact sur l’expérience de connexion**|**Événement**|
---------|---------|---------
Impossible d’obtenir une valeur pour SAMAccountName de l’objet utilisateur|Échec de connexion|ID d’événement 364 avec le message d’exception MSIS8012: Impossible de trouver samAccountName de l’utilisateur: {0}.|
L’attribut CanonicalName n’est pas accessible|Échec de connexion|ID d’événement 364 avec le message d’exception MSIS8013: CanonicalName: '{0}' de l’utilisateur: '{1}' est dans un format incorrect.|
Plusieurs objets utilisateur sont trouvent dans une forêts|Échec de connexion|ID d’événement 364 avec le message d’exception MSIS8015: trouvé plusieurs comptes d’utilisateur avec l’identité '{0}' dans la forêt «{1}» avec des identités: {2}|
Plusieurs objets utilisateur sont trouvent dans plusieurs forêts|Échec de connexion|ID d’événement 364 avec le message d’exception MSIS8014: trouvé plusieurs comptes d’utilisateur avec l’identité '{0}' dans des forêts: {1}|

## <a name="see-also"></a>Voir aussi
[Opérations ADFS](../../ad-fs/AD-FS-2016-Operations.md)


