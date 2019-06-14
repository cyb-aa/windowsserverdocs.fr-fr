---
title: Étapes courantes de résolution des problèmes de Windows Admin Center
description: Étapes courantes de résolution des problèmes de Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 06/07/2019
ms.openlocfilehash: 8e718eda7859c5e0b6949829c225b28e882525ad
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811710"
---
# <a name="troubleshooting-windows-admin-center"></a>Résolution des problèmes de Windows Admin Center

> S’applique à : Windows Admin Center, version préliminaire de Windows Admin Center

> [!Important]
> Ce guide vous aidera à diagnostiquer et à résoudre les problèmes qui vous empêchent d’utiliser Windows Admin Center. Si vous rencontrez un problème avec un outil spécifique, vérifiez s'il s'agit d'un [problème connu.](http://aka.ms/wacknownissues)

## <a name="installer-fails-with-message-the-module-microsoftpowershelllocalaccounts-could-not-be-loaded"></a>Programme d’installation échoue avec le message : **_Le Module 'Microsoft.PowerShell.LocalAccounts' n’a pas pu être chargé._ **

Cela peut se produire si votre chemin du module PowerShell par défaut a été modifié ou supprimé. Pour résoudre ce problème, assurez-vous que ```%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules``` est la **premier** élément dans votre variable d’environnement PSModulePath. Vous pouvez y parvenir avec la ligne suivante de PowerShell :

```powershell
[Environment]::SetEnvironmentVariable("PSModulePath","%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules;" + ([Environment]::GetEnvironmentVariable("PSModulePath","User")),"User")
```

## <a name="i-get-a-this-sitepage-cant-be-reached-error-in-my-web-browser"></a>Je reçois une erreur **Impossible d’atteindre ce site/cette page** dans mon navigateur web

### <a name="if-youve-installed-windows-admin-center-as-an-app-on-windows-10"></a>Si vous avez installé Windows Admin Center sous la forme d’une **application sur Windows 10**

* Vérifiez que Windows Admin Center est en cours d’exécution. Recherchez l’icône Windows Admin Center ![](../media/trayIcon.PNG) dans la barre d’état système ou **Windows Admin Center Desktop / SmeDesktop.exe** dans le Gestionnaire des tâches. Si ce n’est pas le cas, lancez **Windows Admin Center** à partir du menu Démarrer.

> [!NOTE] 
> Après avoir redémarré, vous devez lancer Windows Admin Center à partir du menu Démarrer.  

* [Vérifier la version de Windows](#check-the-windows-version)

* Assurez-vous que vous utilisez Microsoft Edge ou Google Chrome comme navigateur web.

* Avez-vous sélectionné le bon certificat lors du [premier lancement ?](../use/get-started.md#selecting-a-client-certificate)

  * Essayez d’ouvrir votre navigateur dans une session privée. Si cela fonctionne, vous devez effacer votre cache.

* Récemment mise à jour Windows 10 vers une nouvelle build ou une version ?

  * Cela peut avoir effacé vos paramètres d’hôtes approuvés. [Suivez ces instructions pour mettre à jour vos paramètres d’hôtes approuvés.](#configure-trustedhosts)

### <a name="if-youve-installed-windows-admin-center-as-a-gateway-on-windows-server"></a>Si vous avez installé Windows Admin Center sous la forme d'une **passerelle sur Windows Server**

* Vous mettez à niveau à partir d’une version précédente de Windows Admin Center ? Vérifiez que la règle de pare-feu n’a pas été supprimée en raison [ce problème](known-issues.md#upgrade). Utilisez la commande PowerShell suivante pour déterminer si la règle existe. Dans le cas contraire, suivez [ces instructions](known-issues.md#upgrade) pour la recréer.
    
    ```powershell
    Get-NetFirewallRule -DisplayName "SmeInboundOpenException"
    ```

* [Vérifiez la version Windows](#check-the-windows-version) du client et du serveur.

* Assurez-vous que vous utilisez Microsoft Edge ou Google Chrome comme navigateur web.

* Sur le serveur, ouvrez le Gestionnaire des tâches > Services et vérifiez que **ServerManagementGateway / Windows Admin Center** est en cours d’exécution.
![](../media/Service-TaskMan.PNG)

* Tester la connexion réseau à la passerelle (remplacez \<valeurs > avec les informations de votre déploiement)

    ```powershell
    Test-NetConnection -Port <port> -ComputerName <gateway> -InformationLevel Detailed
    ```

### <a name="if-you-have-installed-windows-admin-center-in-an-azure-windows-server-vm"></a>Si vous avez installé Windows Admin Center dans une machine virtuelle Azure Windows Server

* [Vérifier la version de Windows](#check-the-windows-version)
* Avez-vous ajouté une règle de port d’entrée pour HTTPS ? 
* [En savoir plus sur l’installation de Windows Admin Center dans une machine virtuelle Azure](https://docs.microsoft.com/windows-server/manage/windows-admin-center/configure/azure-integration#use-a-windows-admin-center-gateway-deployed-in-azure)

### <a name="check-the-windows-version"></a>Vérifier la version de Windows

* Ouvrez la boîte de dialogue Exécuter (touche Windows + R) et lancez ```winver```.

* Si vous utilisez Windows 10 version 1703 ou antérieure, Windows Admin Center n’est pas pris en charge sur votre version de Microsoft Edge. Mettez à niveau vers une version récente de Windows 10 ou utilisez Chrome.

* Si vous utilisez une version préliminaire d’insider de Windows 10 ou Server avec une version de build entre 17134 et 17637, Windows avait un bogue qui a provoqué le Windows Admin Center échec. Utilisez une version prise en charge actuelle de Windows.

### <a name="make-sure-the-windows-remote-management-winrm-service-is-running-on-both-the-gateway-machine-and-managed-node"></a>Assurez-vous que le service Windows Remote Management (WinRM) est en cours d’exécution sur l’ordinateur passerelle et le nœud géré

* Ouvrez la boîte de dialogue exécution avec la touche Windows + R
* Type ```services.msc``` et appuyez sur entrée
* Dans la fenêtre qui s’ouvre, recherchez la gestion à distance de Windows (WinRM), assurez-vous qu’il est en cours d’exécution et configuré pour démarrer automatiquement

### <a name="did-you-upgrade-your-server-from-2016-to-2019"></a>Vous mettez à niveau votre serveur à partir de 2016 à 2019 ?

* Cela peut avoir effacé vos paramètres d’hôtes approuvés. [Suivez ces instructions pour mettre à jour vos paramètres d’hôtes approuvés.](#configure-trustedhosts) 

## <a name="i-get-the-message-cant-connect-securely-to-this-page-this-might-be-because-the-site-uses-outdated-or-unsafe-tls-security-settings"></a>J’obtiens le message : « Impossible de se connecter en toute sécurité à cette page. Cela peut être, car le site utilise des paramètres de sécurité TLS obsolètes ou potentiellement dangereux.

<!--REF: https://docs.microsoft.com/iis/get-started/whats-new-in-iis-10/http2-on-iis#when-is-http2-not-supported -->
Votre ordinateur est limité pour les connexions HTTP/2. Windows Admin Center utilise l’authentification Windows intégrée, ce qui n’est pas pris en charge dans HTTP/2. Ajoutez les deux valeurs suivantes sous la ```HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Http\Parameters``` clé sur **l’ordinateur qui exécute le navigateur** pour retirer la restriction de HTTP/2 :

```
EnableHttp2Cleartext=dword:00000000
EnableHttp2Tls=dword:00000000
```

## <a name="im-having-trouble-with-the-remote-desktop-events-and-powershell-tools"></a>J’ai des problèmes avec les outils de bureau à distance, les événements et PowerShell.

Ces trois outils requièrent le protocole websocket, qui est fréquemment bloqué par les serveurs proxy et pare-feu. Si vous utilisez Google Chrome, il existe un [problème connu](known-issues.md#google-chrome) avec le protocole WebSocket et l’authentification NTLM.

## <a name="i-can-connect-to-some-servers-but-not-others"></a>Je peux me connecter à certains serveurs, mais pas à d’autres

* Ouvrez une session sur l’ordinateur de la passerelle localement et essayez ```Enter-PSSession <machine name>``` dans PowerShell, en remplaçant \<nom_machine > par le nom de l’ordinateur que vous voulez gérer dans Windows Admin Center. 

* Si votre environnement utilise un groupe de travail au lieu d’un domaine, voir [utilisation de Windows Admin Center dans un groupe de travail](#using-windows-admin-center-in-a-workgroup).

* **À l’aide de comptes d’administrateur local :** Si vous utilisez un compte d’utilisateur local qui n’est pas le compte administrateur intégré, vous devez activer la stratégie sur l’ordinateur cible en exécutant la commande suivante dans PowerShell ou à une invite de commandes en tant qu’administrateur sur l’ordinateur cible :

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

- **Windows 10**

    ```powershell
    Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any
    ```

### <a name="configure-trustedhosts"></a>Configurer TrustedHosts

Lorsque vous installez Windows Admin Center, vous avez la possibilité de laisser Windows Admin Center gérer le paramètre TrustedHosts de la passerelle. Cela est nécessaire dans un environnement de groupe de travail, ou lors de l’utilisation d'informations d’identification d’administrateur local dans un domaine. Si vous choisissez de renoncer à ce paramètre, vous devez configurer TrustedHosts manuellement.

**Pour modifier les hôtes approuvés à l’aide des commandes PowerShell :**

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
   >     Set-Item WSMan:\localhost\Client\TrustedHosts -Value '*'

4. Lorsque vous avez terminé le test, vous pouvez émettre la commande suivante à partir d’une session PowerShell avec élévation de privilèges pour effacer vos paramètres TrustedHosts :

    ```powershell
    Clear-Item WSMan:localhost\Client\TrustedHosts
    ```

5. Si vous avez précédemment exporté vos paramètres, ouvrez le fichier, copiez les valeurs et utilisez cette commande :

    ```powershell
    Set-Item WSMan:localhost\Client\TrustedHosts -Value '<paste values from text file>'
    ```

## <a name="i-previously-had-windows-admin-center-installed-and-now-nothing-else-can-use-the-same-tcpip-port"></a>J’avais Windows Admin Center installé, et rien d’autre pouvez maintenant utiliser le même port TCP/IP

Exécuter manuellement ces deux commandes dans une invite de commandes avec élévation de privilèges :

```cmd
netsh http delete sslcert ipport=0.0.0.0:443
netsh http delete urlacl url=https://+:443/
```

## <a name="azure-features-dont-work-properly-in-edge"></a>Fonctionnalités Azure ne fonctionnent pas correctement dans Edge

Edge a [problèmes connus](https://github.com/AzureAD/azure-activedirectory-library-for-js/wiki/Known-issues-on-Edge) associées aux zones de sécurité qui affectent la connexion Azure dans Windows Admin Center. Si vous ne parvenez pas à l’aide des fonctionnalités Azure lorsque vous utilisez Edge, essayez d’ajouter https://login.microsoftonline.com, https://login.live.com et l’URL de votre passerelle en tant que sites de confiance et à des sites autorisés pour les paramètres de bloqueur de fenêtres publicitaires de bord de votre navigateur de côté client. 

Pour ce faire :
1. Recherchez **Options Internet** dans le Windows Menu Démarrer
2. Accédez à la **sécurité** onglet
3. Sous le **Sites de confiance** option, cliquez sur le **sites** bouton et ajoutez les URL dans la boîte de dialogue qui s’ouvre. Vous devez ajouter l’URL de votre passerelle, ainsi que https://login.microsoftonline.com et https://login.live.com.
4. Accédez à la **confidentialité** onglet
5. Sous le **bloqueur de fenêtres publicitaires** section, cliquez sur le **paramètres** bouton et ajoutez les URL dans la boîte de dialogue qui s’ouvre. Vous devez ajouter l’URL de votre passerelle, ainsi que https://login.microsoftonline.com et https://login.live.com.

## <a name="having-an-issue-with-an-azure-related-feature"></a>Vous rencontrez un problème avec une fonctionnalité liés à Azure ?

Veuillez nous envoyer un e-mail à wacFeedbackAzure@microsoft.com avec les informations suivantes :
* Informations relatives au problème général à partir de la [questions répertoriées ci-dessous](#providing-feedback-on-issues).
* Décrivez votre problème et les étapes que vous avez suivies pour reproduire le problème. 
* Précédemment inscrire votre passerelle vers Azure en utilisant le script téléchargeable New-AadApp.ps1 et puis mettre à niveau vers la version 1807 ? Ou vous n’avez inscrit votre passerelle vers Azure à l’aide de l’interface utilisateur à partir de la passerelle Paramètres > Azure ?
* Est votre compte Azure associé à plusieurs répertoires/clients ?
    * Si Oui : Lorsque vous inscrivez l’application Azure AD pour Windows Admin Center, a été le répertoire que vous avez utilisé votre répertoire par défaut dans Azure ? 
* Votre compte Azure a accès à plusieurs abonnements ?
* L’abonnement que vous utilisiez a-t-elle attachée de facturation ?
* Ont été vous connecté à plusieurs comptes Azure lorsque vous avez rencontré le problème ?
* Votre compte Azure requiert-il l’authentification multifacteur ?
* Est l’ordinateur que vous essayez de gérer une machine virtuelle Azure ?
* Windows Admin Center est installé sur une machine virtuelle Azure ?

## <a name="providing-feedback-on-issues"></a>Envoi de commentaires sur les problèmes

Accédez à l’Observateur d’événements > Applications et Services > Microsoft-ServerManagementExperience et recherchez les erreurs ou les avertissements.

Sur notre [UserVoice](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5BBug%5D) entrez un bogue qui décrit le problème.

Merci d’indiquer les erreurs ou avertissements que vous trouvez dans le journal des événements, ainsi que les informations suivantes : 

* Plateforme sur laquelle Windows Admin Center est **installé** (Windows 10 ou Windows Server) :
    * Si installé sur le serveur, ce qui est le Windows [version](#check-the-windows-version) de **l’ordinateur qui exécute le navigateur** pour accéder à Windows Admin Center : 
    * Vous utilisez le certificat auto-signé créé par le programme d’installation ?
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

