---
title: Changer une lettre de lecteur
description: Comment modifier ou affecter une lettre de lecteur dans Windows à l’aide de la gestion des disques.
ms.date: 10/24/2018
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: b972ab05c192dca9a9a0a2bda4f083d2906acadb
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "66812482"
---
# <a name="change-a-drive-letter"></a>Changer une lettre de lecteur

> **S’applique à :** Windows 10, Windows 8.1, Windows 7, Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si vous n’aimez pas la lettre de lecteur affectée à un lecteur, ou si vous avez un lecteur qui ne dispose pas encore d’une lettre de lecteur, vous pouvez utiliser la gestion des disques pour la modifier.

> [!IMPORTANT]
> Si vous modifiez la lettre de lecteur d’un lecteur au sein duquel Windows ou des applications sont installés, l’exécution des applications ou la découverte du lecteur peut poser problème. Pour cette raison, nous vous suggérons de ne pas modifier la lettre du lecteur sur lequel Windows ou des applications sont installés.

Voici comment modifier la lettre du lecteur (pour monter le lecteur dans un dossier vide afin qu’il apparaisse simplement comme un autre dossier, consultez [Attribuer un chemin d’accès de dossier de point de montage à un lecteur](assign-a-mount-point-folder-path-to-a-drive.md)).

1. Ouvrez la fonctionnalité de gestion des disques avec les autorisations d’administrateur. 
    Pour ce faire, dans la zone de recherche de la barre des tâches, tapez **Gestion des disques**, sélectionnez **Gestion des disques** et maintenez la sélection (ou faites un clic droit), puis sélectionnez **Exécuter en tant qu’administrateur** > **Oui**. Si vous ne pouvez pas ouvrir la fonctionnalité en tant qu’administrateur, tapez **Gestion de l’ordinateur**, puis accédez à **Stockage** > **Gestion des disques**.
1. Dans Gestion des disques, faites un clic droit sur le lecteur pour lequel vous souhaitez modifier ou ajouter une lettre de lecteur, puis sélectionnez **Modifier la lettre de lecteur et les chemins d’accès**.

    ![Gestion des disques affichant un lecteur](media/change-drive-letter.png)
    > [!TIP]
    > Si vous ne voyez pas l’option **Modifier la lettre de lecteur et les chemins d’accès** ou si elle est grisée, il est possible le volume ne soit pas prêt à recevoir une lettre de lecteur, par exemple s’il n’est pas alloué et s’il doit être [initialisé](initialize-new-disks.md). Ou bien, il n’est peut-être pas censé être accessible, ce qui est le cas des partitions système EFI et des partitions de récupération. Si vous avez vérifié qu’il s’agit d’un volume formaté avec une lettre de lecteur à laquelle vous pouvez accéder et que vous ne pouvez pas la modifier, cette rubrique ne peut malheureusement probablement pas vous aider, nous vous suggérons de [contacter Microsoft](https://support.microsoft.com/contactus/) ou le fabricant de votre PC pour obtenir une aide supplémentaire.

1. Pour modifier la lettre de lecteur, sélectionnez **Modifier**. Pour ajouter une lettre de lecteur si le lecteur n’en possède pas déjà, sélectionnez **Ajouter**.

    ![La boîte de dialogue Modifier la lettre de lecteur et les chemins d’accès](media/change-drive-letter2.png)
1. Sélectionnez la nouvelle lettre de lecteur, puis **OK** et **Oui** lorsque vous êtes informé que les programmes s’appuyant sur la lettre de lecteur peuvent ne pas fonctionner correctement.

    ![La boîte de dialogue Modifier la lettre de lecteur et les chemins d’accès indiquant la modification de la lettre de lecteur](media/change-drive-letter3.png)