---
title: Étapes courantes de résolution des problèmes de Windows Admin Center
description: Étapes courantes de résolution des problèmes de Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 06/07/2019
ms.openlocfilehash: 5df216d8c7b829a6c60db4e5d771824a7bacdb47
ms.sourcegitcommit: 2a15de216edde8b8e240a4aa679dc6d470e4159e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77465323"
---
# <a name="troubleshooting-windows-admin-center"></a>Résolution des problèmes de Windows Admin Center

> S’applique à : Centre d’administration Windows, version préliminaire du centre d’administration Windows

> [!Important]
> Ce guide vous aidera à diagnostiquer et à résoudre les problèmes qui vous empêchent d’utiliser Windows Admin Center. Si vous rencontrez un problème avec un outil spécifique, vérifiez s'il s'agit d'un [problème connu.](https://aka.ms/wacknownissues)

## <a name="installer-fails-with-message-_the-module-microsoftpowershelllocalaccounts-could-not-be-loaded_"></a>Le programme d’installation échoue avec le message :  **_le module « Microsoft. PowerShell. LocalAccounts » n’a pas pu être chargé._**

Cela peut se produire si le chemin d’accès de votre module PowerShell par défaut a été modifié ou supprimé. Pour résoudre le problème, assurez-vous que ```%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules``` est le **premier** élément de votre variable d’environnement PSModulePath. Pour ce faire, vous pouvez utiliser la ligne de commande PowerShell suivante :

```powershell
[Environment]::SetEnvironmentVariable("PSModulePath","%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules;" + ([Environment]::GetEnvironmentVariable("PSModulePath","User")),"User")
```

## <a name="i-get-a-this-sitepage-cant-be-reached-error-in-my-web-browser"></a>Je reçois une erreur **Impossible d’atteindre ce site/cette page** dans mon navigateur web

### <a name="if-youve-installed-windows-admin-center-as-an-app-on-windows-10"></a>Si vous avez installé Windows Admin Center sous la forme d’une **application sur Windows 10**

* Vérifiez que Windows Admin Center est en cours d’exécution. Recherchez l’icône du centre d’administration Windows ![](../media/trayIcon.PNG) dans la barre d’état système ou le **Bureau du centre d’administration Windows/SmeDesktop. exe** dans le gestionnaire des tâches. Si ce n’est pas le cas, lancez **Windows Admin Center** à partir du menu Démarrer.

> [!NOTE] 
> Après avoir redémarré, vous devez lancer Windows Admin Center à partir du menu Démarrer.  

* [Vérifier la version de Windows](#check-the-windows-version)

* Assurez-vous que vous utilisez Microsoft Edge ou Google Chrome comme navigateur web.

* Avez-vous sélectionné le bon certificat lors du [premier lancement ?](../use/get-started.md#selecting-a-client-certificate)

  * Essayez d’ouvrir votre navigateur dans une session privée. Si cela fonctionne, vous devez effacer votre cache.

* Avez-vous récemment mis à niveau Windows 10 vers une nouvelle build ou version ?

  * Cela peut avoir effacé les paramètres des hôtes approuvés. [Suivez ces instructions pour mettre à jour vos paramètres d’hôtes approuvés.](#configure-trustedhosts)

### <a name="if-youve-installed-windows-admin-center-as-a-gateway-on-windows-server"></a>Si vous avez installé Windows Admin Center sous la forme d'une **passerelle sur Windows Server**

* [Vérifiez la version Windows](#check-the-windows-version) du client et du serveur.

* Assurez-vous que vous utilisez Microsoft Edge ou Google Chrome comme navigateur web.

* Sur le serveur, ouvrez le gestionnaire des tâches > Services et assurez-vous que le **Centre d’administration ServerManagementGateway/Windows** est en cours d’exécution.
![](../media/Service-TaskMan.PNG)

* Tester la connexion réseau à la passerelle (remplacez \<valeurs > par les informations de votre déploiement)

    ```powershell
    Test-NetConnection -Port <port> -ComputerName <gateway> -InformationLevel Detailed
    ```

### <a name="if-you-have-installed-windows-admin-center-in-an-azure-windows-server-vm"></a>Si vous avez installé le centre d’administration Windows dans une machine virtuelle Azure Windows Server

* [Vérifier la version de Windows](#check-the-windows-version)
* Avez-vous ajouté une règle de port d’entrée pour HTTPS ? 
* [En savoir plus sur l’installation du centre d’administration Windows dans une machine virtuelle Azure](https://docs.microsoft.com/windows-server/manage/windows-admin-center/configure/azure-integration#use-a-windows-admin-center-gateway-deployed-in-azure)

### <a name="check-the-windows-version"></a>Vérifier la version de Windows

* Ouvrez la boîte de dialogue Exécuter (touche Windows + R) et lancez ```winver```.

* Si vous utilisez Windows 10 version 1703 ou antérieure, Windows Admin Center n’est pas pris en charge sur votre version de Microsoft Edge. Mettez à niveau vers une version récente de Windows 10 ou utilisez Chrome.

* Si vous utilisez une version d’Insider preview de Windows 10 ou Server avec une version de build comprise entre 17134 et 17637, Windows avait un bogue qui provoquait l’échec du centre d’administration Windows. Utilisez une version actuelle de Windows prise en charge.

### <a name="make-sure-the-windows-remote-management-winrm-service-is-running-on-both-the-gateway-machine-and-managed-node"></a>Assurez-vous que le service Windows Remote Management (WinRM) est en cours d’exécution sur l’ordinateur de passerelle et le nœud géré.

* Ouvrir la boîte de dialogue exécuter avec WindowsKey + R
* Tapez ```services.msc``` et appuyez sur entrée.
* Dans la fenêtre qui s’ouvre, recherchez Windows Remote Management (WinRM), assurez-vous qu’il est en cours d’exécution et qu’il est configuré pour démarrer automatiquement

### <a name="did-you-upgrade-your-server-from-2016-to-2019"></a>Avez-vous mis à niveau votre serveur de 2016 à 2019 ?

* Cela peut avoir effacé les paramètres des hôtes approuvés. [Suivez ces instructions pour mettre à jour vos paramètres d’hôtes approuvés.](#configure-trustedhosts) 

## <a name="i-get-the-message-cant-connect-securely-to-this-page-this-might-be-because-the-site-uses-outdated-or-unsafe-tls-security-settings"></a>J’obtiens le message suivant : «impossible de se connecter en toute sécurité à cette page. Cela peut être dû au fait que le site utilise des paramètres de sécurité TLS obsolètes ou non sécurisés.

Votre ordinateur est limité aux connexions HTTP/2. Le centre d’administration Windows utilise l’authentification Windows intégrée, qui n’est pas prise en charge dans HTTP/2. Ajoutez les deux valeurs de Registre suivantes sous la clé ```HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Http\Parameters``` sur **l’ordinateur exécutant le navigateur** pour supprimer la restriction http/2 :

```
EnableHttp2Cleartext=dword:00000000
EnableHttp2Tls=dword:00000000
```

## <a name="im-having-trouble-with-the-remote-desktop-events-and-powershell-tools"></a>Je rencontre des problèmes avec les Bureau à distance, les événements et les outils PowerShell.

Ces trois outils nécessitent le protocole WebSocket, qui est généralement bloqué par les pare-feu et les serveurs proxy. Si vous utilisez Google Chrome, il existe un [problème connu](known-issues.md#google-chrome) avec les WebSockets et l’authentification NTLM.

## <a name="i-can-connect-to-some-servers-but-not-others"></a>Je peux me connecter à certains serveurs, mais pas à d’autres

* Connectez-vous à la machine de passerelle localement et essayez d' ```Enter-PSSession <machine name>``` dans PowerShell, en remplaçant \<nom de l’ordinateur > par le nom de l’ordinateur que vous essayez de gérer dans le centre d’administration Windows. 

* Si votre environnement utilise un groupe de travail au lieu d’un domaine, voir [utilisation de Windows Admin Center dans un groupe de travail](#using-windows-admin-center-in-a-workgroup).

* **Utilisation de comptes d’administrateur local :** si vous utilisez un compte d’utilisateur local différent du compte d'administrateur intégré, vous devez activer la stratégie sur l’ordinateur cible en exécutant la commande suivante dans PowerShell ou sur une invite de commandes en tant qu’administrateur sur l’ordinateur cible :

    ```
    REG ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1
    ```

## <a name="using-windows-admin-center-in-a-workgroup"></a>Utilisation de Windows Admin Center dans un groupe de travail

### <a name="what-account-are-you-using"></a>Quel compte utilisez-vous ?
Assurez-vous que les informations d’identification que vous utilisez sont membres du groupe des administrateurs locaux du serveur cible. Dans certains cas, WinRM requiert également l’appartenance au groupe des Utilisateurs de gestion à distance. Si vous utilisez un compte d’utilisateur local **différent du compte d’administrateur intégré**, vous devez activer la stratégie sur l’ordinateur cible en exécutant la commande suivante dans PowerShell ou sur une invite de commandes en tant qu’administrateur sur l’ordinateur cible :

```cmd
REG ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1
```
### <a name="are-you-connecting-to-a-workgroup-machine-on-a-different-subnet"></a>Vous connectez-vous à un ordinateur de groupe de travail sur un sous-réseau différent ?

Pour vous connecter à un ordinateur de groupe de travail qui n’est pas sur le même sous-réseau que la passerelle, assurez-vous que le port du pare-feu pour WinRM (TCP 5985) autorise le trafic entrant sur l’ordinateur cible. Vous pouvez exécuter la commande suivante dans PowerShell ou sur une invite de commandes en tant qu’administrateur sur l’ordinateur cible pour créer cette règle de pare-feu :

- **Windows Server**

    ```powershell
    Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any
    ```

- **Windows 10**

    ```powershell
    Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any
    ```

### <a name="configure-trustedhosts"></a>Configurer TrustedHosts

Lorsque vous installez Windows Admin Center, vous avez la possibilité de laisser Windows Admin Center gérer le paramètre TrustedHosts de la passerelle. Cela est nécessaire dans un environnement de groupe de travail, ou lors de l’utilisation d'informations d’identification d’administrateur local dans un domaine. Si vous choisissez de renoncer à ce paramètre, vous devez configurer TrustedHosts manuellement.

**Pour modifier TrustedHosts à l’aide de commandes PowerShell :**

1. Ouvrez une session PowerShell administrateur.
2. Afficher vos paramètres TrustedHosts actuels :

    ```powershell
    Get-Item WSMan:\localhost\Client\TrustedHosts
    ```

   > [!WARNING]
   > Si la valeur actuelle de votre TrustedHosts n’est pas vide, les commandes ci-dessous remplacent le paramètre. Nous vous recommandons d’enregistrer la valeur actuelle dans un fichier texte avec la commande suivante afin de pouvoir la restaurer si nécessaire :
   > 
   > `Get-Item WSMan:localhost\Client\TrustedHosts | Out-File C:\OldTrustedHosts.txt`

3. Définissez TrustedHosts sur le NetBIOS, l'adresse IP ou le nom de domaine complet des ordinateurs que vous souhaitez gérer :

    ```powershell
    Set-Item WSMan:localhost\Client\TrustedHosts -Value '192.168.1.1,server01.contoso.com,server02'
    ```

   > [!TIP]
   > Pour définir facilement tous les TrustedHosts à la fois, vous pouvez utiliser un caractère générique.
   >
   > ```powershell
   > Set-Item WSMan:\localhost\Client\TrustedHosts -Value '*'
   > ```

4. Lorsque vous avez terminé le test, vous pouvez émettre la commande suivante à partir d’une session PowerShell avec élévation de privilèges pour effacer vos paramètres TrustedHosts :

    ```powershell
    Clear-Item WSMan:localhost\Client\TrustedHosts
    ```

5. Si vous avez précédemment exporté vos paramètres, ouvrez le fichier, copiez les valeurs et utilisez cette commande :

    ```powershell
    Set-Item WSMan:localhost\Client\TrustedHosts -Value '<paste values from text file>'
    ```

## <a name="i-previously-had-windows-admin-center-installed-and-now-nothing-else-can-use-the-same-tcpip-port"></a>Le centre d’administration Windows était déjà installé et, à présent, rien d’autre ne peut utiliser le même port TCP/IP

Exécutez manuellement ces deux commandes dans une invite de commandes avec élévation de privilèges :

```cmd
netsh http delete sslcert ipport=0.0.0.0:443
netsh http delete urlacl url=https://+:443/
```

## <a name="azure-features-dont-work-properly-in-edge"></a>Les fonctionnalités Azure ne fonctionnent pas correctement dans Edge

Edge présente des [problèmes connus](https://github.com/AzureAD/azure-activedirectory-library-for-js/wiki/Known-issues-on-Edge) liés aux zones de sécurité qui affectent la connexion à Azure dans le centre d’administration Windows. Si vous rencontrez des problèmes lors de l’utilisation des fonctionnalités Azure lors de l’utilisation de Edge, essayez d’ajouter https://login.microsoftonline.com, https://login.live.com et l’URL de votre passerelle en tant que sites approuvés et aux sites autorisés pour les paramètres du bloqueur de fenêtres publicitaires Edge sur votre navigateur côté client. 

Pour effectuer cette opération :
1. Rechercher des **Options Internet** dans le menu Démarrer de Windows
2. Accédez à l’onglet **sécurité** .
3. Sous l’option **sites de confiance** , cliquez sur le bouton **sites** et ajoutez les URL dans la boîte de dialogue qui s’ouvre. Vous devez ajouter votre URL de passerelle, ainsi que https://login.microsoftonline.com et https://login.live.com.
4. Accéder à l’onglet **confidentialité**
5. Dans la section **bloqueur de fenêtres publicitaires** , cliquez sur le bouton **paramètres** et ajoutez les URL dans la boîte de dialogue qui s’ouvre. Vous devez ajouter votre URL de passerelle, ainsi que https://login.microsoftonline.com et https://login.live.com.

## <a name="having-an-issue-with-an-azure-related-feature"></a>Vous rencontrez un problème avec une fonctionnalité Azure ?

Envoyez-nous un e-mail à wacFeedbackAzure@microsoft.com avec les informations suivantes :
* Informations générales sur les problèmes des [questions ci-dessous](#providing-feedback-on-issues).
* Décrivez votre problème et les étapes que vous avez suivies pour reproduire le problème. 
* Avez-vous déjà inscrit votre passerelle auprès d’Azure à l’aide du script téléchargeable New-AadApp. ps1, puis vous avez effectué une mise à niveau vers la version 1807 ? Ou avez-vous inscrit votre passerelle sur Azure à l’aide de l’interface utilisateur à partir des paramètres de la passerelle > Azure ?
* Votre compte Azure est-il associé à plusieurs annuaires/locataires ?
    * Si oui : lors de l’inscription de l’application Azure AD au centre d’administration Windows, le répertoire utilisé par défaut dans Azure est-il utilisé ? 
* Votre compte Azure peut-il accéder à plusieurs abonnements ?
* L’abonnement que vous utilisez a-t-il été lié à la facturation ?
* Vous êtes-vous connecté à plusieurs comptes Azure lorsque vous rencontrez le problème ?
* Votre compte Azure nécessite-t-il l’authentification multifacteur ?
* L’ordinateur que vous essayez de gérer est-il une machine virtuelle Azure ?
* Le centre d’administration Windows est-il installé sur une machine virtuelle Azure ?

## <a name="providing-feedback-on-issues"></a>Fournir des commentaires sur les problèmes

Accédez à l’Observateur d’événements > Applications et Services > Microsoft-ServerManagementExperience et recherchez les erreurs ou les avertissements.

Sur notre [UserVoice](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5BBug%5D) entrez un bogue qui décrit le problème.

Merci d’indiquer les erreurs ou avertissements que vous trouvez dans le journal des événements, ainsi que les informations suivantes : 

* Plateforme sur laquelle Windows Admin Center est **installé** (Windows 10 ou Windows Server) :
    * S’il est installé sur le serveur, quelle est la [version](#check-the-windows-version) Windows de **l’ordinateur exécutant le navigateur** pour accéder au centre d’administration Windows : 
    * Utilisez-vous le certificat auto-signé créé par le programme d’installation ?
    * Si vous utilisez votre propre certificat, le nom du sujet correspond-il à l’ordinateur ?
    * Si vous utilisez votre propre certificat, spécifie-t-il un autre nom de sujet ?
* L'avez-vous installé avec le paramètre de port par défaut ?
    * Si ce n’est pas le cas, quel port avez-vous spécifié ?
* L’ordinateur sur lequel Windows Admin Center est **installé** est-il joint à un domaine ?
* Windows [version](#check-the-windows-version) sur lequel Windows Admin Center est **installé** :
* L'ordinateur que vous **essayez de gérer** est-il joint à un domaine ?
* Windows [version](#check-the-windows-version) de l’ordinateur que vous **essayez de gérer** :
* Quel navigateur utilisez-vous ?
    * Si vous utilisez Google Chrome, quelle est sa version ? (Aide > À propos de Google Chrome)

