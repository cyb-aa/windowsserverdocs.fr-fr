---
title: Gérer le contrôle d’accès basé sur les rôles avec Windows PowerShell
description: Cette rubrique fait partie du Guide de gestion des adresses IP (IPAM) de Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4f13f78e-0114-4e41-9a28-82a4feccecfc
ms.author: lizross
author: eross-msft
ms.openlocfilehash: a5cd347b849948052f4f7caa7fa8a863808e8c26
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309539"
---
# <a name="manage-role-based-access-control-with-windows-powershell"></a>Gérer le contrôle d’accès basé sur les rôles avec Windows PowerShell

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour apprendre à utiliser IPAM pour gérer le contrôle d’accès en fonction du rôle avec Windows PowerShell.  
  
>[!NOTE]
>Pour obtenir des informations de référence sur les commandes IPAM Windows PowerShell, consultez les [applets de commande IpamServer dans Windows PowerShell](https://docs.microsoft.com/powershell/module/ipamserver/?view=win10-ps).  
  
Les nouvelles commandes IPAM de Windows PowerShell vous permettent de récupérer et de modifier les étendues d’accès des objets DNS et DHCP. Le tableau suivant illustre la commande correcte à utiliser pour chaque objet IPAM.  
  
|Objet IPAM|Commande|Description|  
|---------------|-----------|---------------|  
|Serveur DNS|IpamDnsServer|Cette applet de commande retourne l’objet serveur DNS dans IPAM.|  
|Zone DNS|IpamDnsZone|Cette applet de commande retourne l’objet de zone DNS dans IPAM.|  
|Enregistrement de ressource DNS|IpamResourceRecord|Cette applet de commande retourne l’objet enregistrement de ressource DNS dans IPAM.|  
|Redirecteur conditionnel DNS|IpamDnsConditionalForwarder|Cette applet de commande retourne l’objet de redirecteur conditionnel DNS dans IPAM.|  
|Serveur DHCP|IpamDhcpServer|Cette applet de commande retourne l’objet serveur DHCP dans IPAM.|  
|Étendue globale DHCP|IpamDhcpSuperscope|Cette applet de commande retourne l’objet d’étendue globale DHCP dans IPAM.|  
|Étendue DHCP|IpamDhcpScope|Cette applet de commande retourne l’objet d’étendue DHCP dans IPAM.|  
  
Dans l’exemple suivant de sortie de commande, l’applet de commande `Get-IpamDnsZone` récupère la zone DNS **Dublin.contoso.com** .  
  
```  
PS C:\Users\Administrator.CONTOSO> Get-IpamDnsZone -ZoneType Forward -ZoneName dublin.contoso.com  
  
ZoneName             : dublin.contoso.com  
ZoneType             : Forward  
AccessScopePath      : \Global\Dublin  
IsSigned             : False  
DynamicUpdateStatus  : None  
ScavengeStaleRecords : False  
```  
  
## <a name="setting-access-scopes-on-ipam-objects"></a>Définition d’étendues d’accès sur des objets IPAM  
Vous pouvez définir des étendues d’accès sur des objets IPAM à l’aide de la commande `Set-IpamAccessScope`. Vous pouvez utiliser cette commande pour définir l’étendue d’accès à une valeur spécifique pour un objet, ou pour faire en sorte que les objets héritent de l’étendue d’accès des objets parents. Vous pouvez configurer les objets suivants à l’aide de cette commande.  
  
-   Étendue DHCP  
  
-   Serveur DHCP  
  
-   Étendue globale DHCP  
  
-   Redirecteur conditionnel DNS  
  
-   Enregistrements de ressources DNS  
  
-   Serveur DNS  
  
-   Zone DNS  
  
-   Bloc d’adresses IP  
  
-   Plage d’adresses IP  
  
-   Espace d’adressage IP  
  
-   Sous-réseau d’adresses IP  
  
Voici la syntaxe de la commande `Set-IpamAccessScope`.  
  
```  
NAME  
    Set-IpamAccessScope  
  
SYNTAX  
    Set-IpamAccessScope [-IpamRange] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDnsServer] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDhcpServer] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDhcpSuperscope] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDhcpScope] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDnsConditionalForwarder] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDnsResourceRecord] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDnsZone] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamAddressSpace] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamSubnet] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamBlock] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  [<CommonParameters>]  
```  
  
Dans l’exemple suivant, l’étendue d’accès de la zone DNS **Dublin.contoso.com** est modifiée de **Dublin** à **Europe**.  
  
```  
PS C:\Users\Administrator.CONTOSO> Get-IpamDnsZone -ZoneType Forward -ZoneName dublin.contoso.com  
  
ZoneName             : dublin.contoso.com  
ZoneType             : Forward  
AccessScopePath      : \Global\Dublin  
IsSigned             : False  
DynamicUpdateStatus  : None  
ScavengeStaleRecords : False  
  
PS C:\Users\Administrator.CONTOSO> $a = Get-IpamDnsZone -ZoneType Forward -ZoneName dublin.contoso.com  
PS C:\Users\Administrator.CONTOSO> Set-IpamAccessScope -IpamDnsZone -InputObject $a -AccessScopePath \Global\Europe -PassThru  
  
ZoneName             : dublin.contoso.com  
ZoneType             : Forward  
AccessScopePath      : \Global\Europe  
IsSigned             : False  
DynamicUpdateStatus  : None  
ScavengeStaleRecords : False  
```  
  


