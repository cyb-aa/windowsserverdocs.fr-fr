---
title: Gérer WindowsServer
description: Découvrez des outils, des recommandations et des conseils déiés à la gestion de WindowsServer
ms.prod: windows-server-threshold
ms.technology: manage
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 03/16/2018
ms.localizationpriority: medium
ms.openlocfilehash: 4faadb811927626c26a5b01e2ce0598d40792b68
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066933"
---
# Gérer WindowsServer

>S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012

>[!TIP]
> Vous recherchez des informations sur des versions plus anciennes de WindowsServer? Consultez nos autres [bibliothèques Windows Server](/previous-versions/windows/) sur docs.microsoft.com. Vous pouvez également [rechercher dans ce site](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) des informations spécifiques.

 <ul class="cardse panelContent cols cols3">
    <li>
        <a href="https://docs.microsoft.com/windows-insider/at-work-pro/wip-4-biz-feedback-hub">
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h2>Gérer</h2>
                <p>Une fois que vous avez déployé WindowsServer dans votre environnement, en incluant les rôles spécifiques pour les fonctionnalités et fonctions dont vous avez besoin, l’étape suivante consiste en la gestion de ces serveurs. WindowsServer comporte un certain nombre d’outils pour vous aider à comprendre votre environnement WindowsServer, gérer des serveurs spécifiques, régler finement les performances et, pour finir, automatiser de nombreuses tâches de gestion. </p>
                    </div>
                </div>
            </div>
        </div>
        </a>
    </li> 
</ul> 

## Gérer les environnements et les systèmes WindowsServer
Les outils que vous utilisez pour gérer les instances de WindowsServer dépendent, dans une grande mesure, des types de système que vous avez déployés (WindowsServer avec la fonctionnalité Expérience de bureau par rapport à Server Core), des machines physiques par rapport aux machines virtuelles et de l’emplacement de vos serveurs. Utilisez les informations suivantes pour accomplir les tâches de gestion de base sur WindowsServer.

Utilisez le tableau suivant pour déterminer les outils à utiliser.

| Je suis   | Installer et exécuter Windows Admin Center | Exécuter le Gestionnaire de serveur WindowsServer | Exécuter le Gestionnaire de serveur dans RSAT sur Windows10 |
|--------|----------------------|--------------------------------------|------------------------------------------|
| Devant un PC Windows10 | X  |                                      | X                                        |
| Devant un système WindowsServer exécutant l’expérience de bureau | X | X | X |
| Devant un système WindowsServer exécutant Server Core |X (installer sur Windows10, utiliser pour gérer Server Core) | | X |
| Éloigné de mon système WindowsServer |X | | X |
| Éloigné de mon système WindowsServer, mais il dispose de l’expérience de bureau |X | Utiliser les services Bureau à distance pour se connecter au serveur, puis utiliser le Gestionnaire de serveur | X |

Outre les outils mentionnés ci-après, vous pouvez également utiliser les [Services Bureau à distance](../remote/remote-desktop-services/welcome-to-rds.md) pour l’accès aux serveurs locaux, distants et virtuels. Vous pouvez alors utiliser le Gestionnaire de serveur pour effectuer les tâches de gestion.

### Gérer des systèmes locaux, des systèmes distants et des systèmes sans interface utilisateur avec Windows Admin Center
[Windows Admin Center](../manage/windows-admin-center/overview.md) est une application de gestion basée sur un navigateur qui permet d’administrer localement des serveurs WindowsServer, sans dépendance à Azure ou au cloud. Windows Admin Center vous donne un contrôle total sur tous les aspects de votre infrastructure de serveurs. Il s’avère particulièrement utile pour la gestion de réseaux privés qui ne sont pas connectés à Internet. Vous pouvez installer Windows Admin Center sur Windows10, sur un serveur passerelle, ou directement sur le système WindowsServer que vous souhaitez gérer.

>[!NOTE]
>Windows Admin Center est le nom officiel de ce que nous appelions auparavant «Projet Honolulu».

### Gérer des systèmes locaux avec le Gestionnaire de serveur
Le [Gestionnaire de serveur](server-manager/server-manager.md) est une console de gestion incluse dans l’installation complète de WindowsServer. (Il n’est pas disponible pour les installations qui n’ont pas d’interface utilisateur - Server Core n’inclut pas le Gestionnaire de serveur). Utilisez le Gestionnaire de serveur pour installer et supprimer des rôles de serveur, ajouter et supprimer des serveurs distants, démarrer et arrêter des services et afficher les données collectées sur votre environnement.

### Gérer les systèmes distants et les systèmes sans interface utilisateur avec RSAT (Remote Server Administration Tools)
Si votre environnement comprend des installations de Server Core ou des serveurs distants (locaux ou des ordinateurs virtuels), vous pouvez utiliser les outils [RSAT (Remote Server Administration Tools)](../remote/remote-server-administration-tools.md) pour gérer ces systèmes. Les outils RSAT comprennent le Gestionnaire de serveur, qui vous permet de gérer tous vos serveurs.

> [!IMPORTANT]
> RSAT s’exécute sous Windows10. Il est impossible d’installer les outils RSAT sur WindowsServerCore.

Vous pouvez également gérer les installations Server Core à partir de la ligne de commande. Voir [Tâches d’administration dans Server Core](server-core/server-core-administer.md).

### Gérer les mises à jour des systèmes WindowsServer
Vous pouvez utiliser [WindowsServer Update Services (WSUS)](windows-server-update-services/get-started/windows-server-update-services-wsus.md) pour gérer et déployer les mises à jour des systèmes de votre environnement WindowsServer.

## Collecter des informations sur votre environnement
Bon nombre de décisions que vous prenez en tant qu’administrateur dépendent des données recueillies sur les systèmes et les utilisateurs de votre environnement. Utilisez les informations et les outils suivants pour collecter ces données.

Commencez par [Configurer les données de diagnostic Windows dans votre organisation](/windows/configuration/configure-windows-diagnostic-data-in-your-organization) pour plus d’informations sur les données de diagnostic qui peuvent être collectées à partir de Windows10 et WindowsServer.

### [Collection des événements de configuration et de démarrage](get-started-with-setup-and-boot-event-collection.md)
La Collection des événements de configuration et de démarrage vous permet de désigner un ordinateur «collecteur» capable de rassembler divers événements importants qui se produisent sur d’autres ordinateurs lors de leur démarrage ou de leur processus de configuration. Vous pouvez ensuite analyser les événements collectés avec l’Observateur d’événements, l’analyseur de messages, Wevtutil ou les applets de commande Windows PowerShell. 

### [Journalisation de l’inventaire logiciel](software-inventory-logging/get-started-with-software-inventory-logging.md)

La journalisation de l’inventaire logiciel dans WindowsServer est une fonctionnalité comprenant des applets de commande PowerShell qui permettant aux administrateurs de serveurs de récupérer la liste des logiciels Microsoft qui sont installés sur ces derniers. De plus, elle collecte et transmet ces données régulièrement à un serveur web cible via le réseau, à l’aide du protocole HTTPS, à des fins d’agrégation. La gestion de cette fonctionnalité, principalement pour la collecte et le transfert quotidiens, est également assurée par des commandes PowerShell.

### [Journalisation des accès utilisateur](user-access-logging/get-started-with-user-access-logging.md)

La Journalisation des accès utilisateur regroupe les événements d’appareils clients uniques et les requête des utilisateurs qui sont journalisés sur un ordinateur exécutant WindowsServer2016, WindowsServer2012R2 ou WindowsServer2012dans une base de données locale. Ces enregistrements sont accessibles (via une requête d’un administrateur de serveur) pour récupérer les quantités et les instances par rôle de serveur, par utilisateur, par périphérique, par serveur local et par date. De plus, la journalisation des accès utilisateur permet aux développeurs de logiciels nonMicrosoft d’instrumenter les événements de journalisation des accès utilisateur qui doivent être regroupés. 

## Optimiser les performances de votre environnement WindowsServer
Utilisez les informations suivantes pour optimiser les performances de votre environnement.

### [Conseils d’optimisation des performances](performance-tuning/index.md)
Passez en revue un ensemble d’instructions utiles pour régler les paramètres de serveur dans WindowsServer2016 et obtenir des gains de performance ou d’efficacité énergétique, en particulier lorsque la nature de la charge de travail varie peu au fil du temps.

### [Microsoft Server Performance Advisor](server-performance-advisor/microsoft-server-performance-advisor.md)

Avec MicrosoftServer Performance Advisor (SPA), vous pouvez collecter des indicateurs de performances pour diagnostiquer les problèmes de performances sur les serveurs Windows, de manière transparente, sans ajouter d’agents logiciels ni reconfigurer les serveurs de production. SPA génère des rapports de performances complets et des historiques graphiques contenant des recommandations.


## Automatiser la gestion de WindowsServer

WindowsServer inclut un ensemble de commandes et de modules Windows PowerShell que vous pouvez utiliser pour automatiser les tâches de gestion.

### [Windows PowerShell](/powershell/scripting/powershell-scripting?view=powershell-5.1)
Windows PowerShell est un interpréteur de ligne de commande et un langage de script conçu pour automatiser rapidement les tâches d’administration. 

### [Commandes Windows](windows-commands/windows-commands.md)

Les outils de ligne de commande Windows sont utilisés pour effectuer des tâches d’administration dans Windows. Vous pouvez utiliser la référence des commandes pour vous familiariser avec les outils de ligne de commande, pour en savoir plus sur le shell de commande et automatiser les tâches en ligne de commande à l’aide de fichiers de commandes ou d’outils de script.

## Présentation de Windows Server Insider
### [Insights système](..\manage\system-insights\overview.md)
Insights système est une nouvelle fonctionnalité qui prend en charge les analyses prédictives en mode natif dans Windows Server. Ces fonctionnalités de prévision analysent en local les données du système Windows Server, telles que des compteurs de performance ou les événements ETW, pour aider les administrateurs informatiques à détecter de manière proactive et à résoudre les comportements problématiques dans leurs déploiements. 
