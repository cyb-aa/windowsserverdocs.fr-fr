---
ms.assetid: 1ea2e1be-874f-4df3-bc9a-eb215002da91
title: "Configurer la prise en charge ADFS pour l’authentification des certificats utilisateur"
description: 
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.service: active-directory
ms.technology: identity-adfs
ms.openlocfilehash: 9941d4dd997e857874aceddc920ec7f9d8944a81
ms.sourcegitcommit: d351cdbb0bf2533d6db35626ebbc4924b3834309
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/23/2018
---
# <a name="configuring-ad-fs-for-user-certificate-authentication"></a>La configuration ADFS pour l’authentification des certificats utilisateur

>S’applique à: Windows Server2016, Windows Server2012R2

ADFS peut être configuré pour x509 à l’aide d’un des modes d’authentification des certificats utilisateur décrit dans [cet article](ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md). Cette fonctionnalité peut être utilisée [avec Azure ActiveDirectory](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/) ou sur son propre pour permettre aux clients et périphériques est configuré avec des certificats d’utilisateur de l’accès ADFS ressources à partir de l’intranet ou l’extranet.

## <a name="prerequisites"></a>Conditions préalables
- Assurez-vous que vos certificats d’utilisateur sont approuvés par les serveurs de tous les services ADFS et WAP
- Assurez-vous que le certificat racine de la chaîne d’approbation pour vos certificats d’utilisateur est dans le magasin NTAuth dans ActiveDirectory
- Si vous utilisez ADFS dans le mode d’authentification autre certificat, assurez-vous que vos serveurs ADFS et WAP disposent des certificats SSL qui contiennent le nom d’hôte ADFS le préfixe «certauth», par exemple «certauth.fs.contoso.com», et que le trafic vers ce nom d’hôte est autorisé à travers le pare-feu
- Si l’authentification de certificat à partir de l’extranet, assurez-vous qu’au moins un AIA et au moins un CDP ou OCSP l’emplacement de la liste spécifiée dans vos certificats sont accessibles à partir d’internet.
- Si vous configurez les services ADFS pour l’authentification du certificat Azure AD, vérifiez que vous avez configuré le [paramètres Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-certificate-based-authentication-get-started#step-2-configure-the-certificate-authorities) et [requis de règles de revendications ADFS](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-certificate-based-authentication-ios#requirements) pour l’émetteur de certificat et le numéro de série
- Également pour l’authentification de certificat Azure AD, pour les clients Exchange ActiveSync, le client certificat doit avoir l’adresse de messagerie routable utilisateurs dans Exchange en ligne dans le nom de l’entité de sécurité ou la valeur de nom RFC822 du champ autre nom du sujet. (Azure ActiveDirectory mappe la valeur RFC822 à l’attribut Adresse Proxy dans le répertoire).

## <a name="configure-ad-fs-for-user-certificate-authentication"></a>Configurer ADFS pour l’authentification des certificats utilisateur  
- Activer l’authentification de certificat utilisateur comme méthode d’authentification extranet dans ADFS, à l’aide de la console de gestion ADFS ou l’applet de commande PowerShell Set-AdfsGlobalAuthenticationPolicy ou intranet
- Assurez-vous que l’ensemble de la chaîne d’approbation, y compris tous les certificats intermédiaires, est installé sur chaque serveur ADFS et WAP. Les certificats intermédiaires doivent être installés dans le magasin d’autorités de certification intermédiaires ordinateur local sur les serveurs de tous les services ADFS et WAP.
- Si vous souhaitez utiliser des revendications basées sur les champs des certificats et des extensions en plus EKU (type de revendication https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku), configurez pass revendication supplémentaires par le biais de règles sur l’approbation de fournisseur de revendications ActiveDirectory.  Voir ci-dessous pour obtenir la liste complète des demandes de certificats disponibles.  
- [Facultatif] Configurer des autorités de certification émettrice autorisées pour les certificats de client utilisant les instructions sous «Gestion des émetteurs approuvés pour l’authentification du client» dans [cet article](https://technet.microsoft.com/en-us/library/dn786429(v=ws.11).aspx).

## <a name="configure-seamless-certificate-authentication-for-chrome-browser-on-windows-desktops"></a>Configurer transparente l’authentification par certificat pour le navigateur Chrome sur les ordinateurs de bureau Windows
Lorsque plusieurs certificats d’utilisateur (par exemple, les certificats Wi-Fi) sont présents sur l’ordinateur qui répondent à des fins d’authentification de client, le navigateur Chrome sur le bureau Windows invite l’utilisateur de sélectionner le certificat approprié. Cela peut être déconcertant pour l’utilisateur final. Pour optimiser cette expérience, vous pouvez définir une stratégie pour Chrome pour la sélection automatique le bon certificat pour une meilleure expérience utilisateur. Cette stratégie peut être définie manuellement en apportant une modification du Registre ou configurée automatiquement via un GPO (pour définir les clés de Registre). Cela nécessite des certificats du client d’utilisateur pour l’authentification par rapport à ADFS pour que les émetteurs distincts à partir d’autres cas d’utilisation. 

Pour plus d’informations sur cette configuration pour Chrome, reportez-vous à cette [lien](http://www.chromium.org/administrators/policy-list-3#AutoSelectCertificateForUrls).  


## <a name="troubleshooting"></a>Résolution des problèmes
- Si les demandes de certificat d’authentification échouent avec une réponse «Contenu à partir de https://certauth.fs.contoso.com ne» HTTP 204, vérifiez que la racine et tous les certificats d’autorité de certification intermédiaires sont installés, respectivement, à la racine de confiance autorité de certification et l’autorité de certification intermédiaire de certificat magasins sur tous les serveurs de fédération.
- Si les demandes de certificat d’authentification échouent pour des raisons inconnues, exporter le certificat client vers un fichier .cer et exécutez la commande 

`certutil -f -urlfetch -verify certificatefilename.cer`

Assurez-vous de n’importe quel et delta résoudre des emplacements CRL.  Notez que les emplacements de révocation de certificats delta sont trouvés en fonction du contenu de la révocation des certificats.

## <a name="reference-complete-list-of-user-certificate-claim-types-and-example-values"></a>Référence: La liste complète des certificats d’utilisateur de revendication types et des exemples de valeurs

|Type de revendication|Exemple de valeur
|-----|-----
|https://schemas.microsoft.com/2012/12/certificatecontext/field/x509version | 3
|https://schemas.microsoft.com/2012/12/certificatecontext/field/signaturealgorithm | sha256RSA
|https://schemas.microsoft.com/2012/12/certificatecontext/field/issuer | CN = entca, DC = domain, DC = contoso, DC = com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/issuername | CN = entca, DC = domain, DC = contoso, DC = com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/notbefore | 12/05/2016 20:50:18
|https://schemas.microsoft.com/2012/12/certificatecontext/field/notafter | 12/05/2017 20:50:18
|https://schemas.microsoft.com/2012/12/certificatecontext/field/subject | E =user@contoso.com, CN = utilisateurs, CN = Users, DC = domain, DC = contoso, DC = com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/subjectname | E =user@contoso.com, CN = utilisateurs, CN = Users, DC = domain, DC = contoso, DC = com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/rawdata | {En Base64données certificat numérique}
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage | Bits DigitalSignature
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage | KeyEncipherment
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/subjectkeyidentifier | 9D11941EC06FACCCCB1B116B56AA97F3987D620A
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/authoritykeyidentifier | ID de la clé = d6 13 e3 6C BC et nbsp; e5 d8 15 52 0 a fd 36 6 a d5 0 b 51 f3 0 b 25 7f
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/certificatetemplatename | Utilisateur
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/san | Autre nom de Principal du nom: =user@contoso.com, nom RFC822 =user@contoso.com
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku | 1.3.6.1.4.1.311.10.3.4


