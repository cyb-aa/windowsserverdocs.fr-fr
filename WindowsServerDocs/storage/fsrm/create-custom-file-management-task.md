---
title: Créer une tâche de gestion de fichiers personnalisée
description: Cet article explique comment créer une tâche de gestion de fichiers personnalisée, ainsi que des tâches personnalisées.
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: dd52a94657fb73d28b3bc1552a058b7f3ca954ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867260"
---
# <a name="create-a-custom-file-management-task"></a>Créer une tâche de gestion de fichiers personnalisée

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

L'expiration n’est pas toujours une action souhaitée à effectuer sur les fichiers. Les tâches de gestion de fichiers permettent également d’exécuter des commandes personnalisées.

> [!Note]
> Cette procédure suppose que vous êtes familiarisé avec les tâches de gestion de fichiers et ne couvre donc que l'onglet **Action** où les paramètres personnalisés sont configurés.

## <a name="to-create-a-custom-task"></a>Pour créer une tâche personnalisée

1.  Cliquez sur le nœud **Tâches de gestion de fichiers**.

2.  Cliquez avec le bouton droit sur **Tâches de gestion de fichiers**, puis cliquez sur **Créer une tâche de gestion de fichiers** (ou cliquez sur **Créer une tâche de gestion de fichiers** dans le volet **Actions**). La boîte de dialogue **Créer une tâche de gestion de fichiers** s’affiche.

3.  Sous l’onglet **Action**, entrez les informations suivantes :

    -   **Type**. Sélectionnez **Personnalisée** dans le menu déroulant.
    -   **Exécutable**. Tapez ou sélectionnez une commande à exécuter lorsque la tâche de gestion de fichiers traite les fichiers. Cet exécutable doit être configuré comme accessible en écriture uniquement par les administrateurs et le système. Si d’autres utilisateurs disposent d'un accès en écriture à l'exécutable, celui-ci ne s’exécutera pas correctement.
    -   **Paramètres de commande**. Pour configurer les arguments passés à l’exécutable lorsqu’une tâche de gestion de fichiers traite les fichiers, modifiez la zone de texte intitulée **Arguments**. Pour insérer des variables supplémentaires dans le texte, placez le curseur dans la zone de texte à l'endroit où vous souhaitez insérer la variable, sélectionnez la variable à insérer, puis cliquez sur **Insérer une variable**. Le texte entre crochets insère les informations de variables que l'exécutable peut recevoir. Par exemple, le \[chemin du fichier Source\] variable insère le nom du fichier qui doit être traité par l’exécutable. Si vous le souhaitez, cliquez sur le bouton **Répertoire de travail** pour spécifier l’emplacement de l'exécutable personnalisé.
    -   **Sécurité de la commande**. Configurez les paramètres de sécurité pour cet exécutable. Par défaut, la commande s'exécute en tant que Service local, qui est le compte le plus restrictif disponible.

4.  Cliquez sur **OK**.

## <a name="see-also"></a>Voir aussi

-   [Gestion de la classification](classification-management.md)
-   [Tâches de gestion de fichiers](file-management-tasks.md)