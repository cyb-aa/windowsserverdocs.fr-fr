---
title: Matériel compatible avec la protection de l’intégrité du code basée sur la virtualisation Windows Server
ms.prod: windows-server
ms.topic: article
ms.assetid: 15ded82c-f70f-4efb-9e26-2731127931af
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 32194d6f0634ab9cee90b321ea7a1f3e2769542d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856872"
---
# <a name="compatible-hardware-with-windows-server-virtualization-based-protection-of-code-integrity"></a>Matériel compatible avec la protection de l’intégrité du code basée sur la virtualisation Windows Server

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Windows Server 2016 a introduit une nouvelle protection du code basée sur la virtualisation pour aider à protéger les machines physiques et virtuelles contre les attaques qui modifient le code système. Pour atteindre ce niveau de protection élevé, Microsoft travaille en tandem avec les fabriques de matériel informatique (fabricants d’ordinateurs OEM) pour empêcher les écritures malveillantes dans le code d’exécution du système. Cette protection peut être appliquée à n’importe quel système et est utilisée comme l’un des blocs de construction permettant d’implémenter l’intégrité de l’hôte Hyper-V pour les machines virtuelles protégées. 

Comme pour toute protection matérielle, certains systèmes peuvent ne pas être conformes en raison de problèmes tels que le marquage incorrect des pages de mémoire en tant qu’exécutables ou en tentant de modifier le code au moment de l’exécution, ce qui peut entraîner des défaillances inattendues, y compris la perte de données ou une erreur d’écran bleu (également appelée erreur d’arrêt). 

Pour être compatible et prendre en charge la nouvelle fonctionnalité de sécurité, les fabricants d’ordinateurs OEM doivent implémenter la table d’adresses mémoire définie dans UEFI 2,6, qui a été publié en janvier 2016. L’adoption de la nouvelle norme UEFI prend du temps. pendant ce temps, pour empêcher les clients rencontrant des problèmes, nous souhaitons fournir des informations sur les systèmes et les configurations sur lesquels nous avons testé ce jeu de fonctionnalités, ainsi que sur les systèmes dont nous savons qu’ils ne sont pas compatibles. 

## <a name="non-compatible-systems"></a>Systèmes non compatibles

Les configurations suivantes sont connues pour être non compatibles avec la protection basée sur la virtualisation de l’intégrité du code et ne peuvent pas être utilisées en tant qu’hôte pour les machines virtuelles protégées :

- Serveurs Dell PowerEdge exécutant les contrôleurs RAID PERC H330 pour plus d’informations, consultez l’article suivant de la page support Dell [H330 : activation de la « prise en charge d’Hyper-V Guardian hôte » ou « Device Guard » sur le système d’exploitation Win 2016 provoque l’échec du démarrage du système d’exploitation](http://www.dell.com/Support/Article/us/en/19/QNA44045).  


## <a name="compatible-systems"></a>Systèmes compatibles

Il s’agit des systèmes que nous et nos partenaires testons dans notre environnement. Veillez à vérifier que le système fonctionne comme prévu dans votre environnement : 

- Machines virtuelles : vous pouvez activer la protection basée sur la virtualisation de l’intégrité du code sur les machines virtuelles qui s’exécutent sur un hôte Hyper-V à compter de Windows Server 2016.



