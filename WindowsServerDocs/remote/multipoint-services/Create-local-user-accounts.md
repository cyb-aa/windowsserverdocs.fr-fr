---
title: Créer des comptes d’utilisateur locaux
description: En savoir plus à propo thte trois types de comptes d’utilisateur dans MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 33321932-4266-4961-9924-2cb4620bfcb4
author: evaseydl
ms.author: evas
manager: scottman
ms.openlocfilehash: e2e5a6c79e6dcd603d19ca868df1d11fce2bf746
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823670"
---
# <a name="create-local-user-accounts"></a>Créer des comptes d’utilisateur locaux
Trois niveaux de comptes d’utilisateurs locaux peuvent être créés dans l’aide du gestionnaire MultiPoint : Comptes d’utilisateur standard ; Utilisateurs du tableau de bord multiPoint, souffrant de droits d’administration ; et les comptes d’utilisateur administratif complets.  
  
Utilisez la procédure suivante pour créer un compte d’utilisateur local sur un serveur MultiPoint. Si votre environnement inclut plusieurs serveurs MultiPoint et que vous souhaitez que l’utilisateur doit pouvoir se connecter à n’importe quelle station sur n’importe quel serveur, vous devez créer un compte d’utilisateur local sur chacun de vos serveurs. Cette configuration présente certaines limitations. Dans un environnement de domaine, vous pouvez également permettre d’utiliser leurs comptes de domaine. Pour une vue d’ensemble de vos options, consultez [planifier des comptes d’utilisateur pour votre environnement Windows MultiPoint Services](Plan-user-accounts-for-your-MultiPoint-services-environment.md).  
   
1.  Ouvrez une session le serveur en tant qu’administrateur, puis ouvrez le gestionnaire MultiPoint.  
  
2.  Cliquez sur le **utilisateurs** onglet, puis cliquez sur **ajouter le compte d’utilisateur**.  
  
    L’Assistant Ajout d’un compte d’utilisateur s’ouvre.  
  
3.  Entrez un nom de compte et le mot de passe pour le nouveau compte d’utilisateur, puis cliquez sur **suivant**.  
  
4.  Sélectionnez le type de compte d’utilisateur que vous souhaitez créer :  
  
    -   **Utilisateur standard** - peut ouvrir une session sur une station et effectuer des tâches de l’utilisateur, mais n’a pas accès au gestionnaire MultiPoint ou le tableau de bord MultiPoint Server et ne peut pas arrêter le système.  
  
    -   **Utilisateur du tableau de bord multiPoint** -dispose de droits d’administration limités. Un utilisateur de tableau de bord peut ouvrir le tableau de bord et effectuer des tâches telles que l’enregistrement des utilisateurs sur le système ou l’arrêt de l’ordinateur MultiPoint Server, mais l’utilisateur n’a pas accès au gestionnaire MultiPoint.  
  
    -   **Utilisateur administratif** a des droits d’administration complets dans MultiPoint Server. Par exemple, un utilisateur administratif peut exécuter le gestionnaire MultiPoint, ajouter et supprimer des utilisateurs, modifier les paramètres système et mettre à jour de pilotes.  
  
5.  Cliquez sur **suivant**, puis cliquez sur **Terminer** pour créer le compte d’utilisateur.