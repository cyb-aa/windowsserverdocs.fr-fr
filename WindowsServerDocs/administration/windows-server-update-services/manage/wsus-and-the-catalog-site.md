---
title: WSUS et le Site du catalogue
description: Rubrique de Windows Server Update Service (WSUS) - comment importer des correctifs dans les services WSUS en accédant au site du catalogue Microsoft Update
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f19a8659-5a96-4fdd-a052-29e4547fe51a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 298df36b856cba97ec19126f77456785e5eb6f50
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810928"
---
# <a name="wsus-and-the-catalog-site"></a>WSUS et le Site du catalogue

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Le Site du catalogue est l’emplacement de Microsoft à partir de laquelle vous pouvez importer des pilotes de correctifs logiciels et de matériel.

## <a name="the-microsoft-update-catalog-site"></a>Le Site du catalogue Microsoft Update
Pour importer des correctifs dans les services WSUS, vous devez accéder à du Site de catalogue Microsoft Update à partir d’un ordinateur WSUS. N’importe quel ordinateur ayant la console d’administration WSUS installée, s’il est un serveur WSUS, peut être utilisé pour importer des correctifs à partir du Site du catalogue. Vous devez être connecté à l’ordinateur en tant qu’administrateur pour importer les correctifs logiciels.

#### <a name="to-access-the-microsoft-update-catalog-site"></a>Pour accéder au Site de catalogue de mise à jour Microsoft

1.  Dans la console d’administration WSUS, sélectionnez le nœud serveur supérieur ou **mises à jour**, puis, dans le **Actions** volet, cliquez sur **importer les mises à jour**. Une fenêtre de navigateur s’ouvre sur le site Web du catalogue Microsoft Update.

2.  Pour accéder aux mises à jour sur ce site, vous devez installer le contrôle activeX catalogue Microsoft Update.

3.  Vous pouvez parcourir ce site pour les pilotes de matériel et les correctifs logiciels Windows. Lorsque vous avez trouvé ceux que vous le souhaitez, ajoutez-les à votre panier.

4.  Lorsque vous avez terminé de navigation, accédez au panier et cliquez sur Importer pour importer vos mises à jour. Pour télécharger les mises à jour sans les importer, désactivez le **importer directement dans Windows Server Update Services** case à cocher.

Mises à jour approuvées, importées à partir du Site de catalogue Microsoft Update sont téléchargées à la prochaine fois que le serveur WSUS synchronise. Ils ne sont pas téléchargées au moment de l’importation à partir du Site de catalogue de mise à jour de Microsoft.

Notez que vous devez accéder à du Site de catalogue Microsoft Update cependant la console WSUS pour vous assurer que les mises à jour sont importés dans un format compatible avec WSUS. Si vous accédez manuellement le site Web du catalogue Microsoft Update, les mises à jour que vous téléchargez ne sont pas importés dans le serveur WSUS, mais au lieu de cela, sont téléchargés en tant qu’individu *. Fichiers MSU. WSUS n’a pas actuellement d’un mécanisme pris en charge pour l’importation de fichiers dans le \*. Format de fichier MSU.

Si vous exécutez le nettoyage du serveur de l’Assistant, les mises à jour importées à partir du catalogue Microsoft Update et qui sont définies comme non approuvé ou refusé peuvent être supprimées à partir du serveur WSUS. S’ils sont supprimés, ils peuvent être réimportées à partir du catalogue Microsoft Update.

> [!NOTE]
> Vous pouvez supprimer des mises à jour qui sont importées à partir du catalogue Microsoft Update qui sont définies comme non approuvé ou refusé, en exécutant l’Assistant de nettoyage de serveur WSUS. Vous pouvez réimportées mises à jour qui ont été précédemment supprimés à partir de vos systèmes WSUS par le biais du catalogue Microsoft Update.

## <a name="restricting-access-to-hotfixes"></a>Restreindre l’accès aux correctifs logiciels
Administrateurs WSUS peuvent envisager de restreindre l’accès aux correctifs logiciels qu’ils ont téléchargé depuis le Site de catalogue de mise à jour Microsoft. Afin de rendre cette restriction, procédez comme suit :

#### <a name="to-restrict-access-to-hotfixes"></a>Pour restreindre l’accès aux correctifs

1.  Activer l’authentification Windows sur la racine virtuelle IIS contenu.

    -   Démarrez le Gestionnaire IIS.

    -   Accédez au nœud de contenu sous le site web d’Administration de WSUS.

    -   Sur le **contenu accueil** volet, double-cliquez dans la **authentification** option.

    -   Sélectionnez **l’authentification anonyme** et cliquez sur **désactiver** dans le **Actions** volet de droite.

    -   Sélectionnez **l’authentification Windows** et cliquez sur **activer** dans le **Actions** volet de droite.

2.  Créer un groupe de cible WSUS pour les ordinateurs qui nécessitent le correctif logiciel et les ajouter au groupe. Pour plus d’informations sur les ordinateurs et les groupes, consultez [ordinateurs la gestion des clients WSUS et les groupes d’ordinateurs WSUS](managing-wsus-client-computers-and-wsus-computer-groups.md) dans ce guide et la section [3.3. Configurer des groupes d’ordinateurs WSUS](../deploy/2-configure-wsus.md#23-configure-wsus-computer-groups) de l’étape 3 : Configurer WSUS, dans le guide de déploiement de WSUS.

3.  Téléchargez les fichiers pour le correctif logiciel.

4.  Définissez les autorisations de ces fichiers afin que seuls les comptes d’ordinateur de ces machines peuvent les lire. Vous devez également autoriser l’accès complet du compte Service réseau pour les fichiers.

5.  Approuver le correctif logiciel pour le groupe de cible WSUS créé à l’étape 2.

## <a name="importing-updates-in-different-languages"></a>L’importation des mises à jour dans différentes langues
Le site Web du catalogue Microsoft Update inclut les mises à jour qui prennent en charge plusieurs langues. Il est très **important** pour faire correspondre les langues prises en charge par le serveur WSUS avec les langages pris en charge par ces mises à jour. Si le serveur WSUS ne prend pas en charge toutes les langues incluses dans la mise à jour, la mise à jour ne sera pas déployé sur les ordinateurs clients. De même, si une mise à jour prise en charge plusieurs langues a été téléchargé sur le serveur WSUS, mais pas encore déployé sur les ordinateurs clients et désélectionne un administrateur l’une des langues inclus la mise à jour, la mise à jour ne sera pas déployé sur les clients.
