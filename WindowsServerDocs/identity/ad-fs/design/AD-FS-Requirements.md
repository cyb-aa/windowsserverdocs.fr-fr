---
ms.assetid: 8ce6e7c4-cf8e-4b55-980c-048fea28d50f
title: "Batterie de serveurs de fédération à l’aide de SQL Server"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e23154b20dfcd642178a5ac0de17f1b6a490f91f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-fs-requirements"></a>Configuration ADFS requise

>S’applique à: Windows Server2012R2

Voici les différentes spécifications que vous devez vous conformer lors du déploiement d’AD FS:  
  
-   [Configuration requise des certificats](AD-FS-Requirements.md#BKMK_1)  
  
-   [Configuration matérielle requise](AD-FS-Requirements.md#BKMK_2)  
  
-   [Configuration logicielle requise](AD-FS-Requirements.md#BKMK_3)  
  
-   [Configuration requise de ADDS](AD-FS-Requirements.md#BKMK_4)  
  
-   [Configuration requise de base de données](AD-FS-Requirements.md#BKMK_5)  
  
-   [Configuration requise du navigateur](AD-FS-Requirements.md#BKMK_6)  
  
-   [Configuration requise extranet](AD-FS-Requirements.md#BKMK_extranet)  
  
-   [Configuration réseau requise](AD-FS-Requirements.md#BKMK_7)  
  
-   [Configuration requise pour Windows store d’attributs](AD-FS-Requirements.md#BKMK_8)  
  
-   [Configuration requise d’application](AD-FS-Requirements.md#BKMK_9)  
  
-   [Configuration requise pour l’authentification](AD-FS-Requirements.md#BKMK_10)  
  
-   [Exigences de jonction d’espace de travail](AD-FS-Requirements.md#BKMK_11)  
  
-   [Exigences de chiffrement](AD-FS-Requirements.md#BKMK_12)  
  
-   [Autorisations requises](AD-FS-Requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>Configuration requise des certificats  
Les certificats jouent un rôle déterminant dans la sécurisation des communications entre les serveurs de fédération, les proxys d’Application Web, les applications prenant en charge claims\ et les clients Web. La configuration requise pour les certificats varie, selon que vous configurez un serveur de fédération ou un ordinateur de proxy, comme décrit dans cette section.  
  
**Certificats de serveur de fédération**  
  
|||  
|-|-|  
|**Type de certificat**|**Configuration requise, prise en charge et éléments à connaître**|  
|**Les certificats SSL \(SSL\) sécurisés:** il s’agit d’un certificat SSL standard qui est utilisé pour sécuriser les communications entre les serveurs de fédération et les clients.|-Ce certificat doit être un publiquement trusted\ * X509 certificat v3.<br />-Tous les clients qui accèdent à n’importe quel point de terminaison AD FS doivent approuver ce certificat. Il est fortement recommandé d’utiliser des certificats émis par une autorité de certification publique \(third\-party\) \(CA\). Vous pouvez utiliser un certificat SSL signé intégrée avec succès sur les serveurs de fédération dans un environnement de laboratoire de test. Toutefois, pour un environnement de production, nous vous recommandons d’obtenir le certificat à partir d’une autorité de certification publique.<br />-Prend en charge de toute taille de clé prises en charge par Windows Server 2012 R2 pour les certificats SSL.<br />-Ne pas prise en charge des certificats qui utilisent des clés CNG.<br />-Lorsqu’il est utilisé avec l’espace de travail Join\/Device Registration Service, l’autre nom du sujet du certificat SSL pour le service AD FS doit contenir la valeur enterpriseregistration qui est suivie par le suffixe de nom d’utilisateur Principal \(UPN\) de votre organisation, par exemple, inscriptionentreprise.contoso.com.<br />-Certificats génériques sont pris en charge. Lorsque vous créez votre batterie AD FS, vous devrez fournir le nom du service pour le service AD FS \ (par exemple, **adfs.contoso.com**.<br />-Il est vivement recommandé d’utiliser le même certificat SSL pour le Proxy d’Application Web. Il est toutefois **requis** pour correspondre à la prise en charge des points de terminaison de l’authentification intégrée de Windows via le Proxy d’Application Web et de l’authentification de la Protection étendue est allumée \(default setting\).<br />-Le nom d’objet de ce certificat est utilisé pour représenter le nom du Service de fédération pour chaque instance d’AD FS que vous déployez. Pour cette raison, vous souhaiterez choisir un nom d’objet sur tous les certificats émis par le CA\ nouveautés qui reflète le nom de votre entreprise ou organisation auprès des partenaires.<br />    L’identité du certificat doit correspondre au nom de service de fédération \ (par exemple, fs.contoso.com\). L’identité est une extension autre nom de sujet de type dNSName ou, s’il n’existe aucune entrée d’autre nom de l’objet, le nom d’objet spécifié comme un nom commun. Plusieurs entrées pour l’autre nom de l’objet peuvent être présentes dans le certificat, fourni un d’eux correspond au nom de service de fédération.<br />-   **Important:** il est vivement recommandé d’utiliser le même certificat SSL sur tous les nœuds de votre batterie AD FS, ainsi que tous les proxys d’Application Web de votre batterie AD FS.|  
|**Certificat de communication du service:** ce certificat permet de sécurité de message WCF pour sécuriser les communications entre les serveurs de fédération.|-Par défaut, le certificat SSL est utilisé en tant que certificat de communication du service.  Mais vous avez également la possibilité de configurer un autre certificat en tant que le certificat de communication du service.<br />-   **Important:** si vous utilisez le certificat SSL en tant que le certificat de communication du service, l’expiration du certificat SSL, veillez à configurer le certificat SSL renouvelé en tant que votre certificat de communication du service. Cela ne se produit pas automatiquement.<br />: Ce certificat doit être approuvé par les clients d’AD FS qui utilisent la sécurité de Message WCF.<br />-Nous vous recommandons d’utiliser un certificat d’authentification serveur émis par une autorité de certification publique \(third\-party\) \(CA\).<br />-Le certificat de communication du service ne peut pas être un certificat qui utilise des clés CNG.<br />-Ce certificat peut être géré à l’aide de la console de gestion AD FS.|  
|**Certificat de signature de Token\:** il s’agit d’un X509 standard certificat qui est utilisé pour signer de façon sécurisée tous les jetons que le serveur de fédération émet.|-Par défaut, AD FS crée un certificat signé intégrée avec des clés de 2 048 bits.<br />-Certificats d’autorité de certification émise sont également pris en charge et peuvent être modifiées à l’aide de la gestion AD FS de composants<br />-Autorité de certification a émis les certificats doive être stocké & accessibles via un fournisseur de chiffrement de fournisseur de services cryptographiques.<br />-Le certificat de signature de jeton ne peut pas être un certificat qui utilise des clés CNG.<br />-AD FS ne requiert pas de certificats inscrits en externe pour la signature de jetons.<br />    AD FS renouvelle automatiquement ces certificats signés intégrée avant qu’ils n’expirent, tout d’abord la configuration de nouveaux certificats en tant que certificats secondaires pour permettre aux partenaires de les utiliser, puis retourner à principal dans un processus appelé substitution automatique de certificat. Nous vous recommandons d’utiliser la valeur par défaut, les certificats de signature de jetons générés automatiquement.<br />    Si votre organisation comporte des stratégies qui nécessitent des certificats différents d’être configuré pour la signature de jetons, vous pouvez spécifier les certificats au moment de l’installation à l’aide de Powershell \ (utilisez le paramètre – SigningCertificateThumbprint de la cmdlet\ Install\-AdfsFarm).  Après l’installation, vous pouvez afficher et gérer des certificats de signature de jeton à l’aide de la console de gestion AD FS ou les applets de commande Powershell Set-AdfsCertificate et les applet de commande Get-AdfsCertificate.<br />    Lorsque les certificats inscrits en externe sont utilisés pour la signature de jetons, AD FS n’effectue pas le renouvellement automatique de certificat ou substitution.  Ce processus doit être effectué par un administrateur.<br />    Pour autoriser la substitution de certificat quand un seul certificat est sur le point d’expirer, un certificat de signature de jetons secondaire peut être configuré dans AD FS. Par défaut, tous les certificats de signature de jeton sont publiées dans les métadonnées de fédération, mais uniquement le certificat de signature token\ principal est utilisé par AD FS pour la signature des jetons.|  
|**Certificat de chiffrement/decryption\ Token\:** cela est une norme X509 de certificat qui est utilisé pour chiffrer/decrypt\ les jetons entrants. Il est également publié dans les métadonnées de fédération.|-Par défaut, AD FS crée un certificat signé intégrée avec des clés de 2 048 bits.<br />-Certificats d’autorité de certification émise sont également pris en charge et peuvent être modifiées à l’aide de la gestion AD FS de composants<br />-Autorité de certification a émis les certificats doive être stocké & accessibles via un fournisseur de chiffrement de fournisseur de services cryptographiques.<br />-Le certificat de chiffrement/decryption\ token\ ne peut pas être un certificat qui utilise des clés CNG.<br />-Par défaut, AD FS génère et utilise son propre, certificats générés en interne et signé intégrée pour le déchiffrement de jeton.  AD FS ne nécessite pas de certificats inscrits en externe à cet effet.<br />    En outre, AD FS renouvelle automatiquement ces certificats signés intégrée avant expiration.<br />    **Nous vous recommandons d’utiliser la valeur par défaut, les certificats générés automatiquement pour le déchiffrement de jeton.**<br />    Si votre organisation comporte des stratégies qui nécessitent des certificats différents d’être configuré pour le déchiffrement de jeton, vous pouvez spécifier les certificats au moment de l’installation à l’aide de Powershell \ (utilisez le paramètre – DecryptionCertificateThumbprint de la cmdlet\ Install\-AdfsFarm).  Après l’installation, vous pouvez afficher et gérer des certificats de déchiffrement de jeton à l’aide de la console de gestion AD FS ou les applets de commande Powershell Set-AdfsCertificate et les applet de commande Get-AdfsCertificate.<br />    **Lorsque les certificats inscrits en externe sont utilisés pour le déchiffrement de jeton, AD FS n’effectue pas le renouvellement automatique de certificat. Ce processus doit être effectué par un administrateur**.<br />-Le compte de service AD FS doit avoir accès à la clé privée du certificat signature token\ dans le magasin personnel de l’ordinateur local. Cela est prise en charge par le programme d’installation. Vous pouvez également utiliser la gestion AD FS enfichable pour vous assurer de cet accès si vous modifiez par la suite le certificat de signature token\.|  
  
> [!CAUTION]  
> Les certificats qui sont utilisés pour la signature de token\ et token\-decrypting\/le chiffrement sont essentielles à la stabilité du Service de fédération. La gestion de leurs propres certificats de signature de token\ et token\-decrypting\ et le cryptage de nos clients de vérifier que ces certificats sont sauvegardés et sont disponibles indépendamment pendant un événement de récupération.  
  
> [!NOTE]  
> Dans ADFS, vous pouvez modifier le niveau \(SHA\) algorithme de hachage sécurisé utilisé pour les signatures numériques \(more secure\) SHA\-1 ou SHA\-256. AD FS ne prend pas en charge l’utilisation de certificats avec d’autres méthodes de hachage, telles que MD5 \ (par défaut algorithme de hachage qui sont utilisé avec le tool\ de ligne de commande Makecert.exe). Comme meilleure pratique de sécurité, nous vous recommandons d’utiliser SHA\-256 \ (qui est définie par default\) pour toutes les signatures. SHA\-1 est recommandée pour utiliser uniquement dans les scénarios dans lesquels vous devez interagir avec un produit qui ne prend pas en charge les communications utilisant SHA\-256, comme un versions de produit ou hérité des éditeurs autres que Microsoft d’AD FS.  
  
> [!NOTE]  
> Après avoir reçu un certificat à partir d’une autorité de certification, assurez-vous que tous les certificats sont importés dans le magasin de certificats personnel de l’ordinateur local. Vous pouvez importer des certificats dans le magasin personnel avec le composant enfichable MMC.  
  
## <a name="BKMK_2"></a>Configuration matérielle requise  
Les conditions matérielles minimale et recommandée suivantes s’appliquent pour les serveurs de fédération AD FS dans Windows Server 2012 R2:  
  
||||  
|-|-|-|  
|**Configuration matérielle requise**|**Configuration minimale requise**|**Configuration recommandée**|  
|Vitesse du processeur|1,4 GHz 64 bits processeur|Quad\ cœur, 2 GHz|  
|MÉMOIRE RAM|512 MO|4GO|  
|Espace disque|32GO|100GO|  
  
## <a name="BKMK_3"></a>Configuration logicielle requise  
La configuration requise pour AD FS suivantes sont pour les fonctionnalités de serveur qui sont intégrée dans le système d’exploitation Windows Server® 2012 R2:  
  
-   Pour l’accès extranet, vous devez déployer le service de rôle Proxy d’Application Web \ - partie du rôle serveur accès à distance de Windows Server® 2012 R2. Les versions antérieures d’un serveur proxy de fédération ne sont pas pris en charge avec AD FS dans Windows Server® 2012 R2.  
  
-   Un serveur de fédération et le service de rôle Proxy d’Application Web ne peut pas être installés sur le même ordinateur.  
  
## <a name="BKMK_4"></a>Configuration requise de ADDS  
**Configuration requise de contrôleur de domaine**  
  
Contrôleurs de domaine dans tous les domaines de l’utilisateur et le domaine auquel appartiennent les serveurs AD FS doivent exécuter Windows Server 2008 ou version ultérieure.  
  
> [!NOTE]  
> Toute prise en charge pour les environnements avec des contrôleurs de domaine Windows Server 2003 prend fin après l’étendu prennent en charge fin Date pour Windows Server 2003. Les clients sont fortement recommandées pour mettre à niveau les contrôleurs de domaine dès que possible. Visitez [cette page](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO) pour plus d’informations sur la politique de Support Microsoft. Pour les problèmes rencontrés, qui sont spécifiques aux environnements de contrôleur de domaine Windows Server 2003, correctifs seront émis uniquement pour les problèmes de sécurité et si un correctif peut être émis avant l’expiration du Support étendu pour Windows Server 2003.  
  
**Configuration requise functional\ au niveau du domaine**  
  
Tous les domaines de compte d’utilisateur et le domaine auquel appartiennent les serveurs ADFS doivent être exécutés sur le niveau fonctionnel du domaine de Windows Server2003 ou version ultérieure.  
  
La plupart des fonctionnalités d’AD FS ne nécessitent pas de modification du niveau functional\ AD DS fonctionnent correctement. Toutefois, fonctionnel de domaine Windows Server 2008 niveau ou une version ultérieure est requis pour l’authentification du certificat client fonctionner correctement si le certificat est mappé explicitement sur un compte d’utilisateur dans AD DS.  
  
**Configuration requise du schéma**  
  
-   AD FS ne nécessite pas de modifications de schéma ou du niveau functional\ dans les services AD DS.  
  
-   Pour utiliser la fonctionnalité de jonction d’espace, le schéma de serveurs AD FS joints à la forêt doit être défini vers Windows Server 2012 R2.  
  
**Exigences relatives aux comptes de service**  
  
-   N’importe quel compte de service standard peut être utilisé comme compte de service pour AD FS. Comptes de Service géré de groupe sont également pris en charge. Cela nécessite au moins un contrôleur de domaine \ (il est recommandé de déployer deux ou more\) qui exécute Windows Server 2012 ou version ultérieure.  
  
-   Pour l’authentification Kerberos fonctionne entre les clients appartenant à domain\ et AD FS, le «HOST\ / < adfs\_service\_name >» doit être enregistré en tant qu’un SPN du compte de service. Par défaut, AD FS cela configurera lors de la création d’une nouvelle batterie AD FS si elle dispose des autorisations suffisantes pour effectuer cette opération.  
  
-   Le compte de service AD FS doit être approuvé dans chaque domaine de l’utilisateur qui contient l’authentification des utilisateurs pour le service AD FS.  
  
**Configuration requise du domaine**  
  
-   Tous les serveurs ADFS doivent être joint à un domaine ADDS.  
  
-   Tous les serveurs AD FS dans une batterie de serveurs doivent être déployés dans un domaine unique.  
  
-   Le domaine que les serveurs AD FS sont joints au doit approuver chaque domaine de compte d’utilisateur qui contient les utilisateurs de l’authentification sur le service AD FS.  
  
**Configuration requise de forêts multiples**  
  
-   Le domaine que les serveurs AD FS sont joints au doit approuver chaque domaine de compte d’utilisateur ou de la forêt qui contient les utilisateurs de l’authentification sur le service AD FS.  
  
-   Le compte de service AD FS doit être approuvé dans chaque domaine de l’utilisateur qui contient l’authentification des utilisateurs pour le service AD FS.  
  
## <a name="BKMK_5"></a>Configuration requise de base de données  
Voici la configuration requise et les restrictions qui s’appliquent en fonction du type de magasin de configuration:  
  
**WID**  
  
-   Une batterie WID dispose d’un nombre maximal de serveurs de fédération 30 si vous avez moins de 100 approbations de partie confiance.  
  
-   Profil de résolution d’artefacts dans SAML 2.0 n’est pas pris en charge dans la base de données de configuration WID.  Détection de relecture de jetons n’est pas pris en charge dans la base de données de configuration WID. Cette fonctionnalité est uniquement utilisée uniquement dans les scénarios où AD FS agissant en tant que le fournisseur de fédération et utilise des jetons de sécurité à partir de fournisseurs de revendications externe.  
  
-   Les centres de déploiement de serveurs AD FS dans données distincts pour le basculement ou l’équilibrage de charge géographique est pris en charge tant que le nombre de serveurs ne dépasse pas 30.  
  
Le tableau suivant fournit un résumé pour l’utilisation d’une batterie WID.  Il permet de planifier votre implémentation.  
  
||||  
|-|-|-|  
||1 \-100 RP approbations|Plus de 100 RP approbations|  
|1 \-30 AD FS nœuds|WID pris en charge|Non pris en charge à l’aide de WID \-SQL requis|  
|Plus de 30 AD FS nœuds|Non pris en charge à l’aide de WID \-SQL requis|Non pris en charge à l’aide de WID \-SQL requis|  
  
**SQLServer**  
  
Pour AD FS dans Windows Server 2012 R2, vous pouvez utiliser SQL Server 2008 et versions ultérieures  
  
## <a name="BKMK_6"></a>Configuration requise du navigateur  
Lors de l’authentification ADFS est effectuée via un navigateur ou d’un contrôle de navigateur, votre navigateur doit satisfaire aux exigences suivantes:  
  
-   JavaScript doit être activé.  
  
-   Les cookies doivent être activés.  
  
-   Serveur Indication du nom \(SNI\) doivent être pris en charge.  
  
-   Pour l’authentification par certificat utilisateur certificat & appareil \ (functionality\ de joindre un espace de travail), le navigateur doit prendre en charge l’authentification du certificat SSL client  
  
Plusieurs plateformes et navigateurs clés ont subi une validation pour le rendu et fonctionnalités lesquels sont répertoriées ci-dessous. Navigateurs et les appareils non traitées dans ce tableau sont toujours pris en charge s’ils répondent aux exigences répertoriées ci-dessus:  
  
|||  
|-|-|  
|**Navigateurs**|**Plates-formes**|  
|INTERNET EXPLORER 10.0|Windows 7, Windows 8.1, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2|  
|INTERNET EXPLORER VERSION 11.0|Windows7, Windows 8.1, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2|  
|Service Broker d’authentification Web Windows|Windows8.1|  
|Firefox \[v21\]|Windows 7, Windows 8.1|  
|Safari \[v7\]|iOS 6, Mac OS\-X 10.7|  
|Chrome \[v27\]|Windows 7, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Mac OS\-X 10.7|  
  
> [!IMPORTANT]  
> Problème connu \-Firefox: jonction des fonctionnalités qui identifiant l’appareil à l’aide du certificat de périphérique ne fonctionne pas sur les plateformes Windows. Firefox ne prend pas en charge exécution SSL authentification par certificat client à l’aide de certificats configurés pour le magasin de certificats utilisateur sur les clients Windows.  
  
**Cookies**  
  
AD FS crée des cookies persistants et session\ qui doivent être stockés sur les ordinateurs clients pour fournir de connexion, -hors connexion, seul sur connexion \(SSO\) et d’autres fonctionnalités. Par conséquent, le navigateur client doit être configuré pour accepter les cookies. Les cookies qui sont utilisés pour l’authentification sont toujours les cookies de session Secure Hypertext Transfer Protocol \(HTTPS\) qui sont écrits pour le serveur d’origine. Si le navigateur client n’est pas configuré pour autoriser ces cookies, AD FS ne peut pas fonctionner correctement. Les cookies persistants permettent de préserver l’utilisateur de sélectionner le fournisseur de revendications. Vous pouvez les désactiver à l’aide d’un paramètre de configuration dans le fichier de configuration pour les pages de connexion AD FS. Prise en charge de TLS\/SSL est requis pour des raisons de sécurité.  
  
## <a name="BKMK_extranet"></a>Configuration requise extranet  
Pour fournir un accès extranet pour le service AD FS, vous devez déployer le service de rôle Proxy d’Application Web en tant que le rôle accès extranet qui transmet les demandes d’authentification à le de manière sécurisée pour le service AD FS. Cela fournit l’isolation des points de terminaison de service AD FS, ainsi que l’isolation de toutes les clés de sécurité \ (par exemple, le jeton certificates\ signature) à partir de demandes qui proviennent d’internet. En outre, les fonctionnalités telles que le verrouillage de compte Extranet logicielle nécessitent l’utilisation du proxy d’Application Web. Pour plus d’informations sur le Proxy d’Application Web, voir [Proxy d’Application Web](https://technet.microsoft.com/library/dn584107.aspx).  
  
Si vous souhaitez utiliser un proxy tiers third\ pour l’accès extranet, ce proxy tiers third\ doit prendre en charge le protocole défini dans [http:///\/download.microsoft.com\/download\/9\/5\/E\/95EF66AF\-9026\-4BB0\-A41D\-A4F81802D92C\/%5bMS\-ADFSPIP%5d.pdf](https://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/%5bMS-ADFSPIP%5d.pdf).  
  
## <a name="BKMK_7"></a>Configuration réseau requise  
Configuration des services réseau suivants correctement est essentielle pour la réussite du déploiement d’AD FS dans votre organisation:  
  
**La configuration du pare-feu d’entreprise**  
  
Pare-feu situé entre le Proxy d’Application Web et de la batterie de serveurs de fédération et le pare-feu entre les clients et le Proxy d’Application Web doivent avoir le port TCP 443 activé entrant.  
  
En outre, si l’authentification des certificats utilisateur client \ (clientTLS X509 à l’aide de l’authentification utilisateur certificates\) est requis, AD FS dans Windows Server 2012 R2 nécessite que le port TCP 49443 soit activé entrant sur le pare-feu entre les clients et le Proxy d’Application Web. Cela n'est pas requis sur le pare-feu entre le Proxy d’Application Web et la fédération servers\).  
  
**Configuration du service DNS**  
  
-   Pour l’accès intranet, tous les clients l’accès au service AD FS dans le réseau d’entreprise interne \(intranet\) doivent être en mesure de résoudre le nom du service AD FS \ (nom fourni par le certificate\ SSL) pour l’équilibrage de charge pour les serveurs AD FS ou le serveur AD FS.  
  
-   Pour l’accès extranet, tous les clients l’accès au service AD FS à partir en dehors du réseau d’entreprise \(extranet\/internet\) doivent être en mesure de résoudre le nom du service AD FS \ (nom fourni par le certificate\ SSL) pour l’équilibrage de charge pour les serveurs Proxy d’Application Web ou le serveur de Proxy d’Application Web.  
  
-   Pour l’accès extranet de fonctionner correctement, chaque serveur de Proxy d’Application Web dans le réseau de périmètre doit être en mesure de résoudre le nom du service AD FS \ (nom fourni par le certificate\ SSL) pour l’équilibrage de charge pour les serveurs AD FS ou le serveur AD FS. Cela peut être obtenue à l’aide d’un autre serveur DNS dans le réseau DMZ ou en modifiant la résolution de serveur local à l’aide du fichier HOSTS.  
  
-   Pour l’authentification Windows intégrée pour le travail à l’intérieur du réseau et en dehors du réseau pour un sous-ensemble des points de terminaison exposés par le biais du Proxy d’Application Web, vous devez utiliser un un enregistrement \(not CNAME\) pour pointer vers les équilibreurs de charge.  
  
Pour plus d’informations sur la configuration DNS d’entreprise pour le service de fédération et Device Registration Service, consultez [configurer le DNS d’entreprise pour le Service de fédération et DRS](https://technet.microsoft.com/library/dn486786.aspx).  
  
Pour plus d’informations sur la configuration DNS d’entreprise pour les serveurs proxy d’Application Web, voir la section «Configurer le serveur DNS» dans [étape 1: configurer l’Infrastructure du Proxy d’Application Web](https://technet.microsoft.com/library/dn383644.aspx).  
  
Pour plus d’informations sur la façon de configurer une adresse IP de cluster ou le nom de domaine complet à l’aide d’équilibrage de charge réseau de cluster, voir définition des paramètres de Cluster à [http:///\/go.microsoft.com\/fwlink\/? LinkId\ = 75282](https://go.microsoft.com/fwlink/?LinkId=75282).  
  
## <a name="BKMK_8"></a>Configuration requise pour Windows store d’attributs  
AD FS requiert au moins un magasin d’attributs à utiliser pour l’authentification des utilisateurs et l’extraction des revendications de sécurité pour ces utilisateurs. Pour obtenir la liste des attributs magasins pris en charge par AD FS, consultez [The Role of Attribute Stores](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md).  
  
> [!NOTE]  
> AD FS crée automatiquement un magasin d’attributs «Active Directory», par défaut. Configuration requise pour Windows store d’attributs dépendent si votre organisation agit comme le partenaire de compte \ (hébergeant l’utilisateurs\ fédérés) ou le partenaire de ressource \ (hébergeant l’appel à leurs fédérée).  
  
**Magasins d’attributs LDAP**  
  
Lorsque vous travaillez avec d’autres Lightweight Directory Access Protocol \ magasins d’attributs \-based (LDAP\), vous devez vous connecter à un serveur LDAP qui prend en charge l’authentification Windows intégrée. La chaîne de connexion LDAP doit également être écrite sous la forme d’une URL LDAP, comme décrit dans le document RFC 2255.  
  
Il est également obligatoire que le compte de service pour le service AD FS a le droit de récupérer des informations utilisateur dans le magasin d’attributs LDAP.  
  
**Magasins d’attributs SQL Server**  
  
Pour AD FS dans Windows Server 2012 R2 pour fonctionner correctement, les ordinateurs qui hébergent le magasin d’attributs SQL Server doivent exécuter Microsoft SQL Server 2008 ou version supérieure. Lorsque vous travaillez avec des magasins d’attributs SQL\, vous devez également configurer une chaîne de connexion.  
  
**Magasins d’attributs personnalisés**  
  
Vous pouvez développer des magasins d’attributs personnalisés pour activer des scénarios avancés.  
  
-   Le langage de stratégie qui est intégré à AD FS peut référencer des magasins d’attributs personnalisés afin qu’un des scénarios suivants peuvent être amélioré:  
  
    -   Créer des revendications pour un utilisateur authentifié localement  
  
    -   Compléter les revendications pour un utilisateur authentifié de manière externe  
  
    -   Autoriser un utilisateur à obtenir un jeton  
  
    -   Autoriser un service pour obtenir un jeton d’un utilisateur  
  
    -   Émission des données supplémentaires dans des jetons de sécurité émis par AD FS pour les parties de confiance.  
  
-   Tous les magasins d’attributs personnalisés doivent être générés au-dessus .NET 4.0 ou une version ultérieure.  
  
Lorsque vous travaillez avec un magasin d’attributs personnalisés, vous devrez peut-être également configurer une chaîne de connexion. Dans ce cas, vous pouvez entrer un code personnalisé de votre choix qui permet une connexion à votre magasin d’attributs personnalisés. La chaîne de connexion dans cette situation est un ensemble de paires nom\/valeur interprétées comme étant implémentées par le développeur du magasin d’attributs personnalisé. Pour plus d’informations sur le développement et à l’aide des magasins d’attributs personnalisés, voir [vue d’ensemble du Windows Store attributs](https://go.microsoft.com/fwlink/?LinkId=190782).  
  
## <a name="BKMK_9"></a>Configuration requise d’application  
AD FS prend en charge les applications prenant en charge claims\ qui utilisent les protocoles suivants:  
  
-   WS-Federation  
  
-   WS-Trust  
  
-   L’utilisation de profils IDPLite, SPLite & eGov1.5 du protocole SAML 2.0.  
  
-   Profil de l’octroi d’autorisation OAuth 2.0  
  
AD FS prend également en charge l’authentification et autorisation pour toutes les applications autres que claims\-prenant en charge qui sont pris en charge par le Proxy d’Application Web.  
  
## <a name="BKMK_10"></a>Configuration requise pour l’authentification  
**Les services AD DS \(Primary Authentication\) d’authentification**  
  
Pour l’accès intranet, les mécanismes d’authentification standard suivants pour les services AD DS sont pris en charge:  
  
-   Authentification intégrée de Windows à l’aide de négocier pour Kerberos et NTLM  
  
-   Authentification par formulaire à l’aide d’username\/mots de passe  
  
-   Authentification du certificat à l’aide de certificats mappés aux comptes d’utilisateur dans AD DS  
  
Pour l’accès extranet, les mécanismes d’authentification suivantes sont prises en charge:  
  
-   Authentification par formulaire à l’aide d’username\/mots de passe  
  
-   Authentification du certificat à l’aide de certificats sont mappés aux comptes d’utilisateur dans AD DS  
  
-   Authentification intégrée de Windows à l’aide de négocier \(NTLM only\) pour les points de terminaison WS-Trust acceptent l’authentification intégrée Windows.  
  
Pour l’authentification de certificat:  
  
-   S’étend aux cartes à puce qui peuvent être protégé de code confidentiel.  
  
-   L’interface graphique utilisateur pour l’utilisateur à entrer son code pin n’est pas fourni par AD FS et est nécessaire pour faire partie du système d’exploitation client qui s’affiche lors de l’utilisation de client TLS.  
  
-   Le lecteur et le fournisseur de services de chiffrement \(CSP\) pour la carte à puce doivent fonctionner sur l’ordinateur sur lequel se trouve le navigateur.  
  
-   Le certificat de carte à puce doit être lié à une racine approuvée sur tous les serveurs AD FS et les serveurs Proxy d’Application Web.  
  
-   Le certificat doit être mappé au compte d’utilisateur dans AD DS par une des méthodes suivantes:  
  
    -   Le nom de sujet du certificat correspond au nom unique LDAP d’un compte d’utilisateur dans AD DS.  
  
    -   L’extension altname objet du certificat a l’UPN nom \(UPN\) d’un compte d’utilisateur dans AD DS.  
  
Pour l’authentification intégrée Windows transparente à l’aide de Kerberos dans l’intranet,  
  
-   Il est nécessaire pour le nom du service doit faire partie des Sites de confiance ou sites Intranet locaux.  
  
-   En outre, le HOST\ / < adfs\_service\_name > SPN doit être défini sur la batterie AD FS s’exécute sur le compte de service.  
  
**Authentification prennent facteurs**  
  
AD FS prend en charge une authentification supplémentaire \ (au-delà de l’authentification principale pris en charge par AD DS\) à l’aide d’un fournisseur de modèle selon laquelle vendors\/clients peuvent créer leur propres carte prennent-facteur d’authentification administrateur peut enregistrer et utiliser lors de la connexion.  
  
Chaque carte de l’authentification Multifacteur doit être créé sur .NET 4.5.  
  
Pour plus d’informations sur l’authentification Multifacteur, voir [gérer les risques avec une authentification multifacteur supplémentaire pour les Applications sensibles](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).  
  
**Authentification des appareils**  
  
AD FS prend en charge l’authentification des appareils à l’aide de certificats configurés par le Service d’inscription de périphérique pendant le fait de l’espace de travail utilisateur final joindre leur appareil.  
  
## <a name="BKMK_11"></a>Exigences de jonction d’espace de travail  
Les utilisateurs finaux peuvent jonction leurs périphériques à une organisation à l’aide d’AD FS. Cela est pris en charge par le Service d’inscription de périphérique dans AD FS. Par conséquent, les utilisateurs finaux obtenir l’avantage de l’authentification unique entre les applications prises en charge par AD FS. En outre, les administrateurs peuvent gérer les risques en limitant l’accès uniquement aux périphériques qui ont été joint à l’organisation un espace de travail d’applications. Voici les exigences suivantes pour activer ce scénario.  
  
-   AD FS prend en charge la jonction d’espace pour Windows 8.1 et iOS 5\ +  
  
-   Pour utiliser la fonctionnalité de jonction d’espace, le schéma de serveurs AD FS joints à la forêt doit être Windows Server 2012 R2.  
  
-   L’autre nom du sujet du certificat SSL pour le service AD FS doit contenir la valeur enterpriseregistration qui est suivie par le suffixe de nom d’utilisateur Principal \(UPN\) de votre organisation, par exemple, enterpriseregistration.corp.contoso.com.  
  
## <a name="BKMK_12"></a>Exigences de chiffrement  
Le tableau suivant fournit des informations de prise en charge de chiffrement supplémentaire sur le jeton AD FS de signature, la fonctionnalité encryption\/déchiffrement de jeton:  
  
||||  
|-|-|-|  
|**Algorithme**|**Longueurs de clé**|**Protocols\/Applications\/commentaires**|  
|TripleDES – par défaut 192 \ (pris en charge 192 – 256\) \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#tripledes\-cbc](http://www.w3.org/2001/04/xmlenc)|>\= 192|Algorithme pris en charge pour le chiffrement du jeton de sécurité.|  
|Aes128 \-http:///\/ www.w3.org \/2001\/04\/xmlenc\#aes128\-cbc|128|Algorithme pris en charge pour le chiffrement du jeton de sécurité.|  
|AES192 \-http:///\/ www.w3.org \/2001\/04\/xmlenc\#aes192\-cbc|192|Algorithme pris en charge pour le chiffrement du jeton de sécurité.|  
|AES256 \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#aes256\-cbc](http://www.w3.org/2001/04/xmlenc)|256|**Par défaut**. Algorithme pris en charge pour le chiffrement du jeton de sécurité.|  
|TripleDESKeyWrap \-http:///\/ www.w3.org \/2001\/04\/xmlenc\#kw\-tripledes|Toutes les tailles de clé prises en charge par .NET 4.0\+|Algorithme pris en charge pour le chiffrement de la clé symétrique qui chiffre un jeton de sécurité.|  
|AES128KeyWrap \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#kw\-aes128](http://www.w3.org/2001/04/xmlenc)|128|Algorithme pris en charge pour le chiffrement de la clé symétrique qui chiffre le jeton de sécurité.|  
|AES192KeyWrap \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#kw\-aes192](http://www.w3.org/2001/04/xmlenc)|192|Algorithme pris en charge pour le chiffrement de la clé symétrique qui chiffre le jeton de sécurité.|  
|AES256KeyWrap \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#kw\-aes256](http://www.w3.org/2001/04/xmlenc)|256|Algorithme pris en charge pour le chiffrement de la clé symétrique qui chiffre le jeton de sécurité.|  
|RsaV15KeyWrap \-http:///\/ www.w3.org \/2001\/04\/xmlenc\#rsa\-1\_5|1024|Algorithme pris en charge pour le chiffrement de la clé symétrique qui chiffre le jeton de sécurité.|  
|RsaOaepKeyWrap \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#rsa\-oaep\-mgf1p](http://www.w3.org/2001/04/xmlenc)|1024|Par défaut. Algorithme pris en charge pour le chiffrement de la clé symétrique qui chiffre le jeton de sécurité.|  
|SHA1\-http: / / / \ / www.w3.org \/PICS\/DSig\/SHA1\_1\_0.html|N\/A|Utilisé par le serveur AD FS dans la génération d’ID source artefact: dans ce scénario, le service STS utilise SHA1 \ (par la recommandation dans le standard\ SAML 2.0) pour créer une valeur de 160 bits court pour l’ID d’objet source.<br /><br />Également utilisé par l’agent web ADFS \ (composant hérité de WS2003 timeframe\) pour identifier les modifications dans un délai de «dernière mise à jour» de la valeur afin qu’il sait quand mettre à jour les informations dans le service STS.|  
|SHA1withRSA\-<br /><br />http:///\/ www.w3.org \/PICS\/DSig\/RSA\-SHA1\_1\_0.html|N\/A|Utilisé lorsque le serveur AD FS valide la signature de AuthenticationRequest SAML, signer la demande de résolution d’artefacts ou la réponse, créer le certificat de signature de token\.<br /><br />Dans ces cas, SHA256 est la valeur par défaut et SHA1 est utilisé uniquement si le partenaire \(relying party\) ne peut pas en charge SHA256 et doivent utiliser SHA1.|  
  
## <a name="BKMK_13"></a>Autorisations requises  
L’administrateur qui effectue l’installation et la configuration initiale d’AD FS doit disposer des autorisations d’administrateur de domaine dans le domaine local \ (en d’autres termes, le domaine auquel est joint le serveur de fédération à. \)  
  
## <a name="see-also"></a>Voir aussi  
[Guide de conception ADFS dans Windows Server2012R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

