---
title: Présentation du canal semi-annuel de WindowsServer
description: Microsoft a rationalisé la maintenance de WindowsServer pour simplifier le test, la gestion et le déploiement des mises à jour de système d’exploitation.
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: jaimeo
ms.localizationpriority: high
ms.date: 05/07/2018
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 9cf87597-b15d-4f43-8aa1-91e60367f011
ms.openlocfilehash: 2995fca3085d6611ecce083685dca0e587913f17
ms.sourcegitcommit: fcc26ec5a2cc73b59c5752377b39c070d288655e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/19/2018
ms.locfileid: "8976725"
---
# Présentation du canal semi-annuel de WindowsServer

>S’applique à: WindowsServer (canal semi-annuel)

Le modèle de publication de WindowsServer propose une nouvelle option qui s’aligne sur les modèles comparables de publication et de maintenance [Windows10](https://docs.microsoft.com/windows/deployment/update/waas-overview) et d'[Office365 ProPlus](https://support.office.com/article/Overview-of-the-upcoming-changes-to-Office-365-ProPlus-update-management-78b33779-9356-4cdf-9d2c-08350ef05cca?ui=en-US&rs=en-US&ad=US). Si vous avez utilisé Windows10 ou Office365ProPlus, ces améliorations vous sont peut-être déjà familières.

**Deux canaux principaux de publication sont proposés aux clients de Windows Server, le canal de maintenance à long terme et le canal semi-annuel.** Vous pouvez conserver les serveurs sur le canal de maintenance à long terme (LTSC), les déplacer vers le nouveau canal semi-annuel, ou avoir des serveurs sur les deux, selon la configuration la mieux adaptée à vos besoins.


## Canal de maintenance à long terme (LTSC)
C’est le modèle de publication que vous connaissez déjà (auparavant appelé «*Branche* LTSB (Long Term Servicing Branch)») dans lequel une nouvelle version majeure de WindowsServer est publiée tous les 2 à 3ans. Les utilisateurs ont droit à 5ans de support standard et 5ans de support étendu. Ce canal convient aux systèmes qui nécessitent une option de maintenance et une stabilité fonctionnelle prolongées. Les déploiements de WindowsServer2016 et des versions antérieures de WindowsServer ne seront pas affectés par les nouvelles versions du canal semi-annuel. Le canal de maintenance à long terme continuera de recevoir des mises à jour de sécurité et autres, mais ne recevra pas les nouvelles fonctionnalités.

> [!Note]  
> **Le produit de canal de maintenance à long terme actuel est WindowsServer2016**. Si vous voulez rester dans ce canal, vous devez installer (ou continuer à utiliser) WindowsServer2016, qui peut être installé selon l'option d’installation minimale ou selon l'option Expérience utilisateur. Voir [Mise en route de WindowsServer2016](server-basics.md) pour plus d’informations. 



## Canal semi-annuel 
Le canal semi-annuel est parfait pour les clients qui innovent rapidement pour tirer parti des nouvelles fonctionnalités du système d’exploitation à un rythme plus soutenu, tant dans les applications, en particulier celles intégrés aux conteneurs et microservices, que dans le datacenter hybride à définition logicielle. Le canal semi-annuel publiera de nouvelles versions des produits WindowsServer deux fois par an, au printemps et en automne. Chaque version de ce canal sera prise en charge pendant 18mois à compter de la version initiale.

La plupart des fonctionnalités introduites dans le canal semi-annuel seront cumulées dans la prochaine version du canal de maintenance à long terme de WindowsServer. Les éditions, les fonctionnalités et le contenu du support peuvent varier d’une version à l’autre en fonction des commentaires des clients.

Le canal semi-annuel sera proposé aux clients faisant l’objet d’une licence en volume avec [Software Assurance](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx), ainsi que par le biais de la place de marché Microsoft Azure, d’autres fournisseurs de services d’hébergement ou cloud, ainsi que de programmes de fidélisation comme les abonnements VisualStudio.

> [!Note]  
> **La version actuelle du canal semi-annuel est Windows Server, version1803**. Si vous voulez intégrer des serveurs à ce canal, vous devez installer WindowsServer, version1803, qui peut être installé en mode d’installation minimale ou, comme NanoServer, exécuté dans un conteneur. Voir [Présentation de WindowsServer, version1803](get-started-with-1803.md) pour savoir comment obtenir et activer WindowsServer, version1803. Les mises à niveau sur place de WindowsServer2016 à WindowsServer version1803 ne sont pas prises en charge, car elles dépendent de **canaux de publication différents**. Windows Server, version1803 n’est pas une mise à jour vers WindowsServer2016; c’est la prochaine version de WindowsServer dans le canal semi-annuel.



Dans ce modèle, les versions de Windows Server sont identifiées par l’année et le mois de version: par exemple, en2017, une version du mois9 (septembre) serait identifiée comme **version 1709**. Les nouvelles versions de WindowsServer dans le canal semi-annuel paraîtront deux fois par an. La politique de support pour chaque version couvre une durée de 18mois.

## Devez-vous conserver vos serveurs dans le canal de maintenance à long terme ou les faire passer au canal semi-annuel?
Voici les principales différences à prendre en compte:

- Avez-vous besoin d’innover rapidement? Avez-vous besoin d’accéder avant tout le monde aux nouvelles fonctionnalités de WindowsServer? Avez-vous besoin de prendre en charge des applications hybrides à haute fréquence, des infrastructures Hyper\-V et DevOps? Si ou, vous devez envisager de **rejoindre le canal semi-annuel** en installant [WindowsServer version1803](get-started-with-1803.md). Comme décrit dans cette rubrique, vous recevrez les nouvelles versions deux fois par an, avec 18mois de support de production standard assurés par version. Vous en bénéficiez au travers de la licence en volume, d’Azure ou des services d’abonnement à Visual Studio. Actuellement, les versions du canal semi-annuel nécessitent la licence en volume et Software Assurance, si vous souhaitez utiliser le produit en production.
- Avez-vous besoin de stabilité et de prévisibilité? Avez-vous besoin d’exécuter des machines virtuelles et des charges de travail classiques sur des serveurs physiques? Si ou, vous devez envisager de **conserver ces serveurs sur le canal de maintenance à long terme**. La version actuelle du canal de maintenance à long terme est [WindowsServer2016](server-basics.md). Comme décrit dans cette rubrique, vous aurez accès aux nouvelles versions tous les 2 à 3ans, avec 5ans de support standard, suivis de 5ans de support étendu par version. Les versions du canal de maintenance à long terme sont disponibles au travers de tous les mécanismes de publication. Les versions du canal de maintenance à long terme sont disponibles à tout le monde, indépendamment du modèle de licence utilisé. 

Le tableau suivant résume les principales différences entre les canaux, à partir de Windows Server, version1803:

|  | Canal de maintenance à long terme (WindowsServer2016) |Canal semi-annuel (WindowsServer) |
| ------------------- | ------------------------------------ | ------------------------------------------------- |
|Scénarios recommandés | Serveurs de fichiers à usage général, charges de travail Microsoft et non-Microsoft, applications traditionnelles, rôles d’infrastructure, centre de données défini par logiciel et infrastructure hyperconvergée | Scénarios d'applications en conteneur, d'hôtes de conteneur et d'application bénéficiant d'une innovation plus rapide |
| Nouvelles versions | Tous les 2 à 3ans |Tous les 6mois |
| Support |5ans de support standard plus 5ans de support étendu | 18mois |
| Éditions | Toutes les éditions disponibles de Windows Server | Éditions Standard et Datacenter |
| Qui peut l'utiliser | Tous les clients par le biais de tous les canaux | Clients Software Assurance et cloud uniquement |
| Options d’installation | Server Core et Serveur avec Expérience utilisateur | Server Core pour hôte et image de conteneur et image du conteneur Nano Server |                |


## Compatibilité des appareils
Sauf indication contraire, la configuration matérielle minimale requise pour exécuter les versions du canal semi-annuel sera la même que pour la version de WindowsServer la plus récente publiée sur le canal de maintenance à long terme. Par exemple, **la version actuelle du canal de maintenance à long terme est WindowsServer2016**. La plupart des pilotes matériels continueront de fonctionner dans ces versions.

## Maintenance
Les versions du canal de maintenance à long terme et du canal semi-annuel seront prises en charge et bénéficieront de mises à jour de sécurité et autres. La différence est la durée de prise en charge de la version, comme indiqué ci-dessus.

### Outils de maintenance
Il existe de nombreux outils avec lesquels les professionnels de l’informatique peuvent effectuer la maintenance de WindowsServer. Chaque option possède ses avantages et ses inconvénients, allant des fonctionnalités et du contrôle aux faibles exigences en termes d’administration en passant par la simplicité. Voici quelques exemples des outils de maintenance disponibles pour gérer les mises à jour de maintenance:

- **Windows Update (autonome)**: cette option est disponible uniquement pour les serveurs connectés à Internet pour lesquels Windows Update est activé.
- **WindowsServer Update Services (WSUS)** fournit un contrôle étendu sur les mises à jour Windows10 et WindowsServer et est disponible en mode natif dans le système d’exploitation WindowsServer. Outre la possibilité de reporter les mises à jour, les organisations peuvent ajouter une couche d’approbation des mises à jour et choisir de les déployer sur des ordinateurs spécifiques ou des groupes d’ordinateurs dès qu’ils sont prêts.
- **System Center Configuration Manager** fournit un contrôle accru sur la maintenance. Les professionnels de l’informatique peuvent reporter les mises à jour, les approuver et disposer de plusieurs options destinées au ciblage des déploiements et à la gestion des heures de déploiement et d’utilisation de la bande passante.

Vous avez probablement déjà choisi d’utiliser au moins une de ces options en fonction de vos ressources, de votre personnel et de votre expertise. Vous pouvez continuer à utiliser le même processus pour les versions du canal semi-annuel: par exemple, si vous gérez déjà les mises à jour à l’aide de SystemCenter ConfigurationManager, vous pouvez continuer à l’utiliser. Vous pouvez, de même, continuer à utiliser WSUS.

## Obtenir des versions d’évaluation par le biais du programme WindowsInsider
Tester les premières versions de WindowsServer est utile à la fois pour Microsoft et ses clients, car cela permet de détecter les problèmes éventuels avant le lancement. Cela donne également aux clients une occasion unique d’influencer directement les fonctionnalités du produit. 

Microsoft dépend de la réception de commentaires tout au long du processus de développement afin de pouvoir apporter des modifications aussi rapidement que possible. Les tests anticipés et les commentaires sont essentiels pour un modèle de publication rapide.

Pour savoir comment s'impliquer dans le Programme Windows Insider, voir la [documentation relative au Programme Windows Insider pour Server](https://docs.microsoft.com/windows-insider/at-work/).
# Rubriques associées
[Canaux de maintenance de WindowsServer2019: canal de maintenance à long terme (LTSC) et canal semi-annuel (SAC)](https://docs.microsoft.com/windows-server/get-started-19/servicing-channels-19)

[Modifications apportées à NanoServer dans le canal semi-annuel de WindowsServer](nano-in-semi-annual-channel.md)

[Politique de support de WindowsServer](https://support.microsoft.com/en-us/lifecycle)

[Configuration requise pour Windows Server 2016](https://docs.microsoft.com/windows-server/get-started/system-requirements) 




