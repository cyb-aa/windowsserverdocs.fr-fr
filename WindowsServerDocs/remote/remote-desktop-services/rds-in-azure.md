---
title: Déployer en toute transparence des services Bureau à distance avec ARM et Azure Marketplace
description: Découvrez comment créer un petit déploiement RDS dans Azure en utilisant les modèles ARM et à la place de marché Azure.
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
ms.openlocfilehash: e4de3d4ac14a0dbc5500fd7ab8bd8f1568f3da53
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823080"
---
# <a name="seamlessly-deploy-rds-with-arm-and-azure-marketplace"></a>Déployer en toute transparence des services Bureau à distance avec ARM et Azure Marketplace

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2

Services Bureau à distance (RDS) est la plate-forme idéale pour héberger à moindre coût des applications et des ordinateurs de bureau Windows. Vous pouvez utiliser un [offre de la place de marché Azure](#basic-rds-through-the-azure-marketplace) ou un [modèle de démarrage rapide](#Customized-RDS-using-Quickstart-templates) pour créer rapidement un services Bureau à distance sur le déploiement d’Azure IaaS. Place de marché Azure crée un domaine de test, rendant un mécanisme simple et facile pour les tests et la preuve de concept. Les modèles de démarrage rapide, quant à eux, pouvoir utiliser un domaine existant, ce qui les rend un excellent outil pour créer un environnement de production. Une fois configurés, vous pouvez vous connecter aux postes de travail publiés et des applications à partir de diverses plateformes et appareils, utilisent les applications de bureau à distance Microsoft pour Windows, Mac, iOS et Android.

## <a name="basic-rds-through-the-azure-marketplace"></a>RDS de base via la place de marché Azure

Création de votre déploiement via la place de marché Azure est le moyen le plus rapide pour être opérationnel et en cours d’exécution. Lorsque tout est terminé, votre environnement doit ressembler à la [architecture de services Bureau à distance de base](desktop-hosting-logical-architecture.md#basic-deployment). L’offre crée tous les composants de services Bureau à distance que vous avez besoin : il vous suffit de faire est de fournir des informations. 

Vous devrez fournir les informations suivantes lorsque vous déployez l’offre de la place de marché :
- Nom d’utilisateur administrateur et le mot de passe. Il s’agit d’un nouvel utilisateur qui gèrera le déploiement.
- Nom DNS et le nom de domaine AD. Il s’agit de nouvelles ressources sont créées. Assurez-vous que les noms sont significatifs.
- Taille de machine virtuelle. Vous pouvez choisir la taille des machines virtuelles à utiliser pour les points de terminaison RDSH. Vous pouvez également modifier manuellement les tailles après le déploiement initial pour vous aider à optimiser les machines virtuelles pour vos charges de travail et de coût.

Suivez ces étapes pour créer votre déploiement de RDS de faible encombrement à partir de la place de marché Azure : 

1. Lancer le déploiement de RDS de la place de marché Azure :
   1. Connectez-vous à la [Azure portal](https://portal.azure.com).
   2. Cliquez sur **New** pour ajouter votre déploiement.
   3. Tapez « Services Bureau à distance » dans le champ de recherche et appuyez sur ENTRÉE.
   4. Cliquez sur **Services Bureau à distance (RDS) - Basic - Développement/Test**, puis cliquez sur **créer**.
   5. Suivez les étapes décrites dans le portail pour créer et déployer RDS. Vous allez ajouter les détails de configuration de clés, telles que les informations répertoriées ci-dessus. 
2. Se connecter à votre déploiement. Une fois le déploiement terminé, vérifiez la section des sorties des dernières étapes terminer et se connecter à votre déploiement.
   1. Téléchargez et exécutez [ce script PowerShell](https://gallery.technet.microsoft.com/Azure-Resource-Manager-4ea7e328) sur votre appareil de test pour installer les certificats nécessaires pour se connecter au déploiement des services Bureau à distance. 
   
      Cette étape est uniquement nécessaire pendant la phase de test. Lorsque vous déployez des services Bureau à distance dans Azure en production, veillez à suivre les meilleures pratiques, comme l’achat et l’utilisation d’un certificat SSL publiquement approuvé sur vos serveurs web.

   2. Lorsque vous y êtes invité, connectez-vous à votre compte Azure. Sélectionnez l’abonnement Azure, le groupe de ressources et l’adresse IP publique créée pour ce nouveau déploiement.
   3. Lorsque le script est terminé, la page Web du Bureau à distance démarre dans votre navigateur par défaut. Vous pouvez vérifiez de nouveau la page Web du Bureau à distance en comparant l’URL de la page à l’adresse DNS que vous avez fourni pendant le déploiement. 
   
      Connectez-vous avec les informations d’identification d’administrateur que vous avez créé au cours du déploiement pour que le bureau par défaut publié pour vous. Vous pouvez également diriger les utilisateurs du site Web de bureau à distance pour tester leurs postes de travail et les applications.

      > [!TIP]
      > Oublier l’utilisateur de nom ou l’administrateur de domaine ? Vous pouvez accéder au nouveau groupe de ressources dans le portail, cliquez sur **déploiements**, puis affichez les paramètres que vous avez entré.

Maintenant que vous avez un déploiement services Bureau à distance, vous pouvez [ajouter et gérer les utilisateurs](rds-user-management.md).

## <a name="customized-rds-using-quickstart-templates"></a>Services Bureau à distance personnalisé à l’aide de modèles de démarrage rapide

Vous pouvez utiliser des modèles Azure Resource Manager pour déployer des services Bureau à distance dans Azure. Cela est particulièrement utile si vous souhaitez un déploiement services Bureau à distance de base, mais les composants existants (par exemple, AD) que vous souhaitez utiliser. Contrairement à la place de marché offre, vous pouvez apporter des personnalisations supplémentaires, telles que l’utilisation d’une annonce existante sur un réseau virtuel, à l’aide d’une image de système d’exploitation personnalisée pour les machines virtuelles RDSH et la superposition sur la haute disponibilité pour les composants des services Bureau à distance. Après avoir ajouté sur la haute disponibilité pour chaque composant, votre environnement doit ressembler à la [hautement les architecture de services Bureau à distance disponible](desktop-hosting-logical-architecture.md#highly-available-deployment).

Suivez ces étapes pour créer votre déploiement de RDS de faible encombrement avec un modèle de services Bureau à distance Azure : 

1. Choisissez votre modèle Azure Quickstart :
   1. Accédez à la [RDS Azure Quickstart Templates](https://aka.ms/rdautomation) site.
   2. Choisissez le modèle qui correspond à ce que vous essayez de faire. Assurez-vous que vous remplissez les conditions requises pour ce modèle spécifique. (Par exemple, si vous sont que vous souhaitez utiliser une image personnalisée pour vos machines virtuelles, assurez-vous que vous avez déjà téléchargé cette image vers un compte de stockage Azure.)
   3. Cliquez sur **déployer vers Azure**.
   4. Vous devrez fournir des détails (par exemple, le nom d’utilisateur admin, nom de domaine AD) dans le portail Azure. Cette valeur varie en fonction du modèle que vous choisissez.
   5. Cliquez sur **achat**.
2. Se connecter à votre déploiement. 
   1. Téléchargez et exécutez [ce script PowerShell](https://gallery.technet.microsoft.com/Azure-Resource-Manager-4ea7e328) sur votre appareil de test pour installer les certificats nécessaires pour se connecter au déploiement des services Bureau à distance. 
   
      Cette étape est uniquement nécessaire pendant la phase de test. Lorsque vous déployez des services Bureau à distance dans Azure en production, veillez à suivre les meilleures pratiques, comme l’achat et l’utilisation d’un certificat SSL publiquement approuvé sur vos serveurs web.

   2. Lorsque vous y êtes invité, connectez-vous à votre compte Azure. Sélectionnez l’abonnement Azure, le groupe de ressources et l’adresse IP publique créée pour ce nouveau déploiement.
   3. Lorsque le script est terminé, la page Web du Bureau à distance démarre dans votre navigateur par défaut. Vous pouvez vérifiez de nouveau la page Web du Bureau à distance en comparant l’URL de la page à l’adresse DNS que vous avez fourni pendant le déploiement. 
   
      Connectez-vous avec les informations d’identification d’administrateur que vous avez créé au cours du déploiement pour que le bureau par défaut publié pour vous. Vous pouvez également diriger les utilisateurs du site Web de bureau à distance pour tester leurs postes de travail et les applications.

      > [!TIP]
      > Oublier l’utilisateur de nom ou l’administrateur de domaine ? Vous pouvez accéder au nouveau groupe de ressources dans le portail, cliquez sur **déploiements**, puis affichez les paramètres que vous avez entré.

Maintenant que vous avez un déploiement services Bureau à distance, vous pouvez [ajouter et gérer les utilisateurs](rds-user-management.md).
