---
ms.assetid: 30657638-5709-48c5-87aa-98f688e07b4c
title: 'Liste de vérification : implémentation d’une conception SSO de Web'
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: b2e09addbdc192bc6ce93a4402e6a6e61873ad56
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192362"
---
# <a name="checklist-implementing-a-web-sso-design"></a>Liste de vérification : implémentation d'une conception SSO de web

Cette liste de vérification principale inclut croisé\-référencer des liens vers des concepts importants relatifs à l’unique Web\-connexion\-sur \(SSO\) conception d’Active Directory Federation Services \(AD FS \). Elle comprend également des liens vers des listes de vérification subordonnées qui vous aideront à exécuter les tâches requises pour implémenter cette conception.  
  
> [!NOTE]  
> Effectuez dans l’ordre les tâches répertoriées dans cette liste de vérification. Lorsqu'un lien de référence vous redirige vers une rubrique conceptuelle ou une liste de vérification subordonnée, revenez à la présente rubrique après avoir consulté la rubrique conceptuelle ou effectué les tâches de la liste de vérification subordonnée, afin de pouvoir effectuer les tâches restantes décrites dans cette liste de vérification principale.  
  
![sso de Web](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**liste de vérification : implémentation d’une conception SSO de web**  
  
||Tâche|Référence|  
|-|--------|-------------|  
|![sso de Web](media/icon_checkboxo.gif)|Passez en revue les concepts importants relatifs à la conception SSO de Web et déterminez quels objectifs de déploiement AD FS que vous pouvez utiliser pour personnaliser cette conception en fonction des besoins de votre organisation. **Remarque :**|![sso de Web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[conception SSO de Web](https://technet.microsoft.com/library/dd807033.aspx)<br /><br />![sso de Web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identifier vos objectifs de déploiement AD FS](https://technet.microsoft.com/library/dd807053.aspx)|  
|![sso de Web](media/icon_checkboxo.gif)|Passez en revue le matériel, le logiciel, le certificat, Domain Name System \(DNS\), magasin d’attributs et la configuration requise du client pour le déploiement d’AD FS dans votre organisation.|![sso de Web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[annexe a : examen de la configuration requise pour AD FS](https://technet.microsoft.com/library/ff678034.aspx)|  
|![sso de Web](media/icon_checkboxo.gif)|En fonction de votre plan de conception, installez un ou plusieurs serveurs de fédération dans le réseau d’entreprise ou dans le réseau de périmètre. **Remarque :** La conception SSO de Web ne nécessite qu’un seul serveur de fédération pour fonctionner correctement. Un serveur de fédération unique agit dans le rôle de fournisseur de revendications et le rôle de partie de confiance.|![sso de Web](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[liste de vérification : Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)|  
|![sso de Web](media/icon_checkboxo.gif)|\(Facultatif\) déterminer si votre organisation a besoin d’un serveur proxy de fédération dans le réseau de périmètre.|![sso de Web](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[liste de vérification : configuration d’un serveur de fédération proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![sso de Web](media/icon_checkboxo.gif)|En fonction de votre plan de conception SSO de web et de la manière dont vous prévoyez de l'utiliser, ajoutez au service de fédération le magasin d'attributs, les approbations de partie de confiance, les revendications et les règles de revendication appropriés.|![sso de Web](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[liste de vérification : Configuration de l’organisation partenaire de compte](Checklist--Configuring-the-Account-Partner-Organization.md)|  
|![sso de Web](media/icon_checkboxo.gif)|Si vous êtes un administrateur de l’organisation partenaire de ressource, revendications\-activer votre application de navigateur Web, une application de service Web ou une application de Microsoft® Office SharePoint® Server à l’aide de WIF et le SDK WIF. **Remarque :**|![sso de Web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />![sso de Web](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266)| 
