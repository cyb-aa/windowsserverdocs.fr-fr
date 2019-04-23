---
title: Présentation de Windows Server, version 1709
description: Comment obtenir, installer et activer
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: jaimeo
ms.localizationpriority: high
ms.date: 12/5/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 9cf87597-b15d-4f43-8aa1-91e60367f011
ms.openlocfilehash: 96f098457b9bac4541421c7889aefb030e3d8804
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868800"
---
# <a name="introducing-windows-server-version-1709"></a>Présentation de Windows Server, version 1709

>S'applique à : Windows Server (canal semi-annuel)

**Windows Server, version 1709 est la première version dans le nouveau canal semi-annuel.** 

## <a name="what-the-semi-annual-channel-is--and-isnt"></a>Qu’est-ce que le canal semi-annuel, et qu’est-ce qu’il n’est pas
Première version publiée dans ce nouveau canal, Windows Server version 1709 *n’est pas* une mise à jour ou un « service pack » pour Windows Server 2016. C’est la première des publications semi-annuelles parues sur un **nouveau canal de publication**. Celui-ci est conçu pour les clients qui adoptent une « cadence de cloud », comme ceux dont les cycles de développement sont rapides ou les hébergeurs qui suivent le rythme des derniers investissements Hyper\-V. Chaque version de ce canal sera prise en charge pendant 18 mois à compter de la version initiale. Pour plus d’informations sur le canal semi-annuel, ainsi que des **conseils pour choisir son canal** (ou y rester), voir [Présentation du canal semi-annuel](semi-annual-channel-overview.md).


**Le produit actuel du canal de maintenance à long terme est Windows Server 2016**. Le canal de maintenance à long terme (LTSC) est recommandé si vous avez besoin de stabilité et de prévisibilité à long terme dans votre système d’exploitation de serveur pour prendre en charge des applications et des charges de travail classiques. Si vous voulez rester dans le cana de maintenance à long terme, vous devez installer (ou continuer à utiliser) Windows Server 2016, qui peut être installé en mode d’installation minimale ou avec l’Expérience de bureau. Voir [Mise en route de Windows Server 2016](https://docs.microsoft.com/windows-server/get-started/server-basics) pour plus d’informations.


## <a name="whats-different-about-1709"></a>Qu’est-ce qui change dans la version 1709 ?

Windows Server, version 1709 s’exécute en mode d’installation minimale. Cela signifie qu’il n’y a pas de console locale ni d’interface utilisateur graphique et que sa gestion se fait à distance. Toutefois, il offre des avantages importants tels qu’une configuration matérielle requise moindre, une surface d’attaque considérablement réduite et des mises à jour moins fréquentes. Si vous débutez dans l’utilisation du mode d’installation minimale, la rubrique [Gérer un serveur en mode d’installation minimale](../administration/server-core/server-core-manage.md) vous aidera à vous familiariser avec cet environnement. [Gérer Windows Server 2016](../administration/manage-windows-server.md) vous décrit les différentes options disponibles pour la gestion à distance des serveurs.

[Nouveautés de Windows Server version 1709](whats-new-in-windows-server-1709.md) vous présente les nouvelles fonctionnalités ajoutées dans Windows Server, version 1709.

### <a name="why-does-windows-server-version-1709-offer-only-the-server-core-installation-option"></a>Pourquoi seule l’option d’installation minimale est proposée pour Windows Server, version 1709 ?
Lors de la planification de chaque version de Windows Server, l’une des étapes les plus importantes est pour nous de prendre en considération les réponses des clients à nos questions telles que « comment utilisez-vous Windows Server ? ». Ou encore, « quelles nouvelles fonctionnalités auront le plus d’impact sur vos déploiements Windows Server et par extension, sur vos activités quotidiennes ? ». Vos commentaires nous indiquent que proposer des innovations le plus rapidement et de manière aussi efficace que possible est une priorité. Parallèlement, les clients qui innovent plus rapidement nous ont indiqué qu’ils utilisent principalement des scripts de ligne de commande avec PowerShell pour gérer leurs centres de données. Par conséquent, ils n’ont pas réellement besoin de l’interface utilisateur graphique de bureau disponible dans l’installation de Windows Server avec Expérience de bureau. En nous concentrant sur l’option d’installation minimale, nous pouvons consacrer plus de ressources à ces innovations, tout en conservant les fonctionnalités traditionnelles et la compatibilité des applications de la plateforme Windows Server. Si vous avez des commentaires à ce sujet ou sur d’autres problèmes concernant Windows Server et nos versions ultérieures, vous pouvez nous faire part de vos suggestions et commentaires par le biais du [le Hub de commentaires](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app).


### <a name="what-about-nano-server"></a>Qu’en est-il de Nano Server ?
Nano Server est disponible en tant que système d’exploitation de conteneur. Voir [Modifications apportées à Nano Server dans le canal semi-annuel de Windows Server](nano-in-semi-annual-channel.md) pour plus d’informations.

## <a name="additional-information-about-this-release"></a>Informations supplémentaires sur cette version
Pour obtenir une présentation complète des éléments clés sur Windows Server, version 1709, vous devez également consulter les rubriques suivantes avant l’installation :

- Quel est le matériel nécessaire pour l’exécuter ? Voir [Configuration requise](system-requirements.md) ; la configuration système requise pour cette version est la même que pour Windows Server 2016.
- Quelles nouvelles caractéristiques et fonctionnalités ont été ajoutées ? Voir [Nouveautés de Windows Server version 1709](whats-new-in-windows-server-1709.md).
- Qu’est-ce qui a été supprimé ? Voir [Fonctionnalités supprimées ou dont la suppression est prévue à compter de Windows Server (version 1709)](Removed-Features-1709.md)
- Quels sont les problèmes propres à cette version qui doivent être corrigés ? Voir [Notes de publication : points importants sur Windows Server, version 1709](server-1709-relnotes.md)


## <a name="where-to-obtain-windows-server-version-1709"></a>Où obtenir Windows Server, version 1709

Cette version doit faire l’objet d’une nouvelle installation.

- VLSC : Clients de licence en volume avec [Software Assurance](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx) peut obtenir cette version en accédant à la [Volume Licensing Service Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx) et en cliquant sur **Sign In**. Ensuite, cliquez sur **Téléchargements et clés** et recherchez cette version. 

- Windows Server, version 1709 est également disponible dans [Microsoft Azure](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.WindowsServer?tab=Overview).

- Les participants dans **abonnements Visual Studio :** Si vous participez déjà dans les abonnements Visual Studio, vous pouvez obtenir Windows Server, version 1709 en accédant à la [page de téléchargement de Visual Studio abonné](https://my.visualstudio.com/downloads?pid=2347) et de la fin du téléchargement disponible Il. Si vous n’êtes pas encore abonné, accédez à la page [Abonnements Visual Studio](https://www.visualstudio.com/subscriptions/) pour vous inscrire, puis visitez la [page de téléchargement des abonnés Visual Studio](https://my.visualstudio.com/downloads?pid=2347) comme indiqué ci-dessus. Les versions obtenues au travers des abonnements Visual Studio sont destinées au développement et aux tests uniquement.




## <a name="activating-windows-server-version-1709"></a>Activation de Windows Server, version 1709

- Si vous avez obtenu cette version à partir du Centre de gestion de licences en volume, vous pouvez l’activer à l’aide de votre clé d’activation Windows Server 2016.
- Si vous utilisez Microsoft Azure, cette version devrait être activée automatiquement.
- Si vous obtenez cette version à partir de la page Abonnements Visual Studio, elle devrait également s’activer automatiquement.