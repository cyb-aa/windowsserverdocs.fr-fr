---
title: Étape 1 Configuration de l’Infrastructure d’accès à distance
description: Cette rubrique fait partie du guide les Clients DirectAccess de gérer à distance dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0e7d1f5b-c939-47ca-892f-5bb285027fbc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dc08d89f7d84b5435e97ed5ed77eb72d003b0c84
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888570"
---
# <a name="step-1-configure-the-remote-access-infrastructure"></a>Étape 1 Configuration de l’Infrastructure d’accès à distance

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

**Remarque :** Windows Server 2012 associe DirectAccess et le service Routage et accès distant (RRAS) dans un rôle Accès à distance unique.  
  
Cette rubrique décrit comment configurer l’infrastructure requise pour un déploiement avancé de l’accès à distance à l’aide d’un seul serveur d’accès à distance dans un environnement mixte IPv4 et IPv6. Avant de commencer les étapes de déploiement, vérifiez que vous avez effectué les étapes de planification décrites dans [étape 1 : Planifier l’Infrastructure d’accès à distance](../plan/Step-1-Plan-the-Remote-Access-Infrastructure.md).  
  
|Tâche|Description|  
|----|--------|  
|Configurer les paramètres réseau serveur|Configurez les paramètres réseau serveur sur le serveur d'accès à distance.|  
|Configurer le routage dans le réseau d'entreprise|Configurez le routage dans le réseau d'entreprise pour garantir que le trafic est correctement acheminé.|  
|Configurer les pare-feu|Configurer des pare-feu supplémentaires, si nécessaire.|  
|Configurer les autorités de certification et les certificats|Configurer une autorité de certification (CA), si nécessaire et tout autre modèle de certificat requis dans le déploiement.|  
|Configurer le serveur DNS|Configurez les paramètres DNS pour le serveur d'accès à distance.|  
|Configurer Active Directory|Joindre des ordinateurs clients et le serveur d’accès à distance au domaine Active Directory.|  
|Configurer les objets de stratégie de groupe|Configurer les objets de stratégie de groupe (GPO) pour le déploiement, si nécessaire.|  
|Configurer les groupes de sécurité|Configurez les groupes de sécurité qui contiendront les ordinateurs clients DirectAccess, ainsi que tous les autres groupes de sécurité requis dans le déploiement.|  
|Configurer le serveur Emplacement réseau|Configurez le serveur Emplacement réseau, y compris l'installation du certificat de site web du serveur Emplacement réseau.|  
  
> [!NOTE]  
> Cette rubrique inclut des exemples d'applets de commande Windows PowerShell que vous pouvez utiliser pour automatiser certaines des procédures décrites. Pour plus d’informations, consultez [Utilisation des applets de commande](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_ConfigNetworkSettings"></a>Configurer les paramètres de réseau de serveur  
Selon si vous décidez de placer le serveur d’accès à distance à la périphérie ou derrière un périphérique de traduction d’adresses réseau (NAT), les paramètres d’adresse d’interface réseau suivants sont requis pour un déploiement de serveur unique dans un environnement avec IPv4 et IPv6. Toutes les adresses IP sont configurées à l'aide de l'option **Modifier les paramètres de la carte** du **Centre Réseau et partage Windows**.  
  
**Topologie de périmètre**:  
  
Requiert les éléments suivants :  
  
-   Deux accessibles sur Internet adresses publiques consécutives statiques IPv4 ou IPv6.  
  
    > [!NOTE]  
    > Deux adresses IPv4 publiques consécutives sont requises pour Teredo. Si vous n'utilisez pas Teredo, vous pouvez configurer une seule adresse IPv4 statique publique.  
  
-   Une seule adresse IPv4 ou IPv6 statique interne.  
  
**Derrière un périphérique NAT (deux cartes réseau)**:  
  
Nécessite une seule interne côté réseau IPv4 ou IPv6 adresse statique.  
  
**Derrière un périphérique NAT (une carte réseau)**:  
  
Nécessite une adresse IPv4 ou IPv6 statique unique.  
  
Si le serveur d’accès à distance a deux cartes réseau (une pour le profil de domaine et l’autre pour un profil public ou privé), mais que vous utilisez une topologie de carte réseau unique, il est recommandé comme suit :  
  
1.  Assurez-vous que la deuxième carte réseau est également classée dans le profil de domaine.  
  
2.  Si la deuxième carte réseau ne peut pas être configurée pour le profil de domaine pour une raison quelconque, la stratégie DirectAccess IPsec doit être limitée manuellement à tous les profils à l’aide de la commande Windows PowerShell suivante :  
  
    ```  
    $gposession = Open-NetGPO -PolicyStore <Name of the server GPO>  
    Set-NetIPsecRule -DisplayName <Name of the IPsec policy> -GPOSession $gposession -Profile Any  
    Save-NetGPO -GPOSession $gposession  
    ```  
  
    Les noms des stratégies IPsec à utiliser dans cette commande sont **DirectAccess-DaServerToInfra** et **DirectAccess-DaServerToCorp**.  
  
## <a name="BKMK_ConfigRouting"></a>Configurer le routage du réseau d’entreprise  
Procédez comme suit pour configurer le routage dans le réseau d'entreprise :  
  
-   Lorsqu'IPv6 natif est déployé dans l'organisation, ajoutez un itinéraire afin que les routeurs du réseau interne redirigent le trafic IPv6 via le serveur d'accès à distance.  
  
-   Configurez manuellement les itinéraires IPv4 et IPv6 de l'organisation sur les serveurs d'accès à distance. Ajoutez un itinéraire publié afin que tout le trafic avec un (/ 48) préfixe IPv6 est transféré au réseau interne. De plus, pour le trafic IPv4, ajoutez des itinéraires explicites afin que le trafic IPv4 soit transféré au réseau interne.  
  
## <a name="BKMK_ConfigFirewalls"></a>Configurer des pare-feu  
Selon les paramètres de réseau que vous avez choisi, lorsque vous utilisez des pare-feu supplémentaires dans votre déploiement, appliquez les exceptions de pare-feu suivantes pour le trafic d’accès à distance :  
  
### <a name="remote-access-server-on-ipv4-internet"></a>Serveur d’accès distant sur Internet IPv4  
Appliquez les exceptions de pare-feu côté Internet suivantes pour le trafic d’accès à distance lorsque le serveur d’accès à distance est sur Internet IPv4 :  
  
-   **Trafic Teredo**  
  
    Port de destination UDP (Datagram Protocol) utilisateur 3544 entrant et port UDP source 3544 sortant. Appliquer cette exemption pour les deux les adresses publiques consécutives IPv4 sur Internet sur le serveur d’accès à distance.  
  
-   **trafic 6to4**  
  
    IP protocole 41 entrant et sortant. Appliquer cette exemption pour les deux les adresses publiques consécutives IPv4 sur Internet sur le serveur d’accès à distance.  
  
-   **Trafic IP-HTTPS**  
  
    Port de destination TCP (Control Protocol) de transmission 443 et le port TCP source 443 sortant. Lorsque le serveur d’accès à distance dispose d’une seule carte réseau et que le serveur Emplacement réseau se trouve sur le serveur d’accès à distance, le port TCP 62000 est également requis. Appliquer ces exemptions uniquement pour l’adresse à laquelle résout le nom externe du serveur.  
  
    > [!NOTE]  
    > Cette exemption est configurée sur le serveur d’accès à distance. Toutes les autres exemptions sont configurées sur le pare-feu de périmètre.  
  
### <a name="remote-access-server-on-ipv6-internet"></a>Serveur d’accès distant sur Internet IPv6  
Appliquez les exceptions de pare-feu côté Internet suivantes pour le trafic d’accès à distance lorsque le serveur d’accès à distance est sur Internet IPv6 :  
  
-   Protocole IP 50  
  
-   Port UDP de destination 500 entrant et port UDP source 500 sortant.  
  
-   Internet Control Message Protocol for IPv6 (ICMPv6) le trafic entrant et sortant - pour les implémentations Teredo uniquement.  
  
### <a name="remote-access-traffic"></a>Trafic d’accès à distance  
Appliquez les exceptions de pare-feu de réseau interne suivantes pour le trafic d’accès à distance :  
  
-   ISATAP : Protocole 41 entrant et sortant  
  
-   TCP/UDP pour tout trafic IPv4 ou IPv6  
  
-   ICMP pour tout trafic IPv4 ou IPv6  
  
## <a name="BKMK_ConfigCAs"></a>Configurez les certificats et les autorités de certification  
Accès à distance dans Windows Server 2012, vous permet de choisir entre l’utilisation de certificats pour l’authentification d’ordinateur ou à l’aide d’une authentification Kerberos intégrée qui utilise des noms d’utilisateur et mots de passe. Vous devez également configurer un certificat IP-HTTPS sur le serveur d’accès à distance. Cette section explique comment configurer ces certificats.  
  
Pour plus d’informations sur la configuration d’une infrastructure à clé publique (PKI), consultez [Active Directory Certificate Services](https://technet.microsoft.com/library/cc770357.aspx).  
  
### <a name="BKMK_ConfigIPsec"></a>Configurer l’authentification IPsec  
Un certificat est requis sur le serveur d’accès à distance et tous les clients DirectAccess afin qu’ils peuvent utiliser l’authentification IPsec. Le certificat doit être émis par une autorité de certification interne (CA). Serveurs d’accès à distance et les clients DirectAccess doivent approuver l’autorité de certification qui émet les certificats racines et intermédiaires.  
  
##### <a name="to-configure-ipsec-authentication"></a>Pour configurer l'authentification IPsec  
  
1.  Sur l’autorité de certification interne, décidez si vous souhaitez utiliser le modèle de certificat d’ordinateur par défaut, ou si vous souhaitez créer un nouveau modèle de certificat comme décrit dans [création de modèles de certificat](https://technet.microsoft.com/library/cc731705.aspx).  
  
    > [!NOTE]  
    > Si vous créez un nouveau modèle, il doit être configuré pour l’authentification du client.  
  
2.  Déployez le modèle de certificat si nécessaire. Pour plus d’informations, voir [Déploiement de modèles de certificat](https://technet.microsoft.com/library/cc770794.aspx).  
  
3.  Configurer le modèle pour l’inscription automatique si nécessaire.  
  
4.  Configurer l’inscription automatique de certificat si nécessaire. Pour plus d’informations, voir [Configurer l’inscription automatique de certificat](https://technet.microsoft.com/library/cc731522.aspx).  
  
### <a name="BKMK_ConfigCertTemp"></a>Configurer les modèles de certificat  
Lorsque vous utilisez une autorité de certification interne pour émettre des certificats, vous devez configurer des modèles de certificat pour le certificat IP-HTTPS et le certificat de site Web de serveur emplacement réseau.  
  
##### <a name="to-configure-a-certificate-template"></a>Pour configurer un modèle de certificat  
  
1.  Sur l’autorité de certification interne, créez un modèle de certificat comme décrit dans [Création de modèles de certificat](https://technet.microsoft.com/library/cc731705.aspx).  
  
2.  Déployez le modèle de certificat comme décrit dans [Déploiement de modèles de certificat](https://technet.microsoft.com/library/cc770794.aspx).  
  
Après avoir préparé vos modèles, vous pouvez les utiliser pour configurer les certificats. Consultez les procédures suivantes pour plus d’informations :  
  
-   [Configurer le certificat IP-HTTPS](#BKMK_IPHTTPS)  
  
-   [Configurer le serveur emplacement réseau](#BKMK_ConfigNLS)  
  
### <a name="BKMK_IPHTTPS"></a>Configurer le certificat IP-HTTPS  
L'accès à distance requiert un certificat IP-HTTPS pour authentifier les connexions IP-HTTPS auprès du serveur d'accès à distance. Il existe trois options de certificat pour le certificat IP-HTTPS :  
  
-   **Public**  
  
    Fourni par un tiers.  
  
-   **Privé**  
  
    Le certificat est basé sur le modèle de certificat que vous avez créé dans [configuration de modèles de certificat](assetId:///6a5ec5c1-d653-47b1-a567-cc485004e7bc#ConfigCertTemp). Il requiert, point de distribution de liste de révocation révocation certificat qui est accessible à partir d’un nom de domaine complet pouvant être résolu publiquement.  
  
-   **Auto-signé**  
  
    Ce certificat nécessite un point de distribution CRL qui est accessible à partir d’un nom de domaine complet pouvant être résolu publiquement.  
  
    > [!NOTE]  
    > Les certificats auto-signés ne sont pas utilisables dans des déploiements multisites.  
  
Assurez-vous que le certificat de site web pour l'authentification IP-HTTPS est conforme aux exigences suivantes :  
  
-   Le nom de sujet du certificat doit être le nom de domaine complet (FQDN) pouvant être résolu en externe de l’URL IP-HTTPS (l’adresse ConnectTo) qui est utilisé uniquement pour les connexions IP-HTTPS du serveur accès à distance.  
  
-   Le nom commun du certificat doit correspondre au nom du site IP-HTTPS.  
  
-   Dans le champ objet, spécifiez l’adresse IPv4 de la carte externe du serveur d’accès à distance ou le nom de domaine complet de l’URL IP-HTTPS.  
  
-   Pour le **Enhanced Key Usage** champ, utilisez l’identificateur d’objet (OID) authentification du serveur.  
  
-   Pour le champ **Points de distribution de la liste de révocation de certificats**, indiquez un point de distribution de liste de révocation de certificats accessible par les clients DirectAccess connectés à Internet  
  
-   Le certificat IP-HTTPS doit avoir une clé privée.  
  
-   Le certificat IP-HTTPS doit être importé directement dans le magasin personnel.  
  
-   Les certificats IP-HTTPS peuvent utiliser des caractères génériques dans le nom.  
  
##### <a name="to-install-the-ip-https-certificate-from-an-internal-ca"></a>Pour installer le certificat IP-HTTPS à partir d'une autorité de certification interne  
  
1.  Sur le serveur d'accès à distance : Sur le **Démarrer** , tapez**mmc.exe**, puis appuyez sur ENTRÉE.  
  
2.  Dans la console MMC, dans le menu **Fichier**, cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**.  
  
3.  Dans la boîte de dialogue **Ajouter ou supprimer des composants logiciels enfichables**, cliquez sur **Certificats**, sur **Ajouter**, sur **Le compte de l'ordinateur**, sur **Suivant**, sur **Ordinateur local**, sur **Terminer**, puis sur **OK**.  
  
4.  Dans l'arborescence de la console du composant logiciel enfichable Certificats, ouvrez **Certificats (ordinateur local)\Personnel\Certificats**.  
  
5.  Avec le bouton droit **certificats**, pointez sur **toutes les tâches**, cliquez sur **demander un nouveau certificat**, puis cliquez sur **suivant** deux fois...  
  
6.  Sur le **demander des certificats** page, sélectionnez la case à cocher du modèle de certificat que vous avez créé dans la configuration de modèles de certificat et si nécessaire, cliquez sur **plus d’informations est nécessaire pour s’inscrire pour cela certificat**.  
  
7.  Dans la boîte de dialogue **Propriétés du certificat**, sous l'onglet **Sujet**, dans la zone **Nom du sujet**, dans **Type**, sélectionnez **Nom commun**.  
  
8.  Dans **valeur**, spécifiez l’adresse IPv4 de la carte externe du serveur d’accès à distance ou le nom de domaine complet de l’URL IP-HTTPS, puis cliquez sur **ajouter**.  
  
9. Dans la zone **Autre nom**, dans **Type**, sélectionnez **DNS**.  
  
10. Dans **valeur**, spécifiez l’adresse IPv4 de la carte externe du serveur d’accès à distance ou le nom de domaine complet de l’URL IP-HTTPS, puis cliquez sur **ajouter**.  
  
11. Sous l'onglet **Général**, dans **Nom convivial**, vous pouvez entrer un nom qui vous permettra d'identifier le certificat.  
  
12. Sous l'onglet **Extensions**, en regard de l'option **Utilisation améliorée de la clé**, cliquez sur la flèche, puis assurez-vous qu'Authentification du serveur figure dans la liste **Expressions sélectionnées**.  
  
13. Cliquez sur **OK**, sur **Inscrire**, puis sur **Terminer**.  
  
14. Dans le volet détails du composant logiciel enfichable Certificats, vérifiez que le nouveau certificat a été inscrit avec la finalité de l’authentification du serveur.  
  
## <a name="BKMK_ConfigDNS"></a>Configurer le serveur DNS  
Vous devez configurer manuellement une entrée DNS pour le site web du serveur Emplacement réseau du réseau interne de votre déploiement.  
  
### <a name="NLS_DNS"></a>Pour ajouter la sonde web et serveur d’emplacement réseau  
  
1.  Sur le serveur DNS du réseau interne : Sur le **Démarrer** , tapez**dnsmgmt.msc**, puis appuyez sur ENTRÉE.  
  
2.  Dans le volet gauche de la console **Gestionnaire DNS**, développez la zone de recherche directe de votre domaine. Cliquez sur le domaine, puis cliquez sur **nouvel hôte (A ou AAAA)**.  
  
3.  Dans le **nouvel hôte** boîte de dialogue le **nom (utilise nom du domaine parent si vide)** , entrez le nom DNS pour le site de serveur d’emplacement réseau (c’est le nom que les clients DirectAccess utilisent pour se connecter à la serveur d’emplacement réseau). Dans le **adresse IP** , entrez l’adresse IPv4 du serveur d’emplacement réseau, puis cliquez sur **ajouter un hôte**, puis cliquez sur **OK**.  
  
4.  Dans le **nouvel hôte** boîte de dialogue le **nom (utilise nom du domaine parent si vide)** , entrez le nom DNS de la sonde web (le nom de la sonde web par défaut est directaccess-webprobehost). Dans la zone **Adresse IP**, entrez l'adresse IPv4 de la sonde web, puis cliquez sur **Ajouter un hôte**.  
  
5.  Répétez ce processus pour directaccess-corpconnectivityhost et tout vérificateur de connectivité créé manuellement. Dans le **DNS** boîte de dialogue, cliquez sur **OK**.  
  
6.  Cliquez sur **Terminé**.  
  
![Windows PowerShell](../../../../media/Step-1-Configure-the-Remote-Access-Infrastructure/PowerShellLogoSmall.gif)Windows PowerShell équivalente commandes ***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
Add-DnsServerResourceRecordA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv4Address <network_location_server_IPv4_address>  
Add-DnsServerResourceRecordAAAA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv6Address <network_location_server_IPv6_address>  
```  
  
Vous devez également configurer les entrées DNS pour les éléments suivants :  
  
-   **Le serveur IP-HTTPS**  
  
    Clients DirectAccess doivent être en mesure de résoudre le nom DNS du serveur d’accès à distance à partir d’Internet.  
  
-   **Vérification de la révocation de révocation de certificats**  
  
    DirectAccess utilise la vérification de révocation de certificats pour la connexion IP-HTTPS entre les clients DirectAccess et le serveur d’accès à distance et pour la connexion HTTPS entre le client DirectAccess et le serveur emplacement réseau. Dans les deux cas, les clients DirectAccess doivent être en mesure de résoudre le point de distribution de liste de révocation de certificats et d'y accéder.  
  
-   **ISATAP**  
  
    ISATAP Intrasite Automatic Tunnel Addressing Protocol () utilise des tunnels pour permettre aux clients DirectAccess pour se connecter au serveur d’accès à distance via Internet IPv4, qui encapsule les paquets IPv6 au sein d’un en-tête IPv4. Il est utilisé par l'accès à distance pour fournir la connectivité IPv6 aux hôtes ISATAP sur un intranet. Dans un environnement de réseau IPv6 non natif, le serveur d’accès à distance se configure automatiquement à le comme routeur ISATAP. La prise en charge de la résolution pour le nom ISATAP est requise.  
  
## <a name="BKMK_ConfigAD"></a>Configurer Active Directory  
Le serveur d'accès à distance et tous les ordinateurs clients DirectAccess doivent être joints à un domaine Active Directory. Les ordinateurs clients DirectAccess doivent être membres de l'un des types de domaines suivants :  
  
-   les domaines qui appartiennent à la même forêt que le serveur d'accès à distance ;  
  
-   les domaines qui appartiennent à des forêts dotées d'une relation d'approbation bidirectionnelle avec la forêt du serveur d'accès à distance ;  
  
-   les domaines dotés d'une approbation bidirectionnelle avec le domaine du serveur d'accès à distance.  
  
#### <a name="to-join-the-remote-access-server-to-a-domain"></a>Pour joindre le serveur d’accès à distance à un domaine  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **Serveur local**. Dans le volet d’informations, cliquez sur le lien en regard de **Nom de l’ordinateur**.  
  
2.  Dans la boîte de dialogue **Propriétés système**, cliquez sur l'onglet **Nom de l'ordinateur**, puis sur **Changer**.  
  
3.  Dans le **nom de l’ordinateur** , tapez le nom de l’ordinateur si vous modifiez également le nom d’ordinateur lorsque vous joignez le serveur au domaine. Sous **membre**, cliquez sur **domaine**, puis tapez le nom du domaine auquel vous souhaitez joindre le serveur, (par exemple, corp.contoso.com), puis cliquez sur **OK**.  
  
4.  Lorsque vous y êtes invité pour un nom d’utilisateur et le mot de passe, entrez le nom d’utilisateur et le mot de passe d’un utilisateur disposant d’autorisations pour joindre des ordinateurs au domaine, puis cliquez sur **OK**.  
  
5.  Lorsqu’une boîte de dialogue vous souhaitant la bienvenue dans le domaine s’affiche, cliquez sur **OK**.  
  
6.  Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **OK**.  
  
7.  Dans la boîte de dialogue **Propriétés système**, cliquez sur **Fermer**.  
  
8.  Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **Redémarrer maintenant**.  
  
#### <a name="to-join-client-computers-to-the-domain"></a>Pour joindre des ordinateurs clients au domaine  
  
1.  Sur le **Démarrer** , tapez**explorer.exe**, puis appuyez sur ENTRÉE.  
  
2.  Cliquez avec le bouton droit sur l'icône Ordinateur, puis cliquez sur **Propriétés**.  
  
3.  Dans la page **Système**, cliquez sur **Paramètres système avancés**.  
  
4.  Dans la boîte de dialogue **Propriétés système** , sous l’onglet **Nom de l’ordinateur** , cliquez sur **Modifier**.  
  
5.  Dans le **nom de l’ordinateur** , tapez le nom de l’ordinateur si vous modifiez également le nom d’ordinateur lorsque vous joignez le serveur au domaine. Sous **Membre de**, cliquez sur **Domaine**, et tapez le nom du domaine auquel vous voulez joindre le serveur (par exemple, corp.contoso.com), puis cliquez sur **OK**.  
  
6.  Lorsque vous y êtes invité pour un nom d’utilisateur et le mot de passe, entrez le nom d’utilisateur et le mot de passe d’un utilisateur disposant d’autorisations pour joindre des ordinateurs au domaine, puis cliquez sur **OK**.  
  
7.  Lorsqu’une boîte de dialogue vous souhaitant la bienvenue dans le domaine s’affiche, cliquez sur **OK**.  
  
8.  Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **OK**.  
  
9. Dans le **propriétés système** boîte de dialogue, cliquez sur Fermer.  
  
10. Lorsque vous y êtes invité, cliquez sur **Redémarrer maintenant**.  
  
![Windows PowerShell](../../../../media/Step-1-Configure-the-Remote-Access-Infrastructure/PowerShellLogoSmall.gif)Windows PowerShell équivalente commandes ***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
> [!NOTE]  
> Vous devez fournir les informations d’identification de domaine après avoir entré la commande suivante.  
  
```  
Add-Computer -DomainName <domain_name>  
Restart-Computer  
```  
  
## <a name="BKMK_ConfigGPOs"></a>Configurer la stratégie de groupe  
Pour déployer l’accès à distance, vous avez besoin d’un minimum de deux objets de stratégie de groupe. Un objet de stratégie de groupe contient des paramètres pour le serveur d’accès à distance, et un autre contenant des paramètres pour les ordinateurs clients DirectAccess. Lorsque vous configurez l’accès à distance, l’Assistant crée automatiquement les objets de stratégie de groupe requis. Toutefois, si votre organisation impose une convention d’affectation de noms, ou vous n’avez pas les autorisations requises pour créer ou modifier des objets de stratégie de groupe, ils doivent être créés avant de configurer l’accès à distance.  
  
Pour créer des objets de stratégie de groupe, consultez [créer et modifier un objet de stratégie de groupe](https://technet.microsoft.com/library/cc754740.aspx).  
  
Un administrateur peut lier manuellement les objets de stratégie de groupe DirectAccess à une unité d’organisation (UO). Considérez les points suivants :  
  
1.  Lier les objets de stratégie de groupe créés pour les unités d’organisation respectives avant de configurer DirectAccess.  
  
2.  Configurez DirectAccess en spécifiant un groupe de sécurité pour les ordinateurs clients.  
  
3.  Stratégie de groupe est configurées automatiquement, indépendamment de si l’administrateur dispose des autorisations requises pour lier les objets de stratégie de groupe au domaine.  
  
4.  Si les objets de stratégie de groupe sont déjà liés à une unité d’organisation, les liens ne seront pas supprimés, mais ils ne sont pas liés au domaine.  
  
5.  Pour le serveur de stratégie de groupe, l’unité d’organisation doit contenir le serveur ordinateur objet ; autrement, l’objet de stratégie de groupe sera lié à la racine du domaine.  
  
6.  Si l’unité d’organisation n’a pas été précédemment liée en exécutant l’Assistant Installation DirectAccess, une fois la configuration terminée, l’administrateur peut lier des objets de stratégie de groupe DirectAccess pour les unités d’organisation requises et supprimer le lien vers le domaine.  
  
    Pour plus d’informations, voir [Lier un objet de stratégie de groupe](https://technet.microsoft.com/library/cc732979.aspx).  
  
> [!NOTE]  
> Si un objet de stratégie de groupe a été créé manuellement, il est possible que l’objet de stratégie de groupe ne sera pas disponible lors de la configuration de DirectAccess. L’objet de stratégie de groupe n’a ne peut-être pas été répliqué sur le contrôleur de domaine le plus proche de l’ordinateur de gestion. L’administrateur peut attendre pour la réplication vers ou forcez la réplication.  
  
## <a name="BKMK_ConfigSGs"></a>Configurer des groupes de sécurité  
Les paramètres DirectAccess contenus dans l’objet stratégie de groupe d’ordinateur client sont appliqués uniquement aux ordinateurs qui sont membres des groupes de sécurité que vous spécifiez lors de la configuration d’accès à distance.  
  
### <a name="Sec_Group"></a>Pour créer un groupe de sécurité pour les clients DirectAccess  
  
1.  Sur le **Démarrer** , tapez**DSA.msc**, puis appuyez sur ENTRÉE.  
  
2.  Dans la console **Utilisateurs et ordinateurs Active Directory**, dans le volet gauche, développez le domaine contenant le groupe de sécurité, cliquez avec le bouton droit sur **Utilisateurs**, pointez sur **Nouveau**, puis cliquez sur **Groupe**.  
  
3.  Dans la boîte de dialogue **Nouvel objet - Groupe**, sous **Nom du groupe**, entrez le nom du groupe de sécurité.  
  
4.  Sous **Étendue du groupe**, cliquez sur **Globale**. Sous **Type de groupe**, cliquez sur **Sécurité**, puis cliquez sur **OK**.  
  
5.  Double-cliquez sur le groupe de sécurité ordinateurs de clients DirectAccess, puis, dans le **propriétés** boîte de dialogue, cliquez sur le **membres** onglet.  
  
6.  Sous l'onglet **Membres** , cliquez sur **Ajouter**.  
  
7.  Dans la boîte de dialogue **Sélectionner Utilisateurs, contacts, ordinateurs ou comptes de service**, sélectionnez les ordinateurs clients que vous voulez activer pour DirectAccess, puis cliquez sur **OK**.  
  
![Windows PowerShell](../../../../media/Step-1-Configure-the-Remote-Access-Infrastructure/PowerShellLogoSmall.gif)**commandes Windows PowerShell équivalentes**  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
New-ADGroup -GroupScope global -Name <DirectAccess_clients_group_name>  
Add-ADGroupMember -Identity DirectAccess_clients_group_name -Members <computer_name>  
```  
  
## <a name="BKMK_ConfigNLS"></a>Configurer le serveur emplacement réseau  
Le serveur emplacement réseau doit se trouver sur un serveur avec une haute disponibilité, et il a besoin d’un certificat Secure Sockets Layer (SSL) valide qui est approuvé par les clients DirectAccess.  
  
> [!NOTE]  
> Si le site de serveur d’emplacement réseau se trouve sur le serveur d’accès à distance, un site Web est créé automatiquement lorsque vous configurez l’accès à distance et il est lié au certificat de serveur que vous fournissez.  
  
Il existe deux options de certificat pour le certificat de serveur d'emplacement réseau :  
  
-   **Privé**  
  
    > [!NOTE]  
    > Le certificat est basé sur le modèle de certificat que vous avez créé dans [configuration de modèles de certificat](assetId:///6a5ec5c1-d653-47b1-a567-cc485004e7bc#ConfigCertTemp).  
  
-   **Auto-signé**  
  
    > [!NOTE]  
    > Les certificats auto-signés ne sont pas utilisables dans des déploiements multisites.  
  
Si vous utilisez un certificat privé ou un certificat auto-signé, ils requièrent les éléments suivants :  
  
-   Un certificat de site web utilisé pour le serveur Emplacement réseau. Le sujet de certificat doit être l'URL du serveur Emplacement réseau.  
  
-   Un point de distribution CRL qui présente une haute disponibilité sur le réseau interne.  
  
#### <a name="to-install-the-network-location-server-certificate-from-an-internal-ca"></a>Pour installer le certificat du serveur Emplacement réseau à partir d'une autorité de certification interne  
  
1.  Sur le serveur qui hébergera le site web du serveur Emplacement réseau : Sur le **Démarrer** , tapez**mmc.exe**, puis appuyez sur ENTRÉE.  
  
2.  Dans la console MMC, dans le menu **Fichier**, cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**.  
  
3.  Dans la boîte de dialogue **Ajouter ou supprimer des composants logiciels enfichables**, cliquez sur **Certificats**, sur **Ajouter**, sur **Le compte de l'ordinateur**, sur **Suivant**, sur **Ordinateur local**, sur **Terminer**, puis sur **OK**.  
  
4.  Dans l'arborescence de la console du composant logiciel enfichable Certificats, ouvrez **Certificats (ordinateur local)\Personnel\Certificats**.  
  
5.  Avec le bouton droit **certificats**, pointez sur **toutes les tâches**, cliquez sur **demander un nouveau certificat**, puis cliquez sur **suivant** à deux reprises.  
  
6.  Sur le **demander des certificats** page, sélectionnez la case à cocher du modèle de certificat que vous avez créé dans la configuration de modèles de certificat et si nécessaire, cliquez sur **plus d’informations est nécessaire pour s’inscrire pour cela certificat**.  
  
7.  Dans la boîte de dialogue **Propriétés du certificat**, sous l'onglet **Sujet**, dans la zone **Nom du sujet**, dans **Type**, sélectionnez **Nom commun**.  
  
8.  Dans le champ **Valeur**, entrez le nom de domaine complet (FQDN) du site web du serveur Emplacement réseau, puis cliquez sur **Ajouter**.  
  
9. Dans la zone **Autre nom**, dans **Type**, sélectionnez **DNS**.  
  
10. Dans le champ **Valeur**, entrez le nom de domaine complet (FQDN) du site web du serveur Emplacement réseau, puis cliquez sur **Ajouter**.  
  
11. Sous l'onglet **Général**, dans **Nom convivial**, vous pouvez entrer un nom qui vous permettra d'identifier le certificat.  
  
12. Cliquez sur **OK**, sur **Inscrire**, puis sur **Terminer**.  
  
13. Dans le volet détails du composant logiciel enfichable Certificats, vérifiez qu’un nouveau certificat a été inscrit avec le but de l’authentification du serveur.  
  
#### <a name="to-configure-the-network-location-server"></a>Pour configurer le serveur Emplacement réseau  
  
1.  Configurez un site web sur un serveur à haute disponibilité. Le site web ne nécessite aucun contenu, mais quand vous le testez, vous pouvez définir une page par défaut qui fournit un message lorsque les clients se connectent.  
  
    Cette étape n’est pas requise si le site de serveur d’emplacement réseau est hébergé sur le serveur d’accès à distance.  
  
2.  Liez un certificat de serveur HTTPS au site web. Le nom commun du certificat doit correspondre au nom du site du serveur Emplacement réseau. Assurez-vous que les clients DirectAccess font confiance à l'autorité de certification émettrice.  
  
    Cette étape n’est pas requise si le site de serveur d’emplacement réseau est hébergé sur le serveur d’accès à distance.  
  
3.  Configurer un site de révocation de certificats que hass haute disponibilité sur le réseau interne.  
  
    Les points de distribution de listes de révocation de certificat sont accessibles via :  
  
    -   Serveurs Web qui utilisent une URL basée sur HTTP, tels que : https://crl.corp.contoso.com/crld/corp-APP1-CA.crl  
  
    -   Serveurs de fichiers qui sont accessibles via un chemin universal naming convention (UNC), tel que \\\crl.corp.contoso.com\crld\corp-APP1-CA.crl  
  
    Si le point de distribution CRL interne est accessible uniquement via IPv6, vous devez configurer un pare-feu Windows avec la règle de sécurité de connexion de fonctions avancées de sécurité. Cela exempte de la protection IPsec à partir de l’espace d’adressage IPv6 de votre intranet pour les adresses IPv6 de vos points de distribution CRL.  
  
4.  Assurez-vous que les clients DirectAccess sur le réseau interne peuvent résoudre le nom du serveur d’emplacement réseau, et que les clients DirectAccess sur Internet ne peut pas résoudre le nom.  
  
## <a name="BKMK_Links"></a>Voir aussi  
  
-   [Étape 2 : Configurer le serveur d’accès à distance](Step-2-Configure-the-Remote-Access-Server.md)

