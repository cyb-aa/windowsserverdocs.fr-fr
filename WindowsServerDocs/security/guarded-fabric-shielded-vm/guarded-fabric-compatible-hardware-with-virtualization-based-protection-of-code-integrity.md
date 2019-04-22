---
title: Matériel compatible avec la protection sur la virtualisation Windows Server de l’intégrité du Code
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 15ded82c-f70f-4efb-9e26-2731127931af
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: a52ff808af94159fe50c72bf0466768047ceea90
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820370"
---
# <a name="compatible-hardware-with-windows-server-virtualization-based-protection-of-code-integrity"></a>Matériel compatible avec la protection sur la virtualisation Windows Server de l’intégrité du Code

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Windows Server 2016 a introduit un nouveau groupe de protection de code basé sur la virtualisation pour protéger physiques et machines virtuelles contre les attaques qui modifient le code du système. Pour atteindre ce niveau de protection élevé, Microsoft travaille en tandem avec les fabricants de matériel ordinateur (Original Equipment Manufacturers ou OEM) pour empêcher les écritures malveillants dans le code de l’exécution du système. Cette protection peut être appliquée à n’importe quel système et est utilisée comme l’un des blocs de construction pour l’implémentation de l’intégrité de l’hôte Hyper-V pour les machines virtuelles protégées (machines virtuelles). 

Comme avec toute protection matérielle, certains systèmes peuvent ne pas être conformes en raison de problèmes tels que marquage incorrect de pages de mémoire en tant qu’exécutables ou en essayant de réellement modifier le code au moment de l’exécution, ce qui peut entraîner des erreurs inattendues, y compris la perte de données ou un bleu Erreur de l’écran (également appelé une erreur d’arrêt). 

Pour être compatibles et prennent entièrement en charge la nouvelle fonctionnalité de sécurité, les fabricants OEM doivent implémenter la Table des adresses mémoire définis dans UEFI 2.6, ce qui a été publiée en janvier 2016. L’adoption de la nouvelle UEFI standard prend du temps ; pendant ce temps, pour empêcher les clients de rencontrer des problèmes, nous souhaitons fournissent des informations sur les systèmes et les configurations que nous avons testés à cet ensemble de fonctionnalités à ainsi que les systèmes qui nous savons ne sont ne pas compatibles. 

## <a name="non-compatible-systems"></a>Systèmes non compatibles

Les configurations suivantes sont connues comme étant non compatible avec la protection sur la virtualisation de l’intégrité du code et ne peut pas être utilisé en tant qu’hôte pour les machines virtuelles protégées :

- Les serveurs PowerEdge Dell en cours d’exécution contrôleurs RAID PERC H330 pour plus d’informations, consultez l’article suivant du support technique de Dell [H330 – activation « Support de Hyper-V Guardian hôte » ou « Device Guard » sur le système d’exploitation de Win 2016 entraîne l’échec de démarrage du système d’exploitation](http://www.dell.com/Support/Article/us/en/19/QNA44045).  


## <a name="compatible-systems"></a>Systèmes compatibles

Voici les systèmes de nos partenaires et nous avons testé dans notre environnement. Assurez-vous que vous vérifiez que le système fonctionne comme prévu dans votre environnement : 

- Machines virtuelles – vous pouvez activer la protection de basés sur la virtualisation de l’intégrité du code sur des machines virtuelles qui s’exécutent sur un hôte Hyper-V à partir de Windows Server 2016.



