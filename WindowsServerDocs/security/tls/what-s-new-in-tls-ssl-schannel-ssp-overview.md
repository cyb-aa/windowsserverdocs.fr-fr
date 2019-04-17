---
title: "TLS - vue d’ensemble du protocole SSL (SSP Schannel)"
description: "Sécurité de Windows Server"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c8836345-16bb-4dcc-8d2b-2b9b687456a3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: b846ed54a1f7c8ef7a85ea9f836ffa75d0036ae6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="overview-of-tls---ssl-schannel-ssp"></a>Vue d’ensemble de TLS / SSL (SSP Schannel)

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique destinée aux professionnels de l’informatique décrit les modifications apportées aux fonctionnalités dans la sécurité prise en charge de fournisseur (SSP Schannel), qui inclut la sécurité TLS (Transport Layer) et les protocoles d’authentification DTLS Datagram Transport Layer Security (), la couche SSL (Secure Sockets) pour Windows Server 2012 R2, Windows Server 2012, Windows 8.1 et Windows 8.

Schannel est un fournisseur SSP (Security Support Provider) qui implémente les protocoles d’authentification standard SSL, TLS et DTLS Internet. Le fournisseur d’Interface SSPI (Security Support) est une API utilisée par les systèmes Windows pour exécuter des fonctions liées à la sécurité, y compris l’authentification. L’interface SSPI fonctionne comme une interface commune à plusieurs fournisseurs de prise en charge de sécurité (SSP), y compris le SSP. Schannel

Pour plus d’informations sur l’implémentation Microsoft de TLS et SSL dans le SSP Schannel, voir la [référence technique de TLS/SSL (2003)](https://technet.microsoft.com/library/cc784149(v=ws.10).aspx).


##<a name="tlsssl-schannel-ssp-features"></a>Fonctionnalités TLS/SSL (SSP Schannel)
La section suivante décrit les fonctionnalités de TLS dans le SSP. Schannel

### <a name="tls-session-resumption"></a>Reprise de session TLS
Le protocole de sécurité TLS (Transport Layer), un composant du fournisseur SSP Schannel, est utilisé pour sécuriser les données envoyées entre les applications via un réseau non approuvé. TLS/SSL peut être utilisé pour authentifier les serveurs et les ordinateurs clients et pour crypter les messages entre les parties authentifiées.

Les périphériques qui connectent fréquemment TLS aux serveurs doivent se reconnecter en raison de l’expiration de la session. Windows 8.1 et Windows Server 2012 R2 prennent désormais en charge RFC 5077 (reprise de Session TLS sans état côté serveur). Cette modification fournit aux périphériques Windows Phone et Windows RT:

-   Utilisation des ressources réduites sur le serveur

-   Bande passante réduite, ce qui améliore l’efficacité des connexions client

-   Réduit le temps passé pour les négociations TLS de la connexion.

> [!NOTE]
> L’implémentation côté client de RFC 5077 a été ajoutée dans Windows 8.

Pour plus d’informations sur la reprise de session TLS sans état, consultez le document IETF [RFC 5077.](http://www.ietf.org/rfc/rfc5077)

### <a name="application-protocol-negotiation"></a>Négociation de protocole d’application
 Windows Server 2012 R2 et Windows 8.1 prennent en charge la négociation de protocole d’application TLS côté client afin que les applications puissent exploiter les protocoles dans le cadre du développement standard HTTP 2.0 et les utilisateurs peuvent accéder aux services en ligne comme Google et Twitter à l’aide des applications qui s’exécutent le protocole SPDY.

**Fonctionnement**

Applications serveur et client activer une extension de négociation de protocole application en fournissant des listes d’ID de protocoles d’application pris en charge dans l’ordre de préférence décroissant. Le client TLS indique qu’il prend en charge la négociation de protocole d’application en incluant l’extension de négociation de protocole couche Application (ALPN) avec une liste de protocoles client pris en charge dans le message ClientHello.

Lorsque le client TLS effectue la demande au serveur, le serveur TLS lit sa liste de protocoles pris en charge pour le protocole d’application favori également prend en charge par le client. Si ce protocole est trouvé, le serveur répond avec l’ID de protocole sélectionné et poursuit avec la négociation comme d’habitude. S’il n’existe aucun protocole d’application commun, le serveur envoie une alerte d’échec de négociation irrécupérable.

### <a name="BKMK_TrustedIssuers"></a>Gestion des émetteurs approuvés pour l’authentification du client
Lorsque l’authentification de l’ordinateur client est requise à l’aide de SSL ou TLS, le serveur peut être configuré pour envoyer une liste d’émetteurs de certificats approuvés. Cette liste contient l’ensemble des émetteurs de certificats qui approuvent le serveur et une indication sur l’ordinateur client certificat client sélectionner au cas où plusieurs certificats sont présents. En outre, l’ordinateur client envoie au serveur de la chaîne de certificats doit être validée par rapport à la liste d’émetteurs approuvés configurée.

Avant Windows Server 2012 et Windows 8, les applications ou les processus qui a utilisé le SSP Schannel (y compris IIS et HTTP.sys) pouvaient fournir une liste d’émetteurs approuvés qu’ils pris en charge pour l’authentification du Client via une liste d’approbation de certificats (CTL).

Dans Windows Server 2012 et Windows 8, les modifications ont été apportées au processus d’authentification sous-jacent afin que:

-   Gérer la liste des émetteurs approuvés CTL n’est plus pris en charge.

-   L’envoi par défaut de la liste des émetteurs approuvés est désactivé: valeur par défaut de la clé de Registre SendTrustedIssuerList est désormais 0 (désactivée par défaut) au lieu de 1.

-   La compatibilité avec les versions précédentes des systèmes d’exploitation Windows est préservée.

**Cette valeur ajoute?**

À compter de Windows Server 2012, l’utilisation de la liste CTL a été remplacée par une implémentation basée sur le magasin de certificats. Cela rend plus facile et conviviale via les applets de commande de gestion certificat existantes du fournisseur PowerShell, ainsi que les outils de ligne de commande comme certutil.exe.

Bien que la taille maximale de la liste d’autorités de certification de confiance que le SSP Schannel prend en charge (16 Ko) reste identique à celle de Windows Server 2008 R2, Windows Server 2012, il existe un certificat dédié stocker pour émetteurs d’authentification de client afin que les certificats non liés ne sont pas inclus dans le message.

**Fonctionnement?**

Dans Windows Server 2012, la liste des émetteurs approuvés est configurée à l’aide des magasins de certificats; magasin de certificats d’ordinateur global par défaut et qui est facultatif par site. La source de la liste est être déterminée comme suit:

-   S’il existe une banque d’informations d’identification spécifiques configurée pour le site, il sera utilisé comme source

-   Si aucun certificat n’existe dans le magasin défini par l’application, Schannel vérifie ensuite le **émetteurs d’authentification de Client** stocker sur l’ordinateur local et, si les certificats sont présents, utilise ce magasin comme source. Si aucun certificat n’est trouvé dans les deux magasins, le magasin racines de confiance est vérifié.

-   Si ni les magasins globaux ou locaux contiennent les certificats, le fournisseur Schannel utilise le **autorités de certification racines** stocker en tant que source de la liste d’émetteurs approuvés. (Il s’agit du comportement pour Windows Server 2008 R2).

Si le **autorités de certification racines de confiance** magasin qui a été utilisé contient un mélange des certificats d’émetteur d’autorité de certification racine (auto-signé), seuls les certificats d’émetteur d’autorité de certification seront envoyés au serveur par défaut.

**Comment configurer Schannel pour utiliser le magasin de certificats pour émetteurs approuvés**

Architecture du SSP Schannel dans Windows Server 2012 par défaut utilise les magasins comme décrit ci-dessus pour gérer la liste des émetteurs approuvés. Vous pouvez toujours utiliser les applets de commande de gestion certificat existantes du fournisseur PowerShell, ainsi que les outils de ligne de commande comme Certutil pour gérer les certificats.

Pour plus d’informations sur la gestion des certificats à l’aide du fournisseur PowerShell, consultez [AD CS applets de commande dans Windows](https://technet.microsoft.com/library/hh848365(v=wps.620).aspx).

Pour plus d’informations sur la gestion des certificats à l’aide de l’utilitaire de certificat, consultez [certutil.exe](https://technet.microsoft.com/library/cc732443.aspx).

Pour plus d’informations sur les données, y compris le magasin défini par l’application, sont définies pour une information d’identification Schannel, consultez [structure SCHANNEL_CRED (Windows)](https://msdn.microsoft.com/library/windows/desktop/aa379810(v=vs.85).aspx).

**Valeurs par défaut pour les Modes d’approbation**

Il existe trois Client confiance Modes d’authentification pris en charge par le fournisseur Schannel. Le mode d’approbation contrôle la validation de la chaîne de certificats du client est effectuée et un paramètre système contrôlé par le REG_DWORD "ClientAuthTrustMode" sous HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\Schannel.

|Valeur|Mode de confiance|Description|
|-----|-------|--------|
|0|Approbation de l’ordinateur (par défaut)|Requiert que le certificat client soit émis par un certificat dans la liste des émetteurs approuvés.|
|1|Approbation de racine exclusive|Requiert qu’un certificat client est lié à un certificat racine contenu dans le magasin d’émetteurs approuvés spécifié par l’appelant. Le certificat doit également être émis par un émetteur figurant dans la liste des émetteurs approuvés|
|2|Approbation de l’autorité de certification exclusive|Requiert qu’une chaîne de certificats du client à un certificat d’autorité de certification intermédiaire ou un certificat racine dans le spécifié par l’appelant approuvé store de l’émetteur.|

Pour plus d’informations sur les échecs d’authentification en raison de problèmes de configuration des émetteurs approuvés, consultez l’article de la Base de connaissances [280256](https://support.microsoft.com/kb/2802568).

### <a name="BKMK_SNI"></a>Prise en charge TLS pour les Extensions de l’indicateur de nom de serveur (SNI)
Fonctionnalité d’Indication du nom de serveur étend les protocoles SSL et TLS pour permettre l’identification correcte du serveur lorsque de nombreuses images virtuelles s’exécutent sur un serveur unique. Pour sécuriser la communication entre un ordinateur client et un serveur, l’ordinateur client demande un certificat numérique à partir du serveur. Une fois que le serveur répond à la demande et envoie le certificat, l’ordinateur client examine utilise pour chiffrer les communications et poursuit avec l’échange de demande-réponse normal. Toutefois, dans un scénario d’hébergement virtuel, plusieurs domaines, chacun avec son propre certificat potentiellement distinct, sont hébergés sur un seul serveur. Dans ce cas, le serveur n’a aucun moyen de savoir au préalable quel certificat pour envoyer à l’ordinateur client. La fonctionnalité SNI permet à l’ordinateur client informer le domaine cible plus tôt dans le protocole et cela permet au serveur de sélectionner correctement le certificat approprié.

**Cette valeur ajoute?**

Cette fonctionnalité supplémentaire:

-   Vous permet d’héberger plusieurs sites Web SSL sur une combinaison de port et d’adresse IP unique

-   Réduit l’utilisation de la mémoire lorsque plusieurs sites Web SSL sont hébergés sur un serveur web unique

-   Permet à plusieurs utilisateurs de se connecter simultanément à des sites Web SSL

-   Permet de fournir des indications aux utilisateurs finaux par le biais de l’interface de l’ordinateur pour sélectionner le certificat correct pendant un processus d’authentification client.

**Fonctionnement**

Le SSP Schannel maintient un cache en mémoire des États de connexion client autorisés pour les clients. Ainsi, les ordinateurs clients se reconnecter rapidement au serveur SSL sans soumis à une négociation SSL complète lors des visites suivantes.  Cette utilisation efficace de la gestion des certificats permet à plusieurs sites à être hébergé sur un seul Windows Server 2012 par rapport aux versions précédentes du système d’exploitation.

Sélection de certificat par l’utilisateur final a été améliorée en vous permettant de créer une liste des noms d’émetteurs de certificats probables qui fournissent l’utilisateur final des indications sur celui que vous souhaitez choisir. Cette liste est configurable à l’aide de la stratégie de groupe.

### <a name="BKMK_DTLS"></a>Datagram Transport Layer Security (DTLS)
Le protocole DTLS version 1.0 a été ajouté pour le fournisseur SSP Schannel. Le protocole DTLS assure la confidentialité des communications pour les protocoles de datagrammes. Le protocole permet des applications de communiquer d’une manière qui est conçue pour empêcher l’écoute clandestine, de falsification ou de contrefaçon de message client/serveur. Le protocole DTLS est basé sur le protocole de sécurité TLS (Transport Layer) et fournit des garanties de sécurité équivalentes, réduisant le besoin d’utiliser IPsec ou concevant un protocole de sécurité de couche application personnalisée.

**Cette valeur ajoute?**

Les datagrammes sont communs dans en continu, telles que des jeux ou sécurisé vidéoconférence. Ajouter le protocole DTLS au fournisseur Schannel vous donne la possibilité d’utiliser le modèle Windows SSPI familier sécurisation des communications entre les ordinateurs clients et serveurs. DTLS est délibérément conçu pour être aussi similaire au protocole TLS en tant que possible, à la fois pour réduire nouveaux termes de sécurité et pour optimiser la quantité de la réutilisation du code et d’infrastructure.

**Fonctionnement**

Les applications qui utilisent DTLS sur UDP peuvent utiliser le modèle SSPI dans Windows Server 2012 et Windows 8. Certaines suites de chiffrement sont disponibles pour la configuration, similaire à la façon dont vous pouvez configurer TLS. Schannel continue d’utiliser le fournisseur de chiffrement CNG qui tire profit de la certification FIPS 140, ce qui a été introduite dans Windows Vista.

### <a name="BKMK_Deprecated"></a>Fonctionnalités déconseillées
Dans le SSP Schannel pour Windows Server 2012 et Windows 8, il existe sans déconseillées fonctions ou fonctionnalités.

## <a name="see-also"></a>Voir aussi
-   [Modèle de sécurité nuage privé - fonctionnalités du Wrapper](https://social.technet.microsoft.com/wiki/contents/articles/6756.private-cloud-security-model-wrapper-functionality.aspx)



