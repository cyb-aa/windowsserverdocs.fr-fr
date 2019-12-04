---
title: Identifier et résoudre les problèmes de fonctionnement du serveur d'accès à distance
description: Cette rubrique fait partie du Guide d’analyse et de gestion de l’accès à distance dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7ce84c9f-fd1f-4463-8fc7-d2f33344a2c9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 831f484db8325bf9a27e9065ac5cf74913d0805c
ms.sourcegitcommit: 4a03f263952c993dfdf339dd3491c73719854aba
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74791159"
---
# <a name="identify-and-resolve-remote-access-server-operations-problems"></a>Identifier et résoudre les problèmes de fonctionnement du serveur d'accès à distance

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

**Remarque :** Windows Server 2012 associe DirectAccess et le service Routage et accès distant (RRAS) dans un rôle Accès à distance unique.  
  
Vous pouvez utiliser les procédures suivantes pour identifier les problèmes liés aux opérations de serveur d’accès à distance, leurs causes racines et la résolution requise pour résoudre les problèmes.  
  
> [!NOTE]  
> Vous devez être connecté en tant que membre du groupe Admins du domaine ou membre du groupe Administrateurs sur chaque ordinateur pour effectuer les tâches décrites dans cette rubrique. Si vous ne pouvez pas effectuer une tâche lorsque vous êtes connecté avec un compte membre du groupe administrateurs, essayez d’exécuter la tâche lorsque vous êtes connecté avec un compte membre du groupe Admins du domaine.  
  
Cette rubrique contient des informations sur l’exécution des tâches suivantes :  
  
- Simuler un problème d’opérations  
  
- Identifier le problème des opérations et prendre une mesure corrective  
  
- Restaurer le service d’assistance IP  
  
### <a name="BKMK_Simulate"></a>Simuler un problème d’opérations  
  
> [!CAUTION]  
> Comme votre serveur d’accès à distance est probablement correctement configuré et ne rencontre aucun problème, vous pouvez utiliser la procédure suivante pour simuler un problème d’opérations. Si votre serveur est en train de traiter des clients dans un environnement de production, vous ne souhaiterez peut-être pas effectuer ces actions pour l’instant. Au lieu de cela, vous pouvez lire les étapes pour comprendre comment résoudre les problèmes susceptibles de survenir sur votre serveur d’accès à distance à l’avenir.  
  
Le service d’assistance IP (IPHlpSvc) héberge des technologies de transition IPv6 (par exemple, IP-HTTPs, 6to4 ou Teredo) et il est nécessaire pour que le serveur DirectAccess fonctionne correctement. Pour illustrer un problème d’opérations simulées sur le serveur d’accès à distance, vous devez arrêter le service réseau (IPHlpSvc).  
  
##### <a name="to-stop-the-ip-helper-service"></a>Pour arrêter le service d’assistance IP  
  
1.  Sur l’écran d' **Accueil** du serveur d’accès à distance, cliquez sur **Outils d’administration**, puis double-cliquez sur **services**.  
  
2.  Dans la liste des **services**, faites défiler la liste, cliquez avec le bouton droit sur **assistance IP**, puis cliquez sur **arrêter**.  
  
### <a name="BKMK_Identify"></a>Identifier le problème des opérations et prendre une mesure corrective  
La désactivation du service d’assistance IP entraîne une erreur grave sur le serveur d’accès à distance. Le tableau de bord de surveillance affiche l’état des opérations du serveur et les détails du problème.  
  
##### <a name="to-identify-the-details-and-take-corrective-action"></a>Pour identifier les détails et prendre des mesures correctives  
  
1.  Dans **Gestionnaire de serveur**, cliquez sur **Outils**, puis sur **Gestion de l'accès à distance**.  
  
2.  Cliquez sur **TABLEAU DE BORD** pour accéder à **Tableau de bord des accès distants** dans la **Console de gestion de l'accès distant**.  
  
3.  Assurez-vous que votre serveur d’accès à distance est sélectionné dans le volet gauche, puis dans le volet central, cliquez sur **État des opérations**.  
  
4.  La liste des composants avec des icônes vertes ou rouges s’affiche, indiquant leur état opérationnel. Cliquez sur la ligne **IP-HTTPS** dans la liste. Lorsque vous avez sélectionné une ligne, les détails de l’opération s’affichent dans le volet d' **informations** comme suit :  
  
    **Erreurs**  
  
    Le service d’assistance IP (IPHlpSvc) s’est arrêté. DirectAccess peut ne pas fonctionner comme prévu. Le service d’assistance IP fournit une connectivité de tunnel à l’aide de la plateforme de connectivité, des technologies de transition IPv6 et d’IP-HTTPs.  
  
    **Donne**  
  
    1.  Le service d’assistance IP s’est arrêté.  
  
    2.  Le service d’assistance IP ne répond pas.  
  
    **Résolution**  
  
    1.  Pour vous assurer que le service est en cours d’exécution, tapez **« obtenir-service iphlpsvc »** dans une invite de commandes Windows PowerShell.  
  
    2.  Pour activer le service, tapez **Start-Service iphlpsvc** à partir d’une invite Windows PowerShell avec élévation de privilèges.  
  
    3.  Pour redémarrer le service, tapez **Restart-Service iphlpsvc** à partir d’une invite Windows PowerShell avec élévation de privilèges.  
  
### <a name="BKMK_Restart"></a>Restaurer le service d’assistance IP  
Pour restaurer le service d’assistance IP sur votre serveur d’accès à distance, vous pouvez suivre les étapes de résolution ci-dessus pour démarrer ou redémarrer le service, ou vous pouvez utiliser la procédure suivante pour inverser la procédure utilisée pour simuler l’échec du service d’assistance IP.  
  
##### <a name="to-restart-the-ip-helper-service-on-the-remote-access-server"></a>Pour redémarrer le service d’assistance IP sur le serveur d’accès à distance  
  
1.  Dans l’écran d' **Accueil** , cliquez sur **Outils d’administration**, puis double-cliquez sur **services**.  
  
2.  Dans la liste des **services**, faites défiler la liste, cliquez avec le bouton droit sur **assistance IP**, puis cliquez sur **Démarrer**.  
  
![les commandes Windows PowerShell](../../../media/Identify-and-resolve-Remote-Access-server-operations-problems/PowerShellLogoSmall.gif)***<em>équivalentes</em> Windows PowerShell***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```PowerShell
PS> Get-RemoteAccessHealth | Where-Object {$_.Component -eq "IP-HTTPS"} | Format-List -Property *  
```
