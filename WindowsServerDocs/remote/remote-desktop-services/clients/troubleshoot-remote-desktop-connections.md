---
title: Résolution des problèmes de connexion Bureau à distance
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
ms.openlocfilehash: 4bbdd17f5e6e2b161e0dda0e172ea862a9107841
ms.sourcegitcommit: 564158d760f902ced7f18e6d63a9daafa2a92bd4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2019
ms.locfileid: "64988334"
---
# <a name="troubleshooting-remote-desktop-connections"></a>Résolution des problèmes de connexion Bureau à distance
Pour brèves explications sur certains des problèmes de Services Bureau à distance (RDS) plus courants, consultez [Forum aux questions sur les clients Bureau à distance](https://review.docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-client-faq). Cet article décrit plusieurs approches plus avancées de résolution des problèmes de connexion. Bon nombre de ces procédures s’appliquent si vous dépannez une configuration simple, tel qu’un seul ordinateur physique se connecter à un autre ordinateur physique ou une configuration plus complexe. Certaines procédures résoudre les problèmes qui se produisent uniquement dans des scénarios plus compliqués multi-utilisateur. Pour plus d’informations sur les composants de bureau à distance et comment ils fonctionnent ensemble, consultez [architecture des Services Bureau à distance](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/desktop-hosting-logical-architecture).

> [!NOTE]  
> La plupart des procédures décrites dans cet article vous obligent à accéder à plusieurs ordinateurs, certains d'entre eux que vous deviez accéder à distance. Pour plus d’informations sur les outils d’administration à distance et comment les configurer, consultez [Server Administration outils distant (RSAT) pour les systèmes d’exploitation Windows](https://support.microsoft.com/en-us/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems).

Dans la liste suivante, identifiez le type de problème que vous (ou vos utilisateurs) rencontrez.

- [Le client Bureau à distance ne peut pas se connecter au Bureau à distance, mais aucun symptômes spécifiques ou les messages (étapes de dépannage générales)](#no-specific-symptoms-or-messages-general-troubleshooting-steps)
- [Le client Bureau à distance ne peut pas se connecter au Bureau à distance et reçoit un message « Classe non enregistrée »](#client-cannot-connect-class-not-registered)
- [Le client Bureau à distance ne peut pas se connecter au Bureau à distance et ne reçoit un « aucune licence disponible » ou « erreur de sécurité »](#client-cannot-connect-no-licenses-available)
- [L’utilisateur reçoit un message « Accès refusé », ou il doit fournir les informations d’identification à deux reprises](#user-cannot-authenticate-or-must-authenticate-twice)
- [Sur la connexion, le reçoit un message « Services Bureau à distance sont actuellement occupé. »](#on-connecting-user-receives-remote-desktop-service-is-currently-busy-message)
- [Le client Bureau à distance se déconnecte et ne peut pas se reconnecter à la même session](#rd-client-disconnects-and-cannot-reconnect-to-the-same-session)
- [L’utilisateur se connecte à un ordinateur portable à distance via un réseau sans fil, et ensuite l’ordinateur portable se déconnecte du réseau](#remote-laptop-disconnects-from-wireless-network).
- [L’utilisateur rencontre des performances médiocres ou des problèmes avec les applications à distance](#user-experiences-poor-performance-or-application-problems)

> [!NOTE]  
> Pour obtenir la liste actuelle des codes de déconnexion RDP, consultez [ExtendedDisconnectReasonCode énumération](https://docs.microsoft.com/en-us/windows/desktop/TermServ/extendeddisconnectreasoncode). 

## <a name="no-specific-symptoms-or-messages-general-troubleshooting-steps"></a>Aucun symptômes spécifiques ou des messages (étapes de dépannage générales)

Utilisez ces étapes lorsqu’un client Bureau à distance ne peut pas se connecter à un bureau à distance, mais ne fournit pas de messages ou autres symptômes qui aideraient à identifient la cause. Pour résoudre la plupart des causes plus courantes de ce genre de problème, utilisez les méthodes suivantes :

- [Vérifier l’état du protocole RDP](#check-the-status-of-the-rdp-protocol)
- [Vérifier l’état des services RDP](#check-the-status-of-the-rdp-services)
- [Vérifiez que l’écouteur RDP fonctionne](#check-that-the-rdp-listener-is-functioning)
- [Vérifier le port d’écoute RDP](#check-the-rdp-listener-port)

### <a name="check-the-status-of-the-rdp-protocol"></a>Vérifier l’état du protocole RDP

#### <a name="check-the-status-of-the-rdp-protocol-on-a-local-computer"></a>Vérifier l’état du protocole RDP sur un ordinateur local

Pour vérifier et modifier l’état du protocole RDP sur un ordinateur local, consultez [comment activer le Bureau à distance](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access#how-to-enable-remote-desktop).

> [!NOTE]  
> Si les options du Bureau à distance ne sont pas disponibles, consultez [vérifier si un objet de stratégie de groupe bloque le RDP](#check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer).

#### <a name="check-the-status-of-the-rdp-protocol-on-a-remote-computer"></a>Vérifier l’état du protocole RDP sur un ordinateur distant

> [!IMPORTANT]  
> Suivez attentivement les étapes décrites dans cette section. De graves problèmes peuvent se produire si vous modifiez le Registre de façon incorrecte. Avant de le modifier, [sauvegarder le Registre pour la restauration](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) en cas de problèmes.

Pour vérifier et modifier l’état du protocole RDP sur un ordinateur distant, utilisez une connexion réseau au Registre :

1. Sélectionnez **Démarrer**, sélectionnez **exécuter**, puis entrez **regedt32**.
2. Dans l’Éditeur du Registre, sélectionnez **fichier**, puis sélectionnez **connexion au Registre réseau**.
3. Dans le **sélectionner un ordinateur** boîte de dialogue, entrez le nom de l’ordinateur distant, sélectionnez **vérifier les noms**, puis sélectionnez **OK**.
4. Accédez à **HKEY\_LOCAL\_MACHINE\\système\\CurrentControlSet\\contrôle\\serveur Terminal Server**.  
   ![Éditeur du Registre, montrant l’entrée fDenyTSConnections](..\media\troubleshoot-remote-desktop-connections\RegEntry_fDenyTSConnections.png)
   - Si la valeur de la **fDenyTSConnections** clé est **0**, RDP est activé.
   - Si la valeur de la **fDenyTSConnections** clé est **1**, RDP est désactivée.
5. Pour activer le protocole RDP, modifiez la valeur de **fDenyTSConnections** de **1** à **0**.

#### <a name="check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer"></a>Vérifiez si un objet de stratégie de groupe (GPO) ne bloque le RDP sur un ordinateur local

Si vous ne pouvez pas activer RDP dans l’interface utilisateur ou si la valeur de **fDenyTSConnections** revient à **1** une fois que vous l’avez modifié, un objet de stratégie de groupe peut écraser les paramètres au niveau de l’ordinateur.

Pour vérifier la configuration de stratégie de groupe sur un ordinateur local, ouvrez une fenêtre d’invite de commandes en tant qu’administrateur, puis entrez la commande suivante :
```
gpresult /H c:\gpresult.html
```
Une fois cette commande terminée, ouvrez gpresult.html. Dans **Configuration ordinateur\\modèles d’administration\\les composants Windows\\des Services Bureau à distance\\hôte de Session Bureau à distance\\connexions**, rechercher la **autoriser les utilisateurs à se connecter à distance à l’aide des Services Bureau à distance** stratégie.

- Si le paramètre pour cette stratégie est **activé**, stratégie de groupe ne bloque pas les connexions RDP.
- Si le paramètre pour cette stratégie est **désactivé**, vérifiez **GPO gagnante**. Il s’agit de l’objet de stratégie de groupe qui bloque les connexions RDP.
![Un segment de l’exemple de gpresult.html, dans lequel l’objet de stratégie de groupe au niveau du domaine ** bloc RDP ** est la désactivation de RDP.](..\media\troubleshoot-remote-desktop-connections\GPResult_RDSH_Connections_GP.png)
   
  ![Un segment de l’exemple de gpresult.html, dans lequel ** stratégie de groupe Local ** est la désactivation de RDP.](..\media\troubleshoot-remote-desktop-connections\GPResult_RDSH_Connections_LGP.png)

#### <a name="check-whether-a-gpo-is-blocking-rdp-on-a-remote-computer"></a>Vérifiez si un objet de stratégie de groupe ne bloque le RDP sur un ordinateur distant

Pour vérifier la configuration de stratégie de groupe sur un ordinateur distant, la commande est presque identique à un ordinateur local :
```
gpresult /S <computer name> /H c:\gpresult-<computer name>.html
```
Le fichier que cette commande produit (**gpresult -\<nom de l’ordinateur\>.html**) utilise le format des informations de même que la version de l’ordinateur local (**gpresult.html**) utilise.

#### <a name="modifying-a-blocking-gpo"></a>Modification d’un GPO de blocage

Vous pouvez modifier ces paramètres dans l’éditeur d’objet de groupe de stratégie (GPE) et de la Console de gestion Stratégie de groupe (GPM). Pour plus d’informations sur l’utilisation de stratégie de groupe, consultez [Advanced Group Policy Management](https://docs.microsoft.com/en-us/microsoft-desktop-optimization-pack/agpm/).

Pour modifier la stratégie de blocage, utilisez une des méthodes suivantes :

- Dans GPE, le niveau de stratégie de groupe approprié (par exemple, local ou domaine), accédez à **Configuration ordinateur\\modèles d’administration\\les composants Windows\\Services Bureau à distance\\ Hôte de Session Bureau à distance\\connexions**\\**autoriser les utilisateurs à se connecter à distance à l’aide des Services Bureau à distance**.  
   1. Définir la stratégie sur **activé** ou **pas configuré**.
   2. Sur les ordinateurs affectés, ouvrez une fenêtre d’invite de commandes en tant qu’administrateur et exécutez le **gpupdate /force** commande.
- Dans GPM, accédez à l’unité d’organisation dans lequel la stratégie de blocage est appliquée sur les ordinateurs concernés, puis supprimer la stratégie à partir de l’unité d’organisation.

### <a name="check-the-status-of-the-rdp-services"></a>Vérifier l’état des services RDP

Sur l’ordinateur local (client) et l’ordinateur distant (cible), les services suivants doivent être en cours d’exécution :

- Services Bureau à distance (TermService)
- Redirecteur de Port du mode utilisateur Services Bureau à distance (UmRdpService)

Vous pouvez utiliser le composant logiciel enfichable MMC des Services pour gérer les services localement ou à distance. Vous pouvez également utiliser PowerShell localement ou à distance (si l’ordinateur distant est configuré pour accepter les commandes PowerShell à distance).

![Services Bureau à distance dans le composant logiciel enfichable MMC Services. Ne modifiez pas les paramètres de service par défaut.](..\media\troubleshoot-remote-desktop-connections\RDSServiceStatus.png)

Sur un ordinateur, si un ou les deux services ne sont pas en cours d’exécution, démarrez-les.

> [!NOTE]  
> Si vous démarrez le service Services Bureau à distance, cliquez sur **Oui** pour redémarrer automatiquement le service redirecteur de Port UserMode de Services Bureau à distance.

### <a name="check-that-the-rdp-listener-is-functioning"></a>Vérifiez que l’écouteur RDP fonctionne

> [!IMPORTANT]  
> Suivez attentivement les étapes décrites dans cette section. De graves problèmes peuvent se produire si vous modifiez le Registre de façon incorrecte. Avant de le modifier, [sauvegarder le Registre pour la restauration](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) en cas de problèmes.

#### <a name="check-the-status-of-the-rdp-listener"></a>Vérifier l’état de l’écouteur RDP

Pour cette procédure, utilisez une instance de PowerShell qui dispose des autorisations d’administration. Pour un ordinateur local, vous pouvez également utiliser une invite de commandes qui dispose des autorisations d’administration. Toutefois, cette procédure utilise PowerShell, car les mêmes commandes fonctionnent à la fois localement et à distance.

1. Ouvrez une fenêtre PowerShell. Pour vous connecter à un ordinateur distant, entrez **Enter-PSSession - ComputerName \<nom de l’ordinateur\>** .
2. Entrez **qwinsta**. 
    ![La commande de qwinsta répertorie les processus à l’écoute sur les ports de l’ordinateur.](..\media\troubleshoot-remote-desktop-connections\WPS_qwinsta.png)
3. Si la liste comprend **rdp-tcp** avec l’état **écouter**, l’utilisation de l’écouteur RDP. Passez à [vérifier le port d’écoute RDP](#check-the-rdp-listener-port). Sinon, passez à l’étape 4.
4. Exporter la configuration de l’écouteur RDP à partir d’un ordinateur de travail.
    1. Se connecter à un ordinateur qui a la même version de système d’exploitation que l’ordinateur concerné a et d’accès du Registre de cet ordinateur (par exemple, en utilisant l’Éditeur du Registre).
    2. Accédez à l’entrée de Registre suivante :  
        **Clé HKEY\_LOCAL\_MACHINE\\système\\CurrentControlSet\\contrôle\\serveur Terminal Server\\WinStations\\RDP-Tcp**
    3. Exporter l’entrée vers un fichier .reg. Par exemple, dans l’Éditeur du Registre, cliquez sur l’entrée, sélectionnez **exporter**, puis entrez un nom de fichier pour les paramètres exportés.
    4. Copiez le fichier .reg exporté sur l’ordinateur infecté.
5. Pour importer la configuration de l’écouteur RDP, ouvrez une fenêtre PowerShell qui dispose des autorisations administratives sur l’ordinateur concerné (ou ouvrez la fenêtre PowerShell et vous connecter à distance à l’ordinateur concerné).
   1. Pour sauvegarder l’entrée de Registre existant, entrez la commande suivante :  
   
      ```powershell  
      cmd /c 'reg export "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp" C:\Rdp-tcp-backup.reg'   
      ```  
   

   2. Pour supprimer l’entrée de Registre existant, entrez les commandes suivantes :  
   
      ```powershell  
      Remove-Item -path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp' -Recurse -Force  
      ```
   
   3. Pour importer la nouvelle entrée de Registre et redémarrez le service, entrez les commandes suivantes :  
   
      ```powershell  
      cmd /c 'regedit /s c:\<filename>.reg'  
      Restart-Service TermService -Force  
      ```   
      où \<filename\> est le nom du fichier .reg exporté.
6. Tester la configuration en essayant de la connexion Bureau à distance à nouveau. Si vous ne pouvez toujours pas vous connecter, redémarrez l’ordinateur concerné.
7. Si vous ne pouvez toujours pas vous connecter, [vérifier l’état du certificat auto-signé RDP](#check-the-status-of-the-rdp-self-signed-certificate).

#### <a name="check-the-status-of-the-rdp-self-signed-certificate"></a>Vérifier l’état du certificat auto-signé RDP

1. Si vous ne pouvez toujours pas vous connecter, ouvrez le composant logiciel enfichable MMC Certificats. Lorsque vous êtes invité à sélectionner le magasin de certificats pour gérer, sélectionner **compte d’ordinateur**, puis sélectionnez l’ordinateur concerné.
2. Dans le **certificats** dossier sous **Bureau à distance**, supprimer le certificat auto-signé RDP. 
    ![Certificats de bureau à distance dans le composant logiciel enfichable MMC Certificats.](..\media\troubleshoot-remote-desktop-connections\MMCCert_Delete.png)
3. Sur l’ordinateur concerné, redémarrez le service Services Bureau à distance.
4. Actualisez le composant logiciel enfichable Certificats.
5. Si le certificat auto-signé RDP n’a pas été recréé, [vérifier les autorisations du dossier MachineKeys](#check-the-permissions-of-the-machinekeys-folder).

#### <a name="check-the-permissions-of-the-machinekeys-folder"></a>Vérifiez les autorisations du dossier MachineKeys

1. Sur l’ordinateur concerné, ouvrez l’Explorateur, puis accédez à **C:\\ProgramData\\Microsoft\\Crypto\\RSA\\** .
2. Avec le bouton droit **MachineKeys**, sélectionnez **propriétés**, sélectionnez **sécurité**, puis sélectionnez **avancé**.
3. Assurez-vous que les autorisations suivantes sont configurées :
      - BuiltIn\\administrateurs : Contrôle total
      - Tout le monde : Lecture, écriture

### <a name="check-the-rdp-listener-port"></a>Vérifier le port d’écoute RDP

Sur l’ordinateur local (client) et l’ordinateur distant (cible), l’écouteur RDP doit être à l’écoute sur le port 3389. Aucune autre application ne doit être à l’aide de ce port.

> [!IMPORTANT]  
> Suivez attentivement les étapes décrites dans cette section. De graves problèmes peuvent se produire si vous modifiez le Registre de façon incorrecte. Avant de le modifier, [sauvegarder le Registre pour la restauration](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) en cas de problèmes.

Pour vérifier ou modifier le port RDP, utilisez l’Éditeur du Registre :

1. Cliquez sur Démarrer, sélectionnez **exécuter**, puis entrez **regedt32**.
      - Pour vous connecter à un ordinateur distant, sélectionnez **fichier**, puis sélectionnez **connexion au Registre réseau**.
      - Dans le **sélectionner un ordinateur** boîte de dialogue, entrez le nom de l’ordinateur distant, sélectionnez **vérifier les noms**, puis sélectionnez **OK**.
2. Ouvrez le Registre et accédez à **HKEY\_LOCAL\_MACHINE\\système\\CurrentControlSet\\contrôle\\Terminal Server\\WinStations\\ \<écouteur\>** . 
    ![La sous-clé de numéro de port pour le protocole RDP.](..\media\troubleshoot-remote-desktop-connections\RegEntry_PortNumber.png)
3. Si **PortNumber** a une valeur autre que **3389**, remplacez-la par **3389**. 
   > [!IMPORTANT]  
    > Vous pouvez utiliser les services Bureau à distance à l’aide d’un autre port. Toutefois, nous déconseillons que cela. Résolution des problèmes d’une telle configuration n’entre pas dans le cadre de cet article.
4. Après avoir modifié le numéro de port, redémarrez le service Services Bureau à distance.

#### <a name="check-that-another-application-is-not-trying-to-use-the-same-port"></a>Vérifiez qu’une autre application ne tente pas d’utiliser le même port

Pour cette procédure, utilisez une instance de PowerShell qui dispose des autorisations d’administration. Pour un ordinateur local, vous pouvez également utiliser une invite de commandes qui dispose des autorisations d’administration. Toutefois, cette procédure utilise PowerShell, car les commandes mêmes fonctionnent localement et à distance.

1. Ouvrez une fenêtre PowerShell. Pour vous connecter à un ordinateur distant, entrez **Enter-PSSession - ComputerName \<nom de l’ordinateur\>** .
2. Entrez la commande suivante :  
   
     ```powershell  
    cmd /c 'netstat -ano | find "3389"'  
    ```
  
    ![La commande netstat génère une liste des ports et services qui écoutent leur.](..\media\troubleshoot-remote-desktop-connections\WPS_netstat.png)
1. Recherchez une entrée pour le port TCP 3389 (ou le port RDP attribué) associé à l'état **Écoute**. 
    > [!NOTE]  
   > Le PID (Identificateur de processus) du processus ou service utilisant ce port apparaît sous la colonne PID.
1. Pour déterminer quelle application utilise le port 3389 (ou le port RDP attribué), entrez la commande suivante :  
   
     ```powershell  
    cmd /c 'tasklist /svc | find "<pid listening on 3389>"'  
    ```  
  
    ![La commande tasklist signale les détails d’un processus spécifique.](..\media\troubleshoot-remote-desktop-connections\WPS_tasklist.png)
1. Recherchez une entrée pour le numéro PID associé avec le port (à partir de la **netstat** sortie). Les services ou les processus qui sont associés à ce PID apparaissent à droite.
1. Si une application ou un service autre que les Services Bureau à distance (TermServ.exe) utilise le port, vous pouvez résoudre le conflit en utilisant l’une des méthodes suivantes :
      - Configurez l’autre application ou service pour utiliser un autre port (recommandé).
      - Désinstaller l’autre application ou service.
      - Configurez le RDP pour utiliser un autre port, puis redémarrez le service Services Bureau à distance (non recommandé).

#### <a name="check-whether-a-firewall-is-blocking-the-rdp-port"></a>Vérifiez si un pare-feu ne bloque le port RDP

Utilisez le **psping** outil pour tester si vous pouvez accéder à l’ordinateur affecté à l’aide du port 3389.

1. Sur un ordinateur différent de l’ordinateur concerné, téléchargez **psping** de <https://live.sysinternals.com/psping.exe>.
2. Ouvrez une fenêtre d’invite de commandes en tant qu’administrateur, accédez au répertoire dans lequel vous avez installé **psping**, puis entrez la commande suivante :  
   
   ```  
   psping -accepteula <computer IP>:3389  
   ```
   
3. Vérifiez le résultat de la **psping** commande pour les résultats tels que les éléments suivants :  
      - **Connexion à \<ordinateur IP\>** : L’ordinateur distant est accessible.
      - **(0 % de pertes)** : Toutes les tentatives de connexion a réussi.
      - **L’ordinateur distant a refusé la connexion de réseau**: L’ordinateur distant n’est pas accessible.
      - **(perte 100 %)** : Toutes les tentatives de connexion a échoué.
1. Exécutez **psping** sur plusieurs ordinateurs pour tester leur capacité à se connecter à l’ordinateur concerné.
1. Notez que si l’ordinateur affecté bloque les connexions à partir de tous les autres ordinateurs, certains autres ordinateurs ou qu’un autre ordinateur.
1. Étapes suivantes recommandées :
      - Impliquez vos administrateurs réseau pour vérifier que le réseau autorise le trafic RDP vers l’ordinateur concerné.
      - Identifier les configurations de tous les pare-feu entre les ordinateurs source et de l’ordinateur concerné (y compris les pare-feu Windows sur l’ordinateur affecté) pour déterminer si un pare-feu bloque le port RDP.

## <a name="client-cannot-connect-class-not-registered"></a>Client ne peut pas se connecter, « Classe non inscrite »

Lorsque vous essayez de vous connecter à un ordinateur distant à l’aide d’un client qui exécute Windows 10, version 1709 ou version ultérieure, le client ne se connecte pas pendant que le serveur hôte de Session Bureau à distance signale un message qui contient le code d’erreur « La classe n’est pas enregistré (0 x 80040154) ».

Ce problème se produit si l’utilisateur qui essaie de se connecter dispose d’un profil utilisateur obligatoire. Pour résoudre ce problème, installez le [24 juillet 2018 — KB4338817 (16299.579 de Build du système d’exploitation)](https://support.microsoft.com/en-us/help/4338817/windows-10-update-kb4338817) mise à jour Windows 10.

## <a name="client-cannot-connect-no-licenses-available"></a>Client ne peut pas se connecter, aucune licence disponible.

Cette situation s’applique aux déploiements qui incluent un serveur RDSH et un serveur de licences de bureau à distance.

Identifier le type de comportement utilisateurs constatent :

- La session a été déconnectée, car aucune licence n’est disponible ou aucun serveur de licences n’est disponible
- Accès refusé en raison d’une erreur de sécurité

Connectez-vous à l’hôte de Session Bureau à distance en tant qu’un administrateur de domaine, puis ouvrez l’outil de diagnostic de licences bureau à distance. Recherchez des messages tels que les éléments suivants :

  - La période de grâce pour l’instance distante bureau serveur hôte de Session a expiré, mais le serveur hôte de Session Bureau à distance n’a pas été configuré avec des serveurs de licence. connexions au serveur hôte de Session Bureau à distance seront refusées, sauf si un serveur de licences est configuré pour le serveur hôte de Session Bureau à distance.
  - Serveur de licences \<nom de l’ordinateur\> n’est pas disponible. Cela peut être dû à des problèmes de connectivité réseau, le service de licences de bureau à distance est arrêté sur le serveur de licences ou Gestionnaire de licences bureau à distance n’est plus installé sur l’ordinateur.

Ces problèmes ont tendance à être associé avec les messages utilisateur suivants :

  - La session distante a été déconnectée, car aucune licence d’accès client Bureau à distance ne sont disponibles pour cet ordinateur.
  - La session distante a été déconnectée, car aucun serveur de licences bureau à distance fournir une licence.

Dans ce cas, [configurer le service de licences des services Bureau à distance](#configure-the-rd-licensing-service).

Si l’outil de diagnostic de licences bureau à distance répertorie les autres problèmes, tels que « le composant X.224 du protocole RDP a détecté une erreur dans le flux du protocole et a déconnecté le client, » il peut être un problème qui affecte les certificats de licence. Ces problèmes ont tendance à être associé à des messages de l’utilisateur, telles que les éléments suivants :

Le client n’a pas pu se connecter au serveur Terminal Server en raison d’une erreur de sécurité. Après avoir vérifié que vous êtes connecté au réseau, essayez à nouveau de vous connecter au serveur.

Dans ce cas, [clés de Registre du certificat actualisation le X509](#refresh-the-x509-certificate-registry-keys).

### <a name="configure-the-rd-licensing-service"></a>Configurer le service de licences des services Bureau à distance

La procédure suivante utilise le Gestionnaire de serveur pour apporter les modifications de configuration. Pour plus d’informations sur la façon de configurer et utiliser le Gestionnaire de serveur, consultez [le Gestionnaire de serveur](https://docs.microsoft.com/en-us/windows-server/administration/server-manager/server-manager).

1. Ouvrez le Gestionnaire de serveur, puis accédez à **Services Bureau à distance**.
2. Sur **vue d’ensemble du déploiement**, sélectionnez **tâches**, puis sélectionnez **modifier les propriétés de déploiement**.
3. Sélectionnez **Gestionnaire de licences bureau à distance**, puis sélectionnez le mode de licence approprié pour votre déploiement (**par périphérique** ou **par utilisateur**).
4. Entrez le nom de domaine complet (FQDN) de votre serveur de licences des services Bureau à distance, puis sélectionnez **ajouter**.
5. Si vous avez plusieurs serveurs de licences des services Bureau à distance, répétez l’étape 4 pour chaque serveur. 
    ![Options de configuration de serveur Bureau à distance par licence dans le Gestionnaire de serveur.](..\media\troubleshoot-remote-desktop-connections\RDLicensing_Configure.png)

### <a name="refresh-the-x509-certificate-registry-keys"></a>Clés de Registre du certificat actualisation le X509

> [!IMPORTANT]  
> Suivez attentivement les étapes décrites dans cette section. De graves problèmes peuvent se produire si vous modifiez le Registre de façon incorrecte. Avant de le modifier, [sauvegarder le Registre pour la restauration](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) en cas de problèmes.

Pour résoudre ce problème, sauvegarder et clés de Registre du certificat de supprimer le X509, redémarrez l’ordinateur et serveur de licences de réactiver le Bureau à distance. Pour ce faire, procédez comme suit.

> [!NOTE]  
> Effectuez la procédure suivante sur chaque serveur RDSH.

1. Ouvrez l’Éditeur du Registre et accédez à **HKEY\_LOCAL\_MACHINE\\système\\CurrentControlSet\\contrôle\\Terminal Server\\RCM**.
2. Dans le menu Registre, sélectionnez **exporter un fichier du Registre**.
3. Type **certificat exporté** dans le **nom de fichier** zone, puis sélectionnez **enregistrer**.
4. Cliquez sur chacune des valeurs suivantes, sélectionnez **supprimer**, puis sélectionnez **Oui** pour vérifier la suppression :  
      - **Certificate**
      - **X509 Certificate**
      - **X509 Certificate ID**
      - **X509 Certificate2**
5. Quittez l’Éditeur du Registre et redémarrez le serveur RDSH.

## <a name="user-cannot-authenticate-or-must-authenticate-twice"></a>Utilisateur ne peut pas authentifier ou doit s’authentifier deux fois

Il existe plusieurs problèmes qui peuvent provoquer des problèmes qui affectent l’authentification utilisateur. Cette section traite les cas suivants :

  - [Un utilisateur se voit refuser l’accès avec un message « restricted type d’ouverture de session »](#access-denied-restricted-type-of-logon)
  - [Un utilisateur ou une application est refusée l’accès à l’événement 16965, « un appel à distance à la base de données SAM a été refusé »](#access-denied-a-remote-call-to-the-sam-database-has-been-denied)
  - [Un utilisateur ne peut pas se connecter à l’aide d’une carte à puce](#a-user-cannot-sign-in-by-using-a-smart-card)
  - [Si l’ordinateur distant est verrouillé, un utilisateur doit entrer un mot de passe à deux reprises](#if-the-remote-pc-is-locked-a-user-needs-to-enter-a-password-twice)
  - [Un utilisateur ne peut pas se connecter et reçoit des « authentification messages d’erreur » et « Mise à jour CredSSP chiffrement oracle »](#user-cannot-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages)
  - [Une fois que vous mettez à jour les ordinateurs clients, certains utilisateurs doivent se connecter à deux reprises](#after-you-update-client-computers-some-users-need-to-sign-in-twice).
  - [Les utilisateurs sont interdites sur un déploiement qui utilise RemoteGuard avec plusieurs agents de connexion Bureau à distance](#users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers)

### <a name="access-denied-restricted-type-of-logon"></a>Accès refusé, le type restreint d’ouverture de session

Dans ce cas, un utilisateur de Windows 10 tente de se connecter aux ordinateurs Windows 10 ou Windows Server 2016 est refusé avec le message suivant :

> Connexion Bureau à distance :  
> L’administrateur système a restreint le type d’ouverture de session (réseau ou interactive) que vous pouvez utiliser. Pour obtenir une assistance, contactez votre administrateur système ou le support technique.

Ce problème se produit lorsque l’authentification de niveau réseau (NLA) est requis pour les connexions RDP, et l’utilisateur n’est pas un membre de la **Remote Desktop Users** groupe. Il peut également se produire si le **Remote Desktop Users** groupe n’a pas été affecté à la **accéder à cet ordinateur à partir du réseau** droit d’utilisateur.

Pour remédier à ce problème, suivez une des étapes suivantes :

  - [Modifier l’appartenance au groupe de l’utilisateur ou l’attribution des droits utilisateur](#modify-the-users-group-membership-or-user-rights-assignment).
  - Désactiver l’authentification NLA (non recommandé).
  - Utilisez des clients de bureau à distance Windows 10. Par exemple, les clients Windows 7 n’ont pas ce problème.

#### <a name="modify-the-users-group-membership-or-user-rights-assignment"></a>Modifier l’appartenance au groupe de l’utilisateur ou l’attribution des droits utilisateur

Si ce problème concerne un seul utilisateur, la solution la plus simple à ce problème consiste à ajouter l’utilisateur à la **Remote Desktop Users** groupe.

Si l’utilisateur est déjà un membre de ce groupe (ou si plusieurs membres de groupe ont le même problème), vérifiez la configuration des droits utilisateur sur l’ordinateur distant Windows 10 ou Windows Server 2016.

1. Ouvrez l’éditeur d’objet de groupe de stratégie (GPE) et connectez-vous à la stratégie locale de l’ordinateur distant.
2. Accédez à **Configuration ordinateur\\Windows paramètres\\paramètres de sécurité\\stratégies locales\\attribution des droits utilisateur**, avec le bouton droit **y accéder ordinateur à partir du réseau**, puis sélectionnez **propriétés**.
3. Vérifiez la liste des utilisateurs et groupes pour **Remote Desktop Users** (ou un groupe parent).
4. Si la liste n’inclut pas **Remote Desktop Users** (ou un parent groupe, telles que **tout le monde**) vous devez l’ajouter à la liste (si vous avez plus d’un ou deux ordinateurs dans votre déploiement, utilisez un objet de stratégie de groupe) .  
    Par exemple, l’appartenance par défaut pour **accéder à cet ordinateur à partir du réseau** inclut **tout le monde**. Si votre déploiement utilise un objet de stratégie de groupe pour supprimer **tout le monde**, vous devrez peut-être restaurer l’accès en mettant à jour de l’objet de stratégie de groupe pour ajouter **Remote Desktop Users**.

### <a name="access-denied-a-remote-call-to-the-sam-database-has-been-denied"></a>Accès refusé, un appel à distance à la base de données SAM a été refusé.

Ce comportement est susceptible de se produire si vos contrôleurs de domaine exécutent Windows Server 2016 ou version ultérieure, et les utilisateurs tentent de se connecter à l’aide d’une application de connexion personnalisée. En particulier, les applications qui accèdent aux informations de profil de l’utilisateur dans Active Directory accès seront refusées.

Ce comportement entraîne à partir d’une modification à Windows. Dans Windows Server 2012 R2 et versions antérieures des versions, lorsqu’un utilisateur se connecte à un bureau, contacts de gestionnaire de connexion à distance (RCM) à distance le contrôleur de domaine (DC) pour interroger les configurations qui sont spécifiques au Bureau à distance sur l’objet utilisateur dans le domaine Active Directory Services (AD DS). Ces informations sont affichées dans l’onglet profil de Services Bureau à distance de propriétés de l’objet d’un utilisateur dans les utilisateurs Active Directory et le composant logiciel enfichable MMC d’ordinateurs.

À compter de Windows Server 2016, RCM interroge n’est plus l’objet d’utilisateurs dans AD DS. Si vous avez besoin RCM interroger les services AD DS, étant donné que vous utilisez les attributs des Services Bureau à distance, vous devez activer manuellement le comportement précédent de RCM.

> [!IMPORTANT]  
> Suivez attentivement les étapes décrites dans cette section. De graves problèmes peuvent se produire si vous modifiez le Registre de façon incorrecte. Avant de le modifier, [sauvegarder le Registre pour la restauration](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) en cas de problèmes.

Pour activer le comportement hérité RCM sur un serveur hôte de Session Bureau à distance, configurez les entrées de Registre suivantes, puis redémarrez le **Services Bureau à distance** service :  
  - **Clé HKEY\_LOCAL\_MACHINE\\logiciel\\stratégies\\Microsoft\\Windows NT\\Services Terminal Server**
  - **Clé HKEY\_LOCAL\_MACHINE\\système\\CurrentControlSet\\contrôle\\Terminal Server\\WinStations\\\<nom de la station Windows\>\\**  
      - Name: **fQueryUserConfigFromDC**
      - Tapez : **Reg\_DWORD**
      - Valeur : **1** (décimal)

Pour activer le comportement hérité RCM sur un serveur autre que d’un serveur hôte de Session Bureau à distance, configurer ces entrées de Registre et de l’entrée de Registre supplémentaires suivantes (et redémarrez le service) :
  - **Clé HKEY\_LOCAL\_MACHINE\\système\\CurrentControlSet\\contrôle\\serveur Terminal Server**

Pour plus d’informations sur ce comportement, consultez KB 3200967 [change pour le Gestionnaire de connexions à distance dans Windows Server](https://support.microsoft.com/en-us/help/3200967/changes-to-remote-connection-manager-in-windows-server).

### <a name="a-user-cannot-sign-in-by-using-a-smart-card"></a>Un utilisateur ne peut pas se connecter à l’aide d’une carte à puce

Cet article traite des trois cas courants dans lequel un utilisateur ne peut pas se connecter à un bureau à distance à l’aide d’une carte à puce :  
  - [L’utilisateur ne peut pas se connecter à une filiale qui utilise un en lecture seule contrôleur de domaine (RODC)](#cannot-sign-in-with-a-smart-card-in-a-branch-office-with-a-rodc)
  - [L’utilisateur ne peut pas se connecter à un ordinateur Windows Server 2008 SP2 a récemment été mis à jour](#cannot-sign-in-with-a-smart-card-to-a-windows-server-2008-sp2-computer)
  - [L’utilisateur ne peut pas rester connecté à un ordinateur Windows qui a été récemment mis à jour, et le service Services Bureau à distance devient ne répond pas (se bloque)](#cannot-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs)

#### <a name="cannot-sign-in-with-a-smart-card-in-a-branch-office-with-a-rodc"></a>Impossible de se connecter avec une carte à puce dans une filiale avec un RODC

Ce problème se produit dans les déploiements qui incluent un serveur RDSH sur un site de succursale qui utilise un RODC. Le serveur RDSH est hébergé dans le domaine racine. Les utilisateurs sur le site de succursale appartiennent à un domaine enfant et utilisent des cartes à puce pour l’authentification. Le RODC est configuré pour les mots de passe utilisateur du cache (le RODC appartient à la **groupe de réplication de mot de passe RODC autorisée**). Lorsque les utilisateurs essaient de se connecter à des sessions sur le serveur RDSH, ils reçoivent des messages tels que « l’ouverture de session tentée n’est pas valide. Il s’agit soit en raison d’un nom d’utilisateur ou de l’authentification des informations incorrectes. »

Il s’agit d’un problème connu de la façon qui la racine du contrôleur de domaine et le RODC gérer le chiffrement des informations d’identification de l’utilisateur. La racine du contrôleur de domaine utilise une clé de chiffrement pour chiffre les informations d’identification et le RODC fournit une clé de déchiffrement au client. Toutefois, les deux clés ne correspondent pas.

Pour contourner ce problème, envisagez de modifier votre topologie de contrôleur de domaine en désactivant la mise en cache de mot de passe sur le RODC ou en déployant un contrôleur de domaine accessible en écriture sur le site de succursale. Une autre méthode consiste à déplacer le serveur RDSH au même domaine enfant en tant que les utilisateurs. Une autre solution consiste à autoriser les utilisateurs à se connecter sans cartes à puce. L’une de ces solutions peuvent nécessiter des compromis de performances ou du niveau de sécurité.

#### <a name="cannot-sign-in-with-a-smart-card-to-a-windows-server-2008-sp2-computer"></a>Impossible de se connecter avec une carte à puce sur un ordinateur Windows Server 2008 SP2

Ce problème se produit lorsque les utilisateurs se connectent à un ordinateur Windows Server 2008 SP2 qui a été mis à jour avec KB4093227 (2018.4B). Lorsque les utilisateurs tentent de se connecter à l’aide d’une carte à puce, voient refuser l’accès avec des messages tels que « aucun certificat valide trouvé. Vérifiez que la carte est correctement insérée et s’intègre étroitement. » En même temps, l’ordinateur Windows Server enregistre l’événement d’Application « une erreur s’est produite lors de la récupération d’un certificat numérique à partir de la carte à puce insérée. Signature non valide. »

Pour résoudre ce problème, mettez à jour de l’ordinateur Windows Server avec le 2018.06 B nouvelle publication de base de connaissances 4093227, [Description de la mise à jour de sécurité pour Windows protocole RDP (Remote Desktop) vulnérabilité de déni de service dans Windows Server 2008 : Le 10 avril 2018](https://support.microsoft.com/en-us/help/4093227/security-update-for-vulnerabilities-in-windows-server-2008).

#### <a name="cannot-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs"></a>Ne peut pas rester connecté avec une carte à puce et le blocage du service Services Bureau à distance

Ce problème se produit lorsque les utilisateurs se connectent à un ordinateur Windows ou Windows Server qui a été mis à jour avec la base de connaissances 4056446. Dans un premier temps, l’utilisateur peut être en mesure de se connecter au système à l’aide d’une carte à puce, mais reçoit ensuite un « CUTION\_E\_non\_SERVICE « message d’erreur. L’ordinateur distant peut cesser de répondre.

Pour contourner ce problème, redémarrez l’ordinateur distant.

Pour résoudre ce problème, mettez à jour de l’ordinateur distant avec le correctif approprié :

  - Windows Server 2008 SP2 : Ko 4090928, [Windows les fuites de descripteurs dans le processus lsm.exe et applications de carte à puce peuvent afficher « CUTION\_E\_non\_SERVICE « erreurs](https://support.microsoft.com/en-us/help/4090928/scard-e-no-service-errors-when-windows-leaks-handles-in-the-lsm-exe)
  - Windows Server 2012 R2 : Ko 4103724, [le 17 mai 2018 — KB4103724 (version préliminaire du correctif mensuel)](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724)
  - Windows Server 2016 et Windows 10, version 1607 : Ko 4103720, [le 17 mai 2018 — KB4103720 (Build du système d’exploitation 14393.2273)](https://support.microsoft.com/en-us/help/4103720/windows-10-update-kb4103720)

### <a name="if-the-remote-pc-is-locked-a-user-needs-to-enter-a-password-twice"></a>Si l’ordinateur distant est verrouillé, un utilisateur doit entrer un mot de passe à deux reprises

Ce problème peut se produire lorsqu’un utilisateur tente de se connecter à un bureau à distance qui exécute Windows 10 version 1709 dans un déploiement dans lequel les connexions RDP ne nécessitent pas l’authentification NLA. Dans ces conditions, si le Bureau à distance a été verrouillé, l’utilisateur doit entrer ses informations d’identification à deux reprises lors de la connexion.

Pour résoudre ce problème, mettez à jour de l’ordinateur de la version 1709 Windows 10 avec la base de connaissances 4343893, [30 août 2018 — KB4343893 (16299.637 de Build du système d’exploitation)](https://support.microsoft.com/en-us/help/4343893/windows-10-update-kb4343893).

### <a name="user-cannot-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages"></a>Utilisateur ne peut pas se connecter et a reçu « authentification messages d’erreur » et « Mise à jour CredSSP chiffrement oracle »

Lorsque les utilisateurs essaient de se connecter à l’aide de n’importe quelle version de Windows à partir de Windows Vista SP2 et versions ultérieures ou Windows Server 2008 SP2 et versions ultérieures, leur accès sont refusés et recevoir des messages tels que les éléments suivants :

  - Une erreur d’authentification s’est produite. La fonction demandée n’est pas pris en charge.
  - Cela peut résulter de la mise à jour de CredSSP chiffrement oracle

« Mise à jour CredSSP chiffrement oracle » fait référence à un ensemble de mises à jour de sécurité publiée en mars, avril et mai 2018. CredSSP est un fournisseur d’authentification qui traite les demandes d’authentification pour les autres applications. Le 13 mars 2018, 3 « b » et les mises à jour ultérieures résolu d’exploitation dans lequel un attaquant pourrait relayer des informations d’identification de l’utilisateur pour exécuter du code sur le système cible.

Les mises à jour initiales prise en charge pour un nouvel objet de stratégie de groupe, **mise à jour de chiffrement Oracle**, qui a les paramètres possibles suivants :

  - **Vulnérable**. Applications clientes qui utilisent CredSSP peuvent revenir aux versions non sécurisées, mais ce comportement expose les bureaux à distance à des attaques. Les services qui utilisent CredSSP l’acceptation des clients qui n’ont pas été mis à jour.
  - **Atténué**. Applications clientes qui utilisent CredSSP ne peut pas revenir aux versions non sécurisées, mais les services qui utilisent CredSSP l’acceptation des clients qui n’ont pas été mis à jour.
  - **Mise à jour de force les Clients**. Applications clientes qui utilisent CredSSP ne peut pas revenir aux versions non sécurisées, et les services qui utilisent CredSSP n’acceptera pas les clients non corrigés. 
    Remarque: Ce paramètre ne doit pas être déployé jusqu'à ce que tous les hôtes distants prennent en charge la version la plus récente.

La mise à jour du 8 mai 2018, modifié la valeur par défaut **mise à jour de chiffrement Oracle** définir à partir de **vulnérable** à **mitigé**. Avec cette modification en place, les clients Bureau à distance qui ont les mises à jour ne peut pas se connecter aux serveurs qui n’en avez pas (ou mis à jour des serveurs qui n’ont pas été redémarrés). Pour plus d’informations sur les effets des mises à jour et les types de communication qui ils bloquent, consultez 4093492 Ko, [CredSSP met à jour pour CVE-2018-0886](https://support.microsoft.com/en-us/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018).

Pour résoudre ce problème, assurez-vous que tous les systèmes sont entièrement mis à jour et redémarrées. Pour obtenir une liste complète des mises à jour et plus d’informations sur les vulnérabilités, consultez [CVE-2018-0886 | Vulnérabilité d’exécution de Code à distance CredSSP](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-0886).

Pour contourner ce problème jusqu'à ce que les mises à jour sont terminées, vérifiez 4093492 Ko pour les types autorisés de connexions. S’il n’existe aucune alternative possible, vous pouvez envisager une des méthodes suivantes :

  - Pour les ordinateurs clients affectés, définissez le **mise à jour de chiffrement Oracle** stratégie de sauvegarde pour **vulnérable**.
  - Modifier les stratégies suivantes dans le **Configuration ordinateur\\modèles d’administration\\les composants Windows\\Services Bureau à distance\\hôte de Session Bureau à distance\\ Sécurité** dossier de stratégie de groupe :  
      - **Exiger l’utilisation de la couche de sécurité spécifique pour les connexions à distance (RDP)** : la valeur **activé** et sélectionnez **RDP**.
      - **Exiger une authentification de l’utilisateur pour les connexions à distance à l’aide de l’authentification au niveau du réseau**: la valeur **désactivé**.
      > [!IMPORTANT]  
      > Ces modifications réduisent la sécurité de votre déploiement. Ils doivent uniquement être temporaires, si vous utilisez le tout.

Pour plus d’informations sur l’utilisation de la stratégie de groupe, consultez [modifier un GPO blocage](#modifying-a-blocking-gpo).

### <a name="after-you-update-client-computers-some-users-need-to-sign-in-twice"></a>Une fois que vous mettez à jour les ordinateurs clients, certains utilisateurs doivent se connecter à deux reprises

Les utilisateurs sur une version de Windows 7 ou Windows 10 1709 se connecter à une session Bureau à distance et voir immédiatement un deuxième écran de connexion. Ce problème se produit si les ordinateurs clients ont reçu les mises à jour suivantes :

  - Windows 7 : Ko 4103718, [le 8 mai 2018 — KB4103718 (cumul mensuel)](https://support.microsoft.com/en-us/help/4103718/windows-7-update-kb4103718)
  - Windows 10 1709 : Ko 4103727, [le 8 mai 2018 — KB4103727 (Build du système d’exploitation 16299.431)](https://support.microsoft.com/en-us/help/4103727/windows-10-update-kb4103727)

Pour résoudre ce problème, assurez-vous que les ordinateurs que les utilisateurs souhaitent se connecter à (ainsi que les serveurs RDSH ou RDVI) sont entièrement mis à jour via juin 2018. Cela inclut les mises à jour suivantes :

  - Windows Server 2016 : Ko 4284880, [le 12 juin 2018 — KB4284880 (Build du système d’exploitation 14393.2312)](https://support.microsoft.com/en-us/help/4284880/windows-10-update-kb4284880)
  - Windows Server 2012 R2 : Ko 4284815, [le 12 juin 2018 — KB4284815 (cumul mensuel)](https://support.microsoft.com/en-us/help/4284815/windows-81-update-kb4284815)
  - Windows Server 2012 : Ko 4284855, [le 12 juin 2018 — KB4284855 (cumul mensuel)](https://support.microsoft.com/en-us/help/4284855/windows-server-2012-update-kb4284855)
  - Windows Server 2008 R2 : Ko 4284826, [le 12 juin 2018 — KB4284826 (cumul mensuel)](https://support.microsoft.com/en-us/help/4284826/windows-7-update-kb4284826)
  - Windows Server 2008 SP2 : KB4056564, [Description de la mise à jour de sécurité pour la vulnérabilité d’exécution de code à distance CredSSP dans Windows Server 2008, Windows Embedded POSReady 2009 et Windows Embedded Standard 2009 : 13 mars 2018](https://support.microsoft.com/en-us/help/4056564/security-update-for-vulnerabilities-in-windows-server-2008)

### <a name="users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers"></a>Les utilisateurs sont interdites sur un déploiement qui utilise de Credential Guard à distance avec plusieurs agents de connexion Bureau à distance

Ce problème se produit dans les déploiements de haute disponibilité qui utilisent deux ou plusieurs Desktop connexion Brokers distants, si Credential Guard à distance de Windows Defender est en cours d’utilisation. Les utilisateurs ne peuvent pas se connecter aux bureaux à distance.

Ce problème se produit parce que de Credential Guard à distance utilise Kerberos pour l’authentification et restreint NTLM. Dans une configuration haute disponibilité avec équilibrage de charge, les agents de la connexion Bureau à distance ne peut pas en charge les opérations Kerberos.

Si vous avez besoin d’utiliser une configuration de haute disponibilité avec équilibrage de charge des agents de connexion Bureau à distance, vous pouvez contourner ce problème en désactivant les Credential Guard à distance. Pour plus d’informations sur la gestion de Credential Guard à distance de Windows Defender, consultez [informations d’identification de protéger le Bureau à distance avec Credential Guard à distance de Windows Defender](https://docs.microsoft.com/en-us/windows/security/identity-protection/remote-credential-guard#enable-windows-defender-remote-credential-guard).

## <a name="on-connecting-user-receives-remote-desktop-service-is-currently-busy-message"></a>Sur la connexion, utilisateur reçoit le message « Services Bureau à distance sont actuellement occupé. »

Pour déterminer une réponse appropriée à ce problème, utilisez les étapes suivantes :

1. Le service Services Bureau à distance ne devient instable (par exemple, le client Bureau à distance se « bloquer » à l’écran d’accueil).  
      - Si le service ne répond pas, consultez [problème de mémoire du serveur RDSH](#rdsh-server-memory-issue).
      - Si le client s’affiche pour interagir avec le service normalement, passez à l’étape suivante.
2. Si un ou plusieurs utilisateurs déconnectent les sessions Bureau à distance, peuvent les utilisateurs se connecter à nouveau ?  
   - Si le service continue à refuser les connexions, quel que soit le nombre d’utilisateurs déconnecter les sessions, consultez [problème d’écouteur de bureau à distance](#rd-listener-issue).
   - Si le service commence à accepter des connexions à nouveau après un nombre d’utilisateurs ont leurs sessions déconnectées, [vérifier la stratégie de limite de connexion](#check-the-connection-limit-policy).

### <a name="rdsh-server-memory-issue"></a>Problème de mémoire du serveur RDSH

Une fuite de mémoire a été trouvée sur certains serveurs de Windows Server 2012 R2 RDSH. Au fil du temps, ces serveurs commencent à refuser les connexions Bureau à distance et connexions console locale avec des messages tels que les éléments suivants :

> La tâche que vous essayez de faire ne peut pas être effectuée car le Service Bureau à distance est actuellement occupé. Veuillez réessayer dans quelques minutes. Autres utilisateurs doivent toujours être en mesure de se connecter.

Les clients Bureau à distance tente de se connecter également cesser de répondre.

Pour contourner ce problème, redémarrez le serveur RDSH.

Pour résoudre ce problème, appliquez 4093114 Ko, [10 avril 2018 — KB4093114 (cumul mensuel)] (file:///C:\\utilisateurs\\jesits (deuxième v\\AppData\\Local\\Microsoft\\Windows\\INetCache\\Content.Outlook\\FUB8OO45\\% avril 2010, % 202018 — KB4093114 %20\(mensuel % 20Rollup\)), aux serveurs RDSH.

### <a name="rd-listener-issue"></a>Problème de port d’écoute de bureau à distance

Un problème a été observé sur certains serveurs RDSH qui ont été mis à niveau directement à partir de Windows Server 2008 R2 vers Windows Server 2012 R2 ou Windows Server 2016. Lorsqu’un client Bureau à distance se connecte au serveur RDSH, le serveur RDSH crée un écouteur de bureau à distance pour la session utilisateur. Les serveurs concernés comptabiliser les écouteurs de bureau à distance augmente à mesure que les utilisateurs se connectent, mais jamais diminue.

Pour contourner ce problème, vous pouvez utiliser les méthodes suivantes :

  - Redémarrez le serveur RDSH pour réinitialiser le nombre d’écouteurs de bureau à distance
  - Modifier la stratégie de limite de connexion, configurez-la sur une valeur très élevée. Pour plus d’informations sur la gestion de la stratégie de limite de connexion, consultez [vérifier la stratégie de limite de connexion](#check-the-connection-limit-policy).

Pour résoudre ce problème, appliquez les mises à jour suivantes aux serveurs RDSH :

  - Windows Server 2012 R2 : Ko 4343891, [le 30 août 2018 — KB4343891 (version préliminaire du correctif mensuel)](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)
  - Windows Server 2016 : Ko 4343884, [le 30 août 2018 — KB4343884 (Build du système d’exploitation 14393.2457)](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)

### <a name="check-the-connection-limit-policy"></a>Vérifier la stratégie de limite de connexion

Vous pouvez définir la limite du nombre de connexions Bureau à distance simultanées au niveau ordinateur individuel ou en configurant un objet de stratégie de groupe (GPO). Par défaut, la limite n’est pas définie.

Pour vérifier les paramètres actuels et identifier les objets de stratégie existants sur le serveur RDSH, ouvrez une fenêtre d’invite de commandes en tant qu’administrateur et entrez la commande suivante :
  
```
gpresult /H c:\gpresult.html
```
   
Après cette commande terminée, ouvrez gpresult.html et dans **Configuration ordinateur\\modèles d’administration\\les composants Windows\\Services Bureau à distance\\hôte de Session Bureau à distance \\Connexions**, recherchez le **limiter le nombre de connexions** stratégie.

  - Si le paramètre pour cette stratégie est **désactivé**, puis de la stratégie de groupe n’est pas limiter les connexions RDP.
  - Si le paramètre pour cette stratégie est **activé**, puis vérifiez **GPO gagnante**. Si vous avez besoin supprimer ou modifier la limite de connexions, modifiez cet objet de stratégie de groupe.

Pour appliquer les modifications de stratégie, ouvrez une fenêtre d’invite de commandes sur l’ordinateur affecté et entrez la commande suivante :
  
```
gpupdate /force
```
  
## <a name="rd-client-disconnects-and-cannot-reconnect-to-the-same-session"></a>Client Bureau à distance se déconnecte et ne peut pas se reconnecter à la même session

Une fois que le client Bureau à distance perd sa connexion au Bureau à distance, le client ne peut pas se reconnecter immédiatement. L’utilisateur reçoit des messages d’erreur semblable à celui-ci :

  - Le client n’a pas pu se connecter au serveur Terminal Server en raison d’une erreur de sécurité. Après avoir vérifié que vous êtes connecté au réseau, essayez à nouveau de vous connecter au serveur.
  - Bureau à distance déconnecté. En raison d’une erreur de sécurité, le client n’a pas pu se connecter à l’ordinateur distant. Vérifiez que vous êtes connecté au réseau et essayez de vous reconnecter.

Ultérieurement, le client Bureau à distance se reconnecte au Bureau à distance. Toutefois, plutôt que le client se connecte à la session d’origine, le serveur RDSH connecte le client à une nouvelle session. Le serveur RDSH révèle que la session d’origine n’a pas entré un état déconnecté, mais au lieu de cela reste active.

Pour contourner ce problème, vous pouvez activer le **intervalle de connexion keep-alive configurer** stratégie dans le **Configuration ordinateur\\modèles d’administration\\composants Windows\\ Services Bureau à distance\\hôte de Session Bureau à distance\\connexions** dossier de stratégie de groupe. Si vous activez cette stratégie, vous devez entrer un intervalle de conservation. L’intervalle Keepalive détermine la fréquence, en minutes, le serveur vérifie l’état de session.

Ce problème peut être dû à une configuration incorrecte de vos paramètres d’authentification et de configuration. Vous pouvez configurer ces paramètres au niveau du serveur, ou à l’aide de stratégie de groupe. Les paramètres de stratégie de groupe sont disponibles dans le **Configuration ordinateur\\modèles d’administration\\les composants Windows\\Services Bureau à distance\\hôte de Session Bureau à distance\\ Sécurité** dossier de stratégie de groupe.

1. Sur le serveur hôte de session Bureau à distance, ouvrez Configuration d'hôte de session Bureau à distance.
2. Sous **connexions**, cliquez sur le nom de la connexion, puis cliquez sur **propriétés**.
3. Dans le **propriétés** boîte de dialogue pour la connexion, sur le **général** sous l’onglet **sécurité** de couche, sélectionnez une méthode de sécurité.
4. Dans **niveau de chiffrement**, cliquez sur le niveau que vous voulez. Vous pouvez sélectionner **faible**, **Compatible Client**, **haute**, ou **compatible FIPS**.

> [!NOTE]  
>  - Lorsque les communications entre les clients et serveurs hôtes de Session Bureau à distance nécessitent le niveau de cryptage le plus élevé, utilisez le chiffrement compatible FIPS.
>  - Les paramètres de niveau de chiffrement que vous configurez dans la stratégie de groupe remplacent la configuration que vous définissez à l’aide de l’outil de Configuration de Services Bureau à distance. En outre, si vous activez le [cryptographie système : Utilisez des algorithmes compatibles FIPS pour le chiffrement, le hachage et la signature](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc780081\(v=ws.10\)) stratégie, ce paramètre remplace le **définir le niveau de chiffrement de connexion client** stratégie. La stratégie de chiffrement système se trouve dans le **Configuration ordinateur\\Windows paramètres\\paramètres de sécurité\\stratégies locales\\Options de sécurité** dossier.
>  - Lorsque vous modifiez le niveau de chiffrement, le nouveau niveau de chiffrement prend effet lors de la prochaine connexion d'un utilisateur. Si vous avez besoin de plusieurs niveaux de chiffrement sur un seul serveur, installez plusieurs cartes réseau et configurez chaque carte séparément.
>  - Pour vérifier que le certificat a une clé privée correspondante, dans la Configuration de Services Bureau à distance, cliquez sur la connexion pour laquelle vous souhaitez afficher le certificat, sélectionnez **général**, sélectionnez **modifier**, sélectionnez le certificat que vous souhaitez afficher, puis sélectionnez **afficher le certificat**. En bas de la **général** , onglet de l’instruction, « vous avez une clé privée qui correspond à ce certificat » doit apparaître. Vous pouvez également afficher ces informations à l’aide du composant logiciel enfichable Certificats.
>  - Chiffrement compatible FIPS (le **cryptographie système : Des algorithmes compatibles FIPS d’utilisation pour le chiffrement, le hachage et la signature** stratégie ou le **compatible FIPS** définition dans la Configuration de serveur Bureau à distance) chiffre et déchiffre les données envoyées à partir du client au serveur et à partir de le serveur au client, avec les algorithmes de chiffrement de 140-1 FIPS Federal Information Processing Standard (), à l’aide de modules cryptographiques Microsoft. Pour plus d’informations, consultez [Validation FIPS 140](https://docs.microsoft.com/en-us/windows/security/threat-protection/fips-140-validation).
>  - Le **haute** paramètre crypte les données envoyées du client vers le serveur et à partir du serveur au client à l’aide d’un chiffrement renforcé 128 bits.
>  - Le **Compatible Client** paramètre crypte les données envoyées entre le client et le serveur à la puissance de clé maximale prise en charge par le client.
>  - Le **faible** paramètre crypte les données envoyées du client au serveur à l’aide du cryptage de 56 bits.

## <a name="remote-laptop-disconnects-from-wireless-network"></a>Ordinateur portable à distance se déconnecte de réseau sans fil

Ce problème peut se produire lorsqu’un client Bureau à distance se connecte à un ordinateur portable en utilisant un réseau sans fil 802. 1 x. Par intermittence, l’ordinateur portable se déconnecte du réseau sans fil et ne se reconnecte pas automatiquement.

Il s’agit d’un problème connu qui se produit lorsque le paramètre d’authentification réseau pour la connexion réseau sans fil est **l’authentification utilisateur**.

Pour contourner ce problème, la valeur est le paramètre d’authentification réseau **l’authentification utilisateur ou ordinateur** ou **l’authentification d’ordinateur**.

 > [!NOTE]  
> Pour modifier les paramètres d’authentification réseau sur un seul ordinateur, vous devrez peut-être utiliser le Centre réseau et partage le panneau de configuration pour créer une nouvelle connexion sans fil avec les nouveaux paramètres.

Pour obtenir une description complète de la configuration des paramètres de réseau sans fil à l’aide de la stratégie de groupe, consultez [des stratégies de configuration de réseau sans fil (IEEE 802.11)](https://docs.microsoft.com/en-us/windows-server/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies).

## <a name="user-experiences-poor-performance-or-application-problems"></a>Utilisateur rencontre des performances médiocres ou des problèmes d’application

Cette section traite de plusieurs problèmes courants que les utilisateurs peuvent rencontrer lorsqu’ils utilisent les fonctionnalités de bureau à distance :

  - [Les utilisateurs qui se connectent à de nouvelles machines virtuelles Microsoft Azure par intermittence rencontrent des problèmes](#intermittent-problems-with-new-microsoft-azure-virtual-machines).
  - [Les utilisateurs qui se connectent aux bureaux à distance de Windows 10 version 1709 devront peut-être des problèmes de lecture vidéo](#video-playback-issues-on-windows-10-version-1709).
  - [Les utilisateurs avec des profils utilisateur en lecture seule qui se connectent aux bureaux à distance de Windows 10 ne peuvent pas partager leur bureau.](#desktop-sharing-issues-on-windows-10)
  - [Utilisateurs qui se connectent aux bureaux à distance de Windows 10 à l’aide de clients qui s’exécutent sur des versions antérieures de Windows 10 performances sont faibles lors de l’authentification NLA est désactivé.](#performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled)
  - [Lorsque les utilisateurs exécutent plusieurs applications dans des sessions Bureau à distance et vous déconnecter et vous reconnecter fréquemment, vous pouvez rencontrer un écran noir lors de la reconnexion.](#black-screen-issue)

### <a name="intermittent-problems-with-new-microsoft-azure-virtual-machines"></a>Problèmes intermittents avec les nouvelles machines virtuelles Microsoft Azure

Ce problème concerne les machines virtuelles qui ont été récemment configurés. Une fois que l’utilisateur se connecte à la machine virtuelle, la session Bureau à distance ne peut pas charger tous les paramètres de l’utilisateur correctement.

Pour contourner ce problème, vous déconnecter de la machine virtuelle, attendez au moins 20 minutes, puis reconnectez-vous.

Pour résoudre ce problème, appliquez les mises à jour suivantes pour les machines virtuelles, comme il convient :

  - Windows 10 et Windows Server 2016 : Ko 4343884, [le 30 août 2018 — KB4343884 (Build du système d’exploitation 14393.2457)](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)
  - Windows Server 2012 R2 : Ko 4343891, [le 30 août 2018 — KB4343891 (version préliminaire du correctif mensuel)](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)

### <a name="video-playback-issues-on-windows-10-version-1709"></a>Problèmes de lecture vidéo sur Windows 10 version 1709

Ce problème se produit lorsque les utilisateurs se connectent à des ordinateurs distants qui exécutent Windows 10, version 1709. Lorsque ces utilisateurs lire la vidéos à l’aide de la VMR9 codec (vidéo mélange convertisseur 9), le joueur ne montre qu’une fenêtre de noir.

Il s’agit d’un problème connu dans Windows 10, version 1709. Le problème ne se produit pas dans Windows 10 version 1703.

### <a name="desktop-sharing-issues-on-windows-10"></a>Problèmes sur Windows 10 de partage de bureau

Ce problème se produit lorsque l’utilisateur a un profil utilisateur en lecture seule (et ruche du Registre associé), comme dans un scénario de plein écran. Lorsqu’un tel utilisateur se connecte à un ordinateur distant qui exécute Windows 10, version 1803, ils ne peuvent pas partager leur bureau.

Pour résoudre ce problème, appliquez la mise à jour Windows 10 4340917, [24 juillet 2018 — KB4340917 (17134.191 de Build du système d’exploitation)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917).

### <a name="performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled"></a>Problèmes de performances lorsque vous combinez des versions de Windows 10 si NLA est désactivé.

Ce problème se produit lorsque l’authentification NLA est désactivé, et les ordinateurs client Bureau à distance qui exécutent Windows 10 se connecter aux bureaux à distance qui exécutent des versions différentes de Windows 10. Les utilisateurs de clients de bureau à distance sur les ordinateurs qui exécutent Windows 10, version 1709 ou des versions antérieures performances sont faibles lorsqu’ils se connectent à des bureaux à distance qui exécutent Windows 10, version 1803 ou une version ultérieure.

Cela se produit parce que, lorsque l’authentification NLA est désactivé, les ordinateurs clients plus anciens utilisent un protocole plus lent lorsqu’ils se connectent à Windows 10, version 1803 ou une version ultérieure.

Pour résoudre ce problème, appliquez 4340917 Ko, [24 juillet 2018 — KB4340917 (17134.191 de Build du système d’exploitation)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917).

### <a name="black-screen-issue"></a>Problème de l’écran noir

Ce problème se produit dans Windows 8.0, Windows 8.1, Windows 10 RTM et Windows Server 2012 R2. Un utilisateur lance plusieurs applications dans un bureau à distance, puis se déconnecte de la session. Périodiquement, l’utilisateur se reconnecte au Bureau à distance pour interagir avec les applications et puis déconnecter à nouveau. À un moment donné, lorsque l’utilisateur se reconnecte, la session Bureau à distance affiche simplement un écran noir. L’utilisateur doit se terminer la session Bureau à distance à partir de la console de l’ordinateur distant (ou la console du serveur RDSH). Cette action arrête les applications.

Pour résoudre ce problème, appliquez les mises à jour suivantes comme il convient :

  - Windows 8 et Windows Server 2012 : KB4103719, [le 17 mai 2018 — KB4103719 (version préliminaire du correctif mensuel)](https://support.microsoft.com/en-us/help/4103719/windows-server-2012-update-kb4103719)
  - Windows 8.1 et Windows Server 2012 R2 : KB4103724, [le 17 mai 2018 — KB4103724 (version préliminaire du correctif mensuel)](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724) et Ko 4284863, [le 21 juin 2018 — KB4284863 (version préliminaire du correctif mensuel)](https://support.microsoft.com/en-us/help/4284863/windows-81-update-kb4284863)
  - Windows 10 : Résolu dans KB4284860, [le 12 juin 2018 — KB4284860 (Build du système d’exploitation 10240.17889)](https://support.microsoft.com/en-us/help/4284860/windows-10-update-kb4284860)
