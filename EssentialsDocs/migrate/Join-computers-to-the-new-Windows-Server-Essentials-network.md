---
title: Joindre des ordinateurs pour la nouvelle network1 de WindowsServerEssentials
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d94de050-3300-4323-a5ea-c824cb9cecc9
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c6abc11ba2ce8a9f1d32c6a884db6332586de78b
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="join-computers-to-the-new-windows-server-essentials-network1"></a>Joindre des ordinateurs pour la nouvelle network1 de WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

##  <a name="BKMK_JoinComputers"></a>   
 L’étape suivante du processus de migration consiste à joindre des ordinateurs clients au nouveau réseau WindowsServerEssentials et mettre à jour les paramètres de stratégie de groupe.  
  
### <a name="domain-joined-client-computers"></a>Ordinateurs clients joints au domaine  
 Accédez à **http://***destination-servername***/Connect** et installez le logiciel Connecteur Windows Server comme s’il s’agissait d’un nouvel ordinateur. Le processus d’installation est le même pour les ordinateurs clients joints au domaine ou non joint au domaine.  
  
> [!NOTE]
>  Le logiciel Connecteur Windows Server ne prend pas en charge les ordinateurs qui exécutent WindowsXP ou WindowsVista. Si vous avez des ordinateurs exécutant WindowsXP ou WindowsVista qui sont déjà joint au domaine, vous pouvez ignorer cette étape.  
  
### <a name="non-domain-joined-client-computers"></a>Ordinateurs clients joints au domaine non  
 Accédez à **http://***destination-servername***/Connect** et installez le logiciel Connecteur Windows Server comme s’il s’agissait d’un nouvel ordinateur. Le processus d’installation est le même pour joint au domaine ou les ordinateurs clients joints au domaine.  
  
> [!NOTE]
>  Le logiciel Connecteur Windows Server ne prend pas en charge les ordinateurs qui exécutent WindowsXP ou WindowsVista. Si vous avez des ordinateurs exécutant WindowsXP ou WindowsVista qui sont déjà joint au domaine, vous pouvez ignorer cette étape.  
  
### <a name="ensure-that-group-policy-has-updated"></a>Assurez-vous que la stratégie de groupe a mis à jour  
  
> [!NOTE]
>  Cette étape est facultative, et il est uniquement requise si le serveur Source a été configuré avec des paramètres de stratégie de groupe personnalisés tels que la Redirection de dossiers.  
  
 Alors que le serveur Source et le serveur de Destination sont toujours en ligne, vous devez vous assurer que la stratégie de groupe paramètres ont été répliqués à partir du serveur de Destination sur les ordinateurs clients. Sur chaque ordinateur client, procédez comme suit:  
  
1.  Ouvrez une fenêtre d’invite de commandes.  
  
2.  À l’invite de commandes, tapez **GPRESULT /R**, puis appuyez sur ENTRÉE.  
  
3.  Examinez la sortie de la section stratégie de groupe a été appliquée à partir de: et assurez-vous qu’elle répertorie le serveur de Destination, tel que **DestinationSrv.Domain.local**. Par exemple:  
  
    ```  
    USER SETTINGS  
    --------------  
        CN=User,OU=Users,DC=DOMAIN,DC=Local  
        Last time Group Policy was applied: 1/24/2011 at 1:26:27 PM  
        Group Policy was applied from:      DestinationSrv.Domain.local  
        Group Policy slow link threshold:   500 kbps  
        Domain Name:                        Domain  
        Domain Type:                        Windows 2008  
  
    ```  
  
4.  Si le serveur de Destination n’est pas répertorié, à une invite de commandes, tapez **gpupdate /force**, puis appuyez sur ENTRÉE pour actualiser les paramètres de stratégie de groupe. Puis exécutez de nouveau la procédure précédente.  
  
5.  Si le serveur de Destination n’apparaît toujours pas, il peut être une erreur dans les paramètres de stratégie de groupe ou une erreur dans leur application à cet ordinateur client spécifique. Si le serveur de Destination n’apparaît pas, procédez comme suit:  
  
    1.  Cliquez sur **Démarrer**, cliquez sur **exécuter**, type **RSoP.msc** (jeu de stratégie résultant), puis appuyez sur ENTRÉE.  
  
    2.  Développez l’arborescence avec X jusqu'à ce que vous arriviez à un nœud.  
  
    3.  Cliquez sur le nœud, puis cliquez sur **afficher l’erreur** échouent pour plus d’informations sur la raison pour laquelle les paramètres de stratégie de groupe sur l’ordinateur répertorié.
