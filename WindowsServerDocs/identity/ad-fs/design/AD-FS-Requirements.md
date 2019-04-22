---
ms.assetid: 8ce6e7c4-cf8e-4b55-980c-048fea28d50f
title: Batterie de serveurs de fédération utilisant SQL Server
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1310792158995608e8f477b6df9d6cf7c0284571
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822250"
---
# <a name="ad-fs-requirements"></a>Configuration AD FS requise

>S'applique à : Windows Server 2012 R2

Voici les différentes exigences que vous devez vous conformer lors du déploiement d’AD FS :  
  
-   [Conditions requises pour les certificats](AD-FS-Requirements.md#BKMK_1)  
  
-   [Configuration matérielle requise](AD-FS-Requirements.md#BKMK_2)  
  
-   [Configuration logicielle requise](AD-FS-Requirements.md#BKMK_3)  
  
-   [Requise pour AD DS](AD-FS-Requirements.md#BKMK_4)  
  
-   [Exigences de base de données de configuration](AD-FS-Requirements.md#BKMK_5)  
  
-   [Configuration requise du navigateur](AD-FS-Requirements.md#BKMK_6)  
  
-   [Exigences de l’extranets](AD-FS-Requirements.md#BKMK_extranet)  
  
-   [Configuration réseau requise](AD-FS-Requirements.md#BKMK_7)  
  
-   [Exigences des attributs store](AD-FS-Requirements.md#BKMK_8)  
  
-   [Exigences de l’application](AD-FS-Requirements.md#BKMK_9)  
  
-   [Exigences d’authentification](AD-FS-Requirements.md#BKMK_10)  
  
-   [Configuration requise de Workplace join](AD-FS-Requirements.md#BKMK_11)  
  
-   [Exigences de chiffrement](AD-FS-Requirements.md#BKMK_12)  
  
-   [Exigences relatives aux autorisations](AD-FS-Requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>Configuration requise des certificats  
Les certificats jouent le rôle essentiel dans la sécurisation des communications entre la fédération des revendications de serveurs, les proxys d’Application Web,\-prenant en charge les applications et les clients Web. Les exigences relatives aux certificats varient, selon que vous configurez un serveur de fédération ou d’un ordinateur proxy, comme décrit dans cette section.  
  
**Certificats de serveur de fédération**  
  
|||  
|-|-|  
|**Type de certificat**|**Configuration requise, prise en charge & choses à savoir**|  
|**Secure Sockets Layer \(SSL\) certificat :** Il s’agit d’un certificat SSL standard utilisé pour sécuriser les communications entre les clients et serveurs de fédération.|-Ce certificat doit être approuvé publiquement\* X509 certificat v3.<br />-Tous les clients qui accèdent à n’importe quel point de terminaison AD FS doivent approuver ce certificat. Il est fortement recommandé d’utiliser les certificats sont émis par un public \(troisième\-tiers\) autorité de certification \(autorité de certification\). Vous pouvez utiliser un libre-service\-signé SSL certificat avec succès sur serveurs de fédération dans un environnement de laboratoire de test. Toutefois, pour un environnement de production, nous recommandons que vous obtenez le certificat à partir d’une autorité de certification publique.<br />-Prend en charge de n’importe quelle taille de clé prises en charge par Windows Server 2012 R2 pour les certificats SSL.<br />-Ne pas en charge les certificats qui utilisent des clés CNG.<br />-Quand il est utilisé avec la jonction\/Device Registration Service, l’autre nom du sujet du certificat SSL pour le service AD FS doit contenir l’enterpriseregistration de valeur qui est suivie par le nom d’utilisateur Principal \(UPN\) suffixe de votre organisation, par exemple, enterpriseregistration.contoso.com.<br />-Certificats génériques sont pris en charge. Lorsque vous créez votre batterie AD FS, vous devrez fournir le nom du service pour le service AD FS \(, par exemple, **adfs.contoso.com**.<br />-Il est fortement recommandé d’utiliser le même certificat SSL pour le Proxy d’Application Web. Il s’agit toutefois **requis** soit identique lors de la prise en charge des points de terminaison de l’authentification intégrée de Windows via le Proxy d’Application Web et de l’authentification de Protection étendue est activée \(le paramètre par défaut\).<br />-Le nom du sujet de ce certificat est utilisé pour représenter le nom de Service de fédération pour chaque instance d’AD FS que vous déployez. Pour cette raison, vous souhaiterez choisir un nom de sujet sur toute nouvelle autorité de certification\-a émis les certificats que le nom de votre entreprise ou organisation aux partenaires représente le mieux.<br />    L’identité du certificat doit correspondre au nom de service de fédération \(par exemple, fs.contoso.com\). L’identité est une extension autre nom de sujet de type dNSName ou, s’il n’y a aucune entrée d’autre nom de sujet, le nom du sujet spécifié comme nom commun. Plusieurs entrées de l’autre nom de sujet peuvent être présentes dans le certificat, fournie par un d’eux correspond au nom de service de fédération.<br />-   **Important :** il est fortement recommandé d’utiliser le même certificat SSL sur tous les nœuds de votre batterie de serveurs AD FS, ainsi que tous les proxys d’Application Web dans votre batterie de serveurs AD FS.|  
|**Certificat de communication du service :** Ce certificat active la sécurité de message WCF pour sécuriser les communications entre les serveurs de fédération.|-Par défaut, le certificat SSL est utilisé en tant que certificat de communication du service.  Mais vous avez également la possibilité de configurer un autre certificat comme certificat de communication de service.<br />-   **Important :** si vous utilisez le certificat SSL que le certificat de communication de service, lorsque le certificat SSL arrive à expiration, veillez à configurer le certificat SSL renouvelé en tant que votre certificat de communication du service. Cela n’est pas automatique.<br />-Ce certificat doit être approuvé par les clients des services AD FS qui utilisent la sécurité de Message WCF.<br />-Nous vous recommandons d’utiliser un certificat d’authentification serveur émis par un public \(troisième\-tiers\) autorité de certification \(autorité de certification\).<br />-Le certificat de communication du service ne peut pas être un certificat qui utilise des clés CNG.<br />-Ce certificat peut être géré à l’aide de la console de gestion AD FS.|  
|**Jeton\-certificat de signature :** Certificat X509 standard qui permet de signer de manière sécurisée tous les jetons émis par le serveur de fédération.|-Par défaut, AD FS crée un libre-service\-signé le certificat avec des clés de 2 048 bits.<br />-Certificats d’autorité de certification émise sont également prises en charge et peut être modifiés à l’aide du composant logiciel enfichable Gestion AD FS\-dans<br />-Autorité de certification a émis les certificats doive être stockée & accessibles via un fournisseur de chiffrement de CSP.<br />-Le certificat de signature de jeton ne peut pas être un certificat qui utilise des clés CNG.<br />-AD FS ne nécessite pas de certificats inscrits en externe pour la signature de jetons.<br />    AD FS renouvelle automatiquement ces self\-certificats auto-signés avant leur expiration, tout d’abord configurer les nouveaux certificats en tant que certificat secondaire pour permettre aux partenaires de les utiliser, puis Retournement vers le site principal dans un processus appelé automatique substitution de certificat. Nous vous recommandons d’utiliser la valeur par défaut, les certificats générés automatiquement pour la signature de jetons.<br />    Si votre organisation a des stratégies qui exigent des certificats différents d’être configuré pour la signature de jetons, vous pouvez spécifier les certificats au moment de l’installation à l’aide de Powershell \(utiliser le paramètre – SigningCertificateThumbprint de l’installation \-Applet de commande AdfsFarm\).  Après l’installation, vous pouvez afficher et gérer des certificats de signatures de jeton à l’aide de la console de gestion AD FS ou d’un ensemble d’applets de commande Powershell\-AdfsCertificate et Get\-AdfsCertificate.<br />    Lorsque les certificats inscrits en externe sont utilisés pour la signature de jetons, AD FS n’effectue pas le renouvellement automatique des certificats ou substitution.  Ce processus doit être effectué par un administrateur.<br />    Pour permettre la substitution de certificat quand un certificat est point d’expirer, un certificat de signature de jeton secondaire peut être configuré dans AD FS. Par défaut, tous les certificats de signatures de jeton sont publiés dans les métadonnées de fédération, mais uniquement le jeton principal\-certificat de signature est utilisé par AD FS pour la signature des jetons.|  
|**Jeton\-déchiffrement\/certificat de chiffrement :** Il s’agit d’un standard X509 certificat qui est utilisée pour déchiffrer\/chiffrer les jetons entrants. Il est également publié dans les métadonnées de fédération.|-Par défaut, AD FS crée un libre-service\-signé le certificat avec des clés de 2 048 bits.<br />-Certificats d’autorité de certification émise sont également prises en charge et peut être modifiés à l’aide du composant logiciel enfichable Gestion AD FS\-dans<br />-Autorité de certification a émis les certificats doive être stockée & accessibles via un fournisseur de chiffrement de CSP.<br />-Le jeton\-déchiffrement\/certificat de chiffrement ne peut pas être un certificat qui utilise des clés CNG.<br />-Par défaut, AD FS génère et utilise son propre, en interne généré- and -self\-signé des certificats pour le déchiffrement de jetons.  AD FS ne nécessite pas de certificats inscrits en externe à cet effet.<br />    En outre, AD FS renouvelle automatiquement ces self\-certificats auto-signés avant leur expiration.<br />    **Nous vous recommandons d’utiliser la valeur par défaut, les certificats générés automatiquement pour le déchiffrement de jetons.**<br />    Si votre organisation a des stratégies qui exigent des certificats différents d’être configuré pour le déchiffrement de jeton, vous pouvez spécifier les certificats au moment de l’installation à l’aide de Powershell \(utiliser le paramètre – DecryptionCertificateThumbprint de la Installer\-applet de commande AdfsFarm\).  Après l’installation, vous pouvez afficher et gérer des certificats de déchiffrement de jeton à l’aide de la console de gestion AD FS ou d’un ensemble d’applets de commande Powershell\-AdfsCertificate et Get\-AdfsCertificate.<br />    **Lorsque les certificats inscrits en externe sont utilisés pour le déchiffrement de jetons, AD FS n’effectue pas de renouvellement automatique des certificats.  Ce processus doit être effectué par un administrateur**.<br />-Le compte de service AD FS doit avoir accès au jeton\-signature de clé privée du certificat dans le magasin personnel de l’ordinateur local. Cela est pris en charge par le programme d’installation. Vous pouvez également utiliser le composant logiciel enfichable Gestion AD FS\-pour vous assurer de cet accès si vous modifiez ultérieurement le jeton\-certificat de signature.|  
  
> [!CAUTION]  
> Les certificats sont utilisés pour le jeton\-signature et le jeton\-déchiffrement\/chiffrer sont essentielles à la stabilité du Service de fédération. Les clients qui gèrent leur propre jeton\-signature & jeton\-déchiffrement\/certificats de chiffrement doit garantir que ces certificats sont sauvegardés et ne sont disponibles indépendamment pendant un événement de récupération.  
  
> [!NOTE]  
> Dans AD FS, vous pouvez modifier l’algorithme de hachage sécurisé \(SHA\) niveau qui est utilisé pour les signatures numériques pour soit SHA\-1 ou SHA\-256 \(plus sécurisées\). AD FS ne prend pas en charge l’utilisation de certificats avec d’autres méthodes de hachage, telles que MD5 \(l’algorithme de hachage par défaut qui est utilisé avec la commande Makecert.exe\-outil en ligne\). Pour des raisons de sécurité, nous vous recommandons d’utiliser SHA\-256 \(qui a la valeur par défaut\) pour toutes les signatures. SHA\-1 est recommandé d’utiliser uniquement dans les scénarios où vous devez interagir avec un produit qui ne prend pas en charge les communications à l’aide de SHA\-256, par exemple un non\-versions de produit ou hérité de Microsoft d’AD FS.  
  
> [!NOTE]  
> Après avoir reçu un certificat d'une autorité de certification, assurez-vous que tous les certificats sont importés dans le magasin de certificats personnels de l'ordinateur local. Vous pouvez importer des certificats dans le magasin personnel avec le composant logiciel enfichable MMC Certificats\-dans.  
  
## <a name="BKMK_2"></a>Configuration matérielle requise  
Les conditions matérielles minimales et recommandées suivantes s’appliquent aux serveurs de fédération AD FS dans Windows Server 2012 R2 :  
  
||||  
|-|-|-|  
|**Configuration matérielle requise**|**Configuration minimale requise**|**Configuration recommandée**|  
|Vitesse du processeur|1,4 GHz 64\-bit du processeur|Quadruple\-cœur, 2 GHz|  
|RAM|512 Mo|4 Go|  
|Espace disque|32 Go|100 GO|  
  
## <a name="BKMK_3"></a>Configuration logicielle requise  
Les exigences d’AD FS suivantes concernent les fonctionnalités de serveur intégré dans le système d’exploitation Windows Server® 2012 R2 :  
  
-   Pour l’accès extranet, vous devez déployer le service de rôle Proxy d’Application Web \- fait partie du rôle de serveur d’accès à distance de Windows Server® 2012 R2. Les versions antérieures d’un serveur proxy de fédération ne sont pas pris en charge avec AD FS dans Windows Server® 2012 R2.  
  
-   Un serveur de fédération et le service de rôle Proxy d’Application Web ne peut pas être installés sur le même ordinateur.  
  
## <a name="BKMK_4"></a>Requise pour AD DS  
**Exigences de contrôleur de domaine**  
  
Contrôleurs de domaine dans tous les domaines de l’utilisateur et le domaine auquel appartiennent les serveurs AD FS doivent être en cours d’exécution Windows Server 2008 ou version ultérieure.  
  
> [!NOTE]  
> Prise en charge tous les environnements avec des contrôleurs de domaine Windows Server 2003 se termine après l’étendus prennent en charge End Date pour Windows Server 2003. Les clients sont fortement recommandées pour mettre à niveau les contrôleurs de domaine dès que possible. Visitez [cette page](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO) pour plus d’informations sur la politique de Support Microsoft. Pour les problèmes découverts qui sont spécifiques aux environnements de contrôleur de domaine Windows Server 2003, des correctifs seront émis uniquement pour les problèmes de sécurité et si un correctif peut être émis avant l’expiration du Support étendu pour Windows Server 2003.  
  
**Domaine fonctionnel\-des exigences de niveau**  
  
Tous les domaines de compte d’utilisateur et le domaine auquel appartiennent les serveurs AD FS doivent être exécutés sur le niveau fonctionnel du domaine de Windows Server 2003 ou version ultérieure.  
  
La plupart des fonctionnalités AD FS ne nécessitent pas les services AD DS fonctionnels\-niveau des modifications pour fonctionner correctement. Toutefois, le niveau fonctionnel de domaine Windows Server 2008 ou supérieur est nécessaire au bon fonctionnement de l'authentification de certificat client si le certificat est mappé explicitement sur le compte d'un utilisateur dans AD DS.  
  
**Spécifications de schéma**  
  
-   AD FS ne nécessite pas de modifications de schéma ou fonctionnel\-niveau dans les services AD DS.  
  
-   Pour utiliser la fonctionnalité de jonction, vous devez définir le schéma de la forêt de serveurs AD FS sont joints vers Windows Server 2012 R2.  
  
**Exigences relatives aux comptes de service**  
  
-   N’importe quel compte de service standard peut être utilisé comme compte de service pour AD FS. Les comptes de Service géré de groupe sont également prises en charge. Cela nécessite au moins un contrôleur de domaine \(il est recommandé de déployer deux ou plusieurs\) qui exécute Windows Server 2012 ou version ultérieure.  
  
-   Pour l’authentification Kerberos fonctionne entre le domaine\-joint les clients et les services AD FS, le « hôte\/< adfs\_service\_nom >' doit être inscrit en tant qu’un SPN sur le compte de service. Par défaut, AD FS cela configurera lors de la création d’une nouvelle batterie de serveurs AD FS si elle dispose des autorisations suffisantes pour effectuer cette opération.  
  
-   Le compte de service AD FS doit être approuvé dans chaque domaine de l’utilisateur qui contient les utilisateurs s’authentifiant au service AD FS.  
  
**Configuration requise de domaine**  
  
-   Tous les serveurs AD FS doivent être joint à un domaine AD DS.  
  
-   Tous les serveurs AD FS dans une batterie de serveurs doivent être déployés dans un domaine unique.  
  
-   Le domaine que les serveurs AD FS sont joints au doit approuver chaque domaine de compte d’utilisateur qui contient les utilisateurs s’authentifiant auprès du service AD FS.  
  
**Exigences de plusieurs forêts**  
  
-   Le domaine que les serveurs AD FS sont joints au doit approuver chaque domaine de compte d’utilisateur ou d’une forêt qui contient les utilisateurs s’authentifiant auprès du service AD FS.  
  
-   Le compte de service AD FS doit être approuvé dans chaque domaine de l’utilisateur qui contient les utilisateurs s’authentifiant au service AD FS.  
  
## <a name="BKMK_5"></a>Exigences de base de données de configuration  
Voici la configuration requise et restrictions qui s’appliquent selon le type de magasin de configuration :  
  
**WID**  
  
-   Une batterie de serveurs WID a une limite de 30 serveurs de fédération si vous avez 100 ou moins de confiance.  
  
-   Profil de résolution d’artefact dans SAML 2.0 n’est pas pris en charge dans la base de données de configuration WID.  Détection de relecture de jeton n’est pas pris en charge dans la base de données de configuration WID. Cette fonctionnalité est uniquement utilisée uniquement dans les scénarios où AD FS est agissant comme fournisseur de fédération et consommer des jetons de sécurité provenant de fournisseurs de revendications externe.  
  
-   Déploiement de serveurs AD FS dans les données distincts d’un centre de basculement ou l’équilibrage de charge géographique est prise en charge tant que le nombre de serveurs ne dépasse pas 30.  
  
Le tableau suivant fournit un résumé de l’aide d’une batterie de serveurs WID.  Il permet de planifier votre implémentation.  
  
||||  
|-|-|-|  
||1 \- 100 approbations de partie de confiance|Plus de 100 approbations de partie de confiance|  
|1 \- 30 AD FS nœuds|WID pris en charge|Non pris en charge à l’aide de WID \- SQL requis|  
|Nœuds de plus de 30 AD FS|Non pris en charge à l’aide de WID \- SQL requis|Non pris en charge à l’aide de WID \- SQL requis|  
  
**SQL Server**  
  
Pour AD FS dans Windows Server 2012 R2, vous pouvez utiliser SQL Server 2008 et versions ultérieures  
  
## <a name="BKMK_6"></a>Configuration requise du navigateur  
Lors de l’authentification AD FS est effectuée via un navigateur ou un contrôle de navigateur, votre navigateur doit être conforme aux exigences suivantes :  
  
-   JavaScript doit être activé.  
  
-   Les cookies doivent être activés  
  
-   Indication du nom du serveur \(SNI\) doivent être pris en charge  
  
-   Pour l’authentification par certificat utilisateur certificate & appareil \(fonctionnalités de jointure workplace\), le navigateur doit prendre en charge l’authentification de certificat du client SSL  
  
Plusieurs plateformes et navigateurs clés ont subi validation pour le rendu et les fonctionnalités dont les détails sont répertoriées ci-dessous. Navigateurs et appareils non traitées dans cette table sont toujours pris en charge si elles répondent aux spécifications présentées ci-dessus :  
  
|||  
|-|-|  
|**Navigateurs**|**Plateformes**|  
|INTERNET EXPLORER 10.0|Windows 7, Windows 8.1, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2|  
|IE 11.0|Windows 7, Windows 8.1, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2|  
|Service Broker d’authentification Web Windows|Windows 8.1|  
|Firefox \[v21\]|Windows 7, Windows 8.1|  
|Safari \[v7\]|iOS 6, Mac OS\-X 10.7|  
|Chrome \[v27\]|Windows 7, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Mac OS\-X 10.7|  
  
> [!IMPORTANT]  
> Problème connu \- Firefox : Fonctionnalité de jointure de poste de travail qui identifie l’appareil à l’aide du certificat de l’appareil n’est pas fonctionnelle sur les plateformes Windows. Firefox ne gère pas actuellement les performance SSL authentification par certificat client à l’aide de certificats configurés dans le magasin de certificats utilisateur sur les clients Windows.  
  
**Cookies**  
  
AD FS crée la session\-cookies en et persistants qui doivent être stockées sur les ordinateurs clients pour fournir l’authentification\-, se connecter\-, l’authentification unique\-sur \(SSO\)et d’autres fonctionnalités. Le navigateur client doit donc être configuré de manière à accepter les cookies. Les cookies qui sont utilisés pour l’authentification sont toujours Secure Hypertext Transfer Protocol \(HTTPS\) les cookies de session qui sont écrites pour le serveur d’origine. Si le navigateur client n'est pas configuré de manière à autoriser ces cookies, AD FS ne peut pas fonctionner correctement. Les cookies persistants permettent de conserver le fournisseur de revendications choisi par l'utilisateur. Vous pouvez les désactiver à l’aide d’un paramètre de configuration dans le fichier de configuration pour l’authentification AD FS\-dans les pages. Prise en charge de TLS\/SSL est requis pour des raisons de sécurité.  
  
## <a name="BKMK_extranet"></a>Exigences de l’extranets  
Pour fournir l’accès extranet pour le service AD FS, vous devez déployer le service de rôle Proxy d’Application Web en tant que le rôle accessible sur extranet qui transmet les demandes d’authentification à le de manière sécurisée au service AD FS. Cela fournit l’isolation des points de terminaison du service AD FS, ainsi que l’isolation de toutes les clés de sécurité \(tels que des certificats de signature de jetons\) à partir des demandes qui proviennent d’internet. En outre, les fonctionnalités telles que le verrouillage de compte Extranet réversible nécessitent l’utilisation du Proxy d’Application Web. Pour plus d’informations sur le Proxy d’Application Web, consultez [Proxy d’Application Web](https://technet.microsoft.com/library/dn584107.aspx).  
  
Si vous souhaitez utiliser une troisième\-tiers proxy pour l’accès extranet, ce troisième\-proxy de tiers doit prendre en charge le protocole défini dans [http :\/\/download.microsoft.com\/télécharger\/9\/5\/E\/95EF66AF\-9026\-4BB0\-A41D\-A4F81802D92C\/% 5bMS\-ADFSPIP%5d.pdf](https://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/%5bMS-ADFSPIP%5d.pdf).  
  
## <a name="BKMK_7"></a>Configuration réseau requise  
Configuration des services réseau suivants correctement est essentielle pour réussir le déploiement d’AD FS dans votre organisation :  
  
**Configuration du pare-feu d’entreprise**  
  
Le pare-feu situé entre le Proxy d’Application Web et la batterie de serveurs de fédération et le pare-feu entre les clients et le Proxy d’Application Web doit avoir le port TCP 443 activé entrant.  
  
En outre, si l’authentification du certificat utilisateur client \(authentification clientTLS à l’aide de X509 certificats utilisateur\) est requise, AD FS dans Windows Server 2012 R2 nécessite l’activation de port TCP 49443 entrant sur le pare-feu entre le les clients et le Proxy d’Application Web. Cela n’est pas requis sur le pare-feu entre le Proxy d’Application Web et les serveurs de fédération\).  
  
**Configuration du service DNS**  
  
-   Pour l’accès intranet, tous les clients l’accès à AD FS du service au sein du réseau d’entreprise interne \(intranet\) doit être en mesure de résoudre le nom du service AD FS \(nom fourni par le certificat SSL\) à la charge équilibrage pour les serveurs AD FS ou le serveur AD FS.  
  
-   Service pour l’accès extranet, tous les clients l’accès à AD FS à partir de l’extérieur du réseau d’entreprise \(extranet\/internet\) doit être en mesure de résoudre le nom du service AD FS \(nom fourni par le certificat SSL\) à l’équilibreur de charge pour les serveurs Proxy d’Application Web ou le serveur de Proxy d’Application Web.  
  
-   Pour l’accès extranet fonctionner correctement, chaque serveur de Proxy d’Application Web dans la zone DMZ doit être en mesure de résoudre le nom du service AD FS \(nom fourni par le certificat SSL\) à l’équilibreur de charge pour les serveurs AD FS ou le serveur AD FS. Vous pouvez le faire à l’aide d’un autre serveur DNS dans le réseau de zone DMZ ou en modifiant la résolution de serveur local à l’aide du fichier HOSTS.  
  
-   Pour l’authentification Windows intégrée fonctionne au sein du réseau et en dehors du réseau pour un sous-ensemble de points de terminaison exposés par le biais du Proxy d’Application Web, vous devez utiliser un enregistrement A \(pas CNAME\) pour pointer vers les équilibreurs de charge.  
  
Pour plus d’informations sur la configuration DNS d’entreprise pour le service de fédération et Device Registration Service, consultez [configurer le DNS d’entreprise pour le Service de fédération et DRS](https://technet.microsoft.com/library/dn486786.aspx).  
  
Pour plus d’informations sur la configuration DNS d’entreprise pour les proxys d’Application Web, consultez la section « Configurer des serveurs DNS » dans [étape 1 : Configurer l’Infrastructure de Proxy d’Application Web](https://technet.microsoft.com/library/dn383644.aspx).  
  
Pour plus d’informations sur la façon de configurer une adresse IP de cluster ou cluster de nom de domaine complet à l’aide de NLB, voir définition des paramètres Cluster au [http :\/\/go.microsoft.com\/fwlink\/? LinkId\=75282](https://go.microsoft.com/fwlink/?LinkId=75282).  
  
## <a name="BKMK_8"></a>Exigences des attributs store  
AD FS nécessite au moins un magasin d’attributs à utiliser pour l’authentification des utilisateurs et l’extraction des revendications de sécurité pour ces utilisateurs. Pour une liste d’attributs magasins qu’AD FS prend en charge, consultez [The Role of Attribute Stores](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md).  
  
> [!NOTE]  
> AD FS crée automatiquement un magasin d’attributs « Active Directory », par défaut. Exigences des attributs store varient selon si votre organisation est agissant en tant que le partenaire de compte \(hébergeant les utilisateurs fédérés\) ou le partenaire de ressource \(hébergeant l’application fédérée\).  
  
**Magasins d’attributs LDAP**  
  
Lorsque vous travaillez avec les autres Lightweight Directory Access Protocol \(LDAP\)\-en fonction des magasins d’attributs, vous devez vous connecter à un serveur LDAP qui prend en charge l’authentification Windows intégrée. En outre, la chaîne de connexion LDAP doit être écrite sous la forme d'une URL LDAP, comme indiqué dans le document RFC 2255.  
  
Il est également obligatoire que le compte de service pour le service AD FS a le droit d’extraire les informations utilisateur dans le magasin d’attributs LDAP.  
  
**Magasins d’attributs SQL Server**  
  
Pour AD FS dans Windows Server 2012 R2 pour fonctionner correctement, les ordinateurs qui hébergent le magasin d’attributs SQL Server doivent être en cours d’exécution soit Microsoft SQL Server 2008 ou version ultérieure. Lorsque vous travaillez avec SQL\-en fonction des magasins d’attributs, vous devez également configurer une chaîne de connexion.  
  
**Magasins d’attributs personnalisés**  
  
Vous pouvez développer des magasins d'attributs personnalisés dans le cadre de scénarios avancés.  
  
-   Le langage de stratégie intégré à AD FS peut référencer des magasins d'attributs personnalisés de manière à perfectionner les scénarios suivants :  
  
    -   Créer des revendications pour un utilisateur authentifié localement  
  
    -   Compléter les revendications pour un utilisateur authentifié de manière externe  
  
    -   Autoriser un utilisateur à obtenir un jeton  
  
    -   Autoriser un service à obtenir un jeton au nom d'un utilisateur  
  
    -   Émission de données supplémentaires dans les jetons de sécurité émis par AD FS pour les parties de confiance.  
  
-   Tous les magasins d’attributs personnalisés doivent être créées sur .NET 4.0 ou version ultérieure.  
  
Lorsque vous travaillez avec un magasin d’attributs personnalisés, vous devrez peut-être également configurer une chaîne de connexion. Dans ce cas, vous pouvez entrer un code personnalisé de votre choix qui permet une connexion à votre magasin d’attributs personnalisés. La chaîne de connexion dans cette situation est un ensemble de nom\/paires sont interprétées comme étant implémentées par le développeur du magasin d’attributs personnalisés de valeurs. Pour plus d’informations sur le développement et à l’aide des magasins d’attributs personnalisés, consultez [vue d’ensemble du Store attributs](https://go.microsoft.com/fwlink/?LinkId=190782).  
  
## <a name="BKMK_9"></a>Exigences de l’application  
AD FS prend en charge les revendications\-prenant en charge les applications qui utilisent les protocoles suivants :  
  
-   WS\-fédération  
  
-   WS\-d’approbation  
  
-   Protocole SAML 2.0 à l’aide de profils IDPLite, SPLite & eGov1.5.  
  
-   Profil de l’octroi d’autorisation OAuth 2.0  
  
AD FS prend également en charge l’authentification et l’autorisation pour les non\-revendications\-prenant en charge les applications qui sont prises en charge par le Proxy d’Application Web.  
  
## <a name="BKMK_10"></a>Exigences d’authentification  
**L’authentification AD DS \(l’authentification principale\)**  
  
Pour l’accès intranet, les mécanismes d’authentification standard suivants pour les services AD DS sont prises en charge :  
  
-   Authentification intégrée de Windows à l’aide de Negotiate pour Kerberos et NTLM  
  
-   À l’aide du nom d’utilisateur de l’authentification de formulaires\/les mots de passe  
  
-   Authentification de certificat à l’aide de certificats mappés aux comptes d’utilisateur dans AD DS  
  
Pour l’accès extranet, les mécanismes d’authentification suivantes sont prises en charge :  
  
-   À l’aide du nom d’utilisateur de l’authentification de formulaires\/les mots de passe  
  
-   Authentification de certificat à l’aide de certificats qui sont mappés aux comptes d’utilisateur dans AD DS  
  
-   L’authentification intégrée de Windows à l’aide de Negotiate \(NTLM uniquement\) de WS\-approuver les points de terminaison qui acceptent l’authentification intégrée Windows.  
  
L’authentification par certificat :  
  
-   S’étend aux cartes à puce qui peuvent être protégé de code confidentiel.  
  
-   L’interface graphique utilisateur pour l’utilisateur à entrer son code pin n’est pas fourni par AD FS et est nécessaire pour faire partie du système d’exploitation client qui s’affiche lorsque vous utilisez le client TLS.  
  
-   Le lecteur et le fournisseur de services de chiffrement \(CSP\) pour la carte à puce doivent fonctionner sur l’ordinateur où se trouve le navigateur.  
  
-   Le certificat de carte à puce doit être lié à une racine approuvée sur tous les serveurs AD FS et les serveurs Proxy d’Application Web.  
  
-   Le certificat doit être mappé au compte d'utilisateur dans AD DS à l'aide de l'une des méthodes suivantes :  
  
    -   Le nom du sujet du certificat correspond au nom unique LDAP d'un compte d'utilisateur dans AD DS.  
  
    -   L’extension d’altname de sujet de certificat possède le nom d’utilisateur principal \(UPN\) d’un compte d’utilisateur dans AD DS.  
  
Pour l’authentification Windows intégrée transparente à l’aide de Kerberos dans l’intranet,  
  
-   Il est requis pour le nom du service doit appartenir à des Sites de confiance ou sites Intranet locaux.  
  
-   En outre, l’hôte\/< adfs\_service\_nom > SPN doit être définie sur le compte de service que la batterie de serveurs AD FS s’exécute sur.  
  
**Plusieurs\-facteur d’authentification**  
  
AD FS prend en charge une authentification supplémentaire \(au-delà de l’authentification principale pris en charge par les services AD DS\) en utilisant un modèle de fournisseur par laquelle fournisseurs\/les clients peuvent créer leur propres multi\-adaptateur d’authentification multifacteur qu’un administrateur peut inscrire et utiliser lors de la connexion.  
  
Chaque adaptateur MFA doit être créé sur le .NET 4.5.  
  
Pour plus d’informations sur l’authentification Multifacteur, consultez [gérer les risques avec une authentification multifacteur supplémentaire pour les Applications sensibles](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).  
  
**Authentification des appareils**  
  
AD FS prend en charge l’authentification des appareils à l’aide de certificats configurés par le Service d’inscription de périphérique lors de l’action consistant à un espace de travail utilisateur final joindre leur appareil.  
  
## <a name="BKMK_11"></a>Configuration requise de Workplace join  
Les utilisateurs finaux peuvent jonction leurs appareils à une organisation à l’aide d’AD FS. Cela est pris en charge par le Service de l’inscription d’appareils dans AD FS. Par conséquent, les utilisateurs finaux obtiennent l’avantage supplémentaire de l’authentification unique entre les applications prises en charge par AD FS. En outre, les administrateurs peuvent gérer les risques en limitant l’accès aux applications uniquement aux appareils qui ont été joints à l’organisation. Voici les conditions suivantes pour activer ce scénario.  
  
-   AD FS prend en charge la jonction d’espace pour Windows 8.1 et iOS 5\+ appareils  
  
-   Pour utiliser la fonctionnalité de jonction, le schéma de la forêt de serveurs AD FS sont joints au doit être Windows Server 2012 R2.  
  
-   L’autre nom du sujet du certificat SSL pour le service AD FS doit contenir l’enterpriseregistration de valeur qui est suivie par le nom d’utilisateur Principal \(UPN\) suffixe de votre organisation, par exemple, enterpriseregistration.corp.contoso.com.  
  
## <a name="BKMK_12"></a>Exigences de chiffrement  
Le tableau suivant fournit des informations de prise en charge de chiffrement supplémentaires sur la signature, le chiffrement des jetons de jetons AD FS\/les fonctionnalités de déchiffrement :  
  
||||  
|-|-|-|  
|**Algorithm**|**Longueurs de clé**|**Protocoles\/Applications\/commentaires**|  
|TripleDES – par défaut 192 \(pris en charge – 256 192\) \- [http :\/\/www.w3.org\/2001\/04\/xmlenc\# TripleDES\-cbc](http://www.w3.org/2001/04/xmlenc)|>\= 192|Algorithme pris en charge pour le déchiffrement du jeton de sécurité. Chiffrement du jeton de sécurité avec cet algorithme n’est pas pris en charge.|  
|Aes128 \- http :\/\/www.w3.org\/2001\/04\/xmlenc\#aes128\-cbc|128|Algorithme pris en charge pour le déchiffrement du jeton de sécurité. Chiffrement du jeton de sécurité avec cet algorithme n’est pas pris en charge.|  
|AES192 \- http :\/\/www.w3.org\/2001\/04\/xmlenc\#aes192\-cbc|192|Algorithme pris en charge pour le déchiffrement du jeton de sécurité. Chiffrement du jeton de sécurité avec cet algorithme n’est pas pris en charge.|  
|AES256 \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#aes256\-cbc](http://www.w3.org/2001/04/xmlenc)|256|**Valeur par défaut**. Algorithme pris en charge pour le chiffrement du jeton de sécurité.|  
|TripleDESKeyWrap \- http :\/\/www.w3.org\/2001\/04\/xmlenc\#kw\-tripledes|Toutes les tailles de clé prises en charge par .NET 4.0\+|Algorithme pris en charge pour chiffrer la clé symétrique qui chiffre un jeton de sécurité.|  
|AES128KeyWrap \- [http :\/\/www.w3.org\/2001\/04\/xmlenc\#kw\-aes128](http://www.w3.org/2001/04/xmlenc)|128|Algorithme pris en charge pour chiffrer la clé symétrique qui chiffre le jeton de sécurité.|  
|AES192KeyWrap \- [http :\/\/www.w3.org\/2001\/04\/xmlenc\#kw\-aes192](http://www.w3.org/2001/04/xmlenc)|192|Algorithme pris en charge pour chiffrer la clé symétrique qui chiffre le jeton de sécurité.|  
|AES256KeyWrap \- [http :\/\/www.w3.org\/2001\/04\/xmlenc\#kw\-aes256](http://www.w3.org/2001/04/xmlenc)|256|Algorithme pris en charge pour chiffrer la clé symétrique qui chiffre le jeton de sécurité.|  
|RsaV15KeyWrap \- http :\/\/www.w3.org\/2001\/04\/xmlenc\#rsa\-1\_5|1024|Algorithme pris en charge pour chiffrer la clé symétrique qui chiffre le jeton de sécurité.|  
|RsaOaepKeyWrap \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#rsa\-oaep\-mgf1p](http://www.w3.org/2001/04/xmlenc)|1024|valeur par défaut. Algorithme pris en charge pour chiffrer la clé symétrique qui chiffre le jeton de sécurité.|  
|SHA1\-http:\/\/www.w3.org\/PICS\/DSig\/SHA1\_1\_0.html|N\/A|Utilisé par le serveur AD FS dans la génération de SourceId artefact :  Dans ce scénario, le STS utilise SHA1 \(par la recommandation dans la norme SAML 2.0\) pour créer une valeur courte 160 bits pour l’ID de source d’artefact.<br /><br />Également utilisé par l’agent web ADFS \(composant hérité à partir de l’intervalle de temps WS2003\) pour identifier les modifications dans un délai de « dernière mise à jour » valeur afin qu’il sache quand mettre à jour des informations à partir de STS.|  
|SHA1withRSA\-<br /><br />http:\/\/www.w3.org\/PICS\/DSig\/RSA\-SHA1\_1\_0.html|N\/A|Utilisé lorsque le serveur AD FS valide la signature de SAML AuthenticationRequest, signer la demande de résolution d’artefact ou la réponse, créer le jeton\-certificat de signature.<br /><br />Dans ces cas, SHA256 est la valeur par défaut et SHA1 est utilisé uniquement si le partenaire \(confiance\) ne peut pas prendre en charge les SHA256 et doivent utiliser SHA1.|  
  
## <a name="BKMK_13"></a>Exigences relatives aux autorisations  
L’administrateur qui effectue l’installation et la configuration initiale des services AD FS doit avoir des autorisations d’administrateur de domaine dans le domaine local \(en d’autres termes, le domaine auquel le serveur de fédération est joint à.\)  
  
## <a name="see-also"></a>Voir aussi  
[Guide de conception AD FS dans Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

