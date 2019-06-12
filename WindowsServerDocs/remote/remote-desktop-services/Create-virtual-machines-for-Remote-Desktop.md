---
title: Créer des machines virtuelles pour les services Bureau à distance
description: Créer des machines virtuelles pour héberger les composants de bureau à distance dans le cloud.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b0f62d6f-0915-44ca-afef-be44a922e20e
author: lizap
manager: dongill
ms.openlocfilehash: 5c61d9f08cb799d6a63a004bedab924a6ba37fdc
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446731"
---
# <a name="create-virtual-machines-for-remote-desktop"></a>Créer des machines virtuelles pour les services Bureau à distance

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016

Utilisez les étapes suivantes pour créer les machines virtuelles dans l’environnement du client qui sera utilisé pour exécuter les rôles Windows Server 2016, les services et les fonctionnalités requises pour un déploiement d’hébergement de bureau.   
  
Pour cet exemple d’un déploiement de base, la valeur minimale de 3 machines virtuelles sera créée. Une machine virtuelle héberge les services de rôle Broker pour les connexions Bureau à distance (RD) et le serveur de licences et d’un partage de fichiers pour le déploiement. Une deuxième machine virtuelle héberge les services de rôle passerelle Bureau à distance et accès Web.  Une troisième machine virtuelle héberger le service de rôle hôte de Session Bureau à distance. Pour les très petits déploiements, vous pouvez réduire les coûts de machine virtuelle à l’aide de Proxy d’application AAD pour éliminer tous les points de terminaison publics du déploiement et combinaison de tous les services de rôle sur une seule machine virtuelle. Pour les déploiements plus importants, vous pouvez installer les divers services de rôle sur des machines virtuelles individuelles pour permettre une meilleure mise à l’échelle.  
  
Cette section décrit les étapes nécessaires pour déployer des machines virtuelles pour chaque rôle basées sur des images Windows Server dans le [Microsoft Azure Marketplace](https://azure.microsoft.com/marketplace/). Si vous avez besoin créer des machines virtuelles à partir d’une image personnalisée, qui nécessite de PowerShell, consultez [créer une machine virtuelle Windows avec Resource Manager et PowerShell](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-ps-create/). Puis revenez ici pour attacher des disques de données Azure pour le partage de fichiers et entrez une URL externe pour votre déploiement.  
  
1. [Créer des machines virtuelles Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-hero-tutorial/) pour héberger le serveur Service Broker pour les connexions Bureau à distance, de serveur de licences des services Bureau à distance et de fichier.  
  
   Dans notre cas, nous avons utilisé les conventions d’affectation de noms suivantes :  
   - Service Broker pour les connexions Bureau à distance, le serveur de licences et le serveur de fichiers :   
       - MACHINE VIRTUELLE : Contoso-Cb1  
       - Groupe à haute disponibilité : CbAvSet    
   - L’accès Web Bureau à distance et serveur de passerelle Bureau à distance :   
       - MACHINE VIRTUELLE : Contoso-WebGw1  
       - Groupe à haute disponibilité : WebGwAvSet  
          
   - Hôte de Session Bureau à distance :   
       - MACHINE VIRTUELLE : Sh1 de Contoso  
       - Groupe à haute disponibilité : ShAvSet  
          
   Chaque machine virtuelle utilise le même groupe de ressources.  
2. Créer et attacher un disque de données Azure pour le partage de disque (UPD) du profil utilisateur :  
   1.  Dans le portail Azure, cliquez sur **Parcourir > groupes de ressources**et cliquez sur le groupe de ressources pour le déploiement, puis cliquez sur la machine virtuelle créée pour le répartiteur de connexion Bureau à distance (par exemple, Contoso-Cb1).  
   2.  Cliquez sur **Paramètres > disques > attacher un nouveau disque**.  
   3.  Acceptez les valeurs par défaut pour le nom et le type.  
   4.  Entrez une taille suffisamment grande pour contenir les partages réseau pour l’environnement du client, y compris les certificats et les disques de profil utilisateur (en Go). Vous pouvez vous en approcher 5 Go par utilisateur que vous prévoyez d’avoir  
   5.  Acceptez les valeurs par défaut pour l’emplacement et le caching d’hôte, puis cliquez sur **OK**.  
3. Créer un équilibreur de charge externe pour accéder à l’extérieur du déploiement :
   1. Dans le portail Azure, cliquez sur **Parcourir > équilibreurs de charge**, puis cliquez sur **ajouter**.
   2. Entrez un **nom**, sélectionnez **Public** en tant que le **Type** d’équilibrage de charge, puis sélectionnez approprié **abonnement**,  **Groupe de ressources**, et **emplacement**.
   3. Sélectionnez **choisir une adresse IP publique**, **créer**, entrez un nom et sélectionnez **Ok**.
   4. Sélectionnez **créer** pour créer l’équilibreur de charge.
4. Configurer l’équilibrage de charge externe pour votre déploiement
   1. Dans le portail Azure, cliquez sur **Parcourir > groupes de ressources**et cliquez sur le groupe de ressources pour le déploiement, puis cliquez sur l’équilibrage de charge que vous avez créé pour le déploiement.
   2. Ajouter un pool principal pour l’équilibrage de charge envoyer le trafic vers :
       1. Sélectionnez **pool principal** et **ajouter**.
       2. Entrez un **nom** et sélectionnez  **\+ ajouter une machine virtuelle**.
       3. Sélectionnez **à haute disponibilité** et **WebGwAvSet**.
       4. Sélectionnez **machines virtuelles**, **Contoso-WebGw1**, **sélectionnez**, **OK**, et **OK**.
   3. Ajouter une sonde pour que l’équilibrage de charge sache quelles machines virtuelles sont actives :
       1. Sélectionnez **sondes** et **ajouter**.
       2. Entrez un **nom** (par exemple, HTTPS), sélectionnez **TCP**, entrez **Port** 443, puis sélectionnez **OK**.
   4. Entrez des règles afin d’équilibrer le trafic entrant d’équilibrage de charge :
      1. Sélectionnez **règles d’équilibrage de charge** et **ajouter**
      2. Entrez un **nom** (par exemple, HTTPS), sélectionnez **TCP**et 443 pour les deux le **Port** et **port principal**.
          - Pour obtenir Windows 10 et le déploiement de Windows Server 2016, laissez **persistance de Session** comme **aucun**, sinon sélectionnez **adresse IP du Client**.
      3. Sélectionnez **OK** pour accepter la règle HTTPS.
      4. Créer une nouvelle règle en sélectionnant **ajouter**.
      5. Entrez un **nom** (par exemple, UDP), sélectionnez **UDP**et 3391 à la fois pour le <strong>port et le ** port principal</strong>.
          - Pour un déploiement de Windows 10 et Windows Server 2016, laissez **persistance de Session** comme **aucun**, sinon sélectionnez **adresse IP du Client**.
      6. Sélectionnez **OK** pour accepter la règle UDP.
   5. Entrez une règle NAT entrante pour vous connecter directement à Contoso-WebGw1
       1. Sélectionnez **les règles NAT entrantes** et **ajouter**.
       2. Entrez un **nom** (par exemple, RDP-Contoso-WebGw1), sélectionnez **Customm** pour le service, **TCP** pour le protocole, puis entrez 14000 pour le **Port**.
       3. Sélectionnez **choisissez une machine virtuelle** et WebGw1 de Contoso.
       4. Sélectionnez **personnalisé** pour le mappage de port, entrez 3389 pour le **port cible**, puis sélectionnez **OK**.
5. Entrez un nom d’URL/DNS externe pour votre déploiement pour y accéder en externe :  
   1.  Dans le portail Azure, cliquez sur **Parcourir > groupes de ressources**et cliquez sur le groupe de ressources pour le déploiement, puis cliquez sur l’adresse IP publique que vous avez créé pour l’accès Web Bureau à distance et passerelle Bureau à distance.  
   2.  Cliquez sur **Configuration**, entrez un nom DNS (par exemple, contoso), puis cliquez sur **enregistrer**. Cette étiquette de nom DNS (contoso.westus.cloudapp.azure.com) est le nom DNS que vous utiliserez pour vous connecter à votre serveur d’accès Web de bureau à distance et passerelle Bureau à distance.  

