---
title: 'Étape 2 : Installer Windows Server Essentials en tant que nouveau contrôleur de domaine réplica'
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c7ccfc34-63fd-436b-a1cd-e05810f60bfe
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 5968db77c091dbca1eb7d38f5e924e5f449052ce
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318774"
---
# <a name="step-2-install-windows-server-essentials-as-a-new-replica-domain-controller"></a>Étape 2 : Installer Windows Server Essentials en tant que nouveau contrôleur de domaine réplica

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Cette section décrit comment installer Windows Server Essentials et Windows Server 2012 R2 Standard (avec le rôle expérience Windows Server Essentials activé) en tant que contrôleur de domaine.  
  
 Pour les environnements comprenant jusqu’à 25 utilisateurs et 50 appareils, vous pouvez suivre les étapes de ce guide pour effectuer une migration à partir de versions précédentes de Windows SBS vers Windows Server Essentials. Pour les environnements comprenant jusqu’à 100 utilisateurs et 200 appareils, vous pouvez suivre les mêmes instructions pour migrer vers les éditions standard et Datacenter de Windows Server 2012 R2 avec le rôle expérience Windows Server Essentials installé. Les deux scénarios sont traités dans cette documentation.  
  
> [!IMPORTANT]
>  Si vous migrez vers Windows Server Essentials, le message d’erreur suivant est ajouté quotidiennement au journal des événements pendant la période de grâce de 21 jours, jusqu’à ce que vous supprimiez le serveur source de votre réseau. Après la période de grâce de 21 jours, le serveur source s'arrête. <br> **La vérification du rôle FSMO a détecté dans votre environnement une condition non conforme à la stratégie de licence. Le serveur d’administration doit contenir le contrôleur de domaine principal et le maître d’attribution de noms de domaine Active Directory rôles. Déplacez les rôles de Active Directory vers le serveur d’administration maintenant. Ce serveur sera automatiquement arrêté si le problème n’est pas résolu dans les 21 jours à partir de la première détection de la condition**.   
  
#### <a name="install-windows-server-essentials-or-windows-server-2012-r2-standard-on-the-destination-server"></a>Installer Windows Server Essentials ou Windows Server 2012 R2 Standard sur le serveur de destination  
  
1.  Installez Windows Server Essentials ou Windows Server 2012 R2 Standard avec le rôle expérience Windows Server Essentials activé en suivant les instructions de la procédure d' [installation et de configuration de Windows Server Essentials](../install/Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).  
  
    > [!NOTE]
    >  Si l'Assistant Configuration de Windows Server Essentials démarre, annulez-le.  
  
2.  Transférez les rôles FSMO de votre serveur source.  
  
    > [!NOTE]
    >  Si Windows Server Essentials est le seul contrôleur de domaine dans le domaine, le rôle FSMO est automatiquement déplacé vers le serveur exécutant Windows Server Essentials lorsque vous rétrogradez le serveur source.  
  
3.  Ouvrez le Gestionnaire de serveur et exécutez l'Assistant Ajout de rôles et de fonctionnalités.  
  
4.  S'il n'est pas installé, ajoutez le rôle Expérience Windows Server Essentials.  
  
5.  Une fois ce rôle installé, la tâche Configurer Windows Server Essentials apparaît dans la zone de notification. Cliquez sur la tâche pour lancer l'Assistant Configuration de Windows Server Essentials.  
  
6.  Suivez les instructions pour terminer la configuration de Windows Server Essentials. Avant d'exécuter l'Assistant, procédez comme suit :  
  
    -   Modifiez le nom du serveur si nécessaire, car vous ne pouvez pas modifier le nom après avoir terminé l'Assistant Configuration de Windows Server Essentials.  
  
    -   Vérifiez que l’heure et les paramètres du serveur sont corrects.  
  
7.  Vérifiez l'installation comme suit :  
  
    1.  Ouvrez le Tableau de bord.  
  
    2.  Cliquez sur l'onglet **Utilisateurs** et vérifiez que les comptes d'utilisateur dans Active Directory sont répertoriés.  
  
### <a name="transfer-the-operations-master-roles"></a>Transférer les rôles de maître d’opérations  
 Les rôles de maître d’opérations (également appelés opérations à maître unique flottant ou FSMO) doivent être transférés du serveur source vers le serveur de destination dans un délai de 21 jours après l’installation de Windows Server Essentials sur le serveur de destination.  
  
##### <a name="to-transfer-the-operations-master-roles"></a>Pour transférer les rôles de maître d’opérations  
  
1.  Sur le serveur de destination, ouvrez une fenêtre d'invite de commandes en tant qu'administrateur. Consultez [Pour ouvrir une fenêtre d’invite de commandes en tant qu’administrateur](https://technet.microsoft.com/library/cc947813\(v=WS.10\).aspx).  
  
2.  À l’invite de commandes, tapez **NETDOM QUERY FSMO** et appuyez ensuite sur Entrée.  
  
3.  À l’invite de commandes, tapez **ntdsutil** et appuyez ensuite sur Entrée.  
  
4.  À l'invite de commandes **ntdsutil**, entrez les commandes suivantes :  
  
    1.  Tapez **activate instance NTDS** et appuyez sur ENTRÉE.  
  
    2.  Tapez **roles** et appuyez sur Entrée.  
  
    3.  Tapez **connections** et appuyez sur Entrée.  
  
    4.  Tapez **Connect to server** *< servername\>* (où *< servername\>* est le nom du serveur de destination), puis appuyez sur entrée.  
  
    5.  À l’invite de commandes, tapez **q** et appuyez sur Entrée.  
  
        1.  Tapez **transfer PDC**, appuyez sur ENTRÉE, puis cliquez sur **Oui** dans la boîte de dialogue **Confirmation de transfert de rôle**.  
  
        2.  Tapez **transfer infrastructure master**, appuyez sur Entrée, puis cliquez sur **Oui** dans la boîte de dialogue **Confirmation de transfert de rôle**.  
  
        3.  Tapez **transfer naming master**, appuyez sur Entrée, puis cliquez sur **Oui** dans la boîte de dialogue **Confirmation de transfert de rôle**.  
  
        4.  Tapez **transfer RID master**, appuyez sur Entrée, puis cliquez sur **Oui** dans la boîte de dialogue **Confirmation de transfert de rôle**.  
  
        5.  Tapez **transfer schema master**, appuyez sur Entrée, puis cliquez sur **Oui** dans la boîte de dialogue **Confirmation de transfert de rôle**.  
  
    6.  Tapez **q** et appuyez sur Entrée pour revenir à l’invite de commandes.  
  
> [!NOTE]
>  À partir de n'importe quel serveur sur le réseau, vous pouvez vérifier que les rôles de maître d'opérations ont été transférés vers le serveur de destination. Ouvrez une fenêtre d’invite de commandes en tant qu’administrateur (pour plus d’informations, consultez [Pour ouvrir une fenêtre d’invite de commandes en tant qu’administrateur](https://technet.microsoft.com/library/cc947813\(v=WS.10\).aspx)). Tapez **netdom query fsmo** et appuyez ensuite sur Entrée.  
  
## <a name="next-steps"></a>Étapes suivantes :  
 Vous avez installé Windows Server Essentials en tant que nouveau contrôleur de domaine réplica. Passez maintenant à [l’étape 3 : joindre des ordinateurs au nouveau serveur Windows Server Essentials](Step-3--Join-computers-to-the-new-Windows-Server-Essentials-server.md).  
  
Pour afficher toutes les étapes, consultez [migrer vers Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

