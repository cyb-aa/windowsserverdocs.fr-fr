---
title: Gérer le contrôle d’accès basé sur les rôles avec Windows PowerShell
description: Cette rubrique fait partie du guide de gestion de la gestion des adresses IP (IPAM) dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4f13f78e-0114-4e41-9a28-82a4feccecfc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 11dd417be4720b09851fc03f111edaaf06b55e59
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282131"
---
# <a name="manage-role-based-access-control-with-windows-powershell"></a>Gérer le contrôle d’accès basé sur les rôles avec Windows PowerShell

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour apprendre à utiliser IPAM pour gérer le contrôle d’accès en fonction du rôle avec Windows PowerShell.  
  
>[!NOTE]
>Pour la référence de commande IPAM Windows PowerShell, consultez le [IpamServer des applets de commande dans Windows PowerShell](https://docs.microsoft.com/powershell/module/ipamserver/?view=win10-ps).  
  
Les nouvelles commandes Windows PowerShell IPAM vous offrent la possibilité de récupérer et modifier les étendues d’accès d’objets DNS et DHCP. Le tableau suivant illustre la commande correcte à utiliser pour chaque objet IPAM.  
  
|Objet d’IPAM|Command|Description|  
|---------------|-----------|---------------|  
|Serveur DNS|Get-IpamDnsServer|Cette applet de commande retourne l’objet de serveur DNS dans IPAM|  
|Zone DNS|Get-IpamDnsZone|Cette applet de commande retourne l’objet de zone DNS dans IPAM|  
|Enregistrement de ressource DNS|Get-IpamResourceRecord|Cette applet de commande retourne l’objet d’enregistrement de ressource DNS dans IPAM|  
|Redirecteur DNS conditionnel|Get-IpamDnsConditionalForwarder|Cette applet de commande retourne l’objet de redirecteur conditionnel de DNS dans IPAM|  
|Serveur DHCP|Get-IpamDhcpServer|Cette applet de commande retourne l’objet de serveur DHCP dans IPAM|  
|Étendue globale DHCP|Get-IpamDhcpSuperscope|Cette applet de commande retourne l’objet d’étendue globale DHCP dans IPAM|  
|Étendue DHCP|Get-IpamDhcpScope|Cette applet de commande retourne l’objet d’étendue DHCP dans IPAM|  
  
Dans l’exemple suivant de sortie de commande, le `Get-IpamDnsZone` applet de commande récupère le **dublin.contoso.com** zone DNS.  
  
```  
PS C:\Users\Administrator.CONTOSO> Get-IpamDnsZone -ZoneType Forward -ZoneName dublin.contoso.com  
  
ZoneName             : dublin.contoso.com  
ZoneType             : Forward  
AccessScopePath      : \Global\Dublin  
IsSigned             : False  
DynamicUpdateStatus  : None  
ScavengeStaleRecords : False  
```  
  
## <a name="setting-access-scopes-on-ipam-objects"></a>Définir des étendues d’accès sur les objets IPAM  
Vous pouvez définir des étendues d’accès sur des objets IPAM à l’aide de la `Set-IpamAccessScope` commande. Vous pouvez utiliser cette commande pour définir l’étendue d’accès à une valeur spécifique pour un objet ou les objets à hériter l’étendue d’accès d’objets parents. Voici les objets que vous pouvez configurer avec cette commande.  
  
-   Étendue DHCP  
  
-   Serveur DHCP  
  
-   Étendue globale DHCP  
  
-   Redirecteur DNS conditionnel  
  
-   Enregistrements de ressource DNS  
  
-   Serveur DNS  
  
-   Zone DNS  
  
-   Bloc d’adresses IP  
  
-   Plage d’adresses IP  
  
-   Espace d’adressage IP  
  
-   Sous-réseau d’adresses IP  
  
Voici la syntaxe pour le `Set-IpamAccessScope` commande.  
  
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
  
Dans l’exemple suivant, l’étendue d’accès de la zone DNS **dublin.contoso.com** est remplacée par **Dublin** à **Europe**.  
  
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
  


