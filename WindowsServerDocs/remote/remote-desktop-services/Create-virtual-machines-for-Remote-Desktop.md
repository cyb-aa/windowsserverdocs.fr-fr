---
title: Créer des machines virtuelles pour les services Bureau à distance
description: Créez des machines virtuelles pour héberger des composants de Bureau à distance dans le cloud.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/01/2016
ms.topic: article
ms.assetid: b0f62d6f-0915-44ca-afef-be44a922e20e
author: lizap
manager: dongill
ms.openlocfilehash: fa17c472e3311e4e34ac7b2176d0045886463274
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80818462"
---
# <a name="create-virtual-machines-for-remote-desktop"></a>Créer des machines virtuelles pour les services Bureau à distance

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

Suivez les étapes ci-après pour créer des machines virtuelles dans l’environnement du locataire, qui seront utilisées pour exécuter les rôles, services et fonctionnalités Windows Server 2016 nécessaires au déploiement de l’hébergement de bureaux.   
  
Pour cet exemple de déploiement de base, au minimum 3 machines virtuelles sont créées. Une machine virtuelle héberge les services de rôle Service Broker pour les connexions Bureau à distance et Serveur de licences des services Bureau à distance ainsi qu’un partage de fichiers pour le déploiement. Une deuxième machine virtuelle héberge les services de rôle Passerelle des services Bureau à distance et Accès web des services Bureau à distance.  Une troisième machine virtuelle héberge le service de rôle Hôte de session Bureau à distance. Pour les très petits déploiements, vous pouvez réduire les coûts des machines virtuelles en utilisant d’une part le proxy d’application AAD afin d’éliminer tous les points de terminaison publics du déploiement, et en combinant d’autre part tous les services de rôle sur une seule machine virtuelle. Pour les déploiements plus importants, vous pouvez installer les divers services de rôle sur des machines virtuelles individuelles afin de permettre une meilleure scalabilité.  
  
Cette section décrit les étapes nécessaires au déploiement de machines virtuelles pour chaque rôle basé sur des images Windows Server dans la [Place de marché Microsoft Azure](https://azure.microsoft.com/marketplace/). Si vous devez créer des machines virtuelles à partir d’une image personnalisée nécessitant PowerShell, consultez [Créer une machine virtuelle Windows avec Resource Manager et PowerShell](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-ps-create/). Retournez ensuite ici afin d’attacher des disques de données Azure pour le partage de fichiers, puis entrez une URL externe pour votre déploiement.  
  
1. [Créez des machines virtuelles Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-hero-tutorial/) pour héberger le serveur du service Broker pour les connexions Bureau à distance, le serveur de licences des services Bureau à distance et le serveur de fichiers.  
  
   Dans notre cas, nous avons utilisé les conventions de nommage suivantes :  
   - Serveur du service Broker pour les connexions Bureau à distance, serveur de licences des services Bureau à distance et serveur de fichiers :   
       - Machine virtuelle : Contoso-Cb1  
       - Groupe à haute disponibilité : CbAvSet    
   - Serveur d’accès web des services Bureau à distance et serveur de passerelle Bureau à distance :   
       - Machine virtuelle : Contoso-WebGw1  
       - Groupe à haute disponibilité : WebGwAvSet  
          
   - Hôte de session Bureau à distance :   
       - Machine virtuelle : Contoso-Sh1  
       - Groupe à haute disponibilité : ShAvSet  
          
   Chaque machine virtuelle utilise le même groupe de ressources.  
2. Créez et attachez un disque de données Azure pour le partage UPD (disque de profil utilisateur) :  
   1.  Dans le portail Azure, cliquez sur **Parcourir > Groupes de ressources**, cliquez sur le groupe de ressources du déploiement, puis cliquez sur la machine virtuelle créée dans le cadre du service Broker pour les connexions Bureau à distance (par exemple Contoso-Cb1).  
   2.  Cliquez sur **Paramètres > Disques > Attacher un nouveau disque**.  
   3.  Acceptez les valeurs par défaut pour le nom et le type.  
   4.  Entrez une taille (en Go) suffisante pour prendre en charge les partages réseau de l’environnement du locataire, notamment les disques de profil utilisateur et les certificats. Vous pouvez compter environ 5 Go par utilisateur prévu  
   5.  Acceptez les valeurs par défaut pour l’emplacement et la mise en cache de l’hôte, puis cliquez sur **OK**.  
3. Créez un équilibreur de charge externe pour accéder au déploiement de manière externe :
   1. Dans le portail Azure, cliquez sur **Parcourir > Équilibreurs de charge**, puis cliquez sur **Ajouter**.
   2. Entrez un **Nom**, sélectionnez **Public** pour le **Type** de l’équilibreur de charge, puis sélectionnez les valeurs appropriées pour les options **Abonnement**, **Groupe de ressources** et **Emplacement**.
   3. Sélectionnez **Choisir une adresse IP publique**, **Créer**, entrez un nom, puis sélectionnez **OK**.
   4. Sélectionnez **Créer** pour créer l’équilibreur de charge.
4. Configurer l’équilibreur de charge externe pour votre déploiement
   1. Dans le portail Azure, cliquez sur **Parcourir > Groupes de ressources**, cliquez sur le groupe de ressources du déploiement, puis sur l’équilibreur de charge que vous avez créé pour ce déploiement.
   2. Ajoutez un pool de serveurs back-end auquel l’équilibreur de charge va envoyer le trafic :
       1. Sélectionnez **Pool de back-ends**, puis **Ajouter**.
       2. Entrez un **Nom**, puis sélectionnez **\+ Ajouter une machine virtuelle**.
       3. Sélectionnez **Groupe à haute disponibilité** et **WebGwAvSet**.
       4. Sélectionnez **Machines virtuelles**, **Contoso-WebGw1**, **Sélectionner**, **OK**, puis **OK**.
   3. Ajoutez une sonde pour que l’équilibreur de charge sache quelles sont les machines actives :
       1. Sélectionnez **Sondes**, puis **Ajouter**.
       2. Entrez un **Nom** (par exemple HTTPS), sélectionnez **TCP**, entrez **Port** 443, puis sélectionnez **OK**.
   4. Entrez des règles d’équilibrage de charge pour équilibrer le trafic entrant :
      1. Sélectionnez **Règles d’équilibrage de charge**, puis **Ajouter**
      2. Entrez un **Nom** (par exemple HTTPS), sélectionnez **TCP** et 443 pour le **Port** et le **Port principal**, respectivement.
          - Pour un déploiement Windows 10 et Windows Server 2016, conservez l’option **Persistance de session** avec la valeur **Aucune**. Sinon, sélectionnez **IP du client**.
      3. Sélectionnez **OK** pour accepter la règle HTTPS.
      4. Créez une règle en sélectionnant **Ajouter**.
      5. Entrez un **Nom** (par exemple UDP), sélectionnez **UDP** et 3391 pour le <strong>Port et le **Port principal</strong>, respectivement.
          - Pour un déploiement Windows 10 et Windows Server 2016, conservez l’option **Persistance de session** avec la valeur **Aucune**. Sinon, sélectionnez **IP du client**.
      6. Sélectionnez **OK** pour accepter la règle UDP.
   5. Entrez une règle NAT entrante pour vous connecter directement à Contoso-WebGw1
       1. Sélectionnez **Règles NAT entrantes**, puis **Ajouter**.
       2. Entrez un **Nom** (par exemple RDP-Contoso-WebGw1), sélectionnez **Personnalisé** pour le service, **TCP** pour le protocole, puis entrez 14000 pour le **Port**.
       3. Sélectionnez **Choisir une machine virtuelle**, puis Contoso-WebGw1.
       4. Sélectionnez **Personnalisé** pour le mappage de port, entrez 3389 pour le **Port cible**, puis sélectionnez **OK**.
5. Entrez une URL/un nom DNS externe pour que votre déploiement y accède de manière externe :  
   1.  Dans le portail Azure, cliquez sur **Parcourir > Groupes de ressources**, cliquez sur le groupe de ressources du déploiement, puis sur l’adresse IP publique que vous avez créée pour l’accès web des services Bureau à distance et la passerelle des services Bureau à distance.  
   2.  Cliquez sur **Configuration**, entrez une étiquette de nom DNS (par exemple contoso), puis cliquez sur **Enregistrer**. Cette étiquette de nom DNS (contoso.westus.cloudapp.azure.com) est le nom DNS qui vous permet de vous connecter à votre serveur d’accès web des services Bureau à distance et à votre serveur de passerelle Bureau à distance.  

