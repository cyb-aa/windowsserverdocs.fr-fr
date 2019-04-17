---
title: Configurer la découverte automatique du Cache hébergé de Client par Point de connexion de Service
description: Ce guide fournit des instructions sur le déploiement de BranchCache en mode de cache hébergé sur les ordinateurs exécutant Windows Server2016 et Windows10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: ea1c34fd-5a33-4228-9437-9bb3d44230eb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b12fa6f9e11c8816d74c9013dd80b3fa38d0a478
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
#  <a name="configure-client-automatic-hosted-cache-discovery-by-service-connection-point"></a>Configurer la découverte automatique du Cache hébergé de Client par Point de connexion de Service

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016, Windows Server2012R2, Windows Server2012

Avec cette procédure, vous pouvez utiliser Stratégie de groupe pour activer et configurer le mode de cache hébergé de BranchCache sur les ordinateurs appartenant à un domain\ qui exécutent les systèmes d’exploitation Windows compatibles BranchCache\ suivants.

- Windows10 entreprise
- Windows10 éducation
- Windows8.1 entreprise
- Windows8 entreprise

> [!NOTE]  
> Pour configurer les ordinateurs joints au domaine qui exécutent Windows Server2008R2 ou Windows7, consultez le Windows Server2008R2 [Guide de déploiement BranchCache](https://technet.microsoft.com/library/ee649232.aspx).

L’appartenance au groupe **Admins du domaine**, ou équivalent est le minimum requis pour effectuer cette procédure.

### <a name="to-use-group-policy-to-configure-clients-for-hosted-cache-mode"></a>Pour utiliser une stratégie de groupe pour configurer les clients pour le mode de cache hébergé

1. Sur un ordinateur sur lequel le rôle de serveur Services de domaine ActiveDirectory est installé, ouvrez le Gestionnaire de serveur, sélectionnez le serveur Local, cliquez sur **outils**, puis cliquez sur **gestion des stratégies de groupe**. La console de gestion de stratégie de groupe s’ouvre.

2. Dans la console de gestion des stratégies de groupe, développez le chemin suivant: **forêt:***corp.contoso.com*, **domaines**, *corp.contoso.com*, **objets de stratégie de groupe**, où *corp.contoso.com* est le nom du domaine où se trouvent les comptes d’ordinateur client BranchCache que vous souhaitez configurer.

3. Clic droit **objets de stratégie de groupe**, puis cliquez sur **New**. Le **nouvel objet GPO** boîte de dialogue s’ouvre. Dans **nom**, tapez un nom pour la nouvelle stratégie de groupe \(GPO\) d’objet. Par exemple, si vous souhaitez nommer l’objet les ordinateurs clients BranchCache, tapez **les ordinateurs clients BranchCache**. Cliquez sur **OK**.

4. Dans la console Gestion de stratégie de groupe, vérifiez que **objets de stratégie de groupe** sont sélectionné et dans les détails du volet droit-, cliquez sur l’objet de stratégie de groupe que vous venez créé. Par exemple, si vous avez nommé votre objet stratégie de groupe les ordinateurs clients BranchCache, clic droit **les ordinateurs clients BranchCache**. Cliquez sur **modifier**. La console de l’éditeur de gestion de stratégie de groupe s’ouvre.

5. Dans la console Éditeur de gestion de stratégie de groupe, développez le chemin suivant: **Configuration ordinateur**, **stratégies**, **modèles d’administration: stratégie définitions \(ADMX files\) récupérées à partir de l’ordinateur local**, **réseau**, **BranchCache**.

6. Cliquez sur **BranchCache**, puis, dans le volet d’informations, double-cliquant sur **activer BranchCache**. Le **activer BranchCache** boîte de dialogue s’ouvre.
  
7.  Dans le **activer BranchCache** boîte de dialogue, cliquez sur **activé**, puis cliquez sur **OK**.

8. Dans la console Éditeur de gestion de stratégie de groupe, vérifiez que **BranchCache** est encore sélectionné, puis dans le volet détails double-cliqué-cliquez sur **activer hébergé Cache de la découverte automatique par le Point de connexion de Service**. La boîte de dialogue de paramètre de stratégie s’ouvre.

9. Dans le **activer hébergé Cache de la découverte automatique par le Point de connexion de Service** boîte de dialogue, cliquez sur **activé**, puis cliquez sur **OK**.

10. Permettre aux ordinateurs clients de télécharger et mettre en cache les serveurs de contenu basé sur le serveur de fichier de contenu de BranchCache: dans la console Éditeur de gestion de stratégie de groupe, vérifiez que **BranchCache** est encore sélectionné, puis dans le volet détails double-cliqué-cliquez sur **BranchCache pour fichiers réseau**. Le **configurer BranchCache pour fichiers réseau** boîte de dialogue s’ouvre. 
11. Dans le **configurer BranchCache pour fichiers réseau** boîte de dialogue, cliquez sur **activé**. Dans **Options**, tapez une valeur numérique, en millisecondes, pour le temps de latence réseau maximale aller-retour, puis cliquez sur **OK**.
  
    > [!NOTE]
    > Par défaut, les ordinateurs clients mettent en cache le contenu à partir de serveurs de fichiers si la latence du réseau complet comporte plue de 80millisecondes.
  
12. Pour configurer la quantité d’espace disque allouée sur chaque ordinateur client pour le cache BranchCache: dans la console Éditeur de gestion de stratégie de groupe, vérifiez que **BranchCache** est encore sélectionné, puis dans le volet détails double-cliqué-cliquez sur **définir le pourcentage d’espace disque utilisé pour le cache de l’ordinateur client**. Le **définir le pourcentage d’espace disque utilisé pour le cache de l’ordinateur client** boîte de dialogue s’ouvre. Cliquez sur **activé**, puis dans **Options** tapez une valeur numérique qui représente le pourcentage d’espace disque utilisé sur chaque ordinateur client pour le cache BranchCache. Cliquez sur **OK**.

13. Pour spécifier l’âge par défaut, en jours, pour lesquels les segments sont valides dans le cache de données BranchCache sur les ordinateurs clients: dans la console Éditeur de gestion de stratégie de groupe, vérifiez que **BranchCache** est encore sélectionné, puis dans le volet détails double-cliqué-cliquez sur **définir l’âge des segments dans le cache de données**. Le **définir l’âge des segments dans le cache de données** boîte de dialogue s’ouvre. Cliquez sur **activé**, puis, dans le volet d’informations, tapez le nombre de jours que vous préférez. Cliquez sur **OK**.

14. Configurer les paramètres de stratégie BranchCache supplémentaires pour les ordinateurs clients en fonction de votre déploiement.

15. Actualiser la stratégie de groupe sur les ordinateurs clients des succursales en exécutant la commande **gpupdate /force**, ou en redémarrant les ordinateurs clients.

Votre déploiement en mode de Cache hébergé de BranchCache est maintenant terminée.

Pour plus d’informations sur les technologies de ce guide, voir [des ressources supplémentaires](11-Bc-Hcm-additional-resources.md).