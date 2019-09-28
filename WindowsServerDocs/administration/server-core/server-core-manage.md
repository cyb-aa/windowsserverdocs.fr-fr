---
title: Gérer le Server Core
description: En savoir plus sur la gestion d’une installation Server Core de Windows Server
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 07/23/2019
ms.openlocfilehash: bd96dbfc93f3999d8fb3ddf7ec94cc11025bba30
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383399"
---
# <a name="manage-a-server-core-server"></a>Gérer un serveur Server Core
 
> S’applique à : Windows Server 2019, Windows Server 2016 et Windows Server (canal semi-annuel)

Vous pouvez gérer un serveur Server Core des manières suivantes :
- Utilisation du [Centre d’administration Windows](../../manage/windows-admin-center/overview.md)
- Utilisation de [Outils d’administration de serveur distant](../../remote/remote-server-administration-tools.md) s’exécutant sur Windows 10
- Localement et à distance avec Windows PowerShell
- À distance à l’aide de [Gestionnaire de serveur](../server-manager/server-manager.md)
- À distance à l’aide d’un [composant logiciel enfichable MMC](#managing-with-microsoft-management-console)
- À distance avec [services Bureau à distance](#managing-with-remote-desktop-services)

Vous pouvez également ajouter du matériel et gérer les pilotes localement, à condition de le faire à partir de la ligne de commande.

Il existe des limitations et des conseils importants à prendre en compte lorsque vous travaillez avec Server Core :

- Si vous fermez toutes les fenêtres d’invite de commandes et souhaitez ouvrir une nouvelle fenêtre d’invite de commandes, vous pouvez le faire à partir du gestionnaire des tâches. Appuyez sur **CTRL @ no__t-1ALT @ no__t-2DELETE**, cliquez sur **Démarrer le gestionnaire des tâches**, cliquez sur plus de **Détails > fichier > exécuter**, puis tapez **cmd. exe**. (Tapez **PowerShell. exe** pour ouvrir une fenêtre de commande PowerShell.) Vous pouvez également vous déconnecter, puis vous reconnecter.
- Les commandes ou les outils qui essaient de démarrer l’Explorateur Windows ne fonctionnent pas. Par exemple, en exécutant **Start.** à partir d’une invite de commandes ne fonctionnera pas.
- Il n’existe aucune prise en charge du rendu HTML ou de l’aide HTML dans Server Core.
- Server Core prend en charge Windows Installer en mode silencieux afin que vous puissiez installer des outils et des utilitaires à partir de Windows Installer fichiers. Lorsque vous installez Windows Installer packages sur Server Core, utilisez l’option **/qb** pour afficher l’interface utilisateur de base.
- Pour modifier le fuseau horaire, exécutez **Set-Date**.
- Pour modifier les paramètres internationaux, exécutez **le contrôle Intl. cpl**.
- **Control. exe** ne s’exécute pas de manière autonome. Vous devez l’exécuter avec **timedate. cpl** ou **Intl. cpl**.
- **Winver. exe** n’est pas disponible dans Server Core. Pour obtenir des informations de version, utilisez **systeminfo. exe**.

## <a name="managing-server-core-with-windows-admin-center"></a>Gestion de Server Core avec le centre d’administration Windows
[Windows Admin Center](../../manage/windows-admin-center/overview.md) est une application de gestion basée sur un navigateur qui permet d’administrer localement des serveurs Windows Server, sans dépendance à Azure ou au cloud. Windows Admin Center vous donne un contrôle total sur tous les aspects de votre infrastructure de serveurs. Il s’avère particulièrement utile pour la gestion de réseaux privés qui ne sont pas connectés à Internet. Vous pouvez installer le centre d’administration Windows sur Windows 10, sur un serveur de passerelle, ou sur une installation de Windows Server avec expérience utilisateur, puis vous connecter au système Server Core que vous souhaitez gérer.

## <a name="managing-server-core-remotely-with-server-manager"></a>Gestion à distance de Server Core avec Gestionnaire de serveur

Gestionnaire de serveur est une console de gestion dans Windows Server qui vous permet d’approvisionner et de gérer des serveurs Windows locaux et distants à partir de vos ordinateurs de bureau, sans avoir besoin d’un accès physique aux serveurs ni d’activer le protocole RDP (Bureau à distance Protocol). connexions à chaque serveur. Gestionnaire de serveur prend en charge la gestion multiserveur à distance.

Pour permettre à votre serveur local d’être géré par Gestionnaire de serveur s’exécutant sur un serveur distant, exécutez l’applet de commande Windows PowerShell configure **-SMRemoting. exe – Enable**.

## <a name="managing-with-microsoft-management-console"></a>Gestion à l’aide de Microsoft Management Console

Vous pouvez utiliser de nombreux composants logiciels enfichables pour la console MMC (Microsoft Management Console) à distance pour gérer votre serveur Server Core.

Pour utiliser un composant logiciel enfichable MMC afin de gérer un serveur Server Core qui est membre du domaine : 

1. Démarrez un composant logiciel enfichable MMC, tel que gestion de l’ordinateur.
2. Cliquez avec le bouton droit sur le composant logiciel enfichable, puis cliquez sur **se connecter à un autre ordinateur**.
2. Tapez le nom d’ordinateur du serveur Server Core, puis cliquez sur **OK**. Vous pouvez maintenant utiliser le composant logiciel enfichable MMC pour gérer le serveur Server Core comme vous le feriez pour tout autre ordinateur ou serveur.

Pour utiliser un composant logiciel enfichable MMC afin de gérer un serveur Server Core qui n’est *pas* membre du domaine : 

1. Établissez d’autres informations d’identification à utiliser pour se connecter à l’ordinateur Server Core en tapant la commande suivante à une invite de commandes sur l’ordinateur distant :

   ```
   cmdkey /add:<ServerName> /user:<UserName> /pass:<password>
   ```

   Si vous souhaitez être invité à entrer un mot de passe, omettez l’option **/Pass** .

2. Lorsque vous y êtes invité, tapez le mot de passe correspondant au nom d’utilisateur que vous avez spécifié.
   Si le pare-feu sur le serveur Server Core n’est pas déjà configuré pour autoriser la connexion des composants logiciels enfichables MMC, suivez les étapes ci-dessous pour configurer le pare-feu Windows afin d’autoriser le composant logiciel enfichable MMC. Passez ensuite à l’étape 3.
3. Sur un autre ordinateur, démarrez un composant logiciel enfichable MMC, tel que gestion de l' **ordinateur**.
4. Dans le volet gauche, cliquez avec le bouton droit sur le composant logiciel enfichable, puis cliquez sur **se connecter à un autre ordinateur**. (Par exemple, dans l’exemple gestion de l’ordinateur, cliquez avec le bouton droit sur gestion de l' **ordinateur (local)** .)
5. Sur **un autre ordinateur**, tapez le nom d’ordinateur du serveur Server Core, puis cliquez sur **OK**. Vous pouvez maintenant utiliser le composant logiciel enfichable MMC pour administrer le serveur en mode d’installation minimale comme vous le feriez sur n’importe quel autre ordinateur exécutant un système d’exploitation Windows Server.

### <a name="to-configure-windows-firewall-to-allow-mmc-snap-ins-to-connect"></a>Pour configurer le Pare-feu Windows afin d’autoriser la connexion de composants logiciels enfichables MMC
Pour autoriser tous les composants logiciels enfichables MMC à se connecter, exécutez la commande suivante :

```PowerShell
Enable-NetFirewallRule -DisplayGroup "Remote Administration"
```

Pour autoriser uniquement la connexion de composants logiciels enfichables MMC spécifiques, exécutez la commande suivante :

```PowerShell
Enable-NetFirewallRule -DisplayGroup "<rulegroup>"
```

Où *RuleGroup* est l’un des éléments suivants, selon le composant logiciel enfichable que vous souhaitez connecter :

| Composant logiciel enfichable MMC                            | Groupe de règles                                            |
| ---------------------------------------- | ------------------------------------------------------- |
| Observateur d’événements                           | Gestion à distance des journaux des événements                           |
| Services                               | Gestion des services à distance                             |
| Dossiers partagés                         | Partage de fichiers et d’imprimantes                              |
| Planificateur de tâches                         | Journaux et alertes de performance, partage de fichiers et d’imprimantes |
| Gestion des disques                        | Gestion des volumes à distance                              |
| Pare-feu Windows et sécurité avancée | Gestion à distance du Pare-feu Windows                    |


> [!NOTE] 
> Certains composants logiciels enfichables MMC n’ont pas de groupe de règles correspondant qui leur permet de se connecter via le pare-feu. Cependant, l’activation des groupes de règles Observateur d’événements, Services ou Dossiers partagés permet d’autoriser la connexion de la plupart des autres composants logiciels enfichables. 
>
> Par ailleurs, certains composants logiciels enfichables nécessitent une configuration supplémentaire pour pouvoir se connecter via le Pare-feu Windows :
>
> - Gestion des disques. Vous devez d’abord démarrer le service VDS (Virtual Disk Service) sur le serveur en mode d’installation minimale. Vous devez également configurer les règles du composant logiciel enfichable Gestion des disques de manière appropriée sur le serveur qui exécute ce composant.
> - Moniteur de sécurité IP. Vous devez commencer par activer la gestion à distance de ce composant logiciel enfichable. Pour ce faire, à l’invite de commandes, tapez **cscript \windows\system32\scregedit.wsf/im 1**
> - Fiabilité et performances. Ce composant logiciel enfichable ne nécessite pas de configuration supplémentaire, mais son utilisation sur un serveur en mode d’installation minimale vous permet uniquement de surveiller les performances. Il ne fournit pas de données sur la fiabilité.

## <a name="managing-with-remote-desktop-services"></a>Gestion avec Services Bureau à distance

Vous pouvez utiliser [Bureau à distance](../../remote/remote-desktop-services/welcome-to-rds.md) pour gérer un serveur Server Core à partir d’ordinateurs distants.

Avant de pouvoir accéder à Server Core, vous devez exécuter la commande suivante : 

```
cscript C:\Windows\System32\Scregedit.wsf /ar 0
```

Cette commande configure le mode Bureau à distance pour administration afin d’autoriser les connexions.

## <a name="add-hardware-and-manage-drivers-locally"></a>Ajouter du matériel et gérer les pilotes localement

Pour ajouter du matériel à un serveur Server Core, suivez les instructions fournies par le fournisseur de matériel pour l’installation du nouveau matériel. 

Si le matériel n’est pas Plug-and-Play, vous devez installer le pilote manuellement. Pour ce faire, copiez les fichiers de pilote dans un emplacement temporaire sur le serveur, puis exécutez la commande suivante :

```
pnputil –i –a <driverinf>
```

Où *driverinf* est le nom de fichier du fichier. inf du pilote.

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

Où *nom_service* est le nom du service que vous avez obtenu lors de l’exécution de la **requête SC type = Driver**.