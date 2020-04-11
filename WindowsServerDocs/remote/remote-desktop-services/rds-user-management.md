---
title: Gérer les utilisateurs de votre collection de services Bureau à distance
description: Découvrez comment gérer les utilisateurs dans les services Bureau à distance.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 2727e1ab-69b8-46f3-9f6d-2540324fe596
author: christianmontoya
ms.author: chrimo
ms.date: 03/27/2018
manager: scottman
ms.openlocfilehash: 430c38f98dd9aec3034e023d737952e3015622eb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858682"
---
# <a name="manage-users-in-your-rds-collection"></a>Gérer les utilisateurs de votre collection de services Bureau à distance

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

En tant qu’administrateur, vous pouvez gérer directement les utilisateurs ayant accès à des collections spécifiques. Ainsi, vous pouvez créer une collection avec des applications standard pour les travailleurs de l’information, puis créer une collection distincte avec des applications de modélisation graphique pour les ingénieurs. La gestion de l’accès utilisateur dans un déploiement des Services Bureau à distance nécessite deux étapes principales :

1.    [Créer des utilisateurs et groupes dans Active Directory](#create-your-users-and-groups-in-active-directory)
2.    [Affecter des utilisateurs et des groupes à des collections](#assign-users-and-groups-to-collections)


## <a name="create-your-users-and-groups-in-active-directory"></a>Créer vos utilisateurs et groupes dans Active Directory

Dans un déploiement des services Bureau à distance, Active Directory Domain Services (AD DS) est la source de tous les utilisateurs, groupes et autres objets dans le domaine. Vous pouvez gérer Active Directory directement avec PowerShell, ou vous pouvez utiliser des outils d’interface utilisateur intégrés qui rendent le processus plus simple et plus flexible. Les étapes suivantes vous montreront comment installer ces outils, s’ils ne le sont pas encore, puis comment les utiliser pour gérer les utilisateurs et groupes.

### <a name="install-ad-ds-tools"></a>Installer les outils AD DS

Les étapes suivantes décrivent comment installer les outils AD DS sur un serveur exécutant déjà les services AD DS. Une fois ces outils installés, vous pouvez créer des utilisateurs ou des groupes.

1. Connectez-vous au serveur exécutant Active Directory Domain Services. Pour les déploiements Azure :
   1. Dans le portail Azure, cliquez sur **Parcourir > Groupes de ressources**, puis cliquez sur le groupe de ressources pour le déploiement.
   2. Sélectionnez la machine virtuelle Active Directory.
   3. Cliquez sur **Connexion > Ouvrir** pour ouvrir le client Bureau à distance. Si **Se connecter** est grisé, il est possible que la machine virtuelle n’ait pas d’adresse IP publique. Pour lui en affecter une, effectuez les étapes suivantes, puis réessayez cette étape.
      1. Cliquez sur **Paramètres > Interfaces réseau**, puis sur l’interface réseau correspondante.
      2. Cliquez sur **Paramètres > Adresse IP**.
      3. Pour **Adresse IP publique**, sélectionnez **Activé**, puis cliquez sur **Adresse IP**.
      4. Si vous disposez d’une adresse IP publique existante à utiliser, sélectionnez-la dans la liste. Sinon, cliquez sur **Créer**, entrez un nom, puis cliquez sur **OK** et **Enregistrer**.
   4. Sur le client, cliquez sur **Se connecter**, puis sur **Utiliser un autre compte**. Entrez le nom d’utilisateur et le mot de passe d’un compte d’administrateur de domaine.
   5. Répondez **Oui** à l’invite concernant le certificat.
2. Installez les outils AD DS :
   1. Dans le Gestionnaire de serveur, cliquez sur **Gérer > Ajouter des rôles et fonctionnalités**.
   2. Cliquez sur **Installation basée sur un rôle ou une fonctionnalité**, puis cliquez sur le serveur AD actuel. Suivez les étapes jusqu’à ce que vous arriviez à la **fonctionnalités** onglet.
   3. Développez **Outils d’administration de serveur distant > Outils d’administration de rôles > Outils AD DS et AD LDS**, puis sélectionnez **Outils AD DS**.
   4. Sélectionnez **Redémarrer automatiquement le serveur de destination si nécessaire**, puis cliquez sur **Installer**.

### <a name="create-a-group"></a>Créer un groupe

Vous pouvez utiliser des groupes AD DS pour accorder l’accès à un ensemble d’utilisateurs ayant besoin d’utiliser les mêmes ressources distantes.

1. Dans le Gestionnaire de serveur sur le serveur exécutant les services AD DS, cliquez sur **Outils > Utilisateurs et ordinateurs Active Directory**.
2. Développez le domaine dans le volet gauche pour afficher ses sous-dossiers.
3. Cliquez avec le bouton droit sur le dossier où vous souhaitez créer le groupe, puis cliquez sur **Nouveau > Groupe**.
4. Entrez un nom de groupe approprié, puis sélectionnez **Global** et **Sécurité**.

### <a name="create-a-user-and-add-to-a-group"></a>Créer un utilisateur et l’ajouter à un groupe
1. Dans le Gestionnaire de serveur sur le serveur exécutant les services AD DS, cliquez sur **Outils > Utilisateurs et ordinateurs Active Directory**.
2. Développez le domaine dans le volet gauche pour afficher ses sous-dossiers.
3. Cliquez avec le bouton droit sur **Utilisateurs**, puis cliquez sur **Nouveau > Utilisateur**.
4. Entrez au minimum un prénom et un nom d’ouverture de session de l’utilisateur.
5. Entrez et confirmez un mot de passe pour l’utilisateur. Définissez les options utilisateur appropriées, comme **L’utilisateur doit changer le mot de passe à la prochaine ouverture de session**.
6. Ajoutez le nouvel utilisateur à un groupe :
   1. Dans le dossier **Utilisateurs**, cliquez avec le bouton droit sur le nouvel utilisateur.
   2. Cliquez sur **Ajouter à un groupe**.
   3. Entrez le nom du groupe auquel vous souhaitez ajouter l’utilisateur.

## <a name="assign-users-and-groups-to-collections"></a>Affecter des utilisateurs et des groupes à des collections
Maintenant que vous avez créé les utilisateurs et les groupes dans Active Directory, vous pouvez ajouter une granularité concernant qui a accès aux collections Bureau à distance dans votre déploiement.

1. Connectez-vous au serveur exécutant le rôle Service Broker pour les connexions Bureau à distance en suivant les étapes décrites plus haut.
2. Ajoutez les autres serveurs Bureau à distance au pool de serveurs gérés du Service Broker pour les connexions Bureau à distance :
   1. Dans le Gestionnaire de serveur, cliquez sur **Gérer > Ajouter des serveurs**.
   2. Cliquez sur **Rechercher**.
   3. Cliquez sur chaque serveur dans votre déploiement qui exécute un rôle Services Bureau à distance, puis cliquez sur **OK**.
3. Modifiez une collection pour accorder l’accès à des utilisateurs ou groupes spécifiques :
   1. Dans le Gestionnaire de serveur, cliquez sur **Services Bureau à distance > Vue d’ensemble**, puis cliquez sur une collection spécifique.
   2. Sous **Propriétés**, cliquez sur **Tâches > Modifier les propriétés**.
   3. Cliquez sur **Groupes d’utilisateurs**.
   4. Cliquez sur **Ajouter** et entrez l’utilisateur ou le groupe auquel vous souhaitez accorder l’accès à la collection. Vous pouvez également supprimer des utilisateurs et des groupes de cette fenêtre en sélectionnant l’utilisateur ou le groupe à supprimer, puis en cliquant sur **Supprimer**. 
   
   >[!NOTE] 
   > La fenêtre Groupes d’utilisateurs ne peut jamais être vide. Pour limiter l’étendue des utilisateurs ayant accès à la collection, vous devez tout d’abord ajouter des utilisateurs ou groupes spécifiques avant de supprimer des groupes plus larges.