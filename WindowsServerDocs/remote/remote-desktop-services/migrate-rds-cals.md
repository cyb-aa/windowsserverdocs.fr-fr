---
title: Migrer vos licences d’accès client aux services Bureau à distance
description: Cet article décrit comment migrer vos licences d’accès client des services Bureau à distance vers de nouveaux serveurs de licence Windows Server 2016.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 11/01/2016
ms.topic: article
ms.assetid: 91bdedce-6145-469f-b72e-7e113c4391e9
author: christianmontoya
manager: scottman
ms.openlocfilehash: 5d95c2bc3a92a8cdcba4b308c88d94cb9af6d2a5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855992"
---
# <a name="migrate-your-remote-desktop-services-client-access-licenses-rds-cals"></a>Migrer vos licences d’accès client aux services Bureau à distance

Vous avez trois options pour migrer vos licences d’accès client aux services Bureau à distance :
1. Méthode de connexion automatique : cette méthode recommandée communique via Internet directement avec Microsoft Clearinghouse via le port sortant TCP 443.  
2. Avec un navigateur web : cette méthode permet d’effectuer la migration lorsque le serveur qui exécute l’outil Gestionnaire de licences Bureau à distance n’a pas de connectivité Internet, mais que l’administrateur dispose d’une connexion Internet sur un autre appareil. L’URL de la méthode de migration web s’affiche dans l’Assistant Gestion des licences d’accès client RDS. 
3. Avec un téléphone : cette méthode permet à l’administrateur d’effectuer le processus de migration par téléphone avec un représentant Microsoft. Le numéro de téléphone est déterminé par le pays/la région que vous avez choisi(e) dans l’Assistant d’activation du serveur et s’affiche dans l’Assistant Gestion des licences d’accès client RDS.

Dans cet article, la méthode intitulée [Établir la méthode de migration des licences d’accès client aux services Bureau à distance](#establish-rds-cal-migration-method) met en évidence les étapes générales communes de toutes les méthodes de migration des licences d’accès client aux services Bureau à distance, tandis que [Migrer les licences d’accès client aux services Bureau à distance](#migrate-rds-cals) met en évidence les étapes spécifiques à chaque méthode de migration.

Quelle que soit la méthode de migration, vous devez au minimum être membre du groupe Administrateurs local pour effectuer les étapes de migration.

## <a name="establish-rds-cal-migration-method"></a>Établir la méthode de migration des licences d’accès client aux services Bureau à distance

1. Sur le serveur de licences, ouvrez le **Gestionnaire de licences des services Bureau à distance**. (Cliquez sur **Démarrer, Outils d’administration**. Entrez dans le dossier **Services Bureau à distance** et lancez **Gestionnaire de licences des services Bureau à distance**.)
2. Vérifiez la méthode de connexion du serveur de licences des services Bureau à distance : cliquez avec le bouton droit sur le serveur de licences vers lequel vous souhaitez migrer les licences d’accès client aux services Bureau à distance, puis cliquez sur **Propriétés**. Sur l’onglet **Méthode de connexion**, vérifiez la **méthode de connexion**. Vous pouvez la modifier dans le menu déroulant. Cliquez sur **OK**.
3. Cliquez avec le bouton droit sur le serveur de licences vers lequel vous souhaitez migrer les licences d’accès client aux services Bureau à distance, puis cliquez sur **Gérer les licences d’accès client aux services Bureau à distance**.
4. Suivez les étapes de l’Assistant dans la page **Sélection de l’action**. Cliquez sur **Migrer des licences d’accès client aux services Bureau à distance à partir d’un autre serveur de licences vers ce serveur de licences**.
6. Choisissez le motif de migration des licences d’accès client aux services Bureau à distance, puis cliquez sur **Suivant**. Les options suivantes sont disponibles :
    - Le serveur de licences source est remplacé par ce serveur de licences.
    - Le serveur de licences source n’est plus en cours d’exécution.
7. La page suivante de l’Assistant dépend du motif de migration que vous avez choisi.
    - Si vous avez choisi **Le serveur de licences source est remplacé par ce serveur de licences** en tant que motif pour migrer les licences, la page **Informations sur le serveur de licences source** s’affiche.
    
       Sur la page d’informations sur le serveur de licences source, entrez le nom ou l’adresse IP du serveur de licences source.

       Si le serveur de licences source est disponible sur le réseau, cliquez sur **Suivant**. L’Assistant contacte le serveur de licences source. Si le serveur de licences source exécute un système d’exploitation antérieur à Windows Server 2008 R2 ou si le serveur de licences source est désactivé, un message vous rappelle que vous devez supprimer manuellement les licences d’accès client aux services Bureau à distance du serveur de licences source une fois l’Assistant terminé. Après avoir confirmé que vous compreniez cette exigence, la page **Obtenir des clés de licences client** s’affiche.

       Si le serveur de licences source n’est pas disponible sur le réseau, sélectionnez **Le serveur de licences source spécifié n’est pas disponible sur le réseau**. Spécifiez le système d’exploitation que le serveur de licences source exécute, puis indiquez l’ID de serveur de licences pour le serveur de licences source. Après avoir cliqué sur **Suivant**, il vous est rappelé que vous devez supprimer les licences d’accès client aux services Bureau à distance manuellement du serveur de licences source une fois l’Assistant terminé. Après avoir confirmé que vous compreniez cette exigence, la page **Obtenir des clés de licences client** s’affiche.

    - Si vous avez choisi **Le serveur de licences source ne fonctionne plus** comme motif pour migrer les licences d’accès client aux services Bureau à distance, il vous est rappelé que vous devez supprimer les licences manuellement du serveur de licences source une fois l’Assistant terminé. Après avoir confirmé que vous compreniez cette exigence, la page **Obtenir des clés de licences client** s’affiche.

L’étape suivante consiste à migrer les licences d’accès client. Utilisez les informations ci-dessous pour terminer l’Assistant. Notez que ce que vous voyez dans l’Assistant dépend de la méthode de connexion que vous avez identifiée à l’étape 2 ci-dessus.

## <a name="migrate-rds-cals"></a>Migrer les licences d’accès client aux services Bureau à distance

Il existe trois mécanismes pour migrer les licences vers le serveur de licences de destination. Poursuivez les étapes correspondant à la **méthode de connexion** définie à l’étape 2 :
  - [Méthode de connexion automatique](#automatic-connection-method)
  - [Avec un navigateur web](#using-a-web-browser)
  - [Avec un téléphone](#using-a-telephone)

### <a name="automatic-connection-method"></a>Méthode de connexion automatique

1. Sur la page **Programme de licence**, sélectionnez le programme approprié via lequel vous avez acheté vos licences d’accès client aux services Bureau à distance, puis cliquez sur **Suivant**.
2. Entrez les informations requises (généralement un code de licence ou un numéro de contrat, en fonction du **programme de licence**), puis cliquez sur **Suivant**. Consultez la documentation qui vous a été fournie lors de l’achat de vos licences d’accès client aux services Bureau à distance.
4. Sélectionnez la version de produit appropriée, le type de licence et la quantité de licences d’accès client aux services Bureau à distance pour votre environnement en vous basant sur votre contrat de licences d’accès client aux services Bureau à distance, puis cliquez sur **Suivant**.
5. Le centre d'échanges Microsoft est automatiquement contacté afin de traiter votre demande. Les licences d’accès client aux services Bureau à distance sont migrées sur le serveur de licences.
6. Cliquez sur **Terminer** pour terminer le processus de migration des licences d’accès client Bureau à distance.

### <a name="using-a-web-browser"></a>Avec un navigateur web
1. Sur la page **Obtenir des clés de licences client**, cliquez sur le lien hypertexte pour vous connecter au site web du gestionnaire de licences des Services Bureau à distance.
   Si vous exécutez le Gestionnaire de licences des Services Bureau à distance sur un ordinateur qui n’a pas de connectivité Internet, notez l’adresse du site web du gestionnaire de licences des Services Bureau à distance, puis connectez-vous au site web à partir d’un ordinateur qui dispose d’une connectivité Internet. 
2. Sur la page web du gestionnaire de licences des Services Bureau à distance, sous **Sélectionner l’option**, sélectionnez **Gérer les licences d’accès client**, puis cliquez sur **Suivant**.
3. Fournissez les informations requises, puis cliquez sur **Suivant** :
    - **ID du serveur de licences cible** : nombre à 35 chiffres, par groupes de 5, qui s’affiche dans la page **Obtenir des clés de licences client** de l’Assistant Gestion des licences d’accès client RDS.
    - **Motif de la récupération** : Choisissez le motif de migration des licences d’accès client aux services Bureau à distance.
    - **Programme de licence** : Choisissez le programme par le biais duquel vous avez acheté vos licences d’accès client aux services Bureau à distance.
4. Fournissez les informations requises, puis cliquez sur **Suivant** :
   - Nom de famille
   - Prénom
   - Nom de la société
   - Pays/région

     Vous pouvez également fournir les informations facultatives demandées, comme l’adresse de la société, l’adresse de courrier et le numéro de téléphone. Dans le champ de l’unité d’organisation, vous pouvez décrire l’unité de votre organisation qui sert ce serveur de licences.

5. Les informations que vous devez fournir dans cette page dépendent du programme de licence que vous avez sélectionné dans la page précédente. Vous devez généralement indiquer un code de licence ou un numéro de contrat de licence. Consultez la documentation qui vous a été fournie lors de l’achat de vos licences d’accès client aux services Bureau à distance. En outre, vous devez spécifier le type de licences d’accès client aux services Bureau à distance et la quantité que vous souhaitez migrer vers le serveur de licences.
6. Après avoir entré les informations demandées, cliquez sur **Suivant**.
7. Vérifiez que toutes les informations que vous avez entrées sont correctes, puis cliquez sur **Suivant** pour envoyer votre demande à Microsoft Clearinghouse. La page web affiche ensuite un ID de jeu de clés de licence généré par Microsoft Clearinghouse.

   > [!IMPORTANT] 
   > Conservez une copie de l’ID du jeu de clés de licence. Conservez cet ID, car vous devrez le communiquer au Microsoft Clearinghouse dans le cas où vous auriez besoin d’une assistance pour récupérer les licences d’accès client aux services Bureau à distance.

8. Dans la même page **Obtenir des clés de licences client**, entrez l’ID de jeu de clés de licence, puis cliquez sur **Suivant** pour migrer les licences d’accès client Bureau à distance vers votre serveur de licences.
9. Cliquez sur **Terminer** pour terminer le processus de migration des licences d’accès client Bureau à distance.

### <a name="using-a-telephone"></a>Avec un téléphone
1. Sur la page **Obtenir des clés de licences client**, utilisez le numéro de téléphone affiché pour appeler Microsoft Clearinghouse. Communiquez au représentant l’ID de votre serveur de licences Bureau à distance et les informations requises pour le programme de licence par le biais duquel vous avez acheté vos licences d’accès client aux services Bureau à distance. Le représentant traite ensuite votre demande de migration de licences d’accès client aux services Bureau à distance et vous fournit un identificateur (ID) unique correspondant à ces licences. Cet identificateur unique constitue l’**ID du jeu de clés de licence**.

   > [!IMPORTANT]
   > Conservez une copie de l’ID du jeu de clés de licence. Conservez cet ID, car vous devrez le communiquer au Microsoft Clearinghouse dans le cas où vous auriez besoin d’une assistance pour récupérer les licences d’accès client aux services Bureau à distance.

2. Dans la même page **Obtenir des clés de licences client**, entrez l’ID de jeu de clés de licence, puis cliquez sur **Suivant** pour migrer les licences d’accès client Bureau à distance vers votre serveur de licences.
3. Cliquez sur **Terminer** pour terminer le processus de migration des licences d’accès client Bureau à distance.
