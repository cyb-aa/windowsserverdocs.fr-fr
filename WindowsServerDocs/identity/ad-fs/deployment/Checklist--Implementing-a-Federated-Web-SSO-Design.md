---
ms.assetid: 6b49cde3-d2cb-4ece-b9b7-dc600e037495
title: 'Liste de vérification : implémentation d’une conception SSO de Web fédéré'
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 165471fd06031a68343a54d019357afee782d082
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359949"
---
# <a name="checklist-implementing-a-federated-web-sso-design"></a>Liste de vérification : Implémentation d’une conception SSO de web fédéré

Cette liste de contrôle parente\-comprend des liens vers des liens vers des concepts importants relatifs à la\-de\-SSO Web fédéré sur \(\) services ADFS \(AD FS.\) Elle comprend également des liens vers des listes de vérification subordonnées qui vous aideront à exécuter les tâches requises pour implémenter cette conception.  
  
> [!NOTE]  
> Effectuez dans l’ordre les tâches répertoriées dans cette liste de vérification. Lorsqu'un lien de référence vous redirige vers une rubrique conceptuelle ou une liste de vérification subordonnée, revenez à la présente rubrique après avoir consulté la rubrique conceptuelle ou effectué les tâches de la liste de vérification subordonnée, afin de pouvoir effectuer les tâches restantes décrites dans cette liste de vérification principale.  
  
Liste de vérification de l’authentification unique Web fédérée ![](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**: implémentation d’une conception SSO de Web fédéré**  
  
||Tâche|Référence|  
|-|--------|-------------|  
|![SSO Web fédéré](media/icon_checkboxo.gif)|Passez en revue les concepts importants relatifs à la conception SSO de Web fédéré et déterminez les AD FS objectifs de déploiement que vous pouvez utiliser pour personnaliser cette conception en fonction des besoins de votre organisation.|![](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[conception SSO de Web fédéré](https://technet.microsoft.com/library/dd807050.aspx) fédérée de SSO Web<br /><br />![SSO Web fédéré](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identifiant vos objectifs de déploiement AD FS](https://technet.microsoft.com/library/dd807053.aspx)<br /><br />![de l’authentification unique Web fédérée](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planification de votre déploiement](https://technet.microsoft.com/library/dd807083.aspx)|  
|![SSO Web fédéré](media/icon_checkboxo.gif)|Passez en revue le matériel, les logiciels, les certificats, le système de nom de domaine \(les\)DNS, le magasin d’attributs et la configuration requise du client pour le déploiement de AD FS dans votre organisation.|![](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[l’annexe A de la SSO Web fédéré : examen de la configuration requise pour la AD FS](https://technet.microsoft.com/library/ff678034.aspx)|  
|![SSO Web fédéré](media/icon_checkboxo.gif)|Passez en revue les concepts importants relatifs aux revendications, aux règles de revendication, aux magasins d’attributs et à la base de données de configuration AD FS avant de déployer des AD FS dans les deux organisations partenaires.|](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[concepts clés AD FS](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md) de la ![de la technologie SSO fédérée|  
|![SSO Web fédéré](media/icon_checkboxo.gif)|Selon votre plan de conception, installez un ou plusieurs serveurs de Fédération dans chaque organisation partenaire. **Remarque :** Pour la conception SSO de Web fédéré, vous devez disposer d’au moins un serveur de Fédération dans l’organisation partenaire de compte et au moins un serveur de Fédération dans l’organisation partenaire de ressource.|Liste de vérification de l’authentification unique Web fédérée ![](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[: configuration d’un serveur de Fédération](Checklist--Setting-Up-a-Federation-Server.md)|  
|![SSO Web fédéré](media/icon_checkboxo.gif)|\(facultatif\) Déterminez si votre organisation a besoin ou non d’un serveur proxy de Fédération. Si votre plan de conception appelle un proxy, vous pouvez installer un ou plusieurs serveurs proxy de Fédération dans chaque organisation partenaire.|Liste de vérification de l’authentification unique Web fédérée ![](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[: configuration d’un serveur proxy de Fédération](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![SSO Web fédéré](media/icon_checkboxo.gif)|En fonction de votre plan de conception, partagez les certificats, configurez les clients, et configurez les relations d'approbation dans les deux organisations partenaires afin qu'elles puissent communiquer entre elles via une approbation de fédération.|](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[liste de vérification de l’authentification unique Web fédérée ![: configuration de l’organisation partenaire de compte](Checklist--Configuring-the-Account-Partner-Organization.md)<br /><br />](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[liste de vérification de l’authentification unique Web fédérée ![: configuration de l’organisation partenaire de ressource](Checklist--Configuring-the-Resource-Partner-Organization.md)|  
|![SSO Web fédéré](media/icon_checkboxo.gif)|Si vous êtes administrateur dans l’organisation du partenaire de ressource, les revendications\-activer votre application de navigateur Web, votre application de service Web ou votre application Microsoft® Office SharePoint® Server à l’aide de WIF et du kit de développement logiciel (SDK) WIF.|![authentification Web fédérée de](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />![le kit de](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[développement logiciel (SDK) Web fédéré de Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)|  
