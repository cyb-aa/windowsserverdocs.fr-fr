---
title: Préparer la migration vers MultiPoint Services
description: Décrit les informations à recueillir avant de migrer vers MultiPoint services dans Windows Server 2016
ms.date: 07/29/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 3060c531-98a2-4957-a02c-be273f25f493
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 3333570aae34f2c102c36382eeffcb5411b7dd83
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858702"
---
# <a name="prepare-to-migrate-to-multipoint-services-in-windows-server-2016"></a>Préparer la migration vers MultiPoint services dans Windows Server 2016

>S’applique à Windows Server 2016

Utilisez les informations suivantes pour rassembler les informations dont vous avez besoin pour migrer le service de rôle MultiPoint services à partir d’un serveur source exécutant une version antérieure de Windows Server 2016 vers un serveur de destination exécutant Windows Server 2016 RTM.

Au minimum, vous devez être membre du groupe Administrateurs sur le serveur source et le serveur de destination pour installer, supprimer ou configurer MultiPoint services.

>[!NOTE]
> Les étapes ci-dessous ne fournissent pas de conseils pour la migration des données enregistrées dans le dossier utilisateur ou les dossiers partagés. Assurez-vous que les utilisateurs sauvegardent leurs données avant de commencer la migration.

Utilisez le gestionnaire MultiPoint pour récupérer les informations nécessaires à la migration. Vous aurez besoin d’une autorisation d’administrateur de serveur pour utiliser le gestionnaire MultiPoint.

Enregistrez les paramètres MultiPoint Server, User et Environment dans la [feuille de collecte des données de migration](multipoint-services-migration-worksheet.md). Suivez les étapes ci-dessous pour recueillir ces informations.

## <a name="multipoint-server-settings-for-the-local-server"></a>Paramètres MultiPoint Server pour le serveur local
1. Démarrez le gestionnaire MultiPoint.
2. Dans l’onglet dossier de **démarrage** , sélectionnez le serveur local, puis cliquez sur **modifier les paramètres du serveur.**
3. Enregistrez les paramètres dans la feuille de calcul des données.
4. Fermez la fenêtre Paramètres.

## <a name="managed-servers-and-computers"></a>Serveurs gérés et ordinateurs

Vous pouvez trouver les noms des serveurs et des ordinateurs gérés dans l’onglet dossier de **démarrage** du gestionnaire multipoint.

## <a name="station-settings"></a>Paramètres de station
Si vous configurez la connexion automatique ou l’orientation de l’affichage pour la station, procédez comme suit pour récupérer ces informations. Sinon, vous pouvez ignorer cette étape.

Pour récupérer les paramètres de la station :

1. Accédez à l’onglet **stations** dans le gestionnaire multipoint.
2. Recherchez une station qui a « Oui » dans la colonne de **connexion automatique** .
3. Sélectionnez cette station, puis cliquez sur **configurer la station**.
4. Enregistrez l’utilisateur qui est utilisé pour la connexion automatique.

Pour récupérer les paramètres d’orientation de l’affichage, affichez les **paramètres de station** pour chaque station.

## <a name="list-of-users"></a>Liste des utilisateurs
1. Dans le gestionnaire MultiPoint, cliquez sur l’onglet **utilisateurs** .
2. Enregistrez les accoutns **utilisateur du tableau de bord** d' **administrateur** et multipoint.
3. Enregistrez les utilisateurs standard.

## <a name="vdi-template-location"></a>Emplacement du modèle VDI
 Si vous avez déjà activé la fonctionnalité de modèle VDI, enregistrez l’emplacement du modèle VDI. Tant que les serveurs source et de destination se trouvent sur le même réseau, vous pouvez importer le modèle à l’aide du gestionnaire MultiPoint.
 
## <a name="next-step"></a>Étape suivante
Vous êtes maintenant prêt à [migrer vers multipoint services](multipoint-services-migration-steps.md) dans la version RTM de Windows Server 2016.