---
title: Mises à jour de sécurité étendues Windows Server 2008 et 2008 R2
description: Découvrez comment utiliser les mises à jour de sécurité étendues pour Windows Server 2008 et 2008 R2 au terme de leur cycle de support.
ms.prod: windows-server
ms.technology: server-general
ms.mktglfcycl: manage
author: iainfoulds
ms.author: iainfou
ms.topic: get-started-article
ms.localizationpriority: high
ms.date: 12/16/2019
ms.openlocfilehash: 83ab3663b2c03017ba1bf613a49c394be0511002
ms.sourcegitcommit: b649047f161cb605df084f18b573f796a584753b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76162500"
---
# <a name="how-to-use-windows-server-2008-and-2008-r2-extended-security-updates-esu"></a>Guide pratique pour utiliser les mises à jour de sécurité étendues Windows Server 2008 et 2008 R2

>S'applique à : Windows Server 2008 / 2008 R2

Windows Server 2008 et Windows Server 2008 R2 arrivent au terme de leur cycle de support le 14 janvier 2020. Le canal de maintenance à long terme de Windows Server offre au minimum dix ans de support : cinq ans pour le support standard et cinq ans pour le support étendu. Ce support comprend les mises à jour de sécurité régulières.

La fin du support signifie également la fin des mises à jour de sécurité. Ce scénario peut poser des problèmes de sécurité ou de conformité et mettre en péril des applications métier. Microsoft vous recommande d’[effectuer la mise à niveau vers la version actuelle de Windows Server](modernize-windows-server-2008.md) pour bénéficier des dernières avancées en matière de sécurité, de performances et d’innovations.

Si vous ne pouvez pas mettre à niveau tous vos serveurs avant la fin du cycle de support, les options suivantes vous aideront à protéger vos applications et vos données durant la transition :

* Migrer les charges de travail Windows Server 2008 et 2008 R2 existantes en l’état vers des machines virtuelles Azure.
    * La migration vers Azure vous donne automatiquement trois années supplémentaires de mises à jour de sécurité étendues. Aucuns frais supplémentaires ne sont facturés pour les mises à jour de sécurité étendues au-delà du coût des machines virtuelles Azure et aucune configuration supplémentaire n’est requise.
* Acheter un abonnement aux mises à jour de sécurité étendues pour vos serveurs et bénéficier d’une protection jusqu’au moment où vous décidez de passer à une version plus récente de Windows Server.
    * Ces mises à jour sont fournies pendant une durée maximum de trois ans après la fin du cycle de support.

Au terme de cette période, les ordinateurs ne peuvent plus recevoir de mises à jour supplémentaires.

## <a name="what-are-extended-security-updates-for-windows-server"></a>Présentation des mises à jour de sécurité étendues pour Windows Server

Les mises à jour de sécurité étendues pour Windows Server regroupent les mises à jour et bulletins de sécurité considérés comme *critiques* et *importants* pendant une période maximale de trois ans à compter du 14 janvier 2020. Les mises à jour de sécurité étendues n’incluent pas ce qui suit :

* Nouvelles fonctionnalités
* Correctifs logiciels n’ayant pas trait à la sécurité demandés par les clients
* Demandes de modification de la conception

Pour plus d’informations, consultez les [questions fréquentes sur les mises à jour de sécurité étendues](https://www.microsoft.com/cloud-platform/extended-security-updates).

## <a name="register-for-extended-security-updates"></a>S’inscrire aux mises à jour de sécurité étendues

Pour utiliser les mises à jour de sécurité étendues, vous devez créer une clé d’activation multiple (MAK) et l’appliquer aux ordinateurs Windows Server 2008 et 2008 R2. Cette clé indique aux serveurs Windows Update que vous pouvez continuer à recevoir des mises à jour de sécurité. Utilisez le portail Azure pour vous inscrire aux mises à jour de sécurité étendues et gérer ces clés, même si vous utilisez uniquement des ordinateurs locaux.

> [!NOTE]
> Si vous exécutez des machines virtuelles Windows Server 2008 / 2008 R2 dans Azure, vous n’avez pas besoin d’effectuer les étapes suivantes. Les machines virtuelles Azure reçoivent automatiquement les mises à jour de sécurité étendues. Il est inutile de créer une ressource de mises à jour de sécurité étendues et une clé. Par ailleurs, l’utilisation de mises à jour de sécurité étendues avec les machines virtuelles Azure n’engendre pas de frais supplémentaires.

> [!NOTE]
> Avant de suivre les étapes ci-dessous, envoyez un e-mail à [winsvresuchamps@microsoft.com](mailto:winsvresuchamps@microsoft.com) avec ces informations pour figurer sur liste verte :
> * Nom du client :
> * Abonnement Azure :
> * Numéro de contrat EA (pour les mises à jour de sécurité étendues) :
> * Nombre de serveurs de mises à jour de sécurité étendues :
> 
> L’équipe passera en revue les informations fournies et ajoutera l’utilisateur/abonnement à la liste verte.
> 
> Si le demandeur n’est pas sur liste verte, l’erreur suivante peut se produire : [Le type de ressource est introuvable dans l’espace de noms « Microsoft.WindowsESU »](https://social.msdn.microsoft.com/Forums/office/94b16a89-3149-43da-865d-abf7dba7b977/the-resource-type-could-not-be-found-in-the-namespace-microsoftwindowsesu-for-api-version).

Pour inscrire des machines virtuelles non-Azure aux mises à jour de sécurité étendues et créer une clé, effectuez les étapes suivantes dans le portail Azure :

1. Connectez-vous au [portail Azure](https://portal.azure.com/).
1. Dans la zone de recherche située en haut du portail Azure, recherchez et sélectionnez **Mises à jour de sécurité étendues**.

    ![Rechercher les mises à jour de sécurité étendues dans le portail Azure](media/extended-security-updates/esu-portal-search.png)

    Si vous n’avez pas encore utilisé les mises à jour de sécurité étendues, commencez par **+ Créer** une ressource de mises à jour de sécurité étendues. Sinon, sélectionnez votre ressource dans la liste.

1. Sous **S’inscrire aux mises à jour de service étendues**, sélectionnez **Démarrer**.

    ![Démarrer les mises à jour de sécurité étendues dans le portail Azure](media/extended-security-updates/get-started-with-esu.png)

1. Pour créer votre première clé, sélectionnez **Obtenir la clé**.

    ![Créer une clé dans le portail Azure](media/extended-security-updates/get-key.png)

    > [!NOTE]
    > Vous devez avoir un abonnement Azure associé à votre compte pour créer la ressource de mises à jour de sécurité étendues et la clé. Si aucun abonnement Azure n’est associé à votre compte, connectez-vous avec un autre compte d’utilisateur ou créez un abonnement Azure en suivant les étapes guidées du portail.

1. Sous **Détails relatifs à Azure**, sélectionnez votre abonnement Azure, un groupe de ressources et l’emplacement de votre clé.

    Sous **Détails de l’inscription**, entrez les informations suivantes :

    | Paramètre             | Value |
    |---------------------|-------|
    | Nom de clé            | Nom d’affichage de votre clé, par exemple *Contrat01*. |
    | Numéro de contrat    | Numéro de contrat généré par le système de gestion des contrats de licence en volume ou MSLicense pour les programmes Accord Entreprise. |
    | Nombre d'ordinateurs | Choisissez le nombre d’ordinateurs sur lesquels vous souhaitez installer les mises à jour de sécurité étendues avec cette clé. |
    | Système d’exploitation    | Choisissez le système d’exploitation avec lequel vous souhaitez utiliser cette clé, par exemple Windows Server 2008 ou Windows Server 2008 R2. |

    Quand vous êtes prêt, sélectionnez **Passer en revue + s’inscrire**.

1. Une fois la validation réussie, un récapitulatif de vos choix pour la nouvelle ressource de registre vous est présenté. Si nécessaire, corrigez les erreurs de validation ou mettez à jour votre choix de configuration. Les [conditions d’utilisation](https://azure.microsoft.com/support/legal/) et la [politique de confidentialité](https://privacy.microsoft.com/privacystatement) d’Azure sont disponibles.

    Cochez la case pour confirmer que vous disposez d’ordinateurs éligibles et que la clé ne sera utilisée que dans votre organisation :

    ![Confirmer que la clé sera utilisée uniquement par votre organisation](media/extended-security-updates/confirm-key-usage.png)

    Quand vous êtes prêt, sélectionnez **Créer** pour générer la clé MAK.

L’inscription aux mises à jour de sécurité étendues peut désormais être utilisée avec vos ordinateurs. La clé créée doit être appliquée aux ordinateurs Windows Server 2008 et 2008 R2 dont vous souhaitez qu’ils bénéficient des mises à jour de sécurité.

## <a name="download-and-apply-extended-security-updates"></a>Télécharger et appliquer des mises à jour de sécurité étendues

La remise, le téléchargement et l’application des mises à jour de sécurité étendues pour Windows Server sont semblables aux processus de déploiement existants. Les mises à jour fournies dans le cadre des mises à jour de sécurité étendues ont uniquement trait à la *sécurité* et sont publiées chaque mardi (« Patch Tuesday »).

Vous pouvez installer les mises à jour à l’aide des outils et des processus déjà en place. La seule différence est que le système doit être inscrit à l’aide de la clé générée à la section précédente pour pouvoir télécharger et installer les mises à jour.

Pour les machines virtuelles Azure, le processus d’activation de l’ordinateur pour recevoir les mises à jour de sécurité étendues est automatique. Les mises à jour sont téléchargées et installées sans aucune configuration supplémentaire.
