---
ms.assetid: 5b9fc9c1-5d12-4ad4-8ddc-3b8a6d45b217
title: Créer une partie de confiance
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b000d4cfd4ded7ad37dbb235a9d33c83d8951707
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444361"
---
# <a name="create-a-relying-party-trust"></a>Créer une partie de confiance


Le document suivant fournit des informations sur la création manuelle d’une partie de confiance et à l’aide des métadonnées de fédération.
  
## <a name="to-create-a-claims-aware-relying-party-trust-manually"></a>Pour créer un revendications confiance prenant en charge les approuver manuellement 

Pour ajouter une nouvelle partie de confiance à l’aide du composant logiciel enfichable Gestion AD FS\-manuellement et à configurer les paramètres, effectuez la procédure suivante sur un serveur de fédération.  

Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).
  
1. Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion AD FS**.  
  
2.  Sous **Actions**, cliquez sur **ajouter Relying Party Trust**.  
![partie de confiance](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  Sur le **Bienvenue** page, choisissez **prenant en charge des revendications** et cliquez sur **Démarrer**.  
![partie de confiance](media/Create-a-Relying-Party-Trust/addtrust2.PNG) 
  
4.  Dans la page **Sélectionner une source de données**, cliquez sur **Entrer manuellement les données concernant la partie de confiance**, puis sur **Suivant**.  
![partie de confiance](media/Create-a-Relying-Party-Trust/addtrust3.PNG) 
  
5.  Sur le **entrer le nom complet** page, tapez un nom dans **nom d’affichage**, sous **Notes** une description pour cette partie de confiance, puis tapez **suivant** .  
![partie de confiance](media/Create-a-Relying-Party-Trust/addtrust4.PNG) 

6. Sur le **configurer le certificat** page, si vous avez un certificat de chiffrement de jeton facultatif, cliquez sur **Parcourir** à localiser un fichier de certificat, puis cliquez sur **suivant**.  
![partie de confiance](media/Create-a-Relying-Party-Trust/addtrust5.PNG) 

7.  Sur le **configurer l’URL** , effectuez l’une des opérations suivantes, cliquez sur **suivant**, puis passez à l’étape 8 :  
  
    -   Sélectionnez le **activer la prise en charge de WS\-protocole Federation passif** case à cocher. Sous **partie de confiance tiers WS\-URL du protocole passif de fédération**, tapez l’URL pour cette partie de confiance, puis cliquez sur **suivant**.  
  
    -   Activez la case à cocher **Activer la prise en charge du protocole WebSSO SAML 2.0**. Sous **service URL SSO SAML 2.0 partie de confiance**, tapez le Security Assertion Markup Language \(SAML\) URL de point de terminaison pour cette partie de confiance du service, puis cliquez sur **suivant**.  
![partie de confiance](media/Create-a-Relying-Party-Trust/addtrust6.PNG)   

8. Dans la page **Configurer les identificateurs**, spécifiez un ou plusieurs identificateurs pour cette partie de confiance, cliquez sur **Ajouter** pour les ajouter à la liste, puis cliquez sur **Suivant**.  
![partie de confiance](media/Create-a-Relying-Party-Trust/addtrust8.PNG)
  
9.  Sur le **choisir la stratégie de contrôle d’accès** sélectionnez une stratégie, puis cliquez sur **suivant**.  Pour plus d’informations sur les stratégies de contrôle d’accès, consultez [stratégies de contrôle d’accès dans AD FS](Access-Control-Policies-in-AD-FS.md). 
![partie de confiance](media/Create-a-Relying-Party-Trust/addtrust9.PNG)

10. Dans la page **Prêt à ajouter l'approbation**, vérifiez les paramètres, puis cliquez sur **Suivant** pour enregistrer les informations de la nouvelle approbation de partie de confiance.  
   ![partie de confiance](media/Create-a-Relying-Party-Trust/addtrust10.PNG) 
11. Dans la page **Terminer** , cliquez sur **Fermer**. Cette action affiche automatiquement la boîte de dialogue **Modifier les règles de revendication**.  
![partie de confiance](media/Create-a-Relying-Party-Trust/addtrust11.PNG) 

## <a name="to-create-a-claims-aware-relying-party-trust-using-federation-metadata"></a>Pour créer un revendications confiance prenant en charge les faire confiance à l’aide des métadonnées de fédération

Pour ajouter une nouvelle partie de confiance, à l’aide du composant logiciel enfichable Gestion AD FS, en important automatiquement les données de configuration relatives au partenaire à partir des métadonnées de fédération du partenaire a publiées sur un réseau local ou à Internet, effectuez la procédure suivante sur un serveur de fédération dans l’organisation partenaire de compte.

>[!NOTE]
>Même si elle a longtemps été courant d’utiliser des certificats avec des noms d’hôte non qualifié tel que https://myserver, ces certificats n’ont aucune valeur de sécurité et peut permettre à un pirate d’emprunter l’identité d’un Service de fédération qui publie des métadonnées de fédération. Par conséquent, lors de l’interrogation des métadonnées de fédération, vous devez uniquement utiliser un nom de domaine complet comme https://myserver.contoso.com.

Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).


1. Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion AD FS**.  
  
2. Sous **Actions**, cliquez sur **ajouter Relying Party Trust**.  
   ![partie de confiance](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3. Sur le **Bienvenue** page, choisissez **prenant en charge des revendications** et cliquez sur **Démarrer**.  
   ![partie de confiance](media/Create-a-Relying-Party-Trust/addtrust2.PNG) 
  
4. Sur le **sélectionner une Source de données** , cliquez sur <strong>importer les données sur la partie de confiance publiées en ligne ou sur un réseau local *. Dans ** adresse de métadonnées de fédération (nom d’hôte ou URL)</strong>, tapez la fédération métadonnées URL ou nom d’hôte pour le partenaire, puis cliquez sur **suivant**.  
   ![partie de confiance](media/Create-a-Relying-Party-Trust/addtrust12.PNG) 

5. Dans la page spécifier le nom complet, tapez un nom dans **surnom**, sous Notes tapez une description pour cette partie de confiance, puis cliquez sur **suivant**.

6. Dans la page Choisir les règles d’autorisation d’émission, sélectionnez **autoriser tous les utilisateurs à accéder à cette confiance** ou **refuser tous les utilisateurs l’accès à cette partie de confiance**, puis cliquez sur **suivant**.

7. Sur la page Prêt pour ajouter l’approbation, passez en revue les paramètres, puis cliquez sur **suivant** pour enregistrer votre partie de confiance les informations de confidentialité.

8. Dans la page Terminer, cliquez sur **fermer**. Cette action affiche automatiquement la boîte de dialogue Modifier les règles de revendication. Pour plus d'informations sur l'ajout de règles de revendication pour cette approbation de partie de confiance, voir Références supplémentaires.




## <a name="see-also"></a>Voir aussi  
[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
