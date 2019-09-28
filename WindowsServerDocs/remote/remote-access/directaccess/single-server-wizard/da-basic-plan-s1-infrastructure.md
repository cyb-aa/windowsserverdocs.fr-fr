---
title: Étape 1 planifier l’infrastructure DirectAccess de base
description: Cette rubrique fait partie du guide déployer un serveur DirectAccess unique à l’aide de l’Assistant Prise en main pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6f4c727dc8f7905502d47119bd0e911537e827aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404881"
---
# <a name="step-1-plan-the-basic-directaccess-infrastructure"></a>Étape 1 planifier l’infrastructure DirectAccess de base
La première étape pour un déploiement de base de DirectAccess sur un serveur unique consiste à effectuer la planification de l’infrastructure requise pour le déploiement. Cette rubrique décrit les étapes de planification de cette infrastructure :  
  
|Tâche|Description|  
|----|--------|  
|Planifier la topologie et les paramètres réseau|Décidez où placer le serveur DirectAccess \(AT la périphérie, ou derrière une traduction d’adresses réseau \(NAT @ no__t-2 ou un pare-feu @ no__t-3, et planifiez l’adressage IP et le routage.|  
|Planifier la configuration requise pour le pare-feu|Planifiez l'autorisation de DirectAccess via des pare-feu de périmètre.|  
|Planifier les exigences en matière de certificats|DirectAccess peut utiliser Kerberos ou des certificats pour l'authentification client. Dans ce déploiement de base de DirectAccess, un proxy Kerberos est automatiquement configuré et l'authentification est accomplie à l'aide d'informations d'identification Active Directory.|  
|Planifier la configuration DNS requise|Planifiez les paramètres DNS pour le serveur DirectAccess, les serveurs d’infrastructure et la connectivité client.|  
|Planifier Active Directory|Planifiez vos contrôleurs de domaine et la configuration Active Directory requise.|  
|Planifier les objets de stratégie de groupe|Déterminez quels objets de stratégie de groupe sont requis dans votre organisation et comment les créer ou les modifier.|  
  
Vous pouvez effectuer les tâches de planification dans l'ordre qui vous convient.  
  
## <a name="bkmk_1_1_Network_svr_top_settings"></a>Planifier la topologie et les paramètres du réseau  
  
### <a name="plan-network-adapters-and-ip-addressing"></a>Planifier les cartes réseau et l'adressage IP  
  
1.  Identifiez la topologie des cartes réseau à utiliser. DirectAccess peut être configuré de l'une ou l'autre des manières suivantes :  
  
    -   Avec deux cartes réseau : soit à la périphérie avec une carte réseau connectée à Internet et l’autre au réseau interne, soit derrière un périphérique NAT, un pare-feu ou un périphérique routeur, avec une carte réseau connectée à un réseau de périmètre et l’autre à la réseaux.  
  
    -   Derrière un périphérique NAT avec une carte réseau : le serveur DirectAccess est installé derrière un périphérique NAT et la carte réseau unique est connectée au réseau interne.  
  
2.  Déterminez vos exigences en matière d'adressage IP :  
  
    DirectAccess utilise IPv6 avec IPsec pour créer une connexion sécurisée entre les ordinateurs clients DirectAccess et le réseau d'entreprise interne. Pourtant, DirectAccess ne requiert pas systématiquement une connectivité Internet IPv6 ni une prise en charge IPv6 native sur les réseaux internes. Au lieu de cela, il configure et utilise automatiquement des technologies de transition IPv6 pour transférer le trafic IPv6 sur Internet IPv4 \(6to4, Teredo, IP @ no__t-1HTTPS @ no__t-2 et sur votre intranet @ no__t-3only intranet \(NAT64 ou ISATAP @ no__t-5. Pour obtenir une vue d'ensemble de ces technologies de transition, consultez les ressources suivantes :  
  
    -   [Technologies de transition IPv6](https://technet.microsoft.com/library/bb726951.aspx)  
  
    -   [Spécification du protocole de tunnel IP-HTTPs](https://msdn.microsoft.com/library/dd358571(PROT.10).aspx)  
  
3.  Configurez les cartes et l'adressage requis selon le tableau suivant. Pour les déploiements derrière un périphérique NAT à l’aide d’une seule carte réseau, configurez vos adresses IP en utilisant uniquement la colonne **carte réseau interne** .  
  
    ||Carte réseau externe|Carte réseau interne<sup>1</sup>|Exigences en matière de routage|  
    |-|--------------|--------------------|------------|  
    |Internet IPv4 et intranet IPv4|Configurez ce qui suit :<br /><br />-Une adresse IPv4 publique statique avec le masque de sous-réseau approprié.<br />-Adresse IPv4 de passerelle par défaut de votre pare-feu Internet ou du fournisseur de services Internet local \(ISP @ no__t-1.|Configurez ce qui suit :<br /><br />-Une adresse intranet IPv4 avec le masque de sous-réseau approprié.<br />-Un suffixe DNS de connexion @ no__t-0specific de votre espace de noms intranet. Vous devez également configurer un serveur DNS sur l'interface interne.<br />-Ne configurez pas de passerelle par défaut sur les interfaces intranet.|Pour configurer le serveur DirectAccess afin d'atteindre tous les sous-réseaux du réseau IPv4 interne, procédez comme suit :<br /><br />1.  Répertoriez les espaces d'adressage IPv4 pour tous les emplacements sur votre intranet.<br />2.  Utilisez les commandes **route add \-P** ou **netsh interface ipv4 add route** pour ajouter les espaces d’adressage IPv4 en tant qu’itinéraires statiques dans la table de routage IPv4 du serveur DirectAccess.|  
    |Internet IPv6 et intranet IPv6|Configurez ce qui suit :<br /><br />-Utiliser la configuration d’adresse configurée automatiquement fournie par votre fournisseur de services Internet.<br />-Utilisez la commande **route print** pour vous assurer qu’il existe un itinéraire IPv6 par défaut pointant vers le routeur du fournisseur de services Internet dans la table de routage IPv6.<br />-Déterminez si le fournisseur de services Internet et les routeurs intranet utilisent les préférences de routeur par défaut décrites dans le document RFC 4191 et utilisent une préférence par défaut supérieure à celle de vos routeurs intranet locaux. Si vous avez répondu par l'affirmative dans les deux cas, aucune autre configuration n'est requise pour l'itinéraire par défaut. Une préférence plus élevée pour le routeur ISP permet de s'assurer que l'itinéraire IPv6 par défaut actif du serveur DirectAccess pointe vers Internet IPv6.<br /><br />Étant donné que le serveur DirectAccess est un routeur IPv6, si vous disposez d'une infrastructure IPv6 native, l'interface Internet peut également atteindre les contrôleurs de domaine sur l'intranet. Dans ce cas, ajoutez des filtres de paquets au contrôleur de domaine dans le réseau de périmètre pour empêcher la connectivité à l’adresse IPv6 de l’interface Internet @ no__t-0facing du serveur DirectAccess.|Configurez ce qui suit :<br /><br />-Si vous n’utilisez pas les niveaux de préférence par défaut, configurez vos interfaces intranet à l’aide de la commande **netsh interface ipv6 set InterfaceIndex ignoredefaultroutes @ no__t-1enabled** . Cette commande vérifie que les itinéraires par défaut supplémentaires qui pointent vers les routeurs intranet ne seront pas ajoutés à la table de routage IPv6. Vous pouvez obtenir l'information InterfaceIndex de vos interfaces intranet dans l'affichage de la commande netsh interface show interface.|Si vous avez un intranet IPv6, procédez comme suit pour configurer le serveur DirectAccess afin d'atteindre tous les emplacements IPv6 :<br /><br />1.  Répertoriez les espaces d'adressage IPv6 pour tous les emplacements sur votre intranet.<br />2.  Utilisez la commande **netsh interface ipv6 add route** pour ajouter les espaces d'adressage IPv6 en tant qu'itinéraires statiques dans la table de routage IPv6 du serveur DirectAccess.|  
    |Internet IPv4 et intranet IPv6|Le serveur DirectAccess transfère le trafic de l'itinéraire IPv6 par défaut à l'aide de l'interface de carte Microsoft 6to4 à un relais 6to4 sur Internet IPv4. Vous pouvez configurer un serveur DirectAccess pour l’adresse IPv4 du relais Microsoft 6to4 sur Internet IPv4 \(used lorsque le protocole IPv6 natif n’est pas déployé dans le réseau d’entreprise @ no__t-1 avec la commande suivante : netsh interface IPv6 6to4 Set Relay Name @ no__ commande t-2192.88.99.1 @ no__t-3enabled.|||  
  
    > [!NOTE]  
    > Notez les points suivants :  
    >   
    > 1.  Si une adresse IPv4 publique a été attribuée au client DirectAccess, il utilisera la technologie de transition 6to4 pour se connecter à l'intranet. Si le client DirectAccess ne peut pas se connecter au serveur DirectAccess avec 6to4, il utilisera l’adresse IP @ no__t-0HTTPS.  
    > 2.  Les ordinateurs clients IPv6 natif se connectent au serveur DirectAccess via IPv6 natif et aucune technologie de transition n'est requise.  
  
### <a name="ConfigFirewalls"></a>Planifier la configuration requise du pare-feu  
Si le serveur DirectAccess se trouve derrière un pare-feu de périmètre, les exceptions suivantes sont requises pour le trafic DirectAccess quand le serveur DirectAccess est sur Internet IPv4 :  
  
-   trafic 6to4 : protocole IP 41 entrant et sortant.  
  
-   IP @ no__t-0HTTPS-Transmission Control Protocol \(TCP @ no__t-2 port de destination 443 et port TCP source 443 sortant.  
  
-   Si vous déployez DirectAccess avec une seule carte réseau, puis installez le serveur d'emplacement réseau sur le serveur DirectAccess, vous devez également exempter le port TCP 62000.  
  
    > [!NOTE]  
    > Cette exemption se trouve sur le serveur DirectAccess. Toutes les autres exceptions sont sur le pare-feu de périmètre.  
  
Les exceptions suivantes sont requises pour le trafic DirectAccess que le serveur DirectAccess est sur Internet IPv6 :  
  
-   Protocole IP 50  
  
-   Port UDP de destination 500 entrant et port UDP source 500 sortant.  
  
Quand vous utilisez des pare-feu supplémentaires, appliquez les exceptions de pare-feu de réseau interne suivantes pour le trafic DirectAccess :  
  
-   ISATAP-Protocole 41 entrant et sortant  
  
-   TCP @ no__t-0UDP pour tout le trafic IPv4 @ no__t-1IPv6  
  
### <a name="bkmk_1_2_CAs_and_certs"></a>Planifier les exigences de certificat  
En matière de certificats, les exigences pour IPsec incluent un certificat d'ordinateur utilisé par les ordinateurs clients DirectAccess pour établir la connexion IPsec entre le client et le serveur DirectAccess, ainsi qu'un certificat d'ordinateur utilisé par les serveurs DirectAccess pour établir des connexions IPsec avec les clients DirectAccess. Pour DirectAccess dans Windows Server 2012 R2 et Windows Server 2012, l’utilisation de ces certificats IPsec n’est pas obligatoire. L'Assistant Mise en route configure le serveur DirectAccess en tant que proxy Kerberos pour effectuer l'authentification IPsec sans exiger de certificats.
  
1.  **IP @ no__t-1HTTPS Server**. Quand vous configurez DirectAccess, le serveur DirectAccess est automatiquement configuré pour agir en tant qu’écouteur Web IP @ no__t-0HTTPS. Le site IP @ no__t-0HTTPS requiert un certificat de site Web et les ordinateurs clients doivent être en mesure de contacter la liste de révocation de certificats \(CRL @ no__t-2 pour le certificat. L'Assistant Activation de DirectAccess essaie d'utiliser le certificat VPN SSTP. Si SSTP n’est pas configuré, il vérifie si un certificat pour IP @ no__t-0HTTPS est présent dans le magasin personnel de l’ordinateur. Si aucun certificat n’est disponible, il crée automatiquement un certificat auto @ no__t-0signed.
  
2.  **Serveur Emplacement réseau**. Le serveur emplacement réseau est un site Web utilisé pour détecter si les ordinateurs clients se trouvent dans le réseau d’entreprise. Le serveur emplacement réseau requiert un certificat de site Web. Pour ce certificat, les clients DirectAccess doivent pouvoir contacter le site contenant la liste de révocation de certificats. L’Assistant Activation de l’accès à distance vérifie si un certificat pour le serveur emplacement réseau est présent dans le magasin personnel de l’ordinateur. S’il n’est pas présent, il crée automatiquement un certificat auto @ no__t-0signed.
  
Les exigences de certification de ces deux cas de figure sont résumées dans le tableau ci-après :  
  
|Authentification IPsec|IP @ no__t-0HTTPS Server|Serveur Emplacement réseau|  
|------------|----------|--------------|  
|Une autorité de certification interne est requise pour émettre des certificats d’ordinateur sur le serveur et les clients DirectAccess pour l’authentification IPsec lorsque vous n’utilisez pas le proxy Kerberos pour l’authentification.|Autorité de certification publique : il est recommandé d’utiliser une autorité de certification publique pour émettre le certificat IP @ no__t-0HTTPS, ce qui garantit que le point de distribution de la liste de révocation de certificats est disponible en externe.|Autorité de certification interne : vous pouvez utiliser une autorité de certification interne pour émettre le certificat de site Web du serveur d’emplacement réseau. Assurez-vous que le point de distribution de la liste de révocation de certificats est hautement disponible à partir du réseau interne.|  
||Autorité de certification interne : vous pouvez utiliser une autorité de certification interne pour émettre le certificat IP @ no__t-0HTTPS. Toutefois, vous devez vous assurer que le point de distribution de liste de révocation de certificats est disponible en externe.|Certificat auto @ no__t-0signed : vous pouvez utiliser un certificat Self @ no__t-1signed pour le site Web du serveur emplacement réseau. Toutefois, vous ne pouvez pas utiliser un certificat auto @ no__t-2signed dans des déploiements multisites.|  
||Certificat Self @ no__t-0signed : vous pouvez utiliser un certificat Self @ no__t-1signed pour le serveur IP @ no__t-2HTTPS. Toutefois, vous devez vous assurer que le point de distribution de liste de révocation de certificats est disponible en externe. Un certificat Self @ no__t-0signed ne peut pas être utilisé dans un déploiement multisite.||  
  
#### <a name="bkmk_website_cert_IPHTTPS"></a>Planifier des certificats pour IP @ no__t-1HTTPS et le serveur d’emplacement réseau  
Pour configurer un certificat à ces fins, reportez-vous à [Déployer un serveur DirectAccess unique avec des paramètres avancés](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md). Si aucun certificat n’est disponible, l’Assistant Prise en main crée automatiquement des certificats auto @ no__t-0signed à cet effet.
  
> [!NOTE]
> Si vous configurez des certificats pour IP @ no__t-0HTTPS et le serveur d’emplacement réseau manuellement, assurez-vous que les certificats ont un nom d’objet. S'ils n'en comportent pas, mais qu'ils possèdent un autre nom, l'Assistant DirectAccess ne les acceptera pas.  
  
#### <a name="plan-dns-requirements"></a>Planifier la configuration DNS requise  
Dans un déploiement de DirectAccess, DNS est obligatoire pour ce qui suit :  
  
-   **Demandes des clients DirectAccess**. DNS sert à résoudre les demandes des ordinateurs clients DirectAccess qui ne se trouvent pas sur le réseau interne. Les clients DirectAccess essaient de se connecter au serveur d'emplacement réseau DirectAccess afin de déterminer s'ils se situent sur Internet ou sur le réseau d'entreprise : Si la connexion réussit, c'est que les clients se situent sur l'intranet. DirectAccess n'est donc pas utilisé et les demandes des clients sont résolues à l'aide du serveur DNS configuré sur la carte réseau de l'ordinateur client. Si la connexion échoue, les clients sont considérés comme étant sur Internet. Les clients DirectAccess utilisent la table de stratégie de résolution de noms \(NRPT @ no__t-1 pour déterminer le serveur DNS à utiliser lors de la résolution des demandes de nom. Vous pouvez préciser que les clients doivent utiliser DirectAccess DNS64 pour résoudre les noms, ou un autre serveur DNS interne. Pendant la résolution des noms, la table NRPT est utilisée par les clients DirectAccess pour identifier la manière de traiter une demande. Les clients demandent un nom de domaine complet (FQDN) ou un seul nom @ no__t-0label, tel que http : \/ @ no__t-2internal. Si un seul nom @ no__t-0label est demandes, un suffixe DNS est ajouté pour créer un nom de domaine complet (FQDN). Si la requête DNS correspond à une entrée dans la NRPT, et que DNS4 ou un serveur DNS intranet est spécifié pour l'entrée, alors la requête est envoyée pour la résolution de noms à l'aide du serveur spécifié. S'il existe une correspondance alors qu'aucun serveur DNS n'est spécifié, alors celle-ci indique une règle d'exemption et la résolution de noms habituelle est appliquée.  
  
    Quand un nouveau suffixe est ajouté à la NRPT dans la console de gestion DirectAccess, vous avez la possibilité de détecter automatiquement les serveurs DNS par défaut pour le suffixe en cliquant sur le bouton **Détecter**. La détection automatique fonctionne comme suit :  
  
    1.  Si le réseau d’entreprise est IPv4 @ no__t-0based ou IPv4 et IPv6, l’adresse par défaut est l’adresse DNS64 de la carte interne sur le serveur DirectAccess.  
  
    2.  Si le réseau d’entreprise est IPv6 @ no__t-0based, l’adresse par défaut est l’adresse IPv6 des serveurs DNS du réseau d’entreprise.  
  
-   **Serveurs d’infrastructure**
  
    1.  **Serveur Emplacement réseau**. Les clients DirectAccess essaient d'atteindre le serveur Emplacement réseau pour déterminer s'ils se situent sur le réseau interne. Les clients qui se trouvent sur le réseau interne doivent être en mesure de résoudre le nom du serveur d'emplacement réseau, mais il est impératif de les empêcher de le faire quand ils se situent sur Internet. Pour le garantir, le nom de domaine complet (FQDN) du serveur d'emplacement réseau est, par défaut, ajouté en tant que règle d'exemption à la NRPT. De plus, quand vous configurez DirectAccess, les règles suivantes sont automatiquement créées :  
  
        1.  Règle de suffixe DNS pour le domaine racine ou le nom de domaine du serveur DirectAccess et les adresses IPv6 qui correspondent aux serveurs DNS intranet configurés sur le serveur DirectAccess. Par exemple, si le serveur DirectAccess est membre du domaine corp.contoso.com, une règle est créée pour le suffixe DNS corp.contoso.com.  
  
        2.  Règle d'exemption pour le nom de domaine complet du serveur d'emplacement réseau. Par exemple, si l’URL du serveur emplacement réseau est https : @no__t -0\/nls.corp.contoso.com, une règle d’exemption est créée pour le nom de domaine complet nls.corp.contoso.com.  
  
        **IP @ no__t-1HTTPS Server**. Le serveur DirectAccess joue le rôle d’écouteur IP @ no__t-0HTTPS et utilise son certificat de serveur pour s’authentifier auprès des clients IP @ no__t-1HTTPS. Le nom d’adresse IP @ no__t-0HTTPS doit pouvoir être résolu par les clients DirectAccess qui utilisent des serveurs DNS publics.  
  
        **Vérificateurs de connectivité**. DirectAccess crée une sonde web par défaut utilisée par les ordinateurs clients DirectAccess pour vérifier la connectivité au réseau interne. Pour que la sonde fonctionne bien comme prévu, les noms suivants doivent être enregistrés manuellement dans le DNS :  
  
        1.  DirectAccess @ no__t-0webprobehost : doit être résolu en adresse IPv4 interne du serveur DirectAccess, ou en adresse IPv6 dans un environnement IPv6 @ no__t-1only.  
  
        2.  DirectAccess @ no__t-0corpconnectivityhost-doit correspondre à localhost \(loopback @ no__t-2. Des enregistrements A et AAAA doivent être créés : enregistrement A avec la valeur 127.0.0.1 et enregistrement AAAA avec la valeur construite à partir du préfixe NAT64 avec les 32 derniers bits sous la forme 127.0.0.1. Vous pouvez récupérer le préfixe NAT64 en exécutant l’applet de commande @ no__t-0netnattransitionconfiguration.  
  
        Vous pouvez créer d'autres vérificateurs de connectivité à l'aide d'autres adresses web sur HTTP ou PING. Pour chaque vérificateur de connectivité, une entrée DNS doit exister.  
  
#### <a name="dns-server-requirements"></a>Configuration requise du serveur DNS  
  
-   Pour les clients DirectAccess, vous devez utiliser un serveur DNS qui exécute Windows Server 2008, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016 ou tout autre serveur DNS qui prend en charge IPv6.  
  
> [!NOTE]  
> Il est déconseillé d'utiliser des serveurs DNS qui exécutent Windows Server 2003 lorsque vous déployez DirectAccess. Bien que les serveurs DNS Windows Server 2003 prennent en charge les enregistrements IPv6, Windows Server 2003 n’est plus pris en charge par Microsoft. En outre, vous ne devez pas déployer DirectAccess si vos contrôleurs de domaine exécutent Windows Server  2003 en raison d’un problème avec le service de réplication de fichiers. Pour plus d'informations, consultez [Configurations non prises en charge par DirectAccess](../DirectAccess-Unsupported-Configurations.md).  
  
### <a name="bkmk_1_4_NLS"></a>Planifier le serveur emplacement réseau  
Le serveur d'emplacement réseau est un site web utilisé pour détecter si les clients DirectAccess se trouvent dans le réseau d'entreprise. Les clients inclus dans le réseau d'entreprise n'utilisent pas DirectAccess pour atteindre les ressources internes, puisqu'ils se connectent directement.  
  
L'Assistant Mise en route configure automatiquement le serveur d'emplacement réseau sur le serveur DirectAccess et le site web est automatiquement créé quand vous déployez DirectAccess. L'installation est ainsi simple car sans infrastructure de certificats.
  
Si vous souhaitez déployer un serveur emplacement réseau et que vous n’utilisez pas de certificats auto @ no__t-0signed, reportez-vous à [déployer un serveur DirectAccess unique avec des paramètres avancés](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).
  
### <a name="bkmk_1_6_AD"></a>Active Directory de plan  
DirectAccess utilise des objets de stratégie de groupe Active Directory et Active Directory comme suit :
  
-   **Authentification**. Active Directory est utilisé à des fins d’authentification. Le tunnel DirectAccess utilise l'authentification Kerberos pour que l'utilisateur accède aux ressources internes.
  
-   **Objets de stratégie de groupe**. DirectAccess rassemble les paramètres de configuration dans des objets de stratégie de groupe qui sont appliqués aux serveurs et clients DirectAccess.
  
-   **Groupes de sécurité**. DirectAccess utilise des groupes de sécurité pour rassembler et identifier des ordinateurs clients et des serveurs DirectAccess. Les stratégies de groupes sont appliquées au groupe de sécurité requis.

**Configuration requise pour la Active Directory**  
  
Quand vous planifiez Active Directory pour un déploiement de DirectAccess, les conditions suivantes sont requises :  
  
-   Au moins un contrôleur de domaine installé sur Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008. 
  
    Si le contrôleur de domaine se trouve sur un réseau de périmètre \(and par conséquent accessible à partir de la carte réseau Internet @ no__t-1facing du serveur DirectAccess @ no__t-2 empêche le serveur DirectAccess de l’atteindre en ajoutant des filtres de paquets sur le domaine pour empêcher la connectivité à l’adresse IP de la carte Internet.  
  
-   Le serveur DirectAccess doit être membre d'un domaine.  
  
-   Les clients DirectAccess doivent appartenir au domaine. Les clients peuvent appartenir :  
  
    -   à tout domaine inclus dans la même forêt que le serveur DirectAccess ;  
  
    -   Tout domaine disposant d’une approbation à deux no__t-0way avec le domaine du serveur DirectAccess.  
  
    -   N’importe quel domaine d’une forêt qui a une approbation à deux no__t-0way avec la forêt à laquelle le domaine DirectAccess appartient.  
  
> [!NOTE]
> - Le serveur DirectAccess ne peut pas être un contrôleur de domaine.  
> - Le contrôleur de domaine Active Directory utilisé pour DirectAccess ne doit pas être accessible à partir de la carte Internet externe du serveur DirectAccess \(the ne doit pas figurer dans le profil de domaine du pare-feu Windows @ no__t-1.  
  
### <a name="bkmk_1_7_GPOs"></a>Planifier les objets de stratégie de groupe  
Les paramètres DirectAccess configurés quand vous configurez DirectAccess sont rassemblés dans des objets de stratégie de groupe \(GPO @ no__t-1. Deux objets de stratégie de groupe différents sont renseignés avec les paramètres DirectAccess, puis distribués comme suit :  
  
-   **Objet de stratégie de groupe de client DirectAccess**. Cet objet de stratégie de groupe contient des paramètres client, notamment les paramètres de technologie de transition IPv6, les entrées de la table NRPT et les règles de sécurité de connexion du Pare-feu Windows avec fonctions avancées de sécurité. L'objet de stratégie de groupe est appliqué aux groupes de sécurité spécifiés pour les ordinateurs clients.  
  
-   **Objet de stratégie de groupe de serveur DirectAccess**. Cet objet de stratégie de groupe contient les paramètres de configuration DirectAccess qui sont appliqués à tout serveur configuré en tant que serveur DirectAccess dans votre déploiement. Il contient également les règles de sécurité de connexion du Pare-feu Windows avec fonctions avancées de sécurité.  
  
Vous pouvez configurer les objets de stratégie de groupe de deux manières :  
  
1.  **Automatiquement**. Vous pouvez spécifier qu’ils doivent être créés automatiquement. Un nom par défaut est spécifié pour chaque objet de stratégie de groupe. Les objets de stratégie de groupe sont automatiquement créés par l'Assistant Mise en route.
  
2.  **Manuellement**. Vous pouvez utiliser des objets de stratégie de groupe qui ont été prédéfinis par l’administrateur Active Directory.
  
Notez qu'une fois que DirectAccess est configuré pour utiliser des objets de stratégie de groupe spécifiques, il ne peut plus être configuré pour en utiliser d'autres.
  
> [!IMPORTANT]
> Que vous utilisiez des objets de stratégie de groupe configurés automatiquement ou manuellement, vous devez ajouter une stratégie pour la détection de liaison lente si vos clients utilisent la 3G. Chemin d’accès stratégie de groupe pour **Policy : Configurez stratégie de groupe détection de liaison lente @ no__t-0 : **Configuration de l’ordinateur \/ les stratégies \/ modèles d’administration système \/ \/ stratégie de groupe**.  
  
> [!CAUTION]  
> Utilisez la procédure suivante pour sauvegarder tous les objets de stratégie de groupe DirectAccess avant d'exécuter des applets de commande DirectAccess : [Sauvegarder et restaurer la configuration DirectAccess](https://go.microsoft.com/fwlink/?LinkID=257928)  
  
#### <a name="automatically-created-gpos"></a>Objets de stratégie de groupe @ no__t-0created automatiquement  
Notez les points suivants lors de l’utilisation automatique des objets de stratégie de groupe @ no__t-0created :  
  
Les objets de stratégie de groupe créés automatiquement sont appliqués en fonction du paramètre de l'emplacement et du paramètre de la cible du lien, de la manière suivante :  
  
-   Pour l'objet de stratégie de groupe des serveurs DirectAccess, les paramètres de l'emplacement et du lien pointent tous les deux vers le domaine qui contient le serveur DirectAccess.  
  
-   Quand les objets de stratégie de groupe des clients sont créés, l'emplacement est défini sur un domaine unique dans lequel l'objet de stratégie de groupe est créé. Le nom de l'objet de stratégie de groupe est recherché dans chaque domaine, puis renseigné avec les paramètres DirectAccess s'il existe. La cible du lien est définie à la racine du domaine dans lequel l'objet de stratégie de groupe a été créé. Un objet de stratégie de groupe est créé pour chaque domaine qui contient des ordinateurs clients, et il est lié à la racine de son domaine respectif.  
  
Quand vous utilisez des objets de stratégie de groupe créés automatiquement, pour appliquer les paramètres DirectAccess, l'administrateur du serveur DirectAccess requiert les autorisations suivantes :  
  
-   Autorisations de création d'objets de stratégie de groupe pour chaque domaine.  
  
-   Autorisations de lien pour toutes les racines de domaine client sélectionnées.  
  
-   Autorisations de lien pour les racines de domaine des objets de stratégie de groupe des serveurs.  
  
-   Autorisations de création, modification, suppression et modification de la sécurité pour les objets de stratégie de groupe.  
  
-   Nous recommandons à l'administrateur DirectAccess de posséder des autorisations de lecture des objets de stratégie de groupe pour chaque domaine requis. Ainsi, DirectAccess peut vérifier qu'il n'existe pas d'objets de stratégie de groupe portant le même nom lors de la création de ces derniers.  
  
Notez que si les autorisations adéquates pour la liaison des objets de stratégie de groupe n'existent pas, un avertissement est émis. DirectAccess continue de fonctionner mais aucune liaison ne se produit. Si cet avertissement est émis, les liens ne sont pas créés automatiquement, même si les autorisations sont ajoutées plus tard. L'administrateur doit alors créer les liens manuellement.  
  
#### <a name="manually-created-gpos"></a>Objets de stratégie de groupe @ no__t-0created manuellement  
Notez les points suivants lors de l’utilisation manuelle des objets de stratégie de groupe @ no__t-0created :  
  
-   Les objets de stratégie de groupe doivent déjà exister avant d'exécuter l'Assistant Mise en route de l'accès à distance.  
  
-   Lors de l’utilisation des objets de stratégie de groupe no__t-0created manuellement, pour appliquer les paramètres DirectAccess, l’administrateur DirectAccess requiert des autorisations complètes sur les objets de stratégie de groupe \(Edit, supprimer, modifier la sécurité @ no__t-2 sur les objets de stratégie de groupe @ no__t-3created manuellement.  
  
-   De plus, une recherche de lien vers l'objet de stratégie de groupe est effectuée dans tout le domaine. Si l'objet de stratégie de groupe n'est pas lié dans le domaine, alors un lien est automatiquement créé à la racine du domaine. Si les autorisations requises pour créer le lien ne sont pas disponibles, un avertissement est émis.  
  
Notez que si les autorisations adéquates pour la liaison des objets de stratégie de groupe n'existent pas, un avertissement est émis. DirectAccess continue de fonctionner mais aucune liaison ne se produit. Si cet avertissement est émis, les liens ne sont pas créés automatiquement, même si les autorisations sont ajoutées plus tard. L'administrateur doit alors créer les liens manuellement.  
  
#### <a name="recovering-from-a-deleted-gpo"></a>Récupération après la suppression d'un objet de stratégie de groupe  
Si un objet de stratégie de groupe de serveur, de client ou de serveur d’applications DirectAccess a été supprimé par inadvertance et qu’aucune sauvegarde n’est disponible, vous devez supprimer les paramètres de configuration et re @ no__t-0configure. Si vous disposez d'une sauvegarde, vous pouvez restaurer l'objet de stratégie de groupe.  
  
Le message d'erreur suivant d'affiche dans la **Gestion DirectAccess** : L' **objet de stratégie de groupe <GPO name> est introuvable**. Pour supprimer les paramètres de configuration, procédez comme suit :  
  
1.  Exécutez l’applet de commande PowerShell **Uninstall @ no__t-1remoteaccess**.  
  
2.  No__t-0open de **Gestion DirectAccess**.  
  
3.  Un message d'erreur indique que l'objet de stratégie de groupe est introuvable. Cliquez sur **Supprimer les paramètres de configuration**. Une fois l’opération terminée, le serveur est restauré à l’état non @ no__t-0configured.  
  
### <a name="BKMK_Links"></a>Étape suivante  
  
-   [Étape 2 : Planifier un déploiement de base de DirectAccess](da-basic-plan-s2-deployment.md)  
  
