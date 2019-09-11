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
ms.openlocfilehash: 91a1d69e4f7a77633d460f594ad3091f359df9c3
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867844"
---
# <a name="ad-fs-requirements"></a>Configuration AD FS requise

Voici les différentes exigences auxquelles vous devez vous conformer lors du déploiement de AD FS :  
  
-   [Conditions requises pour les certificats](AD-FS-Requirements.md#BKMK_1)  
  
-   [Configuration matérielle requise](AD-FS-Requirements.md#BKMK_2)  
  
-   [Configuration logicielle requise](AD-FS-Requirements.md#BKMK_3)  
  
-   [Configuration requise pour la AD DS](AD-FS-Requirements.md#BKMK_4)  
  
-   [Configuration requise pour la base de données de configuration](AD-FS-Requirements.md#BKMK_5)  
  
-   [Configuration requise pour le navigateur](AD-FS-Requirements.md#BKMK_6)  
  
-   [Exigences relatives aux extranets](AD-FS-Requirements.md#BKMK_extranet)  
  
-   [Configuration réseau requise](AD-FS-Requirements.md#BKMK_7)  
  
-   [Conditions requises pour le magasin d’attributs](AD-FS-Requirements.md#BKMK_8)  
  
-   [Configuration requise pour l’application](AD-FS-Requirements.md#BKMK_9)  
  
-   [Conditions d’authentification](AD-FS-Requirements.md#BKMK_10)  
  
-   [Conditions requises pour la jonction d’espace de travail](AD-FS-Requirements.md#BKMK_11)  
  
-   [Exigences de chiffrement](AD-FS-Requirements.md#BKMK_12)  
  
-   [Exigences relatives aux autorisations](AD-FS-Requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>Certificats requis  
Les certificats jouent le rôle le plus critique dans la sécurisation des communications entre les serveurs de Fédération\-, les proxys d’application Web, les applications prenant en charge les revendications et les clients Web. La configuration requise pour les certificats varie selon que vous configurez un serveur de Fédération ou un ordinateur proxy, comme décrit dans cette section.  
  
**Certificats de serveur de Fédération**  
  
|||  
|-|-|  
|**Type de certificat**|**Exigences, prise en charge & éléments à connaître**|  
|**Protocole SSL \(certificat\) SSL :** Il s’agit d’un certificat SSL standard utilisé pour sécuriser les communications entre les serveurs de Fédération et les clients.|-Ce certificat doit être un certificat x509\* v3 approuvé publiquement.<br />-Tous les clients qui accèdent à un point de terminaison AD FS doivent approuver ce certificat. Il est fortement recommandé d’utiliser des certificats émis par une autorité de \(\) certification\-d’autorité \(\)de certification tierce publique. Vous pouvez utiliser un certificat\-SSL auto-signé avec succès sur les serveurs de Fédération dans un environnement de laboratoire de test. Toutefois, pour un environnement de production, nous vous recommandons d'obtenir le certificat auprès d'une autorité de certification publique.<br />-Prend en charge n’importe quelle taille de clé prise en charge par Windows Server 2012 R2 pour les certificats SSL.<br />-Ne prend pas en charge les certificats qui utilisent des clés CNG.<br />-Lorsqu’il est utilisé avec\/Workplace Join service d’inscription d’appareils, l’autre nom de l’objet du certificat SSL pour le service AD FS doit contenir la valeur enterpriseregistration suivie du nom \(d’utilisateurprincipal.Suffixe UPN\) de votre organisation, par exemple, enterpriseregistration.contoso.com.<br />-Les certificats génériques sont pris en charge. Lorsque vous créez votre batterie de AD FS, vous êtes invité à fournir le nom du service AD FS \(, par exemple, **ADFS.contoso.com**.<br />-Il est fortement recommandé d’utiliser le même certificat SSL pour le proxy d’application Web. Cela doit toutefois être le même pour la prise en charge des points de terminaison d’authentification **intégrée de Windows** via le proxy d’application Web et lorsque \(l’authentification\)de protection étendue est activée par défaut.<br />-Le nom d’objet de ce certificat est utilisé pour représenter le nom de service FS (Federation Service) pour chaque instance de AD FS que vous déployez. Pour cette raison, vous souhaiterez peut-être choisir un nom de sujet pour les\-nouveaux certificats émis par l’autorité de certification qui représentent le plus le nom de votre entreprise ou organisation auprès des partenaires.<br />    L’identité du certificat doit correspondre au nom \(du service de Fédération, par exemple, FS.contoso.com.\) L’identité est soit une extension de l’autre nom de l’objet de type dNSName, soit, s’il n’y a pas d’autres entrées de nom d’objet, le nom d’objet spécifié comme nom commun. Plusieurs entrées de nom alternatif de l’objet peuvent être présentes dans le certificat, à condition que l’une d’elles corresponde au nom du service de Fédération.<br />-   **Important :** il est fortement recommandé d’utiliser le même certificat SSL sur tous les nœuds de votre batterie de AD FS, ainsi que sur tous les proxys d’application Web de votre batterie de serveurs AD FS.|  
|**Certificat de communication du service :** Ce certificat active la sécurité de message WCF pour sécuriser les communications entre les serveurs de fédération.|-Par défaut, le certificat SSL est utilisé comme certificat de communication du service.  Toutefois, vous avez également la possibilité de configurer un autre certificat comme certificat de communication de service.<br />-   **Important :** si vous utilisez le certificat SSL comme certificat de communication du service, lors de l’expiration du certificat SSL, veillez à configurer le certificat SSL renouvelé en tant que certificat de communication du service. Cela ne se produit pas automatiquement.<br />-Ce certificat doit être approuvé par les clients de AD FS qui utilisent la sécurité de message WCF.<br />-Nous vous recommandons d’utiliser un certificat d’authentification serveur émis par une autorité de \(\) certification\-d’autorité \(\)de certification tierce publique.<br />-Le certificat de communication du service ne peut pas être un certificat qui utilise des clés CNG.<br />-Ce certificat peut être géré à l’aide de la console de gestion AD FS.|  
|**Certificat\-de signature de jetons :** Certificat X509 standard qui permet de signer de manière sécurisée tous les jetons émis par le serveur de fédération.|-Par défaut, AD FS crée un certificat\-auto-signé avec des clés de 2048 bits.<br />-Les certificats émis par l’autorité de certification sont également pris en charge et peuvent\-être modifiés à l’aide du composant logiciel enfichable Gestion AD FS<br />-Les certificats émis par l’autorité de certification doivent être stockés & accessibles via un fournisseur de chiffrement CSP.<br />-Le certificat de signature de jetons ne peut pas être un certificat qui utilise des clés CNG.<br />-AD FS ne requiert pas de certificats inscrits en externe pour la signature des jetons.<br />    AD FS renouvelle automatiquement ces certificats auto\--signés avant qu’ils n’expirent, en configurant d’abord les nouveaux certificats en tant que certificats secondaires pour permettre aux partenaires de les consommer, puis en basculant sur le serveur principal dans un processus appelé automatique. substitution de certificat. Nous vous recommandons d’utiliser les certificats par défaut générés automatiquement pour la signature des jetons.<br />    Si votre organisation a des stratégies qui requièrent la configuration de certificats différents pour la signature de jetons, vous pouvez spécifier les certificats au \(moment de l’installation à l’aide de PowerShell, en utilisant le paramètre – SigningCertificateThumbprint de l’installation. \-Applet\)de commande AdfsFarm.  Après l’installation, vous pouvez afficher et gérer les certificats de signature de jetons à l’aide de la console\-de gestion AD FS\-ou des applets de commande PowerShell Set AdfsCertificate et obtenir AdfsCertificate.<br />    Lorsque des certificats inscrits en externe sont utilisés pour la signature de jetons, AD FS n’effectue pas de renouvellement ou de substitution de certificat automatique.  Ce processus doit être effectué par un administrateur.<br />    Pour permettre la substitution de certificat lorsqu’un certificat est proche de l’expiration, un certificat de signature de jetons secondaire peut être configuré dans AD FS. Par défaut, tous les certificats de signature de jetons sont publiés dans les métadonnées de Fédération\-, mais seul le certificat de signature de jetons principal est utilisé par AD FS pour signer des jetons.|  
|**Certificat\-de\/chiffrement du déchiffrement de jetons :** Il s’agit d’un certificat x509 standard utilisé pour déchiffrer\/les jetons entrants. Il est également publié dans les métadonnées de fédération.|-Par défaut, AD FS crée un certificat\-auto-signé avec des clés de 2048 bits.<br />-Les certificats émis par l’autorité de certification sont également pris en charge et peuvent\-être modifiés à l’aide du composant logiciel enfichable Gestion AD FS<br />-Les certificats émis par l’autorité de certification doivent être stockés & accessibles via un fournisseur de chiffrement CSP.<br />-Le certificat\-de\/chiffrement du déchiffrement de jetons ne peut pas être un certificat qui utilise des clés CNG.<br />-Par défaut, AD FS génère et utilise ses propres certificats générés en interne et\-auto-signés pour le déchiffrement de jetons.  AD FS ne requiert pas de certificats inscrits en externe à cet effet.<br />    En outre, AD FS renouvelle automatiquement ces certificats auto\--signés avant qu’ils n’expirent.<br />    **Nous vous recommandons d’utiliser les certificats par défaut générés automatiquement pour le déchiffrement de jeton.**<br />    Si votre organisation a des stratégies qui nécessitent la configuration de certificats différents pour le déchiffrement de jetons, vous pouvez spécifier les certificats au \(moment de l’installation à l’aide de PowerShell, en utilisant le paramètre – DecryptionCertificateThumbprint de l' Installez\-l’applet\)de commande AdfsFarm.  Après l’installation, vous pouvez afficher et gérer les certificats de déchiffrement de jetons à l’aide de la\-console de gestion\-AD FS ou des applets de commande PowerShell Set AdfsCertificate et obtenir AdfsCertificate.<br />    **Lorsque des certificats inscrits en externe sont utilisés pour le déchiffrement des jetons, AD FS n’effectue pas de renouvellement automatique des certificats.  Ce processus doit être effectué par un administrateur**.<br />-Le compte de service AD FS doit avoir accès à la\-clé privée du certificat de signature de jetons dans le magasin personnel de l’ordinateur local. Ce processus est pris en charge par le programme d’installation de. Vous pouvez également utiliser le composant logiciel enfichable\-gestion de la AD FS pour garantir cet accès si vous modifiez par la suite le certificat de signature de jetons.\-|  
  
> [!CAUTION]  
> Les certificats utilisés pour la signature\-des jetons et\-le\/chiffrement du déchiffrement des jetons sont essentiels pour la stabilité du service FS (Federation Service). Les clients qui gèrent leur\-propre signature de\-jetons &\/le chiffrement des certificats de chiffrement doivent s’assurer que ces certificats sont sauvegardés et sont disponibles indépendamment pendant un événement de récupération.  
  
> [!NOTE]  
> Dans AD FS vous pouvez modifier le \(niveau SHA\) de l’algorithme de hachage sécurisé utilisé pour les signatures numériques\-pour SHA 1\-ou \(SHA 256\)plus sécurisé. AD FS ne prend pas en charge l’utilisation de certificats avec d’autres méthodes de hachage \(, telles que MD5, l’algorithme de hachage par défaut utilisé\-avec l'\)outil en ligne de commande Makecert. exe. En guise de meilleure pratique de sécurité, nous vous recommandons\-d' \(utiliser l’algorithme SHA\) 256 qui est défini par défaut pour toutes les signatures. L'\-utilisation de l’algorithme SHA 1 est recommandée dans les scénarios où vous devez interagir avec un produit qui ne prend pas en\-charge les communications utilisant l’algorithme SHA 256, tel qu’un produit non\-Microsoft ou des versions héritées de AD FS.  
  
> [!NOTE]  
> Après avoir reçu un certificat d'une autorité de certification, assurez-vous que tous les certificats sont importés dans le magasin de certificats personnels de l'ordinateur local. Vous pouvez importer des certificats dans le magasin personnel avec le composant logiciel\-enfichable MMC Certificats.  
  
## <a name="BKMK_2"></a>Configuration matérielle requise  
Les configurations matérielles minimales et recommandées suivantes s’appliquent aux AD FS les serveurs de Fédération dans Windows Server 2012 R2 :  
  
||||  
|-|-|-|  
|**Configuration matérielle requise**|**Configuration minimale requise**|**Configuration recommandée**|  
|Vitesse du processeur|processeur 64\-bits 1,4 GHz|Quadruple\-cœur, 2 GHz|  
|RAM|512 Mo|4 Go|  
|Espace disque|32 Go|100 GO|  
  
## <a name="BKMK_3"></a>Configuration logicielle requise  
La configuration AD FS suivante concerne les fonctionnalités serveur intégrées au système d’exploitation Windows Server® 2012 R2 :  
  
-   Pour l’accès extranet, vous devez déployer la partie service \- de rôle proxy d’application Web du rôle de serveur d’accès à distance Windows Server® 2012 R2. Les versions antérieures d’un serveur proxy de Fédération ne sont pas prises en charge avec AD FS dans Windows Server® 2012 R2.  
  
-   Un serveur de Fédération et le service de rôle proxy d’application Web ne peuvent pas être installés sur le même ordinateur.  
  
## <a name="BKMK_4"></a>Configuration requise pour la AD DS  
**Configuration requise pour le contrôleur de domaine**  
  
Les contrôleurs de domaine de tous les domaines d’utilisateur et le domaine auquel les serveurs AD FS sont joints doivent exécuter Windows Server 2008 ou une version ultérieure.  
  
> [!NOTE]  
> Toute la prise en charge des environnements avec les contrôleurs de domaine Windows Server 2003 se termine après la date de fin de prise en charge étendue de Windows Server 2003. Il est vivement recommandé aux clients de mettre à niveau leurs contrôleurs de domaine dès que possible. Pour plus d’informations sur Support Microsoft cycle de vie, consultez [cette page](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO) . Pour les problèmes découverts spécifiques aux environnements de contrôleur de domaine Windows Server 2003, des correctifs sont émis uniquement pour les problèmes de sécurité et si un correctif peut être émis avant l’expiration du support étendu pour Windows Server 2003.  
  
**Exigences de\-niveau fonctionnel de domaine**  
  
Tous les domaines de compte d’utilisateur et le domaine auquel les serveurs de AD FS sont joints doivent fonctionner au niveau fonctionnel du domaine de Windows Server 2003 ou version ultérieure.  
  
La plupart des fonctionnalités AD FS ne nécessitent pas\-que les modifications apportées au niveau fonctionnel AD DS fonctionnent correctement. Toutefois, le niveau fonctionnel de domaine Windows Server 2008 ou supérieur est nécessaire au bon fonctionnement de l'authentification de certificat client si le certificat est mappé explicitement sur le compte d'un utilisateur dans AD DS.  
  
**Spécifications du schéma**  
  
-   AD FS ne requiert pas de modifications de schéma\-ou de modifications de niveau fonctionnel pour AD DS.  
  
-   Pour utiliser Workplace Join fonctionnalité, le schéma de la forêt à laquelle AD FS serveurs sont joints doit être défini sur Windows Server 2012 R2.  
  
**Exigences relatives au compte de service**  
  
-   N’importe quel compte de service standard peut être utilisé comme compte de service pour AD FS. Les comptes de service administrés de groupe sont également pris en charge. Cela nécessite au moins un contrôleur \(de domaine. il est recommandé de déployer deux ou plusieurs\) serveurs exécutant Windows Server 2012 ou une version ultérieure.  
  
-   Pour que l’authentification Kerberos fonctionne entre\-clients joints à un domaine et AD FS,\/le nom du\_service « hôte < ADFS\_> » doit être enregistré en tant que SPN sur le compte de service. Par défaut, AD FS configure ce paramètre lors de la création d’une batterie de serveurs AD FS s’il dispose des autorisations suffisantes pour effectuer cette opération.  
  
-   Le compte de service AD FS doit être approuvé dans chaque domaine d’utilisateur qui contient les utilisateurs qui s’authentifient auprès du service AD FS.  
  
**Configuration requise du domaine**  
  
-   Tous les serveurs AD FS doivent être joints à un domaine AD DS.  
  
-   Tous les serveurs AD FS au sein d’une batterie de serveurs doivent être déployés dans un domaine unique.  
  
-   Le domaine auquel les serveurs AD FS sont joints doit approuver chaque domaine de compte d’utilisateur qui contient les utilisateurs qui s’authentifient auprès du service AD FS.  
  
**Configuration requise pour plusieurs forêts**  
  
-   Le domaine auquel les serveurs AD FS sont joints doit approuver chaque domaine de compte d’utilisateur ou forêt qui contient les utilisateurs qui s’authentifient auprès du service AD FS.  
  
-   Le compte de service AD FS doit être approuvé dans chaque domaine d’utilisateur qui contient les utilisateurs qui s’authentifient auprès du service AD FS.  
  
## <a name="BKMK_5"></a>Configuration requise pour la base de données de configuration  
Voici les exigences et les restrictions qui s’appliquent selon le type de magasin de configurations :  
  
**WID**  
  
-   Une batterie de serveurs WID a une limite de 30 serveurs de Fédération si vous avez 100 ou moins d’approbations de partie de confiance.  
  
-   Le profil de résolution d’artefact dans SAML 2,0 n’est pas pris en charge dans la base de données de configuration WID.  La détection de relecture de jetons n’est pas prise en charge dans la base de données de configuration WID. Cette fonctionnalité est uniquement utilisée dans les scénarios où AD FS agit en tant que fournisseur de Fédération et utilise des jetons de sécurité provenant de fournisseurs de revendications externes.  
  
-   Le déploiement de serveurs AD FS dans des centres de données distincts pour le basculement ou l’équilibrage de charge géographique est pris en charge tant que le nombre de serveurs n’est pas supérieur à 30.  
  
Le tableau suivant fournit un résumé de l’utilisation d’une batterie de serveurs WID.  Utilisez-le pour planifier votre implémentation.  
  
||||  
|-|-|-|  
||1 \- 100 confiances RP|Plus de 100 confiances RP|  
|1 \- 30 nœuds de AD FS|WID pris en charge|Non pris en charge \- avec SQL wid requis|  
|Plus de 30 nœuds de AD FS|Non pris en charge \- avec SQL wid requis|Non pris en charge \- avec SQL wid requis|  
  
**SQL Server**  
  
Pour AD FS dans Windows Server 2012 R2, vous pouvez utiliser SQL Server 2008 et versions ultérieures  
  
## <a name="BKMK_6"></a>Configuration requise pour le navigateur  
Lorsque AD FS authentification est effectuée par le biais d’un navigateur ou d’un contrôle de navigateur, votre navigateur doit respecter les conditions suivantes :  
  
-   JavaScript doit être activé  
  
-   Les cookies doivent être activés  
  
-   Indication du nom du serveur \(SNI\) doit être pris en charge  
  
-   Pour le certificat utilisateur & fonctionnalité \(\)de jonction d’espace de travail d’authentification de certificat d’appareil, le navigateur doit prendre en charge l’authentification par certificat client SSL  
  
Plusieurs navigateurs et plateformes clés ont été validés pour le rendu et les fonctionnalités dont les détails sont répertoriés ci-dessous. Les navigateurs et les appareils qui ne sont pas couverts dans ce tableau sont toujours pris en charge s’ils répondent aux conditions répertoriées ci-dessus :  
  
|||  
|-|-|  
|**Navigateurs**|**Plateformes**|  
|IE 10,0|Windows 7, Windows 8.1, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2|  
|IE 11,0|Windows, Windows 8.1, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2|  
|Service Broker d’authentification Web Windows|Windows 8.1|  
|V21 \[Firefox\]|Windows 7, Windows 8.1|  
|Safari \[v7\]|iOS 6, Mac OS\-X 10,7|  
|V27 \[chrome\]|Windows 7, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Mac OS\-X 10,7|  
  
> [!IMPORTANT]  
> Problème \- connu Firefox : Workplace Join fonctionnalité qui identifie l’appareil à l’aide d’un certificat d’appareil n’est pas fonctionnelle sur les plateformes Windows. Firefox ne prend pas actuellement en charge l’authentification par certificat client SSL à l’aide de certificats approvisionnés dans le magasin de certificats utilisateur sur les clients Windows.  
  
**Cookies**  
  
AD FS crée des\-cookies persistants et basés sur une session qui doivent être stockés sur les\-ordinateurs clients pour\-fournir la connexion,\-la \(déconnexion, l’authentification\)unique à authentification unique et d’autres fonctionnalités. Le navigateur client doit donc être configuré de manière à accepter les cookies. Les cookies utilisés pour l’authentification sont toujours des cookies de \(\) session HTTPS Secure Hypertext Transfer Protocol écrits pour le serveur d’origine. Si le navigateur client n'est pas configuré de manière à autoriser ces cookies, AD FS ne peut pas fonctionner correctement. Les cookies persistants permettent de conserver le fournisseur de revendications choisi par l'utilisateur. Vous pouvez les désactiver à l’aide d’un paramètre de configuration dans le fichier de configuration\-des pages de connexion AD FS. La prise en\/charge de SSL TLS est requise pour des raisons de sécurité.  
  
## <a name="BKMK_extranet"></a>Exigences relatives aux extranets  
Pour fournir un accès extranet au service AD FS, vous devez déployer le service de rôle proxy d’application Web en tant que rôle accessible par l’extranet qui transmet les demandes d’authentification de manière sécurisée au service AD FS. Cela permet d’isoler les points de terminaison de service AD FS, ainsi que l’isolation \(de toutes les clés de\) sécurité telles que les certificats de signature de jetons des demandes provenant d’Internet. En outre, des fonctionnalités telles que le verrouillage de compte d’extranet souple nécessitent l’utilisation du proxy d’application Web. Pour plus d’informations sur le proxy d’application Web, voir [proxy d’application Web](https://technet.microsoft.com/library/dn584107.aspx).  
  
Si vous souhaitez utiliser un proxy tiers\-pour l’accès extranet, ce proxy tiers\-doit prendre en charge le protocole défini dans [http :\/\/Download.Microsoft.com\/download\/ 9\/5E\/95EF66AF\-90264BB0\-A41DA4F81802D92C%5bMS\-ADFSPIP% 5 d.pdf.\/\-\-\/](https://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/%5bMS-ADFSPIP%5d.pdf)  
  
## <a name="BKMK_7"></a>Configuration réseau requise  
La configuration appropriée des services réseau suivants est essentielle pour réussir le déploiement de AD FS dans votre organisation :  
  
**Configuration du pare-feu d’entreprise**  
  
Le pare-feu situé entre le proxy d’application Web et la batterie de serveurs de Fédération et le pare-feu entre les clients et le proxy d’application Web doit avoir le port TCP 443 activé pour le trafic entrant.  
  
En outre, si l’authentification clientTLS d' \(authentification par certificat utilisateur client à\) l’aide de certificats utilisateur x509 est requise, AD FS dans Windows Server 2012 R2 nécessite que le port TCP 49443 soit activé pour le trafic entrant sur le pare-feu entre les clients et proxy d’application Web. Cela n’est pas obligatoire sur le pare-feu entre le proxy d’application Web\)et les serveurs de Fédération.  
  
**Configuration de DNS**  
  
-   Pour l’accès intranet, tous les clients qui accèdent à AD FS service \(au\) sein de l’intranet interne du réseau d’entreprise \(doivent être en mesure de résoudre\) le nom du service AD FS fourni par le certificat SSL à la charge équilibreur pour les serveurs AD FS ou le serveur AD FS.  
  
-   Pour l’accès extranet, tous les clients qui accèdent à AD FS service \(en\/dehors\) de l’extranet du réseau d’entreprise doivent \(être en mesure de résoudre le nom du service AD FS fourni par le certificatSSL.\) à l’équilibreur de charge pour les serveurs proxy d’application Web ou le serveur proxy d’application Web.  
  
-   Pour que l’accès extranet fonctionne correctement, chaque serveur proxy d’application Web de la zone DMZ doit être en mesure de \(résoudre AD FS nom de service fourni\) par le certificat SSL à l’équilibreur de charge pour les serveurs AD FS ou le serveur AD FS. Pour ce faire, utilisez un autre serveur DNS du réseau DMZ ou modifiez la résolution du serveur local à l’aide du fichier HOSTs.  
  
-   Pour que l’authentification intégrée de Windows fonctionne à l’intérieur du réseau et à l’extérieur du réseau pour un sous-ensemble de points de terminaison exposés via le proxy \(d’application\) Web, vous devez utiliser un enregistrement a non CNAME pour pointer vers les équilibrages de charge.  
  
Pour plus d’informations sur la configuration du DNS d’entreprise pour le service de Fédération et le service d’inscription d’appareils, consultez [configurer les serveurs DNS d’entreprise pour les service FS (Federation Service) et Drs](https://technet.microsoft.com/library/dn486786.aspx).  
  
Pour plus d’informations sur la configuration du DNS d’entreprise pour les proxys d’application Web, consultez la [section « configurer DNS » dans étape 1 : Configurez l’infrastructure](https://technet.microsoft.com/library/dn383644.aspx)du proxy d’application Web.  
  
Pour plus d’informations sur la configuration d’une adresse IP de cluster ou d’un nom de domaine complet de cluster à l’aide de NLB, consultez Spécification des paramètres de cluster à l’adresse [\/http :\/\/Go.Microsoft.com\/fwlink ? LinkId\=75282](https://go.microsoft.com/fwlink/?LinkId=75282).  
  
## <a name="BKMK_8"></a>Conditions requises pour le magasin d’attributs  
AD FS nécessite qu’au moins un magasin d’attributs soit utilisé pour l’authentification des utilisateurs et l’extraction des revendications de sécurité pour ces utilisateurs. Pour obtenir la liste des magasins d’attributs pris en charge par AD FS, consultez [le rôle des magasins d’attributs](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md).  
  
> [!NOTE]  
> AD FS crée automatiquement un magasin d’attributs « Active Directory », par défaut. Les exigences des magasins d’attributs varient selon que votre organisation fait office de \(partenaire de compte hébergeant les utilisateurs\) fédérés \(ou le partenaire de\)ressource hébergeant l’application fédérée.  
  
**Magasins d’attributs LDAP**  
  
Lorsque vous utilisez d’autres magasins d’attributs LDAP \(basés\)sur le protocole LDAP\-, vous devez vous connecter à un serveur LDAP qui prend en charge l’authentification intégrée de Windows. En outre, la chaîne de connexion LDAP doit être écrite sous la forme d'une URL LDAP, comme indiqué dans le document RFC 2255.  
  
Il est également nécessaire que le compte de service pour le service de AD FS ait le droit de récupérer les informations utilisateur dans le magasin d’attributs LDAP.  
  
**SQL Server les magasins d’attributs**  
  
Par AD FS dans Windows Server 2012 R2 pour fonctionner correctement, les ordinateurs qui hébergent le magasin d’attributs SQL Server doivent exécuter Microsoft SQL Server 2008 ou une version ultérieure. Lorsque vous utilisez des magasins\-d’attributs basés sur SQL, vous devez également configurer une chaîne de connexion.  
  
**Magasins d’attributs personnalisés**  
  
Vous pouvez développer des magasins d'attributs personnalisés dans le cadre de scénarios avancés.  
  
-   Le langage de stratégie intégré à AD FS peut référencer des magasins d'attributs personnalisés de manière à perfectionner les scénarios suivants :  
  
    -   Créer des revendications pour un utilisateur authentifié localement  
  
    -   Compléter les revendications pour un utilisateur authentifié de manière externe  
  
    -   Autoriser un utilisateur à obtenir un jeton  
  
    -   Autoriser un service à obtenir un jeton au nom d'un utilisateur  
  
    -   Émission de données supplémentaires dans les jetons de sécurité émis par AD FS aux parties de confiance.  
  
-   Tous les magasins d’attributs personnalisés doivent être basés sur .NET 4,0 ou une version ultérieure.  
  
Lorsque vous utilisez un magasin d’attributs personnalisé, vous devrez peut-être également configurer une chaîne de connexion. Dans ce cas, vous pouvez entrer un code personnalisé de votre choix qui active une connexion à votre magasin d’attributs personnalisé. Dans ce cas, la chaîne de connexion est un ensemble\/de paires nom/valeur qui sont interprétées comme étant implémentées par le développeur du magasin d’attributs personnalisés. Pour plus d’informations sur le développement et l’utilisation de magasins d’attributs personnalisés, consultez [vue d’ensemble du magasin d’attributs](https://go.microsoft.com/fwlink/?LinkId=190782).  
  
## <a name="BKMK_9"></a>Configuration requise pour l’application  
AD FS prend en\-charge les applications prenant en charge les revendications qui utilisent les protocoles suivants :  
  
-   Fédération\-WS  
  
-   Confiance\-WS  
  
-   Protocole SAML 2,0 utilisant IDPLite, divisez & les profils eGov 1.5.  
  
-   Profil d’octroi d’autorisation OAuth 2,0  
  
AD FS prend également en charge l’authentification et l'\-autorisation\-pour toutes les applications prenant en charge les revendications qui sont prises en charge par le proxy d’application Web.  
  
## <a name="BKMK_10"></a>Conditions d’authentification  
**Authentification principale \(de l’authentification AD DS\)**  
  
Pour l’accès intranet, les mécanismes d’authentification standard suivants pour AD DS sont pris en charge :  
  
-   Authentification intégrée de Windows à l’aide de Negotiate pour Kerberos & NTLM  
  
-   Authentification par formulaire à\/l’aide de mots de passe de nom d’utilisateur  
  
-   Authentification par certificat à l’aide de certificats mappés à des comptes d’utilisateurs dans AD DS  
  
Pour l’accès extranet, les mécanismes d’authentification suivants sont pris en charge :  
  
-   Authentification par formulaire à\/l’aide de mots de passe de nom d’utilisateur  
  
-   Authentification par certificat à l’aide de certificats mappés à des comptes d’utilisateurs dans AD DS  
  
-   Authentification intégrée Windows utilisant Negotiate \(NTLM uniquement\) pour les\-points de terminaison WS Trust qui acceptent l’authentification intégrée de Windows.  
  
Pour l’authentification par certificat :  
  
-   S’étend aux cartes à puces qui peuvent être protégées par un code confidentiel.  
  
-   L’interface graphique utilisateur permettant à l’utilisateur d’entrer son code pin n’est pas fournie par AD FS et doit faire partie du système d’exploitation client qui s’affiche lors de l’utilisation du protocole TLS du client.  
  
-   Le lecteur et le CSP \(\) du fournisseur de services de chiffrement pour la carte à puce doivent fonctionner sur l’ordinateur où se trouve le navigateur.  
  
-   Le certificat de carte à puce doit être lié à une racine approuvée sur tous les serveurs AD FS et les serveurs proxy d’application Web.  
  
-   Le certificat doit être mappé au compte d'utilisateur dans AD DS à l'aide de l'une des méthodes suivantes :  
  
    -   Le nom du sujet du certificat correspond au nom unique LDAP d'un compte d'utilisateur dans AD DS.  
  
    -   L’extension autre de l’objet du certificat a l' \(UPN\) du nom d’utilisateur principal d’un compte d’utilisateur dans AD DS.  
  
Pour une authentification intégrée Windows transparente à l’aide de Kerberos dans l’intranet,  
  
-   Il est nécessaire que le nom du service fasse partie des sites de confiance ou des sites Intranet local.  
  
-   En outre, l’hôte\/< nom\_du\_service ADFS > SPN doit être défini sur le compte de service sur lequel s’exécute la batterie de serveurs AD FS.  
  
**Multi\--Factor Authentication**  
  
AD FS prend en charge \(une authentification supplémentaire au-delà\) de l’authentification principale prise en\/charge par AD DS à l’aide\-d’un modèle de fournisseur qui permet aux fournisseurs de créer leur propre adaptateur multi-Factor Authentication qu’un administrateur peut inscrire et utiliser pendant la connexion.  
  
Chaque adaptateur MFA doit être basé sur .NET 4,5.  
  
Pour plus d’informations sur l’authentification multifacteur, consultez [gérer les risques avec des multi-Factor Authentication supplémentaires pour les applications sensibles](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).  
  
**Authentification de l’appareil**  
  
AD FS prend en charge l’authentification des appareils à l’aide de certificats approvisionnés par le service d’inscription des appareils pendant l’acte d’un espace de travail de l’utilisateur final joignant leur appareil.  
  
## <a name="BKMK_11"></a>Conditions requises pour la jonction d’espace de travail  
Les utilisateurs finaux peuvent joindre leurs appareils à une organisation à l’aide de AD FS. Ce service est pris en charge par le service d’inscription des appareils dans AD FS. Par conséquent, les utilisateurs finaux bénéficient de l’avantage supplémentaire de l’authentification unique dans les applications prises en charge par AD FS. En outre, les administrateurs peuvent gérer les risques en limitant l’accès aux applications uniquement aux appareils qui ont été joints à l’espace de travail de l’organisation. Vous trouverez ci-dessous la configuration requise pour activer ce scénario.  
  
-   AD FS prend en charge la jonction d’espace de\+ travail pour les appareils Windows 8.1 et iOS 5  
  
-   Pour utiliser Workplace Join fonctionnalité, le schéma de la forêt à laquelle AD FS serveurs sont joints doit être Windows Server 2012 R2.  
  
-   L’autre nom de l’objet du certificat SSL pour AD FS service doit contenir la valeur enterpriseregistration suivie du suffixe UPN UPN \(\) du nom d’utilisateur principal de votre organisation, par exemple, enterpriseregistration.corp.contoso.com.  
  
## <a name="BKMK_12"></a>Exigences de chiffrement  
Le tableau suivant fournit des informations supplémentaires sur la prise en charge du chiffrement sur\/la AD FS la signature des jetons, la fonctionnalité de déchiffrement de jeton :  
  
||||  
|-|-|-|  
|**Utilisé**|**Longueurs de clé**|**Protocoles\/applications\/commentaires**|  
|TripleDES-default 192 \(pris en charge 192\) – 256 \- [http\/:\/\/www.w3.org 2001 04xmlenc\#TripleDES\/\/CBC\-](http://www.w3.org/2001/04/xmlenc)|>\=192|Algorithme pris en charge pour le déchiffrement du jeton de sécurité. Le chiffrement du jeton de sécurité avec cet algorithme n’est pas pris en charge.|  
|AES128 \- http :\/www.w3.org2001\/04 xmlencAES128\#CBC\/\/\/\-|128|Algorithme pris en charge pour le déchiffrement du jeton de sécurité. Le chiffrement du jeton de sécurité avec cet algorithme n’est pas pris en charge.|  
|Aes192 \- http :\/www.w3.org2001\/04 xmlencAes192\#CBC\/\/\/\-|192|Algorithme pris en charge pour le déchiffrement du jeton de sécurité. Le chiffrement du jeton de sécurité avec cet algorithme n’est pas pris en charge.|  
|AES256 \- [http :\/www.w3.org\/200104\/xmlencAES256\#CBC\/\/\-](http://www.w3.org/2001/04/xmlenc)|256|**Valeur par défaut**. Algorithme pris en charge pour le chiffrement du jeton de sécurité.|  
|TripleDESKeyWrap \- http :\/www.w3.org2001\/04 xmlenckW\#TripleDES\/\/\/\-|Toutes les tailles de clé prises en charge par .NET 4,0\+|Algorithme pris en charge pour le chiffrement de la clé symétrique qui chiffre un jeton de sécurité.|  
|AES128KeyWrap \- [http :\/www.w3.org\/200104\/xmlenckW\#AES128\/\/\-](http://www.w3.org/2001/04/xmlenc)|128|Algorithme pris en charge pour le chiffrement de la clé symétrique qui chiffre le jeton de sécurité.|  
|AES192KeyWrap \- [http :\/www.w3.org\/200104\/xmlenckW\#Aes192\/\/\-](http://www.w3.org/2001/04/xmlenc)|192|Algorithme pris en charge pour le chiffrement de la clé symétrique qui chiffre le jeton de sécurité.|  
|AES256KeyWrap \- [http :\/www.w3.org\/200104\/xmlenckW\#AES256\/\/\-](http://www.w3.org/2001/04/xmlenc)|256|Algorithme pris en charge pour le chiffrement de la clé symétrique qui chiffre le jeton de sécurité.|  
|RsaV15KeyWrap \- http :\/www.w3.org2001\/04 xmlenc RSA1\-5\/\/\/\#\_|1 024|Algorithme pris en charge pour le chiffrement de la clé symétrique qui chiffre le jeton de sécurité.|  
|RsaOaepKeyWrap \- [http :\/www.w3.org\/200104\/xmlencRSAOAEP\-mgf1p\/\/\#\-](http://www.w3.org/2001/04/xmlenc)|1 024|valeur par défaut. Algorithme pris en charge pour le chiffrement de la clé symétrique qui chiffre le jeton de sécurité.|  
|SHA1\-http :\/www.w3.orgpics\/dedsig\/-SHA1\_10.\_HTML\/\/|N\/A|Utilisé par AD FS serveur dans la génération de l’ID d’objet de l’artefact :  Dans ce scénario, le STS utilise SHA1 \(conformément à la recommandation de la norme\) SAML 2,0 pour créer une valeur Short 160 bit pour l’sourceiD d’artefact.<br /><br />Également utilisé par le composant hérité de \(l’agent Web ADFS à\) partir de WS2003 pour identifier les modifications apportées à la valeur d’heure « dernière mise à jour » afin qu’il sache quand mettre à jour les informations du STS.|  
|SHA1withRSA\-<br /><br />http :\/\/www.w3.orgpicsDSig\/RSA SHA1 10\_. html\-\/\/\_|N\/A|Utilisé dans les cas où AD FS serveur valide la signature des AuthenticationRequest SAML, signez la demande ou la réponse de résolution d'\-artefact, créez un certificat de signature de jetons.<br /><br />Dans ce cas, SHA256 est la valeur par défaut et SHA1 est utilisé uniquement si la \(partie\) de confiance partenaire ne peut pas prendre en charge SHA256 et doit utiliser SHA1.|  
  
## <a name="BKMK_13"></a>Exigences relatives aux autorisations  
L’administrateur qui effectue l’installation et la configuration initiale de AD FS doit disposer d’autorisations d’administrateur de domaine dans \(le domaine local en d’autres termes, le domaine auquel le serveur de Fédération est joint.\)  
  
## <a name="see-also"></a>Voir aussi  
[Guide de conception AD FS dans Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

