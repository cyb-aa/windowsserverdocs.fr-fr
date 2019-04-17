---
title: Étapes courantes de résolution des problèmes de Windows Admin Center
description: Étapes courantes de résolution des problèmes de Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 02/12/2019
ms.openlocfilehash: a91a8dcf6f05ef0ef66dee603851150b2145d559
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296958"
---
# Résolution des problèmes de Windows Admin Center

>S’applique à: Windows Admin Center, Windows Admin Center Preview

> [!Important]
> Ce guide vous aidera à diagnostiquer et à résoudre les problèmes qui vous empêchent d’utiliser Windows Admin Center. Si vous rencontrez un problème avec un outil spécifique, vérifiez s'il s'agit d'un [problème connu.](http://aka.ms/wacknownissues)

<a id="toc"></a>

## Liens rapides

* [Le programme d’installation échoue avec le message: ** _le Module «Microsoft.PowerShell.LocalAccounts» n’a pas pu être chargé._**](#psmodulepath)

* Je reçois une erreur **Impossible d’atteindre ce site/cette page** dans mon navigateur web (sélectionnez votre type de déploiement)
    * [J’ai installé Windows Admin Center sous la forme d’une application sur Windows10](#whitescreenw10)
    * [J’ai installé Windows Admin Center sous la forme d’une passerelle sur Windows Server](#whitescreenws)
    * [J’ai installé Windows Admin Center sous la forme d’une passerelle sur une machine virtuelle Azure](#whitescreenazvm)

* [Le chargement de page d’accueil de Windows Admin Center, mais je suis bloqué dans le volet Ajouter une connexion, ou je ne parviens pas à se connecter à n’importe quel ordinateur.](#winvercompat)

* [Je reçois le message: «Error while loading the module. Rpc: Expired retries 'Ping'."](#winvercompat)

* [Je reçois le message: «Impossible de se connecter en toute sécurité à cette page. Cela peut être dans la mesure où le site utilise les paramètres de sécurité TLS obsolètes ou dangereux.»](#tls)

* [Je rencontre des problèmes avec les outils de bureau à distance, les événements et PowerShell.](#websockets)

* [Je rencontre des problèmes à l’aide de fonctionnalités Azure dans Edge](#azlogin)

* [Je peux me connecter à certains serveurs, mais pas à d’autres](#connectionissues)

* [J’utilise Windows Admin Center dans un **groupe de travail**](#workgroup)

* [J’ai réalisé précédemment installé Windows Admin Center et rien d’autre pouvez désormais utiliser le même port TCP/IP](#urlacl)

* [Mon problème n’est pas répertorié ici, ou les étapes de cette page n’ont pas résolu mon problème.](#filebug)

<a id="psmodulepath"></a>

## Programme d’installation échoue avec le message: ** _le Module «Microsoft.PowerShell.LocalAccounts» n’a pas pu être chargé._**

Cela peut se produire si votre chemin par défaut du module PowerShell a été modifié ou supprimé. Pour résoudre ce problème, assurez-vous que l’option ```%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules``` est le **premier** élément dans la variable d’environnement PSModulePath. Vous pouvez y parvenir avec la ligne de PowerShell suivante:

```powershell
[Environment]::SetEnvironmentVariable("PSModulePath","%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules;" + ([Environment]::GetEnvironmentVariable("PSModulePath","User")),"User")
```

## Je reçois une erreur **Impossible d’atteindre ce site/cette page** dans mon navigateur web

<a id="whitescreenw10"></a>

### Si vous avez installé Windows Admin Center sous la forme d’une **application sur Windows10**

* Vérifiez que Windows Admin Center est en cours d’exécution. Recherchez l’icône de Windows Admin Center ![](../media/trayIcon.PNG) dans la barre d’état système ou **Windows Admin Center Desktop / SmeDesktop.exe** dans le Gestionnaire des tâches. Si ce n’est pas le cas, lancez **Windows Admin Center** à partir du menu Démarrer.

> [!NOTE] 
> Après avoir redémarré, vous devez lancer Windows Admin Center à partir du menu Démarrer.  

* [Vérifier la version de Windows](#winvercompat)

* Assurez-vous que vous utilisez Microsoft Edge ou Google Chrome comme navigateur web.

* Avez-vous sélectionné le bon certificat lors du [premier lancement?](../use/get-started.md#selecting-a-client-certificate)

  * Essayez d’ouvrir votre navigateur dans une session privée. Si cela fonctionne, vous devez effacer votre cache.

* Récemment mise à niveau Windows 10 à une nouvelle version ou une version?

  * Ce champ peut avoir désactivé vos paramètres d’hôtes approuvés. [Suivez ces instructions pour mettre à jour vos paramètres d’hôtes approuvés.](#configure-trustedhosts) 

[[retour au début]](#toc)

<a id="whitescreenws"></a>

### Si vous avez installé Windows Admin Center sous la forme d'une **passerelle sur Windows Server**

* Mise à jour à partir d’une version précédente de Windows Admin Center? Vérifiez que la règle de pare-feu n’a pas été supprimée en raison de [ce problème connu](known-issues.md#upgrade). Utilisez la commande PowerShell ci-dessous pour déterminer si la règle existe. Dans le cas contraire, suivez [ces instructions](known-issues.md#upgrade) pour le recréer.
    ```powershell
    Get-NetFirewallRule -DisplayName "SmeInboundOpenException"
    ```

* [Vérifiez la version Windows](#winvercompat) du client et du serveur.

* Assurez-vous que vous utilisez Microsoft Edge ou Google Chrome comme votre navigateur web.

* Sur le serveur, ouvrez le Gestionnaire des tâches > Services et vérifiez que **ServerManagementGateway / Windows Admin Center** est en cours d’exécution.
![](../media/Service-TaskMan.PNG)

* Testez la connexion réseau vers la passerelle (remplacez \<values> par les informations de votre déploiement)
    ```powershell
    Test-NetConnection -Port <port> -ComputerName <gateway> -InformationLevel Detailed
    ```

[[retour au début]](#toc)

<a id="whitescreenazvm"></a>  

### Si vous avez installé Windows Admin Center dans une machine virtuelle Azure Windows Server

* [Vérifier la version de Windows](#winvercompat)
* Avez-vous ajouté une règle de port d’entrée pour HTTPS? 
* [En savoir plus sur l’installation de Windows Admin Center dans une machine virtuelle Azure](https://docs.microsoft.com/windows-server/manage/windows-admin-center/configure/azure-integration#use-a-windows-admin-center-gateway-deployed-in-azure)

[[retour au début]](#toc)

<a id="winvercompat"></a>

### Vérifier la version de Windows

* Ouvrez la boîte de dialogue Exécuter (touche Windows + R) et lancez ```winver```.

* Si vous utilisez Windows10 version1703 ou antérieure, Windows Admin Center n’est pas pris en charge sur votre version de Microsoft Edge. Mettez à niveau vers une version récente de Windows10 ou utilisez Chrome.

* Si vous utilisez une version d’évaluation insider de Windows 10 ou Server avec un numéro de version entre 17134 et 17637, Windows avait un bogue qui a provoqué Windows Admin Center échec. Veuillez utiliser une version prise en charge actuelle de Windows.

### Assurez-vous que le service Windows Remote Management (WinRM) est en cours d’exécution sur l’ordinateur passerelle et le nœud géré

* Ouvrez la boîte de dialogue Exécuter avec la touche Windows + R
* Type de ```services.msc``` et appuyez sur entrée
* Dans la fenêtre qui s’ouvre, recherchez Windows Remote Management (WinRM), assurez-vous qu’il est en cours d’exécution et configuré pour démarrer automatiquement

### Vous mettre à niveau votre serveur 2016 vers 2019?

* Ce champ peut avoir désactivé vos paramètres d’hôtes approuvés. [Suivez ces instructions pour mettre à jour vos paramètres d’hôtes approuvés.](#configure-trustedhosts) 

[[retour au début]](#toc)

<a id="tls"></a>

## Je reçois le message: «Impossible de se connecter en toute sécurité à cette page. Cela peut être dans la mesure où le site utilise les paramètres de sécurité TLS obsolètes ou dangereux.

<!--REF: https://docs.microsoft.com/iis/get-started/whats-new-in-iis-10/http2-on-iis#when-is-http2-not-supported -->
Votre ordinateur est limitée aux connexions HTTP/2. Windows Admin Center utilise l’authentification Windows intégrée, qui n’est pas pris en charge dans HTTP/2. Ajoutez les valeurs de deux Registre suivantes sous le ```HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Http\Parameters``` clés sur **l’ordinateur qui exécute le navigateur** pour supprimer la restriction HTTP/2:

```
EnableHttp2Cleartext=dword:00000000
EnableHttp2Tls=dword:00000000
```

[[retour au début]](#toc)

<a id="websockets"></a> 

## Je rencontre des problèmes avec les outils de bureau à distance, les événements et PowerShell.

Ces trois outils nécessitent le protocole websocket, qui est généralement bloqué par les serveurs proxy et de pare-feu. Si vous utilisez Google Chrome, il existe un [problème connu](known-issues.md#google-chrome) avec le protocole WebSocket et l’authentification NTLM.

[[retour au début]](#toc)


<a id="connectionissues"></a> 

## Je peux me connecter à certains serveurs, mais pas à d’autres
* Ouvrez une session sur l’ordinateur passerelle en locale et essayez l'opération ```Enter-PSSession <machine name>``` dans PowerShell, en remplaçant \<machine name> par le nom de l’ordinateur que vous essayez de gérer dans Windows Admin Center. 

* Si votre environnement utilise un groupe de travail au lieu d’un domaine, voir [utilisation de Windows Admin Center dans un groupe de travail](#workgroup).

* **Utilisation de comptes d’administrateur local:** si vous utilisez un compte d’utilisateur local différent du compte d'administrateur intégré, vous devez activer la stratégie sur l’ordinateur cible en exécutant la commande suivante dans PowerShell ou sur une invite de commandes en tant qu’administrateur sur l’ordinateur cible:

        REG ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1


[[retour au début]](#toc)

<a id="workgroup"></a>

## Utilisation de Windows Admin Center dans un groupe de travail 

### Quel compte utilisez-vous?
Assurez-vous que les informations d’identification que vous utilisez sont membres du groupe des administrateurs locaux du serveur cible. Dans certains cas, WinRM requiert également l’appartenance au groupe des Utilisateurs de gestion à distance. Si vous utilisez un compte d’utilisateur local **différent du compte d’administrateur intégré**, vous devez activer la stratégie sur l’ordinateur cible en exécutant la commande suivante dans PowerShell ou sur une invite de commandes en tant qu’administrateur sur l’ordinateur cible:

```cmd
REG ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1
```
### Vous connectez-vous à un ordinateur de groupe de travail sur un sous-réseau différent?

Pour vous connecter à un ordinateur de groupe de travail qui n’est pas sur le même sous-réseau que la passerelle, assurez-vous que le port du pare-feu pour WinRM (TCP 5985) autorise le trafic entrant sur l’ordinateur cible. Vous pouvez exécuter la commande suivante dans PowerShell ou sur une invite de commandes en tant qu’administrateur sur l’ordinateur cible pour créer cette règle de pare-feu:

- **WindowsServer**

    ```powershell
    Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any
    ```

- **Windows10**

    ```powershell
    Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any
    ```

### Configurer TrustedHosts

Lorsque vous installez Windows Admin Center, vous avez la possibilité de laisser Windows Admin Center gérer le paramètre TrustedHosts de la passerelle. Cela est nécessaire dans un environnement de groupe de travail, ou lors de l’utilisation d'informations d’identification d’administrateur local dans un domaine. Si vous choisissez de renoncer à ce paramètre, vous devez configurer TrustedHosts manuellement.

**Pour modifier TrustedHosts à l’aide des commandes PowerShell:**

1. Ouvrez une session PowerShell administrateur.
2. Afficher vos paramètres TrustedHosts actuels:

    ```powershell
    Get-Item WSMan:\localhost\Client\TrustedHosts
    ```

    > [!WARNING]
    > Si la valeur actuelle de votre TrustedHosts n’est pas vide, les commandes ci-dessous remplacent le paramètre. Nous vous recommandons d’enregistrer la valeur actuelle dans un fichier texte avec la commande suivante afin de pouvoir la restaurer si nécessaire:

    > `Get-Item WSMan:localhost\Client\TrustedHosts | Out-File C:\OldTrustedHosts.txt`

3. Définissez TrustedHosts sur le NetBIOS, l'adresse IP ou le nom de domaine complet des ordinateurs que vous souhaitez gérer:

    ```powershell
    Set-Item WSMan:localhost\Client\TrustedHosts -Value '192.168.1.1,server01.contoso.com,server02'
    ```

    > [!TIP] 
    >Pour définir facilement tous les TrustedHosts à la fois, vous pouvez utiliser un caractère générique.

    >     Set-Item WSMan:\localhost\Client\TrustedHosts -Value '*'

4. Lorsque vous avez terminé le test, vous pouvez émettre la commande suivante à partir d’une session PowerShell avec élévation de privilèges pour effacer vos paramètres TrustedHosts:

    ```powershell
    Clear-Item WSMan:localhost\Client\TrustedHosts
    ```

5. Si vous avez précédemment exporté vos paramètres, ouvrez le fichier, copiez les valeurs et utilisez cette commande:

    ```powershell
    Set-Item WSMan:localhost\Client\TrustedHosts -Value '<paste values from text file>'
    ```

[[retour au début]](#toc)

<a id="urlacl"></a>

## J’ai réalisé précédemment installé Windows Admin Center et rien d’autre pouvez désormais utiliser le même port TCP/IP

Exécutez manuellement ces deux commandes dans une invite de commandes avec élévation de privilèges:

```cmd
netsh http delete sslcert ipport=0.0.0.0:443
netsh http delete urlacl url=https://+:443/
```

[[retour au début]](#toc)

<a id="azlogin"></a>

## Je rencontre des problèmes à l’aide de fonctionnalités Azure dans Edge

Edge a [des problèmes connus](https://github.com/AzureAD/azure-activedirectory-library-for-js/wiki/Known-issues-on-Edge) liés à des zones de sécurité qui affectent la connexion Azure dans Windows Admin Center. Si vous rencontrez des difficultés à l’aide de fonctionnalités Azure lorsque vous utilisez Edge, essayez d’ajouter https://login.microsoftonline.com, https://login.live.com et l’URL de votre passerelle en tant que sites de confiance et à des sites autorisés pour les paramètres de bloqueur de fenêtres contextuelles Edge sur votre navigateur de côté client. 

Pour cela, procédez comme suit:
1. Rechercher des **Options Internet** dans le Menu Démarrer de Windows
2. Accédez à l’onglet **sécurité**
3. Sous l’option de **Sites de confiance** , cliquez sur le bouton **sites** et ajoutez les URL dans la boîte de dialogue qui s’ouvre. Vous devez ajouter l’URL de votre passerelle ainsi que https://login.microsoftonline.com et https://login.live.com.
4. Accédez à l’onglet **confidentialité**
5. Dans la section **Bloqueur de fenêtres contextuelles** , cliquez sur le bouton **paramètres** et ajoutez les URL dans la boîte de dialogue qui s’ouvre. Vous devez ajouter l’URL de votre passerelle ainsi que https://login.microsoftonline.com et https://login.live.com.


[[retour au début]](#toc)

<a id="azissue"></a>

## Vous rencontrez un problème avec une fonctionnalité liées à Azure?

Veuillez nous envoyer un e-mail à wacFeedbackAzure@microsoft.com avec les informations suivantes:
* Informations sur les problèmes généraux à partir du [questions répertoriées ci-dessous](#filebug). 
* Décrire votre problème et les étapes que vous avez effectué pour reproduire le problème. 
* Précédemment inscrire votre passerelle vers Azure à l’aide du script téléchargeable New-AadApp.ps1 et mettez à niveau vers la version 1807? Ou vous inscrire votre passerelle vers Azure à l’aide de l’interface utilisateur à partir de la passerelle paramètres > Azure?
* Votre compte Azure associé à plusieurs répertoires/locataires est-il?
    * Si Oui: lors de l’inscription de l’application Azure AD pour Windows Admin Center, a été le répertoire que vous avez utilisé votre répertoire par défaut dans Azure? 
* Votre compte Azure dispose d’un accès à plusieurs abonnements?
* L’abonnement que vous utilisiez dispose-t-il de facturation attachée?
* Ont été vous connecté à plusieurs comptes Azure lorsque vous avez rencontré le problème?
* Votre compte Azure a besoin d’une authentification multifacteur?
* Est l’ordinateur que vous essayez de gérer une machine virtuelle Azure?
* Windows Admin Center est installé sur une machine virtuelle Azure?

[[retour au début]](#toc)

<a id="filebug"></a>

## Fonctionne toujours pas ou est votre problème ne pas cité ici? [questions courantes de résolution des problèmes]

Accédez à l’Observateur d’événements > Applications et Services > Microsoft-ServerManagementExperience et recherchez les erreurs ou les avertissements.

Sur notre [UserVoice](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5BBug%5D) entrez un bogue qui décrit le problème.

Merci d’indiquer les erreurs ou avertissements que vous trouvez dans le journal des événements, ainsi que les informations suivantes: 

* Plateforme sur laquelle Windows Admin Center est **installé** (Windows10 ou Windows Server):
    * Si installée sur le serveur, ce qui est la [version](#winvercompat) de Windows **l’ordinateur qui exécute le navigateur** pour accéder à Windows Admin Center: 
    * Vous utilisez le certificat auto-signé créé par le programme d’installation?
    * Si vous utilisez votre propre certificat, le nom du sujet correspond-il à l’ordinateur?
    * Si vous utilisez votre propre certificat, spécifie-t-il un autre nom de sujet?
* L'avez-vous installé avec le paramètre de port par défaut?
    * Si ce n’est pas le cas, quel port avez-vous spécifié?
* L’ordinateur sur lequel Windows Admin Center est **installé** est-il joint à un domaine?
* Windows [version](#winvercompat) sur lequel Windows Admin Center est **installé**:
* L'ordinateur que vous **essayez de gérer** est-il joint à un domaine?
* Windows [version](#winvercompat) de l’ordinateur que vous **essayez de gérer**:
* Quel navigateur utilisez-vous?
    * Si vous utilisez Google Chrome, quelle est sa version? (Aide > À propos de Google Chrome)

[[retour au début]](#toc)