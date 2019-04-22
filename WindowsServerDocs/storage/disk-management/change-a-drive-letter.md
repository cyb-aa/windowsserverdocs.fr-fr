---
title: Modifier une lettre de lecteur
description: Comment modifier ou affecter une lettre de lecteur dans Windows à l’aide de gestion des disques.
ms.date: 10/24/2018
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 5f25d49ac399633c048b0c8581551d862145ca76
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812160"
---
# <a name="change-a-drive-letter"></a>Modifier une lettre de lecteur

> **S’applique à :** Windows 10, Windows 8.1, Windows 7, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si vous n’aimez pas la lettre de lecteur affectée à un lecteur, ou si vous avez un lecteur qui ne dispose pas encore une lettre de lecteur, vous pouvez utiliser la gestion des disques pour le modifier.

> [!IMPORTANT]
> Si vous modifiez la lettre de lecteur d’un lecteur dans lequel Windows ou des applications sont installées, les applications peuvent poser un problème en cours d’exécution ou la recherche de ce lecteur. Pour cette raison, nous vous suggérons de ne pas modifier la lettre de lecteur d’un lecteur sur lequel sont installées Windows ou des applications.

Voici comment modifier la lettre de lecteur (pour à la place pour monter le lecteur dans vide dossier afin qu’il apparaisse en tant que juste un autre dossier, consultez [affecter un chemin d’accès du dossier de point de montage à un lecteur](assign-a-mount-point-folder-path-to-a-drive.md)).

1. Ouvrez Gestion des disques avec les autorisations d’administrateur. <br>Pour ce faire, dans la zone de recherche dans la barre des tâches, tapez **gestion des disques**, sélectionnez et maintenez (ou avec le bouton droit) **gestion des disques**, puis sélectionnez **exécuter en tant qu’administrateur**  >  **Oui**. Si vous ne pouvez pas l’ouvrir en tant qu’administrateur, tapez **gestion de l’ordinateur** au lieu de cela, puis accédez à **stockage** > **gestion des disques**.
1. Dans Gestion des disques, cliquez sur le lecteur pour lequel vous souhaitez modifier ou ajouter une lettre de lecteur, puis sélectionnez **modifier la lettre de lecteur et les chemins d’accès**.<br>
![Gestion des disques affichant un lecteur](media/change-drive-letter.png)
    > [!TIP]
    > Si vous ne voyez pas le **modifier la lettre de lecteur et les chemins d’accès** option ou si elle est grisée, il est possible le volume n’est pas prêt à recevoir une lettre de lecteur, ce qui peut être le cas si le disque non alloué et doit être [initialisé](initialize-new-disks.md). Ou bien, peut-être qu’il n'a pas censés être accessibles, ce qui est le cas des partitions de système EFI et des partitions de récupération. Si vous avez vérifié que vous avez un volume formaté avec une lettre de lecteur que vous pouvez accéder à et que vous ne pouvez pas modifier, malheureusement cette rubrique probablement ne peut pas vous aider, donc nous vous suggérons [contactant Microsoft](https://support.microsoft.com/contactus/) ou le fabricant de votre PC de l’aide.

1. Pour modifier la lettre de lecteur, sélectionnez **modifier**. Pour ajouter une lettre de lecteur si le lecteur ne possède pas déjà, sélectionnez **ajouter**.<br>![La boîte de dialogue Modifier la lettre de lecteur et les chemins d’accès](media/change-drive-letter2.png)
3. Sélectionnez la nouvelle lettre de lecteur, sélectionnez **OK**, puis sélectionnez **Oui** lorsque vous y êtes invité sur comment les programmes qui s’appuient sur la lettre de lecteur peuvent ne pas fonctionnement correctement.<br>![La boîte de dialogue Modifier la lettre de lecteur ou chemin d’accès indiquant la modification de la lettre de lecteur](media/change-drive-letter3.png)