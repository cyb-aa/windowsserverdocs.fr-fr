---
title: Étape 1 Plan l’Infrastructure DirectAccess avancée
description: Cette rubrique fait partie du guide de déployer un serveur DirectAccess unique avec les paramètres avancés pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aa3174f3-42af-4511-ac2d-d8968b66da87
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cd94c1a05ba52a590d6f84122f81764f52b1ae47
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880960"
---
# <a name="step-1-plan-the-advanced-directaccess-infrastructure"></a>Étape 1 Plan l’Infrastructure DirectAccess avancée

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

La première étape de la planification d'un déploiement avancé de DirectAccess sur un serveur unique consiste à planifier l'infrastructure requise pour le déploiement. Cette rubrique décrit les étapes de planification de l’infrastructure. Vous pouvez effectuer les tâches de planification dans l'ordre qui vous convient.  
  
|Tâche|Description|  
|----|--------|  
|[1.1 planifier la topologie de réseau et les paramètres](#bkmk_11Networksvrtopsettings)|Décidez où placer le serveur DirectAccess (à la périphérie ou derrière un pare-feu ou un périphérique de traduction d'adresses réseau (NAT)) et planifiez l'adressage IP, le routage et le tunneling forcé.|  
|[1.2 planifier la configuration requise du pare-feu](#bkmk_ConfigFirewalls)|Planifiez l'autorisation du trafic DirectAccess via des pare-feu de périmètre.|  
|[1.3 planifier la configuration requise des certificats](#bkmk_12CAsandCerts)|Déterminez si vous souhaitez utiliser Kerberos ou des certificats pour l'authentification client, et planifiez vos certificats de site web. IP-HTTPS est un protocole de transition utilisé par les clients DirectAccess pour transférer le trafic IPv6 via des réseaux IPv4. Décidez d'authentifier le serveur utilisant IP-HTTPS à l'aide d'un certificat émis par une autorité de certification ou d'un certificat auto-signé émis automatiquement par le serveur DirectAccess.|  
|[1.4 planifier la configuration DNS requise](#bkmk_14Dns)|Planifiez les paramètres DNS (Domain Name System) pour le serveur DirectAccess, les serveurs d'infrastructure, les options de résolution de noms locale et la connectivité client.|  
|[1.5 planifier le serveur emplacement réseau](#bkmk_14NLS)|Les clients DirectAccess utilisent le serveur Emplacement réseau pour déterminer s'ils se trouvent sur le réseau interne. Décidez où placer le site web du serveur Emplacement réseau dans votre organisation (sur le serveur DirectAccess ou sur un autre serveur) et planifiez les certificats requis si le serveur Emplacement réseau se trouve sur le serveur DirectAccess.|  
|[1.6 planifier les serveurs d’administration](#bkmk_15mgmtservers)|Vous pouvez administrer à distance les ordinateurs clients DirectAccess situés en dehors du réseau d'entreprise sur Internet. Planifiez les serveurs d’administration (tels que les serveurs de mise à jour) utilisés lors de l’administration à distance des clients.|  
|[1.7 planifier Active Directory Domain Services](#bkmk_16AD)|Planifiez vos contrôleurs de domaine, la configuration Active Directory requise, l'authentification client et plusieurs domaines.|  
|[1.8 les objets de stratégie de groupe de plan de](#bkmk_17GPOs)|Déterminez quels objets de stratégie de groupe sont requis dans votre organisation et comment les créer ou les modifier.|  
  
## <a name="bkmk_11Networksvrtopsettings"></a>1.1 planifier la topologie de réseau et les paramètres  
Cette section explique comment planifier votre réseau, y compris :  
  
-   [1.1.1 planifier les cartes réseau et l’adressage IP](#BKMK_NA)  
  
-   [1.1.2 planifier la connectivité intranet IPv6](#bkmk_intranet)  
  
-   [1.1.3 planifier le tunneling forcé](#bkmk_force)  
  
### <a name="BKMK_NA"></a>1.1.1 planifier les cartes réseau et l’adressage IP  
  
1.  Identifiez la topologie des cartes réseau à utiliser. DirectAccess peut être configuré en utilisant l'une ou l'autre des topologies suivantes :  
  
    -   **Deux cartes réseau**. Le serveur DirectAccess peut être installé à la périphérie avec la première carte réseau connectée à Internet et la seconde au réseau interne, ou installé derrière un périphérique NAT, un pare-feu ou un routeur, avec la première carte réseau connectée à un réseau de périmètre et la seconde au réseau interne.  
  
    -   **Une seule carte réseau**. Le serveur DirectAccess est installé derrière un périphérique NAT et l'unique carte réseau est connectée au réseau interne.  
  
2.  Identifiez vos exigences en matière d'adressage IP :  
  
    DirectAccess utilise IPv6 avec IPsec pour créer une connexion sécurisée entre les ordinateurs clients DirectAccess et le réseau d'entreprise interne. Pourtant, DirectAccess ne requiert pas systématiquement une connectivité Internet IPv6 ni une prise en charge IPv6 native sur les réseaux internes. Au lieu de cela, il configure et utilise automatiquement des technologies de transition IPv6 pour transférer le trafic IPv6 sur Internet IPv4 (en utilisant 6to4, Teredo ou IP-HTTPS) et sur votre intranet IPv4 uniquement (en utilisant NAT64 ou ISATAP). Pour obtenir une vue d'ensemble de ces technologies de transition, consultez les ressources suivantes :  
  
    -   [Technologies de Transition IPv6](https://technet.microsoft.com/library/bb726951.aspx)  
  
    -   [Spécification du protocole de Tunneling IP-HTTPS](https://msdn.microsoft.com/library/dd358571(PROT.10).aspx)  
  
3.  Configurez les cartes et les adresses requises selon le tableau ci-dessous. Pour les déploiements qui utilisent une seule carte réseau et sont configurés derrière un périphérique NAT, configurez vos adresses IP en utilisant uniquement la colonne **Carte réseau interne**.  
  
    ||Carte réseau externe|Carte réseau interne|Exigences en matière de routage|  
    |-|--------------|--------------|------------|  
    |Internet IPv4 et intranet IPv4|Configurez deux adresses IPv4 publiques statiques, consécutives avec les masques de sous-réseau appropriés (requis pour Teredo uniquement).<br /><br />Configurez également l'adresse IPv4 de la passerelle par défaut de votre pare-feu Internet ou du routeur de votre fournisseur de services Internet local. **Remarque :** Le serveur DirectAccess requiert deux adresses IPv4 publiques consécutives pour pouvoir agir comme un serveur Teredo et pour que les clients Windows puissent utiliser le serveur DirectAccess pour détecter le type de périphérique NAT derrière lequel ils se trouvent.|Configurez ce qui suit :<br /><br />-Adresse intranet IPv4 avec le masque de sous-réseau approprié.<br />-Le suffixe DNS spécifique à la connexion de votre espace de noms intranet. Vous devez également configurer un serveur DNS sur l'interface interne. **Attention :** Ne configurez pas de passerelle par défaut sur toutes les interfaces intranet.|Pour configurer le serveur DirectAccess afin d'atteindre tous les sous-réseaux du réseau IPv4 interne, procédez comme suit :<br /><br />-Répertorie les espaces d’adressage IPv4 pour tous les emplacements sur votre intranet.<br />-Utilisez le **itinéraire Ajouter -p** ou**netsh interface ipv4 ajouter itinéraire** commande pour ajouter les espaces d’adressage IPv4 en tant qu’itinéraires statiques dans la table de routage IPv4 du serveur DirectAccess.|  
    |Internet IPv6 et intranet IPv6|Configurez ce qui suit :<br /><br />-Utilisez la configuration d’adresse qui est fournie par votre fournisseur de services Internet.<br />-Utilisez le **Route Print** commande pour vous assurer qu’itinéraire IPv6 par défaut existe et qu’il pointe vers le routeur du fournisseur de services Internet dans la table de routage IPv6.<br />-Déterminer si les fournisseur de services Internet et les routeurs intranet utilisent les préférences de routeur par défaut décrit dans RFC 4191 et à l’aide d’une préférence par défaut plus élevée que vos routeurs intranet locaux.<br />    Si vous avez répondu par l'affirmative dans les deux cas, aucune autre configuration n'est requise pour l'itinéraire par défaut. Une préférence plus élevée pour le routeur ISP permet de s'assurer que l'itinéraire IPv6 par défaut actif du serveur DirectAccess pointe vers Internet IPv6.<br /><br />Étant donné que le serveur DirectAccess est un routeur IPv6, si vous disposez d'une infrastructure IPv6 native, l'interface Internet peut également atteindre les contrôleurs de domaine sur l'intranet. Dans ce cas, ajoutez des filtres de paquets au contrôleur de domaine dans le réseau de périmètre pour empêcher la connectivité IPv6 de l'interface Internet du serveur DirectAccess.|Configurez ce qui suit :<br /><br />-Si vous n’utilisez pas les niveaux de préférence par défaut, vous pouvez configurer vos interfaces intranet à l’aide de la commande suivante**netsh interface ipv6 définir InterfaceIndex ignoredefaultroutes = activé**.<br />    Cette commande vérifie que les itinéraires par défaut supplémentaires qui pointent vers les routeurs intranet ne seront pas ajoutés à la table de routage IPv6. Vous pouvez obtenir l’index de vos interfaces intranet à l’aide de la commande suivante : **netsh interface ipv6 show interface**.|Si vous avez un intranet IPv6, procédez comme suit pour configurer le serveur DirectAccess afin d'atteindre tous les emplacements IPv6 :<br /><br />-Répertorie les espaces d’adressage IPv6 pour tous les emplacements sur votre intranet.<br />-Utilisez le **netsh interface ipv6 ajouter itinéraire** commande pour ajouter les espaces d’adressage IPv6 en tant qu’itinéraires statiques dans la table de routage IPv6 du serveur DirectAccess.|  
    |Internet IPv4 et intranet IPv6|Le serveur DirectAccess transfère le trafic de l'itinéraire IPv6 par défaut via la carte Microsoft 6to4 à un relais 6to4 sur Internet IPv4. Vous pouvez configurer un serveur DirectAccess pour l’adresse IPv4 de la carte Microsoft 6to4 à l’aide de la commande suivante : **netsh interface ipv6 6to4 set nom de relais =<ipaddress> état = activé**.|||  
  
    > [!NOTE]  
    > -   Si une adresse IPv4 publique a été attribuée au client DirectAccess, il utilisera la technologie de transition 6to4 pour se connecter à l'intranet. Si une adresse IPv4 privée lui a été attribuée, il utilisera Teredo. Si le client DirectAccess ne peut pas se connecter au serveur DirectAccess avec 6to4 ou Teredo, il utilisera IP-HTTPS.  
    > -   Pour utiliser Teredo, vous devez configurer deux adresses IP consécutives sur la carte réseau tournée vers l'extérieur.  
    > -   Vous ne pouvez pas utiliser Teredo si le serveur DirectAccess possède une seule carte réseau.  
    > -   Les ordinateurs clients IPv6 natif se connectent au serveur DirectAccess via IPv6 natif et aucune technologie de transition n'est requise.  
  
### <a name="bkmk_intranet"></a>1.1.2 planifier la connectivité intranet IPv6  
Pour gérer des clients DirectAccess distants, IPv6 est requis. IPv6 permet aux serveurs d'administration DirectAccess de se connecter aux clients DirectAccess situés sur Internet à des fins d'administration à distance.  
  
> [!NOTE]  
> -   L'utilisation du protocole IPv6 sur votre réseau n'est pas requise pour prendre en charge les connexions initiées par les ordinateurs clients DirectAccess aux ressources IPv4 sur le réseau de votre organisation. NAT64/DNS64 est utilisé à ces fins.  
> -   Si vous n'administrez pas de clients DirectAccess distants, vous n'avez pas besoin de déployer IPv6.  
> -   Le protocole ISATAP (Intra-Site Automatic Tunnel Addressing Protocol) n'est pas pris en charge dans les déploiements de DirectAccess.  
> -   Lorsque vous utilisez IPv6, vous pouvez activer des requêtes d'enregistrement de ressource d'hôte IPv6 (AAAA) pour DNS64 en utilisant la commande Windows PowerShell suivante :   **Set-NetDnsTransitionConfiguration -OnlySendAQuery $false**.  
  
### <a name="bkmk_force"></a>1.1.3 planifier le tunneling forcé  
Avec IPv6 et la table de stratégie de résolution de noms (NRPT), par défaut, les clients DirectAccess séparent leur trafic intranet et Internet comme suit :  
  
-   Les requêtes de noms DNS pour les noms de domaine complets d'intranet et tout le trafic intranet sont échangés sur les tunnels créés avec le serveur DirectAccess ou directement avec les serveurs intranet. Le trafic Intranet des clients DirectAccess correspond au trafic IPv6.  
  
-   Les requêtes de noms DNS pour les noms de domaine complets qui correspondent aux règles d'exemption ou qui ne correspondent pas à l'espace de noms intranet et tout le trafic vers les serveurs Internet sont échangés sur l'interface physique connectée à Internet. Le trafic Internet des clients DirectAccess correspond généralement au trafic IPv4.  
  
Par opposition, quelques implémentations de réseau privé virtuel (VPN) d'accès à distance, dont notamment le client VPN, envoient par défaut la totalité de leur trafic, Internet et intranet via la connexion VPN d'accès à distance. Le trafic Internet est routé par le serveur VPN jusqu'aux serveurs proxy web IPv4 de l'intranet pour l'accès aux ressources Internet IPv4. Il est possible de séparer le trafic intranet et Internet pour les clients VPN d'accès à distance en utilisant le tunneling fractionné. Ceci implique la configuration de la table de routage IP (Internet Protocol) sur les clients VPN, afin que le trafic destiné aux emplacements intranet soit envoyé via la connexion VPN et que le trafic destiné à tous les autres emplacements soit envoyé à l'aide de l'interface physique connectée à Internet.  
  
Vous pouvez configurer des clients DirectAccess pour envoyer l'ensemble de leur trafic via les tunnels au serveur DirectAccess avec Forcer le mode tunneling. Lorsque le tunneling forcé est configuré, les clients DirectAccess détectent qu'ils sont sur Internet et suppriment leur itinéraire par défaut IPv4. À l'exception du trafic de sous-réseau local, tout le trafic envoyé par le client DirectAccess correspond au trafic IPv6 qui traverse des tunnels en direction du serveur DirectAccess.  
  
> [!IMPORTANT]  
> Si vous envisagez d'utiliser le tunneling forcé, ou si vous êtes susceptible de l'ajouter dans le futur, vous devez déployer une configuration à deux tunnels. Pour des raisons de sécurité, le tunneling forcé n'est pas pris en charge dans une configuration à un seul tunnel.  
  
L'activation de Forcer le mode tunneling a les conséquences suivantes :  
  
-   Les clients DirectAccess utilisent uniquement le protocole IP-HTTPS pour obtenir la connectivité IPv6 au serveur DirectAccess via Internet IPv4.  
  
-   Les seuls emplacements qu'un client DirectAccess peut atteindre par défaut avec le trafic IPv4 sont ceux sur son sous-réseau local. Le reste du trafic envoyé par les applications et les services qui fonctionnent sur le client DirectAccess correspond au trafic IPv6, envoyé via la connexion DirectAccess. Par conséquent, les applications IPv4 uniquement sur le client DirectAccess ne peuvent pas être utilisées pour atteindre des ressources Internet, sauf celles sur le sous-réseau local.  
  
> [!IMPORTANT]  
> Pour utiliser le tunneling forcé via DNS64 et NAT64, il convient d'implémenter la connectivité Internet IPv6. Une façon de procéder consiste à rendre le préfixe IP-HTTPS routable globalement de sorte que ipv6.msftncsi.com soit accessible sur IPv6 et que la réponse du serveur Internet aux clients IP-HTTPS puisse être retournée via le serveur DirectAccess.  
>   
> Étant donné que cela n'est pas faisable dans la plupart des cas, la meilleure solution consiste à créer des serveurs NCSI virtuels sur le réseau d'entreprise, comme suit :  
>   
> 1.  Ajoutez une entrée NRPT pour ipv6.msftncsi.com et résolvez-la par rapport à DNS64 sur un site web interne (éventuellement un site web IPv4).  
> 2.  Ajoutez une entrée NRPT pour dns.msftncsi.com et résolvez-la par rapport à un serveur DNS d'entreprise pour retourner l'enregistrement de ressource d'hôte IPv6 (AAAA) fd3e:4f5a:5b81::1. (L'utilisation de DNS64 pour envoyer uniquement les requêtes d'enregistrement de ressource d'hôte (A) pour ce nom de domaine complet peut ne pas fonctionner car il est configuré dans des déploiements IPv4 uniquement. Par conséquent, vous devez le configurer pour qu'il soit résolu directement par rapport au DNS d'entreprise.)  
  
## <a name="bkmk_ConfigFirewalls"></a>1.2 planifier la configuration requise du pare-feu  
Si le serveur DirectAccess se trouve derrière un pare-feu de périmètre, les exceptions suivantes sont requises pour le trafic d'accès à distance quand le serveur DirectAccess est sur Internet IPv4 :  
  
-   Utilisateur le trafic Teredo le port UDP (Datagram Protocol) destination 3544 entrant et port UDP source 3544 sortant.  
  
-   6to4 le trafic IP protocole 41 entrant et sortant.  
  
-   Port de destination IP-HTTPS-protocole TCP (Transmission Control) 443 et le port TCP source 443 sortant.  
  
-   Si vous déployez l'accès à distance avec une seule carte réseau et installez le serveur Emplacement réseau sur le serveur DirectAccess, vous devez également exempter le port TCP 62000.  
  
    > [!NOTE]  
    > Cette exemption est sur le serveur DirectAccess et toutes les autres exemptions sont sur le pare-feu de périmètre.  
  
Pour le trafic Teredo et 6to4, ces exceptions doivent être appliquées pour les deux adresses IPv4 publiques consécutives côté Internet sur le serveur DirectAccess. Pour IP-HTTPS, les exceptions doivent être appliquées sur l'adresse enregistrée sur le serveur DNS public.  
  
Les exceptions suivantes sont requises pour le trafic d'accès à distance lorsque le serveur DirectAccess est sur Internet IPv6 :  
  
-   ID de protocole IP 50  
  
-   Port UDP de destination 500 entrant et port UDP source 500 sortant  
  
-   Trafic ICMPv6 entrant et sortant (uniquement lors de l'utilisation de Teredo)  
  
Lorsque vous utilisez des pare-feu supplémentaires, appliquez les exceptions de pare-feu de réseau interne suivantes pour le trafic d'accès à distance :  
  
-   ISATAP : protocole 41 entrant et sortant  
  
-   TCP/UDP pour tout le trafic IPv4 et IPv6  
  
-   ICMP pour tout le trafic IPv4 et IPv6 (uniquement lors de l'utilisation de Teredo)  
  
## <a name="bkmk_12CAsandCerts"></a>1.3 planifier la configuration requise des certificats  
Il existe trois scénarios qui requièrent des certificats lorsque vous déployez un serveur DirectAccess unique :  
  
-   [1.3.1 planifier des certificats d’ordinateur pour l’authentification IPsec](#BKMK_compcert)  
  
    En matière de certificats, les exigences pour IPsec incluent un certificat d'ordinateur utilisé par les ordinateurs clients DirectAccess pour établir la connexion IPsec entre le client et le serveur DirectAccess, ainsi qu'un certificat d'ordinateur utilisé par les serveurs DirectAccess pour établir des connexions IPsec avec les clients DirectAccess.  
  
    L’utilisation de ces certificats IPsec n’est pas obligatoire pour DirectAccess dans Windows Server 2012. Le serveur DirectAccess peut également agir en tant que proxy Kerberos pour effectuer l'authentification IPsec sans exiger de certificats. Si le protocole Kerberos est utilisé, il fonctionne via SSL et le proxy Kerberos utilise le certificat configuré pour IP-HTTPS à cet effet. Certains scénarios d'entreprise (y compris l'authentification client par déploiement multisite et mot de passe à usage unique) requièrent l'utilisation de l'authentification par certificat et non pas du protocole Kerberos.  
  
-   [1.3.2 planifier des certificats pour IP-HTTPS](#bkmk_iph)  
  
    Quand vous configurez l'accès à distance, le serveur DirectAccess est automatiquement configuré pour agir en tant qu'écouteur IP-HTTPS. Le site IP-HTTPS requiert un certificat de site web. Les ordinateurs clients doivent être en mesure de contacter le site de la liste de révocation de certificats correspondant.  
  
-   [1.3.3 planifier des certificats de site Web pour le serveur emplacement réseau](#bkmk_webnlc)  
  
    Le serveur Emplacement réseau est un site web utilisé pour détecter si les ordinateurs clients se trouvent dans le réseau d'entreprise. Le serveur emplacement réseau requiert un certificat de site Web. Pour ce certificat, les clients DirectAccess doivent pouvoir contacter le site contenant la liste de révocation de certificats.  
  
Les exigences liées à l'autorité de certification sont résumées pour chaque scénario dans le tableau ci-dessous.  
  
|Authentification IPsec|Serveur IP-HTTPS|Serveur Emplacement réseau|  
|------------|----------|--------------|  
|Une autorité de certification interne est requise pour émettre les certificats d’ordinateur pour le serveur DirectAccess et les clients pour l’authentification IPsec quand vous n’utilisez pas le proxy Kerberos pour l’authentification|Autorité de certification interne :<br /><br />Vous pouvez utiliser une autorité de certification interne pour émettre le certificat IP-HTTPS. Toutefois, vous devez vous assurer que le point de distribution de la liste de révocation de certificats est disponible en externe.|Autorité de certification interne :<br /><br />Vous pouvez utiliser une autorité de certification interne pour émettre le certificat de site web du serveur Emplacement réseau. Assurez-vous que le point de distribution de la liste de révocation de certificats présente une haute disponibilité à partir du réseau interne.|  
||Certificat auto-signé :<br /><br />Vous pouvez utiliser un certificat auto-signé pour le serveur IP-HTTPS. Toutefois, vous devez vous assurer que le point de distribution de la liste de révocation de certificats est disponible en externe.<br /><br />Un certificat auto-signé n'est pas utilisable dans des déploiements multisites.|Certificat auto-signé :<br /><br />Vous pouvez utiliser un certificat auto-signé pour le site web du serveur Emplacement réseau.<br /><br />Un certificat auto-signé n'est pas utilisable dans des déploiements multisites.|  
||**Recommandé**<br /><br />Autorité de certification publique :<br /><br />Il est recommandé d'utiliser une autorité de certification publique pour émettre le certificat IP-HTTPS. Cela garantit que le point de distribution de la liste de révocation de certificats est disponible en externe.|  
  
### <a name="BKMK_compcert"></a>1.3.1 planifier des certificats d’ordinateur pour l’authentification IPsec  
Si vous utilisez l'authentification IPsec basée sur les certificats, le serveur et les clients DirectAccess doivent obtenir un certificat d'ordinateur. La façon la plus simple d'installer les certificats est de configurer une inscription automatique basée sur une stratégie de groupe pour les certificats d'ordinateur. Cela garantit que tous les membres du domaine obtiendront un certificat à partir d'une autorité de certification d'entreprise. Si vous n'avez pas d'entreprise définis dans votre organisation, consultez[Active Directory Certificate Services](https://technet.microsoft.com/library/cc770357.aspx).  
  
Ce certificat présente les exigences suivantes :  
  
-   Le certificat doit bénéficier d'une utilisation améliorée de la clé d'authentification client.  
  
-   Le certificat client et le certificat serveur doivent être liés au même certificat racine. Ce certificat racine doit être sélectionné dans les paramètres de configuration DirectAccess.  
  
### <a name="bkmk_iph"></a>1.3.2 planifier des certificats pour IP-HTTPS  
Le serveur DirectAccess se comporte comme un écouteur IP-HTTPS, et vous devez installer manuellement un certificat de site web HTTPS sur ce serveur. Tenez compte des éléments suivants quand vous planifiez des certificats :  
  
-   Il est recommandé d'utiliser une autorité de certification publique, afin que les listes de révocation de certificats soient rapidement disponibles.  
  
-   Dans le champ **Objet**, spécifiez l'adresse IPv4 de la carte Internet du serveur DirectAccess ou le nom de domaine complet (FQDN) de l'URL IP-HTTPS (l'adresse ConnectTo). Si le serveur DirectAccess se trouve derrière un périphérique NAT, vous devez spécifier le nom public ou l'adresse du périphérique NAT.  
  
-   Le nom commun du certificat doit correspondre au nom du site IP-HTTPS.  
  
-   Pour le champ **Utilisation améliorée de la clé**, utilisez l'identificateur d'objet (OID) d'authentification du serveur.  
  
-   Pour le champ **Points de distribution de la liste de révocation de certificats**, indiquez un point de distribution de liste de révocation de certificats accessible par les clients DirectAccess connectés à Internet  
  
-   Le certificat IP-HTTPS doit avoir une clé privée.  
  
-   Le certificat IP-HTTPS doit être importé directement dans le magasin personnel.  
  
-   Les certificats IP-HTTPS peuvent utiliser des caractères génériques dans le nom.  
  
Si vous envisagez d'utiliser IP-HTTPS sur un port non standard, procédez comme suit sur le serveur DirectAccess :  
  
1.  Supprimez la liaison de certificat existante pour 0.0.0.0:443 et remplacez-la par une liaison de certificat pour le port que vous avez choisi. Dans le cadre de cet exemple, le port 44500 est utilisé. Avant de supprimer la liaison de certificat, affichez et copiez **appid**.  
  
    1.  Pour supprimer la liaison de certificat, entrez :  
  
        ```  
        netsh http delete ssl ipport=0.0.0.0:443  
        ```  
  
    2.  Pour ajouter la nouvelle liaison de certificat, entrez :  
  
        ```  
        netsh http add ssl ipport=0.0.0.0:44500 certhash=<use the thumbprint from the DirectAccess server SSL cert> appid=<use the appid from the binding that was deleted>  
        ```  
  
2.  Pour modifier l'URL IP-HTTPS sur le serveur, entrez :  
  
    ```  
    Netsh int http set int url=https://<DirectAccess server name (for example server.contoso.com)>:44500/IPHTTPS  
    ```  
  
    ```  
    Net stop iphlpsvc & net start iphlpsvc  
    ```  
  
3.  Changez la réservation d'URL pour kdcproxy.  
  
    1.  Pour supprimer la réservation d'URL existante, entrez :  
  
        ```  
        netsh http del urlacl url=https://+:443/KdcProxy/  
        ```  
  
    2.  Pour ajouter une nouvelle réservation d'URL, entrez :  
  
        ```  
        netsh http add urlacl url=https://+:44500/KdcProxy/ sddl=D:(A;;GX;;;NS)  
        ```  
  
4.  Ajoutez ce paramètre pour que kppsvc soit à l'écoute sur le port non standard. Pour ajouter l'entrée de Registre, entrez :  
  
    ```  
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\KPSSVC\Settings /v HttpsUrlGroup /t REG_MULTI_SZ /d +:44500 /f  
    ```  
  
5.  Pour redémarrer le service kdcproxy sur le contrôleur de domaine, entrez :  
  
    ```  
    net stop kpssvc & net start kpssvc  
    ```  
  
Pour utiliser IP-HTTPS sur un port non standard, procédez comme suit sur le contrôleur de domaine :  
  
1.  Modifiez le paramètre IP-HTTPS dans l'objet de stratégie de groupe client.  
  
    1.  Ouvrez l'Éditeur d'objets de stratégie de groupe.  
  
    2.  Accédez à Configuration ordinateur => Stratégies => Modèles d'administration => Réseau => Paramètres TCPIP => Technologies de transition IPv6.  
  
    3.  Ouvrez le paramètre d’état IP-HTTPS et remplacez l'URL par **https://<nom serveur DirectAccess (par exemple, serveur.contoso.com)>:44500/IPHTTPS**.  
  
    4.  Cliquez sur **Appliquer**.  
  
2.  Modifiez les paramètres client du proxy Kerberos dans l'objet de stratégie de groupe client.  
  
    1.  Dans l'Éditeur d'objets de stratégie de groupe, accédez à Configuration ordinateur => Stratégies => Modèles d'administration => Système => Kerberos => Spécifiez les serveurs proxy KDC pour les clients Kerberos.  
  
    2.  Ouvrez le paramètre d’état IPHTTPS et remplacez l'URL par **https://<nom serveur DirectAccess (par exemple, serveur.contoso.com)>:44500/IPHTTPS**.  
  
    3.  Cliquez sur **Appliquer**.  
  
3.  Modifiez les paramètres de stratégie IPsec client pour utiliser ComputerKerb et UserKerb.  
  
    1.  Dans l'Éditeur d'objets de stratégie de groupe, accédez à Configuration ordinateur => Stratégies => Paramètres Windows => Paramètres de sécurité => Pare-feu Windows avec fonctions avancées de sécurité.  
  
    2.  Cliquez sur **Règles de sécurité et de connexion**, puis double-cliquez sur **Règle IPsec**.  
  
    3.  Sous l'onglet **Authentification**, cliquez sur **Avancé**.  
  
    4.  Pour Auth1 : supprimez la méthode d'authentification existante et remplacez-la par ComputerKerb. Pour Auth2 : supprimez la méthode d'authentification existante et remplacez-la par UserKerb.  
  
    5.  Cliquez sur **Appliquer**, puis sur **OK**.  
  
Pour terminer le processus manuel permettant d'utiliser un port IP-HTTPS non standard, exécutez **gpupdate /force** sur les ordinateurs clients et le serveur DirectAccess.  
  
### <a name="bkmk_webnlc"></a>1.3.3 planifier des certificats de site Web pour le serveur emplacement réseau  
Lors de la planification du site web du serveur Emplacement réseau, notez les points suivants :  
  
-   Dans le champ **Objet**, indiquez l'adresse IP de l'interface intranet du serveur Emplacement réseau ou le nom de domaine complet de l'URL d'emplacement réseau.  
  
-   Dans le champ **Utilisation améliorée de la clé**, utilisez l'identificateur d'objet (OID) d'authentification du serveur.  
  
-   Dans le champ **Points de distribution de la liste de révocation des certificats**, utilisez un point de distribution de liste de révocation des certificats accessible par les clients DirectAccess connectés à l'intranet. Ce point de distribution de liste de révocation de certificats ne doit pas être accessible depuis l'extérieur du réseau interne.  
  
-   Si par la suite, vous projetez de configurer un déploiement multisite ou de cluster, le nom du certificat ne doit correspondre au nom interne d'aucun serveur DirectAccess qui sera ajouté au déploiement.  
  
    > [!NOTE]  
    > Assurez-vous que les certificats pour IP-HTTPS et le serveur Emplacement réseau ont un **Nom de sujet**. Si le certificat n'a pas de **Nom de sujet**, mais possède un **Autre nom**, il ne sera pas accepté par l'Assistant Accès à distance.  
  
## <a name="bkmk_14Dns"></a>1.4 planifier la configuration DNS requise  
Cette section explique la configuration DNS requise pour les serveurs d'infrastructure et les demandes des clients DirectAccess dans un déploiement d'accès à distance. Elle inclut les sous-sections suivantes :  
  
-   [1.4.1 planifier la configuration requise du serveur DNS](#bkmk_dnsserverrequirements)  
  
-   [1.4.2 planifier la résolution de noms locale](#bkmk_dnslocalname)  
  
**Demandes des clients DirectAccess**  
  
DNS sert à résoudre les demandes des ordinateurs clients DirectAccess qui ne se trouvent pas sur le réseau interne (ou d'entreprise). Les clients DirectAccess essaient de se connecter au serveur Emplacement réseau DirectAccess afin de déterminer s'ils se situent sur Internet ou sur le réseau interne :  
  
-   Si la connexion réussit, les clients sont identifiés comme figurant sur le réseau interne. DirectAccess n'est pas utilisé et les demandes des clients sont résolues à l'aide du serveur DNS configuré sur la carte réseau de l'ordinateur client.  
  
-   Si la connexion échoue, les clients sont considérés comme étant sur Internet. Les clients DirectAccess utilisent la table de stratégie de résolution de noms (NRPT) pour déterminer quel serveur DNS utiliser pour résoudre les demandes de noms.  
  
Vous pouvez préciser que les clients doivent utiliser DirectAccess DNS64 pour résoudre les noms, ou un autre serveur DNS interne. Pendant la résolution des noms, la table NRPT est utilisée par les clients DirectAccess pour identifier la manière de traiter une demande. Les clients demandent un nom de domaine complet ou un nom simple tel que https://internal. Si un nom en une partie est demandé, un suffixe DNS est ajouté pour créer un nom de domaine complet (FQDN). Si la requête DNS correspond à une entrée dans la table NRPT, et que DNS64 ou un serveur DNS sur le réseau interne est spécifié pour l'entrée, alors la requête est envoyée pour la résolution de noms à l'aide du serveur spécifié. S'il existe une correspondance alors qu'aucun serveur DNS n'est spécifié, celle-ci indique une règle d'exemption et la résolution de noms habituelle est appliquée.  
  
> [!NOTE]  
> Notez que quand un nouveau suffixe est ajouté à la table NRPT dans la console de gestion de l'accès à distance, vous avez la possibilité de détecter automatiquement les serveurs DNS par défaut pour le suffixe en cliquant sur **Détecter**.  
  
La détection automatique fonctionne comme suit :  
  
-   Si le réseau d'entreprise est basé sur IPv4 ou s'il utilise IPv4 et IPv6, l'adresse par défaut correspond à l'adresse DNS64 de la carte interne sur le serveur DirectAccess.  
  
-   Si le réseau d'entreprise est basé sur IPv6, l'adresse par défaut correspond à l'adresse IPv6 des serveurs DNS du réseau d'entreprise.  
  
**Serveurs d’infrastructure**  
  
-   **Serveur d’emplacement réseau**  
  
    Les clients DirectAccess essaient d'atteindre le serveur Emplacement réseau pour déterminer s'ils se situent sur le réseau interne. Les clients qui se trouvent sur le réseau interne doivent être en mesure de résoudre le nom du serveur Emplacement réseau, mais il est impératif de les empêcher de le faire quand ils se situent sur Internet. Pour garantir cela, le nom de domaine complet (FQDN) du serveur Emplacement réseau est, par défaut, ajouté en tant que règle d'exemption à la table NRPT. De plus, quand vous configurez l'accès à distance, les règles suivantes sont automatiquement créées :  
  
    -   Règle de suffixe DNS pour le domaine racine ou le nom de domaine du serveur DirectAccess et les adresses IPv6 qui correspondent à l'adresse DNS64. Dans les réseaux d'entreprise IPv6 uniquement, les serveurs DNS intranet sont configurés sur le serveur DirectAccess. Par exemple, si le serveur DirectAccess est membre du domaine corp.contoso.com, une règle est créée pour le suffixe DNS corp.contoso.com.  
  
    -   Règle d'exemption pour le nom de domaine complet du serveur d'emplacement réseau. Par exemple, si l’URL du serveur emplacement réseau est https://nls.corp.contoso.com, une règle d’exemption est créée pour le nom de domaine complet nls.corp.contoso.com.  
  
-   **Serveur IP-HTTPS**  
  
    Le serveur DirectAccess agit en tant qu'écouteur IP-HTTPS et utilise son certificat de serveur pour s'authentifier auprès des clients IP-HTTPS. Les clients DirectAccess qui utilisent des serveurs DNS publics doivent être en mesure de résoudre le nom IP-HTTPS.  
  
-   **Vérification de la révocation de révocation de certificats**  
  
    DirectAccess utilise la vérification de la révocation des certificats pour la connexion IP-HTTPS entre les clients DirectAccess et le serveur DirectAccess, ainsi que pour la connexion HTTPS entre le client DirectAccess et le serveur Emplacement réseau. Dans les deux cas, les clients DirectAccess doivent être en mesure de résoudre le point de distribution de liste de révocation de certificats et d'y accéder.  
  
-   **ISATAP**  
  
    Le protocole ISATAP permet aux ordinateurs de l'entreprise d'acquérir une adresse IPv6 et il encapsule les paquets IPv6 au sein d'un en-tête IPv4. Il est utilisé par le serveur DirectAccess pour fournir la connectivité IPv6 aux hôtes ISATAP sur un intranet. Dans un environnement réseau IPv6 non natif, le serveur DirectAccess se configure lui-même automatiquement en tant que routeur ISATAP.  
  
    Comme ISATAP n'est plus pris en charge dans DirectAccess, vous devez vous assurer que vos serveurs DNS sont configurés pour ne pas répondre aux requêtes ISATAP. Par défaut, le service Serveur DNS bloque la résolution de noms pour le nom ISATAP via la liste rouge de requêtes DNS globale. Ne supprimez pas le nom ISATAP de la liste rouge de requêtes globale.  
  
-   **Vérificateurs de connectivité**  
  
    L'accès à distance crée une sonde web par défaut utilisée par les ordinateurs clients DirectAccess pour vérifier la connectivité au réseau interne. Pour que la sonde fonctionne comme prévu, les noms suivants doivent être enregistrés manuellement dans DNS :  
  
    -   **DirectAccess-webprobehost**-doit se résoudre en adresse IPv4 interne du serveur DirectAccess, ou à l’adresse IPv6 dans un environnement IPv6 uniquement.  
  
    -   **DirectAccess-corpconnectivityhost**-doit correspondre à l’adresse d’hôte local (bouclage). Les enregistrements de ressource hôte (A) et (AAAA) doivent être créés : un enregistrement de ressource hôte (A) doté de la valeur 127.0.0.1 et un enregistrement de ressource hôte (AAAA) avec la valeur construite à partir du préfixe NAT64 avec les 32 derniers bits sous la forme 127.0.0.1. Pour récupérer le préfixe NAT64, vous pouvez exécuter la commande Windows PowerShell **get-netnattransitionconfiguration**.  
  
        > [!NOTE]  
        > Cela n'est valide que dans un environnement IPv4 uniquement. Dans un environnement IPv4 + IPv6 ou IPv6 uniquement, seul un enregistrement de ressource hôte (AAAA) doit être créé avec l'adresse IP de bouclage ::1.  
  
    Vous pouvez créer d'autres vérificateurs de connectivité en utilisant d'autres adresses web via HTTP ou **ping**. Pour chaque vérificateur de connectivité, une entrée DNS doit exister.  
  
### <a name="bkmk_dnsserverrequirements"></a>1.4.1 planifier la configuration requise du serveur DNS  
La configuration requise pour DNS lorsque vous déployez DirectAccess est présentée ci-dessous.  
  
-   Pour les clients DirectAccess, vous devez utiliser un serveur DNS exécutant Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008 ou tout autre serveur DNS qui prend en charge IPv6.  
  
    > [!NOTE]  
    > Il est déconseillé d'utiliser des serveurs DNS qui exécutent Windows Server 2003 lorsque vous déployez DirectAccess. Bien que les serveurs DNS Windows Server 2003 prennent en charge les enregistrements IPv6, Windows Server 2003 n’est plus pris en charge par Microsoft. En outre, vous ne devez pas déployer DirectAccess si vos contrôleurs de domaine exécutent Windows Server  2003 en raison d’un problème avec le service de réplication de fichiers. Pour plus d'informations, consultez [Configurations non prises en charge par DirectAccess](https://technet.microsoft.com/library/dn464274.aspx).  
  
-   Utilisez un serveur DNS qui prend en charge les mises à jour dynamiques. Vous pouvez utiliser des serveurs DNS qui ne prennent pas en charge les mises à jour dynamiques, mais vous devez mettre à jour manuellement les entrées sur ces serveurs.  
  
-   Le nom de domaine complet de vos points de distribution de liste de révocation de certificats accessibles par Internet doit pouvoir être résolu à l'aide de serveurs DNS Internet. Par exemple, si URL https://crl.contoso.com/crld/corp-DC1-CA.crl est dans le **Points de Distribution CRL** champ du certificat IP-HTTPS du serveur DirectAccess, vous devez vous assurer que le nom de domaine complet crld.contoso.com ne peut être résolu à l’aide de serveurs DNS Internet.  
  
### <a name="bkmk_dnslocalname"></a>1.4.2 planifier la résolution de noms locale  
Lorsque vous planifiez la résolution de noms locale, prenez en compte les problèmes suivants :  
  
**NRPT**  
  
Vous pouvez être amené à créer des règles NRPT supplémentaires dans les cas suivants :  
  
-   Si vous avez besoin d'ajouter des suffixes DNS pour votre espace de noms intranet.  
  
-   Si les noms de domaine complets de vos points de distribution de liste de révocation de certificats sont basés sur votre espace de noms intranet, vous devez ajouter des règles d'exemption pour les noms de domaine complets des points de distribution de liste de révocation de certificats.  
  
-   Si vous avez un environnement DNS « split brain », vous devez ajouter des règles d'exemption pour les noms des ressources pour lesquelles vous souhaitez que les clients DirectAccess se trouvant sur Internet puissent accéder à la version Internet, plutôt qu'à la version intranet.  
  
-   Si vous redirigez le trafic vers un site web externe via vos serveurs proxy web intranet, le site web externe est disponible uniquement à partir de l'intranet et il utilise les adresses de vos serveurs proxy web pour autoriser les demandes entrantes. Dans ce cas, ajoutez une règle d'exemption pour le nom de domaine complet du site web externe et spécifiez que la règle utilise votre serveur proxy web intranet, plutôt que les adresses IPv6 des serveurs DNS intranet.  
  
    Par exemple, si vous testez un site web externe nommé test.contoso.com, ce nom ne peut pas être résolu via les serveurs DNS Internet, mais le serveur proxy web de Contoso sait comment résoudre le nom et diriger les demandes pour le site web vers le serveur web externe. Pour empêcher que les utilisateurs qui ne trouvent pas sur l'intranet Contoso puissent accéder au site, le site web externe autorise uniquement les demandes provenant de l'adresse Internet IPv4 du proxy web Contoso. Par conséquent, les utilisateurs de l'intranet peuvent accéder au site web car ils utilisent le proxy web Contoso, ce qui n'est pas le cas des utilisateurs DirectAccess. En configurant une règle d'exemption NRPT pour test.contoso.com qui utilise le proxy web Contoso, les demandes de page web pour test.contoso.com sont routées vers le serveur proxy web intranet via le réseau Internet IPv4.  
  
**Noms d’étiquette unique**  
  
Unique des noms d’étiquette, tel que https://paycheck, sont parfois utilisés pour les serveurs intranet. Si un nom en une partie est demandé et qu'une liste de recherche de suffixe DNS est configurée, les suffixes DNS de la liste sont ajoutés au nom en une partie. Par exemple, lorsqu’un utilisateur sur un ordinateur qui est membre du domaine corp.contoso.com tape https://paycheck dans le navigateur web, le nom de domaine complet qui est créée en tant que le nom est paycheck.corp.contoso.com. Par défaut, le suffixe ajouté est basé sur le suffixe DNS principal de l'ordinateur client.  
  
> [!NOTE]  
> Dans un scénario d'espace de noms dissocié (où un ou plusieurs ordinateurs de domaine ont un suffixe DNS qui ne correspond pas au domaine Active Directory auquel les ordinateurs appartiennent), vous devez vous assurer que la liste de recherche est personnalisée pour inclure tous les suffixes requis. Par défaut, l'Assistant Accès à distance configurera le nom DNS Active Directory comme suffixe DNS principal sur le client. Assurez-vous d'ajouter le suffixe DNS utilisé par les clients pour la résolution de noms.  
  
Si plusieurs domaines et Windows Internet Name Service (WINS) sont déployés dans votre organisation et que vous vous connectez à distance, les noms en une partie peuvent être résolus comme suit :  
  
-   Déployez une zone de recherche directe WINS dans DNS. Lorsque vous essayez de résoudre computername.dns.zone1.corp.contoso.com, la demande est dirigée vers le serveur WINS qui utilise uniquement computername. Le client pense qu'il émet un enregistrement de ressource hôte (A) DNS standard, mais il s'agit en fait d'une demande NetBIOS. Pour plus d'informations, voir Gestion d'une zone de recherche directe.  
  
-   Ajoutez un suffixe DNS, tel que dns.zone1.corp.contoso.com, à l'objet de stratégie de groupe de la stratégie de domaine par défaut.  
  
**DNS « split brain »**  
  
Le DNS « split brain » désigne l'utilisation du même domaine DNS pour la résolution des noms Internet et intranet.  
  
Pour les déploiements DNS « split brain », vous devez répertorier les noms de domaines complets dupliqués sur Internet et intranet et décider quelles ressources le client DirectAccess doit atteindre l’intranet ou la version d’Internet. Pour chaque nom qui correspond à une ressource pour laquelle vous souhaitez que les clients DirectAccess atteignent la version Internet, vous devez ajouter le nom de domaine complet correspondant en tant que règle d'exemption à la table NRPT pour vos clients DirectAccess.  
  
Dans un environnement DNS « split brain », si vous souhaitez que les deux versions de la ressource soient disponibles, configurez vos ressources intranet avec d'autres noms qui ne sont pas des doublons des noms déjà utilisés sur Internet, et indiquez à vos utilisateurs d'utiliser l'autre nom lorsqu'ils se trouvent sur le réseau intranet. Par exemple, configurez l'autre nom www.internal.contoso.com pour le nom interne www.contoso.com.  
  
Dans un environnement DNS non « split brain », l'espace de noms Internet est différent de l'espace de noms intranet. Par exemple, la société Contoso Corporation utilise contoso.com sur Internet et corp.contoso.com sur l'intranet. Étant donné que toutes les ressources intranet utilisent le suffixe DNS corp.contoso.com, la règle NRPT pour corp.contoso.com dirige toutes les requêtes de nom DNS pour les ressources intranet vers les serveurs DNS intranet. Les requêtes de nom DNS pour les noms avec le suffixe contoso.com ne correspondent pas à la règle d'espace de noms intranet corp.contoso.com dans la table NRPT et sont envoyées aux serveurs DNS Internet. Avec un déploiement DNS non « split brain », étant donné qu'il n'y a aucune duplication de noms de domaines complets pour les ressources intranet et Internet, aucune configuration supplémentaire n'est requise pour la table NRPT. Les clients DirectAccess peuvent accéder à la fois aux ressources Internet et intranet de leur organisation.  
  
**Comportement de résolution de noms locale pour les clients DirectAccess**  
  
Si un nom ne peut pas être résolu avec DNS, pour résoudre le nom du sous-réseau local, le service Client DNS dans Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows 8, et Windows 7 permettent la résolution de noms locale, avec l’élément Link-Local Multicast nom R esolution (LLMNR) et NetBIOS sur les protocoles TCP/IP.  
  
La résolution de noms locale est généralement nécessaire pour la connectivité de réseau pair à pair lorsque l'ordinateur se trouve sur des réseaux privés, tels que des réseaux domestiques à sous-réseau unique. Lorsque le service Client DNS exécute la résolution de noms locale pour des noms de serveur intranet et que l'ordinateur est connecté sur Internet à un sous-réseau partagé, les utilisateurs malveillants peuvent capturer LLMNR et NetBIOS sur des messages TCP/IP afin de déterminer des noms de serveur intranet. Dans la page DNS de l'Assistant Installation du serveur d'infrastructure, vous configurez le comportement de résolution de noms locale en fonction des types de réponses reçus à partir des serveurs DNS intranet. Les options suivantes sont disponibles :  
  
-   **Utiliser la résolution de noms locale si le nom n'existe pas dans DNS**. Cette option est la plus sécurisée car le client DirectAccess exécute uniquement la résolution de noms locale pour les noms de serveur qui ne peuvent pas être résolus par les serveurs DNS intranet. Si les serveurs DNS intranet sont accessibles, les noms des serveurs intranet sont résolus. S'ils ne sont pas accessibles ou que d'autres types d'erreurs DNS surviennent, les noms de serveur intranet ne seront pas révélés sur le sous-réseau via la résolution de noms locale.  
  
-   **Utiliser la résolution de noms locale si le nom n'existe pas dans DNS ou si les serveurs DNS ne sont pas accessibles lorsque l'ordinateur client se trouve sur un réseau privé (recommandé)**. Cette option est recommandée car elle autorise l'utilisation de la résolution de noms locale sur un réseau privé uniquement lorsque les serveurs DNS intranet sont inaccessibles.  
  
-   **Utiliser la résolution de noms locale pour toute erreur de résolution DNS (le moins restrictif)**. Il s'agit de l'option la moins sécurisée car les noms de serveurs réseau intranet peuvent être révélés au sous-réseau local via la résolution de noms locale.  
  
## <a name="bkmk_14NLS"></a>1.5 planifier le serveur emplacement réseau  
Le serveur Emplacement réseau est un site web utilisé pour détecter si les clients DirectAccess se trouvent dans le réseau d'entreprise. Les clients figurant sur le réseau d'entreprise n'utilisent pas DirectAccess pour atteindre les ressources internes mais se connectent directement.  
  
Le site web du serveur Emplacement réseau peut être hébergé sur le serveur DirectAccess ou sur un autre serveur dans votre organisation. Si vous hébergez le serveur Emplacement réseau sur le serveur DirectAccess, le site web est créé automatiquement lorsque vous installez le rôle serveur d'accès à distance. Si vous hébergez le serveur Emplacement réseau ou un autre serveur dans votre organisation exécutant un système d'exploitation Windows, vous devez vous assurer qu'Internet Information Services (IIS) est installé sur ce serveur et que le site web est créé. DirectAccess ne configure pas les paramètres sur un serveur Emplacement réseau distant.  
  
Assurez-vous que le site web du serveur Emplacement réseau respecte les exigences suivantes :  
  
-   Il s'agit d'un site web doté d'un certificat de serveur HTTPS.  
  
-   Les ordinateurs clients DirectAccess doivent faire confiance à l'autorité de certification qui a émis le certificat de serveur pour le site web du serveur Emplacement réseau.  
  
-   Les ordinateurs clients DirectAccess figurant sur le réseau interne doivent être en mesure de résoudre le nom du site du serveur Emplacement réseau.  
  
-   Le site du serveur Emplacement réseau doit disposer d'une haute disponibilité pour les ordinateurs figurant sur le réseau interne.  
  
-   Le serveur Emplacement réseau ne doit pas être accessible aux ordinateurs clients DirectAccess figurant sur Internet.  
  
-   Le certificat de serveur doit être vérifié par rapport à une liste de révocation de certificats.  
  
### <a name="151-plan-certificates-for-the-network-location-server"></a>1.5.1 Planifier des certificats pour le serveur Emplacement réseau  
Quand vous obtenez le certificat de site web à utiliser pour le serveur Emplacement réseau, prenez en compte les éléments suivants :  
  
1.  Dans le champ **Objet**, spécifiez une adresse IP de l'interface intranet du serveur Emplacement réseau ou le nom de domaine complet de l'URL d'emplacement réseau.  
  
2.  Dans le champ **Utilisation améliorée de la clé**, utilisez l'identificateur d'objet (OID) d'authentification du serveur.  
  
3.  Dans le champ **Points de distribution de la liste de révocation des certificats**, utilisez un point de distribution de liste de révocation des certificats accessible par les clients DirectAccess connectés à l'intranet. Ce point de distribution de liste de révocation de certificats ne doit pas être accessible depuis l'extérieur du réseau interne.  
  
### <a name="152-plan-dns-for-the-network-location-server"></a>1.5.2 Planifier DNS pour le serveur Emplacement réseau  
Les clients DirectAccess essaient d'atteindre le serveur Emplacement réseau pour déterminer s'ils se situent sur le réseau interne. Les clients qui se trouvent sur le réseau interne doivent être en mesure de résoudre le nom du serveur Emplacement réseau, mais il est impératif de les empêcher de le faire quand ils se situent sur Internet. Pour garantir cela, le nom de domaine complet (FQDN) du serveur Emplacement réseau est, par défaut, ajouté en tant que règle d'exemption à la table NRPT.  
  
## <a name="bkmk_15mgmtservers"></a>1.6 planifier les serveurs d’administration  
Les clients DirectAccess initient une communication avec les serveurs d'administration qui fournissent des services tels que Windows Update et les mises à jour d'antivirus. Les clients DirectAccess utilisent également le protocole Kerberos pour contacter les contrôleurs de domaine afin de s'authentifier avant d'accéder au réseau interne. Au cours de l'administration distante des clients DirectAccess, les serveurs d'administration communiquent avec les ordinateurs clients pour exécuter des fonctions d'administration telles que l'évaluation de l'inventaire logiciel et matériel. L'accès à distance peut détecter automatiquement certains serveurs d'administration, dont notamment :  
  
-   Domaine contrôleurs-la détection automatique des contrôleurs de domaine est effectuée pour tous les domaines dans la même forêt que le serveur DirectAccess et les ordinateurs clients.  
  
-   System Center Configuration Manager serveurs-la détection automatique des serveurs System Center Configuration Manager est effectuée pour tous les domaines dans la même forêt que le serveur DirectAccess et les ordinateurs clients.  
  
Les contrôleurs de domaine et les serveurs System Center Configuration Manager sont détectés automatiquement la première fois que DirectAccess est configuré. Contrôleurs de domaine détectés ne sont pas affichés dans la console, mais les paramètres peuvent être récupérées à l’aide de l’applet de commande Windows PowerShell **Get-DAMgmtServer-Type All**. Si un contrôleur de domaine ou des serveurs System Center Configuration Manager sont modifiés, cliquez sur **Actualiser les serveurs d'administration** dans la console d'administration de l'accès à distance pour actualiser la liste des serveurs d'administration.  
  
**Configuration requise du serveur Management**  
  
-   Les serveurs d'administration doivent être accessibles via le premier tunnel (infrastructure). Lorsque vous configurez l'accès à distance, l'ajout de serveurs dans la liste des serveurs d'administration les rend accessibles via ce tunnel.  
  
-   Les serveurs d'administration qui établissent des connexions avec les clients DirectAccess doivent prendre entièrement en charge IPv6, en utilisant une adresse IPv6 native ou une adresse attribuée par ISATAP.  
  
## <a name="bkmk_16AD"></a>1.7 planifier Active Directory Domain Services  
Cette section explique comment DirectAccess utilise les services de domaine Active Directory (AD DS), et elle inclut les sous-sections suivantes :  
  
-   [1.7.1 planifier l’authentification client](#bkmk_clientauth)  
  
-   [1.7.2 planifier plusieurs domaines](#bkmk_multiple)  
  
DirectAccess utilise AD DS et groupes Active Directory des objets de stratégie (GPO) comme suit :  
  
-   **Authentification**  
  
    Les services de domaine Active Directory sont utilisés pour l'authentification. Le tunnel d'infrastructure utilise l'authentification NTLMv2 pour le compte d'ordinateur qui se connecte au serveur DirectAccess et le compte doit être répertorié dans un domaine Active Directory. Le tunnel intranet utilise l'authentification Kerberos pour que l'utilisateur crée le second tunnel.  
  
-   **Objets de stratégie de groupe**  
  
    DirectAccess regroupe les paramètres de configuration dans des objets de stratégie de groupe (GPO) qui sont appliqués aux serveurs DirectAccess, aux clients et aux serveurs d'applications internes.  
  
-   **Groupes de sécurité**  
  
    DirectAccess utilise les groupes de sécurité pour regrouper et identifier les ordinateurs clients DirectAccess. Les objets GPO sont appliqués au groupe de sécurité requis.  
  
-   **Stratégies IPsec étendues**  
  
    DirectAccess peut utiliser l'authentification et le chiffrement IPsec entre les clients et le serveur DirectAccess. Vous pouvez étendre l'authentification et le chiffrement IPsec à partir du client jusqu'aux serveurs d'applications internes spécifiés. Pour cela, ajoutez les serveurs d'applications requis dans un groupe de sécurité.  
  
**Requise pour AD DS**  
  
Lorsque vous planifiez d'utiliser les services de domaine Active Directory pour un déploiement DirectAccess, prenez en compte les exigences suivantes :  
  
-   Au moins un contrôleur de domaine doit être installé avec le système d’exploitation Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008.  
  
    Si le contrôleur de domaine se trouve sur un réseau de périmètre (et est donc accessible depuis la carte réseau Internet du serveur DirectAccess), vous devez empêcher le serveur DirectAccess d'y accéder en ajoutant des filtres de paquets sur le contrôleur de domaine, pour empêcher toute connectivité à l'adresse IP de la carte Internet.  
  
-   Le serveur DirectAccess doit être membre d'un domaine.  
  
-   Les clients DirectAccess doivent appartenir au domaine. Les clients peuvent appartenir :  
  
    -   à tout domaine inclus dans la même forêt que le serveur DirectAccess ;  
  
    -   à tout domaine doté d'une approbation bidirectionnelle avec le domaine du serveur DirectAccess ;  
  
    -   à tout domaine inclus dans une forêt dotée d'une approbation bidirectionnelle avec la forêt à laquelle le domaine DirectAccess appartient.  
  
> [!NOTE]  
> -   Le serveur DirectAccess ne peut pas être un contrôleur de domaine.  
> -   Le contrôleur de domaine AD DS utilisé pour DirectAccess ne doit pas être accessible à partir de la carte Internet externe du serveur DirectAccess (c'est-à-dire que la carte ne doit pas figurer dans le profil de domaine du Pare-feu Windows).  
  
### <a name="bkmk_clientauth"></a>1.7.1 planifier l’authentification client  
DirectAccess vous permet de choisir entre l'utilisation de certificats pour l'authentification IPsec des ordinateurs et l'utilisation d'un proxy Kerberos intégré qui effectue l'authentification en utilisant les noms d'utilisateur et les mots de passe.  
  
Si vous choisissez d'utiliser les informations d'identification AD DS pour l'authentification, DirectAccess utilise un tunnel de sécurité unique qui utilise la clé Kerberos de l'ordinateur pour la première authentification et la clé Kerberos de l'utilisateur pour la seconde. Quand vous utilisez ce mode pour l'authentification, DirectAccess utilise un tunnel de sécurité unique qui fournit l'accès au serveur DNS, au contrôleur de domaine et aux autres serveurs sur le réseau interne.  
  
Si vous choisissez d'utiliser une authentification à deux facteurs ou la protection d'accès réseau (NAP), DirectAccess utilise deux tunnels de sécurité. L'Assistant Configuration de l'accès à distance configure des règles de sécurité de connexion de Pare-feu Windows avec fonctions avancées de sécurité, qui spécifient l'utilisation des types suivants d'informations d'identification durant la négociation des associations de sécurité IPsec pour les tunnels au serveur DirectAccess :  
  
-   Le tunnel d'infrastructure utilise les informations d'identification de la clé Kerberos de l'ordinateur pour la première authentification et de la clé Kerberos de l'utilisateur pour la seconde.  
  
-   Le tunnel intranet utilise les informations d'identification du certificat d'ordinateur pour la première authentification et de la clé Kerberos de l'utilisateur pour la seconde.  
  
Lorsque DirectAccess choisit d’autoriser l’accès aux clients qui exécutent Windows 7 ou dans un déploiement multisite, il utilise deux tunnels de sécurité. L'Assistant Configuration de l'accès à distance configure des règles de sécurité de connexion de Pare-feu Windows avec fonctions avancées de sécurité, qui spécifient l'utilisation des types suivants d'informations d'identification durant la négociation des associations de sécurité IPsec pour les tunnels au serveur DirectAccess :  
  
-   Le tunnel d'infrastructure utilise les informations d'identification du certificat d'ordinateur pour la première authentification et NTLMv2 pour la seconde. Les informations d'identification NTLMv2 forcent l'utilisation du protocole AuthIP (Authenticated Internet Protocol) et fournissent l'accès à un serveur DNS et contrôleur de domaine avant que le client DirectAccess puisse utiliser les informations d'identification Kerberos pour le tunnel intranet.  
  
-   Le tunnel intranet utilise les informations d'identification du certificat d'ordinateur pour la première authentification et de la clé Kerberos de l'utilisateur pour la seconde.  
  
### <a name="bkmk_multiple"></a>1.7.2 planifier plusieurs domaines  
La liste des serveurs d'administration doit inclure les contrôleurs de domaine de tous les domaines qui contiennent des groupes de sécurité qui incluent des ordinateurs clients DirectAccess. Elle doit contenir tous les domaines qui contiennent des comptes d'utilisateur susceptibles d'utiliser les ordinateurs qui sont configurés en tant que clients DirectAccess. Cela garantit que des utilisateurs qui ne se trouvent pas dans le même domaine que l'ordinateur client qu'ils utilisent seront authentifiés auprès d'un contrôleur de domaine dans le domaine de l'utilisateur. Cela s'effectue automatiquement si les domaines figurent dans une même forêt.  
  
> [!NOTE]  
> Si des ordinateurs figurant dans les groupes de sécurité sont utilisés pour des ordinateurs clients ou des serveurs d'applications dans d'autres forêts, les contrôleurs de domaine de ces forêts ne sont pas détectés automatiquement. Vous pouvez exécuter la tâche **Actualiser les serveurs d'administration** dans la console d'administration de l'accès à distance pour détecter ces contrôleurs de domaine.  
  
Dans la mesure du possible, les suffixes de noms de domaine communs doivent être ajoutés dans la table de stratégie de résolution de noms pendant le déploiement de l'accès à distance. Par exemple, si vous possédez deux domaines, domain1.corp.contoso.com et domain2.corp.contoso.com, au lieu d'ajouter deux entrées dans la table NRPT, vous pouvez ajouter une entrée de suffixe DNS commun, où le suffixe de nom de domaine est corp.contoso.com. Cela s'effectue automatiquement pour des domaines dans une même racine, mais des domaines qui ne figurent pas dans une même racine doivent être ajoutés manuellement.  
  
Si le service WINS (Windows Internet Name Service) est déployé dans un environnement à plusieurs domaines, vous devez déployer une zone de recherche directe WINS dans DNS. Pour plus d’informations, consultez **unique des noms d’étiquette** dans le [1.4.2 planifier la résolution de noms locale](#bkmk_dnslocalname) section plus haut dans ce document.  
  
## <a name="bkmk_17GPOs"></a>1.8 les objets de stratégie de groupe de plan de  
Cette section explique le rôle des objets de stratégie de groupe (GPO) dans votre infrastructure d'accès à distance, et elle inclut les sous-sections suivantes :  
  
-   [1.8.1 configurer les objets stratégie de groupe créés automatiquement](#bkmk_autoGPO)  
  
-   [1.8.2 configurer la stratégie de groupe créés manuellement](#bkmk_manualGPO)  
  
-   [1.8.3 administrer les objets stratégie de groupe dans un environnement de plusieurs contrôleurs de domaine](#bkmk_multiDC)  
  
-   [1.8.4 administrer les GPO de l’accès à distance avec des autorisations limitées](#bkmk_manageGPO)  
  
-   [1.8.5 récupérer à partir d’un objet de stratégie de groupe supprimé](#bkmk_delGPO)  
  
Les paramètres DirectAccess configurés quand vous configurez l'accès à distance sont rassemblés dans des objets de stratégie de groupe. Les types suivants d'objets de stratégie de groupe sont renseignés avec les paramètres DirectAccess, puis distribués comme suit :  
  
-   **Client DirectAccess GPO**  
  
    Cet objet de stratégie de groupe contient des paramètres client, notamment les paramètres de technologie de transition IPv6, les entrées de la table NRPT et les règles de sécurité de connexion du Pare-feu Windows avec fonctions avancées de sécurité. Il est appliqué aux groupes de sécurité spécifiés pour les ordinateurs clients.  
  
-   **Serveur DirectAccess GPO**  
  
    Cet objet de stratégie de groupe contient les paramètres de configuration DirectAccess qui sont appliqués à tout serveur configuré en tant que serveur DirectAccess dans votre déploiement. Il contient également les règles de sécurité de connexion du Pare-feu Windows avec fonctions avancées de sécurité.  
  
-   **Serveurs d’applications GPO**  
  
    Cet objet de stratégie de groupe contient les paramètres des serveurs d'applications sélectionnés auxquels vous étendez éventuellement l'authentification et le chiffrement à partir des clients DirectAccess. Si l'authentification et le chiffrement ne sont pas étendus, cet objet de stratégie de groupe n'est pas utilisé.  
  
Vous pouvez configurer les objets de stratégie de groupe de deux manières :  
  
-   **Automatiquement**-vous pouvez spécifier qu’ils sont créés automatiquement. Un nom par défaut est spécifié pour chaque objet de stratégie de groupe.  
  
-   **Manuellement**-vous pouvez utiliser des objets de stratégie de groupe qui ont été prédéfinis par l’administrateur Active Directory.  
  
> [!NOTE]  
> Une fois que DirectAccess a été configuré pour utiliser des objets de stratégie de groupe spécifiques, il ne peut plus être configuré pour en utiliser d'autres.  
  
Que vous utilisiez des objets de stratégie de groupe configurés automatiquement ou manuellement, vous devez ajouter une stratégie pour la détection des liaisons lentes si vos clients utilisent des réseaux 3G. Le chemin d’accès pour **stratégie : Configurer la détection de liaison lente de stratégie de groupe** est : **Computer configuration/Policies/Administrative Templates/System/Group Policy**.  
  
> [!CAUTION]  
> Utilisez la procédure suivante pour sauvegarder tous les objets de stratégie de groupe d'accès à distance avant d'exécuter des applets de commande DirectAccess : [Sauvegarder et restaurer la configuration d’Accès à distance](https://go.microsoft.com/fwlink/?LinkID=257928).  
  
Si les autorisations appropriées (répertoriées dans les sections suivantes) pour lier des objets de stratégie de groupe n'existent pas, un avertissement est émis. L'accès à distance continue de fonctionner mais aucune liaison ne se produit. Si cet avertissement est émis, les liens ne sont pas créés automatiquement, même si les autorisations sont ajoutées plus tard. L'administrateur doit créer les liens manuellement.  
  
### <a name="bkmk_autoGPO"></a>1.8.1 configurer les objets stratégie de groupe créés automatiquement  
Prenez en compte les points suivants quand vous utilisez des objets de stratégie de groupe créés automatiquement.  
  
Les objets de stratégie de groupe créés automatiquement sont appliqués en fonction du paramètre d'emplacement et du paramètre de cible du lien, de la façon suivante :  
  
-   Pour l'objet de stratégie de groupe de serveur DirectAccess, les paramètres d'emplacement et de lien pointent sur le domaine qui contient le serveur DirectAccess.  
  
-   Quand les objets de stratégie de groupe de serveur d'applications et de client sont créés, l'emplacement est défini sur un domaine unique dans lequel l'objet de stratégie de groupe est créé. Le nom de l'objet de stratégie de groupe est recherché dans chaque domaine, puis complété avec les paramètres DirectAccess s'il existe. La cible du lien est définie à la racine du domaine dans lequel l'objet de stratégie de groupe a été créé. Un objet de stratégie de groupe est créé pour chaque domaine qui contient des ordinateurs clients ou des serveurs d'applications, et il est lié à la racine de son domaine respectif.  
  
Quand vous utilisez des objets de stratégie de groupe créés automatiquement pour appliquer des paramètres DirectAccess, l'administrateur de l'accès à distance requiert les autorisations suivantes :  
  
-   autorisations de créer des objets de stratégie de groupe pour chaque domaine ;  
  
-   autorisations de créer des liens pour toutes les racines de domaine client sélectionnées ;  
  
-   autorisations de créer des liens pour les racines de domaine des objets de stratégie de groupe de serveur.  
  
Les autorisations suivantes sont également requises :  
  
-   Autorisations de créer, modifier, supprimer et modifier la sécurité pour les objets de stratégie de groupe.  
  
-   Il est recommandé que l'administrateur de l'accès à distance possède les autorisations requises pour lire les objets de stratégie de groupe pour chaque domaine requis. Ainsi, l'accès à distance peut vérifier qu'il n'existe pas de doublons de noms d'objets de stratégie de groupe lors de la création de ces derniers.  
  
### <a name="bkmk_manualGPO"></a>1.8.2 configurer la stratégie de groupe créés manuellement  
Prenez en compte les points suivants quand vous utilisez des objets de stratégie de groupe créés manuellement :  
  
-   Les objets de stratégie de groupe doivent déjà exister quand vous exécutez l'Assistant Configuration de l'accès à distance.  
  
-   Pour appliquer les paramètres DirectAccess, l'administrateur de l'accès à distance a besoin des autorisations complètes (modification, suppression, modification de la sécurité) sur les objets de stratégie de groupe créés manuellement.  
  
-   Un lien à l'objet de stratégie de groupe est recherché dans l'ensemble du domaine. Si l'objet de stratégie de groupe n'est pas lié dans le domaine, un lien est automatiquement créé à la racine du domaine. Si les autorisations requises pour créer le lien ne sont pas disponibles, un avertissement est émis.  
  
### <a name="bkmk_multiDC"></a>1.8.3 administrer les objets stratégie de groupe dans un environnement de plusieurs contrôleurs de domaine  
Chaque objet de stratégie de groupe est administré par un contrôleur de domaine spécifique, comme suit :  
  
-   L'objet de stratégie de groupe de serveur est administré par l'un des contrôleurs de domaine figurant dans le site Active Directory qui est associé au serveur. Si les contrôleurs de domaine de ce site sont en lecture seule, l'objet de stratégie de groupe de serveur est administré par le contrôleur de domaine accessible en écriture le plus proche du serveur DirectAccess.  
  
-   Les objets de stratégie de groupe de serveur d'applications et de client sont administrés par le contrôleur de domaine qui s'exécute en tant que contrôleur de domaine principal (PDC).  
  
Pour modifier manuellement les paramètres des objets de stratégie de groupe, prenez en compte les points suivants :  
  
-   Pour l'objet de stratégie de groupe de serveur, afin d'identifier le contrôleur de domaine qui est associé au serveur DirectAccess, ouvrez une invite de commandes avec élévation de privilèges sur le serveur DirectAccess et exécutez **nltest /dsgetdc: /writable**.  
  
-   Par défaut, quand vous apportez des modifications à l'aide d'applets de commande Windows PowerShell de gestion de réseau ou quand vous apportez des modifications à partir de la console de gestion des stratégies de groupe, le contrôleur de domaine qui agit en tant que contrôleur de domaine principal est utilisé.  
  
De plus, si vous modifiez des paramètres sur un contrôleur de domaine qui n'est pas le contrôleur de domaine associé au serveur DirectAccess (pour l'objet de stratégie de groupe de serveur) ou le contrôleur de domaine principal (pour les objets de stratégie de groupe de serveur d'applications et de client), prenez en compte les points suivants :  
  
-   Avant de modifier les paramètres, assurez-vous que le contrôleur de domaine est répliqué avec un objet de stratégie de groupe à jour et sauvegardez vos paramètres GPO. Pour plus d’informations, consultez [Sauvegarder et restaurer la configuration d’Accès à distance](https://go.microsoft.com/fwlink/?LinkID=257928). Si l'objet de stratégie de groupe n'est pas mis à jour, des conflits de fusion peuvent survenir durant la réplication, ce qui peut entraîner une configuration incorrecte de l'accès à distance.  
  
-   Une fois que vous avez modifié les paramètres, vous devez attendre la réplication des modifications sur les contrôleurs de domaine associés aux objets de stratégie de groupe. N'apportez pas de modifications supplémentaires via la console de gestion de l'accès à distance ou à l'aide d'applets de commande PowerShell pour l'accès à distance tant que la réplication n'est pas terminée. Si un objet de stratégie de groupe est modifié sur deux contrôleurs de domaine avant la fin de la réplication, des conflits de fusion peuvent survenir et entraîner une configuration incorrecte de l'accès à distance.  
  
Vous pouvez également modifier le paramètre par défaut en utilisant la boîte de dialogue **Modifier le contrôleur de domaine** de la console de gestion des stratégies de groupe ou en utilisant l'applet de commande Windows PowerShell **Open-NetGPO**, afin que les modifications utilisent le contrôleur de domaine que vous spécifiez.  
  
-   Pour effectuer cette opération dans la console de gestion des stratégies de groupe, cliquez avec le bouton droit sur le domaine ou le conteneur Sites, et cliquez sur **Modifier le contrôleur de domaine**.  
  
-   Pour effectuer cette opération dans Windows PowerShell, spécifiez le paramètre **DomainController** pour l'applet de commande **Open-NetGPO**. Par exemple, pour activer les profils privé et public dans le Pare-feu Windows sur un objet de stratégie de groupe nommé domain1\DA_Server_GPO _Europe en utilisant un contrôleur de domaine nommé europe-dc.corp.contoso.com, entrez le code suivant :  
  
    ```  
    $gpoSession = Open-NetGPO -PolicyStore "domain1\DA_Server_GPO _Europe" -DomainController "europe-dc.corp.contoso.com"  
    Set-NetFirewallProfile -GpoSession $gpoSession -Name @("Private","Public") -Enabled True  
    Save-NetGPO -GpoSession $gpoSession  
    ```  
  
### <a name="bkmk_manageGPO"></a>1.8.4 administrer les GPO de l’accès à distance avec des autorisations limitées  
Pour gérer un déploiement de l'accès à distance, l'administrateur de l'accès à distance a besoin des autorisations complètes (lecture, modification, suppression et modification de la sécurité) sur les objets de stratégie de groupe qui sont utilisés dans le déploiement. Cela est dû au fait que la console de gestion de l'accès à distance et les modules PowerShell pour l'accès à distance lisent et écrivent la configuration dans les objets de stratégie de groupe d'accès à distance (objets de stratégie de groupe de client, de serveur et de serveur d'applications).  
  
Dans de nombreuses organisations, l'administrateur de domaine qui est en charge des opérations liées aux objets de stratégie de groupe n'est pas la même personne que l'administrateur de l'accès à distance en charge de la configuration de l'accès à distance. Ces organisations peuvent avoir mis en place des politiques empêchant l'administrateur de l'accès à distance d'avoir des autorisations complètes sur les objets de stratégie de groupe dans le domaine. L'administrateur de domaine peut également être tenu de passer en revue la configuration des stratégies avant de l'appliquer à des ordinateurs dans le domaine.  
  
Pour prendre en compte ces exigences, l'administrateur de domaine doit créer deux copies de chaque objet de stratégie de groupe : une copie intermédiaire et une copie de production. L'administrateur de l'accès à distance obtient des autorisations complètes sur les objets de stratégie de groupe intermédiaires. Il spécifie ces objets de stratégie de groupe intermédiaires dans la console de gestion de l'accès à distance et dans les applets de commande Windows PowerShell en tant qu'objets de stratégie de groupe utilisés pour le déploiement de l'accès à distance. Cela permet à l'administrateur de l'accès à distance de lire et modifier la configuration de l'accès à distance quand et comme cela est nécessaire.  
  
L'administrateur de domaine doit s'assurer que les objets de stratégie de groupe intermédiaires ne sont liés à aucune étendue de gestion dans le domaine et que l'administrateur de l'accès à distance ne possède pas les autorisations de créer des liens vers les objets de stratégie de groupe dans le domaine. Ceci garantit que les modifications apportées par l'administrateur de l'accès à distance aux objets de stratégie de groupe intermédiaires n'ont aucun effet sur les ordinateurs dans le domaine.  
  
L'administrateur de domaine lie les objets de stratégie de groupe de production à l'étendue de gestion requise et applique le filtrage de sécurité approprié. Cela garantit que les modifications apportées à ces objets de stratégie de groupe sont appliquées aux ordinateurs figurant dans le domaine (ordinateurs clients, serveurs DirectAccess et serveurs d'applications). L'administrateur de l'accès à distance ne possède aucune autorisation sur les objets de stratégie de groupe de production.  
  
Quand des modifications sont apportées aux objets de stratégie de groupe intermédiaires, l'administrateur de domaine peut passer en revue la configuration des stratégies dans ces objets de stratégie de groupe pour s'assurer qu'elle respecte les exigences de sécurité définies dans l'organisation. L'administrateur de domaine exporte alors les paramètres à partir des objets de stratégie de groupe intermédiaires en utilisant la fonctionnalité de sauvegarde et importe les paramètres dans les objets de stratégie de groupe de production correspondants, qui seront appliqués aux ordinateurs dans le domaine.  
  
Le diagramme suivant illustre cette configuration.  
  
![Gérer les GPO de l’accès à distance](../../../media/Step-1-Plan-the-DirectAccess-Infrastructure/DA_Plan_Advanced_Step1_GPOS.png)  
  
### <a name="bkmk_delGPO"></a>1.8.5 récupérer à partir d’un objet de stratégie de groupe supprimé  
Si un objet de stratégie de groupe de client, de serveur DirectAccess ou de serveur d'applications est supprimé par inadvertance et qu'aucune sauvegarde n'est disponible, vous devez supprimer les paramètres de configuration et les reconfigurer. Si vous disposez d'une sauvegarde, vous pouvez restaurer l'objet de stratégie de groupe.  
  
La console de gestion de l'accès à distance affiche le message d'erreur suivant : **GPO <GPO name> Impossible de trouver**. Pour supprimer les paramètres de configuration, procédez comme suit :  
  
1.  Exécutez l'applet de commande Windows PowerShell **Uninstall-remoteaccess**.  
  
2.  Ouvrez la console de gestion de l'accès à distance.  
  
3.  Un message d'erreur indique que l'objet de stratégie de groupe est introuvable. Cliquez sur **Supprimer les paramètres de configuration**. Après cela, le serveur sera rétabli à l'état Non configuré.  
  
## <a name="BKMK_Links"></a>Étape suivante  
  
-   [Étape 2 : Planifier les déploiements de DirectAccess](da-adv-plan-s2-deployments.md)  
  


