---
title: Étape 1 configurer l’infrastructure DirectAccess
description: Cette rubrique fait partie du guide ajouter DirectAccess à un déploiement d’accès à distance (VPN) existant pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5dc529f7-7bc3-48dd-b83d-92a09e4055c4
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4437101c6cde25ebb370fe54a2f8ef821997f15d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388771"
---
# <a name="step-1-configure-the-directaccess-infrastructure"></a>Étape 1 configurer l’infrastructure DirectAccess

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Cette rubrique décrit comment configurer l'infrastructure requise pour activer DirectAccess dans un déploiement VPN existant. Avant de commencer les étapes de déploiement, assurez-vous que vous avez effectué les étapes de planification décrites dans [Step 1 : Planifier l’infrastructure DirectAccess @ no__t-0.  
  
|Tâche|Description|  
|----|--------|  
|Configurer les paramètres réseau serveur|Configurez les paramètres réseau serveur sur le serveur d'accès à distance.|  
|Configurer le routage dans le réseau d'entreprise|Configurez le routage dans le réseau d'entreprise pour garantir que le trafic est correctement acheminé.|  
|Configurer les pare-feu|Configurer des pare-feu supplémentaires, si nécessaire.|  
|Configurer les autorités de certification et les certificats|L'Assistant Activation de DirectAccess configure un proxy Kerberos intégré qui s'authentifie à l'aide des noms d'utilisateur et mots de passe. Il configure également un certificat IP-HTTPS sur le serveur d'accès à distance.|  
|Configurer le serveur DNS|Configurez les paramètres DNS pour le serveur d'accès à distance.|  
|Configurer Active Directory|Joignez les ordinateurs clients au domaine Active Directory.|  
|Configurer les objets de stratégie de groupe|Configurez les objets de stratégie de groupe pour le déploiement, le cas échéant.|  
|Configurer les groupes de sécurité|Configurez les groupes de sécurité qui contiendront les ordinateurs clients DirectAccess, ainsi que tous les autres groupes de sécurité requis dans le déploiement.|  
|Configurer le serveur Emplacement réseau|L'Assistant Activation de DirectAccess configure le serveur d'emplacement réseau sur le serveur DirectAccess.|  
  
## <a name="ConfigNetworkSettings"></a>Configurer les paramètres réseau du serveur  
Les paramètres d'interface réseau suivants sont requis pour un déploiement à un seul serveur dans un environnement avec IPv4 et IPv6. Toutes les adresses IP sont configurées à l'aide de l'option **Modifier les paramètres de la carte** du **Centre Réseau et partage Windows**.  
  
-   Topologie de périmètre  
  
    -   Une adresse IPv4 ou IPv6 statique publique côté Internet.  
  
    -   Une seule adresse IPv4 ou IPv6 statique interne.  
  
-   Derrière un périphérique NAT (deux cartes réseau)  
  
    -   Une seule adresse IPv4 ou IPv6 statique interne côté réseau.  
  
-   Derrière un périphérique NAT (une carte réseau)  
  
    -   Une seule adresse IPv4 ou IPv6 statique.  
  
> [!NOTE]  
> Dans le cas où le serveur d'accès à distance dispose de deux cartes réseau (l'une classifiée dans le profil de domaine et l'autre dans un profil public/privé), mais où une seule topologie de carte réseau sera utilisée, la recommandation est la suivante :  
>   
> 1.  Vérifiez que la deuxième carte réseau est également classifiée dans le profil de domaine. Recommandé.  
> 2.  Si, pour une raison quelconque, il est impossible de configurer la deuxième carte réseau pour le profil de domaine, vous devez définir manuellement l'étendue de la stratégie IPsec DirectAccess à tous les profils à l'aide des commandes Windows PowerShell suivantes :  
>   
>     ```  
>     $gposession = Open-NetGPO -PolicyStore <Name of the server GPO>  
>     Set-NetIPsecRule -DisplayName <Name of the IPsec policy> -GPOSession $gposession -Profile Any  
>     Save-NetGPO -GPOSession $gposession  
>     ```  
  
## <a name="ConfigRouting"></a>Configurer le routage dans le réseau d’entreprise  
Procédez comme suit pour configurer le routage dans le réseau d'entreprise :  
  
-   Lorsqu'IPv6 natif est déployé dans l'organisation, ajoutez un itinéraire afin que les routeurs du réseau interne redirigent le trafic IPv6 via le serveur d'accès à distance.  
  
-   Configurez manuellement les itinéraires IPv4 et IPv6 de l'organisation sur les serveurs d'accès à distance. Ajoutez un itinéraire publié afin que tout le trafic ayant un préfixe IPv6 (/48) d'organisation soit transféré au réseau interne. De plus, pour le trafic IPv4, ajoutez des itinéraires explicites afin que le trafic IPv4 soit transféré au réseau interne.  
  
## <a name="ConfigFirewalls"></a>Configurer des pare-feu  
Lorsque vous utilisez des pare-feu supplémentaires dans votre déploiement, appliquez les exceptions de pare-feu côté Internet suivantes pour le trafic d’accès à distance lorsque le serveur d’accès à distance est sur le réseau Internet IPv4 :  
  
-   trafic 6to4 : protocole IP 41 entrant et sortant.  
  
-   IP-HTTPs-port TCP (Transmission Control Protocol) 443 et port TCP source 443 sortant. Lorsque le serveur d’accès à distance dispose d’une seule carte réseau et que le serveur Emplacement réseau se trouve sur le serveur d’accès à distance, le port TCP 62000 est également requis.  
  
Lorsque vous utilisez des pare-feu supplémentaires, appliquez les exceptions de pare-feu côté Internet suivantes pour le trafic d’accès à distance lorsque le serveur d’accès à distance est sur le réseau Internet IPv6 :  
  
-   Protocole IP 50  
  
-   Port UDP de destination 500 entrant et port UDP source 500 sortant.  
  
Lorsque vous utilisez des pare-feu supplémentaires, appliquez les exceptions de pare-feu de réseau interne suivantes pour le trafic d’accès à distance :  
  
-   ISATAP-Protocole 41 entrant et sortant  
  
-   TCP/UDP pour tout le trafic IPv4/IPv6  
  
## <a name="ConfigCAs"></a>Configurer les autorités de certification et les certificats  
L'Assistant Activation de DirectAccess configure un proxy Kerberos intégré qui s'authentifie à l'aide des noms d'utilisateur et mots de passe. Il configure également un certificat IP-HTTPS sur le serveur d'accès à distance.  
  
### <a name="ConfigCertTemp"></a>Configurer des modèles de certificats  
Lorsque vous utilisez une autorité de certification interne pour émettre des certificats, vous devez configurer un modèle de certificat pour le certificat IP-HTTPS et le certificat de site web du serveur Emplacement réseau.  
  
##### <a name="to-configure-a-certificate-template"></a>Pour configurer un modèle de certificat  
  
1.  Sur l’autorité de certification interne, créez un modèle de certificat comme décrit dans [Création de modèles de certificat](https://technet.microsoft.com/library/cc731705.aspx).  
  
2.  Déployez le modèle de certificat comme décrit dans [Déploiement de modèles de certificat](https://technet.microsoft.com/library/cc770794.aspx).  
  
### <a name="configure-the-ip-https-certificate"></a>Configurer le certificat IP-HTTPS.  
L'accès à distance requiert un certificat IP-HTTPS pour authentifier les connexions IP-HTTPS auprès du serveur d'accès à distance. Il existe trois options de certificat pour le certificat IP-HTTPS :  
  
-   **Public**-fourni par un tiers.  
  
    Un certificat utilisé pour l'authentification IP-HTTPS. Si le nom de sujet du certificat n'est pas composé de caractères génériques, il doit être l'URL de nom de domaine complet (FQDN) pouvant être résolue en externe qui est utilisée uniquement pour les connexions IP-HTTPS du serveur d'accès à distance.  
  
-   **Privé**: les éléments suivants sont requis, s’ils n’existent pas déjà :  
  
    -   Un certificat de site web utilisé pour l'authentification IP-HTTPS. Le sujet du certificat doit être un nom de domaine complet (FQDN) pouvant être résolu en externe et accessible à partir d'Internet.  
  
    -   Un point de distribution de liste de révocation de certificats accessible à partir d’un nom FQDN pouvant être résolu publiquement.  
  
-   **Auto-signé**-les éléments suivants sont requis, s’ils n’existent pas déjà :  
  
    > [!NOTE]  
    > Les certificats auto-signés ne sont pas utilisables dans des déploiements multisites.  
  
    -   Un certificat de site web utilisé pour l'authentification IP-HTTPS. Le sujet du certificat doit être un nom de domaine complet (FQDN) pouvant être résolu en externe et accessible à partir d'Internet.  
  
    -   Un point de distribution de liste de révocation de certificats accessible à partir d'un nom de domaine complet (FQDN) pouvant être résolu publiquement.  
  
Assurez-vous que le certificat de site web pour l'authentification IP-HTTPS est conforme aux exigences suivantes :  
  
-   Le nom commun du certificat doit correspondre au nom du site IP-HTTPS.  
  
-   Dans le champ Objet, spécifiez l'adresse IPv4 de la carte externe du serveur d'accès à distance ou le nom de domaine complet (FQDN) de l'URL IP-HTTPS.  
  
-   Pour le champ Utilisation améliorée de la clé, utilisez l’identificateur d’objet (OID) d’authentification du serveur.  
  
-   Pour le champ Points de distribution de la liste de révocation de certificats, indiquez un point de distribution de liste de révocation de certificats accessible par les clients DirectAccess connectés à Internet  
  
-   Le certificat IP-HTTPS doit avoir une clé privée.  
  
-   Le certificat IP-HTTPS doit être importé directement dans le magasin personnel.  
  
-   Les certificats IP-HTTPS peuvent utiliser des caractères génériques dans le nom.  
  
##### <a name="to-install-the-ip-https-certificate-from-an-internal-ca"></a>Pour installer le certificat IP-HTTPS à partir d'une autorité de certification interne  
  
1.  Sur le serveur d'accès à distance : Dans l’écran **Démarrer** , tapez**MMC. exe**, puis appuyez sur entrée.  
  
2.  Dans la console MMC, dans le menu **Fichier**, cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**.  
  
3.  Dans la boîte de dialogue **Ajouter ou supprimer des composants logiciels enfichables**, cliquez sur **Certificats**, sur **Ajouter**, sur **Le compte de l'ordinateur**, sur **Suivant**, sur **Ordinateur local**, sur **Terminer**, puis sur **OK**.  
  
4.  Dans l'arborescence de la console du composant logiciel enfichable Certificats, ouvrez **Certificats (ordinateur local)\Personnel\Certificats**.  
  
5.  Cliquez avec le bouton droit sur **Certificats**, pointez sur **Toutes les Tâches**, puis cliquez sur **Demander un nouveau certificat**.  
  
6.  Cliquez sur **Suivant** deux fois.  
  
7.  Dans la page **demander des certificats** , activez la case à cocher du modèle de certificat et, si nécessaire, cliquez sur des **informations supplémentaires sont requises pour s’inscrire pour ce certificat**.  
  
8.  Dans la boîte de dialogue **Propriétés du certificat**, sous l'onglet **Sujet**, dans la zone **Nom du sujet**, dans **Type**, sélectionnez **Nom commun**.  
  
9. Dans le champ **Valeur**, spécifiez l'adresse IPv4 de la carte externe du serveur d'accès à distance ou le nom de domaine complet (FQDN) de l'URL IP-HTTPS, puis cliquez sur **Ajouter**.  
  
10. Dans la zone **Autre nom**, dans **Type**, sélectionnez **DNS**.  
  
11. Dans le champ **Valeur**, spécifiez l'adresse IPv4 de la carte externe du serveur d'accès à distance ou le nom de domaine complet (FQDN) de l'URL IP-HTTPS, puis cliquez sur **Ajouter**.  
  
12. Sous l'onglet **Général**, dans **Nom convivial**, vous pouvez entrer un nom qui vous permettra d'identifier le certificat.  
  
13. Sous l'onglet **Extensions**, en regard de l'option **Utilisation améliorée de la clé**, cliquez sur la flèche, puis assurez-vous qu'Authentification du serveur figure dans la liste **Expressions sélectionnées**.  
  
14. Cliquez sur **OK**, sur **Inscrire**, puis sur **Terminer**.  
  
15. Dans le volet d'informations du composant logiciel enfichable Certificats, vérifiez qu'un nouveau certificat a été inscrit avec Rôles prévus égal à Authentification du serveur.  
  
## <a name="ConfigDNS"></a>Configurer le serveur DNS  
Vous devez configurer manuellement une entrée DNS pour le site web du serveur Emplacement réseau du réseau interne de votre déploiement.  
  
### <a name="NLS_DNS"></a>Pour créer le serveur d’emplacement réseau et les enregistrements DNS de sonde Web  
  
1.  Sur le serveur DNS du réseau interne : Dans l’écran d' **Accueil** , tapez * * dnsmgmt. msc * *, puis appuyez sur entrée.  
  
2.  Dans le volet gauche de la console **Gestionnaire DNS**, développez la zone de recherche directe de votre domaine. Cliquez avec le bouton droit sur le domaine, puis cliquez sur **Nouvel hôte (A ou AAAA)** .  
  
3.  Dans la boîte de dialogue **Nouvel hôte**, dans la zone **Nom (utilise le domaine parent si ce champ est vide)** , entrez le nom DNS du site web du serveur d'emplacement réseau (il s'agit du nom que les clients DirectAccess utilisent pour se connecter au serveur d'emplacement réseau). Dans la zone **Adresse IP**, entrez l'adresse IPv4 du serveur d'emplacement réseau, puis cliquez sur **Ajouter un hôte**. Dans la boîte de dialogue **DNS**, cliquez sur **OK**.  
  
4.  Dans la boîte de dialogue **Nouvel hôte**, dans la zone **Nom (utilise le domaine parent si ce champ est vide)** , entrez le nom DNS de la sonde web. (Le nom de la sonde web par défaut est directaccess-webprobehost.) Dans la zone **Adresse IP**, entrez l'adresse IPv4 de la sonde web, puis cliquez sur **Ajouter un hôte**. Répétez ce processus pour directaccess-corpconnectivityhost et tout vérificateur de connectivité créé manuellement. Dans la boîte de dialogue **DNS**, cliquez sur **OK**.  
  
5.  Cliquez sur **Terminé**.  

](../../../media/Step-1-Configure-the-DirectAccess-Infrastructure_3/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em> @no__t 0Windows PowerShell***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
Add-DnsServerResourceRecordA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv4Address <network_location_server_IPv4_address>  
Add-DnsServerResourceRecordAAAA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv6Address <network_location_server_IPv6_address>  
```  
  
Vous devez également configurer les entrées DNS pour les éléments suivants :  
  
-   **Le serveur IP-HTTPS**-les clients DirectAccess doivent être en mesure de résoudre le nom DNS du serveur d’accès à distance à partir d’Internet.  
  
-   **Vérification de la révocation de la liste de**révocation de certificats : DirectAccess utilise la vérification de la révocation des certificats pour la connexion IP-HTTPS entre les clients DirectAccess et le serveur d’accès à distance, ainsi que pour la connexion HTTPS entre le client DirectAccess et le réseau serveur d’emplacement. Dans les deux cas, les clients DirectAccess doivent être en mesure de résoudre le point de distribution de liste de révocation de certificats et d'y accéder.  
  
## <a name="ConfigAD"></a>Configurer Active Directory  
Le serveur d'accès à distance et tous les ordinateurs clients DirectAccess doivent être joints à un domaine Active Directory. Les ordinateurs clients DirectAccess doivent être membres de l'un des types de domaines suivants :  
  
-   les domaines qui appartiennent à la même forêt que le serveur d'accès à distance ;  
  
-   les domaines qui appartiennent à des forêts dotées d'une relation d'approbation bidirectionnelle avec la forêt du serveur d'accès à distance ;  
  
-   les domaines dotés d'une approbation bidirectionnelle avec le domaine du serveur d'accès à distance.  
  
#### <a name="to-join-client-computers-to-the-domain"></a>Pour joindre des ordinateurs clients au domaine  
  
1.  Dans l’écran d' **Accueil** , tapez **Explorer. exe**, puis appuyez sur entrée.  
  
2.  Cliquez avec le bouton droit sur l'icône Ordinateur, puis cliquez sur **Propriétés**.  
  
3.  Dans la page **Système**, cliquez sur **Paramètres système avancés**.  
  
4.  Dans la boîte de dialogue **Propriétés système**, sous l'onglet **Nom de l'ordinateur**, cliquez sur **Modifier**.  
  
5.  Dans **Nom de l'ordinateur**, tapez le nom de l'ordinateur si vous modifiez également le nom de l'ordinateur lors de la jonction du serveur au domaine. Sous **Membre de**, cliquez sur **Domaine**, et tapez le nom du domaine auquel vous voulez joindre le serveur, par exemple, corp.contoso.com, puis cliquez sur **OK**.  
  
6.  Lorsque vous êtes invités à entrer un nom d’utilisateur et un mot de passe, tapez le nom d’utilisateur et le mot de passe d’un utilisateur qui dispose des droits pour joindre des ordinateurs au domaine, puis cliquez sur **OK**.  
  
7.  Lorsqu’une boîte de dialogue vous souhaitant la bienvenue dans le domaine s’affiche, cliquez sur **OK**.  
  
8.  Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **OK**.  
  
9. Dans la boîte de dialogue **Propriétés système**, cliquez sur Fermer. Lorsque vous y êtes invité, cliquez sur **Redémarrer maintenant**.  
  
](../../../media/Step-1-Configure-the-DirectAccess-Infrastructure_3/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em> @no__t 0Windows PowerShell***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
Notez que vous devez fournir des informations d’identification de domaine après avoir entré la commande Add-Computer ci-dessous.  
  
```  
Add-Computer -DomainName <domain_name>  
Restart-Computer  
```  
  
## <a name="ConfigGPOs"></a>Configurer des objets de stratégie de groupe  
Pour déployer l’accès à distance, vous avez besoin d’un minimum de deux objets stratégie de groupe : un objet stratégie de groupe contient des paramètres pour le serveur d’accès à distance et un autre contient des paramètres pour les ordinateurs clients DirectAccess. Quand vous configurez l’accès à distance, l’Assistant crée automatiquement les objets de stratégie de groupe requis. Toutefois, si votre organisation impose une convention d’affectation de noms, ou si vous ne disposez pas des autorisations nécessaires pour créer ou modifier des objets stratégie de groupe, vous devez les créer avant de configurer l’accès à distance.  
  
Pour créer des objets stratégie de groupe, consultez [créer et modifier un objet stratégie de groupe](https://technet.microsoft.com/library/cc754740.aspx).  
  
> [!IMPORTANT]  
> L’administrateur peut lier manuellement les objets de stratégie de groupe DirectAccess à une unité d’organisation en procédant comme suit :  
>   
> 1.  Avant de configurer DirectAccess, liez les objets de stratégie de groupe créés aux unités d'organisation respectives.  
> 2.  Configurez DirectAccess, en spécifiant un groupe de sécurité pour les ordinateurs clients.  
> 3.  L’administrateur de l’accès à distance peut être autorisé ou non à lier les objets stratégie de groupe au domaine. Dans l’un ou l’autre cas, les objets de stratégie de groupe seront automatiquement configurés. Si les objets de stratégie de groupe sont déjà liés à une unité d'organisation, les liens ne seront pas supprimés, et les objets de stratégie de groupe ne seront pas liés au domaine. Pour un objet de stratégie de groupe serveur, l'unité d'organisation doit contenir l'objet ordinateur serveur, ou l'objet de stratégie de groupe sera lié à la racine du domaine.  
> 4.  Si la liaison à l’unité d’organisation n’a pas été effectuée avant l’exécution de l’Assistant DirectAccess, une fois la configuration terminée, l’administrateur de domaine peut lier les objets de stratégie de groupe DirectAccess aux unités d’organisation requises. Il est possible de supprimer le lien au domaine. Vous trouverez les étapes de liaison d’un objet stratégie de groupe à une unité d’organisation [ici](https://technet.microsoft.com/library/cc732979.aspx).  
  
> [!NOTE]  
> Si un objet stratégie de groupe a été créé manuellement, il est possible, au cours de la configuration de DirectAccess, que l’objet stratégie de groupe ne soit pas disponible. L’objet stratégie de groupe n’a peut-être pas été répliqué sur le contrôleur de domaine le plus proche sur l’ordinateur de gestion. Dans ce cas, l'administrateur peut attendre la fin de la réplication ou forcer la réplication.  
  
## <a name="ConfigSGs"></a>Configurer des groupes de sécurité  
Les paramètres DirectAccess contenus dans l’objet stratégie de groupe de l’ordinateur client sont appliqués uniquement aux ordinateurs qui sont membres des groupes de sécurité que vous spécifiez lors de la configuration de l’accès à distance. De plus, si vous utilisez des groupes de sécurité pour gérer vos serveurs d'applications, créez un groupe de sécurité pour ces serveurs.  
  
### <a name="Sec_Group"></a>Pour créer un groupe de sécurité pour les clients DirectAccess  
  
1.  Dans l’écran d' **Accueil** , tapez**DSA. msc**, puis appuyez sur entrée. Dans la console **Utilisateurs et ordinateurs Active Directory**, dans le volet gauche, développez le domaine contenant le groupe de sécurité, cliquez avec le bouton droit sur **Utilisateurs**, pointez sur **Nouveau**, puis cliquez sur **Groupe**.  
  
2.  Dans la boîte de dialogue **Nouvel objet - Groupe**, sous **Nom du groupe**, entrez le nom du groupe de sécurité.  
  
3.  Sous **Étendue du groupe**, cliquez sur **Globale**. Sous **Type de groupe**, cliquez sur **Sécurité**, puis cliquez sur **OK**.  
  
4.  Double-cliquez sur le groupe de sécurité des ordinateurs clients DirectAccess, puis dans la boîte de dialogue des propriétés, cliquez sur l'onglet **Membres**.  
  
5.  Sous l'onglet **Membres** , cliquez sur **Ajouter**.  
  
6.  Dans la boîte de dialogue **Sélectionner Utilisateurs, contacts, ordinateurs ou comptes de service**, sélectionnez les ordinateurs clients que vous voulez activer pour DirectAccess, puis cliquez sur **OK**.  
  
](../../../media/Step-1-Configure-the-DirectAccess-Infrastructure_3/PowerShellLogoSmall.gif)**commandes Windows PowerShell équivalentes** @no__t 0Windows PowerShell  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
New-ADGroup -GroupScope global -Name <DirectAccess_clients_group_name>  
Add-ADGroupMember -Identity DirectAccess_clients_group_name -Members <computer_name>  
```  
  
## <a name="ConfigNLS"></a>Configurer le serveur emplacement réseau  
Le serveur d'emplacement réseau doit être sur un serveur avec un haut niveau de disponibilité, et un certificat SSL valide doit être approuvé par les clients DirectAccess. Il existe deux options de certificat pour le certificat de serveur d'emplacement réseau :  
  
-   **Privé**: les éléments suivants sont requis, s’ils n’existent pas déjà :  
  
    -   un certificat de site web utilisé pour le serveur d'emplacement réseau. Le sujet de certificat doit être l'URL du serveur Emplacement réseau.   
  
    -   un point de distribution de liste de révocation de certificats à haut niveau de disponibilité à partir du réseau interne.  
  
-   **Auto-signé**-les éléments suivants sont requis, s’ils n’existent pas déjà :  
  
    > [!NOTE]  
    > Les certificats auto-signés ne sont pas utilisables dans des déploiements multisites.  
  
    -   un certificat de site web utilisé pour le serveur d'emplacement réseau. Le sujet de certificat doit être l'URL du serveur Emplacement réseau.  
  
> [!NOTE]  
> Si le serveur d'emplacement réseau se trouve sur le serveur d'accès à distance, un site web lié au certificat de serveur que vous fournissez est automatiquement créé lors de la configuration de l'accès à distance.  
  
#### <a name="to-install-the-network-location-server-certificate-from-an-internal-ca"></a>Pour installer le certificat du serveur Emplacement réseau à partir d'une autorité de certification interne  
  
1.  Sur le serveur qui hébergera le site web du serveur Emplacement réseau : Dans l’écran **Démarrer** , tapez**MMC. exe**, puis appuyez sur entrée.  
  
2.  Dans la console MMC, dans le menu **Fichier**, cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**.  
  
3.  Dans la boîte de dialogue **Ajouter ou supprimer des composants logiciels enfichables**, cliquez sur **Certificats**, sur **Ajouter**, sur **Le compte de l'ordinateur**, sur **Suivant**, sur **Ordinateur local**, sur **Terminer**, puis sur **OK**.  
  
4.  Dans l'arborescence de la console du composant logiciel enfichable Certificats, ouvrez **Certificats (ordinateur local)\Personnel\Certificats**.  
  
5.  Cliquez avec le bouton droit sur **Certificats**, pointez sur **Toutes les Tâches**, puis cliquez sur **Demander un nouveau certificat**.  
  
6.  Cliquez sur **Suivant** deux fois.  
  
7.  Dans la page **demander des certificats** , activez la case à cocher du modèle de certificat et, si nécessaire, cliquez sur des **informations supplémentaires sont requises pour s’inscrire pour ce certificat**.  
  
8.  Dans la boîte de dialogue **Propriétés du certificat**, sous l'onglet **Sujet**, dans la zone **Nom du sujet**, dans **Type**, sélectionnez **Nom commun**.  
  
9. Dans le champ **Valeur**, entrez le nom de domaine complet (FQDN) du site web du serveur Emplacement réseau, puis cliquez sur **Ajouter**.  
  
10. Dans la zone **Autre nom**, dans **Type**, sélectionnez **DNS**.  
  
11. Dans le champ **Valeur**, entrez le nom de domaine complet (FQDN) du site web du serveur Emplacement réseau, puis cliquez sur **Ajouter**.  
  
12. Sous l'onglet **Général**, dans **Nom convivial**, vous pouvez entrer un nom qui vous permettra d'identifier le certificat.  
  
13. Cliquez sur **OK**, sur **Inscrire**, puis sur **Terminer**.  
  
14. Dans le volet d'informations du composant logiciel enfichable Certificats, vérifiez qu'un nouveau certificat a été inscrit avec Rôles prévus égal à Authentification du serveur.  
  

  


