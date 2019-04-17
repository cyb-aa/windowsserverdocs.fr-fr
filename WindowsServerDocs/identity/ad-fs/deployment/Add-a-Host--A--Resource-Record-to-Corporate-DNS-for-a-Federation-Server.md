---
ms.assetid: 026747c7-4c34-41c7-b7ea-27f9a7f64a35
title: "Ajouter un enregistrement de ressource hôte (A) au DNS d’entreprise pour un serveur de fédération"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 425cfe794095f1515eb3fae2f1a5e5db90ba3d00
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>Ajouter un enregistrement de ressource hôte (A) au DNS d’entreprise pour un serveur de fédération

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012


Pour les clients sur l’entreprise réseau puisse accéder à un serveur de fédération à l’aide de l’authentification Windows intégrée, un enregistrement de ressource hôte \(A\) doit d’abord être créé dans le \(DNS\) système de nom de domaine d’entreprise qui résout le nom d’hôte du serveur de fédération compte \ (par exemple, fs.fabrikam.com\) à l’adresse IP du serveur de fédération ou du cluster de serveurs de fédération. Vous pouvez utiliser la procédure suivante pour ajouter un enregistrement de ressource hôte \(A\) au DNS d’entreprise pour un serveur de fédération.  
  
L’appartenance au groupe **administrateurs**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>Pour ajouter un enregistrement de ressource hôte \(A\) au DNS d’entreprise pour un serveur de fédération  
  
1.  Sur un serveur DNS pour le réseau d’entreprise, ouvrez le DNS de composants.  
  
2.  Dans la console de l’arborescence, la zone de recherche directe applicable, clic droit, puis cliquez sur **nouvel hôte \(A or AAAA\)**.  
  
3.  Dans **nom**, tapez uniquement le nom d’ordinateur du serveur de fédération ou du cluster de serveurs de fédération; par exemple, pour le nom de domaine complet nom \(FQDN\) fs.fabrikam.com, type **fs**.  
  
4.  Dans **adresse IP**, tapez l’adresse IP pour le serveur de fédération ou d’un cluster de serveurs de fédération, par exemple, 192.168.1.4.  
  
5.  Cliquez sur **ajouter l’hôte**.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification: Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Exigences de résolution de nom pour les serveurs de fédération](https://technet.microsoft.com/library/dd807055.aspx)  
  

