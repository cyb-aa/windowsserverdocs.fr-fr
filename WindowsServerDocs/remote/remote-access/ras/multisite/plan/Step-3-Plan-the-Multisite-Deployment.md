---
title: Étape 3 planifier le déploiement multisite
description: Cette rubrique fait partie du guide déployer plusieurs serveurs d’accès à distance dans un déploiement multisite dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e5ea9d22-a503-4ed4-96b3-0ee2ccf4fd17
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1320a5c8b8c267f270dae43e764533d9289006a4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404454"
---
# <a name="step-3-plan-the-multisite-deployment"></a>Étape 3 planifier le déploiement multisite

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Après avoir planifié l’infrastructure multisite, planifiez les exigences de certificat supplémentaires, comment les ordinateurs clients sélectionnent les points d’entrée et les adresses IPv6 affectées dans votre déploiement.  

Les sections suivantes fournissent des informations détaillées sur la planification.
  
## <a name="bkmk_3_1_IPHTTPS"></a>3,1 planifier des certificats IP-HTTPs  
Quand vous configurez les points d’entrée, vous configurez chaque point d’entrée avec une adresse ConnectTo spécifique. Le certificat IP-HTTPs pour chaque point d’entrée doit correspondre à l’adresse ConnectTo. Notez les points suivants lors de l’obtention du certificat :  
  
-   Vous ne pouvez pas utiliser des certificats auto-signés dans un déploiement multisite.  
  
-   Il est recommandé d'utiliser une autorité de certification publique, afin que les listes de révocation de certificats soient rapidement disponibles.  
  
-   Dans le champ objet, spécifiez l’adresse IPv4 de la carte externe du serveur d’accès à distance (si l’adresse ConnectTo a été spécifiée comme une adresse IP et non un nom DNS) ou le nom de domaine complet (FQDN) de l’URL IP-HTTPs.  
  
-   Le nom commun du certificat doit correspondre au nom du site Web IP-HTTPs. L’utilisation d’une URL générique qui correspond au nom DNS ConnectTo est également prise en charge.  
  
-   Les certificats IP-HTTPs peuvent utiliser des caractères génériques dans le nom de l’objet. Le même certificat générique peut être utilisé pour tous les points d’entrée.  
  
-   Pour le champ Utilisation améliorée de la clé, utilisez l’identificateur d’objet (OID) d’authentification du serveur.  
  
-   Si vous prenez en charge des ordinateurs clients exécutant Windows 7 dans le déploiement multisite, dans le champ points de distribution de la liste de révocation de certificats, spécifiez un point de distribution de liste de révocation de certificats accessible par les clients DirectAccess qui sont connectés à Internet. Cela n’est pas nécessaire pour les clients exécutant Windows 8 (par défaut, la vérification de la révocation de la liste de révocation des certificats est désactivée pour IP-HTTPs sur ces clients).  
  
-   Le certificat IP-HTTPS doit avoir une clé privée.  
  
-   Le certificat IP-HTTPs doit être importé directement dans le magasin personnel de l’ordinateur et non de l’utilisateur.  
  
## <a name="bkmk_3_2_NLS"></a>3,2 planifier le serveur emplacement réseau  
Le site Web du serveur emplacement réseau peut être hébergé sur le serveur d’accès à distance ou sur un autre serveur de votre organisation. Si vous hébergez le serveur emplacement réseau sur le serveur d’accès à distance, le site Web est créé automatiquement lorsque vous déployez l’accès à distance. Si vous hébergez le serveur emplacement réseau sur un autre serveur exécutant un système d’exploitation Windows dans votre organisation, vous devez vous assurer que Internet Information Services (IIS) est installé pour créer le site Web.  
  
### <a name="321-certificate-requirements-for-the-network-location-server"></a>3.2.1 exigences relatives aux certificats pour le serveur emplacement réseau  
Assurez-vous que le site Web du serveur emplacement réseau répond aux conditions requises suivantes pour le déploiement des certificats :  
  
-   Il requiert un certificat de serveur HTTPs.  
  
-   Si le serveur emplacement réseau se trouve sur le serveur d’accès à distance et que vous avez choisi d’utiliser un certificat auto-signé lors du déploiement du serveur d’accès à distance unique, vous devez reconfigurer le déploiement sur un seul serveur pour utiliser un certificat émis par une autorité de certification interne.  
  
-   Les ordinateurs clients DirectAccess doivent faire confiance à l'autorité de certification qui a émis le certificat de serveur pour le site web du serveur Emplacement réseau.  
  
-   Les ordinateurs clients DirectAccess sur le réseau interne doivent être en mesure de résoudre le nom du site Web du serveur d’emplacement réseau.  
  
-   Le site Web du serveur emplacement réseau doit être hautement disponible pour les ordinateurs du réseau interne.  
  
-   Le serveur Emplacement réseau ne doit pas être accessible aux ordinateurs clients DirectAccess figurant sur Internet.  
  
-   Le certificat de serveur doit être vérifié par rapport à une liste de révocation de certificats (CRL).  
  
-   Les certificats avec caractères génériques ne sont pas pris en charge lorsque le serveur emplacement réseau est hébergé sur le serveur d’accès à distance.  
  
Lorsque vous obtenez le certificat de site Web à utiliser pour le serveur emplacement réseau, notez les points suivants :  
  
1.  Dans le champ Objet, indiquez une adresse IP de l'interface intranet du serveur d'emplacement réseau ou le nom de domaine complet de l'URL d'emplacement réseau. Notez que vous ne devez pas spécifier d’adresse IP si le serveur emplacement réseau est hébergé sur le serveur d’accès à distance. Cela est dû au fait que le serveur emplacement réseau doit utiliser le même nom de sujet pour tous les points d’entrée, et que tous les points d’entrée n’ont pas la même adresse IP.  
  
2.  Dans le champ Utilisation améliorée de la clé, utilisez l'identificateur d'objet (OID) d'authentification du serveur.  
  
3.  Pour le champ points de distribution de la liste de révocation de certificats, utilisez un point de distribution de liste de révocation de certificats accessible par les clients DirectAccess connectés à l’intranet.  
  
### <a name="322dns-for-the-network-location-server"></a>3.2.2 DNS pour le serveur emplacement réseau  
Si vous hébergez le serveur emplacement réseau sur le serveur d’accès à distance, vous devez ajouter une entrée DNS pour le site Web du serveur emplacement réseau pour chaque point d’entrée de votre déploiement. Notez les éléments suivants :  
  
-   Le nom du sujet du premier certificat de serveur d’emplacement réseau dans le déploiement multisite est utilisé comme URL du serveur d’emplacement réseau pour tous les points d’entrée. par conséquent, le nom du sujet et l’URL du serveur d’emplacement réseau ne peuvent pas être identiques au nom d’ordinateur du premier serveur d’accès à distance dans le déploiement. Il doit s’agir d’un nom de domaine complet dédié pour le serveur emplacement réseau.  
  
-   Le service fourni par le trafic du serveur d’emplacement réseau est équilibré entre les points d’entrée utilisant DNS, et par conséquent, il doit y avoir une entrée DNS avec la même URL pour chaque point d’entrée, configuré avec l’adresse IP interne du point d’entrée.  
  
-   Tous les points d’entrée doivent être configurés avec un certificat de serveur emplacement réseau portant le même nom d’objet (qui correspond à l’URL du serveur emplacement réseau).  
  
-   L’infrastructure de serveur emplacement réseau (paramètres DNS et de certificat) pour un point d’entrée doit être créée avant l’ajout du point d’entrée.  
  
## <a name="bkmk_3_3_IPsec"></a>3,3 planifier le certificat racine IPsec pour tous les serveurs d’accès à distance  
Notez les points suivants lors de la planification de l’authentification du client IPsec dans un déploiement multisite :  
  
1.  Si vous avez choisi d’utiliser le proxy Kerberos intégré pour l’authentification de l’ordinateur lorsque vous configurez le serveur d’accès à distance unique, vous devez modifier le paramètre pour utiliser des certificats d’ordinateur émis par une autorité de certification interne, puisque le proxy Kerberos n’est pas pris en charge pour un multisite déploiement.  
  
2.  Si vous avez utilisé un certificat auto-signé, vous devez reconfigurer le déploiement sur un seul serveur pour utiliser un certificat émis par une autorité de certification interne.  
  
3.  Pour que l’authentification IPsec aboutisse lors de l’authentification du client, tous les serveurs d’accès à distance doivent disposer d’un certificat émis par l’autorité de certification racine ou intermédiaire IPsec, et avec l’OID d’authentification du client pour une utilisation améliorée de la clé.  
  
4.  La même racine IPsec ou le même certificat intermédiaire doit être installé sur tous les serveurs d’accès à distance dans le déploiement multisite.  
  
## <a name="bkmk_3_4_GSLB"></a>3,4 planifier l’équilibrage de charge du serveur global  
Dans un déploiement multisite, vous pouvez également configurer un équilibreur de charge de serveur global. Un équilibreur de charge de serveur global peut être utile à votre organisation si votre déploiement couvre une grande distribution géographique, car il peut répartir la charge du trafic entre les points d’entrée.  L’équilibrage de charge du serveur global peut être configuré pour fournir aux clients DirectAccess les informations de point d’entrée du point d’entrée le plus proche. Le processus fonctionne comme suit :  
  
1.  Les ordinateurs clients qui exécutent Windows 10 ou Windows 8 possèdent une liste d’adresses IP de l’équilibreur de charge du serveur global, chacune associée à un point d’entrée.  
  
2.  L’ordinateur client Windows 10 ou Windows 8 tente de résoudre le nom de domaine complet de l’équilibreur de charge du serveur global dans le DNS public en une adresse IP. Si l’adresse IP résolue est indiquée en tant qu’adresse IP de l’équilibreur de charge du serveur global d’un point d’entrée, l’ordinateur client sélectionne automatiquement ce point d’entrée et se connecte à son URL IP-HTTPs (adresse ConnectTo) ou à son adresse IP du serveur Teredo. Notez que l’adresse IP de l’équilibreur de charge du serveur global ne doit pas nécessairement être identique à l’adresse ConnectTo ou à l’adresse du serveur Teredo du point d’entrée, car les ordinateurs clients n’essaient jamais de se connecter à l’adresse IP de l’équilibreur de charge du serveur global.  
  
3.  Si l’ordinateur client se trouve derrière un proxy Web (et ne peut pas utiliser la résolution DNS), ou si le nom de domaine complet de l’équilibreur de charge du serveur global n’est pas résolu en une adresse IP d’équilibrage de charge du serveur global configurée, un point d’entrée est sélectionné automatiquement à l’aide d’une sonde HTTPs pour URL IP-HTTPs de tous les points d’entrée. Le client se connecte au serveur qui répond en premier.  
  
Pour obtenir la liste des périphériques d’équilibrage de charge de serveur globaux qui prennent en charge l’accès à distance, accédez à la page Rechercher un partenaire sur [Microsoft Server et la plateforme Cloud](https://www.microsoft.com/server-cloud/).  
  
## <a name="bkmk_3_5_EP_Selection"></a>3,5 planifier la sélection du point d’entrée du client DirectAccess  
Quand vous configurez un déploiement multisite, par défaut, les ordinateurs clients Windows 10 et Windows 8 sont configurés avec les informations requises pour se connecter à tous les points d’entrée du déploiement et pour se connecter automatiquement à un point d’entrée unique à partir d’une sélection utilisé. Vous pouvez également configurer votre déploiement pour autoriser les ordinateurs clients Windows 10 et Windows 8 à sélectionner manuellement le point d’entrée auquel ils se connecteront. Si un ordinateur client Windows 10 ou Windows 8 est actuellement connecté au point d’entrée États-Unis et que la sélection de point d’entrée automatique est activée, si le point d’entrée de États-Unis devient inaccessible, après quelques minutes, l’ordinateur client tentera de se connecter. via le point d’entrée de l’Europe. Il est recommandé d’utiliser la sélection automatique des points d’entrée. Toutefois, le fait d’autoriser la sélection manuelle des points d’entrée permet aux utilisateurs finaux de se connecter à un autre point d’entrée en fonction des conditions de réseau actuelles. Par exemple, si un ordinateur est connecté au point d’entrée États-Unis et que la connexion au réseau interne devient beaucoup plus lente que prévu. Dans ce cas, l’utilisateur final peut choisir manuellement de se connecter au point d’entrée Europe pour améliorer la connexion au réseau interne.  
  
> [!NOTE]  
> Une fois qu’un utilisateur final a sélectionné un point d’entrée manuellement, l’ordinateur client ne reviendra pas à la sélection automatique des points d’entrée. Autrement dit, si le point d’entrée sélectionné manuellement devient inaccessible, l’utilisateur final doit revenir à la sélection automatique des points d’entrée ou sélectionner manuellement un autre point d’entrée.  
  
 Les ordinateurs clients Windows 7 sont configurés avec les informations requises pour se connecter à un point d’entrée unique dans le déploiement multisite. Ils ne peuvent pas stocker les informations de plusieurs points d’entrée simultanément. Par exemple, un ordinateur client Windows 7 peut être configuré pour se connecter au point d’entrée États-Unis, mais pas au point d’entrée de l’Europe. Si le point d’entrée États-Unis est inaccessible, l’ordinateur client Windows 7 perd la connectivité au réseau interne jusqu’à ce que le point d’entrée soit accessible. L’utilisateur final ne peut pas apporter de modifications pour tenter de se connecter au point d’entrée Europe.  
  
## <a name="bkmk_3_6_IPv6"></a>3,6 planifier les préfixes et le routage  
  
### <a name="internal-ipv6-prefix"></a>Préfixe IPv6 interne  
Lors du déploiement du serveur d’accès à distance unique, vous avez planifié les préfixes IPv6 du réseau interne. Notez les points suivants dans un déploiement multisite :  
  
1.  Si vous avez inclus tous vos sites Active Directory lors de la configuration de votre déploiement de l’accès à distance à serveur unique, les préfixes IPv6 de réseau interne seront déjà définis dans la console de gestion de l’accès à distance.  
  
2.  Si vous créez des sites de Active Directory supplémentaires pour le déploiement multisite, vous devez planifier de nouveaux préfixes IPv6 pour les sites supplémentaires et les définir dans l’accès à distance. Notez que les préfixes IPv6 peuvent être configurés uniquement à l’aide de la console de gestion de l’accès à distance ou des applets de commande PowerShell si IPv6 est déployé sur le réseau interne de l’entreprise.  
  
### <a name="ipv6-prefix-for-directaccess-client-computers-ip-https-prefix"></a>Préfixe IPv6 pour les ordinateurs clients DirectAccess (préfixe IP-HTTPs)  
  
1.  Si le protocole IPv6 est déployé dans le réseau interne de l’entreprise, vous devez planifier un préfixe IPv6 à attribuer aux ordinateurs clients DirectAccess dans n’importe quel point d’entrée supplémentaire de votre déploiement.  
  
2.  Assurez-vous que les préfixes IPv6 à attribuer aux ordinateurs clients DirectAccess dans chaque point d’entrée sont distincts et qu’il n’y a pas de chevauchement des préfixes IPv6.  
  
3.  Si IPv6 n’est pas déployé sur le réseau d’entreprise, un préfixe IP-HTTPs pour chaque point d’entrée est automatiquement sélectionné lors de l’ajout du point d’entrée.  
  
### <a name="ipv6-prefix-for-vpn-clients"></a>Préfixe IPv6 pour les clients VPN  
Si vous avez déployé le VPN sur le serveur d’accès à distance unique, notez les points suivants :  
  
1.  L’ajout d’un préfixe VPN IPv6 à un point d’entrée est requis uniquement si vous souhaitez autoriser la connectivité IPv6 du client VPN au réseau d’entreprise.  
  
2.  Le préfixe VPN ne peut être configuré que sur un point d’entrée à l’aide de la console de gestion de l’accès à distance ou de l’applet de commande PowerShell si IPv6 est déployé sur le réseau interne de l’entreprise et si le VPN est activé sur le point d’entrée.  
  
3.  Le préfixe VPN doit être unique dans chaque point d’entrée et ne doit pas se chevaucher avec d’autres préfixes VPN ou IP-HTTPs.  
  
4.  Si IPv6 n’est pas déployé dans le réseau d’entreprise, les clients VPN qui se connectent au point d’entrée ne reçoivent pas d’adresse IPv6.  
  
### <a name="routing"></a>Routage  
Dans un déploiement multisite, le routage symétrique est appliqué à l’aide de Teredo et IP-HTTPs. Quand IPv6 est déployé dans le réseau d’entreprise, notez les points suivants :  
  
1. Les préfixes Teredo et IP-HTTPs de chaque point d’entrée doivent être routables sur le réseau d’entreprise vers leur serveur d’accès à distance associé.  
  
2. Les itinéraires doivent être configurés dans l’infrastructure de routage du réseau d’entreprise.  
  
3. Pour chaque point d’entrée, il doit y avoir un à trois itinéraires dans le réseau interne :  
  
   1. Préfixe IP-HTTPs : ce préfixe est choisi par l’administrateur dans l’Assistant Ajouter un point d’entrée.  
  
   2. Préfixe IPv6 VPN (facultatif). Ce préfixe peut être choisi après l’activation du VPN pour un point d’entrée  
  
   3. Préfixe Teredo (facultatif). Ce préfixe est pertinent uniquement si le serveur d’accès à distance est configuré avec deux adresses IPv4 publiques consécutives sur la carte externe. Le préfixe est basé sur la première adresse IPv4 publique de la paire d’adresses. Par exemple, si les adresses externes sont :  
  
      1. www\.xxx. yyy. zzz  
  
      2. www\.xxx. yyy. zzz + 1  
  
      Le préfixe Teredo à configurer est alors 2001:0 : WWXX : YYZZ ::/64, où WWXX : YYZZ est la représentation hexadécimale de l’adresse IPv4 www\.xxx. yyy. zzz.  
  
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
  
   4. Tous les itinéraires ci-dessus doivent être routés vers l’adresse IPv6 de la carte interne du serveur d’accès à distance (ou vers l’adresse IP virtuelle interne pour un point d’entrée à charge équilibrée).  
  
> [!NOTE]  
> Quand IPv6 est déployé dans le réseau d’entreprise et que l’administration du serveur d’accès à distance est effectuée à distance via DirectAccess, les itinéraires des préfixes Teredo et IP-HTTPs de tous les autres points d’entrée doivent être ajoutés à chaque serveur d’accès à distance afin que le trafic soit transféré au réseau interne.  
  
### <a name="active-directory-site-specific-ipv6-prefixes"></a>Active Directory des préfixes IPv6 spécifiques au site  
Lorsqu’un ordinateur client exécutant Windows 10 ou Windows 8 est connecté à un point d’entrée, l’ordinateur client est immédiatement associé au site Active Directory du point d’entrée, et il est configuré avec des préfixes IPv6 associés au point d’entrée. La préférence est que les ordinateurs clients se connectent aux ressources à l’aide de ces préfixes IPv6, car ils sont configurés dynamiquement dans la table de stratégie de préfixe IPv6 avec une priorité plus élevée lors de la connexion à un point d’entrée.  
  
Si votre organisation utilise une topologie de Active Directory avec des préfixes IPv6 spécifiques à un site (par exemple, un app.corp.com de nom de domaine complet de ressource interne est hébergé dans Amérique du Nord et en Europe avec une adresse IP spécifique à un site dans chaque emplacement), ce n’est pas configuré par par défaut, l’utilisation de la console d’accès à distance et des préfixes IPv6 spécifiques au site ne sont pas configurés pour chaque point d’entrée. Si vous ne souhaitez pas activer ce scénario facultatif, vous devez configurer chaque point d’entrée avec les préfixes IPv6 spécifiques qui doivent être préférés par les ordinateurs clients qui se connectent à un point d’entrée spécifique. Procédez comme suit :  
  
1.  Pour chaque objet de stratégie de groupe utilisé pour les ordinateurs clients Windows 10 ou Windows 8, exécutez l’applet de commande PowerShell Set-DAEntryPointTableItem  
  
2.  Définissez le paramètre EntryPointRange pour l’applet de commande avec les préfixes IPv6 spécifiques au site. Par exemple, pour ajouter les préfixes spécifiques au site 2001 : DB8:1 : 1 ::/64 et 2001 : DB : 1:2 ::/64 à un point d’entrée nommé Europe, exécutez la commande suivante :  
  
    ```  
    $entryPointName = "Europe"  
    $prefixesToAdd = @("2001:db8:1:1::/64", "2001:db8:1:2::/64")  
    $clientGpos = (Get-DAClient).GpoName  
    $clientGpos | % { Get-DAEntryPointTableItem -EntryPointName $entryPointName -PolicyStore $_ | %{ Set-DAEntryPointTableItem -PolicyStore $_.PolicyStore -EntryPointName $_.EntryPointName -EntryPointRange ($_.EntryPointRange) + $prefixesToAdd}}  
    ```  
  
3.  Lorsque vous modifiez le paramètre EntryPointRange, assurez-vous que vous ne supprimez pas les préfixes 128 bits existants qui appartiennent aux points de terminaison de tunnel IPsec et à l’adresse DNS64.  
  
## <a name="bkmk_3_7_TransitionIPv6"></a>3,7 planifier la transition vers IPv6 lorsque l’accès à distance multisite est déployé  
De nombreuses organisations utilisent le protocole IPv4 sur le réseau de l’entreprise. Avec l’épuisement des préfixes IPv4 disponibles, de nombreuses entreprises effectuent la transition de IPv4 uniquement à des réseaux IPv6 uniquement.  
  
Cette transition est très probablement effectuée en deux étapes :  
  
1.  D’un réseau IPv4 uniquement à un réseau d’entreprise IPv6 + IPv4.  
  
2.  D’un IPv4 IPv6 à un réseau d’entreprise IPv6 uniquement.  
  
Dans chaque partie, la transition peut être effectuée par étapes. Dans chaque phase, un seul sous-réseau du réseau peut être remplacé par la nouvelle configuration réseau. Par conséquent, un déploiement multisite DirectAccess est requis pour prendre en charge un déploiement hybride où, par exemple, certains des points d’entrée appartiennent à un sous-réseau IPv4 uniquement et d’autres appartiennent à un sous-réseau IPv6 + IPv4. En outre, les modifications de configuration pendant les processus de transition ne doivent pas rompre la connectivité du client via DirectAccess.  
  
### <a name="TransitionIPv4toMixed"></a>Passage d’un réseau d’entreprise IPv4 uniquement à un réseau d’entreprise IPv6 + IPv4  
Lorsque vous ajoutez des adresses IPv6 à un réseau d’entreprise IPv4 uniquement, vous souhaiterez peut-être ajouter une adresse IPv6 à un serveur DirectAccess déjà déployé. En outre, vous souhaiterez peut-être ajouter un point d’entrée ou un nœud à un cluster à charge équilibrée avec des adresses IPv4 et IPv6 pour le déploiement de DirectAccess.  
  
L’accès à distance vous permet d’ajouter des serveurs avec des adresses IPv4 et IPv6 à un déploiement qui a été configuré à l’origine avec uniquement des adresses IPv4. Ces serveurs sont ajoutés en tant que serveurs IPv4 uniquement et leurs adresses IPv6 sont ignorées par DirectAccess ; par conséquent, votre organisation ne peut pas tirer parti des avantages de la connectivité IPv6 native sur ces nouveaux serveurs.  
  
Pour transformer le déploiement en déploiement IPv6 + IPv4 et tirer parti des fonctionnalités IPv6 natives, vous devez réinstaller DirectAccess. Pour maintenir la connectivité client au cours de la réinstallation, consultez transition d’un déploiement IPv4 uniquement vers un déploiement IPv6 à l’aide de deux déploiements DirectAccess.  
  
> [!NOTE]  
> Comme avec un réseau uniquement IPv4, dans un réseau mixte IPv4 + IPv6, l’adresse du serveur DNS qui est utilisée pour résoudre les demandes DNS du client doit être configurée avec l’DNS64 qui est déployé sur les serveurs d’accès à distance eux-mêmes, et non avec un DNS d’entreprise.  
  
### <a name="TransitionMixedtoIPv6"></a>Passage d’un IPv4 IPv6 à un réseau d’entreprise IPv6 uniquement  
DirectAccess vous permet d’ajouter des points d’entrée IPv6 uniquement si le premier serveur d’accès à distance dans le déploiement avait à l’origine des adresses IPv4 et IPv6, ou seulement une adresse IPv6. Autrement dit, vous ne pouvez pas passer d’un réseau IPv4 uniquement à un réseau IPv6 uniquement en une seule étape sans réinstaller DirectAccess. Pour passer directement d’un réseau IPv4 uniquement à un réseau IPv6 uniquement, consultez transition d’un déploiement IPv4 uniquement vers un déploiement IPv6 uniquement à l’aide de deux déploiements DirectAccess.  
  
Après avoir effectué la transition d’un déploiement IPv4 uniquement vers un déploiement IPv6 + IPv4, vous pouvez passer à un réseau IPv6 uniquement. Pendant et après la transition, notez les points suivants :  
  
-   Si des serveurs principaux IPv4 uniquement restent dans le réseau d’entreprise, ils ne seront pas accessibles aux clients qui se connectent par le biais des points d’entrée IPv6 uniquement.  
  
-   Lors de l’ajout de points d’entrée uniquement IPv6 à un déploiement IPv4 + IPv6, DNS64 et NAT64 ne sont pas activés sur les nouveaux serveurs. Les clients qui se connectent à ces points d’entrée seront automatiquement configurés pour utiliser les serveurs DNS d’entreprise.  
  
-   Si vous devez supprimer des adresses IPv4 d’un serveur déployé, vous devez supprimer le serveur du déploiement de DirectAccess, supprimer son adresse réseau d’entreprise IPv4, puis l’ajouter à nouveau au déploiement.  
  
Pour prendre en charge la connectivité client au réseau d’entreprise, vous devez vous assurer que le serveur emplacement réseau peut être résolu par le DNS d’entreprise en son adresse IPv6. Une autre adresse IPv4 peut également être définie, mais elle n’est pas obligatoire.  
  
### <a name="DualDeployment"></a>Passage d’un déploiement IPv4 uniquement à un déploiement IPv6 uniquement à l’aide de deux déploiements DirectAccess  
La transition d’un réseau d’entreprise IPv4 uniquement vers un réseau d’entreprise IPv6 uniquement ne peut pas être effectuée sans réinstaller le déploiement de DirectAccess. Pour maintenir la connectivité du client pendant la transition, vous pouvez utiliser un autre déploiement de DirectAccess. Un double déploiement est nécessaire lorsque la première étape de transition se termine (mise à niveau du réseau uniquement IPv4 vers IPv4 + IPv6) et que vous envisagez de vous préparer à une transition future vers une prise en compte de réseau d’entreprise IPv6 uniquement en tirant parti des avantages de la connectivité IPv6 native. Le déploiement double est décrit dans les étapes générales suivantes :  
  
1.  Installez un deuxième déploiement de DirectAccess. Vous pouvez installer DirectAccess sur de nouveaux serveurs ou supprimer des serveurs du premier déploiement et les utiliser pour le deuxième déploiement.  
  
    > [!NOTE]  
    > Lors de l’installation d’un déploiement de DirectAccess supplémentaire en même temps qu’un déploiement actuel, assurez-vous que deux points d’entrée ne partagent pas le même préfixe client.  
    >   
    > Si vous installez DirectAccess à l’aide de l’Assistant Prise en main ou avec l’applet de commande `Install-RemoteAccess`, l’accès à distance définit automatiquement le préfixe du client du premier point d’entrée dans le déploiement sur une valeur par défaut de < sous-réseau IPv6\_préfixe >: 1000 ::/64. Si nécessaire, vous devez modifier le préfixe.  
  
2.  Supprimez les groupes de sécurité du client choisis du premier déploiement.  
  
3.  Ajoutez les groupes de sécurité client au second déploiement.  
  
    > [!IMPORTANT]  
    > Pour maintenir la connectivité du client tout au long du processus, vous devez ajouter les groupes de sécurité au deuxième déploiement immédiatement après les avoir supprimés de la première. Cela permet de s’assurer que les clients ne seront pas mis à jour avec deux ou zéro objets de stratégie de groupe DirectAccess. Les clients commenceront à utiliser le deuxième déploiement une fois qu’ils auront récupéré et mis à jour leur GPO client.  
  
4.  Facultatif : supprimez les points d’entrée DirectAccess du premier déploiement et ajoutez ces serveurs comme nouveaux points d’entrée dans la seconde.  
  
Une fois la transition terminée, vous pouvez désinstaller le premier déploiement de DirectAccess. Lors de la désinstallation de, les problèmes suivants peuvent se produire :  
  
-   Si le déploiement a été configuré pour prendre en charge uniquement les clients sur les ordinateurs portables, le filtre WMI sera supprimé. Si les groupes de sécurité du client du deuxième déploiement incluent des ordinateurs de bureau, l’objet de stratégie de groupe du client DirectAccess ne filtre pas les ordinateurs de bureau et peut entraîner des problèmes. Si un filtre d’ordinateurs portables est nécessaire, recréez-le en suivant les instructions sur [créer des filtres WMI pour l’objet de stratégie de groupe](https://technet.microsoft.com/library/cc947846.aspx).  
  
-   Si les deux déploiements ont été créés à l’origine sur le même domaine de Active Directory, l’entrée de sonde DNS qui pointe vers localhost sera supprimée et peut entraîner des problèmes de connectivité du client. Par exemple, les clients peuvent se connecter à l’aide d’IP-HTTPs plutôt que de Teredo, ou basculer entre les points d’entrée multisite DirectAccess. Dans ce cas, vous devez ajouter l’entrée DNS suivante au DNS d’entreprise :  
  
    -   Zone : nom de domaine  
  
    -   Nom : DirectAccess-corpConnectivityHost  
  
    -   Adresse IP ::: 1  
  
    -   Type : AAAA  
  
  
  


