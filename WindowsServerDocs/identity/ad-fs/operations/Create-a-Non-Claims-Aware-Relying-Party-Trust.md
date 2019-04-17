---
ms.assetid: 7b7ae389-5032-44f7-9c0a-94398c3e4d88
title: "Créer une approbation de partie de partie de confiance prenant en charge de revendications"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f46675ff4c471af743fd8782c1e3036e7c546256
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-non-claims-aware-relying-party-trust"></a>Créer une approbation de partie de confiance Non charge les revendications

>S’applique à: Windows Server2016, Windows Server2012R2

Dans la gestion ADFS dans composants, autre que celle claims\ charge les approbations de partie de confiance sont des objets qui sont créés pour représenter la relation d’approbation entre le service de fédération et une application basée sur une console Web unique qui n’est pas prenant en charge claims\ et qui est accessible via le Proxy d’Application Web.  
  
Une approbation de partie de confiance autre que celle claims\-prenant en charge est une partie de confiance qui se compose d’identificateurs, les noms et les règles pour l’authentification et d’autorisation lors de l’approbation est accessible via le Proxy d’Application Web. Ces applications de console Web qui ne reposent pas sur les revendications, en d’autres termes, ces applications basées sur les Authentication\ de Windows intégrée, peut avoir des règles d’autorisation qui permettent l’accès en fonction des revendications lors de l’accès est externe au réseau d’entreprise via le Proxy d’Application Web.  
  
Pour ajouter une nouveau autre que celle claims\-prenant en charge de confiance, à l’aide de la gestion ADFS dans composants, effectuez la procédure suivante.  
  
L’appartenance au groupe **administrateurs**, ou équivalente, sur l’ordinateur local est la condition minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
##<a name="to-create-a-non-claims-aware-relying-party-trust-manually"></a>Pour créer un revendications prenant en charge de confiance manuellement 
1. Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion ADFS**.  
  
2.  Sous **Actions**, cliquez sur **ajouter une approbation de partie de confiance **.  
![Partie de confiance](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  Sur le **Bienvenue** page, choisissez **Non revendications prenant en charge** et cliquez sur **Démarrer**.  
![Partie de confiance](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon1.PNG) 
  
4.  Sur le **entrer le nom complet**, tapez un nom dans **nom d’affichage**, sous **Notes** tapez une description pour cette approbation de partie de confiance, puis cliquez sur **suivant **.  
![Partie de confiance](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon2.PNG)

5. Sur le **configurer les identificateurs**, spécifiez un ou plusieurs identificateurs pour cette partie de confiance, cliquez sur **ajouter** pour les ajouter à la liste, puis cliquez sur **suivant**.  
![Partie de confiance](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon3.PNG)

6.  Sur le **stratégie de contrôle d’accès choisir** sélectionnez une stratégie et cliquez sur **suivant**.  Pour plus d’informations sur les stratégies de contrôle d’accès, voir [stratégies de contrôle d’accès dans ADFS](Access-Control-Policies-in-AD-FS.md). 
![Partie de confiance](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon4.PNG)

7. Sur le **prêt à ajouter l’approbation** page, passez en revue les paramètres, puis cliquez sur **suivant** pour enregistrer votre confiance les informations de confidentialité.  
   ![Partie de confiance](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon5.PNG) 

8. Sur le **Terminer**, cliquez sur **fermer**. Cette action affiche automatiquement les **modifier les règles de revendication** boîte de dialogue.  
![Partie de confiance](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon6.PNG)  
  
## <a name="see-also"></a>Voir aussi  
[Opérations ADFS](../../ad-fs/AD-FS-2016-Operations.md) 
