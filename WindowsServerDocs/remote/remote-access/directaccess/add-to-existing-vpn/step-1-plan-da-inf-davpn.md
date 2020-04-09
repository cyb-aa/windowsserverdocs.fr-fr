---
title: Étape 1 planifier l’infrastructure DirectAccess
description: Cette rubrique fait partie du guide ajouter DirectAccess à un déploiement d’accès à distance (VPN) existant pour Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 4ca50ea8-6987-4081-acd5-5bf9ead62acd
ms.author: lizross
author: eross-msft
ms.openlocfilehash: be4d7028f1922152b2779e82a7c78d9b3a5b753a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859552"
---
# <a name="step-1-plan-directaccess-infrastructure"></a>Étape 1 planifier l’infrastructure DirectAccess

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

La première étape de la planification d'un déploiement de base de l'accès à distance sur un serveur unique consiste à planifier l'infrastructure requise pour le déploiement. Cette rubrique décrit les étapes de planification de cette infrastructure :  
  
|Tâche|Description|  
|----|--------|  
|Planifier la topologie et les paramètres réseau|Décidez où placer le serveur d'accès à distance (à la périphérie, ou derrière un pare-feu ou un périphérique de traduction d'adresses réseau (NAT)), et planifiez l'adressage IP et le routage.|  
|Planifier la configuration requise pour le pare-feu|Planifiez l'autorisation de l'accès à distance via des pare-feu de périmètre.|  
|Planifier les exigences en matière de certificats|L'accès à distance peut utiliser Kerberos ou des certificats pour l'authentification client. Dans ce déploiement de l'accès à distance de base, Kerberos est automatiquement configuré et l'authentification est accomplie à l'aide d'un certificat auto-signé émis automatiquement par le serveur d'accès à distance.|  
|Planifier la configuration DNS requise|Planifiez les paramètres DNS pour le serveur d'accès à distance, les serveurs d'infrastructure, les options de résolution de noms locale et la connectivité client.|  
|Planifier Active Directory|Planifiez vos contrôleurs de domaine et la configuration Active Directory requise.|  
|Planifier les objets de stratégie de groupe|Déterminez quels objets de stratégie de groupe sont requis dans votre organisation et comment les créer ou les modifier.|  
  
Vous pouvez effectuer les tâches de planification dans l'ordre qui vous convient.  
  
## <a name="plan-network-topology-and-settings"></a>Planifier la topologie et les paramètres réseau  
  
### <a name="plan-network-adapters-and-ip-addressing"></a>Planifier les cartes réseau et l'adressage IP  
  
1. Identifiez la topologie des cartes réseau à utiliser. L'accès à distance peut être configuré de l'une ou l'autre des manières suivantes :  
  
    - Avec deux cartes réseau : soit à la périphérie avec une seule carte réseau connectée à Internet et l’autre au réseau interne, soit derrière un périphérique NAT, un pare-feu ou un périphérique routeur, avec une carte réseau connectée à un réseau de périmètre et l’autre au réseau interne. réseaux.  
  
    - Derrière un périphérique NAT avec une seule carte réseau : le serveur d’accès à distance est installé derrière un périphérique NAT et la carte réseau unique est connectée au réseau interne.  
  
2. Déterminez vos exigences en matière d'adressage IP :  
  
    DirectAccess utilise IPv6 avec IPsec pour créer une connexion sécurisée entre les ordinateurs clients DirectAccess et le réseau d'entreprise interne. Pourtant, DirectAccess ne requiert pas systématiquement une connectivité Internet IPv6 ni une prise en charge IPv6 native sur les réseaux internes. Au lieu de cela, il configure et utilise automatiquement des technologies de transition IPv6 pour canaliser le trafic IPv6 sur Internet IPv4 (6to4, Teredo, IP-HTTPS) et sur votre intranet IPv4 uniquement (NAT64 ou ISATAP). Pour obtenir une vue d'ensemble de ces technologies de transition, consultez les ressources suivantes :  
  
    - [Technologies de transition IPv6](https://technet.microsoft.com/library/bb726951.aspx)  
  
    - [Spécification du protocole de tunnel IP-HTTPs](https://msdn.microsoft.com/library/dd358571.aspx)  
  
3. Configurez les cartes et l'adressage requis selon le tableau suivant. Pour les déploiements derrière un périphérique NAT à l’aide d’une seule carte réseau, configurez vos adresses IP en utilisant uniquement la colonne « carte réseau interne ».  
  
    |Type d’adresse IP|Carte réseau externe|Carte réseau interne|Exigences en matière de routage|  
    |-|--------------|--------------------|------------|  
    |Internet IPv4 et intranet IPv4|Configurez les éléments suivants :<br/><br/>-Une adresse IPv4 publique statique avec les masques de sous-réseau appropriés.<br/>-Adresse IPv4 de passerelle par défaut de votre pare-feu Internet ou du routeur de votre fournisseur de services Internet local.|Configurez les éléments suivants :<br/><br/>-Une adresse intranet IPv4 avec le masque de sous-réseau approprié.<br/>-Suffixe DNS spécifique à la connexion de votre espace de noms intranet. Vous devez également configurer le serveur DNS sur l'interface interne.<br/>-Ne configurez pas de passerelle par défaut sur les interfaces intranet.|Pour configurer le serveur d'accès à distance afin d'atteindre tous les sous-réseaux du réseau IPv4 interne, procédez comme suit :<br/><br/>1. répertoriez les espaces d’adressage IPv4 pour tous les emplacements sur votre intranet.<br/>2. Utilisez les commandes **route Add-p** ou **netsh interface ipv4 add route** pour ajouter les espaces d’adressage IPv4 en tant qu’itinéraires statiques dans la table de routage IPv4 du serveur d’accès à distance.|  
    |Internet IPv6 et intranet IPv6|Configurez les éléments suivants :<br/><br/>-Utiliser la configuration d’adresse configurée automatiquement fournie par votre fournisseur de services Internet.<br/>-Utilisez la commande **route print** pour vous assurer qu’il existe un itinéraire IPv6 par défaut pointant vers le routeur du fournisseur de services Internet dans la table de routage IPv6.<br/>-Déterminez si le fournisseur de services Internet et les routeurs intranet utilisent les préférences de routeur par défaut décrites dans le document RFC 4191 et utilisent une préférence par défaut supérieure à celle de vos routeurs intranet locaux. Si vous avez répondu par l'affirmative dans les deux cas, aucune autre configuration n'est requise pour l'itinéraire par défaut. Une préférence plus élevée pour le routeur ISP permet de s'assurer que l'itinéraire IPv6 par défaut actif du serveur d'accès à distance pointe vers Internet IPv6.<br/><br/>Étant donné que le serveur d'accès à distance est un routeur IPv6, si vous disposez d'une infrastructure IPv6 native, l'interface Internet peut également atteindre les contrôleurs de domaine sur l'intranet. Dans ce cas, ajoutez des filtres de paquets au contrôleur de domaine dans le réseau de périmètre pour empêcher la connectivité IPv6 de l'interface Internet du serveur d'accès à distance|Configurez les éléments suivants :<br/><br/>-Si vous n’utilisez pas les niveaux de préférence par défaut, configurez vos interfaces intranet à l’aide de la commande **netsh interface ipv6 set InterfaceIndex ignoredefaultroutes = Enabled** . Cette commande vérifie que les itinéraires par défaut supplémentaires qui pointent vers les routeurs intranet ne seront pas ajoutés à la table de routage IPv6. Vous pouvez obtenir l'information InterfaceIndex de vos interfaces intranet dans l'affichage de la commande netsh interface show interface.|Si vous avez un intranet IPv6, procédez comme suit pour configurer le serveur d'accès à distance afin d'atteindre tous les emplacements IPv6 :<br/><br/>1. répertoriez les espaces d’adressage IPv6 pour tous les emplacements sur votre intranet.<br/>2. Utilisez la commande **netsh interface ipv6 add route** pour ajouter les espaces d’adressage IPv6 en tant qu’itinéraires statiques dans la table de routage IPv6 du serveur d’accès à distance.|  
    |Internet IPv6 et intranet IPv4|Le serveur d'accès à distance transfère le trafic de l'itinéraire IPv6 par défaut à l'aide de l'interface de carte Microsoft 6to4 à un relais 6to4 sur Internet IPv4. Vous pouvez configurer un serveur d'accès à distance pour l'adresse IPv4 du relais Microsoft 6to4 sur Internet IPv4 (utilisé quand IPv6 natif n'est pas déployé dans le réseau d'entreprise) avec la commande suivante : netsh interface ipv6 6to4 set relay name=192.88.99.1 state=enabled.|||  
  
    > [!NOTE]
    > 1. Si une adresse IPv4 publique a été attribuée au client DirectAccess, il utilisera la technologie de transition 6to4 pour se connecter à l'intranet. Si le client DirectAccess ne peut pas se connecter au serveur DirectAccess avec 6to4, il utilisera IP-HTTPS.  
    > 2. Les ordinateurs clients IPv6 natif se connectent au serveur d'accès à distance via IPv6 natif et aucune technologie de transition n'est requise.  
  
### <a name="plan-firewall-requirements"></a>Planifier la configuration requise pour le pare-feu

Si le serveur d'accès à distance se trouve derrière un pare-feu de périmètre, les exceptions suivantes sont requises pour le trafic d'accès à distance quand le serveur d'accès à distance est sur Internet IPv4 :  
  
- trafic 6to4 : protocole IP 41 entrant et sortant.  
  
- IP-HTTPs-port TCP (Transmission Control Protocol) 443 et port TCP source 443 sortant.  
  
- Si vous déployez l'accès à distance avec une seule carte réseau, puis que vous installez le serveur d'emplacement réseau sur le serveur d'accès à distance, vous devez également exempter le port TCP 62000.  
  
Les exceptions suivantes sont requises pour le trafic d'accès à distance que le serveur d'accès à distance est sur Internet IPv6 :  
  
- Protocole IP 50  
  
- Port UDP de destination 500 entrant et port UDP source 500 sortant.  
  
Lorsque vous utilisez des pare-feu supplémentaires, appliquez les exceptions de pare-feu de réseau interne suivantes pour le trafic d’accès à distance :  
  
- ISATAP-Protocole 41 entrant et sortant  
  
- TCP/UDP pour tout le trafic IPv4/IPv6  
  
### <a name="plan-certificate-requirements"></a>Planifier les exigences en matière de certificats

En matière de certificats, les exigences pour IPsec incluent un certificat d'ordinateur utilisé par les ordinateurs clients DirectAccess pour établir la connexion IPsec entre le client et le serveur d'accès à distance, ainsi qu'un certificat d'ordinateur utilisé par les serveurs d'accès à distance pour établir des connexions IPsec avec les clients DirectAccess. Pour DirectAccess dans Windows Server 2012, l’utilisation de ces certificats IPsec n’est pas obligatoire. L'Assistant Activation de DirectAccess configure le serveur d'accès à distance en tant que proxy Kerberos pour effectuer l'authentification IPsec sans exiger de certificats.  
  
1. **Serveur IP-HTTPS**: quand vous configurez l’accès à distance, le serveur d’accès à distance est automatiquement configuré pour agir en tant qu’écouteur Web IP-HTTPS. Le site IP-HTTPS requiert un certificat de site web. Les ordinateurs clients doivent être en mesure de contacter le site de la liste de révocation de certificats correspondant. L'Assistant Activation de DirectAccess essaie d'utiliser le certificat VPN SSTP. Si SSTP n'est pas configuré, il vérifie si un certificat pour IP-HTTPS est présent dans le magasin personnel de la machine. Si aucun certificat n'est disponible, il en crée automatiquement un qui est auto-signé.  
  
2. **Serveur d’emplacement réseau**: le serveur d’emplacement réseau est un site Web utilisé pour détecter si les ordinateurs clients se trouvent dans le réseau d’entreprise. Le serveur d'emplacement réseau requiert un certificat de site web. Les clients DirectAccess doivent être capables de contacter le site de la liste de révocation de certificats pour ce certificat. L'Assistant Activation de DirectAccess vérifie si un certificat pour le serveur d'emplacement réseau est présent dans le magasin personnel de l'ordinateur. En cas d'absence, il crée automatiquement un certificat auto-signé.  
  
Les exigences de certification de ces deux cas de figure sont résumées dans le tableau ci-après :  
  
|Authentification IPsec|Serveur IP-HTTPS|Serveur Emplacement réseau|  
|------------|----------|--------------|  
|Une autorité de certification interne est requise pour émettre des certificats d’ordinateur sur le serveur d’accès à distance et les clients pour l’authentification IPsec lorsque vous n’utilisez pas le proxy Kerberos pour l’authentification.|Autorité de certification publique : nous vous recommandons d’utiliser une autorité de certification publique pour émettre le certificat IP-HTTPs. cela garantit que le point de distribution de la liste de révocation de certificats est disponible en externe.|Autorité de certification interne : vous pouvez utiliser une autorité de certification interne pour émettre le certificat de site Web du serveur d’emplacement réseau. Assurez-vous que le point de distribution de la liste de révocation de certificats est hautement disponible à partir du réseau interne.|  
||Autorité de certification interne : vous pouvez utiliser une autorité de certification interne pour émettre le certificat IP-HTTPs. Toutefois, vous devez vous assurer que le point de distribution de liste de révocation de certificats est disponible en externe.|Certificat auto-signé : vous pouvez utiliser un certificat auto-signé pour le site Web du serveur emplacement réseau. Toutefois, vous ne pouvez pas utiliser un certificat auto-signé dans un déploiement multisite.|  
||Certificat auto-signé : vous pouvez utiliser un certificat auto-signé pour le serveur IP-HTTPs. Toutefois, vous devez vous assurer que le point de distribution de liste de révocation de certificats est disponible en externe. Un certificat auto-signé n'est pas utilisable dans un déploiement multisite.||  
  
#### <a name="plan-certificates-for-ip-https"></a>Planifier des certificats pour IP-HTTPS

Le serveur d'accès à distance joue le rôle d'écouteur IP-HTTPS, et vous devez installer manuellement un certificat de site web HTTPS sur le serveur. Notez les points suivants quand vous effectuez la planification :  
  
- Il est recommandé d'utiliser une autorité de certification publique, afin que les listes de révocation de certificats soient rapidement disponibles.  
  
- Dans le champ Objet, spécifiez l'adresse IPv4 de la carte Internet du serveur d'accès à distance ou le nom de domaine complet (FQDN) de l'URL IP-HTTPS (l'adresse ConnectTo). Si le serveur d'accès à distance se trouve derrière un périphérique NAT, vous devez spécifier le nom public ou l'adresse du périphérique NAT.  
  
- Le nom commun du certificat doit correspondre au nom du site IP-HTTPS.  
  
- Pour le champ Utilisation améliorée de la clé, utilisez l’identificateur d’objet (OID) d’authentification du serveur.  
  
- Pour le champ Points de distribution de la liste de révocation de certificats, indiquez un point de distribution de liste de révocation de certificats accessible par les clients DirectAccess connectés à Internet  
  
- Le certificat IP-HTTPS doit avoir une clé privée.  
  
- Le certificat IP-HTTPS doit être importé directement dans le magasin personnel.  
  
- Les certificats IP-HTTPS peuvent utiliser des caractères génériques dans le nom.  
  
#### <a name="plan-website-certificates-for-the-network-location-server"></a>Planifier des certificats de site web pour le serveur Emplacement réseau

Lors de la planification du site web du serveur d'emplacement réseau, notez les points suivants :  
  
- Dans le champ Objet, indiquez une adresse IP de l'interface intranet du serveur d'emplacement réseau ou le nom de domaine complet de l'URL d'emplacement réseau.  
  
- Dans le champ Utilisation améliorée de la clé, utilisez l'identificateur d'objet (OID) d'authentification du serveur.  
  
- Dans le champ Points de distribution de la liste de révocation des certificats, un point de distribution de liste de révocation des certificats accessible par les clients DirectAccess connectés à l'intranet. Ce point de distribution de liste de révocation de certificats ne doit pas être accessible depuis l'extérieur du réseau interne.  
  
- Si par la suite, vous projetez de configurer un déploiement multisite ou de cluster, le nom du certificat ne doit pas correspondre au nom interne du serveur d'accès à distance.  

    > [!NOTE]  
    > Assurez-vous que les certificats pour IP-HTTPS et le serveur d'emplacement réseau ont un **Nom de sujet**. Si le certificat n'a pas de **Nom de sujet** mais a un **Autre nom**, il ne sera pas accepté par l'Assistant Accès à distance.  
  
#### <a name="plan-dns-requirements"></a>Planifier la configuration DNS requise

Dans un déploiement de l'accès à distance, DNS est requis pour ce qui suit :  
  
- **Demandes du client DirectAccess**: DNS est utilisé pour résoudre les demandes des ordinateurs clients DirectAccess qui ne se trouvent pas sur le réseau interne. Les clients DirectAccess essaient de se connecter au serveur d'emplacement réseau DirectAccess afin de déterminer s'ils se situent sur Internet ou sur le réseau d'entreprise : Si la connexion réussit, c'est que les clients se situent sur l'intranet. DirectAccess n'est donc pas utilisé et les demandes des clients sont résolues à l'aide du serveur DNS configuré sur la carte réseau de l'ordinateur client. Si la connexion échoue, les clients sont considérés comme étant sur Internet. Les clients DirectAccess utilisent la table de stratégie de résolution de noms (NRPT) pour déterminer quel serveur DNS utiliser pour résoudre les demandes de noms. Vous pouvez préciser que les clients doivent utiliser DirectAccess DNS64 pour résoudre les noms, ou un autre serveur DNS interne. Pendant la résolution des noms, la table NRPT est utilisée par les clients DirectAccess pour identifier la manière de traiter une demande. Les clients demandent un nom de domaine complet (FQDN) ou un nom en une partie, par exemple <https://internal>. Si un nom en une partie est demandé, un suffixe DNS est ajouté pour créer un nom de domaine complet (FQDN). Si la requête DNS correspond à une entrée dans la NRPT, et que DNS4 ou un serveur DNS intranet est spécifié pour l'entrée, alors la requête est envoyée pour la résolution de noms à l'aide du serveur spécifié. S'il existe une correspondance alors qu'aucun serveur DNS n'est spécifié, alors celle-ci indique une règle d'exemption et la résolution de noms habituelle est appliquée.  
  
    Notez que quand un nouveau suffixe est ajouté à la table de stratégie de résolution de noms (NRPT) dans la console de gestion de l'accès à distance, vous avez la possibilité de détecter automatiquement les serveurs DNS par défaut pour le suffixe en cliquant sur le bouton **Détecter**. La détection automatique fonctionne comme suit :  
  
    1. Si le réseau d'entreprise est basé sur IPv4 ou sur IPv4 et IPv6, l'adresse par défaut correspond à l'adresse DNS64 de la carte interne sur le serveur d'accès à distance.  
  
    2. Si le réseau d'entreprise est basé sur IPv6, l'adresse par défaut correspond à l'adresse IPv6 des serveurs DNS du réseau d'entreprise.  
  
-  **Serveurs d’infrastructure**  
  
    1. **Serveur emplacement réseau**: les clients DirectAccess essaient d’atteindre le serveur emplacement réseau pour déterminer s’ils se trouvent sur le réseau interne. Les clients qui se trouvent sur le réseau interne doivent être en mesure de résoudre le nom du serveur d'emplacement réseau, mais il est impératif de les empêcher de le faire quand ils se situent sur Internet. Pour le garantir, le nom de domaine complet (FQDN) du serveur d'emplacement réseau est, par défaut, ajouté en tant que règle d'exemption à la NRPT. De plus, quand vous configurez l'accès à distance, les règles suivantes sont automatiquement créées :  
  
        1. Règle de suffixe DNS pour le domaine racine ou le nom de domaine du serveur d'accès à distance et les adresses IPv6 qui correspondent aux serveurs DNS intranet configurés sur le serveur d'accès à distance. Par exemple, si le serveur d'accès à distance est membre du domaine corp.contoso.com, une règle est créée pour le suffixe DNS corp.contoso.com.  
  
        2. Règle d'exemption pour le nom de domaine complet du serveur d'emplacement réseau. Par exemple, si l’URL du serveur emplacement réseau est <https://nls.corp.contoso.com>, une règle d’exemption est créée pour le nom de domaine complet nls.corp.contoso.com.  
  
        **Serveur IP-HTTPS**: le serveur d’accès à distance joue le rôle d’écouteur IP-HTTPS et utilise son certificat de serveur pour s’authentifier auprès des clients IP-HTTPS. Les clients DirectAccess doivent être en mesure de résoudre le nom IP-HTTPS à l'aide des serveurs DNS publics.  
  
        **Vérificateurs de connectivité**: l’accès à distance crée une sonde Web par défaut utilisée par les ordinateurs clients DirectAccess pour vérifier la connectivité au réseau interne. Pour que la sonde fonctionne bien comme prévu, les noms suivants doivent être enregistrés manuellement dans le DNS :  
  
        1.  DirectAccess-webprobehost doit correspondre à l’adresse IPv4 interne du serveur d’accès à distance, ou à l’adresse IPv6 dans un environnement IPv6 uniquement.  
  
        2.  DirectAccess-corpconnectivityhost doit être résolu en adresse localhost (Loopback). Des enregistrements A et AAAA doivent être créés : enregistrement A avec la valeur 127.0.0.1 et enregistrement AAAA avec la valeur construite à partir du préfixe NAT64 avec les 32 derniers bits sous la forme 127.0.0.1. Pour récupérer le préfixe, vous pouvez exécuter l'applet de commande get-netnattransitionconfiguration.  
  
            > [!NOTE]  
            > Cela n'est valide que dans un environnement IPv4 uniquement. Dans un environnement IPv4+IPv6 ou IPv6 uniquement, seul un enregistrement AAAA doit être créé avec l'adresse IP de bouclage ::1.  
  
        Vous pouvez créer d'autres vérificateurs de connectivité à l'aide d'autres adresses web sur HTTP ou PING. Pour chaque vérificateur de connectivité, une entrée DNS doit exister.  
  
#### <a name="dns-server-requirements"></a>Configuration de serveur DNS requise  
  
- Pour les clients DirectAccess, vous devez utiliser soit un serveur DNS exécutant Windows Server 2003, Windows Server 2008, Windows Server 2008 R2, Windows Server 2012, soit un serveur DNS qui prend en charge IPv6.  
  
### <a name="plan-active-directory"></a>Planifier Active Directory

L’accès à distance utilise Active Directory et Active Directory stratégie de groupe objets comme suit :  
  
- **Authentification**: Active Directory est utilisé pour l’authentification. Le tunnel intranet utilise l'authentification Kerberos pour que l'utilisateur accède aux ressources internes.  
  
- **Objets de stratégie de groupe**: l’accès à distance rassemble les paramètres de configuration dans des objets de stratégie de groupe qui sont appliqués aux serveurs d’accès à distance, aux clients et aux serveurs d’applications internes.  
  
- **Groupes de sécurité**: l’accès à distance utilise des groupes de sécurité pour rassembler et identifier des ordinateurs clients DirectAccess, ainsi que des serveurs d’accès à distance. Les stratégies de groupes sont appliquées au groupe de sécurité requis.  
  
- **Stratégies IPSec étendues**: l’accès à distance peut utiliser l’authentification et le chiffrement IPSec entre les clients et le serveur d’accès à distance. Vous pouvez étendre l'authentification et le chiffrement IPsec à des serveurs d'applications internes spécifiques.   
  
#### <a name="active-directory-requirements"></a>Configuration requise d'Active Directory  
  
Quand vous planifiez Active Directory pour un déploiement de l'accès à distance, les conditions suivantes sont requises :  
  
- Au moins un contrôleur de domaine installé sur les systèmes d’exploitation Windows Server 2012, Windows Server 2008 R2 Windows Server 2008 ou Windows Server 2003.  
  
    Si le contrôleur de domaine se trouve sur un réseau de périmètre (et est donc accessible depuis la carte réseau Internet du serveur d'accès à distance), empêchez le serveur d'accès à distance d'y accéder en ajoutant des filtres de paquets sur le contrôleur de domaine, pour empêcher toute connectivité à l'adresse IP de la carte Internet.  
  
- Le serveur d’accès à distance doit être membre d’un domaine.  
  
- Les clients DirectAccess doivent appartenir au domaine. Les clients peuvent appartenir :  
  
    - à tout domaine inclus dans la même forêt que le serveur d'accès à distance ;  
  
    - à tout domaine doté d'une approbation bidirectionnelle avec le domaine du serveur d'accès à distance ;  
  
    - à tout domaine inclus dans une forêt dotée d'une approbation bidirectionnelle avec la forêt à laquelle le domaine d'accès à distance appartient.  
  
> [!NOTE]  
> - Le serveur d'accès à distance ne peut pas être un contrôleur de domaine.  
> - Le contrôleur de domaine Active Directory utilisé pour l'accès à distance ne doit pas être accessible à partir de la carte Internet externe du serveur d'accès à distance (la carte ne doit pas figurer dans le profil de domaine du Pare-feu Windows).  
  
### <a name="plan-group-policy-objects"></a>Planifier les objets de stratégie de groupe

Les paramètres DirectAccess configurés quand vous configurez l'accès à distance sont rassemblés dans des objets de stratégie de groupe. Trois objets de stratégie de groupe différents sont renseignés avec les paramètres DirectAccess, puis distribués comme suit :  
  
- **Objet de stratégie de groupe du client DirectAccess**: cet objet de stratégie de groupe contient des paramètres client, notamment les paramètres de technologie de transition IPv6, les entrées NRPT et les règles de sécurité de connexion du pare-feu Windows avec fonctions avancées L'objet de stratégie de groupe est appliqué aux groupes de sécurité spécifiés pour les ordinateurs clients.  
  
- **Objet de stratégie de groupe du serveur DirectAccess**: cet objet de stratégie de groupe contient les paramètres de configuration DirectAccess qui sont appliqués à tout serveur configuré en tant que serveur d’accès à distance dans votre déploiement. Il contient également les règles de sécurité de connexion du Pare-feu Windows avec fonctions avancées de sécurité.  
  
- **Objet de stratégie de groupe serveurs d’applications**: cet objet de stratégie de groupe contient des paramètres pour les serveurs d’applications sélectionnés auxquels vous étendez éventuellement l’authentification et le chiffrement à partir des clients DirectAccess. Si l'authentification et le chiffrement ne sont pas étendus, cet objet de stratégie de groupe n'est pas utilisé.  
  
Les objets de stratégie de groupe sont créés automatiquement par l'Assistant Activation de DirectAccess et un nom par défaut est spécifié pour chaque objet de stratégie de groupe.  
  
> [!CAUTION]  
> Utilisez la procédure suivante pour sauvegarder tous les objets de stratégie de groupe d'accès à distance avant d'exécuter des applets de commande DirectAccess : [Sauvegarder et restaurer la configuration de l'accès à distance](https://go.microsoft.com/fwlink/?LinkID=257928)  
  
Vous pouvez configurer les objets de stratégie de groupe de deux manières :  
  
1. **Automatiquement**: vous pouvez spécifier qu’elles sont créées automatiquement. Un nom par défaut est spécifié pour chaque objet de stratégie de groupe.  
  
2. **Manuellement**: vous pouvez utiliser des objets de stratégie de groupe qui ont été prédéfinis par l’administrateur Active Directory.  
  
Notez qu'une fois que DirectAccess est configuré pour utiliser des objets de stratégie de groupe spécifiques, il ne peut plus être configuré pour en utiliser d'autres.  
  
#### <a name="automatically-created-gpos"></a>Objets de stratégie de groupe créés automatiquement

Notez les points suivants quand vous utilisez des objets de stratégie de groupe créés automatiquement :  
  
Les objets de stratégie de groupe créés automatiquement sont appliqués en fonction du paramètre de l'emplacement et du paramètre de la cible du lien, de la manière suivante :  
  
- Pour l'objet de stratégie de groupe Serveur DirectAccess, les paramètres de l'emplacement et du lien pointent tous les deux vers le domaine qui contient le serveur d'accès à distance.  
  
- Quand les objets de stratégie de groupe des clients sont créés, l'emplacement est défini sur un domaine unique dans lequel l'objet de stratégie de groupe est créé. Le nom de l'objet de stratégie de groupe est recherché dans chaque domaine, puis renseigné avec les paramètres DirectAccess s'il existe. La cible du lien est définie à la racine du domaine dans lequel l'objet de stratégie de groupe a été créé. Un objet de stratégie de groupe est créé pour chaque domaine qui contient des ordinateurs clients ou des serveurs d'applications, et il est lié à la racine de son domaine respectif.  
  
Quand vous utilisez des objets de stratégie de groupe créés automatiquement, pour appliquer les paramètres DirectAccess, l'administrateur du serveur d'accès à distance requiert les autorisations suivantes :  
  
- Autorisations de création d'objets de stratégie de groupe pour chaque domaine.  
  
- Autorisations de lien pour toutes les racines de domaine client sélectionnées.  
  
- Autorisations de lien pour les racines de domaine des objets de stratégie de groupe des serveurs.  
  
- Autorisations de création, modification, suppression et modification de la sécurité pour les objets de stratégie de groupe.  
  
- Nous recommandons à l'administrateur de l'accès à distance de posséder des autorisations de lecture des objets de stratégie de groupe pour chaque domaine requis. Ainsi, l'accès à distance peut vérifier qu'il n'existe pas de doublons de noms d'objets de stratégie de groupe lors de la création de ces derniers.  
  
Notez que si les autorisations adéquates pour la liaison des objets de stratégie de groupe n'existent pas, un avertissement est émis. L'accès à distance continue de fonctionner mais aucune liaison ne se produit. Si cet avertissement est émis, les liens ne sont pas créés automatiquement, même après la mise en place des autorisations. L'administrateur doit alors créer les liens manuellement.  
  
#### <a name="manually-created-gpos"></a>Objets de stratégie de groupe créés manuellement

Notez les points suivants quand vous utilisez des objets de stratégie de groupe créés manuellement :  
  
- Les objets de stratégie de groupe doivent déjà exister avant d'exécuter l'Assistant Configuration de l'accès à distance.  
  
- Quand vous utilisez des objets de stratégie de groupe créés manuellement, pour appliquer les paramètres DirectAccess, l'administrateur de l'accès à distance requiert des autorisations complètes (modification, suppression, modification de la sécurité) sur les objets de stratégie de groupe créés manuellement.  
  
- De plus, une recherche de lien vers l'objet de stratégie de groupe est effectuée dans tout le domaine. Si l'objet de stratégie de groupe n'est pas lié dans le domaine, alors un lien est automatiquement créé à la racine du domaine. Si les autorisations requises pour créer le lien ne sont pas disponibles, un avertissement est émis.  
  
Notez que si les autorisations adéquates pour la liaison des objets de stratégie de groupe n'existent pas, un avertissement est émis. L'accès à distance continue de fonctionner mais aucune liaison ne se produit. Si cet avertissement est émis, les liens ne sont pas créés automatiquement, même si les autorisations sont ajoutées plus tard. L'administrateur doit alors créer les liens manuellement.  
  
#### <a name="recovering-from-a-deleted-gpo"></a>Récupération après la suppression d'un objet de stratégie de groupe

Si un objet de stratégie de groupe d'un serveur, client ou serveur d'applications d'accès à distance est supprimé par inadvertance et qu'aucune sauvegarde n'est disponible, vous devez supprimer les paramètres de configuration et recommencer la configuration. Si vous disposez d'une sauvegarde, vous pouvez restaurer l'objet de stratégie de groupe.  
  
La gestion de l' **accès à distance** affiche le message d’erreur suivant : le **GPO (nom de l’objet de stratégie de groupe) est introuvable**. Pour supprimer les paramètres de configuration, procédez comme suit :  
  
1. Exécutez l'applet de commande PowerShell **Uninstall-remoteaccess**.  
  
2. Rouvrez la **gestion de l’accès à distance**.  
  
3. Un message d'erreur indique que l'objet de stratégie de groupe est introuvable. Cliquez sur **Supprimer les paramètres de configuration**. Le serveur est ensuite rétabli à l'état Non configuré.  

