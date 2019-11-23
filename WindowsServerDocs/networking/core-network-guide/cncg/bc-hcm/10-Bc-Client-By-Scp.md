---
title: Configurer la découverte automatique du cache hébergé par le point de connexion de service
description: Ce guide fournit des instructions sur le déploiement de BranchCache en mode de cache hébergé sur les ordinateurs exécutant Windows Server 2016 et Windows 10
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: ea1c34fd-5a33-4228-9437-9bb3d44230eb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 60ccc8b80537da0d0b689f6c508c75ef15a339c5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406392"
---
#  <a name="configure-client-automatic-hosted-cache-discovery-by-service-connection-point"></a>Configurer la découverte automatique du cache hébergé par le point de connexion de service

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Avec cette procédure, vous pouvez utiliser stratégie de groupe pour activer et configurer le mode de cache hébergé de BranchCache sur un domaine\-ordinateurs joints qui exécutent les systèmes d’exploitation Windows BranchCache\-s suivants.

- Windows 10 Entreprise
- Windows 10 Éducation
- Windows 8.1 Entreprise
- Windows 8 Entreprise

> [!NOTE]  
> Pour configurer des ordinateurs joints à un domaine qui exécutent Windows Server 2008 R2 ou Windows 7, consultez le Guide de [déploiement](https://technet.microsoft.com/library/ee649232.aspx)de windows Server 2008 R2 BranchCache.

Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs du domaine** ou à un groupe équivalent.

### <a name="to-use-group-policy-to-configure-clients-for-hosted-cache-mode"></a>Pour utiliser stratégie de groupe pour configurer des clients pour le mode de cache hébergé

1. Sur un ordinateur sur lequel est installé le rôle de serveur Active Directory Domain Services, ouvrez Gestionnaire de serveur, sélectionnez le serveur local, cliquez sur **Outils**, puis sur **gestion des stratégie de groupe**. La console de gestion stratégie de groupe s’ouvre.

2. Dans la console de gestion de stratégie de groupe, développez le chemin suivant : **Forest :** *Corp.contoso.com*, **Domains**, *corp.contoso.com*, **stratégie de groupe Objects**, où *Corp.contoso.com* est le nom du domaine dans lequel se trouvent les comptes d’ordinateur client BranchCache que vous souhaitez configurer.

3. Cliquez avec le bouton droit\-sur **stratégie de groupe objets**, puis cliquez sur **nouveau**. La boîte de dialogue **Nouvel objet GPO** s'ouvre. Dans **nom**, tapez un nom pour le nouvel objet stratégie de groupe \(\)d’objets de stratégie de groupe. Par exemple, si vous voulez nommer l'objet Ordinateurs clients BranchCache, tapez **Ordinateurs clients BranchCache**. Cliquez sur **OK**.

4. Dans la console de gestion stratégie de groupe, assurez-vous que **stratégie de groupe objets** est sélectionné, et dans le\-volet d’informations, cliquez sur l’objet de stratégie de groupe que vous venez de créer. Par exemple, si vous avez nommé votre objet de stratégie de groupe ordinateurs clients BranchCache, cliquez avec le bouton droit\-sur **ordinateurs clients BranchCache**. Cliquez sur **Modifier**. La console Éditeur de gestion des stratégies de groupe s’ouvre.

5. Dans la console Éditeur de gestion des stratégies de groupe, développez le chemin suivant : **Configuration ordinateur**, **stratégies**, **Modèles d’administration : définitions de stratégie \(fichiers ADMX\) récupérés à partir de l’ordinateur local**, **réseau**, **BranchCache**.

6. Cliquez sur **BranchCache**, puis dans le volet d’informations, double\-cliquez sur **activer BranchCache**. La boîte de dialogue **Activer BranchCache** s'ouvre.
  
7.  Dans la boîte de dialogue **Activer BranchCache**, cliquez sur **Activé**, puis sur **OK**.

8. Dans la console Éditeur de gestion des stratégies de groupe, assurez-vous que **BranchCache** est toujours sélectionné, puis dans le volet d’informations double\-cliquez sur **activer la découverte automatique du cache hébergé par le point de connexion de service**. La boîte de dialogue paramètre de stratégie s’ouvre.

9. Dans la boîte de dialogue **activer la découverte automatique du cache hébergé par le point de connexion de service** , cliquez sur **activé**, puis sur **OK**.

10. Pour permettre aux ordinateurs clients de télécharger et de mettre en cache du contenu à partir des serveurs de contenu\-basés sur le serveur de fichiers BranchCache : dans la console Éditeur de gestion des stratégies de groupe, assurez-vous que **BranchCache** est toujours sélectionné, puis dans le volet d’informations double\-cliquez sur **BranchCache pour fichiers réseau**. La boîte de dialogue **Configurer BranchCache pour les fichiers réseau** s'ouvre. 
11. Dans la boîte de dialogue **Configurer BranchCache pour les fichiers réseau**, cliquez sur **Activé**. Dans **Options**, tapez une valeur numérique, en millisecondes, pour la durée de latence réseau maximale aller-retour, puis cliquez sur **OK**.
  
    > [!NOTE]
    > Par défaut, les ordinateurs clients cachent du contenu à partir de serveurs de fichiers si la latence du réseau de boucles est supérieure à 80 millisecondes.
  
12. Pour configurer la quantité d’espace disque disponible sur chaque ordinateur client pour le cache BranchCache : dans la console Éditeur de gestion des stratégies de groupe, assurez-vous que **BranchCache** est toujours sélectionné, puis dans le volet d’informations double\-cliquez sur **définir le pourcentage d’espace disque utilisé pour le cache de l’ordinateur client**. La boîte de dialogue **Définir le pourcentage d'espace disque utilisé pour la mémoire cache de l'ordinateur client** s'ouvre. Cliquez sur **Activé**, et puis dans **Options** tapez une valeur numérique qui représente le pourcentage d'espace disque utilisé sur chaque ordinateur client pour le cache BranchCache. Cliquez sur **OK**.

13. Pour spécifier l’ancienneté par défaut, en jours, pendant laquelle les segments sont valides dans le cache de données BranchCache sur les ordinateurs clients : dans la console Éditeur de gestion des stratégies de groupe, assurez-vous que **BranchCache** est toujours sélectionné, puis, dans le volet d’informations double\-cliquez sur **définir l’âge des segments dans le cache de données**. La boîte de dialogue **définir l’âge des segments dans le cache de données** s’ouvre. Cliquez sur **activé**, puis dans le volet d’informations, tapez le nombre de jours que vous préférez. Cliquez sur **OK**.

14. Configurez des paramètres de stratégie BranchCache supplémentaires pour les ordinateurs clients en fonction de votre déploiement.

15. Actualisez stratégie de groupe sur les ordinateurs clients des succursales en exécutant la commande **gpupdate/force**ou en redémarrant les ordinateurs clients.

Le déploiement du mode de cache hébergé de BranchCache est maintenant terminé.

Pour plus d’informations sur les technologies de ce guide, consultez [ressources supplémentaires](11-Bc-Hcm-additional-resources.md).