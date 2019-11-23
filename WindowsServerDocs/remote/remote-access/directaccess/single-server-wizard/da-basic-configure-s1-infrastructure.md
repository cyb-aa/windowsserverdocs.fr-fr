---
title: Étape 1 configurer l’infrastructure DirectAccess de base
description: Cette rubrique fait partie du guide déployer un serveur DirectAccess unique à l’aide de l’Assistant Prise en main pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ba4de2a4-f237-4b14-a8a7-0b06bfcd89ad
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2cd84949dddf75730aca6302f1244f784b5933d0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388574"
---
# <a name="step-1-configure-the-basic-directaccess-infrastructure"></a>Étape 1 configurer l’infrastructure DirectAccess de base

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit comment configurer l’infrastructure requise pour un déploiement de base DirectAccess qui utilise un serveur DirectAccess unique dans un environnement mixte IPv4 et IPv6. Avant de commencer les étapes de déploiement, assurez-vous que vous avez effectué les étapes de planification décrites dans [planifier un déploiement de base de DirectAccess](../../../remote-access/directaccess/single-server-wizard/Plan-a-Basic-DirectAccess-Deployment.md).  
  
|Tâche|Description|  
|----|--------|  
|Configurer les paramètres réseau serveur|Configurez les paramètres réseau serveur sur le serveur DirectAccess.|  
|Configurer le routage dans le réseau d'entreprise|Configurez le routage dans le réseau d'entreprise pour garantir que le trafic est correctement acheminé.|  
|Configurer les pare-feu|Configurer des pare-feu supplémentaires, si nécessaire.|  
|Configurer le serveur DNS|Configurer les paramètres DNS pour le serveur DirectAccess|  
|Configurer Active Directory|Joignez les ordinateurs clients et le serveur DirectAccess au domaine Active Directory.|  
|Configurer les objets de stratégie de groupe|Configurez les objets de stratégie de groupe pour le déploiement, le cas échéant.|  
|Configurer les groupes de sécurité|Configurez les groupes de sécurité qui contiendront les ordinateurs clients DirectAccess, ainsi que tous les autres groupes de sécurité requis dans le déploiement.|  
  
> [!NOTE]  
> Cette rubrique inclut des exemples d'applets de commande Windows PowerShell que vous pouvez utiliser pour automatiser certaines des procédures décrites. Pour plus d’informations, consultez [Utilisation des applets de commande](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="ConfigNetworkSettings"></a>Configurer les paramètres réseau du serveur  
Les paramètres d'interface réseau suivants sont requis pour un déploiement à un seul serveur dans un environnement avec IPv4 et IPv6. Toutes les adresses IP sont configurées à l'aide de l'option **Modifier les paramètres de la carte** du **Centre Réseau et partage Windows**.  
  
-   Topologie de périmètre  
  
    -   Une adresse IPv4 ou IPv6 statique publique côté Internet.  
  
        > [!NOTE]  
        > Deux adresses IPv4 publiques consécutives sont requises pour Teredo. Si vous n'utilisez pas Teredo, vous pouvez configurer une seule adresse IPv4 statique publique.  
  
    -   Une seule adresse IPv4 ou IPv6 statique interne.  
  
-   Derrière un périphérique NAT (deux cartes réseau)  
  
    -   Une seule adresse IPv4 ou IPv6 statique interne côté réseau.  
  
    -   Une seule adresse IPv4 ou IPv6 statique côté réseau de périmètre.  
  
-   Derrière un périphérique NAT (une carte réseau)  
  
    -   Une seule adresse IPv4 ou IPv6 statique.  
  
> [!NOTE]  
> Si le serveur DirectAccess dispose d’au moins deux cartes réseau (l’une classée dans le profil de domaine et l’autre dans un profil public/privé), mais que vous souhaitez utiliser une seule topologie de carte réseau, les recommandations sont les suivantes :  
>   
> 1.  Assurez-vous que la seconde carte réseau et toutes les éventuelles cartes réseau supplémentaires sont classées dans le profil de domaine.  
> 2.  Si, pour une raison quelconque, il est impossible de configurer la deuxième carte réseau pour le profil de domaine, vous devez définir manuellement l’étendue de la stratégie IPsec DirectAccess à tous les profils à l’aide des commandes Windows PowerShell suivantes :  
>   
>     ```  
>     $gposession = Open-NetGPO -PolicyStore <Name of the server GPO>  
>     Set-NetIPsecRule -DisplayName <Name of the IPsec policy> -GPOSession $gposession -Profile Any  
>     Save-NetGPO -GPOSession $gposession  
>     ```  
>   
>     Les noms des stratégies IPsec sont DirectAccess-DaServerToInfra et DirectAccess-DaServerToCorp.  
  
## <a name="ConfigRouting"></a>Configurer le routage dans le réseau d’entreprise  
Procédez comme suit pour configurer le routage dans le réseau d'entreprise :  
  
-   Lorsqu'IPv6 natif est déployé dans l'organisation, ajoutez un itinéraire afin que les routeurs du réseau interne redirigent le trafic IPv6 via le serveur d'accès à distance.  
  
-   Configurez manuellement les itinéraires IPv4 et IPv6 de l'organisation sur les serveurs d'accès à distance. Ajoutez un itinéraire publié afin que tout le trafic ayant un préfixe IPv6 (/48) d'organisation soit transféré au réseau interne. De plus, pour le trafic IPv4, ajoutez des itinéraires explicites afin que le trafic IPv4 soit transféré au réseau interne.  
  
## <a name="ConfigFirewalls"></a>Configurer des pare-feu  
Lorsque vous utilisez des pare-feu supplémentaires dans votre déploiement, appliquez les exceptions de pare-feu côté Internet suivantes pour le trafic d’accès à distance lorsque le serveur d’accès à distance est sur le réseau Internet IPv4 :  
  
-   trafic 6to4 : protocole IP 41 entrant et sortant.  
  
-   IP-HTTPs-port TCP (Transmission Control Protocol) 443 et port TCP source 443 sortant. Lorsque le serveur d’accès à distance dispose d’une seule carte réseau et que le serveur Emplacement réseau se trouve sur le serveur d’accès à distance, le port TCP 62000 est également requis.  
  
    > [!NOTE]  
    > Cette exemption doit être configurée sur le serveur d’accès à distance. Toutes les autres exemptions doivent être configurées sur le pare-feu de périmètre.  
  
> [!NOTE]  
> Pour les trafics Teredo et 6to4, ces exceptions doivent être appliquées pour les deux adresses IPv4 publiques consécutives côté Internet sur le serveur d’accès à distance. Pour IP-HTTPS, les exceptions ne doivent être appliquées qu’à l’adresse de résolution du nom externe du serveur.  
  
Lorsque vous utilisez des pare-feu supplémentaires, appliquez les exceptions de pare-feu côté Internet suivantes pour le trafic d’accès à distance lorsque le serveur d’accès à distance est sur le réseau Internet IPv6 :  
  
-   Protocole IP 50  
  
-   Port UDP de destination 500 entrant et port UDP source 500 sortant.  
  
Lorsque vous utilisez des pare-feu supplémentaires, appliquez les exceptions de pare-feu de réseau interne suivantes pour le trafic d’accès à distance :  
  
-   ISATAP-Protocole 41 entrant et sortant  
  
-   TCP/UDP pour tout le trafic IPv4/IPv6  
  
## <a name="ConfigDNS"></a>Configurer le serveur DNS  
Vous devez configurer manuellement une entrée DNS pour le site web du serveur Emplacement réseau du réseau interne de votre déploiement.  
  
### <a name="NLS_DNS"></a>Pour créer le serveur d’emplacement réseau et les enregistrements DNS de sonde NCSI  
  
1.  Sur le serveur DNS du réseau interne, exécutez **dnsmgmt. msc** , puis appuyez sur entrée.  
  
2.  Dans le volet gauche de la console **Gestionnaire DNS**, développez la zone de recherche directe de votre domaine. Cliquez avec le bouton droit sur le domaine, puis cliquez sur **Nouvel hôte (A ou AAAA)** .  
  
3.  Dans la boîte de dialogue **Nouvel hôte**, dans la zone **Nom (utilise le domaine parent si ce champ est vide)** , entrez le nom DNS du site web du serveur d'emplacement réseau (il s'agit du nom que les clients DirectAccess utilisent pour se connecter au serveur d'emplacement réseau). Dans la zone **Adresse IP**, entrez l'adresse IPv4 du serveur d'emplacement réseau, puis cliquez sur **Ajouter un hôte**. Dans la boîte de dialogue **DNS**, cliquez sur **OK**.  
  
4.  Dans la boîte de dialogue **Nouvel hôte**, dans la zone **Nom (utilise le domaine parent si ce champ est vide)** , entrez le nom DNS de la sonde web. (Le nom de la sonde web par défaut est directaccess-webprobehost.) Dans la zone **Adresse IP**, entrez l'adresse IPv4 de la sonde web, puis cliquez sur **Ajouter un hôte**. Répétez ce processus pour directaccess-corpconnectivityhost et tout vérificateur de connectivité créé manuellement. Dans la boîte de dialogue **DNS**, cliquez sur **OK**.  
  
5.  Cliquez sur **Terminé**.  
  
![les commandes Windows PowerShell](../../../media/Step-1-Configure-the-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)***<em>équivalentes</em> Windows PowerShell***  

L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
Add-DnsServerResourceRecordA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv4Address <network_location_server_IPv4_address>  
Add-DnsServerResourceRecordAAAA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv6Address <network_location_server_IPv6_address>  
```  
  
Vous devez également configurer les entrées DNS pour les éléments suivants :  
  
-   **Le serveur IP-HTTPS** -les clients DirectAccess doivent être en mesure de résoudre le nom DNS du serveur d’accès à distance à partir d’Internet.  
  
-   **Vérification de la révocation** des certificats : DirectAccess utilise la vérification de la révocation des certificats pour la connexion IP-HTTPS entre les clients DirectAccess et le serveur d’accès à distance, ainsi que pour la connexion HTTPS entre le client DirectAccess et le serveur d’emplacement réseau. Dans les deux cas, les clients DirectAccess doivent être en mesure de résoudre le point de distribution de liste de révocation de certificats et d'y accéder.  
  
## <a name="ConfigAD"></a>Configurer Active Directory  
Le serveur d'accès à distance et tous les ordinateurs clients DirectAccess doivent être joints à un domaine Active Directory. Les ordinateurs clients DirectAccess doivent être membres de l'un des types de domaines suivants :  
  
-   les domaines qui appartiennent à la même forêt que le serveur d’accès à distance ;  
  
-   les domaines qui appartiennent à des forêts dotées d'une relation d'approbation bidirectionnelle avec la forêt du serveur d'accès à distance ;  
  
-   les domaines dotés d'une approbation bidirectionnelle avec le domaine du serveur d'accès à distance.  
  
#### <a name="to-join-the-remote-access-server-to-a-domain"></a>Pour joindre le serveur d’accès à distance à un domaine  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **Serveur local**. Dans le volet d’informations, cliquez sur le lien en regard de **Nom de l’ordinateur**.  
  
2.  Dans la boîte de dialogue **Propriétés système** , cliquez sur l’onglet nom de l' **ordinateur** . Sous l’onglet nom de l' **ordinateur** , cliquez sur **modifier**.  
  
3.  Dans **Nom de l’ordinateur**, tapez le nom de l’ordinateur si vous modifiez également le nom de l’ordinateur lors de la jonction du serveur au domaine. Sous **Membre de**, cliquez sur **Domaine**, et tapez le nom du domaine auquel vous voulez joindre le serveur, par exemple, corp.contoso.com, puis cliquez sur **OK**.  
  
4.  Lorsque vous êtes invités à entrer un nom d’utilisateur et un mot de passe, tapez le nom d’utilisateur et le mot de passe d’un utilisateur qui dispose des droits pour joindre des ordinateurs au domaine, puis cliquez sur **OK**.  
  
5.  Lorsqu’une boîte de dialogue vous souhaitant la bienvenue dans le domaine s’affiche, cliquez sur **OK**.  
  
6.  Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **OK**.  
  
7.  Dans la boîte de dialogue **Propriétés système**, cliquez sur **Fermer**.  
  
8.  Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **Redémarrer maintenant**.  
  
#### <a name="to-join-client-computers-to-the-domain"></a>Pour joindre des ordinateurs clients au domaine  
  
1.  Exécutez **Explorer. exe**.  
  
2.  Cliquez avec le bouton droit sur l'icône Ordinateur, puis cliquez sur **Propriétés**.  
  
3.  Dans la page **Système**, cliquez sur **Paramètres système avancés**.  
  
4.  Dans la boîte de dialogue **Propriétés système**, sous l'onglet **Nom de l'ordinateur**, cliquez sur **Modifier**.  
  
5.  Dans **Nom de l'ordinateur**, tapez le nom de l'ordinateur si vous modifiez également le nom de l'ordinateur lors de la jonction du serveur au domaine. Sous **Membre de**, cliquez sur **Domaine**, et tapez le nom du domaine auquel vous voulez joindre le serveur, par exemple, corp.contoso.com, puis cliquez sur **OK**.  
  
6.  Lorsque vous êtes invités à entrer un nom d’utilisateur et un mot de passe, tapez le nom d’utilisateur et le mot de passe d’un utilisateur qui dispose des droits pour joindre des ordinateurs au domaine, puis cliquez sur **OK**.  
  
7.  Lorsqu’une boîte de dialogue vous souhaitant la bienvenue dans le domaine s’affiche, cliquez sur **OK**.  
  
8.  Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **OK**.  
  
9. Dans la boîte de dialogue **Propriétés système**, cliquez sur Fermer. Lorsque vous y êtes invité, cliquez sur **Redémarrer maintenant**.  
  
![les commandes Windows PowerShell](../../../media/Step-1-Configure-the-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)***<em>équivalentes</em> Windows PowerShell***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
Notez que vous devez fournir des informations d’identification de domaine après avoir entré la commande Add-Computer ci-dessous.  
  
```  
Add-Computer -DomainName <domain_name>  
Restart-Computer  
```  
  
## <a name="ConfigGPOs"></a>Configurer des objets de stratégie de groupe  
Pour déployer l’accès à distance, vous avez besoin d’un minimum de deux objets de stratégie de groupe : un objet de stratégie de groupe contient des paramètres pour le serveur d’accès à distance et un autre contient des paramètres pour les ordinateurs clients DirectAccess. Quand vous configurez l’accès à distance, l’Assistant crée automatiquement l’objet de stratégie de groupe requis. Toutefois, si votre organisation impose une convention d’affectation de noms, ou si vous ne disposez pas des autorisations requises pour créer ou modifier des objets de stratégie de groupe, ceux-ci doivent être créés avant la configuration de l’accès à distance.  
  
Pour créer un objet de stratégie de groupe, consultez [créer et modifier un objet stratégie de groupe](https://technet.microsoft.com/library/cc754740.aspx).  
  
> [!IMPORTANT]  
> L’administrateur peut lier manuellement l’objet de stratégie de groupe DirectAccess à une unité d’organisation en procédant comme suit :  
>   
> 1.  Avant de configurer DirectAccess, liez les objets de stratégie de groupe créés aux unités d'organisation respectives.  
> 2.  Configurez DirectAccess, en spécifiant un groupe de sécurité pour les ordinateurs clients.  
> 3.  L’administrateur peut, ou non, disposer des autorisations requises pour lier les objets de stratégie de groupe au domaine. Dans l’un ou l’autre cas, les objets de stratégie de groupe seront automatiquement configurés. Si les objets de stratégie de groupe sont déjà liés à une unité d’organisation, les liens ne seront pas supprimés. Les objets de stratégie de groupe ne seront pas liés au domaine. Pour un objet de stratégie de groupe serveur, l’unité d’organisation doit contenir l’objet ordinateur serveur, ou l’objet de stratégie de groupe sera lié à la racine du domaine.  
> 4.  Si la liaison à l’unité d’organisation n’a pas été effectuée avant l’exécution de l’Assistant DirectAccess, l’administrateur peut, une fois la configuration terminée, lier les objets de stratégie de groupe DirectAccess aux unités d’organisation requises. Il est possible de supprimer le lien au domaine. Les étapes de liaison d’un objet de stratégie de groupe à une unité d’organisation sont disponibles [ici](https://technet.microsoft.com/library/cc732979.aspx)  
  
> [!NOTE]  
> Si un objet de stratégie de groupe a été créé manuellement, il est possible, au cours de la configuration de DirectAccess, que l’objet de stratégie de groupe ne soit pas disponible. L’objet de stratégie de groupe n’a peut-être pas été répliqué sur le contrôleur de domaine le plus proche de l’ordinateur de gestion. Dans ce cas, l'administrateur peut attendre la fin de la réplication ou forcer la réplication.  
  
## <a name="ConfigSGs"></a>Configurer des groupes de sécurité  
Les paramètres DirectAccess contenus dans les objets de stratégie de groupe de l’ordinateur client sont appliqués uniquement aux ordinateurs qui sont membres des groupes de sécurité que vous spécifiez lors de la configuration de l’accès à distance.  
  
### <a name="Sec_Group"></a>Pour créer un groupe de sécurité pour les clients DirectAccess  
  
1.  Exécutez **DSA. msc**. Dans la console **Utilisateurs et ordinateurs Active Directory**, dans le volet gauche, développez le domaine contenant le groupe de sécurité, cliquez avec le bouton droit sur **Utilisateurs**, pointez sur **Nouveau**, puis cliquez sur **Groupe**.  
  
2.  Dans la boîte de dialogue **Nouvel objet - Groupe**, sous **Nom du groupe**, entrez le nom du groupe de sécurité.  
  
3.  Sous **Étendue du groupe**, cliquez sur **Globale**. Sous **Type de groupe**, cliquez sur **Sécurité**, puis cliquez sur **OK**.  
  
4.  Double-cliquez sur le groupe de sécurité des ordinateurs clients DirectAccess, puis dans la boîte de dialogue des propriétés, cliquez sur l'onglet **Membres**.  
  
5.  Sous l'onglet **Membres** , cliquez sur **Ajouter**.  
  
6.  Dans la boîte de dialogue **Sélectionner Utilisateurs, contacts, ordinateurs ou comptes de service**, sélectionnez les ordinateurs clients que vous voulez activer pour DirectAccess, puis cliquez sur **OK**.  
  
![les commandes Windows PowerShell](../../../media/Step-1-Configure-the-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)**équivalentes** Windows PowerShell  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
New-ADGroup -GroupScope global -Name <DirectAccess_clients_group_name>  
Add-ADGroupMember -Identity DirectAccess_clients_group_name -Members <computer_name>  
```  
  
## <a name="BKMK_Links"></a>Étape suivante  
  
-   [Étape 2 : configurer le serveur DirectAccess de base](da-basic-configure-s2-server.md)  
  


