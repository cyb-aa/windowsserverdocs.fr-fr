---
title: Déployer en toute transparence des services Bureau à distance avec ARM et la Place de marché Azure
description: Découvrez comment créer un déploiement de Services Bureau à distance réduit dans Azure en utilisant les modèles ARM et la Place de marché Azure.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5f72ceb6-6f90-48f6-bfc3-bdad63984ce7
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 02/10/2017
ms.openlocfilehash: 218e61e5ebe110502ebe139b27607bfeff104fde
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "63712621"
---
# <a name="seamlessly-deploy-rds-with-arm-and-azure-marketplace"></a>Déployer en toute transparence des services Bureau à distance avec ARM et la Place de marché Azure

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

La plateforme des Services Bureau à distance (RDS) est idéale pour héberger à moindre coût des applications et des ordinateurs de bureau Windows. Vous pouvez utiliser une offre de la [Place de marché Azure](#basic-rds-through-the-azure-marketplace) ou un [modèle de démarrage rapide](#customized-rds-using-quickstart-templates) pour créer rapidement un service Bureau à distance sur un déploiement Azure IaaS. La Place de marché Azure crée un domaine de test, simplifiant ainsi le mécanisme de test et d’établissement de la preuve de concept. Les modèles de démarrage rapide, quant à eux, peuvent utiliser un domaine existant, ce qui en fait un excellent outil pour créer un environnement de production. Une fois la configuration terminée, vous pouvez vous connecter aux postes de travail et applications publiés depuis différentes plates-formes et périphériques, en utilisant les applications Bureau à distance Microsoft pour Windows, Mac, iOS et Android.

## <a name="basic-rds-through-the-azure-marketplace"></a>Services Bureau à distance de base via la place de marché Azure

La création de votre déploiement via la place de marché Azure est le moyen le plus rapide pour être opérationnel. Lorsque vous avez tout terminé, votre environnement doit ressembler à l’[architecture de Services Bureau à distance de base](desktop-hosting-logical-architecture.md#basic-deployment). L’offre crée tous les composants de services Bureau à distance dont vous avez besoin : tout ce qu’il vous reste à faire, c’est fournir quelques informations. 

Lorsque vous déployez l’offre de la Place de marché, vous devez fournir les informations suivantes :
- Nom et mot de passe d’administrateur par défaut. Il s’agit d’un nouvel utilisateur qui gèrera le déploiement.
- Nom DNS et nom de domaine AD. Il s’agit de nouvelles ressources. Assurez-vous que leurs noms sont significatifs.
- Taille de la machine virtuelle. Vous pouvez choisir la taille des machines virtuelles à utiliser pour les points de terminaison RDSH. Vous pouvez également modifier manuellement les tailles après le déploiement initial pour vous aider à optimiser le nombre de machines virtuelles pour vos charges de travail et de coût.

Suivez ces étapes pour créer votre déploiement de Services Bureau à distance à faible empreinte, à partir de la place de marché Azure : 

1. Lancer le déploiement de Services Bureau à distance à partir de la place de marché Azure :
   1. Connectez-vous au [Portail Azure](https://portal.azure.com).
   2. Cliquez sur **Nouveau** pour ajouter votre déploiement.
   3. Tapez « Services Bureau à distance » dans le champ de recherche, puis appuyez sur Entrée.
   4. Cliquez sur **Services Bureau à distance (RDS) - Environnement de Développement/Test de base**, puis sur **Créer**.
   5. Suivez la procédure décrite dans le portail pour créer et déployer les Services Bureau à distance. Vous allez ajouter les détails de configuration clés, telles que les informations répertoriées ci-dessus. 
2. Se connecter à votre déploiement. Une fois le déploiement terminé, vérifiez la section des sorties pour connaître les dernières étapes terminer et vous connecter à votre déploiement.
   1. Téléchargez et exécutez [ce script PowerShell](https://gallery.technet.microsoft.com/Azure-Resource-Manager-4ea7e328) sur votre appareil de test, pour installer les certificats nécessaires à la connexion au déploiement des Services Bureau à distance. 
   
      Cette étape est uniquement nécessaire pendant la phase de test. Lorsque vous déployez des services Bureau à distance dans Azure en production, veillez à suivre les meilleures pratiques, comme l’achat et l’utilisation d’un certificat SSL publiquement approuvé sur vos serveurs web.

   2. Lorsque vous y êtes invité, connectez-vous à votre compte Azure. Sélectionnez l’abonnement Azure, le groupe de ressources et l’adresse IP publique créée pour ce nouveau déploiement.
   3. Lorsque le script est terminé, la page web du Bureau à distance a démarré dans votre navigateur par défaut. Vous pouvez vérifier à deux reprises La page web Bureau à distance... en comparant son URL à l’adresse DNS que vous avez fourni durant le déploiement. 
   
      Connectez-vous en utilisant les informations d’identification d’administrateur que vous avez créées au cours du déploiement pour afficher le bureau par défaut publié pour vous. Vous pouvez également diriger des utilisateurs vers le site web de des services Bureau à distance, pour tester leurs bureaux et applications.

      > [!TIP]
      > Vous avez oublié le nom de domaine ou l’administrateur administrateur ? Vous pouvez revenir au nouveau groupe de ressources dans le portail. Pour cela, cliquez sur **Déploiements**, puis affichez les paramètres que vous avez entré.

Maintenant que vous avez un déploiement du services Bureau à distance, vous pouvez [Ajouter et gérer les utilisateurs](rds-user-management.md).

## <a name="customized-rds-using-quickstart-templates"></a>Services Bureau à distance personnalisé à l’aide de modèles de démarrage rapide

Vous pouvez utiliser des modèles Azure Resource Manager pour déployer des services Bureau à distance dans Azure. Cela est particulièrement utile si vous souhaitez un déploiement des services Bureau à distance de base, mais ils ont des composants existants (par exemple, AD) que nous vous recommandons d’utiliser. Contrairement aux offres de la place de marché, vous pouvez apporter des personnalisations supplémentaires, telles que l’utilisation d’une instance AD sur un réseau virtuel, l’utilisation d’une image de système d’exploitation pour les machines virtuelles RDSH, et la superposition de la haute disponibilité pour les composants RDS. Après avoir ajouté la haute disponibilité pour chaque composant, votre environnement doit ressembler à l’[architecture des Services Bureau à distance hautement disponibles](desktop-hosting-logical-architecture.md#highly-available-deployment).

Procédez comme suit pour créer votre déploiement des Services Bureau à distance à faible empreinte : 

1. Sélectionnez votre modèle Azure Quickstart :
   1. Accédez au site [Modèles de démarrage rapide des Services Bureau à distance](https://aka.ms/rdautomation).
   2. Choisissez le modèle qui correspond à votre objectif. Assurez-vous que vous remplissez toutes les conditions préalables pour ce modèle spécifique. (Par exemple, si vous voulez utiliser une image personnalisée pour vos machines virtuelles, assurez-vous que vous avez déjà téléchargé cette image sur un compte de stockage Azure.)
   3. Cliquez sur **Déployer sur Azure**.
   4. Vous devrez nous fournir quelques détails (par exemple, le nom d’un utilisateur administrateur, le nom du domaine AD) dans le portail Azure. Cette valeur varie en fonction du modèle que vous choisissez.
   5. Cliquez sur **Acheter**.
2. Se connecter à votre déploiement. 
   1. Téléchargez et exécutez [ce script PowerShell](https://gallery.technet.microsoft.com/Azure-Resource-Manager-4ea7e328) sur votre appareil de test, pour installer les certificats nécessaires aux Services Bureau à distance. 
   
      Cette étape est uniquement nécessaire pendant la phase de test. Lorsque vous déployez des services Bureau à distance dans Azure en production, veillez à suivre les meilleures pratiques, comme l’achat et l’utilisation d’un certificat SSL publiquement approuvé sur vos serveurs web.

   2. Lorsque vous y êtes invité, connectez-vous à votre compte Azure. Sélectionnez l’abonnement Azure, le groupe de ressources et l’adresse IP publique créée pour ce nouveau déploiement.
   3. Lorsque le script est terminé, la page web du Bureau à distance a démarré dans votre navigateur par défaut. Vous pouvez vérifier à deux reprises La page web Bureau à distance... en comparant son URL à l’adresse DNS que vous avez fourni durant le déploiement. 
   
      Connectez-vous en utilisant les informations d’identification d’administrateur que vous avez créées au cours du déploiement pour afficher le bureau par défaut publié pour vous. Vous pouvez également diriger des utilisateurs vers le site web de des services Bureau à distance, pour tester leurs bureaux et applications.

      > [!TIP]
      > Vous avez oublié le nom de domaine ou l’administrateur administrateur ? Vous pouvez revenir au nouveau groupe de ressources dans le portail. Pour cela, cliquez sur **Déploiements**, puis affichez les paramètres que vous avez entré.

Maintenant que vous avez un déploiement du services Bureau à distance, vous pouvez [Ajouter et gérer les utilisateurs](rds-user-management.md).
