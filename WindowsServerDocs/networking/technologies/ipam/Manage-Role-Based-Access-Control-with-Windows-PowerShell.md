---
title: Gérer basé sur les rôles contrôle d’accès avec Windows PowerShell
description: Cette rubrique fait partie du guide de gestion de la gestion des adresses IP (IPAM) dans Windows Server2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4f13f78e-0114-4e41-9a28-82a4feccecfc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: df6fa423a4ec891f1ad3faefad6c6054519542c4
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="manage-role-based-access-control-with-windows-powershell"></a>Gérer basé sur les rôles contrôle d’accès avec Windows PowerShell

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour savoir comment utiliser IPAM pour gérer le contrôle d’accès basé sur les rôles avec Windows PowerShell.  
  
>[!NOTE]
>Pour la référence de commande IPAM Windows PowerShell, voir [applets de commande de serveur de gestion des adresses IP (IPAM) dans Windows PowerShell](https://technet.microsoft.com/library/jj553807.aspx).  
  
Les nouvelles commandes Windows PowerShell IPAM vous donnent la possibilité de récupérer et modifier les étendues d’accès des objets DNS et DHCP. Le tableau suivant illustre la commande appropriée à utiliser pour chaque objet IPAM.  
  
|Objet IPAM|Commande|Description|  
|---------------|-----------|---------------|  
|Serveur DNS|Get-IpamDnsServer|Cette applet de commande retourne l’objet serveur DNS dans IPAM|  
|Zone DNS|Get-IpamDnsZone|Cette applet de commande retourne l’objet de zone DNS dans IPAM|  
|Enregistrement de ressource DNS|Get-IpamResourceRecord|Cette applet de commande retourne l’objet d’enregistrement de ressource DNS dans IPAM|  
|Redirecteur conditionnel DNS|Get-IpamDnsConditionalForwarder|Cette applet de commande retourne l’objet de redirecteur conditionnel DNS dans IPAM|  
|Serveur DHCP|Get-IpamDhcpServer|Cette applet de commande retourne l’objet serveur DHCP dans IPAM|  
|Étendue globale DHCP|Get-IpamDhcpSuperscope|Cette applet de commande retourne l’objet d’étendue globale DHCP dans IPAM|  
|Étendue DHCP|Get-IpamDhcpScope|Cette applet de commande retourne l’objet d’étendue DHCP dans IPAM|  
  
Dans l’exemple suivant de sortie de la commande, le `Get-IpamDnsZone` applet de commande extrait le **dublin.contoso.com** zone DNS.  
  
```  
PS C:\Users\Administrator.CONTOSO> Get-IpamDnsZone -ZoneType Forward -ZoneName dublin.contoso.com  
  
ZoneName             : dublin.contoso.com  
ZoneType             : Forward  
AccessScopePath      : \Global\Dublin  
IsSigned             : False  
DynamicUpdateStatus  : None  
ScavengeStaleRecords : False  
```  
  
## <a name="setting-access-scopes-on-ipam-objects"></a>Définition des étendues d’accès sur des objets IPAM  
Vous pouvez définir des étendues d’accès sur des objets IPAM à l’aide de la `Set-IpamAccessScope` commande. Vous pouvez utiliser cette commande pour définir l’étendue d’accès à une valeur spécifique pour un objet ou les objets à hériter l’étendue d’accès d’objets parents. Voici les objets que vous pouvez configurer avec cette commande.  
  
-   Étendue DHCP  
  
-   Serveur DHCP  
  
-   Étendue globale DHCP  
  
-   Redirecteur conditionnel DNS  
  
-   Enregistrements de ressource DNS  
  
-   Serveur DNS  
  
-   Zone DNS  
  
-   Bloc d’adresses IP  
  
-   Plage d’adresses IP  
  
-   Espace d’adressage IP  
  
-   Sous-réseau d’adresse IP  
  
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
  
Dans l’exemple suivant, l’étendue d’accès de la zone DNS **dublin.contoso.com** est changé de **Dublin** à **Europe**.  
  
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
  


