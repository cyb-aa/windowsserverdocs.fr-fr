---
title: Jonction de domaine hors connexion DirectAccess
description: Ce guide explique les étapes à suivre pour effectuer une jonction de domaine hors connexion avec DirectAccess dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 55528736-6c19-40bd-99e8-5668169ef3c7
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 43f0c606f16af00797f0325b793d476e6c2892f8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815662"
---
# <a name="directaccess-offline-domain-join"></a>Jonction de domaine hors connexion DirectAccess

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Ce guide explique les étapes à suivre pour effectuer une jonction de domaine hors connexion avec DirectAccess. Au cours d’une jonction de domaine hors connexion, un ordinateur est configuré pour joindre un domaine sans connexion physique ou VPN.  
  
Ce guide comporte les rubriques suivantes :  
  
- Vue d’ensemble de la jonction de domaine hors connexion  
  
- Configuration requise pour la jonction de domaine hors connexion
  
- Processus de jonction de domaine hors connexion
  
- Étapes pour effectuer une jonction de domaine hors connexion  
  
## <a name="offline-domain-join-overview"></a>Vue d’ensemble de la jonction de domaine hors connexion  
Introduite dans Windows Server 2008 R2, les contrôleurs de domaine incluent une fonctionnalité appelée jonction de domaine hors connexion. Un utilitaire de ligne de commande nommé Djoin. exe vous permet de joindre un ordinateur à un domaine sans avoir à contacter physiquement un contrôleur de domaine lors de l’exécution de l’opération de jonction de domaine. Les étapes générales de l’utilisation de Djoin. exe sont les suivantes :  
  
1.  Exécutez **Djoin/provision** pour créer les métadonnées de compte d’ordinateur. La sortie de cette commande est un fichier. txt qui comprend un objet BLOB encodé en base 64.  
  
2.  Exécutez **Djoin/requestodj** pour insérer les métadonnées du compte d’ordinateur à partir du fichier. txt dans le répertoire Windows de l’ordinateur de destination.  
  
3.  Redémarrez l’ordinateur de destination et l’ordinateur sera joint au domaine.  
  
### <a name="offline-domain-join-with-directaccess-policies-scenario-overview"></a><a name="BKMK_ODJOverview"></a>Vue d’ensemble du scénario de jonction de domaine hors connexion avec des stratégies DirectAccess  
La jonction de domaine hors connexion DirectAccess est un processus que les ordinateurs exécutant Windows Server 2016, Windows Server 2012, Windows 10 et Windows 8 peuvent utiliser pour joindre un domaine sans être physiquement joint au réseau d’entreprise, ou être connectés via un VPN. Cela permet de joindre des ordinateurs à un domaine à partir d’emplacements où il n’existe aucune connectivité à un réseau d’entreprise. La jonction de domaine hors connexion pour DirectAccess fournit des stratégies DirectAccess aux clients pour permettre l’approvisionnement à distance.  
  
Une jonction de domaine crée un compte d’ordinateur et établit une relation d’approbation entre un ordinateur exécutant un système d’exploitation Windows et un domaine de Active Directory.  
  
## <a name="prepare-for-offline-domain-join"></a><a name="BKMK_ODJRequirements"></a>Préparer la jonction de domaine hors connexion  
  
1.  Créez le compte d’ordinateur.  
  
2.  Inventoriez l’appartenance de tous les groupes de sécurité auxquels appartient le compte d’ordinateur.  
  
3.  Collectez les certificats d’ordinateur, les stratégies de groupe et les objets de stratégie de groupe requis à appliquer aux nouveaux clients.  
  
. Les sections suivantes décrivent la configuration requise pour le système d’exploitation et les informations d’identification requises pour effectuer une jonction de domaine hors connexion DirectAccess à l’aide de Djoin. exe.  
  
### <a name="operating-system-requirements"></a>Configuration requise pour le système d'exploitation  
Vous pouvez exécuter Djoin. exe pour DirectAccess uniquement sur les ordinateurs qui exécutent Windows Server 2016, Windows Server 2012 ou Windows 8. L’ordinateur sur lequel vous exécutez Djoin. exe pour approvisionner des données de compte d’ordinateur dans AD DS doit exécuter Windows Server 2016, Windows 10, Windows Server 2012 ou Windows 8. L’ordinateur que vous souhaitez joindre au domaine doit également exécuter Windows Server 2016, Windows 10, Windows Server 2012 ou Windows 8.  
  
### <a name="credential-requirements"></a>Informations d’identification requises  
Pour effectuer une jonction de domaine hors connexion, vous devez disposer des droits nécessaires pour joindre les stations de travail au domaine. Les membres du groupe Admins du domaine disposent de ces droits par défaut. Si vous n’êtes pas membre du groupe Admins du domaine, un membre du groupe Admins du domaine doit effectuer l’une des actions suivantes pour vous permettre de joindre des stations de travail au domaine :  
  
-   Utilisez stratégie de groupe pour accorder les droits d’utilisateur requis. Cette méthode vous permet de créer des ordinateurs dans le conteneur ordinateurs par défaut et dans toute unité d’organisation créée ultérieurement (si aucune entrée de contrôle d’accès (ACE) n’est ajoutée).  
  
-   Modifiez la liste de contrôle d’accès (ACL) du conteneur ordinateurs par défaut pour le domaine afin de déléguer les autorisations appropriées.  
  
-   Créez une unité d’organisation et modifiez la liste de contrôle d’accès sur cette UO pour vous accorder l’autorisation **créer un enfant-autoriser** . Transmettez le paramètre **/machineOU** à la commande **Djoin/provision** .  
  
Les procédures suivantes montrent comment accorder les droits d’utilisateur avec stratégie de groupe et comment déléguer les autorisations appropriées.  
  
#### <a name="granting-user-rights-to-join-workstations-to-the-domain"></a>Octroi de droits d’utilisateur pour joindre des stations de travail au domaine  
Vous pouvez utiliser la Console de gestion des stratégies de groupe (GPMC) pour modifier la stratégie de domaine ou créer une stratégie qui possède des paramètres qui permettent aux utilisateurs d’ajouter des stations de travail à un domaine.  
  
Pour accorder des droits d’utilisateur, il est nécessaire d’appartenir au minimum au **groupe Admins du domaine**ou à un groupe équivalent.  Passez en revue les détails sur l’utilisation des comptes et des appartenances aux groupes appropriés dans les [groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) (https://go.microsoft.com/fwlink/?LinkId=83477).   
  
###### <a name="to-grant-rights-to-join-workstations-to-a-domain"></a>Pour accorder des droits pour joindre des stations de travail à un domaine  
  
1.  Cliquez sur **Démarrer**, sur **Outils d'administration**, puis sur **Gestion des stratégies de groupe**.  
  
2.  Double-cliquez sur le nom de la forêt, double-cliquez sur **domaines**, double-cliquez sur le nom du domaine dans lequel vous souhaitez joindre un ordinateur, cliquez avec le bouton droit sur **stratégie de domaine par défaut**, puis cliquez sur **modifier**.  
  
3.  Dans l’arborescence de la console, double-cliquez sur **Configuration ordinateur**, sur **stratégies**, sur **Paramètres Windows**, sur paramètres de **sécurité**, sur **stratégies locales**, puis sur **attribution des droits utilisateur**.  
  
4.  Dans le volet d’informations, double-cliquez sur **Ajouter des stations de travail au domaine**.  
  
5.  Activez la case à cocher **définir ces paramètres de stratégie** , puis cliquez sur **Ajouter un utilisateur ou un groupe**.  
  
6.  Tapez le nom du compte auquel vous souhaitez accorder les droits d’utilisateur, puis cliquez deux fois sur **OK** .  
  
## <a name="offline-domain-join-process"></a><a name="BKMK_ODKSxS"></a>Processus de jonction de domaine hors connexion  
Exécutez Djoin. exe à partir d’une invite de commandes avec élévation de privilèges pour approvisionner les métadonnées du compte d’ordinateur. Lorsque vous exécutez la commande d’approvisionnement, les métadonnées de compte d’ordinateur sont créées dans un fichier binaire que vous spécifiez dans le cadre de la commande.  
  
Pour plus d’informations sur la fonction NetProvisionComputerAccount utilisée pour approvisionner le compte d’ordinateur au cours d’une jonction de domaine hors connexion, consultez [fonction NetProvisionComputerAccount](https://go.microsoft.com/fwlink/?LinkId=162426) (https://go.microsoft.com/fwlink/?LinkId=162426). Pour plus d’informations sur la fonction NetRequestOfflineDomainJoin qui s’exécute localement sur l’ordinateur de destination, consultez [fonction NetRequestOfflineDomainJoin](https://go.microsoft.com/fwlink/?LinkId=162427) (https://go.microsoft.com/fwlink/?LinkId=162427).  
  
## <a name="steps-for-performing-a-directaccess-offline-domain-join"></a><a name="BKMK_ODJSteps"></a>Procédure d’exécution d’une jonction de domaine hors connexion DirectAccess  
Le processus de jonction de domaine hors connexion comprend les étapes suivantes :  
  
1.  Créez un compte d’ordinateur pour chacun des clients distants et générez un package d’approvisionnement à l’aide de la commande Djoin. exe à partir d’un ordinateur déjà joint au domaine dans le réseau d’entreprise.  
  
2.  Ajouter l’ordinateur client au groupe de sécurité DirectAccessClients  
  
3.  Transférez le package d’approvisionnement en toute sécurité sur les ordinateurs distants qui vont rejoindre le domaine.  
  
4.  Appliquez le package d’approvisionnement et joignez le client au domaine.  
  
5.  Redémarrez le client pour terminer la jonction de domaine et établir la connectivité.  
  
Il existe deux options à prendre en compte lors de la création du paquet de configuration pour le client. Si vous avez utilisé l’Assistant Prise en main pour installer DirectAccess sans infrastructure à clé publique, vous devez utiliser l’option 1 ci-dessous. Si vous avez utilisé l’Assistant Installation avancée pour installer DirectAccess avec l’infrastructure à clé publique, vous devez utiliser l’option 2 ci-dessous.  
  
Pour effectuer la jonction de domaine hors connexion, procédez comme suit :  
  
##### <a name="option1-create-a-provisioning-package-for-the-client-without-pki"></a>Option1 : créer un package d’approvisionnement pour le client sans infrastructure à clé publique  
  
1.  À l’invite de commandes de votre serveur d’accès à distance, tapez la commande suivante pour approvisionner le compte d’ordinateur :  
  
    ```  
    Djoin /provision /domain <your domain name> /machine <remote machine name> /policynames DA Client GPO name /rootcacerts /savefile c:\files\provision.txt /reuse  
    ```  
  
##### <a name="option2-create-a-provisioning-package-for-the-client-with-pki"></a>Option2 : créer un package d’approvisionnement pour le client avec l’infrastructure à clé publique  
  
1.  À l’invite de commandes de votre serveur d’accès à distance, tapez la commande suivante pour approvisionner le compte d’ordinateur :  
  
    ```  
    Djoin /provision /machine <remote machine name> /domain <Your Domain name> /policynames <DA Client GPO name> /certtemplate <Name of client computer cert template> /savefile c:\files\provision.txt /reuse  
    ```  
  
##### <a name="add-the-client-computer-to-the-directaccessclients-security-group"></a>Ajouter l’ordinateur client au groupe de sécurité DirectAccessClients  
  
1.  Sur votre contrôleur de domaine, dans l’écran d' **Accueil** , tapez **Active** et sélectionnez **Active Directory les utilisateurs et les ordinateurs** à partir de l’écran **applications** .  
  
2.  Développez l’arborescence sous votre domaine, puis sélectionnez le conteneur **utilisateurs** .  
  
3.  Dans le volet d’informations, cliquez avec le bouton droit sur **DirectAccessClients**, puis cliquez sur **Propriétés**.  
  
4.  Sous l'onglet **Membres**, cliquez sur **Ajouter**.  
  
5.  Cliquez sur **Types d'objets**, sélectionnez **Ordinateurs**, puis cliquez sur **OK**.  
  
6.  Tapez le nom du client à ajouter, puis cliquez sur **OK**.  
  
7.  Cliquez sur **OK** pour fermer la boîte de dialogue Propriétés de **DirectAccessClients** , puis fermez **Active Directory utilisateurs et ordinateurs**.  
  
##### <a name="copy-and-then-apply-the-provisioning-package-to-the-client-computer"></a>Copier, puis appliquer le package d’approvisionnement à l’ordinateur client  
  
1.  Copiez le package d’approvisionnement à partir de c:\files\provision.txt sur le serveur d’accès à distance, où il a été enregistré, sur c:\provision\provision.txt sur l’ordinateur client.  
  
2.  Sur l’ordinateur client, ouvrez une invite de commandes avec élévation de privilèges, puis tapez la commande suivante pour demander la jonction de domaine :  
  
    ```  
    Djoin /requestodj /loadfile C:\provision\provision.txt /windowspath %windir% /localos  
    ```  
  
3.  Redémarrez l’ordinateur client. L’ordinateur sera joint au domaine. Après le redémarrage, le client sera joint au domaine et disposer d’une connexion au réseau d’entreprise avec DirectAccess.  
  
## <a name="see-also"></a>Voir aussi  
[NetProvisionComputerAccount fonction)](https://go.microsoft.com/fwlink/?LinkId=162426)  
[NetRequestOfflineDomainJoin fonction)](https://go.microsoft.com/fwlink/?LinkId=162427)  
  


