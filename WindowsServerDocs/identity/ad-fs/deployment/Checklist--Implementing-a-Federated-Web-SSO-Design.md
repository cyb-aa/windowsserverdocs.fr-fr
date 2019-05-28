---
ms.assetid: 6b49cde3-d2cb-4ece-b9b7-dc600e037495
title: 'Liste de vérification : implémentation d’une conception SSO Web fédéré'
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 1f7a65687ac07c9f7d9a814b50c4090c70817b4f
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192370"
---
# <a name="checklist-implementing-a-federated-web-sso-design"></a>Liste de vérification : implémentation d'une conception SSO de web fédéré

Cette liste de vérification principale inclut croisé\-référencer des liens vers des concepts importants relatifs à l’unique Web fédérée\-connexion\-sur \(SSO\) conception d’Active Directory Federation Services \(AD FS\). Elle comprend également des liens vers des listes de vérification subordonnées qui vous aideront à exécuter les tâches requises pour implémenter cette conception.  
  
> [!NOTE]  
> Effectuez dans l’ordre les tâches répertoriées dans cette liste de vérification. Lorsqu'un lien de référence vous redirige vers une rubrique conceptuelle ou une liste de vérification subordonnée, revenez à la présente rubrique après avoir consulté la rubrique conceptuelle ou effectué les tâches de la liste de vérification subordonnée, afin de pouvoir effectuer les tâches restantes décrites dans cette liste de vérification principale.  
  
![authentification unique web fédérée](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**liste de vérification : implémentation d’une conception SSO de web fédéré**  
  
||Tâche|Référence|  
|-|--------|-------------|  
|![sso de web fédéré](media/icon_checkboxo.gif)|Passez en revue les concepts importants relatifs à la conception SSO de Web fédéré et déterminez quels objectifs de déploiement AD FS que vous pouvez utiliser pour personnaliser cette conception en fonction des besoins de votre organisation.|![authentification unique web fédérée](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Federated Web SSO Design](https://technet.microsoft.com/library/dd807050.aspx)<br /><br />![authentification unique web fédérée](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identifier vos objectifs de déploiement AD FS](https://technet.microsoft.com/library/dd807053.aspx)<br /><br />![authentification unique web fédérée](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planification de votre déploiement](https://technet.microsoft.com/library/dd807083.aspx)|  
|![sso de web fédéré](media/icon_checkboxo.gif)|Passez en revue le matériel, le logiciel, le certificat, Domain Name System \(DNS\), magasin d’attributs et la configuration requise du client pour le déploiement d’AD FS dans votre organisation.|![authentification unique web fédérée](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[annexe a : examen de la configuration requise pour AD FS](https://technet.microsoft.com/library/ff678034.aspx)|  
|![sso de web fédéré](media/icon_checkboxo.gif)|Passez en revue les concepts importants sur les revendications, les règles associées, les magasins d’attributs et la base de données de configuration AD FS avant de déployer AD FS dans les deux organisations partenaires.|![authentification unique web fédérée](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Understanding Key AD FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
|![sso de web fédéré](media/icon_checkboxo.gif)|En fonction de votre plan de conception, installez un ou plusieurs serveurs de fédération dans chaque organisation partenaire. **Remarque :** Pour la conception SSO de Web fédéré, vous avez besoin d’au moins un serveur de fédération dans l’organisation partenaire de compte et au moins un serveur de fédération dans l’organisation partenaire ressource.|![authentification unique web fédérée](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[liste de vérification : Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)|  
|![sso de web fédéré](media/icon_checkboxo.gif)|\(Facultatif\) déterminer si votre organisation a besoin d’un serveur proxy de fédération. Si votre plan de conception fait appel à un proxy, vous pouvez installer un ou plusieurs proxys de serveur de fédération dans chaque organisation partenaire.|![authentification unique web fédérée](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[liste de vérification : configuration d’un serveur de fédération proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![sso de web fédéré](media/icon_checkboxo.gif)|En fonction de votre plan de conception, partagez les certificats, configurez les clients, et configurez les relations d'approbation dans les deux organisations partenaires afin qu'elles puissent communiquer entre elles via une approbation de fédération.|![authentification unique web fédérée](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[liste de vérification : Configuration de l’organisation partenaire de compte](Checklist--Configuring-the-Account-Partner-Organization.md)<br /><br />![authentification unique web fédérée](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[liste de vérification : Configuration de l’organisation partenaire de ressource](Checklist--Configuring-the-Resource-Partner-Organization.md)|  
|![sso de web fédéré](media/icon_checkboxo.gif)|Si vous êtes un administrateur de l’organisation partenaire de ressource, revendications\-activer votre application de navigateur Web, une application de service Web ou une application de Microsoft® Office SharePoint® Server à l’aide de WIF et le SDK WIF.|![authentification unique web fédérée](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />![authentification unique web fédérée](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266)|  
