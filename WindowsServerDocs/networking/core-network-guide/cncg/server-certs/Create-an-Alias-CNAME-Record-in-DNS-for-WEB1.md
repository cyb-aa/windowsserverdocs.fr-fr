---
title: Créer un enregistrement d’Alias (CNAME) dans DNS pour WEB1
description: Cette rubrique fait partie du guide de certificats de serveur de déploiement pour 802.1 X câblé et sans fil déploiements
manager: brianlic
ms.topic: article
ms.assetid: bfae23f0-ae12-486b-94fe-50a137e141a5
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 65f10efe1cfd2866fb99406bf031197f6268b05b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="create-an-alias-cname-record-in-dns-for-web1"></a>Créer un enregistrement d’Alias \(CNAME\) dans DNS pour WEB1

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette procédure pour ajouter un enregistrement de ressource Alias de nom canonique \(CNAME\) pour votre serveur Web à une zone DNS sur votre contrôleur de domaine. Avec des enregistrements CNAME, vous pouvez utiliser plusieurs noms pour pointer vers un seul ordinateur hôte, ce qui permet de faire des éléments tels que l’ordinateur hôte à la fois un serveur de protocole de transfert de fichiers \(FTP\) et un serveur Web sur le même ordinateur.   
  
Pour cette raison, vous êtes libre d’utiliser votre serveur Web pour héberger la révocation des certificats pour votre autorité de certification de liste \(CRL\) \(CA\) ainsi que pour effectuer des services supplémentaires, tels que serveur FTP ou Web.  
  
Lorsque vous effectuez cette procédure, remplacez **nom d’Alias** et d’autres variables avec les valeurs appropriées pour votre déploiement.  
  
Pour effectuer cette procédure, vous devez être membre du **Admins du domaine**.  
  
## <a name="to-add-an-alias-cname-resource-record-to-a-zone"></a>Pour ajouter un enregistrement de ressource alias \(CNAME\) à une zone  
  
>[!NOTE]  
>Pour effectuer cette procédure à l’aide de Windows PowerShell, voir [Add-DnsServerResourceRecordCName](https://technet.microsoft.com/library/jj649894(v=wps.630).aspx).  
  
1.  Sur DC1, dans le Gestionnaire de serveur, cliquez sur **outils** puis cliquez sur **DNS**. Le DNS Manager Console MMC (MicrosoftManagement) s’ouvre.  
  
2.  Dans l’arborescence de la console, double-cliquez sur **Zones de recherche directe**, avec le bouton droit de la zone de recherche directe dans laquelle vous souhaitez ajouter l’enregistrement de ressource Alias, puis cliquez sur **nouvel Alias \(CNAME\)**. Le **nouvel enregistrement de ressource** boîte de dialogue s’ouvre.  
  
3.  Dans **nom d’Alias**, tapez le nom d’alias **pki**.  
  
4.  Lorsque vous tapez une valeur pour **nom d’Alias**, le **\(FQDN\) de nom de domaine complet** remplit automatiquement dans la boîte de dialogue. Par exemple, si votre nom d’alias est «infrastructure à clé publique» et votre domaine est corp.contoso.com, la valeur **pki.corp.contoso.com** est renseignée automatiquement pour vous.  
  
5.  Dans **\(FQDN\) de nom de domaine complet pour l’ordinateur hôte cible**, tapez le nom de domaine complet de votre serveur Web. Par exemple, si votre serveur Web nommé WEB1 et votre domaine corp.contoso.com, tapez **WEB1.corp.contoso.com**.  
  
6.  Cliquez sur **OK** pour ajouter le nouvel enregistrement à la zone.  
  

