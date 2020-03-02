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
ms.date: 02/21/2020
ms.openlocfilehash: 6c9d732b6ec3d8ceb65c691ab143f09dd8f10f23
ms.sourcegitcommit: 47d2e744a28a3f19347ec4773b7df5f1961ea192
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/21/2020
ms.locfileid: "77552537"
---
# <a name="how-to-use-windows-server-2008-and-2008-r2-extended-security-updates-esu"></a>Guide pratique pour utiliser les mises à jour de sécurité étendues Windows Server 2008 et 2008 R2

>S'applique à : Windows Server 2008 et Windows Server 2008 R2

Windows Server 2008 et Windows Server 2008 R2 sont arrivés au terme de leur cycle de support le 14 janvier 2020. Le canal de maintenance à long terme (LTSC, Long Term Servicing Channel) de Windows Server offre un minimum de dix ans de support : cinq ans pour le support standard et cinq ans pour le support étendu. Ce support comprend les mises à jour de sécurité régulières.

La fin du support signifie également la fin des mises à jour de sécurité. Ce scénario peut poser des problèmes de sécurité ou de conformité et mettre en péril des applications métier. Microsoft vous recommande d’[effectuer la mise à niveau vers la version actuelle de Windows Server](modernize-windows-server-2008.md) pour bénéficier des dernières avancées en matière de sécurité, de performances et d’innovations.

Si vous n’avez pas encore mis à niveau vos serveurs, les options suivantes vous aideront à protéger vos applications et vos données pendant la transition :

* Migrer les charges de travail Windows Server 2008 et 2008 R2 existantes en l’état vers des machines virtuelles Azure.
  * La migration vers Azure vous donne automatiquement trois années supplémentaires de mises à jour de sécurité étendues. Aucuns frais supplémentaires ne sont facturés pour les mises à jour de sécurité étendues au-delà du coût des machines virtuelles Azure et aucune configuration supplémentaire n’est requise.
* Acheter un abonnement aux mises à jour de sécurité étendues pour vos serveurs et bénéficier d’une protection jusqu’au moment où vous décidez de passer à une version plus récente de Windows Server.
  * Ces mises à jour sont fournies pendant une durée maximum de trois ans après la fin du cycle de support.

Après la période de trois ans des mises à jour étendues, nous arrêterons la mise à jour de Windows Server 2008 et 2008 R2. Nous vous recommandons de mettre à jour dès que possible votre version de Windows Server vers une version plus récente.

## <a name="what-are-extended-security-updates-for-windows-server"></a>Présentation des mises à jour de sécurité étendues pour Windows Server

Les mises à jour de sécurité étendues pour Windows Server regroupent les mises à jour et bulletins de sécurité considérés comme *critiques* et *importants* pendant une période maximale de trois ans à compter du 14 janvier 2020. Les mises à jour de sécurité étendues n’incluent pas ce qui suit :

* Nouvelles fonctionnalités
* Correctifs logiciels n’ayant pas trait à la sécurité demandés par les clients
* Demandes de modification de la conception

Pour plus d’informations, consultez les [questions fréquentes sur les mises à jour de sécurité étendues](https://www.microsoft.com/cloud-platform/extended-security-updates).

## <a name="how-to-use-extended-security-updates"></a>Comment utiliser les mises à jour de sécurité étendues

Si vous exécutez des machines virtuelles Windows Server 2008 ou 2008 R2 dans Azure, celles-ci sont automatiquement activées pour les mises à jour de sécurité étendues. Vous n’avez pas besoin de configurer quoi que ce soit et aucun supplément ne vous est facturé pour l’utilisation des mises à jour de sécurité étendues avec les machines virtuelles Azure. Les mises à jour de sécurité étendues sont automatiquement distribuées aux machines virtuelles Azure si celles-ci sont configurées pour recevoir des mises à jour.

Pour d’autres environnements, comme les machines virtuelles locales ou les serveurs physiques, vous devez demander et configurer manuellement les mises à jour de sécurité étendues. Vous pouvez acheter les mises à jour de sécurité étendues via les programmes de licence en volume, comme Contrat Entreprise (EA, Enterprise Agreement), Contrat Souscription Entreprise (EAS, Enterprise Agreement Subscription), EES (Enrollment for Education Solutions) ou SCE (Server and Cloud Enrollment).

Une fois que vous avez acheté les mises à jour de sécurité étendues, vous pouvez utiliser une des méthodes suivantes pour obtenir vos clés :

* Si vous voulez obtenir les clés des mises à jour de sécurité étendues auprès du portail Azure, vous pouvez [vous inscrire pour les mises à jour de sécurité étendues dans le portail Azure](#register-for-extended-security-updates-on-azure-portal).
* Vous pouvez aussi [vous connecter au Centre de gestion des licences en volume Microsoft](#sign-in-to-the-microsoft-volume-licensing-service-center) pour obtenir vos clés sans utiliser le portail Azure.

### <a name="register-for-extended-security-updates-on-azure-portal"></a>S’inscrire pour les mises à jour de sécurité étendues sur le portail Azure

Pour utiliser les mises à jour de sécurité étendues sur des machines virtuelles non-Azure, vous devez créer une clé d’activation multiple (MAK) et l’appliquer aux ordinateurs Windows Server 2008 et 2008 R2. La clé MAK indique aux serveurs Windows Update que vous pouvez continuer à recevoir les mises à jour de sécurité. Utilisez le portail Azure pour vous inscrire aux mises à jour de sécurité étendues et gérer ces clés, même si vous utilisez seulement des ordinateurs locaux.

> [!NOTE]
> Vous n’avez pas besoin de vous inscrire aux mises à jour de sécurité étendues si vous exécutez Windows Server 2008 et 2008 R2 sur des machines virtuelles Azure. Pour d’autres environnements, comme des machines virtuelles locales ou des serveurs physiques, [achetez les mises à jour de sécurité étendues](https://www.microsoft.com/licensing/how-to-buy/how-to-buy) avant d’essayer de vous inscrire et de les utiliser.

Pour inscrire votre machine virtuelle pour les mises à jour de sécurité étendues et créer une clé, ouvrez le portail Azure et suivez ces instructions :

1. Connectez-vous au [portail Azure](https://portal.azure.com/).
2. Dans la zone de recherche en haut du portail Azure, recherchez et sélectionnez **Mises à jour de sécurité étendues**.

    ![Rechercher les mises à jour de sécurité étendues dans le portail Azure](media/extended-security-updates/esu-portal-search.png)

    Si vous n’avez pas déjà utilisé les mises à jour de sécurité étendues, commencez par sélectionner **+ Créer** pour créer une ressource de mises à jour de sécurité étendues. Sinon, sélectionnez votre ressource dans la liste.

3. Sous **S’inscrire aux mises à jour de service étendues**, sélectionnez **Démarrer**.

    ![Bien démarrer avec les mises à jour de sécurité étendues dans le portail Azure](media/extended-security-updates/get-started-with-esu.png)

4. Pour créer votre première clé, sélectionnez **Obtenir la clé**.

    ![Choisir de créer une clé dans le portail Azure](media/extended-security-updates/get-key.png)

    Vous devez avoir un abonnement Azure associé à votre compte pour créer la ressource et la clé des mises à jour de sécurité étendues. Si aucun abonnement Azure n’est associé à votre compte, connectez-vous avec un autre compte d’utilisateur ou créez un abonnement Azure dans le portail Azure.

    Vous devez également attribuer le rôle Contributeur à votre abonnement Azure pour que la mise à jour de sécurité fonctionne. Pour vérifier votre rôle, entrez « Abonnements » dans la zone de recherche. Vous voyez un tableau qui vous indique votre rôle en regard de l’ID et du nom de votre abonnement.

    Si vous n’êtes pas contributeur, vous pouvez demander au propriétaire de l’abonnement de changer votre rôle. Pour connaître le propriétaire de votre abonnement, accédez au tableau des rôles décrit dans le paragraphe précédent et sélectionnez le nom de votre abonnement. Ensuite, accédez au menu sur le côté gauche de la page et sélectionnez **Contrôle d’accès (IAM)**  > **Attributions de rôle** et recherchez la section « Propriétaires » dans le tableau.

5. Si vous voyez une page indiquant « Inscrivez-vous pour obtenir une clé d’activation multiple », cela signifie que vous devez demander l’accès à la préversion privée avant de pouvoir utiliser les mises à jour de sécurité étendues. Si vous ne voyez pas cette page, passez à l’étape 6.

   Pour demander l’accès, sélectionnez **Se joindre à la préversion privée**. Une fenêtre d’e-mail s’ouvre. Cet e-mail est votre demande d’accès à l’équipe du produit.
  
    Entrez les informations suivantes dans votre demande :

    * Nom de client
    * ID d’abonnement Azure
    * Numéro de contrat (pour les mises à jour de sécurité étendues)
    * Nombre de serveurs de mises à jour de sécurité étendues

    Quand vous avez terminé, envoyez l’e-mail.

    L’équipe passera en revue les informations que vous fournissez dans votre e-mail de demande. Si tout semble correct, elle vous ajoutera à la liste approuvée.

    Si l’équipe n’approuve pas votre demande, vous voyez l’erreur suivante :

    [Le type de ressource est introuvable dans l’espace de noms « Microsoft.WindowsESU »](https://social.msdn.microsoft.com/Forums/office/94b16a89-3149-43da-865d-abf7dba7b977/the-resource-type-could-not-be-found-in-the-namespace-microsoftwindowsesu-for-api-version).

6. Sous **Détails relatifs à Azure**, sélectionnez votre abonnement Azure, un groupe de ressources et l’emplacement de votre clé.

    Sous **Détails de l’inscription**, entrez les informations suivantes :

    | Paramètre             | Value |
    |---------------------|-------|
    | Nom de clé            | Nom d’affichage de votre clé, par exemple *Contrat01*. |
    | Numéro de contrat    | Numéro de contrat généré par le système de gestion des contrats de licence en volume ou MSLicense pour les programmes Accord Entreprise. |
    | Nombre d'ordinateurs | Choisissez le nombre d’ordinateurs sur lesquels vous souhaitez installer les mises à jour de sécurité étendues avec cette clé. |
    | Système d’exploitation    | Choisissez le système d’exploitation avec lequel vous souhaitez utiliser cette clé, par exemple Windows Server 2008 ou Windows Server 2008 R2. |

    Quand vous êtes prêt, sélectionnez **Passer en revue + s’inscrire**.

    >[!NOTE]
    >Vérifiez que vous avez sélectionné l’abonnement Azure avec lequel vous avez joint la préversion privée dans votre filtre global. Sélectionnez le bouton **Filtrer** dans le ruban du portail Azure pour vérifier votre filtre d’abonnement global.
    >
    > ![Image du ruban du portail Azure avec le bouton Filtrer sélectionné](media/azure-ribbon-filter.png)

7. Une fois la validation réussie, un récapitulatif de vos choix pour la nouvelle ressource de registre vous est présenté. Si nécessaire, corrigez les erreurs de validation ou mettez à jour votre choix de configuration. Les [conditions d’utilisation](https://azure.microsoft.com/support/legal/) et la [politique de confidentialité](https://privacy.microsoft.com/privacystatement) d’Azure sont disponibles.

    Cochez la case pour confirmer que vous disposez d’ordinateurs éligibles et que la clé ne sera utilisée que dans votre organisation :

    ![Confirmer que la clé sera utilisée uniquement par votre organisation](media/extended-security-updates/confirm-key-usage.png)

    Quand vous êtes prêt, sélectionnez **Créer** pour générer la clé MAK.

L’inscription aux mises à jour de sécurité étendues est désormais disponible pour être utilisée avec vos ordinateurs. La clé créée doit être appliquée aux ordinateurs Windows Server 2008 et 2008 R2 dont vous souhaitez qu’ils bénéficient des mises à jour de sécurité.

### <a name="sign-in-to-the-microsoft-volume-licensing-service-center"></a>Se connecter au centre de gestion des licences en volume Microsoft

Si vous n’avez pas accès au portail Azure, vous pouvez utiliser le centre de gestion des licences en volume pour visualiser et télécharger vos clés d’activation.

Pour obtenir vos clés auprès du centre de gestion des licences en volume :

1. Accédez à la [page du centre de gestion des licences en volume](https://www.microsoft.com/vlsc) et connectez-vous avez vos informations d’identification Azure.

2. Sélectionnez **Licences** > **Résumé de la relation** > **ID du Gestionnaire de licences** > **Clés de produit**.

Pour plus d’informations sur la façon d’obtenir des mises à jour de sécurité étendues pour les appareils Windows éligibles, consultez [notre billet Tech Community](https://techcommunity.microsoft.com/t5/windows-it-pro-blog/obtaining-extended-security-updates-for-eligible-windows-devices/ba-p/1167091#).

## <a name="download-and-apply-extended-security-updates"></a>Télécharger et appliquer les mises à jour de sécurité étendues

La livraison, le téléchargement et l’application des mises à jour de sécurité étendues pour Windows Server ne diffèrent pas des processus de déploiement existants. Les mises à jour fournies via les mises à jour de sécurité étendues concernent seulement la *sécurité* et elles sont publiées chaque mardi (« Patch Tuesday »).

Vous pouvez installer les mises à jour à l’aide des outils et des processus déjà en place. La seule différence est que le système doit être inscrit à l’aide de la clé générée à la section précédente pour pouvoir télécharger et installer les mises à jour.

Pour les machines virtuelles Azure, le processus permettant à l’ordinateur de recevoir les mises à jour de sécurité étendues est automatique. Les mises à jour sont téléchargées et installées sans aucune configuration supplémentaire.
