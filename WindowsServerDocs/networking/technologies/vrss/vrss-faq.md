---
title: vRSS Forum aux Questions
description: Dans cette rubrique, vous trouverez que certaines fréquentes questions et réponses sur l’utilisation de vRSS.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 61ae242e-82a8-430d-b07d-52b86c01e686
ms.localizationpriority: medium
manager: dougkim
ms.date: 09/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3fafe6c39285e65a9d39a76cc6b652dac5c3efbd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840240"
---
# <a name="vrss-frequently-asked-questions"></a>vRSS Forum aux Questions

Dans cette rubrique, vous trouverez que certaines fréquentes questions et réponses sur l’utilisation de vRSS.

## <a name="what-are-the-requirements-for-the-physical-network-adapters-that-i-use-with-vrss"></a>Quelles sont les exigences pour les cartes réseau physiques que j’utilise grâce à vRSS ?

Cartes réseau doivent être compatibles avec la file d’attente de la Machine virtuelle \(VMQ\) et doit avoir une vitesse de liaison de 10 Gbits/s ou plus.

Pour plus d’informations, consultez [planifiez l’utilisation de vRSS](vrss-plan.md).

## <a name="does-vrss-work-with-hyper-threaded-processor-cores"></a>VRSS fonctionne-t-il avec hyper\-thread cœurs de processeur ?

Non. VRSS et VMQ ignorent hyper\-cœurs de processeur multithreads.

## <a name="does-vrss-work-for-host-virtual-nics-vnics"></a>VRSS fonctionne-t-il pour héberger des cartes réseau virtuelles \(cartes réseau virtuelles\)?

Oui. Utilisez le **- ManagementOS** paramètre au lieu de la machine virtuelle \(machine virtuelle\) nom sur le **Set-VMNetworkAdapter** commande Windows PowerShell, et  **Enable-NetAdapterRss** sur la carte réseau virtuelle hôte.

Pour plus d’informations, consultez [commandes PowerShell de Windows pour les flux RSS et vRSS](vrss-wps.md).

## <a name="how-many-logical-processors-does-a-vm-need-to-use-vrss"></a>Le nombre de processeurs logique une machine virtuelle a besoin d’utiliser des vRSS ?

Machines virtuelles doivent deux ou plusieurs processeurs logiques \(LPs\) pour être en mesure d’utiliser vRSS.

Pour plus d’informations, consultez [planifiez l’utilisation de vRSS](vrss-plan.md).

## <a name="is-vrss-compatible-with-nic-teaming"></a>VRSS est compatible avec l’association de cartes réseau ?

Oui. Si vous utilisez l’association de cartes réseau, il est important de configurer correctement les ordinateurs virtuels pour travailler avec les paramètres d’association de cartes réseau. Pour obtenir des informations détaillées sur l’association de cartes réseau de déploiement et la gestion, consultez [association de cartes réseau](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

## <a name="vrss-is-enabled-but-how-do-i-know-if-it-is-working"></a>vRSS est activé, mais comment savoir si elle fonctionne ? 

Vous serez en mesure de savoir vRSS fonctionne en ouvrant le Gestionnaire des tâches dans votre machine virtuelle et en affichant l’utilisation du processeur virtuel. S’il existe plusieurs connexions établies à la machine virtuelle, vous pouvez voir plusieurs cœurs au-dessus de 0 % d’utilisation.

Une seule session TCP ne peut pas être à charge équilibrée sur plusieurs cœurs de processeur logique, votre machine virtuelle doit recevoir TCP plusieurs sessions avant que vous pouvez observer ou non vRSS fonctionne.

Si la machine virtuelle reçoit plusieurs sessions TCP, mais vous ne voyez pas plusieurs cœurs LP au-dessus de 0 % d’utilisation, vérifiez que vous avez effectué toutes les étapes de préparation dans la rubrique [planifiez l’utilisation de vRSS](vrss-plan.md).

## <a name="im-looking-at-the-host-and-not-all-of-the-processors-are-being-used-it-looks-like-every-other-one-is-being-skipped"></a>Je regarde l’hôte et les processeurs ne sont pas tous utilisés. Il semble qu’un sur deux soit ignoré.
  
Vérifiez si l’hyperthreading est activé. VMQ et vRSS sont conçues pour ignorer\-cœurs multithreads.

## <a name="are-there-different-windows-powershell-commands-for-rss-and-vrss"></a>Existe-t-il des différentes commandes Windows PowerShell pour les flux RSS et vRSS ?

Yes et no. Pendant que vous utilisez les mêmes commandes pour RSS dans les hôtes natifs et RSS dans des machines virtuelles, vRSS requiert également VMQ soit activé sur la carte réseau physique - et pour la machine virtuelle et vRSS doit être activé sur le port de commutateur.

Pour plus d’informations, consultez [commandes PowerShell de Windows pour les flux RSS et vRSS](vrss-wps.md).

---
