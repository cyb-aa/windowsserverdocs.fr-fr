---
title: Améliorations du gestionnaire de cache et de mémoire
description: Améliorations du gestionnaire de cache et de mémoire dans Windows Server 2016
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Pavel; ATales
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 5f66f43a0d1b003ab833c91538594510b44027d6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384931"
---
# <a name="cache-and-memory-manager-improvements"></a>Améliorations du gestionnaire de cache et de mémoire

Cette rubrique décrit les améliorations du gestionnaire de cache et du gestionnaire de mémoire dans Windows Server 2012 et 2016.

## <a name="cache-manager-improvements-in-windows-server-2016"></a>Améliorations du gestionnaire de cache dans Windows Server 2016
Le gestionnaire de cache a également ajouté la prise en charge des véritables lectures asynchrones en cache.
Cela peut potentiellement améliorer les performances d’une application si elle s’appuie fortement sur des lectures asynchrones mises en cache.  Bien que la plupart des systèmes de fichiers intégrés aient pris en charge des lectures asynchrones en cache pendant un certain temps, il existait souvent des limitations de performances en raison de différents choix de conception liés à la gestion des pools de threads et des files d’attente de travail internes des systèmes de fichiers.  Avec la prise en charge de kernel-NOMPROPRE, le gestionnaire de cache masque désormais toutes les complexités de gestion des pools de threads et des files d’attente de travail à partir de systèmes de fichiers, ce qui le rend plus efficace lors de la gestion des lectures asynchrones en Le gestionnaire de cache possède un ensemble de datastructures de contrôle pour chacun des niveaux d’imbrication de VHD (maximum pris en charge par le système) pour maximiser le parallélisme.


## <a name="cache-manager-improvements-in-windows-server-2012"></a>Améliorations du gestionnaire de cache dans Windows Server 2012
En plus des améliorations apportées au gestionnaire de cache en termes de logique de lecture anticipée pour les charges de travail séquentielles, une nouvelle API [CcSetReadAheadGranularityEx](https://msdn.microsoft.com/library/windows/hardware/hh406341.aspx) a été ajoutée pour permettre aux pilotes de système de fichiers, tels que SMB, de modifier leurs paramètres de lecture anticipée. Il offre un meilleur débit pour les scénarios de fichiers distants en envoyant plusieurs demandes de lecture anticipée de petite taille au lieu d’envoyer une seule demande de lecture anticipée. Seuls les composants du noyau, tels que les pilotes de système de fichiers, peuvent configurer par programme ces valeurs par fichier.

## <a name="memory-manager-improvements-in-windows-server-2012"></a>Améliorations du gestionnaire de mémoire dans Windows Server 2012
L’activation de la combinaison de pages peut réduire l’utilisation de la mémoire sur les serveurs qui possèdent un grand nombre de pages privées et paginables avec un contenu identique. Par exemple, les serveurs qui exécutent plusieurs instances de la même application gourmande en mémoire, ou une seule application qui fonctionne avec des données très répétitives, peuvent être de bons candidats pour essayer la combinaison de pages. L’inconvénient de l’activation de la combinaison de pages est l’augmentation de l’utilisation du processeur.

Voici quelques exemples de rôles de serveur dans lesquels la combinaison de pages est peu susceptible d’offrir un grand intérêt :

-   Serveurs de fichiers (la plus grande partie de la mémoire est consommée par les pages de fichier qui ne sont pas privées et ne peuvent donc pas être combinées)

-   Serveurs Microsoft SQL configurés pour utiliser AWE ou des pages de grande taille (la majeure partie de la mémoire est privée mais non paginable)

La combinaison de pages est désactivée par défaut, mais peut être activée à l’aide de l’applet de commande Windows PowerShell [Enable-mmagent](https://technet.microsoft.com/library/jj658954.aspx) . La combinaison de pages a été ajoutée dans Windows Server 2012.
