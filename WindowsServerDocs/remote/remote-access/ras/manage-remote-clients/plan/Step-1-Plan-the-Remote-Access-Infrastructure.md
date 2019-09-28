---
title: Étape 1 planifier l’infrastructure d’accès à distance
description: Cette rubrique fait partie du guide gérer des clients DirectAccess à distance dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a1ce7af5-f3fe-4fc9-82e8-926800e37bc1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c8db30d3c5512fc72648c7894d66b715850fb619
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367308"
---
# <a name="step-1-plan-the-remote-access-infrastructure"></a>Étape 1 planifier l’infrastructure d’accès à distance

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

> [!NOTE]
> Windows Server 2016 combine DirectAccess et le service de routage et d’accès à distance (RRAS) dans un rôle d’accès à distance unique.  
  
Cette rubrique décrit les étapes de planification d’une infrastructure que vous pouvez utiliser pour configurer un serveur d’accès à distance unique pour la gestion à distance des clients DirectAccess. Le tableau suivant répertorie les étapes, mais ces tâches de planification n’ont pas besoin d’être effectuées dans un ordre spécifique.  
  
|Tâche|Description|  
|----|--------|  
|[Planifier la topologie du réseau et les paramètres du serveur](#plan-network-topology-and-settings)|Décidez où placer le serveur d’accès à distance (à la périphérie ou derrière un pare-feu ou un périphérique de traduction d’adresses réseau (NAT)) et planifiez l’adressage IP et le routage.|  
|[Planifier la configuration requise du pare-feu](#plan-firewall-requirements)|Planifiez l'autorisation de l'accès à distance via des pare-feu de périmètre.|  
|[Planifier les exigences de certificat](#plan-certificate-requirements)|Décidez si vous allez utiliser le protocole Kerberos ou des certificats pour l’authentification du client, et Planifiez vos certificats de site Web.<br/><br/>IP-HTTPS est un protocole de transition utilisé par les clients DirectAccess pour transférer le trafic IPv6 via des réseaux IPv4. Décidez s’il faut authentifier IP-HTTPs pour le serveur à l’aide d’un certificat émis par une autorité de certification, ou à l’aide d’un certificat auto-signé émis automatiquement par le serveur d’accès à distance.|  
|[Planifier la configuration DNS requise](#plan-dns-requirements)|Planifiez les paramètres DNS (Domain Name System) pour le serveur d’accès à distance, les serveurs d’infrastructure, les options de résolution de noms locale et la connectivité client.| 
|[Planifier la configuration du serveur emplacement réseau](#plan-the-network-location-server-configuration)|Décidez où placer le site Web du serveur emplacement réseau dans votre organisation (sur le serveur d’accès à distance ou un autre serveur) et planifiez les certificats requis si le serveur emplacement réseau se trouve sur le serveur d’accès à distance. **Remarque :** Les clients DirectAccess utilisent le serveur Emplacement réseau pour déterminer s'ils se trouvent sur le réseau interne.|  
|[Planifier les configurations des serveurs d’administration](#plan-management-servers-configuration)|Planifiez les serveurs d’administration (tels que les serveurs de mise à jour) utilisés lors de l’administration à distance des clients. **Remarque :** Les administrateurs peuvent administrer à distance les ordinateurs clients DirectAccess situés en dehors du réseau d’entreprise à l’aide d’Internet.|  
|[Planifier les exigences de Active Directory](#plan-active-directory-requirements)|Planifiez vos contrôleurs de domaine, vos exigences de Active Directory, l’authentification du client et la structure de plusieurs domaines.|  
|[Planifier la création d’objets stratégie de groupe](#plan-group-policy-object-creation)|Décidez quels objets de stratégie de groupe sont requis dans votre organisation et comment créer et modifier les objets de stratégie de groupe.|  
  
## <a name="plan-network-topology-and-settings"></a>Planifier la topologie et les paramètres réseau  
Lorsque vous planifiez votre réseau, vous devez tenir compte de la topologie de la carte réseau, des paramètres d’adressage IP et des conditions requises pour ISATAP.  
  
### <a name="plan-network-adapters-and-ip-addressing"></a>Planifier les cartes réseau et l'adressage IP  
  
1.  Identifiez la topologie de carte réseau que vous souhaitez utiliser. L’accès à distance peut être configuré avec l’une des topologies suivantes :  
  
    -   Avec deux cartes réseau : Le serveur d’accès à distance est installé en périphérie avec une carte réseau connectée à Internet et l’autre au réseau interne.  
  
    -   Avec deux cartes réseau : Le serveur d’accès à distance est installé derrière un périphérique NAT, un pare-feu ou un routeur, avec une carte réseau connectée à un réseau de périmètre et l’autre au réseau interne.  
  
    -   Avec une carte réseau : Le serveur d’accès à distance est installé derrière un périphérique NAT et l’unique carte réseau est connectée au réseau interne.  
  
2.  Identifiez vos exigences en matière d'adressage IP :  
  
    DirectAccess utilise IPv6 avec IPsec pour créer une connexion sécurisée entre les ordinateurs clients DirectAccess et le réseau d'entreprise interne. Pourtant, DirectAccess ne requiert pas systématiquement une connectivité Internet IPv6 ni une prise en charge IPv6 native sur les réseaux internes. Au lieu de cela, il configure et utilise automatiquement des technologies de transition IPv6 pour transférer le trafic IPv6 sur Internet IPv4 (6to4, Teredo ou IP-HTTPs) et sur votre intranet IPv4 uniquement (NAT64 ou ISATAP). Pour obtenir une vue d'ensemble de ces technologies de transition, consultez les ressources suivantes :  
  
    -   [Technologies de transition IPv6](https://technet.microsoft.com/library/bb726951.aspx)  
  
    -   [Spécification du protocole de tunnel IP-HTTPs](https://msdn.microsoft.com/library/dd358571.aspx)  
  
3.  Configurez les cartes et l'adressage requis selon le tableau suivant. Pour les déploiements qui se trouvent derrière un périphérique NAT à l’aide d’une seule carte réseau, configurez vos adresses IP en utilisant uniquement la colonne **carte réseau interne** .  
  
    ||Carte réseau externe|Carte réseau interne<sup>1, ci-dessus</sup>|Exigences en matière de routage|  
    |-|--------------|------------------------|------------|  
    |Internet IPv4 et intranet IPv4|Configurez ce qui suit :<br/><br/>-Deux adresses IPv4 publiques consécutives statiques avec les masques de sous-réseau appropriés (requis pour Teredo uniquement).<br/>-Adresse IPv4 de passerelle par défaut de votre pare-feu Internet ou du routeur de votre fournisseur de services Internet local. **Remarque :** Le serveur d’accès à distance requiert deux adresses IPv4 publiques consécutives afin qu’il puisse agir en tant que serveur Teredo et que les clients Teredo Windows puissent utiliser le serveur d’accès à distance pour détecter le type de périphérique NAT.|Configurez ce qui suit :<br/><br/>-Une adresse intranet IPv4 avec le masque de sous-réseau approprié.<br/>-Suffixe DNS spécifique à la connexion pour votre espace de noms intranet. Vous devez également configurer un serveur DNS sur l'interface interne. **Attention :** Ne configurez pas de passerelle par défaut sur toutes les interfaces intranet.|Pour configurer le serveur d’accès à distance afin d’atteindre tous les sous-réseaux du réseau IPv4 interne, procédez comme suit :<br/><br/>-Répertorier les espaces d’adressage IPv4 pour tous les emplacements sur votre intranet.<br/>-Utilisez les commandes `route add -p` ou `netsh interface ipv4 add route` pour ajouter les espaces d’adressage IPv4 en tant qu’itinéraires statiques dans la table de routage IPv4 du serveur d’accès à distance.|  
    |Internet IPv6 et intranet IPv6|Configurez ce qui suit :<br/><br/>-Utiliser la configuration d’adresse configurée automatiquement fournie par votre fournisseur de services Internet.<br/>-Utilisez la commande `route print` pour vous assurer qu’un itinéraire IPv6 par défaut qui pointe vers le routeur du fournisseur de services Internet existe dans la table de routage IPv6.<br/>-Déterminez si le fournisseur de services Internet et les routeurs intranet utilisent les préférences de routeur par défaut décrites dans le document RFC 4191 et s’ils utilisent une préférence par défaut supérieure à celle de vos routeurs intranet locaux. Si vous avez répondu par l'affirmative dans les deux cas, aucune autre configuration n'est requise pour l'itinéraire par défaut. Une préférence plus élevée pour le routeur ISP permet de s'assurer que l'itinéraire IPv6 par défaut actif du serveur d'accès à distance pointe vers Internet IPv6.<br/><br/>Étant donné que le serveur d'accès à distance est un routeur IPv6, si vous disposez d'une infrastructure IPv6 native, l'interface Internet peut également atteindre les contrôleurs de domaine sur l'intranet. Dans ce cas, ajoutez des filtres de paquets au contrôleur de domaine dans le réseau de périmètre pour empêcher la connectivité à l’adresse IPv6 de l’interface Internet du serveur d’accès à distance.|Configurez ce qui suit :<br/><br/>Si vous n’utilisez pas les niveaux de préférence par défaut, configurez vos interfaces intranet à l’aide de la commande `netsh interface ipv6 set InterfaceIndex ignoredefaultroutes=enabled`. Cette commande vérifie que les itinéraires par défaut supplémentaires qui pointent vers les routeurs intranet ne seront pas ajoutés à la table de routage IPv6. Vous pouvez obtenir le InterfaceIndex de vos interfaces intranet à partir de l’affichage de la commande `netsh interface show interface`.|Si vous avez un intranet IPv6, procédez comme suit pour configurer le serveur d'accès à distance afin d'atteindre tous les emplacements IPv6 :<br/><br/>-Répertorier les espaces d’adressage IPv6 pour tous les emplacements sur votre intranet.<br/>-Utilisez la commande `netsh interface ipv6 add route` pour ajouter les espaces d’adressage IPv6 en tant qu’itinéraires statiques dans la table de routage IPv6 du serveur d’accès à distance.|  
    |Internet IPv4 et intranet IPv6|Le serveur d’accès à distance transfère le trafic de l’itinéraire IPv6 par défaut à l’aide de l’interface de l’adaptateur Microsoft 6to4 vers un relais 6to4 sur Internet IPv4. Lorsque le protocole IPv6 natif n’est pas déployé dans le réseau d’entreprise, vous pouvez utiliser la commande suivante pour configurer un serveur d’accès à distance pour l’adresse IPv4 du relais Microsoft 6to4 sur Internet IPv4 : `netsh interface ipv6 6to4 set relay name=<ipaddress> state=enabled`.|||  
  
    > [!NOTE]  
    > -   Si une adresse IPv4 publique a été attribuée au client DirectAccess, il utilisera la technologie de relais 6to4 pour se connecter à l’intranet. Si une adresse IPv4 privée est affectée au client, celle-ci utilise Teredo. Si le client DirectAccess ne peut pas se connecter au serveur DirectAccess avec 6to4 ou Teredo, il utilisera IP-HTTPS.  
    > -   Pour utiliser Teredo, vous devez configurer deux adresses IP consécutives sur la carte réseau tournée vers l'extérieur.  
    > -   Vous ne pouvez pas utiliser Teredo si le serveur d’accès à distance ne possède qu’une seule carte réseau.  
    > -   Les ordinateurs clients IPv6 natif se connectent au serveur d'accès à distance via IPv6 natif et aucune technologie de transition n'est requise.  
  
### <a name="plan-isatap-requirements"></a>Planifier les exigences ISATAP  
ISATAP est requis pour la gestion à distance de DirectAccessclients, de sorte que les serveurs d’administration DirectAccess puissent se connecter aux clients DirectAccess situés sur Internet. ISATAP n’est pas requis pour prendre en charge les connexions initiées par les ordinateurs clients DirectAccess aux ressources IPv4 sur le réseau d’entreprise. NAT64/DNS64 est utilisé à ces fins. Si votre déploiement requiert ISATAP, utilisez le tableau suivant pour identifier vos besoins.  
  
|Scénario de déploiement ISATAP|Configuration requise|  
|---------------|--------|  
|Intranet IPv6 natif existant (aucun ISATAP n’est requis)|Avec une infrastructure IPv6 native existante, vous spécifiez le préfixe de l’organisation pendant le déploiement de l’accès à distance, et le serveur d’accès à distance ne se configure pas en tant que routeur ISATAP. Procédez comme suit :<br/><br/>1.  Pour vous assurer que les clients DirectAccess sont accessibles à partir de l’intranet, vous devez modifier votre routage IPv6 afin que le trafic de l’itinéraire par défaut soit transféré au serveur d’accès à distance. Si l’espace d’adressage IPv6 de votre intranet utilise une adresse autre qu’un préfixe d’adresse IPv6 48 bits unique, vous devez spécifier le préfixe IPv6 de l’organisation concerné au cours du déploiement.<br/>2.  Si vous êtes actuellement connecté à Internet IPv6, vous devez configurer le trafic de votre itinéraire par défaut afin qu’il soit transféré au serveur d’accès à distance, puis configurer les connexions et itinéraires appropriés sur le serveur d’accès à distance, afin que l’itinéraire par défaut le trafic est transféré à l’appareil connecté à Internet IPv6.|  
|Déploiement ISATAP existant|Si vous disposez d’une infrastructure ISATAP, pendant le déploiement, vous êtes invité à fournir le préfixe 48 bits de l’organisation, et le serveur d’accès à distance ne se configure pas en tant que routeur ISATAP. Pour vous assurer que les clients DirectAccess sont accessibles à partir de l’intranet, vous devez modifier votre infrastructure de routage IPv6 afin que le trafic de l’itinéraire par défaut soit transféré au serveur d’accès à distance. Cette modification doit être effectuée sur le routeur ISATAP existant sur lequel les clients intranet doivent déjà transférer le trafic par défaut.|  
|Aucune connectivité IPv6 existante|Lorsque l’Assistant Configuration de l’accès à distance détecte que le serveur n’a pas de connectivité IPv6 native ou ISATAP, il dérive automatiquement un préfixe 48 bits 6to4 pour l’intranet et configure le serveur d’accès à distance en tant que routeur ISATAP pour fournir IPv6. connectivité aux hôtes ISATAP sur votre intranet. (Un préfixe 6to4 est utilisé uniquement si le serveur a des adresses publiques ; sinon, le préfixe est généré automatiquement à partir d’une plage d’adresses locales unique.)<br/><br/>Pour utiliser ISATAP, procédez comme suit :<br/><br/>1.  Inscrivez le nom ISATAP sur un serveur DNS pour chaque domaine sur lequel vous souhaitez activer la connectivité ISATAP, afin que le nom ISATAP puisse être résolu par le serveur DNS interne à l’adresse IPv4 interne du serveur d’accès à distance.<br/>2.  Par défaut, les serveurs DNS exécutant Windows Server 2012, Windows Server 2008 R2, Windows Server 2008 ou Windows Server 2003 bloquent la résolution du nom ISATAP à l’aide de la liste rouge de requêtes globale. Pour activer ISATAP, vous devez supprimer le nom ISATAP de la liste rouge. Pour plus d'informations, consultez [Supprimer ISATAP de la liste rouge de requêtes globale DNS](https://go.microsoft.com/fwlink/p/?LinkId=168593).<br/><br/>Les hôtes ISATAP Windows qui peuvent résoudre le nom ISATAP configurent automatiquement une adresse avec le serveur d’accès à distance comme suit :<br/><br/>1.  Une adresse IPv6 basée sur ISATAP sur une interface de tunnel ISATAP<br/>2.  Un itinéraire 64 bits qui fournit la connectivité aux autres hôtes ISATAP sur l’intranet<br/>3.  Itinéraire IPv6 par défaut qui pointe vers le serveur d’accès à distance. L’itinéraire par défaut garantit que les hôtes ISATAP intranet peuvent atteindre les clients DirectAccess<br/><br/>Lorsque vos hôtes ISATAP basés sur Windows obtiennent une adresse IPv6 basée sur ISATAP, ils commencent à utiliser le trafic encapsulé ISATAP pour communiquer si la destination est également un hôte ISATAP. Étant donné que ISATAP utilise un seul sous-réseau 64 bits pour l’intranet entier, votre communication passe d’un modèle IPv4 de communication segmenté à un modèle de communication de sous-réseau unique avec IPv6. Cela peut affecter le comportement de certains Active Directory Domain Services (AD DS) et les applications qui reposent sur la configuration de vos sites et services Active Directory. Par exemple, si vous avez utilisé le composant logiciel enfichable sites et services Active Directory pour configurer des sites, des sous-réseaux IPv4 et des transports intersites pour transférer des demandes aux serveurs au sein de sites, cette configuration n’est pas utilisée par les hôtes ISATAP.<br/><br/><ol><li>Pour configurer des sites et des services Active Directory pour le transfert au sein de sites pour les hôtes ISATAP, pour chaque objet de sous-réseau IPv4, vous devez configurer un objet de sous-réseau IPv6 équivalent, dans lequel le préfixe d’adresse IPv6 du sous-réseau exprime la même plage d’hôtes ISATAP. adresses en tant que sous-réseau IPv4. Par exemple, pour le sous-réseau IPv4 192.168.99.0/24 et le préfixe d’adresse ISATAP 64 bits 2002:836b : 1:8000 ::/64, le préfixe d’adresse IPv6 équivalent pour l’objet de sous-réseau IPv6 est 2002:836b : 1:8000:0 : 5efe : 192.168.99.0/120. Pour une longueur de préfixe IPv4 arbitraire (définie sur 24 dans l’exemple), vous pouvez déterminer la longueur de préfixe IPv6 correspondante à partir de la formule 96 + IPv4PrefixLength.</li><li>Pour les adresses IPv6 des clients DirectAccess, ajoutez ce qui suit :<br/><br/><ul><li>Pour les clients DirectAccess Teredo : Un sous-réseau IPv6 pour la plage 2001:0 : WWXX : YYZZ ::/64, dans laquelle WWXX : YYZZ est la version hexadécimale à deux-points de la première adresse IPv4 accessible sur Internet du serveur d’accès à distance. .</li><li>Pour les clients DirectAccess IP-HTTPs : Un sous-réseau IPv6 pour la plage 2002 : WWXX : YYZZ : 8100 ::/56, dans laquelle WWXX : YYZZ est la version hexadécimale à deux-points de la première adresse IPv4 accessible sur Internet (w.x.y. z) du serveur d’accès à distance. .</li><li>Pour les clients DirectAccess 6to4 : Série de préfixes IPv6 6to4 qui commencent par 2002: et représente les préfixes d'adresses IPv4 régionales, publiques qui sont administrés par l'autorité IANA (Internet Assigned Numbers Authority) et les registres régionaux. Le préfixe 6to4 pour un préfixe d’adresse IPv4 publique w. x. y. z/n est 2002 : WWXX : YYZZ ::/[16 + n], où WWXX : YYZZ est la version hexadécimale à deux-points de w.x.y.z.<br/><br/>        Par exemple, la plage 7.0.0.0/8 est administrée par le registre ARIN (American Registry for Internet Numbers) pour l'Amérique du Nord. Le préfixe 6to4 correspondant à cette plage d’adresses IPv6 publiques est 2002:700 ::/24. Pour plus d’informations sur l’espace d’adressage public IPv4, consultez Registre de l' [espace d’adressage IPv4 IANA](https://www.iana.org/assignments/ipv4-address-space/ipv4-address-space.xml). .</li></ul></li></ol>|  
  
> [!IMPORTANT]  
> Assurez-vous que vous n’avez pas d’adresses IP publiques sur l’interface interne du serveur DirectAccess. Si vous avez une adresse IP publique sur l’interface interne, la connectivité via ISATAP peut échouer.  
  
### <a name="plan-firewall-requirements"></a>Planifier la configuration requise pour le pare-feu  
Si le serveur d'accès à distance se trouve derrière un pare-feu de périmètre, les exceptions suivantes sont requises pour le trafic d'accès à distance quand le serveur d'accès à distance est sur Internet IPv4 :  
  
-   Pour IP-HTTPs : Port de destination TCP (Transmission Control Protocol) 443 et port TCP source 443 sortant.  
  
-   Pour le trafic Teredo : Port de destination UDP (User Datagram Protocol) 3544 entrant et port UDP source 3544 sortant.  
  
-   Pour le trafic 6to4 : Protocole IP 41 entrant et sortant.  
  
    > [!NOTE]  
    > Pour les trafics Teredo et 6to4, ces exceptions doivent être appliquées pour les deux adresses IPv4 publiques consécutives côté Internet sur le serveur d’accès à distance.  
    >   
    > Pour IP-HTTPs, les exceptions doivent être appliquées sur l’adresse enregistrée sur le serveur DNS public.  
  
-   Si vous déployez l’accès à distance avec une seule carte réseau et installez le serveur emplacement réseau sur le serveur d’accès à distance, le port TCP 62000.  
  
    > [!NOTE]  
    > Cette exemption est sur le serveur d’accès à distance et les exemptions précédentes se trouvent sur le pare-feu de périmètre.  
  
Les exceptions suivantes sont requises pour le trafic d’accès à distance lorsque le serveur d’accès à distance se trouve sur Internet IPv6 :  
  
-   Protocole IP 50  
  
-   Port UDP de destination 500 entrant et port UDP source 500 sortant.  
  
-   Trafic ICMPv6 entrant et sortant (uniquement lors de l’utilisation de Teredo).  
  
Lorsque vous utilisez des pare-feu supplémentaires, appliquez les exceptions de pare-feu de réseau interne suivantes pour le trafic d’accès à distance :  
  
-   Pour ISATAP : Protocole 41 entrant et sortant  
  
-   Pour tout le trafic IPv4/IPv6 : TCP/UD  
  
-   Pour Teredo : ICMP pour tout le trafic IPv4/IPv6  
  
### <a name="plan-certificate-requirements"></a>Planifier les exigences en matière de certificats  
Il existe trois scénarios qui requièrent des certificats lorsque vous déployez un serveur d’accès à distance unique.  
  
-   **Authentification IPSec**: Les certificats requis pour IPsec incluent un certificat d’ordinateur utilisé par les ordinateurs clients DirectAccess lorsqu’ils établissent la connexion IPsec avec le serveur d’accès à distance, ainsi qu’un certificat d’ordinateur utilisé par les serveurs d’accès à distance pour établir Connexions IPsec avec les clients DirectAccess.  
  
    Pour DirectAccess dans Windows Server 2012, l’utilisation de ces certificats IPsec n’est pas obligatoire. En guise d’alternative, le serveur d’accès à distance peut agir en tant que proxy pour l’authentification Kerberos sans nécessiter de certificats. Si l’authentification Kerberos est utilisée, elle fonctionne via SSL et le protocole Kerberos utilise le certificat configuré pour IP-HTTPs. Certains scénarios d’entreprise (y compris l’authentification du client pour le déploiement multisite et le mot de passe à usage unique) requièrent l’utilisation de l’authentification par certificat et non de l’authentification Kerberos.  
  
-   **Serveur IP-HTTPS**: Lorsque vous configurez l’accès à distance, le serveur d’accès à distance est automatiquement configuré pour agir en tant qu’écouteur Web IP-HTTPs. Le site IP-HTTPS requiert un certificat de site web. Les ordinateurs clients doivent être en mesure de contacter le site de la liste de révocation de certificats correspondant.  
  
-   **Serveur emplacement réseau**: Le serveur Emplacement réseau est un site web utilisé pour détecter si les ordinateurs clients se trouvent dans le réseau d'entreprise. Le serveur emplacement réseau requiert un certificat de site Web. Pour ce certificat, les clients DirectAccess doivent pouvoir contacter le site contenant la liste de révocation de certificats.  
  
La configuration requise de l’autorité de certification pour chacun de ces scénarios est résumée dans le tableau suivant.  
  
|Authentification IPsec|Serveur IP-HTTPS|Serveur Emplacement réseau|  
|------------|----------|--------------|  
|Une autorité de certification interne est requise pour émettre des certificats d’ordinateur au serveur d’accès à distance et aux clients pour l’authentification IPsec lorsque vous n’utilisez pas le protocole Kerberos pour l’authentification.|Autorité de certification interne : Vous pouvez utiliser une autorité de certification interne pour émettre le certificat IP-HTTPS. Toutefois, vous devez vous assurer que le point de distribution de la liste de révocation de certificats est disponible en externe.|Autorité de certification interne : Vous pouvez utiliser une autorité de certification interne pour émettre le certificat de site web du serveur Emplacement réseau. Assurez-vous que le point de distribution de la liste de révocation de certificats est hautement disponible à partir du réseau interne.|  
||Certificat auto-signé : Vous pouvez utiliser un certificat auto-signé pour le serveur IP-HTTPs. Un certificat auto-signé n'est pas utilisable dans un déploiement multisite.|Certificat auto-signé : Vous pouvez utiliser un certificat auto-signé pour le site Web du serveur emplacement réseau. Toutefois, vous ne pouvez pas utiliser un certificat auto-signé dans un déploiement multisite.|  
||Autorité de certification publique : Nous vous recommandons d’utiliser une autorité de certification publique pour émettre le certificat IP-HTTPs. cela garantit que le point de distribution de la liste de révocation de certificats est disponible en externe.||  
  
#### <a name="plan-computer-certificates-for-ipsec-authentication"></a>Planifier des certificats d'ordinateur pour l'authentification IPsec  
Si vous utilisez l’authentification IPsec basée sur les certificats, le serveur et les clients d’accès à distance sont nécessaires pour obtenir un certificat d’ordinateur. La méthode la plus simple pour installer les certificats consiste à utiliser stratégie de groupe pour configurer l’inscription automatique pour les certificats d’ordinateur. Cela garantit que tous les membres du domaine obtiendront un certificat à partir d'une autorité de certification d'entreprise. Si vous n'avez pas d'entreprise définis dans votre organisation, consultez[Active Directory Certificate Services](https://technet.microsoft.com/library/cc770357.aspx).  
  
Ce certificat présente les exigences suivantes :  
  
-   Le certificat doit avoir une utilisation améliorée de la clé pour l’authentification du client.  
  
-   Le client et les certificats de serveur doivent être liés au même certificat racine. Ce certificat racine doit être sélectionné dans les paramètres de configuration DirectAccess.  
  
#### <a name="plan-certificates-for-ip-https"></a>Planifier des certificats pour IP-HTTPS  
Le serveur d'accès à distance joue le rôle d'écouteur IP-HTTPS, et vous devez installer manuellement un certificat de site web HTTPS sur le serveur. Tenez compte des éléments suivants quand vous planifiez des certificats :  
  
-   Il est recommandé d'utiliser une autorité de certification publique, afin que les listes de révocation de certificats soient rapidement disponibles.  
  
-   Dans le champ objet, spécifiez l’adresse IPv4 de la carte Internet du serveur d’accès à distance ou le nom de domaine complet (FQDN) de l’URL IP-HTTPs (adresse ConnectTo). Si le serveur d'accès à distance se trouve derrière un périphérique NAT, vous devez spécifier le nom public ou l'adresse du périphérique NAT.  
  
-   Le nom commun du certificat doit correspondre au nom du site IP-HTTPS.  
  
-   Pour le champ **utilisation améliorée** de la clé, utilisez l’identificateur d’objet (OID) d’authentification du serveur.  
  
-   Pour le champ **Points de distribution de la liste de révocation de certificats**, indiquez un point de distribution de liste de révocation de certificats accessible par les clients DirectAccess connectés à Internet  
  
    > [!NOTE]  
    > Cela est requis uniquement pour les clients exécutant Windows 7.  
  
-   Le certificat IP-HTTPS doit avoir une clé privée.  
  
-   Le certificat IP-HTTPS doit être importé directement dans le magasin personnel.  
  
-   Les certificats IP-HTTPS peuvent utiliser des caractères génériques dans le nom.  
  
#### <a name="plan-website-certificates-for-the-network-location-server"></a>Planifier des certificats de site web pour le serveur Emplacement réseau  
Tenez compte des points suivants lorsque vous planifiez le site Web du serveur emplacement réseau :  
  
-   Dans le champ **Objet**, spécifiez une adresse IP de l'interface intranet du serveur Emplacement réseau ou le nom de domaine complet de l'URL d'emplacement réseau.  
  
-   Pour le champ **utilisation améliorée** de la clé, utilisez l’OID d’authentification du serveur.  
  
-   Pour le champ **points de distribution de la liste de révocation de certificats** , utilisez un point de distribution de liste de révocation de certificats accessible par les clients DirectAccess connectés à l’intranet. Ce point de distribution de liste de révocation de certificats ne doit pas être accessible depuis l'extérieur du réseau interne.  
  
> [!NOTE]  
> Assurez-vous que les certificats pour IP-HTTPs et le serveur d’emplacement réseau ont un nom d’objet. Si le certificat utilise un autre nom, il n’est pas accepté par l’Assistant accès à distance.  
  
#### <a name="plan-dns-requirements"></a>Planifier la configuration DNS requise  
Cette section explique la configuration DNS requise pour les clients et les serveurs dans un déploiement de l’accès à distance.  
  
##### <a name="directaccess-client-requests"></a>Demandes des clients DirectAccess  
DNS sert à résoudre les demandes des ordinateurs clients DirectAccess qui ne se trouvent pas sur le réseau interne. Les clients DirectAccess essaient de se connecter au serveur d’emplacement réseau DirectAccess pour déterminer s’ils se trouvent sur Internet ou sur le réseau d’entreprise.  
  
-   Si la connexion réussit, les clients sont identifiés comme étant sur l’intranet, DirectAccess n’est pas utilisé et les demandes des clients sont résolues à l’aide du serveur DNS configuré sur la carte réseau de l’ordinateur client.  
  
-   Si la connexion échoue, les clients sont considérés comme étant sur Internet. Les clients DirectAccess utilisent la table de stratégie de résolution de noms (NRPT) pour déterminer quel serveur DNS utiliser pour résoudre les demandes de noms. Vous pouvez préciser que les clients doivent utiliser DirectAccess DNS64 pour résoudre les noms, ou un autre serveur DNS interne.  
  
Pendant la résolution des noms, la table NRPT est utilisée par les clients DirectAccess pour identifier la manière de traiter une demande. Les clients demandent un nom de domaine complet (FQDN) ou un nom en une partie, tel que <https://internal>. Si un nom en une partie est demandé, un suffixe DNS est ajouté pour créer un nom de domaine complet (FQDN). Si la requête DNS correspond à une entrée dans la table NRPT et que DNS4 ou qu’un serveur DNS intranet est spécifié pour l’entrée, la requête est envoyée pour la résolution de noms à l’aide du serveur spécifié. Si une correspondance existe mais qu’aucun serveur DNS n’est spécifié, une règle d’exemption et une résolution de noms normale sont appliquées.  
  
Quand un nouveau suffixe est ajouté à la table NRPT dans la console de gestion de l’accès à distance, les serveurs DNS par défaut pour le suffixe peuvent être détectés automatiquement en cliquant sur le bouton **détecter** . La détection automatique fonctionne comme suit :  
  
-   Si le réseau d’entreprise est basé sur IPv4 ou s’il utilise IPv4 et IPv6, l’adresse par défaut est l’adresse DNS64 de la carte interne sur le serveur d’accès à distance.  
  
-   Si le réseau d'entreprise est basé sur IPv6, l'adresse par défaut correspond à l'adresse IPv6 des serveurs DNS du réseau d'entreprise.  
  
##### <a name="infrastructure-servers"></a>Serveurs d'infrastructure  
  
-   **Serveur emplacement réseau**  
  
    Les clients DirectAccess essaient d'atteindre le serveur Emplacement réseau pour déterminer s'ils se situent sur le réseau interne. Les clients sur le réseau interne doivent être en mesure de résoudre le nom du serveur d’emplacement réseau, et ils ne doivent pas pouvoir résoudre le nom lorsqu’ils se trouvent sur Internet. Pour garantir cela, le nom de domaine complet (FQDN) du serveur Emplacement réseau est, par défaut, ajouté en tant que règle d'exemption à la table NRPT. De plus, quand vous configurez l'accès à distance, les règles suivantes sont automatiquement créées :  
  
    -   Une règle de suffixe DNS pour le domaine racine ou le nom de domaine du serveur d’accès à distance, et les adresses IPv6 qui correspondent aux serveurs DNS intranet configurés sur le serveur d’accès à distance. Par exemple, si le serveur d'accès à distance est membre du domaine corp.contoso.com, une règle est créée pour le suffixe DNS corp.contoso.com.  
  
    -   Règle d'exemption pour le nom de domaine complet du serveur d'emplacement réseau. Par exemple, si l’URL du serveur emplacement réseau est <https://nls.corp.contoso.com>, une règle d’exemption est créée pour le nom de domaine complet nls.corp.contoso.com.  
  
-   **Serveur IP-HTTPs**  
  
    Le serveur d’accès à distance joue le rôle d’écouteur IP-HTTPs et utilise son certificat de serveur pour s’authentifier auprès des clients IP-HTTPs. Le nom IP-HTTPs doit pouvoir être résolu par les clients DirectAccess qui utilisent des serveurs DNS publics.  
  
##### <a name="connectivity-verifiers"></a>Vérificateurs de connectivité  
L'accès à distance crée une sonde web par défaut utilisée par les ordinateurs clients DirectAccess pour vérifier la connectivité au réseau interne. Pour que la sonde fonctionne comme prévu, les noms suivants doivent être enregistrés manuellement dans DNS :  
  
-   **DirectAccess-webprobehost** doit correspondre à l’adresse IPv4 interne du serveur d’accès à distance, ou à l’adresse IPv6 dans un environnement IPv6 uniquement.  
  
-   **DirectAccess-corpconnectivityhost** doit correspondre à l’adresse de l’hôte local (bouclage). Vous devez créer des enregistrements A et AAAA. La valeur de l’enregistrement A est 127.0.0.1 et la valeur de l’enregistrement AAAA est construite à partir du préfixe NAT64 avec les 32 derniers bits comme 127.0.0.1. Vous pouvez récupérer le préfixe NAT64 en exécutant l’applet de commande Windows PowerShell **netnatTransitionConfiguration** .  
  
    > [!NOTE]  
    > Cela est valide uniquement dans les environnements IPv4 uniquement. Dans un environnement IPv4 plus IPv6 ou IPv6 uniquement, créez uniquement un enregistrement AAAA avec l’adresse IP de bouclage :: 1.  
  
Vous pouvez créer d’autres vérificateurs de connectivité en utilisant d’autres adresses Web via HTTP ou PING. Pour chaque vérificateur de connectivité, une entrée DNS doit exister.  
  
##### <a name="dns-server-requirements"></a>Configuration requise du serveur DNS  
  
-   Pour les clients DirectAccess, vous devez utiliser un serveur DNS exécutant Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 ou tout autre serveur DNS prenant en charge IPv6.  
  
-   Vous devez utiliser un serveur DNS qui prend en charge les mises à jour dynamiques. Vous pouvez utiliser des serveurs DNS qui ne prennent pas en charge les mises à jour dynamiques, mais les entrées doivent être mises à jour manuellement.  
  
-   Le nom de domaine complet de vos points de distribution de liste de révocation de certificats doit pouvoir être résolu à l’aide de serveurs DNS Internet. Par exemple, si l’URL <https://crl.contoso.com/crld/corp-DC1-CA.crl> se trouve dans le champ **points de distribution de liste de révocation de certificats** du certificat IP-HTTPS du serveur d’accès à distance, vous devez vous assurer que le nom de domaine complet crld.contoso.com peut être résolu à l’aide de serveurs DNS Internet.  
  
#### <a name="plan-for-local-name-resolution"></a>Planifier la résolution de noms locale  
Tenez compte des éléments suivants lorsque vous planifiez la résolution de noms locale :  
  
##### <a name="nrpt"></a>NRPT  
Vous devrez peut-être créer des règles de table de stratégie de résolution de noms supplémentaires dans les situations suivantes :  
  
-   Vous devez ajouter d’autres suffixes DNS pour votre espace de noms intranet.  
  
-   Si les noms de domaine complets de vos points de distribution de liste de révocation de certificats sont basés sur votre espace de noms intranet, vous devez ajouter des règles d’exemption pour les noms de domaine complets des points de distribution CRL.  
  
-   Si vous avez un environnement DNS split-brain, vous devez ajouter des règles d’exemption pour les noms des ressources pour lesquelles vous souhaitez que les clients DirectAccess qui se trouvent sur Internet accèdent à la version Internet, plutôt qu’à la version intranet.  
  
-   Si vous redirigez le trafic vers un site Web externe via vos serveurs proxy Web intranet, le site Web externe est disponible uniquement à partir de l’intranet. Il utilise les adresses de vos serveurs proxy Web pour autoriser les demandes entrantes. Dans ce cas, ajoutez une règle d’exemption pour le nom de domaine complet du site Web externe et spécifiez que la règle utilise votre serveur proxy Web intranet plutôt que les adresses IPv6 des serveurs DNS intranet.  
  
    Par exemple, supposons que vous testiez un site Web externe nommé test.contoso.com. Ce nom ne peut pas être résolu via les serveurs DNS Internet, mais le serveur proxy Web de contoso sait comment résoudre le nom et comment diriger les demandes pour le site Web vers le serveur Web externe. Pour empêcher que les utilisateurs qui ne trouvent pas sur l'intranet Contoso puissent accéder au site, le site web externe autorise uniquement les demandes provenant de l'adresse Internet IPv4 du proxy web Contoso. Ainsi, les utilisateurs de l’intranet peuvent accéder au site Web car ils utilisent le proxy Web Contoso, mais les utilisateurs DirectAccess ne peuvent pas le faire parce qu’ils n’utilisent pas le proxy Web contoso. En configurant une règle d'exemption NRPT pour test.contoso.com qui utilise le proxy web Contoso, les demandes de page web pour test.contoso.com sont routées vers le serveur proxy web intranet via le réseau Internet IPv4.  
  
##### <a name="single-label-names"></a>Noms en une partie  
Les noms en une partie, tels que <https://paycheck>, sont parfois utilisés pour les serveurs intranet. Si un nom d’étiquette unique est demandé et qu’une liste de recherche de suffixe DNS est configurée, les suffixes DNS de la liste sont ajoutés au nom d’étiquette unique. Par exemple, lorsqu’un utilisateur sur un ordinateur qui est membre des types de domaine corp.contoso.com <https://paycheck> dans le navigateur Web, le nom de domaine complet qui est construit comme nom est paycheck.corp.contoso.com. Par défaut, le suffixe ajouté est basé sur le suffixe DNS principal de l’ordinateur client.  
  
> [!NOTE]  
> Dans un scénario d’espace de noms disjoint (où un ou plusieurs ordinateurs de domaine ont un suffixe DNS qui ne correspond pas au domaine Active Directory auquel les ordinateurs sont membres), vous devez vous assurer que la liste de recherche est personnalisée pour inclure tous les suffixes requis. Par défaut, l’Assistant accès à distance configure le nom DNS Active Directory comme suffixe DNS principal sur le client. Assurez-vous d'ajouter le suffixe DNS utilisé par les clients pour la résolution de noms.  
  
Si plusieurs domaines et Windows Internet Name Service (WINS) sont déployés dans votre organisation et que vous vous connectez à distance, les noms uniques peuvent être résolus comme suit :  
  
-   En déployant une zone de recherche directe WINS dans le DNS. Lorsque vous tentez de résoudre computername.dns.zone1.corp.contoso.com, la demande est dirigée vers le serveur WINS qui utilise uniquement le nom de l’ordinateur. Le client pense qu’il émet une demande d’enregistrements A DNS standard, mais il s’agit en fait d’une demande NetBIOS.  
  
    Pour plus d’informations, consultez [gestion d’une zone de recherche directe](https://technet.microsoft.com/library/cc816891(WS.10).aspx).  
  
-   En ajoutant un suffixe DNS (par exemple, dns.zone1.corp.contoso.com) à l’objet de stratégie de groupe de domaine par défaut.  
  
##### <a name="split-brain-dns"></a>DNS « split brain »  
Le DNS split-brain fait référence à l’utilisation du même domaine DNS pour la résolution de noms Internet et intranet.  
  
Pour les déploiements DNS de fractionnement Brain, vous devez répertorier les noms de domaine complets dupliqués sur Internet et l’intranet, et décider quelles ressources le client DirectAccess doit atteindre : la version intranet ou Internet. Lorsque vous souhaitez que les clients DirectAccess atteignent la version Internet, vous devez ajouter le nom de domaine complet correspondant en tant que règle d’exemption à la table NRPT pour chaque ressource.  
  
Dans un environnement DNS split-brain, si vous souhaitez que les deux versions de la ressource soient disponibles, configurez vos ressources intranet avec des noms qui ne dupliquent pas les noms utilisés sur Internet. Ensuite, demandez à vos utilisateurs d’utiliser l’autre nom lorsqu’ils accèdent à la ressource sur l’intranet. Par exemple, configurez www\.internal.contoso.com pour le nom interne de www\.contoso.com.  
  
Dans un environnement DNS non « split brain », l'espace de noms Internet est différent de l'espace de noms intranet. Par exemple, la société Contoso Corporation utilise contoso.com sur Internet et corp.contoso.com sur l'intranet. Étant donné que toutes les ressources intranet utilisent le suffixe DNS corp.contoso.com, la règle NRPT pour corp.contoso.com dirige toutes les requêtes de nom DNS pour les ressources intranet vers les serveurs DNS intranet. Les requêtes DNS pour les noms avec le suffixe contoso.com ne correspondent pas à la règle d’espace de noms intranet corp.contoso.com dans la table NRPT, et elles sont envoyées aux serveurs DNS Internet. Avec un déploiement DNS non « split brain », étant donné qu'il n'y a aucune duplication de noms de domaines complets pour les ressources intranet et Internet, aucune configuration supplémentaire n'est requise pour la table NRPT. Les clients DirectAccess peuvent accéder aux ressources Internet et intranet de leur organisation.  
  
##### <a name="plan-local-name-resolution-behavior-for-directaccess-clients"></a>Planifier le comportement de résolution de noms locale pour les clients DirectAccess  
Si un nom ne peut pas être résolu avec DNS, le service client DNS dans Windows Server 2012, Windows 8, Windows Server 2008 R2 et Windows 7 peut utiliser la résolution de noms locale, avec les protocoles LLMNR (Link-local multicast Name Resolution) et NetBIOS sur TCP/IP, pour résoudre le  nom sur le sous-réseau local. La résolution de noms locale est généralement nécessaire pour la connectivité de réseau pair à pair lorsque l'ordinateur se trouve sur des réseaux privés, tels que des réseaux domestiques à sous-réseau unique.  
  
Lorsque le service client DNS exécute la résolution de noms locale pour les noms de serveur intranet et que l’ordinateur est connecté à un sous-réseau partagé sur Internet, les utilisateurs malveillants peuvent capturer LLMNR et NetBIOS sur des messages TCP/IP pour déterminer les noms de serveur intranet. Dans la page DNS de l’Assistant Installation du serveur d’infrastructure, vous pouvez configurer le comportement de résolution de noms locale en fonction des types de réponses reçus à partir des serveurs DNS intranet. Les options suivantes sont disponibles :  
  
-   **Utilisez la résolution de noms locale si le nom n’existe pas dans le DNS**: Cette option est la plus sécurisée car le client DirectAccess exécute uniquement la résolution de noms locale pour les noms de serveur qui ne peuvent pas être résolus par les serveurs DNS intranet. Si les serveurs DNS intranet sont accessibles, les noms des serveurs intranet sont résolus. S'ils ne sont pas accessibles ou que d'autres types d'erreurs DNS surviennent, les noms de serveur intranet ne seront pas révélés sur le sous-réseau via la résolution de noms locale.  
  
-   **Utilisez la résolution de noms locale si le nom n’existe pas dans DNS ou si les serveurs DNS sont inaccessibles lorsque l’ordinateur client se trouve sur un réseau privé (recommandé)** : Cette option est recommandée car elle autorise l'utilisation de la résolution de noms locale sur un réseau privé uniquement lorsque les serveurs DNS intranet sont inaccessibles.  
  
-   **Utilisez la résolution de noms locale pour tout type d’erreur de résolution DNS (le moins sécurisé)** : Il s'agit de l'option la moins sécurisée car les noms de serveurs réseau intranet peuvent être révélés au sous-réseau local via la résolution de noms locale.  
  
#### <a name="plan-the-network-location-server-configuration"></a>Planifier la configuration du serveur emplacement réseau  
Le serveur Emplacement réseau est un site web utilisé pour détecter si les clients DirectAccess se trouvent dans le réseau d'entreprise. Les clients du réseau d’entreprise n’utilisent pas DirectAccess pour atteindre les ressources internes ; mais au lieu de cela, ils se connectent directement.  
  
Le site Web du serveur emplacement réseau peut être hébergé sur le serveur d’accès à distance ou sur un autre serveur de votre organisation. Si vous hébergez le serveur emplacement réseau sur le serveur d’accès à distance, le site Web est créé automatiquement lorsque vous déployez l’accès à distance. Si vous hébergez le serveur emplacement réseau sur un autre serveur exécutant un système d’exploitation Windows, vous devez vous assurer que Internet Information Services (IIS) est installé sur ce serveur et que le site Web est créé. L’accès à distance ne configure pas les paramètres sur le serveur emplacement réseau.  
  
Assurez-vous que le site Web du serveur emplacement réseau respecte les exigences suivantes :  
  
-   A un certificat de serveur HTTPs.  
  
-   Offre une haute disponibilité aux ordinateurs du réseau interne.  
  
-   N’est pas accessible aux ordinateurs clients DirectAccess sur Internet.  
  
-  
  
En outre, tenez compte des conditions suivantes pour les clients lors de la configuration de votre site Web de serveur emplacement réseau :  
  
-   Les ordinateurs clients DirectAccess doivent faire confiance à l'autorité de certification qui a émis le certificat de serveur pour le site web du serveur Emplacement réseau.  
  
-   Les ordinateurs clients DirectAccess figurant sur le réseau interne doivent être en mesure de résoudre le nom du site du serveur Emplacement réseau.  
  
##### <a name="plan-certificates-for-the-network-location-server"></a>Planifier des certificats pour le serveur emplacement réseau  
Lorsque vous obtenez le certificat de site Web à utiliser pour le serveur emplacement réseau, prenez en compte les éléments suivants :  
  
-   Dans le champ **Objet**, indiquez l'adresse IP de l'interface intranet du serveur Emplacement réseau ou le nom de domaine complet de l'URL d'emplacement réseau.  
  
-   Pour le champ **utilisation améliorée** de la clé, utilisez l’OID d’authentification du serveur.  
  
-   Le certificat du serveur emplacement réseau doit être vérifié par rapport à une liste de révocation de certificats (CRL). Pour le champ **points de distribution de la liste de révocation de certificats** , utilisez un point de distribution de liste de révocation de certificats accessible par les clients DirectAccess connectés à l’intranet. Ce point de distribution de liste de révocation de certificats ne doit pas être accessible depuis l'extérieur du réseau interne.  
  
##### <a name="plan-dns-for-the-network-location-server"></a>Planifier le DNS pour le serveur emplacement réseau  
Les clients DirectAccess essaient d'atteindre le serveur Emplacement réseau pour déterminer s'ils se situent sur le réseau interne. Les clients qui se trouvent sur le réseau interne doivent être en mesure de résoudre le nom du serveur d'emplacement réseau, mais il est impératif de les empêcher de le faire quand ils se situent sur Internet. Pour le garantir, le nom de domaine complet (FQDN) du serveur d'emplacement réseau est, par défaut, ajouté en tant que règle d'exemption à la NRPT.  
  
### <a name="plan-management-servers-configuration"></a>Planifier la configuration des serveurs d’administration  
Les clients DirectAccess initient une communication avec les serveurs d’administration qui fournissent des services tels que les Windows Update et les mises à jour antivirus. Les clients DirectAccess utilisent également le protocole Kerberos pour s’authentifier auprès des contrôleurs de domaine avant d’accéder au réseau interne. Au cours de l'administration distante des clients DirectAccess, les serveurs d'administration communiquent avec les ordinateurs clients pour exécuter des fonctions d'administration telles que l'évaluation de l'inventaire logiciel et matériel. L'accès à distance peut détecter automatiquement certains serveurs d'administration, dont notamment :  
  
-   Contrôleurs de domaine : La détection automatique des contrôleurs de domaine est effectuée pour les domaines qui contiennent des ordinateurs clients et pour tous les domaines de la même forêt que le serveur d’accès à distance.  
  
-   Serveurs System Center Configuration Manager  
  
Les contrôleurs de domaine et les serveurs de System Center Configuration Manager sont détectés automatiquement lors de la première configuration de DirectAccess. Les contrôleurs de domaine détectés ne sont pas affichés dans la console, mais les paramètres peuvent être récupérés à l’aide des applets de commande Windows PowerShell. Si le contrôleur de domaine ou les serveurs de System Center Configuration Manager sont modifiés, cliquez sur **Update Management serveurs** dans la console pour actualiser la liste des serveurs d’administration.  
  
**Configuration requise du serveur d’administration**  
  
-   Les serveurs d’administration doivent être accessibles via le tunnel d’infrastructure. Lorsque vous configurez l'accès à distance, l'ajout de serveurs dans la liste des serveurs d'administration les rend accessibles via ce tunnel.  
  
-   Les serveurs d’administration qui établissent des connexions aux clients DirectAccess doivent prendre entièrement en charge IPv6, au moyen d’une adresse IPv6 native ou à l’aide d’une adresse attribuée par ISATAP.  
  
### <a name="plan-active-directory-requirements"></a>Planifier les exigences de Active Directory  
L’accès à distance utilise Active Directory comme suit :  
  
-   **Authentification**: Le tunnel d’infrastructure utilise l’authentification NTLMv2 pour le compte d’ordinateur qui se connecte au serveur d’accès à distance, et le compte doit se trouver dans un domaine Active Directory. Le tunnel intranet utilise l’authentification Kerberos pour que l’utilisateur crée le tunnel intranet.  
  
-   **Objets stratégie de groupe**: L’accès à distance rassemble les paramètres de configuration dans des objets stratégie de groupe (GPO), qui sont appliqués aux serveurs d’accès à distance, aux clients et aux serveurs d’applications internes.  
  
-   **Groupes de sécurité** : L’accès à distance utilise des groupes de sécurité pour rassembler et identifier des ordinateurs clients DirectAccess. Les objets de stratégie de groupe sont appliqués aux groupes de sécurité requis.  
  
Lorsque vous planifiez un environnement de Active Directory pour un déploiement de l’accès à distance, tenez compte des conditions suivantes :  
  
-   Au moins un contrôleur de domaine est installé sur le système d’exploitation Windows Server 2012, Windows Server 2008 R2 Windows Server 2008 ou Windows Server 2003.  
  
    Si le contrôleur de domaine se trouve sur un réseau de périmètre (et par conséquent accessible à partir de la carte réseau Internet du serveur d’accès à distance), empêchez le serveur d’accès à distance de l’atteindre. Vous devez ajouter des filtres de paquets sur le contrôleur de domaine pour empêcher la connectivité à l’adresse IP de la carte Internet.  
  
-   Le serveur d’accès à distance doit être membre d’un domaine.  
  
-   Les clients DirectAccess doivent appartenir au domaine. Les clients peuvent appartenir :  
  
    -   à tout domaine inclus dans la même forêt que le serveur d'accès à distance ;  
  
    -   à tout domaine doté d'une approbation bidirectionnelle avec le domaine du serveur d'accès à distance ;  
  
    -   N’importe quel domaine d’une forêt ayant une relation d’approbation bidirectionnelle avec la forêt du domaine du serveur d’accès à distance.  
  
> [!NOTE]  
> -   Le serveur d'accès à distance ne peut pas être un contrôleur de domaine.  
> -   Le contrôleur de domaine Active Directory utilisé pour l’accès à distance ne doit pas être accessible à partir de la carte Internet externe du serveur d’accès à distance (la carte ne doit pas figurer dans le profil de domaine du pare-feu Windows).  
  
#### <a name="plan-client-authentication"></a>Planifier l'authentification client  
Dans accès à distance dans Windows Server 2012, vous pouvez choisir entre l’utilisation de l’authentification Kerberos intégrée, qui utilise des noms d’utilisateur et des mots de passe, ou l’utilisation de certificats pour l’authentification de l’ordinateur IPsec.  
  
**Authentification Kerberos**: Quand vous choisissez d’utiliser des informations d’identification Active Directory pour l’authentification, DirectAccess utilise d’abord l’authentification Kerberos pour l’ordinateur, puis il utilise l’authentification Kerberos pour l’utilisateur. Quand vous utilisez ce mode d’authentification, DirectAccess utilise un tunnel de sécurité unique qui fournit l’accès au serveur DNS, au contrôleur de domaine et à tout autre serveur sur le réseau interne.  
  
**Authentification IPSec**: Lorsque vous choisissez d’utiliser l’authentification à deux facteurs ou la protection d’accès réseau, DirectAccess utilise deux tunnels de sécurité. L’Assistant Configuration de l’accès à distance configure les règles de sécurité de connexion dans le pare-feu Windows avec fonctions avancées de sécurité. Ces règles spécifient les informations d’identification suivantes lors de la négociation de la sécurité IPsec sur le serveur d’accès à distance :  
  
-   Le tunnel d’infrastructure utilise les informations d’identification du certificat d’ordinateur pour la première authentification et les informations d’identification de l’utilisateur (NTLMv2) pour la deuxième authentification. Les informations d’identification de l’utilisateur forcent l’utilisation de protocole Authenticated IP (Authenticated Internet Protocol) (AuthIP) et fournissent l’accès à un serveur DNS et un contrôleur de domaine avant que le client DirectAccess puisse utiliser les informations d’identification Kerberos pour le tunnel intranet.  
  
-   Le tunnel intranet utilise les informations d’identification du certificat d’ordinateur pour la première authentification et les informations d’identification de l’utilisateur (Kerberos V5) pour la deuxième authentification.  
  
#### <a name="plan-multiple-domains"></a>Planifier plusieurs domaines  
La liste des serveurs d'administration doit inclure les contrôleurs de domaine de tous les domaines qui contiennent des groupes de sécurité qui incluent des ordinateurs clients DirectAccess. Il doit contenir tous les domaines qui contiennent des comptes d’utilisateur qui peuvent utiliser des ordinateurs configurés en tant que clients DirectAccess. Cela garantit que des utilisateurs qui ne se trouvent pas dans le même domaine que l'ordinateur client qu'ils utilisent seront authentifiés auprès d'un contrôleur de domaine dans le domaine de l'utilisateur.  
  
Cette authentification est automatique si les domaines se trouvent dans la même forêt. S’il existe un groupe de sécurité avec des ordinateurs clients ou des serveurs d’applications qui se trouvent dans des forêts différentes, les contrôleurs de domaine de ces forêts ne sont pas détectés automatiquement. Les forêts ne sont pas non plus détectées automatiquement. Vous pouvez exécuter la tâche **Update Management serveurs** dans la **gestion** de l’accès à distance pour détecter ces contrôleurs de domaine.  
  
Dans la mesure du possible, les suffixes de nom de domaine communs doivent être ajoutés à la table NRPT pendant le déploiement de l’accès à distance. Par exemple, si vous possédez deux domaines, domain1.corp.contoso.com et domain2.corp.contoso.com, au lieu d'ajouter deux entrées dans la table NRPT, vous pouvez ajouter une entrée de suffixe DNS commun, où le suffixe de nom de domaine est corp.contoso.com. Cela se produit automatiquement pour les domaines situés dans la même racine. Les domaines qui ne se trouvent pas dans la même racine doivent être ajoutés manuellement.  
  
### <a name="plan-group-policy-object-creation"></a>Planifier la création d’objets stratégie de groupe  
Lorsque vous configurez l’accès à distance, les paramètres DirectAccess sont collectés dans des objets stratégie de groupe (GPO). Deux objets de stratégie de groupe sont remplis avec les paramètres DirectAccess et sont distribués comme suit :  
  
-   **Objet de stratégie de groupe du client DirectAccess**: Cet objet de stratégie de groupe contient les paramètres client, y compris les paramètres de technologie de transition IPv6, les entrées NRPT et les règles de sécurité de connexion pour le pare-feu Windows avec fonctions avancées de sécurité. Il est appliqué aux groupes de sécurité spécifiés pour les ordinateurs clients.  
  
-   **Objet de stratégie de groupe du serveur DirectAccess**: Cet objet de stratégie de groupe contient les paramètres de configuration DirectAccess qui sont appliqués à tout serveur que vous avez configuré en tant que serveur d’accès à distance dans votre déploiement. Il contient également des règles de sécurité de connexion pour le pare-feu Windows avec fonctions avancées de sécurité.  
  
> [!NOTE]  
> La configuration des serveurs d’applications n’est pas prise en charge dans la gestion à distance des clients DirectAccess, car les clients ne peuvent pas accéder au réseau interne du serveur DirectAccess sur lequel résident les serveurs d’applications. L’étape 4 de l’écran de configuration de l’installation de l’accès à distance n’est pas disponible pour ce type de configuration.  
  
Vous pouvez configurer les objets de stratégie de groupe automatiquement ou manuellement.  
  
**Automatiquement**: Lorsque vous spécifiez que les objets de stratégie de groupe sont créés automatiquement, un nom par défaut est spécifié pour chaque objet de stratégie de groupe.  
  
**Manuellement**: Vous pouvez utiliser les objets de stratégie de groupe qui ont été prédéfinis par l’administrateur Active Directory.  
  
Quand vous configurez vos objets de stratégie de groupe, tenez compte des avertissements suivants :  
  
-   Une fois que DirectAccess a été configuré pour utiliser des objets de stratégie de groupe spécifiques, il ne peut plus être configuré pour en utiliser d'autres.  
  
-   Utilisez la procédure suivante pour sauvegarder tous les objets d’accès à distance stratégie de groupe avant d’exécuter des applets de commande DirectAccess :  
  
    [Sauvegarder et restaurer la configuration d’Accès à distance](https://go.microsoft.com/fwlink/?LinkID=257928).  
  
-   Que vous utilisiez des objets de stratégie de groupe configurés automatiquement ou manuellement, vous devez ajouter une stratégie pour la détection des liaisons lentes si vos clients utilisent 3G. Chemin d’accès pour **Policy : Configurez stratégie de groupe détection de liaison lente @ no__t-0 :  
  
    **Computer configuration/Policies/Administrative Templates/System/Group Policy**.  
  
-   Si les autorisations appropriées pour la liaison des objets de stratégie de groupe n’existent pas, un avertissement est émis. L’opération d’accès à distance se poursuivra, mais la liaison ne se produira pas. Si cet avertissement est émis, les liens ne sont pas créés automatiquement, même si les autorisations sont ajoutées ultérieurement. L'administrateur doit créer les liens manuellement.  
  
#### <a name="automatically-created-gpos"></a>Objets de stratégie de groupe créés automatiquement  
Tenez compte des éléments suivants lorsque vous utilisez des objets de stratégie de groupe créés automatiquement :  
  
Les objets de stratégie de groupe créés automatiquement sont appliqués en fonction de l’emplacement et de la cible du lien, comme suit :  
  
-   Pour l’objet de stratégie de groupe du serveur DirectAccess, l’emplacement et la cible du lien pointent vers le domaine qui contient le serveur d’accès à distance.  
  
-   Lorsque des objets de stratégie de groupe de serveur d’applications et de client sont créés, l’emplacement est défini sur un domaine unique. Le nom de l’objet de stratégie de groupe est recherché dans chaque domaine, et le domaine est rempli avec les paramètres DirectAccess s’il existe.  
  
-   La cible du lien est définie à la racine du domaine dans lequel l'objet de stratégie de groupe a été créé. Un objet de stratégie de groupe est créé pour chaque domaine qui contient des ordinateurs clients ou des serveurs d'applications, et il est lié à la racine de son domaine respectif.  
  
Lorsque vous utilisez des objets de stratégie de groupe créés automatiquement pour appliquer les paramètres DirectAccess, l’administrateur du serveur d’accès à distance requiert les autorisations suivantes :  
  
-   Autorisations de créer des objets de stratégie de groupe pour chaque domaine.  
  
-   Autorisations à lier à toutes les racines de domaine client sélectionnées.  
  
-   Autorisations à lier aux racines de domaine de l’objet de stratégie de groupe du serveur.  
  
-   Autorisations de sécurité pour créer, modifier, supprimer et modifier les objets de stratégie de groupe.  
  
-   Autorisations de lecture de l’objet de stratégie de groupe pour chaque domaine requis. Cette autorisation n’est pas obligatoire, mais elle est recommandée car elle permet l’accès à distance pour vérifier que les objets de stratégie de groupe avec des noms dupliqués n’existent pas lors de la création des objets de stratégie de groupe.  
  
#### <a name="manually-created-gpos"></a>Objets de stratégie de groupe créés manuellement  
Prenez en compte les points suivants quand vous utilisez des objets de stratégie de groupe créés manuellement :  
  
-   Les objets de stratégie de groupe doivent déjà exister quand vous exécutez l'Assistant Configuration de l'accès à distance.  
  
-   Pour appliquer les paramètres DirectAccess, l’administrateur du serveur d’accès à distance requiert des autorisations de sécurité complètes pour créer, modifier, supprimer et modifier les objets de stratégie de groupe créés manuellement.  
  
-   Une recherche est effectuée pour un lien vers l’objet de stratégie de groupe dans l’ensemble du domaine. Si l'objet de stratégie de groupe n'est pas lié dans le domaine, un lien est automatiquement créé à la racine du domaine. Si les autorisations requises pour créer le lien ne sont pas disponibles, un avertissement est émis.  
  
#### <a name="recovering-from-a-deleted-gpo"></a>Récupération après la suppression d'un objet de stratégie de groupe  
Si un objet de stratégie de groupe sur un serveur d’accès à distance, un client ou un serveur d’applications a été supprimé par accident, le message d’erreur suivant s’affiche : **Objet de stratégie de groupe (nom de l’objet de stratégie de groupe) introuvable**.  
  
Si vous disposez d'une sauvegarde, vous pouvez restaurer l'objet de stratégie de groupe. Si aucune sauvegarde n’est disponible, vous devez supprimer les paramètres de configuration et les reconfigurer.  
  
###### <a name="to-remove-configuration-settings"></a>Pour supprimer des paramètres de configuration  
  
1.  Exécutez l’applet de commande Windows PowerShell **Uninstall-RemoteAccess**.  
  
2.  Ouvrez **gestion de l’accès à distance**.  
  
3.  Un message d'erreur indique que l'objet de stratégie de groupe est introuvable. Cliquez sur **Supprimer les paramètres de configuration**. Une fois l’opération terminée, le serveur est restauré à un État non configuré et vous pouvez reconfigurer les paramètres.  
  


