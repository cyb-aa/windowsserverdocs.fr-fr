---
title: Créer une tâche d'expiration de fichiers
description: Cet article décrit le processus de création d’une tâche de gestion des fichiers sur le point d’expirer
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 0901c17203252414a37ccc5205a0946b8bef0d41
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394228"
---
# <a name="create-a-file-expiration-task"></a>Créer une tâche d'expiration de fichiers

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

La procédure suivante vous guide tout au long du processus de création d’une tâche de gestion des fichiers arrivant à expiration. Les tâches d’expiration de fichiers sont utilisées pour déplacer automatiquement tous les fichiers répondant à certains critères vers un répertoire d’expiration spécifié, dans lequel un administrateur peut les sauvegarder et les supprimer.

Lorsqu’une tâche d’expiration de fichiers est exécutée, un répertoire est créé dans le répertoire d’expiration, regroupé par le nom du serveur sur lequel la tâche a été exécutée.

Le nom du nouveau répertoire est basé sur le nom de la tâche de gestion de fichiers et l'heure à laquelle elle a été exécutée. Lorsqu’un fichier arrivé à expiration est trouvé, il est déplacé dans le nouveau répertoire, tout en conservant sa structure de répertoire d’origine.

## <a name="to-create-a-file-expiration-task"></a>Pour créer une tâche d'expiration de fichiers

1. Cliquez sur le nœud **Tâches de gestion de fichiers**.

2. Cliquez avec le bouton droit sur **Tâches de gestion de fichiers**, puis cliquez sur **Créer une tâche de gestion de fichiers** (ou cliquez sur **Créer une tâche de gestion de fichiers** dans le volet **Actions**). La boîte de dialogue **Créer une tâche de gestion de fichiers** s’affiche.

3. Sous l’onglet **Général**, entrez les informations suivantes :

   -   **Nom**. Entrez un nom pour la nouvelle tâche.  

   -   **Description**. Entrez une description facultative de cette tâche.  
    
   -   **Étendue**. Ajoutez les répertoires sur lesquels cette tâche doit s'appliquer à l'aide du bouton **Ajouter**. Vous pouvez éventuellement supprimer des répertoires de la liste à l’aide du bouton **Supprimer**. La tâche de gestion de fichiers s’appliquera à tous les dossiers et sous-dossiers de cette liste.

4. Sous l’onglet **Action**, entrez les informations suivantes :

   - **Type**. Sélectionnez **Expiration de fichier** dans le menu déroulant.

   - **Répertoire d’expiration**. Sélectionnez un répertoire dans lequel les fichiers seront expirés.

     > [!Warning]
     > Ne sélectionnez pas un répertoire se trouvant dans l’étendue de la tâche, comme définie à l’étape précédente. En effet, cela risquerait de provoquer une boucle itérative qui pourrait conduire à une instabilité du système et à la perte de données.

5. Si vous le souhaitez, sous l’onglet **Notification**, cliquez sur **Ajouter** pour envoyer des notifications par courrier électronique, enregistrer un événement dans le journal ou exécuter une commande ou un script un certain nombre de jours avant que la tâche ne réalise une action sur un fichier.

   - Dans la zone de liste déroulante **Nombre de jours avant l’exécution de la tâche pour l’envoi d’une notification**, tapez ou sélectionnez une valeur pour spécifier le nombre de jours minimal avant le traitement d’un fichier auquel une notification sera envoyée.

     > [!Note]
     > Les notifications sont envoyées uniquement lorsque vous exécutez une tâche. Si le nombre de jours minimal spécifié pour envoyer une notification ne correspond pas à une tâche planifiée, la notification sera envoyée à la date de la tâche planifiée précédente.

   - Pour configurer les notifications par courrier électronique, cliquez sur l'onglet **Message électronique** et entrez les informations suivantes :

     - Pour avertir les administrateurs qu’un seuil a été atteint, cochez la case **Envoyer un courrier électronique aux administrateurs suivants**, puis entrez les noms des comptes d’administration qui recevront les notifications. Utilisez le format <em>account@domain</em> et séparez les différents comptes par des points-virgules.  

     - Pour envoyer un courrier électronique à la personne dont les fichiers vont expirer, cochez la case **Envoyer un message électronique à l’utilisateur dont les fichiers vont expirer**.

     - Pour configurer le message, modifiez le contenu par défaut de la ligne d'objet et du corps du message. Le texte entre crochets insère les informations de variables sur l’événement de quota qui a provoqué la notification. Par exemple, la variable **\[propriétaire du fichier Source\]** insère le nom de l’utilisateur dont le fichier est sur le paragraphe de l’expiration. Pour insérer d'autres variables dans le texte, cliquez sur **Insérer une variable**.

     - Pour joindre une liste des fichiers qui vont expirer, cliquez sur **Joindre aux messages électroniques la liste des fichiers sur lesquels une action sera effectuée**, puis tapez ou sélectionnez une valeur pour **Nombre maximal de fichiers dans la liste**.

     - Pour configurer des en-têtes supplémentaires (notamment De, Cc, Cci et Répondre), cliquez sur **Autres en-têtes de courrier électronique**.  

   - Pour consigner un événement, cliquez sur l'onglet **Journal des événements**, cochez la case **Envoyer un avertissement au journal des événements**, puis modifiez l’entrée de journal par défaut.  

   - Pour exécuter une commande ou un script, cliquez sur l'onglet **Commande** et cochez la case **Exécuter cette commande ou ce script**. Puis tapez la commande, ou cliquez sur **Parcourir** pour rechercher l’emplacement où le script est stocké. Vous pouvez également entrer des arguments de commande, sélectionner un répertoire de travail pour la commande ou le script, ou modifier le paramètre de sécurité de la commande.

6. Vous pouvez éventuellement utiliser l’onglet **Rapport** pour générer un ou plusieurs journaux ou rapports de stockage.

   - Pour générer des journaux, cochez la case **Générer un journal**, puis sélectionnez un ou plusieurs journaux disponibles.  

   - Pour générer des rapports, cochez la case **Générer un rapport**, puis sélectionnez un ou plusieurs formats de rapport disponibles.  

   - Pour créer des journaux ou des rapports de stockage générés par courrier électronique, cochez la case **Envoyer des rapports aux administrateurs suivants** et tapez un ou plusieurs destinataires de messagerie administrative en utilisant le format <em>account@domain</em>. Séparez les différentes adresses par des points-virgules.

     > [!Note]
     > Le rapport est enregistré à l’emplacement par défaut des rapports d’incidents. Vous pouvez modifier cet emplacement dans la boîte de dialogue **Options du Gestionnaire de ressources du serveur de fichiers**.
        
7. Vous avez la possibilité d’utiliser l’onglet **Condition** pour n’exécuter cette tâche que sur les fichiers répondant à un ensemble défini de conditions. Les options suivantes sont disponibles :

    -   **Conditions de propriété**. Cliquez sur **Ajouter** pour créer une condition reposant sur la classification du fichier. Cette action ouvre la boîte de dialogue **Condition de propriété** qui permet de sélectionner une propriété, un opérateur à appliquer à la propriété et la valeur à comparer avec la propriété. Après avoir cliqué sur **OK**, vous pouvez créer des conditions supplémentaires ou modifier ou supprimer une condition existante.

    -   **Nombre de jours depuis la dernière modification au fichier**. Cliquez sur la case à cocher, puis entrez un nombre de jours dans la zone de sélection numérique. La tâche de gestion de fichiers ne s’applique alors qu'aux fichiers qui n’ont pas été modifiés depuis plus du nombre de jours spécifié.

    -   **Nombre de jours depuis le dernier accès au fichier**. Cliquez sur la case à cocher, puis entrez un nombre de jours dans la zone de sélection numérique. Si le serveur est configuré pour suivre l'heure du dernier accès aux fichiers, la tâche de gestion de fichiers ne s’applique qu'aux fichiers qui n’ont pas été consultés depuis plus du nombre de jours spécifié. Si le serveur n’est pas configuré pour suivre les heures d’accès, cette condition sera inefficace.

    -   **Nombre de jours depuis la création du fichier**. Cliquez sur la case à cocher, puis entrez un nombre de jours dans la zone de sélection numérique. La tâche ne s’applique alors qu'aux fichiers qui ont été créés au moins depuis le nombre de jours spécifié.  

    -   **En vigueur à partir de**. Définissez une date à laquelle cette tâche de gestion de fichiers doit commencer à traiter les fichiers. Cette option est utile pour retarder la tâche et vous laisser ainsi le temps d'avertir les utilisateurs ou d'effectuer d’autres préparatifs à l'avance.

8. Sous l'onglet **Planification**, cliquez sur **Créer une planification**, puis, dans la boîte de dialogue **Planification**, cliquez sur **Nouvelle**. Une planification par défaut s'affiche, configurée à 9 h 00 tous les jours, que vous pouvez modifier. Lorsque vous avez terminé de configurer la planification, cliquez sur **OK**, puis sur **OK** à nouveau.

## <a name="see-also"></a>Voir également

-   [Gestion de la classification](classification-management.md)
-   [Tâches de gestion de fichiers](file-management-tasks.md)