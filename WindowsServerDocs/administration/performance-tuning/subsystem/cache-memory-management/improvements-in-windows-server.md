---
title: Cache et les améliorations apportées au Gestionnaire de mémoire
description: Cache et les améliorations du Gestionnaire de mémoire dans Windows Server 2016
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Pavel; ATales
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: bd15cfc0714d1992e6b15d716b6b96ce259786da
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868580"
---
# <a name="cache-and-memory-manager-improvements"></a>Cache et les améliorations apportées au Gestionnaire de mémoire

Cette rubrique décrit les améliorations de gestionnaire de Cache et le Gestionnaire de mémoire dans Windows Server 2012 et 2016.

## <a name="cache-manager-improvements-in-windows-server-2016"></a>Améliorations du Gestionnaire de cache dans Windows Server 2016
Gestionnaire de cache a également ajouté la prise en charge pour lit true par mis en cache asynchrone.
Cela peut potentiellement améliorer les performances d’une application si elle s’appuie sur les lectures mis en cache asynchrones.  Alors que la plupart des systèmes de fichiers de zone dans ont pris en charge les lectures asynchrones mis en cache pendant un certain temps, il étaient souvent des limitations de performances en raison des différents choix de conception relatives à la gestion des pools de threads et des files d’attente de travail interne des systèmes de fichiers.  Avec la prise en charge à partir du noyau-approprié, Gestionnaire de Cache masque désormais tout le pool de threads et difficultés de gestion de file d’attente de travail à partir de la rendre plus efficace pour traiter asynchrone des systèmes de fichiers mis en cache des lectures. Gestionnaire de cache a un ensemble de datastructures de contrôle pour chacun des (maximum du système pris en charge) niveaux d’imbrication de disque dur virtuel à optimiser le parallélisme.


## <a name="cache-manager-improvements-in-windows-server-2012"></a>Améliorations du Gestionnaire de cache dans Windows Server 2012
En plus des améliorations du Gestionnaire de Cache lecture anticipée logique pour les charges de travail séquentiels, une nouvelle API [CcSetReadAheadGranularityEx](https://msdn.microsoft.com/library/windows/hardware/hh406341.aspx) a été ajoutée pour permettre à des pilotes de système, telles que SMB, modifiez leurs paramètres de cache de lecture. Il permet de débit plus élevé pour les scénarios de fichier distant en envoyant la que demande de plusieurs petit lecture anticipée au lieu d’envoyer qu'une seule grande lecture anticipée demande. Seuls les composants de noyau, tels que des pilotes de système de fichiers, peuvent configurer par programme ces valeurs sur une base par fichier.

## <a name="memory-manager-improvements-in-windows-server-2012"></a>Améliorations du Gestionnaire de mémoire dans Windows Server 2012
L’activation de combinaison de page peut réduire l’utilisation de mémoire sur les serveurs qui ont un grand nombre de pages privées, paginables avec un contenu identique. Par exemple, les serveurs exécutant plusieurs instances de la même application nécessitant beaucoup de mémoire ou d’une application unique qui fonctionne avec des données très répétitives, peuvent être bons candidats pour essayer la combinaison de page. L’inconvénient de l’activation de combinaison de page est une utilisation accrue du processeur.

Voici quelques exemples de rôles de serveur où la page combinaison est peu probable donner beaucoup d’avantages :

-   Serveurs de fichiers (la plupart de la mémoire est consommée par les pages de fichier qui ne sont pas privés et par conséquent pas pouvant être combiné)

-   Serveurs Microsoft SQL Server qui sont configurés pour utiliser AWE ou des pages de grande taille (la plupart de la mémoire est privé, mais non paginable)

Combinaison de page est désactivée par défaut mais peut être activée à l’aide de la [Enable-MMAgent](https://technet.microsoft.com/library/jj658954.aspx) applet de commande Windows PowerShell. Combinaison de page a été ajoutée dans Windows Server 2012.
