---
title: Administrer Server Core
description: Découvrez comment gérer une installation minimale de Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.date: 12/18/2018
ms.openlocfilehash: 39fbb92645d39a46613f2142d0258c78a6ba425b
ms.sourcegitcommit: 4df1bc940338219316627ad03f0525462a05a606
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/19/2018
ms.locfileid: "8977996"
---
# Administrer un serveur de Server Core

>S’applique à: WindowsServer (canal semi-annuel) et WindowsServer2016

Étant donné que Server Core n’a pas d’une interface utilisateur, vous devez utiliser les applets de commande Windows PowerShell, les outils de ligne de commande ou d’outils à distance pour effectuer des tâches d’administration. Les sections suivantes décrivent les applets de commande PowerShell et les commandes utilisées pour les tâches de base. Vous pouvez également utiliser [Windows Admin Center](../../manage/windows-admin-center/overview.md), un portail de gestion unifiée actuellement en version d’évaluation publique, d’administrer votre installation. 

## Tâches d’administration à l’aide des applets de commande PowerShell
Utilisez les informations suivantes pour effectuer des tâches d’administration de base avec les applets de commande Windows PowerShell.

### Définir une adresse IP statique
Lorsque vous installez un serveur de Server Core, par défaut, il a adresse A DHCP. Si vous avez besoin d’une adresse IP statique, vous pouvez définir à l’aide de la procédure suivante.

Pour afficher votre configuration réseau actuelle, utilisez **Get-NetIPConfiguration**.

Pour afficher les adresses IP que vous utilisez déjà, utilisez **Get-NetIPAddress**.

Pour définir une adresse IP statique, procédez comme suit: 

1. Exécutez **Get-NetIPInterface**. 
2. Notez le numéro de la colonne **IfIndex** pour votre interface de l’adresse IP ou la chaîne **InterfaceDescription** . Si vous avez plusieurs cartes réseau, notez le numéro ou la chaîne correspondant à l’interface que vous voulez définir l’adresse IP statique pour.
3. Exécutez l’applet de commande suivante pour définir l’adresse IP statique:

   ```powershell
   New-NetIPaddress -InterfaceIndex 12 -IPAddress 192.0.2.2 -PrefixLength 24 -DefaultGateway 192.0.2.1
   ```

   où:
   - **InterfaceIndex** a la valeur de **IfIndex** de l’étape 2. (Dans notre exemple, 12)
   - **Adresse IP** est l’adresse IP statique que vous voulez définir. (Dans notre exemple, 191.0.2.2)
   - **PrefixLength** est la longueur du préfixe (une autre forme de masque de sous-réseau) pour l’adresse IP que vous définissez. (Dans notre exemple, il s’agit de 24)
   - **DefaultGateway** est l’adresse IP à la passerelle par défaut. (Pour notre exemple, 192.0.2.1)
4. Exécutez l’applet de commande suivante pour définir le client DNS adresse du serveur: 

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4
   ```
   
   où:
   - **InterfaceIndex** a la valeur de IfIndex de l’étape 2.
   - **ServerAddresses** est l’adresse IP de votre serveur DNS.
5. Pour ajouter plusieurs serveurs DNS, exécutez l’applet de commande suivante: 

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4,192.0.2.5
   ```

   où, dans cet exemple, **192.0.2.4** et **192.0.2.5** sont les deux adresses IP des serveurs DNS.

Si vous devez utiliser DHCP, exécutez **Set-DnsClientServerAddress – InterfaceIndex 12 – ResetServerAddresses**.

### Joindre un domaine
Utilisez les applets de commande suivantes pour joindre un ordinateur à un domaine.

1. Exécutez **Ajouter un ordinateur**. Vous devrez indiquer les informations d’identification joindre le domaine et le nom de domaine.
2. Si vous avez besoin d’ajouter un compte d’utilisateur de domaine au groupe Administrateurs local, exécutez la commande suivante à une invite de commandes (pas dans la fenêtre PowerShell):

   ```
   net localgroup administrators /add <DomainName>\<UserName>
   ```
3. Redémarrez l’ordinateur. Vous pouvez le faire en exécutant **Restart-Computer**.

### Renommer le serveur
Utilisez les étapes suivantes pour renommer le serveur.

1. Déterminez le nom du serveur sur le **nom d’hôte** ou **ipconfig** la commande en cours.
2. Exécutez **Renommer-Computer - ComputerName \<new_name\ >**.
3. Redémarrez l’ordinateur.

### Activer le serveur

Exécutez **slmgr.vbs – ipk\<productkey\ >**. Exécutez ensuite **slmgr.vbs – assembler**. Si l’activation réussit, vous n’obtiendrez un message.

> [!NOTE]
> Vous pouvez également activer le serveur par téléphone, à l’aide d’un [serveur du Service de gestion des clés (KMS)](../../get-started/server-2016-activation.md), ou à distance. Pour activer à distance, exécutez l’applet de commande suivante à partir d’un ordinateur distant: 

>```powershell
>**cscript windows\system32\slmgr.vbs <ServerName> <UserName> <password>:-ato**
>```
 
### Configurer le pare-feu Windows

Vous pouvez configurer le pare-feu Windows localement sur l’ordinateur de Server Core à l’aide de scripts et des applets de commande Windows PowerShell. Voir [NetSecurity](/powershell/module/netsecurity/?view=win10-ps) pour les applets de commande que vous pouvez utiliser pour configurer le pare-feu Windows.

### Activer la communication à distance Windows PowerShell

Vous pouvez activer la communication à distance de Windows PowerShell, dans lequel les commandes tapées dans Windows PowerShell sur un seul ordinateur s’exécuter sur un autre ordinateur. Activer la communication à distance de Windows PowerShell avec **Enable-PSRemoting**.

Pour plus d’informations, voir [Sur distant Forum aux questions](/powershell/module/microsoft.powershell.core/about/about_remote_faq?view=powershell-5.1)
</br>

## Tâches d’administration à partir de la ligne de commande
Utilisez les informations de référence suivantes pour effectuer des tâches d’administration à partir de la ligne de commande.

### Installation et configuration
|Tâche | Commande |
|-----|-------|
|Définir le mot de passe d’administrateur local| **administrateur utilisateur NET** \* |
|Joindre un ordinateur à un domaine| **netdom jointure % Nom_Ordinateur %** **/domain:\<domain\ > /userd:\<domain\\username\ >/passwordd:**\* <br> Redémarrez l’ordinateur.|
|Vérifiez que le domaine a changé.| **set** |
|Supprimer un ordinateur d’un domaine|**suppression de la commande Netdom: \ < computername\ >**| 
|Ajouter un utilisateur au groupe Administrateurs local|**NET groupe local Administrateurs / ajouter \ < domain\\username\ >** |
|Supprimer un utilisateur à partir du groupe Administrateurs local|**NET groupe local Administrateurs Delete \ < domain\\username\ >** |
|Ajouter un utilisateur à l’ordinateur local|**utilisateur NET \ < domain\username\ > * / Ajouter** |
|Ajouter un groupe de l’ordinateur local|**groupe local NET \ < nom de groupe de packages > / Ajouter**|
|Modifier le nom d’un ordinateur joint au domaine|**netdom renamecomputer % Nom_Ordinateur % /NewName:\<new ordinateur name\ > /userd:\<domain\\username\ >/passwordd:** * |
|Vérifiez que le nouveau nom d’ordinateur|**set**| 
|Modifier le nom d’un ordinateur dans un groupe de travail|**netdom renamecomputer \ < currentcomputername\ > /NewName: \ < newcomputername\ >** <br>Redémarrez l’ordinateur.|
|Désactiver la gestion de fichier de pagination|**système informatique de ligne de commande WMI où name = «\ < computername\ >» définir AutomaticManagedPagefile = False**| 
|Configurer le fichier d’échange|**ligne de commande WMI pagefileset où name = «\ < chemin d’accès/filename\ >» définir InitialSize = \ < initialsize\ >, MaximumSize = \ < maxsize\ >** <br>Où *chemin/nom de fichier* est le chemin d’accès et le nom du fichier d’échange, *initialsize* est la taille du fichier d’échange, en octets, de départ et *maxsize* est la taille maximale du fichier, en octets.|
|Passer à une adresse IP statique|**ipconfig/toutes les** <br>Enregistrez les informations pertinentes ou rediriger vers un fichier texte (**ipconfig/tous les > ipconfig.txt**).<br>**interfaces de netsh interface ipv4 afficher**<br>Vérifiez qu’il existe une liste d’interface.<br>**netsh interface ipv4 définir le nom de l’adresse \ source < ID à partir de l’interface list\ > = adresse statique = \ < mailto:\[email préféré d’address\ IP > passerelle = \ < passerelle mailto:\[email address\ >**<br>Exécutez **ipconfig/tous les** à vierfy DHCP activé est défini sur **non**.|
|Définissez une adresse DNS statique.|**netsh interface ipv4 ajouter serveurDNS nom = \<name ou l’ID de la card\ interface réseau > adresse = \<IP adresse de serveur\ DNS principal > index = 1 <br> **netsh interface ipv4 ajouter serveurDNS name = \<name de secondaires DNS server\ > adresse = \<IP adresse de serveur\ DNS secondaire > index = 2 ** <br> Répétez selon les besoins ajouter des serveurs supplémentaires.<br>Exécutez **ipconfig/tous les** pour vérifier que les adresses sont corrects.|
|Passer à une adresse IP DHCP fournie à partir d’une adresse IP statique|**netsh interface ipv4 définir le nom de l’adresse = \ source < adresse IP locale FAT32\ > = DHCP** <br>Exécutez **Ipconfig/tous les** pour vérifier que DHCP activé est défini sur **Oui**.|
|Entrez une clé de produit|**slmgr.vbs – ipk \ < key\ produit >**| 
|Activer le serveur en local|**slmgr.vbs - assembler**| 
|Activer le serveur à distance|**cscript slmgr.vbs – ipk \ < key\ produit > \ < nom de serveur de packages > \ < username\ > \ < password\ >** <br>**cscript slmgr.vbs - assembler \ < servername\ > \ < username\ > \ < password\ >** <br>Obtenir le GUID de l’ordinateur en exécutant **cscript slmgr.vbs-avez-vous** <br> Exécutez **cscript slmgr.vbs - dli \<GUID\ >** <br>Vérifiez qu’état de la licence est défini sur la **licence (activé)**.


### Pare-feu et mise en réseau

|Tâche|Commande| 
|----|-------|
|Configurer votre serveur pour utiliser un serveur proxy|**netsh Winhttp définie proxy \ < servername\ >: \ < port number\ >** <br>**Remarque:** Les installations Server Core ne peut pas accéder à Internet via un proxy qui nécessite un mot de passe pour autoriser les connexions.|
|Configurer votre serveur pour ignorer le proxy pour les adresses Internet|**netsh winttp définie proxy \ < servername\ >: \ < port number\ > liste-contournement = «\ < local\ >»**| 
|Afficher ou modifier la configuration IPSEC|**netsh ipsec**| 
|Afficher ou modifier la configuration NAP|**netsh nap**| 
|Afficher ou modifier l’adresse IP à la traduction d’adresse physique|**arp**| 
|Afficher ou configurer la table de routage|**itinéraire**| 
|Permet d’afficher ou de configurer les paramètres de serveur DNS|**nslookup**| 
|Afficher les statistiques de protocole et les connexions de réseau TCP/IP en cours|**netstat**| 
|Afficher les statistiques de protocole et les connexions TCP/IP en cours à l’aide de NetBIOS via TCP/IP (NBT)|**nbtstat**| 
|Afficher le relais pour les connexions réseau|**pathping**| 
|Définissez le relais pour les connexions réseau|**tracert**| 
|Afficher la configuration du routeur multidiffusion|**mrinfo**| 
|Activer l’administration à distance du pare-feu|**netsh advfirewall firewall définie le groupe de règles = «Gestion à distance du pare-feu Windows» nouvelle enable = yes**| 
 

### Mises à jour, rapport d’erreurs et commentaires

|Tâche|Commande| 
|----|-------|
|Installer une mise à jour|**Wusa \ < update\ > .msu /silencieux**| 
|Mises à jour de la liste installée|**systeminfo**| 
|Supprimer une mise à jour|**Développez /f:\* \ < update\ > .msu c:\test** <br>Accédez à c:\test\ et ouvrez \ < update\ > .xml dans un éditeur de texte.<br>Remplacez **installer** **Supprimer** et enregistrez le fichier.<br>**pkgmgr /n:\ < update\ > .xml**|
|Configurer les mises à jour automatiques|Pour vérifier la valeur actuelle: ** cscript %systemroot%\system32\scregedit.wsf SD/au /v **<br>Pour activer les mises à jour automatiques: **scregedit.wsf cscript SD/au 4** <br>Pour désactiver les mises à jour automatiques: **%systemroot%\system32\scregedit.wsf cscript SD/au 1**| 
|Activer le rapport d’erreurs|Pour vérifier la valeur actuelle: **Query serverWerOptin** <br>Pour envoyer automatiquement des rapports détaillés: **serverWerOptin / détaillées** <br>Pour envoyer automatiquement des rapports de synthèse: **serverWerOptin /summary** <br>Pour désactiver le rapport d’erreurs: **/disable serverWerOptin**|
|Participer au programme d’amélioration (CEIP)|Pour vérifier la valeur actuelle: **Query serverCEIPOptin** <br>Pour activer la CEIP: **/ Enable serverCEIPOptin** <br>Pour désactiver le CEIP: **/disable serverCEIPOptin**|

### Services, les processus et performances

|Tâche|Commande| 
|----|-------|
|Répertorier les services en cours d’exécution|**requête SC** ou **net start**| 
|Démarrer un service|**sc démarrer \<service name\ >** ou **net démarrer \<service name\ >**| 
|Arrêter un service|**sc stop \<service name\ >** ou **net stop \<service name\ >**| 
|Récupérer la liste des applications et les processus associés en cours d’exécution|**tasklist**||Arrêter un processus de force|Exécutez **tasklist** récupérer l’ID de processus (PID), puis exécutez **taskkill/PID \<process ID\ >**|
|Démarrez le Gestionnaire des tâches|**taskmgr**| 
|Créer et gérer la trace de la session et les performances journaux des événements|Pour créer un compteur, la trace, la collecte de données de configuration ou l’API: **créez logman** <br>Pour les propriétés de collecteur de données de requête: **logman requête** <br>Pour démarrer ou arrêter la collecte de données: **start\ logman | arrêter** <br>Pour supprimer un collecteur: **logman supprimer** <br> Pour mettre à jour les propriétés d’un collecteur: **logman mettre à jour** <br>Pour importer un ensemble de collecteur de données à partir d’un fichier XML ou exportez-le vers un fichier XML: **import\ logman | exporter**|

### Journaux des événements

|Tâche|Commande| 
|----|-------|
|Liste des journaux d’événements|**wevtutil el**| 
|Événements de requête dans un journal spécifié|**wevtutil qe /f:text \ < consigner name\ >**| 
|Exporter un journal des événements|**wevtutil epl \ < consigner name\ >**| 
|Effacer un journal des événements|**wevtutil cl \ < consigner name\ >**| 


### Système de fichiers et de disque

|Tâche|Commande|
|----|-------|
|Gérer les partitions de disque|Pour obtenir la liste complète des commandes, exécutez **diskpart /?**|  
|Gérer les logiciels RAID|Pour obtenir la liste complète des commandes, exécutez **diskraid /?**|  
|Gérer les points de montage de volume|Pour obtenir la liste complète des commandes, exécutez **mountvol /?**| 
|Défragmenter un volume|Pour obtenir la liste complète des commandes, exécutez **defrag /?**|  
|Convertir un volume au système de fichiers NTFS|**convertir \ < volume letter\ > FS**| 
|Compression d’un fichier|Pour obtenir la liste complète des commandes, exécutez **compact /?**|  
|Administrer les fichiers ouverts|Pour obtenir la liste complète des commandes, exécutez **openfiles /?**|  
|Administrer les dossiers VSS|Pour obtenir la liste complète des commandes, exécutez **vssadmin /?**| 
|Administrer le système de fichiers|Pour obtenir la liste complète des commandes, exécutez **fsutil /?**||Vérifier la signature d’un fichier|**Sigverif /?**| 
|Prendre possession d’un fichier ou un dossier|Pour obtenir la liste complète des commandes, exécutez **icacls /?**| 
 
### Matériel

|Tâche|Commande| 
|----|-------|
|Ajoutez un pilote pour un nouveau périphérique matériel|Copiez le pilote dans un dossier au %homedrive%\\\ < pilote Files\\\r\n\t\t\t\tDossier >. Exécutez **pnputil -i-Files\\\r\n\t\t\t\tDossier %homedrive%\\\<driver > \\\<driver\ > .inf**|
|Supprimer un pilote d’un périphérique matériel|Pour obtenir la liste des pilotes chargés, exécutez **type de requête sc = pilote**. Ensuite, exécutez **sc supprimer \<service_name\ >**|
