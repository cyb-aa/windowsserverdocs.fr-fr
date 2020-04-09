---
title: Joindre des ordinateurs au nouveau Serveur1 Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: cdfa9504-9881-4265-b308-c7ee8721bfaa
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: db00a395ebdb77ef65d99f58e6bbfbd9db4e53ed
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852562"
---
# <a name="join-computers-to-the-new-windows-server-essentials-server1"></a>Joindre des ordinateurs au nouveau Serveur1 Windows Server Essentials

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_JoinComputers"></a>   
 L’étape suivante du processus de migration consiste à joindre des ordinateurs clients au nouveau réseau Windows Server Essentials et à mettre à jour les paramètres de stratégie de groupe.  
  
> [!NOTE]
>  Si un ordinateur client est déjà joint au serveur source, vous devez d'abord désinstaller le logiciel Connecteur sur l'ordinateur client avant de connecter l'ordinateur au serveur de destination.  
  
 Le processus de connexion d'un ordinateur client au serveur est le même pour les ordinateurs appartenant ou non à un domaine.  
  
- Accédez à **http://** <em>nom-du-serveur-de-destination</em> **/connect** et installez le logiciel Connecteur Windows Server comme s'il s'agissait d'un nouvel ordinateur.  
  
> [!NOTE]
>  Le logiciel Connecteur Windows Server ne prend pas en charge les ordinateurs qui exécutent Windows XP ou Windows Vista. Si vous utilisez des ordinateurs exécutant Windows XP ou Windows Vista qui sont déjà membres du domaine, vous pouvez ignorer cette étape.  
  
### <a name="ensure-that-group-policy-has-updated"></a>Vérifier que la stratégie de groupe a été mise à jour  
  
> [!NOTE]
>  Il s'agit d'une étape facultative, et elle est uniquement requise si le serveur source a été configuré avec des paramètres de stratégie de groupe personnalisés tels que la redirection de dossiers.  
  
 Alors que le serveur source et le serveur de destination sont toujours en ligne, vous devez vous assurer que les paramètres de stratégie de groupe ont été répliqués du serveur de destination vers les ordinateurs clients. Sur chaque ordinateur client, procédez comme suit :  
  
1.  Ouvrez une fenêtre d'invite de commandes.  
  
2.  À l'invite de commandes, tapez **GPRESULT /R** et appuyez sur Entrée.  
  
3.  Examinez la sortie résultante pour la section stratégie de groupe a été appliquée à partir de : et assurez-vous qu’elle répertorie le serveur de destination, par exemple **DestinationSrv. domain. local**. Par exemple :  
  
    ```  
    USER SETTINGS  
    --------------  
        CN=User,OU=Users,DC=DOMAIN,DC=Local  
        Last time Group Policy was applied: 1/24/2011 at 1:26:27 PM  
        Group Policy was applied from:      DestinationSrv.Domain.local  
        Group Policy slow link threshold:   500 kbps  
        Domain Name:                        Domain  
        Domain Type:                        Windows 2011  
  
    ```  
  
4.  Si le serveur de destination n'est pas répertorié, à l'invite de commandes, tapez **gpupdate /force**, puis appuyez sur Entrée pour actualiser les paramètres de stratégie de groupe. Ensuite, exécutez de nouveau la procédure précédente.  
  
5.  Si le serveur de destination n'apparaît pas, une erreur peut s'être produite dans les paramètres de stratégie de groupe ou dans leur application à cet ordinateur client spécifique. Si le serveur de destination n'apparaît pas, procédez comme suit :  
  
    1.  Cliquez sur **Démarrer**, cliquez sur **Exécuter**, tapez **rsop.msc** (jeu de stratégie résultant), puis appuyez sur Entrée.  
  
    2.  Développez l’arborescence avec la Croix (X) jusqu’à ce que vous obteniez un nœud.  
  
    3.  Cliquez avec le bouton droit sur le nœud, puis cliquez sur **Afficher l'erreur** pour obtenir des informations sur la raison de l'échec des paramètres de stratégie de groupe sur l'ordinateur répertorié.
