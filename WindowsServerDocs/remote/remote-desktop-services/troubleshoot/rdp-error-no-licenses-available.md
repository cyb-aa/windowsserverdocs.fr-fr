---
title: Les clients ne peuvent pas se connecter et voient l’erreur Aucune licence disponible
description: Résolution de l’erreur Aucune licence disponible avec une connexion Bureau à distance
audience: itpro
ms.custom: na
ms.reviewer: rklemen
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 74b4656216d5568f546d1a13722eeec748f1f9b5
ms.sourcegitcommit: c5709021aa98abd075d7a8f912d4fd2263db8803
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/18/2020
ms.locfileid: "76265891"
---
# <a name="clients-cant-connect-and-see-no-licenses-available-error"></a>Les clients ne peuvent pas se connecter et voient l’erreur « Aucune licence disponible »

Cette situation s’applique aux déploiements qui incluent un serveur RDSH et un serveur Gestionnaire de licences des services Bureau à distance.

Commencez par identifier le comportement observé par les utilisateurs :

- La session a été déconnectée, car aucune licence ou aucun serveur de licences n’est disponible.
- L’accès a été refusé en raison d’une erreur de sécurité.

Connectez-vous à l’Hôte de session Bureau à distance en tant qu’administrateur de domaine et ouvrez l’outil de diagnostic des licences des services Bureau à distance. Recherchez des messages tels que les suivants :

  - La période de grâce du serveur hôte de session Bureau à distance a expiré, mais le serveur n’a pas été configuré avec des serveurs de licences. Les connexions au serveur hôte de session Bureau à distance seront refusées tant qu’un serveur de licences ne sera pas configuré pour le serveur hôte de session Bureau à distance.
  - Le serveur de licences \<nom_ordinateur\> n’est pas disponible. Cela peut être dû à des problèmes de connectivité réseau, au fait que le Gestionnaire de licences des services Bureau à distance est arrêté sur le serveur de licences ou au fait qu’il n’est plus disponible.

Ces problèmes ont tendance à être associés aux messages utilisateur suivants :

  - La session a distance a été déconnectée, car aucune licence d’accès client Bureau à distance n’est disponible pour cet ordinateur.
  - La session a distance a été déconnectée, car aucun serveur de licences Bureau à distance n’est disponible pour fournir une licence.

Dans ce cas, [configurez le service Gestionnaire de licences des services Bureau à distance](#configure-the-rd-licensing-service).

Si l’outil de diagnostic des licences des services Bureau à distance signale d’autres problèmes, tels que « Le composant X.224 du protocole RDP a détecté une erreur dans le flux du protocole et a déconnecté le client », il y a peut-être un problème qui affecte les certificats de licences. Ces problèmes ont tendance à être associés à des messages utilisateur tels que les suivants :

Le client n’a pas pu se connecter au serveur Terminal Server en raison d’une erreur de sécurité. Après avoir vérifié que vous êtes connecté au réseau, réessayez de vous connecter au serveur.

Dans ce cas, [actualisez les clés de Registre du certificat X509](#refresh-the-x509-certificate-registry-keys).

## <a name="configure-the-rd-licensing-service"></a>Configurer le service Gestionnaire de licences des services Bureau à distance

La procédure suivante utilise le Gestionnaire de serveur pour apporter les modifications de configuration. Pour plus d’informations sur la façon de configurer et d’utiliser le Gestionnaire de serveur, consultez [Gestionnaire de serveur](../../../administration/server-manager/server-manager.md).

1. Ouvrez le **Gestionnaire de serveur**, puis accédez à **Services Bureau à distance**.
2. Dans **Vue d’ensemble du déploiement**, sélectionnez **Tâches**, puis **Modifier les propriétés de déploiement**.
3. Sélectionnez **Gestionnaire de licences des services Bureau à distance**, puis sélectionnez le mode de licence approprié pour votre déploiement (**Par appareil** ou **Par utilisateur**).
4. Entrez le nom de domaine complet de votre serveur de licences des services Bureau à distance, puis sélectionnez **Ajouter**.
5. Si vous avez plusieurs serveurs de licences des services Bureau à distance, répétez l’étape 4 pour chaque serveur. 
    ![Options de configuration de serveur de licences des services Bureau à distance dans le Gestionnaire de serveur.](../media/troubleshoot-remote-desktop-connections/RDLicensing_Configure.png)

## <a name="refresh-the-x509-certificate-registry-keys"></a>Actualiser les clés de Registre du certificat X509

> [!IMPORTANT]  
> Suivez attentivement les instructions de cette section. Une modification incorrecte du Registre peut entraîner de graves problèmes. Avant de commencer à le modifier, [sauvegardez le Registre](https://support.microsoft.com/help/322756) afin de pouvoir le restaurer en cas de problème.

Pour résoudre ce problème, sauvegardez puis supprimez les clés de Registre du certificat X509, redémarrez l’ordinateur, puis réactivez le serveur Gestionnaire de licences des services Bureau à distance. Suivez les étapes ci-après.

> [!NOTE]
> Appliquez la procédure suivante sur chaque serveur RDSH.

Procédez comme suit pour réactiver le serveur de licences des services Bureau à distance :

1. Ouvrez l’Éditeur du Registre et accédez à **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\RCM**.
2. Dans le menu Registre, sélectionnez **Exporter un fichier du Registre**.
3. Entrez **certificat exporté** dans la zone **Nom de fichier**, puis sélectionnez **Enregistrer**.
4. Cliquez avec le bouton droit sur chacune des valeurs suivantes, sélectionnez **Supprimer**, puis **Oui** pour vérifier la suppression :  
      - **Certificate**
      - **X509 Certificate**
      - **X509 Certificate ID**
      - **X509 Certificate2**
5. Fermez l’Éditeur du Registre et redémarrez le serveur RDSH.