---
ms.assetid: a4f7842c-cfca-4d78-916e-023d12a9cdf0
title: "Créer une approbation de fournisseur de revendications"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b4dc20713fdd137b019a072037e35e9219e02fa9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-claims-provider-trust"></a>Créer une approbation de fournisseur de revendications

>S’applique à: Windows Server2016, Windows Server2012R2

Pour ajouter une nouvelle approbation de fournisseur de revendications à l’aide de la gestion ADFS enfichable et configurer manuellement les paramètres, effectuez la procédure suivante sur un serveur de fédération de partenaire de ressource dans l’organisation partenaire de ressource.  
  
L’appartenance au groupe **administrateurs**, ou équivalent, sur l’ordinateur local est le minimum requis pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
## <a name="to-create-a-claims-provider-trust-manually"></a>Pour créer une approbation de fournisseur de revendications manuellement  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion ADFS**.  
  
2.  Sous **Actions**, cliquez sur **ajouter une approbation de fournisseur de revendications **.  
![Approbation de fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim1.PNG)   
  
3.  Sur le **Bienvenue**, cliquez sur **Démarrer **. 
![Approbation de fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim2.PNG)    
  
4.  Sur le **sélectionner une Source de données**, cliquez sur **entrée manuellement des données d’approbation de fournisseur de revendications**, puis cliquez sur **suivant **.  
![Approbation de fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim3.PNG)     

5.  Sur le **entrer le nom complet**, tapez un **nom d’affichage**, sous **Notes**, tapez une description pour cette approbation de fournisseur de revendications, puis cliquez sur **suivant **.  
![Approbation de fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim4.PNG)     

6.  Sur le **configurer l’URL**, spécifiez le **URL Passive WS-Federation** si applicable et cliquez sur **suivant **.
![Approbation de fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim5.PNG)     

8. Sur le **configurer l’identificateur** sous **identificateur d’approbation de fournisseur de revendications**, tapez l’identificateur approprié, puis cliquez sur **suivant **.  
![Approbation de fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim6.PNG)    

9. Sur le **configurer les certificats**, cliquez sur **ajouter** pour rechercher un fichier de certificat et l’ajouter à la liste des certificats, puis cliquez sur **suivant **.  
![Approbation de fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim7.PNG)    

10. Sur le **prêt à ajouter l’approbation**, cliquez sur **suivant** pour enregistrer des informations d’approbation de fournisseur de vos demandes.  
![Approbation de fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim8.PNG)    

11. Sur le **Terminer**, cliquez sur **fermer**. Cette action affiche automatiquement les **modifier les règles de revendication** boîte de dialogue. Pour plus d’informations sur l’ajout de règles pour cette approbation de fournisseur de revendications de revendication, consultez les références supplémentaires suivantes.  
![Approbation de fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim9.PNG)

## <a name="to-create-a-claims-provider-trust-using-federation-metadata"></a>Pour créer une approbation de fournisseur de revendications à l’aide des métadonnées de fédération
Pour ajouter une nouvelle approbation de fournisseur de revendications, à l’aide du composant logiciel enfichable Gestion ADFS, en important automatiquement les données de configuration relatives au partenaire à partir des métadonnées de fédération que le partenaire a publiées sur un réseau local ou à Internet, effectuez la procédure suivante sur un serveur de fédération dans l’organisation partenaire de ressource.

>[!NOTE]
>Bien qu’a longtemps été une pratique courante consiste à utiliser des certificats avec des noms d’hôte non qualifiés comme https://myserver, ces certificats n’ont aucune valeur de sécurité et peuvent permettre à un attaquant d’emprunter l’identité d’un Service de fédération qui publie des métadonnées de fédération. Par conséquent, lors de l’interrogation des métadonnées de fédération, vous devez utiliser uniquement un nom de domaine complet comme https://myserver.contoso.com.

1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion ADFS**.  
  
2.  Sous **Actions**, cliquez sur **ajouter une approbation de partie de confiance **.  
![Approbation de fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim1.PNG)   
  
3.  Sur le **Bienvenue**, cliquez sur **Démarrer **. 
![Approbation de fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim2.PNG)    
  
4.  Sur le **sélectionner une Source de données**, cliquez sur **importer les données sur le fournisseur de revendications publiées en ligne ou sur un réseau local **. Dans la fédération adresse des métadonnées (nom d’hôte ou URL), tapez le **URL des métadonnées de fédération** ou l’hôte de nom du partenaire, puis cliquez sur **suivant **.
![Approbation de fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim10.PNG)    

5.  Spécifiez le nom d’affichage page type un **nom d’affichage**, sous Notes, tapez une description pour cette approbation de fournisseur de revendications, puis cliquez sur **suivant **.

6.  Dans la page prêt à ajouter l’approbation, cliquez sur **suivant** pour enregistrer des informations d’approbation de fournisseur de vos demandes.

7.  Sur la page Terminer, cliquez sur **fermer**. Cela affichera automatiquement la boîte de dialogue Modifier les règles de revendication. Pour plus d’informations sur l’ajout de règles pour cette approbation de fournisseur de revendications de revendication, consultez la section Références supplémentaires ci-dessous.



    
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification: Configuration de l’organisation partenaire de ressource](../../ad-fs/deployment/Checklist--Configuring-the-Resource-Partner-Organization.md)  
  
[Liste de vérification: Approuvent créer des règles de revendication pour un fournisseur de revendications](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Claims-Provider-Trust.md)  
  
## <a name="see-also"></a>Voir aussi  
[Opérations ADFS](../../ad-fs/AD-FS-2016-Operations.md) 
  
