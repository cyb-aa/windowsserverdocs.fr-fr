---
title: Présentation des extensions de Windows Admin Center
description: Présentation des extensions du SDK Windows Admin Center (projet Honolulu)
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 4e54101e90005a1845820ecf0bb99df527ac7051
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869775"
---
# <a name="understanding-windows-admin-center-extensions"></a>Présentation des extensions de Windows Admin Center

>S'applique à : Windows Admin Center, Windows Admin Center Preview

Si vous n’êtes pas encore familiarisé avec le fonctionnement du centre d’administration Windows, commençons par l’architecture de haut niveau. Windows Admin Center est constitué de deux composants principaux :

- Un **service web** léger qui affiche les pages web de l’interface utilisateur de Windows Admin Center en réponse aux requêtes du navigateur web.
- Un **composant de passerelle** qui écoute les demandes de l'API REST dans les pages web et relaie les appels WMI ou les scripts PowerShell à exécuter sur un serveur ou un cluster cible.

![Architecture de Windows Admin Center](../media/understand-extensions/wac-architecture-500px.png)

Les pages web de l'interface utilisateur de Windows Admin Center traitées par le service web présentent deux principaux composants d'interface utilisateur en matière d'extensibilité, les solutions et les outils, qui sont implémentés sous forme d'extensions, ainsi qu'un troisième type d’extensions appelé plug-ins de passerelle.

## <a name="solution-extensions"></a>Extensions de solution

Dans l’écran d’accueil de Windows Admin Center, par défaut, vous pouvez ajouter des connexions de quatre types possibles : connexions de Windows Server, connexions d'ordinateur Windows, connexions de cluster de basculement et connexions de cluster hyperconvergé. Une fois qu’une connexion est ajoutée, son nom et son type s'affichent dans l’écran d’accueil. Un clic sur le nom de connexion effectue une tentative de connexion au serveur cible ou au cluster, puis de chargement de l’interface utilisateur de connexion.

![Architecture de Windows Admin Center](../media/understand-extensions/solutions-ui.png)

Chacun de ces types de connexion est mappé à une solution et les solutions sont définies par le biais d’un type d’extension appelé extensions de « solution ». Les solutions définissent généralement un type d'objet unique que vous souhaitez gérer via Windows Admin Center, tels que des serveurs, des ordinateurs ou des clusters de basculement. Vous pouvez également définir une nouvelle solution pour vous connecter à et gérer d’autres appareils, comme des commutateurs réseau et des serveurs Linux, ou même des services tels que les Services Bureau à distance.

## <a name="tool-extensions"></a>Extensions d'outil

Lorsque vous cliquez sur une connexion dans l’écran d’accueil de Windows Admin Center et que vous vous connectez, l’extension de solution pour le type de connexion sélectionné sera chargée et vous obtiendrez l’interface utilisateur de la solution, notamment une liste d’outils dans le volet de navigation de gauche. Lorsque vous cliquez sur un outil, l’interface utilisateur de l’outil est chargée et affichée dans le volet de droite.

![Architecture de l’interface utilisateur de Windows Admin Center](../media/understand-extensions/ui-architecture.png)

Chacun de ces outils est défini par un deuxième type d’extension appelé extension « outil ». Lorsqu’un outil est chargé, il peut exécuter des appels WMI ou des scripts PowerShell sur un serveur ou un cluster cible et afficher des informations dans l’interface utilisateur ou exécuter des commandes en fonction de l’entrée utilisateur. L'extension outil définit les solutions pour lesquelles il doit s’afficher, ce qui produit un ensemble d’outils différent pour chaque solution. Si vous créez une nouvelle extension solution, vous devez en outre écrire une ou plusieurs extensions d'outil qui fournissent des fonctionnalités pour la solution.

![Liste des outils pour chaque solution](../media/understand-extensions/tools-for-solutions.png)

## <a name="gateway-plugins"></a>Plug-ins de passerelle

Le service de passerelle expose l’API REST pour l’interface utilisateur à appeler et relaie les commandes et les scripts à exécuter sur la cible. Le service de passerelle peut être étendu par des plug-ins de passerelle qui prennent en charge différents protocoles. Windows Admin Center est prédéfini avec deux plug-ins de passerelle, l’un pour l’exécution des scripts PowerShell et l’autre pour les commandes WMI. Si vous avez besoin de communiquer avec la cible via un protocole autre que PowerShell ou WMI, tels que REST, vous pouvez créer un plug-in de passerelle pour cela.

## <a name="next-steps"></a>Étapes suivantes

Selon les fonctionnalités que vous souhaitez créer dans Windows Admin Center, la [création d’une extension d'outil](develop-tool.md) pour une solution de serveur ou de cluster existante peut être suffisante. C'est la première étape la plus simple pour créer des extensions. Toutefois, si votre fonctionnalité doit permettre la gestion d’un appareil, d'un service ou d'un élément entièrement nouveau, plutôt qu’un serveur ou un cluster, vous devez envisager la [création d’une extension de solution](develop-solution.md) avec un ou plusieurs outils. Enfin, si vous avez besoin de communiquer avec la cible par le biais d’un protocole autre que WMI ou PowerShell, vous devez [créer un plug-in de passerelle](develop-gateway-plugin.md). [Consultez d'autres informations sur](developing-extensions.md) pour apprendre à configurer votre environnement de développement et commencer l’écriture de votre première extension.
