---
title: Créer un enregistrement d’alias (CNAME) dans DNS pour WEB1
description: Cette rubrique fait partie du guide déployer des certificats de serveur pour les déploiements sans fil et câblés 802.1 X.
manager: brianlic
ms.topic: article
ms.assetid: bfae23f0-ae12-486b-94fe-50a137e141a5
ms.prod: windows-server
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 77b8e464d2d8fab8803477e59826c3e715c0a6d2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406297"
---
# <a name="create-an-alias-cname-record-in-dns-for-web1"></a>Créer un alias \(CNAMe\) enregistrement dans DNS pour WEB1

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour ajouter un nom canonique d’alias \(CNAMe\) enregistrement de ressource de votre serveur Web à une zone dans DNS sur votre contrôleur de domaine. Avec les enregistrements CNAMe, vous pouvez utiliser plusieurs noms pour pointer vers un seul hôte, ce qui simplifie l’hébergement d’un protocole FTP \(serveur FTP\) et d’un serveur Web sur le même ordinateur.   
  
Pour cette raison, vous êtes libre d’utiliser votre serveur Web pour héberger la liste de révocation de certificats \(\) de liste de révocation de certificats pour votre autorité de certification \(\) de l’autorité de certification, ainsi que pour effectuer des services supplémentaires, tels que FTP ou un serveur Web.  
  
Lorsque vous effectuez cette procédure, remplacez le **nom d’alias** et d’autres variables par des valeurs appropriées pour votre déploiement.  
  
Pour effectuer cette procédure, vous devez être membre du **groupe Admins du domaine**.  
  
## <a name="to-add-an-alias-cname-resource-record-to-a-zone"></a>Pour ajouter un alias \(CNAMe\) enregistrement de ressource à une zone  
  
>[!NOTE]  
>Pour effectuer cette procédure à l’aide de Windows PowerShell, consultez [Add-DnsServerResourceRecordCName](https://technet.microsoft.com/library/jj649894(v=wps.630).aspx).  
  
1.  Sur DC1, dans Gestionnaire de serveur, cliquez sur **Outils** , puis sur **DNS**. La console MMC (Microsoft Management Console) du Gestionnaire DNS s’ouvre.  
  
2.  Dans l’arborescence de la console, double-cliquez sur **zones de recherche directes**, cliquez avec le bouton droit sur la zone de recherche directe dans laquelle vous souhaitez ajouter l’enregistrement de ressource alias, puis cliquez sur **Nouvel alias \(CNAME\)** . La boîte **de dialogue nouvel enregistrement de ressource** s’ouvre.  
  
3.  Dans **nom**de l’alias, tapez le nom d’alias **PKI**.  
  
4.  Lorsque vous tapez une valeur pour **nom d’alias**, le **nom de domaine complet \(FQDN\)** des remplissages automatiques dans la boîte de dialogue. Par exemple, si votre nom d’alias est « PKI » et que votre domaine est corp.contoso.com, la valeur **PKI.Corp.contoso.com** est remplie automatiquement pour vous.  
  
5.  Dans **nom de domaine complet \(\) de noms de domaine complet pour l’ordinateur hôte cible**, tapez le nom de domaine complet de votre serveur Web. Par exemple, si votre serveur Web est nommé WEB1 et que votre domaine est corp.contoso.com, tapez **web1.Corp.contoso.com**.  
  
6.  Cliquez sur **OK** pour ajouter le nouvel enregistrement à la zone.  
  

