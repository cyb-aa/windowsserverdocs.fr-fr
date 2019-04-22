---
title: Gérer les machines virtuelles Windows avec PowerShell Direct
description: Fournit des instructions pour l’utilisation de PowerShell Direct pour gérer des machines virtuelles sans se baser sur un réseau ou d’une connexion à distance avec eux.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5715c02-a90f-4de9-a71e-0fc09093ba2d
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 4081a9737825d2f50f0d3b19b3bada3b9bbc76f1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814700"
---
# <a name="manage-windows-virtual-machines-with-powershell-direct"></a>Gérer les machines virtuelles Windows avec PowerShell Direct

>S'applique à : Windows 10, Windows Server 2016, Windows Server 2019
  
Vous pouvez utiliser PowerShell Direct pour gérer à distance un Windows 10, Windows Server 2016 ou machine virtuelle de Windows Server 2019 à partir d’un ordinateur hôte Windows 10, Windows Server 2016 ou Windows Server 2019 Hyper-V. PowerShell Direct permet la gestion de Windows PowerShell à l’intérieur d’une machine, quel que soit la configuration du réseau ou les paramètres de gestion à distance sur l’hôte Hyper-V ou de la machine virtuelle. Pour les administrateurs Hyper-V, cela facilite la génération de scripts et l’automatisation de la gestion et de la configuration des machines virtuelles.  
  
Vous pouvez exécuter PowerShell Direct de deux manières :  
  
- Créer et quitter une session PowerShell Direct à l’aide des applets de commande PSSession
  
- Exécuter le script ou une commande avec l’applet de commande Invoke-Command
  
Si vous gérez des machines virtuelles plus anciennes, utilisez Connexion à une machine virtuelle (VMConnect) ou [configurez un réseau virtuel pour la machine virtuelle](https://technet.microsoft.com/library/cc816585.aspx).  
  
## <a name="create-and-exit-a-powershell-direct-session-using-pssession-cmdlets"></a>Créer et quitter une session PowerShell Direct à l’aide des applets de commande PSSession  
  
1. Sur l’hôte Hyper-V, ouvrez Windows PowerShell en tant qu’administrateur.  
  
2. Utilisez le [Enter-PSSession](https://technet.microsoft.com/library/hh849707.aspx) pour se connecter à la machine virtuelle. Exécutez une des commandes suivantes pour créer une session en utilisant le nom d’ordinateur virtuel ou le GUID :  
  
    ```  
    Enter-PSSession -VMName <VMName>  
    ```  
  
    ```  
    Enter-PSSession -VMGUID <VMGUID>  
    ```  
  
3. Tapez vos informations d’identification pour la machine virtuelle.   
4. Exécutez toutes les commandes nécessaires. Ces commandes s’exécutent sur la machine virtuelle à l’aide de laquelle vous avez créé la session.  
  
5.  Lorsque vous avez terminé, utilisez le [Exit-PSSession](https://technet.microsoft.com/library/hh849743.aspx) pour fermer la session.   
  
    ```  
    Exit-PSSession  
    ```  
  
## <a name="run-script-or-command-with-invoke-command-cmdlet"></a>Exécuter le script ou une commande avec l’applet de commande Invoke-Command  
Vous pouvez utiliser l’applet de commande [Invoke-Command](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Invoke-Command) pour exécuter un ensemble prédéfini de commandes sur la machine virtuelle. L’exemple suivant illustre l’utilisation de l’applet de commande Invoke-Command, où PSTest est le nom de la machine virtuelle et où le script à exécuter (foo.ps1) se trouve dans le dossier script sur le lecteur C:/ :  
  
```  
Invoke-Command -VMName PSTest  -FilePath C:\script\foo.ps1  
```  
  
Pour exécuter une commande unique, utilisez le paramètre **-ScriptBlock** :  
  
```  
Invoke-Command -VMName PSTest  -ScriptBlock { cmdlet }  
```  
  
## <a name="whats-required-to-use-powershell-direct"></a>Ce qui a obligé d’utiliser PowerShell Direct ?  
Pour créer une session PowerShell Direct sur une machine virtuelle :  
  
-   la machine virtuelle doit être démarrée et s’exécuter localement sur l’hôte ;  
  
-   vous devez être connecté à l’ordinateur hôte en tant qu’administrateur Hyper-V ;  
  
-   vous devez fournir des informations d’identification utilisateur valides pour la machine virtuelle.  
  
-   Le système d’exploitation hôte doit exécuter au moins Windows 10 ou Windows Server 2016.
  
-   La machine virtuelle doit exécuter au moins Windows 10 ou Windows Server 2016.  
  
Vous pouvez utiliser la [Get-VM](https://docs.microsoft.com/powershell/module/hyper-v/get-vm) applet de commande pour vérifier que les informations d’identification que vous utilisez ont le rôle d’administrateur Hyper-V et pour obtenir la liste des machines virtuelles démarrées et exécutées localement sur l’ordinateur hôte.  
  
## <a name="see-also"></a>Voir aussi  
[Enter-PSSession](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Enter-PSSession)  
[Exit-PSSession](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Exit-PSSession)  
[Invoke-Command](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Invoke-Command)  
  


