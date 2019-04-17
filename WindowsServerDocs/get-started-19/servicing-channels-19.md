---
title: Canaux de maintenance
description: 'Explication des canaux de maintenance de Windows Server: LTSC et SAC'
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
ms.localizationpriority: high
ms.openlocfilehash: c4329339e37acbb0b589e767a570073f4ae1363f
ms.sourcegitcommit: e44bacf5bd4dbdd500dcfae803724b7888096582
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/07/2019
ms.locfileid: "9149765"
---
# Canaux de maintenance de Windows Server: SAC et LTSC

>S’applique à: Windows Server 2019, Windows Server 2016


**Deux canaux principaux de publication sont proposés aux clients de WindowsServer, le canal de maintenance à long terme et le canal semi-annuel.** 

Vous pouvez conserver les serveurs sur le canal de maintenance à long terme (LTSC), les déplacer vers le nouveau canal semi-annuel, ou avoir des serveurs sur les deux, selon la configuration la mieux adaptée à vos besoins.


## Canal de maintenance à long terme (LTSC)
C’est le modèle de publication que vous connaissez déjà (auparavant appelé «*Branche* LTSB (Long Term Servicing Branch)») dans lequel une nouvelle version majeure de WindowsServer est publiée tous les 2 à 3ans. Les utilisateurs ont droit à 5ans de support standard et 5ans de support étendu. Ce canal convient aux systèmes qui nécessitent une option de maintenance et une stabilité fonctionnelle prolongées. Les déploiements de WindowsServer2016 et des versions antérieures de WindowsServer ne seront pas affectés par les nouvelles versions du canal semi-annuel. Le canal de maintenance à long terme continuera de recevoir des mises à jour de sécurité et autres, mais ne recevra pas les nouvelles fonctionnalités.

> [!Note]  
> **Le produit de canal de maintenance à long terme actuel est WindowsServer2019**. Si vous voulez rester dans ce canal, vous devez installer (ou continuer à utiliser) WindowsServer2019, qui peut être installé selon l'option d'installation minimale ou selon l'option Expérience utilisateur.

## Canal semi-annuel 
Le canal semi-annuel est parfait pour les clients qui innovent à une cadence élevée pour tirer rapidement parti des nouvelles fonctionnalités du système d’exploitation, tant dans les applications (en particulier dans celles construites en conteneurs et en microservices), que dans les datacenters hybrides à définition logicielle. Le canal semi-annuel publiera de nouvelles versions des produits WindowsServer deux fois par an, au printemps et en automne. Chaque version de ce canal sera prise en charge pendant 18mois à compter de la version initiale.

La plupart des fonctionnalités introduites dans le canal semi-annuel seront cumulées dans la prochaine version du canal de maintenance à long terme de WindowsServer. Les éditions, les fonctionnalités et le contenu du support peuvent varier d’une version à l’autre en fonction des commentaires des clients.

Le canal semi-annuel sera proposé aux clients faisant l’objet d’une licence en volume avec [Software Assurance](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx), ainsi que par le biais de la place de marché Microsoft Azure, d’autres fournisseurs de services d’hébergement ou cloud, ainsi que de programmes de fidélisation comme les abonnements VisualStudio.

> [!Note]  
> **La version actuelle du canal semi-annuel est Windows Server, version1809**. Si vous voulez intégrer des serveurs à ce canal, vous devez installer WindowsServer, version1809, qui peut être installé en mode d'installation minimale ou comme NanoServer exécuté dans un conteneur. Les mises à niveau sur place de WindowsServer2016 à WindowsServer version1809 ne sont pas prises en charge, car elles dépendent de **canaux de publication différents**. Windows Server, version1809 n’est pas une mise à jour vers WindowsServer2016; c’est la prochaine version de WindowsServer dans le canal semi-annuel.



Dans ce modèle, les versions de Windows Server sont identifiées par l’année et le mois de version: par exemple, en2017, une version du mois9 (septembre) serait identifiée comme **version 1709**. Les nouvelles versions de WindowsServer dans le canal semi-annuel paraîtront deux fois par an. La politique de support pour chaque version couvre une durée de 18mois.

## Devez-vous conserver vos serveurs dans le canal de maintenance à long terme ou les faire passer au canal semi-annuel?
Voici les principales différences à prendre en compte:

- Avez-vous besoin d’innover rapidement? Avez-vous besoin d’accéder avant tout le monde aux nouvelles fonctionnalités de WindowsServer? Avez-vous besoin de prendre en charge des applications hybrides à haute fréquence, des infrastructures Hyper\-V et DevOps? Si ou, vous devez envisager de **rejoindre le canal semi-annuel** en installant **WindowsServer version1809**. Comme décrit dans cette rubrique, vous recevrez les nouvelles versions deux fois par an, avec 18mois de support de production standard assurés par version. Vous en bénéficiez au travers de la licence en volume, d’Azure ou des services d’abonnement à Visual Studio. Actuellement, les versions du canal semi-annuel nécessitent la licence en volume et Software Assurance, si vous souhaitez utiliser le produit en production.
- Avez-vous besoin de stabilité et de prévisibilité? Avez-vous besoin d’exécuter des machines virtuelles et des charges de travail classiques sur des serveurs physiques? Si oui, vous devez envisager de **conserver ces serveurs sur le canal de maintenance à long terme**. La version actuelle du canal de maintenance à long terme est **WindowsServer2019**. Comme décrit dans cette rubrique, vous aurez accès aux nouvelles versions tous les 2 à 3ans, avec 5ans de support standard, suivis de 5ans de support étendu par version. Les versions du canal de maintenance à long terme sont disponibles au travers de tous les mécanismes de publication. Les versions du canal de maintenance à long terme sont disponibles à tout le monde, indépendamment du modèle de licence utilisé. 

Le tableau suivant résume les principales différences entre les canaux:

|  | Canal de maintenance à long terme (WindowsServer2019) |Canal semi-annuel (WindowsServer) |
| ------------------- | ------------------------------------ | ------------------------------------------------- |
|Scénarios recommandés | Serveurs de fichiers à usage général, charges de travail Microsoft et non-Microsoft, applications traditionnelles, rôles d’infrastructure, centre de données défini par logiciel et infrastructure hyperconvergée | Scénarios d'applications en conteneur, d'hôtes de conteneur et d'application bénéficiant d'une innovation plus rapide |
| Nouvelles versions | Tous les 2 à 3ans |Tous les 6mois |
| Support |5ans de support standard plus 5ans de support étendu | 18mois |
| Éditions | Toutes les éditions disponibles de Windows Server | Éditions Standard et Datacenter |
| Qui peut l'utiliser | Tous les clients par le biais de tous les canaux | Clients Software Assurance et cloud uniquement |
| Options d’installation | Server Core et Serveur avec Expérience utilisateur | Server Core pour hôte et image de conteneur et image du conteneur Nano Server |                |


## Compatibilité des appareils
Sauf indication contraire, la configuration matérielle minimale requise pour exécuter les versions du canal semi-annuel sera la même que pour la version de WindowsServer la plus récente publiée sur le canal de maintenance à long terme. Par exemple, **la version actuelle du canal de maintenance à long terme est WindowsServer2019**. La plupart des pilotes matériels continueront de fonctionner dans ces versions.

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

## Pour identifier Windows Server 2019 et Windows Server, version 1809

>[!Note]  
> Les instructions ci-dessous sont destinées à vous aider à identifier et à faire la distinction entre les canaux de maintenance LTSC et SAC pour le cycle de vie et uniquement à des fins d'inventaire général.  Elles ne sont pas destinées à la compatibilité des applications ou pour représenter une surface d'API spécifique.  Les développeurs d'applications doivent s'appuyer sur d'autres instructions pour assurer la compatibilité appropriée étant donné que les composants, les API et les fonctionnalités peuvent être éventuellement ajoutés pendant la durée de vie d'un système. [Version du système d'exploitation](https://docs.microsoft.com/windows/desktop/SysInfo/operating-system-version) est un meilleur point de départ pour les développeurs d'applications.

Ouvrez Powershell et utilisez l'applet de commande Get-ItemProperty, ou l'applet de commande Get-ComputerInfo, pour vérifier ces propriétés dans le Registre.  Le numéro de build est indiqué, ainsi que les informations LTSC ou SAC indiquées par la mention ou l'absence de mention de l'année, à savoir 2019.  Les informations LTSC sont indiquées par la mention de l'année 2019. Si cette dernière n'est pas indiquée, il s'agit alors des informations SAC.  Cela définit également la période de lancement de la version avec les informations ReleaseId ou WindowsVersion, à savoir 1809, et précise si l'installation est minimale de type Serveur ou de type Serveur avec expérience utilisateur. 

**Windows Server 2019 Datacenter Edition (LTSC) avec un exemple de l'expérience utilisateur:**

````PowerShell
Get-ItemProperty -Path "HKLM:\Software\Microsoft\Windows NT\CurrentVersion" | Select ProductName, ReleaseId, InstallationType, CurrentMajorVersionNumber,CurrentMinorVersionNumber,CurrentBuild
````

````
ProductName               : Windows Server 2019 Datacenter
ReleaseId                 : 1809
InstallationType          : Server
CurrentMajorVersionNumber : 10
CurrentMinorVersionNumber : 0
CurrentBuild              : 17763
````

**Windows Server, version 1809 (SAC), exemple Standard Edition Server Core:**

````PowerShell
Get-ItemProperty -Path "HKLM:\Software\Microsoft\Windows NT\CurrentVersion" | Select ProductName, ReleaseId, InstallationType, CurrentMajorVersionNumber,CurrentMinorVersionNumber,CurrentBuild
````

````
ProductName               : Windows Server Standard
ReleaseId                 : 1809
InstallationType          : Server Core
CurrentMajorVersionNumber : 10
CurrentMinorVersionNumber : 0
CurrentBuild              : 17763
````

**Exemple de Windows Server 2019 Standard Edition (LTSC) Server Core:**


````PowerShell
Get-ComputerInfo | Select WindowsProductName, WindowsVersion, WindowsInstallationType, OsServerLevel, OsVersion, OsHardwareAbstractionLayer
````

````
WindowsProductName            : Windows Server 2019 Standard
WindowsVersion                : 1809
WindowsInstallationType       : Server Core
OsServerLevel                 : ServerCore
OsVersion                     : 10.0.17763
OsHardwareAbstractionLayer    : 10.0.17763.107
````

Pour exécuter une requête afin de savoir si la nouvelle [fonctionnalité à la demande de compatibilité Application Server Core](https://docs.microsoft.com/windows-server/get-started-19/install-fod-19) est présente sur un serveur, utilisez l'applet de commande [Get-WindowsCapability](https://docs.microsoft.com/powershell/module/dism/get-windowscapability?view=win10-ps) et recherchez:
````
Name    :     ServerCore.AppCompatibility~~~~0.0.1.0
State   :     Installed
````

# Rubriques associées
[Modifications apportées à NanoServer dans le canal semi-annuel de WindowsServer](../get-started/nano-in-semi-annual-channel.md)

[Politique de support de WindowsServer](https://support.microsoft.com/lifecycle)

[Déterminer si Server Core est en cours d'exécution](https://msdn.microsoft.com/library/hh846315%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396)

[Fonction GetProductInfo](https://docs.microsoft.com/windows/desktop/api/sysinfoapi/nf-sysinfoapi-getproductinfo)

[Applets de commande de journalisation d'inventaire logiciel](https://docs.microsoft.com/powershell/module/softwareinventorylogging/?view=winserver2012R2-ps)
