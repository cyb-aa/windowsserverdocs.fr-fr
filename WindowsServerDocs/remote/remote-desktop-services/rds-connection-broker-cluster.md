---
title: Ajouter un serveur Service Broker pour les connexions Bureau à distance pour configurer la haute disponibilité dans les services Bureau à distance
description: Découvrez comment ajouter un service Broker pour les connexions Bureau à distance à un déploiement de services Bureau à distance pour la haute disponibilité.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/10/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 2f4fc63c6ff7c1254fda630a8f34188d8fedc8e5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825040"
---
# <a name="add-the-rd-connection-broker-server-to-the-deployment-and-configure-high-availability"></a>Ajouter le serveur du service Broker pour les connexions Bureau à distance au déploiement et configurer la haute disponibilité

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez déployer un cluster de Broker de connexion Bureau à distance (RD Connection Broker) pour améliorer la disponibilité et la mise à l’échelle de votre infrastructure de Services Bureau à distance. 

## <a name="pre-requisites"></a>Conditions préalables

Configurer un serveur d’agir comme un deuxième agent de connexion Bureau à distance - cela peut être un serveur physique ou une machine virtuelle.

Configurer une base de données pour le service Broker de connexion. Vous pouvez utiliser [base de données SQL Azure](https://azure.microsoft.com/documentation/articles/sql-database-get-started/#create-a-new-aure-sql-database) instance ou SQL Server dans votre environnement local. Nous parlons à l’aide de SQL Azure ci-dessous, mais les étapes s’appliquent toujours à SQL Server. Vous devez rechercher la chaîne de connexion pour la base de données et vérifiez que vous disposez du pilote ODBC.

## <a name="step-1-configure-the-database-for-the-connection-broker"></a>Étape 1 : Configurer la base de données pour le service Broker de connexion

1. Rechercher la chaîne de connexion pour la base de données que vous avez créé, vous avez besoin à la fois pour identifier la version du pilote ODBC vous avez besoin et versions ultérieures, lorsque vous configurez la connexion répartiteur proprement dit (étape 3), donc enregistrez la chaîne un endroit où vous pouvez le référencer facilement. Voici comment vous recherchez la chaîne de connexion SQL Azure :  
    1. Dans le portail Azure, cliquez sur **Parcourir > groupes de ressources** et cliquez sur le groupe de ressources pour le déploiement.   
    2. Sélectionnez la base de données SQL que vous venez de créer (par exemple, DB1-CB).   
    3. Cliquez sur **Paramètres > Propriétés > Afficher les chaînes de connexion de base de données**.   
    4. Copiez la chaîne de connexion pour **ODBC (inclut Node.js)**, qui doit ressembler à ceci :   
      
        Driver={SQL Server Native Client 13.0};Server=tcp:cb-sqls1.database.windows.net,1433;Database=CB-DB1;Uid=sqladmin@contoso;Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;   
  
    5. Remplacez « your_password_here » par le mot de passe réel. Vous allez utiliser cette chaîne entière, avec votre mot de passe fourni lors de la connexion à la base de données. 
2. Installer le pilote ODBC sur le nouvel agent de connexion : 
   1. Si vous utilisez une machine virtuelle pour l’agent de connexion, créez une adresse IP publique pour la première Broker pour les connexions Bureau à distance. (Vous devez uniquement si la machine virtuelle de RDM n’a pas déjà une adresse IP publique pour permettre les connexions RDP).
       1. Dans le portail Azure, cliquez sur **Parcourir > groupes de ressources**et cliquez sur le groupe de ressources pour le déploiement, puis cliquez sur la première machine virtuelle de service Broker pour les connexions Bureau à distance (par exemple, Contoso-Cb1).
       2. Cliquez sur **Paramètres > interfaces réseau**, puis cliquez sur l’interface réseau correspondante.
       3. Cliquez sur **Paramètres > adresse IP**.
       4. Pour **adresse IP publique**, sélectionnez **activé**, puis cliquez sur **adresse IP**.
       5. Si vous avez une adresse IP publique existante à utiliser, sélectionnez-le dans la liste. Sinon, cliquez sur **créer**, entrez un nom, puis cliquez sur **OK** , puis **enregistrer**.
   2. Se connecter à la première Broker pour les connexions Bureau à distance :
       1. Dans le portail Azure, cliquez sur **Parcourir > groupes de ressources**et cliquez sur le groupe de ressources pour le déploiement, puis cliquez sur la première machine virtuelle de service Broker pour les connexions Bureau à distance (par exemple, Contoso-Cb1).
       2. Cliquez sur **Connect > Ouvrez** pour ouvrir le client Bureau à distance.
       3. Dans le client, cliquez sur **Connect**, puis cliquez sur **utiliser un autre compte d’utilisateur**. Entrez le nom d’utilisateur et le mot de passe pour un compte d’administrateur de domaine.
       4. Cliquez sur **Oui** lorsque l’avertissement concernant le certificat.
   3. Téléchargez le [pilote ODBC pour SQL Server](https://www.microsoft.com/download/confirmation.aspx?id=50420) qui correspond à la version dans la chaîne de connexion ODBC. Pour l’exemple de chaîne ci-dessus, nous devons installer la version 13 du pilote ODBC.
   4. Copiez le fichier sqlincli.msi dans le premier serveur du service Broker pour les connexions Bureau à distance.   
   5. Ouvrez le fichier sqlincli.msi et installer le client natif.  
   6. Répétez les étapes 1 à 5 pour chaque courtiers de connexion Bureau à distance supplémentaires (par exemple, Contoso-Cb2).
   7. Installez le pilote ODBC sur chaque serveur qui exécutera le répartiteur de connexion.

## <a name="step-2-configure-load-balancing-on-the-rd-connection-brokers"></a>Étape 2 : Configurer l’équilibrage de charge sur les agents de la connexion Bureau à distance 

Si vous utilisez l’infrastructure Azure, vous pouvez créer un [équilibreur de charge Azure](#create-a-load-balancer); si non, vous pouvez définir jusqu'à [tourniquet DNS](#configure-dns-round--robin). 

### <a name="create-a-load-balancer"></a>Créer un équilibreur de charge  
1. Créer un équilibreur de charge Azure   
      1. Dans le portail Azure, cliquez sur **Parcourir > équilibreurs de charge > ajouter**.   
      2. Entrez un nom pour le nouvel équilibreur de charge (par exemple, hacb).   
      3. Sélectionnez **interne** pour le **schéma**, **réseau virtuel** pour votre déploiement (par exemple, Contoso-VNet) et le **sous-réseau** avec tous les de vos ressources (par exemple, par défaut).   
      4. Sélectionnez **statique** pour le **attribution d’adresses IP** et entrez un **adresse IP privée** qui est actuellement pas en cours d’utilisation (par exemple, 10.0.0.32).   
      5. Sélectionnez l’option appropriée **abonnement**, le **groupe de ressources** avec toutes vos ressources et approprié **emplacement**.   
      6. Sélectionnez **Créer**.   
2. Créer un [sonde](https://azure.microsoft.com/documentation/articles/load-balancer-custom-probe-overview/) à analyser les serveurs qui sont actives :   
      1. Dans le portail Azure, cliquez sur **Parcourir > équilibrages de charge**, puis cliquez sur l’équilibreur de charge vous venez de créer (par exemple, CBLB). Cliquez sur **Paramètres**.   
      2. Cliquez sur **sondes > ajouter**.   
      3. Entrez un nom pour la sonde (par exemple, **RDP**), sélectionnez **TCP** en tant que le **protocole**, entrez **3389** pour le **Port**, puis cliquez sur **OK**.   
3. Créer le pool principal des répartiteurs de connexion :   
      1. Dans **paramètres**, cliquez sur **pools d’adresses principales > ajouter**.   
      2. Entrez un nom (par exemple, CBBackendPool), puis cliquez sur **ajouter une machine virtuelle**.  
      3. Choisissez un groupe à haute disponibilité (par exemple, CbAvSet), puis cliquez sur **OK**.   
      3. Cliquez sur **choisir les machines virtuelles**, sélectionnez chaque machine virtuelle, puis cliquez sur **Sélectionner > OK > OK**.   
4. Créer la règle d’équilibrage de charge RDP :   
      1. Dans **paramètres**, cliquez sur **règles d’équilibrage de charge**, puis cliquez sur **ajouter**.   
      2. Entrez un nom (par exemple, RDP), sélectionnez **TCP** pour le **protocole**, entrez **3389** pour les deux **Port** et **port principal** , puis cliquez sur **OK**.   
5. Ajouter un enregistrement DNS pour l’équilibrage de charge :   
      1. Se connecter à la machine virtuelle du serveur RDM (par exemple, Contoso-CB1). Découvrez le [préparer la machine virtuelle Broker de connexion Bureau à distance](Prepare-the-RD-Connection-Broker-VM-for-Remote-Desktop.md) article pour savoir comment vous connectez à la machine virtuelle.   
      2. Dans le Gestionnaire de serveur, cliquez sur **Outils > DNS**.   
      3. Dans le volet gauche, développez **DNS**, cliquez sur l’ordinateur DNS **Zones de recherche directe**, puis cliquez sur votre nom de domaine (par exemple, Contoso.com). (Il peut prendre quelques secondes pour traiter la requête au serveur DNS pour les informations.)  
      4. Cliquez sur **Action > nouvel hôte (A ou AAAA)**.   
      9. Entrez le nom (par exemple, hacb) et l’adresse IP spécifiée précédemment (par exemple, 10.0.0.32).   
  
### <a name="configure-dns-round-robin"></a>Configurer le tourniquet DNS  
  
Les étapes suivantes sont une alternative à la création d’un équilibreur de charge interne Azure.   
  
1. Se connecter au serveur RDM dans le portail Azure. à l’aide du client Connexion Bureau à distance   
2. Créer des enregistrements DNS :   
      1. Dans le Gestionnaire de serveur, cliquez sur **Outils > DNS**.   
      2. Dans le volet gauche, développez **DNS**, cliquez sur l’ordinateur DNS **Zones de recherche directe**, puis cliquez sur votre nom de domaine (par exemple, Contoso.com). (Il peut prendre quelques secondes pour traiter la requête au serveur DNS pour les informations.)  
      3. Cliquez sur **Action** et **nouvel hôte (A ou AAAA)**.   
      4. Entrez le **nom DNS** pour le service Broker pour les connexions Bureau à distance de cluster (par exemple, hacb), puis entrez le **adresse IP** de la première Broker pour les connexions Bureau à distance.   
      5. Répétez les étapes 3 et 4 pour chaque agent de connexion Bureau à distance supplémentaires, fournissant chaque adresse IP unique pour chaque enregistrement.


Par exemple, si les adresses IP pour les deux machines virtuelles de service Broker pour les connexions Bureau à distance sont 10.0.0.8 et 10.0.0.9, vous devez créer deux enregistrements d’hôte DNS :
 - Nom d’hôte : hacb.contoso.com, adresse IP : 10.0.0.8
 - Nom d’hôte : hacb.contoso.com, adresse IP : 10.0.0.9

## <a name="step-3-configure-the-connection-brokers-for-high-availability"></a>Étape 3 : Configurer les agents de connexion pour la haute disponibilité

1. Ajoutez le nouveau serveur de service Broker pour les connexions Bureau à distance au Gestionnaire de serveur :
   1. Dans le Gestionnaire de serveur, cliquez sur **gérer > ajouter des serveurs**.
   2. Cliquez sur **Rechercher**.
   3. Cliquez sur le serveur RD Connection Broker nouvellement créé (par exemple, Contoso-Cb2) et cliquez sur **OK**.
2. Configurer la haute disponibilité pour le service Broker pour les connexions Bureau à distance :
   1. Dans le Gestionnaire de serveur, cliquez sur **Services Bureau à distance > vue d’ensemble**.
   2. Avec le bouton droit **RD Connection Broker**, puis cliquez sur **configurer une haute disponibilité**.
   3. Page de l’Assistant jusqu'à ce que vous arriviez à la section de type de Configuration. Sélectionnez **serveur de base de données partagé**, puis cliquez sur **suivant**.
   4. Entrez le nom DNS pour le cluster de service Broker pour les connexions Bureau à distance.
   5. Entrez la chaîne de connexion pour la base de données SQL et puis parcourir l’Assistant pour établir une haute disponibilité.
3. Ajouter le nouvel agent de connexion Bureau à distance au déploiement
   1. Dans le Gestionnaire de serveur, cliquez sur **Services Bureau à distance > vue d’ensemble**.
   2. Cliquez sur le service Broker pour les connexions Bureau à distance, puis cliquez sur **ajouter un serveur Service Broker pour les connexions Bureau à distance**.
   3. Page Assistant jusqu'à ce que vous arriviez à la sélection du serveur, puis sélectionnez le serveur RD Connection Broker nouvellement créé (par exemple, Contoso-CB2).
   4. Terminez l’Assistant en acceptant les valeurs par défaut.
4. Configurer des certificats approuvés sur les clients et serveurs de service Broker pour les connexions Bureau à distance.

