---
title: Résolution des problèmes de connexion au Bureau à distance
description: Procédures de dépannage classées par symptôme
ms.custom: na
ms.reviewer: rklemen; josh.bender
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: RobVyK
manager: ''
ms.author: kaushika; rklemen; josh.bender; v-tea
ms.date: 02/22/2019
ms.localizationpriority: medium
ms.openlocfilehash: c6ce719ffa24cfc6704348c17548fe5cf33d9271
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284148"
---
# <a name="troubleshooting-remote-desktop-connections"></a>Résolution des problèmes de connexion au Bureau à distance
Pour obtenir de brèves explications sur certains des problèmes de Services Bureau à distance les plus courants, consultez [Forum aux questions sur les clients Bureau à distance](https://review.docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-client-faq). Cet article décrit plusieurs approches plus avancées de résolution des problèmes de connexion. Bon nombre de ces procédures s’appliquent à la fois au dépannage d’une configuration simple, par exemple un seul ordinateur physique se connectant à un autre ordinateur physique, et d’une configuration plus complexe. Certaines procédures abordent des problèmes qui se produisent uniquement dans des scénarios multi-utilisateurs plus compliqués. Pour plus d’informations sur les composants de Bureau à distance et sur la façon dont ils interagissent, consultez [Architecture des services Bureau à distance](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/desktop-hosting-logical-architecture).

> [!NOTE]  
> Une grande partie des procédures décrites dans cet article vous demandent d’accéder à plusieurs ordinateurs, dont certains à distance. Pour plus d’informations sur les outils d’administration à distance et sur la façon de les configurer, consultez [Outils d’administration de serveur distant (RSAT) pour systèmes d’exploitation Windows](https://support.microsoft.com/en-us/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems).

Dans la liste suivante, identifiez le type de problème que vous (ou vos utilisateurs) rencontrez.

- [Le client Bureau à distance ne peut pas se connecter au Bureau à distance, mais il n’y a aucun symptôme ou message spécifique (étapes de dépannage générales)](#no-specific-symptoms-or-messages-general-troubleshooting-steps)
- [Le client Bureau à distance ne peut pas se connecter au Bureau à distance et reçoit un message « Classe non enregistrée »](#client-cannot-connect-class-not-registered)
- [Le client Bureau à distance ne peut pas se connecter au Bureau à distance et reçoit un message « aucune licence disponible » ou « erreur de sécurité »](#client-cannot-connect-no-licenses-available)
- [L’utilisateur reçoit un message « Accès refusé », ou il doit fournir les informations d’identification à deux reprises](#user-cannot-authenticate-or-must-authenticate-twice)
- [Lors de la connexion, l’utilisateur reçoit un message « Les Services Bureau à distance sont actuellement occupés »](#on-connecting-user-receives-remote-desktop-service-is-currently-busy-message)
- [Le client Bureau à distance se déconnecte et ne peut pas se reconnecter à la même session](#rd-client-disconnects-and-cannot-reconnect-to-the-same-session)
- [L’utilisateur se connecte à un ordinateur portable distant par le biais d’un réseau sans fil, puis l’ordinateur portable se déconnecte du réseau](#remote-laptop-disconnects-from-wireless-network).
- [Les performances sont médiocres ou l’utilisateur rencontre des problèmes avec les applications distantes](#user-experiences-poor-performance-or-application-problems)

> [!NOTE]  
> Pour obtenir la liste actuelle des codes de déconnexion RDP, consultez [ExtendedDisconnectReasonCode, énumération](https://docs.microsoft.com/windows/desktop/TermServ/extendeddisconnectreasoncode). 

## <a name="no-specific-symptoms-or-messages-general-troubleshooting-steps"></a>Aucun symptôme ou message spécifique (étapes de dépannage générales)

Utilisez ces étapes quand un client Bureau à distance ne peut pas se connecter à un Bureau à distance, mais ne fournit pas de messages ou autres symptômes qui aideraient à identifient la cause. Pour éliminer la plupart des causes les plus courantes de ce genre de problème, appliquez les procédures suivantes :

- [Vérifier l’état du protocole RDP](#check-the-status-of-the-rdp-protocol)
- [Vérifier l’état des services RDP](#check-the-status-of-the-rdp-services)
- [Vérifier que l’écouteur RDP fonctionne](#check-that-the-rdp-listener-is-functioning)
- [Vérifier le port d’écoute RDP](#check-the-rdp-listener-port)

### <a name="check-the-status-of-the-rdp-protocol"></a>Vérifier l’état du protocole RDP

#### <a name="check-the-status-of-the-rdp-protocol-on-a-local-computer"></a>Vérifier l’état du protocole RDP sur un ordinateur local

Pour vérifier et changer l’état du protocole RDP sur un ordinateur local, consultez [Comment activer le Bureau à distance](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access#how-to-enable-remote-desktop).

> [!NOTE]  
> Si les options de Bureau à distance ne sont pas disponibles, consultez [Vérifier si un objet de stratégie de groupe bloque le protocole RDP](#check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer).

#### <a name="check-the-status-of-the-rdp-protocol-on-a-remote-computer"></a>Vérifier l’état du protocole RDP sur un ordinateur distant

> [!IMPORTANT]  
> Suivez attentivement les étapes décrites dans cette section. De graves problèmes peuvent se produire si vous modifiez le Registre de façon incorrecte. Avant de le modifier, [sauvegardez le Registre afin de pouvoir le restaurer](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) en cas de problème.

Pour vérifier et changer l’état du protocole RDP sur un ordinateur distant, utilisez une connexion au Registre réseau :

1. Sélectionnez **Démarrer**, **Exécuter**, puis entrez **regedt32**.
2. Dans l’Éditeur du Registre, sélectionnez **Fichier**, puis **Connexion au Registre réseau**.
3. Dans la boîte de dialogue **Sélectionner un ordinateur**, entrez le nom de l’ordinateur distant, sélectionnez **Vérifier les noms**, puis **OK**.
4. Accédez à **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server**.  
   ![Éditeur du Registre montrant l’entrée fDenyTSConnections](../media/troubleshoot-remote-desktop-connections/RegEntry_fDenyTSConnections.png)
   - Si la valeur de la clé **fDenyTSConnections** est **0**, le protocole RDP est activé.
   - Si la valeur de la clé **fDenyTSConnections** est **1**, le protocole RDP est désactivé.
5. Pour activer le protocole RDP, remplacez la valeur **1** de **fDenyTSConnections** par **0**.

#### <a name="check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer"></a>Vérifier si un objet de stratégie de groupe bloque le protocole RDP sur un ordinateur local

Si vous ne pouvez pas activer le protocole RDP dans l’interface utilisateur ou si la valeur de **fDenyTSConnections** revient à **1** après que vous l’avez modifiée, il est possible qu’un objet de stratégie de groupe remplace les paramètres au niveau de l’ordinateur.

Pour vérifier la configuration de stratégie de groupe sur un ordinateur local, ouvrez une fenêtre d’invite de commandes en tant qu’administrateur, puis entrez la commande suivante :
```
gpresult /H c:\gpresult.html
```
Une fois cette commande terminée, ouvrez gpresult.html. Dans **Configuration ordinateur\\Modèles d’administration\\Composants Windows\\Services Bureau à distance\\Hôte de session Bureau à distance\\Connexions**, recherchez la stratégie **Autoriser les utilisateurs à se connecter à distance à l’aide des services Bureau à distance**.

- Si le paramètre de cette stratégie est **Activé**, la stratégie de groupe ne bloque pas les connexions RDP.
- Si le paramètre de cette stratégie est **Désactivé**, vérifiez **OSG gagnant**. Il s’agit de l’objet de stratégie de groupe qui bloque les connexions RDP.
  ![Exemple de segment de gpresult.html, dans lequel l’objet de stratégie de groupe au niveau du domaine **Bloquer RDP** désactive le protocole RDP.](../media/troubleshoot-remote-desktop-connections/GPResult_RDSH_Connections_GP.png)
   
  ![Exemple de segment de gpresult.html, dans lequel **Stratégie de groupe locale** désactive le protocole RDP.](../media/troubleshoot-remote-desktop-connections/GPResult_RDSH_Connections_LGP.png)

#### <a name="check-whether-a-gpo-is-blocking-rdp-on-a-remote-computer"></a>Vérifier si un objet de stratégie de groupe bloque le protocole RDP sur un ordinateur distant

Pour vérifier la configuration de stratégie de groupe sur un ordinateur distant, la commande est presque identique à celle pour un ordinateur local :
```
gpresult /S <computer name> /H c:\gpresult-<computer name>.html
```
Le fichier généré par cette commande (**gpresult-\<nom_ordinateur\>.html**) utilise le même format d’informations que la version pour ordinateur local (**gpresult.html**).

#### <a name="modifying-a-blocking-gpo"></a>Modification d’un objet de stratégie de groupe bloquant

Vous pouvez modifier ces paramètres dans l’Éditeur d’objets de stratégie de groupe et dans la Console de gestion des stratégies de groupe (GPMC). Pour plus d’informations sur l’utilisation de la stratégie de groupe, consultez [Advanced Group Policy Management](https://docs.microsoft.com/microsoft-desktop-optimization-pack/agpm/).

Pour modifier la stratégie bloquante, appliquez l’une des méthodes suivantes :

- Dans l’Éditeur d’objets de stratégie de groupe, accédez au niveau d’objet de stratégie de groupe approprié (par exemple local ou domaine), puis accédez à **Configuration ordinateur\\Modèles d’administration\\Composants Windows\\Services Bureau à distance\\Hôte de session Bureau à distance\\Connexions**\\**Autoriser les utilisateurs à se connecter à distance à l’aide des services Bureau à distance**.  
   1. Affectez la valeur **Activé** ou **Non configuré** à la stratégie.
   2. Sur les ordinateurs affectés, ouvrez une fenêtre d’invite de commandes en tant qu’administrateur et exécutez la commande **gpupdate /force**.
- Dans l’Éditeur d’objets de stratégie de groupe, accédez à l’unité d’organisation dans laquelle la stratégie bloquante est appliquée aux ordinateurs affectés, et supprimez cette stratégie de l’unité d’organisation.

### <a name="check-the-status-of-the-rdp-services"></a>Vérifier l’état des services RDP

Sur l’ordinateur local (client) et l’ordinateur distant (cible), les services suivants doivent être en cours d’exécution :

- Services Bureau à distance (TermService)
- Redirecteur de port du mode utilisateur des services Bureau à distance (UmRdpService)

Vous pouvez utiliser le composant logiciel enfichable MMC Services pour gérer les services localement ou à distance. Vous pouvez également utiliser PowerShell localement ou à distance (si l’ordinateur distant est configuré pour accepter les commandes PowerShell à distance).

![Services Bureau à distance dans le composant logiciel enfichable MMC Services. Ne modifiez pas les paramètres de service par défaut.](../media/troubleshoot-remote-desktop-connections/RDSServiceStatus.png)

Sur l’un ou l’autre ordinateur, si l’un des services n’est pas en cours d’exécution, démarrez-le.

> [!NOTE]  
> Si vous démarrez le service Services Bureau à distance, cliquez sur **Oui** pour redémarrer automatiquement le service Redirecteur de port du mode utilisateur des services Bureau à distance.

### <a name="check-that-the-rdp-listener-is-functioning"></a>Vérifier que l’écouteur RDP fonctionne

> [!IMPORTANT]  
> Suivez attentivement les étapes décrites dans cette section. De graves problèmes peuvent se produire si vous modifiez le Registre de façon incorrecte. Avant de le modifier, [sauvegardez le Registre afin de pouvoir le restaurer](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) en cas de problème.

#### <a name="check-the-status-of-the-rdp-listener"></a>Vérifier l’état de l’écouteur RDP

Pour cette procédure, utilisez une instance de PowerShell qui dispose d’autorisations d’administration. Pour un ordinateur local, vous pouvez également utiliser une invite de commandes qui dispose d’autorisations d’administration. Toutefois, cette procédure utilise PowerShell car les mêmes commandes fonctionnent à la fois localement et à distance.

1. Ouvrez une fenêtre PowerShell. Pour vous connecter à un ordinateur distant, entrez **Enter-PSSession -ComputerName \<nom_ordinateur\>** .
2. Entrez **qwinsta**. 
    ![La commande qwinsta liste les processus à l’écoute sur les ports de l’ordinateur.](../media/troubleshoot-remote-desktop-connections/WPS_qwinsta.png)
3. Si la liste comprend **rdp-tcp** avec l’état **Écouter**, l’écouteur RDP fonctionne. Passez à [Vérifier le port d’écoute RDP](#check-the-rdp-listener-port). Sinon, passez à l’étape 4.
4. Exportez la configuration de l’écouteur RDP à partir d’un ordinateur opérationnel.
    1. Connectez-vous à un ordinateur qui a la même version de système d’exploitation que l’ordinateur affecté, et accédez au Registre de cet ordinateur (par exemple à l’aide de l’Éditeur du Registre).
    2. Accédez à l’entrée de Registre suivante :  
        **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\RDP-Tcp**
    3. Exportez l’entrée vers un fichier .reg. Par exemple, dans l’Éditeur du Registre, cliquez avec le bouton droit sur l’entrée, sélectionnez **Exporter**, puis entrez un nom de fichier pour les paramètres exportés.
    4. Copiez le fichier .reg exporté sur l’ordinateur affecté.
5. Pour importer la configuration de l’écouteur RDP, ouvrez une fenêtre PowerShell qui dispose d’autorisations administratives sur l’ordinateur affecté (ou ouvrez la fenêtre PowerShell et connectez-vous à distance à l’ordinateur affecté).
   1. Pour sauvegarder l’entrée de Registre existante, entrez la commande suivante :  
   
      ```powershell  
      cmd /c 'reg export "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp" C:\Rdp-tcp-backup.reg'   
      ```  
   

   2. Pour supprimer l’entrée de Registre existante, entrez les commandes suivantes :  
   
      ```powershell  
      Remove-Item -path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp' -Recurse -Force  
      ```
   
   3. Pour importer la nouvelle entrée de Registre et redémarrer le service, entrez les commandes suivantes :  
   
      ```powershell  
      cmd /c 'regedit /s c:\<filename>.reg'  
      Restart-Service TermService -Force  
      ```   
      où \<filename\> est le nom du fichier .reg exporté.
6. Testez la configuration en réessayant la connexion Bureau à distance. Si vous ne pouvez toujours pas vous connecter, redémarrez l’ordinateur affecté.
7. Si vous ne pouvez toujours pas vous connecter, [vérifiez l’état du certificat auto-signé RDP](#check-the-status-of-the-rdp-self-signed-certificate).

#### <a name="check-the-status-of-the-rdp-self-signed-certificate"></a>Vérifier l’état du certificat auto-signé RDP

1. Si vous ne pouvez toujours pas vous connecter, ouvrez le composant logiciel enfichable MMC Certificats. Quand vous êtes invité à sélectionner le magasin de certificats à gérer, sélectionnez **Compte d’ordinateur**, puis sélectionnez l’ordinateur affecté.
2. Dans le dossier **Certificats** sous **Bureau à distance**, supprimez le certificat auto-signé RDP. 
    ![Certificats de Bureau à distance dans le composant logiciel enfichable MMC Certificats.](../media/troubleshoot-remote-desktop-connections/MMCCert_Delete.png)
3. Sur l’ordinateur affecté, redémarrez le service Services Bureau à distance.
4. Actualisez le composant logiciel enfichable Certificats.
5. Si le certificat auto-signé RDP n’a pas été recréé, [vérifiez les autorisations du dossier MachineKeys](#check-the-permissions-of-the-machinekeys-folder).

#### <a name="check-the-permissions-of-the-machinekeys-folder"></a>Vérifier les autorisations du dossier MachineKeys

1. Sur l’ordinateur affecté, ouvrez l’Explorateur, puis accédez à **C:\\ProgramData\\Microsoft\\Crypto\\RSA\\** .
2. Cliquez avec le bouton droit sur **MachineKeys**, sélectionnez **Propriétés**, **Sécurité**, puis **Avancé**.
3. Vérifiez que les autorisations suivantes sont configurées :
      - Builtin\\Administrateurs : Contrôle total
      - Tout le monde : Lecture, Écriture

### <a name="check-the-rdp-listener-port"></a>Vérifier le port d’écoute RDP

Sur l’ordinateur local (client) et l’ordinateur distant (cible), l’écouteur RDP doit être à l’écoute sur le port 3389. Aucune autre application ne doit utiliser ce port.

> [!IMPORTANT]  
> Suivez attentivement les étapes décrites dans cette section. De graves problèmes peuvent se produire si vous modifiez le Registre de façon incorrecte. Avant de le modifier, [sauvegardez le Registre afin de pouvoir le restaurer](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) en cas de problème.

Pour vérifier ou changer le port RDP, utilisez l’Éditeur du Registre :

1. Sélectionnez Démarrer, **Exécuter**, puis entrez **regedt32**.
      - Pour vous connecter à un ordinateur distant, sélectionnez **Fichier**, puis **Connexion au Registre réseau**.
      - Dans la boîte de dialogue **Sélectionner un ordinateur**, entrez le nom de l’ordinateur distant, sélectionnez **Vérifier les noms**, puis **OK**.
2. Ouvrez le Registre et accédez à **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<listener\>** . 
    ![La sous-clé PortNumber pour le protocole RDP.](../media/troubleshoot-remote-desktop-connections/RegEntry_PortNumber.png)
3. Si **PortNumber** a une valeur autre que **3389**, remplacez-la par **3389**. 
   > [!IMPORTANT]  
    > Vous pouvez utiliser les services Bureau à distance à l’aide d’un autre port. Toutefois, nous vous le déconseillons. Le dépannage d’une telle configuration n’entre pas dans le cadre de cet article.
4. Après avoir changé le numéro de port, redémarrez le service Services Bureau à distance.

#### <a name="check-that-another-application-is-not-trying-to-use-the-same-port"></a>Vérifier qu’aucune autre application ne tente d’utiliser le même port

Pour cette procédure, utilisez une instance de PowerShell qui dispose d’autorisations d’administration. Pour un ordinateur local, vous pouvez également utiliser une invite de commandes qui dispose d’autorisations d’administration. Toutefois, cette procédure utilise PowerShell car les mêmes commandes fonctionnent localement et à distance.

1. Ouvrez une fenêtre PowerShell. Pour vous connecter à un ordinateur distant, entrez **Enter-PSSession -ComputerName \<nom_ordinateur\>** .
2. Entrez la commande suivante :  
   
     ```powershell  
    cmd /c 'netstat -ano | find "3389"'  
    ```
  
    ![La commande netstat génère une liste de ports et les services qui sont à l’écoute sur ces ports.](../media/troubleshoot-remote-desktop-connections/WPS_netstat.png)
3. Recherchez une entrée pour le port TCP 3389 (ou le port RDP attribué) associé à l'état **Écoute**. 
    > [!NOTE]  
   > Le PID (Identificateur de processus) du processus ou service utilisant ce port apparaît sous la colonne PID.
4. Pour déterminer quelle application utilise le port 3389 (ou le port RDP attribué), entrez la commande suivante :  
   
     ```powershell  
    cmd /c 'tasklist /svc | find "<pid listening on 3389>"'  
    ```  
  
    ![La commande tasklist fournit des détails sur un processus spécifique.](../media/troubleshoot-remote-desktop-connections/WPS_tasklist.png)
5. Recherchez une entrée pour le numéro PID associé au port (dans la sortie de **netstat**). Les services ou processus associés à ce PID apparaissent à droite.
6. Si une application ou un service autre que les Services Bureau à distance (TermServ.exe) utilise le port, vous pouvez résoudre le conflit en appliquant l’une des méthodes suivantes :
      - Configurer l’autre application ou service pour utiliser un autre port (recommandé).
      - Désinstaller l’autre application ou service.
      - Configurer le protocole RDP pour utiliser un autre port, puis redémarrer le service Services Bureau à distance (non recommandé).

#### <a name="check-whether-a-firewall-is-blocking-the-rdp-port"></a>Vérifier si un pare-feu bloque le port RDP

Utilisez l’outil **psping** pour tester si vous pouvez accéder à l’ordinateur affecté à l’aide du port 3389.

1. Sur un ordinateur différent de l’ordinateur affecté, téléchargez **psping** à partir de <https://live.sysinternals.com/psping.exe>.
2. Ouvrez une fenêtre d’invite de commandes en tant qu’administrateur, accédez au répertoire dans lequel vous avez installé **psping**, puis entrez la commande suivante :  
   
   ```  
   psping -accepteula <computer IP>:3389  
   ```
   
3. Vérifiez dans la sortie de la commande **psping** la présence de résultats tels que :  
      - **Connecting to \<adresse_IP_ordinateur\>**  : l’ordinateur distant est accessible.
      - **(0% loss)**  : toutes les tentatives de connexion ont réussi.
      - **The remote computer refused the network connection** : l’ordinateur distant n’est pas accessible.
      - **(100% loss)**  : toutes les tentatives de connexion ont échoué.
1. Exécutez **psping** sur plusieurs ordinateurs afin de tester leur capacité à se connecter à l’ordinateur affecté.
1. Notez si l’ordinateur affecté bloque les connexions à partir de tous les autres ordinateurs, de certains autres ordinateurs ou d’un seul autre ordinateur.
1. Étapes suivantes recommandées :
      - Demander à vos administrateurs réseau de vérifier que le réseau autorise le trafic RDP vers l’ordinateur affecté.
      - Identifier les configurations de tous les pare-feu entre les ordinateurs sources et l’ordinateur affecté (notamment le Pare-feu Windows sur l’ordinateur affecté) afin de déterminer si un pare-feu bloque le port RDP.

## <a name="client-cannot-connect-class-not-registered"></a>Le client ne peut pas se connecter, « Classe non enregistrée »

Quand vous essayez de vous connecter à un ordinateur distant à l’aide d’un client qui exécute Windows 10 version 1709 ou ultérieure, le client peut ne pas se connecter et le serveur Hôte de session Bureau à distance signaler un message qui contient le code d’erreur « Classe non enregistrée (0x80040154) ».

Ce problème se produit si l’utilisateur qui essaie de se connecter a un profil utilisateur obligatoire. Pour résoudre ce problème, installez la mise à jour de Windows 10 du [24 juillet 2018 — KB4338817 (build du système d’exploitation 16299.579)](https://support.microsoft.com/en-us/help/4338817/windows-10-update-kb4338817).

## <a name="client-cannot-connect-no-licenses-available"></a>Le client ne peut pas se connecter, aucune licence disponible

Cette situation s’applique aux déploiements qui incluent un serveur RDSH et un serveur Gestionnaire de licences des services Bureau à distance.

Identifiez le type de comportement observé par les utilisateurs :

- La session a été déconnectée car aucune licence n’est disponible ou aucun serveur de licences n’est disponible
- L’accès a été refusé en raison d’une erreur de sécurité

Connectez-vous à l’Hôte de session Bureau à distance en tant qu’administrateur de domaine et ouvrez l’outil de diagnostic des licences des services Bureau à distance. Recherchez des messages tels que les suivants :

  - La période de grâce pour le serveur Hôte de session Bureau à distance a expiré, mais le serveur Hôte de session Bureau à distance n’a été configuré avec aucun serveur de licence. Les connexions au serveur Hôte de session Bureau à distance seront refusées, sauf si un serveur de licences est configuré pour le serveur Hôte de session Bureau à distance.
  - Le serveur de licences \<nom_ordinateur\> n’est pas disponible. Cela peut être dû à des problèmes de connectivité réseau, au fait que le Gestionnaire de licences des services Bureau à distance est arrêté sur le serveur de licences ou au fait qu’il n’est plus installé sur l’ordinateur.

Ces problèmes ont tendance à être associés aux messages utilisateur suivants :

  - La session a distance a été déconnectée, car aucune licence d’accès client Bureau à distance n’est disponible pour cet ordinateur.
  - La session a distance a été déconnectée, car aucun serveur de licences Bureau à distance n’est disponible pour fournir une licence.

Dans ce cas, [configurez le service Gestionnaire de licences des services Bureau à distance](#configure-the-rd-licensing-service).

Si l’outil de diagnostic des licences des services Bureau à distance signale d’autres problèmes, tels que « Le composant X.224 du protocole RDP a détecté une erreur dans le flux du protocole et a déconnecté le client », il y a peut-être un problème qui affecte les certificats de licences. Ces problèmes ont tendance à être associés à des messages utilisateur tels que les suivants :

Le client n’a pas pu se connecter au serveur Terminal Server en raison d’une erreur de sécurité. Après avoir vérifié que vous êtes connecté au réseau, réessayez de vous connecter au serveur.

Dans ce cas, [actualisez les clés de Registre du certificat X509](#refresh-the-x509-certificate-registry-keys).

### <a name="configure-the-rd-licensing-service"></a>Configurer le service Gestionnaire de licences des services Bureau à distance

La procédure suivante utilise le Gestionnaire de serveur pour apporter les modifications de configuration. Pour plus d’informations sur la façon de configurer et d’utiliser le Gestionnaire de serveur, consultez [Gestionnaire de serveur](https://docs.microsoft.com/windows-server/administration/server-manager/server-manager).

1. Ouvrez le Gestionnaire de serveur, puis accédez à **Services Bureau à distance**.
2. Dans **Vue d’ensemble du déploiement**, sélectionnez **Tâches**, puis **Modifier les propriétés de déploiement**.
3. Sélectionnez **Gestionnaire de licences des services Bureau à distance**, puis sélectionnez le mode de licence approprié pour votre déploiement (**Par appareil** ou **Par utilisateur**).
4. Entrez le nom de domaine complet de votre serveur de licences des services Bureau à distance, puis sélectionnez **Ajouter**.
5. Si vous avez plusieurs serveurs de licences des services Bureau à distance, répétez l’étape 4 pour chaque serveur. 
    ![Options de configuration de serveur de licences des services Bureau à distance dans le Gestionnaire de serveur.](../media/troubleshoot-remote-desktop-connections/RDLicensing_Configure.png)

### <a name="refresh-the-x509-certificate-registry-keys"></a>Actualiser les clés de Registre du certificat X509

> [!IMPORTANT]  
> Suivez attentivement les étapes décrites dans cette section. De graves problèmes peuvent se produire si vous modifiez le Registre de façon incorrecte. Avant de le modifier, [sauvegardez le Registre afin de pouvoir le restaurer](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) en cas de problème.

Pour résoudre ce problème, sauvegardez puis supprimez les clés de Registre du certificat X509, redémarrez l’ordinateur, puis réactivez le serveur Gestionnaire de licences des services Bureau à distance. Pour ce faire, procédez comme suit.

> [!NOTE]  
> Appliquez la procédure suivante sur chaque serveur RDSH.

1. Ouvrez l’Éditeur du Registre et accédez à **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\RCM**.
2. Dans le menu Registre, sélectionnez **Exporter un fichier du Registre**.
3. Tapez **certificat exporté** dans la zone **Nom de fichier**, puis sélectionnez **Enregistrer**.
4. Cliquez avec le bouton droit sur chacune des valeurs suivantes, sélectionnez **Supprimer**, puis **Oui** pour vérifier la suppression :  
      - **Certificate**
      - **X509 Certificate**
      - **X509 Certificate ID**
      - **X509 Certificate2**
5. Fermez l’Éditeur du Registre et redémarrez le serveur RDSH.

## <a name="user-cannot-authenticate-or-must-authenticate-twice"></a>L’utilisateur ne peut pas s’authentifier ou doit s’authentifier deux fois

Plusieurs problèmes peuvent affecter l’authentification utilisateur. Cette section aborde les cas suivants :

  - [Un utilisateur se voit refuser l’accès avec un message « type de connexion restreint »](#access-denied-restricted-type-of-logon)
  - [Un utilisateur ou une application se voit refuser l’accès ave l’événement 16965, « Un appel distant vers la base de données SAM a été refusé »](#access-denied-a-remote-call-to-the-sam-database-has-been-denied)
  - [Un utilisateur ne peut pas se connecter à l’aide d’une carte à puce](#a-user-cannot-sign-in-by-using-a-smart-card)
  - [Si le PC distant est verrouillé, un utilisateur doit entrer un mot de passe à deux reprises](#if-the-remote-pc-is-locked-a-user-needs-to-enter-a-password-twice)
  - [Un utilisateur ne peut pas se connecter et reçoit des messages « Erreur d’authentification » et « Correction d’oracle de chiffrement CredSSP »](#user-cannot-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages)
  - [Après que vous avez mis à jour des ordinateurs clients, certains utilisateurs doivent se connecter à deux reprises](#after-you-update-client-computers-some-users-need-to-sign-in-twice)
  - [Les utilisateurs se voient refuser l’accès sur un déploiement qui utilise RemoteGuard avec plusieurs services Broker pour les connexions Bureau à distance](#users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers)

### <a name="access-denied-restricted-type-of-logon"></a>Accès refusé, type de connexion restreint

Dans cette situation, un utilisateur de Windows 10 qui tente de se connecter à des ordinateurs Windows 10 ou Windows Server2016 se voit refuser l’accès avec le message suivant :

> Connexion Bureau à distance :  
> L’administrateur système a restreint le type de connexion (réseau ou interactive) que vous pouvez utiliser. Demandez de l’aide à votre administrateur ou votre support technique.

Ce problème se produit quand l’authentification au niveau du réseau est nécessaire pour les connexions RDP, et que l’utilisateur n’est pas membre du groupe **Utilisateurs du Bureau à distance**. Il peut également se produire si le groupe **Utilisateurs du Bureau à distance** n’a pas été affecté au droit d’utilisateur **Accéder à cet ordinateur à partir du réseau**.

Pour remédier à ce problème, effectuez l’une des étapes suivantes :

  - [Modifier l’attribution des droits de l’utilisateur ou l’appartenance au groupe de l’utilisateur](#modify-the-users-group-membership-or-user-rights-assignment)
  - Désactiver l’authentification au niveau du réseau (non recommandé)
  - Utilisez des clients de Bureau à distance autres que Windows 10. Les clients Windows 7, par exemple, n’ont pas ce problème.

#### <a name="modify-the-users-group-membership-or-user-rights-assignment"></a>Modifier l’attribution des droits de l’utilisateur ou l’appartenance au groupe de l’utilisateur

Si ce problème concerne un seul utilisateur, la solution la plus simple consiste à ajouter l’utilisateur au groupe **Utilisateurs du Bureau à distance**.

Si l’utilisateur est déjà membre de ce groupe (ou si plusieurs membres du groupe ont le même problème), vérifiez la configuration des droits de l’utilisateur sur l’ordinateur distant Windows 10 ou Windows Server 2016.

1. Ouvrez l’Éditeur d’objets de stratégie de groupe et connectez-vous à la stratégie locale de l’ordinateur distant.
2. Accédez à **Configuration ordinateur\\Paramètres Windows\\Paramètres de sécurité\\Stratégies locales\\Attribution des droits utilisateur**, cliquez avec le bouton droit sur **Accéder à cet ordinateur à partir du réseau**, puis sélectionnez **Propriétés**.
3. Vérifiez si **Utilisateurs du Bureau à distance** (ou un groupe parent) figure dans la liste des utilisateurs et groupes.
4. Si la liste n’inclut pas **Utilisateurs du Bureau à distance** (ou un groupe parent, tel que **Tout le monde**), vous devez l’y ajouter (si vous avez plus d’un ou deux ordinateurs dans votre déploiement, utilisez un objet de stratégie de groupe) .  
    Par exemple, l’appartenance par défaut pour **Accéder à cet ordinateur à partir du réseau** inclut **Tout le monde**. Si votre déploiement utilise un objet de stratégie de groupe pour supprimer **Tout le monde**, vous devrez peut-être restaurer l’accès en mettant à jour l’objet de stratégie de groupe de façon à ajouter **Utilisateurs du Bureau à distance**.

### <a name="access-denied-a-remote-call-to-the-sam-database-has-been-denied"></a>Accès refusé, un appel à distance à la base de données SAM a été refusé

Ce comportement est susceptible de se produire si vos contrôleurs de domaine exécutent Windows Server 2016 ou version ultérieure, et que les utilisateurs tentent de se connecter à l’aide d’une application de connexion personnalisée. Les applications qui accèdent aux informations de profil de l’utilisateur dans Active Directory, notamment, se verront refuser l’accès.

Ce comportement est dû à une modification dans Windows. Dans Windows Server 2012 R2 et versions antérieures, quand un utilisateur se connecte à un Bureau à distance, le Gestionnaire des connexions d’accès à distance contacte le contrôleur de domaine afin d’interroger les configurations qui sont propres au Bureau à distance sur l’objet utilisateur dans Active Directory Domain Services (AD DS). Ces informations sont affichées sous l’onglet Profil des services Bureau à distance des propriétés d’objets d’un utilisateur dans le composant logiciel enfichable MMC Utilisateurs et ordinateurs Active Directory.

À compter de Windows Server 2016, le Gestionnaire des connexions d’accès à distance n’interroge plus l’objet utilisateur dans AD DS. Si vous avez besoin que le Gestionnaire des connexions d’accès à distance interrogent les services AD DS car vous utilisez les attributs des Services Bureau à distance, vous devez activer manuellement le comportement précédent du Gestionnaire des connexions d’accès à distance.

> [!IMPORTANT]  
> Suivez attentivement les étapes décrites dans cette section. De graves problèmes peuvent se produire si vous modifiez le Registre de façon incorrecte. Avant de le modifier, [sauvegardez le Registre afin de pouvoir le restaurer](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) en cas de problème.

Pour activer le comportement hérité du Gestionnaire des connexions d’accès à distance sur un serveur Hôte de session Bureau à distance, configurez les entrées de Registre suivantes, puis redémarrez le service **Services Bureau à distance** :  
  - **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Policies\\Microsoft\\Windows NT\\Terminal Services**
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<Winstation name\>\\**  
      - Nom : **fQueryUserConfigFromDC**
      - Tapez : **Reg\_DWORD**
      - Valeur : **1** (Décimal)

Pour activer le comportement hérité du Gestionnaire des connexions d’accès à distance sur un serveur autre qu’un serveur Hôte de session Bureau à distance, configurez ces entrées de Registre ainsi que l’entrée de Registre supplémentaire suivante (puis redémarrez le service) :
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server**

Pour plus d’informations sur ce comportement, consultez l’article KB 3200967 [Modifications pour le Gestionnaire de connexion à distance dans Windows Server](https://support.microsoft.com/en-us/help/3200967/changes-to-remote-connection-manager-in-windows-server).

### <a name="a-user-cannot-sign-in-by-using-a-smart-card"></a>Un utilisateur ne peut pas se connecter à l’aide d’une carte à puce

Cet article aborde trois cas courants dans lesquels un utilisateur ne peut pas se connecter à un Bureau à distance à l’aide d’une carte à puce :  
  - [L’utilisateur ne peut pas se connecter à une filiale qui utilise un contrôleur de domaine en lecture seule (RODC)](#cannot-sign-in-with-a-smart-card-in-a-branch-office-with-a-rodc)
  - [L’utilisateur ne peut pas se connecter à un ordinateur Windows Server 2008 SP2 récemment mis à jour](#cannot-sign-in-with-a-smart-card-to-a-windows-server-2008-sp2-computer)
  - [L’utilisateur ne peut pas rester connecté à un ordinateur Windows récemment mis à jour, et le service Services Bureau à distance ne répond plus (se bloque)](#cannot-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs)

#### <a name="cannot-sign-in-with-a-smart-card-in-a-branch-office-with-a-rodc"></a>Impossible de se connecter avec une carte à puce dans une filiale avec un RODC

Ce problème se produit dans les déploiements qui incluent un serveur RDSH sur un site de filiale qui utilise un RODC. Le serveur RDSH est hébergé dans le domaine racine. Les utilisateurs sur le site de filiale appartiennent à un domaine enfant et utilisent des cartes à puce pour l’authentification. Le RODC est configuré pour mettre en cache les mots de passe des utilisateurs (le RODC appartient au **Groupe de réplication dont le mot de passe RODC est autorisé**). Quand des utilisateurs essaient d’ouvrir des sessions sur le serveur RDSH, ils reçoivent des messages tels que « La tentative d’ouverture de session n’est pas valide. Ceci est dû soit à un nom d’utilisateur incorrect, soit à des informations d’authentification incorrectes. »

Il s’agit d’un problème connu lié à la manière dont le contrôleur de domaine racine et le RODC gèrent le chiffrement des informations d’identification de l’utilisateur. Le contrôleur de domaine racine utilise une clé de chiffrement pour chiffrer les informations d’identification, et le RODC fournit une clé de déchiffrement au client. Toutefois, les deux clés ne correspondent pas.

Pour contourner ce problème, modifiez la topologie de votre contrôleur de domaine en désactivant la mise en cache des mots de passe sur le RODC ou en déployant un contrôleur de domaine accessible en écriture sur le site de filiale. Une autre méthode consiste à déplacer le serveur RDSH vers le même domaine enfant que les utilisateurs. Une autre alternative consiste à autoriser les utilisateurs à se connecter sans carte à puce. Ces solutions peuvent nécessiter des compromis en matière de performances ou de niveau de sécurité.

#### <a name="cannot-sign-in-with-a-smart-card-to-a-windows-server-2008-sp2-computer"></a>Impossible de se connecter avec une carte à puce sur un ordinateur Windows Server 2008 SP2

Ce problème se produit quand des utilisateurs se connectent à un ordinateur Windows Server 2008 SP2 qui a été mis à jour avec KB4093227 (2018.4B). Quand des utilisateurs tentent de se connecter à l’aide d’une carte à puce, ils se voient refuser l’accès avec des messages tels que « Aucun certificat valide n’a été trouvé. Vérifiez que la carte est bien insérée. » En même temps, l’ordinateur Windows Server enregistre l’événement d’application « Une erreur s’est produite lors du retrait d’un certificat numérique de la carte à puce insérée. Signature non valide. »

Pour résoudre ce problème, mettez à jour l’ordinateur Windows Server avec la nouvelle publication 2018.06 B de KB 4093227, [Description de la mise à jour de sécurité pour la vulnérabilité de déni de service dans le protocole RDP (Remote Desktop Protocol) pour Windows Server 2008 datée du 10 avril 2018](https://support.microsoft.com/en-us/help/4093227/security-update-for-vulnerabilities-in-windows-server-2008).

#### <a name="cannot-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs"></a>Impossible de rester connecté avec une carte à puce et le service Services Bureau à distance se bloque

Ce problème se produit quand les utilisateurs se connectent à un ordinateur Windows ou Windows Server qui a été mis à jour avec le correctif KB 4056446. Dans un premier temps, l’utilisateur peut être en mesure de se connecter au système à l’aide d’une carte à puce, mais ensuite il reçoit un message d’erreur « SCARD\_E\_NO\_SERVICE ». L’ordinateur distant peut cesser de répondre.

Pour contourner ce problème, redémarrez l’ordinateur distant.

Pour résoudre ce problème, mettez à jour l’ordinateur distant avec le correctif approprié :

  - Windows Server 2008 SP2 : KB 4090928, [Fuites de handles dans le processus lsm.exe et les applications de carte à puce peuvent afficher des erreurs « SCARD\_E\_NO\_SERVICE »](https://support.microsoft.com/en-us/help/4090928/scard-e-no-service-errors-when-windows-leaks-handles-in-the-lsm-exe)
  - Windows Server 2012 R2 : KB 4103724, [17 mai 2018—KB4103724 (préversion du correctif cumulatif mensuel)](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724)
  - Windows Server 2016 et Windows 10, version 1607 : KB 4103720, [17 mai 2018—KB4103720 (build du système d’exploitation 14393.2273)](https://support.microsoft.com/en-us/help/4103720/windows-10-update-kb4103720)

### <a name="if-the-remote-pc-is-locked-a-user-needs-to-enter-a-password-twice"></a>Si le PC distant est verrouillé, un utilisateur doit entrer un mot de passe à deux reprises

Ce problème peut se produire quand un utilisateur tente de se connecter à un Bureau à distance qui exécute Windows 10 version 1709 dans un déploiement dans lequel les connexions RDP ne nécessitent pas l’authentification au niveau du réseau. Dans ces conditions, si le Bureau à distance a été verrouillé, l’utilisateur doit entrer ses informations d’identification à deux reprises lors de la connexion.

Pour résoudre ce problème, mettez à jour l’ordinateur Windows 10 version 1709 avec le correctif KB 4343893, [30 août 2018—KB4343893 (build du système d’exploitation 16299.637)](https://support.microsoft.com/en-us/help/4343893/windows-10-update-kb4343893).

### <a name="user-cannot-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages"></a>L’utilisateur ne peut pas se connecter et reçoit des messages « Erreur d’authentification » et « Correction d’oracle de chiffrement CredSSP »

Quand des utilisateurs essaient de se connecter à l’aide de Windows Vista SP2 ou version ultérieure ou de Windows Server 2008 SP2 ou version ultérieure, l’accès leur est refusé et ils reçoivent des messages tels que les suivants :

  - Une erreur d’authentification s’est produite. La fonction demandée n’est pas prise en charge
  - Cela peut être dû à la correction d’oracle de chiffrement CredSSP

Le terme « correction d’oracle de chiffrement CredSSP » fait référence à un ensemble de mises à jour de sécurité publiées en mars, avril et mai 2018. CredSSP est un fournisseur d’authentification qui traite les demandes d’authentification pour d’autres applications. Les mises à jour du 13 mars 2018, « 3B » et ultérieures traitent d’une faille de sécurité qui fait qu’un attaquant pourrait relayer les informations d’identification de l’utilisateur pour exécuter du code sur le système cible.

Les mises à jour initiales ont ajouté la prise en charge d’un nouvel objet de stratégie de groupe, **Correction d’oracle de chiffrement**, qui a les paramètres possibles suivants :

  - **Vulnérable**. Les applications clientes qui utilisent CredSSP peuvent revenir aux versions non sécurisées, mais ce comportement expose les Bureaux distants à des attaques. Les services qui utilisent CredSSP acceptent les clients qui n’ont pas été mis à jour.
  - **Atténué**. Les applications clientes qui utilisent CredSSP ne peuvent pas revenir aux versions non sécurisées, mais les services qui utilisent CredSSP acceptent les clients qui n’ont pas été mis à jour.
  - **Forcer la mise à jour des clients**. Les applications clientes qui utilisent CredSSP ne peuvent pas revenir aux versions non sécurisées, et les services qui utilisent CredSSP n’acceptent pas les clients non corrigés. 
    Remarque: Ce paramètre ne doit pas être déployé tant que tous les hôtes distants ne prennent pas en charge la version la plus récente.

La mise à jour du 8 mai 2018 a changé la valeur par défaut de **Correction d’oracle de chiffrement** de **Vulnérable** à **Atténué**. Avec ce changement, les clients Bureau à distance qui ont les mises à jour ne peuvent pas se connecter aux serveurs qui ne les ont pas (ou aux serveurs mis à jour qui n’ont pas été redémarrés). Pour plus d’informations sur les effets des mises à jour et sur les types de communication qu’ils bloquent, consultez KB 4093492, [Mises à jour de CredSSP pour CVE-2018-0886](https://support.microsoft.com/en-us/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018).

Pour résoudre ce problème, assurez-vous que tous les systèmes sont entièrement mis à jour et ont été redémarrés. Pour obtenir une liste complète des mises à jour et pour plus d’informations sur les vulnérabilités, consultez [CVE-2018-0886 | Vulnérabilité d’exécution de code à distance dans CredSSP](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-0886).

Pour contourner ce problème jusqu’à ce que les mises à jour soient terminées, consultez KB 4093492 afin de connaître les types de connexions autorisés. S’il n’existe aucune alternative possible, vous pouvez envisager l’une des méthodes suivantes :

- Pour les ordinateurs clients affectés, réaffectez la valeur **Vulnérable** à la stratégie **Correction d’oracle de chiffrement**.
- Modifiez les stratégies suivantes dans le dossier de stratégie de groupe **Configuration ordinateur\\Modèles d’administration\\Composants Windows\\Services Bureau à distance\\Hôte de session Bureau à distance\\ Sécurité** :  
  - **Nécessite l’utilisation d’une couche de sécurité spécifique pour les connexions distantes (RDP)**  : affectez la valeur **Activé** et sélectionnez **RDP**.
  - **Requérir l’authentification utilisateur pour les connexions à distance à l’aide de l’authentification au niveau du réseau** : affectez la valeur **Désactivé**.
    > [!IMPORTANT]  
    > Ces modifications réduisent la sécurité de votre déploiement. Elles doivent être uniquement temporaires.

Pour plus d’informations sur l’utilisation de la stratégie de groupe, consultez [Modification d’un objet de stratégie de groupe bloquant](#modifying-a-blocking-gpo).

### <a name="after-you-update-client-computers-some-users-need-to-sign-in-twice"></a>Après que vous avez mis à jour des ordinateurs clients, certains utilisateurs doivent se connecter à deux reprises

Des utilisateurs sur Windows 7 ou Windows 10 1709 se connectent à une session Bureau à distance et voient immédiatement un deuxième écran de connexion. Ce problème se produit si les ordinateurs clients ont reçu les mises à jour suivantes :

  - Windows 7 : KB 4103718, [8 mai 2018—KB4103718 (correctif cumulatif mensuel)](https://support.microsoft.com/en-us/help/4103718/windows-7-update-kb4103718)
  - Windows 10 1709 : KB 4103727, [8 mai 2018—KB4103727 (build du système d’exploitation 16299.431)](https://support.microsoft.com/en-us/help/4103727/windows-10-update-kb4103727)

Pour résoudre ce problème, vérifiez que les ordinateurs auxquels les utilisateurs souhaitent se connecter (ainsi que les serveurs RDSH ou RDVI) sont entièrement mis à jour jusqu’en juin 2018. Celle-ci comprend les mises à jour suivantes :

  - Windows Server 2016 : KB 4284880, [12 juin 2018—KB4284880 (build du système d’exploitation 14393.2312)](https://support.microsoft.com/en-us/help/4284880/windows-10-update-kb4284880)
  - Windows Server 2012 R2 : KB 4284815, [12 juin 2018—KB4284815 (correctif cumulatif mensuel)](https://support.microsoft.com/en-us/help/4284815/windows-81-update-kb4284815)
  - Windows Server 2012 : KB 4284855, [12 juin 2018—KB4284855 (correctif cumulatif mensuel)](https://support.microsoft.com/en-us/help/4284855/windows-server-2012-update-kb4284855)
  - Windows Server 2008 R2 : KB 4284826, [12 juin 2018—KB4284826 (correctif cumulatif mensuel)](https://support.microsoft.com/en-us/help/4284826/windows-7-update-kb4284826)
  - Windows Server 2008 SP2 : KB4056564, [Description de la mise à jour de sécurité pour la vulnérabilité d’exécution de code à distance dans CredSSP pour Windows Server 2008, Windows Embedded POSReady 2009 et Windows Embedded Standard 2009 datée du 13 mars 2018](https://support.microsoft.com/en-us/help/4056564/security-update-for-vulnerabilities-in-windows-server-2008)

### <a name="users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers"></a>Les utilisateurs se voient refuser l’accès sur un déploiement qui utilise Credential Guard à distance avec plusieurs services Broker pour les connexions Bureau à distance

Ce problème se produit dans les déploiements à haute disponibilité qui utilisent plusieurs services Broker pour les connexions Bureau à distance, si Windows Defender Credential Guard à distance est en cours d’utilisation. Les utilisateurs ne peuvent pas se connecter aux Bureaux à distance.

Ce problème se produit car Credential Guard à distance utilise Kerberos pour l’authentification et restreint NTLM. Dans une configuration à haute disponibilité avec équilibrage de charge, les services Broker pour les connexions Bureau à distance ne peuvent pas prendre en charge les opérations Kerberos.

Si vous avez besoin d’utiliser une configuration à haute disponibilité avec des services Broker pour les connexions Bureau à distance à charge équilibrée, vous pouvez contourner ce problème en désactivant Credential Guard à distance. Pour plus d’informations sur la façon de gérer Windows Defender Credential Guard à distance, consultez [Protéger les informations d’identification du Bureau à distance avec Windows Defender Credential Guard à distance](https://docs.microsoft.com/windows/security/identity-protection/remote-credential-guard#enable-windows-defender-remote-credential-guard).

## <a name="on-connecting-user-receives-remote-desktop-service-is-currently-busy-message"></a>Lors de la connexion, l’utilisateur reçoit un message « Les Services Bureau à distance sont actuellement occupés »

Pour déterminer comment répondre au mieux à ce problème, effectuez les étapes suivantes :

1. Le service Services Bureau à distance cesse-t-il de répondre ? (Par exemple, le client Bureau à distance semble « se bloquer » à l’écran d’accueil.)  
      - Si le service ne répond pas, consultez [Problème de mémoire du serveur RDSH](#rdsh-server-memory-issue).
      - Si le client semble interagir normalement avec le service, passez à l’étape suivante.
2. Si un ou plusieurs utilisateurs déconnectent leurs sessions Bureau à distance, peuvent-ils se reconnecter ?  
   - Si le service continue à refuser les connexions quel que soit le nombre d’utilisateurs qui déconnectent leurs sessions, consultez [Problème d’écouteur de Bureau à distance](#rd-listener-issue).
   - Si le service commence à réaccepter les connexions après que plusieurs utilisateurs ont déconnecté leurs sessions, [vérifiez la stratégie de limite du nombre de connexions](#check-the-connection-limit-policy).

### <a name="rdsh-server-memory-issue"></a>Problème de mémoire du serveur RDSH

Une fuite de mémoire a été constatée sur certains serveurs RDSH Windows Server 2012 R2. Avec le temps, ces serveurs commencent à refuser les connexions Bureau à distance et les connexions de console locale, avec des messages tels que les suivants :

> La tâche que vous essayez de réaliser ne peut pas se terminer car le service Bureau à distance est actuellement occupé. Réessayez dans quelques minutes. Cela ne devrait pas empêcher d’autres utilisateurs d’ouvrir une session.

Les clients Bureau à distance tentant de se connecter cessent également de répondre.

Pour contourner ce problème, redémarrez le serveur RDSH.

Pour résoudre ce problème, appliquez le correctif KB 4093114, [10 avril 2018—KB4093114 (correctif cumulatif mensuel)](file:///C:/Users/v-jesits/AppData/Local/Microsoft/Windows/INetCache/Content.Outlook/FUB8OO45/April%2010,%202018—KB4093114%20(Monthly%20Rollup)), sur les serveurs RDSH.

### <a name="rd-listener-issue"></a>Problème d’écouteur de Bureau à distance

Un problème a été observé sur certains serveurs RDSH qui ont été mis à niveau directement de Windows Server 2008 R2 vers Windows Server 2012 R2 ou Windows Server 2016. Quand un client Bureau à distance se connecte au serveur RDSH, celui-ci crée un écouteur de Bureau à distance pour la session utilisateur. Les serveurs affectés comptabilisent les écouteurs de Bureau à distance, dont le nombre augmente à mesure que des utilisateurs se connectent mais ne diminue jamais.

Pour contourner ce problème, vous pouvez appliquer les méthodes suivantes :

  - Redémarrer le serveur RDSH pour réinitialiser le nombre d’écouteurs de Bureau à distance
  - Modifier la stratégie de limite du nombre de connexions, en lui affectant une valeur très élevée. Pour plus d’informations sur la gestion de la stratégie de limite du nombre de connexions, consultez [Vérifier la stratégie de limite du nombre de connexions](#check-the-connection-limit-policy).

Pour résoudre ce problème, appliquez les mises à jour suivantes aux serveurs RDSH :

  - Windows Server 2012 R2 : KB 4343891, [30 août 2018—KB4343891 (préversion du correctif cumulatif mensuel)](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)
  - Windows Server 2016 : KB 4343884, [30 août 2018—KB4343884 (build du système d’exploitation 14393.2457)](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)

### <a name="check-the-connection-limit-policy"></a>Vérifier la stratégie de limite du nombre de connexions

Vous pouvez définir la limite du nombre de connexions Bureau à distance simultanées au niveau de l’ordinateur ou en configurant un objet de stratégie de groupe. Par défaut, la limite n’est pas définie.

Pour vérifier les paramètres actuels et identifier les objets de stratégie de groupe existants sur le serveur RDSH, ouvrez une fenêtre d’invite de commandes en tant qu’administrateur et entrez la commande suivante :
  
```
gpresult /H c:\gpresult.html
```
   
Une fois la commande terminée, ouvrez gpresult.html et, dans **Configuration ordinateur\\Modèles d’administration\\Composants Windows\\Services Bureau à distance\\Hôte de session Bureau à distance\\Connexions**, recherchez la stratégie **Limiter le nombre de connexions**.

  - Si le paramètre de cette stratégie est **Désactivé**, cela signifie que la stratégie de groupe ne limite pas le nombre de connexions RDP.
  - Si le paramètre de cette stratégie est **Activé**, vérifiez **OSG gagnant**. Si vous avez besoin de supprimer ou de changer la limite du nombre de connexions, modifiez cet objet de stratégie de groupe.

Pour appliquer les modifications de stratégie, ouvrez une fenêtre d’invite de commandes sur l’ordinateur affecté et entrez la commande suivante :
  
```
gpupdate /force
```
  
## <a name="rd-client-disconnects-and-cannot-reconnect-to-the-same-session"></a>Le client Bureau à distance se déconnecte et ne peut pas se reconnecter à la même session

Quand un client Bureau à distance perd sa connexion au Bureau à distance, il ne peut pas se reconnecter immédiatement. L’utilisateur reçoit des messages d’erreur semblables à ceux-ci :

  - Le client n’a pas pu se connecter au serveur Terminal Server en raison d’une erreur de sécurité. Après avoir vérifié que vous êtes connecté au réseau, réessayez de vous connecter au serveur.
  - Bureau à distance déconnecté. Le client n’a pas pu se connecter à l’ordinateur distant en raison d’une erreur de sécurité. Vérifiez que vous êtes connecté au réseau et essayez de vous reconnecter.

Ultérieurement, le client Bureau à distance se reconnecte au Bureau à distance. Toutefois, plutôt que de connecter le client à la session d’origine, le serveur RDSH le connecte à une nouvelle session. Une inspection du serveur RDSH révèle que la session d’origine n’a pas basculé à l’état « déconnectée », mais qu’elle reste active.

Pour contourner ce problème, vous pouvez activer la stratégie **Configurer l’intervalle de conservation des connexions** dans le dossier de stratégie de groupe **Configuration ordinateur\\Modèles d’administration\\Composants Windows\\Services Bureau à distance\\Hôte de session Bureau à distance\\Connexions**. Si vous activez cette stratégie, vous devez entrer un intervalle de conservation. L’intervalle de conservation détermine la fréquence, en minutes, à laquelle le serveur vérifie l’état de session.

Ce problème peut être dû à une configuration incorrecte de vos paramètres d’authentification et de configuration. Vous pouvez configurer ces paramètres au niveau du serveur ou à l’aide d’objets de stratégie de groupe. Les paramètres de stratégie de groupe sont disponibles dans le dossier de stratégie de groupe **Configuration ordinateur\\Modèles d’administration\\Composants Windows\\Services Bureau à distance\\Hôte de session Bureau à distance\\ Sécurité**.

1. Sur le serveur Hôte de session Bureau à distance, ouvrez Configuration d'hôte de session Bureau à distance.
2. Sous **Connexions**, cliquez avec le bouton droit sur le nom de la connexion, puis cliquez sur **Propriétés**.
3. Dans la boîte de dialogue **Propriétés** de la connexion, sous l’onglet **Général**, dans la couche **Sécurité**, sélectionnez une méthode de sécurité.
4. Dans **Niveau de chiffrement**, cliquez sur le niveau souhaité. Vous pouvez sélectionner **Faible**, **Compatible avec le client**, **Élevé** ou **Compatible FIPS**.

> [!NOTE]  
>  - Quand les communications entre les clients et les serveurs Hôtes de session Bureau à distance nécessitent le niveau de chiffrement le plus élevé, utilisez le chiffrement Compatible FIPS.
>  - Les paramètres de niveau de chiffrement que vous configurez dans la stratégie de groupe remplacent la configuration que vous définissez à l’aide de l’outil Configuration des services Bureau à distance. En outre, si vous activez la stratégie [Chiffrement système : utilisez des algorithmes compatibles FIPS pour le chiffrement, le hachage et la signature](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc780081\(v=ws.10\)), ce paramètre remplace la stratégie **Définir le niveau de chiffrement de la connexion client**. La stratégie de chiffrement système se trouve dans le dossier **Configuration ordinateur\\Paramètres Windows\\Paramètres de sécurité\\Stratégies locales\\Options de sécurité**.
>  - Lorsque vous modifiez le niveau de chiffrement, le nouveau niveau de chiffrement prend effet lors de la prochaine connexion d'un utilisateur. Si vous avez besoin de plusieurs niveaux de chiffrement sur un serveur, installez plusieurs cartes réseau et configurez chaque carte séparément.
>  - Pour vérifier que le certificat a une clé privé correspondante, dans Configuration des services Bureau à distance, cliquez sur la connexion pour laquelle vous souhaitez afficher le certificat, sélectionnez **Général**, **Modifier**, sélectionnez le certificat à afficher, puis **Afficher le certificat**. En bas de l’onglet **Général**, l’instruction, « Vous avez une clé privée qui correspond à ce certificat » doit apparaître. Vous pouvez également afficher ces informations à l’aide du composant logiciel enfichable Certificats.
>  - Le chiffrement compatible FIPS (la stratégie **Chiffrement système : utilisez des algorithmes compatibles FIPS pour le chiffrement, le hachage et la signature** ou le paramètre **Compatible FIPS** dans Configuration des services Bureau à distance) chiffre et déchiffre les données envoyées du client au serveur et du serveur au client, avec les algorithmes de chiffrement FIPS (Federal Information Processing Standard) 140-1, à l’aide de modules de chiffrement Microsoft. Pour plus d’informations, consultez [Validation FIPS 140](https://docs.microsoft.com/windows/security/threat-protection/fips-140-validation).
>  - Le paramètre **Élevé** chiffre les données envoyées du client au serveur et du serveur au client à l’aide d’un chiffrement renforcé 128 bits.
>  - Le paramètre **Compatible avec le client** chiffre les données envoyées entre le client et le serveur avec la force de clé maximale prise en charge par le client.
>  - Le paramètre **Faible** chiffre les données envoyées du client au serveur à l’aide du chiffrement 56 bits.

## <a name="remote-laptop-disconnects-from-wireless-network"></a>L’ordinateur portable distant se déconnecte du réseau sans fil

Ce problème peut se produire quand un client Bureau à distance se connecte à un ordinateur portable par le biais d’un réseau sans fil 802.1x. Par intermittence, l’ordinateur portable se déconnecte du réseau sans fil et ne se reconnecte pas automatiquement.

Il s’agit d’un problème connu qui se produit quand le paramètre d’authentification réseau pour la connexion réseau sans fil est **Authentification de l’utilisateur**.

Pour contourner ce problème, affectez la valeur **Authentification de l’utilisateur ou de l’ordinateur** ou **Authentification de l’ordinateur** au paramètre d’authentification réseau.

 > [!NOTE]  
> Pour modifier les paramètres d’authentification réseau sur un seul ordinateur, vous devrez peut-être utiliser le panneau de configuration Centre réseau et partage afin de créer une nouvelle connexion sans fil avec les nouveaux paramètres.

Pour obtenir une description complète de la façon de configurer les paramètres de réseau sans fil à l’aide d’objets de stratégie de groupe, consultez [Configurer des stratégies de réseau sans fil (IEEE 802.11)](https://docs.microsoft.com/windows-server/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies).

## <a name="user-experiences-poor-performance-or-application-problems"></a>Les performances sont médiocres ou l’utilisateur rencontre des problèmes d’application

Cette section aborde plusieurs problèmes courants que les utilisateurs peuvent rencontrer quand ils utilisent la fonctionnalité Bureau à distance :

  - [Les utilisateurs qui se connectent à de nouvelles machines virtuelles Microsoft Azure rencontrent des problèmes par intermittence](#intermittent-problems-with-new-microsoft-azure-virtual-machines).
  - [Les utilisateurs qui se connectent à des Bureaux à distance Windows 10 version 1709 peuvent rencontrer des problèmes de lecture vidéo](#video-playback-issues-on-windows-10-version-1709).
  - [Les utilisateurs avec des profils utilisateur en lecture seule qui se connectent à des Bureaux à distance Windows 10 ne peuvent pas partager leur Bureau.](#desktop-sharing-issues-on-windows-10)
  - [Les utilisateurs qui se connectent à des Bureaux à distance Windows 10 à l’aide de clients qui s’exécutent sur des versions antérieures de Windows 10 constatent une dégradation des performances quand l’authentification au niveau du réseau est désactivée.](#performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled)
  - [Quand des utilisateurs exécutent plusieurs applications dans des sessions Bureau à distance et qu’ils se déconnectent puis se reconnectent fréquemment, un écran noir peut s’afficher lors de la reconnexion.](#black-screen-issue)

### <a name="intermittent-problems-with-new-microsoft-azure-virtual-machines"></a>Problèmes intermittents avec les nouvelles machines virtuelles Microsoft Azure

Ce problème affecte les machines virtuelles provisionnées récemment. Une fois que l’utilisateur s’est connecté à la machine virtuelle, la session Bureau à distance peut ne pas charger correctement tous les paramètres de l’utilisateur.

Pour contourner ce problème, déconnectez-vous de la machine virtuelle, attendez au moins 20 minutes, puis reconnectez-vous.

Pour résoudre ce problème, appliquez les mises à jour suivantes aux machines virtuelles :

  - Windows 10 et Windows Server 2016 KB 4343884, [30 août 2018—KB4343884 (build du système d’exploitation 14393.2457)](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)
  - Windows Server 2012 R2 : KB 4343891, [30 août 2018—KB4343891 (préversion du correctif cumulatif mensuel)](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)

### <a name="video-playback-issues-on-windows-10-version-1709"></a>Problèmes de lecture vidéo sur Windows 10 version 1709

Ce problème se produit quand des utilisateurs se connectent à des ordinateurs distants qui exécutent Windows 10 version 1709. Quand ces utilisateurs lisent une vidéo à l’aide du codec VMR9 (Video Mixing Renderer 9), le lecteur n’affiche qu’une fenêtre noire.

Il s’agit d’un problème connu dans Windows 10 version 1709. Ce problème ne se produit pas dans Windows 10 version 1703.

### <a name="desktop-sharing-issues-on-windows-10"></a>Problèmes de partage de Bureau sur Windows 10

Ce problème se produit quand l’utilisateur a un profil utilisateur en lecture seule (et une ruche du Registre associée), comme dans un scénario de kiosque. Quand un tel utilisateur se connecte à un ordinateur distant qui exécute Windows 10 version 1803, il ne peut pas partager son Bureau.

Pour résoudre ce problème, appliquez la mise à jour Windows 10 4340917, [24 juillet 2018—KB4340917 (build du système d’exploitation 17134.191)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917).

### <a name="performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled"></a>Problèmes de performances quand vous combinez des versions de Windows 10 si l’authentification au niveau du réseau est désactivée

Ce problème se produit quand l’authentification au niveau du réseau est désactivée et que des ordinateurs clients Bureau à distance qui exécutent Windows 10 se connectent à des Bureaux à distance qui exécutent différentes versions de Windows 10. Les utilisateurs de clients Bureau à distance sur des ordinateurs qui exécutent Windows 10 version 1709 ou version antérieure constatent une dégradation des performances quand ils se connectent à des Bureaux à distance qui exécutent Windows 10 version 1803 ou ultérieure.

Cela se produit car, lorsque l’authentification au niveau du réseau est désactivée, les ordinateurs clients plus anciens utilisent un protocole plus lent quand ils se connectent à Windows 10 version 1803 ou ultérieure.

Pour résoudre ce problème, appliquez le correctif KB 4340917, [24 juillet 2018—KB4340917 (build du système d’exploitation 17134.191)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917).

### <a name="black-screen-issue"></a>Problème d’écran noir

Ce problème se produit dans Windows 8.0, Windows 8.1, Windows 10 RTM et Windows Server 2012 R2. Un utilisateur lance plusieurs applications dans un Bureau à distance, puis se déconnecte de la session. Périodiquement, l’utilisateur se reconnecte au Bureau à distance pour interagir avec les applications, puis il se  déconnecte à nouveau. À un moment donné, quand l’utilisateur se reconnecte, la session Bureau à distance affiche simplement un écran noir. L’utilisateur doit mettre fin à la session Bureau à distance à partir de la console de l’ordinateur distant (ou de la console du serveur RDSH). Cette action arrête les applications.

Pour résoudre ce problème, appliquez les mises à jour suivantes comme il convient :

  - Windows 8 et Windows Server 2012 : KB4103719, [17 mai 2018—KB4103719 (préversion du correctif cumulatif mensuel)](https://support.microsoft.com/en-us/help/4103719/windows-server-2012-update-kb4103719)
  - Windows 8.1 et Windows Server 2012 R2 : KB4103724, [17 mai 2018—KB4103724 (préversion du correctif cumulatif mensuel)](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724) et KB4284863, [21 juin 2018—KB4284863 (préversion du correctif cumulatif mensuel)](https://support.microsoft.com/en-us/help/4284863/windows-81-update-kb4284863)
  - Windows 10 : Résolu dans KB4284860, [le 12 juin 2018 — KB4284860 (Build du système d’exploitation 10240.17889)](https://support.microsoft.com/en-us/help/4284860/windows-10-update-kb4284860)
