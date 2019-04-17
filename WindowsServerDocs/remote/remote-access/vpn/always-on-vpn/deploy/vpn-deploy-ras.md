---
title: Configurer le serveur d'accès à distance pour VPN Toujours actif (AlwaysOn)
description: RRAS est conçu pour exécuter correctement comme un routeur et un serveur d’accès à distance; Par conséquent, il prend en charge un large éventail de fonctionnalités.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.reviewer: deverette
ms.openlocfilehash: a338ddfec1ed5cd0e9198f64dc4952eb591cdc1b
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067163"
---
# Étape3. Configurer le serveur d'accès à distance pour VPN Toujours actif (AlwaysOn)

>S’applique à: Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Précédente:** étape 2. Configurer l’Infrastructure de serveur](vpn-deploy-server-infrastructure.md)<br>
& #187;  [ **Précédente:** étape 4. Installer et configurer le serveur NPS (Network Policy)](vpn-deploy-nps.md)


RRAS est conçu pour effectuer bien comme un routeur et un serveur d’accès à distance, car il prend en charge un large éventail de fonctionnalités. Dans le cadre de ce déploiement, vous avez besoin uniquement un petit sous-ensemble de ces fonctionnalités: prise en charge des connexions VPN IKEv2 et le routage de réseau local.

IKEv2 est décrite dans Internet Engineering Task Force Request for 7296 de commentaires de protocole de tunnel VPN. Le principal avantage de IKEv2 est qu’il tolère d’interruption de la connexion réseau sous-jacente. Par exemple, si la connexion est temporairement perdue ou si un utilisateur déplace sur un ordinateur client à partir d’un réseau à l’autre, IKEv2 restaure automatiquement la connexion VPN lorsque la connexion réseau est rétablie, sans intervention de l’utilisateur.

Configurer le serveur RRAS pour prendre en charge les connexions de protocole IKEv2 lors de la désactivation des protocoles inutilisés, ce qui réduit l’encombrement de sécurité du serveur. En outre, configurez le serveur pour affecter des adresses aux clients VPN à partir d’un pool d’adresses statique. Vous pouvez facilement attribuer des adresses à partir d’un pool ou un serveur DHCP; Toutefois, à l’aide d’un serveur DHCP ajoute de la complexité de la conception et offre un minimum.


>[!Important]
>Il est important de:
>- Installer deux cartes de réseau Ethernet dans le serveur physique. Si vous installez le serveur VPN sur un ordinateur virtuel, vous devez créer deux commutateurs virtuels externes, une pour chaque carte réseau physique; puis créez deux cartes réseau virtuel pour la machine virtuelle, avec chaque carte réseau connecté à un seul commutateur virtuel.
>
>- Installer le serveur sur votre réseau de périmètre entre votre edge et le pare-feu interne, avec une carte réseau connecté au réseau de périmètre externe et une carte réseau connecté au réseau de périmètre interne.


>[!Warning]
>Avant de commencer, veillez à activer IPv6 sur le serveur VPN. Sinon, une connexion ne peut pas être établie et affiche un message d’erreur.

## Installer l’accès distant comme serveur VPN passerelle RAS

Dans cette procédure, vous installez le rôle de l’accès à distance en tant que seul locataire RAS passerelle VPN serveur. Pour plus d’informations, voir [Accès àdistance](../../../Remote-Access.md).


### Installer le rôle d’accès à distance à l’aide de Windows PowerShell

1. Ouvrez Windows PowerShell en tant **qu’administrateur**.

2. Tapez la commande suivante et appuyez sur **entrée**:

   `Install-WindowsFeature DirectAccess-VPN -IncludeManagementTools`

   Une fois l’installation terminée, le message suivant s’affiche dans Windows PowerShell.
    
   | Réussite | Redémarrage nécessité | Code de sortie | Résultat de la fonctionnalité                             |
   |---------|----------------|-----------|--------------------------------------------|
   | Vrai    | Non             | Réussite   | Kit d’Administration de Gestionnaire des connexions RAS { |
   ---

### Installer le rôle d’accès à distance à l’aide du Gestionnaire de serveur

Vous pouvez utiliser la procédure suivante pour installer le rôle d’accès à distance à l’aide du Gestionnaire de serveur.

1.  Sur le serveur VPN, dans le Gestionnaire de serveur, cliquez sur **Gérer** et cliquez sur **Ajouter des rôles et fonctionnalités**. <p>L’Assistant Ajout de rôles et fonctionnalités s’ouvre.

2.  Sur la Before vous commencer page, cliquez sur**suivant**.

3.  Sur la page Select Installation Type, sélectionnez l’option **d’installation en fonction du rôle ou une fonctionnalité** et cliquez sur **suivant**.

4.  Sur la page de serveur de destination Select, sélectionnez l’option **Sélectionner un serveur à partir du pool de serveurs** .

5.  Sous le Pool de serveurs, sélectionnez l’ordinateur local et cliquez sur **suivant**.

6.  Dans la page de rôles sélectionner le serveur, dans les **rôles**, cliquez sur **L’accès à distance**, puis cliquez sur **suivant**.

7.  Dans la page Sélection de fonctionnalités, cliquez sur **suivant**.

8.  Sur la page accès à distance, cliquez sur **suivant**.

9.  Dans la page de service de rôle Select, dans **les services de rôle**, cliquez sur**DirectAccess et VPN (RAS)**.<p>La boîte de dialogue **Assistant Ajout de rôles et fonctionnalités** s’ouvre.

10. Sur l’ajout de rôles et de la boîte de dialogue fonctionnalités, cliquez sur **Ajouter des fonctionnalités** et cliquez sur **suivant**.

11. Sur la page de rôle de serveur Web (IIS), cliquez sur **suivant**.

12. Sur la page de services de rôle Select, cliquez sur **suivant**.

13. Sur la page de sélections Confirm installation, passez en revue vos choix, puis cliquez sur **installer**.

14. Lorsque l’installation est terminée, cliquez sur **Fermer**.

## Configurer l’accès à distance comme serveur VPN

Dans cette section, vous pouvez configurer l’accès distant VPN pour autoriser les connexions VPN IKEv2, refuser les connexions à partir d’autres protocoles VPN et affectez un pool d’adresses IP statiques pour l’émission d’adresses IP à la connexion des clients VPN autorisés.

1.  Sur le serveur VPN, dans le Gestionnaire de serveur, cliquez sur l’indicateur de **Notifications** .

2.  Dans le menu **tâches** , cliquez sur **Ouvrir l’Assistant**.<p>L’Assistant Configurer l’accès à distance s’ouvre. 

    >[!NOTE] 
    >L’Assistant Configurer l’accès à distance peut-être ouvrir derrière le Gestionnaire de serveur. Si vous estimez que l’Assistant est trop long pour ouvrir, déplacer ou réduisez le Gestionnaire de serveur pour savoir si l’Assistant est derrière elle. Si ce n’est pas le cas, attendez que l’Assistant initialiser.

3.  Cliquez sur **déployer VPN uniquement**.<p>Le routage et à distance accès Console MMC (Microsoft Management) s’ouvre.

4.  Cliquez sur le serveur VPN, puis cliquez sur **configurer et activer le routage et l’accès à distance**.<p>Le routage et l’Assistant de configuration de serveur accès à distance s’ouvre.

5.  Dans la page le routage et l’Assistant de configuration de serveur accès à distance, cliquez sur **suivant**.

6.  Dans la **Configuration**, cliquez sur **Configuration personnalisée**, puis cliquez sur **suivant**.

7.  Dans **Configuration personnalisée**, cliquez sur **l’accès VPN**, puis cliquez sur **suivant**.<p>La page fin le routage et l’Assistant de configuration de serveur accès à distance s’ouvre.

8.  Cliquez sur **Terminer** pour fermer l’Assistant, puis cliquez sur **OK** pour fermer la boîte de dialogue Routage et d’accès à distance.

9.  Cliquez sur **Démarrer le service** pour démarrer l’accès à distance.

10. Dans la console MMC accès à distance, cliquez sur le serveur VPN, puis cliquez sur **Propriétés**.

11. Dans les propriétés, cliquez sur l’onglet **sécurité** et faire:

    a. Cliquez sur le **fournisseur d’authentification** et **L’authentification RADIUS**.
    
    b. Cliquez sur **Configurer**.<p>La boîte de dialogue authentification RADIUS s’ouvre.
    
    c. Cliquez sur **Ajouter**.<p>La boîte de dialogue Ajouter un serveur RADIUS s’ouvre.
    
    d. Dans la zone **nom du serveur**, tapez le nom de domaine complet (FQDN) du serveur NPS sur votre réseau d’entreprise / d’entreprise.<p>Par exemple, si le nom NetBIOS de votre serveur NPS est NPS1 et votre nom de domaine est corp.contoso.com, tapez **NPS1.corp.contoso.com**.
    
    e. Dans le **secret partagé**, cliquez sur **Modifier**.<p>La boîte de dialogue Modifier le Secret s’ouvre.
    
    f. Dans la **nouvelle clé secrète**, tapez une chaîne de texte.
    
    g. **Confirmer le nouveau secret**, tapez la même chaîne de texte, puis cliquez sur **OK**.

    >[!IMPORTANT] 
    >Enregistrez cette chaîne de texte. Lorsque vous configurez le serveur NPS sur votre réseau d’entreprise / d’entreprise, vous allez ajouter ce serveur VPN en tant que Client RADIUS. Au cours de cette configuration, vous allez utiliser cet même secret partagé afin que le serveur NPS et les serveurs VPN peuvent communiquer.

12. **Ajouter un serveur RADIUS**, passez en revue les paramètres par défaut:

    - **Délai d’expiration**
    
    - **Étendue initiale**
    
    - **Port**

13. Si nécessaire, modifiez les valeurs pour correspondre à la configuration requise pour votre environnement, puis cliquez sur **OK**.<p>Un NAS est un périphérique qui fournit un certain niveau d’accès à un réseau plus grand. Un NAS à l’aide d’une infrastructure RADIUS est également un client RADIUS, envoyer des demandes de connexion et des messages de gestion des comptes à un serveur RADIUS pour l’authentification, l’autorisation et la prise en compte.

14. Passez en revue le paramètre de **fournisseur d’authentifications**:

    | Si vous souhaitez que le …  | Alors...             |
    |---------------------|-------------------|
    | Activité d’accès à distance ouvert une session sur le serveur d’accès à distance | Assurez-vous que la **Gestion des comptes Windows** est activée.      |
    | NPS pour effectuer des services de gestion des comptes pour le VPN   | Changer de **fournisseur de gestion des comptes** de **Gestion des comptes RADIUS** , puis configurez le serveur NPS en tant que le fournisseur de comptes. |
    ---

15. Cliquez sur l’onglet **IPv4** et faire:

    a. Cliquez sur le **pool d’adresses statique**.
    
    b. Cliquez sur **Ajouter** pour configurer un pool d’adresses IP.<p>Le pool d’adresses doit contenir des adresses à partir du réseau de périmètre interne. Ces adresses sont sur la connexion de réseau interne sur le serveur VPN, pas au réseau d’entreprise.
    
    c. Dans la zone **adresse IP de début**, tapez l’adresse IP de début de la plage que vous souhaitez attribuer à des clients VPN.
    
    d. Dans la zone **adresse IP de fin**, tapez l’adresse IP de fin dans la plage que vous souhaitez attribuer à des clients VPN, ou dans le **nombre d’adresses**, tapez le numéro de l’adresse que vous voulez rendre disponible. Si vous utilisez DHCP pour ce sous-réseau, assurez-vous que vous configurez une adresse d’exclusion correspondante sur vos serveurs DHCP.
    
    e. (Facultatif) Si vous utilisez le protocole DHCP, cliquez sur **la carte**et dans la liste des résultats, cliquez sur la carte Ethernet connectée à votre réseau de périmètre interne.

16. (Facultatif) *Si vous configurez l’accès conditionnel pour une connexion VPN*, dans la liste déroulante **certificat** , sous la **Liaison de certificat SSL**, sélectionnez l’authentification du serveur VPN.

17. (Facultatif) *Si vous configurez l’accès conditionnel pour une connexion VPN*, dans la console MMC NPS, développez **Stratégies Policies\\Network** et faire: 

    a. **Connexions au serveur d’accès à distance et de routage Microsoft** de droite-la stratégie réseau et sélectionnez **Propriétés**.
    
    b. Sélectionnez le **accorder l’accès. Accorder l’accès si la demande de connexion correspondant à cette stratégie** option.
    
    c. Sous Type de serveur d’accès réseau, sélectionnez **Le serveur d’accès à distance (VPN-accès à distance)** dans le menu déroulant.

3.  Dans le routage et l’accès à distance de MMC, cliquez sur **Ports** , puis cliquez sur **Propriétés**. <p>La boîte de dialogue de propriétés de Ports s’ouvre.

4.  Cliquez sur **Miniport WAN (SSTP)** et cliquez sur **configurer**. La configurer le périphérique - Miniport WAN (SSTP) s’ouvre.

    a. Désactivez les cases à cocher **connexions d’accès à distance (uniquement entrantes)** et les **connexions de routage à la demande (entrantes et sortantes)** .
    
    b. Cliquez sur **OK**.

5.  Cliquez sur **Miniport réseau étendu (L2TP)** et cliquez sur **configurer**. La configurer le périphérique - boîte de dialogue de Miniport réseau étendu (L2TP) s’ouvre.

    a. Dans le **nombre Maximum de ports**, tapez le nombre de ports pour faire correspondre le nombre maximal de connexions VPN simultanées que vous souhaitez prendre en charge.
    
    b. Cliquez sur **OK**.

6.  Cliquez sur **Miniport WAN (PPTP)** et cliquez sur **configurer**. La configurer le périphérique - ouvre la boîte de dialogue de Miniport WAN (PPTP).

    a. Dans le **nombre Maximum de ports**, tapez le nombre de ports pour faire correspondre le nombre maximal de connexions VPN simultanées que vous souhaitez prendre en charge.
    
    b. Cliquez sur **OK**.
    
7. Cliquez sur **Miniport réseau étendu (IKEv2)** et cliquez sur **configurer**. La configurer le périphérique - boîte de dialogue de Miniport réseau étendu (IKEv2) s’ouvre.

    a. Dans le **nombre Maximum de ports**, tapez le nombre de ports pour faire correspondre le nombre maximal de connexions VPN simultanées que vous souhaitez prendre en charge.
    
    b. Cliquez sur **OK**.

7.  Si vous y êtes invité, cliquez sur **Oui** pour confirmer le redémarrage du serveur, puis cliquez sur **Fermer** pour redémarrer le serveur.

## Étape suivante
[Étape 4. Installer et configurer le serveur NPS (Network Policy)](vpn-deploy-nps.md): dans cette étape, vous installez le serveur NPS (Network Policy Server) à l’aide de Windows PowerShell ou l’Assistant de fonctionnalités et un serveur gestionnaire ajouter des rôles. Vous configurez également NPS pour gérer l’authentification, l’autorisation et les droits de gestion des comptes de connexion toutes les requêtes qu’il reçoit à partir du serveur VPN.





---
