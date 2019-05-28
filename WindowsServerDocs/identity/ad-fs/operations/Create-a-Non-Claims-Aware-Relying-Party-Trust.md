---
ms.assetid: 7b7ae389-5032-44f7-9c0a-94398c3e4d88
title: Créer une approbation de partie de confiance qui ne prend pas en charge les revendications
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cdd0b32b50f676007a6cc922bc15b95bb61323be
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189674"
---
# <a name="create-a-non-claims-aware-relying-party-trust"></a>Créer une Non-Claims-Aware partie de confiance


Dans le composant logiciel enfichable Gestion AD FS\-dans, non\-revendications\-prenant en charge les approbations de partie de confiance sont des objets qui sont créés pour représenter l’approbation entre le service de fédération et un seul site web\-application qui n’est pas basée sur revendications\-prenant en charge et qui est accessible via le Proxy d’Application Web.  
  
Un non\-revendications\-prenant en charge de confiance est une partie de confiance qui se compose d’identificateurs, les noms et les règles pour l’authentification et d’autorisation lors de l’approbation est accessible via le Proxy d’Application Web. Ces web\-en fonction des applications qui ne reposent pas sur les revendications, en d’autres termes, ces authentification Windows intégrée\-en fonction d’applications, peuvent avoir des règles d’autorisation qui permettent d’accéder est basée sur les revendications lorsque l’accès est externe au réseau d’entreprise via le Proxy d’Application Web.  
  
Pour ajouter un nouveau non\-revendications\-prenant en charge de confiance, à l’aide du composant logiciel enfichable Gestion AD FS\-, effectuez la procédure suivante.  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
##<a name="to-create-a-non-claims-aware-relying-party-trust-manually"></a>Pour créer un revendications confiance prenant en charge manuellement 
1. Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion AD FS**.  
  
2.  Sous **Actions**, cliquez sur **ajouter Relying Party Trust**.  
![partie de confiance](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  Sur le **Bienvenue** page, choisissez **revendications Non prenant en charge les** et cliquez sur **Démarrer**.  
![partie de confiance](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon1.PNG) 
  
4.  Sur le **entrer le nom complet** page, tapez un nom dans **nom d’affichage**, sous **Notes** une description pour cette partie de confiance, puis tapez **suivant** .  
![partie de confiance](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon2.PNG)

5. Dans la page **Configurer les identificateurs**, spécifiez un ou plusieurs identificateurs pour cette partie de confiance, cliquez sur **Ajouter** pour les ajouter à la liste, puis cliquez sur **Suivant**.  
![partie de confiance](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon3.PNG)

6.  Sur le **choisir la stratégie de contrôle d’accès** sélectionnez une stratégie, puis cliquez sur **suivant**.  Pour plus d’informations sur les stratégies de contrôle d’accès, consultez [stratégies de contrôle d’accès dans AD FS](Access-Control-Policies-in-AD-FS.md). 
![partie de confiance](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon4.PNG)

7. Dans la page **Prêt à ajouter l'approbation**, vérifiez les paramètres, puis cliquez sur **Suivant** pour enregistrer les informations de la nouvelle approbation de partie de confiance.  
   ![partie de confiance](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon5.PNG) 

8. Dans la page **Terminer** , cliquez sur **Fermer**. Cette action affiche automatiquement la boîte de dialogue **Modifier les règles de revendication**.  
![partie de confiance](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon6.PNG)  
  
## <a name="see-also"></a>Voir aussi  
[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
