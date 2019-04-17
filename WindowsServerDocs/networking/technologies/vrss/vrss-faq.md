---
title: vRSS Forum aux Questions
description: Vous trouverez dans cette rubrique, que certaines couramment les questions fréquentes et réponses sur l’utilisation de vRSS.
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
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133815"
---
# vRSS Forum aux Questions

Vous trouverez dans cette rubrique, que certaines couramment les questions fréquentes et réponses sur l’utilisation de vRSS.

## Quelles sont les exigences pour les cartes réseau physiques que j’utilise avec vRSS?

Cartes réseau doivent être compatibles avec la file d’attente de la Machine virtuelle \(VMQ\) et doivent avoir au moins une vitesse de liaison de 10 Gbits/s.

Pour plus d’informations, voir [planifier l’utilisation de vRSS](vrss-plan.md).

## VRSS fonctionne avec les cœurs de processeur hyper\-threaded?

Non. À la fois vRSS et VMQ ignorent les cœurs de processeur hyper\-threaded.

## VRSS fonctionne pour les \(vNICs\) cartes réseau virtuelles hôtes?

Oui. Utilisez le paramètre **- ManagementOS** au lieu du nom \(VM\) de machine virtuelle sur la commande Windows PowerShell **Set-VMNetworkAdapter** et **Enable-NetAdapterRss** sur la carte réseau virtuelle hôte.

Pour plus d’informations, voir [Les commandes Windows PowerShell pour RSS et vRSS](vrss-wps.md).

## Le nombre de processeurs logique une machine virtuelle a besoin d’utiliser des vRSS?

Machines virtuelles ont besoin de deux ou plusieurs \(LPs\) processeurs logiques pour être en mesure d’utiliser vRSS.

Pour plus d’informations, voir [planifier l’utilisation de vRSS](vrss-plan.md).

## VRSS est compatible avec l’association de cartes?

Oui. Si vous utilisez l’association de cartes réseau, il est important de configurer correctement VMQ pour fonctionner avec les paramètres d’association de cartes. Pour obtenir des informations détaillées sur l’association de cartes de déploiement et de gestion, consultez [l’Association de cartes](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

## vRSS est activé, mais comment savoir si elle fonctionne? 

Vous serez en mesure de savoir vRSS fonctionne bien en ouvrant le Gestionnaire des tâches dans votre machine virtuelle et l’affichage de l’utilisation du processeur virtuel. S’il existe plusieurs connexions établies à la machine virtuelle, vous pouvez voir plusieurs core au-dessus de 0 % d’utilisation.

Dans la mesure où une seule session TCP ne peut pas être réparties sur plusieurs cœurs de processeurs logiques, votre machine virtuelle doit être réception TCP plusieurs sessions avant que vous pouvez observer ou non vRSS fonctionne.

Si l’ordinateur virtuel reçoit plusieurs sessions TCP, mais vous ne voyez pas plusieurs core LP au-dessus de l’utilisation de 0 %, assurez-vous que vous avez terminé toutes les étapes de préparation de la rubrique [planifier l’utilisation de vRSS](vrss-plan.md).

## J’ai regardant l’hôte et tous les processeurs sont utilisés. Il semble que chaque une autre est ignorée.
  
Vérifiez si hyper-threading est activée. À la fois VMQ et vRSS sont conçus pour ignorer les cœurs hyper\-threaded.

## Existe-t-il des différentes commandes Windows PowerShell pour RSS et vRSS?

Oui et non. Pendant que vous utilisez les mêmes commandes pour RSS dans les hôtes natifs et RSS dans des machines virtuelles, vRSS requiert également VMQ être activée sur la carte réseau physique - et pour la machine virtuelle et le vRSS être activée sur le port de commutateur.

Pour plus d’informations, voir [Les commandes Windows PowerShell pour RSS et vRSS](vrss-wps.md).

---
