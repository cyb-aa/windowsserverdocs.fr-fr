---
ms.assetid: 30657638-5709-48c5-87aa-98f688e07b4c
title: "Liste de vérification: implémentation d’une conception SSO de Web"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 265daf3acb9632aa92f85962abc44a6a9ea8dfed
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="checklist-implementing-a-web-sso-design"></a>Liste de vérification: Implémentation d’une conception SSO de Web

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette liste de vérification principale inclut une nouveauté-référence des liens vers des concepts importants relatifs à la conception de connexion de Single\ sur \(SSO\) pour \(ADFS\) ActiveDirectory Federation Services Web. Il contient également des liens vers des listes de vérification qui vous aideront à exécuter les tâches requises pour implémenter cette conception subordonnées.  
  
> [!NOTE]  
> Effectuez les tâches de cette liste de vérification dans l’ordre. Lorsqu’un lien de référence vous redirige vers une rubrique conceptuelle ou une liste de vérification subordonnée, revenez à cette rubrique après avoir consulté la rubrique conceptuelle ou effectué les tâches dans la liste de vérification subordonnée, afin de pouvoir effectuer les tâches restantes de cette liste de vérification.  
  
![Sso de Web](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**liste de vérification: implémentation d’une conception SSO de Web**  
  
||Tâche|Référence|  
|-|--------|-------------|  
|![Sso de Web](media/icon_checkboxo.gif)|Passez en revue les concepts importants relatifs à la conception SSO de Web et déterminez quels objectifs de déploiement ADFS que vous pouvez utiliser pour personnaliser cette conception en fonction des besoins de votre organisation. **Remarque:**|![Sso de Web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[conception SSO de Web](https://technet.microsoft.com/library/dd807033.aspx)<br /><br />![Sso de Web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identifier vos objectifs de déploiement ADFS](https://technet.microsoft.com/library/dd807053.aspx)|  
|![Sso de Web](media/icon_checkboxo.gif)|Passez en revue le matériel, logiciel, certificat, Domain Name System \(DNS\), magasin d’attributs et configuration requise du client pour le déploiement d’ADFS dans votre organisation.|![Sso de Web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[annexe a: examen des annonces de la configuration requise pour FS](https://technet.microsoft.com/library/ff678034.aspx)|  
|![Sso de Web](media/icon_checkboxo.gif)|En fonction de votre plan de conception, installez un ou plusieurs serveurs de fédération dans le réseau d’entreprise ou dans le réseau de périmètre. **Remarque:** la conception SSO de Web ne nécessite qu’un seul serveur de fédération pour fonctionner correctement. Un serveur de fédération unique agit dans le rôle de fournisseur de revendications et le rôle de partie de confiance.|![Sso de Web](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[liste de vérification: Setting Up a Federation Server](Checklist--Setting-Up-a-Federation-Server.md)|  
|![Sso de Web](media/icon_checkboxo.gif)|\(Optional\) déterminer si votre organisation a besoin ou non un serveur proxy de fédération dans le réseau de périmètre.|![Sso de Web](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[liste de vérification: configuration d’un serveur Proxy de fédération](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![Sso de Web](media/icon_checkboxo.gif)|En fonction de votre plan de conception SSO de Web et comment vous souhaitez utiliser, ajoutez le magasin d’attributs appropriés, approbations, les revendications, de partie de confiance et règles de revendication au Service de fédération.|![Sso de Web](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[liste de vérification: configuration de l’organisation partenaire de compte](Checklist--Configuring-the-Account-Partner-Organization.md)|  
|![Sso de Web](media/icon_checkboxo.gif)|Si vous êtes administrateur dans l’organisation partenaire de ressource, activez les claims\ votre application de navigateur Web, une application de service Web ou une application Microsoft® Office SharePoint® Server à l’aide de WIF et le SDK WIF. **Remarque:**|![Sso de Web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[WindowsIdentityFoundation](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />![Sso de Web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[SDK de WindowsIdentityFoundation](https://go.microsoft.com/fwlink/?LinkId=122266)| 
