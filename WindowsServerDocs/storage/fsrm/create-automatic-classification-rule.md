---
title: "Créer une règle de classification automatique"
description: "Cet article explique comment créer une règle de classification pour une propriété."
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c472949228184c6202681d257412c046bbc90d37
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="create-an-automatic-classification-rule"></a>Créer une règle de classification automatique

> S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2

La procédure suivante vous guide tout au long du processus de création d’une règle de classification. Chaque règle définit la valeur d’une propriété unique. Par défaut, une règle est exécutée une seule fois et ignore les fichiers auxquels une valeur de propriété a déjà été affectée. Toutefois, vous pouvez configurer une règle pour évaluer les fichiers, qu'une valeur soit déjà affectée ou non à la propriété.

## <a name="to-create-a-classification-rule"></a>Pour créer une règle de classification

1.  Dans **Gestion de la classification**, cliquez sur le nœud **Règles de classification**.

2.  Cliquez avec le bouton droit sur **Règles de classification**, puis cliquez sur **Créer une règle** (ou cliquez sur **Créer une règle** dans le volet **Actions**). La boîte de dialogue **Définitions de règle de la classification** s’affiche.

3.  Sous l’onglet **Paramètres de la règle**, entrez les informations suivantes:

    -   **Nom de la règle**. Donnez un nom à la règle.
    -   **Activée**. Cette règle ne sera appliquée que si la case à cocher Activée est activée. Pour désactiver la règle, désactivez cette case à cocher.
    -   **Description**. Tapez une description facultative de cette règle.
    -   **Étendue**. Cliquez sur **Ajouter** pour sélectionner l'emplacement auquel cette règle s’applique. Vous pouvez ajouter plusieurs emplacements ou supprimer un emplacement en cliquant sur **Supprimer**. La règle de classification s’appliquera à tous les dossiers et sous-dossiers de cette liste.

4.  Sous l’onglet **Classification**, entrez les informations suivantes:

    -   **Mécanisme de classification**. Choisissez une méthode d’affectation de la valeur de propriété.
    -   **Nom de la propriété**. Sélectionnez la propriété que cette règle affectera.
    -   **Valeur de la propriété**. Sélectionnez la valeur de propriété que cette règle affectera.

5.  Si vous le souhaitez, cliquez sur le bouton **Avancé** pour sélectionner d'autres options. Sous l'onglet **Type d’évaluation**, la case à cocher **Réévaluer les fichiers** est désactivée par défaut. Les options qui peuvent être sélectionnées ici sont les suivantes:

    -   **Réévaluer les fichiers** désactivé: une règle est appliquée à un fichier si et seulement si la propriété spécifiée par la règle n’a pas été définie à n’importe quelle valeur du fichier.
    -   **Réévaluer les fichiers** activé et l'option **Remplacer la valeur existante** sélectionnée: la règle doit être appliquée aux fichiers chaque fois que le processus de classification automatique s’exécute. Par exemple, si un fichier a une propriété booléenne définie sur **Oui**, une règle qui utilise le classificateur de dossiers pour définir tous les fichiers sur **Non** avec cette option définie laisse la propriété définie sur **Non**.
    -   **Réévaluer les fichiers** activé et l'option **Agréger les valeurs** sélectionnée: la règle doit être appliquée aux fichiers chaque fois que le processus de classification automatique s’exécute. Toutefois, lorsque la règle a déterminé la valeur à définir pour le fichier de propriété, elle agrège cette valeur à celle déjà présente dans le fichier. Par exemple, si un fichier a une propriété booléenne définie sur **Oui**, une règle qui utilise le classificateur de dossiers pour définir tous les fichiers sur **Non** avec cette option définie laisse la propriété définie sur **Oui**.

    Sous l'onglet **Paramètres de classification supplémentaires**, vous pouvez spécifier d'autres paramètres qui sont reconnus par la méthode de classification sélectionnée en entrant le nom et la valeur et en cliquant sur le bouton **Insérer**.

6.  Cliquez sur **OK** ou sur **Annuler** pour fermer la boîte de dialogue **Avancé**.

7.  Cliquez sur **OK**.

## <a name="see-also"></a>Articles associés

-   [Créer une propriété de classification](create-classification-property.md)
-   [Gestion de la classification](classification-management.md)