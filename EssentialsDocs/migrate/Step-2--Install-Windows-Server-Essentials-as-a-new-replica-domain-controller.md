---
title: "Étape2: Installer WindowsServerEssentials en tant que nouveau contrôleur de domaine réplica"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c7ccfc34-63fd-436b-a1cd-e05810f60bfe
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 757012b7d1a57a001e3b55cdc0604b63852a3d3c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="step-2-install-windows-server-essentials-as-a-new-replica-domain-controller"></a>Étape2: Installer WindowsServerEssentials en tant que nouveau contrôleur de domaine réplica

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Cette section décrit comment installer un contrôleur de domaine WindowsServerEssentials et Windows Server2012R2 Standard (avec le rôle expérience WindowsServerEssentials activé).  
  
 Pour les environnements comptant jusqu'à 25utilisateurs et 50appareils, vous pouvez suivre les étapes décrites dans ce guide pour migrer à partir de versions antérieures de Windows SBS vers WindowsServerEssentials. Pour les environnements comptant jusqu'à 100utilisateurs et 200appareils, vous pouvez suivre les mêmes recommandations pour migrer vers les éditions Standard et Datacenter de Windows Server2012R2 avec le rôle expérience WindowsServerEssentials installé. Les deux scénarios sont traités dans cette documentation.  
  
> [!IMPORTANT]
>  Si vous migrez vers WindowsServerEssentials, le message d’erreur suivant est ajouté au journal des événements chaque jour pendant la période de grâce de 21jours jusqu'à la suppression du serveur Source à partir de votre réseau. Après la période de grâce de 21jours, le serveur Source s’arrête. <br> **La vérification du rôle FSMO a détecté une condition dans votre environnement qui est conformes à la stratégie de licence. Le serveur d’administration doit contenir le contrôleur de domaine principal et les rôles de maître d’ActiveDirectory de noms de domaine. Déplacez les rôles ActiveDirectory vers le serveur d’administration maintenant. Ce serveur sera automatiquement arrêté si le problème n’est pas résolu dans les 21jours à partir de la première détection de la condition**.   
  
#### <a name="install-windows-server-essentials-or-windows-server-2012-r2-standard-on-the-destination-server"></a>Installer WindowsServerEssentials ou Windows Server2012R2 Standard sur le serveur de Destination  
  
1.  Installer WindowsServerEssentials ou Windows Server2012R2 Standard avec le rôle expérience WindowsServerEssentials activé en suivant les instructions de [installer et configurer WindowsServerEssentials](../install/Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).  
  
    > [!NOTE]
    >  Si l’Assistant Configurer WindowsServerEssentials démarre, annulez-le.  
  
2.  Transférer les rôles FSMO de votre serveur Source.  
  
    > [!NOTE]
    >  Si WindowsServerEssentials est le seul contrôleur de domaine dans le domaine, le rôle FSMO est automatiquement déplacé vers le serveur exécutant WindowsServerEssentials quand vous rétrogradez le serveur Source.  
  
3.  Ouvrez le Gestionnaire de serveur et exécutez l’Assistant Ajout de rôles et fonctionnalités.  
  
4.  Le cas contraire, ajoutez le rôle expérience WindowsServerEssentials.  
  
5.  Après avoir installé le rôle expérience WindowsServerEssentials, la tâche Configurer WindowsServerEssentials apparaît dans la zone de notification. Cliquez sur la tâche pour lancer l’Assistant Configurer WindowsServerEssentials.  
  
6.  Suivez les instructions pour terminer la configuration de WindowsServerEssentials. Avant d’exécuter l’Assistant, procédez comme suit:  
  
    -   Modifier le nom du serveur si nécessaire, car vous ne pouvez pas modifier le nom après avoir terminé l’Assistant Configurer WindowsServerEssentials.  
  
    -   Assurez-vous que l’heure et les paramètres sont corrects.  
  
7.  Vérifier l’installation, comme suit:  
  
    1.  Ouvrez le tableau de bord.  
  
    2.  Cliquez sur **utilisateurs** onglet et vérifiez que les comptes d’utilisateur dans ActiveDirectory sont répertoriés.  
  
### <a name="transfer-the-operations-master-roles"></a>Transférer les rôles de maître d’opérations  
 Les rôles de maître d’opérations (également appelé opérations à maître uniques flottant ou FSMO) opérations doivent être transférés du serveur Source vers le serveur de Destination sous 21jours après l’installation de WindowsServerEssentials sur le serveur de Destination.  
  
##### <a name="to-transfer-the-operations-master-roles"></a>Pour transférer les rôles de maître d’opérations  
  
1.  Sur le serveur de Destination, ouvrez une fenêtre d’invite de commandes en tant qu’administrateur. Voir [pour ouvrir une fenêtre d’invite de commandes en tant qu’administrateur](https://technet.microsoft.com/library/cc947813\(v=WS.10\).aspx).  
  
2.  À l’invite de commandes, tapez **NETDOM QUERY FSMO**, puis appuyez sur ENTRÉE.  
  
3.  À l’invite de commandes, tapez **ntdsutil**, puis appuyez sur ENTRÉE.  
  
4.  À la **ntdsutil** invite de commandes, entrez les commandes suivantes:  
  
    1.  Type **activer instance NTDS**, puis appuyez sur ENTRÉE.  
  
    2.  Type **rôles**, puis appuyez sur ENTRÉE.  
  
    3.  Type **connexions**, puis appuyez sur ENTRÉE.  
  
    4.  Type **se connecter au serveur***< serverName\ >* (où *< serverName\ >* est le nom du serveur de Destination), puis appuyez sur ENTRÉE.  
  
    5.  À l’invite de commandes, tapez **q**, puis appuyez sur ENTRÉE.  
  
        1.  Type **transfer PDC**, appuyez sur entrée, puis cliquez sur **Oui** dans les **Confirmation de transfert de rôle** boîte de dialogue.  
  
        2.  Type **maître d’infrastructure transfert**, appuyez sur entrée, puis cliquez sur **Oui** dans les **Confirmation de transfert de rôle** boîte de dialogue.  
  
        3.  Type **transfert du maître d’attribution de noms**, appuyez sur entrée, puis cliquez sur **Oui** dans les **Confirmation de transfert de rôle** boîte de dialogue.  
  
        4.  Type **transférer le maître RID**, appuyez sur entrée, puis cliquez sur **Oui** dans les **Confirmation de transfert de rôle** boîte de dialogue.  
  
        5.  Type **contrôleur de schéma de transfert**, appuyez sur entrée, puis cliquez sur **Oui** dans les **Confirmation de transfert de rôle** boîte de dialogue.  
  
    6.  Type **q**, puis appuyez sur ENTRÉE pour revenir à l’invite de commandes.  
  
> [!NOTE]
>  À partir de n’importe quel serveur sur le réseau, vous pouvez vérifier que les rôles de maître d’opérations ont été transférés vers le serveur de Destination. Ouvrez une fenêtre d’invite de commandes en tant qu’administrateur (pour plus d’informations, voir [pour ouvrir une fenêtre d’invite de commandes en tant qu’administrateur](https://technet.microsoft.com/library/cc947813\(v=WS.10\).aspx)). Type **netdom query fsmo**, puis appuyez sur ENTRÉE.  
  
## <a name="next-steps"></a>Étapes suivantes  
 Vous avez installé WindowsServerEssentials en tant que nouveau contrôleur de domaine réplica. Passez maintenant à [étape3: joindre des ordinateurs au nouveau serveur WindowsServerEssentials](Step-3--Join-computers-to-the-new-Windows-Server-Essentials-server.md).  
  
Pour afficher toutes les étapes, voir [migrer vers WindowsServerEssentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

