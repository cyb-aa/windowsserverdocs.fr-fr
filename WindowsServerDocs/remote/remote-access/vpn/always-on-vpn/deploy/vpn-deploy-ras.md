---
title: Configurer le serveur d'accès à distance pour VPN Toujours actif (AlwaysOn)
description: RRAS est conçue pour effectuer ainsi qu’un routeur et un serveur d’accès à distance ; Par conséquent, il prend en charge un large éventail de fonctionnalités.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.reviewer: deverette
ms.openlocfilehash: 3920f7f075f4742a62577ade809cc0494b05bf1f
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749442"
---
# <a name="step-3-configure-the-remote-access-server-for-always-on-vpn"></a>Étape 3. Configurer le serveur d'accès à distance pour VPN Toujours actif (AlwaysOn)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Précédent :** Étape 2. Configurer l’Infrastructure de serveur](vpn-deploy-server-infrastructure.md)
- [**Précédent :** Étape 4. Installer et configurer le serveur de stratégie réseau (NPS)](vpn-deploy-nps.md)

RRAS est conçu pour effectuer ainsi qu’un routeur et un serveur d’accès à distance, car il prend en charge un large éventail de fonctionnalités. Dans le cadre de ce déploiement, vous avez besoin uniquement un petit sous-ensemble de ces fonctionnalités : prise en charge des connexions VPN IKEv2 et le routage de réseau local.

IKEv2 est décrite dans la Internet Engineering Task Force demande de commentaires 7296 de protocole de tunnel VPN. Le principal avantage de IKEv2 est qu’il tolère les interruptions de la connexion réseau sous-jacente. Par exemple, si la connexion est perdue temporairement ou si un utilisateur déplace un ordinateur client à partir d’un réseau à un autre, IKEv2 restaure automatiquement la connexion VPN lorsque la connexion réseau est rétablie, tout cela sans intervention de l’utilisateur.

Configurer le serveur RRAS pour prendre en charge les connexions IKEv2 lors de la désactivation des protocoles inutilisés, ce qui réduit l’encombrement de sécurité du serveur. En outre, configurez le serveur pour affecter des adresses aux clients VPN à partir d’un pool d’adresses statiques. Vous pouvez raisonnablement affecter des adresses à partir d’un pool ou un serveur DHCP ; Toutefois, à l’aide d’un serveur DHCP augmente la complexité de la conception et offre l’avantage minimal.

>[!IMPORTANT]
>Il est important de :
>- Installer deux cartes réseau Ethernet sur le serveur physique. Si vous installez le serveur VPN sur une machine virtuelle, vous devez créer deux commutateurs virtuels externes, une pour chaque carte réseau physique ; puis créez deux cartes réseau virtuelles pour la machine virtuelle, avec chaque carte réseau connectée à un commutateur virtuel.
>
>- Installez le serveur sur votre réseau de périmètre entre votre edge et le pare-feu internes, avec une carte réseau connectée au réseau de périmètre externe et une carte réseau connectée au réseau de périmètre interne.

>[!WARNING]
>Avant de commencer, assurez-vous d’activer IPv6 sur le serveur VPN. Sinon, Impossible d’établir une connexion et affiche un message d’erreur.

## <a name="install-remote-access-as-a-ras-gateway-vpn-server"></a>Installer l’accès distant comme un serveur VPN de passerelle RAS

Dans cette procédure, vous installez le rôle accès à distance en tant que serveur VPN de passerelle RAS client unique. Pour plus d’informations, voir [Accès à distance](../../../Remote-Access.md).

### <a name="install-the-remote-access-role-by-using-windows-powershell"></a>Installer le rôle accès à distance à l’aide de Windows PowerShell

1. Ouvrez PowerShell de Windows en tant que **administrateur**.

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

### <a name="install-the-remote-access-role-by-using-server-manager"></a>Installer le rôle accès à distance à l’aide du Gestionnaire de serveur

Vous pouvez utiliser la procédure suivante pour installer le rôle accès à distance à l’aide du Gestionnaire de serveur.

1. Sur le serveur VPN, dans le Gestionnaire de serveur, sélectionnez **gérer** et sélectionnez **Ajout de rôles et fonctionnalités**.
   
   L’Assistant Ajout de rôles et de fonctionnalités s’ouvre.

2. Sur l’avant de commencer page, sélectionnez **suivant**.

3. Dans la page Sélectionner le Type d’Installation, sélectionnez le **installation en fonction du rôle ou une fonctionnalité** option et sélectionnez **suivant**.

4. Dans la page serveur de sélectionner la destination, sélectionnez le **sélectionner un serveur du pool de serveurs** option.

5. Sous le Pool de serveurs, sélectionnez l’ordinateur local puis **suivant**.

6. Dans le, sélectionnez les rôles de serveur page **rôles**, sélectionnez **accès à distance**, puis **suivant**.

7. Dans la page Sélectionner les fonctionnalités, sélectionnez **suivant**.

8. Dans la page accès à distance, sélectionnez **suivant**.

9.  Dans la page de service de sélectionner un rôle, dans **services de rôle**, sélectionnez **DirectAccess et VPN (RAS)** .

   Le **Assistant Ajouter des rôles et fonctionnalités** boîte de dialogue s’ouvre.

11. Dans la boîte de dialogue Ajouter des rôles et fonctionnalités, sélectionnez **ajouter des fonctionnalités** puis sélectionnez **suivant**.

12. Dans la page rôle de serveur Web (IIS), sélectionnez **suivant**.

13. Dans la page services de sélectionner un rôle, sélectionnez **suivant**.

14. Sur la page Confirmer les sélections installation, passez en revue vos choix, puis sélectionnez **installer**.

15. Une fois l’installation terminée, sélectionnez **fermer**.

## <a name="configure-remote-access-as-a-vpn-server"></a>Configurer l’accès à distance comme un serveur VPN

Dans cette section, vous pouvez configurer l’accès à distance VPN pour autoriser les connexions VPN IKEv2, refuser les connexions à partir d’autres protocoles VPN et affecter un pool d’adresses IP statiques pour l’émission d’adresses IP à la connexion des clients VPN autorisés.

1. Sur le serveur VPN, dans le Gestionnaire de serveur, sélectionnez le **Notifications** indicateur.

2. Dans le **tâches** menu, sélectionnez **ouvrir l’Assistant Mise en route**

   L’Assistant de configuration de l’accès à distance s’ouvre.

   >[!NOTE]
   >L’Assistant Configuration de l’accès à distance peut ouvrir derrière le Gestionnaire de serveur. Si vous pensez que l’Assistant prend trop de temps pour ouvrir, déplacer ou réduisez le Gestionnaire de serveur pour déterminer si l’Assistant est situé derrière lui. Si ce n’est pas le cas, attendez que l’Assistant d’initialisation.

3. Sélectionnez **déployer uniquement les VPN**.

    Le routage et à distance accès Console MMC (Microsoft Management) s’ouvre.

4. Cliquez sur le serveur VPN, puis sélectionnez **configurer et activer le routage et accès à distance**.

   Le routage et l’Assistant de configuration de serveur accès à distance s’ouvre.

5. Dans l’écran Bienvenue le routage et l’Assistant de configuration de serveur accès à distance, sélectionnez **suivant**.

6. Dans **Configuration**, sélectionnez **Configuration personnalisée**, puis sélectionnez **suivant**.

7. Dans **Configuration personnalisée**, sélectionnez **accès VPN**, puis sélectionnez **suivant**.

   La fin de le routage et l’Assistant de configuration de serveur accès à distance s’ouvre.

8. Sélectionnez **Terminer** pour fermer l’Assistant, puis sélectionnez **OK** pour fermer la boîte de dialogue Routage et accès distant.

9. Sélectionnez **démarrer le service** pour démarrer l’accès à distance.

10. Dans la console MMC accès à distance, le serveur VPN avec le bouton droit, puis sélectionnez **propriétés**.

11. Dans Propriétés, sélectionnez le **sécurité** onglet et faire :

    a. Sélectionnez **fournisseur d’authentification** et sélectionnez **l’authentification RADIUS**.

    b. Sélectionnez **configurer**.

       La boîte de dialogue authentification RADIUS s’ouvre.

    c. Sélectionnez **ajouter**.

       La boîte de dialogue Ajouter un serveur RADIUS s’ouvre.

    d. Dans **nom du serveur**, entrez le nom de domaine complet (FQDN) du serveur NPS sur le réseau de votre organisation / d’entreprise.
    
       Par exemple, si le nom NetBIOS de votre serveur NPS est serveur NPS1 et votre nom de domaine est corp.contoso.com, entrez **NPS1.corp.contoso.com**.

    e. Dans **secret partagé**, sélectionnez **modification**.

       La boîte de dialogue Modifier le Secret s’ouvre.

    f. Dans **nouveau secret**, entrez une chaîne de texte.

    g. Dans **confirmer le nouveau secret**, entrez la même chaîne de texte, puis sélectionnez **OK**.

    >[!IMPORTANT]
    >Enregistrez cette chaîne de texte. Lorsque vous configurez le serveur NPS sur le réseau de votre organisation / d’entreprise, vous allez ajouter ce serveur VPN comme Client RADIUS. Au cours de cette configuration, vous allez utiliser cet même secret partagé afin que le serveur NPS et les serveurs VPN peuvent communiquer.

12. Dans **ajouter un serveur RADIUS**, passez en revue les paramètres par défaut :

    - **Délai d’attente**

    - **Score initial**

    - **Port**

13. Si nécessaire, modifiez les valeurs en fonction des besoins de votre environnement, puis sélectionnez **OK**.

    Un NAS est un périphérique qui fournit un certain niveau d’accès à un réseau plus large. Un NAS à l’aide d’une infrastructure RADIUS est également un client RADIUS, envoie des demandes de connexion et des messages de comptabilité à un serveur RADIUS pour l’authentification, autorisation et la comptabilité.

14. Vérifiez le paramètre pour **comptabilité fournisseur**:

    |                    Si vous souhaitez que la...                     |                                                     Alors...                                                      |
    |-----------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|
    | Activité d’accès à distance enregistrée sur le serveur d’accès à distance |                               Assurez-vous que l’option **Windows comptabilité** est sélectionné.                               |
    |        NPS pour effectuer des services de gestion des comptes pour VPN         | Modification **fournisseur de comptes** à **gestion des comptes RADIUS** , puis configurez le serveur NPS en tant que le fournisseur de comptes. |

15. Sélectionnez le **IPv4** onglet et faire :

    a. Sélectionnez **pool d’adresses statiques**.

    b. Sélectionnez **ajouter** pour configurer un pool d’adresses IP.

       Le pool d’adresses statiques doit contenir des adresses du réseau de périmètre interne. Ces adresses sont sur la connexion de réseau interne sur le serveur VPN, et non dans le réseau d’entreprise.

    c. Dans **adresse IP de début**, entrez l’adresse IP de début dans la plage que vous souhaitez affecter à des clients VPN.

    d. Dans **adresse IP de fin**, entrez l’adresse IP de fin dans la plage que vous souhaitez affecter à des clients VPN, ou dans **nombre d’adresses**, entrez le numéro de l’adresse que vous souhaitez rendre disponibles. Si vous utilisez DHCP pour ce sous-réseau, veillez à configurer une adresse d’exclusion correspondant sur vos serveurs DHCP.

    e. (Facultatif) Si vous utilisez DHCP, sélectionnez **adaptateur**et dans la liste des résultats, sélectionnez la carte Ethernet connectée à votre réseau de périmètre interne.

16. (Facultatif) *Si vous configurez l’accès conditionnel pour la connectivité VPN*, à partir de la **certificat** déroulante liste sous **liaison de certificat SSL**, sélectionnez le serveur VPN authentification.

17. (Facultatif) *Si vous configurez l’accès conditionnel pour la connectivité VPN*, dans la console MMC NPS, développez **stratégies\\stratégies réseau** et faire : 

    a. Le droit **connexions à Microsoft Routing et accès à distance** stratégie réseau et sélectionnez **propriétés**.

    b. Sélectionnez le **accorder l’accès. Accorder l’accès si la demande de connexion correspond à cette stratégie** option.

    c. Sous Type de serveur d’accès réseau, sélectionnez **serveur d’accès à distance (VPN-accès à distance)** à partir de la liste déroulante.

18. Dans le routage et l’accès à distance de MMC, cliquez sur **Ports,** , puis sélectionnez **propriétés**. 
    
    La boîte de dialogue Propriétés des Ports s’ouvre.

19. Sélectionnez **Miniport WAN (SSTP)** et sélectionnez **configurer**. Configurer le périphérique - Miniport WAN (SSTP) s’ouvre.

    a. Effacer la **connexions d’accès à distance (uniquement entrantes)** et **connexions de routage à la demande (entrantes et sortantes)** cases à cocher.

    b. Sélectionnez **OK**.

20. Sélectionnez **Miniport de réseau étendu (L2TP)** et sélectionnez **configurer**. Configurer le périphérique - Miniport de réseau étendu (L2TP) s’ouvre.

    a. Dans **nombre Maximum de ports**, entrez le nombre de ports pour faire correspondre le nombre maximal de connexions VPN simultanées que vous souhaitez prendre en charge.

    b. Sélectionnez **OK**.

21. Sélectionnez **Miniport de réseau étendu (PPTP)** et sélectionnez **configurer**. Configurer le périphérique - Miniport WAN (PPTP) s’ouvre.

    a. Dans **nombre Maximum de ports**, entrez le nombre de ports pour faire correspondre le nombre maximal de connexions VPN simultanées que vous souhaitez prendre en charge.

    b. Sélectionnez **OK**.

22. Sélectionnez **Miniport de réseau étendu (IKEv2)** et sélectionnez **configurer**. Configurer le périphérique - Miniport de réseau étendu (IKEv2) s’ouvre.

     a. Dans **nombre Maximum de ports**, entrez le nombre de ports pour faire correspondre le nombre maximal de connexions VPN simultanées que vous souhaitez prendre en charge.

     b. Sélectionnez **OK**.

23. Si vous y êtes invité, sélectionnez **Oui** pour confirmer le redémarrage du serveur et sélectionnez **fermer** pour redémarrer le serveur.

## <a name="next-step"></a>Étape suivante

[Étape 4. Installer et configurer le serveur de stratégie réseau (NPS)](vpn-deploy-nps.md): Dans cette étape, vous installez le serveur NPS (Network Policy Server) à l’aide de Windows PowerShell ou l’Assistant de fonctionnalités et un gestionnaire serveur ajouter des rôles. Vous configurez également NPS pour gérer l’authentification, l’autorisation et les droits de gestion des comptes de connexion toutes les demandes qu’il reçoit à partir du serveur VPN.
