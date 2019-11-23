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
  
## <a name="step-6-add-a-host-a-and-alias-cname-resource-record-to-corporate-dns-for-the-federation-service-and-drs"></a>Étape 6 : ajouter un ordinateur hôte \(un\) et un alias \(enregistrement de ressource CNAMe\) à DNS d’entreprise pour le service FS (Federation Service) et DRS  
Vous devez ajouter les enregistrements de ressources suivants au système de noms de domaine d’entreprise \(\) DNS pour votre service de Fédération et votre service d’inscription d’appareil que vous avez configurés dans les étapes précédentes.  
  
|Entrée|Type|Address|  
|---------|--------|-----------|  
|nom de\_du service de\_de Fédération|Hôte \(un\)|Adresse IP du serveur AD FS ou adresse IP de l’équilibreur de charge configuré devant votre batterie de serveurs AD FS|  
|enterpriseregistration|Alias \(CNAMe\)|serveur de\_de Fédération\_name.contoso.com|  
  
Vous pouvez utiliser la procédure suivante pour ajouter un ordinateur hôte \(un\) et un alias \(CNAMe\) des enregistrements de ressources au DNS d’entreprise pour le serveur de Fédération et le service d’inscription d’appareils.  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **administrateurs**ou à un groupe équivalent.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>Pour ajouter un ordinateur hôte \(un\) et un alias \(enregistrement de ressource CNAMe\) à DNS pour votre serveur de Fédération  
  
1.  Sur votre contrôleur de domaine, dans Gestionnaire de serveur, dans le menu **Outils** , cliquez sur **DNS** pour ouvrir le\-du composant DNS Snap dans.  
  
2.  Dans l’arborescence de la console, développez le nœud **domain\_controller\_Name** , développez **zones de recherche directes**, cliquez avec\-le bouton droit sur le **nom\_de domaine**, puis cliquez sur **nouvel hôte \(A ou AAAA\)** .  
  
3.  Dans la zone **nom** , tapez le nom à utiliser pour votre batterie de AD FS.  
  
4.  Dans la zone **adresse IP**, saisissez l’adresse IP de votre serveur de fédération. Cliquez sur **Ajouter un hôte**.  
  
5.  Cliquez avec le bouton droit\-sur le nœud **nom du\_de domaine** , puis cliquez sur **Nouvel alias \(CNAME\)** .  
  
6.  Dans la boîte de dialogue **Nouvel enregistrement de ressource**, tapez **enterpriseregistration** dans la zone **Nom de l'alias**.  
  
7.  Dans la zone nom de domaine complet \(\) de nom de domaine complet de la zone hôte cible, tapez **federation\_service\_\_nom de la batterie de serveurs. domaine\_Name.com**, puis cliquez sur **OK**.  
  
    > [!IMPORTANT]  
    > Dans un déploiement réel, si votre entreprise possède plusieurs suffixes de nom d’utilisateur principal \(\) suffixes UPN, vous devez créer plusieurs enregistrements CNAMe pour chacun de ces suffixes UPN dans DNS.  
  
## <a name="see-also"></a>Voir aussi 

[Déploiement d’AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guide de déploiement de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Déploiement d’une batterie de serveurs de fédération](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

