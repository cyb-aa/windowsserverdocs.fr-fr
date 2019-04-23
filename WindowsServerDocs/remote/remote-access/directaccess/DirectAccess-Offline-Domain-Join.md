---
title: Jonction de domaine hors connexion DirectAccess
description: Ce guide explique les étapes pour effectuer une jonction de domaine hors connexion avec DirectAccess dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 55528736-6c19-40bd-99e8-5668169ef3c7
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a9cd811c680f15d53ecbd28d9201f28d9cb8af2f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853060"
---
# <a name="directaccess-offline-domain-join"></a>Jonction de domaine hors connexion DirectAccess

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Ce guide explique les étapes pour effectuer une jonction de domaine hors connexion avec DirectAccess. Au cours d’une jonction de domaine hors connexion, un ordinateur est configuré pour joindre un domaine sans connexion VPN ou physique.  
  
Ce guide comporte les rubriques suivantes :  
  
- Vue d’ensemble de jonction de domaine hors connexion  
  
- Configuration requise pour la jonction de domaine hors connexion
  
- Processus de jonction de domaine hors connexion
  
- Instructions pour effectuer une jonction de domaine hors connexion  
  
## <a name="offline-domain-join-overview"></a>Vue d’ensemble de jonction de domaine hors connexion  
Introduite dans Windows Server 2008 R2, les contrôleurs de domaine incluent une fonctionnalité appelée la jonction de domaine hors connexion. Un utilitaire de ligne de commande nommé Djoin.exe vous permet de joindre un ordinateur à un domaine sans contacter physiquement un contrôleur de domaine lors de l’exécution de l’opération de jonction de domaine. Les étapes générales pour l’utilisation de Djoin.exe sont :  
  
1.  Exécutez **djoin /provision** pour créer les métadonnées de compte d’ordinateur. La sortie de cette commande est un fichier .txt qui inclut un blob codé en base 64.  
  
2.  Exécutez **djoin /requestODJ** pour insérer les métadonnées de compte d’ordinateur à partir du fichier .txt dans le répertoire Windows de l’ordinateur de destination.  
  
3.  Redémarrez l’ordinateur de destination, et l’ordinateur est joint au domaine.  
  
### <a name="BKMK_ODJOverview"></a>Jonction de domaine hors connexion avec la vue d’ensemble du scénario DirectAccess stratégies  
Jonction de domaine hors connexion DirectAccess est un processus qui les ordinateurs exécutant Windows Server 2016, Windows Server 2012, Windows 10 et Windows 8 peuvent utiliser pour joindre un domaine sans être physiquement joint au réseau d’entreprise ou connectés via VPN. Cela rend possible de joindre des ordinateurs à un domaine à partir d’emplacements où il n’existe aucune connectivité à un réseau d’entreprise. Jonction de domaine hors connexion pour DirectAccess fournit des stratégies DirectAccess aux clients pour permettre la configuration à distance.  
  
Une jonction de domaine crée un compte d’ordinateur et établit une relation d’approbation entre un ordinateur exécutant un système d’exploitation de Windows et un domaine Active Directory.  
  
## <a name="BKMK_ODJRequirements"></a>Préparer pour la jonction de domaine hors connexion  
  
1.  Créer le compte d’ordinateur.  
  
2.  L’appartenance de tous les groupes de sécurité auquel appartient le compte d’ordinateur d’inventaire.  
  
3.  Collecter les certificats d’ordinateur requis, les stratégies de groupe et les objets de stratégie de groupe à appliquer pour les nouveaux clients.  
  
. Les sections suivantes expliquent les exigences de système d’exploitation et les informations d’identification requises pour effectuer une jonction de domaine hors connexion DirectAccess à l’aide de Djoin.exe.  
  
### <a name="operating-system-requirements"></a>Configuration requise pour le système d'exploitation  
Vous pouvez exécuter Djoin.exe pour DirectAccess uniquement sur les ordinateurs qui exécutent Windows Server 2016, Windows Server 2012 ou Windows 8. L’ordinateur sur lequel vous exécutez Djoin.exe aux données de compte d’ordinateur approvisionner dans AD DS doit être en cours d’exécution pour Windows Server 2016, Windows 10, Windows Server 2012 ou Windows 8. L’ordinateur que vous souhaitez joindre au domaine doit également être en cours d’exécution Windows Server 2016, Windows 10, Windows Server 2012 ou Windows 8.  
  
### <a name="credential-requirements"></a>Informations d’identification requises  
Pour effectuer une jonction de domaine hors connexion, vous devez disposer des droits nécessaires joindre des stations de travail au domaine. Les membres du groupe Admins du domaine ont ces droits par défaut. Si vous n’êtes pas membre du groupe Admins du domaine, un membre du groupe Admins du domaine doit effectuer une des actions suivantes pour vous permettent de joindre des stations de travail au domaine :  
  
-   Stratégie de groupe permet d’accorder les droits d’utilisateur requis. Cette méthode vous permet de créer des ordinateurs dans le conteneur ordinateurs par défaut et dans toute unité d’organisation (UO) qui est créée ultérieurement (si aucun entrées de contrôle d’accès (ACE) Deny ne sont ajoutées).  
  
-   Modifier la liste de contrôle d’accès (ACL) du conteneur ordinateurs par défaut pour le domaine déléguer des autorisations appropriées pour vous.  
  
-   Créer une unité d’organisation et de modifier l’ACL sur cette unité d’organisation pour qu’il vous accorde le **créer un enfant - autoriser** autorisation. Passer le **/machineOU** paramètre à la **djoin /provision** commande.  
  
Les procédures suivantes montrent comment accorder les droits d’utilisateur avec la stratégie de groupe et comment déléguer des autorisations appropriées.  
  
#### <a name="granting-user-rights-to-join-workstations-to-the-domain"></a>L’octroi des droits utilisateur pour joindre des stations de travail au domaine  
Vous pouvez utiliser la Console de gestion des stratégies de groupe (GPMC) pour modifier la stratégie de domaine ou de créer une nouvelle stratégie qui a des paramètres qui accordent des droits d’utilisateur pour ajouter des stations de travail à un domaine.  
  
L’appartenance au **Admins du domaine**, ou équivalente, est la condition minimale requise pour accorder des droits d’utilisateur.  Examinez les informations relatives à l’aide de comptes appropriés et les appartenances au groupe [locaux et les groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) (https://go.microsoft.com/fwlink/?LinkId=83477).   
  
###### <a name="to-grant-rights-to-join-workstations-to-a-domain"></a>Pour accorder des droits pour joindre des stations de travail à un domaine  
  
1.  Cliquez sur **Démarrer**, sur **Outils d'administration**, puis sur **Gestion des stratégies de groupe**.  
  
2.  Double-cliquez sur le nom de la forêt, double-cliquez sur **domaines**, double-cliquez sur le nom du domaine dans lequel vous souhaitez joindre un ordinateur, avec le bouton droit **Default Domain Policy**, puis cliquez sur  **Modifier**.  
  
3.  Dans l’arborescence de la console, double-cliquez sur **Configuration ordinateur**, double-cliquez sur **stratégies**, double-cliquez sur **Windows paramètres**, double-cliquez sur **sécurité Paramètres**, double-cliquez sur **stratégies locales**, puis double-cliquez sur **attribution des droits utilisateur**.  
  
4.  Dans le volet détails, double-cliquez sur **ajouter des stations de travail au domaine**.  
  
5.  Sélectionnez le **définir ces paramètres de stratégie** case à cocher, puis cliquez sur **ajouter un utilisateur ou groupe**.  
  
6.  Tapez le nom du compte que vous souhaitez accorder les droits d’utilisateur, puis cliquez sur **OK** à deux reprises.  
  
## <a name="BKMK_ODKSxS"></a>Processus de jonction de domaine hors connexion  
Exécutez Djoin.exe à une invite de commandes avec élévation de privilèges pour configurer les métadonnées de compte d’ordinateur. Lorsque vous exécutez la commande de configuration, les métadonnées de compte d’ordinateur sont créée dans un fichier binaire que vous spécifiez dans le cadre de la commande.  
  
Pour plus d’informations sur la fonction NetProvisionComputerAccount qui est utilisée pour configurer le compte d’ordinateur pendant une jonction de domaine hors connexion, consultez [NetProvisionComputerAccount fonction](https://go.microsoft.com/fwlink/?LinkId=162426) (https://go.microsoft.com/fwlink/?LinkId=162426). Pour plus d’informations sur la fonction NetRequestOfflineDomainJoin qui s’exécute localement sur l’ordinateur de destination, consultez [NetRequestOfflineDomainJoin fonction](https://go.microsoft.com/fwlink/?LinkId=162427) (https://go.microsoft.com/fwlink/?LinkId=162427).  
  
## <a name="BKMK_ODJSteps"></a>Instructions pour effectuer une jonction de domaine hors connexion DirectAccess  
Le processus de jonction de domaine hors connexion comprend les étapes suivantes :  
  
1.  Créer un nouveau compte d’ordinateur pour chacun des clients distants et de générer un package d’approvisionnement à l’aide de la commande Djoin.exe à partir d’un déjà joint à un domaine ordinateur dans le réseau d’entreprise.  
  
2.  Ajouter l’ordinateur client au groupe de sécurité DirectAccessClients  
  
3.  Transférer le package d’approvisionnement en toute sécurité aux ordinateurs distants (s) que devra joindre le domaine.  
  
4.  Appliquer le package d’approvisionnement et de joindre le client au domaine.  
  
5.  Redémarrez le client pour terminer la jonction de domaine et d’établir la connectivité.  
  
Il existe deux options à prendre en compte lors de la création du paquet d’approvisionnement pour le client. Si vous avez utilisé l’Assistant Installation DirectAccess sans infrastructure à clé publique, vous devez utiliser l’option 1 ci-dessous. Si vous avez utilisé l’Assistant Installation avancée pour installer DirectAccess avec l’infrastructure à clé publique, vous devez utiliser l’option 2 ci-dessous.  
  
Procédez comme suit pour effectuer la jonction de domaine hors connexion :  
  
##### <a name="option1-create-a-provisioning-package-for-the-client-without-pki"></a>Option 1 : Créer un package d’approvisionnement pour le client sans infrastructure à clé publique  
  
1.  À l’invite de commande de votre serveur d’accès à distance, tapez la commande suivante pour configurer le compte d’ordinateur :  
  
    ```  
    Djoin /provision /domain <your domain name> /machine <remote machine name> /policynames DA Client GPO name /rootcacerts /savefile c:\files\provision.txt /reuse  
    ```  
  
##### <a name="option2-create-a-provisioning-package-for-the-client-with-pki"></a>Option2 : Créer un package d’approvisionnement pour le client avec l’infrastructure à clé publique  
  
1.  À l’invite de commande de votre serveur d’accès à distance, tapez la commande suivante pour configurer le compte d’ordinateur :  
  
    ```  
    Djoin /provision /machine <remote machine name> /domain <Your Domain name> /policynames <DA Client GPO name> /certtemplate <Name of client computer cert template> /savefile c:\files\provision.txt /reuse  
    ```  
  
##### <a name="add-the-client-computer-to-the-directaccessclients-security-group"></a>Ajouter l’ordinateur client au groupe de sécurité DirectAccessClients  
  
1.  Sur votre contrôleur de domaine à partir de **Démarrer** , tapez **Active** et sélectionnez **Active Directory Users and Computers** de **applications** écran .  
  
2.  Développez l’arborescence sous votre domaine, puis sélectionnez le **utilisateurs** conteneur.  
  
3.  Dans le volet détails, cliquez sur **DirectAccessClients**, puis cliquez sur **propriétés**.  
  
4.  Sous l'onglet **Membres** , cliquez sur **Ajouter**.  
  
5.  Cliquez sur **Types d'objets**, sélectionnez **Ordinateurs**, puis cliquez sur **OK**.  
  
6.  Tapez le nom de client à ajouter, puis cliquez sur **OK**.  
  
7.  Cliquez sur **OK** pour fermer la **DirectAccessClients** boîte de dialogue Propriétés et fermez **Active Directory Users and Computers**.  
  
##### <a name="copy-and-then-apply-the-provisioning-package-to-the-client-computer"></a>Copier, puis appliquer le package d’approvisionnement à l’ordinateur client  
  
1.  Copiez le package d’approvisionnement à partir de c:\files\provision.txt sur le serveur d’accès à distance, où il a été enregistré, à c:\provision\provision.txt sur l’ordinateur client.  
  
2.  Sur l’ordinateur client, ouvrez une invite de commandes avec élévation de privilèges, puis tapez la commande suivante pour demander la jonction de domaine :  
  
    ```  
    Djoin /requestodj /loadfile C:\provision\provision.txt /windowspath %windir% /localos  
    ```  
  
3.  Redémarrez l’ordinateur client. L’ordinateur est joint au domaine. Après le redémarrage, le client sera joint au domaine et dispose d’une connectivité au réseau d’entreprise avec DirectAccess.  
  
## <a name="see-also"></a>Voir aussi  
[NetProvisionComputerAccount (fonction)](https://go.microsoft.com/fwlink/?LinkId=162426)  
[NetRequestOfflineDomainJoin (fonction)](https://go.microsoft.com/fwlink/?LinkId=162427)  
  


