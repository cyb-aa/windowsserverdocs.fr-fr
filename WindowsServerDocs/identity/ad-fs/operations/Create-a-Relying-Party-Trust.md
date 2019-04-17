---
ms.assetid: 5b9fc9c1-5d12-4ad4-8ddc-3b8a6d45b217
title: "Créer une partie de confiance"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 14e1cc732ed60b7f05a9a4a9aac9037c48b702f2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-relying-party-trust"></a>Créer une partie de confiance

>S’applique à: Windows Server2016, Windows Server2012R2

Le document suivant fournit des informations sur la création manuelle d’une relation de confiance et à l’aide des métadonnées de fédération.
  
## <a name="to-create-a-claims-aware-relying-party-trust-manually"></a>Pour créer un revendications tiers de partie de confiance prenant en charge approuver manuellement 

Pour ajouter une nouvelle approbation de partie de confiance à l’aide de la gestion ADFS enfichable et configurer manuellement les paramètres, effectuez la procédure suivante sur un serveur de fédération.  

L’appartenance au groupe **administrateurs**, ou équivalente, sur l’ordinateur local est la condition minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).
  
1. Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion ADFS**.  
  
2.  Sous **Actions**, cliquez sur **ajouter une approbation de partie de confiance **.  
![Partie de confiance](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  Sur le **Bienvenue** page, choisissez **prenant en charge des revendications** et cliquez sur **Démarrer **.  
![Partie de confiance](media/Create-a-Relying-Party-Trust/addtrust2.PNG) 
  
4.  Sur le **sélectionner une Source de données**, cliquez sur **entrer manuellement les données sur la partie de confiance**, puis cliquez sur **suivant **.  
![Partie de confiance](media/Create-a-Relying-Party-Trust/addtrust3.PNG) 
  
5.  Sur le **entrer le nom complet**, tapez un nom dans **nom d’affichage**, sous **Notes** tapez une description pour cette approbation de partie de confiance, puis cliquez sur **suivant **.  
![Partie de confiance](media/Create-a-Relying-Party-Trust/addtrust4.PNG) 

6. Sur le **configurer le certificat** page, si vous disposez d’un certificat de chiffrement de jetons facultatif, cliquez sur **Parcourir** pour rechercher un fichier de certificat, puis cliquez sur **suivant **.  
![Partie de confiance](media/Create-a-Relying-Party-Trust/addtrust5.PNG) 

7.  Sur le **configurer l’URL**, effectuez au moins des opérations suivantes, cliquez sur **suivant**, puis passez à l’étape8:  
  
    -   Sélectionnez le **activer la prise en charge du protocole WS-Federation passif** case à cocher. Sous **URL protocole WS-Federation passif partie de confiance**, tapez l’URL pour cette approbation de partie de confiance, puis cliquez sur **suivant**.  
  
    -   Sélectionnez le **activer la prise en charge du protocole WebSSO SAML 2.0** case à cocher. Sous **URL partie de confiance service SSO SAML 2.0**, tapez l’URL de point de terminaison du service \(SAML\) Security Assertion Markup Language pour cette approbation de partie de confiance, puis cliquez sur **suivant**.  
![Partie de confiance](media/Create-a-Relying-Party-Trust/addtrust6.PNG)   

8. Sur le **configurer les identificateurs**, spécifiez un ou plusieurs identificateurs pour cette partie de confiance, cliquez sur **ajouter** pour les ajouter à la liste, puis cliquez sur **suivant**.  
![Partie de confiance](media/Create-a-Relying-Party-Trust/addtrust8.PNG)
  
9.  Sur le **stratégie de contrôle d’accès choisir** sélectionnez une stratégie et cliquez sur **suivant**.  Pour plus d’informations sur les stratégies de contrôle d’accès, voir [stratégies de contrôle d’accès dans ADFS](Access-Control-Policies-in-AD-FS.md). 
![Partie de confiance](media/Create-a-Relying-Party-Trust/addtrust9.PNG)

10. Sur le **prêt à ajouter l’approbation** page, passez en revue les paramètres, puis cliquez sur **suivant** pour enregistrer votre confiance les informations de confidentialité.  
   ![Partie de confiance](media/Create-a-Relying-Party-Trust/addtrust10.PNG) 
11. Sur le **Terminer**, cliquez sur **fermer**. Cette action affiche automatiquement les **modifier les règles de revendication** boîte de dialogue.  
![Partie de confiance](media/Create-a-Relying-Party-Trust/addtrust11.PNG) 

## <a name="to-create-a-claims-aware-relying-party-trust-using-federation-metadata"></a>Pour créer un revendications tiers de partie de confiance prenant en charge faire confiance à l’aide des métadonnées de fédération

Pour ajouter une nouvelle partie de confiance, à l’aide du composant logiciel enfichable Gestion ADFS, en important automatiquement les données de configuration relatives au partenaire à partir des métadonnées de fédération que le partenaire a publiées sur un réseau local ou à Internet, effectuez la procédure suivante sur un serveur de fédération dans l’organisation partenaire de compte.

>[!NOTE]
>Bien qu’a longtemps été une pratique courante consiste à utiliser des certificats avec des noms d’hôte non qualifiés comme https://myserver, ces certificats n’ont aucune valeur de sécurité et peuvent permettre à un attaquant d’emprunter l’identité d’un Service de fédération qui publie des métadonnées de fédération. Par conséquent, lors de l’interrogation des métadonnées de fédération, vous devez utiliser uniquement un nom de domaine complet comme https://myserver.contoso.com.

L’appartenance au groupe **administrateurs**, ou équivalente, sur l’ordinateur local est la condition minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).


1. Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion ADFS**.  
  
2.  Sous **Actions**, cliquez sur **ajouter une approbation de partie de confiance **.  
![Partie de confiance](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  Sur le **Bienvenue** page, choisissez **prenant en charge des revendications** et cliquez sur **Démarrer **.  
![Partie de confiance](media/Create-a-Relying-Party-Trust/addtrust2.PNG) 
  
4.  Sur le **sélectionner une Source de données**, cliquez sur **importer les données sur la partie de confiance publiées en ligne ou sur un réseau local*. Dans **adresse des métadonnées de fédération (nom d’hôte ou URL)**, tapez la fédération métadonnées URL ou nom d’hôte du partenaire, puis cliquez sur **suivant**.  
![Partie de confiance](media/Create-a-Relying-Party-Trust/addtrust12.PNG) 

5.  Sur la page spécifier le nom complet, tapez un nom dans **nom d’affichage**, sous Notes tapez une description pour cette approbation de partie de confiance, puis cliquez sur **suivant**.

6.  Sur la page Choisir les règles d’autorisation d’émission, sélectionnez **autoriser tous les utilisateurs à accéder à cette partie de confiance** ou **refuser à tous les utilisateurs l’accès à cette partie de confiance**, puis cliquez sur **suivant**.

7.  Sur la page prêt à ajouter l’approbation, passez en revue les paramètres, puis cliquez sur **suivant** pour enregistrer votre confiance les informations de confidentialité.

8.  Sur la page Terminer, cliquez sur **fermer**. Cette action affiche automatiquement la boîte de dialogue Modifier les règles de revendication. Pour plus d’informations sur l’ajout de règles de revendication pour cette approbation de partie de confiance, voir Références supplémentaires.




## <a name="see-also"></a>Voir aussi  
[Opérations ADFS](../../ad-fs/AD-FS-2016-Operations.md) 
