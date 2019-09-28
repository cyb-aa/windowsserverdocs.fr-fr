---
title: Planifier un ensemble de rapports
description: Cet article explique comment générer un ensemble de rapports à intervalles réguliers
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: b718ceed7a378649c51e1ca64bffaaddf051c292
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394057"
---
# <a name="schedule-a-set-of-reports"></a>Planifier un ensemble de rapports

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Pour générer un ensemble de rapports à intervalles réguliers, vous devez planifier une *tâche de rapport.* La tâche de rapport spécifie les rapports à générer et les paramètres à utiliser ; les volumes et les dossiers concernés par le rapport ; la fréquence de génération des rapports et les formats d'enregistrement des fichiers de rapport.

Les rapports planifiés sont enregistrés dans un emplacement par défaut que vous pouvez spécifier dans la boîte de dialogue **Options du Gestionnaire de ressources du serveur de fichiers**. Vous avez également la possibilité d’envoyer les rapports par courrier électronique à un groupe d’administrateurs.

> [!Note]
> Pour minimiser l’impact du traitement des rapports sur les performances, générez plusieurs rapports en même temps pour que les données ne soient collectées qu'une seule fois. Pour ajouter rapidement des rapports à des tâches existantes, vous pouvez utiliser l'action **Ajouter ou supprimer des rapports pour une tâche de rapport**. Cela permet d'ajouter ou de supprimer des rapports dans plusieurs tâches de rapports et de modifier les paramètres de rapport. Pour modifier les planifications ou les adresses d'expédition, vous devez modifier chaque tâche.

## <a name="to-schedule-a-report-task"></a>Pour planifier une tâche de rapport

1. Cliquez sur le nœud **Gestion des rapports de stockage**.

2. Cliquez avec le bouton droit sur **Gestion des rapports de stockage**, puis cliquez sur **Planifier une nouvelle tâche de rapport** (ou sélectionnez **Planifier une nouvelle tâche de rapport** à partir du volet **Actions**). La boîte de dialogue **Propriétés des tâches de rapports de stockage** s'affiche.

3. Pour sélectionner les volumes ou les dossiers sur lesquels vous voulez générer des rapports :

   -   Sous **Étendue**, cliquez sur **Ajouter**.
   -   Recherchez le volume ou le dossier sur lequel vous souhaitez générer les rapports, sélectionnez-le, puis cliquez sur **OK** pour ajouter le chemin d’accès à la liste.
   -   Ajoutez autant de volumes ou de dossiers que vous souhaitez inclure dans les rapports. (Pour supprimer un volume ou un dossier, cliquez sur le chemin d’accès, puis cliquez sur **Supprimer**).

4. Pour spécifier les rapports à générer :

   -  Sous **Données de rapport**, sélectionnez chaque rapport à inclure. Par défaut, tous les rapports sont générés pour une tâche de rapport planifiée.

   Pour modifier les paramètres d’un rapport :

   -   Cliquez sur l’étiquette du rapport, puis sur **Modifier les paramètres**.
   -   Dans la boîte de dialogue **Paramètres du rapport**, modifiez les paramètres selon vos besoins, puis cliquez sur **OK**.

   -   Pour afficher la liste des paramètres de tous les rapports sélectionnés, cliquez sur **Consulter les rapports sélectionnés**. Cliquez ensuite sur **Fermer**.

5. Pour spécifier les formats d'enregistrement des rapports :

   -  Sous **Formats des rapports**, sélectionnez un ou plusieurs formats pour les rapports planifiés. Par défaut, les rapports sont générés en Dynamic HTML (DHTML). Vous pouvez également sélectionner les formats HTML, XML, CSV et texte. Les rapports sont enregistrés dans l’emplacement par défaut des rapports planifiés.

6. Pour envoyer aux administrateurs les copies des rapports par courrier électronique :

   - Sous l'onglet **Remise**, cochez la case **Envoyer des rapports aux administrateurs suivants**, puis entrez les noms des comptes administratifs qui recevront les rapports. 
   - Utilisez le format <em>account@domain</em> et séparez les différents comptes par des points-virgules.

7. Pour planifier les rapports :

   Sous l'onglet **Planification**, cliquez sur **Créer une planification**, puis, dans la boîte de dialogue **Planification**, cliquez sur **Nouvelle**. Une planification par défaut s'affiche, configurée à 9 h 00 tous les jours, que vous pouvez modifier.

   -   Pour spécifier une fréquence de génération des rapports, sélectionnez un intervalle dans la liste déroulante **Planifier une tâche**.
       Vous pouvez planifier des rapports quotidiens, hebdomadaires ou mensuels, ou ne générer les rapports qu’une seule fois. Vous pouvez également générer des rapports au démarrage du système ou à l'ouverture de session, ou si l’ordinateur a été inactif pendant une durée spécifiée.
   -   Pour fournir d'autres informations de planification pour l’intervalle choisi, modifiez ou définissez les valeurs dans les options **Planifier une tâche**.
       Ces options varient en fonction de l’intervalle que vous choisissez. Par exemple, pour un rapport hebdomadaire, vous pouvez spécifier le nombre de semaines entre les rapports et les jours de la semaine auxquels générer des rapports.
   -   Pour spécifier l’heure du jour à laquelle vous souhaitez générer le rapport, tapez ou sélectionnez la valeur dans la zone **Heure de début**.
   -   Pour accéder à des options de planification supplémentaires (notamment, la date de début et la date de fin de la tâche), cliquez sur **Avancé**.
   -   Pour enregistrer la planification, cliquez sur **OK**.
   -  Pour créer une planification supplémentaire pour une tâche (ou modifier une planification existante), sous l'onglet **Planification**, cliquez sur **Modifier la planification**.

8. Pour enregistrer la tâche de rapport, cliquez sur **OK**.

La tâche de rapport est ajoutée au nœud **Gestion des rapports de stockage**. Les tâches sont identifiées par les rapports à générer, l’espace de noms à étudier et la planification de rapport.

De plus, vous pouvez afficher l’état actuel du rapport (si le rapport est ou non en cours d’exécution), la date de la dernière exécution et son résultat, ainsi que la prochaine date d’exécution planifiée.

## <a name="see-also"></a>Voir aussi

-   [Gestion des rapports de stockage](storage-reports-management.md)
-   [Définition des options du Gestionnaire de ressources du serveur de fichiers](setting-file-server-resource-manager-options.md)


