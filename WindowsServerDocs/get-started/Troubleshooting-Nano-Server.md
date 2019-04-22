---
title: Résolution des problèmes de Nano Server
description: Console de récupération, services de gestion d’urgence, débogage du noyau
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.date: 09/06/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e427c66f-9571-4b8c-b65d-e7370d91544d
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 0f5d3e352cd022853a1602c67c3aaf2530cfc696
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813640"
---
# <a name="troubleshooting-nano-server"></a>Résolution des problèmes de Nano Server

>S'applique à : Windows Server 2016

> [!IMPORTANT]
> À compter de Windows Server, version 1709, Nano Server sera uniquement disponible sous forme d’[image du système d’exploitation de base du conteneur](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Consultez [Modifications apportées à Nano Server](nano-in-semi-annual-channel.md) pour en savoir plus. 

Cette rubrique contient des informations sur les outils que vous pouvez utiliser pour la connexion aux installations Nano Server, et pour diagnostiquer et réparer ces dernières.  
  
## <a name="using-the-nano-server-recovery-console"></a>Utilisation de la console de récupération Nano Server 
 
Nano Server inclut une console de récupération qui vous assure un accès à votre serveur Nano Server même si une erreur de configuration du réseau interfère avec la connexion à ce serveur. Vous pouvez utiliser la console de récupération pour corriger le problème réseau, puis vous servir de vos outils de gestion à distance habituels.  
  
Lorsque vous démarrez Nano Server dans une machine virtuelle ou sur un ordinateur physique équipé d’un moniteur et d’un clavier, une invite d’ouverture de session en mode texte s’affiche en plein écran. Connectez-vous avec un compte Administrateur pour afficher le nom d’ordinateur et l’adresse IP du serveur Nano Server. Vous pouvez utiliser les commandes suivantes pour naviguer dans cette console :  
  
-   Utilisez les touches de direction pour faire défiler l’écran.  
  
-   Utilisez la touche TAB pour vous déplacer jusqu’à n’importe quel texte qui commence par **>**, puis appuyez sur ENTRÉE pour effectuer la sélection.  
  
-   Pour revenir à l’écran ou à la page précédente, appuyez sur Échap. Si vous appuyez sur Échap alors que vous vous trouvez dans la page d’accueil, votre session sera fermée.  
  
-   Certains écrans affichent des fonctionnalités supplémentaires à la dernière ligne. Par exemple, si vous explorez une carte réseau, vous pouvez la désactiver à l’aide de la touche F4.  
  
La console de récupération vous permet d’afficher et de configurer les cartes réseau et les paramètres TCP/IP, ainsi que les règles de pare-feu.
> [!NOTE]  
    > La console de récupération ne prend en charge que les fonctions de base du clavier. Les voyants du clavier, le pavé numérique et les touches de changement de la disposition du clavier, comme Verr. maj. et Verr. num., ne sont pas pris en charge. Seuls les claviers et le jeu de caractères anglais sont pris en charge.

## <a name="accessing-nano-server-over-a-serial-port-with-emergency-management-services"></a>Accès à Nano Server via un port série avec les services de gestion d’urgence  
Les services de gestion d’urgence (EMS) vous permettent d’effectuer des opérations de dépannage de base, de consulter l’état du réseau et d’ouvrir des sessions de console (y compris CMD/PowerShell) en utilisant un émulateur de terminal via un port série. Grâce à cette méthode, vous n’avez pas besoin de recourir à un clavier et à un moniteur pour résoudre les problèmes d’un serveur. Pour plus d’informations sur EMS, consultez [Informations techniques de référence sur les services de gestion d’urgence](https://technet.microsoft.com/library/cc784411(v=ws.10).aspx).

Pour activer EMS sur une image Nano Server afin de pouvoir utiliser ultérieurement ces services en cas de besoin, exécutez l’applet de commande suivante :  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\EnablingEMS.vhdx   -EnableEMS   -EMSPort 3   -EMSBaudRate 9600`  
  
Cet exemple d’applet de commande active EMS sur le port série 3 avec une vitesse (en bauds) de 9 600 bit/s. Si vous n’incluez pas ces paramètres, le port 1 et 115 200 bit/s sont utilisés comme valeurs par défaut. Pour utiliser cette applet de commande avec les médias VHDX, veillez à inclure la fonctionnalité Hyper-V et les modules Windows PowerShell correspondants.

## <a name="kernel-debugging"></a>Débogage du noyau  
Vous pouvez configurer l’image Nano Server de sorte qu’elle prenne en charge plusieurs méthodes de débogage du noyau. Pour utiliser le débogage du noyau avec une image VHDX, veillez à inclure la fonctionnalité Hyper-V et les modules Windows PowerShell correspondants. Pour plus d’informations sur le débogage à distance du noyau en général, consultez [Setting Up Kernel-Mode Debugging over a Network Cable Manually](https://msdn.microsoft.com/library/windows/hardware/hh439346%28v=vs.85%29.aspx) (Configuration manuelle du débogage en mode noyau via un câble réseau) et [Remote Debugging Using WinDbg](https://msdn.microsoft.com/library/windows/hardware/hh451173%28v=vs.85%29.aspx) (Débogage à distance à l’aide de WinDbg).  
  
### <a name="debugging-using-a-serial-port"></a>Débogage à l’aide d’un port série  
Pour activer le débogage de l’image à l’aide d’un port série, utilisez l’exemple d’applet de commande suivant :  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\KernelDebuggingSerial   -DebugMethod Serial   -DebugCOMPort 1   -DebugBaudRate 9600`  
  
Cet exemple active le débogage sur le port série 2 avec une vitesse (en bauds) de 9 600 bit/s. Si vous ne spécifiez pas ces paramètres, le port 2 et 115 200 bit/s sont utilisés comme valeurs par défaut. Si vous prévoyez d’utiliser EMS et le débogage du noyau, vous devez configurer deux ports série distincts pour leur utilisation.  
  
### <a name="debugging-over-a-tcpip-network"></a>Débogage via un réseau TCP/IP  
Pour activer le débogage via un réseau TCP/IP, utilisez l’exemple d’applet de commande suivant :  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\KernelDebuggingNetwork   -DebugMethod Net   -DebugRemoteIP 192.168.1.100   -DebugPort 64000`  
  
Cette applet de commande active le débogage du noyau de sorte que seul l’ordinateur présentant l’adresse IP 192.168.1.100 est autorisé à se connecter, toutes les communications transitant via le port 64000. Les paramètres -DebugRemoteIP et -DebugPort sont obligatoires, avec un numéro de port supérieur à 49152. Cette applet de commande génère une clé de chiffrement dans un fichier accompagnant le disque dur virtuel créé, qui est requise pour la communication via le port. Vous pouvez également spécifier votre propre clé avec le paramètre -DebugKey, comme dans l’exemple suivant :  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\KernelDebuggingNetwork   -DebugMethod Net   -DebugRemoteIP 192.168.1.100   -DebugPort 64000   -DebugKey 1.2.3.4`  
  
### <a name="debugging-using-the-ieee1394-protocol-firewire"></a>Débogage à l’aide du protocole IEEE 1394 (FireWire)  
Pour activer le débogage via IEEE 1394, utilisez l’exemple d’applet de commande suivant :  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\KernelDebuggingFireWire   -DebugMethod 1394   -DebugChannel 3`  
  
Le paramètre -DebugChannel est obligatoire.  
  
### <a name="debugging-using-usb"></a>Débogage via USB  
Pour activer le débogage via USB, utilisez l’applet de commande suivante :  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\KernelDebuggingUSB   -DebugMethod USB   -DebugTargetName KernelDebuggingUSBNano`  
  
Lorsque vous connectez le débogueur distant au serveur Nano Server créé, spécifiez le nom de la cible défini par le paramètre -DebugTargetName.    