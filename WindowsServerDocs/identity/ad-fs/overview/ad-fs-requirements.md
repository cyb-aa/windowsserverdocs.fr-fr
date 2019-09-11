---
ms.assetid: 28f4a518-1341-4a10-8a4e-5f84625b314b
title: Configuration requise des services AD FS
description: Configuration requise pour l’installation de Services ADFS.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/06/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b93848a47eca952ebbeeec2a55c3e6f9b60dbfb8
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865478"
---
# <a name="ad-fs-requirements"></a>Configuration AD FS requise



Voici la configuration requise pour le déploiement de AD FS :  
  
-   [Conditions requises pour les certificats](ad-fs-requirements.md#BKMK_1)  
  
-   [Configuration matérielle requise](ad-fs-requirements.md#BKMK_2)  
  
-   [Configuration requise du proxy](ad-fs-requirements.md#BKMK_3)  
  
-   [Configuration requise pour la AD DS](ad-fs-requirements.md#BKMK_4)  
  
-   [Configuration requise pour la base de données de configuration](ad-fs-requirements.md#BKMK_5)  
  
-   [Configuration requise pour le navigateur](ad-fs-requirements.md#BKMK_6)  

-   [Configuration réseau requise](ad-fs-requirements.md#BKMK_7)  
  
-   [Exigences relatives aux autorisations](ad-fs-requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>Certificats requis  
  
### <a name="ssl-certificates"></a>Certificats SSL

Chaque AD FS et le serveur proxy d’application Web dispose d’un certificat SSL pour traiter les requêtes HTTPs auprès du service de Fédération.  Le proxy d’application Web peut avoir des certificats SSL supplémentaires pour traiter les demandes aux applications publiées.

**Recommandés** Utilisez le même certificat SSL pour tous les AD FS les serveurs de Fédération et les proxys d’application Web. 

**Exigences**

Les certificats SSL sur les serveurs de Fédération doivent remplir les conditions suivantes
- Le certificat est approuvé publiquement (pour les déploiements de production)
- Le certificat contient la valeur de l’utilisation améliorée de la clé d’authentification du serveur
- Le certificat contient le nom du service de Fédération, tel que « fs.contoso.com » dans l’objet ou l’autre nom de l’objet (SAN)
- Pour l’authentification par certificat utilisateur sur le port 443, le certificat contient «certauth. \<nom\>du service de Fédération, tel que « certauth.FS.contoso.com » dans le San
- Pour l’inscription de l’appareil ou pour une authentification moderne auprès des ressources locales à l’aide de clients antérieurs à Windows 10, le réseau SAN doit contenir «enterpriseregistration. suffixe\>UPN» pour chaque suffixe UPN utilisé dans votre organisation. \<

Les certificats SSL sur le proxy d’application Web doivent remplir les conditions suivantes
- Si le proxy est utilisé pour effectuer un proxy AD FS les demandes qui utilisent l’authentification intégrée de Windows, le certificat SSL proxy doit être identique (utiliser la même clé) que le certificat SSL du serveur de Fédération.
- Si la propriété AD FS « ExtendedProtectionTokenCheck » est activée (paramètre par défaut dans AD FS), le certificat SSL proxy doit être identique (utiliser la même clé) que le certificat SSL du serveur de Fédération.
- Dans le cas contraire, les conditions requises pour le certificat SSL proxy sont les mêmes que celles du certificat SSL du serveur de Fédération.

### <a name="service-communication-certificate"></a>Certificat de communication du service
Ce certificat n’est pas requis pour la plupart des scénarios de AD FS, y compris Azure AD et Office 365. Par défaut, AD FS configure le certificat SSL fourni lors de la configuration initiale en tant que certificat de communication du service.

**Recommandés**
- Utilisez le même certificat que celui utilisé pour SSL.  

### <a name="token-signing-certificate"></a>Certificat de signature de jetons
Ce certificat est utilisé pour signer les jetons émis aux parties de confiance. par conséquent, les applications de partie de confiance doivent reconnaître le certificat et sa clé associée est connue et approuvée. Lorsque le certificat de signature de jetons change, par exemple lorsqu’il expire et que vous configurez un nouveau certificat, toutes les parties de confiance doivent être mises à jour.

**Recommandés** Utilisez les certificats de signature de jetons auto-signés par défaut générés par AD FS.  

**Exigences**
- Si votre organisation exige que les certificats de l’infrastructure à clé publique de l’entreprise soient utilisés pour la signature des jetons, vous pouvez le faire à l’aide du paramètre SigningCertificateThumbprint de l’applet de commande Install-AdfsFarm.
- Que vous utilisiez les certificats générés par défaut ou les certificats inscrits en externe, lorsque le certificat de signature de jetons est modifié, vous devez vous assurer que toutes les parties de confiance sont mises à jour avec les nouvelles informations de certificat.  Dans le cas contraire, les ouvertures de session sur des parties de confiance non mises à jour échouent.

### <a name="token-encryptingdecrypting-certificate"></a>Certificat de chiffrement/déchiffrement de jetons
Ce certificat est utilisé par les fournisseurs de revendications qui chiffrent les jetons émis pour AD FS.

**Recommandés** Utilisez les certificats de déchiffrement de jetons auto-signés par défaut AD FS, générés en interne.  

**Exigences**
- Si votre organisation exige que les certificats de l’infrastructure à clé publique de l’entreprise soient utilisés pour la signature des jetons, vous pouvez le faire à l’aide du paramètre DecryptingCertificateThumbprint de l’applet de commande Install-AdfsFarm.
- Que vous utilisiez les certificats générés par défaut ou les certificats inscrits en externe, lorsque le certificat de déchiffrement de jetons est modifié, vous devez vous assurer que tous les fournisseurs de revendications sont mis à jour avec les nouvelles informations de certificat.  Dans le cas contraire, les ouvertures de session utilisant des fournisseurs de revendications non mis à jour échouent.
  
> [!CAUTION]  
> Les certificats utilisés pour la signature\-des jetons et\-le\/chiffrement du déchiffrement des jetons sont essentiels pour la stabilité du service FS (Federation Service). Les clients qui gèrent leur\-propre signature de\-jetons &\/le chiffrement des certificats de chiffrement doivent s’assurer que ces certificats sont sauvegardés et sont disponibles indépendamment pendant un événement de récupération.  

### <a name="user-certificates"></a>Certificats utilisateur
- Lors de l’utilisation de l’authentification par certificat utilisateur x509 avec AD FS, tous les certificats d’utilisateur doivent être chaînés à une autorité de certification racine approuvée par les serveurs proxy d’application Web et AD FS.

## <a name="BKMK_2"></a>Configuration matérielle requise  
AD FS et le proxy d’application Web requis (physique ou virtuel) sont contrôlés sur le processeur. vous devez donc dimensionner votre batterie de serveurs pour la capacité de traitement.  
- Utilisez la [feuille de calcul de planification de la capacité AD FS 2016](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx) pour déterminer le nombre de AD FS et de serveurs proxy d’application Web dont vous aurez besoin.

La mémoire et les disques requis pour AD FS sont relativement statiques, consultez le tableau ci-dessous :


|**Configuration matérielle requise**|**Configuration minimale requise**|**Configuration recommandée**|
|----- | ----- |-----|
|RAM|2 Go|4 Go |
|Espace disque|32 Go|100 GO |

**SQL Server Configuration matérielle requise**

Si vous utilisez SQL Server pour votre base de données de configuration AD FS, redimensionnez le SQL Server en fonction des recommandations de SQL Server de base.  La taille de la base de données AD FS est très petite et AD FS n’entraîne pas une charge de traitement importante sur l’instance de base de données.  Toutefois, AD FS ne se connecte pas à la base de données plusieurs fois pendant une authentification, de sorte que la connexion réseau doit être robuste.  Malheureusement, SQL Azure n’est pas pris en charge pour la base de données de configuration AD FS.
  
## <a name="BKMK_3"></a>Configuration requise du proxy  
  
-   Pour l’accès extranet, vous devez déployer la partie service \- de rôle proxy d’application Web du rôle serveur d’accès à distance. 

-   Les proxys tiers doivent prendre en charge le [protocole MS-ADFSPIP](https://msdn.microsoft.com/library/dn392811.aspx) pour être pris en charge en tant que proxy AD FS.  Pour obtenir la liste des fournisseurs tiers, consultez le [Forum aux questions](AD-FS-FAQ.md).

-   AD FS 2016 requiert des serveurs proxy d’application Web sur Windows Server 2016.  Un proxy de niveau inférieur ne peut pas être configuré pour une batterie de serveurs AD FS 2016 s’exécutant au niveau du comportement de la batterie de serveurs 2016.
  
-   Un serveur de Fédération et le service de rôle proxy d’application Web ne peuvent pas être installés sur le même ordinateur.  
  
## <a name="BKMK_4"></a>Configuration requise pour la AD DS  
**Configuration requise pour le contrôleur de domaine**  
  
- AD FS nécessite des contrôleurs de domaine exécutant Windows Server 2008 ou une version ultérieure.

- Au moins un contrôleur de domaine Windows Server 2016 est requis pour Microsoft Passport for Work.
  
> [!NOTE]  
> Toute la prise en charge des environnements avec les contrôleurs de domaine Windows Server 2003 est terminée. Pour plus d’informations sur le cycle de vie de Support Microsoft, consultez [cette page](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO) .  
  
**Exigences de\-niveau fonctionnel de domaine**  
  
 - Tous les domaines de compte d’utilisateur et le domaine auquel les serveurs de AD FS sont joints doivent fonctionner au niveau fonctionnel du domaine de Windows Server 2003 ou version ultérieure.  
  
 - Un niveau fonctionnel de domaine Windows Server 2008 ou une version ultérieure est requis pour l’authentification par certificat client si le certificat est explicitement mappé au compte d’un utilisateur dans AD DS.  
  
**Spécifications du schéma**  
  
-   Les nouvelles installations de AD FS 2016 requièrent le schéma Active Directory 2016 (version minimale 85).

-   L’élévation du niveau de comportement de la batterie de serveurs AD FS (FBL) au niveau 2016 nécessite le schéma Active Directory 2016 (version minimale 85).  

  
**Exigences relatives au compte de service**  
  
-   N’importe quel compte de domaine standard peut être utilisé comme compte de service pour AD FS. Les comptes de service administrés de groupe sont également pris en charge. Les autorisations requises lors de l’exécution sont automatiquement ajoutées lorsque vous configurez AD FS.

-   L’attribution des droits utilisateur requise pour le compte de service Active Directory est « ouvrir une session en tant que service »

-   Les attributions des droits utilisateur requises pour les autorisations « NT Service\adfssrv » et « NT Service\drs » sont « générer des audits de sécurité » et « ouvrir une session en tant que service ».

-   Les comptes de service administrés de groupe nécessitent au moins un contrôleur de domaine exécutant Windows Server 2012 ou une version ultérieure.  GMSA doit résider sous le conteneur par défaut « CN = Managed Service Accounts ».  

-   Pour l’authentification Kerberos, le nom de principal`HOST/<adfs\_service\_name>`du service «» doit être inscrit sur le compte de service AD FS. Par défaut, AD FS configure ce paramètre lors de la création d’une batterie de AD FS.  En cas d’échec, comme dans le cas d’une collision ou d’autorisations insuffisantes, un avertissement s’affiche et vous devez l’ajouter manuellement.  
   
**Configuration requise du domaine**  
  
-   Tous les serveurs AD FS doivent être joints à un domaine AD DS.  
  
-   Tous les serveurs AD FS au sein d’une batterie de serveurs doivent être déployés dans le même domaine.  
   
**Configuration requise pour plusieurs forêts**  
  
-   Le domaine auquel les serveurs AD FS sont joints doit approuver chaque domaine ou forêt qui contient les utilisateurs qui s’authentifient auprès du service AD FS.  

-   La forêt, à laquelle le compte de service AD FS est membre, doit approuver toutes les forêts de connexion utilisateur. 
  
-   Le compte de service AD FS doit avoir les autorisations nécessaires pour lire les attributs utilisateur dans chaque domaine qui contient les utilisateurs qui s’authentifient auprès du service AD FS.  
  
## <a name="BKMK_5"></a>Configuration requise pour la base de données de configuration  
Cette section décrit les conditions requises et les restrictions pour les batteries de AD FS qui utilisent respectivement la base de données interne Windows (WID) ou SQL Server en tant que base de données :  
  
**WID**  
  
-   Le profil de résolution d’artefact de SAML 2,0 n’est pas pris en charge dans une batterie de serveurs WID.    

-   La détection de relecture de jetons n’est pas prise en charge par une batterie de serveurs WID. (Cette fonctionnalité n’est utilisée que dans les scénarios où AD FS agit en tant que fournisseur de Fédération et utilise des jetons de sécurité provenant de fournisseurs de revendications externes.)  
  
Le tableau suivant fournit un résumé du nombre de serveurs AD FS pris en charge dans un schéma WID ou une batterie de SQL Server.    
  
  
|| 1-100 approbations de partie de confiance configurées dans AD FS | Plus de 100 confiances RP configurées  |
| --- |--- | --- |
|1-30 serveurs AD FS|WID pris en charge|Non pris en charge avec WID-SQL Server requis |
|Plus de 30 serveurs AD FS|Non pris en charge avec WID-SQL Server requis|Non pris en charge avec WID-SQL Server requis  
  
**SQL Server**  
  
- Pour AD FS dans Windows Server 2016, SQL Server 2008 et versions ultérieures sont prises en charge.

- La résolution d’artefacts SAML et la détection de relecture de jetons sont prises en charge dans une batterie de SQL Server.  
  
## <a name="BKMK_6"></a>Configuration requise pour le navigateur  
Lorsque AD FS authentification est effectuée par le biais d’un navigateur ou d’un contrôle de navigateur, votre navigateur doit respecter les conditions suivantes :  
  
- JavaScript doit être activé  
  
- Pour l’authentification unique, le navigateur client doit être configuré pour autoriser les cookies  
  
- Indication du nom du serveur \(SNI\) doit être pris en charge  
  
- Pour le certificat utilisateur & l’authentification par certificat d’appareil, le navigateur doit prendre en charge l’authentification par certificat client SSL  

- Pour une connexion transparente à l’aide de l’authentification intégrée Windows, le nom du service de Fédération\/(par exemple, https :\/FS.contoso.com) doit être configuré dans la zone Intranet local ou dans la zone sites de confiance.
  ## <a name="BKMK_7"></a>Configuration réseau requise  
 
**Configuration requise du pare-feu**  
  
Le pare-feu situé entre le proxy d’application Web et la batterie de serveurs de Fédération et le pare-feu entre les clients et le proxy d’application Web doit avoir le port TCP 443 activé pour le trafic entrant.  
  
En outre, si l’authentification par \(certificat utilisateur du client clientTLS l’authentification à l’aide de certificats\) utilisateur x509 est requise et que le point de terminaison certauth sur le port 443 n’est pas activé, AD FS 2016 requiert l’activation du port TCP 49443 entrant sur le pare-feu entre les clients et le proxy d’application Web. Cela n’est pas obligatoire sur le pare-feu entre le proxy d’application Web\)et les serveurs de Fédération. 

Pour plus d’informations sur la configuration requise des ports hybrides [, consultez ports et protocoles d’identité hybrides](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-ports). 

Pour plus d’informations, consultez [meilleures pratiques pour la sécurisation des services ADFS](../deployment/Best-Practices-Securing-AD-FS.md)
  
**Configuration DNS requise**  
  
-   Pour l’accès intranet, tous les clients qui accèdent à AD FS service \(au\) sein de l’intranet du réseau d’entreprise interne doivent être en mesure de résoudre le nom du service AD FS en équilibreur de charge pour les serveurs AD FS ou le serveur AD FS.  
  
-   Pour l’accès extranet, tous les clients qui accèdent à AD FS service \(en\/dehors\) de l’extranet du réseau d’entreprise doivent être en mesure de résoudre le nom du service AD FS en équilibreur de charge pour les serveurs proxy d’application Web ou serveur proxy d’application Web.  
  
-   Chaque serveur proxy d’application Web de la zone DMZ doit être en mesure de résoudre AD FS nom de service en équilibreur de charge pour les serveurs AD FS ou le serveur AD FS. Cela peut être accompli à l’aide d’un serveur DNS secondaire dans le réseau DMZ ou en modifiant la résolution du serveur local à l’aide du fichier HOSTs.  
  
-   Pour l’authentification intégrée de Windows, vous devez utiliser un enregistrement \(DNS a\) non CNAME pour le nom du service FS (Federation Service).  

-   Pour l’authentification par certificat utilisateur sur le port 443, «certauth. \<lenom\>du service de Fédération doit être configuré dans DNS pour être résolu sur le serveur de Fédération ou le proxy d’application Web.

-   Pour l’inscription des appareils ou pour l’authentification moderne auprès des ressources locales à l’aide de clients antérieurs à Windows 10, «enterpriseregistration. suffixe\>UPN», pour chaque suffixe UPN utilisé dans votre organisation, doit être configuré pour être résolu en serveur de Fédération ou proxy d’application Web. \<

**Configuration requise pour la Load Balancer**
- L’équilibreur de charge ne doit pas arrêter le protocole SSL. AD FS prend en charge plusieurs cas d’usage avec l’authentification par certificat, ce qui s’interrompt lors de l’arrêt de SSL. L’arrêt de SSL sur l’équilibreur de charge n’est pas pris en charge pour les cas d’usage. 
- Il est recommandé d’utiliser un équilibreur de charge qui prend en charge SNI. Dans le cas contraire, l’utilisation de la liaison de secours 0.0.0.0 sur votre serveur proxy d’application AD FS/Web doit fournir une solution de contournement.
- Il est recommandé d’utiliser les points de terminaison de sonde d’intégrité HTTP (et non HTTPs) pour effectuer des vérifications de l’intégrité de l’équilibreur de charge pour le trafic de routage. Cela permet d’éviter tout problème lié à SNI. La réponse à ces points de terminaison de sonde est HTTP 200 OK et est traitée localement sans dépendre des services principaux. La sonde HTTP est accessible via HTTP à l’aide du chemin d’accès « /ADFS/Probe »
    - http://&lt;nom&gt;du proxy d’application Web/ADFS/Probe
    - http://&lt;nom&gt;du serveur ADFS/ADFS/Probe
    - &lt;adresse&gt;IP du proxy d’application Web http:///ADFS/Probe
    - &lt;adresse&gt;IP http://ADFS/ADFS/Probe
- Il n’est pas recommandé d’utiliser le tourniquet (Round Robin) DNS pour équilibrer la charge. L’utilisation de ce type d’équilibrage de charge ne fournit pas de méthode automatisée pour supprimer un nœud de l’équilibreur de charge à l’aide de sondes d’intégrité. 
- Il n’est pas recommandé d’utiliser une affinité de session basée sur IP ou des sessions rémanentes pour le trafic d’authentification pour AD FS au sein de l’équilibreur de charge. Cela peut entraîner une surcharge de certains nœuds lors de l’utilisation du protocole d’authentification hérité pour que les clients de messagerie se connectent à Office 365 mail services (Exchange Online). 

## <a name="BKMK_13"></a>Exigences relatives aux autorisations  
L’administrateur qui effectue l’installation et la configuration initiale de AD FS doivent disposer d’autorisations d’administrateur local sur le serveur de AD FS.  Si l’administrateur local ne dispose pas des autorisations nécessaires pour créer des objets dans Active Directory, il doit d’abord avoir un administrateur de domaine pour créer les objets AD requis, puis configurer la batterie de serveurs AD FS à l’aide du paramètre AdminConfiguration.  
  
  

