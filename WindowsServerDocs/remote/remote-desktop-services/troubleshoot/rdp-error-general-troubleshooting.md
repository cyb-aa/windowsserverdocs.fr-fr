---
title: Résolution de problèmes de connexion Bureau à distance d’ordre général
description: Résolution de l’erreur « Classe non enregistrée » avec une connexion Bureau à distance.
audience: itpro
ms.custom: na
ms.reviewer: rklemen
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: kaushika-msft
manager: ''
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 39b11dac044c38f1ae80d4401fbb66af0317ab56
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870703"
---
# <a name="general-remote-desktop-connection-troubleshooting"></a>Résolution de problèmes de connexion Bureau à distance d’ordre général

Suivez ces étapes quand un client Bureau à distance ne peut pas se connecter à un Bureau à distance, mais ne fournit pas de messages ou ne présente pas d’autres symptômes qui aideraient à en identifier la cause.

## <a name="check-the-status-of-the-rdp-protocol"></a>Vérifier l’état du protocole RDP

### <a name="check-the-status-of-the-rdp-protocol-on-a-local-computer"></a>Vérifier l’état du protocole RDP sur un ordinateur local

Pour vérifier et changer l’état du protocole RDP sur un ordinateur local, consultez [Comment activer le Bureau à distance](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access#how-to-enable-remote-desktop).

> [!NOTE]  
> Si les options de Bureau à distance ne sont pas disponibles, consultez [Vérifier si un objet de stratégie de groupe bloque le protocole RDP](#check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer).

### <a name="check-the-status-of-the-rdp-protocol-on-a-remote-computer"></a>Vérifier l’état du protocole RDP sur un ordinateur distant

> [!IMPORTANT]  
> Suivez attentivement les instructions de cette section. Une modification incorrecte du Registre peut entraîner de graves problèmes. Avant de commencer à le modifier, [sauvegardez le Registre](https://support.microsoft.com/help/322756) afin de pouvoir le restaurer en cas de problème.

Pour vérifier et changer l’état du protocole RDP sur un ordinateur distant, utilisez une connexion au Registre réseau :

1. Tout d’abord, dans le menu **Démarrer**, sélectionnez **Exécuter**. Dans la zone de texte qui s’affiche, entrez **regedt32**.
2. Dans l’Éditeur du Registre, sélectionnez **Fichier**, puis **Connexion au Registre réseau**.
3. Dans la boîte de dialogue **Sélectionner un ordinateur**, entrez le nom de l’ordinateur distant, sélectionnez **Vérifier les noms**, puis **OK**.
4. Accédez à **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server**.  
   ![Éditeur du Registre montrant l’entrée fDenyTSConnections](../media/troubleshoot-remote-desktop-connections/RegEntry_fDenyTSConnections.png)
   - Si la valeur de la clé **fDenyTSConnections** est **0**, le protocole RDP est activé.
   - Si la valeur de la clé **fDenyTSConnections** est **1**, le protocole RDP est désactivé.
5. Pour activer le protocole RDP, remplacez la valeur **1** de **fDenyTSConnections** par **0**.

### <a name="check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer"></a>Vérifier si un objet de stratégie de groupe bloque le protocole RDP sur un ordinateur local

Si vous ne pouvez pas activer le protocole RDP dans l’interface utilisateur ou si vous avez modifié la valeur de **fDenyTSConnections** et qu’elle est revenue à **1**, il se peut qu’un objet de stratégie de groupe remplace les paramètres au niveau de l’ordinateur.

Pour vérifier la configuration de stratégie de groupe sur un ordinateur local, ouvrez une fenêtre d’invite de commandes en tant qu’administrateur, puis entrez la commande suivante :

```cmd
gpresult /H c:\gpresult.html
```

Une fois cette commande terminée, ouvrez gpresult.html. Dans **Configuration ordinateur\\Modèles d’administration\\Composants Windows\\Services Bureau à distance\\Hôte de session Bureau à distance\\Connexions**, recherchez la stratégie **Autoriser les utilisateurs à se connecter à distance à l’aide des services Bureau à distance**.

- Si le paramètre de cette stratégie est **Activé**, la stratégie de groupe ne bloque pas les connexions RDP.
- Si le paramètre de cette stratégie est **Désactivé**, vérifiez **OSG gagnant**. Il s’agit de l’objet de stratégie de groupe qui bloque les connexions RDP.
  ![Exemple de segment de gpresult.html, dans lequel l’objet de stratégie de groupe au niveau du domaine **Bloquer RDP** désactive le protocole RDP.](../media/troubleshoot-remote-desktop-connections/GPResult_RDSH_Connections_GP.png)
   
  ![Exemple de segment de gpresult.html, dans lequel **Stratégie de groupe locale** désactive le protocole RDP.](../media/troubleshoot-remote-desktop-connections/GPResult_RDSH_Connections_LGP.png)

### <a name="check-whether-a-gpo-is-blocking-rdp-on-a-remote-computer"></a>Vérifier si un objet de stratégie de groupe bloque le protocole RDP sur un ordinateur distant

Pour vérifier la configuration de stratégie de groupe sur un ordinateur distant, la commande est presque identique à celle pour un ordinateur local :

```cmd
gpresult /S <computer name> /H c:\gpresult-<computer name>.html
```

Le fichier généré par cette commande (**gpresult-\<nom_ordinateur\>.html**) utilise le même format d’informations que la version pour ordinateur local (**gpresult.html**).

### <a name="modifying-a-blocking-gpo"></a>Modification d’un objet de stratégie de groupe bloquant

Vous pouvez modifier ces paramètres dans l’Éditeur d’objets de stratégie de groupe et dans la Console de gestion des stratégies de groupe (GPMC). Pour plus d’informations sur l’utilisation de la stratégie de groupe, consultez [Advanced Group Policy Management](https://docs.microsoft.com/microsoft-desktop-optimization-pack/agpm/).

Pour modifier la stratégie bloquante, appliquez l’une des méthodes suivantes :

- Dans l’Éditeur de stratégies de groupe, accédez au niveau d’objet de stratégie de groupe approprié (par exemple local ou domaine), puis accédez à **Configuration ordinateur** > **Modèles d’administration** > **Composants Windows** > **Services Bureau à distance** > **Hôte de session Bureau à distance** > **Connexions** > **Autoriser les utilisateurs à se connecter à distance à l’aide des services Bureau à distance**.  
   1. Affectez la valeur **Activé** ou **Non configuré** à la stratégie.
   2. Sur les ordinateurs affectés, ouvrez une fenêtre d’invite de commandes en tant qu’administrateur et exécutez la commande **gpupdate /force**.
- Dans la Console de gestion des stratégies de groupe, accédez à l’unité d’organisation dans laquelle la stratégie bloquante est appliquée aux ordinateurs affectés, et supprimez cette stratégie de l’unité d’organisation.

## <a name="check-the-status-of-the-rdp-services"></a>Vérifier l’état des services RDP

Sur l’ordinateur local (client) et l’ordinateur distant (cible), les services suivants doivent être en cours d’exécution :

- Services Bureau à distance (TermService)
- Redirecteur de port du mode utilisateur des services Bureau à distance (UmRdpService)

Vous pouvez utiliser le composant logiciel enfichable MMC Services pour gérer les services localement ou à distance. Vous pouvez également utiliser PowerShell pour gérer les services localement ou à distance (si l’ordinateur distant est configuré pour accepter les applets de commande PowerShell à distance).

![Services Bureau à distance dans le composant logiciel enfichable MMC Services. Ne modifiez pas les paramètres de service par défaut.](../media/troubleshoot-remote-desktop-connections/RDSServiceStatus.png)

Sur l’un ou l’autre ordinateur, si l’un des services n’est pas en cours d’exécution, démarrez-le.

> [!NOTE]  
> Si vous démarrez le service Services Bureau à distance, cliquez sur **Oui** pour redémarrer automatiquement le service Redirecteur de port du mode utilisateur des services Bureau à distance.

## <a name="check-that-the-rdp-listener-is-functioning"></a>Vérifier que l’écouteur RDP fonctionne

> [!IMPORTANT]  
> Suivez attentivement les instructions de cette section. Une modification incorrecte du Registre peut entraîner de graves problèmes. Avant de commencer à le modifier, [sauvegardez le Registre](https://support.microsoft.com/help/322756) afin de pouvoir le restaurer en cas de problème.

### <a name="check-the-status-of-the-rdp-listener"></a>Vérifier l’état de l’écouteur RDP

Pour cette procédure, utilisez une instance de PowerShell qui dispose d’autorisations d’administration. Pour un ordinateur local, vous pouvez également utiliser une invite de commandes qui dispose d’autorisations d’administration. Toutefois, cette procédure utilise PowerShell, car les mêmes applets de commande fonctionnent à la fois localement et à distance.

1. Pour vous connecter à un ordinateur distant, exécutez l’applet de commande suivante :

   ```powershell
   Enter-PSSession -ComputerName <computer name>
   ```

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
   1. Pour sauvegarder l’entrée de Registre existante, entrez l’applet de commande suivante :  
   
      ```powershell  
      cmd /c 'reg export "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp" C:\Rdp-tcp-backup.reg'   
      ```  
   

   2. Pour supprimer l’entrée de Registre existante, entrez les applets de commande suivantes :  
   
      ```powershell  
      Remove-Item -path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp' -Recurse -Force  
      ```
   
   3. Pour importer la nouvelle entrée de Registre et redémarrer le service, entrez les applets de commande suivantes :  
   
      ```powershell  
      cmd /c 'regedit /s c:\<filename>.reg'  
      Restart-Service TermService -Force  
      ```
      
      Remplacez \<filename\> par le nom du fichier .reg exporté.

6. Testez la configuration en réessayant la connexion Bureau à distance. Si vous ne pouvez toujours pas vous connecter, redémarrez l’ordinateur affecté.
7. Si vous ne pouvez toujours pas vous connecter, [vérifiez le statut du certificat auto-signé RDP](#check-the-status-of-the-rdp-self-signed-certificate).

### <a name="check-the-status-of-the-rdp-self-signed-certificate"></a>Vérifier l’état du certificat auto-signé RDP

1. Si vous ne pouvez toujours pas vous connecter, ouvrez le composant logiciel enfichable MMC Certificats. Quand vous êtes invité à sélectionner le magasin de certificats à gérer, sélectionnez **Compte d’ordinateur**, puis sélectionnez l’ordinateur affecté.
2. Dans le dossier **Certificats** sous **Bureau à distance**, supprimez le certificat auto-signé RDP. 
    ![Certificats de Bureau à distance dans le composant logiciel enfichable MMC Certificats.](../media/troubleshoot-remote-desktop-connections/MMCCert_Delete.png)
3. Sur l’ordinateur affecté, redémarrez le service Services Bureau à distance.
4. Actualisez le composant logiciel enfichable Certificats.
5. Si le certificat auto-signé RDP n’a pas été recréé, [vérifiez les autorisations du dossier MachineKeys](#check-the-permissions-of-the-machinekeys-folder).

### <a name="check-the-permissions-of-the-machinekeys-folder"></a>Vérifier les autorisations du dossier MachineKeys

1. Sur l’ordinateur affecté, ouvrez l’Explorateur, puis accédez à **C:\\ProgramData\\Microsoft\\Crypto\\RSA\\** .
2. Cliquez avec le bouton droit sur **MachineKeys**, sélectionnez **Propriétés**, **Sécurité**, puis **Avancé**.
3. Vérifiez que les autorisations suivantes sont configurées :
      - Builtin\\Administrateurs : Contrôle total
      - Tout le monde : Lecture, Écriture

## <a name="check-the-rdp-listener-port"></a>Vérifier le port d’écoute RDP

Sur l’ordinateur local (client) et l’ordinateur distant (cible), l’écouteur RDP doit être à l’écoute sur le port 3389. Aucune autre application ne doit utiliser ce port.

> [!IMPORTANT]  
> Suivez attentivement les instructions de cette section. Une modification incorrecte du Registre peut entraîner de graves problèmes. Avant de commencer à le modifier, [sauvegardez le Registre](https://support.microsoft.com/help/322756) afin de pouvoir le restaurer en cas de problème.

Pour vérifier ou changer le port RDP, utilisez l’Éditeur du Registre :

1. Accédez au menu Démarrer, sélectionnez **Exécuter**, puis entrez **regedt32** dans la zone de texte qui s’affiche.
      - Pour vous connecter à un ordinateur distant, sélectionnez **Fichier**, puis **Connexion au Registre réseau**.
      - Dans la boîte de dialogue **Sélectionner un ordinateur**, entrez le nom de l’ordinateur distant, sélectionnez **Vérifier les noms**, puis **OK**.
2. Ouvrez le Registre et accédez à **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<listener\>** . 
    ![La sous-clé PortNumber pour le protocole RDP.](../media/troubleshoot-remote-desktop-connections/RegEntry_PortNumber.png)
3. Si **PortNumber** a une valeur autre que **3389**, remplacez-la par **3389**. 
   > [!IMPORTANT]  
    > Vous pouvez utiliser les services Bureau à distance à l’aide d’un autre port. Toutefois, nous vous déconseillons de le faire. Cet article n’explique pas comment résoudre les problèmes avec ce type de configuration.
4. Après avoir changé le numéro de port, redémarrez le service Services Bureau à distance.

### <a name="check-that-another-application-isnt-trying-to-use-the-same-port"></a>Vérifier qu’aucune autre application ne tente d’utiliser le même port

Pour cette procédure, utilisez une instance de PowerShell qui dispose d’autorisations d’administration. Pour un ordinateur local, vous pouvez également utiliser une invite de commandes qui dispose d’autorisations d’administration. Toutefois, cette procédure utilise PowerShell, car les mêmes applets de commande fonctionnent localement et à distance.

1. Ouvrez une fenêtre PowerShell. Pour vous connecter à un ordinateur distant, entrez **Enter-PSSession -ComputerName \<nom_ordinateur\>** .
2. Entrez la commande suivante :  
   
     ```powershell  
    cmd /c 'netstat -ano | find "3389"'  
    ```
  
    ![La commande netstat génère une liste de ports et les services qui sont à l’écoute sur ces ports.](../media/troubleshoot-remote-desktop-connections/WPS_netstat.png)
3. Recherchez une entrée pour le port TCP 3389 (ou le port RDP attribué) associé à l'état **Écoute**. 
    > [!NOTE]  
   > L’identificateur de processus (PID, process identifier) du processus ou service utilisant ce port apparaît sous la colonne PID.
4. Pour déterminer quelle application utilise le port 3389 (ou le port RDP attribué), entrez la commande suivante :  
   
     ```powershell  
    cmd /c 'tasklist /svc | find "<pid listening on 3389>"'  
    ```  
  
    ![La commande tasklist fournit des détails sur un processus spécifique.](../media/troubleshoot-remote-desktop-connections/WPS_tasklist.png)
5. Recherchez une entrée pour le numéro PID associé au port (dans la sortie de **netstat**). Les services ou processus associés à ce PID apparaissent dans la colonne de droite.
6. Si une application ou un service autre que les Services Bureau à distance (TermServ.exe) utilise le port, vous pouvez résoudre le conflit en appliquant l’une des méthodes suivantes :
      - Configurer l’autre application ou service pour utiliser un autre port (recommandé).
      - Désinstaller l’autre application ou service.
      - Configurer le protocole RDP pour utiliser un autre port, puis redémarrer le service Services Bureau à distance (non recommandé).

### <a name="check-whether-a-firewall-is-blocking-the-rdp-port"></a>Vérifier si un pare-feu bloque le port RDP

Utilisez l’outil **psping** pour tester si vous pouvez accéder à l’ordinateur affecté à l’aide du port 3389.

1. Accédez à un autre ordinateur qui n’est pas affecté et téléchargez **psping** à l’adresse <https://live.sysinternals.com/psping.exe>.
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
