---
title: "Gérer les disques"
description: "Cet article explique comment gérer les disques"
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: ae96071733b961fbe65551120894c21c633db83e
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="manage-disks"></a>Gérer les disques

> **S’applique à:** Windows10, Windows8.1, WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012

Cette rubrique et ses sous-rubriques abordent l’utilisation de l’outil Gestion des disques pour gérer les disques d’un ordinateur. Elles incluent également des informations sur la conversion de disques entre différents styles de partition et sur la façon dont Windows gère l’état en ligne de nouveaux disques.

## <a name="converting-disk-types"></a>Conversion de types de disque

Bien que l’outil Gestion des disques vous permette d’appliquer différents types et styles de partition aux disques, certaines opérations sont irréversibles (sauf si vous reformatez le disque). Vous devez sélectionner avec soin le type de disque et le style de partition les plus appropriés pour votre application.

Le tableau suivant présente les options de conversion de disques avec les différents styles de partition:

| Type de disque | Conversion en MBR  | Conversion en GPT| Conversion en disque dynamique |
| ---- | --- | --- |--- |
| <p>Enregistrement de démarrage principal (MBR)</p> | <p>--</p> | <p>Autorisé (si aucun volume sur le disque).</p> | <p>Autorisé, mais le disque risque de ne pas démarrer. Voir remarque.</p> |
| <p>Table de partition GUID (GPT)</p> | <p>Autorisé (si aucun volume sur le disque).</p> | <p>--</p>  | <p>Autorisé, mais le disque risque de ne pas démarrer. Voir remarque.</p> |


> [!NOTE]
> Dans un scénario à démarrages multiples, si vous avez démarré dans un système d’exploitation, et que vous convertissez un disque de base MBR qui contient un autre système d’exploitation en disque dynamique, vous ne serez plus en mesure de démarrer dans l’autre système d’exploitation.

## <a name="online-and-offline-status"></a>État en ligne/connexion

L’outil Gestion des disques affiche l’état en ligne et hors connexion des disques. 

Dans Windows, par défaut, tous les disques nouvellement détectés sont mis en ligne avec un accès en lecture et écriture. Dans WindowsServer, par défaut, les disques nouvellement détectés sont mis en ligne avec un accès en lecture et écriture, sauf s’ils se trouvent sur un bus partagé (par exemple, SCSI, iSCSI, Serial Attached SCSI ou Fibre Channel). Les disques qui se trouvent sur un bus partagé seront hors connexion la première fois qu’ils sont détectés.

Si un disque est hors connexion, vous devez le mettre en ligne avant de l’initialiser ou de créer des volumes sur celui-ci. 

-  Pour mettre un disque en ligne ou hors connexion, cliquez avec le bouton droit sur son nom, puis choisissez l’action appropriée.

## <a name="see-also"></a>Articles associés

-   [Initialiser les nouveaux disques](initialize-new-disks.md)
-   [Déplacer des disques vers un autre ordinateur](move-disks-to-another-computer.md)
-   [Convertir un disque dynamique en disque de base](change-a-dynamic-disk-back-to-a-basic-disk.md)
-   [Convertir un disque d’enregistrement de démarrage principal en disque de table de partition GUID](change-an-mbr-disk-into-a-gpt-disk.md)
-   [Convertir un disque de table de partition GUID en disque d’enregistrement de démarrage principal](change-a-gpt-disk-into-an-mbr-disk.md)
-   [Gérer les disques durs virtuels](manage-virtual-hard-disks.md)