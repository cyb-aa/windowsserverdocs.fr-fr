---
title: Vue d’ensemble de TLS-SSL (SSP Schannel)
description: Sécurité de Windows Server
ms.prod: windows-server
ms.technology: security-tls-ssl
ms.topic: article
ms.assetid: c8836345-16bb-4dcc-8d2b-2b9b687456a3
author: justinha
ms.author: justinha
manager: brianlic
ms.date: 05/16/2018
ms.openlocfilehash: 5478a97a6b333cfc92de100440d53a769a8c0fd9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855185"
---
# <a name="overview-of-tls---ssl-schannel-ssp"></a>Vue d’ensemble de TLS-SSL (SSP Schannel)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10

Cette rubrique destinée aux professionnels de l’informatique décrit les modifications apportées aux fonctionnalités du fournisseur SSP (Security Support Provider) Schannel, y compris les protocoles d’authentification TLS (Transport Layer Security), protocole SSL (SSL) et DTLS (Datagram Transport Layer Security), pour Windows Server 2012 R2, Windows Server 2012, Windows 8.1 et Windows 8.

Schannel est un fournisseur de service de sécurité (SSP) qui implémente les protocoles d’authentification standard Internet SSL, TLS et DTLS. L’interface SSPI (Security Support Provider Interface) est une API utilisée par les systèmes Windows pour exécuter des fonctions relatives à la sécurité, dont notamment l’authentification. L’interface SSPI fonctionne comme une interface commune à plusieurs fournisseurs SSP, y compris le fournisseur SSP Schannel.

Pour plus d’informations sur l’implémentation par Microsoft de TLS et SSL dans le SSP Schannel, consultez les informations [techniques de référence sur TLS/SSL (2003)](https://technet.microsoft.com/library/cc784149(v=ws.10).aspx).


## <a name="tlsssl-schannel-ssp-features"></a>Fonctionnalités de TLS/SSL (SSP Schannel)
Les rubriques suivantes décrivent les fonctionnalités de TLS dans le SSP Schannel.

### <a name="tls-session-resumption"></a>Reprise de session TLS
Le protocole TLS (Transport Layer Security), composant du fournisseur SSP (Security Support Provider) Schannel, est utilisé pour sécuriser les données envoyées entre les applications via un réseau non approuvé. Vous pouvez utiliser TLS/SSL pour authentifier les serveurs et les ordinateurs clients et pour crypter les messages entre les parties authentifiées.

En raison de l’expiration de session, les appareils qui connectent fréquemment TLS aux serveurs doivent se reconnecter. Windows 8.1 et Windows Server 2012 R2 prennent désormais en charge RFC 5077 (reprise de session TLS sans état côté serveur). Cette modification fournit des Windows Phone et des appareils Windows RT avec :

-   Utilisation des ressources sur le serveur réduite

-   Bande passante réduite, ce qui améliore l’efficacité des connexions client

-   Temps consacré aux négociations TLS réduit grâce aux reprises de connexion.

> [!NOTE]
> L’implémentation côté client de RFC 5077 a été ajoutée dans Windows 8.

Pour plus d’informations sur la reprise de session TLS sans état, consultez le document IETF [RFC 5077](http://www.ietf.org/rfc/rfc5077).

### <a name="application-protocol-negotiation"></a>Négociation de protocole d’application
 Windows Server 2012 R2 et Windows 8.1 prennent en charge la négociation de protocole d’application TLS côté client afin que les applications puissent exploiter les protocoles dans le cadre du développement standard HTTP 2,0 et que les utilisateurs puissent accéder aux services en ligne tels que Google et Twitter à l’aide d’applications exécutant le protocole SPDY.

**Fonctionnement**

Les applications serveur et client permettent l’extension de négociation de protocole d’application en fournissant des listes d’ID de protocoles d’application pris en charge par ordre de préférence décroissant. Le client TLS indique qu’il prend en charge la négociation de protocole d’application en incluant l’extension ALPN (Application Layer Protocol Negotiation) avec une liste de protocoles pris en charge par les clients dans le message ClientHello.

Lorsque le client TLS effectue la demande auprès du serveur, le serveur TLS lit sa liste de protocoles pris en charge pour trouver le protocole d’application favori également pris en charge par le client. Si ce protocole est trouvé, le serveur répond avec l’ID de protocole sélectionné et poursuit avec la négociation comme d’habitude. S’il n’existe aucun protocole d’application commun, le serveur envoie une alerte d’échec de négociation irrécupérable.

### <a name="management-of-trusted-issuers-for-client-authentication"></a><a name="BKMK_TrustedIssuers"></a>Gestion des émetteurs approuvés pour l’authentification du client
Lorsque l’authentification de l’ordinateur client avec SSL ou TLS est requise, le serveur peut être configuré pour envoyer une liste d’émetteurs de certificats approuvés. Cette liste contient l’ensemble des émetteurs de certificats qui seront approuvés par le serveur. En présence de plusieurs certificats, elle renseigne l’ordinateur client sur le certificat client à sélectionner. De plus, la chaîne de certificat que l’ordinateur client envoie au serveur doit être validée par rapport à la liste d’émetteurs approuvés configurée.

Avant Windows Server 2012 et Windows 8, les applications ou les processus qui utilisaient le SSP Schannel (y compris HTTP. sys et IIS) pouvaient fournir une liste des émetteurs approuvés qu’ils ont pris en charge pour l’authentification du client par le biais d’une liste de certificats de confiance (CTL).

Dans Windows Server 2012 et Windows 8, des modifications ont été apportées au processus d’authentification sous-jacent, de sorte que :

-   La gestion de la liste d’émetteurs approuvés CTL n’est plus prise en charge.

-   Le comportement d’envoi de la liste d’émetteurs approuvés est désactivé par défaut : la valeur par défaut de la clé de Registre SendTrustedIssuerList est désormais 0 (désactivée par défaut) au lieu de 1.

-   La compatibilité avec les versions précédentes des systèmes d’exploitation Windows est préservée.

**Quelle est la valeur ajoutée ?**

À partir de Windows Server 2012, l’utilisation de la liste CTL a été remplacée par une implémentation basée sur le magasin de certificats. Cela rend la gestion plus facile et conviviale grâce aux applets de commande de gestion de certificat existantes du fournisseur PowerShell et aux outils en ligne de commande, comme certutil.exe.

Bien que la taille maximale de la liste d’autorités de certification approuvées prise en charge par le SSP Schannel (16 Ko) reste identique à celle de Windows Server 2008 R2, dans Windows Server 2012, il existe un nouveau magasin de certificats dédié pour les émetteurs d’authentification du client, de sorte que les certificats sans lien ne sont pas inclus dans le message.

**Comment cela fonctionne-t-il ?**

Dans Windows Server 2012, la liste des émetteurs approuvés est configurée à l’aide de magasins de certificats. un magasin de certificats d’ordinateur global par défaut et un magasin facultatif par site. La source de la liste est déterminée comme suit :

-   Si un magasin d’informations d’identification spécifique est configuré pour le site, il sera utilisé comme source.

-   Si le magasin défini par l’application ne contient aucun certificat, Schannel vérifie le magasin **Émetteurs d’authentification de client** sur l’ordinateur local. Si ce magasin contient des certificats, Schannel l’utilise comme source. Si aucun certificat n’est trouvé dans les deux magasins, le magasin Racines de confiance est vérifié.

-   Si ni les magasins globaux ni les magasins locaux ne contiennent de certificats, le fournisseur Schannel utilise le magasin **autorités de certification racines de confiance certification** comme source de la liste des émetteurs approuvés. (Il s’agit du comportement de Windows Server 2008 R2.)

Si le magasin d' **autorités de certification racine de confiance** utilisé contient un mélange de certificats d’émetteur d’autorité de certification (ca) et racine (auto-signé), seuls les certificats de l’émetteur de l’autorité de certification sont envoyés au serveur par défaut.

**Comment configurer Schannel pour utiliser le magasin de certificats pour les émetteurs approuvés**

L’architecture du SSP Schannel dans Windows Server 2012 utilise par défaut les magasins comme décrit ci-dessus pour gérer la liste des émetteurs approuvés. Vous pouvez toujours utiliser les applets de commande de gestion des certificats existantes du fournisseur PowerShell et les outils en ligne de commande comme Certutil pour gérer les certificats.

Pour plus d’informations sur la gestion des certificats avec le fournisseur PowerShell, consultez [Applets de commande d’administration AD CS dans Windows](https://technet.microsoft.com/library/hh848365(v=wps.620).aspx).

Pour plus d’informations sur la gestion des certificats à l’aide de l’utilitaire de certificat, consultez [certutil.exe](https://technet.microsoft.com/library/cc732443.aspx).

Pour plus d’informations sur les données (y compris celles du magasin défini par l’application) définies pour les informations d’identification d’un Schannel, consultez [Structure SCHANNEL_CRED (Windows)](https://msdn.microsoft.com/library/windows/desktop/aa379810(v=vs.85).aspx).

**Valeurs par défaut des modes d’approbation**

Le fournisseur Schannel prend en charge trois modes de confiance pour l’authentification du client. Le mode d’approbation contrôle la manière dont la validation de la chaîne de certificats du client est effectuée et est un paramètre à l’ensemble du système contrôlé par le REG_DWORD « ClientAuthTrustMode » sous HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\Schannel.

|Valeur|Mode de confiance|Description|
|-----|-------|--------|
|0|Approbation de machine (par défaut)|Requiert que le certificat client soit émis par un certificat de la liste des émetteurs approuvés.|
|1|Approbation de racine exclusive|Requiert qu’un certificat client soit chaîné vers un certificat racine contenu dans le magasin d’émetteurs approuvés spécifié par l’appelant. Le certificat doit également être émis par un émetteur figurant dans la liste des émetteurs approuvés.|
|2|Approbation de l’autorité de certification exclusive|Requiert qu’un certificat client soit chaîné vers un certificat d’autorité de certification intermédiaire ou un certificat racine du magasin d’émetteurs approuvés spécifié par l’appelant.|

Pour plus d’informations sur les échecs d’authentification dus aux problèmes de configuration des émetteurs approuvés, consultez l’article [280256](https://support.microsoft.com/kb/2802568) de la Base de connaissances.

### <a name="tls-support-for-server-name-indicator-sni-extensions"></a><a name="BKMK_SNI"></a>Prise en charge TLS pour les extensions SNI (Server Name Indicator)
La fonctionnalité d’indication du nom de serveur étend les protocoles SSL et TLS pour permettre l’identification correcte du serveur lorsque de nombreuses images virtuelles s’exécutent sur un serveur unique. Pour sécuriser correctement les communications entre un ordinateur client et un serveur, l’ordinateur client demande un certificat numérique au serveur. Une fois que le serveur a répondu à la demande et envoyé le certificat, l’ordinateur client l’examine, l’utilise pour chiffrer les communications et poursuit avec l’échange de type demande-réponse normal. Toutefois, dans un scénario d’hébergement virtuel, plusieurs domaines, chacun avec son propre certificat potentiellement distinct, sont hébergés sur un seul serveur. Dans ce cas, le serveur n’a aucun moyen de savoir au préalable quel certificat envoyer à l’ordinateur client. La fonctionnalité SNI permet à l’ordinateur client d’informer le domaine cible plus tôt dans le protocole et cela permet au serveur de sélectionner correctement le certificat approprié.

**Quelle est la valeur ajoutée ?**

Ces fonctionnalités supplémentaires :

-   vous permettent d’héberger plusieurs sites Web SSL sur une combinaison de port et d’adresse IP unique ;

-   réduisent l’utilisation de la mémoire lorsque plusieurs sites Web SSL sont hébergés sur un serveur Web unique ;

-   permettent à plusieurs utilisateurs de se connecter simultanément à des sites Web SSL ;

-   vous permettent de fournir des indications aux utilisateurs finaux par le biais de l’interface de l’ordinateur pour qu’ils sélectionnent le certificat correct au cours d’un processus d’authentification client.

**Fonctionnement**

Le fournisseur SSP Schannel maintient un cache en mémoire des états de connexion client autorisés pour les clients. Cela permet aux ordinateurs clients de se reconnecter rapidement au serveur SSL sans faire l’objet d’une négociation SSL complète lors des visites suivantes.  Cette utilisation efficace de la gestion des certificats permet d’héberger plus de sites sur une seule version de Windows Server 2012 par rapport aux versions précédentes du système d’exploitation.

La sélection de certificat par l’utilisateur final a été améliorée en vous permettant d’établir une liste de noms d’émetteurs de certificats probables afin de fournir à l’utilisateur final des indications quant au certificat à choisir. Cette liste peut être configurée à l’aide d’une stratégie de groupe.

### <a name="datagram-transport-layer-security-dtls"></a><a name="BKMK_DTLS"></a>DTLS (Datagram Transport Layer Security)
Le protocole DTLS version 1.0 a été ajouté au fournisseur SSP Schannel. Le protocole DTLS assure la confidentialité des communications pour les protocoles de datagrammes. Le protocole permet aux applications client/serveur de communiquer d’une manière conçue pour empêcher toute tentative d’écoute clandestine, de falsification ou de contrefaçon de message. Le protocole DTLS s’appuie sur le protocole TLS (Transport Layer Security) et fournit des garanties de sécurité équivalentes, réduisant le besoin d’utiliser IPsec ou concevant un protocole personnalisé de sécurité de la couche Application.

**Quelle est la valeur ajoutée ?**

Les datagrammes sont courants en diffusion multimédia en continu, comme les jeux ou la vidéoconférence sécurisée. L’ajout du protocole DTLS au fournisseur Schannel vous permet d’utiliser le modèle Windows SSPI familier pour la sécurisation des communications entre les ordinateurs clients et les serveurs. Le protocole DTLS a été délibérément conçu pour être aussi similaire que possible au protocole TLS, à la fois pour réduire au maximum les innovations en termes de sécurité et pour augmenter au maximum le degré de réutilisation de code et d’infrastructure.

**Fonctionnement**

Les applications qui utilisent DTLS sur UDP peuvent utiliser le modèle SSPI dans Windows Server 2012 et Windows 8. Certaines suites de chiffrement sont disponibles pour être configurées de façon similaire à TLS. Schannel continue à utiliser le fournisseur de services de chiffrement CNG qui tire profit de la certification FIPS 140, laquelle a été introduite pour la première fois dans Windows Vista.

### <a name="deprecated-functionality"></a><a name="BKMK_Deprecated"></a>Fonctionnalités déconseillées
Dans le SSP Schannel pour Windows Server 2012 et Windows 8, il n’existe pas de fonctionnalités ou de fonctionnalités déconseillées.

## <a name="see-also"></a>Voir aussi
-   [Modèle de sécurité du Cloud privé-fonctionnalités du Wrapper](https://social.technet.microsoft.com/wiki/contents/articles/6756.private-cloud-security-model-wrapper-functionality.aspx)



