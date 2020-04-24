---
ms.assetid: 28f4a518-1341-4a10-8a4e-5f84625b314b
title: Configuration requise des services AD FS
description: Configuration requise pour l’installation des services de fédération Active Directory (AD FS)
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/06/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a2f4c9ac05e72083fab3e3a926dbdd2876214a7b
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "77517534"
---
# <a name="ad-fs-requirements"></a>Configuration AD FS requise



Voici les conditions requises pour le déploiement d’AD FS :  
  
-   [Conditions requises pour les certificats](ad-fs-requirements.md#BKMK_1)  
  
-   [Configuration matérielle requise](ad-fs-requirements.md#BKMK_2)  
  
-   [Conditions requises pour le proxy](ad-fs-requirements.md#BKMK_3)  
  
-   [Configuration AD DS requise](ad-fs-requirements.md#BKMK_4)  
  
-   [Conditions requises pour la base de données de configuration](ad-fs-requirements.md#BKMK_5)  
  
-   [Conditions requises pour les navigateurs](ad-fs-requirements.md#BKMK_6)  

-   [Configuration requise pour le réseau](ad-fs-requirements.md#BKMK_7)  
  
-   [Conditions requises pour les autorisations](ad-fs-requirements.md#BKMK_13)  
  
## <a name="certificate-requirements"></a><a name="BKMK_1"></a>Conditions requises pour les certificats  
  
### <a name="ssl-certificates"></a>Certificats SSL

Chaque serveur AD FS et Proxy d’application web dispose d’un certificat SSL pour traiter les requêtes HTTPS destinées au service de fédération.  Le proxy d’application web peut avoir des certificats SSL supplémentaires pour traiter les requêtes destinées aux applications publiées.

**Recommandation :** Utilisez le même certificat SSL pour tous les serveurs AD FS de fédération et proxys d’application web. 

**Conditions requises :**

Les certificats SSL sur les serveurs de fédération doivent remplir les conditions suivantes :
- Le certificat est approuvé publiquement (pour les déploiements de production).
- Le certificat contient la valeur d’utilisation améliorée de la clé (EKU) d’authentification du serveur.
- Le certificat contient le nom du service de fédération, par exemple « fs.contoso.com », dans l’objet ou l’autre nom de l’objet.
- Pour l’authentification par certificat utilisateur sur le port 443, le certificat contient « certauth.\<nom_service_fédération\> », par exemple « certauth.fs.contoso.com » dans l’autre nom de l’objet.
- Pour l’inscription de l’appareil ou pour l’authentification moderne auprès de ressources locales à l’aide de clients antérieurs à Windows 10, l’autre nom de l’objet doit contenir « enterpriseregistration.\<suffixe_upn\> » pour chaque suffixe UPN utilisé dans votre organisation.

Les certificats SSL sur le proxy d’application web doivent remplir les conditions suivantes :
- Si le proxy est utilisé pour les requêtes AD FS qui utilisent l’authentification Windows intégrée, le certificat SSL proxy doit être identique (il doit utiliser la même clé) au certificat SSL du serveur de fédération.
- Si la propriété AD FS « ExtendedProtectionTokenCheck » est activée (paramètre par défaut dans AD FS), le certificat SSL proxy doit être identique (il doit utiliser la même clé) au certificat SSL du serveur de fédération.
- Dans le cas contraire, les conditions requises pour le certificat SSL proxy sont les mêmes que celles du certificat SSL du serveur de fédération.

### <a name="service-communication-certificate"></a>Certificat de communication du service
Ce certificat n’est pas demandé dans la plupart des scénarios AD FS, notamment Azure AD et Office 365. Par défaut, AD FS configure le certificat SSL fourni lors de la configuration initiale en tant que certificat de communication du service.

**Recommandation :**
- Utilisez le même certificat que celui utilisé pour SSL.  

### <a name="token-signing-certificate"></a>Certificat de signature de jetons
Ce certificat est utilisé pour signer les jetons délivrés aux parties de confiance. Par conséquent, les applications de partie de confiance doivent reconnaître le certificat et sa clé associée comme connus et approuvés. Quand le certificat de signature de jetons change, par exemple quand il expire et que vous configurez un nouveau certificat, toutes les parties de confiance doivent être mises à jour.

**Recommandation :** Utilisez les certificats de signature de jetons auto-signés par défaut générés en interne par AD FS.  

**Conditions requises :**
- Si votre organisation exige que les certificats de l’infrastructure à clé publique de l’entreprise soient utilisés pour la signature des jetons, vous pouvez le faire à l’aide du paramètre SigningCertificateThumbprint de l’applet de commande Install-AdfsFarm.
- Que vous utilisiez les certificats par défaut générés en interne ou les certificats inscrits en externe, quand le certificat de signature de jetons est changé, vous devez vous assurer que toutes les parties de confiance sont mises à jour avec les nouvelles informations de certificat.  Dans le cas contraire, les ouvertures de session sur les parties de confiance non mises à jour échouent.

### <a name="token-encryptingdecrypting-certificate"></a>Certificat de chiffrement/déchiffrement de jetons
Ce certificat est utilisé par les fournisseurs de revendications qui chiffrent les jetons délivrés à AD FS.

**Recommandation :** Utilisez les certificats de déchiffrement de jetons auto-signés par défaut générés en interne par AD FS.  

**Conditions requises :**
- Si votre organisation demande d’utiliser les certificats de l’infrastructure à clé publique de l’entreprise pour la signature des jetons, c’est possible avec le paramètre DecryptingCertificateThumbprint de l’applet de commande Install-AdfsFarm.
- Que vous utilisiez les certificats par défaut générés en interne ou les certificats inscrits en externe, quand le certificat de déchiffrement de jetons est changé, vous devez vous assurer que tous les fournisseurs de revendications sont mis à jour avec les nouvelles informations de certificat.  Dans le cas contraire, les ouvertures de session utilisant des fournisseurs de revendications non mis à jour échouent.
  
> [!CAUTION]  
> Les certificats utilisés pour la signature de jetons et pour le chiffrement\/déchiffrement de jetons sont essentiels pour la stabilité du service de fédération. Les clients qui gèrent leurs propres certificats de chiffrement\/déchiffrement de jetons et certificats de signature de jetons doivent s’assurer que ces certificats sont sauvegardés et disponibles indépendamment pendant un événement de récupération.  

### <a name="user-certificates"></a>Certificats utilisateur
- Lors de l’utilisation de l’authentification par certificat utilisateur x509 avec AD FS, tous les certificats utilisateur doivent être chaînés à une autorité de certification racine approuvée par les serveurs Proxy d’application web et AD FS.

## <a name="hardware-requirements"></a><a name="BKMK_2"></a>Configuration matérielle requise  
La configuration matérielle (physique ou virtuelle) requise pour AD FS et le proxy d’application web est dictée par le processeur. Vous devez donc dimensionner votre batterie de serveurs en fonction de la capacité de traitement.  
- Utilisez la [feuille de calcul de planification de capacité AD FS 2016](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx) pour déterminer le nombre de serveurs AD FS et Proxy d’application web dont vous aurez besoin.

Les besoins en mémoire et en disques pour AD FS sont relativement statiques. Consultez le tableau ci-dessous :


|**Configuration matérielle requise**|**Configuration minimale requise**|**Configuration recommandée**|
|----- | ----- |-----|
|Mémoire vive (RAM)|2 Go|4 Go |
|Espace disque|32 Go|100 Go |

**Configuration matérielle requise pour SQL Server**

Si vous utilisez SQL Server pour votre base de données de configuration AD FS, dimensionnez le serveur SQL Server en fonction des recommandations SQL Server de base.  La base de données AD FS est très petite, et AD FS n’entraîne pas une charge de traitement importante sur l’instance de base de données.  En revanche, AD FS se connecte à la base de données plusieurs fois pendant une authentification ; la connexion réseau doit donc être robuste.  Malheureusement, SQL Azure n’est pas pris en charge pour la base de données de configuration AD FS.
  
## <a name="proxy-requirements"></a><a name="BKMK_3"></a>Conditions requises pour le proxy  
  
-   Pour l’accès extranet, vous devez déployer le service de rôle Proxy d’application web, qui fait partie du rôle serveur Accès à distance. 

-   Les proxys tiers doivent prendre en charge le protocole [MS-ADFSPIP](https://msdn.microsoft.com/library/dn392811.aspx) pour être pris en charge en tant que proxy AD FS.  Pour obtenir la liste des fournisseurs tiers, consultez le [FAQ](AD-FS-FAQ.md).

-   AD FS 2016 exige des serveurs proxy d’application web sur Windows Server 2016.  Vous ne pouvez pas configurer un proxy de niveau inférieur pour une batterie de serveurs AD FS 2016 s’exécutant au niveau du comportement de batterie de serveurs 2016.
  
-   Un serveur de fédération et le service de rôle Proxy d’application web ne peuvent pas être installés sur le même ordinateur.  
  
## <a name="ad-ds-requirements"></a><a name="BKMK_4"></a>Configuration AD DS requise  
**Conditions requises pour les contrôleurs de domaine**  
  
- AD FS exige des contrôleurs de domaine exécutant Windows Server 2008 ou ultérieur.

- Au moins un contrôleur de domaine Windows Server 2016 est nécessaire pour Microsoft Passport for Work.
  
> [!NOTE]  
> Toute prise en charge des environnements avec des contrôleurs de domaine Windows Server 2003 est terminée. Pour plus d’informations sur le cycle de vie Support Microsoft, consultez [cette page](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO).  
  
**Exigences relatives au niveau fonctionnel du domaine**  
  
 - Tous les domaines de comptes d’utilisateur et le domaine auquel les serveurs AD FS sont joints doivent opérer au niveau fonctionnel du domaine Windows Server 2003 ou supérieur.  
  
 - Un niveau fonctionnel du domaine Windows Server 2008 ou supérieur est nécessaire pour l’authentification de certificat client si le certificat est mappé explicitement au compte d’un utilisateur dans AD DS.  
  
**Exigences relatives au schéma**  
  
-   Les nouvelles installations d’AD FS 2016 nécessitent le schéma Active Directory 2016 (version minimale 85).

-   L’élévation du niveau de comportement de batterie de serveurs AD FS au niveau 2016 nécessite le schéma Active Directory 2016 (version minimale 85).  

  
**Exigences relatives au compte de service**  
  
-   N’importe quel compte de domaine standard peut être utilisé comme compte de service pour AD FS. Les comptes de service administrés de groupe sont également pris en charge. Les autorisations requises au moment de l’exécution sont ajoutées automatiquement quand vous configurez AD FS.

-   L’attribution des droits utilisateur requise pour le compte de service Active Directory est « Ouvrir une session en tant que service ».

-   Les attributions des droits utilisateur requises pour « NT Service\adfssrv » et « NT Service\drs » sont « Générer des audits de sécurité » et « Ouvrir une session en tant que service ».

-   Les comptes de service administrés de groupe nécessitent au moins un contrôleur de domaine exécutant Windows Server 2012 ou version ultérieure.  Le compte de service administré de groupe doit résider sous le conteneur « CN=Managed Service Accounts » par défaut.  

-   Pour l’authentification Kerberos, le nom de principal du service « `HOST/<adfs\_service\_name>` » doit être inscrit sur le compte de service AD FS. Par défaut, AD FS configure ce paramètre lors de la création d’une batterie de serveurs AD FS.  En cas d’échec, comme dans le cas d’une collision ou d’autorisations insuffisantes, un avertissement s’affiche et vous devez l’ajouter manuellement.  
   
**Exigences relatives au domaine**  
  
-   Tous les serveurs AD FS doivent être joints à un domaine AD DS.  
  
-   Tous les serveurs AD FS au sein d’une batterie de serveurs doivent être déployés dans le même domaine.  
   
**Exigences relatives aux forêts multiples**  
  
-   Le domaine auquel les serveurs AD FS sont joints doit approuver chaque domaine ou forêt qui contient des utilisateurs s’authentifiant auprès du service AD FS.  

-   La forêt dont le compte de service AD FS est membre doit approuver toutes les forêts de connexion utilisateur. 
  
-   Le compte de service AD FS doit avoir les autorisations nécessaires pour lire les attributs utilisateur dans chaque domaine qui contient des utilisateurs s’authentifiant auprès du service AD FS.  
  
## <a name="configuration-database-requirements"></a><a name="BKMK_5"></a>Conditions requises pour la base de données de configuration  
Cette section décrit les conditions requises et les restrictions pour les batteries de serveurs AD FS qui utilisent respectivement la base de données interne Windows (WID, Windows Internal Database) ou SQL Server comme base de données :  
  
**Base de données interne Windows**  
  
-   Le profil de résolution d’artefact de SAML 2.0 n’est pas pris en charge dans une batterie de serveurs WID.    

-   La détection de relecture de jetons n’est pas prise en charge dans une batterie de serveurs WID. (Cette fonctionnalité n’est utilisée que dans les scénarios où AD FS agit en tant que fournisseur de fédération et consomme des jetons de sécurité provenant de fournisseurs de revendications externes.)  
  
Le tableau suivant fournit un récapitulatif du nombre de serveurs AD FS pris en charge dans une batterie de serveurs WID ou SQL Server.    
  
  
|| 1 - 100 approbations de partie de confiance configurées dans AD FS | Plus de 100 approbations de partie de confiance configurées  |
| --- |--- | --- |
|1 - 30 serveurs AD FS|WID pris en charge|Utilisation de WID non prise en charge - SQL Server nécessaire |
|Plus de 30 serveurs AD FS|Utilisation de WID non prise en charge - SQL Server nécessaire|Utilisation de WID non prise en charge - SQL Server nécessaire  
  
**SQL Server**  
  
- Pour AD FS dans Windows Server 2016, SQL Server 2008 et versions ultérieures sont prises en charge.

- La résolution d’artefacts SAML et la détection de relecture de jetons sont prises en charge dans une batterie SQL Server.  
  
## <a name="browser-requirements"></a><a name="BKMK_6"></a>Conditions requises pour les navigateurs  
Quand l’authentification AD FS est effectuée par le biais d’un navigateur ou d’un contrôle de navigateur, votre navigateur doit remplir les conditions suivantes :  
  
- JavaScript doit être activé.  
  
- Pour l’authentification unique, le navigateur client doit être configuré pour autoriser les cookies.  
  
- L’indication du nom du serveur \(SNI\) doit être prise en charge.  
  
- Pour le certificat utilisateur et l’authentification par certificat d’appareil, le navigateur doit prendre en charge l’authentification par certificat client SSL.  

- Pour bénéficier d’une connexion fluide à l’aide de l’authentification Windows intégrée, le nom du service de fédération (par exemple https:\/\/fs.contoso.com) doit être configuré dans la zone Intranet local ou dans la zone Sites de confiance.
  ## <a name="network-requirements"></a><a name="BKMK_7"></a>Conditions requises pour le réseau  
 
**Exigences relatives au pare-feu**  
  
Le pare-feu situé entre le proxy d’application web et la batterie de serveurs de fédération ainsi que le pare-feu situé entre les clients et le proxy d’application web doivent avoir le port TCP 443 activé pour le trafic entrant.  
  
De plus, si l’authentification par certificat utilisateur client \(authentification clientTLS à l’aide de certificats utilisateur x509\) est requise et que le point de terminaison certauth sur le port 443 n’est pas activé, AD FS 2016 exige que le port TCP 49443 soit activé dans le sens entrant sur le pare-feu entre les clients et le proxy d’application web. Cela n’est pas obligatoire sur le pare-feu entre le proxy d’application web et les serveurs de fédération. 

Pour plus d’informations sur les exigences relatives aux ports hybrides, consultez [Ports et protocoles nécessaires à l’identité hybride](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-ports). 

Pour plus d’informations, consultez [Bonnes pratiques pour la sécurisation des services AD FS](../deployment/Best-Practices-Securing-AD-FS.md).
  
**Exigences relatives au système DNS**  
  
-   Pour l’accès intranet, tous les clients qui accèdent au service AD FS au sein du réseau d’entreprise interne \(intranet\) doivent être en mesure de résoudre le nom du service AD FS en équilibreur de charge pour le ou les serveurs AD FS.  
  
-   Pour l’accès extranet, tous les clients qui accèdent au service AD FS à partir de l’extérieur du réseau d’entreprise \(extranet\/internet\) doivent être en mesure de résoudre le nom du service AD FS en équilibreur de charge pour le ou les serveurs Proxy d’application web.  
  
-   Chaque serveur Proxy d’application web de la zone DMZ doit être capable de résoudre le nom du service AD FS en équilibreur de charge pour le ou les serveurs AD FS. Cela peut être accompli en faisant appel à un serveur DNS secondaire dans le réseau DMZ ou en changeant la résolution du serveur local à l’aide du fichier HOSTS.  
  
-   Pour l’authentification Windows intégrée, vous devez utiliser un enregistrement DNS A \(pas un enregistrement CNAME\) pour le nom du service de fédération.  

-   Pour l’authentification par certificat utilisateur sur le port 443, « certauth.\<nom_service_fédération\> » doit être configuré dans le système DNS pour correspondre au serveur de fédération ou au proxy d’application web.

-   Pour l’inscription de l’appareil ou pour l’authentification moderne auprès de ressources locales à l’aide de clients antérieurs à Windows 10, « enterpriseregistration.\<suffixe_upn\> », pour chaque suffixe UPN utilisé dans votre organisation, doit être configuré pour correspondre au serveur de fédération ou au proxy d’application web.

**Exigences relatives à l’équilibreur de charge**
- L’équilibreur de charge NE DOIT PAS mettre fin au protocole SSL. AD FS prend en charge plusieurs cas d’usage avec authentification par certificat qui seront interrompus lors de l’arrêt du protocole SSL. L’arrêt du protocole SSL sur l’équilibreur de charge n’est pris en charge pour aucun cas d’usage. 
- Nous vous recommandons d’utiliser un équilibreur de charge qui prend en charge SNI. Dans le cas contraire, l’utilisation de la liaison de secours 0.0.0.0 sur votre serveur Proxy d’application web/AD FS doit fournir une solution de contournement.
- Nous vous recommandons d’utiliser les points de terminaison de sonde d’intégrité HTTP (et non HTTPS) afin d’effectuer des vérifications de l’intégrité de l’équilibreur de charge pour le routage du trafic. Cela permet d’éviter tout problème lié à SNI. La réponse à ces points de terminaison de sonde est HTTP 200 OK. Elle est traitée localement sans dépendance envers les services back-end. La sonde HTTP est accessible par le biais du protocole HTTP à l’aide du chemin « /adfs/probe »
    - http://&lt;nom_proxy_d’application_web&gt;/adfs/probe
    - http://&lt;nom_serveur_ADFS&gt;/adfs/probe
    - http://&lt;adrese_IP_proxy_d’application_web&gt;/adfs/probe
    - http://&lt;adrese_IP_ADFS&gt;/adfs/probe
- Il n’est PAS recommandé d’utiliser le tourniquet (Round Robin) DNS pour équilibrer la charge. L’utilisation de ce type d’équilibrage de charge ne fournit pas de méthode automatisée pour supprimer un nœud de l’équilibreur de charge à l’aide de sondes d’intégrité. 
- Il n’est PAS recommandé d’utiliser l’affinité de session basée sur IP ou des sessions rémanentes pour le trafic d’authentification vers AD FS au sein de l’équilibreur de charge. Cela peut entraîner une surcharge de certains nœuds lors de l’utilisation du protocole d’authentification hérité pour que les clients d’e-mail se connectent aux services de messagerie Office 365 (Exchange Online). 

## <a name="permissions-requirements"></a><a name="BKMK_13"></a>Conditions requises pour les autorisations  
L’administrateur qui effectue l’installation et la configuration initiale d’AD FS doit disposer d’autorisations d’administrateur local sur le serveur AD FS.  Si l’administrateur local ne dispose pas des autorisations nécessaires pour créer des objets dans Active Directory, il doit d’abord demander à un administrateur de domaine de créer les objets AD nécessaires, puis configurer la batterie de serveurs AD FS à l’aide du paramètre AdminConfiguration.  
  
  

