---
title: Canaux de maintenance
description: 'Explication des canaux de maintenance de Windows Server : LTSC et SAC'
ms.prod: windows-server
ms.technology: server-general
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: high
ms.date: 05/21/2019
ms.openlocfilehash: 3d443ff123cc041196f59d93d156415c34bdf70f
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75947872"
---
# <a name="windows-server-servicing-channels-ltsc-and-sac"></a>Canaux de maintenance de Windows Server : LTSC et SAC

>S'applique à : Windows Server 2019, Windows Server 2016, Windows Server (Canal semi-annuel)

Deux canaux principaux de publication sont proposés aux clients de Windows Server, le canal de maintenance à long terme (TLSC, Long-Term Servicing Channel) et le canal semi-annuel (SAC, Semi-Annual Channel).

Vous pouvez conserver les serveurs sur le canal de maintenance à long terme (LTSC), les déplacer vers le nouveau canal semi-annuel, ou avoir des serveurs sur les deux, selon la configuration la mieux adaptée à vos besoins.

## <a name="long-term-servicing-channel-ltsc"></a>Canal de maintenance à long terme (LTSC)

Il s'agit du modèle de publication que vous connaissez déjà (anciennement « Long-Term Servicing *Branch* ») selon lequel une nouvelle version majeure de Windows Server est publiée tous les 2 à 3 ans. Les utilisateurs ont droit à 5 ans de support standard et 5 ans de support étendu. Ce canal convient aux systèmes qui nécessitent une option de maintenance plus longue et une stabilité fonctionnelle. Les déploiements de Windows Server 2016 et des versions antérieures de Windows Server ne seront pas affectés par les nouvelles versions du canal semi-annuel. Le canal de maintenance à long terme continuera de recevoir des mises à jour de sécurité et autres, mais ne recevra pas les nouvelles fonctionnalités.

> [!Note]  
> **Le produit de canal de maintenance à long terme actuel est Windows Server 2019**. Si vous voulez rester dans ce canal, vous devez installer (ou continuer à utiliser) Windows Server 2019, qui peut être installé avec l’option d’installation Server Core ou Expérience utilisateur.

## <a name="semi-annual-channel"></a>Canal semi-annuel

Le canal semi-annuel est parfait pour les clients qui innovent rapidement pour tirer parti des nouvelles fonctionnalités du système d’exploitation à un rythme plus soutenu, tant dans les applications, en particulier celles intégrés aux conteneurs et microservices, que dans le datacenter hybride à définition logicielle. Le canal semi-annuel publie de nouvelles versions des produits Windows Server deux fois par an, au printemps et en automne. Chaque version de ce canal est prise en charge pendant 18 mois à compter de la version initiale.

La plupart des fonctionnalités introduites dans le canal semi-annuel sont cumulées dans la prochaine version du canal de maintenance à long terme de Windows Server. Les éditions, les fonctionnalités et le contenu de support peuvent varier d’une version à l’autre en fonction des commentaires des clients.

Le canal semi-annuel est proposé aux clients détenteurs d’une licence en volume avec [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default.aspx), ainsi que dans la Place de marché Azure, via d’autres fournisseurs de services d’hébergement ou cloud et dans les programmes de fidélité comme les abonnements Visual Studio.

> [!Note]  
> **La version actuelle du canal semi-annuel est Windows Server, version 1903**. Si vous voulez intégrer des serveurs à ce canal, vous devez installer Windows Server, version 1903, qui peut être installé en mode Server Core ou, comme Nano Server, exécuté dans un conteneur. Les mises à niveau sur place à partir d’une version de canal de maintenance à long terme ne sont pas prises en charge parce qu’elles se trouvent dans des **canaux de distribution différents**. Les versions du canal semi-annuel ne sont pas des mises à jour. Elles sont les prochaines versions de Windows Server dans le canal semi-annuel.

Dans ce modèle, les versions de Windows Server sont identifiées par l’année et le mois de version : par exemple, en 2017, une version du mois 9 (septembre) serait identifiée comme **version 1709**. La publication des nouvelles versions de Windows Server dans le canal semi-annuel est semestrielle. Le cycle de vie du support de chaque version dure 18 mois.

## <a name="should-you-keep-servers-on-the-ltsc-or-move-them-to-the-semi-annual-channel"></a>Devez-vous conserver vos serveurs dans le canal de maintenance à long terme ou les faire passer au canal semi-annuel ?

Voici les principales différences à prendre en compte :

- Avez-vous besoin d’innover rapidement ? Avez-vous besoin d’accéder avant tout le monde aux nouvelles fonctionnalités de Windows Server ? Avez-vous besoin de prendre en charge des applications hybrides à cadence soutenue, des infrastructures Hyper\-V et DevOps ? Si oui, vous devriez **rejoindre le canal semi-annuel** en installant **Windows Server version 1903**. Comme décrit dans cette rubrique, vous recevrez les nouvelles versions deux fois par an, avec 18 mois de support de production standard par version. Vous en bénéficiez par le biais d’une licence en volume, d’Azure ou des services d’abonnement à Visual Studio. Actuellement, les versions du canal semi-annuel nécessitent une licence en volume et Software Assurance, si vous souhaitez utiliser le produit en production.
- Avez-vous besoin de stabilité et de prévisibilité ? Avez-vous besoin d’exécuter des machines virtuelles et des charges de travail classiques sur des serveurs physiques ? Si oui, vous devriez **conserver ces serveurs sur le canal de maintenance à long terme**. La version actuelle du canal de maintenance à long terme est **Windows Server 2019**. Comme décrit dans cette rubrique, vous aurez accès aux nouvelles versions tous les 2 à 3 ans, avec 5 ans de support standard, suivis de 5 ans de support étendu par version. Les versions du canal de maintenance à long terme sont disponibles au travers de tous les mécanismes de publication. Les versions du canal de maintenance à long terme sont à la disposition de tout le monde, indépendamment du modèle de licence utilisé. 

Le tableau suivant résume les principales différences entre les canaux :


|                       |                                                              Canal de maintenance à long terme (Windows Server 2019)                                                               |                                   Canal semi-annuel (Windows Server)                                   |
|-----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| Scénarios recommandés | Serveurs de fichiers à usage général, charges de travail Microsoft et non-Microsoft, applications traditionnelles, rôles d’infrastructure, centre de données défini par logiciel et infrastructure hyperconvergée | Scénarios d’applications conteneurisées, d’hôtes de conteneur et d’application bénéficiant d’une innovation plus rapide |
|     Nouvelles versions      |                                                                               Tous les 2 à 3 ans                                                                                |                                              Tous les 6 mois                                              |
|        Assistance        |                                                       5 ans de support standard plus 5 ans de support étendu                                                        |                                                18 mois                                                 |
|       Éditions        |                                                                    Toutes les éditions disponibles de Windows Server                                                                     |                                     Éditions Standard et Datacenter                                     |
|      Qui peut l’utiliser      |                                                                      Tous les clients par le biais de tous les canaux                                                                      |                               Clients Software Assurance et cloud uniquement                                |
| Options d'installation  |                                                                Server Core et Serveur avec Expérience utilisateur                                                                |                 Server Core pour hôte et image de conteneur et image du conteneur Nano Server                 |

## <a name="device-compatibility"></a>Compatibilité des appareils

Sauf indication contraire, la configuration matérielle minimale requise pour exécuter les versions du canal semi-annuel sera la même que pour la version de Windows Server la plus récente publiée sur le canal de maintenance à long terme. Par exemple, **la version actuelle du canal de maintenance à long terme est Windows Server 2019**. La plupart des pilotes matériels continueront de fonctionner dans ces versions.

## <a name="servicing"></a>Maintenance

Les versions du canal de maintenance à long terme et du canal semi-annuel seront prises en charge et bénéficieront de mises à jour de sécurité et autres. La différence est la durée de prise en charge de la version, comme indiqué ci-dessus.

### <a name="servicing-tools"></a>Outils de maintenance

Il existe de nombreux outils avec lesquels les professionnels de l’informatique peuvent effectuer la maintenance de Windows Server. Chaque option a ses avantages et ses inconvénients, allant des fonctionnalités et du contrôle aux faibles exigences en termes d’administration en passant par la simplicité. Voici quelques exemples des outils de maintenance disponibles pour gérer les mises à jour de maintenance :

- **Windows Update (autonome)**  : Cette option est disponible uniquement pour les serveurs connectés à Internet pour lesquels Windows Update est activé.
- **Windows Server Update Services (WSUS)** fournit un contrôle étendu sur les mises à jour Windows 10 et Windows Server et est disponible en mode natif dans le système d’exploitation Windows Server. Outre la possibilité de reporter les mises à jour, les organisations peuvent ajouter une couche d’approbation des mises à jour et choisir de les déployer sur des ordinateurs spécifiques ou des groupes d’ordinateurs dès qu’ils sont prêts.
- **System Center Configuration Manager** fournit un contrôle accru sur la maintenance. Les professionnels de l’informatique peuvent reporter les mises à jour, les approuver et disposer de plusieurs options destinées au ciblage des déploiements et à la gestion des heures de déploiement et d’utilisation de la bande passante.

Vous avez probablement déjà choisi d’utiliser au moins une de ces options en fonction de vos ressources, de votre personnel et de votre expertise. Vous pouvez continuer à utiliser le même processus pour les versions du canal semi-annuel : par exemple, si vous gérez déjà les mises à jour à l’aide de System Center Configuration Manager, vous pouvez continuer à l’utiliser. De même, vous pouvez continuer à utiliser WSUS.

## <a name="where-to-obtain-semi-annual-channel-releases"></a>Où obtenir les versions du canal semi-annuel

Les versions du canal semi-annuel doivent faire l’objet d’une nouvelle installation.

- Centre de gestion des licences en volume (VLSC) : les clients ayant une licence en volume avec [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default.aspx) peuvent obtenir cette version en accédant au [Centre de gestion des licences en volume](https://www.microsoft.com/Licensing/servicecenter/default.aspx) et en cliquant sur **Se connecter**. Ensuite, cliquez sur **Téléchargements et clés** et recherchez cette version. 

- Les versions du canal semi-annuel sont également disponibles dans [Microsoft Azure](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.WindowsServer?tab=Overview).

- Abonnements Visual Studio : Les abonnés Visual Studio peuvent obtenir des versions du canal semi-annuel en les téléchargeant à partir de la [page de téléchargement des abonnés Visual Studio](https://my.visualstudio.com/downloads?pid=2347). Si vous n’êtes pas encore abonné, accédez à la page [Abonnements Visual Studio](https://www.visualstudio.com/subscriptions/) pour vous inscrire, puis visitez la [page de téléchargement des abonnés Visual Studio](https://my.visualstudio.com/downloads?pid=2347) comme indiqué ci-dessus. Les versions obtenues via les abonnements Visual Studio sont destinées au développement et aux tests uniquement.

- Obtenir des préversions par le biais du programme Windows Insider : Tester les premières versions de Windows Server est utile à la fois pour Microsoft et ses clients, car cela permet de détecter les problèmes éventuels avant la publication. Cela donne également aux clients une occasion unique d’avoir une influence directe sur les fonctionnalités du produit.   
Microsoft dépend de la réception de commentaires tout au long du processus de développement afin de pouvoir apporter des modifications aussi rapidement que possible. Les tests anticipés et les commentaires sont essentiels pour un modèle de publication rapide. Pour participer au Programme Windows Insider, consultez la [documentation relative au Programme Windows Insider pour Server](https://docs.microsoft.com/windows-insider/at-work/).

## <a name="activating-semi-annual-channel-releases"></a>Activer les versions du canal semi-annuel

- Si vous utilisez Microsoft Azure, cette version devrait être activée automatiquement.
- Si vous avez obtenu cette version à partir du Centre de gestion des licences en volume ou via les abonnements Visual Studio, vous pouvez l’activer à l’aide de votre CSVLK Windows Server 2019 avec votre environnement de système de gestion des clés (KMS). Pour plus d’informations, consultez [Clés d’installation de client KMS](../get-started/kmsclientkeys.md).

Les versions du canal semi-annuel qui ont été publiées avant Windows Server 2019 utilisent le CSVLK de Windows Server 2016.

## <a name="why-do-semi-annual-channel-releases-offer-only-the-server-core-installation-option"></a>Pourquoi les versions de canal semi-annuel offrent uniquement l’option d’installation Server Core ?

Lors de la planification de chaque version de Windows Server, l’une des étapes les plus importantes est pour nous de prendre en considération les réponses des clients à nos questions telles que « comment utilisez-vous Windows Server ? ». Ou encore, « quelles nouvelles fonctionnalités auront le plus d’impact sur vos déploiements Windows Server et par extension, sur vos activités quotidiennes ? ». Vos commentaires nous indiquent que proposer des innovations le plus rapidement et efficacement possible est une priorité. Parallèlement, les clients qui innovent plus rapidement nous ont indiqué qu’ils utilisent principalement des scripts de ligne de commande avec PowerShell pour gérer leurs centres de données. Par conséquent, ils n’ont pas réellement besoin de l’interface graphique utilisateur du bureau disponible dans l’installation de Windows Server avec Expérience de bureau, particulièrement depuis que [Windows Admin Center](../manage/windows-admin-center/overview.md) est disponible pour gérer à distance vos serveurs.

En nous concentrant sur l’option d’installation Server Core, nous pouvons consacrer plus de ressources à ces innovations, tout en conservant les fonctionnalités traditionnelles et la compatibilité des applications de la plateforme Windows Server. Si vous avez des commentaires à ce sujet ou sur d’autres problèmes concernant Windows Server et nos versions ultérieures, vous pouvez nous faire part de vos suggestions et commentaires par le biais du [Hub de commentaires](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app).

## <a name="what-about-nano-server"></a>Qu’en est-il de Nano Server ?

Nano Server est disponible en tant que système d’exploitation de conteneur dans le canal semi-annuel. Pour plus de détails, consultez [Modifications apportées à Nano Server dans le canal semi-annuel de Windows Server](../get-started/nano-in-semi-annual-channel.md).

## <a name="how-to-tell-whether-a-server-is-running-an-ltsc-or-sac-release"></a>Comment savoir si un serveur exécute une version LTSC ou SAC

En règle générale, les versions du canal de maintenance à long terme telles que Windows Server 2019 sont publiées en même temps en tant que nouvelle version du canal semi-annuel, par exemple, Windows Server, version 1809. Du coup, il est un peu difficile de déterminer si un serveur exécute une version de canal semi-annuel. Au lieu de regarder le numéro de build, vous devez regarder le nom du produit : Les versions du canal semi-annuel utilisent le nom de produit « Windows Server Standard » ou « Windows Server Datacenter » sans numéro de version, tandis que les versions du canal de maintenance à long terme incluent le numéro de version, par exemple, « Windows Server 2019 Datacenter ».

>[!Note]  
> Les instructions ci-dessous sont destinées à aider à identifier et à différencier LTSC et SAC à des fins de cycle de vie et d’inventaire général uniquement.  Elles ne sont pas destinées à la compatibilité des applications ni à représenter une surface d'API spécifique.  Les développeurs d’applications doivent s’appuyer sur d’autres instructions pour assurer la compatibilité appropriée étant donné que les composants, les API et les fonctionnalités peuvent être éventuellement ajoutés pendant la durée de vie d’un système. [Version du système d’exploitation](https://docs.microsoft.com/windows/desktop/SysInfo/operating-system-version) est un meilleur point de départ pour les développeurs d’applications.

Ouvrez Powershell et utilisez l’applet de commande Get-ItemProperty, ou l’applet de commande Get-ComputerInfo, pour vérifier ces propriétés dans le Registre.  En plus du numéro de build, LTSC ou SAC sont indiqués par la présence ou l’absence de l’année, par exemple 2019.  LTSC indique l’année, pas SAC.  Cela définit également la période de publication de la version avec les informations ReleaseId ou WindowsVersion, à savoir 1809, et précise si l'installation est Server Core ou de type Serveur avec expérience utilisateur. 

**Exemple Windows Server 2019 Datacenter Edition (LTSC) avec Expérience utilisateur :**

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

**Exemple Windows Server, version 1809 (SAC) Standard Edition avec Server Core :**

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

**Exemple Windows Server 2019 Standard Edition (LTSC) avec Server Core :**


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

Pour savoir si la nouvelle [fonctionnalité à la demande de compatibilité Application Server Core](https://docs.microsoft.com/windows-server/get-started-19/install-fod-19) est présente sur un serveur, utilisez l’applet de commande [Get-WindowsCapability](https://docs.microsoft.com/powershell/module/dism/get-windowscapability?view=win10-ps) et recherchez :
````
Name    :     ServerCore.AppCompatibility~~~~0.0.1.0
State   :     Installed
````

## <a name="see-also"></a>Voir aussi

[Modifications apportées à Nano Server dans le canal semi-annuel de Windows Server](../get-started/nano-in-semi-annual-channel.md)

[Politique de support de Windows Server](https://support.microsoft.com/lifecycle)

[Déterminer si l’option Server Core s’exécute](https://msdn.microsoft.com/library/hh846315%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396)

[GetProductInfo, fonction](https://docs.microsoft.com/windows/desktop/api/sysinfoapi/nf-sysinfoapi-getproductinfo)

[Applets de commande de journalisation d'inventaire logiciel](https://docs.microsoft.com/powershell/module/softwareinventorylogging/?view=winserver2012R2-ps)
