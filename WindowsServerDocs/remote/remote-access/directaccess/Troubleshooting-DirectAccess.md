---
title: Dépannage de DirectAccess
description: Cette rubrique fournit des informations sur le dépannage des déploiements de DirectAccess dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 61040e19-5960-4eb0-b612-d710627988f7
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ec725eea286c359461b0f4a7b8763b97464e7067
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867090"
---
# <a name="troubleshooting-directaccess"></a>Dépannage de DirectAccess

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Suivez ces étapes pour résoudre les problèmes d’accès à distance (DirectAccess).  
  
|||  
|-|-|  
|**Problème**|**Résolution**|  
|Console de gestion des accès à distance est impossible d’afficher la configuration de DirectAccess|**Pour restaurer les informations de configuration manquantes**<br />-Si vous dépannez un déploiement multisite, assurez-vous que le contrôleur de domaine le plus proche pour le point d’entrée est disponible.<br />-Utilisez le **Get-DAEntrypointDC** applet de commande pour récupérer le nom du contrôleur de domaine le plus proche pour le point d’entrée. Si le contrôleur de domaine n’est pas en cours d’exécution, utilisez la **Set-DAEntryPointDC** applet de commande pour pointer vers un autre contrôleur de domaine.<br />-Exécutez **gpresult** à partir d’une invite de commandes avec élévation de privilèges sur le serveur pour vérifier le serveur reçoit les objets de stratégie de groupe DirectAccess.<br />-Activer la journalisation de l’interface (UI) d’utilisateur.<br />-Utilisez la commande suivante pour démarrer l’enregistrement de Windows PowerShell :<pre>logman create trace ETWTrace -ow -o c:\ETWTrace.etl -p {AAD4C46D-56DE-4F98-BDA2-B5EAEBDD2B04} 0xffffffffffffffff 0xff -nb 16 16 -bs 1024 -mode 0x2 -max 2048 -ets <br />logman update trace ETWTrace -p {62DFF3DA-7513-4FCA-BC73-25B111FBB1DB} 0xffffffffffffffff 0xff -ets</pre><repro>-Fermez et rouvrez l’interface utilisateur.<br />-Désactivez la journalisation de Windows Powershell. Collecter les fichiers journaux de suivi d’événements. En outre, de collecter tous les journaux à partir de la **%windir%/tracing** dossier.|  
|Échec de l’application de la configuration de DirectAccess|**Pour actualiser la configuration DirectAccess**<br />-Si vous dépannez un déploiement multisite, assurez-vous que le contrôleur de domaine le plus proche pour le point d’entrée est disponible.<br />-Utilisez le **Get-DAEntrypointDC** applet de commande pour récupérer le nom du contrôleur de domaine le plus proche pour le point d’entrée. Si le contrôleur de domaine n’est pas en cours d’exécution, utilisez la **Set-DAEntryPointDC** applet de commande pour pointer vers un autre contrôleur de domaine.<br />-Utilisez la commande suivante pour démarrer l’enregistrement de Windows Powershell :<br /><pre>logman create trace ETWTrace -ow -o c:\ETWTrace.etl -p {AAD4C46D-56DE-4F98-BDA2-B5EAEBDD2B04} 0xffffffffffffffff 0xff -nb 16 16 -bs 1024 -mode 0x2 -max 2048 -ets<br />logman update trace ETWTrace -p {62DFF3DA-7513-4FCA-BC73-25B111FBB1DB} 0xffffffffffffffff 0xff -ets</pre>    <repro><br />-Cliquez sur **appliquer**.<br />-Une fois la défaillance se produit, désactiver la journalisation de Windows Powershell et collecter le journal de suivi d’événements.|  
|DirectAccess est configuré, mais les clients ne sont pas en mesure de se connecter aux ressources internes|**Pour résoudre les problèmes de connexion client**<br />-Cliquez sur le **état des opérations** onglet dans la console de gestion de l’accès à distance et vous assurer que tous les composants affichent une icône verte. Si ce n’est pas le cas, vérifiez les détails de l’erreur et suivez les étapes de résolution.<br />-Exécutez l’accès à distance Server Best Practices Analyzer (BPA). S’il existe des avertissements ou erreurs, suivez les étapes de résolution pour résoudre le problème.|  
|Rencontrer des problèmes liés à une configuration multisite (par exemple, l’activation de des points d’entrée multisite, l’ajout ou définition du contrôleur de domaine pour un point d’entrée)|Suivez les étapes de [résoudre les problèmes d’un déploiement Multisite](https://technet.microsoft.com/library/jj554657(v=ws.11).aspx).|  
|Vignette d’état de configuration du tableau de bord affiche un avertissement ou une erreur|Suivez les étapes de [surveiller l’état de distribution de configuration du serveur d’accès à distance](https://technet.microsoft.com/library/jj574221(v=ws.11).aspx).|  
|Fait de rencontrer des problèmes liés à la configuration (par exemple, la configuration échoue lorsque vous activez l’équilibrage de charge ou il existe des problèmes lorsque vous ajoutez ou supprimez des serveurs à partir d’un cluster) d’équilibrage de charge|Si vous ont été l’activation de l’équilibrage de charge ou l’ajout d’un nœud, et la configuration actualisés chaque fois que vous avez cliqué sur **appliquer**, mais le cluster n’a pas correctement sur le serveur, exécutez la commande suivante : **cmd.exe /c « reg add HKLM\ SYSTEM\CurrentControlSet\Services\RaMgmtSvc\Parameters /f /v DebugFlag /t REG_DWORD /d « « 0xffffffff « » «** pour collecter l’utilisateur ouvre une interface sur le nouveau serveur.|  
|État des opérations affiche une erreur ou un avertissement, une fois la procédure suivante pour corriger la situation|Si l’état des opérations ne s’affichent des informations incorrectes (telles que les erreurs, même une fois que vous les corrigez) :<br /><br />-   Enable the registry key **cmd.exe /c "reg add HKLM\SYSTEM\CurrentControlSet\Services\RaMgmtSvc\Parameters /f /v EnableTracing /t REG_DWORD /d ""5"" "**.<br />-L’état des opérations d’actualisation et de collecter les journaux à partir de **%windir%/tracing**.|  
|Windows 8 et les ordinateurs clients DirectAccess ultérieure « No Internet » de rapport sous forme d’état pour la connexion de DirectAccess et indicateur de statut de connectivité réseau (NCSI) signale une connectivité limitée.|Cela peut se produire lorsque le tunneling forcé est activé dans la configuration de DirectAccess et, pour cette raison, seul IPHTTPS est utilisé. Pour résoudre ce problème, vous pouvez créer et configurer un serveur proxy. NCSI utilise ensuite le serveur proxy pour effectuer des vérifications de connectivité Internet. Il est recommandé que vous ajoutez un proxy statique à Table de stratégie de résolution de noms (NRPT) à l’aide de la procédure suivante.<br /><br />Avant d’exécuter les commandes dans cette procédure, assurez-vous que vous remplacez tous les noms de domaine, les noms d’ordinateurs et les autres variables de commande Windows PowerShell avec des valeurs qui sont appropriées pour votre déploiement.<br /><br />**Configurer un proxy statique pour une règle NRPT**<br />1.  Afficher le «. » Règle NRPT : `Get-DnsClientNrptRule -GpoName "corp.example.com\DirectAccess Client Settings" -Server <DomainControllerNetBIOSName>`<br />2.  Notez le nom (GUID) de la «. » Règle NRPT. Le nom (GUID) doit commencer par **DA-{.}**<br />3.  Définir le proxy pour le «. » Règle NRPT pour **proxy.corp.example.com:8080**:  `Set-DnsClientNrptRule -Name "DA-{..}" -Server <DomainControllerNetBIOSName> -GPOName "corp.example.com\DirectAccess Client Settings" -DAProxyServerName "proxy.corp.example.com:8080" -DAProxyType "UseProxyName"`<br />4.  Afficher le «. » Règle NRPT à nouveau en exécutant `Get-DnsClientNrptRule`et vérifiez que **ProxyFQDN:port** est maintenant configuré correctement.<br />5.  Actualiser la stratégie de groupe en exécutant `gpupdate /force` sur un client DirectAccess lorsque le client est connecté en interne, puis afficher l’à l’aide de la table NRPT `Get-DnsClientNrptPolicy` et vérifiez que le «. » montre la règle **ProxyFQDN:port**.|  
  


