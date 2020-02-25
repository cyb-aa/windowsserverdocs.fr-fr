---
ms.assetid: 1ea2e1be-874f-4df3-bc9a-eb215002da91
title: Configurer la prise en charge AD FS pour l’authentification des certificats utilisateur
description: ''
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 6c8a3b30a337c164227bf344b5704cc7e782461a
ms.sourcegitcommit: 1c75e4b3f5895f9fa33efffd06822dca301d4835
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77517514"
---
# <a name="configuring-ad-fs-for-user-certificate-authentication"></a>Configuration de AD FS pour l’authentification par certificat utilisateur

L’authentification par certificat utilisateur est utilisée principalement dans deux cas d’utilisation
* Les utilisateurs utilisent des cartes à puce pour se connecter à leur système AD FS
* Les utilisateurs utilisent des certificats approvisionnés sur des appareils mobiles


## <a name="prerequisites"></a>Composants requis
1) Déterminez le mode de AD FS l’authentification par certificat utilisateur que vous souhaitez activer à l’aide de l’un des modes décrits dans [cet article](ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md) .
2) Assurez-vous que votre chaîne d’approbation des certificats utilisateur est installée & approuvée par tous les serveurs AD FS et WAP, y compris les autorités de certification intermédiaires. En général, cette opération est effectuée via un objet de stratégie de groupe sur des serveurs AD FS/WAP
3)  Assurez-vous que le certificat racine de la chaîne de confiance pour vos certificats utilisateur se trouve dans le magasin NTAuth dans Active Directory
4) Si vous utilisez AD FS en mode d’authentification par certificat de substitution, vérifiez que vos serveurs AD FS et WAP ont des certificats SSL qui contiennent le nom d’hôte AD FS préfixé avec « certauth », par exemple « certauth.fs.contoso.com », et que le trafic vers ce nom d’hôte est autorisé via le pare-feu
5) Si vous utilisez l’authentification par certificat à partir de l’extranet, assurez-vous qu’au moins un AIA et au moins un emplacement CDP ou OCSP de la liste spécifiée dans vos certificats est accessible à partir d’Internet.
6) De même, pour l’authentification par certificat de Azure AD, pour les clients Exchange ActiveSync, le certificat client doit avoir l’adresse de messagerie routable des utilisateurs dans Exchange Online, dans la valeur nom du principal ou nom RFC822 du champ autre nom de l’objet. (Azure Active Directory mappe la valeur RFC822 à l’attribut d’adresse de proxy dans le répertoire.)


## <a name="configure-ad-fs-for-user-certificate-authentication"></a>Configurer AD FS pour l'authentification des certificats utilisateur  

Activez l’authentification par certificat utilisateur comme méthode d’authentification intranet ou extranet dans AD FS, à l’aide de la console de gestion AD FS ou de l’applet de commande PowerShell `Set-AdfsGlobalAuthenticationPolicy`.

Si vous configurez AD FS pour Azure AD l’authentification par certificat, vérifiez que vous avez configuré les [paramètres de Azure ad](https://docs.microsoft.com/azure/active-directory/active-directory-certificate-based-authentication-get-started#step-2-configure-the-certificate-authorities) et les [règles de revendication AD FS requises](https://docs.microsoft.com/azure/active-directory/active-directory-certificate-based-authentication-ios#requirements) pour l’émetteur et le numéro de série du certificat.

En outre, il existe quelques aspects facultatifs.
- Si vous souhaitez utiliser des revendications basées sur les champs et les extensions de certificat en plus de l’utilisation améliorée de la valeur (type de revendication https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku), configurez des revendications supplémentaires via des règles sur l’approbation du fournisseur de revendications Active Directory.  Pour obtenir la liste complète des revendications de certificat disponibles, voir ci-dessous.  
- Si vous devez restreindre l’accès en fonction du type de certificat, vous pouvez utiliser les propriétés supplémentaires sur le certificat dans AD FS règles d’autorisation d’émission pour l’application. Les scénarios courants sont « autoriser uniquement les certificats approvisionnés par un fournisseur MDM » ou « autoriser uniquement les certificats de carte à puce ».
>[!IMPORTANT]
> Les clients qui utilisent le workflow de code d’appareil pour l’authentification et l’authentification des appareils à l’aide d’un fournisseur d’identité autre que Azure AD (par exemple AD FS) ne seront pas en mesure d’appliquer l’accès basé sur les appareils (par exemple, n’autoriser que les appareils gérés à l’aide d’un service MDM tiers) pour les ressources Azure AD. Pour protéger l’accès aux ressources de votre entreprise dans Azure AD et empêcher toute fuite de données, les clients doivent configurer Azure AD accès conditionnel en fonction de l’appareil (par exemple, « exiger que l’appareil soit marqué comme réclamation » pour accorder le contrôle dans Azure AD accès conditionnel).
- Configurez les autorités de certification émettrices autorisées pour les certificats clients en suivant les instructions de la section « gestion des émetteurs approuvés pour l’authentification du client » dans [cet article](https://technet.microsoft.com/library/dn786429(v=ws.11).aspx).
- Vous pouvez envisager de modifier les pages de connexion pour les adapter aux besoins de vos utilisateurs finaux lors de l’authentification par certificat. Les cas les plus courants sont le changement de la connexion avec votre certificat x509 à un utilisateur plus convivial

## <a name="configure-seamless-certificate-authentication-for-chrome-browser-on-windows-desktops"></a>Configurer l’authentification de certificat transparente pour le navigateur Chrome sur les ordinateurs de bureau Windows
Lorsque plusieurs certificats d’utilisateur (tels que des certificats Wi-Fi) sont présents sur l’ordinateur et satisfont aux objectifs de l’authentification du client, le navigateur Chrome sur Windows Desktop invite l’utilisateur à sélectionner le bon certificat. Cela peut prêter à confusion pour l’utilisateur final. Pour optimiser cette expérience, vous pouvez définir une stratégie de chrome pour sélectionner automatiquement le certificat approprié pour une meilleure expérience utilisateur. Cette stratégie peut être définie manuellement en effectuant une modification du registre ou une configuration automatique via un objet de stratégie de groupe (pour définir les clés de registre). Cela nécessite que les certificats de votre client utilisateur soient authentifiés pour que les AD FS aient des émetteurs distincts d’autres cas d’usage. 

Pour plus d’informations sur la configuration de ce pour Chrome, reportez-vous à ce [lien](http://www.chromium.org/administrators/policy-list-3#AutoSelectCertificateForUrls).  


## <a name="troubleshoot-certificate-authentication"></a>Résoudre les problèmes d’authentification de certificat
Ce document se concentre sur la résolution des problèmes courants lorsque AD FS est configuré pour l’authentification par certificat pour les utilisateurs. 

### <a name="check-if-certificate-trusted-issuers-is-configured-properly-in-all-the-ad-fswap-servers"></a>Vérifier si les émetteurs approuvés du certificat sont configurés correctement sur tous les serveurs AD FS/WAP
*Symptôme commun : HTTP 204 "aucun contenu à partir de https\://certauth.adfs.contoso.com"*

AD FS utilise le système d’exploitation Windows sous-jacent pour prouver la possession du certificat utilisateur et s’assurer qu’il correspond à un émetteur approuvé en procédant à la validation de la chaîne d’approbation de certificat. Pour correspondre à l’émetteur approuvé, vous devez vous assurer que toutes les autorités racine et intermédiaire sont configurées en tant qu’émetteurs approuvés dans le magasin autorités de certification de l’ordinateur local. Pour valider cette valeur automatiquement, utilisez l' [outil Analyseur de Diagnostic AD FS](https://adfshelp.microsoft.com/DiagnosticsAnalyzer/Analyze). L’outil interroge tous les serveurs et s’assure que les bons certificats sont configurés correctement. 
1)  Téléchargez et exécutez l’outil conformément aux instructions fournies dans le lien ci-dessus.
2)  Télécharger les résultats et passer en revue les échecs

### <a name="check-if-certificate-authentication-is-enabled-in-the-ad-fs-authentication-policy"></a>Vérifier si l’authentification par certificat est activée dans la stratégie d’authentification AD FS
AD FS n’active pas l’authentification par certificat par défaut. Reportez-vous au début de ce document pour savoir comment activer l’authentification par certificat. 

### <a name="check-if-certificate-authentication-is-enabled-in-the-ad-fs-authentication-policy"></a>Vérifier si l’authentification par certificat est activée dans la stratégie d’authentification AD FS
AD FS effectue l’authentification par certificat utilisateur par défaut sur le port 49443 avec le même nom d’hôte que AD FS (par exemple `adfs.contoso.com`). Vous pouvez également configurer AD FS pour utiliser le port 443 (port HTTPs par défaut) à l’aide de la liaison SSL alternative. Toutefois, l’URL utilisée dans cette configuration est `certauth.<adfs-farm-name>` (par exemple, `certauth.contoso.com`). Pour plus d’informations, consultez [ce lien](ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md) . Le cas le plus courant de la connectivité réseau est qu’un pare-feu n’a pas été correctement configuré et bloque ou interfère le trafic d’authentification des certificats utilisateur. En règle générale, un écran vide ou une erreur de serveur 500 s’affiche lorsque ce problème se produit. 
1)  Notez le nom d’hôte et le port que vous avez configurés dans AD FS
2)  Assurez-vous que tous les pare-feu devant AD FS ou le proxy d’application Web (WAP) sont configurés pour autoriser la combinaison `hostname:port` pour votre batterie de AD FS. Vous devez faire référence à votre ingénieur réseau pour effectuer cette étape. 

### <a name="check-certificate-revocation-list-connectivity"></a>Vérifier la connectivité de la liste de révocation de certificats
Les listes de révocation de certificats (CRL) sont des points de terminaison qui sont codés dans le certificat utilisateur pour effectuer des vérifications de révocation du Runtime. Par exemple, si un appareil qui contenait un certificat est volé, un administrateur peut ajouter le certificat à la liste des certificats révoqués. Tout point de terminaison qui acceptait ce certificat précédemment échouerait à présent à l’authentification.

Chaque AD FS et chaque serveur WAP doit atteindre le point de terminaison de la liste de révocation de certificats pour valider si le certificat qui lui a été présenté est toujours valide et n’a pas été révoqué. La validation de la liste de révocation de certificats peut être effectuée via HTTPs, HTTP, LDAP ou via OCSP (Online Certificate Status Protocol). Si AD FS/les serveurs WAP ne peuvent pas atteindre le point de terminaison, l’authentification échoue. Suivez les étapes ci-dessous pour le résoudre. 
1) Consultez votre ingénieur d’infrastructure à clé publique pour déterminer les points de terminaison de liste de révocation de certificats utilisés pour révoquer les certificats utilisateur de votre système PKI. 
2)  Sur chaque AD FS/serveur WAP, assurez-vous que les points de terminaison de la liste de révocation des certificats sont accessibles via le protocole utilisé (généralement HTTPs ou HTTP).
3)  Pour la validation avancée, [activez la journalisation des événements CAPI2](https://blogs.msdn.microsoft.com/benjaminperkins/2013/09/30/enable-capi2-event-logging-to-troubleshoot-pki-and-ssl-certificate-issues/) sur chaque AD FS/serveur WAP
4) Rechercher l’ID d’événement 41 (vérifier la révocation) dans les journaux des opérations CAPI2
5) Rechercher `‘\<Result value="80092013"\>The revocation function was unable to check revocation because the revocation server was offline.\</Result\>'`

***Conseil***: vous pouvez cibler un seul AD FS ou serveur WAP pour faciliter la résolution des problèmes en configurant la résolution DNS (fichier hosts sur Windows) pour qu’elle pointe vers un serveur spécifique. Cela vous permet d’activer le suivi ciblant un serveur. 

### <a name="check-if-this-is-a-server-name-indication-sni-issue"></a>Vérifier s’il s’agit d’un problème Indication du nom du serveur (SNI)
AD FS nécessite que l’appareil client (ou les navigateurs) et les équilibrages de charge prennent en charge SNI. Certains périphériques clients (généralement des versions antérieures d’Android) peuvent ne pas prendre en charge SNI. En outre, les équilibreurs de charge ne prennent peut-être pas en charge SNI ou n’ont pas été configurés pour SNI. Dans ces cas, vous risquez de voir des échecs de certification d’utilisateur. 
1)  Collaborez avec votre ingénieur réseau pour vous assurer que les Load Balancer pour AD FS/WAP prennent en charge SNI
2)  Dans le cas où SNI ne peut pas être pris en charge AD FS a une solution en suivant les étapes ci-dessous.
    *   Ouvrir une fenêtre d’invite de commandes avec élévation de privilèges sur le serveur de AD FS principal
    *   Tapez ```Netsh http show sslcert```
    *   Copier les « GUID de l’application » et « Certificate Hash » du service de Fédération
    *   Tapez `netsh http add sslcert ipport=0.0.0.0:{your_certauth_port} certhash={your_certhash} appid={your_applicaitonGUID}`

### <a name="check-if-the-client-device-has-been-provisioned-with-the-certificate-correctly"></a>Vérifiez si le périphérique client a été approvisionné correctement avec le certificat
Vous remarquerez peut-être que certains appareils fonctionnent correctement, mais pas les autres. Dans ce cas, cela est généralement dû au fait que le certificat utilisateur n’est pas configuré correctement sur l’appareil client. Suivez les étapes ci-dessous. 
1)  Si le problème est spécifique à un appareil Android, le problème le plus courant est que la chaîne de certificats n’est pas entièrement fiable sur l’appareil Android.  Reportez-vous à votre fournisseur MDM pour vérifier que le certificat a été configuré correctement et que la chaîne entière est entièrement fiable sur l’appareil Android. 
2)  Si le problème est spécifique à un appareil Windows, vérifiez si le certificat est configuré correctement en consultant le magasin de certificats Windows de l’utilisateur connecté (et non système/ordinateur).
3)  Exportez le certificat de l’utilisateur client dans le fichier. cer et exécutez la commande « certutil-f-urlfetch-Verify certificatefilename. cer ».


### <a name="check-if-the-tls-version-is-compatible-between-ad-fswap-servers-and-the-client-device"></a>Vérifier si la version TLS est compatible entre les serveurs AD FS/WAP et l’appareil client
Dans de rares cas, un périphérique client (généralement des appareils mobiles) est mis à jour pour prendre en charge uniquement une version plus récente de TLS (par exemple, 1,3) ou vous pouvez rencontrer le problème inverse où les AD FS/serveurs WAP ont été mis à jour pour utiliser uniquement une version TLS plus élevée et l’appareil client ne le prend pas en charge. Vous pouvez utiliser les outils SSL en ligne pour vérifier vos serveurs AD FS/WAP et voir s’ils sont compatibles avec l’appareil. Pour plus d’informations sur la façon de contrôler les versions TLS, consultez [ce lien](manage-ssl-protocols-in-ad-fs.md).

### <a name="check-if-azure-ad-promptloginbehavior-is-configured-correctly-on-your-federated-domain-settings"></a>Vérifiez si Azure AD PromptLoginBehavior est correctement configuré sur vos paramètres de domaine fédéré
De nombreuses applications Office 365 envoient prompt = Login à Azure AD. Azure AD, par défaut, le convertit en une nouvelle connexion de mot de passe à AD FS. Par conséquent, même si vous avez configuré l’authentification par certificat dans AD FS, vos utilisateurs finaux verront uniquement un mot de passe de connexion. 
1)  Récupération des paramètres du domaine fédéré à l’aide de la commande « MsolDomainFederationSettings »
2)  Vérifiez que le paramètre PromptLoginBehavior a la valeur « Disabled » ou « NativeSupport ».

Pour plus d’informations, consultez [ce lien](ad-fs-prompt-login.md). 

### <a name="additional-troubleshooting"></a>Résolution des problèmes supplémentaires
Il s’agit d’occurrences rares
1)  Si vos listes de révocation de certificats sont très longues, il se peut qu’elles atteignent un délai d’expiration lors de la tentative de téléchargement. Dans ce cas, vous devez mettre à jour le « MaxFieldLength » et le « MaxRequestByte » en fonction des https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows




## <a name="reference-complete-list-of-user-certificate-claim-types-and-example-values"></a>Référence : liste complète des types de revendications de certificat utilisateur et des exemples de valeurs

|                                         Type de la revendication.                                         |                              Exemple de valeur                               |
|--------------------------------------------------------------------------------------------|--------------------------------------------------------------------------|
|         https://schemas.microsoft.com/2012/12/certificatecontext/field/x509version         |                                    3                                     |
|     https://schemas.microsoft.com/2012/12/certificatecontext/field/signaturealgorithm      |                                sha256RSA                                 |
|           https://schemas.microsoft.com/2012/12/certificatecontext/field/issuer            |                 CN = EntCA, DC = domain, DC = contoso, DC = com                  |
|         https://schemas.microsoft.com/2012/12/certificatecontext/field/issuername          |                 CN = EntCA, DC = domain, DC = contoso, DC = com                  |
|          https://schemas.microsoft.com/2012/12/certificatecontext/field/notbefore          |                           12/05/2016 20:50:18                            |
|          https://schemas.microsoft.com/2012/12/certificatecontext/field/notafter           |                           12/05/2017 20:50:18                            |
|           https://schemas.microsoft.com/2012/12/certificatecontext/field/subject           |   E =user@contoso.com, CN = user, CN = Users, DC = domain, DC = contoso, DC = com   |
|         https://schemas.microsoft.com/2012/12/certificatecontext/field/subjectname         |   E =user@contoso.com, CN = user, CN = Users, DC = domain, DC = contoso, DC = com   |
|           https://schemas.microsoft.com/2012/12/certificatecontext/field/rawdata           |                {Données de certificat numérique encodées en base64}                 |
|        https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage         |                             DigitalSignature                             |
|        https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage         |                             KeyEncipherment                              |
|  https://schemas.microsoft.com/2012/12/certificatecontext/extension/subjectkeyidentifier   |                 9D11941EC06FACCCCB1B116B56AA97F3987D620A                 |
| https://schemas.microsoft.com/2012/12/certificatecontext/extension/authoritykeyidentifier  |    KeyID = D6 13 E3 6B BC E5 D8 15 52 0A FD 36 6A D5 0b 51 F3-25 7F     |
| https://schemas.microsoft.com/2012/12/certificatecontext/extension/certificatetemplatename |                                   Utilisateur                                   |
|           https://schemas.microsoft.com/2012/12/certificatecontext/extension/san           | Autre nom : nom principal =user@contoso.com, nom RFC822 =user@contoso.com |
|           https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku           |                          1.3.6.1.4.1.311.10.3.4                          |

## <a name="related-links"></a>Liens connexes
* [Configurer une autre liaison de nom d’hôte pour AD FS authentification par certificat](ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md)
* [Configurer des autorités de certification dans Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-certificate-based-authentication-get-started#step-2-configure-the-certificate-authorities)
