---
title: 'Étape 3 : Approuver et déployer des mises à jour dans WSUS'
description: Rubrique Windows Server Update Services (WSUS) – Approuver et déployer des mises à jour dans WSUS est la troisième étape du processus de déploiement de WSUS, qui en compte quatre
ms.prod: windows-server
ms.reviewer: na
ms.technology: manage-wsus
ms.topic: article
ms.assetid: 8d728ff9-170f-47e6-aefe-52be93315a75
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7731cc84f946bfab7f53a3446ed90d1be92cae75
ms.sourcegitcommit: 3c3dfee8ada0083f97a58997d22d218a5d73b9c4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80639798"
---
# <a name="step-3-approve-and-deploy-updates-in-wsus"></a>Étape 3 : Approuver et déployer des mises à jour dans WSUS

>S'applique à : Windows Server 2019, Windows Server (Canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Les ordinateurs d’un groupe d’ordinateurs contactent automatiquement le serveur WSUS dans les 24 heures suivantes pour obtenir des mises à jour. Vous pouvez utiliser la fonctionnalité de création de rapports WSUS pour déterminer si ces mises à jour ont été déployées sur les ordinateurs de test. Lorsque les tests sont correctement effectués, vous pouvez approuver les mises à jour pour les groupes d’ordinateurs applicables dans votre organisation. La liste de contrôle suivante décrit les étapes de l’approbation et du déploiement des mises à jour à l’aide de la console de gestion des services WSUS.

|Tâche|Description|
|----|--------|
|[3.1. Approuver et déployer des mises à jour WSUS](3-approve-and-deploy-updates-in-wsus.md#BKM_3.1.)|Approuvez et déployez des mises à jour WSUS à l’aide de la console de gestion des services WSUS.|
|[3.2. Configurer les règles d’approbation automatique](3-approve-and-deploy-updates-in-wsus.md#BKM_3.2.a.)|Configurez WSUS pour approuver automatiquement l'installation des mises à jour pour les groupes sélectionnés et pour déterminer comment approuver les révisions des mises à jour existantes.|
|[3.3. Passer en revue les mises à jour installées avec les rapports WSUS](3-approve-and-deploy-updates-in-wsus.md#BKM_3.3.)|Passez en revue les mises à jour qui ont été installées, les ordinateurs qui ont reçu ces mises à jour et d’autres informations à l’aide de la fonctionnalité de création de rapports WSUS.|

## <a name="31-approve-and-deploy-wsus-updates"></a><a name="BKM_3.1."></a>3.1. Approuver et déployer des mises à jour WSUS
Utilisez la procédure suivante pour approuver et déployer des mises à jour.

#### <a name="to-approve-and-deploy-wsus-updates"></a>Pour approuver et déployer des mises à jour WSUS

1.  Dans la console d’administration WSUS, cliquez sur **Mises à jour**. Dans le volet droit, une synthèse de l’état des mises à jour est affichée pour **Toutes les mises à jour**, **Mises à jour critiques**, **Mises à jour de sécurité**et **Mises à jour WSUS**.

2.  Dans la section **Toutes les mises à jour** , cliquez sur **Mises à jour requises par des ordinateurs**.

3.  Dans la liste des mises à jour, sélectionnez celles à approuver pour une installation dans votre groupe d’ordinateurs de test. Les informations relatives à une mise à jour sélectionnée sont disponibles dans le volet inférieur du panneau **Mises à jour** . Pour sélectionner plusieurs mises à jour contiguës, maintenez la touche **Maj** enfoncée tout en cliquant sur les noms des mises à jour. Pour sélectionner plusieurs mises à jour non contiguës, appuyez sur la touche **CTRL** tout en cliquant sur les noms des mises à jour.

4.  Cliquez avec le bouton droit sur la sélection, puis cliquez sur **Approuver**.

5.  Dans la boîte de dialogue **Approuver les mises à jour** , sélectionnez votre groupe de test, puis cliquez sur la flèche vers le bas.

6.  Cliquez sur **Approuvée pour l’installation**, puis sur **OK**.

7.  La fenêtre **Progression de l’approbation** s’affiche. Elle indique la progression des tâches qui affectent l’approbation des mises à jour. Une fois le processus d’approbation terminé, cliquez sur **Fermer**.

## <a name="32-configure-auto-approval-rules"></a><a name="BKM_3.2.a."></a>3.2. Configurer les règles d'approbation automatique
Approbations automatiques vous permet de déterminer comment approuver automatiquement l'installation des mises à jour pour les groupes sélectionnés et comment approuver les révisions des mises à jour existantes.

#### <a name="to-configure-automatic-approvals"></a>Pour configurer Approbations automatiques

1.  Dans la console d'administration WSUS, sous **Update Services**, développez le serveur WSUS, puis cliquez sur **Options**. La fenêtre **Options** s'ouvre.

2.  Dans **Options**, cliquez sur **Approbations automatiques**. La boîte de dialogue Approbations automatiques s'ouvre.

3.  Dans **Règles de mise à jour**, cliquez sur **Nouvelle règle**. La boîte de dialogue **Ajouter une règle** s’ouvre.

4.  Dans **Ajouter une règle**, à l’ **Étape 1 : Sélectionner des propriétés**, sélectionnez une ou plusieurs options dans la liste suivante :

    -   **Lorsqu’une mise à jour se trouve dans une classification précise**

    -   **Lorsqu’une mise à jour se trouve dans un produit précis**

    -   **Définir un délai pour l'approbation**

5.  À l’ **Étape 2 : Modifier les propriétés**, cliquez sur chacune des options répertoriées, puis sélectionnez les options appropriées pour chacune d’elles.

6.  À l’**Étape 3 : Spécifier un nom**, tapez le nom de votre règle, puis cliquez sur **OK**.

7.  Cliquez sur **OK** pour fermer la boîte de dialogue Approbations automatiques.

## <a name="33-review-installed-updates-with-wsus-reports"></a><a name="BKM_3.3."></a>3.3. Passer en revue les mises à jour installées avec les rapports WSUS
24 heures après avoir approuvé les mises à jour, vous pouvez utiliser la fonctionnalité de création de rapports WSUS pour déterminer si les mises à jour ont été déployées sur les ordinateurs du groupe de test. Pour vérifier l’état d’une mise à jour, vous pouvez utiliser la fonctionnalité de création de rapports WSUS comme suit.

#### <a name="to-review-updates"></a>Pour passer en revue les mises à jour

1.  Dans le volet de navigation de la console d’administration WSUS, cliquez sur **Rapports**.

2.  Dans la page **Rapports** , cliquez sur le rapport **Synthèse de l’état des mises à jour** . La fenêtre du **rapport relatif aux mises à jour** s’affiche.

3.  Si vous voulez filtrer la liste des mises à jour, sélectionnez les critères à utiliser, par exemple **Inclure les mises à jour dans ces classifications**, puis cliquez sur **Exécuter le rapport**.

4.  Le volet du **rapport relatif aux mises à jour** s’affiche. Vous pouvez vérifier l’état de mises à jour individuelles en sélectionnant la mise à jour dans la section gauche du volet. La dernière section du volet de rapport indique la synthèse de l’état de la mise à jour.

5.  Vous pouvez enregistrer ou imprimer ce rapport en cliquant sur l’icône appropriée de la barre d’outils.

6.  Après avoir testé les mises à jour, vous pouvez les approuver pour une installation dans les groupes d’ordinateurs applicables de votre organisation.
