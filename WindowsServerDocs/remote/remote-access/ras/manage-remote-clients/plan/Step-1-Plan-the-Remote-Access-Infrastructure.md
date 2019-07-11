---
title: Étape 1 planifier l’Infrastructure d’accès à distance
description: Cette rubrique fait partie du guide les Clients DirectAccess de gérer à distance dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a1ce7af5-f3fe-4fc9-82e8-926800e37bc1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f3b1837145dee5767741052c548a4b44da56659b
ms.sourcegitcommit: be243a92f09048ca80f85d71555ea6ee3751d712
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67792330"
---
# <a name="step-1-plan-the-remote-access-infrastructure"></a>Étape 1 planifier l’Infrastructure d’accès à distance

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

> [!NOTE]
> Windows Server 2016 combine DirectAccess, routage et accès distant (RRAS) en un seul rôle accès à distance.  
  
Cette rubrique décrit les étapes de planification d’une infrastructure que vous pouvez utiliser pour configurer un serveur d’accès à distance unique pour la gestion à distance des clients DirectAccess. Le tableau suivant répertorie les étapes, mais ces tâches de planification n’êtes pas obligé de le faire dans un ordre spécifique.  
  
|Tâche|Description|  
|----|--------|  
|[Planifier les paramètres de serveur et de la topologie réseau](#plan-network-topology-and-settings)|Décidez où placer le serveur d’accès à distance (à la périphérie ou derrière un pare-feu ou un périphérique de traduction d’adresses réseau (NAT)) et planifiez l’adressage IP et le routage.|  
|[Planifier la configuration requise du pare-feu](#plan-firewall-requirements)|Planifiez l'autorisation de l'accès à distance via des pare-feu de périmètre.|  
|[Planifier la configuration requise des certificats](#plan-certificate-requirements)|Décidez si vous utilisez le protocole Kerberos ou des certificats pour l’authentification du client et planifier des certificats de votre site Web.<br/><br/>IP-HTTPS est un protocole de transition utilisé par les clients DirectAccess pour transférer le trafic IPv6 via des réseaux IPv4. Décider s’il faut authentifier IP-HTTPS pour le serveur à l’aide d’un certificat émis par une autorité de certification (CA), ou à l’aide d’un certificat auto-signé émis automatiquement par le serveur d’accès à distance.|  
|[Planifier la configuration DNS requise](#plan-dns-requirements)|Planifier les paramètres de système DNS (Domain Name) pour le serveur accès à distance, les serveurs d’infrastructure, les options de résolution de nom local et les connectivité client.| 
|[Planifier la configuration du serveur emplacement réseau](#plan-the-network-location-server-configuration)|Décidez où placer le site de serveur d’emplacement réseau dans votre organisation (sur le serveur d’accès à distance ou un autre serveur), et la planification de la configuration requise des certificats si le serveur emplacement réseau se trouvera sur le serveur d’accès à distance. **Remarque :** Les clients DirectAccess utilisent le serveur Emplacement réseau pour déterminer s'ils se trouvent sur le réseau interne.|  
|[Planifier les configurations de serveurs d’administration](#plan-management-servers-configuration)|Planifiez les serveurs d’administration (tels que les serveurs de mise à jour) utilisés lors de l’administration à distance des clients. **Remarque :** Les administrateurs peuvent administrer à distance les ordinateurs clients DirectAccess situés en dehors du réseau d’entreprise à l’aide d’Internet.|  
|[Planifier la configuration requise d’Active Directory](#plan-active-directory-requirements)|Planifiez vos contrôleurs de domaine, vos exigences d’Active Directory, l’authentification du client et plusieurs structure de domaine.|  
|[Planifier la création d’objets de stratégie de groupe](#plan-group-policy-object-creation)|Décidez quels objets stratégie de groupe sont requis dans votre organisation et comment créer et modifier les objets de stratégie de groupe.|  
  
## <a name="plan-network-topology-and-settings"></a>Planifier la topologie et les paramètres réseau  
Lorsque vous planifiez votre réseau, vous devez prendre en compte la topologie de carte réseau, les paramètres pour l’adressage IP et la configuration requise pour ISATAP.  
  
### <a name="plan-network-adapters-and-ip-addressing"></a>Planifier les cartes réseau et l'adressage IP  
  
1.  Identifiez la topologie de carte réseau que vous souhaitez utiliser. Accès à distance peut être configuré avec une des topologies suivantes :  
  
    -   Avec deux cartes réseau : Le serveur d’accès à distance est installé à la périphérie avec une carte réseau connectée à Internet et l’autre au réseau interne.  
  
    -   Avec deux cartes réseau : Le serveur d’accès à distance est installé derrière un périphérique NAT, un pare-feu ou un routeur, avec une carte réseau connectée à un réseau de périmètre et l’autre au réseau interne.  
  
    -   Avec une carte réseau : Le serveur d’accès à distance est installé derrière un périphérique NAT et l’unique carte réseau est connectée au réseau interne.  
  
2.  Identifiez vos exigences en matière d'adressage IP :  
  
    DirectAccess utilise IPv6 avec IPsec pour créer une connexion sécurisée entre les ordinateurs clients DirectAccess et le réseau d'entreprise interne. Pourtant, DirectAccess ne requiert pas systématiquement une connectivité Internet IPv6 ni une prise en charge IPv6 native sur les réseaux internes. Au lieu de cela, il configure automatiquement et utilise les technologies de transition IPv6 pour transférer le trafic IPv6 sur Internet IPv4 (6to4, Teredo ou IP-HTTPS) et sur votre intranet IPv4 uniquement (NAT64 ou ISATAP). Pour obtenir une vue d'ensemble de ces technologies de transition, consultez les ressources suivantes :  
  
    -   [Technologies de Transition IPv6](https://technet.microsoft.com/library/bb726951.aspx)  
  
    -   [Spécification du protocole de Tunneling IP-HTTPS](https://msdn.microsoft.com/library/dd358571.aspx)  
  
3.  Configurez les cartes et l'adressage requis selon le tableau suivant. Pour les déploiements qui sont trouvent derrière un périphérique NAT à l’aide d’une seule carte réseau, configurez vos adresses IP en utilisant uniquement le **carte réseau interne** colonne.  
  
    ||Carte réseau externe|Carte réseau interne<sup>1, ci-dessus</sup>|Exigences en matière de routage|  
    |-|--------------|------------------------|------------|  
    |Internet IPv4 et intranet IPv4|Configurez ce qui suit :<br/><br/>-Adresses IPv4 publiques consécutives de static deux avec les masques de sous-réseau appropriés (requis pour Teredo uniquement).<br/>-Une passerelle par défaut une adresse IPv4 pour votre pare-feu Internet ou du routeur de fournisseur de services Internet local. **Remarque :** Le serveur d’accès à distance nécessite deux adresses IPv4 publiques consécutives afin qu’il peut agir comme un serveur Teredo et les clients Teredo Windows peuvent utiliser le serveur d’accès à distance pour détecter le type de périphérique NAT.|Configurez ce qui suit :<br/><br/>-Adresse intranet IPv4 avec le masque de sous-réseau approprié.<br/>-Un suffixe DNS spécifique à la connexion pour votre espace de noms intranet. Vous devez également configurer un serveur DNS sur l'interface interne. **Attention :** Ne configurez pas de passerelle par défaut sur toutes les interfaces intranet.|Pour configurer le serveur d’accès à distance pour atteindre tous les sous-réseaux du réseau IPv4 interne, procédez comme suit :<br/><br/>-Répertorie les espaces d’adressage IPv4 pour tous les emplacements sur votre intranet.<br/>-Utilisez le `route add -p` ou `netsh interface ipv4 add route` espaces d’adressage de commandes pour ajouter l’IPv4 en tant qu’itinéraires statiques dans la table de routage IPv4 du serveur d’accès à distance.|  
    |Internet IPv6 et intranet IPv6|Configurez ce qui suit :<br/><br/>-Utilisez la configuration d’adresse automatique fournie par votre fournisseur de services Internet.<br/>-Utilisez le `route print` commande pour vous assurer qu’un itinéraire IPv6 par défaut qui pointe vers le routeur ISP existe dans la table de routage IPv6.<br/>-Déterminer si les fournisseur de services Internet et les routeurs intranet utilisent les préférences de routeur par défaut comme décrit dans RFC 4191 et s’ils sont utiliser une préférence par défaut plus élevée que vos routeurs intranet locaux. Si vous avez répondu par l'affirmative dans les deux cas, aucune autre configuration n'est requise pour l'itinéraire par défaut. Une préférence plus élevée pour le routeur ISP permet de s'assurer que l'itinéraire IPv6 par défaut actif du serveur d'accès à distance pointe vers Internet IPv6.<br/><br/>Étant donné que le serveur d'accès à distance est un routeur IPv6, si vous disposez d'une infrastructure IPv6 native, l'interface Internet peut également atteindre les contrôleurs de domaine sur l'intranet. Dans ce cas, ajoutez des filtres de paquets au contrôleur de domaine dans le réseau de périmètre pour empêcher la connectivité à l’adresse IPv6 de l’interface Internet du serveur d’accès à distance.|Configurez ce qui suit :<br/><br/>Si vous n’utilisez pas les niveaux de préférence par défaut, configurez vos interfaces intranet à l’aide de la `netsh interface ipv6 set InterfaceIndex ignoredefaultroutes=enabled` commande. Cette commande vérifie que les itinéraires par défaut supplémentaires qui pointent vers les routeurs intranet ne seront pas ajoutés à la table de routage IPv6. Vous pouvez obtenir l’information InterfaceIndex de vos interfaces intranet à partir de l’affichage de la `netsh interface show interface` commande.|Si vous avez un intranet IPv6, procédez comme suit pour configurer le serveur d'accès à distance afin d'atteindre tous les emplacements IPv6 :<br/><br/>-Répertorie les espaces d’adressage IPv6 pour tous les emplacements sur votre intranet.<br/>-Utilisez le `netsh interface ipv6 add route` commande pour ajouter les espaces d’adressage IPv6 en tant qu’itinéraires statiques dans la table de routage IPv6 du serveur d’accès à distance.|  
    |Internet IPv4 et intranet IPv6|Le serveur d’accès à distance transfère acheminer le trafic IPv6 de valeur par défaut à l’aide de l’interface de carte Microsoft 6to4 à un relais 6to4 sur Internet IPv4. Lorsque le protocole IPv6 natif n’est pas déployé dans le réseau d’entreprise, vous pouvez utiliser la commande suivante pour configurer un serveur d’accès à distance pour l’adresse IPv4 du relais Microsoft 6to4 sur Internet IPv4 : `netsh interface ipv6 6to4 set relay name=<ipaddress> state=enabled`.|||  
  
    > [!NOTE]  
    > -   Si une adresse IPv4 publique a été attribuée au client DirectAccess, il utilise la technologie de relais 6to4 pour se connecter à l’intranet. Si le client est attribué à une adresse IPv4 privée, il utilisera Teredo. Si le client DirectAccess ne peut pas se connecter au serveur DirectAccess avec 6to4 ou Teredo, il utilisera IP-HTTPS.  
    > -   Pour utiliser Teredo, vous devez configurer deux adresses IP consécutives sur la carte réseau tournée vers l'extérieur.  
    > -   Vous ne pouvez pas utiliser Teredo si le serveur d’accès à distance n'a qu’une seule carte réseau.  
    > -   Les ordinateurs clients IPv6 natif se connectent au serveur d'accès à distance via IPv6 natif et aucune technologie de transition n'est requise.  
  
### <a name="plan-isatap-requirements"></a>Planifier la configuration requise ISATAP  
ISATAP est requis pour la gestion à distance de DirectAccessclients, afin que les serveurs d’administration DirectAccess peuvent se connecter aux clients DirectAccess situés sur Internet. ISATAP n’est pas nécessaire pour prendre en charge les connexions initiées par les ordinateurs clients DirectAccess aux ressources IPv4 sur le réseau d’entreprise. NAT64/DNS64 est utilisé à ces fins. Si votre déploiement requiert ISATAP, utilisez le tableau suivant pour identifier vos besoins.  
  
|Scénario de déploiement ISATAP|Configuration requise|  
|---------------|--------|  
|Existant intranet IPv6 natif (aucune ISATAP n’est requise)|Avec une infrastructure IPv6 native existante, vous spécifiez le préfixe de l’organisation au cours du déploiement de l’accès à distance, et le serveur d’accès à distance ne configure pas lui-même en tant que routeur ISATAP. Effectuez ce qui suit :<br/><br/>1.  Pour vous assurer que les clients DirectAccess sont accessibles depuis l’intranet, vous devez modifier votre routage IPv6 afin que la valeur par défaut acheminer le trafic est transféré au serveur d’accès à distance. Si votre espace d’adressage IPv6 intranet utilise une adresse autre que celle d’un préfixe d’adresse IPv6 48 bits unique, vous devez spécifier le préfixe IPv6 organisation pertinentes au cours du déploiement.<br/>2.  Si vous êtes actuellement connecté à Internet IPv6, vous devez configurer votre trafic d’itinéraire par défaut afin qu’il soit envoyé au serveur d’accès à distance et puis configurez les connexions appropriées et les itinéraires sur le serveur d’accès à distance, afin que l’itinéraire par défaut le trafic est transféré à l’appareil est connecté à Internet IPv6.|  
|Déploiement ISATAP existant|Si vous avez une infrastructure ISATAP existante, au cours du déploiement, vous êtes invité au préfixe de 48 bits de l’organisation, et le serveur d’accès à distance ne configure pas lui-même en tant que routeur ISATAP. Pour vous assurer que les clients DirectAccess sont accessibles depuis l’intranet, vous devez modifier votre infrastructure de routage IPv6 afin que la valeur par défaut acheminer le trafic est transféré au serveur d’accès à distance. Cette modification doit être effectuée sur le routeur ISATAP existant à laquelle les clients intranet doivent être déjà transfère le trafic par défaut.|  
|Aucune connectivité IPv6 existante|Lorsque l’Assistant Installation de l’accès à distance détecte que le serveur ne dispose d’aucune connectivité IPv6 native ou basée sur ISATAP, elle dérive d’un préfixe de 48 bits 6to4 pour l’intranet et configure automatiquement le serveur d’accès à distance comme routeur ISATAP pour fournir de IPv6 connectivité aux hôtes ISATAP sur votre intranet. (Un préfixe 6to4 est utilisé uniquement si le serveur dispose des adresses publiques, sinon le préfixe est généré automatiquement à partir d’une plage d’adresse locale unique).<br/><br/>Pour utiliser ISATAP les opérations suivantes :<br/><br/>1.  Inscrire le nom ISATAP sur un serveur DNS pour chaque domaine sur lequel vous souhaitez activer la connectivité basée sur ISATAP, afin que le nom ISATAP ne peut être résolu par le serveur DNS interne à l’adresse IPv4 interne du serveur d’accès à distance.<br/>2.  Par défaut, les serveurs DNS exécutant Windows Server 2012, Windows Server 2008 R2, Windows Server 2008 ou Windows Server 2003 bloquent la résolution du nom ISATAP à l’aide de la liste rouge de requêtes globale. Pour activer ISATAP, vous devez supprimer le nom ISATAP de la liste rouge. Pour plus d'informations, consultez [Supprimer ISATAP de la liste rouge de requêtes globale DNS](https://go.microsoft.com/fwlink/p/?LinkId=168593).<br/><br/>Windows-les hôtes ISATAP qui peuvent résoudre le nom ISATAP automatiquement configurent une adresse avec le serveur d’accès à distance comme suit :<br/><br/>1.  Une adresse IPv6 basée sur ISATAP sur une interface de tunneling ISATAP<br/>2.  Un itinéraire 64 bits qui fournit la connectivité aux hôtes ISATAP sur l’intranet<br/>3.  Un itinéraire IPv6 par défaut qui pointe vers le serveur d’accès à distance. L’itinéraire par défaut garantit que les hôtes ISATAP intranet peuvent atteindre les clients DirectAccess<br/><br/>Lorsque vos hôtes ISATAP Windows obtiennent une adresse IPv6 basée sur ISATAP, ils commencent à utiliser de trafic encapsulé par ISATAP pour communiquer si la destination est également un hôte ISATAP. Étant donné que ISATAP utilise un seul sous-réseau 64 bits pour l’ensemble de votre intranet, votre communication passe à partir d’un modèle IPv4 segmenté de communication, à un modèle de communication de sous-réseau unique avec IPv6. Cela peut affecter le comportement de certains Services de domaine Active Directory (AD DS) et les applications qui reposent sur votre configuration de Services et Sites Active Directory. Par exemple, si vous avez utilisé les Sites Active Directory et le composant logiciel enfichable Services pour configurer des sites, les sous-réseaux IPv4 et les transports inter-sites pour transférer des requêtes pour les serveurs de sites, cette configuration n’est pas utilisée par les hôtes ISATAP.<br/><br/><ol><li>Pour configurer les Sites Active Directory et Services destinés à être transférés au sein des sites pour les hôtes ISATAP, pour chaque objet de sous-réseau IPv4, vous devez configurer un objet de sous-réseau IPv6 équivalent, dans lequel le préfixe d’adresse IPv6 pour le sous-réseau exprime le même hôte de la plage de ISATAP adresses de sous-réseau IPv4. Par exemple, pour l’IPv4 sous-réseau 192.168.99.0/24 et l’adresse ISATAP de 64 bits du préfixe 2002:836b:1:8000 :: / 64, le préfixe d’adresse IPv6 équivalent pour l’objet de sous-réseau IPv6 est 2002:836b:1:8000:0:5efe:192.168.99.0/120. Pour une longueur de préfixe IPv4 arbitraire (défini à 24 dans l’exemple), vous pouvez déterminer la longueur du préfixe IPv6 correspondante à partir de la formule 96 + IPv4PrefixLength.</li><li>Pour les adresses IPv6 de clients DirectAccess, ajoutez ce qui suit :<br/><br/><ul><li>Pour les clients DirectAccess basés sur Teredo : Un sous-réseau IPv6 pour la plage 2001:0:WWXX:YYZZ :: / 64, dans laquelle WWXX : YYZZ est la version hexadécimale à deux-points de la première adresse IPv4 du côté Internet du serveur d’accès à distance. .</li><li>Pour les clients DirectAccess basées sur IP-HTTPS : Un sous-réseau IPv6 pour la plage 2002:WWXX:YYZZ:8100 :: / 56, dans laquelle WWXX : YYZZ est la version hexadécimale à deux-points de la première adresse accessible sur Internet IPv4 (w.x.y.z) du serveur d’accès à distance. .</li><li>Pour les clients DirectAccess 6to4 : Série de préfixes IPv6 6to4 qui commencent par 2002: et représente les préfixes d'adresses IPv4 régionales, publiques qui sont administrés par l'autorité IANA (Internet Assigned Numbers Authority) et les registres régionaux. Le préfixe 6to4 pour un w.x.y.z/n de préfixe d’adresse IPv4 publique est 2002:WWXX:YYZZ :: / [16 + n], dans laquelle WWXX : YYZZ est la version hexadécimale à deux-points w.x.y.z.<br/><br/>        Par exemple, la plage 7.0.0.0/8 est administrée par le registre ARIN (American Registry for Internet Numbers) pour l'Amérique du Nord. Le préfixe 6to4 correspondant pour cette plage d’adresses IPv6 publique est 2002:700 :: / 24. Pour plus d’informations sur l’espace d’adresse publique IPv4, consultez [Registre de l’espace adressage IANA IPv4](https://www.iana.org/assignments/ipv4-address-space/ipv4-address-space.xml). .</li></ul></li></ol>|  
  
> [!IMPORTANT]  
> Assurez-vous que vous n’avez pas les adresses IP publiques sur l’interface interne du serveur DirectAccess. Si vous avez l’adresse IP publique sur l’interface interne, la connectivité via ISATAP peut échouer.  
  
### <a name="plan-firewall-requirements"></a>Planifier la configuration requise pour le pare-feu  
Si le serveur d'accès à distance se trouve derrière un pare-feu de périmètre, les exceptions suivantes sont requises pour le trafic d'accès à distance quand le serveur d'accès à distance est sur Internet IPv4 :  
  
-   Pour IP-HTTPS : Port de destination TCP (Control Protocol) de transmission 443 et le port TCP source 443 sortant.  
  
-   Pour le trafic Teredo : Port de destination UDP (Datagram Protocol) utilisateur 3544 entrant et port UDP source 3544 sortant.  
  
-   Pour le trafic 6to4 : IP protocole 41 entrant et sortant.  
  
    > [!NOTE]  
    > Pour les trafics Teredo et 6to4, ces exceptions doivent être appliquées pour les deux adresses IPv4 publiques consécutives côté Internet sur le serveur d’accès à distance.  
    >   
    > Les exceptions doivent être appliquées sur l’adresse qui est enregistré sur le serveur DNS public pour IP-HTTPS.  
  
-   Si vous déployez l’accès à distance avec une seule carte réseau et installez le serveur emplacement réseau sur le serveur d’accès à distance, le port TCP 62000.  
  
    > [!NOTE]  
    > Cette exemption est sur le serveur d’accès à distance et les exemptions précédentes se trouvent sur le pare-feu de périmètre.  
  
Les exceptions suivantes sont requises pour le trafic d’accès à distance lorsque le serveur d’accès à distance est sur Internet IPv6 :  
  
-   Protocole IP 50  
  
-   Port UDP de destination 500 entrant et port UDP source 500 sortant.  
  
-   Trafic ICMPv6 entrant et sortant (uniquement lors de l’utilisation de Teredo).  
  
Lorsque vous utilisez des pare-feu supplémentaires, appliquez les exceptions de pare-feu de réseau interne suivantes pour le trafic d’accès à distance :  
  
-   Pour ISATAP : Protocole 41 entrant et sortant  
  
-   Pour tout trafic IPv4/IPv6 : TCP/UD  
  
-   Pour Teredo : ICMP pour tout le trafic IPv4/IPv6  
  
### <a name="plan-certificate-requirements"></a>Planifier les exigences en matière de certificats  
Il existe trois scénarios qui nécessitent des certificats lorsque vous déployez un seul serveur d’accès à distance.  
  
-   **L’authentification IPsec**: Configuration requise des certificats pour IPsec inclure un certificat d’ordinateur qui est utilisé par les ordinateurs clients DirectAccess lorsqu’ils établissent la connexion IPsec avec le serveur d’accès à distance et un certificat d’ordinateur qui est utilisé par les serveurs d’accès à distance pour établir Connexions IPsec avec les clients DirectAccess.  
  
    Pour DirectAccess dans Windows Server 2012, l’utilisation de ces certificats IPsec n’est pas obligatoire. Comme alternative, le serveur d’accès à distance peut agir en tant que proxy pour l’authentification Kerberos sans exiger de certificats. Si l’authentification Kerberos est utilisée, il fonctionne via SSL, et le protocole Kerberos utilise le certificat qui a été configuré pour IP-HTTPS. Certains scénarios d’entreprise (y compris le déploiement multisite et l’authentification de client de mot de passe à usage unique) requièrent l’utilisation de l’authentification par certificat et pas l’authentification Kerberos.  
  
-   **Serveur IP-HTTPS**: Lorsque vous configurez l’accès à distance, le serveur d’accès à distance est automatiquement configuré pour agir en tant qu’écouteur web IP-HTTPS. Le site IP-HTTPS requiert un certificat de site web. Les ordinateurs clients doivent être en mesure de contacter le site de la liste de révocation de certificats correspondant.  
  
-   **Serveur d’emplacement réseau**: Le serveur Emplacement réseau est un site web utilisé pour détecter si les ordinateurs clients se trouvent dans le réseau d'entreprise. Le serveur emplacement réseau requiert un certificat de site Web. Pour ce certificat, les clients DirectAccess doivent pouvoir contacter le site contenant la liste de révocation de certificats.  
  
Les exigences d’autorité de certification pour chacun de ces scénarios est résumée dans le tableau suivant.  
  
|Authentification IPsec|Serveur IP-HTTPS|Serveur Emplacement réseau|  
|------------|----------|--------------|  
|Une autorité de certification interne est requise pour émettre les certificats d’ordinateur pour le serveur d’accès à distance et les clients pour l’authentification IPsec quand vous n’utilisez pas le protocole Kerberos pour l’authentification.|Autorité de certification interne : Vous pouvez utiliser une autorité de certification interne pour émettre le certificat IP-HTTPS. Toutefois, vous devez vous assurer que le point de distribution de la liste de révocation de certificats est disponible en externe.|Autorité de certification interne : Vous pouvez utiliser une autorité de certification interne pour émettre le certificat de site web du serveur Emplacement réseau. Assurez-vous que le point de distribution de la liste de révocation de certificats est hautement disponible à partir du réseau interne.|  
||Certificat auto-signé : Vous pouvez utiliser un certificat auto-signé pour le serveur IP-HTTPS. Un certificat auto-signé n'est pas utilisable dans un déploiement multisite.|Certificat auto-signé : Vous pouvez utiliser un certificat auto-signé pour le site Web serveur emplacement réseau ; Toutefois, vous ne pouvez pas utiliser un certificat auto-signé dans des déploiements multisites.|  
||Autorité de certification publique : Nous recommandons que vous utilisez une autorité de certification publique pour émettre le certificat IP-HTTPS, cela garantit que le point de distribution CRL est disponible en externe.||  
  
#### <a name="plan-computer-certificates-for-ipsec-authentication"></a>Planifier des certificats d'ordinateur pour l'authentification IPsec  
Si vous utilisez l’authentification IPsec basée sur certificat, le serveur d’accès à distance et les clients sont requis pour obtenir un certificat d’ordinateur. Pour installer les certificats, le plus simple consiste à utiliser la stratégie de groupe pour configurer l’inscription automatique de certificats d’ordinateur. Cela garantit que tous les membres du domaine obtiendront un certificat à partir d'une autorité de certification d'entreprise. Si vous n'avez pas d'entreprise définis dans votre organisation, consultez[Active Directory Certificate Services](https://technet.microsoft.com/library/cc770357.aspx).  
  
Ce certificat présente les exigences suivantes :  
  
-   Le certificat doit avoir l’utilisation de la clé (EKU) d’authentification client.  
  
-   Le client et les certificats de serveur doivent être liés au même certificat racine. Ce certificat racine doit être sélectionné dans les paramètres de configuration DirectAccess.  
  
#### <a name="plan-certificates-for-ip-https"></a>Planifier des certificats pour IP-HTTPS  
Le serveur d'accès à distance joue le rôle d'écouteur IP-HTTPS, et vous devez installer manuellement un certificat de site web HTTPS sur le serveur. Tenez compte des éléments suivants quand vous planifiez des certificats :  
  
-   Il est recommandé d'utiliser une autorité de certification publique, afin que les listes de révocation de certificats soient rapidement disponibles.  
  
-   Dans le champ objet, spécifiez l’adresse IPv4 de la carte Internet du serveur d’accès à distance ou le nom de domaine complet de l’URL IP-HTTPS (l’adresse ConnectTo). Si le serveur d'accès à distance se trouve derrière un périphérique NAT, vous devez spécifier le nom public ou l'adresse du périphérique NAT.  
  
-   Le nom commun du certificat doit correspondre au nom du site IP-HTTPS.  
  
-   Pour le **Enhanced Key Usage** champ, utilisez l’identificateur d’objet (OID) authentification du serveur.  
  
-   Pour le champ **Points de distribution de la liste de révocation de certificats**, indiquez un point de distribution de liste de révocation de certificats accessible par les clients DirectAccess connectés à Internet  
  
    > [!NOTE]  
    > Cela est uniquement requis pour les clients qui exécutent Windows 7.  
  
-   Le certificat IP-HTTPS doit avoir une clé privée.  
  
-   Le certificat IP-HTTPS doit être importé directement dans le magasin personnel.  
  
-   Les certificats IP-HTTPS peuvent utiliser des caractères génériques dans le nom.  
  
#### <a name="plan-website-certificates-for-the-network-location-server"></a>Planifier des certificats de site web pour le serveur Emplacement réseau  
Considérez les éléments suivants lorsque vous planifiez le site de serveur d’emplacement réseau :  
  
-   Dans le champ **Objet**, spécifiez une adresse IP de l'interface intranet du serveur Emplacement réseau ou le nom de domaine complet de l'URL d'emplacement réseau.  
  
-   Pour le **Enhanced Key Usage** champ, utilisez l’OID d’authentification du serveur.  
  
-   Pour le **Points de Distribution CRL** champ, utilisez un point de distribution CRL qui est accessible par les clients DirectAccess qui sont connectés à l’intranet. Ce point de distribution de liste de révocation de certificats ne doit pas être accessible depuis l'extérieur du réseau interne.  
  
> [!NOTE]  
> Assurez-vous que les certificats de serveur d’emplacement réseau et IP-HTTPS ont un nom de sujet. Si le certificat utilise un autre nom, il ne sera pas accepté par l’Assistant accès à distance.  
  
#### <a name="plan-dns-requirements"></a>Planifier la configuration DNS requise  
Cette section explique la configuration DNS requise pour les clients et serveurs dans un déploiement de l’accès à distance.  
  
##### <a name="directaccess-client-requests"></a>Demandes des clients DirectAccess  
DNS sert à résoudre les demandes des ordinateurs clients DirectAccess qui ne se trouvent pas sur le réseau interne. Les clients DirectAccess essaient de se connecter au serveur d’emplacement réseau DirectAccess pour déterminer s’ils se situent sur Internet ou sur le réseau d’entreprise.  
  
-   Si la connexion est établie, les clients sont déterminées comme étant sur l’intranet, DirectAccess n’est pas utilisé et les demandes des clients sont résolues en utilisant le serveur DNS configuré sur la carte réseau de l’ordinateur client.  
  
-   Si la connexion échoue, les clients sont considérés comme étant sur Internet. Les clients DirectAccess utilisent la table de stratégie de résolution de noms (NRPT) pour déterminer quel serveur DNS utiliser pour résoudre les demandes de noms. Vous pouvez préciser que les clients doivent utiliser DirectAccess DNS64 pour résoudre les noms, ou un autre serveur DNS interne.  
  
Pendant la résolution des noms, la table NRPT est utilisée par les clients DirectAccess pour identifier la manière de traiter une demande. Les clients demandent un nom de domaine complet ou un nom simple tel que <https://internal>. Si un nom en une partie est demandé, un suffixe DNS est ajouté pour créer un nom de domaine complet (FQDN). Si la requête DNS correspond à une entrée dans la table NRPT et que DNS4 ou un serveur DNS intranet est spécifié pour l’entrée, la requête est envoyée pour la résolution de noms à l’aide du serveur spécifié. Si une correspondance existe mais qu’aucun serveur DNS est spécifié, une règle d’exemption et la normale résolution de noms est appliquée.  
  
Lorsqu’un nouveau suffixe est ajouté à la table NRPT dans la console de gestion de l’accès à distance, les serveurs DNS par défaut pour le suffixe peuvent être découverte automatiquement en cliquant sur le **détecter** bouton. La détection automatique fonctionne comme suit :  
  
-   Si le réseau d’entreprise est basé sur IPv4, ou il utilise IPv4 et IPv6, l’adresse par défaut est l’adresse DNS64 de la carte interne sur le serveur d’accès à distance.  
  
-   Si le réseau d'entreprise est basé sur IPv6, l'adresse par défaut correspond à l'adresse IPv6 des serveurs DNS du réseau d'entreprise.  
  
##### <a name="infrastructure-servers"></a>Serveurs d'infrastructure  
  
-   **Serveur d’emplacement réseau**  
  
    Les clients DirectAccess essaient d'atteindre le serveur Emplacement réseau pour déterminer s'ils se situent sur le réseau interne. Les clients sur le réseau interne doivent être en mesure de résoudre le nom du serveur d’emplacement réseau, et ils doivent être empêchés de résolution du nom quand ils se situent sur Internet. Pour garantir cela, le nom de domaine complet (FQDN) du serveur Emplacement réseau est, par défaut, ajouté en tant que règle d'exemption à la table NRPT. De plus, quand vous configurez l'accès à distance, les règles suivantes sont automatiquement créées :  
  
    -   Un suffixe DNS, la règle pour le domaine racine ou le nom de domaine du serveur d’accès à distance et les adresses IPv6 qui correspondent aux serveurs DNS intranet configurés sur le serveur d’accès à distance. Par exemple, si le serveur d'accès à distance est membre du domaine corp.contoso.com, une règle est créée pour le suffixe DNS corp.contoso.com.  
  
    -   Règle d'exemption pour le nom de domaine complet du serveur d'emplacement réseau. Par exemple, si l’URL du serveur emplacement réseau est <https://nls.corp.contoso.com>, une règle d’exemption est créée pour le nom de domaine complet nls.corp.contoso.com.  
  
-   **Serveur IP-HTTPS**  
  
    Le serveur d’accès à distance agit en tant qu’écouteur IP-HTTPS et utilise son certificat de serveur pour s’authentifier auprès des clients IP-HTTPS. Le nom IP-HTTPS doit pouvoir être résolu par les clients DirectAccess qui utilisent des serveurs DNS publics.  
  
##### <a name="connectivity-verifiers"></a>Vérificateurs de connectivité  
L'accès à distance crée une sonde web par défaut utilisée par les ordinateurs clients DirectAccess pour vérifier la connectivité au réseau interne. Pour que la sonde fonctionne comme prévu, les noms suivants doivent être enregistrés manuellement dans DNS :  
  
-   **DirectAccess-webprobehost** doit se résoudre en adresse IPv4 interne du serveur d’accès à distance, ou à l’adresse IPv6 dans un environnement IPv6 uniquement.  
  
-   **DirectAccess-corpconnectivityhost** doit se résoudre l’adresse d’hôte local (bouclage). Vous devez créer des enregistrements A et AAAA. La valeur de l’enregistrement A est 127.0.0.1, et la valeur de l’enregistrement AAAA est construite à partir du préfixe NAT64 avec les 32 derniers bits sous la forme 127.0.0.1. Le préfixe NAT64 peut être récupéré en exécutant la **Get-netnatTransitionConfiguration** applet de commande Windows PowerShell.  
  
    > [!NOTE]  
    > Cela n’est valide uniquement dans les environnements IPv4 uniquement. Dans IPv4 et IPv6 ou un environnement IPv6 uniquement, créez un enregistrement AAAA avec l’adresse IP de bouclage :: 1.  
  
Vous pouvez créer d’autres vérificateurs de connectivité à l’aide d’autres adresses web via HTTP ou PING. Pour chaque vérificateur de connectivité, une entrée DNS doit exister.  
  
##### <a name="dns-server-requirements"></a>Configuration requise du serveur DNS  
  
-   Pour les clients DirectAccess, vous devez utiliser un serveur DNS exécutant Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 ou n’importe quel serveur DNS qui prend en charge IPv6.  
  
-   Vous devez utiliser un serveur DNS qui prend en charge les mises à jour dynamiques. Vous pouvez utiliser des serveurs DNS qui ne prennent pas en charge les mises à jour dynamiques, mais ensuite entrées doivent être mis à jour manuellement.  
  
-   Le nom de domaine complet pour vos points de distribution CRL doit pouvoir être résolu à l’aide de serveurs DNS Internet. Par exemple, si URL <https://crl.contoso.com/crld/corp-DC1-CA.crl> est dans le **Points de Distribution CRL** champ du certificat IP-HTTPS du serveur d’accès à distance, vous devez vous assurer que le nom de domaine complet crld.contoso.com ne peut être résolu à l’aide de serveurs DNS Internet.  
  
#### <a name="plan-for-local-name-resolution"></a>Plan de résolution de noms locale  
Considérez les éléments suivants lors de la planification pour la résolution de noms locale :  
  
##### <a name="nrpt"></a>NRPT  
Vous devrez peut-être créer des règles de table (NRPT) stratégie de résolution de noms supplémentaires dans les situations suivantes :  
  
-   Vous devez ajouter des suffixes DNS pour votre espace de noms intranet.  
  
-   Si les noms de domaine complets de vos points de distribution CRL sont basées sur votre espace de noms intranet, vous devez ajouter des règles d’exemption pour les noms de domaine complets des points de distribution CRL.  
  
-   Si vous avez un environnement DNS « split brain », vous devez ajouter des règles d’exemption pour les noms des ressources pour lequel vous souhaitez que les clients DirectAccess qui sont trouvent sur Internet pour accéder à la version d’Internet, plutôt que la version intranet.  
  
-   Si vous redirigez le trafic vers un site Web externe via vos serveurs de proxy web intranet, le site Web externe est disponible uniquement à partir de l’intranet. Il utilise les adresses de vos serveurs de proxy web pour autoriser les demandes entrantes. Dans ce cas, ajoutez une règle d’exemption pour le nom de domaine complet du site Web externe et spécifier que la règle utilise votre serveur de proxy web intranet plutôt que les adresses IPv6 des serveurs DNS intranet.  
  
    Par exemple, supposons que vous testez un site de Web externe nommé test.contoso.com. Ce nom n’est pas être résolu via les serveurs DNS Internet, mais le serveur de proxy web Contoso sait comment résoudre le nom et diriger les demandes pour le site Web au serveur web externe. Pour empêcher que les utilisateurs qui ne trouvent pas sur l'intranet Contoso puissent accéder au site, le site web externe autorise uniquement les demandes provenant de l'adresse Internet IPv4 du proxy web Contoso. Par conséquent, les utilisateurs intranet peuvent accéder au site Web, car ils utilisent le proxy web Contoso, mais les utilisateurs DirectAccess ne peut pas, car ils n’utilisent pas le proxy web Contoso. En configurant une règle d'exemption NRPT pour test.contoso.com qui utilise le proxy web Contoso, les demandes de page web pour test.contoso.com sont routées vers le serveur proxy web intranet via le réseau Internet IPv4.  
  
##### <a name="single-label-names"></a>Noms en une partie  
Unique des noms d’étiquette, tel que <https://paycheck>, sont parfois utilisés pour les serveurs intranet. Si un nom en une partie est demandé et une liste de recherche de suffixe DNS est configurée, les suffixes DNS dans la liste seront ajoutés au nom d’étiquette unique. Par exemple, lorsqu’un utilisateur sur un ordinateur qui est membre du domaine corp.contoso.com tape <https://paycheck> dans le navigateur web, le nom de domaine complet qui est créée en tant que le nom est paycheck.corp.contoso.com. Par défaut, le suffixe ajouté est basé sur le suffixe DNS principal de l’ordinateur client.  
  
> [!NOTE]  
> Dans un scénario d’espace de noms disjoint (où un ou plusieurs ordinateurs du domaine a un suffixe DNS qui ne correspond pas au domaine Active Directory auquel appartiennent les ordinateurs), vous devez vous assurer que la liste de recherche est personnalisée pour inclure tous les suffixes requis. Par défaut, l’Assistant accès à distance, configure le nom DNS d’Active Directory comme suffixe DNS principal sur le client. Assurez-vous d'ajouter le suffixe DNS utilisé par les clients pour la résolution de noms.  
  
Si plusieurs domaines et Windows Internet Service WINS (Name) sont déployés dans votre organisation, et vous vous connectez à distance, les noms en une partie peuvent être résolus comme suit :  
  
-   En déployant une zone de recherche directe WINS dans DNS. Lorsque vous tentez de résoudre computername.dns.zone1.corp.contoso.com, la demande est dirigée vers le serveur WINS qui utilise uniquement le nom d’ordinateur. Le client pense qu’il émet une demande d’enregistrements DNS A régulière, mais il s’agit en fait une demande NetBIOS.  
  
    Pour plus d’informations, consultez [la gestion d’une Zone de recherche directe](https://technet.microsoft.com/library/cc816891(WS.10).aspx).  
  
-   En ajoutant un suffixe DNS (par exemple, que dns.zone1.corp.contoso.com) à l’objet stratégie de groupe du domaine par défaut.  
  
##### <a name="split-brain-dns"></a>DNS « split brain »  
DNS « split brain » fait référence à l’utilisation du même domaine DNS pour la résolution de noms Internet et intranet.  
  
Pour les déploiements DNS « split brain », vous devez répertorier les noms de domaines complets dupliqués sur Internet et intranet et décider quelles ressources le client DirectAccess doit atteindre l’intranet ou la version d’Internet. Lorsque vous souhaitez que les clients DirectAccess atteignent la version d’Internet, vous devez ajouter le nom de domaine complet correspondant en tant qu’une règle d’exemption à la table NRPT pour chaque ressource.  
  
Dans un environnement DNS « split brain », si vous souhaitez que les deux versions de la ressource soit disponible, configurez vos ressources intranet avec des noms qui ne dupliquent pas les noms qui sont utilisés sur Internet. Ensuite, demandez à vos utilisateurs à utiliser l’autre nom lorsqu’ils accèdent à la ressource sur l’intranet. Par exemple, configurez www\.internal.contoso.com pour le nom interne de www\.contoso.com.  
  
Dans un environnement DNS non « split brain », l'espace de noms Internet est différent de l'espace de noms intranet. Par exemple, la société Contoso Corporation utilise contoso.com sur Internet et corp.contoso.com sur l'intranet. Étant donné que toutes les ressources intranet utilisent le suffixe DNS corp.contoso.com, la règle NRPT pour corp.contoso.com dirige toutes les requêtes de nom DNS pour les ressources intranet vers les serveurs DNS intranet. Les requêtes DNS pour les noms avec le suffixe contoso.com ne correspondent pas à la règle d’espace de noms intranet corp.contoso.com dans la table NRPT, et ils sont envoyés aux serveurs DNS Internet. Avec un déploiement DNS non « split brain », étant donné qu'il n'y a aucune duplication de noms de domaines complets pour les ressources intranet et Internet, aucune configuration supplémentaire n'est requise pour la table NRPT. Les clients DirectAccess peuvent accéder aux ressources Internet et intranet pour leur organisation.  
  
##### <a name="plan-local-name-resolution-behavior-for-directaccess-clients"></a>Planifier le comportement de résolution de noms locale pour les clients DirectAccess  
Si un nom ne peut pas être résolu avec DNS, le service Client DNS dans Windows Server 2012, Windows 8, Windows Server 2008 R2 et Windows 7 peuvent utiliser la résolution de noms locale, avec le lien-Local Multicast LLMNR (Name Resolution) et NetBIOS sur les protocoles TCP/IP pour résoudre le  nom du sous-réseau local. La résolution de noms locale est généralement nécessaire pour la connectivité de réseau pair à pair lorsque l'ordinateur se trouve sur des réseaux privés, tels que des réseaux domestiques à sous-réseau unique.  
  
Lorsque le service Client DNS effectue la résolution de noms locale pour les noms de serveur intranet, et l’ordinateur est connecté à un sous-réseau partagé sur Internet, les utilisateurs malveillants peuvent capturer LLMNR et NetBIOS sur des messages TCP/IP afin de déterminer les noms de serveur intranet. Dans la page DNS de l’Assistant d’installation de Server Infrastructure, vous pouvez configurer le comportement de résolution de noms locale selon les types de réponses reçus des serveurs DNS intranet. Les options ci-dessous sont disponibles :  
  
-   **Utiliser la résolution de noms locale si le nom n’existe pas dans DNS**: Cette option est la plus sécurisée car le client DirectAccess exécute uniquement la résolution de noms locale pour les noms de serveur qui ne peuvent pas être résolus par les serveurs DNS intranet. Si les serveurs DNS intranet sont accessibles, les noms des serveurs intranet sont résolus. S'ils ne sont pas accessibles ou que d'autres types d'erreurs DNS surviennent, les noms de serveur intranet ne seront pas révélés sur le sous-réseau via la résolution de noms locale.  
  
-   **Utiliser la résolution de noms locale si le nom n’existe pas dans DNS ou de serveurs DNS ne sont pas accessibles lorsque l’ordinateur client est sur un réseau privé (recommandé)** : Cette option est recommandée car elle autorise l'utilisation de la résolution de noms locale sur un réseau privé uniquement lorsque les serveurs DNS intranet sont inaccessibles.  
  
-   **Utiliser la résolution de noms locale pour tout type d’erreur de résolution DNS (moins sécurisé)** : Il s'agit de l'option la moins sécurisée car les noms de serveurs réseau intranet peuvent être révélés au sous-réseau local via la résolution de noms locale.  
  
#### <a name="plan-the-network-location-server-configuration"></a>Planifier la configuration du serveur emplacement réseau  
Le serveur Emplacement réseau est un site web utilisé pour détecter si les clients DirectAccess se trouvent dans le réseau d'entreprise. Les clients du réseau d’entreprise n’utilisent pas de DirectAccess pour accéder aux ressources internes ; mais au lieu de cela, ils se connectent directement.  
  
Le site de serveur d’emplacement réseau peut être hébergé sur le serveur d’accès à distance ou sur un autre serveur dans votre organisation. Si vous hébergez le serveur d’emplacement réseau sur le serveur d’accès à distance, le site Web est créé automatiquement lorsque vous déployez l’accès à distance. Si vous hébergez le serveur d’emplacement réseau sur un autre serveur exécutant un système d’exploitation de Windows, il se peut que vous devez vous assurer que les Internet Information Services (IIS) est installé sur ce serveur, et que le site Web est créé. Accès à distance ne configure pas les paramètres sur le serveur emplacement réseau.  
  
Assurez-vous que le site de serveur d’emplacement réseau respecte les exigences suivantes :  
  
-   Dispose d’un certificat de serveur HTTPS.  
  
-   A une haute disponibilité pour les ordinateurs sur le réseau interne.  
  
-   N’est pas accessible aux ordinateurs clients DirectAccess sur Internet.  
  
-  
  
En outre, tenez compte des exigences suivantes pour les clients lorsque vous configurez votre site de serveur d’emplacement réseau :  
  
-   Les ordinateurs clients DirectAccess doivent faire confiance à l'autorité de certification qui a émis le certificat de serveur pour le site web du serveur Emplacement réseau.  
  
-   Les ordinateurs clients DirectAccess figurant sur le réseau interne doivent être en mesure de résoudre le nom du site du serveur Emplacement réseau.  
  
##### <a name="plan-certificates-for-the-network-location-server"></a>Planifier des certificats pour le serveur emplacement réseau  
Lorsque vous obtenez le certificat de site Web à utiliser pour le serveur emplacement réseau, considérez les points suivants :  
  
-   Dans le champ **Objet**, indiquez l'adresse IP de l'interface intranet du serveur Emplacement réseau ou le nom de domaine complet de l'URL d'emplacement réseau.  
  
-   Pour le **Enhanced Key Usage** champ, utilisez l’OID d’authentification du serveur.  
  
-   Le certificat de serveur d’emplacement réseau doit être vérifié par rapport à une liste de révocation de certificats (CRL). Pour le **Points de Distribution CRL** champ, utilisez un point de distribution CRL qui est accessible par les clients DirectAccess qui sont connectés à l’intranet. Ce point de distribution de liste de révocation de certificats ne doit pas être accessible depuis l'extérieur du réseau interne.  
  
##### <a name="plan-dns-for-the-network-location-server"></a>Planifier DNS pour le serveur emplacement réseau  
Les clients DirectAccess essaient d'atteindre le serveur Emplacement réseau pour déterminer s'ils se situent sur le réseau interne. Les clients qui se trouvent sur le réseau interne doivent être en mesure de résoudre le nom du serveur d'emplacement réseau, mais il est impératif de les empêcher de le faire quand ils se situent sur Internet. Pour le garantir, le nom de domaine complet (FQDN) du serveur d'emplacement réseau est, par défaut, ajouté en tant que règle d'exemption à la NRPT.  
  
### <a name="plan-management-servers-configuration"></a>Planifier la configuration des serveurs d’administration  
Les clients DirectAccess lancent des communications avec les serveurs d’administration qui fournissent des services tels que Windows Update et mises à jour antivirus. Clients DirectAccess utilisent également le protocole Kerberos pour s’authentifier auprès des contrôleurs de domaine avant qu’ils accèdent au réseau interne. Au cours de l'administration distante des clients DirectAccess, les serveurs d'administration communiquent avec les ordinateurs clients pour exécuter des fonctions d'administration telles que l'évaluation de l'inventaire logiciel et matériel. L'accès à distance peut détecter automatiquement certains serveurs d'administration, dont notamment :  
  
-   Contrôleurs de domaine : La détection automatique des contrôleurs de domaine est effectuée pour les domaines contenant des ordinateurs clients et pour tous les domaines dans la même forêt que le serveur d’accès à distance.  
  
-   Serveurs System Center Configuration Manager  
  
Contrôleurs de domaine et System Center Configuration Manager de serveurs sont détectés automatiquement la première fois DirectAccess est configuré. Les contrôleurs de domaine détectés ne sont pas affichés dans la console, mais les paramètres peuvent être récupérées à l’aide des applets de commande Windows PowerShell. Si le contrôleur de domaine ou serveurs System Center Configuration Manager sont modifiées, en cliquant sur **serveurs d’administration de mise à jour** dans la console actualise la liste de serveurs de gestion.  
  
**Configuration requise du serveur Management**  
  
-   Serveurs d’administration doivent être accessibles via le tunnel d’infrastructure. Lorsque vous configurez l'accès à distance, l'ajout de serveurs dans la liste des serveurs d'administration les rend accessibles via ce tunnel.  
  
-   Serveurs d’administration qui établissent des connexions aux clients DirectAccess doivent prendre entièrement en charge IPv6, au moyen d’une adresse IPv6 native ou à l’aide d’une adresse attribuée par ISATAP.  
  
### <a name="plan-active-directory-requirements"></a>Planifier la configuration requise d’Active Directory  
Accès à distance utilise Active Directory comme suit :  
  
-   **Authentification** : Le tunnel d’infrastructure utilise l’authentification NTLMv2 pour le compte d’ordinateur qui se connecte au serveur d’accès à distance, et le compte doit être dans un domaine Active Directory. Le tunnel intranet utilise l’authentification Kerberos pour l’utilisateur pour créer le tunnel intranet.  
  
-   **Les objets de stratégie de groupe**: Accès à distance rassemble les paramètres de configuration dans les objets de stratégie de groupe (GPO), qui sont appliqués aux serveurs d’accès distant, les clients et les serveurs d’applications internes.  
  
-   **Groupes de sécurité** : Accès à distance utilise des groupes de sécurité pour rassembler et identifier les ordinateurs clients DirectAccess. Stratégie de groupe est appliqués aux groupes de sécurité requis.  
  
Lorsque vous planifiez un environnement Active Directory pour un déploiement de l’accès à distance, tenez compte des exigences suivantes :  
  
-   Au moins un contrôleur de domaine est installé sur le système d’exploitation Windows Server 2012, Windows Server 2008 R2 Windows Server 2008 ou Windows Server 2003.  
  
    Si le contrôleur de domaine est sur un réseau de périmètre (et est donc accessible à partir de l’adaptateur de réseau côté Internet du serveur d’accès à distance), empêcher le serveur d’accès à distance d’y accéder. Vous devez ajouter des filtres de paquets sur le contrôleur de domaine pour empêcher toute connectivité à l’adresse IP de la carte Internet.  
  
-   Le serveur d’accès à distance doit être membre d’un domaine.  
  
-   Les clients DirectAccess doivent appartenir au domaine. Les clients peuvent appartenir :  
  
    -   à tout domaine inclus dans la même forêt que le serveur d'accès à distance ;  
  
    -   à tout domaine doté d'une approbation bidirectionnelle avec le domaine du serveur d'accès à distance ;  
  
    -   N’importe quel domaine dans une forêt ayant une approbation bidirectionnelle avec la forêt du domaine du serveur d’accès à distance.  
  
> [!NOTE]  
> -   Le serveur d'accès à distance ne peut pas être un contrôleur de domaine.  
> -   Le contrôleur de domaine Active Directory qui est utilisé pour l’accès à distance ne doit pas être accessible à partir de la carte Internet externe du serveur d’accès à distance (la carte ne doit pas figurer dans le profil de domaine du pare-feu de Windows).  
  
#### <a name="plan-client-authentication"></a>Planifier l'authentification client  
L’accès à distance dans Windows Server 2012, vous pouvez choisir entre l’utilisation de l’authentification Kerberos intégrée, qui utilise des noms d’utilisateur et mots de passe, ou à l’aide de certificats pour l’authentification d’ordinateur IPsec.  
  
**L’authentification Kerberos**: Lorsque vous choisissez d’utiliser les informations d’identification Active Directory pour l’authentification, DirectAccess utilise tout d’abord l’authentification Kerberos pour l’ordinateur, puis il utilise l’authentification Kerberos pour l’utilisateur. Lorsque vous utilisez ce mode d’authentification, DirectAccess utilise un tunnel de sécurité unique qui fournit l’accès pour le serveur DNS, le contrôleur de domaine et tout autre serveur sur le réseau interne  
  
**L’authentification IPsec**: Lorsque vous choisissez d’utiliser l’authentification à deux facteurs ou la Protection d’accès réseau, DirectAccess utilise deux tunnels de sécurité. L’Assistant Installation de l’accès à distance configure des règles de sécurité de connexion dans le pare-feu Windows avec fonctions avancées de sécurité. Ces règles spécifient les informations d’identification suivantes lors de la négociation de sécurité IPsec pour le serveur d’accès à distance :  
  
-   Le tunnel d’infrastructure utilise les informations d’identification de certificat pour la première authentification et les informations d’identification de l’utilisateur (NTLMv2) pour la seconde authentification. Informations d’identification utilisateur forcent l’utilisation du protocole Authenticated Internet Protocol (AuthIP), et fournissent l’accès à un serveur DNS et le contrôleur de domaine avant que le client DirectAccess peut utiliser les informations d’identification Kerberos pour le tunnel intranet.  
  
-   Le tunnel intranet utilise les informations d’identification de certificat pour la première authentification et les informations d’identification de l’utilisateur (Kerberos V5) pour la seconde authentification.  
  
#### <a name="plan-multiple-domains"></a>Planifier plusieurs domaines  
La liste des serveurs d'administration doit inclure les contrôleurs de domaine de tous les domaines qui contiennent des groupes de sécurité qui incluent des ordinateurs clients DirectAccess. Il doit contenir tous les domaines qui contiennent des comptes d’utilisateurs qui peuvent utiliser des ordinateurs configurés en tant que clients DirectAccess. Cela garantit que des utilisateurs qui ne se trouvent pas dans le même domaine que l'ordinateur client qu'ils utilisent seront authentifiés auprès d'un contrôleur de domaine dans le domaine de l'utilisateur.  
  
Cette authentification est automatique si les domaines se trouvent dans la même forêt. S’il existe un groupe de sécurité avec les ordinateurs clients ou serveurs d’applications qui se trouvent dans des forêts différentes, les contrôleurs de domaine de ces forêts ne sont pas détectés automatiquement. Forêts ne sont également pas détectés automatiquement. Vous pouvez exécuter la tâche **serveurs d’administration de mise à jour** dans le **gestion de l’accès à distance** pour détecter ces contrôleurs de domaine.  
  
Si possible, common suffixes de noms de domaine doivent être ajoutés à la table NRPT pendant le déploiement de l’accès à distance. Par exemple, si vous possédez deux domaines, domain1.corp.contoso.com et domain2.corp.contoso.com, au lieu d'ajouter deux entrées dans la table NRPT, vous pouvez ajouter une entrée de suffixe DNS commun, où le suffixe de nom de domaine est corp.contoso.com. Cela se produit automatiquement pour les domaines dans la même racine. Les domaines qui ne sont pas dans la même racine doivent être ajoutés manuellement.  
  
### <a name="plan-group-policy-object-creation"></a>Planifier la création d’objets de stratégie de groupe  
Lorsque vous configurez l’accès à distance, les paramètres DirectAccess sont rassemblés dans des objets de stratégie de groupe (GPO). Deux objets de stratégie de groupe sont renseignés avec les paramètres DirectAccess, et ils sont distribués comme suit :  
  
-   **Le client DirectAccess GPO**: Cet objet de stratégie de groupe contient des paramètres de client, y compris les paramètres de technologie de transition IPv6, des entrées NRPT et des règles de sécurité de connexion pour le pare-feu Windows avec fonctions avancées de sécurité. Il est appliqué aux groupes de sécurité spécifiés pour les ordinateurs clients.  
  
-   **Le serveur DirectAccess GPO**: Cet objet de stratégie de groupe contient les paramètres de configuration DirectAccess qui sont appliqués à n’importe quel serveur que vous avez configuré comme serveur d’accès à distance dans votre déploiement. Il contient également des règles de sécurité de connexion pour le pare-feu Windows avec fonctions avancées de sécurité.  
  
> [!NOTE]  
> Configuration des serveurs d’applications n’est pas pris en charge dans la gestion à distance des clients DirectAccess, car les clients ne peuvent pas accéder au réseau interne du serveur DirectAccess où résident les serveurs d’application. Étape 4 dans l’écran de configuration de configuration de l’accès à distance n’est pas disponible pour ce type de configuration.  
  
Vous pouvez configurer la stratégie de groupe automatiquement ou manuellement.  
  
**Automatiquement**: Lorsque vous spécifiez que les GPO sont créés automatiquement, un nom par défaut est spécifié pour chaque objet de stratégie de groupe.  
  
**Manuellement**: Vous pouvez utiliser des objets de stratégie de groupe qui ont été prédéfinis par l’administrateur Active Directory.  
  
Lorsque vous configurez vos GPO, prenez en compte les avertissements suivants :  
  
-   Une fois que DirectAccess a été configuré pour utiliser des objets de stratégie de groupe spécifiques, il ne peut plus être configuré pour en utiliser d'autres.  
  
-   Utilisez la procédure suivante pour sauvegarder tous les objets de stratégie de groupe de l’accès à distance avant d’exécuter les applets de commande DirectAccess :  
  
    [Sauvegarder et restaurer la configuration d’Accès à distance](https://go.microsoft.com/fwlink/?LinkID=257928).  
  
-   Si vous utilisez automatiquement ou stratégie de groupe configurés manuellement, vous devez ajouter une stratégie pour la détection de liaison lente si vos clients utilisent la 3G. Le chemin d’accès pour **stratégie : Configurer la détection de liaison lente de stratégie de groupe** est :  
  
    **Computer configuration/Policies/Administrative Templates/System/Group Policy**.  
  
-   Si les autorisations appropriées pour la liaison d’objets n’existent pas, un avertissement est émis. L’accès à distance continue de fonctionner, mais de liaison n’a pas lieu. Si cet avertissement est émis, liens ne sont pas créés automatiquement, même si les autorisations sont ajoutées plus tard. L'administrateur doit créer les liens manuellement.  
  
#### <a name="automatically-created-gpos"></a>Créé automatiquement des objets stratégie de groupe  
Prenez en compte les éléments suivants lorsque vous utilisez automatiquement créé des objets stratégie de groupe :  
  
Créé automatiquement GPO sont appliqués en fonction de l’emplacement et la cible du lien, comme suit :  
  
-   Pour l’objet stratégie de groupe du serveur DirectAccess, l’emplacement et la cible du lien pointent sur le domaine qui contient le serveur d’accès à distance.  
  
-   Lors de l’application client et serveur GPO sont créés, l’emplacement est défini sur un domaine unique. Le nom de l’objet stratégie de groupe est recherché dans chaque domaine, et le domaine est rempli avec les paramètres DirectAccess s’il existe.  
  
-   La cible du lien est définie à la racine du domaine dans lequel l'objet de stratégie de groupe a été créé. Un objet de stratégie de groupe est créé pour chaque domaine qui contient des ordinateurs clients ou des serveurs d'applications, et il est lié à la racine de son domaine respectif.  
  
Lorsque vous utilisez créés automatiquement GPO pour appliquer les paramètres DirectAccess, l’administrateur de serveur d’accès à distance requiert les autorisations suivantes :  
  
-   Autorisations pour créer des objets stratégie de groupe pour chaque domaine.  
  
-   Autorisations pour lier à toutes les racines de domaine client sélectionné.  
  
-   Autorisations pour créer un lien vers les racines de domaine du serveur de stratégie de groupe.  
  
-   Autorisations de sécurité pour créer, modifier, supprimer et modifier les objets de stratégie de groupe.  
  
-   GPO autorisations en lecture pour chaque domaine requis. Cette autorisation n’est pas requise, mais il est recommandé, car elle permet un accès à distance vérifier que les objets stratégie de groupe avec des noms dupliqués n’existent pas lors de la création de stratégie de groupe.  
  
#### <a name="manually-created-gpos"></a>Créé manuellement la stratégie de groupe  
Prenez en compte les points suivants quand vous utilisez des objets de stratégie de groupe créés manuellement :  
  
-   Les objets de stratégie de groupe doivent déjà exister quand vous exécutez l'Assistant Configuration de l'accès à distance.  
  
-   Pour appliquer les paramètres DirectAccess, l’administrateur de serveur d’accès à distance requiert des autorisations de sécurité totale à créer, modifier, supprimer et modifier les objets de stratégie de groupe créés manuellement.  
  
-   Une recherche est effectuée pour un lien vers l’objet de stratégie de groupe dans l’ensemble du domaine. Si l'objet de stratégie de groupe n'est pas lié dans le domaine, un lien est automatiquement créé à la racine du domaine. Si les autorisations requises pour créer le lien ne sont pas disponibles, un avertissement est émis.  
  
#### <a name="recovering-from-a-deleted-gpo"></a>Récupération après la suppression d'un objet de stratégie de groupe  
Si un objet de stratégie de groupe sur un serveur accès à distance, le client ou le serveur d’applications a été supprimé par inadvertance, le message d’erreur suivant s’affiche : **Impossible de trouver l’objet de stratégie de groupe (nom de l’objet stratégie de groupe)** .  
  
Si vous disposez d'une sauvegarde, vous pouvez restaurer l'objet de stratégie de groupe. Si aucune sauvegarde n’est disponible, vous devez supprimer les paramètres de configuration et les configurer à nouveau.  
  
###### <a name="to-remove-configuration-settings"></a>Pour supprimer les paramètres de configuration  
  
1.  Exécutez l’applet de commande Windows PowerShell **Uninstall-RemoteAccess**.  
  
2.  Ouvrez **gestion de l’accès à distance**.  
  
3.  Un message d'erreur indique que l'objet de stratégie de groupe est introuvable. Cliquez sur **Supprimer les paramètres de configuration**. Une fois terminé, le serveur sera restauré à un état non configuré, et vous pouvez reconfigurer les paramètres.  
  


