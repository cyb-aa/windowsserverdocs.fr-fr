---
ms.assetid: aca4a4fa-b12c-4eed-a499-f9aedb7d2fd6
title: "Configuration de DNS d’entreprise pour le Service de fédération et DRS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9b66bed99cbc2ac2cdf116579adaea282c45fabe
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="configure-corporate-dns-for-the-federation-service-and-drs"></a>Configuration de DNS d’entreprise pour le Service de fédération et DRS

>S’applique à: Windows Server2016, Windows Server2012R2
  
## <a name="step-6-add-a-host-a-and-alias-cname-resource-record-to-corporate-dns-for-the-federation-service-and-drs"></a>Étape6: Ajouter un ordinateur hôte d’enregistrement de ressource \(CNAME\) \(A\) et les Alias dans DNS d’entreprise pour le Service de fédération et DRS  
Vous devez ajouter les enregistrements de ressource suivants d’entreprise \(DNS\) système de nom de domaine pour votre service de fédération et Device Registration Service que vous avez configuré dans les étapes précédentes.  
  
|Entrée|Type|Adresse|  
|---------|--------|-----------|  
|federation\_service\_name|Héberger \(A\)|Adresse IP du serveur ADFS ou l’adresse IP de l’équilibrage de charge qui est configuré sur le devant de votre batterie de serveurs ADFS|  
|enterpriseregistration|Alias \(CNAME\)|federation\_server\_name.contoso.com|  
  
Vous pouvez utiliser la procédure suivante pour ajouter un hôte \(A\) et alias \(CNAME\) les enregistrements DNS d’entreprise pour le serveur de fédération et le Service d’inscription de périphérique.  
  
L’appartenance au groupe **administrateurs**, ou équivalent, est le minimum requis pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>Pour ajouter un hôte \(A\) et alias \(CNAME\) les enregistrements dans DNS pour votre serveur de fédération  
  
1.  Sur vous contrôleur de domaine dans le Gestionnaire de serveur, sur le **outils** menu, cliquez sur **DNS** pour ouvrir le DNS de composants.  
  
2.  Dans l’arborescence de la console, développez le **domain\_controller\_name** nœud, développez **Zones de recherche directe**, clic droit **domain\_name**, puis cliquez sur **nouvel hôte \(A or AAAA\)**.  
  
3.  Dans le **nom**, tapez le nom à utiliser pour votre batterie ADFS.  
  
4.  Dans le **adresse IP** , tapez l’adresse IP de votre serveur de fédération. Cliquez sur **ajouter l’hôte**.  
  
5.  Clic droit le **domain\_name** nœud, puis cliquez sur **nouvel Alias \(CNAME\)**.  
  
6.  Dans le **nouvel enregistrement de ressource** boîte de dialogue, tapez **enterpriseregistration** dans les **nom d’Alias** zone.  
  
7.  Dans le nom de domaine complet nom \(FQDN\) de la zone d’ordinateur hôte cible, tapez **federation\_service\_farm\_name.domain\_name.com**, puis cliquez sur **OK**.  
  
    > [!IMPORTANT]  
    > Dans un déploiement réel, si votre société possède plusieurs suffixes de nom d’utilisateur Principal \(UPN\), vous devez créer plusieurs enregistrements CNAME pour chaque suffixe UPN dans DNS.  
  
## <a name="see-also"></a>Voir aussi 

[Déploiement d’ADFS](../../ad-fs/AD-FS-Deployment.md)  

[Guide de déploiement de Windows Server2012R2 ADFS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Déploiement d’une batterie de serveurs de fédération](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

