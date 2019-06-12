---
title: Migrer vos licences d’accès client aux services Bureau à distance
description: Cet article décrit comment migrer des Services Bureau à distance licences d’accès Client vers de nouveaux serveurs de licence de Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
msreviewer: ''
nams.suite: ''
nams.technology: remote-desktop-services
ms.author: chrimo
ms.date: 11/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 91bdedce-6145-469f-b72e-7e113c4391e9
author: christianmontoya
manager: scottman
ms.openlocfilehash: c947375b58c0ad88781335b799055e101bd2a193
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447099"
---
# <a name="migrate-your-remote-desktop-services-client-access-licenses-rds-cals"></a>Migrer vos licences d’accès client aux services Bureau à distance

Vous avez trois options pour migrer vos licences d’accès client services Bureau à distance :
1. Méthode de connexion automatique : Cette méthode recommandée communique via internet directement à Microsoft Clearinghouse sortant via le port TCP 443.  
2. À l’aide d’un navigateur web : Cette méthode permet la migration lorsque le serveur qui exécute l’outil Gestionnaire de licences bureau à distance n’a pas de connectivité internet, mais l’administrateur possède une connexion internet sur un appareil distinct. L’URL pour la méthode de migration Web s’affiche dans l’Assistant Gestion des licences d’accès client RDS. 
3. À l’aide d’un téléphone : Cette méthode permet à l’administrateur terminer le processus de migration par téléphone avec un représentant Microsoft. Le numéro de téléphone est déterminé par la pays/la région que vous avez choisi dans l’Assistant Activation du serveur s’affiche dans l’Assistant Gestion des licences d’accès client RDS.

Dans cet article, le [méthode de migration établir RDS CAL](#establish-rds-cal-migration-method) met en évidence les étapes générales communes sur n’importe quelle méthode de migration RDS CAL, tandis que [migrer les licences](#migrate-rds-cals) met en évidence les étapes spécifiques à chaque migration méthode.

Quelle que soit la méthode de migration, vous devez, au minimum, être membre du groupe Administrateurs local pour effectuer les étapes de migration.

## <a name="establish-rds-cal-migration-method"></a>Établir la méthode de migration des licences d’accès client RDS

1. Sur le serveur de licences, ouvrez **Gestionnaire de licences bureau à distance**. (Cliquez sur **Démarrer > Outils d’administration**. Entrez le **Services Bureau à distance** directory et lancement **Gestionnaire de licences bureau à distance**.)
2. Vérifiez que la méthode de connexion pour le serveur de licences bureau à distance : cliquez sur le serveur de licences auquel vous souhaitez migrer les licences d’accès client RDS, puis cliquez sur **propriétés**. Sur le **méthode de connexion** , vérifiez le **méthode de connexion** -vous pouvez le modifier dans le menu déroulant. Cliquez sur **OK**.
3. Cliquez sur le serveur de licences auquel vous souhaitez migrer les licences d’accès client RDS, puis cliquez sur **gérer les licences d’accès client RDS**.
4. Suivez les étapes de l’Assistant pour le **sélection d’Action** page. Cliquez sur **migrer des licences à partir d’un autre serveur de licences pour ce serveur de licences**.
6. Choisissez le motif pour migrer les licences d’accès client RDS, puis cliquez sur **suivant**. Les options suivantes sont disponibles :
    - Le serveur de licences source est remplacé par ce serveur de licences.
    - Le serveur de licences source ne fonctionne plus.
7. La page suivante de l’Assistant dépend de la raison de migration que vous avez choisi.
    - Si vous avez choisi **le serveur de licences source est remplacé par ce serveur de licences** en tant que la raison pour migrer les licences RDS CAL, le **informations de serveur de licences Source** page s’affiche.
    
       Sur la page d’informations de serveur de licences Source, entrez le nom ou l’adresse IP du serveur de licences source.

       Si le serveur de licences source est disponible sur le réseau, cliquez sur **suivant**. L’Assistant contacte le serveur de licences source. Si le serveur de licences source exécute un système d’exploitation antérieurs à Windows Server 2008 R2 ou le serveur de licences source est désactivé, un message vous rappelle que vous devez supprimer les licences RDS CAL manuellement à partir du serveur de licences source une fois l’Assistant terminé. Après avoir confirmé que vous compreniez cette exigence, le **obtenir le jeu de clés de licence Client** page s’affiche.

       Si le serveur de licences source n’est pas disponible sur le réseau, sélectionnez **le serveur de licences source spécifié n’est pas disponible sur le réseau**. Spécifiez le système d’exploitation que le serveur de licences source est en cours d’exécution, puis indiquez l’ID de serveur de licences pour le serveur de licences source. Après avoir cliqué sur **suivant**, il vous est rappelé que vous devez supprimer les licences RDS CAL manuellement à partir du serveur de licences source une fois l’Assistant terminé. Après avoir confirmé que vous compreniez cette exigence, le **obtenir le jeu de clés de licence Client** page s’affiche.

    - Si vous avez choisi **le serveur de licences source ne fonctionne plus** en tant que la raison pour migrer les licences RDS CAL, vous sont un rappel que vous devez supprimer les licences RDS CAL manuellement à partir du serveur de licences source une fois l’Assistant terminé. Après avoir confirmé que vous compreniez cette exigence, le **obtenir le jeu de clés de licence Client** page s’affiche.

L’étape suivante consiste à migrer les licences d’accès client - utilisez les informations ci-dessous pour terminer l’Assistant. Notez que ce que vous voyez dans l’Assistant dépend de la méthode de connexion que vous avez identifiée à l’étape 2 ci-dessus.

## <a name="migrate-rds-cals"></a>Migrer des licences d’accès client RDS

Il existe trois mécanismes pour migrer des licences vers le serveur de licences de destination ; Poursuivez les étapes correspondant à la **méthode de connexion** vérifié à l’étape 2 :
  - [Méthode de connexion automatique](#automatic-connection-method)
  - [À l’aide d’un navigateur web](#using-a-web-browser)
  - [À l’aide d’un téléphone](#using-a-telephone)

### <a name="automatic-connection-method"></a>Méthode de connexion automatique

1. Sur le **programme de licence** page, sélectionnez le programme par le biais duquel vous avez acheté vos CAL RDS, puis cliquez sur **suivant**.
2. Entrez les informations requises (généralement un code de licence ou un numéro de contrat, selon le **programme de licence**), puis cliquez sur **suivant**. Consultez la documentation fournie lorsque vous avez acheté vos CAL de services Bureau à distance.
4. Sélectionnez la version appropriée du produit, le type de licence et la quantité de licences pour votre environnement en fonction de votre contrat d’achat de RDS CAL, puis cliquez sur **suivant**.
5. Le centre d'échanges Microsoft est automatiquement contacté afin de traiter votre demande. Les licences RDS CAL sont migrées sur le serveur de licences.
6. Cliquez sur **Terminer** pour terminer le processus de migration de RDS CAL.

### <a name="using-a-web-browser"></a>À l’aide d’un navigateur web
1. Sur le **obtenir le jeu de clés de licence Client** , cliquez sur le lien hypertexte pour vous connecter au site Web du Gestionnaire de licences des Services Bureau à distance.
   Si vous exécutez le Gestionnaire de licences bureau à distance sur un ordinateur qui n’a pas de connectivité Internet, notez l’adresse du site Web du Gestionnaire de licences des Services Bureau à distance, puis connectez-vous au site Web à partir d’un ordinateur qui dispose d’une connectivité Internet. 
2. Dans la page Web du Gestionnaire de licences des Services Bureau à distance, sous **sélectionner l’Option**, sélectionnez **gérer les licences d’accès client**, puis cliquez sur **suivant**.
3. Indiquez les informations requises suivantes, puis cliquez sur **suivant**:
    - **ID de licence serveur cible**: Un nombre à 35 chiffres, groupés par 5, qui est affiché dans le **obtenir le jeu de clés de licence Client** page dans l’Assistant Gestion des licences d’accès client RDS.
    - **Raison pour la récupération**: Choisissez le motif pour migrer les licences RDS CAL.
    - **Programme de licence**: Choisissez le programme par le biais duquel vous avez acheté vos CAL de services Bureau à distance.
4. Indiquez les informations requises suivantes, puis cliquez sur **suivant**:
   - Le nom de famille
   - Prénom ou le nom donné
   - Nom de la société
   - Pays/région

     Vous pouvez également fournir les informations facultatives demandées, telles que l’adresse de la société, adresse e-mail et numéro de téléphone. Dans le champ d’unité d’organisation, vous pouvez décrire l’unité au sein de votre organisation qui sert de ce serveur de licences.

5. Le programme de licence que vous avez sélectionné dans la page précédente détermine les informations que vous devez fournir dans la page suivante. Dans la plupart des cas, vous devez fournir un code de licence ou un numéro de contrat. Consultez la documentation fournie lorsque vous avez acheté vos CAL de services Bureau à distance. En outre, vous devez spécifier le type de licence d’accès client RDS et la quantité que vous souhaitez migrer vers le serveur de licences.
6. Après avoir entré les informations demandées, cliquez sur **Suivant**.
7. Vérifiez que toutes les informations que vous avez entré est correct, puis cliquez sur **suivant** pour envoyer votre demande à Microsoft Clearinghouse. La page web affiche ensuite un ID de jeu de clés de licence généré par Microsoft Clearinghouse.

   > [!IMPORTANT] 
   > Conserver une copie de l’ID de jeu de clés de licence. Cette information vous facilite les communications avec Microsoft Clearinghouse, si vous avez besoin une assistance pour récupérer des licences d’accès client RDS.

8. Sur le même **obtenir le jeu de clés de licence Client** page, entrez l’ID de jeu de clés de licence, puis cliquez sur **suivant** pour migrer les licences RDS CAL vers votre serveur de licences.
9. Cliquez sur **Terminer** pour terminer le processus de migration de RDS CAL.

### <a name="using-a-telephone"></a>À l’aide d’un téléphone
1. Sur le **obtenir le jeu de clés de licence Client** page, utilisez le numéro de téléphone affiché pour appeler Microsoft Clearinghouse. Communiquez au représentant l’ID de votre serveur de licences bureau à distance et les informations requises pour le programme de licence par le biais duquel vous avez acheté vos CAL de services Bureau à distance. Le représentant traite votre demande pour migrer les licences RDS CAL, puis vous donne un ID unique pour ces licences. Cet ID unique est appelé le **ID de jeu de clés de licence**.

   > [!IMPORTANT]
   > Conserver une copie de l’ID de jeu de clés de licence. Cette information vous facilite les communications avec Microsoft Clearinghouse si vous avez besoin une assistance pour récupérer des licences d’accès client RDS.

2. Sur le même **obtenir le jeu de clés de licence Client** page, entrez l’ID de jeu de clés de licence, puis cliquez sur **suivant** pour migrer les licences RDS CAL vers votre serveur de licences.
3. Cliquez sur **Terminer** pour terminer le processus de migration de RDS CAL.
