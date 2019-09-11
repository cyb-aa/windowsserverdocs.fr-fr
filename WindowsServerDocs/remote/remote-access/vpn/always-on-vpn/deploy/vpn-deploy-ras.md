---
title: Configurer le serveur d'accès à distance pour VPN Toujours actif (AlwaysOn)
description: RRAS est conçu pour fonctionner correctement à la fois comme un routeur et un serveur d’accès à distance. par conséquent, il prend en charge un vaste éventail de fonctionnalités.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.reviewer: deverette
ms.openlocfilehash: 8ed7dd9b8b02ab58cfb6dacf031576004c8d0290
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871308"
---
# <a name="step-3-configure-the-remote-access-server-for-always-on-vpn"></a>Étape 3. Configurer le serveur d'accès à distance pour VPN Toujours actif (AlwaysOn)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Premier** Étape 2. Configurer l’infrastructure de serveur](vpn-deploy-server-infrastructure.md)
- [**Premier** Étape 4. Installer et configurer le serveur NPS (Network Policy Server)](vpn-deploy-nps.md)

RRAS est conçu pour fonctionner correctement à la fois comme un routeur et un serveur d’accès à distance, car il prend en charge un vaste éventail de fonctionnalités. Dans le cadre de ce déploiement, vous n’avez besoin que d’un petit sous-ensemble de ces fonctionnalités : la prise en charge des connexions VPN IKEv2 et du routage LAN.

IKEv2 est un protocole de tunneling VPN décrit dans Internet Engineering Task Force Request for Comments 7296. Le principal avantage de IKEv2 est qu’il tolère les interruptions de la connexion réseau sous-jacente. Par exemple, si la connexion est temporairement perdue ou si un utilisateur déplace un ordinateur client d’un réseau vers un autre, IKEv2 restaure automatiquement la connexion VPN lorsque la connexion réseau est rétablie, le tout sans intervention de l’utilisateur.

Configurez le serveur RRAS pour prendre en charge les connexions IKEv2 tout en désactivant les protocoles inutilisés, ce qui réduit l’encombrement de sécurité du serveur. En outre, configurez le serveur pour affecter des adresses aux clients VPN à partir d’un pool d’adresses statiques. Vous pouvez assigner des adresses à partir d’un pool ou d’un serveur DHCP. Toutefois, l’utilisation d’un serveur DHCP complique la conception et offre des avantages minimes.

>[!IMPORTANT]
>Il est important de :
>- Installez deux cartes réseau Ethernet sur le serveur physique. Si vous installez le serveur VPN sur une machine virtuelle, vous devez créer deux commutateurs virtuels externes, un pour chaque carte réseau physique ; puis créer deux cartes réseau virtuelles pour la machine virtuelle, chaque carte réseau étant connectée à un commutateur virtuel.
>
>- Installez le serveur sur votre réseau de périmètre entre vos pare-feu de périphérie et internes, avec une carte réseau connectée au réseau de périmètre externe et une carte réseau connectée au réseau de périmètre interne.

>[!WARNING]
>Avant de commencer, veillez à activer IPv6 sur le serveur VPN. Dans le cas contraire, une connexion ne peut pas être établie et un message d’erreur s’affiche.

## <a name="install-remote-access-as-a-ras-gateway-vpn-server"></a>Installer l’accès à distance en tant que serveur VPN de passerelle RAS

Dans cette procédure, vous installez le rôle accès à distance en tant que serveur VPN de passerelle RAS à client unique. Pour plus d’informations, voir [Accès à distance](../../../Remote-Access.md).

### <a name="install-the-remote-access-role-by-using-windows-powershell"></a>Installer le rôle accès à distance à l’aide de Windows PowerShell

1. Ouvrez Windows PowerShell en tant qu' **administrateur**.

2. Entrez et exécutez l’applet de commande suivante :

   ```powershell
   Install-WindowsFeature DirectAccess-VPN -IncludeManagementTools
   ```

   Une fois l’installation terminée, le message suivant s’affiche dans Windows PowerShell.

   ```powershell
   | Success | Restart Needed | Exit Code |               Feature Result               |
   |---------|----------------|-----------|--------------------------------------------|
   |  True   |       No       |  Success  | {RAS Connection Manager Administration Kit |
   ```

### <a name="install-the-remote-access-role-by-using-server-manager"></a>Installez le rôle accès à distance à l’aide de Gestionnaire de serveur

Vous pouvez utiliser la procédure suivante pour installer le rôle accès à distance à l’aide de Gestionnaire de serveur.

1. Sur le serveur VPN, dans Gestionnaire de serveur, sélectionnez **gérer** , puis sélectionnez **Ajouter des rôles et des fonctionnalités**.
   
   L’Assistant Ajout de rôles et de fonctionnalités s’ouvre.

2. Dans la page avant de commencer, sélectionnez **suivant**.

3. Dans la page Sélectionner le type d’installation, sélectionnez l’option d’installation basée sur un **rôle ou une fonctionnalité** , puis sélectionnez **suivant**.

4. Dans la page Sélectionner le serveur de destination, sélectionnez l’option **Sélectionner un serveur à partir du pool de serveurs** .

5. Sous pool de serveurs, sélectionnez l’ordinateur local, puis cliquez sur **suivant**.

6. Dans la page Sélectionner des rôles de serveurs, dans **rôles**, sélectionnez **accès à distance**, puis **suivant**.

7. Dans la page Sélectionner des fonctionnalités, sélectionnez **suivant**.

8. Sur la page accès à distance, sélectionnez **suivant**.

9.  Dans la page Sélectionner un service de rôle, dans **services de rôle**, sélectionnez **DirectAccess et VPN (RAS)** .

   La boîte de dialogue **Assistant Ajout de rôles et de fonctionnalités** s’ouvre.

11. Dans la boîte de dialogue Ajouter des rôles et des fonctionnalités, sélectionnez **Ajouter des fonctionnalités** , puis sélectionnez **suivant**.

12. Dans la page rôle de serveur Web (IIS), sélectionnez **suivant**.

13. Dans la page Sélectionner les services de rôle, sélectionnez **suivant**.

14. Sur la page confirmer les sélections d’installation, passez en revue vos choix, puis sélectionnez **installer**.

15. Une fois l’installation terminée, sélectionnez **Fermer**.

## <a name="configure-remote-access-as-a-vpn-server"></a>Configurer l’accès à distance en tant que serveur VPN

Dans cette section, vous pouvez configurer le VPN d’accès à distance pour autoriser les connexions VPN IKEv2, refuser les connexions à partir d’autres protocoles VPN et affecter un pool d’adresses IP statiques pour l’émission d’adresses IP à la connexion de clients VPN autorisés.

1. Sur le serveur VPN, dans Gestionnaire de serveur, sélectionnez l’indicateur **notifications** .

2. Dans le menu **tâches** , sélectionnez **ouvrir l’Assistant prise en main**

   L’Assistant Configuration de l’accès à distance s’ouvre.

   >[!NOTE]
   >L’Assistant Configuration de l’accès à distance peut s’ouvrir derrière Gestionnaire de serveur. Si vous pensez que l’Assistant prend trop de temps pour s’ouvrir, déplacez ou réduisez Gestionnaire de serveur pour savoir si l’Assistant est en arrière-plan. Si ce n’est pas le cas, attendez que l’Assistant s’initialise.

3. Sélectionnez **déployer un VPN uniquement**.

    La console MMC (Microsoft Management Console) routage et accès distant s’ouvre.

4. Cliquez avec le bouton droit sur le serveur VPN, puis sélectionnez **configurer et activer le routage et l’accès distant**.

   L’Assistant Installation du serveur de routage et d’accès à distance s’ouvre.

5. Dans l’Assistant Installation du serveur de routage et d’accès à distance, sélectionnez **suivant**.

6. Dans **configuration**, sélectionnez **configuration personnalisée**, puis cliquez sur **suivant**.

7. Dans **configuration personnalisée**, sélectionnez **accès VPN**, puis sélectionnez **suivant**.

   L’Assistant Installation du serveur de routage et d’accès à distance s’ouvre.

8. Sélectionnez **Terminer** pour fermer l’Assistant, puis sélectionnez **OK** pour fermer la boîte de dialogue routage et accès distant.

9. Sélectionnez **Démarrer le service** pour démarrer l’accès à distance.

10. Dans la console MMC d’accès à distance, cliquez avec le bouton droit sur le serveur VPN, puis sélectionnez **Propriétés**.

11. Dans Propriétés, sélectionnez l’onglet **sécurité** et effectuez les opérations suivantes :

    a. Sélectionnez **fournisseur d’authentification** , puis **authentification RADIUS**.

    b. Sélectionnez **configurer**.

       La boîte de dialogue authentification RADIUS s’ouvre.

    c. Sélectionnez **Ajouter**.

       La boîte de dialogue Ajouter un serveur RADIUS s’ouvre.

    d. Dans **nom du serveur**, entrez le nom de domaine complet (FQDN) du serveur NPS sur votre réseau d’entreprise/d’entreprise.
    
       Par exemple, si le nom NetBIOS de votre serveur NPS est le serveur NPS1 et que votre nom de domaine est corp.contoso.com, entrez **NPS1.Corp.contoso.com**.

    e. Dans **secret partagé**, sélectionnez **modifier**.

       La boîte de dialogue Modifier le secret s’ouvre.

    f. Dans **nouveau secret**, entrez une chaîne de texte.

    g. Dans **confirmer le nouveau secret**, entrez la même chaîne de texte, puis sélectionnez **OK**.

    >[!IMPORTANT]
    >Enregistrez cette chaîne de texte. Quand vous configurez le serveur NPS sur votre réseau d’entreprise/d’entreprise, vous allez ajouter ce serveur VPN en tant que client RADIUS. Au cours de cette configuration, vous allez utiliser ce même secret partagé afin que les serveurs NPS et VPN puissent communiquer.

12. Dans **Ajouter un serveur RADIUS**, passez en revue les paramètres par défaut pour :

    - **Délai d’expiration**

    - **Score initial**

    - **Importer**

13. Si nécessaire, modifiez les valeurs pour qu’elles correspondent à la configuration requise pour votre environnement et sélectionnez **OK**.

    Un NAS est un appareil qui fournit un certain niveau d’accès à un réseau de plus grande taille. Un NAS utilisant une infrastructure RADIUS est également un client RADIUS, qui envoie des demandes de connexion et des messages de gestion de comptes à un serveur RADIUS pour l’authentification, l’autorisation et la gestion des comptes.

14. Vérifiez le paramètre du **fournisseur de gestion des comptes**:

    |                    Si vous souhaitez que le...                     |                                                     Alors...                                                      |
    |-----------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|
    | Activité d’accès à distance enregistrée sur le serveur d’accès à distance |                               Assurez-vous que la **gestion des comptes Windows** est sélectionnée.                               |
    |        NPS pour effectuer des services de gestion de comptes pour VPN         | Remplacez le **fournisseur de gestion de comptes** par **RADIUS Accounting** , puis configurez le serveur NPS en tant que fournisseur de gestion des comptes. |

15. Sélectionnez l’onglet **IPv4** et procédez comme suit :

    a. Sélectionnez **pool d’adresses statiques**.

    b. Sélectionnez **Ajouter** pour configurer un pool d’adresses IP.

       Le pool d’adresses statiques doit contenir les adresses du réseau de périmètre interne. Ces adresses sont sur la connexion réseau interne sur le serveur VPN, et non sur le réseau d’entreprise.

    c. Dans **adresse IP de début**, entrez l’adresse IP de début dans la plage que vous souhaitez affecter aux clients VPN.

    d. Dans **adresse IP de fin**, entrez l’adresse IP de fin dans la plage que vous souhaitez affecter aux clients VPN ou, dans **nombre d’adresses**, entrez le numéro de l’adresse que vous souhaitez rendre disponible. Si vous utilisez DHCP pour ce sous-réseau, assurez-vous de configurer une exclusion d’adresses correspondante sur vos serveurs DHCP.

    e. Facultatif Si vous utilisez DHCP, sélectionnez **carte**et, dans la liste des résultats, sélectionnez la carte Ethernet connectée à votre réseau de périmètre interne.

16. Facultatif *Si vous configurez l’accès conditionnel pour la connectivité VPN*, dans la liste déroulante **certificat** , sous **liaison de certificat SSL**, sélectionnez l’authentification du serveur VPN.

17. Facultatif *Si vous configurez l’accès conditionnel pour la connectivité VPN*, dans la console MMC NPS, développez **stratégies\\stratégies réseau** et procédez comme suit : 

    a. Cliquez avec le bouton droit sur connexions à la stratégie réseau du **serveur d’accès à distance et de routage Microsoft** , puis sélectionnez **Propriétés**.

    b. Sélectionnez accorder **l’accès. Accordez l’accès si la demande de connexion** correspond à cette option de stratégie.

    c. Sous type de serveur d’accès réseau, sélectionnez **serveur d’accès à distance (VPN-accès** à distance) dans la liste déroulante.

18. Dans la console MMC routage et accès distant, cliquez avec le bouton droit sur **ports,** puis sélectionnez **Propriétés**. 
    
    La boîte de dialogue Propriétés des ports s’ouvre.

19. Sélectionnez **Miniport WAN (SSTP)** et sélectionnez **configurer**. La boîte de dialogue Configurer l’appareil-Miniport WAN (SSTP) s’ouvre.

    a. Désactivez les cases à cocher **connexions d’accès à distance (entrant uniquement)** et **connexions de routage à la demande (entrantes et sortantes)** .

    b. Sélectionnez **OK**.

20. Sélectionnez **Miniport WAN (L2TP)** et sélectionnez **configurer**. La boîte de dialogue Configurer l’appareil-Miniport WAN (L2TP) s’ouvre.

    a. Dans nombre **maximal**de ports, entrez le nombre de ports correspondant au nombre maximal de connexions VPN simultanées que vous souhaitez prendre en charge.

    b. Sélectionnez **OK**.

21. Sélectionnez **Miniport WAN (PPTP)** et sélectionnez **configurer**. La boîte de dialogue Configurer l’appareil-Miniport WAN (PPTP) s’ouvre.

    a. Dans nombre **maximal**de ports, entrez le nombre de ports correspondant au nombre maximal de connexions VPN simultanées que vous souhaitez prendre en charge.

    b. Sélectionnez **OK**.

22. Sélectionnez **Miniport WAN (IKEv2)** et sélectionnez **configurer**. La boîte de dialogue Configurer l’appareil-Miniport WAN (IKEv2) s’ouvre.

     a. Dans nombre **maximal**de ports, entrez le nombre de ports correspondant au nombre maximal de connexions VPN simultanées que vous souhaitez prendre en charge.

     b. Sélectionnez **OK**.

23. Si vous y êtes invité, sélectionnez **Oui** pour confirmer le redémarrage du serveur, puis sélectionnez **Fermer** pour redémarrer le serveur.

## <a name="next-step"></a>Étape suivante

[Étape 4. Installer et configurer le serveur NPS (Network Policy Server](vpn-deploy-nps.md)) : Dans cette étape, vous installez le serveur NPS (Network Policy Server) à l’aide de Windows PowerShell ou de l’Assistant Gestionnaire de serveur ajouter des rôles et des fonctionnalités. Vous configurez également NPS pour gérer toutes les tâches d’authentification, d’autorisation et de comptabilité pour les demandes de connexion qu’il reçoit du serveur VPN.
