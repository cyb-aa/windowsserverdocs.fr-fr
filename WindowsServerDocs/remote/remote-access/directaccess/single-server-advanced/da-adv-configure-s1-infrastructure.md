---
title: Étape 1 configurer une infrastructure DirectAccess avancée
description: Cette rubrique fait partie du guide déployer un serveur DirectAccess unique avec des paramètres avancés pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 43abc30a-300d-4752-b845-10a6b9f32244
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 30705a9aa55cdc652280c27c327cf865a47c5a11
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404939"
---
# <a name="step-1-configure-advanced-directaccess-infrastructure"></a>Étape 1 configurer une infrastructure DirectAccess avancée

>S'applique à : Windows Server 2012 R2, Windows Server 2012

Cette rubrique décrit comment configurer l'infrastructure requise pour un déploiement avancé de l'accès à distance qui utilise un serveur DirectAccess unique dans un environnement mixte IPv4 et IPv6. Avant de commencer les étapes de déploiement, assurez-vous que vous avez effectué les étapes de planification décrites dans [planifier un déploiement avancé de DirectAccess](../../../remote-access/directaccess/single-server-advanced/Plan-an-Advanced-DirectAccess-Deployment.md).  
  
|Tâche|Description|  
|----|--------|  
|1.1 Configurer les paramètres réseau serveur|Configurez les paramètres réseau serveur sur le serveur DirectAccess.|  
|1.2 Configurer le tunneling forcé|Configurez le tunneling forcé.|  
|1.3 Configurer le routage dans le réseau d'entreprise|Configurez le routage dans le réseau d'entreprise.|  
|1.4 Configurer les pare-feu|Configurer des pare-feu supplémentaires, si nécessaire.|  
|1.5 Configurer les autorités de certification et les certificats|Configurez une autorité de certification, si nécessaire, et tout autre modèle de certificat requis dans le déploiement.|  
|1.6 Configurer le serveur DNS|Configurez les paramètres DNS (Domain Name System) pour le serveur DirectAccess.|  
|1.7 Configurer Active Directory|Joignez les ordinateurs clients et le serveur DirectAccess au domaine Active Directory.|  
|1.8 Configurer les objets de stratégie de groupe|Configurez les objets de stratégie de groupe pour le déploiement, le cas échéant.|  
|1.9 Configurer les groupes de sécurité|Configurez les groupes de sécurité qui contiendront les ordinateurs clients DirectAccess, ainsi que tous les autres groupes de sécurité requis dans le déploiement.|  
|1.10 Configurer le serveur Emplacement réseau|Configurez le serveur Emplacement réseau, y compris l'installation du certificat de site web du serveur Emplacement réseau.|  
  
> [!NOTE]  
> Cette rubrique inclut des exemples d'applets de commande Windows PowerShell que vous pouvez utiliser pour automatiser certaines des procédures décrites. Pour plus d’informations, consultez [Utilisation des applets de commande](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="ConfigNetworkSettings"></a>1,1 configurer les paramètres réseau du serveur  
Les paramètres d'interface réseau suivants sont requis pour un déploiement à un seul serveur dans un environnement utilisant IPv4 et IPv6. Toutes les adresses IP sont configurées à l'aide de l'option **Modifier les paramètres de la carte** du **Centre Réseau et partage Windows**.  
  
**Topologie de périphérie**  
  
-   Deux adresses IPv4 ou IPv6 statiques publiques, consécutives côté Internet  
  
    > [!NOTE]  
    > Deux adresses publiques sont requises pour Teredo. Si vous n'utilisez pas Teredo, vous pouvez configurer une seule adresse IPv4 statique publique.  
  
-   Une seule adresse IPv4 ou IPv6 statique interne  
  
**Derrière un périphérique NAT (avec deux cartes réseau)**  
  
-   Une seule adresse IPv4 ou IPv6 statique côté Internet  
  
-   Une seule adresse IPv4 ou IPv6 statique interne côté réseau  
  
**Derrière un périphérique NAT (avec une seule carte réseau)**  
  
-   Une seule adresse IPv4 ou IPv6 statique interne côté réseau  
  
> [!NOTE]  
> Si un serveur DirectAccess avec deux cartes réseau ou plus (une classée dans le profil de domaine et l'autre dans un profil public ou privé) est configuré avec une topologie à une seule carte réseau, nous vous conseillons de procéder comme suit :  
>   
> -   Assurez-vous que la seconde carte réseau et toutes les éventuelles cartes réseau supplémentaires sont classées dans le profil de domaine.  
> -   Si la deuxième carte réseau ne peut pas être configurée pour le profil de domaine, la stratégie IPsec DirectAccess doit être limitée manuellement à tous les profils à l’aide de la commande Windows PowerShell suivante après la configuration de DirectAccess :  
>   
>     ```  
>     $gposession = Open-NetGPO "PolicyStore <Name of the server GPO>  
>     Set-NetIPsecRule "DisplayName <Name of the IPsec policy> "GPOSession $gposession "Profile Any  
>     Save-NetGPO "GPOSession $gposession  
>     ```  
  
## <a name="BKMK_forcetunnel"></a>1,2 configurer le tunneling forcé  
Le tunneling forcé peut être configuré à l'aide de l'Assistant Configuration de l'accès à distance. Il est présenté sous la forme d'une case à cocher dans l'Assistant Configuration des clients distants. Ce paramètre affecte uniquement les clients DirectAccess. Si le réseau VPN est activé, les clients VPN utilisent par défaut le tunneling forcé. Les administrateurs peuvent modifier ce paramètre pour les clients VPN à partir du profil client.  
  
Cocher la case pour le tunneling forcé a les conséquences suivantes :  
  
-   cela active le tunneling forcé sur les clients DirectAccess ;  
  
-   cela ajoute une entrée **Any** dans la table de stratégie de résolution de noms (NRPT) pour les clients DirectAccess, ce qui signifie que tout le trafic DNS sera dirigé vers les serveurs DNS du réseau interne ;  
  
-   cela configure les clients DirectAccess de sorte qu'ils utilisent toujours la technologie de transition IP-HTTPS.  
  
Pour mettre des ressources Internet à la disposition des clients DirectAccess qui utilisent le tunneling forcé, vous pouvez utiliser un serveur proxy susceptible de recevoir les demandes IPv6 pour des ressources Internet et les traduire en demandes pour des ressources Internet IPv4. Pour configurer un serveur proxy pour les ressources Internet, vous devez modifier l'entrée par défaut dans la table NRPT pour ajouter le serveur proxy. Pour cela, vous pouvez utiliser les applets de commande PowerShell pour l'accès à distance ou les applets de commande PowerShell pour DNS. Par exemple, utilisez l'applet de commande PowerShell pour l'accès à distance comme suit :  
  
```  
Set-DAClientDNSConfiguration "DNSSuffix "." "ProxyServer <Name of the proxy server:port>  
```  
  
> [!NOTE]  
> Si DirectAccess et le réseau VPN sont activés sur un même serveur, que le réseau VPN est en mode de tunneling forcé et que le serveur est déployé dans une topologie de périmètre ou une topologie Derrière NAT (avec deux cartes réseau, l'une connectée au domaine et l'autre à un réseau privé), le trafic Internet VPN ne peut pas être transféré via l'interface externe du serveur DirectAccess. Pour implémenter ce scénario, les organisations doivent déployer l'accès à distance sur le serveur derrière un pare-feu dans une topologie à une seule carte réseau. Les organisations peuvent également utiliser un serveur proxy distinct dans le réseau interne pour transférer le trafic Internet provenant des clients VPN.  
  
> [!NOTE]  
> Si une organisation utilise un proxy web pour que les clients DirectAccess accèdent aux ressources Internet et que le proxy d'entreprise n'est pas capable de traiter les ressources réseau internes, les clients DirectAccess ne sont pas en mesure d'accéder aux ressources internes s'ils sont à l'extérieur de l'intranet. Dans un tel scénario, pour permettre aux clients DirectAccess d'accéder aux ressources internes, créez manuellement des entrées NRPT pour les suffixes du réseau interne en utilisant la page DNS de l'Assistant d'infrastructure. N'appliquez pas les paramètres de proxy sur ces suffixes NRPT. Ces suffixes doivent être remplis avec les entrées du serveur DNS par défaut.  
  
## <a name="ConfigRouting"></a>1,3 configurer le routage dans le réseau d’entreprise  
Procédez comme suit pour configurer le routage dans le réseau d'entreprise :  
  
-   Lorsque le protocole IPv6 natif est déployé dans l'organisation, ajoutez un itinéraire afin que les routeurs du réseau interne redirigent le trafic IPv6 via le serveur DirectAccess.  
  
-   Configurez manuellement les itinéraires IPv4 et IPv6 de l’organisation sur les serveurs DirectAccess. Ajoutez un itinéraire publié afin que tout le trafic ayant un préfixe IPv6 (/48) d'organisation soit transféré au réseau interne. Pour le trafic IPv4, ajoutez des itinéraires explicites afin que le trafic IPv4 soit transféré au réseau interne.  
  
## <a name="ConfigFirewalls"></a>1,4 configurer des pare-feu  
Lorsque vous utilisez des pare-feu supplémentaires dans votre déploiement, appliquez les exceptions de pare-feu côté Internet suivantes pour le trafic d'accès à distance lorsque le serveur DirectAccess est sur le réseau Internet IPv4 :  
  
-   Trafic Teredo : port de destination UDP (User Datagram Protocol) 3544 entrant et port UDP source 3544 sortant.  
  
-   trafic 6to4 «protocole IP 41 entrant et sortant.  
  
-   Le port de destination TCP 443 IP-HTTPs et le port TCP source 443 sortant. Lorsque le serveur DirectAccess dispose d'une seule carte réseau et que le serveur Emplacement réseau se trouve sur le serveur DirectAccess, le port TCP 62000 est également requis.  
  
    > [!NOTE]  
    > Cette exemption doit être configurée sur le serveur DirectAccess, alors que toutes les autres exemptions doivent être configurées sur le pare-feu de périmètre.  
  
> [!NOTE]  
> Pour le trafic Teredo et 6to4, ces exceptions doivent être appliquées pour les deux adresses IPv4 publiques consécutives côté Internet sur le serveur DirectAccess. Pour IP-HTTPS, les exceptions ne doivent être appliquées qu’à l’adresse de résolution du nom public du serveur.  
  
Lorsque vous utilisez des pare-feu supplémentaires, appliquez les exceptions de pare-feu côté Internet suivantes pour le trafic d'accès à distance lorsque le serveur DirectAccess est sur le réseau Internet IPv6 :  
  
-   Protocole IP 50  
  
-   Port UDP de destination 500 entrant et port UDP source 500 sortant.  
  
-   Trafic entrant et sortant du trafic ICMPv6 (Internet Control Message Protocol pour IPv6) uniquement pour les implémentations Teredo.  
  
Lorsque vous utilisez des pare-feu supplémentaires, appliquez les exceptions de pare-feu de réseau interne suivantes pour le trafic d’accès à distance :  
  
-   ISATAP "Protocole 41 entrant et sortant  
  
-   TCP/UDP pour tout le trafic IPv4/IPv6  
  
-   ICMP pour tout le trafic IPv4/IPv6  
  
## <a name="ConfigCAs"></a>1,5 configurer les autorités de certification et les certificats  
L’accès à distance dans Windows Server 2012 vous permet de choisir entre l’utilisation de certificats pour l’authentification de l’ordinateur et l’utilisation d’un proxy Kerberos intégré qui s’authentifie à l’aide de noms d’utilisateur et de mots de passe. Vous devez également configurer un certificat IP-HTTPS sur le serveur DirectAccess.  
  
Pour plus d’informations, voir [Services de certificat Active Directory](https://technet.microsoft.com/library/cc770357.aspx).  
  
### <a name="151-configure-ipsec-authentication"></a>1.5.1 Configurer l'authentification IPsec  
Un certificat d'ordinateur est requis sur le serveur DirectAccess et sur tous les clients DirectAccess pour pouvoir utiliser l'authentification IPsec. Ce certificat doit être émis par une autorité de certification interne et les serveurs et clients DirectAccess doivent faire confiance à la chaîne d'autorité de certification qui émet les certificats racines et intermédiaires.  
  
##### <a name="to-configure-ipsec-authentication"></a>Pour configurer l'authentification IPsec  
  
1.  Dans l’autorité de certification interne, décidez si vous allez utiliser le modèle de certificat d' **ordinateur** , ou si vous allez créer un nouveau modèle de certificat comme décrit dans [création de modèles de certificats](https://technet.microsoft.com/library/cc731705.aspx).  
  
    > [!NOTE]  
    > Si vous créez un nouveau modèle, vous devez le configurer pour l'authentification client.  
  
2.  Déployez le modèle de certificat, si nécessaire. Pour plus d’informations, voir [Déploiement de modèles de certificat](https://technet.microsoft.com/library/cc770794.aspx).  
  
3.  Configurez le modèle de certificat pour l'inscription automatique, si nécessaire. Pour plus d’informations, voir [Configurer l’inscription automatique de certificat](https://technet.microsoft.com/library/cc731522.aspx).  
  
### <a name="ConfigCertTemp"></a>1.5.2 configurer les modèles de certificats  
Lorsque vous utilisez une autorité de certification interne pour émettre des certificats, vous devez configurer un modèle de certificat pour le certificat IP-HTTPS et le certificat de site web du serveur Emplacement réseau.  
  
##### <a name="to-configure-a-certificate-template"></a>Pour configurer un modèle de certificat  
  
1.  Sur l’autorité de certification interne, créez un modèle de certificat comme décrit dans [Création de modèles de certificat](https://technet.microsoft.com/library/cc731705.aspx).  
  
2.  Déployez le modèle de certificat comme décrit dans [Déploiement de modèles de certificat](https://technet.microsoft.com/library/cc770794.aspx).  
  
### <a name="153-configure-the-ip-https-certificate"></a>1.5.3 Configurer le certificat IP-HTTPS  
L'accès à distance nécessite un certificat IP-HTTPS pour authentifier les connexions IP-HTTPS auprès du serveur DirectAccess. Il existe trois options de certificat disponibles pour l'authentification IP-HTTPS :  
  
**Certificat public**  
  
Un certificat public est fourni par un tiers. Si le nom du sujet du certificat ne contient pas de caractère générique, il doit s'agir de l'URL de nom de domaine complet (FQDN) pouvant être résolue en externe, qui est utilisée uniquement pour les connexions IP-HTTPS du serveur DirectAccess.  
  
**Certificat privé**  
  
Si vous utilisez un certificat privé, les éléments suivants sont requis, s'ils n'existent pas encore :  
  
-   Un certificat de site web utilisé pour l'authentification IP-HTTPS. L’objet du certificat doit être un nom complet FQDN pouvant être résolu en externe accessible à partir d’Internet. Le certificat est basé sur le modèle de certificat que vous avez créé en suivant les instructions dans 1.5.2 configurer les modèles de certificats.  
  
-   Un point de distribution de liste de révocation de certificats accessible à partir d’un nom FQDN pouvant être résolu publiquement.  
  
**Certificat auto-signé**  
  
Si vous utilisez un certificat auto-signé, les éléments suivants sont requis, s'ils n'existent pas encore :  
  
-   Un certificat de site web utilisé pour l'authentification IP-HTTPS. L’objet du certificat doit être un nom complet FQDN pouvant être résolu en externe accessible à partir d’Internet.  
  
-   Un point de distribution de liste de révocation de certificats accessible à partir d'un nom FQDN pouvant être résolu publiquement.  
  
> [!NOTE]  
> Les certificats auto-signés ne sont pas utilisables dans des déploiements multisites.  
  
Assurez-vous que le certificat de site web utilisé pour l'authentification IP-HTTPS est conforme aux exigences suivantes :  
  
-   Le nom commun du certificat doit correspondre au nom du site IP-HTTPS.  
  
-   Dans le champ **objet** , spécifiez le nom de domaine complet de l’URL IP-HTTPS.  
  
-   Pour le champ **Utilisation améliorée de la clé**, utilisez l'identificateur d'objet (OID) d'authentification du serveur.  
  
-   Pour le champ **Points de distribution de la liste de révocation de certificats**, indiquez un point de distribution de liste de révocation de certificats accessible par les clients DirectAccess connectés à Internet  
  
-   Le certificat IP-HTTPS doit avoir une clé privée.  
  
-   Le certificat IP-HTTPS doit être importé directement dans le magasin personnel.  
  
-   Les certificats IP-HTTPS peuvent utiliser des caractères génériques dans le nom.  
  
##### <a name="to-install-the-ip-https-certificate-from-an-internal-ca"></a>Pour installer le certificat IP-HTTPS à partir d'une autorité de certification interne  
  
1.  Sur le serveur DirectAccess : Dans l’écran **Démarrer** , tapez**MMC. exe**, puis appuyez sur entrée.  
  
2.  Dans la console MMC, dans le menu **Fichier**, cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**.  
  
3.  Dans la boîte de dialogue **Ajouter ou supprimer des composants logiciels enfichables**, cliquez sur **Certificats**, sur **Ajouter**, sur **Le compte de l'ordinateur**, sur **Suivant**, sur **Ordinateur local**, sur **Terminer**, puis sur **OK**.  
  
4.  Dans l'arborescence de la console du composant logiciel enfichable Certificats, ouvrez **Certificats (ordinateur local)\Personnel\Certificats**.  
  
5.  Cliquez avec le bouton droit sur **Certificats**, pointez sur **Toutes les Tâches**, puis cliquez sur **Demander un nouveau certificat**.  
  
6.  Cliquez sur **Suivant** deux fois.  
  
7.  Dans la page **demander des certificats** , activez la case à cocher correspondant au modèle de certificat que vous avez créé précédemment (pour plus d’informations, consultez 1.5.2 configurer les modèles de certificats). Si nécessaire, cliquez sur **L'inscription pour obtenir ce certificat nécessite des informations supplémentaires**.  
  
8.  Dans la boîte de dialogue **Propriétés du certificat**, sous l'onglet **Sujet**, dans la zone **Nom du sujet**, dans **Type**, sélectionnez **Nom commun**.  
  
9. Dans **Valeur**, spécifiez l'adresse IPv4 de la carte côté externe du serveur DirectAccess ou le nom de domaine complet (FQDN) de l'URL IP-HTTPS, puis cliquez sur **Ajouter**.  
  
10. Dans la zone **Autre nom**, dans **Type**, sélectionnez **DNS**.  
  
11. Dans **Valeur**, spécifiez l'adresse IPv4 de la carte côté externe du serveur DirectAccess ou le nom de domaine complet (FQDN) de l'URL IP-HTTPS, puis cliquez sur **Ajouter**.  
  
12. Sous l'onglet **Général**, dans **Nom convivial**, vous pouvez entrer un nom qui vous permettra d'identifier le certificat.  
  
13. Sous l'onglet **Extensions**, cliquez sur la flèche en regard de l'option **Utilisation améliorée de la clé** et assurez-vous qu'**Authentification du serveur** apparaît dans la liste **Expressions sélectionnées**.  
  
14. Cliquez sur **OK**, sur **Inscrire**, puis sur **Terminer**.  
  
15. Dans le volet d'informations du composant logiciel enfichable Certificats, vérifiez que le nouveau certificat a été inscrit avec Rôles prévus égal à Authentification du serveur.  
  
## <a name="ConfigDNS"></a>1,6 configuration du serveur DNS  
Vous devez configurer manuellement une entrée DNS pour le site web du serveur Emplacement réseau du réseau interne de votre déploiement.  
  
### <a name="NLS_DNS"></a>Pour créer le serveur d’emplacement réseau  
  
1.  Sur le serveur DNS du réseau interne : Dans l’écran d' **Accueil** , tapez**dnsmgmt. msc**, puis appuyez sur entrée.  
  
2.  Dans le volet gauche de la console **Gestionnaire DNS**, développez la zone de recherche directe de votre domaine. Cliquez avec le bouton droit sur le domaine et cliquez sur **Nouvel hôte (A ou AAAA)** .  
  
3.  Dans la boîte de dialogue **Nouvel hôte**, dans la zone **Adresse IP** :  
  
    -   Dans la zone **Nom (utilise le domaine parent si ce champ est vide)** , entrez le nom DNS du site web du serveur Emplacement réseau (il s'agit du nom que les clients DirectAccess utilisent pour se connecter au serveur Emplacement réseau).  
  
    -   Entrez l'adresse IPv4 ou IPv6 du serveur Emplacement réseau, puis cliquez sur **Ajouter un hôte** et sur **OK**.  
  
4.  Dans la boîte de dialogue **Nouvel hôte** :  
  
    -   Dans la zone **Nom (utilise le domaine parent si ce champ est vide)** , entrez le nom DNS de la sonde web (le nom de la sonde web par défaut est **directaccess-webprobehost**).  
  
    -   Dans la zone **Adresse IP**, entrez l'adresse IPv4 ou IPv6 de la sonde web, puis cliquez sur **Ajouter un hôte**.  
  
    -   Répétez ce processus pour **directaccess-corpconnectivityhost** et tout vérificateur de connectivité créé manuellement.  
  
5.  Dans la boîte de dialogue **DNS**, cliquez sur **OK**, puis sur **Terminé**.  
  
](../../../media/Step-1-Configuring-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em> @no__t 0Windows PowerShell***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
Add-DnsServerResourceRecordA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv4Address <network_location_server_IPv4_address>  
Add-DnsServerResourceRecordAAAA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv6Address <network_location_server_IPv6_address>  
```  
  
Vous devez également configurer les entrées DNS pour les éléments suivants :  
  
-   **Serveur IP-HTTPs**  
  
    Les clients DirectAccess doivent être en mesure de résoudre le nom DNS du serveur DirectAccess à partir d'Internet.  
  
-   **Vérification de la révocation des certificats**  
  
    DirectAccess utilise la vérification de la révocation des certificats pour la connexion IP-HTTPS entre les clients DirectAccess et le serveur DirectAccess, ainsi que pour la connexion HTTPS entre le client DirectAccess et le serveur Emplacement réseau. Dans les deux cas, les clients DirectAccess doivent être en mesure de résoudre le point de distribution de liste de révocation de certificats et d'y accéder.  
  
-   **ISATAP**  
  
    Le protocole ISATAP (IntraSite Automatic Tunnel Addressing Protocol) utilise le tunneling pour permettre aux clients DirectAccess de se connecter au serveur DirectAccess sur le réseau Internet IPv4, en encapsulant les paquets IPv6 au sein d'un en-tête IPv4. Il est utilisé par l'accès à distance pour fournir la connectivité IPv6 aux hôtes ISATAP sur un intranet. Dans un environnement réseau IPv6 non natif, le serveur DirectAccess se configure lui-même automatiquement en tant que routeur ISATAP. La prise en charge de la résolution pour le nom ISATAP est requise.  
  
## <a name="ConfigAD"></a>1,7 configurer Active Directory  
Le serveur DirectAccess et tous les ordinateurs clients DirectAccess doivent être joints à un domaine Active Directory. Les ordinateurs clients DirectAccess doivent être membres de l'un des types de domaines suivants :  
  
-   Domaines qui appartiennent à la même forêt que le serveur DirectAccess  
  
-   Domaines qui appartiennent à des forêts dotées d'une relation d'approbation bidirectionnelle avec la forêt du serveur DirectAccess  
  
-   Domaines dotés d'une approbation bidirectionnelle avec le domaine du serveur DirectAccess  
  
#### <a name="to-join-the-directaccess-server-to-a-domain"></a>Pour joindre le serveur DirectAccess à un domaine  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **Serveur local**. Dans le volet d’informations, cliquez sur le lien en regard de **Nom de l’ordinateur**.  
  
2.  Dans la boîte de dialogue **Propriétés système**, cliquez sur l'onglet **Nom de l'ordinateur**, puis sur **Changer**.  
  
3.  Dans **Nom de l’ordinateur**, tapez le nom de l’ordinateur si vous modifiez également le nom de l’ordinateur lors de la jonction du serveur au domaine. Sous **Membre de**, cliquez sur **Domaine**, et tapez le nom du domaine auquel vous voulez joindre le serveur (par exemple, corp.contoso.com), puis cliquez sur **OK**.  
  
4.  Lorsque vous êtes invités à entrer un nom d’utilisateur et un mot de passe, tapez le nom d’utilisateur et le mot de passe d’un utilisateur qui dispose des droits pour joindre des ordinateurs au domaine, puis cliquez sur **OK**.  
  
5.  Lorsqu'une boîte de dialogue de bienvenue s'affiche dans le domaine, cliquez sur **OK**.  
  
6.  Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **OK**.  
  
7.  Dans la boîte de dialogue **Propriétés système**, cliquez sur **Fermer**.  
  
8.  Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **Redémarrer maintenant**.  
  
#### <a name="to-join-client-computers-to-the-domain"></a>Pour joindre des ordinateurs clients au domaine  
  
1.  Dans l’écran d' **Accueil** , tapez**Explorer. exe**, puis appuyez sur entrée.  
  
2.  Cliquez avec le bouton droit sur l'icône Ordinateur, puis cliquez sur **Propriétés**.  
  
3.  Dans la page **Système**, cliquez sur **Paramètres système avancés**.  
  
4.  Dans la boîte de dialogue **Propriétés système** , sous l’onglet **Nom de l’ordinateur** , cliquez sur **Modifier**.  
  
5.  Dans **Nom de l'ordinateur**, tapez le nom de l'ordinateur si vous modifiez également le nom de l'ordinateur lors de la jonction du serveur au domaine. Sous **Membre de**, cliquez sur **Domaine**, et tapez le nom du domaine auquel vous voulez joindre le serveur (par exemple, corp.contoso.com), puis cliquez sur **OK**.  
  
6.  Lorsque vous êtes invités à entrer un nom d’utilisateur et un mot de passe, tapez le nom d’utilisateur et le mot de passe d’un utilisateur qui dispose des droits pour joindre des ordinateurs au domaine, puis cliquez sur **OK**.  
  
7.  Lorsqu'une boîte de dialogue de bienvenue s'affiche dans le domaine, cliquez sur **OK**.  
  
8.  Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **OK**.  
  
9. Dans la boîte de dialogue **Propriétés système**, cliquez sur **Fermer**.  
  
10. Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **Redémarrer maintenant**.  
  
](../../../media/Step-1-Configuring-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em> @no__t 0Windows PowerShell***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
> [!NOTE]  
> Vous devez fournir des informations d'identification de domaine quand vous entrez la commande **Add-Computer** suivante.  
  
```  
Add-Computer -DomainName <domain_name>  
Restart-Computer  
```  
  
## <a name="ConfigGPOs"></a>1,8 configurer les objets de stratégie de groupe  
Un minimum de deux objets stratégie de groupe sont requis pour déployer l’accès à distance :  
  
-   l'un contient les paramètres pour le serveur DirectAccess ;  
  
-   l'autre contient les paramètres pour les ordinateurs clients DirectAccess.  
  
Quand vous configurez l’accès à distance, l’Assistant crée automatiquement les objets de stratégie de groupe requis. Toutefois, si votre organisation impose une convention d'affectation des noms, vous pouvez taper un nom dans la boîte de dialogue d'objet de stratégie de groupe dans la console de gestion de l'accès à distance. Pour plus d’informations, consultez 2,7. Résumé de la configuration et autres objets de stratégie de groupe. Si vous avez créé des autorisations, l'objet de stratégie de groupe sera créé. Si vous ne disposez pas des autorisations requises pour créer des objets de stratégie de groupe, ceux-ci doivent être créés avant la configuration de l'accès à distance.  
  
Pour créer des objets stratégie de groupe, consultez [créer et modifier un objet stratégie de groupe](https://technet.microsoft.com/library/cc754740.aspx).  
  
> [!IMPORTANT]  
> Les administrateurs peuvent lier manuellement les objets de stratégie de groupe DirectAccess à une unité d’organisation (UO) en procédant comme suit :  
>   
> 1.  Avant de configurer DirectAccess, liez les objets de stratégie de groupe créés aux unités d'organisation respectives.  
> 2.  Configurez DirectAccess en spécifiant un groupe de sécurité pour les ordinateurs clients.  
> 3.  L’administrateur de l’accès à distance peut être autorisé ou non à lier les objets stratégie de groupe au domaine. Dans l’un ou l’autre cas, les objets de stratégie de groupe seront automatiquement configurés. Si les objets de stratégie de groupe sont déjà liés à une unité d'organisation, les liens ne seront pas supprimés, et les objets de stratégie de groupe ne seront pas liés au domaine. Pour un objet de stratégie de groupe serveur, l'unité d'organisation doit contenir l'objet ordinateur serveur, ou l'objet de stratégie de groupe sera lié à la racine du domaine.  
> 4.  Si vous n’avez pas lié l’unité d’organisation avant d’exécuter l’Assistant DirectAccess, une fois la configuration terminée, l’administrateur de domaine peut lier les objets de stratégie de groupe DirectAccess aux unités d’organisation requises. Il est possible de supprimer le lien au domaine. Pour plus d’informations, voir [Lier un objet de stratégie de groupe](https://technet.microsoft.com/library/cc732979.aspx).  
  
> [!NOTE]  
> Si un objet stratégie de groupe a été créé manuellement, il est possible que l’objet stratégie de groupe ne soit pas disponible pendant la configuration de DirectAccess. L’objet stratégie de groupe n’a peut-être pas été répliqué sur le contrôleur de domaine le plus proche sur l’ordinateur de gestion. Dans ce cas, l'administrateur peut attendre la fin de la réplication ou forcer la réplication.  
  
### <a name="181-configure-remote-access-gpos-with-limited-permissions"></a>1.8.1 Configurer les objets de stratégie de groupe d'accès à distance avec des autorisations limitées  
Dans un déploiement qui utilise des objets de stratégie de groupe intermédiaires et de production, l’administrateur du domaine doit effectuer les opérations suivantes :  
  
1.  Obtenir la liste des objets de stratégie de groupe requis pour le déploiement de l'accès à distance à partir de l'administrateur de l'accès à distance. Pour plus d’informations, consultez 1,8 plan stratégie de groupe objets.  
  
2.  Pour chaque objet de stratégie de groupe demandé par l'administrateur de l'accès à distance, créer une paire d'objets de stratégie de groupe avec des noms différents. Le premier sera utilisé comme objet de stratégie de groupe intermédiaire et le second comme objet de stratégie de groupe de production.  
  
    Pour créer des objets stratégie de groupe, consultez [créer et modifier un objet stratégie de groupe](https://technet.microsoft.com/library/cc754740.aspx).  
  
3.  Pour lier les objets de stratégie de groupe de production, voir [Lier un objet de stratégie de groupe](https://technet.microsoft.com/library/cc732979).  
  
4.  Accordez à l'administrateur de l'accès à distance les autorisations **Modifier les paramètres, supprimer, modifier la sécurité** sur tous les objets de stratégie de groupe intermédiaires. Pour plus d’informations, voir [Déléguer des autorisations pour un groupe ou un utilisateur sur un objet de stratégie de groupe](https://technet.microsoft.com/library/cc754542).  
  
5.  Refuser les autorisations d’administrateur d’accès à distance pour lier des objets de stratégie de groupe dans tous les domaines (ou vérifier que l’administrateur de l’accès à distance ne dispose pas de ces autorisations). Pour plus d’informations, voir [Déléguer des autorisations pour lier des objets de stratégie de groupe](https://technet.microsoft.com/library/cc755086).  
  
Quand les administrateurs de l'accès à distance configurent l'accès à distance, ils doivent toujours spécifier uniquement les objets de stratégie de groupe intermédiaires (pas les objets de stratégie de groupe de production). Ceci est vrai dans la configuration initiale de l'accès à distance et lors de l'exécution d'opérations de configuration supplémentaires nécessitant des objets de stratégie de groupe supplémentaires. Par exemple, lors de l'ajout de points d'entrée dans un déploiement multisite ou de l'activation d'ordinateurs clients dans des domaines supplémentaires.  
  
Une fois que l'administrateur de l'accès à distance a terminé toutes les modifications de la configuration de l'accès à distance, l'administrateur de domaine doit passer en revue les paramètres figurant dans les objets de stratégie de groupe intermédiaires, et utiliser la procédure suivante pour copier les paramètres dans les objets de stratégie de groupe de production.  
  
> [!TIP]  
> Effectuez la procédure suivante après chaque modification de la configuration de l'accès à distance.  
  
##### <a name="to-copy-settings-to-the-production-gpos"></a>Pour copier les paramètres dans les objets de stratégie de groupe de production  
  
1.  Vérifiez que tous les objets de stratégie de groupe intermédiaires dans le déploiement de l'accès à distance ont été répliqués sur tous les contrôleurs de domaine figurant dans le domaine. Ceci est requis pour garantir que la configuration la plus à jour est importée dans les objets de stratégie de groupe de production. Pour plus d’informations, consultez vérifier stratégie de groupe état de l’infrastructure.  
  
2.  Exportez les paramètres en sauvegardant tous les objets de stratégie de groupe intermédiaires dans le déploiement de l'accès à distance. Pour plus d’informations, consultez sauvegarder un objet stratégie de groupe.  
  
3.  Pour chaque objet de stratégie de groupe de production, modifiez les filtres de sécurité pour qu'ils correspondent aux filtres de sécurité de l'objet de stratégie de groupe intermédiaire correspondant. Pour plus d’informations, consultez filtrer à l’aide de groupes de sécurité.  
  
    > [!NOTE]  
    > Ceci est nécessaire car l'option **Importer les paramètres** ne copie pas le filtre de sécurité de l'objet de stratégie de groupe source.  
  
4.  Pour chaque objet de stratégie de groupe de production, importez les paramètres à partir de la sauvegarde de l'objet de stratégie de groupe intermédiaire correspondant, comme suit :  
  
    1.  Dans la Console de gestion des stratégies de groupe (GPMC), développez le nœud objets stratégie de groupe dans la forêt et le domaine qui contient l’objet de stratégie de groupe de production dans lequel les paramètres seront importés.  
  
    2.  Cliquez avec le bouton droit sur l'objet de stratégie de groupe et cliquez sur **Importer les paramètres**.  
  
    3.  Dans l'**Assistant Importation des paramètres**, dans la page **Bienvenue**, cliquez sur **Suivant**.  
  
    4.  Dans la page **Objet de stratégie de groupe de sauvegarde**, cliquez sur **Sauvegarde**.  
  
    5.  Dans la boîte de dialogue **Objet de stratégie de groupe de sauvegarde**, dans la zone **Emplacement**, entrez le chemin de l'emplacement où vous voulez stocker les sauvegardes des objets de stratégie de groupe, ou cliquez sur **Parcourir** pour localiser le dossier.  
  
    6.  Dans la zone **Description**, tapez une description de l'objet de stratégie de groupe de production, puis cliquez sur **Sauvegarder**.  
  
    7.  Une fois la sauvegarde terminée, cliquez sur **OK**, puis, dans la page **Objet de stratégie de groupe de sauvegarde**, cliquez sur **Suivant**.  
  
    8.  Dans la page **Emplacement de sauvegarde**, dans la zone **Dossier de sauvegarde**, entrez le chemin d'accès de l'emplacement dans lequel la sauvegarde de l'objet de stratégie de groupe intermédiaire correspondant a été stockée à l'étape 2, ou cliquez sur **Parcourir** pour localiser le dossier, puis cliquez sur **Suivant**.  
  
    9. Dans la page **Objet de stratégie de groupe (GPO) source**, cochez la case **N'afficher que la dernière version des objets GPO** pour masquer les sauvegardes plus anciennes, et sélectionnez l'objet de stratégie de groupe intermédiaire correspondant. Cliquez sur **Afficher les paramètres** pour passer en revue les paramètres de l'accès à distance avant de les appliquer à l'objet de stratégie de groupe de production, puis cliquez sur **Suivant**.  
  
    10. Dans la page **Analyse de la sauvegarde**, cliquez sur **Suivant**, puis sur **Terminer**.  
  
](../../../media/Step-1-Configuring-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em> @no__t 0Windows PowerShell***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
-   Pour sauvegarder l’objet de stratégie de groupe de client intermédiaire « paramètres du client DirectAccess-intermédiaire » dans le domaine « corp.contoso.com » dans le dossier de sauvegarde «C:\Backups @ no__t-0 :  
  
    ```  
    $backup = Backup-GPO "Name 'DirectAccess Client Settings - Staging' "Domain 'corp.contoso.com' "Path 'C:\Backups\'  
    ```  
  
-   Pour afficher le filtrage de sécurité de l’objet de stratégie de groupe de client intermédiaire « paramètres du client DirectAccess-intermédiaire » dans le domaine « corp.contoso.com » :  
  
    ```  
    Get-GPPermission "Name 'DirectAccess Client Settings - Staging' "Domain 'corp.contoso.com' "All | ?{ $_.Permission "eq 'GpoApply'}  
    ```  
  
-   Pour ajouter le groupe de sécurité « Corp. contoso. com\DirectAccess clients » au filtre de sécurité de l’objet de stratégie de groupe de client de production « paramètres du client DirectAccess «production » dans le domaine « corp.contoso.com » :  
  
    ```  
    Set-GPPermission "Name 'DirectAccess Client Settings - Production' "Domain 'corp.contoso.com' "PermissionLevel GpoApply "TargetName 'corp.contoso.com\DirectAccess clients' "TargetType Group  
    ```  
  
-   Pour importer les paramètres de la sauvegarde vers l’objet de stratégie de groupe de client de production « paramètres du client DirectAccess «production » dans le domaine « corp.contoso.com » :  
  
    ```  
    Import-GPO "BackupId $backup.Id "Path $backup.BackupDirectory "TargetName 'DirectAccess Client Settings - Production' "Domain 'corp.contoso.com'  
    ```  
  
## <a name="ConfigSGs"></a>1,9 configurer des groupes de sécurité  
Les paramètres DirectAccess contenus dans l’objet stratégie de groupe de l’ordinateur client sont appliqués uniquement aux ordinateurs qui sont membres des groupes de sécurité que vous spécifiez lorsque vous configurez l’accès à distance. De plus, si vous utilisez des groupes de sécurité pour gérer vos serveurs d'applications, créez un groupe de sécurité pour ces serveurs.  
  
### <a name="Sec_Group"></a>Pour créer un groupe de sécurité pour les clients DirectAccess  
  
1.  Dans l’écran d' **Accueil** , tapez**DSA. msc**, puis appuyez sur entrée. Dans la console **Utilisateurs et ordinateurs Active Directory**, dans le volet gauche, développez le domaine contenant le groupe de sécurité, cliquez avec le bouton droit sur **Utilisateurs**, pointez sur **Nouveau**, puis cliquez sur **Groupe**.  
  
2.  Dans la boîte de dialogue **Nouvel objet - Groupe**, sous **Nom du groupe**, entrez le nom du groupe de sécurité.  
  
3.  Sous **Étendue du groupe**, cliquez sur **Globale**. Sous **Type de groupe**, cliquez sur **Sécurité**, puis cliquez sur **OK**.  
  
4.  Double-cliquez sur le groupe de sécurité des ordinateurs clients DirectAccess, puis, dans la boîte de dialogue des propriétés, cliquez sur l'onglet **Membres**.  
  
5.  Sous l'onglet **Membres** , cliquez sur **Ajouter**.  
  
6.  Dans la boîte de dialogue **Sélectionner Utilisateurs, contacts, ordinateurs ou comptes de service**, sélectionnez les ordinateurs clients que vous voulez activer pour DirectAccess, puis cliquez sur **OK**.  
  
](../../../media/Step-1-Configuring-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)**commandes Windows PowerShell équivalentes** @no__t 0Windows PowerShell  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
New-ADGroup -GroupScope global -Name <DirectAccess_clients_group_name>  
Add-ADGroupMember -Identity DirectAccess_clients_group_name -Members <computer_name>  
```  
  
## <a name="ConfigNLS"></a>1,10 Configuration du serveur emplacement réseau  
Le serveur Emplacement réseau doit être un serveur avec une haute disponibilité et il doit posséder un certificat SSL valide approuvé par les clients DirectAccess. Il existe deux options de certificat pour le certificat de serveur d'emplacement réseau :  
  
-   **Certificat privé**  
  
    Ce certificat est basé sur le modèle de certificat que vous avez créé en suivant les instructions dans [1.5.2 configurer les modèles de certificats](#ConfigCertTemp).  
  
-   **Certificat auto-signé**  
  
    > [!NOTE]  
    > Les certificats auto-signés ne sont pas utilisables dans des déploiements multisites.  
  
Les éléments suivants sont requis pour les deux types de certificats, s'ils n'existent pas encore :  
  
-   Un certificat de site web utilisé pour le serveur Emplacement réseau. Le sujet de certificat doit être l'URL du serveur Emplacement réseau.  
  
-   Un point de distribution de liste de révocation de certificats à haute disponibilité à partir du réseau interne.  
  
> [!NOTE]  
> Si le site web du serveur Emplacement réseau se trouve sur le serveur DirectAccess, un site web est créé automatiquement quand vous configurez l'accès à distance. Ce site est lié au certificat de serveur que vous fournissez.  
  
#### <a name="to-install-the-network-location-server-certificate-from-an-internal-ca"></a>Pour installer le certificat du serveur Emplacement réseau à partir d'une autorité de certification interne  
  
1.  Sur le serveur qui hébergera le site web du serveur Emplacement réseau : Dans l’écran **Démarrer** , tapez**MMC. exe**, puis appuyez sur entrée.  
  
2.  Dans la console MMC, dans le menu **Fichier**, cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**.  
  
3.  Dans la boîte de dialogue **Ajouter ou supprimer des composants logiciels enfichables**, cliquez sur **Certificats**, sur **Ajouter**, sur **Le compte de l'ordinateur**, sur **Suivant**, sur **Ordinateur local**, sur **Terminer**, puis sur **OK**.  
  
4.  Dans l'arborescence de la console du composant logiciel enfichable Certificats, ouvrez **Certificats (ordinateur local)\Personnel\Certificats**.  
  
5.  Cliquez avec le bouton droit sur **Certificats**, pointez sur **Toutes les Tâches**, puis cliquez sur **Demander un nouveau certificat**.  
  
6.  Cliquez sur **Suivant** deux fois.  
  
7.  Sur la page **demander des certificats** , activez la case à cocher correspondant au modèle de certificat que vous avez créé en suivant les instructions de 1.5.2 configurer les modèles de certificats. Si nécessaire, cliquez sur **L'inscription pour obtenir ce certificat nécessite des informations supplémentaires**.  
  
8.  Dans la boîte de dialogue **Propriétés du certificat**, sous l'onglet **Sujet**, dans la zone **Nom du sujet**, dans **Type**, sélectionnez **Nom commun**.  
  
9. Dans le champ **Valeur**, entrez le nom de domaine complet (FQDN) du site web du serveur Emplacement réseau, puis cliquez sur **Ajouter**.  
  
10. Dans la zone **Autre nom**, dans **Type**, sélectionnez **DNS**.  
  
11. Dans le champ **Valeur**, entrez le nom de domaine complet (FQDN) du site web du serveur Emplacement réseau, puis cliquez sur **Ajouter**.  
  
12. Sous l'onglet **Général**, dans **Nom convivial**, vous pouvez entrer un nom qui vous permettra d'identifier le certificat.  
  
13. Cliquez sur **OK**, sur **Inscrire**, puis sur **Terminer**.  
  
14. Dans le volet d'informations du composant logiciel enfichable Certificats, vérifiez qu'un nouveau certificat a été inscrit avec Rôles prévus égal à Authentification du serveur.  
  
#### <a name="to-configure-the-network-location-server"></a>Pour configurer le serveur Emplacement réseau  
  
1.  Configurez un site web sur un serveur à haute disponibilité. Le site web ne nécessite aucun contenu, mais quand vous le testez, vous pouvez définir une page par défaut qui fournit un message lorsque les clients se connectent.  
  
    > [!NOTE]  
    > Cette étape n'est pas nécessaire si le site web du serveur Emplacement réseau est hébergé sur le serveur DirectAccess.  
  
2.  Liez un certificat de serveur HTTPS au site web. Le nom commun du certificat doit correspondre au nom du site du serveur Emplacement réseau. Assurez-vous que les clients DirectAccess font confiance à l'autorité de certification émettrice.  
  
    > [!NOTE]  
    > Cette étape n'est pas nécessaire si le site web du serveur Emplacement réseau est hébergé sur le serveur DirectAccess.  
  
3.  Configurez un site de liste de révocation de certificats à haute disponibilité à partir du réseau interne.  
  
    Les points de distribution de listes de révocation de certificat sont accessibles via :  
  
    -   Serveurs Web à l’aide d’une URL HTTP, telle que : https://crl.corp.contoso.com/crld/corp-APP1-CA.crl  
  
    -   Serveurs de fichiers accessibles par le biais d’un chemin d’accès UNC (Universal Naming Convention), tel que \\ \ CRL. Corp. contoso. com\crld\corp-APP1-CA.crl  
  
    Si le point de distribution de liste de révocation de certificats interne est accessible uniquement via IPv6, vous devez configurer une règle de sécurité de connexion de Pare-feu Windows avec fonctions avancées de sécurité pour exempter la protection IPsec de l'adresse IPv6 de votre intranet jusqu'aux adresses IPv6 de vos points de distribution de liste de révocation de certificats.  
  
4.  Assurez-vous que les clients DirectAccess figurant sur le réseau interne peuvent résoudre le nom du serveur Emplacement réseau. Assurez-vous que le nom ne peut pas être résolu par les clients DirectAccess sur Internet.  
  
## <a name="BKMK_Links"></a>Étape suivante  
  
-   [Étape 2 : Configurer des serveurs DirectAccess avancés](da-adv-configure-s2-servers.md)  
  


