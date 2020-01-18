---
title: Administrer Server Core
description: En savoir plus sur l’administration d’une installation Server Core de Windows Server
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.date: 12/18/2018
ms.openlocfilehash: 577014f6fd7e3a3eb58567b1a644d44360f9e498
ms.sourcegitcommit: 51e0b575ef43cd16b2dab2db31c1d416e66eebe8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76259057"
---
# <a name="administer-a-server-core-server"></a>Administrer un serveur Server Core

>S’applique à : Windows Server 2019, Windows Server 2016 et Windows Server (canal semi-annuel)

Étant donné que Server Core n’a pas d’interface utilisateur, vous devez utiliser les applets de commande Windows PowerShell, les outils en ligne de commande ou les outils de contrôle à distance pour effectuer des tâches d’administration de base. Les sections suivantes décrivent les applets de commande et les commandes PowerShell utilisées pour les tâches de base. Vous pouvez également utiliser le [Centre d’administration Windows](../../manage/windows-admin-center/overview.md), un portail de gestion unifié actuellement en préversion publique, pour administrer votre installation. 

## <a name="administrative-tasks-using-powershell-cmdlets"></a>Tâches d’administration à l’aide des applets de commande PowerShell
Utilisez les informations suivantes pour effectuer des tâches d’administration de base avec les applets de commande Windows PowerShell.

### <a name="set-a-static-ip-address"></a>Définir une adresse IP statique
Lorsque vous installez un serveur Server Core, par défaut, il a une adresse DHCP. Si vous avez besoin d’une adresse IP statique, vous pouvez la définir à l’aide des étapes suivantes.

Pour afficher votre configuration réseau actuelle, utilisez la **NetIPConfiguration**.

Pour afficher les adresses IP que vous utilisez déjà, utilisez la **NetIPAddress**.

Pour définir une adresse IP statique, procédez comme suit : 

1. Exécutez la **NetIPInterface**. 
2. Notez le nombre dans la colonne **IfIndex** pour votre interface IP ou la chaîne **InterfaceDescription** . Si vous avez plusieurs cartes réseau, notez le nombre ou la chaîne correspondant à l’interface pour laquelle vous souhaitez définir l’adresse IP statique.
3. Exécutez l’applet de commande suivante pour définir l’adresse IP statique :

   ```powershell
   New-NetIPaddress -InterfaceIndex 12 -IPAddress 192.0.2.2 -PrefixLength 24 -DefaultGateway 192.0.2.1
   ```

   où :
   - **InterfaceIndex** est la valeur de **IfIndex** de l’étape 2. (Dans notre exemple, 12)
   - **IPAddress** est l’adresse IP statique que vous souhaitez définir. (Dans notre exemple, 191.0.2.2)
   - **PrefixLength** est la longueur de préfixe (une autre forme de masque de sous-réseau) pour l’adresse IP que vous définissez. (Pour notre exemple, 24)
   - **DefaultGateway** est l’adresse IP de la passerelle par défaut. (Pour notre exemple, 192.0.2.1)
4. Exécutez l’applet de commande suivante pour définir l’adresse du serveur client DNS : 

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4
   ```
   
   où :
   - **InterfaceIndex** est la valeur de IfIndex de l’étape 2.
   - **ServerAddresses** est l’adresse IP de votre serveur DNS.
5. Pour ajouter plusieurs serveurs DNS, exécutez l’applet de commande suivante : 

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4,192.0.2.5
   ```

   dans cet exemple, **192.0.2.4 et** et **192.0.2.5** sont des adresses IP de serveurs DNS.

Si vous devez passer à l’utilisation de DHCP, exécutez **Set-DnsClientServerAddress – InterfaceIndex 12 – ResetServerAddresses**.

### <a name="join-a-domain"></a>Joindre un domaine
Utilisez les applets de commande suivantes pour joindre un ordinateur à un domaine.

1. Exécutez **Add-Computer**. Vous êtes invité à entrer les informations d’identification pour joindre le domaine et le nom de domaine.
2. Si vous devez ajouter un compte d’utilisateur de domaine au groupe Administrateurs local, exécutez la commande suivante à partir d’une invite de commandes (et non dans la fenêtre PowerShell) :

   ```
   net localgroup administrators /add <DomainName>\<UserName>
   ```
3. Redémarrez l’ordinateur. Pour ce faire, vous pouvez exécuter **Restart-Computer**.

### <a name="rename-the-server"></a>Renommer le serveur
Pour renommer le serveur, procédez comme suit.

1. Recherchez le nom actuel du serveur à l’aide de la commande **hostname** ou **ipconfig** .
2. Exécutez **Rename-Computer-ComputerName \<new_name\>** .
3. Redémarrez l’ordinateur.

### <a name="activate-the-server"></a>Activer le serveur

Exécutez **slmgr. vbs – ipk\<productkey\>** . Exécutez ensuite **slmgr. vbs – ATO**. Si l’activation a échoué, vous n’obtiendrez pas de message.

> [!NOTE]
> Vous pouvez également activer le serveur par téléphone, à l’aide d’un [serveur de service de gestion de clés (kms)](../../get-started/server-2016-activation.md), ou à distance. Pour activer à distance, exécutez l’applet de commande suivante à partir d’un ordinateur distant : 
> 
> ```
> cscript windows\system32\slmgr.vbs <ServerName> <UserName> <password>:-ato
> ```
 
### <a name="configure-windows-firewall"></a>Configurer le Pare-feu Windows

Vous pouvez configurer le Pare-feu Windows localement sur le serveur en mode d’installation minimale à l’aide des applets de commande et des scripts de Windows PowerShell. Consultez [netsecurity](/powershell/module/netsecurity/?view=win10-ps) pour les applets de commande que vous pouvez utiliser pour configurer le pare-feu Windows.

### <a name="enable-windows-powershell-remoting"></a>Activer la communication à distance Windows PowerShell

Vous pouvez activer la communication à distance Windows PowerShell, qui permet d’exécuter sur un ordinateur des commandes qui ont été entrées dans Windows PowerShell sur un autre ordinateur. Activez la communication à distance Windows PowerShell avec **Enable-PSRemoting**.

Pour plus d’informations, consultez [à propos de la FAQ à distance](/powershell/module/microsoft.powershell.core/about/about_remote_faq?view=powershell-5.1).

## <a name="administrative-tasks-from-the-command-line"></a>Tâches d’administration à partir de la ligne de commande
Utilisez les informations de référence suivantes pour effectuer des tâches d’administration à partir de la ligne de commande.

### <a name="configuration-and-installation"></a>Configuration et installation

|                             Tâche                              |                                                                                                                                                                                                                 Commande                                                                                                                                                                                                                 |
|---------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|             Définir le mot de passe d’administrateur local             |                                                                                                                                                                                                      **administrateur de l’utilisateur net** \*                                                                                                                                                                                                      |
|                  Joindre un ordinateur à un domaine                  |                                                                                                                                                       **NETDOM Join% ComputerName%** **/domain :\<domaine\>/userd :\<domaine\\nom d’utilisateur\>/passwordd :** \* <br> Redémarrez l’ordinateur.                                                                                                                                                        |
|              Confirmer le changement de domaine              |                                                                                                                                                                                                                 **set**                                                                                                                                                                                                                 |
|                Supprimer un ordinateur d’un domaine                |                                                                                                                                                                                                   **Netdom Remove \<nom_ordinateur\>**                                                                                                                                                                                                    |
|         Ajouter un utilisateur au groupe Administrateurs local          |                                                                                                                                                                                       **net localgroup Administrators/Add \<domaine\\nom d’utilisateur\>**                                                                                                                                                                                       |
|       Supprimer un utilisateur du groupe Administrateurs local       |                                                                                                                                                                                     **net localgroup Administrators/Delete \<domaine\\nom d’utilisateur\>**                                                                                                                                                                                      |
|               Ajouter un utilisateur à l’ordinateur local                |                                                                                                                                                                                                **net user \<domaine\nom_utilisateur\> \*/Add**                                                                                                                                                                                                 |
|               Ajouter un groupe à l’ordinateur local               |                                                                                                                                                                                                 **nom du groupe \<net localgroup\>/Add**                                                                                                                                                                                                  |
|          Renommer un ordinateur joint au domaine          |                                                                                                                                                           **NETDOM renamecomputer% ComputerName%/NewName :\<nouveau nom d’ordinateur\>/userd :\<domaine\\nom d’utilisateur\>/passwordd :** \*                                                                                                                                                            |
|                 Confirmer le nouveau nom de l’ordinateur                 |                                                                                                                                                                                                                 **set**                                                                                                                                                                                                                 |
|         Renommer un ordinateur dans un groupe de travail         |                                                                                                                                                                **Netdom renamecomputer \<currentcomputername\>/NewName :\<NEWCOMPUTERNAME\>** <br>Redémarrez l’ordinateur.                                                                                                                                                                 |
|                Désactiver la gestion des fichiers de pagination                 |                                                                                                                                                                        **WMIC ComputerSystem où name = "\<ComputerName\>" Set AutomaticManagedPagefile = false**                                                                                                                                                                         |
|                   Configurer un fichier de pagination                   |                                                            **WMIC pagefileset où name = "\<chemin/nom de fichier\>" Set InitialSize =\<InitialSize\>, MaximumSize =\<MaxSize\>** <br>Où *chemin d’accès/nom* de fichier est le chemin d’accès et le nom du fichier d’échange, *InitialSize* est la taille de départ du fichier d’échange, en octets, et *MaxSize* est la taille maximale du fichier d’échange, en octets.                                                             |
|                 Modifier une adresse IP statique                 | **ipconfig/all** <br>Enregistrez les informations pertinentes ou redirigez-les vers un fichier texte (**ipconfig/all > ipconfig. txt**).<br>**netsh interface IPv4 show interfaces**<br>Vérifiez l’existence d’une liste d’interfaces.<br>**netsh interface IPv4 set address \<nom ID de la liste d’interfaces\> source = static address =\<adresse IP préférée\> Gateway =\<adresse de passerelle\>**<br>Exécutez **ipconfig/all** pour vérifier que DHCP Enabled a la valeur **no**. |
|                   Définissez une adresse DNS statique.                   |   <strong>netsh interface IPv4 Add dnsserver Name =\<nom ou ID de la carte d’interface réseau\> Address =\<adresse IP du serveur DNS principal\> index = 1 <br></strong>netsh interface IPv4 Add dnsserver Name =\<nom du serveur DNS secondaire\> Address =\<adresse IP du serveur DNS secondaire\> index = 2\*\* <br> Répétez l’opération si nécessaire pour ajouter des serveurs supplémentaires.<br>Exécutez **ipconfig/all** pour vérifier que les adresses sont correctes.   |
| Changer une adresse IP statique en adresse IP fournie par un DHCP |                                                                                                                                      **netsh interface IPv4 Set Address Name =\<adresse IP du système local\> source = DHCP** <br>Exécutez **ipconfig/all** pour vérifier que le protocole DCHP Enabled a la valeur **Oui**.                                                                                                                                      |
|                      Entrer une clé de produit                      |                                                                                                                                                                                                   **slmgr. vbs – IPK \<clé de produit\>**                                                                                                                                                                                                    |
|                  Activer le serveur localement                  |                                                                                                                                                                                                           **slmgr. vbs-ATO**                                                                                                                                                                                                            |
|                 Activer le serveur à distance                  |                                            **cscript slmgr. vbs – IPK \<clé de produit\>\<nom du serveur\>\<nom d’utilisateur\>\<mot de passe\>** <br>**cscript slmgr. vbs-ATO \<ServerName\> \<nom d’utilisateur\> \<mot de passe\>** <br>Récupérez le GUID de l’ordinateur en exécutant **cscript slmgr. vbs-** <br> Exécutez **cscript slmgr. vbs-dli \<GUID\>** <br>Vérifiez que l’état de la licence est défini sur **Licensed (activé)** .                                             |

### <a name="networking-and-firewall"></a>Mise en réseau et pare-feu

|Tâche|Commande| 
|----|-------|
|Configurer votre serveur pour utiliser un serveur proxy|**netsh WinHTTP Set proxy \<ServerName\>:\<numéro de port\>** <br>**Remarque :** Les installations Server Core ne peuvent pas accéder à Internet via un proxy qui requiert un mot de passe pour autoriser les connexions.|
|Configurer votre serveur pour contourner le proxy pour les adresses Internet|**netsh WinHTTP Set proxy \<ServerName\>:\<numéro de port\> Bypass-List = "\<local\>"**| 
|Afficher ou modifier la configuration IPSEC|**netsh ipsec**| 
|Afficher ou modifier la configuration NAP|**netsh nap**| 
|Afficher ou modifier l’adresse IP à la traduction d’adresses physiques|**arp**| 
|Afficher ou configurer la table de routage locale|**route**| 
|Afficher ou configurer les paramètres du serveur DNS|**nslookup**| 
|Afficher les statistiques du protocole et les connexions réseau TCP/IP actuelles|**netstat**| 
|Affichage des statistiques de protocole et des connexions TCP/IP actuelles à l’aide de NetBIOS sur TCP/IP (NBT)|**nbtstat**| 
|Afficher les sauts pour les connexions réseau|**pathping**| 
|Suivi des tronçons pour les connexions réseau|**tracert**| 
|Afficher la configuration du routeur de multidiffusion|**mrinfo**| 
|Activer l’administration à distance du pare-feu|**netsh advfirewall firewall set Rule Group = "gestion à distance du pare-feu Windows Defender" New Enable = Oui**| 
 

### <a name="updates-error-reporting-and-feedback"></a>Mises à jour, rapports d’erreurs et commentaires

|                               Tâche                                |                                                                                                                               Commande                                                                                                                                |
|-------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                         Installer une mise à jour                         |                                                                                                                    **Wusa \<mettre à jour\>. msu/quiet**                                                                                                                    |
|                      Afficher les mises à jour installées                       |                                                                                                                            **systeminfo**                                                                                                                            |
|                         Supprimer une mise à jour                          |                                 **Expand/f :\* \<mettre à jour\>. msu c:\test** <br>Accédez à répertoire c:\test\ et ouvrez \<mettre à jour\>. xml dans un éditeur de texte.<br>Remplacez **install** par **Remove** et enregistrez le fichier.<br>**pkgmgr/n :\<mettre à jour\>. Xml**                                 |
|                    Configurer les mises à jour automatiques                    |          Pour vérifier le paramètre actuel : **cscript%systemroot%\SYSTEM32\SCREGEDIT.wsf/au/v \*\*<br>pour activer les mises à jour automatiques : \*\*cscript scregedit. wsf/au 4** <br>Pour désactiver les mises à jour automatiques : **cscript%systemroot%\SYSTEM32\SCREGEDIT.wsf/au 1**          |
|                      Activer les rapports d’erreurs                       | Pour vérifier le paramètre actuel : **serverWerOptin/Query** <br>Pour envoyer automatiquement des rapports détaillés : **serverWerOptin/detailed** <br>Pour envoyer automatiquement des rapports de synthèse : **serverWerOptin/Summary** <br>Pour désactiver le rapport d’erreurs : **serverWerOptin/Disable** |
| Participer au Programme d’amélioration du produit |                                                     Pour vérifier le paramètre actuel : **serverCEIPOptin/Query** <br>Pour activer CEIP : **serverCEIPOptin/Enable** <br>Pour désactiver le programme d’amélioration du produit : **serverCEIPOptin/Disable**                                                      |

### <a name="services-processes-and-performance"></a>Services, processus et performances

|                               Tâche                               |                                                                                                                                                                                                             Commande                                                                                                                                                                                                              |
|------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                    Répertorier les services en cours d’exécution                     |                                                                                                                                                                                                  **requête SC** ou **net start**                                                                                                                                                                                                   |
|                         Démarrer un service                          |                                                                                                                                                                                 **sc start \<nom du service\>** ou **net start \<nom du service\>**                                                                                                                                                                                  |
|                          Arrêter un service                          |                                                                                                                                                                                  **sc stop \<nom du service\>** ou **net stop \<nom du service\>**                                                                                                                                                                                   |
| Obtenir une liste des applications en cours d’exécution et des processus associés |                                                                                                                                                                                                           **tasklist**                                                                                                                                                                                                           |
|                        Démarrer le Gestionnaire des tâches                        |                                                                                                                                                                                                           **taskmgr**                                                                                                                                                                                                            |
|    Créer et gérer des journaux de performances et de session de suivi d’événements    | Pour créer un compteur, une trace, une collecte de données de configuration ou une API : **Logman créer** <br>Pour interroger les propriétés du collecteur de données : **Logman Query** <br>Pour démarrer ou arrêter la collecte de données : **logman start\|Stop** <br>Pour supprimer un collecteur : **Logman Delete** <br> Pour mettre à jour les propriétés d’un collecteur : **logman update** <br>Pour importer un ensemble de collecteurs de données à partir d’un fichier XML ou l’exporter dans un fichier XML : **Logman import\|exporter** |

### <a name="event-logs"></a>Journaux des événements

|Tâche|Commande| 
|----|-------|
|Répertorier les journaux des événements|**wevtutil El**| 
|Interroger des événements dans un journal spécifié|**wevtutil qe/f : texte \<le nom du journal\>**| 
|Exporter un journal des événements|**wevtutil EPL \<nom du journal\>**| 
|Effacer un journal des événements|**wevtutil CL \<nom du journal\>**| 


### <a name="disk-and-file-system"></a>Système de disque et de fichiers

|                   Tâche                   |                        Commande                        |
|------------------------------------------|-------------------------------------------------------|
|          Gérer les partitions de disque          | Pour obtenir la liste complète des commandes, exécutez **diskpart/ ?**  |
|           Gérer les volumes RAID logiciels           | Pour obtenir la liste complète des commandes, exécutez **DiskRAID/ ?**  |
|        Gérer les points de montage de volume        | Pour obtenir la liste complète des commandes, exécutez **mountvol/ ?**  |
|           défragmenter un volume            |  Pour obtenir la liste complète des commandes, exécutez **Defrag/ ?**   |
| Convertir un volume en système de fichiers NTFS |        **convertir \<lettre de volume\>/FS : NTFS**         |
|              Compacter un fichier              |  Pour obtenir la liste complète des commandes, exécutez **compact/ ?**  |
|          Administrer des fichiers ouverts           | Pour obtenir la liste complète des commandes, exécutez **openfiles/ ?** |
|          Administrer des dossiers VSS          | Pour obtenir la liste complète des commandes, exécutez **vssadmin/ ?**  |
|        Administrer le système de fichiers        |  Pour obtenir la liste complète des commandes, exécutez **fsutil/ ?**   |
|    S’approprier un fichier ou un dossier    |  Pour obtenir la liste complète des commandes, exécutez **icacls/ ?**   |
 
### <a name="hardware"></a>Matériel

|Tâche|Commande| 
|----|-------|
|Ajouter un lecteur pour un nouveau périphérique matériel|Copiez le pilote dans un dossier à l’adresse% HomeDrive%\\\<dossier du pilote\>. Exécuter **PnPUtil-i-a% HomeDrive%\\\<dossier du pilote\>\\\<driver\>. inf**|
|Supprimer un lecteur pour un périphérique matériel|Pour obtenir la liste des pilotes chargés, exécutez la **requête SC type = Driver**. Exécutez ensuite **sc delete \<service_name\>**|
