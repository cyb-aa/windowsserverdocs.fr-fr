---
title: Gérer le Server Core
description: Découvrez comment gérer une installation Server Core de Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: 761bfc681d7e39059884977cd99997ea9996268b
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811353"
---
# <a name="manage-a-server-core-server"></a>Gérer un serveur Server Core
 
> S’applique à : Windows Server (canal semi-annuel) et Windows Server 2016

Vous pouvez gérer un serveur Server Core de plusieurs manières :
- À l’aide de [Windows Admin Center](../../manage/windows-admin-center/overview.md)
- À l’aide de [outils d’Administration de serveur distant](../../remote/remote-server-administration-tools.md) s’exécutant sur Windows 10
- Localement et à distance avec Windows PowerShell
- À distance en utilisant [Gestionnaire de serveur](../server-manager/server-manager.md)
- À distance en utilisant un [le composant logiciel enfichable MMC](#managing-with-microsoft-management-console)
- À distance avec [Services Bureau à distance](#managing-with-remote-desktop-services)

Vous pouvez également ajouter un matériel et gérer les pilotes localement, tant que vous le faire à partir de la ligne de commande.

Il existe des limitations importantes et des conseils à prendre en compte lorsque vous travaillez avec Server Core :

- Si vous fermez toutes les fenêtres d’invite de commandes et que vous souhaitez ouvrir une nouvelle fenêtre d’invite de commandes, vous pouvez le faire à partir du Gestionnaire des tâches. Appuyez sur **CTRL\+ALT\+supprimer**, cliquez sur **démarrer le Gestionnaire des tâches**, cliquez sur **plus de détails > fichier > exécuter**, puis tapez  **cmd.exe**. (Type **Powershell.exe** pour ouvrir une fenêtre de commande PowerShell.) Vous pouvez également vous déconnecter et se reconnecter.
- Les commandes ou les outils qui essaient de démarrer l’Explorateur Windows ne fonctionnent pas. Par exemple, en cours d’exécution **Démarrer.** à partir d’une invite de commandes ne fonctionnera pas.
- Il n’existe aucune prise en charge pour le rendu HTML ou de l’aide HTML dans Server Core.
- Server Core, prend en charge le programme d’installation de Windows en mode silencieux, afin que vous pouvez installer les outils et utilitaires à partir des fichiers de programme d’installation de Windows. Lors de l’installation des packages Windows Installer sur Server Core, utilisez le **/qb** option pour afficher l’interface utilisateur de base.
- Pour modifier le fuseau horaire, exécutez **Set-Date**.
- Pour modifier les paramètres internationaux, exécutez **control intl.cpl**.
- **Control.exe** ne s’exécute pas son propre. Vous devez l’exécuter avec soit **Timedate.cpl** ou **Intl.cpl**.
- **Winver.exe** n’est pas disponible dans Server Core. Pour obtenir des informations de version, utilisez **Systeminfo.exe**.

## <a name="managing-server-core-with-windows-admin-center"></a>La gestion de Server Core avec Windows Admin Center
[Windows Admin Center](../../manage/windows-admin-center/overview.md) est une application de gestion basée sur un navigateur qui permet d’administrer localement des serveurs Windows Server, sans dépendance à Azure ou au cloud. Windows Admin Center vous donne un contrôle total sur tous les aspects de votre infrastructure de serveurs. Il s’avère particulièrement utile pour la gestion de réseaux privés qui ne sont pas connectés à Internet. Vous pouvez installer Windows Admin Center sur Windows 10, sur un serveur de passerelle ou sur une installation de Windows Server avec la fonctionnalité expérience utilisateur et ensuite vous connecter au système Server Core que vous souhaitez gérer.

## <a name="managing-server-core-remotely-with-server-manager"></a>La gestion de Server Core à distance avec le Gestionnaire de serveur

Le Gestionnaire de serveur est une console de gestion dans Windows Server qui vous permet de configurer et gérer des serveurs Windows locaux et distants à partir de vos postes de travail, sans avoir besoin d’accéder physiquement aux serveurs ni activer le protocole Bureau à distance (RDP) connexions à chaque serveur. Le Gestionnaire de serveur prend en charge la gestion multiserveur à distance.

Pour activer votre serveur local pour être gérée par le Gestionnaire de serveur en cours d’exécution sur un serveur distant, exécutez l’applet de commande Windows PowerShell **SMRemoting.exe-configurer – activer**.

## <a name="managing-with-microsoft-management-console"></a>Gestion de Microsoft Management Console

Vous pouvez utiliser les nombreux composants logiciel enfichables pour Microsoft Management Console (MMC) à distance pour gérer votre serveur Server Core.

Pour utiliser un composant logiciel enfichable MMC pour gérer un serveur Server Core, qui est membre du domaine : 

1. Démarrer un composant logiciel enfichable MMC, tel que gestion de l’ordinateur.
2. Cliquez sur le composant logiciel enfichable, puis cliquez sur **se connecter à un autre ordinateur**.
2. Tapez le nom d’ordinateur du serveur Server Core, puis cliquez sur **OK**. Vous pouvez maintenant utiliser le composant logiciel enfichable MMC pour gérer le serveur Server Core, comme vous le feriez pour n’importe quel autre ordinateur ou serveur.

Pour utiliser un composant logiciel enfichable MMC pour gérer un serveur Server Core est *pas* un membre de domaine : 

1. Établir les autres informations d’identification à utiliser pour se connecter à l’ordinateur Server Core en tapant la commande suivante à une invite de commandes sur l’ordinateur distant :
1. 
   ```
   cmdkey /add:<ServerName> /user:<UserName> /pass:<password>
   ```

   Si vous souhaitez être invité à entrer un mot de passe, omettez la **/passer** option.

2. Lorsque vous y êtes invité, tapez le mot de passe pour le nom d’utilisateur que vous avez spécifié.
   Si le pare-feu sur le serveur de Server Core n’est pas déjà configuré pour autoriser les composants logiciel enfichables MMC pour vous connecter, suivez les étapes ci-dessous pour configurer le pare-feu Windows pour autoriser le composant logiciel enfichable MMC. Puis passez à l’étape 3.
3. Sur un autre ordinateur, démarrez un composant logiciel enfichable MMC, tel que **gestion de l’ordinateur**.
4. Dans le volet gauche, cliquez sur le composant logiciel enfichable, puis cliquez sur **se connecter à un autre ordinateur**. (Par exemple, dans l’exemple de gestion de l’ordinateur, droit **gestion de l’ordinateur (Local)** .)
5. Dans **un autre ordinateur**, tapez le nom d’ordinateur du serveur Server Core, puis cliquez sur **OK**. Vous pouvez maintenant utiliser le composant logiciel enfichable MMC pour administrer le serveur en mode d’installation minimale comme vous le feriez sur n’importe quel autre ordinateur exécutant un système d’exploitation Windows Server.

### <a name="to-configure-windows-firewall-to-allow-mmc-snap-ins-to-connect"></a>Pour configurer le Pare-feu Windows afin d’autoriser la connexion de composants logiciels enfichables MMC
Pour autoriser tous les composants logiciel enfichables MMC pour vous connecter, exécutez la commande suivante :

```
Enable-NetFirewallRule -DisplayGroup "Remote Administration"
```

Pour autoriser uniquement des composants logiciel enfichables MMC pour vous connecter, exécutez la commande suivante :
```
Enable-NetFirewallRule -DisplayGroup "<rulegroup>"
```

Où *rulegroup* est une des opérations suivantes, selon le composant logiciel enfichable que vous souhaitez vous connecter :

| Composant logiciel enfichable MMC                            | Groupe de règles                                            |
|----------------------------------------|-------------------------------------------------------|
| Observateur d’événements                           | Gestion à distance des journaux des événements                           |
| Services                               | Gestion des services à distance                             |
| Dossiers partagés                         | Partage de fichiers et d’imprimantes                              |
| Planificateur de tâches                         | Les journaux de performances et alertes, partage de fichiers et imprimantes |
| Gestion des disques                        | Gestion des volumes à distance                              |
| Pare-feu de Windows et la sécurité avancée | Gestion à distance du Pare-feu Windows                    |


> [!NOTE] 
> Certains composants logiciels enfichables MMC n’ont pas un groupe de règles correspondant qui leur permet de se connecter via le pare-feu. Cependant, l’activation des groupes de règles Observateur d’événements, Services ou Dossiers partagés permet d’autoriser la connexion de la plupart des autres composants logiciels enfichables. 
>
> Par ailleurs, certains composants logiciels enfichables nécessitent une configuration supplémentaire pour pouvoir se connecter via le Pare-feu Windows :
>
> - Gestion des disques. Vous devez d’abord démarrer le service VDS (Virtual Disk Service) sur le serveur en mode d’installation minimale. Vous devez également configurer les règles du composant logiciel enfichable Gestion des disques de manière appropriée sur le serveur qui exécute ce composant.
> - Moniteur de sécurité IP. Vous devez commencer par activer la gestion à distance de ce composant logiciel enfichable. Pour ce faire, à une invite de commandes, tapez **Cscript \windows\system32\scregedit.wsf /im 1**
> - Fiabilité et performances. Ce composant logiciel enfichable ne nécessite pas de configuration supplémentaire, mais son utilisation sur un serveur en mode d’installation minimale vous permet uniquement de surveiller les performances. Il ne fournit pas de données sur la fiabilité.

## <a name="managing-with-remote-desktop-services"></a>Gestion avec les Services Bureau à distance

Vous pouvez utiliser [Bureau à distance](../../remote/remote-desktop-services/welcome-to-rds.md) pour gérer un serveur Server Core sur des ordinateurs distants.

Avant de pouvoir accéder à Server Core, vous devez exécuter la commande suivante : 
```
cscript C:\Windows\System32\Scregedit.wsf /ar 0
```
Cette commande configure le mode Bureau à distance pour administration afin d’autoriser les connexions.

## <a name="add-hardware-and-manage-drivers-locally"></a>Ajouter un matériel et gérer les pilotes localement

Pour ajouter un matériel à un serveur Server Core, suivez les instructions fournies par le fournisseur de matériel pour installer un nouveau matériel. 

Si le matériel n’est pas plug-and-play, vous devez installer manuellement le pilote. Pour ce faire, copiez les fichiers de pilote dans un emplacement temporaire sur le serveur et puis exécutez la commande suivante :

```
pnputil –i –a <driverinf>
```

Où *driverinf* est le nom de fichier du fichier .inf du pilote.

Si vous y êtes invité, redémarrez l’ordinateur.

Pour voir quels pilotes sont installés, exécutez la commande suivante : 

```
sc query type= driver
```

> [!NOTE] 
> N’oubliez pas d’inclure l’espace après le signe égal pour que la commande s’exécute correctement.

Pour désactiver un pilote de périphérique, exécutez la commande suivante :

```
sc delete <service_name>
```

Où *service_name* est le nom du service que vous avez obtenu lors de l’exécution **type de requête sc = pilote**.