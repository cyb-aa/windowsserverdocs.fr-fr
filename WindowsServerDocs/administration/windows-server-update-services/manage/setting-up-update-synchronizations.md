---
title: Configuration des synchronisations de mise à jour
description: Rubrique Windows Server Update Service (WSUS)-procédure d’installation et de configuration des synchronisations de mise à jour
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ddd5c395-451b-44a0-8e08-a05db26d2282
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9c7bca5be7a8ec0e857cba65680fbc3b967af4f8
ms.sourcegitcommit: 3c3dfee8ada0083f97a58997d22d218a5d73b9c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80639755"
---
# <a name="setting-up-update-synchronizations"></a>Configuration des synchronisations de mise à jour

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Au cours de la synchronisation, un serveur WSUS télécharge les mises à jour (mises à jour des métadonnées et des fichiers) à partir d’une source de mise à jour. Il télécharge également les nouvelles classifications et les catégories de produits, le cas échéant. Lorsque votre serveur WSUS se synchronise pour la première fois, il télécharge toutes les mises à jour que vous avez spécifiées lors de la configuration des options de synchronisation. Après la première synchronisation, votre serveur WSUS télécharge uniquement les mises à jour à partir de la source de mise à jour, ainsi que les révisions dans les métadonnées des mises à jour existantes et les expirations des mises à jour.

La première fois qu’un serveur WSUS télécharge des mises à jour peut prendre beaucoup de temps. Si vous configurez plusieurs serveurs WSUS, vous pouvez accélérer le processus dans une certaine mesure en téléchargeant toutes les mises à jour sur un serveur WSUS, puis en copiant les mises à jour dans les répertoires de contenu des autres serveurs WSUS.

Vous pouvez copier le contenu d’un répertoire de contenu du serveur WSUS vers un autre. L’emplacement du répertoire de contenu est spécifié lors de l’exécution de la procédure de publication WSUS. Vous pouvez utiliser l’outil WSUSutil. exe pour exporter des métadonnées de mise à jour d’un serveur WSUS vers un fichier. Vous pouvez ensuite importer ce fichier dans d’autres serveurs WSUS.

## <a name="setting-up-update-synchronizations"></a>Configuration des synchronisations de mise à jour
La page **options** est le point d’accès central dans la console d’administration WSUS pour personnaliser la façon dont votre serveur WSUS synchronise les mises à jour. Vous pouvez spécifier quelles mises à jour sont synchronisées automatiquement, où votre serveur obtient les mises à jour, les paramètres de connexion et le calendrier de synchronisation. Vous pouvez également utiliser l’Assistant Configuration de la page **options** pour configurer ou reconfigurer votre serveur WSUS à tout moment.

### <a name="synchronizing-update-by-product-and-classification"></a>Synchronisation des mises à jour par produit et classification
Un serveur WSUS télécharge les mises à jour en fonction des produits ou des familles de produits (par exemple, Windows ou Windows Server 2008, Datacenter Edition) et des classifications (par exemple, mises à jour critiques ou mises à jour de sécurité) que vous spécifiez. lors de la première synchronisation, le serveur WSUS télécharge toutes les mises à jour disponibles dans les catégories que vous avez spécifiées. Dans les synchronisations suivantes, votre serveur WSUS télécharge uniquement les mises à jour les plus récentes (ou les modifications apportées aux mises à jour déjà disponibles sur votre serveur WSUS) pour les catégories que vous avez spécifiées.

Vous pouvez spécifier des produits et des classifications de mise à jour dans la page **options** sous **produits et classifications**. Les produits sont répertoriés dans une hiérarchie, regroupés par famille de produits. Si vous sélectionnez Windows, vous sélectionnez automatiquement chaque produit qui se trouve sous cette hiérarchie de produits. En activant la case à cocher parent, vous sélectionnez tous les éléments sous celui-ci, ainsi que toutes les futures versions. Si vous activez les cases à cocher enfants, les cases à cocher parent ne sont pas activées. La valeur par défaut pour Products est tous les produits Windows, et le paramètre par défaut pour les classifications est critique et mises à jour de sécurité.

Si un serveur WSUS est exécuté en mode réplica, vous ne pourrez pas effectuer cette tâche. Pour plus d’informations sur le mode réplica, consultez [exécution du mode RÉPLICA WSUS](running-wsus-replica-mode.md)et [étape 1 : préparer votre déploiement WSUS](../plan/plan-your-wsus-deployment.md).

##### <a name="to-specify-update-products-and-classifications-for-synchronization"></a>Pour spécifier les produits et classifications de mise à jour pour la synchronisation

1.  Dans la console d’administration WSUS, cliquez sur le nœud **options** .

2.  Cliquez sur **produits et classifications**, puis sur l’onglet **produits** .

3.  Activez les cases à cocher des produits ou des familles de produits que vous souhaitez mettre à jour avec WSUS, puis cliquez sur **OK**.

4.  Sous l’onglet **classifications** , activez les cases à cocher des classifications de mise à jour que vous souhaitez synchroniser avec votre serveur WSUS, puis cliquez sur **OK**.

> [!NOTE]
> Vous pouvez supprimer des produits ou des classifications de la même façon. Votre serveur WSUS arrêtera la synchronisation des nouvelles mises à jour pour les produits que vous avez effacés. Toutefois, les mises à jour qui ont été synchronisées pour ces produits avant que vous les supprimiez seront conservées sur votre serveur WSUS et seront listées comme étant disponibles.
> 
> Pour supprimer ces produits, refusez la mise à jour, comme décrit dans [opérations de mise à jour](updates-operations.md), puis utilisez l' [Assistant Nettoyage de serveur](the-server-cleanup-wizard.md) pour les supprimer.

### <a name="synchronizing-updates-by-language"></a>Synchronisation des mises à jour par langue
Votre serveur WSUS télécharge les mises à jour en fonction des langues que vous spécifiez. Vous pouvez synchroniser les mises à jour dans toutes les langues dans lesquelles elles sont disponibles, ou vous pouvez spécifier un sous-ensemble de langues. Si vous disposez d’une hiérarchie de serveurs WSUS et que vous devez télécharger des mises à jour dans différentes langues, assurez-vous que vous avez spécifié toutes les langues nécessaires sur le serveur en amont. Sur un serveur en aval, vous pouvez spécifier un sous-ensemble des langues que vous avez spécifiées sur le serveur en amont.

### <a name="synchronizing-updates-from-the-microsoft-update-catalog"></a>Synchronisation des mises à jour à partir du catalogue Microsoft Update
Pour plus d’informations sur la synchronisation des mises à jour à partir du site du catalogue Microsoft Update, voir : [WSUS et le site du catalogue](wsus-and-the-catalog-site.md).

## <a name="configuring-proxy-server-settings"></a>Configuration des paramètres du serveur proxy
Vous pouvez configurer votre serveur WSUS pour utiliser un serveur proxy lors de la synchronisation avec un serveur en amont ou Microsoft Update. Ce paramètre s’applique uniquement lorsque votre serveur WSUS exécute des synchronisations. Par défaut, votre serveur WSUS tente de se connecter directement au serveur en amont ou à Microsoft Update.

#### <a name="to-specify-a-proxy-server-for-synchronization"></a>Pour spécifier un serveur proxy pour la synchronisation

1.  Dans la console d’administration WSUS, cliquez sur **options**, puis sur **source de mise à jour et serveur proxy**.

2.  Sous l’onglet **serveur proxy** , activez la case à cocher **utiliser un serveur proxy lors** de la synchronisation, puis tapez le nom du serveur et le numéro de port du serveur proxy.

    > [!NOTE]
    > Configurez WSUS avec le même numéro de port que celui que le serveur proxy est configuré pour utiliser.

    -   Si vous souhaitez vous connecter au serveur proxy avec des informations d’identification spécifiques de l’utilisateur, activez la case à cocher **utiliser les informations d’identification de l’utilisateur pour se connecter au serveur proxy** , puis entrez le nom d’utilisateur, le domaine et le mot de passe de l’utilisateur dans les zones correspondantes.

    -   Si vous souhaitez activer l’authentification de base pour l’utilisateur qui se connecte au serveur proxy, activez la case à cocher **autoriser l’authentification de base (le mot de passe est envoyé en texte clair)** .

3.  Cliquez sur **OK**.

    > [!NOTE]
    > Étant donné que WSUS initialise tout son trafic réseau, il n’est pas nécessaire de configurer le pare-feu Windows sur un serveur WSUS connecté directement à Microsoft Update.

## <a name="configuring-the-update-source"></a>Configuration de la source de mise à jour
La source de mise à jour est l’emplacement à partir duquel votre serveur WSUS obtient ses mises à jour et met à jour les métadonnées. Vous pouvez spécifier que la source de mise à jour doit être Microsoft Update ou un autre serveur WSUS (le serveur WSUS qui agit en tant que source de mise à jour est le serveur en amont, et votre serveur est le serveur en aval).

Les options de personnalisation de la synchronisation de votre serveur WSUS avec la source de mise à jour sont les suivantes :

-   Vous pouvez spécifier un port personnalisé pour la synchronisation. Pour plus d’informations sur la configuration des ports, consultez [étape 3 : configurer WSUS](../deploy/2-configure-wsus.md) dans le Guide de déploiement de WSUS.

-   Vous pouvez utiliser SSL (Secure Socket Layer) pour sécuriser la synchronisation des informations de mise à jour entre les serveurs WSUS. Pour plus d’informations sur l’utilisation de SSL, consultez la section «3,5. Sécuriser WSUS avec le protocole protocole SSL» de l' [étape 3 : configurer WSUS](../deploy/2-configure-wsus.md) dans le Guide de déploiement de WSUS.

## <a name="synchronizing-manually-or-automatically"></a>Synchronisation manuelle ou automatique
Vous pouvez soit synchroniser votre serveur WSUS manuellement, soit spécifier une heure pour qu’il se synchronise automatiquement.

#### <a name="to-manually-synchronize-the-wsus-server"></a>Pour synchroniser manuellement le serveur WSUS

1.  Dans la console d’administration WSUS, cliquez sur **options**, puis sur planification de la **synchronisation**.

2.  Cliquez sur **synchroniser manuellement**, puis sur **OK**.

#### <a name="to-set-up-an-automatic-synchronization-schedule"></a>Pour configurer un calendrier de synchronisation automatique

1.  Dans la console d’administration WSUS, cliquez sur **options**, puis sur planification de la **synchronisation**.

2.  Cliquez sur **synchroniser automatiquement**.

3.  Pour la **première synchronisation**, sélectionnez l’heure à laquelle vous souhaitez que la synchronisation démarre chaque jour.

4.  pour les **synchronisations par jour**, sélectionnez le nombre de synchronisations que vous voulez faire chaque jour. Par exemple, si vous souhaitez quatre synchronisations par jour à partir de 3:00 A.M., les synchronisations auront lieu à 3:00 h 00, 9:00 3:00 h 00 et 9:00 P.M. tous les jours. (Notez qu’un décalage temporel aléatoire sera ajouté à l’heure de synchronisation planifiée pour libérer les connexions au serveur à Microsoft Update.)

5.  Cliquez sur **OK**.

#### <a name="to-synchronize-your-wsus-server-immediately"></a>Pour synchroniser immédiatement votre serveur WSUS

1.  Dans la console d’administration WSUS, sélectionnez le nœud de serveur supérieur.

2.  Dans le volet **vue d’ensemble** , sous état de la **synchronisation**, cliquez sur **Synchroniser maintenant**.

> [!NOTE]
> La synchronisation est lancée par le serveur en aval.
