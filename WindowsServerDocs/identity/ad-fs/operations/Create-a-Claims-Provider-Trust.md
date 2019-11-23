---
ms.assetid: a4f7842c-cfca-4d78-916e-023d12a9cdf0
title: Créer une approbation de fournisseur de revendications
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 4539e8abd1af1eca7bacb51971e6d355bb0aab28
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407658"
---
# <a name="create-a-claims-provider-trust"></a>Créer une approbation de fournisseur de revendications

Pour ajouter une nouvelle approbation de fournisseur de revendications à l’aide du composant logiciel enfichable Gestion des AD FS\-dans et configurer manuellement les paramètres, effectuez la procédure suivante sur un serveur de Fédération du partenaire de ressource dans l’organisation partenaire de ressource.  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **administrateurs**ou à un groupe équivalent sur l’ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
## <a name="to-create-a-claims-provider-trust-manually"></a>Pour créer une approbation de fournisseur de revendications manuellement  
  
1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  
  
2.  Sous **actions**, cliquez sur **Ajouter une approbation de fournisseur de revendications**.  
![l’approbation du fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim1.PNG)   
  
3.  Dans la page **Bienvenue** , cliquez sur **Démarrer**. 
![l’approbation du fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim2.PNG)    
  
4.  Dans la page **Sélectionner une source de données**, cliquez sur **Entrer les données d’approbation de fournisseur de revendications manuellement**, puis sur **Suivant**.  
![l’approbation du fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim3.PNG)     

5.  Dans la page **Entrer le nom complet**, tapez un **Nom complet**, sous **Remarques**, tapez une description pour cette approbation de fournisseur de revendications, puis cliquez sur **Suivant**.  
![l’approbation du fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim4.PNG)     

6.  Dans la page **configurer l’URL** , spécifiez l' **URL passive WS-Federation** le cas échéant, puis cliquez sur **suivant**.
![l’approbation du fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim5.PNG)     

8. Dans la page **Configurer l’identificateur**, sous **Identificateur d’approbation de fournisseur de revendications**, tapez l’identificateur approprié, puis cliquez sur **Suivant**.  
![l’approbation du fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim6.PNG)    

9. Dans la page **Configurer les certificats**, cliquez sur **Ajouter** pour rechercher un fichier de certificat et l’ajouter à la liste des certificats, puis cliquez sur **Suivant**.  
![l’approbation du fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim7.PNG)    

10. Dans la page **Prêt à ajouter l’approbation**, cliquez sur **Suivant** pour enregistrer les informations de votre approbation de fournisseur de revendications.  
![l’approbation du fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim8.PNG)    

11. Dans la page **Terminer** , cliquez sur **Fermer**. Cette action affiche automatiquement la boîte de dialogue **Modifier les règles de revendication**. Pour plus d’informations sur la façon de procéder à l’ajout de règles de revendication pour cette approbation de fournisseur de revendications, consultez les références supplémentaires suivantes.  
![l’approbation du fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim9.PNG)

## <a name="to-create-a-claims-provider-trust-using-federation-metadata"></a>Pour créer une approbation de fournisseur de revendications à l’aide des métadonnées de Fédération
Pour ajouter une nouvelle approbation de fournisseur de revendications à l’aide du composant logiciel enfichable de gestion AD FS, en important automatiquement les données de configuration relatives au partenaire à partir des métadonnées de Fédération que le partenaire a publiées sur un réseau local ou sur Internet, effectuez la procédure suivante sur un serveur de Fédération dans l’organisation du partenaire de ressource.

>[!NOTE]
>Bien qu’il s’agisse d’une pratique courante d’utiliser des certificats avec des noms d’hôte non qualifiés comme https :\//MyServer, ces certificats n’ont aucune valeur de sécurité et peuvent permettre à une personne malveillante d’emprunter l’identité d’un service FS (Federation Service) qui publie des métadonnées de Fédération. Par conséquent, lors de l’interrogation des métadonnées de Fédération, vous devez uniquement utiliser un nom de domaine complet, tel que `https://myserver.contoso.com`.

1.  Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sélectionnez **gestion des AD FS**.  
  
2.  Sous **actions**, cliquez sur **Ajouter une approbation de fournisseur de revendications**.  
![l’approbation du fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim1.PNG)   
  
3.  Dans la page **Bienvenue** , cliquez sur **Démarrer**. 
![l’approbation du fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim2.PNG)    
  
4.  Dans la page **Sélectionner une source de données**, cliquez sur **Importer les données publiées en ligne ou sur un réseau local concernant le fournisseur de revendications**. Dans adresse des métadonnées de Fédération (nom d’hôte ou URL), tapez l' **URL des métadonnées de Fédération** ou le nom d’hôte du partenaire, puis cliquez sur **suivant**.
![l’approbation du fournisseur de revendications](media/Create-a-Claims-Provider-Trust/addclaim10.PNG)    

5.  Dans la page spécifier le nom complet, tapez un **nom complet**, sous remarques, tapez une description pour cette approbation de fournisseur de revendications, puis cliquez sur **suivant**.

6.  Dans la page prêt à ajouter l’approbation, cliquez sur **suivant** pour enregistrer vos informations d’approbation de fournisseur de revendications.

7.  Dans la page terminer, cliquez sur **Fermer**. La boîte de dialogue Modifier les règles de revendication s’affiche automatiquement. Pour plus d’informations sur la façon de procéder à l’ajout de règles de revendication pour cette approbation de fournisseur de revendications, consultez la section Références supplémentaires ci-dessous.



    
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : configuration de l’organisation partenaire de ressource](../../ad-fs/deployment/Checklist--Configuring-the-Resource-Partner-Organization.md)  
  
[Liste de vérification : création de règles de revendication pour une approbation de fournisseur de revendications](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Claims-Provider-Trust.md)  
  
## <a name="see-also"></a>Voir aussi  
[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
  
