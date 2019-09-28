---
title: Recommandations relatives à l’optimisation des performances pour Windows Server 2016
description: Recommandations relatives à l’optimisation des performances pour Windows Server 2016
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: phstee
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: f684a87b091ffd95bb65c0b5f3aa0dfc9f405825
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355036"
---
# <a name="performance-tuning-guidelines-for-windows-server-2016"></a>Recommandations relatives à l’optimisation des performances pour Windows Server 2016

Lorsque vous exécutez un système de serveur dans votre organisation, les paramètres de serveur par défaut ne sont peut-être pas adaptés aux besoins de votre entreprise. Par exemple, vous pourriez avoir besoin de la consommation d'énergie la plus basse possible, ou du temps de latence le plus bas possible, ou du débit maximum possible sur votre serveur. Ce guide fournit un ensemble d’instructions utiles pour régler les paramètres de serveur dans Windows Server 2016 et obtenir des gains de performance ou d’efficacité énergétique, en particulier lorsque la nature de la charge de travail varie peu au fil du temps.

Il est important que vos changements de paramètres tiennent compte du matériel, de la charge de travail, des budgets énergétiques et des objectifs de performance de votre serveur. Ce guide décrit chaque paramètre et ses effets potentiels pour vous aider à prendre une décision éclairée quant à sa pertinence par rapport à votre système, à votre charge de travail, à vos performances et à vos objectifs d'utilisation d'énergie.

> [!warning]
> Les paramètres du Registre et les paramètres d’optimisation ont considérablement changé entre les différentes versions de Windows Server. Veillez à utiliser les recommandations d’optimisation les plus récentes pour éviter des résultats inattendus.

## <a name="in-this-guide"></a>Dans ce guide
Ce guide fournit des recommandations relatives aux performances et à l’optimisation de Windows Server 2016 qui sont réparties dans trois catégories :

|Matériel de serveur | Rôle serveur | Sous-système de serveur |
|:---:|:---:|:---:|
|[Considérations relatives aux performances du matériel](hardware/index.md) |[Serveurs Active Directory](role/active-directory-server/index.md) |[Gestion de la mémoire et du cache](subsystem/cache-memory-management/index.md)|
|[Considérations relatives à la puissance du matériel](hardware/power.md)|[Serveurs de fichiers](role/file-server/index.md)|[Sous-système de mise en réseau](../../networking/technologies/network-subsystem/net-sub-performance-top.md)|
||[Serveurs Hyper-V Server](role/hyper-v-server/index.md)|[Espaces de stockage direct](subsystem/storage-spaces-direct/index.md)|
||[Services Bureau à distance](role/remote-desktop/session-hosts.md)|[Mise en réseau SDN (Software Defined Networking)](subsystem/software-defined-networking/index.md)|
||[Serveurs Web](role/web-server/index.md)||
||[Conteneurs Windows Server](role/windows-server-container/index.md)||


## <a name="changes-in-this-version"></a>Modifications de cette version

### <a name="sections-added"></a>Sections ajoutées
- [Considérations relatives à la configuration de type d'installation Nano Server](../../get-started/getting-started-with-nano-server.md)


- [Mise en réseau SDN (Software Defined Networking)](subsystem/software-defined-networking/index.md) et recommandations relatives à la configuration de passerelle [HNV](subsystem/software-defined-networking/hnv-gateway-performance.md) et [SLB](subsystem/software-defined-networking/slb-gateway-performance.md)

- [Espaces de stockage direct](subsystem/storage-spaces-direct/index.md)

- [HTTP1.1 et HTTP2](role/web-server/http-performance.md)

- [Conteneurs Windows Server](role/windows-server-container/index.md)

### <a name="sections-changed"></a>Sections modifiées

- Mises à jour de la section [Recommandations relatives à Active Directory](role/active-directory-server/index.md)

- Mises à jour de la section [Recommandations relatives aux serveurs de fichiers](role/file-server/index.md)

- Mises à jour de la section [Recommandations relatives aux serveur Web](role/web-server/index.md)

- Mises à jour de la section [Recommandations relatives à la puissance du matériel](hardware/power.md)

- Mises à jour de la section [Recommandations relatives à l’optimisation de PowerShell](powershell/index.md)

- Mises à jour importantes de la section [Recommandations relatives à Hyper-V](role/hyper-v-server/index.md)

- Section *Optimisation des performances pour les charges de travail supprimée*, liens vers les ressources pertinentes ajoutés à l'[article Ressources d’optimisation supplémentaires](additional-resources.md)

- *Suppression de sections dédiées au stockage*, en faveur de la nouvelle section [Espaces de stockage directs](subsystem/storage-spaces-direct/index.md) et du contenu canonique de Technet

- *Suppression de la section réseau dédiée*, en faveur du contenu canonique de Technet  
