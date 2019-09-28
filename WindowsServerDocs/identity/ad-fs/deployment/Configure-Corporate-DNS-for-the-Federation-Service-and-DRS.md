---
ms.assetid: aca4a4fa-b12c-4eed-a499-f9aedb7d2fd6
title: Configurer le système DNS d’entreprise pour le service de fédération et DRS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9f0b04f9dc050117fdefc630759c86d2b1bb1ecc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408443"
---
# <a name="configure-corporate-dns-for-the-federation-service-and-drs"></a>Configurer le système DNS d’entreprise pour le service de fédération et DRS
  
## <a name="step-6-add-a-host-a-and-alias-cname-resource-record-to-corporate-dns-for-the-federation-service-and-drs"></a>Étape 6 : Ajouter un hôte \(A @ no__t-1 et un alias \(CNAME @ no__t-3 à un enregistrement de ressource DNS pour les service FS (Federation Service) et DRS  
Vous devez ajouter les enregistrements de ressource suivants au système de nom de domaine d’entreprise @no__t 0DNS @ no__t-1 pour votre service de Fédération et votre service d’inscription de périphérique que vous avez configurés dans les étapes précédentes.  
  
|Entrée|Type|Address|  
|---------|--------|-----------|  
|Fédération @ no__t-0Service @ no__t-1Nom|@No__t hôte-0A @ no__t-1|Adresse IP du serveur AD FS ou adresse IP de l’équilibreur de charge configuré devant votre batterie de serveurs AD FS|  
|enterpriseregistration|Alias \(CNAME @ no__t-1|Fédération @ no__t-0server\_name.contoso.com|  
  
Vous pouvez utiliser la procédure suivante pour ajouter des enregistrements de ressource Host \(A @ no__t-1 et alias \(CNAME @ no__t-3 à des enregistrements de ressources DNS d’entreprise pour le serveur de Fédération et le service d’inscription d’appareils.  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **administrateurs**ou à un groupe équivalent.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>Pour ajouter un ordinateur hôte \(A @ no__t-1 et alias \(CNAME @ no__t-3 enregistrements de ressources à DNS pour votre serveur de Fédération  
  
1.  Sur votre contrôleur de domaine, dans Gestionnaire de serveur, dans le menu **Outils** , cliquez sur **DNS** pour ouvrir le composant logiciel enfichable DNS @ no__t-2in.  
  
2.  Dans l’arborescence de la console, développez le nœud **Domain @ no__t-1controller @ no__t-2name** , développez **zones de recherche directes**, cliquez avec le bouton droit sur @ no__t-4click **Domain @ no__t-6Name**, puis cliquez sur **nouvel hôte \(A ou AAAA @ no__t-9**.  
  
3.  Dans la zone **nom** , tapez le nom à utiliser pour votre batterie de AD FS.  
  
4.  Dans la zone **adresse IP**, saisissez l’adresse IP de votre serveur de fédération. Cliquez sur **Ajouter un hôte**.  
  
5.  Right @ no__t-0click le nœud **Domain @ no__t-2name** , puis cliquez sur **New alias \(CNAME @ no__t-5**.  
  
6.  Dans la boîte de dialogue **Nouvel enregistrement de ressource**, tapez **enterpriseregistration** dans la zone **Nom de l'alias**.  
  
7.  Dans le nom de domaine complet \(FQDN @ no__t-1 de la zone hôte cible, tapez **Federation @ no__t-3Service @ no__t-4farm @ no__t-5name. Domain @ no__t-6name. com**, puis cliquez sur **OK**.  
  
    > [!IMPORTANT]  
    > Dans un déploiement réel, si votre entreprise possède plusieurs suffixes de nom d’utilisateur principal \(UPN @ no__t-1, vous devez créer plusieurs enregistrements CNAMe pour chacun de ces suffixes UPN dans DNS.  
  
## <a name="see-also"></a>Voir aussi 

[Déploiement d’AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guide de déploiement de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Déploiement d’une batterie de serveurs de fédération](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

