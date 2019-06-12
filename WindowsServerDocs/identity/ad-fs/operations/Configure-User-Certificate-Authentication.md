---
ms.assetid: 1ea2e1be-874f-4df3-bc9a-eb215002da91
title: Configurer la prise en charge AD FS pour l’authentification de certificat utilisateur
description: ''
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c69192a4223379b896a57eb04a38e37863c1366e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444310"
---
# <a name="configuring-ad-fs-for-user-certificate-authentication"></a>Configuration d’AD FS pour l’authentification de certificat utilisateur


AD FS peut être configurée pour x509 à l’aide d’un des modes d’authentification des certificats utilisateur décrit dans [cet article](ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md). Cette fonctionnalité peut être utilisée [avec Azure Active Directory](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/) ou sur sa propre pour activer les clients et appareils mis en service avec des certificats d’utilisateur pour l’accès AD FS ressources à partir de l’intranet ou l’extranet.

## <a name="prerequisites"></a>Prérequis
- Assurez-vous que vos certificats d’utilisateur sont approuvés par les serveurs de tous les services AD FS et WAP
- Vérifiez que le certificat racine de la chaîne d’approbation pour vos certificats de l’utilisateur est dans le magasin NTAuth dans Active Directory
- Si vous utilisez AD FS en mode d’authentification de certificat de remplacement, assurez-vous que vos serveurs AD FS et WAP possèdent des certificats SSL qui contiennent le nom d’hôte AD FS précédé de « certauth », par exemple « certauth.fs.contoso.com », et que le trafic vers ce nom d’hôte est autorisé via le pare-feu
- Si vous utilisez l’authentification par certificat à partir de l’extranet, assurez-vous que AIA au moins un et l’emplacement d’au moins un CDP ou OCSP à partir de la liste spécifiée dans vos certificats sont accessibles à partir d’internet.
- Si vous configurez les services AD FS pour l’authentification de certificat Azure AD, vérifiez que vous avez configuré le [paramètres Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-certificate-based-authentication-get-started#step-2-configure-the-certificate-authorities) et [requis de règles de revendication AD FS](https://docs.microsoft.com/azure/active-directory/active-directory-certificate-based-authentication-ios#requirements) pour le certificat de l’émetteur et numéro de série
- Également pour l’authentification de certificat Azure AD, pour les clients Exchange ActiveSync, le certificat client doit avoir l’adresse de messagerie routable utilisateurs dans Exchange online dans le nom de Principal ou la valeur nom RFC822 du champ autre nom du sujet. (Azure Active Directory mappe la valeur RFC822 à l’attribut Adresse Proxy dans le répertoire).

## <a name="configure-ad-fs-for-user-certificate-authentication"></a>Configurer AD FS pour l'authentification des certificats utilisateur  
- Activer l’authentification des certificats utilisateur en tant qu’un intranet ou d’une méthode d’authentification extranet dans AD FS, à l’aide de la console de gestion AD FS ou l’applet de commande PowerShell Set-AdfsGlobalAuthenticationPolicy
- Vérifiez que toute la chaîne d’approbation, y compris tous les certificats intermédiaires, est installée sur chaque serveur AD FS et WAP. Les certificats intermédiaires doivent être installés dans le magasin d’autorités de certification intermédiaires ordinateur local sur les serveurs de tous les services AD FS et WAP.
- Si vous souhaitez utiliser des revendications basées sur les champs des certificats et des extensions en plus EKU (type de revendication https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku), configurer pass revendication supplémentaires via des règles sur l’approbation de fournisseur de revendications Active Directory.  Voir ci-dessous pour obtenir la liste complète des revendications de certificat disponible.  
- [Facultatif] Configurer des autorités de certification émettrice autorisées pour les certificats de client en utilisant les instructions sous « Gestion des émetteurs approuvés pour l’authentification client » dans [cet article](https://technet.microsoft.com/library/dn786429(v=ws.11).aspx).

## <a name="configure-seamless-certificate-authentication-for-chrome-browser-on-windows-desktops"></a>Configurer transparente l’authentification par certificat pour le navigateur Chrome sur les ordinateurs de bureau Windows
Lorsque plusieurs certificats d’utilisateur (par exemple, de certificats Wi-Fi) sont présents sur l’ordinateur qui répondent à des fins d’authentification du client, le navigateur Chrome sur le bureau de Windows invitera l’utilisateur à sélectionner le certificat approprié. Cela peut prêter à confusion pour l’utilisateur final. Pour optimiser cette expérience, vous pouvez définir une stratégie pour Chrome pour le certificat approprié pour une meilleure expérience utilisateur de sélection automatique. Cette stratégie peut être définie manuellement en apportant une modification de Registre ou configurée automatiquement via un GPO (pour définir les clés de Registre). Cela requiert vos certificats de client utilisateur pour l’authentification auprès d’AD FS pour que les émetteurs distinctes à partir d’autres cas d’utilisation. 

Pour plus d’informations sur cette configuration pour Chrome, reportez-vous à ce [lien](http://www.chromium.org/administrators/policy-list-3#AutoSelectCertificateForUrls).  


## <a name="troubleshooting"></a>Résolution des problèmes
- Si les demandes d’authentification de certificat échouent avec un HTTP 204 « aucun contenu à partir de https :\//certauth.fs.contoso.com « réponse, vérifiez que la racine et les certificats d’autorité de certification intermédiaires sont installés, respectivement, à l’autorité de certification racine approuvée et certificat d’autorité de certification intermédiaire stocke sur tous les serveurs de fédération.
- Si des demandes de certificat d’authentification échouent pour une raison inconnue, exporter le certificat client vers un fichier .cer et exécutez la commande 

`certutil -f -urlfetch -verify certificatefilename.cer`

Vérifiez que tout et résolution des emplacements CRL de delta.  Notez que les emplacements de CRL delta sont trouvent en fonction du contenu de la liste CRL de Base.

## <a name="reference-complete-list-of-user-certificate-claim-types-and-example-values"></a>Référence : La liste complète des certificat utilisateur types de revendication et exemples de valeurs

|                                         Type de revendication                                         |                              Exemple de valeur                               |
|--------------------------------------------------------------------------------------------|--------------------------------------------------------------------------|
|         https://schemas.microsoft.com/2012/12/certificatecontext/field/x509version         |                                    3                                     |
|     https://schemas.microsoft.com/2012/12/certificatecontext/field/signaturealgorithm      |                                sha256RSA                                 |
|           https://schemas.microsoft.com/2012/12/certificatecontext/field/issuer            |                 CN=entca, DC=domain, DC=contoso, DC=com                  |
|         https://schemas.microsoft.com/2012/12/certificatecontext/field/issuername          |                 CN=entca, DC=domain, DC=contoso, DC=com                  |
|          https://schemas.microsoft.com/2012/12/certificatecontext/field/notbefore          |                           12/05/2016 20:50:18                            |
|          https://schemas.microsoft.com/2012/12/certificatecontext/field/notafter           |                           12/05/2017 20:50:18                            |
|           https://schemas.microsoft.com/2012/12/certificatecontext/field/subject           |   E =user@contoso.com, CN = utilisateurs, CN = Users, DC = domain, DC = contoso, DC = com   |
|         https://schemas.microsoft.com/2012/12/certificatecontext/field/subjectname         |   E =user@contoso.com, CN = utilisateurs, CN = Users, DC = domain, DC = contoso, DC = com   |
|           https://schemas.microsoft.com/2012/12/certificatecontext/field/rawdata           |                {En Base64 des données de certificat numérique}                 |
|        https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage         |                             DigitalSignature                             |
|        https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage         |                             KeyEncipherment                              |
|  https://schemas.microsoft.com/2012/12/certificatecontext/extension/subjectkeyidentifier   |                 9D11941EC06FACCCCB1B116B56AA97F3987D620A                 |
| https://schemas.microsoft.com/2012/12/certificatecontext/extension/authoritykeyidentifier  |    KeyID=d6 13 e3 6b bc e5 d8 15 52 0a fd 36 6a d5 0b 51 f3 0b 25 7f     |
| https://schemas.microsoft.com/2012/12/certificatecontext/extension/certificatetemplatename |                                   Utilisateur                                   |
|           https://schemas.microsoft.com/2012/12/certificatecontext/extension/san           | Autre nom de nom : Principal =user@contoso.com, nom RFC822 =user@contoso.com |
|           https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku           |                          1.3.6.1.4.1.311.10.3.4                          |

