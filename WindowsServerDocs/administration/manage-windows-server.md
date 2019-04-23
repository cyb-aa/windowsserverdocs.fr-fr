---
title: Gestion
description: En savoir plus sur les outils, les recommandations et les conseils sur la gestion de Windows Server
ms.prod: windows-server-threshold
layout: LandingPage
ms.technology: manage
ms.topic: landing-page
author: lizap
ms.author: elizapo
ms.localizationpriority: high
ms.openlocfilehash: e6a5357e3e33b3d3318a3e281bbb5c80be842155
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890510"
---
# <a name="management"></a>Gestion


>[!TIP]
> Vous recherchez des informations sur des versions plus anciennes de Windows Server ? Consultez nos autres [bibliothèques Windows Server](/previous-versions/windows/) sur docs.microsoft.com. Vous pouvez également [rechercher dans ce site](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) des informations spécifiques.

<hr />

Une fois que vous avez déployé Windows Server dans votre environnement, en incluant les rôles spécifiques pour les fonctionnalités et fonctions dont vous avez besoin, l’étape suivante consiste en la gestion de ces serveurs. Windows Server comporte un certain nombre d’outils pour vous aider à comprendre votre environnement Windows Server, gérer des serveurs spécifiques, régler finement les performances et, pour finir, automatiser de nombreuses tâches de gestion. 

Les outils que vous utilisez pour gérer les instances de Windows Server dépendent, dans une grande mesure, des types de système que vous avez déployés (Windows Server avec la fonctionnalité Expérience de bureau par rapport à Server Core), des machines physiques par rapport aux machines virtuelles et de l’emplacement de vos serveurs. Utilisez les informations suivantes pour accomplir les tâches de gestion de base sur Windows Server.

Utilisez le tableau suivant pour déterminer les outils à utiliser.

| Je suis   | Installer et exécuter Windows Admin Center | Exécuter le Gestionnaire de serveur Windows Server | Exécuter le Gestionnaire de serveur dans RSAT sur Windows 10 |
|--------|----------------------|--------------------------------------|------------------------------------------|
| Devant un PC Windows 10 | X  |                                      | X                                        |
| Devant un système Windows Server exécutant l’expérience de bureau | X | X | X |
| Devant un système Windows Server exécutant Server Core |X (installer sur Windows 10, utiliser pour gérer Server Core) | | X |
| Éloigné de mon système Windows Server |X | | X |
| Éloigné de mon système Windows Server, mais il dispose de l’expérience de bureau |X | Utiliser les services Bureau à distance pour se connecter au serveur, puis utiliser le Gestionnaire de serveur | X |

Outre les outils mentionnés ci-après, vous pouvez également utiliser les [Services Bureau à distance](../remote/remote-desktop-services/welcome-to-rds.md) pour l’accès aux serveurs locaux, distants et virtuels. Vous pouvez alors utiliser le Gestionnaire de serveur pour effectuer les tâches de gestion.

<HR />

<ul class="cardsI panelContent">
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3>Gérer les environnements et les systèmes Windows Server</h3>
<HR />
                        <p><h3><a href="../manage/windows-admin-center/overview.md">Gérer des systèmes locaux, les systèmes distants et les systèmes sans interface utilisateur avec Windows Admin Center</a></h3>Application de gestion basée sur un navigateur qui permet d'administrer localement des serveurs Windows Server, sans dépendance à Azure ou au cloud. Windows Admin Center (anciennement appelé « Projet Honolulu ») vous donne un contrôle total sur tous les aspects de votre infrastructure de serveurs et s'avère particulièrement utile pour la gestion de réseaux privés qui ne sont pas connectés à Internet. Vous pouvez installer Windows Admin Center sur Windows 10, sur un serveur passerelle, ou directement sur le système Windows Server que vous souhaitez gérer.</p>
<HR />
                        <p><h3><a href="server-manager/server-manager.md">Gérer des systèmes sur site avec le Gestionnaire de serveur</a></h3>Console de gestion incluse dans l'installation complète de Windows Server. (Il n’est pas disponible pour les installations qui n’ont pas l’interface utilisateur, Server Core n’inclut pas le Gestionnaire de serveur). Utilisez le Gestionnaire de serveur pour installer et supprimer des rôles de serveur, ajouter et supprimer des serveurs distants, démarrer et arrêter les services et afficher les données collectées relatives à votre environnement.</p>
<HR />
                        <p><h3><a href="../remote/remote-server-administration-tools.md">Gérer des systèmes distants et les systèmes sans interface utilisateur avec Server Administration Tools (distant)</a></h3>Si votre environnement comprend des installations de Server Core ou des serveurs distants (locaux ou machines virtuelles), vous pouvez utiliser les outils d'administration de serveur distant (RSAT) pour gérer ces systèmes. Les outils RSAT comprennent le Gestionnaire de serveur, qui vous permet de gérer tous vos serveurs. Notez que les outils RSAT s'exécutent sur Windows 10. Il est impossible d'installer les outils RSAT sur Windows Server Core. Vous pouvez également gérer les installations Server Core à partir de la ligne de commande. Consultez <a href="server-core/server-core-administer.md">tâches d’administration fondamentales dans Server Core</a>
<HR />
                        <p><h3><a href="windows-server-update-services/get-started/windows-server-update-services-wsus.md">Gérer les mises à jour des systèmes Windows Server</a></h3>Utilisez Windows Server Update Services (WSUS) pour gérer et déployer les mises à jour des systèmes dans votre environnement Windows Server.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3>Collecter des informations sur votre environnement</h3>
<HR />
                        <p><h3><a href="get-started-with-setup-and-boot-event-collection.md">Le programme d’installation et de collecte d’événements de démarrage</a></h3>La Collection des événements de configuration et de démarrage vous permet de désigner un ordinateur « collecteur » capable de rassembler divers événements importants qui se produisent sur d’autres ordinateurs lors de leur démarrage ou de leur processus de configuration. Vous pouvez ensuite analyser les événements collectés avec l’Observateur d’événements, l’Analyseur de messages, Wevtutil ou les applets de commande Windows PowerShell. </p>
<HR />
                        <p><h3><a href="software-inventory-logging/get-started-with-software-inventory-logging.md">Software Inventory Logging (SIL)</a></h3>La journalisation de l’inventaire logiciel dans Windows Server est une fonctionnalité comprenant des applets de commande PowerShell qui permettant aux administrateurs de serveurs de récupérer la liste des logiciels qui sont installés sur ces derniers. De plus, elle collecte et transmet ces données régulièrement à un serveur web cible via le réseau, à l'aide du protocole HTTPS, à des fins d'agrégation. La gestion de cette fonctionnalité, principalement pour la collecte et le transfert quotidiens, est également assurée par des commandes PowerShell.</p>
<HR />
                        <p><h3><a href="user-access-logging/get-started-with-user-access-logging.md">Journalisation des (accès utilisateur UAL) des accès utilisateur</a></h3>La Journalisation des accès utilisateur regroupe les événements d’appareils clients uniques et les requête des utilisateurs qui sont journalisés sur un ordinateur exécutant Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 dans une base de données locale. Ces enregistrements sont accessibles (via une requête d’un administrateur de serveur) pour récupérer les quantités et les instances par rôle de serveur, par utilisateur, par périphérique, par serveur local et par date. De plus, la journalisation des accès utilisateur permet aux développeurs de logiciels non Microsoft d’instrumenter les événements de journalisation des accès utilisateur qui doivent être regroupés. </a>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3>Optimiser les performances de votre environnement Windows Server</h3>
<HR />
                        <p><h3><a href="performance-tuning/index.md">Instructions de réglage des performances</a></h3>Passez en revue un ensemble d'instructions utiles pour régler les paramètres de serveur dans Windows Server et obtenir des gains de performances ou d'efficacité énergétique, en particulier lorsque la nature de la charge de travail varie peu au fil du temps.</p>
<HR />
                        <p><h3><a href="server-performance-advisor/microsoft-server-performance-advisor.md">Microsoft Server Performance Advisor</a></h3>Avec Microsoft Server Performance Advisor (SPA), vous pouvez collecter des indicateurs de performances pour diagnostiquer les problèmes de performances sur les serveurs Windows, de manière transparente, sans ajouter d’agents logiciels ni reconfigurer les serveurs de production. SPA génère des rapports de performances complets et des historiques graphiques contenant des recommandations.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3>Automatiser la gestion de Windows Server</h3>
<HR />
                        <p><h3><a href="https://docs.microsoft.com/powershell/scripting/powershell-scripting?view=powershell-5.1">Windows PowerShell</a></h3>Windows PowerShell est un interpréteur de ligne de commande et un langage de script conçu pour automatiser rapidement les tâches d'administration. </p>
<HR />
                        <p><h3><a href="windows-commands/windows-commands.md">Commandes Windows</a></h3>Les outils de ligne de commande Windows sont utilisés pour effectuer des tâches d’administration dans Windows. Vous pouvez utiliser la référence des commandes pour vous familiariser avec les outils de ligne de commande, pour en savoir plus sur le shell de commande et automatiser les tâches en ligne de commande à l’aide de fichiers de commandes ou d’outils de script.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3>Automatiser la gestion de Windows Server</h3>
<HR />
                        <p><h3><a href="..\manage\system-insights\overview.md">Informations système</h3></a>Analytique prédictive natif localement analyser les données de système de Windows Server, telles que les compteurs de performances et des événements ETW, aider les administrateurs informatiques de façon proactive détecter et résoudre les problématique comportement dans les déploiements.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>