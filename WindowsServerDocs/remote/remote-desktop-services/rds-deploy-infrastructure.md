---
title: Déployer un environnement des services Bureau à distance
ms.custom: na
ms.prod: windows-server-threshold
description: Étapes de base pour déployer un environnement des services Bureau à distance.
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/10/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 5b9ce1bb87a7a2ad8819235edc412fd095bc2985
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "66805133"
---
# <a name="deploy-your-remote-desktop-environment"></a>Déployer un environnement des services Bureau à distance

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

Effectuez les étapes suivantes pour déployer les serveurs Bureau à distance dans votre environnement. Vous pouvez installer les rôles serveur sur des ordinateurs physiques ou des machines virtuelles, selon que vous créez un environnement local, hybride ou basé sur le cloud. 

Si vous utilisez des machines virtuelles pour certains des serveurs des Services Bureau à distance, assurez-vous d’avoir [préparé ces machines virtuelles](rds-prepare-vms.md).
  
  
1.  Ajoutez tous les serveurs que vous utiliserez pour les Services Bureau à distance au Gestionnaire de serveur :  
    1.  Dans le Gestionnaire de serveur, cliquez sur **Gérer** > **Ajouter des serveurs**.  
    2.  Cliquez sur **Rechercher**.  
    3.  Cliquez sur chaque serveur du déploiement (par exemple, Contoso-Cb1, Contoso-WebGw1 et Contoso-Sh1) et cliquez sur **OK**.  
2.  Créez un déploiement basé sur une session pour déployer les composants des Services Bureau à distance :  
    1.  Dans le Gestionnaire de serveur, cliquez sur **Gérer** > **Ajouter des rôles et fonctionnalités**.  
    2.  Cliquez sur **Installation des services Bureau à distance**, **Déploiement standard**, puis **Déploiement de bureaux basés sur une session**.  
    3.  Sélectionnez les serveurs appropriés pour le serveur du service Broker pour les connexions Bureau à distance, le serveur d’accès Bureau à distance par le Web et le serveur Hôte de session Bureau à distance (par exemple, Contoso-Cb1, Contoso-WebGw1 et Contoso-SH1, respectivement).  
    4.  Sélectionnez **Redémarrer automatiquement le serveur de destination si nécessaire**, puis cliquez sur **Déployer**.  
    5.  Attendez que le déploiement se termine avec succès.  
3.  Ajoutez le serveur de licences des services Bureau à distance :  
    1.  Dans le Gestionnaire de serveur, cliquez sur **Services Bureau à distance > Vue d’ensemble > + Gestionnaire de licences des services Bureau à distance**.  
    2.  Sélectionnez la machine virtuelle où sera installé le serveur de licences des services Bureau à distance (par exemple Contoso-Cb1).  
    3.  Cliquez sur **Suivant**, puis sur **Ajouter**.  
4.  Activez le serveur de licences des services Bureau à distance et ajoutez-le au groupe de serveurs de licences :  
    1.  Dans le Gestionnaire de serveur, cliquez sur **Outils > Services Terminal Server > Gestionnaire de licences des services Bureau à distance**.  
    2.  Dans le Gestionnaire de licences des services Bureau à distance, sélectionnez le serveur, puis cliquez sur **Action > Activer le serveur**.  
    3.  Acceptez les valeurs par défaut dans l’Assistant Activation du serveur jusqu’à ce que vous atteigniez la page **Informations sur la société**. Ensuite, entrez les informations sur votre société.  
    4.  Acceptez les valeurs par défaut pour toutes les autres pages jusqu’à la dernière. Effacez **Démarrer l’Assistant Installation de licences**, puis cliquez sur **Terminer**.  
    5.  Cliquez sur **Action > Examen de la configuration > Ajouter au groupe > OK**. Entrez les informations d’identification d’un utilisateur appartenant au groupe Administrateurs AAD DC et inscrivez-vous en tant que point de connexion de service (SCP). Cette étape peut ne pas fonctionner si vous utilisez Azure AD Domain Services, mais vous pouvez ignorer les avertissements ou erreurs.  
5.  Ajoutez le nom du certificat et du serveur de passerelle Bureau à distance :  
    1.  Dans le Gestionnaire de serveur, cliquez sur **Services Bureau à distance > Vue d’ensemble > + Passerelle des services Bureau à distance**.  
    2.  Dans l’Assistant Ajouter des serveurs de passerelle Bureau à distance, sélectionnez la machine virtuelle où vous souhaitez installer le serveur de passerelle Bureau à distance (par exemple Contoso-WebGw1).  
    3.  Entrez le nom du certificat SSL pour le serveur de passerelle Bureau à distance en utilisant le nom DNS complet (FQDN) du serveur de passerelle Bureau à distance. Dans Azure, il s’agit de l’étiquette **Nom DNS**, au format nom_service.emplacement.cloudapp.azure.com. Par exemple, contoso.westus.cloudapp.azure.com.  
    4.  Cliquez sur **Suivant**, puis sur **Ajouter**.
6.  Créez et installez des certificats auto-signés pour le serveur de passerelle Bureau à distance et le serveur du service Broker pour les connexions Bureau à distance.

       > [!NOTE]
       > Si vous fournissez et installez des certificats à partir d’une autorité de certification approuvée, appliquez les procédures de l’étape h à l’étape k pour chaque rôle. Vous devrez disposer du fichier .pfx pour chacun de ces certificats.
       
    1.  Dans le Gestionnaire de serveur, cliquez sur **Services Bureau à distance > Vue d’ensemble > Tâches > Modifier les propriétés de déploiement**.  
    2.  Développez **Certificats**, puis faites défiler jusqu’à la table. Cliquez sur **Passerelle des services Bureau à distance > Créer un certificat**.  
    3.  Entrez le nom du certificat, en utilisant le nom FQDN externe du serveur de passerelle Bureau à distance (par exemple contoso.westus.cloudapp.azure.com), puis entrez le mot de passe.  
    4.  Sélectionnez **Stocker ce certificat** puis accédez au dossier partagé que vous avez créé pour les certificats à l’étape précédente. (Par exemple \Contoso-Cb1\Certificates.)  
    5.  Entrez un nom de fichier pour le certificat (par exemple ContosoRdGwCert), puis cliquez sur **Enregistrer**.  
    6.  Sélectionnez **Autoriser l’ajout du certificat au magasin de certificats Autorités de certification racines de confiance sur les ordinateurs de destination**, puis cliquez sur **OK**.  
    7.  Cliquez sur **Appliquer**, puis attendez que le certificat soit appliqué avec succès sur le serveur de passerelle Bureau à distance.  
    8.  Cliquez sur **Accès Bureau à distance par le Web > Sélectionner un certificat existant**.  
    9.  Accédez au certificat créé pour le serveur de passerelle Bureau à distance (par exemple ContosoRdGwCert), puis cliquez sur **Ouvrir**.  
    10. Entrez le mot de passe du certificat, sélectionnez **Autoriser l’ajout du certificat au magasin de certificats Autorités de certification racines de confiance sur les ordinateurs de destination**, puis cliquez sur **OK**.  
    11. Cliquez sur **Appliquer**, puis attendez que le certificat soit appliqué au serveur d’accès Bureau à distance par le Web.  
    12. Répétez les sous-étapes 1 à 11 pour **Service Broker pour les connexions Bureau à distance - Activer l’authentification unique** et **Service Broker pour les connexions Bureau à distance - Services de publication**, en utilisant le nom FQDN interne du serveur du service Broker pour les connexions Bureau à distance pour le nom du nouveau certificat (par exemple Contoso-Cb1.Contoso.com).  
7.  Exportez les certificats publics auto-signés et copiez-les sur un ordinateur client. Si vous utilisez des certificats provenant d’une autorité de certification approuvée, vous pouvez ignorer cette étape.  
    1.  Lancez certlm.msc.  
    2.  Développez **Personnel**, puis cliquez sur **Certificats.**  
    3.  Dans le volet droit, cliquez sur le certificat de service Broker pour les connexions Bureau à distance destiné à l’authentification du client, par exemple **Contoso-Cb1.Contoso.com**.  
    4.  Cliquez sur **Toutes les tâches > Exporter**.  
    5.  Acceptez les options par défaut dans l’Assistant Exportation de certificat jusqu’à ce que vous atteigniez la page **Fichier à exporter**.  
    6.  Accédez au dossier partagé que vous avez créé pour les certificats, par exemple \Contoso-Cb1\Certificates.  
    7.  Entrez un nom de fichier, par exemple ContosoCbClientCert, puis cliquez sur **Enregistrer**.  
    8.  Cliquez sur **Suivant**, puis sur **Terminer**.  
    9.  Répétez les sous-étapes 1 à 8 pour le certificat de passerelle Bureau à distance et Web, (par exemple contoso.westus.cloudapp.azure.com), en donnant au certificat exporté un nom de fichier approprié, par exemple **ContosoWebGwClientCert**.  
    10. Dans l’Explorateur de fichiers, accédez au dossier où les certificats sont stockés, par exemple \Contoso-Cb1\Certificates.  
    11. Sélectionnez les deux certificats clients exportés, effectuez un clic droit, puis cliquez sur **Copier**.  
    12. Collez les certificats sur l’ordinateur client local.  
8.  Configurer les propriétés de la Passerelle des services Bureau à distance et du Gestionnaire de licences des services Bureau à distance :  
    1.  Dans le Gestionnaire de serveur, cliquez sur **Services Bureau à distance > Vue d’ensemble > Tâches > Modifier les propriétés de déploiement**.  
    2.  Développez **Passerelle des services Bureau à distance** et désactivez l’option **Ignorer le serveur de passerelle des services Bureau à distance pour les adresses locales**.  
    3.  Développez **Gestionnaire de licences des services Bureau à distance** et sélectionnez **Par utilisateur**  
    4.  Cliquez sur **OK**.  
10. Créez une collection de sessions. Les étapes suivantes créent une collection de base. Pour plus d’informations sur les collections, consultez [Créer une collection de Services Bureau à distance pour les ordinateurs de bureau et des applications à exécuter](rds-create-collection.md).
 
    1.  Dans le gestionnaire de serveur, cliquez sur **Services Bureau à distance > Collections > Tâches > Créer une collection de sessions**.  
    2.  Entrez un nom de collection (par exemple ContosoDesktop).  
    3.  Sélectionnez un serveur Hôte de session Bureau à distance (Contoso-Sh1), acceptez les groupes d’utilisateurs par défaut (Contoso\Utilisateurs du domaine) et entrez le chemin UNC (Universal Naming Convention) des disques de profil utilisateur créés ci-dessus (\Contoso-Cb1\UserDisks).  
    4.  Définissez une taille maximale, puis cliquez sur **Créer**.  
  

Vous venez de créer une infrastructure des Services Bureau à distance de base. Si vous avez besoin de créer un déploiement hautement disponible, vous pouvez ajouter un [cluster du service Broker pour les connexions](rds-connection-broker-cluster.md) ou un [second serveur Hôte de session Bureau à distance](rds-scale-rdsh-farm.md).

