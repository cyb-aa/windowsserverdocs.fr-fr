---
title: Résolution des problèmes de connexions Bureau à distance
description: Procédures de dépannage organisés par symptôme
ms.custom: na
ms.reviewer: rklemen;josh.bender
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: jobende
manager: ''
ms.author: v-tea;kaushika;rklemen;josh.bender
ms.date: 02/22/2019
ms.localizationpriority: medium
ms.openlocfilehash: 8965df09fd57decf7f4a1b6b89861e5a67034a0a
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297398"
---
# Résolution des problèmes de connexions Bureau à distance
D’une brève explication des problèmes de Services Bureau à distance (RDS) plus courants, consultez le [Forum aux questions sur les clients Bureau à distance](https://review.docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-client-faq). Cet article décrit plusieurs approches plus avancées de résolution des problèmes de connexion. Nombre de ces procédures s’appliquent si vous rencontrez une configuration simple, tel qu’un ordinateur physique se connectant à un autre ordinateur physique ou une configuration plus complexe. Certaines procédures résoudre les problèmes qui se produisent uniquement dans les scénarios multi-utilisateur plus complexes. Pour plus d’informations sur les composants de bureau à distance et comment elles fonctionnent ensemble, consultez [architecture des Services Bureau à distance](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/desktop-hosting-logical-architecture).

> [!NOTE]  
> La plupart des procédures décrites dans cet article vous oblige à accéder à plusieurs ordinateurs, que certains d'entre eux vous devrez peut-être accéder à distance. Pour plus d’informations sur les outils d’administration à distance et comment les configurer, consultez [Server Administration outils distant (RSAT) pour les systèmes d’exploitation Windows](https://support.microsoft.com/en-us/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems).

Dans la liste suivante, identifiant le type de problème que vous (ou vos utilisateurs) rencontrez.

- [Le client Bureau à distance ne peuvent pas se connecter au Bureau à distance, mais il n’existe aucune symptômes spécifiques ou des messages (étapes de résolution des problèmes généraux)](#no-specific-symptoms-or-messages-general-troubleshooting-steps)
- [Le client Bureau à distance ne peut pas se connecter au Bureau à distance et reçoit un message «Classe non inscrite»](#client-cannot-connect-class-not-registered)
- [Le client Bureau à distance ne peuvent pas se connecter au Bureau à distance et ne reçoit un «aucune licence disponible» ou «erreur de sécurité»](#client-cannot-connect-no-licenses-available)
- [L’utilisateur reçoit un message «Accès refusé» ou doit fournir deux fois les informations d’identification](#user-cannot-authenticate-or-must-authenticate-twice)
- [Sur la connexion, le reçoit un message «Service Bureau à distance est occupé»](#on-connecting-user-receives-remote-desktop-service-is-currently-busy-message)
- [Le client Bureau à distance se déconnecte et ne peut pas se reconnecter à la même session](#rd-client-disconnects-and-cannot-reconnect-to-the-same-session)
- [L’utilisateur se connecte à un ordinateur portable à distance sur un réseau sans fil, et ensuite l’ordinateur portable se déconnecte du réseau](#remote-laptop-disconnects-from-wireless-network).
- [L’utilisateur rencontre une baisse des performances ou des problèmes avec des applications à distance](#user-experiences-poor-performance-or-application-problems)

> [!NOTE]  
> Pour obtenir la liste actuelle des codes de déconnexion RDP, voir [l’énumération ExtendedDisconnectReasonCode](https://docs.microsoft.com/en-us/windows/desktop/TermServ/extendeddisconnectreasoncode). 

## Aucune symptômes spécifiques ou des messages (étapes de résolution des problèmes généraux)

Utilisez ces étapes lorsqu’un client Bureau à distance ne peuvent pas se connecter à un ordinateur de bureau à distance, mais ne fournit pas de messages ou autres problèmes qui permettraient identifient la cause. Pour résoudre la plupart des causes plus courantes de ce type de problème, utilisez les méthodes suivantes:

- [Vérifier l’état du protocole RDP](#check-the-status-of-the-rdp-protocol)
- [Vérifier l’état des services RDP](#check-the-status-of-the-rdp-services)
- [Vérifiez que l’écouteur RDP fonctionne](#check-that-the-rdp-listener-is-functioning)
- [Vérifier le port d’écoute RDP](#check-the-rdp-listener-port)

### Vérifier l’état du protocole RDP

#### Vérifier l’état du protocole RDP sur un ordinateur local

Pour vérifier et modifier l’état du protocole RDP sur un ordinateur local, voir [comment activer le Bureau à distance](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access#how-to-enable-remote-desktop).

> [!NOTE]  
> Si les options de bureau à distance ne sont pas disponibles, consultez [vérifier si un objet de stratégie de groupe bloque RDP](#check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer).

#### Vérifier l’état du protocole RDP sur un ordinateur distant

> [!IMPORTANT]  
> Suivez les étapes décrites dans cette section avec soin. De graves problèmes peuvent se produire si vous modifiez le registre de façon incorrecte. Avant que vous modifiez, [sauvegardez le Registre pour la restauration](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) en cas de problèmes.

Pour vérifier et modifier l’état du protocole RDP sur un ordinateur distant, utilisez une connexion de Registre réseau:

1. Sélectionnez **Démarrer**, sélectionnez **exécuter**, puis entrez **regedt32**.
2. Dans l’Éditeur du Registre, sélectionnez le **fichier**et sélectionnez **Connexion au Registre réseau**.
3. Dans la boîte de dialogue **Sélectionner un ordinateur** , entrez le nom de l’ordinateur distant, sélectionnez **Vérifier les noms**et sélectionnez **OK**.
4. Accédez au **serveur HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal**.  
   ![Éditeur du Registre, montrant l’entrée fDenyTSConnections](..\media\troubleshoot-remote-desktop-connections\RegEntry_fDenyTSConnections.png)
   - Si la valeur de la clé **fDenyTSConnections** est **égal à 0**, RDP est activé.
   - Si la valeur de la clé **fDenyTSConnections** est **1**, RDP est désactivée.
5. Pour activer le protocole RDP, remplacez la valeur de **fDenyTSConnections** de **1** à **0**.

#### Vérifier si un objet de stratégie de groupe (GPO) bloque RDP sur un ordinateur local

Si vous ne pouvez pas activer le protocole RDP dans l’interface utilisateur ou si la valeur de **fDenyTSConnections** revient à **1** , une fois que vous l’avez modifié, un objet de stratégie de groupe peut-être remplacer les paramètres au niveau de l’ordinateur.

Pour vérifier la configuration de stratégie de groupe sur un ordinateur local, ouvrez une fenêtre d’invite de commandes en tant qu’administrateur et entrez la commande suivante:
```
gpresult /H c:\gpresult.html
```
Une fois cette commande est terminée, ouvrez gpresult.html. Dans **Configuration ordinateur\\modèles d’administration\\composants Components\\Remote Bureau à Services\\Remote de la Session Bureau à Host\\Connections**, recherchez la stratégie **Autoriser les utilisateurs à se connecter à distance à l’aide des Services Bureau à distance** .

- Si le paramètre de cette stratégie est **activée**, la stratégie de groupe ne bloque pas les connexions RDP.
- Si le paramètre de cette stratégie est **désactivé**, vérifiez **Gagnante de stratégie de groupe**. Il s’agit de l’objet de stratégie de groupe qui bloque les connexions RDP.
![Un segment de l’exemple de gpresult.html, dans lequel l’objet de stratégie de groupe au niveau du domaine ** bloc RDP ** est la désactivation de RDP.](..\media\troubleshoot-remote-desktop-connections\GPResult_RDSH_Connections_GP.png)
   
  ![Un segment de l’exemple de gpresult.html, dans lequel ** la stratégie de groupe Local ** est la désactivation de RDP.](..\media\troubleshoot-remote-desktop-connections\GPResult_RDSH_Connections_LGP.png)

#### Vérifier si un objet de stratégie de groupe bloque RDP sur un ordinateur distant

Pour vérifier la configuration de la stratégie de groupe sur un ordinateur distant, la commande est presque de la même que celle d’un ordinateur local:
```
gpresult /S <computer name> /H c:\gpresult-<computer name>.html
```
Le fichier que cette commande génère (**gpresult-\<computer name\>.html**) utilise le format des informations de même que la version de l’ordinateur local (**gpresult.html**).

#### Modification d’un blocage de la stratégie de groupe

Vous pouvez modifier ces paramètres dans l’éditeur d’objet de stratégie de groupe (GPE) et la Console de gestion de la stratégie de groupe (GPM). Pour plus d’informations sur l’utilisation de la stratégie de groupe, reportez-vous à la section [Gestion avancée des stratégies de groupe](https://docs.microsoft.com/en-us/microsoft-desktop-optimization-pack/agpm/).

Pour modifier la stratégie de blocage, utilisez l’une des méthodes suivantes:

- Dans GPE, accédez au niveau de stratégie de groupe approprié (par exemple, local ou de domaine) et accédez à **Configuration ordinateur\\modèles d’administration\\composants Components\\Remote Bureau à Services\\Remote de la Session Bureau à Host\\Connections**\\**Autoriser les utilisateurs à se connecter à distance à l’aide des Services Bureau à distance**.  
   1. Définissez la stratégie sur **activé** ou **Non configuré**.
   2. Sur les ordinateurs concernés, ouvrez une fenêtre d’invite de commandes en tant qu’administrateur et exécutez la commande **gpupdate/force** .
- Dans GPM, accédez à l’unité d’organisation dans laquelle la blocage de la stratégie est appliquée sur les ordinateurs concernés et supprimer la stratégie de l’unité d’organisation.

### Vérifier l’état des services RDP

Sur l’ordinateur local (client) et l’ordinateur distant (cible), les services suivants doivent être en cours d’exécution:

- Services Bureau à distance (Terminal Server)
- Redirecteur de Port du mode utilisateur des Services Bureau à distance (UmRdpService)

Vous pouvez utiliser le composant logiciel enfichable MMC Services pour gérer les services localement ou à distance. Vous pouvez également utiliser PowerShell localement ou à distance (si l’ordinateur distant est configuré pour accepter les commandes PowerShell à distance).

![Services Bureau à distance dans le composant logiciel enfichable MMC des Services. Ne modifiez pas les paramètres de service par défaut.](..\media\troubleshoot-remote-desktop-connections\RDSServiceStatus.png)

Sur l’ordinateur, si un ou les deux services ne sont pas en cours d’exécution, les démarrer.

> [!NOTE]  
> Si vous démarrez le service de Services Bureau à distance, cliquez sur **Oui** pour redémarrer automatiquement le service redirecteur de Port en mode utilisateur de Services Bureau à distance.

### Vérifiez que l’écouteur RDP fonctionne

> [!IMPORTANT]  
> Suivez les étapes décrites dans cette section avec soin. De graves problèmes peuvent se produire si vous modifiez le registre de façon incorrecte. Avant que vous modifiez, [sauvegardez le Registre pour la restauration](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) en cas de problèmes.

#### Vérifier l’état de l’écouteur RDP

Pour cette procédure, utilisez une instance de PowerShell qui a des autorisations d’administration. Pour un ordinateur local, vous pouvez également utiliser une invite de commandes qui possède des autorisations administratives. Toutefois, cette procédure utilise PowerShell dans la mesure où les mêmes commandes à la fois localement et à distance.

1. Ouvrez une fenêtre PowerShell. Pour vous connecter à un ordinateur distant, entrez **\<computer name\> Enter-PSSession-ComputerName**.
2. Entrez **qwinsta**. 
    ![La commande qwinsta répertorie les processus à l’écoute sur les ports de l’ordinateur.](..\media\troubleshoot-remote-desktop-connections\WPS_qwinsta.png)
3. Si la liste comprend **rdp-tcp** avec l’état **écouter**, l’écouteur RDP fonctionne. Passez à [vérifier le port d’écoute RDP](#check-the-rdp-listener-port). Sinon, passez à l’étape 4.
4. Exportez la configuration d’écouteur RDP à partir d’un ordinateur de travail.
    1. Se connecter à un ordinateur qui a la même version de système d’exploitation que l’ordinateur concerné a et l’accès de Registre de l’ordinateur (par exemple, en utilisant l’Éditeur du Registre).
    2. Accédez à l’entrée de Registre suivante:  
        **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\RDP-Tcp**
    3. Exporter l’écriture dans un fichier .reg. Par exemple, dans l’Éditeur du Registre, avec le bouton droit de l’entrée, sélectionnez **Exporter**, puis entrez un nom de fichier pour les paramètres exportés.
    4. Copiez le fichier .reg exporté sur l’ordinateur concerné.
5. Pour importer la configuration de l’écouteur RDP, ouvrez une fenêtre PowerShell qui a des autorisations d’administration sur l’ordinateur affecté (ou ouvrez la fenêtre PowerShell et se connecter à distance à l’ordinateur concerné).
   1. Pour sauvegarder l’entrée de Registre existante, entrez la commande suivante:  
   
      ```powershell  
      cmd /c 'reg export "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp" C:\Rdp-tcp-backup.reg'   
      ```  
   

   2. Pour supprimer l’entrée de Registre existante, entrez les commandes suivantes:  
   
      ```powershell  
      Remove-Item -path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp' -Recurse -Force  
      ```
   
   3. Pour importer la nouvelle entrée de Registre, puis redémarrez le service, entrez les commandes suivantes:  
   
      ```powershell  
      cmd /c 'regedit /s c:\<filename>.reg'  
      Restart-Service TermService -Force  
      ```   
      où \<filename\> est le nom du fichier .reg exporté.
6. Testez la configuration en essayant de la connexion Bureau à distance à nouveau. Si vous ne pouvez pas à vous connecter, redémarrez l’ordinateur concerné.
7. Si vous avez toujours ne peut pas se connecter, [Vérifiez l’état du certificat auto-signé RDP](#check-the-status-of-the-rdp-self-signed-certificate).

#### Vérifier l’état du certificat auto-signé RDP

1. Si vous ne pouvez pas à vous connecter, ouvrez le composant logiciel enfichable MMC de certificats. Lorsque vous êtes invité à sélectionner le magasin de certificats pour gérer, sélectionnez le **compte d’ordinateur**et sélectionnez l’ordinateur concerné.
2. Dans le dossier **certificats** sous **Bureau à distance**, supprimez le certificat auto-signé RDP. 
    ![Certificats de bureau à distance dans le composant logiciel enfichable MMC Certificats.](..\media\troubleshoot-remote-desktop-connections\MMCCert_Delete.png)
3. Sur l’ordinateur concerné, redémarrez le service de Services Bureau à distance.
4. Actualisez le composant logiciel enfichable Certificats.
5. Si le certificat auto-signé RDP n’a pas été recréé, [Vérifiez les autorisations du dossier MachineKeys](#check-the-permissions-of-the-machinekeys-folder).

#### Vérifiez les autorisations du dossier MachineKeys

1. Sur l’ordinateur concerné, ouvrez l’Explorateur et accédez à **C:\\ProgramData\\Microsoft\\Crypto\\RSA\\**.
2. Avec le bouton droit **MachineKeys**, sélectionnez **Propriétés**, sélectionnez **la sécurité**et sélectionnez **Avancé**.
3. Assurez-vous que les autorisations suivantes sont configurées:
      - BUILTIN\\Administrateurs: Un contrôle total
      - Tout le monde: Lire, écrire

### Vérifier le port d’écoute RDP

Sur l’ordinateur local (client) et l’ordinateur distant (cible), l’écouteur RDP doit être à l’écoute sur le port 3389. Aucune autre application ne doit utiliser ce port.

> [!IMPORTANT]  
> Suivez les étapes décrites dans cette section avec soin. De graves problèmes peuvent se produire si vous modifiez le registre de façon incorrecte. Avant que vous modifiez, [sauvegardez le Registre pour la restauration](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) en cas de problèmes.

Pour vérifier ou modifier le port du protocole RDP, utilisez l’Éditeur du Registre:

1. Cliquez sur Démarrer, sélectionnez **exécuter**, puis entrez **regedt32**.
      - Pour se connecter à un ordinateur distant, sélectionnez le **fichier**, puis sélectionnez la **Connexion au Registre réseau**.
      - Dans la boîte de dialogue **Sélectionner un ordinateur** , entrez le nom de l’ordinateur distant, sélectionnez **Vérifier les noms**et sélectionnez **OK**.
2. Ouvrez le Registre et accédez à **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<listener\>**. 
    ![La sous-clé de numéro de port pour le protocole RDP.](..\media\troubleshoot-remote-desktop-connections\RegEntry_PortNumber.png)
3. Si le **numéro de port** a une valeur autre que **3389**, modifiez-le **3389**. 
   > [!IMPORTANT]  
    > Vous pouvez utiliser les services Bureau à distance à l’aide d’un autre port. Toutefois, nous ne recommandons pas qui vous faire. Résolution des problèmes d’une telle configuration n’entre pas dans le cadre de cet article.
4. Après avoir modifié le numéro de port, redémarrez le service de Services Bureau à distance.

#### Vérifiez qu’une autre application ne tente pas d’utiliser le même port

Pour cette procédure, utilisez une instance de PowerShell qui a des autorisations d’administration. Pour un ordinateur local, vous pouvez également utiliser une invite de commandes qui possède des autorisations administratives. Toutefois, cette procédure utilise PowerShell dans la mesure où les mêmes commandes localement et à distance.

1. Ouvrez une fenêtre PowerShell. Pour vous connecter à un ordinateur distant, entrez **\<computer name\> Enter-PSSession-ComputerName**.
2. Entrez la commande suivante:  
   
     ```powershell  
    cmd /c 'netstat -ano | find "3389"'  
    ```
  
    ![La commande netstat donne une liste des ports et des services à l’écoute sur ces derniers.](..\media\troubleshoot-remote-desktop-connections\WPS_netstat.png)
1. Recherchez une entrée pour le port TCP 3389 (ou le port RDP assigné) avec l’état **à l’écoute**. 
    > [!NOTE]  
   > Le PID (identificateur de processus) du processus ou du service à l’aide de ce port s’affiche sous la colonne PID.
1. Pour déterminer quelle application utilise le port 3389 (ou le port RDP assigné), entrez la commande suivante:  
   
     ```powershell  
    cmd /c 'tasklist /svc | find "<pid listening on 3389>"'  
    ```  
  
    ![La commande tasklist signale les détails d’un processus spécifique.](..\media\troubleshoot-remote-desktop-connections\WPS_tasklist.png)
1. Recherchez une entrée pour le nombre de PID associé au port (à partir de la sortie **netstat** ). Les services ou les processus associés à ce PID s’affichent sur la droite.
1. Si une application ou un service autre que des Services Bureau à distance (TermServ.exe) utilise le port, vous pouvez résoudre le conflit en utilisant l’une des méthodes suivantes:
      - Configurer l’autre application ou le service pour utiliser un autre port (recommandé).
      - Désinstallez l’autre application ou service.
      - Configurer le protocole RDP pour utiliser un autre port, puis redémarrez le service de Services Bureau à distance (non recommandé).

#### Vérifier si un pare-feu bloque le port RDP

Utilisez l’outil **psping** pour tester si vous pouvez atteindre l’ordinateur affecté à l’aide de port 3389.

1. Sur un ordinateur qui est différent de l’ordinateur concerné, téléchargez **psping** à partir de <https://live.sysinternals.com/psping.exe>.
2. Ouvrez une fenêtre d’invite de commandes en tant qu’administrateur, accédez au répertoire dans lequel vous avez installé **psping**, puis entrez la commande suivante:  
   
   ```  
   psping -accepteula <computer IP>:3389  
   ```
   
3. Vérifier la sortie de la commande **psping** pour afficher les résultats par exemple, les éléments suivants:  
      - **Connexion à \<computer IP\>**: l’ordinateur distant est accessible.
      - **(perte 0 %)**: toutes les tentatives de connexion a réussi.
      - **L’ordinateur distant a refusé la connexion réseau**: l’ordinateur distant n’est pas accessible.
      - **(100 % perte)**: toutes les tentatives de connexion a échoué.
1. Exécutez **psping** sur plusieurs ordinateurs de tester leur capacité à se connecter à l’ordinateur concerné.
1. Notez que si l’ordinateur concerné bloque les connexions à partir de tous les autres ordinateurs, certains autres ordinateurs ou qu’un autre ordinateur.
1. Recommandé les étapes suivantes:
      - Incitez vos administrateurs réseau pour vérifier que le réseau autorise le trafic RDP à l’ordinateur concerné.
      - Examinez les configurations de tous les pare-feu entre les ordinateurs source et de l’ordinateur affecté (y compris le pare-feu Windows sur l’ordinateur concerné) pour déterminer si un pare-feu bloque le port RDP.

## Client ne peut pas se connecter, «Classe non inscrite»

Lorsque vous essayez de vous connecter à un ordinateur distant à l’aide d’un client qui exécute Windows 10, version 1709 ou version ultérieure, le client ne peut pas se connecter tandis que le serveur hôte de Session Bureau à distance signale un message qui contient le code d’erreur «Classe n’est pas inscrit (0 x 80040154)».

Ce problème se produit si l’utilisateur qui essaie de se connecter dispose d’un profil utilisateur obligatoire. Pour résoudre ce problème, installez le [24 juillet 2018 — KB4338817 (16299.579 de Build du système d’exploitation)](https://support.microsoft.com/en-us/help/4338817/windows-10-update-kb4338817) mise à jour Windows 10.

## Client ne peut pas se connecter, aucune licence disponible

Cette situation s’applique aux déploiements qui incluent un serveur d’hôtes de service et un serveur de licences de bureau à distance.

Identifiez le type de comportement, les utilisateurs voient:

- La session a été déconnectée car aucune licence n’est disponible ou aucun serveur de licences n’est disponible
- Accès refusé en raison d’une erreur de sécurité

Se connecter à l’hôte de Session Bureau à distance en tant qu’un administrateur de domaine et ouvrez le Diagnoser de licence des services Bureau à distance. Recherchez les messages tels que les éléments suivants:

  - La période de grâce pour la télécommande serveur hôte de Session bureau a expiré, mais le serveur hôte de Session Bureau à distance n’a pas été configuré avec les serveurs de licence. les connexions au serveur hôte de Session Bureau à distance seront refusées, sauf si un serveur de licences est configuré pour le serveur hôte de Session Bureau à distance.
  - Licence serveur \<computer name\> n’est pas disponible. Cela peut être dû à des problèmes de connectivité réseau, le service de licences de bureau à distance est arrêté sur le serveur de licences ou Gestionnaire de licences des services Bureau à distance n’est plus installé sur l’ordinateur.

Ces problèmes ont tendance à être associées à des messages d’utilisateur suivants:

  - La session à distance a été déconnectée, car aucune licence d’accès client Bureau à distance ne sont disponibles pour cet ordinateur.
  - La session à distance a été déconnectée car aucun serveur de licences bureau à distance fournir une licence.

Dans ce cas, [Configurez le service Gestionnaire de licences des services Bureau à distance](#configure-the-rd-licensing-service).

Si la Diagnoser de licence des services Bureau à distance répertorie les autres problèmes, tels que «le composant X.224 du protocole RDP a détecté une erreur dans le flux du protocole et a déconnecté le client,» il peut être un problème qui a une incidence sur les certificats de licence. Ces problèmes ont tendance à être associées à des messages de l’utilisateur, telles que les éléments suivants:

Le client n’a pas pu se connecter aux services Terminal server en raison d’une erreur de sécurité. Après avoir vérifié que vous êtes connecté au réseau, essayez de vous connecter au serveur à nouveau.

Dans ce cas, les [clés de Registre de certificat actualiser le X509](#refresh-the-x509-certificate-registry-keys).

### Configurer le service Gestionnaire de licences des services Bureau à distance

La procédure suivante utilise le Gestionnaire de serveur pour apporter les modifications de configuration. Pour savoir comment configurer et utiliser le Gestionnaire de serveur, consultez [Le Gestionnaire de serveur](https://docs.microsoft.com/en-us/windows-server/administration/server-manager/server-manager).

1. Ouvrez le Gestionnaire de serveur et accédez à des **Services Bureau à distance**.
2. Sur **Vue d’ensemble du déploiement**, sélectionnez les **tâches**, puis sélectionnez **Modifier les propriétés du déploiement**.
3. Sélectionnez le **Gestionnaire de licences des services Bureau à distance**, puis sélectionnez le mode de licence approprié pour votre déploiement (**Par périphérique** ou **Par utilisateur**).
4. Entrez le nom de domaine complet (FQDN) de votre serveur de licences des services Bureau à distance, puis sélectionnez **Ajouter**.
5. Si vous avez plusieurs serveurs de licence des services Bureau à distance, répétez l’étape 4 pour chaque serveur. 
    ![Options de configuration de serveur licence des services Bureau à distance dans le Gestionnaire de serveur.](..\media\troubleshoot-remote-desktop-connections\RDLicensing_Configure.png)

### Clés de Registre de certificat actualiser le X509

> [!IMPORTANT]  
> Suivez les étapes décrites dans cette section avec soin. De graves problèmes peuvent se produire si vous modifiez le registre de façon incorrecte. Avant que vous modifiez, [sauvegardez le Registre pour la restauration](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) en cas de problèmes.

Pour résoudre ce problème, sauvegarder et clés de Registre du certificat de supprimer le X509, redémarrez l’ordinateur, puis serveur gestionnaire de licences de réactiver le Bureau à distance. Pour ce faire, procédez comme suit.

> [!NOTE]  
> Effectuer la procédure suivante sur chacun des serveurs hôtes de service.

1. Ouvrez l’Éditeur du Registre et accédez à **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\RCM**.
2. Dans le menu du Registre, sélectionnez **Exporter un fichier de Registre**.
3. Tapez **Certificat exporté** dans la zone de **nom de fichier** et sélectionnez **Enregistrer**.
4. Avec le bouton droit à chacune des valeurs suivantes, sélectionnez **Supprimer**et sélectionnez **Oui** pour vérifier la suppression:  
      - **Certificat**
      - **X509 du certificat**
      - **ID du certificat X509**
      - **X509 Certificate2**
5. Quittez l’Éditeur du Registre, puis redémarrez le serveur d’hôtes de service.

## Utilisateur ne peut pas s’authentifier ou doit s’authentifier deux fois

Il existe plusieurs problèmes qui peuvent provoquer des problèmes qui affectent l’authentification des utilisateurs. Cette section traite les cas suivants:

  - [Accès avec un message «restreint type d’ouverture de session» est refusé à un utilisateur](#access-denied-restricted-type-of-logon)
  - [Accès avec l’événement 16965 est refusée à un utilisateur ou une application, «un appel à distance à la base de données SAM a été refusé»](#access-denied-a-remote-call-to-the-sam-database-has-been-denied)
  - [Un utilisateur ne peut pas se connecter à l’aide d’une carte à puce](#a-user-cannot-sign-in-by-using-a-smart-card)
  - [Si l’ordinateur distant est verrouillé, un utilisateur doit entrer un mot de passe deux fois](#if-the-remote-pc-is-locked-a-user-needs-to-enter-a-password-twice)
  - [Un utilisateur ne peut pas se connecter et reçoit «authentification messages d’erreur» et «Mise à jour CredSSP chiffrement oracle»](#user-cannot-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages)
  - [Une fois que vous mettez à jour les ordinateurs clients, certains utilisateurs devront se connecter deux fois](#after-you-update-client-computers-some-users-need-to-sign-in-twice).
  - [Les utilisateurs sont refusés sur un déploiement qui utilise RemoteGuard avec plusieurs agents de connexion Bureau à distance](#users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers)

### Accès refusé, type restreint d’ouverture de session

Dans ce cas, un utilisateur de Windows 10 tente de se connecter à des ordinateurs Windows 10 ou Windows Server 2016 est refusé avec le message suivant:

> Connexion Bureau à distance:  
> L’administrateur système a restreint le type de connexion (réseau ou interactif) que vous pouvez utiliser. Pour obtenir une assistance, contactez votre administrateur système ou le support technique.

Ce problème se produit lorsque l’authentification de niveau réseau (NLA) est requise pour les connexions RDP, et l’utilisateur n’est pas un membre du groupe **d’Utilisateurs du Bureau à distance** . Il peut également se produire si le groupe **d’Utilisateurs du Bureau à distance** n’a pas été attribué à l’utilisateur **d’accéder à cet ordinateur à partir du réseau** approprié.

Pour remédier à ce problème, effectuez l’une des étapes suivantes:

  - [Modifier l’appartenance à un groupe de l’utilisateur ou l’attribution des droits d’utilisateur](#modify-the-users-group-membership-or-user-rights-assignment).
  - Désactiver la fonctionnalité NLA (non recommandé).
  - Utilisez des clients Bureau à distance autres que Windows 10. Par exemple, les clients Windows 7 n’ont pas ce problème.

#### Modifier l’appartenance à un groupe de l’utilisateur ou l’attribution des droits d’utilisateur

Si ce problème concerne un seul utilisateur, la solution la plus simple à ce problème consiste à ajouter l’utilisateur au groupe **d’Utilisateurs du Bureau à distance** .

Si l’utilisateur est déjà un membre de ce groupe (ou si plusieurs membres du groupe ont le même problème), vérifiez la configuration de droits d’utilisateur sur l’ordinateur distant de Windows 10 ou Windows Server 2016.

1. Ouvrez l’éditeur d’objet de stratégie de groupe (GPE) et connectez-vous à la stratégie locale de l’ordinateur distant.
2. Accédez à **l’Attribution des droits d’ordinateur Configuration\\Windows Settings\\Security Settings\\Local Policies\\User**, avec le bouton droit **d’accéder à cet ordinateur à partir du réseau**et sélectionnez **Propriétés**.
3. Vérifier la liste des utilisateurs et groupes **d’Utilisateurs du Bureau à distance** (ou un groupe parent).
4. Si la liste n’inclut pas les **Utilisateurs du Bureau à distance** (ou un groupe parent, par exemple, **tout le monde**) vous devez l’ajouter à la liste (si vous avez deux ou plusieurs ordinateurs dans votre déploiement, utilisez un objet de stratégie de groupe).  
    Par exemple, les membres par défaut pour **accéder à cet ordinateur à partir du réseau** inclut **tout le monde**. Si votre déploiement utilise un objet de stratégie de groupe pour supprimer **tout le monde**, vous devrez peut-être restaurer l’accès en mettant à jour de l’objet de stratégie de groupe pour ajouter des **Utilisateurs du Bureau à distance**.

### Accès refusé, un appel à distance à la base de données SAM a été refusé.

Ce comportement est la plus susceptible de se produire si vos contrôleurs de domaine exécutent Windows Server 2016 ou version ultérieure, et que les utilisateurs tentent de se connecter à l’aide d’une application de connexion personnalisée. En particulier, les applications d’accéder aux informations de profil de l’utilisateur dans Active Directory seront refusées accès.

Ce comportement produit d’un changement de Windows. Dans Windows Server 2012 R2 et versions antérieures versions, lorsqu’un utilisateur se connecte à un ordinateur de bureau, contacts de gestionnaire de connexion à distance (RCM) à distance le contrôleur de domaine (DC) pour interroger les configurations spécifiques au Bureau à distance sur l’objet utilisateur dans le domaine Active Directory Services (AD DS). Ces informations s’affiche dans l’onglet profil de Services Bureau à distance de propriétés de l’objet d’un utilisateur dans les utilisateurs Active Directory et le composant logiciel enfichable MMC ordinateurs.

À compter de Windows Server 2016, RCM interroge n’est plus l’objet d’utilisateurs dans AD DS. Si vous avez besoin RCM d’interroger AD DS, car vous utilisez les attributs de Services Bureau à distance, vous devez activer manuellement le comportement RCM précédent.

> [!IMPORTANT]  
> Suivez les étapes décrites dans cette section avec soin. De graves problèmes peuvent se produire si vous modifiez le registre de façon incorrecte. Avant que vous modifiez, [sauvegardez le Registre pour la restauration](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) en cas de problèmes.

Pour activer le comportement de RCM hérité sur un serveur hôte de Session Bureau à distance, configurez les entrées de Registre suivantes, puis redémarrez le service de **Services Bureau à distance** :  
  - **Services de NT\\Terminal HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Policies\\Microsoft\\Windows**
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<Winstation name\>\\**  
      - Nom: **fQueryUserConfigFromDC**
      - Type: **Reg\_DWORD**
      - Valeur: **1** (décimal)

Pour activer le comportement de RCM hérité sur un serveur autre qu’un serveur hôte de Session Bureau à distance, configurer ces entrées de Registre et l’entrée de Registre supplémentaires suivantes (, puis redémarrez le service):
  - **Serveur HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal**

Pour plus d’informations sur ce comportement, consultez Ko 3200967 [modifications au Gestionnaire des connexions à distance dans Windows Server](https://support.microsoft.com/en-us/help/3200967/changes-to-remote-connection-manager-in-windows-server).

### Un utilisateur ne peut pas se connecter à l’aide d’une carte à puce

Cet article traite des trois dans de rares cas courants dans lequel un utilisateur ne peut pas se connecter à un ordinateur de bureau à distance à l’aide d’une carte à puce:  
  - [L’utilisateur ne peut pas se connecter à une filiale qui utilise un en lecture seule domaine contrôleur (contrôleur)](#cannot-sign-in-with-a-smart-card-in-a-branch-office-with-a-rodc)
  - [L’utilisateur ne peut pas se connecter à un ordinateur Windows Server 2008 SP2 qui a été récemment mis à jour](#cannot-sign-in-with-a-smart-card-to-a-windows-server-2008-sp2-computer)
  - [L’utilisateur ne peut pas rester connecté à un ordinateur Windows qui a été récemment mis à jour et les services Bureau à distance qui ne répond pas (se bloque)](#cannot-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs)

#### Impossible de se connecter avec une carte à puce dans une filiale avec un contrôleur

Ce problème se produit dans les déploiements qui incluent un serveur d’hôtes de service sur un site de branche qui utilise un contrôleur. Le serveur d’hôtes de service est hébergé dans le domaine racine. Les utilisateurs sur le site de branche appartiennent à un domaine enfant et utilisent des cartes à puce pour l’authentification. Le contrôleur est configurée pour les mots de passe utilisateur cache (le contrôleur auquel appartient le **Groupe de réplication du mot de passe de contrôleur autorisé**). Lorsque les utilisateurs tentent de se connecter à des sessions sur le serveur d’hôtes de service, ils reçoivent des messages tels que «l’ouverture de session tentée n’est pas valide. Il s’agit soit en raison d’un nom d’utilisateur ou l’authentification des informations incorrectes.»

Il s’agit d’un problème connu de la manière qui la racine du contrôleur de domaine et le contrôleur gérer le chiffrement des informations d’identification de l’utilisateur. La racine du contrôleur de domaine utilise une clé de chiffrement pour chiffre les informations d’identification, et le type de contrôleur fournit une clé de déchiffrement au client. Toutefois, les deux clés ne correspondent pas.

Pour contourner ce problème, envisagez de modifier votre topologie de contrôleur de domaine en désactivant la mise en cache du mot de passe sur le contrôleur ou en déployant un contrôleur de domaine accessible en écriture sur le site de succursale. Une autre méthode consiste à déplacer le serveur d’hôtes de service sur le même domaine enfant en tant que les utilisateurs. Une autre solution consiste à permettre aux utilisateurs de se connecter sans les cartes à puce. Une de ces solutions peuvent nécessiter des compromis de performances ou du niveau de sécurité.

#### Impossible de se connecter avec une carte à puce sur un ordinateur Windows Server 2008 SP2

Ce problème se produit lorsque les utilisateurs de se connecter à un ordinateur Windows Server 2008 SP2 qui a été mis à jour avec KB4093227 (2018.4B). Lorsque les utilisateurs tentent de se connecter à l’aide d’une carte à puce, il sont refusées avec des messages tels que «aucun certificat valide trouvé. Vérifiez que la carte est insérée correctement et s’adapte étroitement.» En même temps, l’ordinateur Windows Server enregistre l’événement d’Application «une erreur s’est produite lors de la récupération d’un certificat numérique à partir de la carte à puce. Signature non valide.»

Pour résoudre ce problème, vous devez mettre à jour l’ordinateur de Windows Server avec le 2018.06Bre-version de 4093227 Ko, [Description de la mise à jour de sécurité pour Windows protocole RDP (Remote Desktop) vulnérabilité de déni de service dans Windows Server 2008: 10 avril 2018](https://support.microsoft.com/en-us/help/4093227/security-update-for-vulnerabilities-in-windows-server-2008).

#### Ne peut pas rester connecté avec une carte à puce et de blocages de service de Services Bureau à distance

Ce problème se produit lorsque les utilisateurs de se connecter à un ordinateur Windows ou Windows Server qui a été mis à jour avec 4056446 Ko. Dans un premier temps, l’utilisateur peut être en mesure de se connecter au système à l’aide d’une carte à puce, mais puis reçoit un message d’erreur «SCARD\_E\_NO\_SERVICE». L’ordinateur distant peut devenir ne répond pas.

Pour contourner ce problème, redémarrez l’ordinateur distant.

Pour résoudre ce problème, mettez à jour l’ordinateur distant avec le correctif approprié:

  - Windows Server 2008 SP2: Ko 4090928, [Windows les fuites de handles dans le processus lsm.exe et applications de carte à puce peuvent afficher les erreurs de «SCARD\_E\_NO\_SERVICE»](https://support.microsoft.com/en-us/help/4090928/scard-e-no-service-errors-when-windows-leaks-handles-in-the-lsm-exe)
  - Windows Server 2012 R2: Ko 4103724, [17 mai 2018 — KB4103724 (version d’évaluation de correctif cumulatif mensuel)](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724)
  - Windows Server 2016 et Windows 10, version 1607: 4103720 Ko, [17 mai 2018 — KB4103720 (14393.2273 de Build du système d’exploitation)](https://support.microsoft.com/en-us/help/4103720/windows-10-update-kb4103720)

### Si l’ordinateur distant est verrouillé, un utilisateur doit entrer un mot de passe deux fois

Ce problème peut se produire lorsqu’un utilisateur tente de se connecter à un bureau distant qui exécute Windows 10 version 1709 dans un déploiement dans lequel les connexions RDP ne nécessitent pas NLA. Dans ces conditions, si le Bureau à distance a été verrouillé, l’utilisateur doit entrer ses informations d’identification deux fois lors de la connexion.

Pour résoudre ce problème, mettez à jour de l’ordinateur de la version 1709 Windows 10 avec 4343893 Ko, [30 août 2018 — KB4343893 (16299.637 de Build du système d’exploitation)](https://support.microsoft.com/en-us/help/4343893/windows-10-update-kb4343893).

### Utilisateur ne peut pas se connecter et reçoit «authentification messages d’erreur» et «Mise à jour CredSSP chiffrement oracle»

Lorsque les utilisateurs tentent de se connecter à l’aide de n’importe quelle version de Windows à partir de Windows Vista SP2 et versions ultérieures ou Windows Server 2008 SP2 et versions ultérieures, ils l’accès sont refusés et recevoir des messages tels que les éléments suivants:

  - Une erreur d’authentification s’est produite. La fonction demandée n’est pas pris en charge.
  - Il peut s’agir en raison de la mise à jour CredSSP chiffrement oracle

«Mise à jour CredSSP chiffrement oracle» fait référence à un ensemble de mises à jour de sécurité publiée en mars, avril et mai de 2018. CredSSP est un fournisseur d’authentification qui traite les demandes d’authentification pour d’autres applications. Le mois de mars 13 2018, 3 «b» et les mises à jour ultérieures adressé une attaque du mécanisme dans lequel une personne malveillante pourrait transmettre des informations d’identification de l’utilisateur pour exécuter du code sur le système cible.

Les mises à jour initiales ajouté la prise en charge pour un nouvel objet de stratégie de groupe, **Mise à jour de chiffrement Oracle**, qui comporte les paramètres possibles suivants:

  - **Vulnérable**. Les applications clientes qui utilisent CredSSP peuvent revenir aux versions non sécurisées, mais ce comportement expose les ordinateurs de bureau à distance à des attaques. Les services qui utilisent CredSSP acceptent des clients qui n’ont pas été mis à jour.
  - **Atténué**. Applications clientes qui utilisent CredSSP ne peuvent pas revenir aux versions non sécurisées, mais les services qui utilisent CredSSP l’acceptation des clients qui n’ont pas été mis à jour.
  - **Force mis à jour les Clients**. Applications clientes qui utilisent CredSSP ne peuvent pas revenir aux versions non sécurisées, et les services qui utilisent CredSSP n’acceptent pas les clients non corrigés. 
    Remarque: Ce paramètre n’a pas doit être déployé jusqu'à ce que tous les hôtes distants prennent en charge la version la plus récente.

La mise à jour de 8 mai 2018, modifié le paramètre de **Chiffrement Oracle correction** par défaut à partir de **vulnérable** à **atténué**. Avec cette modification en place, les clients Bureau à distance qui ont les mises à jour ne peuvent pas se connecter aux serveurs qui n’ont pas leur (ou mise à jour des serveurs qui n’ont pas été redémarrés). Pour plus d’informations sur les effets des mises à jour et les types de communication qui ils bloquent, voir 4093492 Ko, [CredSSP mises à jour pour CVE-2018-0886](https://support.microsoft.com/en-us/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018).

Pour résoudre ce problème, vérifiez que tous les systèmes sont entièrement mis à jour et redémarrés. Pour obtenir la liste complète des mises à jour et plus d’informations sur les vulnérabilités, consultez [CVE-2018-0886 | Vulnérabilité d’exécution de Code à distance CredSSP](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-0886).

Pour contourner ce problème jusqu'à ce que les mises à jour sont terminées, vérifiez 4093492 Ko pour les types de connexions autorisés. S’il n’existe aucune alternatives envisageable, vous pouvez envisager une des méthodes suivantes:

  - Pour les ordinateurs clients affectés, définissez la stratégie de **Mise à jour de chiffrement Oracle** vers **vulnérable**.
  - Modifiez les stratégies suivantes dans le dossier de la stratégie de groupe **Configuration ordinateur\\modèles d’administration\\composants Components\\Remote Bureau à Services\\Remote de la Session Bureau à Host\\Security** :  
      - **Exiger l’utilisation de la couche de sécurité spécifiques pour les connexions à distance (RDP)**: défini sur **Enabled** et sélectionnez **RDP**.
      - **Exiger l’authentification utilisateur pour les connexions à distance à l’aide de l’authentification au niveau du réseau**: défini sur **désactivé**.
      > [!IMPORTANT]  
      > Ces modifications réduisent la sécurité de votre déploiement. Ils ne doivent être temporaires, si vous les utilisez du tout.

Pour plus d’informations sur l’utilisation de la stratégie de groupe, consultez [modification d’un blocage de la stratégie de groupe](#modifying-a-blocking-gpo).

### Une fois que vous mettez à jour les ordinateurs clients, certains utilisateurs devront se connecter deux fois

Les utilisateurs sur certaines versions de Windows 7 ou Windows 10 1709 se connecter à une session Bureau à distance et d’afficher immédiatement un deuxième écran de connexion. Ce problème se produit si les ordinateurs clients ont reçu les mises à jour suivantes:

  - Windows 7: Ko 4103718, [8 mai 2018 — KB4103718 (correctif cumulatif mensuel)](https://support.microsoft.com/en-us/help/4103718/windows-7-update-kb4103718)
  - Windows 10 version 1709: Ko 4103727, [8 mai 2018 — KB4103727 (Build du système d’exploitation 16299.431)](https://support.microsoft.com/en-us/help/4103727/windows-10-update-kb4103727)

Pour résoudre ce problème, assurez-vous que les ordinateurs que les utilisateurs souhaitent se connecter au (, ainsi que les serveurs hôtes de service ou RDVI) sont entièrement mis à jour par le biais de juin 2018. Cela inclut les mises à jour suivantes:

  - Windows Server 2016: Ko 4284880, [12 juin 2018, KB4284880 (Build du système d’exploitation 14393.2312)](https://support.microsoft.com/en-us/help/4284880/windows-10-update-kb4284880)
  - Windows Server 2012 R2: Ko 4284815, [12 juin 2018, KB4284815 (correctif cumulatif mensuel)](https://support.microsoft.com/en-us/help/4284815/windows-81-update-kb4284815)
  - Windows Server 2012: Ko 4284855, [12 juin 2018, KB4284855 (correctif cumulatif mensuel)](https://support.microsoft.com/en-us/help/4284855/windows-server-2012-update-kb4284855)
  - Windows Server 2008 R2: Ko 4284826, [12 juin 2018, KB4284826 (correctif cumulatif mensuel)](https://support.microsoft.com/en-us/help/4284826/windows-7-update-kb4284826)
  - Windows Server 2008 SP2: KB4056564, [Description de la mise à jour de sécurité pour la vulnérabilité de l’exécution de code à distance CredSSP dans Windows Server 2008, Windows Embedded POSReady 2009 et 2009 Standard incorporé Windows: 13 mars 2018](https://support.microsoft.com/en-us/help/4056564/security-update-for-vulnerabilities-in-windows-server-2008)

### Les utilisateurs sont refusés sur un déploiement qui utilise Credential Guard à distance avec plusieurs services Bureau à distance connexions Bureau à distance

Ce problème se produit dans les déploiements de haute disponibilité qui utilisent les deux ou plusieurs à distance Bureau connexions Bureau à distance, si Windows Defender Credential Guard à distance est en cours d’utilisation. Les utilisateurs ne peuvent pas se connecter à des bureaux distants.

Ce problème se produit car Credential Guard à distance utilise Kerberos pour l’authentification et limite les NTLM. Toutefois, dans une configuration haute disponibilité avec équilibrage de charge, le Bureau à distance connexions Bureau à distance ne peut pas prendre en charge Kerberos opérations.

Si vous avez besoin d’utiliser une configuration de haute disponibilité avec équilibrage de charge Bureau à distance connexions Bureau à distance, vous pouvez contourner ce problème en désactivant la Credential Guard à distance. Pour plus d’informations sur la façon de gérer Windows Defender Credential Guard à distance, consultez les [informations d’identification de protéger les services Bureau à distance avec Windows Defender Credential Guard à distance](https://docs.microsoft.com/en-us/windows/security/identity-protection/remote-credential-guard#enable-windows-defender-remote-credential-guard).

## Sur la connexion, utilisateur reçoit le message «Service Bureau à distance est occupé»

Pour déterminer une réponse appropriée à ce problème, procédez comme suit:

1. Est le service de Services Bureau à distance ne répond pas (par exemple, le client Bureau à distance s’affiche «bloque» à l’écran d’accueil).  
      - Si le service ne répond pas, voir le [problème de mémoire de serveurs hôtes de service](#rdsh-server-memory-issue).
      - Si le client s’affiche pour interagir avec le service normalement, passez à l’étape suivante.
2. Si un ou plusieurs utilisateurs déconnectent leurs sessions de bureau à distance, peuvent les utilisateurs se connecter à nouveau?  
   - Si le service continue de refuser les connexions, quel que soit le nombre d’utilisateurs déconnecter des sessions sur leurs, voir [problème écouteur de bureau à distance](#rd-listener-issue).
   - Si le service commence à accepter des connexions à nouveau après un nombre d’utilisateurs ont leurs sessions déconnectées, [Vérifiez la stratégie de limite de connexion](#check-the-connection-limit-policy).

### Problème de mémoire de serveurs hôtes de service

Une fuite de mémoire a été trouvée sur certains serveurs hôtes de service Windows Server 2012 R2. Au fil du temps, ces serveurs commencent à refuser les connexions Bureau à distance et les connexions console locale des messages tels que les éléments suivants:

> Impossible de terminer la tâche que vous essayez de faire dans la mesure où le Service de bureau à distance est actuellement occupé. Veuillez réessayer dans quelques minutes. Autres les utilisateurs doivent toujours être en mesure de se connecter.

Les clients Bureau à distance tente de se connecter également devient instable.

Pour contourner ce problème, redémarrez le serveur d’hôtes de service.

Pour résoudre ce problème, appliquez 4093114 Ko, [10 avril 2018, KB4093114 (correctif cumulatif mensuel)] (file:///C:\\Users\\v-jesits\\AppData\\Local\\Microsoft\\Windows\\INetCache\\Content.Outlook\\FUB8OO45\\April%2010,%202018—KB4093114%20\(Monthly% 20Rollup\)), aux serveurs hôtes de service.

### Problème écouteur de bureau à distance

Un problème a été observé sur certains serveurs hôtes de service qui ont été mis à niveau directement à partir de Windows Server 2008 R2 vers Windows Server 2012 R2 ou Windows Server 2016. Lorsqu’un client Bureau à distance se connecte au serveur d’hôtes de service, le serveur d’hôtes de service crée un écouteur de bureau à distance pour la session de l’utilisateur. Les serveurs concernés conservent un décompte des écouteurs de bureau à distance augmente à mesure que les utilisateurs se connectent, mais jamais diminue.

Pour contourner ce problème, vous pouvez utiliser les méthodes suivantes:

  - Redémarrez le serveur d’hôtes de service pour réinitialiser le nombre d’écouteurs de bureau à distance
  - Modifier la stratégie de limite de connexion, la définition d’une valeur très volumineux. Pour plus d’informations sur la gestion de la stratégie de limite de connexion, voir [vérifier la stratégie de limite de connexion](#check-the-connection-limit-policy).

Pour résoudre ce problème, appliquez les mises à jour suivantes pour les serveurs hôtes de service:

  - Windows Server 2012 R2: Ko 4343891, [30 août 2018 — KB4343891 (version d’évaluation de correctif cumulatif mensuel)](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)
  - Windows Server 2016: Ko 4343884, [30 août 2018 — KB4343884 (Build du système d’exploitation 14393.2457)](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)

### Vérifiez la stratégie de limite de connexion

Vous pouvez définir la limite du nombre de connexions Bureau à distance simultanées au niveau de l’ordinateur individuel ou en configurant un objet de stratégie de groupe (GPO). Par défaut, la limite n’est pas définie.

Pour vérifier les paramètres actuels et identifier les objets de stratégie de groupe existants sur le serveur d’hôtes de service, ouvrez une fenêtre d’invite de commandes en tant qu’administrateur et entrez la commande suivante:
  
```
gpresult /H c:\gpresult.html
```
   
Une fois cette commande est terminée, ouvrez gpresult.html et dans **Configuration ordinateur\\modèles d’administration\\composants Components\\Remote Bureau à Services\\Remote de la Session Bureau à Host\\Connections**, recherchez le **limiter le nombre de connexions** stratégie.

  - Si le paramètre de cette stratégie est **désactivé**, la stratégie de groupe est limitation pas de connexions RDP.
  - Si le paramètre de cette stratégie est **activé**, puis vérifiez **Gagnante de stratégie de groupe**. Si vous avez besoin supprimer ou modifier la limite de connexion, modifiez cet objet de stratégie de groupe.

Pour appliquer les modifications dans la stratégie, ouvrez une fenêtre d’invite de commandes sur l’ordinateur concerné et entrez la commande suivante:
  
```
gpupdate /force
```
  
## Client Bureau à distance se déconnecte et ne peut pas se reconnecter à la même session

Une fois que le client Bureau à distance perd sa connexion avec le Bureau à distance, le client ne peut pas se reconnecter immédiatement. L’utilisateur reçoit des messages d’erreur tels que les éléments suivants:

  - Le client n’a pas pu se connecter aux services Terminal server en raison d’une erreur de sécurité. Après avoir vérifié que vous êtes connecté au réseau, essayez de vous connecter au serveur à nouveau.
  - Services Bureau à distance est déconnectée. En raison d’une erreur de sécurité, le client n’a pas pu se connecter à l’ordinateur distant. Vérifiez que vous êtes connecté au réseau et essayez de vous reconnecter.

À un moment ultérieur, le client Bureau à distance se reconnecte pour le Bureau à distance. Toutefois, au lieu de connexion du client à la session d’origine, le serveur d’hôtes de service connecte le client à une nouvelle session. Le serveur d’hôtes de service révèle que la session d’origine n’avez pas entré un état déconnecté, mais au lieu de cela reste actif.

Pour contourner ce problème, vous pouvez activer la stratégie **d’intervalle de persistance de connexion de configuration** dans la configuration ordinateur\\modèles d’administration\\composants Components\\Remote Bureau à Services\\Remote de la Session Bureau à Host\\Connections ** **dossiers de stratégie de groupe. Si vous activez cette stratégie, vous devez entrer un intervalle de persistance. L’intervalle de persistance détermine la fréquence à laquelle, en minutes, le serveur vérifie l’état de session.

Ce problème peut être dû à une configuration incorrecte de vos paramètres de configuration et d’authentification. Vous pouvez configurer ces paramètres au niveau du serveur, ou à l’aide de stratégie de groupe. Les paramètres de stratégie de groupe sont disponibles dans le dossier de la stratégie de groupe **Configuration ordinateur\\modèles d’administration\\composants Components\\Remote Bureau à Services\\Remote de la Session Bureau à Host\\Security** .

1. Sur le serveur hôte de Session Bureau à distance, ouvrez la Configuration de l’hôte de la Session Bureau à distance.
2. Sous **connexions**, double-cliquez sur le nom de la connexion, puis cliquez sur **Propriétés**.
3. Dans la boîte de dialogue des **Propriétés** de la connexion, sous l’onglet **Général** , dans la couche de **sécurité** , sélectionnez une méthode de sécurité.
4. Dans le **niveau de chiffrement**, cliquez sur le niveau que vous souhaitez. Vous pouvez sélectionner **faible**, **Compatible avec le Client**, **élevé**ou **Compatible FIPS**.

> [!NOTE]  
>  - Lorsque les communications entre les clients et serveurs hôtes de Session Bureau à distance exigent le niveau le plus élevé de chiffrement, utilisez le chiffrement compatible FIPS.
>  - Les paramètres correspondants de chiffrement que vous configurez la stratégie de groupe remplacent la configuration que vous définissez à l’aide de l’outil de Configuration de Services Bureau à distance. En outre, si vous activez la [Cryptographie système: algorithmes compatibles FIPS d’utilisation pour le chiffrement, le hachage et la signature](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc780081\(v=ws.10\)) la stratégie, ce paramètre remplace la stratégie de **définir le niveau de chiffrement de connexion client** . La stratégie de chiffrement système se trouve dans le dossier **Ordinateur Configuration\\Windows Settings\\Security Settings\\Local Policies\\Security Options** .
>  - Lorsque vous modifiez le niveau de chiffrement, le nouveau niveau de chiffrement prend effet la prochaine fois qu’un utilisateur se connecte. Si vous avez besoin de plusieurs niveaux de chiffrement sur un serveur, installez plusieurs cartes réseau et configurez chaque carte séparément.
>  - Pour vérifier que le certificat présente une clé privée correspondante, dans la Configuration de Services Bureau à distance, cliquez sur la connexion pour laquelle vous souhaitez afficher le certificat, sélectionnez **Général**, sélectionnez **Modifier**, sélectionnez le certificat que vous souhaitez afficher et sélectionnez **Afficher le certificat**. En bas de l’onglet **Général** , l’instruction, «vous avez une clé privée qui correspond à ce certificat» doit s’afficher. Vous pouvez également afficher ces informations à l’aide du composant logiciel enfichable Certificats.
>  - Chiffrement compatible FIPS (le **Cryptographie système: algorithmes compatibles FIPS d’utilisation pour le chiffrement, le hachage et la signature** stratégie ou le paramètre de Configuration de serveur de bureau à distance **Compatible FIPS** ) chiffre et déchiffre les données envoyées à partir de la client au serveur et à partir du serveur au client, avec les algorithmes de chiffrement 140-1 FIPS Federal Information Processing Standard (), à l’aide des modules de chiffrement de Microsoft. Pour plus d’informations, voir [FIPS 140 Validation](https://docs.microsoft.com/en-us/windows/security/threat-protection/fips-140-validation).
>  - Le paramètre **élevé** chiffre les données envoyées à partir du client vers le serveur et à partir du serveur au client à l’aide de cryptage 128 bits.
>  - Le paramètre **Compatible avec le Client** chiffre les données envoyées entre le client et le serveur à la puissance maximale prise en charge par le client.
>  - Le paramètre de **faible** chiffre les données envoyées à partir du client au serveur à l’aide du chiffrement 56 bits.

## Un ordinateur portable à distance se déconnecte réseau sans fil

Ce problème peut se produire lorsqu’un client Bureau à distance se connecte à un ordinateur portable à l’aide d’un réseau sans fil 802. 1 x. Par intermittence, l’ordinateur portable se déconnecte du réseau sans fil et ne pas reconnecter automatiquement.

Il s’agit d’un problème connu qui se produit lorsque le paramètre d’authentification réseau pour la connexion réseau sans fil est **l’authentification des utilisateurs**.

Pour contourner ce problème, définissez le paramètre d’authentification réseau pour **l’authentification utilisateur ou un ordinateur** ou **d’authentification de l’ordinateur**.

 > [!NOTE]  
> Pour modifier les paramètres d’authentification réseau sur un seul ordinateur, vous devrez peut-être utiliser le panneau de configuration Centre réseau et partage pour créer une nouvelle connexion sans fil avec les nouveaux paramètres.

Pour obtenir une description complète de la configuration des paramètres de réseau sans fil à l’aide de la stratégie de groupe, voir [les stratégies de configurer le réseau sans fil (IEEE 802.11)](https://docs.microsoft.com/en-us/windows-server/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies).

## Expériences utilisateur des performances médiocres ou des problèmes d’application

Cette section aborde les plusieurs problèmes courants que les utilisateurs peuvent rencontrer lorsqu’ils utilisent des fonctionnalités de bureau à distance:

  - [Les utilisateurs qui se connectent à nouvelles machines virtuelles Microsoft Azure par intermittence rencontrent des problèmes](#intermittent-problems-with-new-microsoft-azure-virtual-machines).
  - [Les utilisateurs qui se connectent à des bureaux distants de Windows 10 version 1709 peuvent avoir des problèmes de lecture vidéo](#video-playback-issues-on-windows-10-version-1709).
  - [Les utilisateurs avec des profils utilisateur en lecture seule qui se connectent aux ordinateurs de bureau à distance de Windows 10 ne peuvent pas partager leurs postes de travail.](#desktop-sharing-issues-on-windows-10)
  - [Utilisateurs qui se connectent aux ordinateurs de bureau à distance Windows 10 à l’aide de clients qui s’exécutent sur des versions antérieures de Windows 10 rencontrent des performances médiocres lorsque NLA est désactivé.](#performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled)
  - [Lorsque les utilisateurs exécutent plusieurs applications dans des sessions de bureau à distance et se déconnecter et reconnecter fréquemment, ils peuvent rencontrer un écran noir sur la reconnexion.](#black-screen-issue)

### Problèmes par intermittence avec les machines virtuelles Microsoft Azure

Ce problème a une incidence sur les ordinateurs virtuels qui ont été récemment configurés. Une fois que l’utilisateur se connecte à la machine virtuelle, la session Bureau à distance ne peut pas charger tous les paramètres de l’utilisateur correctement.

Pour contourner ce problème, se déconnecter à partir de la machine virtuelle, attendez au moins 20 minutes, puis connectez-vous à nouveau.

Pour résoudre ce problème, appliquez les mises à jour suivantes pour les machines virtuelles, selon le cas:

  - Windows 10 et Windows Server 2016: Ko 4343884, [30 août 2018 — KB4343884 (Build du système d’exploitation 14393.2457)](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)
  - Windows Server 2012 R2: Ko 4343891, [30 août 2018 — KB4343891 (version d’évaluation de correctif cumulatif mensuel)](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)

### Problèmes de lecture vidéo sur Windows 10 version 1709

Ce problème se produit lorsque les utilisateurs se connectent à des ordinateurs distants qui exécutent Windows 10, version 1709. Lorsque ces utilisateurs lire du contenu vidéos à l’aide de la VMR9 codec (MixingRenderer9 vidéo), le joueur n'indique qu’une fenêtre de noire.

Il s’agit d’un problème connu dans Windows 10, version 1709. Le problème ne se produit pas dans Windows 10 version 1703.

### Problèmes sur Windows 10 de partage de bureau

Ce problème se produit lorsque l’utilisateur a un profil utilisateur en lecture seule (et la ruche du Registre associé), comme dans un scénario kiosque. Lorsqu’un utilisateur se connecte à un ordinateur distant qui exécute Windows 10, version 1803, ils ne peuvent pas partager son bureau.

Pour résoudre ce problème, appliquer la mise à jour Windows 10 4340917, [24 juillet 2018 — KB4340917 (17134.191 de Build du système d’exploitation)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917).

### Problèmes de performances lorsque vous mélangez des versions de Windows 10 si NLA est désactivée

Ce problème se produit lorsque l’authentification NLA est désactivé, et les ordinateurs client Bureau à distance qui exécutent Windows 10 se connectent à des bureaux distants qui exécutent les différentes versions de Windows 10. Les utilisateurs de clients Bureau à distance sur les ordinateurs qui exécutent Windows 10, version 1709 ou versions antérieures rencontrer des performances médiocres lorsqu’ils se connectent à des bureaux distants qui exécutent Windows 10, version 1803 ou une version ultérieure.

Cela se produit car, lorsque l’authentification NLA est désactivée, les anciens ordinateurs clients utilisent un protocole plus lent lorsqu’ils se connectent à Windows 10, version 1803 ou une version ultérieure.

Pour résoudre ce problème, appliquez 4340917 Ko, [24 juillet 2018 — KB4340917 (17134.191 de Build du système d’exploitation)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917).

### Problème d’écran noir

Ce problème se produit dans Windows 8.0, Windows 8.1, Windows 10 RTM et Windows Server 2012 R2. Un utilisateur lance plusieurs applications dans un ordinateur de bureau à distance, puis se déconnecte la session. Régulièrement, l’utilisateur se reconnecte pour le Bureau à distance et interagir avec les applications, puis déconnectez-vous à nouveau. À un moment donné, lorsque l’utilisateur se reconnecte, la session Bureau à distance affiche simplement un écran noir. L’utilisateur doit se terminer la session Bureau à distance à partir de la console de l’ordinateur distant (ou la console de serveurs hôtes de service). Cette action empêche les applications.

Pour résoudre ce problème, appliquez les mises à jour suivantes selon le cas:

  - Windows 8 et Windows Server 2012: KB4103719, [17 mai 2018 — KB4103719 (version d’évaluation de correctif cumulatif mensuel)](https://support.microsoft.com/en-us/help/4103719/windows-server-2012-update-kb4103719)
  - Windows 8.1 et Windows Server 2012 R2: KB4103724, [17 mai 2018 — KB4103724 (version d’évaluation de correctif cumulatif mensuel)](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724) et Ko 4284863, [21 juin 2018, KB4284863 (version d’évaluation de correctif cumulatif mensuel)](https://support.microsoft.com/en-us/help/4284863/windows-81-update-kb4284863)
  - Windows 10: Résolu dans KB4284860, [12 juin 2018, KB4284860 (Build du système d’exploitation 10240.17889)](https://support.microsoft.com/en-us/help/4284860/windows-10-update-kb4284860)
