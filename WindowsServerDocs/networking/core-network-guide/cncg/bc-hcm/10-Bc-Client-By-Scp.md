---
title: Configurer la découverte automatique du cache hébergé par le point de connexion de service
description: Ce guide fournit des instructions sur le déploiement de BranchCache en mode de cache hébergé sur les ordinateurs exécutant Windows Server 2016 et Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: ea1c34fd-5a33-4228-9437-9bb3d44230eb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bd77fc76a999517cb8372aec8dfad25b4dd5be3b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829720"
---
#  <a name="configure-client-automatic-hosted-cache-discovery-by-service-connection-point"></a>Configurer la découverte automatique du cache hébergé par le point de connexion de service

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Avec cette procédure, vous pouvez utiliser Stratégie de groupe pour activer et configurer le mode de cache hébergé de BranchCache sur domaine\-joint les ordinateurs qui exécutent le BranchCache suivant\-systèmes d’exploitation Windows.

- Windows 10 Entreprise
- Windows 10 Éducation
- Windows 8.1 Entreprise
- Windows 8 Entreprise

> [!NOTE]  
> Pour configurer les ordinateurs joints au domaine qui exécutent Windows Server 2008 R2 ou Windows 7, consultez Windows Server 2008 R2 [Guide de déploiement BranchCache](https://technet.microsoft.com/library/ee649232.aspx).

Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs du domaine** ou à un groupe équivalent.

### <a name="to-use-group-policy-to-configure-clients-for-hosted-cache-mode"></a>Pour utiliser la stratégie de groupe pour configurer les clients pour le mode de cache hébergé

1. Sur un ordinateur sur lequel le rôle de serveur Services de domaine Active Directory est installé, ouvrez le Gestionnaire de serveur, sélectionnez le serveur Local, cliquez sur **outils**, puis cliquez sur **Group Policy Management**. La console Gestion de stratégie de groupe s’ouvre.

2. Dans la console de gestion des stratégies de groupe, développez le chemin suivant : **Forêt :** *corp.contoso.com*, **domaines**, *corp.contoso.com*, **les objets de stratégie de groupe**, où *corp.contoso.com* est le nom du domaine où se trouvent les comptes d’ordinateur client BranchCache que vous souhaitez configurer.

3. Droite\-cliquez sur **les objets de stratégie de groupe**, puis cliquez sur **New**. La boîte de dialogue **Nouvel objet GPO** s'ouvre. Dans **nom**, tapez un nom pour le nouvel objet de stratégie de groupe \(GPO\). Par exemple, si vous voulez nommer l'objet Ordinateurs clients BranchCache, tapez **Ordinateurs clients BranchCache**. Cliquez sur **OK**.

4. Dans la console de gestion de stratégie de groupe, vérifiez que **les objets de stratégie de groupe** est sélectionné, dans le volet de droite détails\-cliquez sur l’objet de stratégie de groupe que vous venez de créer. Par exemple, si vous avez nommé votre objet stratégie de groupe BranchCache les ordinateurs clients, avec le bouton droit\-cliquez sur **les ordinateurs clients BranchCache**. Cliquez sur **Modifier**. La console de l’éditeur de gestion de stratégie de groupe s’ouvre.

5. Dans la console Éditeur de gestion de stratégie de groupe, développez le chemin suivant : **Configuration ordinateur**, **Stratégies**, **Modèles d'administration : Définitions de stratégie \(fichiers ADMX\) récupérées à partir de l’ordinateur local**, **réseau**, **BranchCache**.

6. Cliquez sur **BranchCache**, puis dans le volet de détails, double-cliquez\-cliquez sur **activer BranchCache**. La boîte de dialogue **Activer BranchCache** s'ouvre.
  
7.  Dans la boîte de dialogue **Activer BranchCache**, cliquez sur **Activé**, puis sur **OK**.

8. Dans la console Éditeur de gestion de stratégie de groupe, vérifiez que **BranchCache** est toujours sélectionné, puis, dans le double du volet détails\-cliquez sur **activer hébergé Cache la découverte automatique par connexion de Service Point**. La boîte de dialogue de paramètre de stratégie s’ouvre.

9. Dans le **activer hébergé Cache la découverte automatique par Point de connexion de Service** boîte de dialogue, cliquez sur **activé**, puis cliquez sur **OK**.

10. Pour activer les ordinateurs clients à télécharger et le contenu du cache du serveur de fichiers BranchCache\-en fonction des serveurs de contenu : Dans la console Éditeur de gestion de stratégie de groupe, vérifiez que **BranchCache** est toujours sélectionné, puis, dans le double du volet détails\-cliquez sur **BranchCache pour fichiers réseau**. La boîte de dialogue **Configurer BranchCache pour les fichiers réseau** s'ouvre. 
11. Dans la boîte de dialogue **Configurer BranchCache pour les fichiers réseau**, cliquez sur **Activé**. Dans **Options**, tapez une valeur numérique, en millisecondes, pour la durée de latence réseau maximale aller-retour, puis cliquez sur **OK**.
  
    > [!NOTE]
    > Par défaut, les ordinateurs clients mettent en cache le contenu à partir de serveurs de fichiers si la latence du réseau aller-retour est supérieure à 80 millisecondes.
  
12. Pour configurer la quantité d'espace disque allouée sur chaque ordinateur client pour le cache BranchCache : Dans la console Éditeur de gestion de stratégie de groupe, vérifiez que **BranchCache** est toujours sélectionné, puis, dans le double du volet détails\-cliquez sur **définir le pourcentage d’espace disque utilisé pour le cache de l’ordinateur client**. La boîte de dialogue **Définir le pourcentage d'espace disque utilisé pour la mémoire cache de l'ordinateur client** s'ouvre. Cliquez sur **Activé**, et puis dans **Options** tapez une valeur numérique qui représente le pourcentage d'espace disque utilisé sur chaque ordinateur client pour le cache BranchCache. Cliquez sur **OK**.

13. Pour spécifier l’âge par défaut, en jours, pour laquelle les segments sont valides dans le cache de données BranchCache sur les ordinateurs clients : Dans la console Éditeur de gestion de stratégie de groupe, vérifiez que **BranchCache** est toujours sélectionné, puis, dans le double du volet détails\-cliquez sur **définir l’âge des segments dans le cache de données**. Le **définir l’âge des segments dans le cache de données** boîte de dialogue s’ouvre. Cliquez sur **activé**, puis, dans le volet d’informations, tapez le nombre de jours que vous préférez. Cliquez sur **OK**.

14. Configurer des paramètres de stratégie de BranchCache supplémentaires pour les ordinateurs clients en fonction de votre déploiement.

15. Actualiser la stratégie de groupe sur les ordinateurs clients des succursales en exécutant la commande **gpupdate /force**, ou en redémarrant les ordinateurs clients.

Votre déploiement en mode de Cache hébergé de BranchCache est maintenant terminée.

Pour plus d’informations sur les technologies de ce guide, consultez [des ressources supplémentaires](11-Bc-Hcm-additional-resources.md).