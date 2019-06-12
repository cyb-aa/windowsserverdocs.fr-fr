---
title: Services Bureau à distance - stockage sécurisé des données
description: Informations de planification pour le stockage sécurisé des données à l’aide de disques de profil utilisateur (UPD) dans RDS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 37b7f68e-7c3a-4190-a52f-99ae96885fae
author: lizap
ms.author: elizapo
ms.date: 11/21/2016
manager: dongill
ms.openlocfilehash: c3c7be624e3b093347807a5ee131270d3c802f1a
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66805162"
---
# <a name="remote-desktop-services---secure-data-storage-with-upds"></a>Services Bureau à distance - stockage sécurisé des données avec des disques de profil utilisateur

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016

Ressources de business Store, les données de personnalisation utilisateur et les paramètres en toute sécurité sur site ou dans Azure. Hôtes de Session Bureau à distance utilisent l’authentification AD et permettre aux utilisateurs avec les ressources que dont ils ont besoin dans un environnement personnalisé, en toute sécurité. 

Garantir que les utilisateurs ont une expérience cohérente, quel que soit le point de terminaison à partir duquel ils accéder à leurs ressources à distance, est un aspect important de la gestion d’un déploiement services Bureau à distance. Disques de profil utilisateur (UPD) autorise les données utilisateur, les personnalisations et paramètres d’application à suivre un utilisateur au sein d’une collection unique. Un UPD est un utilisateur unique, fichier de disque dur virtuel par regroupement enregistré dans un partage central qui est monté sur une session utilisateur quand ils se connectent : l’UPD est traitée comme un lecteur local pour la durée de cette session. 

À partir de l’utilisateur, l’UPD offre une expérience de famililar - ils enregistrement leurs documents à leurs Documents dossier (sur ce qui semble être un lecteur local), modifiez leurs paramètres d’application comme d’habitude et effectuer des personnalisations à leur environnement Windows. Toutes ces données, y compris la ruche du Registre, sont stockées sur l’UPD et persiste dans un partage réseau central. UPD sont disponibles uniquement pour l’utilisateur lorsque l’utilisateur est connecté activement à un bureau ou à RemoteApp. UPD peuvent uniquement se déplacer au sein d’une collection, car l’ensemble de l’utilisateur `C:\Users\<username\>` directory (y compris AppData\Local) est stocké sur l’UPD.

Vous pouvez utiliser [applets de commande PowerShell](https://technet.microsoft.com/library/jj215443.aspx) pour désigner le chemin d’accès pour le partage central, la taille de chaque UPD, et quels dossiers doivent être inclus ou exclus du profil utilisateur enregistré dans l’UPD. Vous pouvez également activer UPD via le Gestionnaire de serveur en accédant à **Services Bureau à distance** > **Collections** > **une Collection de bureaux**  >  **Propriétés de la Collection de bureaux** > **disques de profil utilisateur**. Notez que vous activez ou désactivez les UPD pour tous les utilisateurs d’une collection entière, mais pas pour des utilisateurs spécifiques dans cette collection. UPD doivent être stockés sur un partage de fichiers central où les serveurs dans la collection disposent des autorisations Contrôle total. 

Vous pouvez obtenir une haute disponibilité pour votre UPD en les stockant dans Azure avec [espaces de stockage Direct](rds-storage-spaces-direct-deployment.md). 