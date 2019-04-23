---
ms.assetid: aca4a4fa-b12c-4eed-a499-f9aedb7d2fd6
title: Configurer le système DNS d’entreprise pour le service de fédération et DRS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9b66bed99cbc2ac2cdf116579adaea282c45fabe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876390"
---
# <a name="configure-corporate-dns-for-the-federation-service-and-drs"></a>Configurer le système DNS d’entreprise pour le service de fédération et DRS

>S'applique à : Windows Server 2016, Windows Server 2012 R2
  
## <a name="step-6-add-a-host-a-and-alias-cname-resource-record-to-corporate-dns-for-the-federation-service-and-drs"></a>Étape 6 : Ajouter un hôte \(A\) et Alias \(CNAME\) enregistrement de ressource au DNS d’entreprise pour le Service de fédération et DRS  
Vous devez ajouter les enregistrements de ressource suivants au système de nom de domaine d’entreprise \(DNS\) pour votre service de fédération et le Service d’inscription de périphérique que vous avez configuré dans les étapes précédentes.  
  
|Entrée|Type|Address|  
|---------|--------|-----------|  
|fédération\_service\_nom|Hôte \(A\)|Adresse IP du serveur AD FS ou l’adresse IP de l’équilibreur de charge est configuré devant votre batterie de serveurs AD FS|  
|enterpriseregistration|Alias \(CNAME\)|federation\_server\_name.contoso.com|  
  
Vous pouvez utiliser la procédure suivante pour ajouter un hôte \(A\) et alias \(CNAME\) enregistrements de ressource au DNS d’entreprise pour le serveur de fédération et Device Registration Service.  
  
L’appartenance au **administrateurs**, ou équivalente, est la configuration minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>Pour ajouter un hôte \(A\) et alias \(CNAME\) des enregistrements de ressource DNS pour votre serveur de fédération  
  
1.  Sur vous contrôleur de domaine dans le Gestionnaire de serveur, sur le **outils** menu, cliquez sur **DNS** pour ouvrir le composant logiciel enfichable DNS\-dans.  
  
2.  Dans l’arborescence de la console, développez le **domaine\_contrôleur\_nom** nœud, développez **Zones de recherche directe**, à droite\-cliquez sur **domaine\_nom**, puis cliquez sur **nouvel hôte \(A ou AAAA\)**.  
  
3.  Dans le **nom** , tapez le nom à utiliser pour votre batterie de serveurs AD FS.  
  
4.  Dans la zone **adresse IP**, saisissez l’adresse IP de votre serveur de fédération. Cliquez sur **Ajouter un hôte**.  
  
5.  Droite\-cliquez sur le **domaine\_nom** nœud, puis cliquez sur **nouvel Alias \(CNAME\)**.  
  
6.  Dans la boîte de dialogue **Nouvel enregistrement de ressource**, tapez **enterpriseregistration** dans la zone **Nom de l'alias**.  
  
7.  Dans le nom de domaine pleinement qualifié \(nom de domaine complet\) de la zone d’hôte cible, tapez **fédération\_service\_batterie\_où\_name.com**, puis Cliquez sur **OK**.  
  
    > [!IMPORTANT]  
    > Dans un déploiement réel, si votre société comprend plusieurs nom d’utilisateur Principal \(UPN\) suffixes, vous devez créer plusieurs enregistrements CNAME pour chacun de ces suffixes UPN dans DNS.  
  
## <a name="see-also"></a>Voir aussi 

[Déploiement d’AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guide de déploiement de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Déploiement d’une batterie de serveurs de fédération](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

