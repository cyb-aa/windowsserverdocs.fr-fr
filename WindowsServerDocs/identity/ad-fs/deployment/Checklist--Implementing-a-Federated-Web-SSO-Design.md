---
ms.assetid: 6b49cde3-d2cb-4ece-b9b7-dc600e037495
title: "Liste de vérification: implémentation d’une conception SSO de Web fédéré"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 1122ee3814f656a7229dc2946d7441d525de5ae2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="checklist-implementing-a-federated-web-sso-design"></a>Liste de vérification: Implémentation d’une conception SSO de Web fédéré

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette liste de vérification principale inclut une nouveauté-référence des liens vers des concepts importants relatifs à la conception \(SSO\) Single\-Sign\-On de Web fédéré pour \(ADFS\) ActiveDirectory Federation Services. Il contient également des liens vers des listes de vérification qui vous aideront à exécuter les tâches requises pour implémenter cette conception subordonnées.  
  
> [!NOTE]  
> Effectuez les tâches de cette liste de vérification dans l’ordre. Lorsqu’un lien de référence vous redirige vers une rubrique conceptuelle ou une liste de vérification subordonnée, revenez à cette rubrique une fois que vous passez en revue la rubrique conceptuelle ou que vous effectuez les tâches dans la liste de vérification subordonnée, afin que vous pouvez poursuivre les tâches restantes de cette liste de vérification.  
  
![Federated web sso](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**liste de vérification: implémentation d’une conception SSO de Web fédéré**  
  
||Tâche|Référence|  
|-|--------|-------------|  
|![Sso de web fédéré](media/icon_checkboxo.gif)|Passez en revue les concepts importants relatifs à la conception SSO de Web fédéré et déterminez quels objectifs de déploiement ADFS que vous pouvez utiliser pour personnaliser cette conception en fonction des besoins de votre organisation.|![Federated web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[conception SSO de Web fédéré](https://technet.microsoft.com/library/dd807050.aspx)<br /><br />![Federated web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identifier vos objectifs de déploiement ADFS](https://technet.microsoft.com/library/dd807053.aspx)<br /><br />![Federated web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planification de votre déploiement](https://technet.microsoft.com/library/dd807083.aspx)|  
|![Sso de web fédéré](media/icon_checkboxo.gif)|Passez en revue le matériel, logiciel, certificat, Domain Name System \(DNS\), magasin d’attributs et configuration requise du client pour le déploiement d’ADFS dans votre organisation.|![Federated web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[annexe a: examen des annonces de la configuration requise pour FS](https://technet.microsoft.com/library/ff678034.aspx)|  
|![Sso de web fédéré](media/icon_checkboxo.gif)|Passez en revue les concepts importants relatifs aux revendications, de la base de données de configuration ADFS, les magasins d’attributs et règles de revendication avant de déployer les services ADFS dans les deux organisations partenaires.|![Federated web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Understanding Key ADFS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
|![Sso de web fédéré](media/icon_checkboxo.gif)|En fonction de votre plan de conception, installez un ou plusieurs serveurs de fédération dans chaque organisation partenaire. **Remarque:** pour la conception SSO de Web fédéré, vous avez besoin d’au moins un serveur de fédération dans l’organisation partenaire de compte et au moins un serveur de fédération dans l’organisation partenaire de ressource.|![Federated web sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[liste de vérification: Setting Up a Federation Server](Checklist--Setting-Up-a-Federation-Server.md)|  
|![Sso de web fédéré](media/icon_checkboxo.gif)|\(Optional\) déterminer si votre organisation a besoin ou non un serveur proxy de fédération. Si votre plan de conception fait appel à un serveur proxy, vous pouvez installer un ou plusieurs proxys de serveur de fédération dans chaque organisation partenaire.|![Federated web sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[liste de vérification: configuration d’un serveur Proxy de fédération](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![Sso de web fédéré](media/icon_checkboxo.gif)|En fonction de votre plan de conception, partager des certificats, configurez les clients et configurer les relations d’approbation dans les deux organisations partenaires afin qu’ils puissent communiquer via une approbation de fédération.|![Federated web sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[liste de vérification: configuration de l’organisation partenaire de compte](Checklist--Configuring-the-Account-Partner-Organization.md)<br /><br />![Federated web sso](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[liste de vérification: configuration de l’organisation partenaire de ressource](Checklist--Configuring-the-Resource-Partner-Organization.md)|  
|![Sso de web fédéré](media/icon_checkboxo.gif)|Si vous êtes administrateur dans l’organisation partenaire de ressource, activez les claims\ votre application de navigateur Web, une application de service Web ou une application Microsoft® Office SharePoint® Server à l’aide de WIF et le SDK WIF.|![Federated web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[WindowsIdentityFoundation](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />![Federated web sso](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[SDK de WindowsIdentityFoundation](https://go.microsoft.com/fwlink/?LinkId=122266)|  
