---
title: "Créer une exception au filtre de fichiers"
description: "Cet article explique comment créer une exception au filtre de fichiers"
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 1f0e93cb2535862b9259d438de00c3b769c2282c
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="create-a-file-screen-exception"></a>Créer une exception au filtre de fichiers

> S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2

Vous devez parfois autoriser des exceptions au filtrage des fichiers. Par exemple, vous voudriez bloquer des fichiers vidéos à partir d’un serveur de fichiers, mais vous devez autoriser votre groupe de formation à enregistrer les fichiers vidéo pour leur formation assistée par ordinateur. Pour autoriser les fichiers bloqués par les autres filtres de fichiers, créez une *exception au filtre de fichiers*.

Une exception au filtre de fichiers est un type spécial de filtre de fichiers qui remplace tout filtrage de fichiers qui autrement s’appliquerait à un dossier et à tous ses sous-dossiers dans un chemin d’accès spécifique. Autrement dit, il crée une exception à toutes les règles dérivées d’un dossier parent.

> [!Note]
> Vous ne pouvez pas créer une exception au filtre de fichiers sur un dossier parent dans lequel un filtre de fichiers est déjà défini. Vous devez attribuer l’exception à un sous-dossier ou modifier le filtre de fichiers existant.

<br />
Vous affectez des groupes de fichiers pour déterminer les types de fichiers autorisés dans l’exception au filtre de fichiers.

## <a name="to-create-a-file-screen-exception"></a>Pour créer une exception au filtre de fichiers

1.  Dans **Gestion du filtrage de fichiers**, cliquez sur le nœud **Filtres de fichiers**.

2.  Cliquez avec le bouton droit sur **Filtres de fichiers**, puis cliquez sur **Créer une exception de filtre de fichiers** (ou sélectionnez **Créer une exception de filtre de fichiers** à partir du volet **Actions**). La boîte de dialogue **Créer une exception de filtre de fichiers** s’affiche.

3.  Dans la zone de texte **Chemin d’accès de l’exception**, tapez ou sélectionnez le chemin d’accès auquel l’exception s'applique. L’exception s’applique au dossier sélectionné et à tous ses sous-dossiers.

4.  Pour spécifier les fichiers à exclure du filtrage de fichiers:

    -   Sous **Groupes de fichiers**, sélectionnez chaque groupe de fichiers que vous souhaitez exclure du filtrage de fichiers. (Pour activer la case à cocher du groupe de fichiers, double-cliquez sur le nom du groupe.)
    -   Si vous souhaitez afficher les types de fichiers inclus et exclus par un groupe de fichiers, cliquez sur le nom du groupe, puis cliquez sur **Modifier**.
    -   Pour créer un groupe de fichiers, cliquez sur **Créer**.

5.  Cliquez sur **OK**.

## <a name="see-also"></a>Articles associés

-   [Gestion du filtrage de fichiers](file-screening-management.md)
-   [Définir des groupes de fichiers pour le filtrage](define-file-groups-for-screening.md)


