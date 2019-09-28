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
# <a name="checklist-implementing-a-federated-web-sso-design"></a>Liste de vérification : implémentation d'une conception SSO de web fédéré

Cette liste de contrôle parente comprend des liens entre @ no__t-0reference et des concepts importants relatifs à la conception Web unique fédérée @ no__t-1Sign @ no__t-2On \(SSO @ no__t-4 pour Services ADFS \(AD FS @ no__t-6. Elle comprend également des liens vers des listes de vérification subordonnées qui vous aideront à exécuter les tâches requises pour implémenter cette conception.  
  
> [!NOTE]  
> Effectuez dans l’ordre les tâches répertoriées dans cette liste de vérification. Lorsqu'un lien de référence vous redirige vers une rubrique conceptuelle ou une liste de vérification subordonnée, revenez à la présente rubrique après avoir consulté la rubrique conceptuelle ou effectué les tâches de la liste de vérification subordonnée, afin de pouvoir effectuer les tâches restantes décrites dans cette liste de vérification principale.  
  
![federated Web SSO @ no__t-1Checklist : implémentation d’une conception SSO de web fédéré**  
  
||Tâche|Référence|  
|-|--------|-------------|  
|![SSO Web fédéré](media/icon_checkboxo.gif)|Passez en revue les concepts importants relatifs à la conception SSO de Web fédéré et déterminez les AD FS objectifs de déploiement que vous pouvez utiliser pour personnaliser cette conception en fonction des besoins de votre organisation.|](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[conception SSO Web fédérée](https://technet.microsoft.com/library/dd807050.aspx) de l’authentification unique Web @no__t 0federated<br /><br />![federated Web SSO](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identifiant vos objectifs de déploiement AD FS](https://technet.microsoft.com/library/dd807053.aspx)<br /><br />![federated Web SSO](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planification de votre déploiement](https://technet.microsoft.com/library/dd807083.aspx)|  
|![SSO Web fédéré](media/icon_checkboxo.gif)|Passez en revue le matériel, les logiciels, les certificats, le système de noms de domaine @no__t 0DNS @ no__t-1, le magasin d’attributs et la configuration client requise pour le déploiement d’AD FS dans votre organisation.|![federated Web SSO @ no__t-1Appendix A : examen de la configuration requise pour AD FS](https://technet.microsoft.com/library/ff678034.aspx)|  
|![SSO Web fédéré](media/icon_checkboxo.gif)|Passez en revue les concepts importants relatifs aux revendications, aux règles de revendication, aux magasins d’attributs et à la base de données de configuration AD FS avant de déployer des AD FS dans les deux organisations partenaires.|![federated Web SSO](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Understanding Key AD FS concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
|![SSO Web fédéré](media/icon_checkboxo.gif)|Selon votre plan de conception, installez un ou plusieurs serveurs de Fédération dans chaque organisation partenaire. **Remarque :** Pour la conception SSO de Web fédéré, vous devez disposer d’au moins un serveur de Fédération dans l’organisation partenaire de compte et au moins un serveur de Fédération dans l’organisation partenaire de ressource.|![federated Web SSO @ no__t-1Checklist : Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)|  
|![SSO Web fédéré](media/icon_checkboxo.gif)|\(Optional @ no__t-1 Déterminez si votre organisation a besoin d’un serveur proxy de Fédération. Si votre plan de conception appelle un proxy, vous pouvez installer un ou plusieurs serveurs proxy de Fédération dans chaque organisation partenaire.|![federated Web SSO @ no__t-1Checklist : configuration d’un serveur de fédération proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![SSO Web fédéré](media/icon_checkboxo.gif)|En fonction de votre plan de conception, partagez les certificats, configurez les clients, et configurez les relations d'approbation dans les deux organisations partenaires afin qu'elles puissent communiquer entre elles via une approbation de fédération.|![federated Web SSO @ no__t-1Checklist : Configuration de l’organisation partenaire de compte @ no__t-0<br /><br />![federated Web SSO @ no__t-1Checklist : Configuration de l’organisation du partenaire de ressource @ no__t-0|  
|![SSO Web fédéré](media/icon_checkboxo.gif)|Si vous êtes administrateur dans l’organisation du partenaire de ressource, les revendications @ no__t-0enable votre application de navigateur Web, votre application de service Web ou votre application Microsoft® Office SharePoint® Server à l’aide de WIF et du kit de développement logiciel (SDK) WIF.|![federated Web SSO](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />![federated Web SSO](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266)|  
