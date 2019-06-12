---
title: Déployer un environnement des services Bureau à distance
ms.custom: na
ms.prod: windows-server-threshold
description: Étapes de base pour déployer un environnement de bureau à distance.
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
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66805133"
---
# <a name="deploy-your-remote-desktop-environment"></a>Déployer un environnement des services Bureau à distance

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016

Utilisez les étapes suivantes pour déployer les serveurs Bureau à distance dans votre environnement. Vous pouvez installer les rôles de serveur sur les ordinateurs physiques ou des machines virtuelles, selon que vous créez un local, basé sur le cloud ou environnement hybride. 

Si vous utilisez des machines virtuelles pour tous les serveurs des Services Bureau à distance, assurez-vous que vous avez [préparé ces machines virtuelles](rds-prepare-vms.md).
  
  
1.  Ajoutez tous les serveurs que vous allez utiliser pour les Services Bureau à distance au Gestionnaire de serveur :  
    1.  Dans le Gestionnaire de serveur, cliquez sur **gérer** > **ajouter des serveurs**.  
    2.  Cliquez sur **Rechercher**.  
    3.  Cliquez sur chaque serveur dans le déploiement (par exemple, Contoso-Cb1 WebGw1 de Contoso et Sh1 de Contoso) et cliquez sur **OK**.  
2.  Créer un déploiement basé sur session pour déployer les composants des Services Bureau à distance :  
    1.  Dans le Gestionnaire de serveur, cliquez sur **gérer** > **Ajout de rôles et fonctionnalités**.  
    2.  Cliquez sur **installation des Services Bureau à distance**, **déploiement Standard**, et **déploiement de postes de travail basé sur Session**.  
    3.  Sélectionnez les serveurs appropriés pour le serveur de service Broker pour les connexions Bureau à distance, le serveur d’accès Web de bureau à distance et le serveur hôte de Session Bureau à distance (par exemple, Contoso-Cb1, Contoso-WebGw1, Contoso-SH1, respectivement).  
    4.  Sélectionnez **redémarrer automatiquement le serveur de destination si nécessaire**, puis cliquez sur **déployer**.  
    5.  Attendez que le déploiement se termine avec succès  
3.  Ajoutez le serveur de licences des services Bureau à distance :  
    1.  Dans le Gestionnaire de serveur, cliquez sur **Services Bureau à distance > vue d’ensemble > + licence des services Bureau à distance**.  
    2.  Sélectionnez la machine virtuelle où le serveur de licences bureau à distance sera être installé (par exemple, Contoso-Cb1).  
    3.  Cliquez sur **suivant**, puis cliquez sur **ajouter**.  
4.  Activer le serveur de licences bureau à distance et l’ajouter au groupe de serveurs de licences :  
    1.  Dans le Gestionnaire de serveur, cliquez sur **Outils > Services Terminal Server > Gestionnaire de licences bureau à distance**.  
    2.  Dans le Gestionnaire de licences des services Bureau à distance, sélectionnez le serveur, puis cliquez sur **Action > activer le serveur**.  
    3.  Acceptez les valeurs par défaut dans l’Assistant Activation du serveur acceptant les valeurs par défaut jusqu'à ce que vous atteigniez le **informations sur la société** page. Ensuite, entrez les informations de votre société.  
    4.  Acceptez les valeurs par défaut pour les pages restantes jusqu'à ce que la dernière page. Effacer **démarrer l’Assistant Installation de licences**, puis cliquez sur **Terminer**.  
    5.  Cliquez sur **Action > Passez en revue la Configuration > Ajouter au groupe > OK**. Entrez les informations d’identification d’un utilisateur dans le groupe AAD DC Administrators et inscrire en tant que SCP. Cette étape peut ne pas fonctionne si vous utilisez Azure AD Domain Services, mais vous pouvez ignorer les avertissements ou erreurs.  
5.  Ajoutez le nom de serveur et le certificat de passerelle Bureau à distance :  
    1.  Dans le Gestionnaire de serveur, cliquez sur **Services Bureau à distance > vue d’ensemble > + passerelle Bureau à distance**.  
    2.  Dans l’Assistant Ajouter des serveurs de passerelle Bureau à distance, sélectionnez l’ordinateur virtuel dans lequel vous souhaitez installer le serveur de passerelle Bureau à distance (par exemple, Contoso-WebGw1).  
    3.  Entrez le nom du certificat SSL pour le serveur de passerelle Bureau à distance à l’aide de l’externe entièrement DNS nom complet (FQDN) du serveur de passerelle Bureau à distance. Dans Azure, il s’agit du **nom DNS** étiqueter et utilise le format servicename.location.cloudapp.azure.com. Par exemple, contoso.westus.cloudapp.azure.com.  
    4.  Cliquez sur **suivant**, puis cliquez sur **ajouter**.
6.  Créer et installer des certificats auto-signés pour les serveurs de passerelle Bureau à distance et service Broker pour les connexions Bureau à distance.

       > [!NOTE]
       > Si vous êtes en fournissant et installation de certificats à partir d’une autorité de certification approuvée, effectuez les procédures à partir de l’étape h à l’étape k pour chaque rôle. Vous devez avoir le fichier .pfx disponible pour chacun de ces certificats.
       
    1.  Dans le Gestionnaire de serveur, cliquez sur **Services Bureau à distance > vue d’ensemble > tâches > modifier les propriétés de déploiement**.  
    2.  Développez **certificats**, puis faites défiler jusqu'à la table. Cliquez sur **passerelle Bureau à distance > créer un certificat**.  
    3.  Entrez le nom du certificat, à l’aide du nom de domaine complet externe du serveur de passerelle Bureau à distance (par exemple, contoso.westus.cloudapp.azure.com), puis entrez le mot de passe.  
    4.  Sélectionnez **Store ce certificat** puis accédez au dossier partagé que vous avez créé pour les certificats à l’étape précédente. (Par exemple, \Contoso-Cb1\Certificates.)  
    5.  Entrez un nom de fichier du certificat (par exemple, ContosoRdGwCert), puis cliquez sur **enregistrer**.  
    6.  Sélectionnez **autoriser le certificat doit être ajouté au magasin de certificats Autorités de certification racine de confiance sur les ordinateurs de destination**, puis cliquez sur **OK**.  
    7.  Cliquez sur **appliquer**, puis attendez que le certificat à appliquer avec succès sur le serveur de passerelle Bureau à distance.  
    8.  Cliquez sur **RD Web Access > Sélectionner un certificat existant**.  
    9.  Accédez au certificat créé pour le serveur de passerelle Bureau à distance (par exemple, ContosoRdGwCert), puis cliquez sur **Open**.  
    10. Entrez le mot de passe du certificat, sélectionnez **autoriser le certificat doit être ajouté au magasin de certificat racine approuvé sur les ordinateurs de destination**, puis cliquez sur **OK**.  
    11. Cliquez sur **appliquer**, puis attendez que le certificat à appliquer au serveur d’accès Web de bureau à distance.  
    12. Répétez des sous-étapes 1 à 11 pour le **Broker pour les connexions Bureau à distance - activer l’authentification unique** et **Broker pour les connexions Bureau à distance - publication de services**, utilisant le FQDN interne du serveur Service Broker pour les connexions Bureau à distance pour le nouveau nom du certificat (par exemple, Contoso-Cb1.Contoso.com).  
7.  Exporter les certificats publics auto-signés et les copier sur un ordinateur client. Si vous utilisez des certificats à partir d’une autorité de certification approuvée, vous pouvez ignorer cette étape.  
    1.  Lancez certlm.msc.  
    2.  Développez **personnel**, puis cliquez sur **certificats**.  
    3.  Dans le volet de droite, cliquez sur le certificat de service Broker pour les connexions Bureau à distance destiné à l’authentification du client, par exemple **Contoso-Cb1.Contoso.com**.  
    4.  Cliquez sur **toutes les tâches > Exporter**.  
    5.  Acceptez les options par défaut dans l’Assistant Exportation de certificat acceptent les valeurs par défaut jusqu'à ce que vous atteigniez le **fichier à exporter** page.  
    6.  Accédez au dossier partagé que vous avez créé pour les certificats, par exemple \Contoso-Cb1\Certificates.  
    7.  Entrez un nom de fichier, par exemple ContosoCbClientCert et puis cliquez sur **enregistrer**.  
    8.  Cliquez sur **Suivant**, puis sur **Terminer**.  
    9.  Répétez les étapes 1 à 8 pour le certificat de passerelle Bureau à distance et Web, (par exemple contoso.westus.cloudapp.azure.com), ce qui donne le certificat exporté un nom de fichier approprié, par exemple **ContosoWebGwClientCert**.  
    10. Dans l’Explorateur de fichiers, accédez au dossier où les certificats sont stockés, par exemple \Contoso-Cb1\Certificates.  
    11. Sélectionnez les deux certificats de client exporté, puis effectuez un clic droit, puis cliquez sur **copie**.  
    12. Collez le dirigez sur l’ordinateur client local.  
8.  Configurer les propriétés de déploiement de passerelle Bureau à distance et les licences des services Bureau à distance :  
    1.  Dans le Gestionnaire de serveur, cliquez sur **Services Bureau à distance > vue d’ensemble > tâches > modifier les propriétés de déploiement**.  
    2.  Développez **passerelle Bureau à distance** et désactivez le **serveur de passerelle Bureau à distance de contournement pour les adresses locales** option.  
    3.  Développez **Gestionnaire de licences bureau à distance** et sélectionnez **par utilisateur**  
    4.  Cliquez sur **OK**.  
10. Créer une collection de sessions. Les étapes suivantes créent une collection de base. Découvrez [créer une collection de Services Bureau à distance pour les ordinateurs de bureau et des applications pour exécuter](rds-create-collection.md) pour plus d’informations sur les collections.
 
    1.  Dans le Gestionnaire de serveur, cliquez sur **Services Bureau à distance > Collections > tâches > créer une Collection de sessions**.  
    2.  Entrez une nom (par exemple, ContosoDesktop) de la collection.  
    3.  Sélectionnez un serveur hôte de Session Bureau à distance (Contoso-Sh1), accepter les groupes d’utilisateurs par défaut (utilisateurs de Contoso) et entrez le chemin d’accès UNC Universal Naming Convention () pour les disques de profil utilisateur créés ci-dessus (\Contoso-Cb1\UserDisks).  
    4.  Définir une taille maximale, puis cliquez sur **créer**.  
  

Vous venez de créer une infrastructure de Services Bureau à distance de base. Si vous avez besoin créer un déploiement hautement disponible, vous pouvez ajouter un [cluster du service Broker pour les connexions](rds-connection-broker-cluster.md) ou un [second serveur hôte de Session Bureau à distance](rds-scale-rdsh-farm.md).

