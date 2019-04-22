---
title: Préparer la migration vers MultiPoint Services
description: Décrit les informations à rassembler avant de migrer vers les Services MultiPoint dans Windows Server 2016
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3060c531-98a2-4957-a02c-be273f25f493
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 9650ebcae7e6207a226617d401d892049405901f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824380"
---
# <a name="prepare-to-migrate-to-multipoint-services-in-windows-server-2016"></a>Préparer la migration vers Services MultiPoint dans Windows Server 2016

>S'applique à : Windows Server 2016

Utilisez les informations suivantes pour collecter les informations que vous devez migrer le service de rôle de MultiPoint Services à partir d’un serveur source exécutant une version antérieure de Windows Server 2016 sur un serveur de destination exécutant Windows Server 2016 RTM.

Au minimum, vous devez être membre du groupe Administrateurs sur le serveur source et le serveur de destination pour installer, supprimer ou configurer MultiPoint Services.

>[!NOTE]
> Les étapes décrites ici ne fournissent pas de conseils pour migrer les données enregistrement dans le dossier de l’utilisateur ou des dossiers partagés. Assurez-vous que les utilisateurs sauvegarder leurs données avant de commencer la migration.

Utilisez le gestionnaire MultiPoint pour récupérer les informations requises pour la migration. Vous aurez besoin des droits d’administrateur de serveur pour utiliser le gestionnaire MultiPoint.

Enregistrez les paramètres de serveur, utilisateur et environnement MultiPoint dans la [feuille de collecte de données de migration](multipoint-services-migration-worksheet.md). Utilisez les étapes suivantes pour rassembler ces informations.

## <a name="multipoint-server-settings-for-the-local-server"></a>Paramètres multiPoint Server pour le serveur local
1. Démarrez le gestionnaire MultiPoint.
2. Sur le **accueil** onglet, sélectionnez le serveur local, puis cliquez sur **modifier les paramètres du serveur.**
3. Enregistrez les paramètres dans la feuille de données.
4. Fermez la fenêtre de paramètres.

## <a name="managed-servers-and-computers"></a>Ordinateurs et serveurs gérés

Vous pouvez rechercher les noms des serveurs gérés et des ordinateurs sur le **accueil** onglet dans le gestionnaire MultiPoint.

## <a name="station-settings"></a>Paramètres de la station
Si l’orientation de l’ouverture de session automatique ou d’affichage est configurés pour la station, procédez comme suit pour récupérer ces informations. Sinon, vous pouvez ignorer cette étape.

Pour récupérer les paramètres de station :

1. Accédez à la **Stations** onglet dans le gestionnaire MultiPoint.
2. Rechercher une station ayant « Oui » dans le **ouverture de session automatique** colonne.
3. Sélectionnez cette station, puis cliquez sur **configurer la station**.
4. Enregistrement de l’utilisateur qui est utilisé pour l’ouverture de session automatique.

Pour récupérer les paramètres d’orientation de l’affichage, afficher le **paramètres de station** pour chaque station.

## <a name="list-of-users"></a>Liste des utilisateurs
1. Cliquez sur le **utilisateurs** onglet dans le gestionnaire MultiPoint.
2. Enregistrement le **administrateur** et **utilisateur du tableau de bord MultiPoint** souscrire comptes.
3. Enregistrer les utilisateurs standards.

## <a name="vdi-template-location"></a>Emplacement du modèle d’infrastructure VDI
 Si vous déjà activé la fonctionnalité de modèle d’infrastructure VDI, notez l’emplacement du modèle d’infrastructure VDI. Tant que les serveurs source et de destination se trouvent sur le même réseau, vous pouvez importer le modèle à l’aide du gestionnaire MultiPoint.
 
## <a name="next-step"></a>Étape suivante
Vous êtes maintenant prêt à [migrer vers les Services MultiPoint](multipoint-services-migration-steps.md) dans la version RTM de Windows Server 2016.