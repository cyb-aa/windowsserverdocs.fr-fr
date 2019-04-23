---
title: Gérer les utilisateurs de votre collection Services Bureau à distance
description: Découvrez comment gérer les utilisateurs dans les Services Bureau à distance.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2727e1ab-69b8-46f3-9f6d-2540324fe596
author: christianmontoya
ms.author: chrimo
ms.date: 03/27/2018
manager: scottman
ms.openlocfilehash: 4b45061697926a3003712a88610cb17ef3c00c45
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859640"
---
# <a name="manage-users-in-your-rds-collection"></a>Gérer les utilisateurs de votre collection Services Bureau à distance

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

En tant qu’administrateur, vous pouvez gérer directement les utilisateurs ayant accès à des regroupements spécifiques. De cette façon, vous pouvez créer une collection avec des applications standards pour les professionnels de l’information, mais ensuite créer une collection distincte avec des applications de modélisation de graphiques pour les ingénieurs. Il existe deux étapes principales pour la gestion des accès utilisateur dans un déploiement Services Bureau à distance (RDS) :

1.  [Créer des utilisateurs et groupes dans Active Directory](#create-your-users-and-groups-in-active-directory)
2.  [Affecter des utilisateurs et des groupes aux collections](#assign-users-and-groups-to-collections)


## <a name="create-your-users-and-groups-in-active-directory"></a>Créer vos utilisateurs et groupes dans Active Directory

Dans un déploiement services Bureau à distance, les Services de domaine Active Directory (AD DS) est la source de tous les utilisateurs, groupes et d’autres objets dans le domaine. Vous pouvez gérer Active Directory directement avec PowerShell, ou vous pouvez utiliser intégrées dans les outils d’interface utilisateur qui ajoutent de manière simple et flexible. Les étapes suivantes vous guide pour installer ces outils, si vous n’avez pas déjà installés et ensuite utiliser ces outils pour gérer les utilisateurs et groupes.

### <a name="install-ad-ds-tools"></a>Installer les outils AD DS

Les étapes suivantes décrivent comment installer les outils AD DS sur un serveur exécutant déjà les services AD DS. Une fois installé, vous pouvez ensuite créer des utilisateurs ou créer des groupes.

1. Se connecter au serveur exécutant les Services de domaine Active Directory. Pour les déploiements Azure :
   1. Dans le portail Azure, cliquez sur **Parcourir > groupes de ressources**, puis cliquez sur le groupe de ressources pour le déploiement
   2. Sélectionnez l’ordinateur Active Directory.
   3. Cliquez sur **Connect > Ouvrez** pour ouvrir le client Bureau à distance. Si **Connect** est grisée, la machine virtuelle ne peut pas avoir une adresse IP publique. Pour lui donner un effectuer les étapes suivantes, puis réessayez cette étape.
      1. Cliquez sur **Paramètres > interfaces réseau**, puis cliquez sur l’interface réseau correspondante.
      2. Cliquez sur **Paramètres > adresse IP**.
      3. Pour **adresse IP publique**, sélectionnez **activé**, puis cliquez sur **adresse IP**.
      4. Si vous avez une adresse IP publique existante à utiliser, sélectionnez-le dans la liste. Sinon, cliquez sur **créer**, entrez un nom, puis cliquez sur **OK** et **enregistrer**.
   4. Dans le client, cliquez sur **Connect**, puis cliquez sur **utiliser un autre compte**. Entrez le nom d’utilisateur et le mot de passe pour un compte d’administrateur de domaine.
   5. Cliquez sur **Oui** lorsque vous êtes invité sur le certificat.
2. Installer les outils AD DS :
   1. Dans le Gestionnaire de serveur, cliquez sur **gérer > ajouter des rôles et fonctionnalités**.
   2. Cliquez sur **installation en fonction du rôle ou une fonctionnalité**, puis cliquez sur le serveur AD actuel. Suivez les étapes jusqu'à ce que vous arriviez à la **fonctionnalités** onglet.
   3. Développez **outils d’Administration de serveur distant > Outils d’Administration de rôles > Outils AD DS et AD LDS**, puis sélectionnez **outils AD DS**.
   4. Sélectionnez **redémarrer automatiquement le serveur de destination si nécessaire**, puis cliquez sur **installer**.

### <a name="create-a-group"></a>Créer un groupe

Vous pouvez utiliser les groupes AD DS pour accorder l’accès à un ensemble d’utilisateurs ayant besoin d’utiliser les mêmes ressources à distance.

1. Dans le Gestionnaire de serveur sur le serveur exécutant les services AD DS, cliquez sur **Outils > Active Directory Users and Computers**.
2. Développez le domaine dans le volet gauche pour afficher ses sous-dossiers.
3. Cliquez sur le dossier où vous souhaitez créer le groupe, puis cliquez sur **Nouveau > groupe**.
4. Entrez un nom de groupe approprié, puis sélectionnez **Global** et **sécurité**.

### <a name="create-a-user-and-add-to-a-group"></a>Créer un utilisateur et l’ajouter à un groupe
1. Dans le Gestionnaire de serveur sur le serveur exécutant les services AD DS, cliquez sur **Outils > Active Directory Users and Computers**.
2. Développez le domaine dans le volet gauche pour afficher ses sous-dossiers.
3. Avec le bouton droit **utilisateurs**, puis cliquez sur **Nouveau > utilisateur**.
4. Entrez au minimum, un prénom et un nom d’ouverture de session utilisateur.
5. Entrez et confirmez un mot de passe pour l’utilisateur. Définir les options de l’utilisateur approprié, comme **utilisateur doit changer de mot de passe à la prochaine ouverture de session**.
6. Ajouter le nouvel utilisateur à un groupe :
   1. Dans le **utilisateurs** dossier avec le bouton droit le nouvel utilisateur.
   2. Cliquez sur **ajouter à un groupe**.
   3. Entrez le nom du groupe auquel vous souhaitez ajouter l’utilisateur.

## <a name="assign-users-and-groups-to-collections"></a>Affecter des utilisateurs et des groupes aux collections
Maintenant que vous avez créé les utilisateurs et groupes dans Active Directory, vous pouvez ajouter certains granularité concernant qui a accès aux collections de bureau à distance dans votre déploiement.

1. Se connecter au serveur exécutant le rôle de Broker de connexion Bureau à distance (RD Connection Broker), en suivant les étapes décrites précédemment.
2. Ajoutez les autres serveurs Bureau à distance au pool de Broker de connexion Bureau à distance des serveurs gérés :
   1. Dans le Gestionnaire de serveur, cliquez sur **gérer > ajouter des serveurs**.
   2. Cliquez sur **Rechercher**.
   3. Cliquez sur chaque serveur dans votre déploiement qui exécute un rôle Services Bureau à distance, puis cliquez sur **OK**.
3. Modifier une collection pour attribuer l’accès à des utilisateurs ou groupes spécifiques :
   1. Dans le Gestionnaire de serveur, cliquez sur **Services Bureau à distance > vue d’ensemble**, puis cliquez sur un regroupement spécifique.
   2. Sous **propriétés**, cliquez sur **tâches > modifier les propriétés**.
   3. Cliquez sur **groupes d’utilisateurs**.
   4. Cliquez sur **ajouter** et entrez l’utilisateur ou le groupe que vous souhaitez avoir accès à la collection. Vous pouvez également supprimer les utilisateurs et groupes à partir de cette fenêtre en sélectionnant l’utilisateur ou le groupe à supprimer, puis en cliquant sur **supprimer**. 
   
   >[!NOTE] 
   > La fenêtre de groupes de l’utilisateur ne peut jamais être vide. Pour limiter la portée des utilisateurs ayant accès à la collection, vous devez tout d’abord ajouter des utilisateurs ou groupes spécifiques avant de supprimer des groupes plus larges.