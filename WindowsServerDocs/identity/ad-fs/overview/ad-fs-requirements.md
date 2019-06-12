---
ms.assetid: 28f4a518-1341-4a10-8a4e-5f84625b314b
title: Configuration requise des services AD FS
description: Configuration requise pour installer les Services de fédération Active Directory.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/06/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e62032533b15ec3d93896d242273612faafdca58
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444104"
---
# <a name="ad-fs-requirements"></a>Configuration AD FS requise



Voici la configuration requise pour le déploiement d’AD FS :  
  
-   [Conditions requises pour les certificats](ad-fs-requirements.md#BKMK_1)  
  
-   [Configuration matérielle requise](ad-fs-requirements.md#BKMK_2)  
  
-   [Conditions du proxy](ad-fs-requirements.md#BKMK_3)  
  
-   [Requise pour AD DS](ad-fs-requirements.md#BKMK_4)  
  
-   [Exigences de base de données de configuration](ad-fs-requirements.md#BKMK_5)  
  
-   [Configuration requise du navigateur](ad-fs-requirements.md#BKMK_6)  

-   [Configuration réseau requise](ad-fs-requirements.md#BKMK_7)  
  
-   [Exigences relatives aux autorisations](ad-fs-requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>Configuration requise des certificats  
  
### <a name="ssl-certificates"></a>Certificats SSL

Chaque serveur AD FS et Proxy d’Application Web a un certificat SSL pour traiter les demandes HTTPS pour le service de fédération.  Le Proxy d’Application Web peut avoir des certificats SSL supplémentaires pour traiter les demandes aux applications publiées.

**Recommandation :** Utiliser le même certificat SSL pour tous les serveurs de fédération AD FS et proxys d’Application Web. 

**Configuration requise :**

Certificats SSL sur les serveurs de fédération doivent remplir les conditions suivantes
- Certificat est approuvé publiquement (pour les déploiements de production)
- Certificat contient la valeur de l’utilisation de clé améliorée (EKU, Distributed File System) de serveur d’authentification
- Certificat contient le nom de service de fédération, tel que « fs.contoso.com » dans l’objet ou le nom SAN (Subject Alternative)
- Pour l’authentification des certificats utilisateur sur le port 443, le certificat contient « certauth. \<nom de service de fédération\>», tels que « certauth.fs.contoso.com » dans le réseau SAN
- Pour inscrire un appareil ou pour l’authentification moderne pour les ressources locales à l’aide de clients de versions antérieures de Windows 10, le réseau SAN doit contenir « enterpriseregistration. \<suffixe upn\>« pour chaque suffixe UPN en cours d’utilisation dans votre organisation.

Certificats SSL sur le Proxy d’Application Web doivent remplir les conditions suivantes
- Si le proxy est utilisé pour les demandes de proxy AD FS qui utilisent l’authentification Windows intégrée, le certificat SSL de proxy doivent être le même (utiliser la même clé) que le certificat SSL de serveur de fédération
- Si la propriété AD FS « ExtendedProtectionTokenCheck » est activé (le paramètre par défaut dans AD FS), le certificat SSL de proxy doit être le même (utiliser la même clé) que le certificat SSL de serveur de fédération
- Sinon, la configuration requise pour le certificat SSL de proxy est les mêmes que celles pour le certificat SSL de serveur de fédération

### <a name="service-communication-certificate"></a>Certificat de Communication du service
Ce certificat n’est pas obligatoire pour la plupart des scénarios AD FS, y compris Azure AD et Office 365. Par défaut, AD FS configure le certificat SSL fourni lors de la configuration initiale en tant que le certificat de communication du service.

**Recommandation :**
- Utiliser le même certificat que vous utilisez pour SSL.  

### <a name="token-signing-certificate"></a>Certificat de signature de jetons
Ce certificat est utilisé pour signer les jetons émis pour les parties de confiance, afin d’applications de confiance doivent reconnaître le certificat et elle est associée la clé en tant que connu et approuvé. Lorsque la signature certificat modifications apportées aux jetons, tels que lorsqu’il expire et que vous configurez un nouveau certificat, toutes les parties de confiance doivent être mis à jour.

**Recommandation :** Utilisez la valeur par défaut d’AD FS, jeton généré en interne, auto-signé certificats de signature.  

**Configuration requise :**
- Si votre organisation exige que les certificats à partir de l’infrastructure à clé publique de l’entreprise soit utilisée pour la signature de jetons, cela est possible en utilisant le paramètre SigningCertificateThumbprint de l’applet de commande Install-AdfsFarm.
- Si vous utilisez les certificats par défaut généré de manière interne ou inscrits en externe des certificats, lorsque le certificat de signature de jetons est modifié, vous doivent vous assurer que toutes les parties de confiance sont mis à jour avec les nouvelles informations de certificat.  Sinon, les connexions à des parties de confiance ne pas mis à jour risque d’échouer.

### <a name="token-encryptingdecrypting-certificate"></a>Certificat de chiffrement/déchiffrement de jetons
Ce certificat est utilisé par les fournisseurs de revendications qui chiffrement les jetons émis pour AD FS.

**Recommandation :** Utilisez la par défaut d’AD FS, le jeton généré en interne, auto-signé certificats de déchiffrement.  

**Configuration requise :**
- Si votre organisation exige que les certificats à partir de l’infrastructure à clé publique de l’entreprise soit utilisée pour la signature de jetons, cela est possible en utilisant le paramètre DecryptingCertificateThumbprint de l’applet de commande Install-AdfsFarm.
- Si vous utilisez les certificats par défaut généré de manière interne ou inscrits en externe des certificats, lorsque le certificat de déchiffrement de jeton est modifié, vous doivent vérifier que tous les fournisseurs de revendications sont mis à jour avec les nouvelles informations de certificat.  Sinon, les ouvertures de session à l’aide d’une des revendications fournisseurs ne pas mis à jour risque d’échouer.
  
> [!CAUTION]  
> Les certificats sont utilisés pour le jeton\-signature et le jeton\-déchiffrement\/chiffrer sont essentielles à la stabilité du Service de fédération. Les clients qui gèrent leur propre jeton\-signature & jeton\-déchiffrement\/certificats de chiffrement doit garantir que ces certificats sont sauvegardés et ne sont disponibles indépendamment pendant un événement de récupération.  

### <a name="user-certificates"></a>Certificats utilisateur
- Lorsque vous utilisez x509 authentification des certificats utilisateur avec AD FS, tous les certificats de l’utilisateur doit être lié à une autorité de certification racine approuvée par les serveurs AD FS et Proxy d’Application Web.

## <a name="BKMK_2"></a>Configuration matérielle requise  
AD FS et Proxy d’Application Web configuration matérielle requise (physique ou virtuel) est contrôlés sur des UC, donc vous devez déterminer la taille votre batterie de serveurs pour la capacité de traitement.  
- Utilisez le [feuille de calcul Planification des capacités AD FS 2016](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx) pour déterminer le nombre de serveurs AD FS et Proxy d’Application Web, vous devez.

La mémoire et disque requise pour ADFS est relativement statique, consultez le tableau ci-dessous :


|**Configuration matérielle requise**|**Configuration minimale requise**|**Configuration recommandée**|
|----- | ----- |-----|
|RAM|2 Go|4 Go |
|Espace disque|32 Go|100 GO |

**Configuration matérielle requise pour SQL Server**

Si vous utilisez SQL Server pour votre base de données de configuration AD FS, taille de SQL Server selon les recommandations de SQL Server plus simples.  La taille de la base de données AD FS est très petite et AD FS ne place pas d’une charge de traitement considérable sur l’instance de base de données.  AD FS, toutefois, se connecte-t-il à la base de données plusieurs fois lors de l’authentification, la connexion réseau devrait donc être robuste.  Malheureusement, SQL Azure n’est pas pris en charge pour la base de données de configuration AD FS.
  
## <a name="BKMK_3"></a>Conditions du proxy  
  
-   Pour l’accès extranet, vous devez déployer le service de rôle Proxy d’Application Web \- fait partie du rôle de serveur d’accès à distance. 

-   Les proxys de tiers doivent prendre en charge la [protocole de MS-ADFSPIP](https://msdn.microsoft.com/en-us/library/dn392811.aspx) pour être pris en charge comme un proxy AD FS.  Pour obtenir la liste de 3ème partie fournisseurs voir le [FAQ](AD-FS-FAQ.md#what-third-party-proxies-are-available-for-ad-fs-that-support-ms-adfspip).

-   AD FS 2016 nécessite des serveurs Proxy d’Application Web sur Windows Server 2016.  Un proxy de niveau inférieur ne peut pas être configuré pour une batterie de serveurs AD FS 2016 en cours d’exécution au niveau du comportement de batterie de serveurs 2016.
  
-   Un serveur de fédération et le service de rôle Proxy d’Application Web ne peut pas être installés sur le même ordinateur.  
  
## <a name="BKMK_4"></a>Requise pour AD DS  
**Exigences de contrôleur de domaine**  
  
- AD FS nécessite des contrôleurs de domaine exécutant Windows Server 2008 ou version ultérieure.

- Au moins un contrôleur de domaine Windows Server 2016 est requis pour Microsoft Passport for Work.
  
> [!NOTE]  
> Prise en charge tous les environnements avec des contrôleurs de domaine Windows Server 2003 s’est terminée. Visitez [cette page](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO) pour plus d’informations sur la politique de Support Microsoft.  
  
**Domaine fonctionnel\-des exigences de niveau**  
  
 - Tous les domaines de compte d’utilisateur et le domaine auquel appartiennent les serveurs AD FS doivent être exécutés sur le niveau fonctionnel du domaine de Windows Server 2003 ou version ultérieure.  
  
 - Fonctionnel de domaine de Windows Server 2008 niveau ou une version ultérieure est requis pour l’authentification par certificat client si le certificat est mappé explicitement à un compte d’utilisateur dans AD DS.  
  
**Spécifications de schéma**  
  
-   Les nouvelles installations d’AD FS 2016 nécessitent le schéma Active Directory 2016 (version minimale 85).

-   Déclencher le niveau de comportement de batterie de serveurs AD FS (FBL) au niveau 2016 nécessite le schéma Active Directory 2016 (version minimale 85).  

  
**Exigences relatives aux comptes de service**  
  
-   N’importe quel compte de domaine standard peut être utilisé comme compte de service pour AD FS. Les comptes de Service géré de groupe sont également prises en charge. Les autorisations requises lors de l’exécution sont ajoutées automatiquement lors de la configuration AD FS.

-   L’attribution des droits utilisateur nécessaires pour le compte de service AD est « Une session en tant que Service »

-   L’attribution des droits utilisateur requis pour le « NT Service\adfssrv » et « NT Service\drs » sont « Générer des Audits de sécurité » et « Session en tant que Service ».

-   Comptes de service administré de groupe requièrent au moins un contrôleur de domaine exécutant Windows Server 2012 ou version ultérieure.  Le compte GMSA doit résider sous la valeur par défaut « CN = conteneur de comptes de Service administrés.  

-   Pour l’authentification Kerberos, le nom de principal du service '`HOST/<adfs\_service\_name>`' doit être enregistré sur le compte de service AD FS. Par défaut, AD FS cela configurera lors de la création d’une nouvelle batterie de serveurs AD FS.  En cas d’échec, comme dans le cas d’une collision ou des autorisations insuffisantes, vous verrez un avertissement et vous devez l’ajouter manuellement.  
   
**Configuration requise de domaine**  
  
-   Tous les serveurs AD FS doivent être joint à un domaine AD DS.  
  
-   Tous les serveurs AD FS dans une batterie de serveurs doivent être déployés dans le même domaine.  
   
**Exigences de plusieurs forêts**  
  
-   Le domaine auquel appartiennent les serveurs AD FS doit approuver chaque domaine ou une forêt qui contient les utilisateurs s’authentifiant auprès du service AD FS.  

-   La forêt, dont le compte de service AD FS est membre, doit approuver toutes les forêts de connexion utilisateur. 
  
-   Le compte de service AD FS doit avoir les autorisations nécessaires pour lire les attributs utilisateur dans chaque domaine qui contient les utilisateurs s’authentifiant au service AD FS.  
  
## <a name="BKMK_5"></a>Exigences de base de données de configuration  
Cette section décrit les exigences et restrictions pour les batteries de serveurs AD FS qui utilisent respectivement la base de données interne de Windows (WID) ou SQL Server comme base de données :  
  
**WID**  
  
-   Le profil de résolution d’artefact de SAML 2.0 n’est pas pris en charge dans une batterie WID.    

-   Détection de relecture de jetons n’est pas pris en charge d’une batterie de serveurs WID. (Cette fonctionnalité est uniquement utilisée uniquement dans les scénarios où AD FS est agissant comme fournisseur de fédération et consommer des jetons de sécurité provenant de fournisseurs de revendications externe).  
  
Le tableau suivant fournit un résumé du nombre de serveurs AD FS est pris en charge dans un rapport à WID une batterie de serveurs SQL Server.    
  
  
|| 1 - 100, partie de confiance (RP) approbations configurées dans AD FS | Plus de 100 approbations de partie de confiance configurées  |
| --- |--- | --- |
|Serveurs 1-30 AD FS|WID pris en charge|Non pris en charge à l’aide de WID - SQL Server requis |
|Serveurs de plus de 30 AD FS|Non pris en charge à l’aide de WID - SQL Server requis|Non pris en charge à l’aide de WID - SQL Server requis  
  
**SQL Server**  
  
- Pour AD FS dans Windows Server 2016, SQL Server 2008 et versions ultérieures sont pris en charge.

- Résolution d’artefacts SAML et de détection de relecture de jetons sont pris en charge dans une batterie de serveurs SQL Server.  
  
## <a name="BKMK_6"></a>Configuration requise du navigateur  
Lors de l’authentification AD FS est effectuée via un navigateur ou un contrôle de navigateur, votre navigateur doit être conforme aux exigences suivantes :  
  
- JavaScript doit être activé.  
  
- Pour l’authentification unique, le navigateur client doit être configuré pour autoriser les cookies  
  
- Indication du nom du serveur \(SNI\) doivent être pris en charge  
  
- Utilisateur certificate & appareil l’authentification par certificat, le navigateur doit prendre en charge l’authentification de certificat du client SSL  

- Pour l’authentification transparente à l’aide de l’authentification Windows intégrée, le nom de service de fédération (tel que https :\/\/fs.contoso.com) doit être configuré dans la zone intranet local ou sites de confiance.
  ## <a name="BKMK_7"></a>Configuration réseau requise  
 
**Configuration requise du pare-feu**  
  
Le pare-feu situé entre le Proxy d’Application Web et la batterie de serveurs de fédération et le pare-feu entre les clients et le Proxy d’Application Web doit avoir le port TCP 443 activé entrant.  
  
En outre, si l’authentification du certificat utilisateur client \(authentification clientTLS à l’aide de X509 certificats utilisateur\) est requis et le point de terminaison certauth sur le port 443 n’est pas activé, AD FS 2016 nécessite que le port TCP 49443 soit activée. trafic entrant sur le pare-feu entre les clients et le Proxy d’Application Web. Cela n’est pas requis sur le pare-feu entre le Proxy d’Application Web et les serveurs de fédération\). 

Pour plus d’informations sur le port hybride exigences consultez [hybride identité Ports et protocoles](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-ports). 

Pour plus d’informations, consultez [meilleures pratiques pour la sécurisation d’Active Directory Federation Services](../deployment/Best-Practices-Securing-AD-FS.md)
  
**Configuration DNS requise**  
  
-   Pour l’accès intranet, tous les clients l’accès à AD FS du service au sein du réseau d’entreprise interne \(intranet\) doit être en mesure de résoudre le nom du service AD FS pour l’équilibrage de charge pour les serveurs AD FS ou le serveur AD FS.  
  
-   Service pour l’accès extranet, tous les clients l’accès à AD FS à partir de l’extérieur du réseau d’entreprise \(extranet\/internet\) doit être en mesure de résoudre le nom du service AD FS pour l’équilibrage de charge pour les serveurs Proxy d’Application Web ou le serveur Proxy d’Application Web.  
  
-   Chaque serveur Proxy d’Application Web dans la zone DMZ doit être en mesure de résoudre le nom du service AD FS pour l’équilibrage de charge pour les serveurs AD FS ou le serveur AD FS. Vous pouvez le faire à l’aide d’un autre serveur DNS dans le réseau de zone DMZ ou en modifiant la résolution de serveur local à l’aide du fichier HOSTS.  
  
-   Pour l’authentification Windows intégrée, vous devez utiliser un enregistrement DNS A \(pas CNAME\) pour le nom de service de fédération.  

-   Pour l’authentification des certificats utilisateur sur le port 443, « certauth. \<nom de service de fédération\>« doit être configuré dans le système DNS pour résoudre le proxy d’application web ou de serveur de fédération.

-   Pour inscrire un appareil ou pour l’authentification moderne pour les ressources locales à l’aide de clients de versions antérieures de Windows 10, « enterpriseregistration. \<suffixe upn\>«, pour chaque suffixe UPN en cours d’utilisation dans votre organisation, doit être configuré pour résoudre pour le proxy d’application web ou de serveur de fédération.

**Exigences d’équilibreur de charge**
- L’équilibreur de charge ne doit pas mettre fin à SSL. AD FS prend en charge plusieurs cas d’utilisation avec l’authentification par certificat s’arrête lors de l’arrêt SSL. Fin d’exécution de l’équilibreur de charge SSL n’est pas pris en charge pour les cas d’usage. 
- Il est recommandé d’utiliser un équilibreur de charge qui prend en charge SNI. Dans le cas contraire, à l’aide de la 0.0.0.0 secours de liaison de vos services AD FS / serveur Proxy d’Application Web doit fournir une solution de contournement.
- Il est recommandé d’utiliser les points de terminaison du sonde d’intégrité HTTP (pas HTTPS) pour effectuer des vérifications d’intégrité d’équilibreur de charge pour le routage du trafic. Cela évite tout problème lié à SNI. La réponse à ces points de terminaison de sonde est un OK de 200 HTTP et est pris en charge localement avec aucune dépendance envers les services back-end. La sonde HTTP sont accessibles via HTTP en utilisant le chemin d’accès « / adfs/probe »
    - http://&lt;nom de Proxy d’Application Web &gt; /adfs/probe
    - http://&lt;nom du serveur ADFS &gt; /adfs/probe
    - http://&lt;adresse IP de Proxy d’Application Web &gt; /adfs/probe
    - http://&lt;adresse IP de l’ADFS &gt; /adfs/probe
- Il n’est pas recommandé d’utiliser DNS tourniquet (Round Robin) qui permet d’équilibrer la charge. À l’aide de ce type d’équilibrage de charge ne fournit pas un moyen automatisé pour supprimer un nœud d’équilibrage de charge à l’aide de sondes d’intégrité. 
- Il n’est pas recommandé d’utiliser l’affinité de session basé sur IP ou des sessions rémanentes pour le trafic d’authentification pour AD FS au sein de l’équilibreur de charge. Cela peut entraîner une surcharge de certains nœuds lors de l’utilisation du protocole d’authentification hérités pour les clients de messagerie pour vous connecter aux services de messagerie Office 365 (Exchange Online). 

## <a name="BKMK_13"></a>Exigences relatives aux autorisations  
L’administrateur qui effectue l’installation et la configuration initiale des services AD FS doit avoir des autorisations d’administrateur local sur le serveur AD FS.  Si l’administrateur local n’a pas d’autorisations pour créer des objets dans Active Directory, ils doivent définir un administrateur de domaine créer les objets AD requis, puis configurez la batterie de serveurs AD FS en utilisant le paramètre AdminConfiguration.  
  
  

