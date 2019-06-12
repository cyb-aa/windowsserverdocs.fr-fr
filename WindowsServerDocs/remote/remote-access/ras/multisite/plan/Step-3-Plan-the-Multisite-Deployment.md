---
title: Plan de l’étape 3 du déploiement Multisite
description: Cette rubrique fait partie du guide de déploiement de plusieurs serveurs d’accès distant dans un déploiement Multisite dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e5ea9d22-a503-4ed4-96b3-0ee2ccf4fd17
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 29d52e57a18bf956d135179b503255efd256b35e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446854"
---
# <a name="step-3-plan-the-multisite-deployment"></a>Plan de l’étape 3 du déploiement Multisite

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Après la planification de l’infrastructure multisite, planifier des exigences de certificat supplémentaires, comment les ordinateurs clients pour sélectionner des points d’entrée et les adresses IPv6 affectées dans votre déploiement.  

Les sections suivantes fournissent des informations de planification détaillées.
  
## <a name="bkmk_3_1_IPHTTPS"></a>Certificats IP-HTTPS planifier 3.1  
Lorsque vous configurez les points d’entrée, vous configurez chaque point d’entrée avec une adresse ConnectTo spécifique. Le certificat IP-HTTPS pour chaque point d’entrée doit correspondre à l’adresse ConnectTo. Notez les points suivants lors de l’obtention du certificat :  
  
-   Vous ne pouvez pas utiliser des certificats auto-signés dans un déploiement multisite.  
  
-   Il est recommandé d'utiliser une autorité de certification publique, afin que les listes de révocation de certificats soient rapidement disponibles.  
  
-   Dans le champ objet, spécifiez soit l’adresse IPv4 de la carte externe du serveur d’accès à distance (si l’adresse ConnectTo a été spécifiée comme une adresse IP et pas un nom DNS), ou le nom de domaine complet de l’URL IP-HTTPS.  
  
-   Le nom commun du certificat doit correspondre au nom du site Web IP-HTTPS. Utilisation d’une URL générique qui correspond au nom DNS de ConnectTo est également prise en charge.  
  
-   Certificats IP-HTTPS peuvent utiliser des caractères génériques dans le nom du sujet. Le même certificat générique peut être utilisé pour tous les points d’entrée.  
  
-   Pour le champ Utilisation améliorée de la clé, utilisez l’identificateur d’objet (OID) d’authentification du serveur.  
  
-   Si vous prenez en charge des ordinateurs clients qui exécutent Windows 7 dans le déploiement multisite, dans le champ Points de Distribution CRL, spécifiez un point de distribution CRL qui est accessible par les clients DirectAccess qui sont connectés à Internet. Cela n’est pas nécessaire pour les clients qui exécutent Windows 8 (par défaut de que la vérification de révocation de révocation de certificats est désactivée pour IP-HTTPS sur ces clients).  
  
-   Le certificat IP-HTTPS doit avoir une clé privée.  
  
-   Le certificat IP-HTTPS doit être importé directement dans le magasin personnel de l’ordinateur et non de l’utilisateur.  
  
## <a name="bkmk_3_2_NLS"></a>3.2 planifier le serveur emplacement réseau  
Le site de serveur d’emplacement réseau peut être hébergé sur le serveur d’accès à distance ou sur un autre serveur dans votre organisation. Si vous hébergez le serveur d’emplacement réseau sur le serveur d’accès à distance, le site Web est créé automatiquement lorsque vous déployez l’accès à distance. Si vous hébergez le serveur d’emplacement réseau sur un autre serveur exécutant un système d’exploitation de Windows dans votre organisation, il se peut que vous devez vous assurer que les Internet Information Services (IIS) est installé pour créer le site Web.  
  
### <a name="321-certificate-requirements-for-the-network-location-server"></a>3.2.1 certificats requis pour le serveur emplacement réseau  
Assurez-vous que le site de serveur d’emplacement réseau respecte les exigences suivantes pour le déploiement du certificat :  
  
-   Elle nécessite un certificat de serveur HTTPS.  
  
-   Si le serveur emplacement réseau se trouve sur le serveur d’accès à distance et que vous avez sélectionné à utiliser un certificat auto-signé lorsque vous avez déployé le serveur d’accès à distance unique, vous devez reconfigurer le déploiement de serveur unique pour utiliser un certificat émis par une autorité de certification interne.  
  
-   Les ordinateurs clients DirectAccess doivent faire confiance à l'autorité de certification qui a émis le certificat de serveur pour le site web du serveur Emplacement réseau.  
  
-   Les ordinateurs clients DirectAccess sur le réseau interne doivent être en mesure de résoudre le nom du site de serveur emplacement réseau.  
  
-   Le site de serveur d’emplacement réseau doit être hautement disponible sur les ordinateurs sur le réseau interne.  
  
-   Le serveur Emplacement réseau ne doit pas être accessible aux ordinateurs clients DirectAccess figurant sur Internet.  
  
-   Le certificat de serveur doit être vérifié par rapport à une liste de révocation de certificats (CRL).  
  
-   Certificats avec caractères génériques ne sont pas pris en charge lorsque le serveur emplacement réseau est hébergé sur le serveur d’accès à distance.  
  
Lors de l’obtention du certificat de site Web à utiliser pour le serveur emplacement réseau, notez les points suivants :  
  
1.  Dans le champ Objet, indiquez une adresse IP de l'interface intranet du serveur d'emplacement réseau ou le nom de domaine complet de l'URL d'emplacement réseau. Notez que vous ne devez pas spécifier une adresse IP si le serveur emplacement réseau est hébergé sur le serveur d’accès à distance. Il s’agit, car le serveur emplacement réseau doit utiliser le même nom de sujet pour tous les points d’entrée, et pas tous les points d’entrée ont la même adresse IP.  
  
2.  Dans le champ Utilisation améliorée de la clé, utilisez l'identificateur d'objet (OID) d'authentification du serveur.  
  
3.  Pour le champ Points de Distribution CRL, utilisez un point de distribution CRL qui est accessible par les clients DirectAccess qui sont connectés à l’intranet.  
  
### <a name="322dns-for-the-network-location-server"></a>3.2.2DNS pour le serveur emplacement réseau  
Si vous hébergez le serveur d’emplacement réseau sur le serveur d’accès à distance, vous devez ajouter une entrée DNS pour le site de serveur d’emplacement réseau pour chaque point d’entrée dans votre déploiement. Notez les points suivants :  
  
-   Le nom de sujet du premier certificat de serveur emplacement réseau dans le déploiement multisite est utilisé en tant que l’URL du serveur emplacement réseau pour tous les points d’entrée, le nom du sujet et l’URL du serveur emplacement réseau ne peut donc être le même que le nom d’ordinateur de la premier serveur d’accès à distance dans le déploiement. Il doit être un nom de domaine complet dédié pour le serveur emplacement réseau.  
  
-   Le service fourni par le trafic de serveur d’emplacement réseau est équilibré entre les points d’entrée à l’aide de DNS, et par conséquent, il doit y avoir une entrée DNS avec la même URL pour chaque point d’entrée, configuré avec l’adresse IP interne du point d’entrée.  
  
-   Tous les points d’entrée doivent être configurés avec un certificat de serveur d’emplacement réseau portant le même nom de sujet (qui correspond à l’URL du serveur emplacement réseau).  
  
-   L’infrastructure de serveur d’emplacement réseau (paramètres DNS et de certificat) pour un point d’entrée doit être créé avant d’ajouter le point d’entrée.  
  
## <a name="bkmk_3_3_IPsec"></a>3.3 planifier le certificat racine IPsec pour tous les serveurs d’accès à distance  
Notez les points suivants lors de la planification pour l’authentification de client IPsec dans un déploiement multisite :  
  
1.  Si vous avez choisi d’utiliser le proxy Kerberos intégré pour l’authentification d’ordinateur lorsque vous configurez le serveur d’accès à distance unique, vous devez modifier le paramètre à utiliser des certificats d’ordinateur émis par une autorité de certification interne, dans la mesure où le proxy Kerberos n’est pas pris en charge pour un déploiement multisite déploiement.  
  
2.  Si vous avez utilisé un certificat auto-signé, vous devez reconfigurer le déploiement de serveur unique pour utiliser un certificat émis par une autorité de certification interne.  
  
3.  Pour l’authentification d’IPsec réussir pendant l’authentification du client, tous les serveurs d’accès à distance doivent avoir un certificat émis par la racine de IPsec ou d’une autorité de certification intermédiaire et avec l’OID d’authentification du Client pour l’utilisation améliorée de la clé.  
  
4.  Le même IPsec certificat racine ou intermédiaire doit être installé sur tous les serveurs d’accès à distance dans le déploiement multisite.  
  
## <a name="bkmk_3_4_GSLB"></a>3.4 planifier l’équilibrage de charge de serveur global  
Dans un déploiement multisite, vous pouvez en outre configurer un équilibreur de charge globaux du serveur. Un équilibreur de charge globaux du serveur peut être utile à votre organisation si votre déploiement couvre une grande distribution géographique, car il peut distribuer la charge du trafic entre les points d’entrée.  L’équilibreur de charge globaux du serveur peut être configuré pour fournir des clients DirectAccess avec les informations de point d’entrée le plus proche du point d’entrée. Le processus fonctionne comme suit :  
  
1.  Les ordinateurs clients exécutant Windows 10 ou Windows 8 ont une liste d’adresses IP d’équilibrage de charge globaux du serveur, chacun étant associé à un point d’entrée.  
  
2.  L’ordinateur client Windows 10 ou Windows 8 essaie de résoudre le nom de domaine complet de l’équilibreur de charge globaux du serveur dans le DNS public en une adresse IP. Si l’adresse IP résolue est répertorié comme adresse IP serveur global équilibreur de charge d’un point d’entrée, l’ordinateur client sélectionne ce point d’entrée automatiquement et se connecte à son URL IP-HTTPS (adresse ConnectTo), ou à son adresse IP du serveur Teredo. Notez que l’adresse IP de l’équilibreur de charge globaux du serveur ne doit pas être identique à l’adresse ConnectTo ou à l’adresse du serveur Teredo du point d’entrée, étant donné que les ordinateurs clients n’essaient jamais se connecter à l’adresse IP d’équilibrage de charge globaux du serveur.  
  
3.  Si l’ordinateur client se trouve derrière un proxy web (et ne peut pas utiliser la résolution DNS), ou si l’équilibreur de charge globaux du serveur nom de domaine complet n’aboutit pas à un configurés globaux du serveur load balancer adresse IP, puis un point d’entrée sera sélectionné automatiquement à l’aide d’une sonde HTTPS les URL IP-HTTPS de tous les points d’entrée. Le client se connecte au serveur qui répond en premier.  
  
Pour obtenir la liste des appareils qui prennent en charge l’accès à distance d’équilibrage de charge globaux du serveur, accédez à la rechercher une page de partenaire à [Microsoft Server et la plateforme Cloud](https://www.microsoft.com/server-cloud/).  
  
## <a name="bkmk_3_5_EP_Selection"></a>3.5 planifier la sélection de point d’entrée DirectAccess client  
Lorsque vous configurez un déploiement multisite, par défaut, Windows 10 et les ordinateurs clients Windows 8 sont configurés avec les informations requises pour se connecter à tous les points d’entrée dans le déploiement et se connecter automatiquement à un point d’entrée unique basé sur une sélection algorithme. Vous pouvez également configurer votre déploiement pour permettre à Windows 10 et les ordinateurs clients Windows 8 afin de sélectionner manuellement l’entrée pointent vers lequel ils se connectent. Si un ordinateur client Windows 10 ou Windows 8 est actuellement connecté au point d’entrée des États-Unis et la sélection de point de saisie automatique est activée, si le point d’entrée United States devient inaccessible, après quelques minutes l’ordinateur client tentent de se connecter via le point d’entrée en Europe. À l’aide de la sélection de point d’entrée automatique est recommandé ; Toutefois, permettant une saisie manuelle sélection permet aux utilisateurs finaux de se connecter à un point d’entrée différents selon les conditions actuelles du réseau. Par exemple, si un ordinateur est connecté au point d’entrée des États-Unis et de la connexion au réseau interne devient beaucoup plus lent que prévu. Dans ce cas, l’utilisateur final peut sélectionner manuellement pour se connecter au point d’entrée en Europe pour améliorer la connexion au réseau interne.  
  
> [!NOTE]  
> Une fois un utilisateur final sélectionne un point d’entrée manuellement, l’ordinateur client ne reviendra pas à la sélection de point d’entrée automatique. Autrement dit, si le point d’entrée sélectionnées manuellement devient inaccessible, l’utilisateur final doit soit revenir à la sélection de point d’entrée automatique, ou sélectionner manuellement un autre point d’entrée.  
  
 Les ordinateurs clients Windows 7 sont configurés avec les informations requises pour se connecter à un point d’entrée unique dans le déploiement multisite. Ils ne peuvent pas stocker les informations de plusieurs points d’entrée simultanément. Par exemple, un ordinateur client de Windows 7 peut être configuré pour se connecter au point d’entrée des États-Unis, mais pas le point d’entrée en Europe. Si le point d’entrée United States est inaccessible, l’ordinateur client Windows 7 perdent la connectivité au réseau interne jusqu'à ce que le point d’entrée est accessible. L’utilisateur final ne peut pas effectuer toute modification apportée à essayer de se connecter au point d’entrée en Europe.  
  
## <a name="bkmk_3_6_IPv6"></a>3.6 planifier les préfixes et le routage  
  
### <a name="internal-ipv6-prefix"></a>Préfixe IPv6 interne  
Lors du déploiement de l’accès à distance unique serveur prévu des préfixes IPv6 de réseau interne Notez ce qui suit dans un déploiement multisite :  
  
1.  Si vous avez inclus tous vos sites Active Directory lorsque vous avez configuré votre déploiement de l’accès à distance de serveur unique, les préfixes IPv6 de réseau interne seront déjà être définies dans la console de gestion de l’accès à distance.  
  
2.  Si vous créez des sites Active Directory supplémentaires pour le déploiement multisite, vous devez planifier de nouveaux préfixes IPv6 pour les sites supplémentaires et les définir l’accès à distance. Notez que les préfixes IPv6 ne peuvent être configurés à l’aide de la console de gestion de l’accès à distance ou les applets de commande PowerShell si IPv6 est déployé dans le réseau d’entreprise interne.  
  
### <a name="ipv6-prefix-for-directaccess-client-computers-ip-https-prefix"></a>Préfixe IPv6 pour les ordinateurs clients DirectAccess (préfixe IP-HTTPS)  
  
1.  Si IPv6 est déployé dans le réseau d’entreprise interne, vous devez planifier un préfixe IPv6 à affecter aux ordinateurs de clients DirectAccess dans des points d’entrée supplémentaires dans votre déploiement.  
  
2.  Assurez-vous que les préfixes IPv6 à affecter aux ordinateurs de clients DirectAccess dans chaque point d’entrée sont distincts et qu’il n’existe pas de chevauchement dans les préfixes IPv6.  
  
3.  Si IPv6 n’est pas déployé dans le réseau d’entreprise, un préfixe IP-HTTPS pour chaque point d’entrée est automatiquement sélectionné lorsque vous ajoutez le point d’entrée.  
  
### <a name="ipv6-prefix-for-vpn-clients"></a>Préfixe IPv6 pour les clients VPN  
Si vous avez déployé des VPN sur le serveur d’accès à distance unique, notez les points suivants :  
  
1.  L’ajout d’un VPN IPv6 préfixe à un point d’entrée n’est requis si vous souhaitez autoriser les clients VPN connectivité IPv6 au réseau d’entreprise.  
  
2.  Le préfixe VPN peut uniquement être configuré sur un point d’entrée à l’aide de la console de gestion de l’accès à distance ou d’une applet de commande PowerShell si IPv6 est déployé dans le réseau d’entreprise interne, et si VPN est activé sur le point d’entrée.  
  
3.  Le préfixe VPN doit être unique dans chaque point d’entrée et ne doit pas se chevaucher avec d’autres préfixes VPN ou IP-HTTPS.  
  
4.  Si IPv6 n’est pas déployé dans le réseau d’entreprise, les clients VPN en se connectant au point d’entrée ne seront pas affectés une adresse IPv6.  
  
### <a name="routing"></a>Routage  
Dans un déploiement multisite le routage symétrique est appliqué à l’aide de Teredo et IP-HTTPS. Lorsque le protocole IPv6 est déployé dans le réseau d’entreprise, notez les points suivants :  
  
1. Les préfixes de Teredo et IP-HTTPS de chaque point d’entrée doivent être routables sur le réseau d’entreprise à leur serveur d’accès à distance associé.  
  
2. Les itinéraires doivent être configurées dans l’infrastructure de routage du réseau d’entreprise.  
  
3. Pour chaque point d’entrée du réseau interne doit être un à trois routes :  
  
   1. Préfixe de l’ordinateur ce préfixe IP-HTTPS est choisi par l’administrateur dans l’ajout d’un Assistant de Point d’entrée.  
  
   2. Préfixe IPv6 de VPN (facultatif). Ce préfixe peut être choisi après l’activation de VPN pour un point d’entrée  
  
   3. Préfixe de Teredo (facultatif). Ce préfixe est pertinent uniquement si le serveur d’accès à distance est configuré avec deux adresses IPv4 publiques consécutives sur l’adaptateur externe. Le préfixe est basé sur la première adresse IPv4 publique de la paire d’adresses. Par exemple, si les adresses externes sont :  
  
      1. www.xxx.yyy.zzz  
  
      2. www.xxx.yyy.zzz+1  
  
      Le préfixe de Teredo à configurer est 2001:0:WWXX:YYZZ :: / 64, où WWXX : YYZZ est la représentation hexadécimale de la www.xxx.yyy.zzz adresse IPv4.  
  
      Notez que vous pouvez utiliser le script suivant pour calculer le préfixe Teredo :  
  
      ```  
      $TeredoIPv4 = (Get-NetTeredoConfiguration).ServerName # Use for a Remote Access server that is already configured  
      $TeredoIPv4 = "20.0.0.1" # Use for an IPv4 address  
  
          [Byte[]] $TeredoServerAddressBytes = `  
          [System.Net.IPAddress]::Parse("2001::").GetAddressBytes()[0..3] + `  
          [System.Net.IPAddress]::Parse($TeredoIPv4).GetAddressBytes() + `  
          [System.Net.IPAddress]::Parse("::").GetAddressBytes()[0..7]  
  
      Write-Host "The server's Teredo prefix is $([System.Net.IPAddress]$TeredoServerAddressBytes)/64"  
      ```  
  
   4. Tous les itinéraires ci-dessus doivent être acheminées vers l’adresse IPv6 sur la carte réseau interne du serveur d’accès à distance (ou à l’adresse IP (VIP) virtuelle interne pour une charge équilibrée point d’entrée).  
  
> [!NOTE]  
> Lorsque le protocole IPv6 est déployé dans le réseau d’entreprise et l’administration de serveur d’accès à distance est effectué à distance via DirectAccess, les itinéraires pour le Teredo et préfixes IP-HTTPS de tous les autres points d’entrée doivent être ajoutés à chaque serveur d’accès à distance afin que le trafic sera transféré au réseau interne.  
  
### <a name="active-directory-site-specific-ipv6-prefixes"></a>Préfixes IPv6 de spécifique à un site Active Directory  
Lorsqu’un ordinateur client exécutant Windows 10 ou Windows 8 est connecté à un point d’entrée, l’ordinateur client est immédiatement associé au site Active Directory du point d’entrée et est configuré avec les préfixes IPv6 associées au point d’entrée. La préférence est pour les ordinateurs clients pour se connecter à des ressources à l’aide de ces préfixes IPv6, dans la mesure où ils sont configurés dynamiquement dans la table de stratégie de préfixe IPv6 avec une priorité plus élevée lors de la connexion à un point d’entrée.  
  
Si votre organisation utilise une topologie Active Directory avec les préfixes IPv6 spécifiques au site (par exemple un app.corp.com de nom de domaine complet de ressource interne est hébergé en Amérique du Nord et en Europe avec une adresse IP spécifique dans chaque emplacement) Ceci n’est pas configuré par par défaut à l’aide de la console de l’accès à distance et les préfixes IPv6 spécifiques au site n’est pas configuré pour chaque point d’entrée. Si vous ne souhaitez pas activer ce scénario facultatif, vous devez configurer chaque point d’entrée avec les préfixes IPv6 spécifiques qui doivent être utilisés dans les ordinateurs clients se connectant à un point d’entrée spécifique. Procédez comme suit :  
  
1.  Pour chaque objet stratégie de groupe utilisé pour Windows 10 ou les ordinateurs clients Windows 8, exécutez l’applet de commande Set-DAEntryPointTableItem PowerShell  
  
2.  Définir le paramètre EntryPointRange pour l’applet de commande avec les préfixes IPv6 spécifiques au site. Par exemple, pour ajouter le site préfixes 2001:db8:1:1 :: / 64 et 2001:db:1:2 :: / 64 pour un point d’entrée appelé Europe, exécutez la commande suivante  
  
    ```  
    $entryPointName = "Europe"  
    $prefixesToAdd = @("2001:db8:1:1::/64", "2001:db8:1:2::/64")  
    $clientGpos = (Get-DAClient).GpoName  
    $clientGpos | % { Get-DAEntryPointTableItem -EntryPointName $entryPointName -PolicyStore $_ | %{ Set-DAEntryPointTableItem -PolicyStore $_.PolicyStore -EntryPointName $_.EntryPointName -EntryPointRange ($_.EntryPointRange) + $prefixesToAdd}}  
    ```  
  
3.  Lorsque vous modifiez le paramètre EntryPointRange, assurez-vous que vous ne supprimez pas les préfixes de 128 bits existants qui appartiennent à des points de terminaison du tunnel IPsec et l’adresse DNS64.  
  
## <a name="bkmk_3_7_TransitionIPv6"></a>3.7 planifier la transition vers IPv6 lorsque l’accès à distance multisite est déployé  
De nombreuses organisations utilisent le protocole IPv4 sur le réseau d’entreprise. Avec l’épuisement des préfixes IPv4 disponibles, de nombreuses organisations font la transition d’IPv4 uniquement à des réseaux IPv6 uniquement.  
  
Cette transition est probablement se déroule en deux étapes :  
  
1.  À partir d’un IPv4 uniquement à un réseau d’entreprise IPv6 + IPv4.  
  
2.  À partir d’IPv6 + IPv4 vers un réseau d’entreprise IPv6 uniquement.  
  
Dans chaque partie, la transition peut être effectuée par étapes. Dans chaque phase qu’un seul sous-réseau du réseau peut être modifié pour la nouvelle configuration de réseau. Par conséquent, un déploiement multisite DirectAccess est requis pour prendre en charge un déploiement hybride où, par exemple, certains des points d’entrée appartient à un sous-réseau IPv4 uniquement et d’autres utilisateurs appartiennent à un sous-réseau IPv6 + IPv4. En outre, les modifications de configuration pendant les processus de transition ne doivent pas interrompre la connectivité de client par le biais de DirectAccess.  
  
### <a name="TransitionIPv4toMixed"></a>Effectuer une transition entre un IPv4 uniquement à un réseau d’entreprise IPv6 + IPv4  
Lorsque vous ajoutez des adresses IPv6 à un réseau d’entreprise IPv4 uniquement, vous souhaiterez ajouter une adresse IPv6 à un serveur DirectAccess déjà déployé. En outre, vous souhaiterez ajouter un point d’entrée ou un nœud à un cluster à charge équilibrée de la charge avec les adresses IPv4 et IPv6 pour le déploiement de DirectAccess.  
  
Accès à distance vous permet de vous permet d’ajouter des serveurs avec des adresses IPv4 et IPv6 pour un déploiement qui a été initialement configuré avec des adresses IPv4 uniquement. Ces serveurs sont ajoutés en tant que serveurs IPv4 uniquement et leurs adresses IPv6 sont ignorés par DirectAccess ; Par conséquent, votre organisation ne peut pas tirer parti des avantages de la connectivité IPv6 native sur ces nouveaux serveurs.  
  
Pour transformer le déploiement à un déploiement IPv6 + IPv4 et tirer parti des fonctionnalités d’IPv6 natives, vous devez réinstaller DirectAccess. Pour maintenir la connectivité des clients tout au long de la réinstallation voir Transition à partir d’un IPv4 uniquement à un déploiement IPv6 uniquement à l’aide de deux déploiements de DirectAccess.  
  
> [!NOTE]  
> Comme avec un réseau IPv4 uniquement, dans un réseau IPv4 + IPv6 mixte, l’adresse du serveur DNS qui est utilisé pour résoudre les demandes de client DNS doit être configuré avec DNS64 qui est déployé sur les serveurs d’accès à distance proprement dits et non avec un nom DNS d’entreprise.  
  
### <a name="TransitionMixedtoIPv6"></a>Effectuer une transition entre IPv6 + IPv4 à un réseau d’entreprise IPv6 uniquement  
DirectAccess permet d’ajouter des points d’entrée uniquement IPv6 uniquement si le premier serveur d’accès à distance dans le déploiement initial était à la fois IPv4 et des adresses IPv6 ou uniquement une adresse IPv6. Autrement dit, vous ne peut pas effectuer une transition entre un réseau exclusivement IPv4 vers un réseau IPv6 uniquement dans une seule étape sans avoir à réinstaller DirectAccess. Pour passer directement à partir d’un réseau IPv4 uniquement à un réseau IPv6 uniquement, voir la Transition à partir d’un IPv4 uniquement à un déploiement IPv6 uniquement à l’aide de deux déploiements de DirectAccess.  
  
Une fois que vous avez terminé la transition à partir d’un déploiement IPv4 uniquement à un déploiement IPv6 + IPv4, vous pouvez passer à un réseau IPv6 uniquement. Pendant et après la transition, notez les points suivants :  
  
-   Si tous les serveurs backend IPv4 uniquement restent dans le réseau d’entreprise, ils ne seront pas accessibles aux clients qui se connectent via les points d’entrée IPv6 uniquement.  
  
-   Lorsque vous ajoutez des points d’entrée IPv6 uniquement à un déploiement IPv4 + IPv6, DNS64 et NAT64 ne seront pas activé sur les nouveaux serveurs. Les clients qui se connectent à ces points d’entrée seront être automatiquement configurés pour utiliser les serveurs DNS d’entreprise.  
  
-   Si vous avez besoin de supprimer des adresses IPv4 à partir d’un serveur déployé, vous devez supprimer le serveur du déploiement de DirectAccess, supprimez son adresse de réseau d’entreprise IPv4 et ajoutez-le à nouveau au déploiement.  
  
Pour prendre en charge la connectivité du client au réseau d’entreprise, vous devez vous assurer que le serveur emplacement réseau peut être résolu par DNS d’entreprise et son adresse IPv6. Une adresse IPv4 supplémentaire peut être également définie, mais n’est pas nécessaire.  
  
### <a name="DualDeployment"></a>Effectuer une transition entre un IPv4 uniquement à un déploiement IPv6 uniquement à l’aide de deux déploiements de DirectAccess  
La transition à partir d’un IPv4 uniquement à un réseau d’entreprise IPv6 uniquement ne peut pas être effectuée sans avoir à réinstaller le déploiement de DirectAccess. Pour maintenir la connectivité des clients pendant la transition, vous pouvez utiliser un autre déploiement de DirectAccess. Déploiement double est requis lorsque la première phase de transition termine (réseau IPv4 uniquement mis à niveau vers IPv4 + IPv6) et que vous souhaitez préparer pour une future transition vers un IPv6 uniquement réseau d’entreprise orto tirer parti des avantages de connectivité IPv6 natives. Le déploiement double est décrit dans les étapes générales suivantes :  
  
1.  Installer un deuxième déploiement DirectAccess. Vous pouvez installer DirectAccess sur les nouveaux serveurs, ou supprimer des serveurs à partir du premier déploiement et les utiliser pour le deuxième déploiement.  
  
    > [!NOTE]  
    > Lorsque vous installez un déploiement DirectAccess supplémentaires en même temps qu’actuel, assurez-vous qu’aucun point de deux entrée ne partage le même préfixe de client.  
    >   
    > Si vous installez à l’aide de l’Assistant Mise en route de DirectAccess, ou avec l’applet de commande `Install-RemoteAccess`, accès à distance définit automatiquement le préfixe de client du premier point d’entrée dans le déploiement vers une valeur par défaut de < sous-réseau IPv6\_préfixe > : 1000 :: / 64. Si nécessaire, vous devez modifier le préfixe.  
  
2.  Supprimez les groupes de sécurité du client choisi le premier déploiement.  
  
3.  Ajoutez les groupes de sécurité client pour le deuxième déploiement.  
  
    > [!IMPORTANT]  
    > Pour maintenir la connectivité client au cours du processus, vous devez ajouter les groupes de sécurité pour le deuxième déploiement immédiatement après leur suppression de la première. Cela garantit que les clients ne seront pas mis à jour avec deux ou aucune stratégie de groupe DirectAccess. Les clients commence à utiliser le deuxième déploiement une fois qu’ils récupèrent et mettre à jour leur client de stratégie de groupe.  
  
4.  Facultatif : Supprimer les points d’entrée DirectAccess à partir du premier déploiement et ajouter ces serveurs en tant que de nouveaux points d’entrée dans la seconde.  
  
Lorsque vous avez terminé la transition, vous pouvez désinstaller le premier déploiement de DirectAccess. Lors de la désinstallation, les problèmes suivants peuvent se produire :  
  
-   Si le déploiement a été configuré pour prendre en charge uniquement les clients sur des ordinateurs portables, le filtre WMI est supprimé. Si les groupes de sécurité de client du deuxième déploiement incluent des ordinateurs de bureau, le client DirectAccess GPO ne filtrera pas les ordinateurs de bureau et peut entraîner des problèmes sur ces derniers. Si un filtre d’ordinateurs portables est nécessaire, recréez l’en suivant les instructions de [créer des filtres WMI pour l’objet de stratégie de groupe](https://technet.microsoft.com/library/cc947846.aspx).  
  
-   Si les deux déploiements ont été initialement créés sur le même domaine Active Directory, le DNS de sonde entrée qui pointe vers localhost sera supprimé et peut provoquer des problèmes de connectivité client. Par exemple, les clients peuvent se connecter à l’aide d’IP-HTTPS au lieu de Teredo, ou basculer entre les points d’entrée multisite DirectAccess. Dans ce cas, vous devez ajouter l’entrée DNS suivante au DNS d’entreprise :  
  
    -   Zone : nom de domaine  
  
    -   Nom : directaccess-corpConnectivityHost  
  
    -   Adresse IP : :: 1  
  
    -   Tapez : AAAA  
  
  
  


