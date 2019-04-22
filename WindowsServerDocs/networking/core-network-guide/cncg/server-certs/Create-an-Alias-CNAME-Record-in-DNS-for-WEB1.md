---
title: Créer un enregistrement d’alias (CNAME) dans DNS pour WEB1
description: Cette rubrique fait partie du guide des certificats de serveur de déploiement pour les déploiements de sans fil et câblé à 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: bfae23f0-ae12-486b-94fe-50a137e141a5
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 442ee8b70311eaad3f0b3f263003786b6beab8bc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813160"
---
# <a name="create-an-alias-cname-record-in-dns-for-web1"></a>Créer un Alias \(CNAME\) enregistrements dans DNS pour WEB1

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour ajouter un nom canonique d’Alias \(CNAME\) enregistrement de ressource pour votre serveur Web pour une zone DNS sur votre contrôleur de domaine. Avec les enregistrements CNAME, vous pouvez utiliser plusieurs noms pour pointer vers un seul hôte, ce qui facilite certaines tâches telles que hôte à la fois un protocole de transfert de fichiers \(FTP\) serveur et un serveur Web sur le même ordinateur.   
  
Pour cette raison, vous êtes libre d’utiliser votre serveur Web pour héberger la liste de révocation de certificat \(CRL\) pour votre autorité de certification \(autorité de certification\) ainsi que pour effectuer des services supplémentaires, tels que serveur FTP ou Web.  
  
Lorsque vous effectuez cette procédure, remplacez **nom d’Alias** et d’autres variables avec des valeurs qui sont appropriées pour votre déploiement.  
  
Pour effectuer cette procédure, vous devez être membre du **Admins du domaine**.  
  
## <a name="to-add-an-alias-cname-resource-record-to-a-zone"></a>Pour ajouter un alias \(CNAME\) enregistrement de ressource à une zone  
  
>[!NOTE]  
>Pour effectuer cette procédure à l’aide de Windows PowerShell, consultez [Add-DnsServerResourceRecordCName](https://technet.microsoft.com/library/jj649894(v=wps.630).aspx).  
  
1.  Sur DC1, dans le Gestionnaire de serveur, cliquez sur **outils** puis cliquez sur **DNS**. Le DNS Manager Console MMC (Microsoft Management) s’ouvre.  
  
2.  Dans l’arborescence de la console, double-cliquez sur **Zones de recherche directe**, avec le bouton droit de la zone de recherche directe dans laquelle vous souhaitez ajouter l’enregistrement de ressource Alias, puis cliquez sur **nouvel Alias \(CNAME\)** . Le **nouvel enregistrement de ressource** boîte de dialogue s’ouvre.  
  
3.  Dans **nom d’Alias**, tapez le nom d’alias **pki**.  
  
4.  Lorsque vous tapez une valeur pour **nom d’Alias**, le **nom de domaine complet \(FQDN\)**  remplit automatiquement dans la boîte de dialogue. Par exemple, si votre nom d’alias est « infrastructure à clé publique » et votre domaine est corp.contoso.com, la valeur **pki.corp.contoso.com** est renseignée automatiquement pour vous.  
  
5.  Dans **nom de domaine complet \(FQDN\) pour l’ordinateur hôte cible**, tapez le nom de domaine complet de votre serveur Web. Par exemple, si votre serveur Web est nommé WEB1 et votre domaine est corp.contoso.com, tapez **WEB1.corp.contoso.com**.  
  
6.  Cliquez sur **OK** pour ajouter le nouvel enregistrement à la zone.  
  

