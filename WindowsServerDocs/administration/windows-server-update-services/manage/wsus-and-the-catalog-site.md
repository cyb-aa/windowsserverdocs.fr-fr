---
title: WSUS et le Site du catalogue
description: 'Rubrique Windows Server Update Service (WSUS) : Comment importer des correctifs dans WSUS en accédant au site du catalogue Microsoft Update'
ms.prod: windows-server
ms.technology: manage-wsus
ms.topic: article
ms.assetid: f19a8659-5a96-4fdd-a052-29e4547fe51a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 44c5ff9ffe793160b0d378a753c3f4c35e40f282
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80828322"
---
# <a name="wsus-and-the-catalog-site"></a>WSUS et le Site du catalogue

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Le site de catalogue est l’emplacement Microsoft à partir duquel vous pouvez importer des correctifs logiciels et des pilotes matériels.

## <a name="the-microsoft-update-catalog-site"></a>Site du catalogue Microsoft Update
Pour pouvoir importer des correctifs dans WSUS, vous devez accéder au site du catalogue Microsoft Update à partir d’un ordinateur WSUS. Tout ordinateur sur lequel la console d’administration WSUS est installée, qu’il s’agisse ou non d’un serveur WSUS, peut être utilisé pour importer des correctifs à partir du site du catalogue. Vous devez avoir ouvert une session sur l’ordinateur en tant qu’administrateur pour importer les correctifs logiciels.

#### <a name="to-access-the-microsoft-update-catalog-site"></a>Pour accéder au site du catalogue Microsoft Update

1.  Dans la console d’administration WSUS, sélectionnez le nœud ou les **mises à jour**du serveur le plus élevé, puis dans le volet **actions** , cliquez sur **Importer les mises à jour**. Une fenêtre de navigateur s’ouvre sur le site Web du catalogue Microsoft Update.

2.  Pour accéder aux mises à jour de ce site, vous devez installer le contrôle activeX du catalogue Microsoft Update.

3.  Vous pouvez parcourir ce site pour obtenir les correctifs logiciels Windows et les pilotes matériels. Une fois que vous avez trouvé ceux que vous souhaitez, ajoutez-les à votre panier.

4.  Une fois la navigation terminée, accédez au panier, puis cliquez sur Importer pour importer vos mises à jour. Pour télécharger les mises à jour sans les importer, désactivez la case à cocher **importer directement dans Windows Server Update Services** .

Les mises à jour approuvées importées à partir du site du catalogue Microsoft Update sont téléchargées lors de la prochaine synchronisation du serveur WSUS. Ils ne sont pas téléchargés au moment de l’importation à partir du site du catalogue Microsoft Update.

Notez que vous devez accéder au site du catalogue Microsoft Update via la console WSUS pour vous assurer que les mises à jour sont importées dans un format compatible avec WSUS. Si vous accédez au site Web du catalogue Microsoft Update manuellement, les mises à jour que vous téléchargez ne sont pas importées dans le serveur WSUS, mais sont téléchargées en tant que individuelles *. Fichiers MSU. WSUS n’a pas actuellement de mécanisme pris en charge pour l’importation de fichiers dans le \*. Format MSU.

Si vous exécutez l’Assistant de nettoyage de serveur, les mises à jour importées à partir du catalogue Microsoft Update définies comme non approuvées ou refusées peuvent être supprimées du serveur WSUS. Si elles sont supprimées, elles peuvent être réimportées à partir du catalogue Microsoft Update.

> [!NOTE]
> Vous pouvez supprimer les mises à jour qui sont importées du catalogue Microsoft Update définies comme non approuvées ou refusées, en exécutant l’Assistant Nettoyage du serveur WSUS. Vous pouvez réimporter les mises à jour qui ont été supprimées précédemment de vos systèmes WSUS via le catalogue de Microsoft Update.

## <a name="restricting-access-to-hotfixes"></a>Restriction de l’accès aux correctifs logiciels
Les administrateurs WSUS peuvent envisager de restreindre l’accès aux correctifs qu’ils ont téléchargés à partir du site du catalogue Microsoft Update. Pour effectuer cette restriction, suivez les étapes ci-dessous :

#### <a name="to-restrict-access-to-hotfixes"></a>Pour restreindre l’accès aux correctifs logiciels

1.  Activez l’authentification Windows sur la racine de contenu IIS.

    -   Démarrez le gestionnaire des services Internet.

    -   Accédez au nœud de contenu sous site Web d’administration WSUS.

    -   Dans le volet d' **informations** , double-cliquez sur l’option **d’authentification** .

    -   Sélectionnez **authentification anonyme** , puis cliquez sur **Désactiver** dans le volet **actions** à droite.

    -   Sélectionnez **authentification Windows** , puis cliquez sur **activer** dans le volet **actions** à droite.

2.  Créez un groupe cible WSUS pour les ordinateurs qui ont besoin du correctif, puis ajoutez-les au groupe. Pour plus d’informations sur les ordinateurs et les groupes, voir [gestion des ordinateurs clients WSUS et des groupes d’ordinateurs WSUS](managing-wsus-client-computers-and-wsus-computer-groups.md) dans ce guide, ainsi que la section [3,3. Configurez les groupes d’ordinateurs WSUS](../deploy/2-configure-wsus.md#23-configure-wsus-computer-groups) de l’étape 3 : configurer WSUS dans le Guide de déploiement de WSUS.

3.  Téléchargez les fichiers du correctif logiciel.

4.  Définissez les autorisations de ces fichiers afin que seuls les comptes d’ordinateur de ces ordinateurs puissent les lire. Vous devez également autoriser le compte de service réseau à accéder à tous les fichiers.

5.  Approuvez le correctif logiciel pour le groupe cible WSUS créé à l’étape 2.

## <a name="importing-updates-in-different-languages"></a>Importation de mises à jour dans différentes langues
Le site Web du catalogue Microsoft Update comprend des mises à jour qui prennent en charge plusieurs langues. Il est très **important** de faire correspondre les langues prises en charge par le serveur WSUS avec les langues prises en charge par ces mises à jour. Si le serveur WSUS ne prend pas en charge toutes les langues incluses dans la mise à jour, la mise à jour n’est pas déployée sur les ordinateurs clients. De même, si une mise à jour prenant en charge plusieurs langues a été téléchargée sur le serveur WSUS mais pas encore déployée sur les ordinateurs clients et qu’un administrateur désélectionne une des langues incluses dans la mise à jour, la mise à jour n’est pas déployée sur les clients.
