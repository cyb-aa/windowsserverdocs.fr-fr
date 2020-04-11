---
title: Ajouter un serveur du service Broker pour les connexions Bureau à distance afin de configurer la haute disponibilité dans les services Bureau à distance
description: Découvrez comment ajouter un service Broker pour les connexions Bureau à distance à un déploiement des services Bureau à distance à des fins de haute disponibilité.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/10/2017
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: dc6a9fa0d6834f63c9935518e4b2c26320a04082
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852962"
---
# <a name="add-the-rd-connection-broker-server-to-the-deployment-and-configure-high-availability"></a>Ajouter le serveur du service Broker pour les connexions Bureau à distance au déploiement et configurer la haute disponibilité

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

Vous pouvez déployer un cluster du service Broker pour les connexions Bureau à distance afin d’améliorer la disponibilité et d’adapter votre infrastructure des services Bureau à distance. 

## <a name="pre-requisites"></a>Prérequis

Configurez un serveur pour qu’il agisse en tant que second serveur du service Broker pour les connexions Bureau à distance. Il peut s’agir d’un serveur physique ou d’une machine virtuelle.

Configurez une base de données du service Broker pour les connexions. Vous pouvez utiliser une instance [Azure SQL Database](https://azure.microsoft.com/documentation/articles/sql-database-get-started/#create-a-new-aure-sql-database) ou SQL Server dans votre environnement local. Nous évoquons l’utilisation d’Azure SQL ci-dessous, mais les étapes s’appliquent tout de même à SQL Server. Vous devez trouver la chaîne de connexion de la base de données et vérifier que vous disposez du pilote ODBC approprié.

## <a name="step-1-configure-the-database-for-the-connection-broker"></a>Étape 1 : Configurer la base de données du service Broker pour les connexions

1. Recherchez la chaîne de connexion de la base de données que vous avez créée. Vous allez en avoir besoin pour identifier la version du pilote ODBC nécessaire et, plus tard, au moment de configurer le service Broker pour les connexions proprement dit (étape 3). Vous devez donc enregistrer la chaîne à un endroit facile à référencer. Voici comment trouver la chaîne de connexion pour Azure SQL :  
    1. Dans le portail Azure, cliquez sur **Parcourir > Groupes de ressources**, puis cliquez sur le groupe de ressources du déploiement.   
    2. Sélectionnez la base de données SQL que vous venez de créer (par exemple CB-DB1).   
    3. Cliquez sur **Paramètres** > **Propriétés** > **Afficher les chaînes de connexion de la base de données**.   
    4. Copiez la chaîne de connexion pour **ODBC (comprend Node.js)** , qui doit ressembler à ceci :   
      
        ```
        Driver={SQL Server Native Client 13.0};Server=tcp:cb-sqls1.database.windows.net,1433;Database=CB-DB1;Uid=sqladmin@contoso;Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
        ```
  
    5. Remplacez « your_password_here » par le mot de passe réel. Vous allez utiliser cette chaîne complète (mot de passe inclus) au moment de la connexion à la base de données. 
2. Installez le pilote ODBC sur le nouveau service Broker pour les connexions : 
   1. Si vous utilisez une machine virtuelle en tant que service Broker pour les connexions, créez une adresse IP publique destinée au premier service Broker pour les connexions Bureau à distance. (Vous devez uniquement effectuer cette opération si la machine virtuelle RDMS ne dispose pas déjà d’une adresse IP publique pour autoriser les connexions RDP.)
       1. Dans le portail Azure, cliquez sur **Parcourir** > **Groupes de ressources**, cliquez sur le groupe de ressources du déploiement, puis cliquez sur la première machine virtuelle du service Broker pour les connexions Bureau à distance (par exemple Contoso-Cb1).
       2. Cliquez sur **Paramètres > Interfaces réseau**, puis cliquez sur l’interface réseau correspondante.
       3. Cliquez sur **Paramètres > Adresse IP**.
       4. Pour **Adresse IP publique**, sélectionnez **Activé**, puis cliquez sur **Adresse IP**.
       5. Si vous souhaitez utiliser une adresse IP publique existante, sélectionnez-la dans la liste. Sinon, cliquez sur **Créer**, entrez un nom, cliquez sur **OK**, puis sur **Enregistrer**.
   2. Connectez-vous au premier service Broker pour les connexions Bureau à distance :
       1. Dans le portail Azure, cliquez sur **Parcourir** > **Groupes de ressources**, cliquez sur le groupe de ressources du déploiement, puis cliquez sur la première machine virtuelle du service Broker pour les connexions Bureau à distance (par exemple Contoso-Cb1).
       2. Cliquez sur **Connexion > Ouvrir** pour ouvrir le client Bureau à distance.
       3. Dans le client, cliquez sur **Connexion**, puis sur **Utiliser un autre compte d’utilisateur**. Entrez le nom d’utilisateur et le mot de passe d’un compte administrateur de domaine.
       4. Cliquez sur **Oui** après avoir consulté l’avertissement relatif au certificat.
   3. Téléchargez le [pilote ODBC pour SQL Server](https://www.microsoft.com/download/confirmation.aspx?id=50420) qui correspond à la version figurant dans la chaîne de connexion ODBC. Pour l’exemple de chaîne ci-dessus, nous devons installer le pilote ODBC version 13.
   4. Copiez le fichier sqlincli.msi sur le premier serveur du service Broker pour les connexions Bureau à distance.   
   5. Ouvrez le fichier sqlincli.msi, puis installez le client natif.  
   6. Répétez les étapes 1 à 5 avec chaque serveur du service Broker pour les connexions Bureau à distance supplémentaire (par exemple Contoso-Cb2).
   7. Installez le pilote ODBC sur chaque serveur exécutant le service Broker pour les connexions.

## <a name="step-2-configure-load-balancing-on-the-rd-connection-brokers"></a>Étape 2 : Configurer l’équilibrage de charge sur les serveurs du service Broker pour les connexions Bureau à distance 

Si vous utilisez l’infrastructure Azure, vous pouvez créer un [équilibreur de charge Azure](#create-a-load-balancer). Sinon, vous pouvez configurer le [tourniquet (round robin) DNS](#configure-dns-round-robin).

### <a name="create-a-load-balancer"></a>Créer un équilibreur de charge  
1. Créer un équilibreur de charge Azure   
      1. Dans le portail Azure, cliquez sur **Parcourir > Équilibreurs de charge > Ajouter**.   
      2. Entrez un nom pour le nouvel équilibreur de charge (par exemple hacb).   
      3. Sélectionnez **Interne** pour le **Schéma**, sélectionnez **Réseau virtuel** pour le déploiement (par exemple Contoso-VNet), puis sélectionnez le **Sous-réseau**  comprenant toutes vos ressources (par exemple le sous-réseau par défaut).   
      4. Sélectionnez **Statique** pour l’**Affectation d’adresses IP**, puis entrez une **adresse IP privée** non utilisée (par exemple 10.0.0.32).   
      5. Sélectionnez l’**Abonnement** approprié, sélectionnez le **Groupe de ressources** comprenant toutes vos ressources, puis sélectionnez l’**emplacement** approprié.   
      6. Sélectionnez **Créer**.   
2. Créez une [sonde](https://azure.microsoft.com/documentation/articles/load-balancer-custom-probe-overview/) pour superviser les serveurs actifs :   
      1. Dans le portail Azure, cliquez sur **Parcourir > Équilibreurs de charge**, puis cliquez sur l’équilibreur de charge que vous venez de créer (par exemple CBLB). Cliquez sur **Paramètres**.   
      2. Cliquez sur **Sondes > Ajouter**.   
      3. Entrez un nom pour la sonde (par exemple **RDP**), sélectionnez **TCP** pour le **Protocole**, entrez **3389** pour le **Port**, puis cliquez sur **OK**.   
3. Créez le pool de back-ends du service Broker pour les connexions :   
      1. Dans **Paramètres**, cliquez sur **Pools d’adresses principaux > Ajouter**.   
      2. Entrez un nom (par exemple CBBackendPool), puis cliquez sur **Ajouter une machine virtuelle**.  
      3. Choisissez un groupe à haute disponibilité (par exemple CbAvSet), puis cliquez sur **OK**.   
      3. Cliquez sur **Choisir les machines virtuelles**, sélectionnez chaque machine virtuelle, puis cliquez sur **Sélectionner > OK > OK**.   
4. Créez la règle d’équilibrage de charge RDP :   
      1. Dans **Paramètres**, cliquez sur **Règles d’équilibrage de charge**, puis sur **Ajouter**.   
      2. Entrez un nom (par exemple RDP), sélectionnez **TCP** pour le **Protocole**, entrez **3389** pour le **Port** et le **Port principal**, puis cliquez sur **OK**.   
5. Ajoutez un enregistrement DNS pour l’équilibreur de charge :   
      1. Connectez-vous à la machine virtuelle du serveur RDMS (par exemple Contoso-CB1). Consultez l’article [Préparer la machine virtuelle du service Broker pour les connexions Bureau à distance](Prepare-the-RD-Connection-Broker-VM-for-Remote-Desktop.md) pour connaître les étapes de connexion à la machine virtuelle.   
      2. Dans le Gestionnaire de serveur, cliquez sur **Outils > DNS**.   
      3. Dans le volet gauche, développez **DNS**, cliquez sur la machine DNS, sur **Zones de recherche directes**, puis sur votre nom de domaine (par exemple Contoso.com). (Le traitement de la requête envoyée au serveur DNS pour l’obtention des informations nécessaires peut prendre quelques secondes.)  
      4. Cliquez sur **Action > Nouvel hôte (A ou AAAA)** .   
      9. Entrez le nom (par exemple hacb) et l’adresse IP spécifiée plus tôt (par exemple 10.0.0.32).   

### <a name="configure-dns-round-robin"></a>Configurer le tourniquet (round robin) DNS  
  
Les étapes suivantes sont une alternative à la création d’un équilibreur de charge interne Azure.   
  
1. Connectez-vous au serveur RDMS dans le portail Azure à l’aide du client connexion Bureau à distance   
2. Créez des enregistrements DNS :   
      1. Dans le Gestionnaire de serveur, cliquez sur **Outils > DNS**.   
      2. Dans le volet gauche, développez **DNS**, cliquez sur la machine DNS, sur **Zones de recherche directes**, puis sur votre nom de domaine (par exemple Contoso.com). (Le traitement de la requête envoyée au serveur DNS pour l’obtention des informations nécessaires peut prendre quelques secondes.)  
      3. Cliquez sur **Action**, puis sur **Nouvel hôte (A ou AAAA)** .   
      4. Entrez le **Nom DNS** du cluster du service Broker pour les connexions Bureau à distance (par exemple hacb), puis entrez l’**Adresse IP** du premier serveur du service Broker pour les connexions Bureau à distance.   
      5. Répétez les étapes 3 à 4 pour chaque serveur du service Broker pour les connexions Bureau à distance supplémentaire, en indiquant une adresse IP unique à chaque enregistrement supplémentaire.


Par exemple, si les adresses IP des deux machines virtuelles du service Broker pour les connexions Bureau à distance sont 10.0.0.8 et 10.0.0.9, vous devez créer deux enregistrements d’hôte DNS :
 - Nom d’hôte : hacb.contoso.com, adresse IP : 10.0.0.8
 - Nom d’hôte : hacb.contoso.com, adresse IP : 10.0.0.9

## <a name="step-3-configure-the-connection-brokers-for-high-availability"></a>Étape 3 : Configurer les serveurs du service Broker pour les connexions à des fins de haute disponibilité

1. Ajoutez le nouveau serveur du service Broker pour les connexions Bureau à distance au Gestionnaire de serveur :
   1. Dans le Gestionnaire de serveur, cliquez sur **Gérer > Ajouter des serveurs**.
   2. Cliquez sur **Rechercher**.
   3. Cliquez sur le serveur du service Broker pour les connexions Bureau à distance récemment créé (par exemple Contoso-Cb2), puis cliquez sur **OK**.
2. Configurez la haute disponibilité du service Broker pour les connexions Bureau à distance :
   1. Dans le Gestionnaire de serveur, cliquez sur **Services Bureau à distance > Vue d’ensemble**.
   2. Cliquez avec le bouton droit sur **Service Broker pour les connexions Bureau à distance**, puis cliquez sur **Configurer une haute disponibilité**.
   3. Exécutez l’Assistant jusqu’à la section Type de configuration. Sélectionnez **Serveur de base de données partagé**, puis cliquez sur **Suivant**.
   4. Entrez le nom DNS du cluster du service Broker pour les connexions Bureau à distance.
   5. Entrez la chaîne de connexion de la base de données SQL, puis exécutez l’Assistant pour établir une haute disponibilité.
3. Ajouter le nouveau service Broker pour les connexions Bureau à distance au déploiement
   1. Dans le Gestionnaire de serveur, cliquez sur **Services Bureau à distance > Vue d’ensemble**.
   2. Cliquez avec le bouton droit sur le service Broker pour les connexions Bureau à distance, puis cliquez sur **Ajouter le serveur du service Broker pour les connexions Bureau à distance**.
   3. Exécutez l’Assistant jusqu’à ce que vous accédiez à la page Sélection du serveur, puis sélectionnez le serveur du service Broker pour les connexions Bureau à distance récemment créé (par exemple Contoso-CB2).
   4. Exécutez l’Assistant en acceptant les valeurs par défaut.
4. Configurez les certificats approuvés sur les clients et les serveurs du service Broker pour les connexions Bureau à distance.

