---
ms.assetid: 5b9fc9c1-5d12-4ad4-8ddc-3b8a6d45b217
title: Créer une partie de confiance
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a0d32edd7ebc23fa724439710c6511642d9c49a3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407649"
---
# <a name="create-a-relying-party-trust"></a>Créer une partie de confiance


Le document suivant fournit des informations sur la création manuelle d’une approbation de partie de confiance et l’utilisation de métadonnées de Fédération.
  
## <a name="to-create-a-claims-aware-relying-party-trust-manually"></a>Pour créer manuellement une approbation de partie de confiance prenant en charge les revendications 

Pour ajouter une nouvelle approbation de partie de confiance à l’aide du composant logiciel enfichable Gestion des AD FS\-dans et configurer manuellement les paramètres, effectuez la procédure suivante sur un serveur de Fédération.  

Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).
  
1. Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  
  
2.  Sous **actions**, cliquez sur **Ajouter une approbation de partie de confiance**.  
![partie de confiance](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  Sur la page **Bienvenue** , choisissez prise en **charge des revendications** , puis cliquez sur **Démarrer**.  
![partie de confiance](media/Create-a-Relying-Party-Trust/addtrust2.PNG) 
  
4.  Dans la page **Sélectionner une source de données**, cliquez sur **Entrer manuellement les données concernant la partie de confiance**, puis sur **Suivant**.  
![partie de confiance](media/Create-a-Relying-Party-Trust/addtrust3.PNG) 
  
5.  Dans la page **spécifier le nom complet** , tapez un nom dans **nom complet**, sous **Remarques** , tapez une description pour cette approbation de partie de confiance, puis cliquez sur **suivant**.  
![partie de confiance](media/Create-a-Relying-Party-Trust/addtrust4.PNG) 

6. Dans la page **configurer le certificat** , si vous disposez d’un certificat de chiffrement de jeton facultatif, cliquez sur **Parcourir** pour rechercher un fichier de certificat, puis cliquez sur **suivant**.  
![partie de confiance](media/Create-a-Relying-Party-Trust/addtrust5.PNG) 

7.  Dans la page **configurer l’URL** , effectuez l’une des opérations suivantes ou les deux, cliquez sur **suivant**, puis passez à l’étape 8 :  
  
    -   Activez la case à cocher **activer la prise en charge de WS\-Federation passive Protocol** . Sous **URL du protocole\-WS passif**de la partie de confiance, tapez l’URL de cette approbation de partie de confiance, puis cliquez sur **suivant**.  
  
    -   Activez la case à cocher **Activer la prise en charge du protocole WebSSO SAML 2.0**. Sous **URL du service SSO saml 2,0**de la partie de confiance, tapez le Security Assertion Markup Language \(URL du point de terminaison de service\) SAML pour cette approbation de partie de confiance, puis cliquez sur **suivant**.  
![partie de confiance](media/Create-a-Relying-Party-Trust/addtrust6.PNG)   

8. Dans la page **Configurer les identificateurs**, spécifiez un ou plusieurs identificateurs pour cette partie de confiance, cliquez sur **Ajouter** pour les ajouter à la liste, puis cliquez sur **Suivant**.  
![partie de confiance](media/Create-a-Relying-Party-Trust/addtrust8.PNG)
  
9.  Dans la **stratégie choisir un Access Control** , sélectionnez une stratégie, puis cliquez sur **suivant**.  Pour plus d’informations sur les stratégies de Access Control, consultez [Access Control des stratégies dans AD FS](Access-Control-Policies-in-AD-FS.md). 
![partie de confiance](media/Create-a-Relying-Party-Trust/addtrust9.PNG)

10. Dans la page **Prêt à ajouter l'approbation**, vérifiez les paramètres, puis cliquez sur **Suivant** pour enregistrer les informations de la nouvelle approbation de partie de confiance.  
   ![partie de confiance](media/Create-a-Relying-Party-Trust/addtrust10.PNG) 
11. Dans la page **Terminer** , cliquez sur **Fermer**. Cette action affiche automatiquement la boîte de dialogue **Modifier les règles de revendication**.  
![partie de confiance](media/Create-a-Relying-Party-Trust/addtrust11.PNG) 

## <a name="to-create-a-claims-aware-relying-party-trust-using-federation-metadata"></a>Pour créer une approbation de partie de confiance prenant en charge les revendications à l’aide de métadonnées de Fédération

Pour ajouter une nouvelle approbation de partie de confiance, en utilisant le composant logiciel enfichable de gestion AD FS, en important automatiquement les données de configuration relatives au partenaire à partir des métadonnées de Fédération que le partenaire a publiées sur un réseau local ou sur Internet, effectuez la procédure suivante sur un serveur de Fédération dans l’organisation partenaire de compte.

>[!NOTE]
>Bien qu’il s’agisse d’une pratique courante d’utiliser des certificats avec des noms d’hôte non qualifiés comme https://myserver, ces certificats n’ont aucune valeur de sécurité et peuvent permettre à une personne malveillante d’emprunter l’identité d’un service FS (Federation Service) qui publie des métadonnées de Fédération. Par conséquent, lors de l’interrogation des métadonnées de Fédération, vous devez uniquement utiliser un nom de domaine complet, tel que https://myserver.contoso.com.

Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).


1. Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  
  
2. Sous **actions**, cliquez sur **Ajouter une approbation de partie de confiance**.  
   ![partie de confiance](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3. Sur la page **Bienvenue** , choisissez prise en **charge des revendications** , puis cliquez sur **Démarrer**.  
   ![partie de confiance](media/Create-a-Relying-Party-Trust/addtrust2.PNG) 
  
4. Dans la page **Sélectionner une source de données** , cliquez sur <strong>Importer les données publiées en ligne ou sur un réseau local dans la partie de confiance. Dans * * adresse des métadonnées de Fédération (nom d’hôte ou URL)</strong>, tapez l’URL des métadonnées de Fédération ou le nom d’hôte du partenaire, puis cliquez sur **suivant**.  
   ![partie de confiance](media/Create-a-Relying-Party-Trust/addtrust12.PNG) 

5. Dans la page spécifier le nom complet, tapez un nom dans **nom complet**, sous remarques, tapez une description pour cette approbation de partie de confiance, puis cliquez sur **suivant**.

6. Dans la page choisir les règles d’autorisation d’émission, sélectionnez **autoriser tous les utilisateurs à accéder à cette partie de confiance** ou **refuser l’accès de tous les utilisateurs à cette partie de confiance**, puis cliquez sur **suivant**.

7. Dans la page prêt à ajouter l’approbation, passez en revue les paramètres, puis cliquez sur **suivant** pour enregistrer vos informations d’approbation de partie de confiance.

8. Dans la page terminer, cliquez sur **Fermer**. Cette action affiche automatiquement la boîte de dialogue Modifier les règles de revendication. Pour plus d'informations sur l'ajout de règles de revendication pour cette approbation de partie de confiance, voir Références supplémentaires.




## <a name="see-also"></a>Voir aussi  
[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
