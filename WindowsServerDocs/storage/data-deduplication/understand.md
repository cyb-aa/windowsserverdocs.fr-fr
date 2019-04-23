---
ms.assetid: acc0803b-fa05-4fc3-b94d-2916abf4fdbd
title: Présentation de la déduplication des données
ms.technology: storage-deduplication
ms.prod: windows-server-threshold
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 09/15/2016
ms.openlocfilehash: cb17329fb0556a25bc49c2fdb6b16f878aa34194
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839380"
---
# <a name="understanding-data-deduplication"></a>Présentation de la déduplication des données

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Ce document décrit le fonctionnement de la [déduplication des données](overview.md).

## <a name="how-does-dedup-work"></a>Comment fonctionne la déduplication des données ?

La déduplication des données dans Windows Server a été créée avec les deux principes suivants:

1. **L’optimisation ne doit pas interférer écritures sur le disque**  
    La déduplication des données permet d’optimiser les données au moyen d’un modèle de post-traitement. Toutes les données sont écrites non optimisées sur le disque pour l’être ensuite par la déduplication des données.

2. **L’optimisation ne doit pas modifier sémantique de l’accès**  
    Les utilisateurs et les applications qui accèdent aux données sur un volume optimisé n’ont pas du tout conscience que les fichiers auxquels ils accèdent ont été dédupliqués.

Une fois qu’elle est activée pour un volume, la déduplication des données s’exécute en arrière-plan pour:

- identifier les modèles récurrents dans tous les fichiers de ce volume ;
- déplacer en toute transparence les parties (ou « blocs ») avec des pointeurs spéciaux appelés [points d’analyse](#dedup-term-reparse-point) qui pointent vers une copie unique de ces blocs.

Cette opération se déroule en quatre étapes :

1. Analysez le système de fichiers à la recherche de fichiers conformes à la stratégie d’optimisation.  
![Rechercher dans le système de fichiers](media/understanding-dedup-how-dedup-works-1.gif)  
2. Divisez les fichiers en blocs de taille variable.  
![Diviser les fichiers en blocs](media/understanding-dedup-how-dedup-works-2.gif)
3. Identifiez les blocs uniques.  
![Identifier les blocs uniques](media/understanding-dedup-how-dedup-works-3.gif)
4. Placez les blocs dans le magasin de blocs et compressez-les si vous le voulez.  
![Placer les blocs dans le magasin de blocs](media/understanding-dedup-how-dedup-works-4.gif)
5. Remplacez le flux d’origine des fichiers maintenant optimisés par un point d’analyse qui pointe vers le magasin de blocs.  
![Remplacer le flux de fichier par un point d’analyse](media/understanding-dedup-how-dedup-works-5.gif)

Quand les fichiers optimisés sont lus, le système de fichiers envoie les fichiers avec un point d’analyse au filtre de système de fichiers de la déduplication des données (Dedup.sys). Le filtre redirige l’opération de lecture vers les blocs appropriés qui constituent le flux de ce fichier dans le magasin de blocs. Les modifications apportées aux plages de fichiers dédupliqués sont écrites sur le disque et sont optimisées par le [travail d’optimisation](understand.md#job-info) quand il est à nouveau exécuté.

## <a id="usage-type"></a>Types d’utilisation
Les types d’utilisation suivants fournissent une configuration de la déduplication des données valable pour les charges de travail courantes :  

| Type d’utilisation | Charges de travail idéales | Différences |
|------------|-----------------|------------------|
| <a id="usage-type-default"></a>Par défaut | Serveur de fichiers à usage général :<ul><li>Partages d’équipe</li><li>Dossiers de travail</li><li>Redirection de dossiers</li><li>Partages de développement de logiciels</li></ul> | <ul><li>Optimisation en arrière-plan</li><li>Stratégie d’optimisation par défaut:<ul><li>Ancienneté minimale des fichiers = 3jours</li><li>Optimiser les fichiers en cours d’utilisation = non</li><li>Optimiser les fichiers partiels = non</li></ul></li></ul> |
| <a id="usage-type-hyperv"></a>Hyper-V | ServeursVDI (Virtual Desktop Infrastructure) | <ul><li>Optimisation en arrière-plan</li><li>Stratégie d’optimisation par défaut:<ul><li>Ancienneté minimale des fichiers = 3jours</li><li>Optimiser les fichiers en cours d’utilisation = oui</li><li>Optimiser les fichiers partiels = oui</li></ul></li><li>Modifications mineures d’implémentation pour l’interopérabilité Hyper-V</li></ul> |
| <a id="usage-type-backup"></a>Sauvegarde | Applications de sauvegarde virtualisée, telles que [Microsoft Data Protection Manager (DPM)](https://technet.microsoft.com/library/hh758173.aspx) | <ul><li>Optimisation de la priorité</li><li>Stratégie d’optimisation par défaut:<ul><li>Ancienneté minimale des fichiers = 0 jour</li><li>Optimiser les fichiers en cours d’utilisation = oui</li><li>Optimiser les fichiers partiels = non</li></ul></li><li>Modifications mineures d’implémentation pour l’interopérabilité avec des solutions DPM ou de type DPM</li></ul> |

## <a id="job-info"></a>Travaux
La déduplication des données utilise une stratégie de post-traitement pour optimiser et maintenir le rendement de l’espace d’un volume.

| Nom du travail | Description du travail | Calendrier par défaut |
|----------|------------------|------------------|
| <a id="job-info-optimization"></a>Optimisation | Le travail **Optimisation** procède à la déduplication en divisant les données d’un volume en blocs selon les paramètres de stratégie de volume, en compressant (éventuellement) ces blocs et en les stockant de façon unique dans le magasin de blocs. Le processus d’optimisation utilisé par la déduplication des données est décrit en détail dans [Fonctionnement de la déduplication des données](understand.md#how-does-dedup-work). | Toutes les heures |
| <a id="job-info-gc"></a>Le Garbage Collection | Le travail **Nettoyage de la mémoire** récupère de l’espace disque en supprimant les blocs inutiles qui ne sont plus référencés par les fichiers qui ont été récemment modifiés ou supprimés. | Tous les samedis à 02h35 |
| <a id="job-info-scrubbing"></a>Nettoyage des données | Le travail **Nettoyage des données** identifie toute altération dans le magasin de blocs causée par des défaillances de disque ou des secteurs défectueux. Quand cela est possible, la déduplication des données peut utiliser automatiquement les fonctionnalités de volume (par exemple, la mise en miroir ou la parité sur un volume des espaces de stockage) pour reconstruire les données endommagées. De plus, la déduplication des données conserve des copies de sauvegarde des blocs populaires quand ils sont référencés plus de 100 fois dans une zone appelée « zone réactive ». | Tous les samedis à 03h35 |
| <a id="job-info-unoptimization"></a>Annulation de l’optimisation | Le travail **Annulation de l’optimisation** est un travail spécial qui ne peut être exécuté que manuellement et qui annule l’optimisation effectuée par la déduplication et désactive la déduplication des données pour ce volume. | [À la demande uniquement](run.md#disabling-dedup) |

## <a id="dedup-term"></a>Terminologie relative à la déduplication des données
| Terme | Définition |
|------|------------|
| <a id="dedup-term-chunk"></a>Chunk | Un bloc est une section d’un fichier qui a été sélectionnée par l’algorithme de segmentation de la déduplication des données comme étant susceptible de se trouver dans d’autres fichiers similaires. |
| <a id="dedup-term-chunk-store"></a>Magasin de blocs | Le magasin de blocs est une série organisée de fichiers de conteneur dans le dossier System Volume Information que la déduplication des données utilise pour stocker de façon unique des blocs. |
| <a id="dedup-term-dedup"></a>déduplication | Abréviation en anglais de la déduplication des données qui est communément employée dans PowerShell, les composants et les API Windows Server, ainsi que dans la communauté Windows Server. |
| <a id="dedup-term-file-metadata"></a>Métadonnées d’un fichier | Chaque fichier contient des métadonnées qui décrivent des propriétés intéressantes sur le fichier qui ne sont pas liées au contenu principal du fichier, par exemple Date de création, Date de la dernière lecture, Auteur, etc. |
| <a id="dedup-term-file-stream"></a>Flux de fichier | Le flux de fichier est le contenu principal du fichier. Il s’agit de la partie du fichier que la déduplication des données optimise. |
| <a id="dedup-term-file-system"></a>Système de fichiers | Le système de fichiers est le logiciel et la structure de données sur disque dont se sert le système d’exploitation pour stocker les fichiers sur un support de stockage. La déduplication des données est prise en charge sur les volumes formatés en NTFS. |
| <a id="dedup-term-file-system-filter"></a>Filtre de système de fichiers | Un filtre de système de fichiers est un plug-in qui modifie le comportement par défaut du système de fichiers. Pour préserver la sémantique de l’accès, la déduplication des données utilise un filtre de système de fichiers (Dedup.sys) pour rediriger les opérations de lecture vers le contenu optimisé de manière entièrement transparente pour l’utilisateur ou l’application qui effectue la demande de lecture. |
| <a id="dedup-term-optimization"></a>Optimisation | Un fichier est considéré comme optimisé (ou dédupliqué) par la déduplication des données s’il a été divisé en blocs et que ses blocs uniques ont été stockés dans le magasin de blocs. |
| <a id="dedup-term-in-policy"></a>Stratégie d’optimisation | La stratégie d’optimisation indique les fichiers à prendre en considération pour la déduplication des données. Par exemple, les fichiers peuvent être considérés comme étant hors stratégie s’ils sont entièrement nouveaux, ouverts, dans un chemin spécifique sur le volume ou d’un certain type de fichier. |
| <a id="dedup-term-reparse-point"></a>Point d’analyse | Un [point d’analyse](https://msdn.microsoft.com/library/windows/desktop/aa365503.aspx) est un indicateur spécial qui demande au système de fichiers de transmettre les E/S à un filtre de système de fichiers spécifié. Quand le flux d’un fichier a été optimisé, la déduplication des données remplace le flux de fichier par un point d’analyse, ce qui permet à la déduplication des données de préserver la sémantique de l’accès pour ce fichier. |
| <a id="dedup-term-volume"></a>Volume | Un volume est une construction Windows pour un lecteur de stockage logique qui peut s’étendre sur plusieurs dispositifs de stockage physiques sur un ou plusieurs serveurs. La déduplication est activée volume par volume. |
| <a id="dedup-term-workload"></a>Charge de travail | Une charge de travail est une application qui s’exécute sur Windows Server. Des exemples de charges de travail incluent le serveur de fichiers à usage général, Hyper-V et SQL Server. |

> [!Warning]  
> Sauf indication contraire des services de support technique Microsoft autorisés, n’essayez pas de modifier manuellement le magasin de blocs. Vous risqueriez d’endommager ou de perdre des données.

## <a name="frequently-asked-questions"></a>Forum Aux Questions
**En quoi diffère la déduplication des données autres produits d’optimisation ?**  
Il existe plusieurs différences importantes entre la fonctionnalité de déduplication des données et d’autres produits d’optimisation de stockage courants :

* *En quoi diffère la déduplication des données Single Instance Store ?*  
    Single-Instance-Store, ou SIS, est une technologie antérieure à la déduplication des données qui a été inaugurée par Windows Storage Server 2008 R2. Pour optimiser un volume, SIS identifiait les fichiers qui étaient en tout point identiques et les remplaçait par des liens logiques donnant accès à une copie unique d’un fichier stocké dans le stockage SIS commun. Contrairement à SIS, la déduplication des données peut économiser de l’espace à partir de fichiers qui ne sont pas identiques, mais qui ont de nombreux modèles communs et à partir de fichiers qui contiennent eux-mêmes de nombreux modèles récurrents. Single-Instance-Store a été déconseillé dans Windows Server2012R2 et supprimé dans Windows Server2016 en faveur de la déduplication des données.

* *Comment la déduplication des données est-elle différente de la compression NTFS ?*  
    La compression NTFS est une fonctionnalité de NTFS que vous pouvez éventuellement activer au niveau du volume. Avec la compression NTFS, chaque fichier est optimisé individuellement via la compression au moment de l’écriture. Contrairement à la compression NTFS, la déduplication des données permet d’obtenir des gains d’espace sur tous les fichiers d’un volume. Cette technologie est plus efficace que la compression NTFS, car les fichiers peuvent <u>à la fois</u> être dupliqués en interne (ce qui est géré par la compression NTFS) et montrer des similitudes avec d’autres fichiers sur le volume (ce qui n’est pas géré par la compression NTFS). De plus, la déduplication des données propose un modèle de post-traitement, ce qui signifie que les fichiers nouveaux ou modifiés sont écrits sur disque sans être optimisés pour l’être ensuite par la déduplication des données.

* *En quoi la déduplication des données diffère-t-elle de formats de fichier d’archive comme zip, rar, 7z, cab, etc. ?*  
    Les formats de fichier d’archive tels que zip, rar, 7z, cab, etc. effectuent la compression sur un jeu spécifié de fichiers. À l’instar de la déduplication des données, les modèles récurrents au sein des fichiers et les modèles en double entre les fichiers sont optimisés. Cependant, vous devez choisir les fichiers à inclure dans l’archive. La sémantique de l’accès présente elle aussi des différences. Pour accéder à un fichier spécifique au sein de l’archive, vous devez ouvrir l’archive, sélectionner un fichier spécifique et décompresser ce fichier pour l’utiliser. La déduplication des données opère en toute transparence pour les utilisateurs et les administrateurs et ne nécessite aucun lancement manuel. Par ailleurs, la déduplication des données conserve la sémantique de l’accès : les fichiers optimisés apparaissent inchangés après l’optimisation.

**Puis-je modifier les paramètres de la déduplication des données pour mon Type d’utilisation sélectionné ?**  
Oui. Bien que la déduplication des données offre des paramètres par défaut valables pour les **charges de travail recommandées**, vous pouvez quand même souhaiter adapter des paramètres de la déduplication des données pour tirer le meilleur parti de votre stockage. En outre, les autres charges de travail [nécessitent quelques réglages pour garantir que la déduplication des données n’interfère pas avec la charge de travail](install-enable.md#enable-dedup-sometimes-considerations).

**Puis-je exécuter manuellement un travail de déduplication des données ?**  
Oui, [tous les travaux de déduplication des données peuvent être exécutés manuellement](run.md#running-dedup-jobs-manually). Cela peut être souhaitable si les travaux planifiés n’ont pas été exécutés en raison de ressources système insuffisantes ou d’une erreur. En outre, le travail Annulation de l’optimisation ne peut être exécuté que manuellement.

**Puis-je surveiller les résultats historiques des travaux de déduplication des données ?**  
Oui, [tous les travaux de déduplication des données créent des entrées dans le journal des événements Windows](run.md#monitoring-dedup).

**Puis-je modifier les planifications par défaut pour les travaux de déduplication des données sur mon système ?**  
Oui, [toutes les planifications sont configurables](advanced-settings.md#modifying-job-schedules). La modification des planifications de la déduplication des données par défaut est particulièrement souhaitable pour garantir que les travaux de déduplication des données ont le temps de se terminer et ne rivalisent pas avec la charge de travail pour les ressources.
