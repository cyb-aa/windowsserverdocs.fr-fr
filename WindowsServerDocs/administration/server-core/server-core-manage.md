---
title: Gérer les serveurs principaux
description: Apprenez à gérer une installation Server Core de Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: 6836e5db36727294d215f7f98e0faeede55a612a
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "1718709"
---
# <a name="manage-a-server-core-server"></a>Gérer un serveur Server Core
 
> S’applique à: Windows Server (Channel annuel séparées) et Windows Server 2016

Vous pouvez gérer un serveur server Core de la manière suivante:
- À l’aide du [Centre d’administration de Windows](../../manage/windows-admin-center/overview.md)
- À l’aide des [Outils d’Administration de serveur distant](../../remote/remote-server-administration-tools.md) en cours d’exécution sur Windows 10
- Localement et à distance à l’aide de Windows PowerShell
- À distance à l’aide du [Gestionnaire de serveur](../server-manager/server-manager.md)
- À distance en utilisant un [composant logiciel enfichable MMC](#managing-with-microsoft-management-console)
- À distance avec les [Services Bureau à distance](#managing-with-remote-desktop-services)

Vous pouvez également ajouter du matériel et gérer les pilotes localement, dans la mesure où vous le faire à partir de la ligne de commande.

Il existe certaines limitations importantes et des conseils à prendre en compte lorsque vous travaillez avec Server Core:

- Si vous fermez toutes les fenêtres d’invite de commandes et que vous souhaitez ouvrir une nouvelle fenêtre d’invite de commandes, vous pouvez le faire dans le Gestionnaire des tâches. Appuyez sur **CTRL\ + ALT\ + SUPPR**, cliquez sur **Démarrer le Gestionnaire des tâches**, cliquez sur **plus de détails > fichier > exécuter**, puis tapez **cmd.exe**. (Tapez **Powershell.exe** pour ouvrir une fenêtre de commande PowerShell). Vous pouvez également vous déconnecter et vous reconnecter.
- Toute commande ou un outil qui tente de démarrer l’Explorateur Windows ne fonctionnera pas. Par exemple, en cours d’exécution **Démarrer.** à partir d’une invite de commandes ne fonctionneront pas.
- Il n’existe aucune prise en charge pour le rendu HTML ou de l’aide HTML dans Server Core.
- Serveur principal prend en charge Windows Installer en mode silencieux, afin que vous pouvez installer les outils et utilitaires à partir des fichiers Windows Installer. Lors de l’installation des packages Windows Installer sur le serveur principal, utilisez **l’option/qb** pour afficher l’interface utilisateur de base.
- Pour modifier le fuseau horaire, exécutez **Set-Date**.
- Pour modifier les paramètres internationaux, exécutez **intl.cpl du contrôle**.
- **Control.exe** n’exécute son propre. Vous devez l’exécuter avec **Timedate.cpl** ou **Intl.cpl**.
- **Winver.exe** n’est pas disponible dans le serveur principal. Pour obtenir des informations de version utilisez **Systeminfo.exe**.

## <a name="managing-server-core-with-windows-admin-center"></a>Gestion des serveurs principaux avec le centre d’administration de Windows
[Centre d’administration de Windows](../../manage/windows-admin-center/overview.md) est une application de gestion basée sur navigateur qui permet l’administration locale des serveurs Windows avec aucune dépendance Azure ou dans le cloud. Centre d’administration de Windows vous donne un contrôle total sur tous les aspects de votre infrastructure de serveur et est particulièrement utile pour la gestion de réseaux privés qui ne sont pas connectés à Internet. Vous pouvez installer le centre d’administration de Windows sur Windows 10, sur un serveur de passerelle ou sur une installation de Windows Server avec la fonctionnalité expérience utilisateur et puis se connecter au système Server Core que vous souhaitez gérer.

## <a name="managing-server-core-remotely-with-server-manager"></a>Gestion de Server Core à distance avec le Gestionnaire de serveur

Gestionnaire de serveur est une console de gestion de Windows Server qui vous permet de mettre en service et gérer les serveurs Windows locales et distantes à partir de votre bureau, sans nécessiter l’accès aux serveurs physique, ou activer le protocole de bureau à distance (RDP) connexions à chaque serveur. Gestionnaire de serveur prend en charge la gestion à distance, plusieurs serveur.

Pour activer votre serveur local à être gérés par le Gestionnaire de serveur en cours d’exécution sur un serveur distant, exécutez l’applet de commande Windows PowerShell **Configure-SMRemoting.exe – activer**.

## <a name="managing-with-microsoft-management-console"></a>Gestion de la Console de gestion Microsoft

Vous pouvez utiliser plusieurs logiciels enfichables pour Microsoft Management Console (MMC) à distance pour gérer votre serveur Server Core.

Pour utiliser un composant logiciel enfichable MMC pour gérer un serveur Server Core qui est un membre de domaine: 

1. Démarrer un composant logiciel enfichable MMC, telles que la gestion de l’ordinateur.
2. Cliquez sur le composant logiciel enfichable, puis cliquez sur **se connecter à un autre ordinateur**.
2. Tapez le nom d’ordinateur du serveur principal du serveur, puis cliquez sur **OK**. Vous pouvez maintenant utiliser le composant logiciel enfichable MMC pour gérer le serveur de base de serveur comme vous le feriez pour n’importe quel autre ordinateur ou serveur.

Utiliser un composant logiciel enfichable MMC pour gérer un serveur server Core c'est-à-dire *pas* membre d’un domaine: 

1. Établir une relation d’autres informations d’identification à utiliser pour se connecter à l’ordinateur Server Core en tapant la commande suivante à une invite de commandes sur l’ordinateur distant:
   ```
   cmdkey /add:<ServerName> /user:<UserName> /pass:<password>
   ```
   Si vous voulez être invité à entrer un mot de passe, omettez **l’option/transmettre** .

2. Lorsque vous y êtes invité, tapez le mot de passe du nom d’utilisateur que vous avez spécifié.
   Si le pare-feu sur le serveur principal du serveur n’est pas déjà configuré pour autoriser des logiciels enfichables MMC pour se connecter, suivez les étapes ci-dessous pour configurer le pare-feu Windows pour autoriser le composant logiciel enfichable MMC. Passez à l’étape 3.
3. Sur un autre ordinateur, démarrez un composant logiciel enfichable MMC, telles que la **Gestion de l’ordinateur**.
4. Dans le volet gauche, cliquez sur le composant logiciel enfichable, puis cliquez sur **se connecter à un autre ordinateur**. (Par exemple, dans l’exemple de gestion de l’ordinateur, vous le feriez avec le bouton droit **Gestion de l’ordinateur (Local)**.)
5. Dans **un autre ordinateur**, tapez le nom d’ordinateur du serveur principal du serveur, puis cliquez sur **OK**. Vous pouvez maintenant utiliser le composant logiciel enfichable MMC pour gérer le serveur de base de serveur comme vous le feriez pour n’importe quel autre ordinateur exécutant un système d’exploitation Windows.

### <a name="to-configure-windows-firewall-to-allow-mmc-snap-ins-to-connect"></a>Pour configurer le pare-feu Windows pour autoriser enfichable MMC (s) pour se connecter
Pour autoriser tous les enfichables MMC pour se connecter, exécutez la commande suivante:

```
Enable-NetFirewallRule -DisplayGroup "Remote Administration"
```

Pour autoriser uniquement spécifiques enfichables MMC pour se connecter, exécutez la commande suivante:
```
Enable-NetFirewallRule -DisplayGroup "<rulegroup>"
```

Où *rulegroup* est une des valeurs suivantes, selon le composant logiciel enfichable que vous souhaitez vous connecter:

| Le composant logiciel enfichable MMC                            | Groupe de règles                                            |
|----------------------------------------|-------------------------------------------------------|
| Observateur d’événements                           | Gestion du journal des événements distants                           |
| Services                               | Gestion de Service à distance                             |
| Dossiers partagés                         | Partage de fichiers et imprimante                              |
| Planificateur de tâches                         | Journaux de performances et alertes, partage de fichiers et imprimante |
| Gestion des disques                        | Gestion des volumes à distance                              |
| Le pare-feu Windows et les fonctions avancées de sécurité | Gestion à distance du pare-feu Windows                    |


> [!NOTE] 
> Certains composants logiciels enfichables MMC n’ont pas un groupe de règles correspondantes qui leur permet de se connecter via le pare-feu. Toutefois, l’activation de groupes de règles pour l’Observateur d’événements, les Services ou les dossiers partagés permet la plupart des autres composants logiciels enfichables pour se connecter. 
>
> En outre, certains composants logiciels enfichables de plus amples configuration avant de se connecter via le pare-feu Windows:
>
> - Gestion des disques. Vous devez d’abord démarrer le Service de disque virtuel (VDS) sur l’ordinateur du serveur principal. Vous devez également configurer les règles de gestion des disques correctement sur l’ordinateur qui exécute le composant logiciel enfichable MMC.
> - Moniteur de sécurité IP. Vous devez d’abord activer la gestion à distance de ce composant logiciel enfichable. Pour ce faire, à une invite de commandes, tapez **Cscript \windows\system32\scregedit.wsf /im 1**
> - Fiabilité et performances. Le composant logiciel enfichable ne requiert aucune autre configuration, mais lorsque vous l’utilisez pour analyser un ordinateur serveur principal, vous ne pouvez analyser les données de performances. Les données de fiabilité ne seront pas disponibles.

## <a name="managing-with-remote-desktop-services"></a>Gestion des services Bureau à distance

Vous pouvez utiliser le [Bureau à distance](../../remote/remote-desktop-services/welcome-to-rds.md) pour gérer un serveur server Core sur des ordinateurs distants.

Avant de pouvoir accéder Server Core, vous devez exécuter la commande suivante: 
```
cscript C:\Windows\System32\Scregedit.wsf /ar 0
```
Cela permet le Bureau à distance pour le mode Administration accepte les connexions.

## <a name="add-hardware-and-manage-drivers-locally"></a>Ajout de matériel et de gérer les pilotes localement

Pour ajouter le matériel pour un serveur server Core, suivez les instructions fournies par le fournisseur de matériel pour l’installation de nouveau matériel. 

Si le matériel n’est pas plug-and-play, vous devrez installer manuellement le pilote. Pour ce faire, copiez les fichiers du pilote vers un emplacement temporaire sur le serveur, puis exécutez la commande suivante:
```
pnputil –i –a <driverinf>
```
Où *driverinf* est le nom de fichier du fichier .inf du pilote.

Si vous y êtes invité, redémarrez l’ordinateur.

Pour voir quels pilotes sont installés, exécutez la commande suivante: 
```
sc query type= driver
```

> [!NOTE] 
> Vous devez inclure l’espace après le signe égal de la commande se termine avec succès.

Pour désactiver un pilote de périphérique, exécutez la commande suivante: 
```
sc delete <service_name>
```

Où *nom_service* est le nom du service que vous avez **type de requête sc = pilote**.