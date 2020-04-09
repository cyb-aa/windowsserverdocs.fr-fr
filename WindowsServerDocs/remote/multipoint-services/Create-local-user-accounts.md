---
title: Créer des comptes d’utilisateur locaux
description: En savoir sur thte trois types de comptes d’utilisateur dans MultiPoint services
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 33321932-4266-4961-9924-2cb4620bfcb4
author: evaseydl
ms.author: evas
manager: scottman
ms.openlocfilehash: b07c88e4961544f5854f6e9d829b8d4b97adf7e8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859762"
---
# <a name="create-local-user-accounts"></a>Créer des comptes d’utilisateur locaux
Trois niveaux de comptes d’utilisateurs locaux peuvent être créés dans à l’aide du gestionnaire MultiPoint : comptes d’utilisateur standard ; Utilisateurs du tableau de bord MultiPoint, qui disposent de droits d’administration limités ; et les comptes d’utilisateur d’administration complets.  
  
Utilisez la procédure suivante pour créer un compte d’utilisateur local sur un serveur MultiPoint. Si votre environnement comprend plusieurs serveurs MultiPoint et que vous souhaitez que l’utilisateur soit en mesure de se connecter à n’importe quelle station sur un serveur, vous devez créer un compte d’utilisateur local sur chacun de vos serveurs. Cette configuration présente des limitations. Dans un environnement de domaine, vous pouvez également permettre aux utilisateurs d’utiliser leurs comptes de domaine. Pour obtenir une vue d’ensemble de vos options, consultez [planifier des comptes d’utilisateur pour votre environnement Windows multipoint services](Plan-user-accounts-for-your-MultiPoint-services-environment.md).  
   
1.  Connectez-vous au serveur en tant qu’administrateur, puis ouvrez le gestionnaire MultiPoint.  
  
2.  Cliquez sur l’onglet **utilisateurs** , puis sur **Ajouter un compte d’utilisateur**.  
  
    L’Assistant Ajout d’un compte d’utilisateur s’ouvre.  
  
3.  Entrez un nom de compte et un mot de passe pour le nouveau compte d’utilisateur, puis cliquez sur **suivant**.  
  
4.  Sélectionnez le type de compte d’utilisateur que vous souhaitez créer :  
  
    -   **Utilisateur standard** : peut se connecter à une station et effectuer des tâches utilisateur, mais n’a pas accès au gestionnaire multipoint ou au tableau de bord du serveur multipoint, et ne peut pas arrêter le système.  
  
    -   **Utilisateur du tableau de bord multipoint** -droits d’administration limités. Un utilisateur du tableau de bord peut ouvrir le tableau de bord et effectuer des tâches telles que la connexion des utilisateurs au système ou l’arrêt de l’ordinateur MultiPoint Server, mais l’utilisateur n’a pas accès au gestionnaire MultiPoint.  
  
    -   **Utilisateur administratif** Dispose de droits d’administration complets dans MultiPoint Server. Par exemple, un utilisateur administratif peut exécuter le gestionnaire MultiPoint, ajouter et supprimer des utilisateurs, modifier les paramètres système et mettre à jour les pilotes.  
  
5.  Cliquez sur **suivant**, puis sur **Terminer** pour créer le compte d’utilisateur.