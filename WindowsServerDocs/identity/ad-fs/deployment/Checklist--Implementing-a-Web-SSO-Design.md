---
ms.assetid: 30657638-5709-48c5-87aa-98f688e07b4c
title: 'Liste de vérification : implémentation d’une conception SSO de Web'
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 8488e0c9195930374aacd959e72d0eff34142ca7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408465"
---
# <a name="checklist-implementing-a-web-sso-design"></a>Liste de vérification : Implémentation d’une conception SSO de web

Cette liste de contrôle parente comprend des liens vers des\-de référence vers des concepts importants relatifs à l’authentification Web unique\-\-sur \(\) services ADFS \(AD FS.\) Elle comprend également des liens vers des listes de vérification subordonnées qui vous aideront à exécuter les tâches requises pour implémenter cette conception.  
  
> [!NOTE]  
> Effectuez dans l’ordre les tâches répertoriées dans cette liste de vérification. Lorsqu'un lien de référence vous redirige vers une rubrique conceptuelle ou une liste de vérification subordonnée, revenez à la présente rubrique après avoir consulté la rubrique conceptuelle ou effectué les tâches de la liste de vérification subordonnée, afin de pouvoir effectuer les tâches restantes décrites dans cette liste de vérification principale.  
  
Liste de vérification de l’authentification unique Web ![](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**: implémentation d’une conception SSO de Web**  
  
||Tâche|Référence|  
|-|--------|-------------|  
|![SSO Web](media/icon_checkboxo.gif)|Passez en revue les concepts importants relatifs à la conception SSO de Web et déterminez les AD FS objectifs de déploiement que vous pouvez utiliser pour personnaliser cette conception en fonction des besoins de votre organisation. **Remarque :**|](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[conception SSO](https://technet.microsoft.com/library/dd807033.aspx) Web sso de ![Web<br /><br />![l’authentification unique Web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identifiant vos objectifs de déploiement AD FS](https://technet.microsoft.com/library/dd807053.aspx)|  
|![SSO Web](media/icon_checkboxo.gif)|Passez en revue le matériel, les logiciels, les certificats, le système de nom de domaine \(les\)DNS, le magasin d’attributs et la configuration requise du client pour le déploiement de AD FS dans votre organisation.|![Web SSO](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[annexe A : examen de la configuration requise pour la AD FS](https://technet.microsoft.com/library/ff678034.aspx)|  
|![SSO Web](media/icon_checkboxo.gif)|Selon votre plan de conception, installez un ou plusieurs serveurs de Fédération dans le réseau d’entreprise ou dans le réseau de périmètre. **Remarque :** La conception SSO de Web ne nécessite qu’un seul serveur de Fédération pour fonctionner correctement. Un serveur de Fédération unique fait partie du rôle de fournisseur de revendications et du rôle de partie de confiance.|Liste de vérification de l’authentification unique Web ![](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[: configuration d’un serveur de Fédération](Checklist--Setting-Up-a-Federation-Server.md)|  
|![SSO Web](media/icon_checkboxo.gif)|\(facultatif\) Déterminez si votre organisation a besoin ou non d’un proxy de serveur de Fédération dans le réseau de périmètre.|Liste de vérification de l’authentification unique Web ![](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[: configuration d’un serveur proxy de Fédération](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![SSO Web](media/icon_checkboxo.gif)|En fonction de votre plan de conception SSO de web et de la manière dont vous prévoyez de l'utiliser, ajoutez au service de fédération le magasin d'attributs, les approbations de partie de confiance, les revendications et les règles de revendication appropriés.|](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[liste de vérification de l’authentification unique web ![: configuration de l’organisation partenaire de compte](Checklist--Configuring-the-Account-Partner-Organization.md)|  
|![SSO Web](media/icon_checkboxo.gif)|Si vous êtes administrateur dans l’organisation du partenaire de ressource, les revendications\-activer votre application de navigateur Web, votre application de service Web ou votre application Microsoft® Office SharePoint® Server à l’aide de WIF et du kit de développement logiciel (SDK) WIF. **Remarque :**|![Web SSO](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />SDK ![Web SSO](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)| 
