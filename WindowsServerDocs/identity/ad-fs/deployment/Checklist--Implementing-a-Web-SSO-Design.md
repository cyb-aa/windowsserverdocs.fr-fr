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
# <a name="checklist-implementing-a-web-sso-design"></a>Liste de vérification : implémentation d'une conception SSO de web

Cette liste de contrôle parente comprend des liens entre @ no__t-0reference et des concepts importants relatifs à la conception Web unique @ no__t-1Sign @ no__t-2On \(SSO @ no__t-4 pour Services ADFS \(AD FS @ no__t-6. Elle comprend également des liens vers des listes de vérification subordonnées qui vous aideront à exécuter les tâches requises pour implémenter cette conception.  
  
> [!NOTE]  
> Effectuez dans l’ordre les tâches répertoriées dans cette liste de vérification. Lorsqu'un lien de référence vous redirige vers une rubrique conceptuelle ou une liste de vérification subordonnée, revenez à la présente rubrique après avoir consulté la rubrique conceptuelle ou effectué les tâches de la liste de vérification subordonnée, afin de pouvoir effectuer les tâches restantes décrites dans cette liste de vérification principale.  
  
![WEB SSO @ no__t-1Checklist : implémentation d’une conception SSO de web**  
  
||Tâche|Référence|  
|-|--------|-------------|  
|![SSO Web](media/icon_checkboxo.gif)|Passez en revue les concepts importants relatifs à la conception SSO de Web et déterminez les AD FS objectifs de déploiement que vous pouvez utiliser pour personnaliser cette conception en fonction des besoins de votre organisation. **Remarque :**|](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[conception SSO Web](https://technet.microsoft.com/library/dd807033.aspx) SSO @no__t 0WEB<br /><br />![WEB authentification unique](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[identifiant vos objectifs de déploiement AD FS](https://technet.microsoft.com/library/dd807053.aspx)|  
|![SSO Web](media/icon_checkboxo.gif)|Passez en revue le matériel, les logiciels, les certificats, le système de noms de domaine @no__t 0DNS @ no__t-1, le magasin d’attributs et la configuration client requise pour le déploiement d’AD FS dans votre organisation.|![WEB SSO @ no__t-1Appendix A : examen de la configuration requise pour AD FS](https://technet.microsoft.com/library/ff678034.aspx)|  
|![SSO Web](media/icon_checkboxo.gif)|Selon votre plan de conception, installez un ou plusieurs serveurs de Fédération dans le réseau d’entreprise ou dans le réseau de périmètre. **Remarque :** La conception SSO de Web ne nécessite qu’un seul serveur de Fédération pour fonctionner correctement. Un serveur de Fédération unique fait partie du rôle de fournisseur de revendications et du rôle de partie de confiance.|![WEB SSO @ no__t-1Checklist : Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)|  
|![SSO Web](media/icon_checkboxo.gif)|\(Optional @ no__t-1 détermine si votre organisation a besoin ou non d’un proxy de serveur de Fédération dans le réseau de périmètre.|![WEB SSO @ no__t-1Checklist : configuration d’un serveur de fédération proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![SSO Web](media/icon_checkboxo.gif)|En fonction de votre plan de conception SSO de web et de la manière dont vous prévoyez de l'utiliser, ajoutez au service de fédération le magasin d'attributs, les approbations de partie de confiance, les revendications et les règles de revendication appropriés.|![WEB SSO @ no__t-1Checklist : Configuration de l’organisation partenaire de compte @ no__t-0|  
|![SSO Web](media/icon_checkboxo.gif)|Si vous êtes administrateur dans l’organisation du partenaire de ressource, les revendications @ no__t-0enable votre application de navigateur Web, votre application de service Web ou votre application Microsoft® Office SharePoint® Server à l’aide de WIF et du kit de développement logiciel (SDK) WIF. **Remarque :**|![WEB SSO](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266)<br /><br />](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Kit de développement logiciel (SDK) Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266) @no__t 0WEB SSO| 
