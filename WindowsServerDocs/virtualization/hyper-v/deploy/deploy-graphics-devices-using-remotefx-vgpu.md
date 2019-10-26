---
title: Déployer des appareils graphiques à l’aide du processeur graphique virtuel RemoteFX
description: Découvrez comment déployer et configurer le processeur graphique virtuel RemoteFX dans Windows Server
ms.prod: windows-server-threshold
ms.reviewer: rickman
author: rick-man
ms.author: rickman
manager: stevelee
ms.topic: article
ms.date: 08/21/2019
ms.openlocfilehash: 7111899557279d825191948e737d83d7467ce786
ms.sourcegitcommit: 81198fbf9e46830b7f77dcd345b02abb71ae0ac2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2019
ms.locfileid: "72923911"
---
# <a name="deploy-graphics-devices-using-remotefx-vgpu"></a>Déployer des appareils graphiques à l’aide du processeur graphique virtuel RemoteFX

> S’applique à : Windows Server 2016, Microsoft Hyper-V Server 2016

La fonctionnalité de processeur graphique virtuel pour RemoteFX permet à plusieurs machines virtuelles de partager un GPU physique. Le rendu et les ressources de calcul sont partagés de façon dynamique entre les machines virtuelles, ce qui rend le processeur graphique virtuel RemoteFX approprié pour les charges de travail à forte rafale, dans lesquelles les ressources GPU dédiées ne sont pas requises. Par exemple, dans un service VDI, le processeur graphique virtuel RemoteFX peut être utilisé pour décharger les coûts de rendu des applications sur le GPU, avec pour effet de réduire la charge de l’UC et d’améliorer l’évolutivité du service.

## <a name="remotefx-vgpu-requirements"></a>Configuration requise pour la fonctionnalité vGPU de RemoteFX

Configuration système requise pour l’ordinateur hôte :

- Windows Server 2016
- Un GPU compatible DirectX 11,0 avec un pilote compatible WDDM 1,2
- Un processeur avec prise en charge de la traduction d’adresses de second niveau (SLAT)

Configuration requise pour la machine virtuelle invitée :

- Système d’exploitation invité pris en charge. Pour plus d’informations, consultez [prise en charge de la carte vidéo 3D RemoteFX](../../../remote/remote-desktop-services/rds-supported-config.md#remotefx-3d-video-adapter-vgpu-support).

Autres considérations relatives aux machines virtuelles invitées :

- Les fonctionnalités OpenGL et OpenCL sont uniquement disponibles dans les invités exécutant Windows 10 ou Windows Server 2016.  
- DirectX 11,0 est disponible uniquement pour les invités exécutant Windows 8 ou version ultérieure.

## <a name="enable-remotefx-vgpu"></a>Activer le processeur graphique virtuel RemoteFX

Pour configurer le processeur graphique virtuel RemoteFX sur votre hôte Windows Server 2016 :

1. Installez les pilotes graphiques recommandés par le fournisseur de votre GPU pour Windows Server 2016.
2. Créer une machine virtuelle exécutant un système d’exploitation invité pris en charge par le processeur graphique virtuel RemoteFX. Pour plus d’informations, consultez [prise en charge de la carte vidéo 3D RemoteFX](../../../remote/remote-desktop-services/rds-supported-config.md#remotefx-3d-video-adapter-vgpu-support).
3. Ajoutez la carte graphique 3D RemoteFX à la machine virtuelle. Pour plus d’informations, consultez [configurer l’adaptateur 3D de processeur graphique virtuel RemoteFX](#configure-the-remotefx-vgpu-3d-adapter).

Par défaut, le processeur graphique virtuel RemoteFX utilise tous les GPU disponibles et pris en charge. Pour limiter les GPU utilisés par le processeur graphique virtuel RemoteFX, procédez comme suit :

1. Dans le Gestionnaire Hyper-V, accédez aux Paramètres Hyper-V.
2. Sélectionnez les **GPU physiques** dans les paramètres Hyper-V.
3. Sélectionnez le GPU que vous ne souhaitez pas utiliser, puis décochez **Utiliser cette unité GPU avec RemoteFX**.

### <a name="configure-the-remotefx-vgpu-3d-adapter"></a>Configurer la carte RemoteFX vGPU 3D

Vous pouvez configurer la carte graphique RemoteFX vGPU 3D à l'aide de l'interface utilisateur du Gestionnaire Hyper-V ou des applets de commande PowerShell.

#### <a name="configure-remotefx-vgpu-with-hyper-v-manager"></a>Configurer le processeur graphique virtuel RemoteFX avec le Gestionnaire Hyper-V

1. Arrêtez la machine virtuelle si elle est en cours d’exécution.
2. Ouvrez le Gestionnaire Hyper-V, accédez à **paramètres de machine virtuelle**, puis sélectionnez **Ajouter du matériel**.
3. Sélectionnez **carte graphique 3D RemoteFX**, puis sélectionnez **Ajouter**.
4. Définissez le nombre maximum de moniteurs, la résolution maximale de ceux-ci et la mémoire vidéo dédiée, ou conservez les valeurs par défaut.

   > [!NOTE]
   > - La définition de valeurs plus élevées pour l’une de ces options aura un impact sur votre mise à l’échelle du service. vous devez donc uniquement définir ce qui est nécessaire.
   > - Lorsque vous avez besoin d’utiliser 1 Go de VRAM dédiée, utilisez une machine virtuelle invitée 64 bits au lieu de 32 bits (x86) pour obtenir les meilleurs résultats.

5. Sélectionnez **OK** pour terminer la configuration.

#### <a name="configure-remotefx-vgpu-with-powershell-cmdlets"></a>Configurer le processeur graphique virtuel RemoteFX avec les applets de commande PowerShell

Utilisez les applets de commande PowerShell suivantes pour ajouter, réviser et configurer l’adaptateur :

- [Add-VMRemoteFx3dVideoAdapter](https://docs.microsoft.com/powershell/module/hyper-v/add-vmremotefx3dvideoadapter?view=win10-ps)
- [VMRemoteFx3dVideoAdapter](https://docs.microsoft.com/powershell/module/hyper-v/get-vmremotefx3dvideoadapter?view=win10-ps)
- [Set-VMRemoteFx3dVideoAdapter](https://docs.microsoft.com/powershell/module/hyper-v/set-vmremotefx3dvideoadapter?view=win10-ps)
- [VMRemoteFXPhysicalVideoAdapter](https://docs.microsoft.com/powershell/module/hyper-v/get-vmremotefxphysicalvideoadapter?view=win10-ps)

## <a name="monitor-performance"></a>Analyser les performances

Les performances et la mise à l’échelle d’un service compatible avec le processeur graphique virtuel RemoteFX sont déterminées par divers facteurs, tels que le nombre de GPU sur votre système, la mémoire totale du GPU, la quantité de mémoire système et la vitesse de la mémoire, le nombre de cœurs de PROCESSEURs et la fréquence d’horloge du processeur, la vitesse de stockage et NUMA. déploiement.

### <a name="host-system-memory"></a>Mémoire du système hôte

Pour chaque machine virtuelle activée avec un processeur graphique virtuel, RemoteFX utilise la mémoire système dans le système d’exploitation invité et dans le serveur hôte. L’hyperviseur garantit la disponibilité de la mémoire système pour un système d’exploitation invité. Sur l’ordinateur hôte, chaque bureau virtuel compatible avec un processeur graphique virtuel doit publier ses besoins en mémoire système sur l’hyperviseur. Lorsque le bureau virtuel compatible avec le processeur graphique virtuel démarre, l’hyperviseur réserve de la mémoire système supplémentaire dans l’hôte.

La mémoire requise pour le serveur RemoteFX est dynamique, car la quantité de mémoire consommée sur le serveur RemoteFX dépend du nombre d’analyses associées aux bureaux virtuels compatibles avec les processeurs virtuels et de la résolution maximale pour Ces analyses.

### <a name="host-gpu-video-memory"></a>Mémoire vidéo de l’ordinateur hôte GPU

Chaque bureau virtuel compatible avec un processeur graphique virtuel utilise la mémoire vidéo matérielle du GPU sur le serveur hôte pour afficher le bureau. En outre, un codec utilise la mémoire vidéo pour compresser l’écran rendu. La quantité de mémoire nécessaire pour le rendu et la compression est directement basée sur le nombre d’analyses approvisionnées sur la machine virtuelle. La quantité de mémoire vidéo réservée varie en fonction de la résolution d’écran du système et du nombre d’analyses. Certains utilisateurs nécessitent une résolution d’écran plus élevée pour des tâches spécifiques, mais il existe une plus grande évolutivité avec des paramètres de résolution inférieurs si tous les autres paramètres restent constants.

### <a name="host-cpu"></a>PROCESSEUR de l’hôte

L’hyperviseur planifie l’hôte et les machines virtuelles sur le processeur. La surcharge est augmentée sur un ordinateur hôte prenant en charge RemoteFX, car le système exécute un processus supplémentaire (rdvgm. exe) par bureau virtuel compatible avec des processeurs virtuels. Ce processus utilise le pilote de périphérique graphique pour exécuter des commandes sur le GPU. Le codec utilise également le processeur pour compresser les données d’écran qui doivent être renvoyées au client.

Un plus grand nombre de processeurs virtuels signifie une meilleure expérience utilisateur. Nous vous recommandons d’allouer au moins deux processeurs virtuels par bureau virtuel compatible avec les processeurs virtuels. Nous vous recommandons également d’utiliser l’architecture x64 pour les bureaux virtuels compatibles avec des processeurs virtuels, car les performances sur les machines virtuelles x64 sont meilleures par rapport aux machines virtuelles x86.

### <a name="gpu-processing-power"></a>Puissance de traitement GPU

Chaque bureau virtuel compatible avec un processeur graphique virtuel possède un processus DirectX correspondant qui s’exécute sur le serveur hôte. Ce processus relit toutes les commandes graphiques qu’il reçoit du bureau virtuel RemoteFX sur le GPU physique. Cela revient à exécuter plusieurs applications DirectX en même temps sur le même GPU physique.

En règle générale, les périphériques et les pilotes graphiques sont configurés pour exécuter uniquement quelques applications sur le Bureau à la fois, mais RemoteFX étire les GPU pour aller encore plus loin. vGPUs sont fournis avec des compteurs de performance qui mesurent la réponse GPU aux demandes RemoteFX et vous aident à vérifier que les GPU ne sont pas étirés trop loin.

Lorsqu’un GPU manque de ressources, l’exécution des opérations de lecture et d’écriture prend beaucoup de temps. Les administrateurs peuvent utiliser des compteurs de performances pour savoir quand ajuster les ressources et éviter les temps d’arrêt pour les utilisateurs.

Pour en savoir plus sur les compteurs de performance pour la surveillance du comportement du processeur graphique RemoteFX [, consultez diagnostiquer les problèmes de performances des graphiques dans Bureau à distance](https://docs.microsoft.com/azure/virtual-desktop/remotefx-graphics-performance-counters).
