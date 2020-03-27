---
title: Résolution de problèmes généraux
description: Cette rubrique fait partie du guide déployer plusieurs serveurs d’accès à distance dans un déploiement multisite dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 354ae5e3-bae1-44f9-afd7-7eaba70f2346
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 513bcae13d4a8f3ab935d2bda77745baa1788fa9
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313793"
---
# <a name="troubleshooting-general-issues"></a>Résolution de problèmes généraux

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique contient des informations de dépannage pour les problèmes généraux liés à l’accès à distance.  
  
## <a name="gpo-retrieval-error"></a>Erreur de récupération de l’objet de stratégie de groupe  
**Erreur reçue**. Impossible de récupérer les paramètres d’objet de stratégie de groupe du serveur DirectAccess. Vérifiez que vous disposez des autorisations de modification pour l’objet de stratégie de groupe.  
  
La console Gestion de l’accès à distance ne répond pas après la réception de cette erreur.  
  
**Cause**  
  
DirectAccess ne peut pas accéder à l’objet de stratégie de groupe de l’un des points d’entrée du déploiement et, par conséquent, le chargement de la configuration échoue.  
  
**Solution**  
  
Assurez-vous que chaque point d’entrée du déploiement a un objet de stratégie de groupe correspondant sur son contrôleur de domaine et vérifiez que l’utilisateur connecté dispose des autorisations en lecture et en écriture pour tous les objets de stratégie de groupe configurés dans le déploiement de l’accès à distance.  
  
Pour contourner ce problème, utilisez les applets de commande de configuration au lieu d’utiliser la console de gestion de l’accès à distance. par exemple, à l’aide de `Get-RemoteAccess` et `Get-DAEntryPoint`.  
  
> [!NOTE]  
> Ce scénario ne se produit pas lorsque l’objet de stratégie de groupe de serveur du point d’entrée actuel n’est pas disponible.  
  
Vous pouvez utiliser l’applet de commande `Get-DAEntryPointDC` pour répertorier tous les contrôleurs de domaine qui stockent des objets de stratégie de groupe de serveur et `Get-DAMultiSite` conjointement avec `Get-RemoteAccess` pour récupérer une liste complète des objets de stratégie de groupe de serveur dans le déploiement. Par exemple :  
  
```  
$ServerGpos = Get-DAEntryPointDC | ForEach-Object {   
   @{   
       GpoName = (Get-RemoteAccess -EntryPoint $_.EntryPointName).ServerGpoName;   
        DC = $_.DomainControllerName }   
}  
$ServerGpos | ForEach-Object { $GpoName = $_['GpoName'] ; $DC = $_['DC'] ; Write-Host "Server GPO '$GpoName' on DC '$DC'" }  
```  
  
## <a name="windows-7-to-windows-8-or-10-client-upgrade"></a>Mise à niveau de Windows 7 vers Windows 8 ou 10 client  
**Symptôme**. Après la mise à niveau d’un client Windows 7 vers Windows 10 ou Windows 8 dans un déploiement multisite, la connexion DirectAccess n’est pas visible dans la liste réseaux.  
  
**Cause**  
  
Les objets de stratégie de groupe Windows 7 dans un déploiement multisite ne contiennent pas la configuration de l’Assistant connectivité réseau Windows 8.  
  
 Les clients Windows 7 doivent utiliser l’Assistant de connectivité DirectAccess pour surveiller leur état de connectivité DirectAccess, ce qui nécessite une configuration manuelle distincte dans les objets de stratégie de groupe du client Windows 7. Lorsque les clients Windows 7 sont mis à niveau vers Windows 10 ou Windows 8, l’Assistant connectivité réseau ne fonctionnera pas si l’objet de stratégie de groupe du client Windows 7 est toujours appliqué.  
  
**Solution**  
  
Si les paramètres de l’Assistant de connectivité DirectAccess sont configurés dans les objets de stratégie de groupe Windows 7, vous pouvez résoudre ce problème avant de mettre à niveau les ordinateurs clients en modifiant les objets de stratégie de groupe Windows 7 à l’aide des applets de commande PowerShell suivantes :  
  
```  
Set-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant" -ValueName "TemporaryValue" -Type Dword -Value 1  
Remove-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant"  
```  
  
Si un client a déjà été mis à niveau ou si le DCA n’est pas configuré, déplacez l’ordinateur client vers le groupe de sécurité Windows 10 ou Windows 8.  
  
## <a name="general-cmdlet-errors"></a>Erreurs d’applet de commande générales  
  
-   **Problème 1**  
  
    **Erreur reçue**. Impossible d’accéder au contrôleur de domaine < domain_controller > pour < SERVER_NAME ou entry_point_name >.  
  
    **Cause**  
  
    Pour assurer la cohérence de la configuration dans un déploiement multisite, il est important de veiller à ce que chaque objet de stratégie de groupe soit géré par un seul contrôleur de domaine. Lorsque le contrôleur de domaine qui gère l’objet de stratégie de groupe du serveur d’un point d’entrée n’est pas disponible, les paramètres de configuration de l’accès à distance ne peuvent pas être lus ou modifiés.  
  
    **Solution**  
  
    Suivez la procédure « pour modifier le contrôleur de domaine qui gère les objets de stratégie de groupe de serveur » décrit dans [2,4. Configurer des objets de stratégie de groupe](assetId:///b1960686-a81e-4f48-83f1-cc4ea484df43#ConfigGPOs).  
  
-   **Problème 2**  
  
    **Erreur reçue**. Impossible d’accéder au contrôleur de domaine principal dans le domaine < domain_name >.  
  
    **Cause**  
  
    Pour assurer la cohérence de la configuration dans un déploiement multisite, il est important de veiller à ce que chaque objet de stratégie de groupe soit géré par un seul contrôleur de domaine. Les objets de stratégie de groupes clients sont gérés sur le contrôleur de domaine principal. Si le contrôleur de domaine principal n’est pas disponible, il est impossible de lire ou de modifier les paramètres de configuration de l’accès à distance.  
  
    **Solution**  
  
    Suivez la procédure « pour transférer le rôle d’émulateur de contrôleur de domaine principal » décrite dans [2,4. Configurer des objets de stratégie de groupe](assetId:///b1960686-a81e-4f48-83f1-cc4ea484df43#ConfigGPOs).  
  


