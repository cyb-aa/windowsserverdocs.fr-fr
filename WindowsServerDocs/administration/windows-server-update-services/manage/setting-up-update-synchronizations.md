---
title: Configuration des synchronisations de mise à jour
description: Rubrique de Windows Server Update Service (WSUS) - comment installer et configurer des synchronisations de mise à jour
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 5fdfaaf1af2b74fe15530095700005a422b64986
ms.sourcegitcommit: a3958dba4c2318eaf2e89c7532e36c78b1a76644
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66719626"
---
# <a name="setting-up-update-synchronizations"></a>Configuration des synchronisations de mise à jour

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Pendant la synchronisation, un serveur WSUS télécharge les mises à jour (mise à jour des métadonnées et fichiers) à partir d’une source de mise à jour. Il télécharge également des classifications de produits et de catégories, le cas échéant. Lorsque votre serveur WSUS synchronise pour la première fois, elle télécharge toutes les mises à jour que vous avez spécifié lorsque vous avez configuré des options de synchronisation. Après la première synchronisation, le serveur WSUS télécharge uniquement les mises à jour à partir de la source de mise à jour, ainsi que des révisions dans les métadonnées de mises à jour existantes et les délais d’expiration pour les mises à jour.

La première fois un serveur WSUS télécharge les mises à jour peut prendre un certain temps. Si vous configurez plusieurs serveurs WSUS, vous pouvez accélérer le processus dans une certaine mesure en téléchargeant toutes les mises à jour sur un serveur WSUS et en copiant les mises à jour vers les répertoires de contenu des autres serveurs WSUS.

Vous pouvez copier le contenu à partir du répertoire de contenu d’un serveur WSUS à un autre. L’emplacement du répertoire de contenu est spécifié lorsque vous exécutez la procédure d’installation de WSUS post. Vous pouvez utiliser l’outil wsusutil.exe pour exporter les métadonnées de mise à jour à partir d’un serveur WSUS dans un fichier. Vous pouvez ensuite importer ce fichier dans les autres serveurs WSUS.

## <a name="setting-up-update-synchronizations"></a>Configuration des synchronisations de mise à jour
Le **Options** page est le point d’accès central dans la Console d’Administration WSUS pour personnaliser la manière dont votre serveur WSUS synchronise les mises à jour. Vous pouvez spécifier quelles mises à jour sont synchronisées automatiquement, dans lequel votre serveur obtient les mises à jour, les paramètres de connexion et le calendrier de synchronisation. Vous pouvez également utiliser l’Assistant de Configuration à partir de la **Options** page pour configurer ou reconfigurer votre serveur WSUS à tout moment.

### <a name="synchronizing-update-by-product-and-classification"></a>Synchronisation des mises à jour par produit et de Classification
Un serveur WSUS télécharge les mises à jour basées sur les produits ou les familles de produits (par exemple, Windows ou Windows Server 2008, Datacenter edition) et les classifications (par exemple, des mises à jour critiques ou des mises à jour de sécurité) que vous spécifiez. à la première synchronisation, le serveur WSUS télécharge toutes les mises à jour disponibles dans les catégories que vous avez spécifiées. Dans les synchronisations suivantes, votre serveur téléchargements uniquement les dernières mises à jour WSUS (ou modifications pour les mises à jour est déjà disponibles sur votre serveur WSUS) pour les catégories vous avez spécifié.

Vous pouvez spécifier la mise à jour de produits et classifications sur la **Options** page sous **produits et Classifications**. Les produits sont répertoriés dans une hiérarchie, regroupée par famille de produits. Si vous sélectionnez Windows, vous sélectionnez automatiquement tous les produits qui relèvent de cette hiérarchie de produit. En sélectionnant la case à cocher parent, vous sélectionnez tous les éléments dans cette section, ainsi que toutes les versions futures. en sélectionnant les cases à cocher enfants ne sélectionne pas les cases à cocher parent. Le paramètre par défaut pour les produits se trouvent tous les produits Windows et le paramètre par défaut pour les classifications doivent impérativement et mises à jour de sécurité.

Si un serveur WSUS s’exécute en mode réplica, vous ne serez pas en mesure d’effectuer cette tâche. Pour plus d’informations sur le mode réplica, consultez [mode réplica de WSUS en cours d’exécution](running-wsus-replica-mode.md), et [étape 1 : Préparer votre déploiement de WSUS](../plan/plan-your-wsus-deployment.md).

##### <a name="to-specify-update-products-and-classifications-for-synchronization"></a>Pour spécifier la mise à jour de produits et classifications pour la synchronisation

1.  Dans la Console d’Administration WSUS, cliquez sur le **Options** nœud.

2.  Cliquez sur **produits et Classifications**, puis cliquez sur le **produits** onglet.

3.  Sélectionnez les cases à cocher des produits ou des familles de produits que vous souhaitez mettre à jour avec WSUS, puis cliquez sur **OK**.

4.  Sur le **Classifications** , sélectionnez les cases à cocher des classifications de mise à jour que vous souhaitez que votre serveur WSUS à synchroniser, puis cliquez sur **OK**.

> [!NOTE]
> Vous pouvez supprimer des produits ou des classifications de la même façon. Votre serveur WSUS s’arrête la synchronisation de nouvelles mises à jour pour les produits que vous avez effacé. Toutefois, les mises à jour qui ont été synchronisées pour ces produits avant que vous avez désactivé les resteront sur votre serveur WSUS et apparaît comme étant disponibles.
> 
> Pour supprimer ces produits, refuser la mise à jour, comme décrit dans [les opérations de mises à jour](updates-operations.md), puis utilisez le [nettoyage du serveur de l’Assistant](the-server-cleanup-wizard.md) pour les supprimer.

### <a name="synchronizing-updates-by-language"></a>Synchronisation des mises à jour par langue
Votre serveur WSUS télécharge les mises à jour basées sur les langues que vous spécifiez. Vous pouvez synchroniser les mises à jour dans toutes les langues dans lesquelles ils sont disponibles, ou vous pouvez spécifier un sous-ensemble de langues. Si vous disposez d’une hiérarchie de serveurs WSUS, et vous devez télécharger les mises à jour dans différentes langues, assurez-vous que vous avez spécifié toutes les langues nécessaires sur le serveur en amont. Sur un serveur en aval, vous pouvez spécifier un sous-ensemble des langues que vous avez spécifié sur le serveur en amont.

### <a name="synchronizing-updates-from-the-microsoft-update-catalog"></a>Synchronisation des mises à jour à partir du catalogue Microsoft Update
Pour plus d’informations sur la synchronisation des mises à jour à partir du site du catalogue Microsoft Update, consultez : [WSUS et le Site du catalogue](wsus-and-the-catalog-site.md).

## <a name="configuring-proxy-server-settings"></a>Configuration des paramètres de serveur Proxy
Vous pouvez configurer votre serveur WSUS pour utiliser un serveur proxy lors de la synchronisation avec un serveur en amont ou de Microsoft Update. Ce paramètre s’applique uniquement lorsque votre serveur WSUS s’exécute des synchronisations. Par défaut votre serveur WSUS tente de se connecter directement au serveur en amont ou Microsoft Update.

#### <a name="to-specify-a-proxy-server-for-synchronization"></a>Pour spécifier un serveur proxy pour la synchronisation

1.  Dans la Console d’Administration WSUS, cliquez sur **Options**, puis cliquez sur **Source de mise à jour et serveur Proxy**.

2.  Sur le **serveur Proxy** onglet, sélectionnez le **utiliser un serveur proxy lors de la synchronisation** case, puis tapez le nom du serveur et numéro de port du serveur proxy.

    > [!NOTE]
    > Configurer WSUS avec le même numéro de port du serveur proxy est configuré pour utiliser.

    -   Si vous souhaitez vous connecter au serveur proxy avec les informations d’identification de l’utilisateur spécifique, sélectionnez le **utiliser les informations d’identification de l’utilisateur pour se connecter au serveur proxy** case à cocher, puis entrez le nom d’utilisateur, le domaine et le mot de passe de l’utilisateur dans les zones correspondantes .

    -   Si vous souhaitez activer l’authentification de base pour l’utilisateur qui se connecte au serveur proxy, activez la **autoriser l’authentification de base (mot de passe est envoyé en texte clair)** case à cocher.

3.  Cliquez sur **OK**.

    > [!NOTE]
    > Étant donné que WSUS initie tout son trafic réseau, il n’est pas nécessaire de configurer le pare-feu de Windows sur un serveur WSUS connecté directement à Microsoft update.

## <a name="configuring-the-update-source"></a>Configuration de la Source de mise à jour
La source de mise à jour est l’emplacement à partir de laquelle votre serveur WSUS obtient ses mises à jour et mettre à jour les métadonnées. Vous pouvez spécifier que la source de mise à jour doit être Microsoft Update ou un autre serveur WSUS (le serveur WSUS qui agit en tant que la source de mise à jour est le serveur en amont, et votre serveur est le serveur en aval).

Options permettant de personnaliser la façon dont votre serveur WSUS se synchronise avec la source de mise à jour sont les suivantes :

-   Vous pouvez spécifier un port personnalisé pour la synchronisation. Pour plus d’informations sur la configuration des ports, consultez [étape 3 : Configurer WSUS](../deploy/2-configure-wsus.md) dans le guide de déploiement de WSUS.

-   Vous pouvez utiliser les couches de Socket sécurisé (SSL) pour sécuriser la synchronisation des informations de mise à jour entre les serveurs WSUS. Pour plus d’informations sur l’utilisation de SSL, consultez la section « 3.5. Sécuriser WSUS avec le protocole Secure Sockets Layer » de [étape 3 : Configurer WSUS](../deploy/2-configure-wsus.md) dans le guide de déploiement de WSUS.

## <a name="synchronizing-manually-or-automatically"></a>Synchronisation manuellement ou automatiquement
Vous pouvez synchroniser votre serveur WSUS manuellement ou spécifier une heure pour qu’il pour synchroniser automatiquement.

#### <a name="to-manually-synchronize-the-wsus-server"></a>Pour synchroniser manuellement le serveur WSUS

1.  Dans la Console d’Administration WSUS, cliquez sur **Options**, puis cliquez sur **calendrier de synchronisation**.

2.  Cliquez sur **synchroniser manuellement**, puis cliquez sur **OK**.

#### <a name="to-set-up-an-automatic-synchronization-schedule"></a>Pour configurer une planification de la synchronisation automatique

1.  Dans la Console d’Administration WSUS, cliquez sur **Options**, puis cliquez sur **calendrier de synchronisation**.

2.  Cliquez sur **synchroniser automatiquement**.

3.  Pour **première synchronisation**, sélectionnez l’heure de la synchronisation pour démarrer chaque jour.

4.  pour **synchronisations par jour**, sélectionnez le nombre de synchronisations que vous voulez faire chaque jour. Par exemple, si vous souhaitez quatre synchronisations de départ de jours à 3 h 00, puis les synchronisations se produira à 3 h 00, 9 h 00, 3 h 00 et 21 h 00 chaque jour. (Notez qu’un décalage aléatoire est ajouté à l’heure de synchronisation planifiée pour espacer les connexions du serveur à Microsoft Update.)

5.  Cliquez sur **OK**.

#### <a name="to-synchronize-your-wsus-server-immediately"></a>Pour synchroniser votre serveur WSUS immédiatement

1.  Dans la Console d’Administration WSUS, sélectionnez le nœud de serveur supérieur.

2.  Dans le **vue d’ensemble** volet, sous **l’état de synchronisation**, cliquez sur **synchroniser maintenant**.

> [!NOTE]
> La synchronisation est lancée par le serveur en aval.
