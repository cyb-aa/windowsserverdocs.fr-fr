---
ms.assetid: a4f7842c-cfca-4d78-916e-023d12a9cdf0
title: Créer une approbation de fournisseur de revendications
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 909be9e4bbcd12b00fd60ff061b1f4e2ede34546
ms.sourcegitcommit: 8eea7aadbe94f5d4635c4ffedc6a831558733cc0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66308540"
---
# <a name="create-a-claims-provider-trust"></a>Créer une approbation de fournisseur de revendications

Pour ajouter une nouvelle approbation de fournisseur de revendications à l’aide du composant logiciel enfichable Gestion AD FS\-manuellement et à configurer les paramètres, effectuez la procédure suivante sur un serveur de fédération du partenaire ressource dans l’organisation partenaire ressource.  
  
L’appartenance au **administrateurs**, ou équivalente, sur l’ordinateur local est la configuration minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
## <a name="to-create-a-claims-provider-trust-manually"></a>Pour créer une approbation de fournisseur de revendications manuellement  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion AD FS**.  
  
2.  Sous **Actions**, cliquez sur **ajouter une approbation de fournisseur de revendications**.  
![Approbation de fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim1.PNG)   
  
3.  Dans la page **Bienvenue** , cliquez sur **Démarrer**. 
![Approbation de fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim2.PNG)    
  
4.  Dans la page **Sélectionner une source de données**, cliquez sur **Entrer les données d’approbation de fournisseur de revendications manuellement**, puis sur **Suivant**.  
![Approbation de fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim3.PNG)     

5.  Dans la page **Entrer le nom complet**, tapez un **Nom complet**, sous **Remarques**, tapez une description pour cette approbation de fournisseur de revendications, puis cliquez sur **Suivant**.  
![Approbation de fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim4.PNG)     

6.  Sur le **configurer l’URL** , spécifiez la **URL Passive WS-Federation** si applicable et cliquez sur **suivant**.
![Approbation de fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim5.PNG)     

8. Dans la page **Configurer l’identificateur**, sous **Identificateur d’approbation de fournisseur de revendications**, tapez l’identificateur approprié, puis cliquez sur **Suivant**.  
![Approbation de fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim6.PNG)    

9. Dans la page **Configurer les certificats**, cliquez sur **Ajouter** pour rechercher un fichier de certificat et l’ajouter à la liste des certificats, puis cliquez sur **Suivant**.  
![Approbation de fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim7.PNG)    

10. Dans la page **Prêt à ajouter l’approbation**, cliquez sur **Suivant** pour enregistrer les informations de votre approbation de fournisseur de revendications.  
![Approbation de fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim8.PNG)    

11. Dans la page **Terminer** , cliquez sur **Fermer**. Cette action affiche automatiquement la boîte de dialogue **Modifier les règles de revendication**. Pour plus d’informations sur l’ajout de règles pour cette approbation de fournisseur de revendications de revendication, consultez les références supplémentaires suivantes.  
![Approbation de fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim9.PNG)

## <a name="to-create-a-claims-provider-trust-using-federation-metadata"></a>Pour créer une approbation de fournisseur de revendications à l’aide des métadonnées de fédération
Pour ajouter une nouvelle approbation de fournisseur de revendications, le composant logiciel enfichable Gestion AD FS, en important automatiquement les données de configuration relatives au partenaire à partir des métadonnées de fédération du partenaire a publiées sur un réseau local ou à Internet, d’effectuer la procédure suivante sur un serveur de fédération dans l’organisation partenaire ressource.

>[!NOTE]
>Même si elle a longtemps été courant d’utiliser des certificats avec des noms d’hôte non qualifié tel que https :\//myserver, ces certificats n’ont aucune valeur de sécurité et peut permettre à un pirate d’emprunter l’identité d’un Service de fédération qui publie la fédération métadonnées. Par conséquent, lors de l’interrogation des métadonnées de fédération, vous devez uniquement utiliser un nom de domaine complet comme `https://myserver.contoso.com`.

1.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis sélectionnez **gestion AD FS**.  
  
2.  Sous **Actions**, cliquez sur **ajouter une approbation de fournisseur de revendications**.  
![Approbation de fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim1.PNG)   
  
3.  Dans la page **Bienvenue** , cliquez sur **Démarrer**. 
![Approbation de fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim2.PNG)    
  
4.  Dans la page **Sélectionner une source de données**, cliquez sur **Importer les données publiées en ligne ou sur un réseau local concernant le fournisseur de revendications**. Dans la fédération adresse des métadonnées (nom d’hôte ou URL), tapez le **URL des métadonnées de fédération** ou hôte de noms pour le partenaire, puis cliquez sur **suivant**.
![Approbation de fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim10.PNG)    

5.  Type de page sur le nom d’affichage spécifier un **nom d’affichage**, sous Notes, tapez une description pour cette approbation de fournisseur de revendications, puis cliquez sur **suivant**.

6.  Dans la page Prêt pour ajouter l’approbation, cliquez sur **suivant** pour enregistrer vos revendications informations d’approbation de fournisseur.

7.  Dans la page Terminer, cliquez sur **fermer**. Cela s’affiche automatiquement la boîte de dialogue Modifier les règles de revendication. Pour plus d’informations sur l’ajout de règles pour cette approbation de fournisseur de revendications de revendication, consultez la section Références supplémentaires ci-dessous.



    
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : Configuration de l’organisation partenaire de ressource](../../ad-fs/deployment/Checklist--Configuring-the-Resource-Partner-Organization.md)  
  
[Liste de vérification : création de règles de revendication pour une approbation de fournisseur de revendications](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Claims-Provider-Trust.md)  
  
## <a name="see-also"></a>Voir aussi  
[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
  
