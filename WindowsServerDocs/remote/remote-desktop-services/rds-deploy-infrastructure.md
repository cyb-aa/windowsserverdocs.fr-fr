---
title: Déployer un environnement des services Bureau à distance
ms.custom: na
ms.prod: windows-server-threshold
description: Procédure de base pour déployer un environnement de bureau à distance.
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
ms.openlocfilehash: a5a56d56038d94869c5246f8d4d3eae2796616a3
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297438"
---
# Déployer un environnement des services Bureau à distance

>S’applique à: Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016

Utilisez les étapes suivantes pour déployer les services Bureau à distance des serveurs dans votre environnement. Vous pouvez installer les rôles de serveur sur des machines physiques ou des machines virtuelles, selon si vous créez un local, basée sur le cloud ou un environnement hybride. 

Si vous utilisez des machines virtuelles pour un des serveurs des Services Bureau à distance, assurez-vous que vous avez [préparé ces machines virtuelles](rds-prepare-vms.md).
  
  
1.  Ajoutez tous les serveurs que vous allez utiliser pour les Services Bureau à distance au Gestionnaire de serveur:  
    1.  Dans le Gestionnaire de serveur cliquez sur **Gérer > ajouter des serveurs**.  
    2.  Cliquez sur **Rechercher**.  
    3.  Cliquez sur chaque serveur dans le déploiement (par exemple, Contoso-Cb1, Contoso-WebGw1 et Sh1-Contoso), puis cliquez sur **OK**.  
2.  Créer un déploiement basé sur une session pour déployer les composants de Services Bureau à distance:  
    1.  Dans le Gestionnaire de serveur, cliquez sur **Gérer > ajouter des rôles et fonctionnalités**.  
    2.  Cliquez sur **installation de Services Bureau à distance**, **Déploiement Standard**et le **déploiement de bureau basée sur la Session**.  
    3.  Sélectionnez les serveurs appropriés pour le serveur de Broker pour les connexions Bureau à distance, le serveur d’accès Web de bureau à distance et le serveur hôte de Session Bureau à distance (par exemple, Contoso-Cb1, Contoso-WebGw1 et Contoso-SH1, respectivement).  
    4.  Sélectionnez **puis redémarrer le serveur de destination automatiquement si nécessaire**, puis cliquez sur **déployer**.  
    5.  Attendez que le déploiement s’achever  
3.  Ajoutez le serveur de licences des services Bureau à distance:  
    1.  Dans le Gestionnaire de serveur, cliquez sur **> de vue d’ensemble des Services Bureau à distance > + Gestionnaire de licences des services Bureau à distance**.  
    2.  Sélectionnez la machine virtuelle dans laquelle le serveur de licences des services Bureau à distance sera installé (par exemple, Contoso-Cb1).  
    3.  Cliquez sur **suivant**, puis cliquez sur **Ajouter**.  
4.  Activer le serveur de licences des services Bureau à distance et l’ajouter au groupe de serveurs de licences:  
    1.  Dans le Gestionnaire de serveur, cliquez sur **Outils > Services Terminal Server > Gestionnaire de licences de bureau à distance**.  
    2.  Dans le Gestionnaire de licences des services Bureau à distance, sélectionnez le serveur, puis cliquez sur **Action > activer serveur**.  
    3.  Acceptez les valeurs par défaut dans l’Assistant Activation du serveur accepter les paramètres par défaut jusqu'à ce que vous atteigniez la page **d’informations sur la société** . Ensuite, entrez les informations de votre société.  
    4.  Acceptez les valeurs par défaut pour les autres pages jusqu'à ce que la dernière page. Désactivez **Démarrer l’Assistant Installation de licences**, puis cliquez sur **Terminer**.  
    5.  Cliquez sur **Action > revoir la Configuration > ajouter au groupe > OK**. Entrez les informations d’identification d’un utilisateur dans le groupe d’administrateurs de contrôleur de domaine AAD et s’inscrire en tant que SCP. Cette étape peut ne pas fonctionne si vous utilisez des Services de domaine Azure AD, mais vous pouvez ignorer les avertissements ou erreurs.  
5.  Ajoutez le nom de certificat et le serveur de passerelle Bureau à distance:  
    1.  Dans le Gestionnaire de serveur, cliquez sur **> de vue d’ensemble des Services Bureau à distance > + passerelle des services Bureau à distance**.  
    2.  Dans l’Assistant Ajout de serveurs de passerelle Bureau à distance, sélectionnez la machine virtuelle dans lequel vous voulez installer le serveur de passerelle Bureau à distance (par exemple, Contoso-WebGw1).  
    3.  Entrez le nom de certificat SSL pour le serveur de passerelle Bureau à distance à l’aide de l’externe complet DNS nom (FQDN) du serveur passerelle des services Bureau à distance. Dans Azure, cela correspond à l’étiquette de **nom DNS** et utilise le format servicename.location.cloudapp.azure.com. Par exemple, contoso.westus.cloudapp.azure.com.  
    4.  Cliquez sur **suivant**, puis cliquez sur **Ajouter**.
6.  Créer et installer des certificats auto-signés pour les serveurs de passerelle Bureau à distance et Broker pour les connexions Bureau à distance.

       > [!NOTE]
       > Si vous êtes en fournissant et l’installation de certificats à partir d’une autorité de certification approuvée, effectuez les procédures à partir de l’étape h à l’étape k pour chaque rôle. Vous devez obtenir le fichier .pfx disponible pour chacun de ces certificats.
       
    1.  Dans le Gestionnaire de serveur, cliquez sur **Services Bureau à distance > vue d’ensemble > tâches > modifier les propriétés du déploiement**.  
    2.  Développez **certificats**, puis faites défiler jusqu'à la table. Sur la **passerelle des services Bureau à distance > créer un nouveau certificat**.  
    3.  Entrez le nom de certificat en utilisant le nom de domaine complet externe du serveur passerelle des services Bureau à distance (par exemple, contoso.westus.cloudapp.azure.com), puis entrez le mot de passe.  
    4.  Sélectionnez **Store ce certificat** , puis accédez au dossier partagé que vous avez créé pour les certificats dans une étape précédente. (Par exemple, \Contoso-Cb1\Certificates.)  
    5.  Entrez un nom de fichier pour le certificat (par exemple, ContosoRdGwCert), puis cliquez sur **Enregistrer**.  
    6.  Sélectionnez **Autoriser le certificat à ajouter au magasin de certificats Autorités de certification racine de confiance sur les ordinateurs de destination**, puis cliquez sur **OK**.  
    7.  Cliquez sur **Appliquer**et attendez que le certificat à appliquer correctement au serveur de passerelle des services Bureau à distance.  
    8.  Cliquez sur **l’accès Web Bureau à distance > sélectionner le certificat existant**.  
    9.  Accédez au certificat créé pour le serveur de passerelle Bureau à distance (par exemple, ContosoRdGwCert), puis cliquez sur **Ouvrir**.  
    10. Entrez le mot de passe pour le certificat, sélectionnez **Autoriser le certificat à ajouter dans le magasin de certificat racine approuvé sur les ordinateurs de destination**, puis cliquez sur **OK**.  
    11. Cliquez sur **Appliquer**et attendez que le certificat à appliquer correctement au serveur d’accès Web de bureau à distance.  
    12. Répétez les étapes 1 à 11 pour le **Broker pour les connexions Bureau à distance - activer Single Sign On** et **Broker pour les connexions Bureau à distance - services de publication**, à l’aide du nom FQDN du serveur Broker pour les connexions Bureau à distance pour le nouveau nom du certificat (par exemple, Contoso-Cb1.Contoso.com).  
7.  Exporter les certificats auto-signés publiques et les copier sur un ordinateur client. Si vous utilisez des certificats à partir d’une autorité de certification approuvée, vous pouvez ignorer cette étape.  
    1.  Lancez certlm.msc.  
    2.  Développez **personnel**, puis cliquez sur **les certificats**.  
    3.  Dans le volet droit, cliquez sur le certificat de Broker pour les connexions Bureau à distance conçu pour l’authentification du client, par exemple **Contoso-Cb1.Contoso.com**.  
    4.  Cliquez sur **toutes les tâches > exporter**.  
    5.  Acceptez que les options par défaut dans l’Assistant Exportation de certificat acceptent les valeurs par défaut jusqu'à ce que vous atteigniez la page du **fichier d’exportation** .  
    6.  Recherchez le dossier partagé que vous avez créé pour les certificats, par exemple \Contoso-Cb1\Certificates.  
    7.  Entrez un nom de fichier, par exemple ContosoCbClientCert et cliquez sur **Enregistrer**.  
    8.  Cliquez sur **suivant**, puis cliquez sur **Terminer**.  
    9.  Répétez les étapes 1 à 8 pour la passerelle des services Bureau à distance et d’un certificat Web, (par exemple contoso.westus.cloudapp.azure.com), ce qui donne le certificat exporté un nom de fichier approprié, par exemple **ContosoWebGwClientCert**.  
    10. Dans l’Explorateur de fichiers, accédez au dossier dans lequel les certificats sont stockés, par exemple \Contoso-Cb1\Certificates.  
    11. Sélectionnez les deux certificats client exporté, puis cliquez sur les, puis cliquez sur **copie**.  
    12. Collez le publiquement sur l’ordinateur client local.  
8.  Configurer les propriétés de déploiement passerelle des services Bureau à distance et les licences des services Bureau à distance:  
    1.  Dans le Gestionnaire de serveur, cliquez sur **Services Bureau à distance > vue d’ensemble > tâches > modifier les propriétés du déploiement**.  
    2.  Développez la **Passerelle des services Bureau à distance** , puis désactivez l’option de **serveur de passerelle des services Bureau à distance de contournement pour les adresses locales** .  
    3.  Développez le **Gestionnaire de licences des services Bureau à distance** et sélectionnez **Par utilisateur**  
    4.  Cliquez sur **OK**.  
10. Créer un regroupement de session. Ces étapes créent une collection de base. Regardez [créer une collection de Services Bureau à distance pour les ordinateurs de bureau et les applications de s’exécuter](rds-create-collection.md) pour plus d’informations sur les collections.
 
    1.  Dans le Gestionnaire de serveur, cliquez sur **Services Bureau à distance > Collections > tâches > créer une Collection de Session**.  
    2.  Entrez une nom (par exemple, ContosoDesktop) de la collection.  
    3.  Sélectionnez un serveur hôte de Session Bureau à distance (Contoso-Sh1), acceptez les groupes d’utilisateurs par défaut (les utilisateurs Contoso) et entrez le chemin d’accès UNC Universal Naming Convention () pour les disques de profil utilisateur créés ci-dessus (\Contoso-Cb1\UserDisks).  
    4.  Définir une taille maximale, puis cliquez sur **créer**.  
  

Vous venez de créer une infrastructure de Services Bureau à distance de base. Si vous avez besoin créer un déploiement hautement disponible, vous pouvez ajouter un [cluster du service Broker pour les connexion](rds-connection-broker-cluster.md) ou un [second serveur hôte de Session Bureau à distance](rds-scale-rdsh-farm.md).

