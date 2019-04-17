---
ms.assetid: 28f4a518-1341-4a10-8a4e-5f84625b314b
title: Configuration requise en 2016 ADFS
description: "Configuration requise pour l’installation d’ActiveDirectory Federation Services."
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/06/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 81b51c751d5f54436b14450ef21bf49feb864290
ms.sourcegitcommit: 556361fe7c73c75d6cdc46a67dc88679fbe89c61
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2018
---
# <a name="ad-fs-requirements"></a>Configuration ADFS requise

>S’applique à: Windows Server2016

Voici la configuration requise pour le déploiement d’ADFS:  
  
-   [Configuration requise des certificats](ad-fs-requirements.md#BKMK_1)  
  
-   [Configuration matérielle requise](ad-fs-requirements.md#BKMK_2)  
  
-   [Configuration requise du proxy](ad-fs-requirements.md#BKMK_3)  
  
-   [Configuration requise de ADDS](ad-fs-requirements.md#BKMK_4)  
  
-   [Configuration requise de base de données](ad-fs-requirements.md#BKMK_5)  
  
-   [Configuration requise du navigateur](ad-fs-requirements.md#BKMK_6)  

-   [Configuration réseau requise](ad-fs-requirements.md#BKMK_7)  
  
-   [Autorisations requises](ad-fs-requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>Configuration requise des certificats  
  
### <a name="ssl-certificates"></a>Certificats SSL

Chaque serveur ADFS et le Proxy d’Application Web possède un certificat SSL pour traiter les demandes HTTPS pour le service de fédération.  Le Proxy d’Application Web peut avoir des certificats SSL supplémentaires pour traiter les demandes aux applications publiées.

**Recommandation:** utiliser le même certificat SSL pour tous les serveurs de fédération ADFS et proxys d’Application Web. 

**Configuration requise:**

Certificats SSL sur les serveurs de fédération doivent respecter les conditions suivantes
- Certificat est approuvé publiquement (pour les déploiements de production)
- Certificat contient la valeur du serveur d’authentification renforcée clé (EKU)
- Certificat contient le nom du service de fédération, par exemple, «fs.contoso.com» dans l’objet ou autre nom de l’objet
- Pour l’authentification des certificats utilisateur sur le port 443, le certificat contient «certauth. \ < nom\ du service de fédération >», par exemple, «certauth.fs.contoso.com» dans le réseau SAN
- Pour l’inscription de l’appareil ou l’authentification moderne sur les ressources locales à l’aide les clients antérieurs à Windows10, le réseau SAN doit contenir «inscriptionentreprise. \ < nom d’utilisateur principal suffix\ >» pour chaque suffixe UPN en cours d’utilisation dans votre organisation.

Certificats SSL sur le Proxy d’Application Web doivent respecter les conditions suivantes
- Si le proxy est utilisé pour les demandes de proxy ADFS qui utilisent l’authentification Windows intégrée, le certificat SSL de serveur proxy doivent être le même (utilisent la même clé) en tant que le certificat SSL de serveur de fédération
- Si la propriété ADFS «ExtendedProtectionTokenCheck» est activé (paramètre dans ADFS par défaut), le certificat SSL de serveur proxy doit être le même (utilisent la même clé) que le certificat SSL de serveur de fédération
- Dans le cas contraire, la configuration requise pour le certificat SSL de serveur proxy est les mêmes que celles pour le certificat SSL de serveur de fédération

### <a name="service-communication-certificate"></a>Certificat de Communication du service
Ce certificat n’est pas requis pour la plupart des scénarios ADFS, y compris AD Azure et Office 365. Par défaut, ADFS configure le certificat SSL fourni lors de la configuration initiale en tant que le certificat de communication du service.

**Recommandation:**
- Utiliser le même certificat que vous utilisez pour SSL.  

### <a name="token-signing-certificate"></a>Certificat de signature de jeton
Ce certificat est jetons émise de connexion utilisée pour les parties de confiance, afin que les applications tierces partie de confiance doivent reconnaître le certificat et il est associé à la clé en tant que connus et approuvés. Lorsque les modifications certificat signature jeton, par exemple, lorsqu’il arrive à expiration et que vous configurez un nouveau certificat, toutes les parties de confiance doivent être mis à jour.

**Recommandation:** utiliser ADFS générée par défaut, en interne, des certificats de signature de jetons auto-signé.  

**Configuration requise:**
- Si votre organisation a besoin d’utiliser des certificats de l’infrastructure à clé publique d’entreprise pour la signature de jetons, cela est possible en utilisant le paramètre SigningCertificateThumbprint de l’applet de commande Install-AdfsFarm.
- Si vous utilisez les certificats par défaut généré de manière interne ou inscrits en externe des certificats, lorsque le certificat de signature de jeton est modifié, vous doivent vérifier que toutes les parties de confiance sont mis à jour avec les nouvelles informations de certificat.  Dans le cas contraire, les ouvertures de session à toutes les parties de confiance ne pas mis à jour échoue.

### <a name="token-encryptingdecrypting-certificate"></a>Certificat de chiffrement/déchiffrement de jeton
Ce certificat est utilisé par les fournisseurs de revendications qui chiffrement les jetons émis à ADFS.

**Recommandation:** utiliser ADFS générée par défaut, en interne, des certificats de déchiffrement de jeton auto-signé.  

**Configuration requise:**
- Si votre organisation a besoin d’utiliser des certificats de l’infrastructure à clé publique d’entreprise pour la signature de jetons, cela est possible en utilisant le paramètre DecryptingCertificateThumbprint de l’applet de commande Install-AdfsFarm.
- Si vous utilisez les certificats par défaut généré de manière interne ou inscrits en externe des certificats, lorsque le certificat de déchiffrement de jeton est modifié, vous doivent vérifier que tous les fournisseurs de revendications sont mis à jour avec les nouvelles informations de certificat.  Dans le cas contraire, les ouvertures de session à l’aide d’un des revendications fournisseurs ne pas mis à jour échoue.
  
> [!CAUTION]  
> Les certificats qui sont utilisés pour la signature de token\ et token\-decrypting\/le chiffrement sont essentielles à la stabilité du Service de fédération. La gestion de leurs propres certificats de signature de token\ et token\-decrypting\ et le cryptage de nos clients de vérifier que ces certificats sont sauvegardés et sont disponibles indépendamment pendant un événement de récupération.  

### <a name="user-certificates"></a>Certificats d’utilisateur
- Lorsque vous utilisez x509 authentification des certificats utilisateur avec ADFS, tous les certificats d’utilisateur doit être lié à une autorité de certification racine qui est approuvée par les serveurs ADFS et le Proxy d’Application Web.

## <a name="BKMK_2"></a>Configuration matérielle requise  
ADFS et le Proxy d’Application Web configuration matérielle requise (physique ou virtuel) est contrôlées de processeur, donc vous devez déterminer la taille votre batterie de serveurs pour la capacité de traitement.  
- Utilisez le [feuille de calcul Planification de la capacité ADFS2016](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx) pour déterminer le nombre de serveurs ADFS et le Proxy d’Application Web, vous aurez besoin.

La configuration de mémoire et du disque requise pour ADFS est relativement statique, consultez le tableau ci-dessous:


|**Configuration matérielle requise**|**Configuration minimale requise**|**Configuration recommandée**|
|----- | ----- |-----|
|MÉMOIRE RAM|2GO|4GO |
|Espace disque|32GO|100GO |

**Configuration matérielle requise pour SQLServer**

Si vous utilisez SQLServer pour votre base de données de configuration ADFS, taille de SQLServer selon les recommandations plus simples de SQLServer.  La taille de la base de données ADFS est très faible et ADFS ne place pas une charge de traitement considérable sur l’instance de base de données.  ADFS est le cas, cependant, se connecter à la base de données plusieurs fois lors de l’authentification, la connexion réseau doit être robuste.  Malheureusement, SQL Azure n’est pas prise en charge pour la base de données de configuration ADFS.
  
## <a name="BKMK_3"></a>Configuration requise du proxy  
  
-   Pour l’accès extranet, vous devez déployer le service de rôle Proxy d’Application Web \ - partie du rôle serveur accès à distance. 

-   Serveurs proxy tiers doit prendre en charge le [protocole MS-ADFSPIP](https://msdn.microsoft.com/en-us/library/dn392811.aspx) pour être pris en charge comme un proxy ADFS.  Pour obtenir la liste de 3ème partie fournisseurs voir les [FAQ](AD-FS-FAQ.md#what-third-party-proxies-are-available-for-ad-fs-that-support-ms-adfspip).

-   ADFS2016 nécessite des serveurs Proxy d’Application Web sur Windows Server2016.  Un proxy de niveau inférieur ne peut pas être configuré pour une batterie de serveurs ADFS2016 en cours d’exécution au niveau du comportement de la batterie de serveurs 2016.
  
-   Un serveur de fédération et le service de rôle Proxy d’Application Web ne peut pas être installés sur le même ordinateur.  
  
## <a name="BKMK_4"></a>Configuration requise de ADDS  
**Configuration requise de contrôleur de domaine**  
  
- ADFS requiert des contrôleurs de domaine exécutant Windows Server2008 ou version ultérieure.

- Au moins un contrôleur de domaine Windows Server2016 est requis pour MicrosoftPassport for Work.
  
> [!NOTE]  
> Toute prise en charge pour les environnements avec des contrôleurs de domaine Windows Server2003 est terminée. Visitez [cette page](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO) pour plus d’informations sur la politique de Support Microsoft.  
  
**Configuration requise functional\ au niveau du domaine**  
  
 - Tous les domaines de compte d’utilisateur et le domaine auquel appartiennent les serveurs ADFS doivent être exécutés sur le niveau fonctionnel du domaine de Windows Server2003 ou version ultérieure.  
  
 - Un domaine Windows Server2008 fonctionnel niveau ou une version ultérieure est requis pour l’authentification par certificat client si le certificat est mappé explicitement sur un compte d’utilisateur dans les services ADDS.  
  
**Configuration requise du schéma**  
  
-   Les nouvelles installations d’ADFS2016 nécessitent le schéma ActiveDirectory2016 (version minimale 85).

-   Augmenter le niveau de comportement de la batterie de serveurs ADFS (FBL) au niveau 2016 nécessite le schéma ActiveDirectory2016 (version minimale 85).  

  
**Exigences relatives aux comptes de service**  
  
-   N’importe quel compte de domaine standard peut être utilisé comme compte de service pour ADFS. Comptes de Service géré de groupe sont également pris en charge. Les autorisations requises lors de l’exécution sont ajoutées automatiquement lors de la configuration ADFS.

-   Comptes de service administré de groupe nécessitent au moins un contrôleur de domaine exécutant Windows Server2012 ou version ultérieure.  La fonctionnalité GMSA doit live sous la valeur par défaut "CN = conteneur des comptes de Service administrés.  

-   Pour l’authentification Kerberos, le nom principal de service «`HOST/<adfs\_service\_name>`» doit être enregistré sur le compte de service ADFS. Par défaut, ADFS cela configurera lors de la création d’une nouvelle batterie ADFS.  En cas d’échec, comme dans le cas d’une collision ou des autorisations insuffisantes, vous verrez un avertissement et vous devez l’ajouter manuellement.  
   
**Configuration requise du domaine**  
  
-   Tous les serveurs ADFS doivent être joint à un domaine ADDS.  
  
-   Tous les serveurs ADFS dans une batterie de serveurs doivent être déployés dans le même domaine.  
   
**Configuration requise de forêts multiples**  
  
-   Le domaine auquel appartiennent les serveurs ADFS doit approuver chaque domaine ou une forêt qui contient les utilisateurs de l’authentification sur le service ADFS.  

-   La forêt que le compte de service ADFS est un membre, doit approuver toutes les forêts d’ouverture de session utilisateur. 
  
-   Le compte de service ADFS doit disposer des autorisations nécessaires pour lire les attributs d’utilisateur dans chaque domaine qui contient l’authentification des utilisateurs pour le service ADFS.  
  
## <a name="BKMK_5"></a>Configuration requise de base de données  
Cette section décrit la configuration requise et les restrictions pour les batteries de serveurs ADFS qui utilisent respectivement la base de données interne Windows (WID) ou SQLServer en tant que la base de données:  
  
**WID**  
  
-   Le profil de résolution d’artefacts SAML 2.0 n’est pas pris en charge dans une batterie WID.    

-   Détection de relecture de jetons n’est pas pris en charge d’une batterie WID. (Cette fonctionnalité est uniquement utilisée uniquement dans les scénarios où ADFS agissant en tant que le fournisseur de fédération et utilise des jetons de sécurité à partir de fournisseurs de revendications externe).  
  
Le tableau suivant fournit un résumé du nombre de serveurs ADFS est pris en charge dans un vs WID une batterie SQLServer.    
  
  
|| 1 - 100 partie de confiance (RP) approbations configurées dans ADFS | Plus de 100approbations RP configurées  |
| --- |--- | --- |
|Serveurs de 1-30 ADFS|WID pris en charge|Non pris en charge à l’aide de WID - SQLServer requis |
|Serveurs de plus de 30 ADFS|Non pris en charge à l’aide de WID - SQLServer requis|Non pris en charge à l’aide de WID - SQLServer requis  
  
**SQLServer**  
  
- Pour ADFS dans Windows Server2016, SQLServer2008 et versions ultérieures sont pris en charge.

- Résolution d’artefacts SAML et la détection des relectures sont pris en charge dans une batterie de serveurs SQLServer.  
  
## <a name="BKMK_6"></a>Configuration requise du navigateur  
Lors de l’authentification ADFS est effectuée via un navigateur ou d’un contrôle de navigateur, votre navigateur doit satisfaire aux exigences suivantes:  
  
-   JavaScript doit être activé.  
  
-   Pour l’authentification unique, le navigateur client doit être configuré pour autoriser les cookies  
  
-   Serveur Indication du nom \(SNI\) doivent être pris en charge.  
  
-   Pour le certificat et périphérique authentification des certificats utilisateur, le navigateur doit prendre en charge l’authentification du certificat SSL client  

-   Pour une authentification transparente à l’aide de l’authentification Windows intégrée, le nom du service de fédération (par exemple, https:\/\/fs.contoso.com) doit être configuré dans la zone intranet local ou de la zone sites de confiance.
## <a name="BKMK_7"></a>Configuration réseau requise  
 
**Configuration requise du pare-feu**  
  
Pare-feu situé entre le Proxy d’Application Web et de la batterie de serveurs de fédération et le pare-feu entre les clients et le Proxy d’Application Web doivent avoir le port TCP 443 activé entrant.  
  
En outre, si l’authentification des certificats utilisateur client \ (clientTLS X509 à l’aide de l’authentification utilisateur certificates\) est requis et le point de terminaison certauth sur le port 443 n’est pas activée, ADFS2016 nécessite que le port TCP49443 soit activé entrant sur le pare-feu entre les clients et le Proxy d’Application Web. Cela n'est pas requis sur le pare-feu entre le Proxy d’Application Web et la fédération servers\). 

Pour plus d’informations sur le port hybride voir Configuration requise [protocoles et Ports d’identité hybride](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-ports). 

Pour plus d’informations, voir [meilleures pratiques pour la sécurisation d’ActiveDirectory Federation Services](..\deployment\Best-Practices-Securing-AD-FS.md)
  
**Configuration DNS requise**  
  
-   Pour l’accès intranet, tous les clients l’accès au service ADFS dans le réseau d’entreprise interne \(intranet\) doivent être en mesure de résoudre le nom du service ADFS pour l’équilibrage de charge pour les serveurs ADFS ou le serveur ADFS.  
  
-   Pour l’accès extranet, tous les clients l’accès au service ADFS à partir en dehors du réseau d’entreprise \(extranet\/internet\) doivent être en mesure de résoudre le nom du service ADFS pour l’équilibrage de charge pour les serveurs Proxy d’Application Web ou le serveur de Proxy d’Application Web.  
  
-   Chaque serveur Proxy d’Application Web dans le réseau de périmètre doit être en mesure de résoudre le nom du service ADFS pour l’équilibrage de charge pour les serveurs ADFS ou le serveur ADFS. Cela peut être obtenue à l’aide d’un autre serveur DNS dans le réseau DMZ ou en modifiant la résolution de serveur local à l’aide du fichier HOSTS.  
  
-   Pour l’authentification Windows intégrée, vous devez utiliser un DNS A enregistrement \(not CNAME\) pour le nom de service de fédération.  

-   Pour l’authentification des certificats utilisateur sur le port 443, «certauth. \ < nom\ du service de fédération >» doivent être configurés dans DNS pour convertir le serveur de fédération ou le proxy d’application web.

-   Pour l’inscription de l’appareil ou l’authentification moderne sur les ressources locales à l’aide les clients antérieurs à Windows10, «inscriptionentreprise. \ < nom d’utilisateur principal suffix\ >», pour chaque suffixe UPN en cours d’utilisation dans votre organisation, doit être configuré pour résoudre le serveur de fédération ou le proxy d’application web.

**Contraintes de l’équilibrage de charge**
- L’équilibrage de charge ne doit pas se terminer SSL. ADFS prend en charge plusieurs cas d’utilisation avec l’authentification par certificat bloque lors de l’arrêt SSL. Fin d’exécution à l’équilibrage de charge SSL n’est pas prise en charge pour les cas d’utilisation. 
- Il est recommandé d’utiliser un équilibrage de charge qui prend en charge la fonctionnalité SNI. Dans le cas contraire, à l’aide de la liaison de secours 0.0.0.0 sur ADFS / serveur Proxy d’Application Web doit fournir une solution de contournement.
- Il est recommandé d’utiliser les points de terminaison HTTP (HTTPS pas) d’intégrité sonde pour effectuer des vérifications d’intégrité d’équilibrage de charge le routage du trafic. Cela permet d’éviter les problèmes liés à la fonctionnalité SNI. La réponse à ces points de terminaison sonde est un OK de 200 HTTP et est pris en charge localement avec aucune dépendance vis-à-vis des services principaux. La sonde HTTP sont accessibles sur HTTP en utilisant le chemin d’accès "sonde/adfs /»
    - http://&lt;nom du Proxy d’Application Web&gt;/adfs/sonde
    - http://&lt;nom de serveur ADFS&gt;/adfs/sonde
    - http://&lt;adresse IP de Proxy d’Application Web&gt;/adfs/sonde
    - http://&lt;adresse IP ADFS&gt;/adfs/sonde
- Il n’est pas recommandé d’utiliser DNS alternée comme un moyen pour équilibrer la charge. À l’aide de ce type d’équilibrage de charge ne fournit pas une méthode automatique pour supprimer un nœud de l’équilibrage de charge à l’aide des sondes d’intégrité. 
- Il n’est pas recommandé d’utiliser l’affinité de session basé sur IP ou des sessions persistantes pour le trafic d’authentification pour ADFS au sein de l’équilibrage de charge. Cela peut entraîner une surcharge de certains nœuds lors de l’utilisation du protocole d’authentification hérité pour les clients de messagerie pour se connecter à des services de messagerie Office365 (Exchange Online). 

## <a name="BKMK_13"></a>Autorisations requises  
L’administrateur qui effectue l’installation et la configuration initiale d’ADFS doit disposer des autorisations d’administrateur local sur le serveur ADFS.  Si l’administrateur local ne dispose pas des autorisations pour créer des objets dans ActiveDirectory, ils doivent tout d’abord avoir un administrateur de domaine créer les objets ActiveDirectory requis, puis configurez la batterie ADFS à l’aide du paramètre AdminConfiguration.  
  
  

