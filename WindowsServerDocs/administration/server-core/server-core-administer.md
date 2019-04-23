---
title: Administrer de Server Core
description: Apprenez à administrer une installation Server Core de Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.date: 12/18/2018
ms.openlocfilehash: 39fbb92645d39a46613f2142d0258c78a6ba425b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842660"
---
# <a name="administer-a-server-core-server"></a>Administrer un serveur Server Core

>S'applique à : Windows Server (canal semi-annuel) et Windows Server 2016

Étant donné que Server Core n’a pas une interface utilisateur, vous devez utiliser les applets de commande Windows PowerShell, les outils de ligne de commande ou les outils à distance pour effectuer des tâches d’administration fondamentales. Les sections suivantes décrivent les applets de commande PowerShell et les commandes utilisées pour les tâches de base. Vous pouvez également utiliser [Windows Admin Center](../../manage/windows-admin-center/overview.md), un portail de gestion unifiée actuellement en version préliminaire publique, pour administrer votre installation. 

## <a name="administrative-tasks-using-powershell-cmdlets"></a>Tâches d’administration à l’aide des applets de commande PowerShell
Utilisez les informations suivantes pour effectuer des tâches administratives de base avec les applets de commande Windows PowerShell.

### <a name="set-a-static-ip-address"></a>Définir une adresse IP statique
Lorsque vous installez un serveur Server Core, par défaut, elle a adresse un DHCP. Si vous avez besoin d’une adresse IP statique, vous pouvez le définir en procédant comme suit.

Pour afficher votre configuration réseau actuelle, utilisez **Get-NetIPConfiguration**.

Pour afficher les adresses IP que vous utilisez déjà, utilisez **Get-NetIPAddress**.

Pour définir une adresse IP statique, procédez comme suit : 

1. Exécutez **Get-NetIPInterface**. 
2. Notez le numéro dans le **IfIndex** colonne pour votre interface IP ou le **InterfaceDescription** chaîne. Si vous avez plusieurs cartes réseau, notez le nombre ou une chaîne correspondant à l’interface que vous souhaitez définir l’adresse IP statique.
3. Exécutez l’applet de commande suivante pour définir l’adresse IP statique :

   ```powershell
   New-NetIPaddress -InterfaceIndex 12 -IPAddress 192.0.2.2 -PrefixLength 24 -DefaultGateway 192.0.2.1
   ```

   où :
   - **InterfaceIndex** est la valeur de **IfIndex** à l’étape 2. (Dans notre exemple, 12)
   - **IPAddress** est l’adresse IP statique que vous souhaitez définir. (Dans notre exemple, 191.0.2.2)
   - **PrefixLength** est la longueur du préfixe (autre forme de masque de sous-réseau) pour l’adresse IP que vous définissez. (Pour notre exemple, 24)
   - **DefaultGateway** est l’adresse IP pour la passerelle par défaut. (Pour notre exemple, 192.0.2.1).
4. Exécutez l’applet de commande suivante pour définir le client DNS à l’adresse du serveur : 

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4
   ```
   
   où :
   - **InterfaceIndex** est la valeur de IfIndex de l’étape 2.
   - **ServerAddresses** est l’adresse IP de votre serveur DNS.
5. Pour ajouter plusieurs serveurs DNS, exécutez l’applet de commande suivante : 

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4,192.0.2.5
   ```

   WHERE, dans cet exemple, **192.0.2.4 et** et **192.0.2.5** sont les deux adresses IP des serveurs DNS.

Si vous avez besoin passer à l’aide de DHCP, exécutez **Set-DnsClientServerAddress – InterfaceIndex 12 – ResetServerAddresses**.

### <a name="join-a-domain"></a>Joindre un domaine
Utilisez les applets de commande suivantes pour joindre un ordinateur à un domaine.

1. Exécutez **Add-Computer**. Vous êtes invité pour les informations d’identification joindre le domaine et le nom de domaine.
2. Si vous devez ajouter un compte d’utilisateur de domaine au groupe Administrateurs local, exécutez la commande suivante à une invite de commandes (pas dans la fenêtre PowerShell) :

   ```
   net localgroup administrators /add <DomainName>\<UserName>
   ```
3. Redémarrez l’ordinateur. Vous pouvez le faire en exécutant **Restart-Computer**.

### <a name="rename-the-server"></a>Renommer le serveur
Utilisez les étapes suivantes pour renommer le serveur.

1. Recherchez le nom actuel du serveur à l’aide de la commande **hostname** ou **ipconfig** .
2. Exécutez **Rename-Computer - ComputerName \<nouveau_nom\>**.
3. Redémarrez l’ordinateur.

### <a name="activate-the-server"></a>Activer le serveur

Exécutez **slmgr.vbs – ipk\<productkey\>**. Puis exécutez **slmgr.vbs – ato**. Si l’activation réussit, vous n’obtiendrez un message.

> [!NOTE]
> Vous pouvez également activer le serveur par téléphone, à l’aide un [serveur du Service de gestion de clés (KMS)](../../get-started/server-2016-activation.md), ou à distance. Pour activer à distance, exécutez l’applet de commande suivante à partir d’un ordinateur distant : 

>```powershell
>**cscript windows\system32\slmgr.vbs <ServerName> <UserName> <password>:-ato**
>```
 
### <a name="configure-windows-firewall"></a>Configurer le Pare-feu Windows

Vous pouvez configurer le Pare-feu Windows localement sur le serveur en mode d’installation minimale à l’aide des applets de commande et des scripts de Windows PowerShell. Consultez [NetSecurity](/powershell/module/netsecurity/?view=win10-ps) pour les applets de commande que vous pouvez utiliser pour configurer le pare-feu de Windows.

### <a name="enable-windows-powershell-remoting"></a>Activer la communication à distance Windows PowerShell

Vous pouvez activer la communication à distance Windows PowerShell, qui permet d’exécuter sur un ordinateur des commandes qui ont été entrées dans Windows PowerShell sur un autre ordinateur. Activer la communication à distance de Windows PowerShell avec **Enable-PSRemoting**.

Pour plus d’informations, consultez [sur FAQ sur à distance](/powershell/module/microsoft.powershell.core/about/about_remote_faq?view=powershell-5.1)
</br>

## <a name="administrative-tasks-from-the-command-line"></a>Tâches administratives à partir de la ligne de commande
Utilisez les informations de référence suivantes pour effectuer des tâches administratives à partir de la ligne de commande.

### <a name="configuration-and-installation"></a>Configuration et installation
|Tâche | Command |
|-----|-------|
|Définir le mot de passe d’administrateur local| **administrateur de l’utilisateur NET** \* |
|Joindre un ordinateur à un domaine| **netdom join % Nom_Ordinateur %** **/Domain :\<domaine\> /userd :\<domaine\\nom d’utilisateur \> /passwordd :**\* <br> Redémarrez l’ordinateur.|
|Confirmer le changement de domaine| **set** |
|Supprimer un ordinateur d’un domaine|**Netdom remove \<Nom_Ordinateur\>**| 
|Ajouter un utilisateur au groupe Administrateurs local|**net localgroup Administrators / ajouter \<domaine\\nom d’utilisateur\>** |
|Supprimer un utilisateur du groupe Administrateurs local|**net localgroup administrateurs /delete \<domaine\\nom d’utilisateur\>** |
|Ajouter un utilisateur à l’ordinateur local|**utilisateur NET \<domaine\nom d’utilisateur\> * / ajouter** |
|Ajouter un groupe à l’ordinateur local|**net localgroup \<group name\> /add**|
|Renommer un ordinateur joint au domaine|**netdom renamecomputer % Nom_Ordinateur % / newname :\<nouveau nom d’ordinateur\> /userd :\<domaine\\nom d’utilisateur \> /passwordd :** * |
|Confirmer le nouveau nom de l’ordinateur|**set**| 
|Renommer un ordinateur dans un groupe de travail|**netdom renamecomputer \<Nom_ordinateur_actuel\> /newname :\<newcomputername\>** <br>Redémarrez l’ordinateur.|
|Désactiver la gestion des fichiers de pagination|**wmic computersystem where name="\<computername\>" set AutomaticManagedPagefile=False**| 
|Configurer un fichier de pagination|**WMIC pagefileset où nom = «\<chemin d’accès/nom de fichier\>« définir InitialSize =\<initialsize\>, MaximumSize =\<maxsize\>** <br>Où *chemin d’accès/nom de fichier* est le chemin d’accès et le nom du fichier de pagination, *initialsize* est la taille de départ du fichier d’échange, en octets, et *maxsize* est la taille maximale de la fichier d’échange, en octets.|
|Modifier une adresse IP statique|**ipconfig /all** <br>Enregistrer les informations pertinentes ou rediriger vers un fichier texte (**ipconfig/all > ipconfig.txt**).<br>**interfaces de netsh interface ipv4 show**<br>Vérifiez qu’il existe une liste des interfaces.<br>**netsh interface ipv4 définir le nom de l’adresse \<ID à partir de la liste des interfaces\> source = adresse statique =\<IP adresse principale\> passerelle =\<adresse de passerelle\>**<br>Exécutez **ipconfig/all** à vierfy DHCP activé est défini sur **non**.|
|Définissez une adresse DNS statique.|**netsh interface ipv4 ajouter dnsserver nom =\<nom ou ID de la carte d’interface réseau\> adresse =\<adresse IP du serveur DNS principal\> index = 1 <br>** netsh interface ipv4 add nom de dnsserver =\<nom de serveur DNS secondaire\> adresse =\<adresse IP du serveur DNS secondaire\> index = 2 ** <br> Répétez au besoin, pour ajouter des serveurs supplémentaires.<br>Exécutez **ipconfig/all** pour vérifier que les adresses sont corrects.|
|Changer une adresse IP statique en adresse IP fournie par un DHCP|**netsh interface ipv4 définir le nom de l’adresse =\<adresse IP du système local\> source = DHCP** <br>Exécutez **Ipconfig/all** pour vérifier que DHCP activé est défini sur **Oui**.|
|Entrer une clé de produit|**slmgr.vbs – ipk \<clé de produit\>**| 
|Activer le serveur localement|**slmgr.vbs -ato**| 
|Activer le serveur à distance|**cscript slmgr.vbs – ipk \<clé de produit\>\<nom du serveur\>\<nom d’utilisateur\>\<mot de passe\>** <br>**cscript slmgr.vbs - ato \<nom_serveur\> \<nom d’utilisateur\> \<mot de passe\>** <br>Obtenir le GUID de l’ordinateur en exécutant **cscript slmgr.vbs-a** <br> Run **cscript slmgr.vbs -dli \<GUID\>** <br>Vérifiez que l’état de licence est définie sur **sous licence (activé)**.


### <a name="networking-and-firewall"></a>Mise en réseau et pare-feu

|Tâche|Command| 
|----|-------|
|Configurer votre serveur pour utiliser un serveur proxy|**la définition de Netsh Winhttp proxy \<nom_serveur\>:\<le numéro de port\>** <br>**Remarque :** Les installations Server Core ne peut pas accéder à Internet via un proxy qui nécessite un mot de passe pour autoriser les connexions.|
|Configurer votre serveur pour ignorer le proxy pour les adresses Internet|**netsh winttp set proxy \<servername\>:\<port number\> bypass-list="\<local\>"**| 
|Afficher ou modifier la configuration IPSEC|**netsh ipsec**| 
|Afficher ou modifier la configuration NAP|**netsh nap**| 
|Afficher ou modifier l’adresse IP pour la traduction d’adresse physique|**arp**| 
|Afficher ou configurer la table de routage|**route**| 
|Afficher ou configurer les paramètres du serveur DNS|**nslookup**| 
|Afficher les statistiques du protocole et les connexions réseau TCP/IP actuelles|**netstat**| 
|Afficher les statistiques du protocole et les connexions TCP/IP actuelles utilisant NetBIOS sur TCP/IP (NBT)|**nbtstat**| 
|Afficher les sauts des connexions réseau|**pathping**| 
|Suivre les sauts des connexions réseau|**tracert**| 
|Afficher la configuration du routeur de multidiffusion|**mrinfo**| 
|Activer l’administration à distance du pare-feu|**netsh advfirewall firewall définie le groupe de règles = activer de nouveau « Pare-feu Windows Remote Management » = yes**| 
 

### <a name="updates-error-reporting-and-feedback"></a>Mises à jour, des rapports d’erreurs et des commentaires

|Tâche|Command| 
|----|-------|
|Installer une mise à jour|**wusa \<update\>.msu /quiet**| 
|Afficher les mises à jour installées|**systeminfo**| 
|Supprimer une mise à jour|**Développez/f:\* \<mettre à jour\>.msu c:\test** <br>Accédez au répertoire c:\test\ et ouvrez \<mettre à jour\>.xml dans un éditeur de texte.<br>Remplacez **installer** avec **supprimer** et enregistrez le fichier.<br>**pkgmgr /n:\<update\>.xml**|
|Configurer les mises à jour automatiques|Pour vérifier le paramètre actuel : ** cscript %systemroot%\system32\scregedit.wsf /AU /v **<br>Pour activer les mises à jour automatiques : **cscript scregedit.wsf /AU 4** <br>Pour désactiver les mises à jour automatiques : **cscript %systemroot%\system32\scregedit.wsf /AU 1**| 
|Activer les rapports d’erreurs|Pour vérifier le paramètre actuel : **serverWerOptin /query** <br>Pour envoyer automatiquement des rapports détaillés : **serverWerOptin / détaillées** <br>Pour envoyer automatiquement des rapports de synthèse : **serverWerOptin /summary** <br>Pour désactiver le rapport d’erreur : **serverWerOptin /disable**|
|Participer au Programme d’amélioration du produit|Pour vérifier le paramètre actuel : **serverCEIPOptin /query** <br>Pour activer la CEIP : **serverCEIPOptin /enable** <br>Pour désactiver le CEIP : **serverCEIPOptin /disable**|

### <a name="services-processes-and-performance"></a>Services, processus et performances

|Tâche|Command| 
|----|-------|
|Répertorier les services en cours d’exécution|**requête SC** ou **net start**| 
|Démarrer un service|**début de SC \<nom_service\>**  ou **net start \<nom du service\>**| 
|Arrêter un service|**arrêt de SC \<nom_service\>**  ou **net stop \<nom du service\>**| 
|Obtenir une liste des applications en cours d’exécution et des processus associés|**tasklist**||Forcer l’arrêt d’un processus|Exécutez **tasklist** récupérer l’ID de processus (PID), puis exécutez **taskkill /PID \<ID de processus\>**|
|Démarrer le Gestionnaire des tâches|**taskmgr**| 
|Créer et gérer des journaux de performances et de session de suivi événement|Pour créer un compteur, la trace, la collecte de données de configuration ou l’API : **logman créez** <br>Interrogation des propriétés de collecteur de données : **logman requête** <br>Pour démarrer ou arrêter la collecte de données : **logman start\|arrêter** <br>Pour supprimer un collecteur : **logman delete** <br> Pour mettre à jour les propriétés d’un collecteur : **logman update** <br>Pour importer un ensemble de collecteurs de données à partir d’un fichier XML ou de l’exporter dans un fichier XML : **logman import\|exporter**|

### <a name="event-logs"></a>Journaux des événements

|Tâche|Command| 
|----|-------|
|Journaux des événements de liste|**wevtutil el**| 
|Interroger les événements dans un journal spécifié|**wevtutil qe /f:text \<log name\>**| 
|Exporter un journal des événements|**wevtutil epl \<nom du journal\>**| 
|Effacer un journal des événements|**wevtutil cl \<nom du journal\>**| 


### <a name="disk-and-file-system"></a>Système de disque et de fichiers

|Tâche|Command|
|----|-------|
|Gérer les partitions de disque|Pour obtenir une liste complète des commandes, exécutez **diskpart / ?**|  
|Gérer les volumes RAID logiciels|Pour obtenir une liste complète des commandes, exécutez **diskraid / ?**|  
|Gérer les points de montage de volume|Pour obtenir une liste complète des commandes, exécutez **mountvol / ?**| 
|défragmenter un volume|Pour obtenir une liste complète des commandes, exécutez **defrag / ?**|  
|Convertir un volume en système de fichiers NTFS|**convertir \<lettre de volume \> /FS : NTFS**| 
|Compacter un fichier|Pour obtenir une liste complète des commandes, exécutez **compact / ?**|  
|Administrer des fichiers ouverts|Pour obtenir une liste complète des commandes, exécutez **openfiles / ?**|  
|Administrer des dossiers VSS|Pour obtenir une liste complète des commandes, exécutez **vssadmin / ?**| 
|Administrer le système de fichiers|Pour obtenir une liste complète des commandes, exécutez **fsutil / ?**||Vérifier la signature d’un fichier|**sigverif /?**| 
|S’approprier un fichier ou un dossier|Pour obtenir une liste complète des commandes, exécutez **icacls / ?**| 
 
### <a name="hardware"></a>Matériel

|Tâche|Command| 
|----|-------|
|Ajouter un lecteur pour un nouveau périphérique matériel|Copier le pilote dans un dossier à %HOMEDRIVE%\\\<dossier de pilote\>. Exécutez **pnputil -i - un %HOMEDRIVE%\\\<dossier de pilote\>\\\<pilote\>.inf**|
|Supprimer un lecteur pour un périphérique matériel|Pour obtenir la liste des pilotes chargés, exécutez **type de requête sc = pilote**. Puis exécutez **sc delete \<service_name\>**|
