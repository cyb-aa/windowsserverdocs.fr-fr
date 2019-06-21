---
title: Résolution de problèmes généraux
description: Cette rubrique fait partie du guide de déploiement de plusieurs serveurs d’accès distant dans un déploiement Multisite dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 354ae5e3-bae1-44f9-afd7-7eaba70f2346
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 87614ac3b83eaacefb4ac5f9fddef238ed500953
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282555"
---
# <a name="troubleshooting-general-issues"></a>Résolution de problèmes généraux

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique contient des informations de résolution des problèmes d’ordre général liés à l’accès à distance.  
  
## <a name="gpo-retrieval-error"></a>Erreur de récupération de stratégie de groupe  
**Erreur reçue**. Paramètres de stratégie de groupe du serveur DirectAccess ne peut pas être récupérées. Assurez-vous de qu'avoir des autorisations de modification pour l’objet de stratégie de groupe.  
  
La console de gestion de l’accès à distance ne répond pas après avoir reçu cette erreur.  
  
**Cause**  
  
DirectAccess ne peut pas accéder à l’objet de stratégie de groupe d’un des points d’entrée dans le déploiement et ainsi la configuration de chargement échoue.  
  
**Solution**  
  
Assurez-vous que chaque point d’entrée dans le déploiement dispose d’un objet de stratégie de groupe correspondant sur son contrôleur de domaine et vérifiez que l’utilisateur connecté a autorisations lecture et écriture pour tous les objets de stratégie de groupe configurés dans le déploiement de l’accès à distance.  
  
Pour résoudre ce problème, utilisez les applets de commande de configuration au lieu d’utiliser la console de gestion de l’accès à distance ; par exemple, à l’aide de `Get-RemoteAccess` et `Get-DAEntryPoint`.  
  
> [!NOTE]  
> Ce scénario ne se produit pas lorsque le serveur de stratégie de groupe du point d’entrée actuel n’est pas disponible.  
  
Vous pouvez utiliser la `Get-DAEntryPointDC` applet de commande pour répertorier tous les contrôleurs de domaine qui stockent les GPO de serveur et `Get-DAMultiSite` conjointement avec `Get-RemoteAccess` pour récupérer une liste complète du serveur de stratégie de groupe dans le déploiement. Exemple :  
  
```  
$ServerGpos = Get-DAEntryPointDC | ForEach-Object {   
   @{   
       GpoName = (Get-RemoteAccess -EntryPoint $_.EntryPointName).ServerGpoName;   
        DC = $_.DomainControllerName }   
}  
$ServerGpos | ForEach-Object { $GpoName = $_['GpoName'] ; $DC = $_['DC'] ; Write-Host "Server GPO '$GpoName' on DC '$DC'" }  
```  
  
## <a name="windows-7-to-windows-8-or-10-client-upgrade"></a>7 de Windows pour Windows 8 ou 10 mise à niveau du client  
**Symptôme**. Une fois un client Windows 7 met à niveau vers Windows 10 ou Windows 8 dans un déploiement multisite, la connexion DirectAccess n’est pas visible dans la liste des réseaux.  
  
**Cause**  
  
Les objets de stratégie de groupe Windows 7 dans un déploiement multisite ne contiennent pas de la configuration d’Assistant de connectivité de réseau de Windows 8.  
  
 Les clients Windows 7 doivent utiliser l’Assistant connectivité DirectAccess pour surveiller leur état de connectivité DirectAccess qui nécessite une configuration manuelle distincte dans la stratégie de groupe du client Windows 7. Lorsque les clients Windows 7 sont mis à niveau vers Windows 10 ou Windows 8, l’Assistant connectivité réseau ne fonctionnera pas si Windows 7 client est toujours appliqué.  
  
**Solution**  
  
Si les paramètres de l’Assistant de connectivité DirectAccess sont configurés dans les objets de stratégie de groupe Windows 7, vous pouvez résoudre ce problème avant la mise à niveau des ordinateurs clients en modifiant la stratégie de groupe Windows 7 à l’aide des applets de commande PowerShell suivante :  
  
```  
Set-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant" -ValueName "TemporaryValue" -Type Dword -Value 1  
Remove-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant"  
```  
  
Si un client a déjà été mis à niveau ou l’Assistant DCA n’est pas configuré, déplacer l’ordinateur client au groupe de sécurité Windows 10 ou Windows 8.  
  
## <a name="general-cmdlet-errors"></a>Erreurs de l’applet de commande générales  
  
-   **Problème 1**  
  
    **Erreur reçue**. Impossible d’atteindre le contrôleur de domaine < contrôleur_de_domaine > pour < nom_serveur ou nom_du_point_entrée >.  
  
    **Cause**  
  
    Pour assurer la cohérence de la configuration dans un déploiement multisite, il est important de veiller à ce que chaque objet de stratégie de groupe soit géré par un seul contrôleur de domaine. Lorsque le contrôleur de domaine qui gère les GPO de serveur d’un point d’entrée n’est pas disponible, les paramètres de configuration de l’accès à distance ne peut pas lire ou de modifier.  
  
    **Solution**  
  
    Suivez la procédure « pour modifier le contrôleur de domaine qui gère la stratégie de groupe serveur » décrite dans [2.4. Configurer la stratégie de groupe](assetId:///b1960686-a81e-4f48-83f1-cc4ea484df43#ConfigGPOs).  
  
-   **Problème 2**  
  
    **Erreur reçue**. Le contrôleur de domaine principal dans le domaine < nom_domaine > n’est pas accessible.  
  
    **Cause**  
  
    Pour assurer la cohérence de la configuration dans un déploiement multisite, il est important de veiller à ce que chaque objet de stratégie de groupe soit géré par un seul contrôleur de domaine. Les objets de stratégie de groupes clients sont gérés sur le contrôleur de domaine principal. Si le contrôleur de domaine principal n’est pas disponible, il est impossible de lire ou de modifier les paramètres de configuration de l’accès à distance.  
  
    **Solution**  
  
    Suivez la procédure « pour transférer le rôle d’émulateur PDC » décrite dans [2.4. Configurer la stratégie de groupe](assetId:///b1960686-a81e-4f48-83f1-cc4ea484df43#ConfigGPOs).  
  


