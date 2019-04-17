---
title: Remplacement de la liste des fournisseurs de noms de domaine
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 104d0412-2d77-4cd4-99f7-65a885522850
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ebaef0f88456f61fa229c9a18ee8014987fe7fa7
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="replace-the-list-of-domain-name-providers"></a>Remplacement de la liste des fournisseurs de noms de domaine

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Vous pouvez remplacer la liste des fournisseurs de noms de domaine qui s’affiche dans l’Assistant Configuration de nom de domaine en effectuant les tâches suivantes:  
  

-   [Créer des fichiers du service de la référence](Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)  
  
-   [Ajoutez une entrée au Registre sur l’ordinateur de référence](Replace-the-List-of-Domain-Name-Providers.md#BKMK_AddRegistry)  

-   [Créer des fichiers du service de la référence](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)  
  
-   [Ajoutez une entrée au Registre sur l’ordinateur de référence](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_AddRegistry)  

  
###  <a name="BKMK_ReferralFiles"></a>Créer des fichiers du service de la référence  
 L’outil Administration du Service de référence crée un ensemble de fichiers qui sont utilisés pour définir la liste des fournisseurs de noms de domaine qui s’affichent dans l’Assistant Configuration de nom de domaine. Un fichier au format XML est créé pour chaque région dans le monde entier et contient des informations pour les fournisseurs de noms de domaine que vous spécifiez dans l’outil. Les fichiers qui sont créés par l’outil doivent se trouver dans un dossier accessible via un lien sécurisé (HTTPS) que vous gérez sur Internet.  
  
##### <a name="to-create-the-referral-files"></a>Pour créer les fichiers de référence  
  
1.  Ouvrez l’outil d’Administration de Service référence.  
  
2.  Cliquez sur **ajouter**.  
  
3.  Dans l’ajout d’une boîte de dialogue du fournisseur de noms de domaine, entrez le nom du fournisseur de noms de domaine.  
  
4.  Ajoutez les domaines de niveau supérieur sont pris en charge par le fournisseur de noms de domaine. Pour ce faire, vous devez en cliquant sur **ajouter**, la saisie de l’identificateur de domaine de niveau supérieur et en sélectionnant les régions pris en charge. Vous pouvez sélectionner **toutes les régions**.  
  
5.  Entrez la description du fournisseur de noms de domaine.  
  
6.  Ajoutez les URL de tous les sites Web qui sont associés au fournisseur de noms de domaine.  
  
7.  Si un logo est disponible pour le fournisseur de noms de domaine, ajoutez le logo en cliquant sur **modification Logo**.  
  
8.  Cliquez sur **enregistrer**.  
  
9. Répétez les étapes2 à 8 pour chaque fournisseur de noms de domaine que vous souhaitez répertorier dans l’Assistant.  
  
10. Après avoir ajouté tous les fournisseurs de nom de domaine, choisissez le dossier où les fichiers de référence sera situés. Gardez à l’esprit lors du choix d’un dossier que les fichiers de référence doivent être accessible via une liaison HTTPS.  
  
11. Cliquez sur **générer des fichiers système de fichiers**.  
  
###  <a name="BKMK_AddRegistry"></a>Ajoutez une entrée au Registre sur l’ordinateur de référence  
 Une entrée de Registre doit être ajoutée pour spécifier où le système d’exploitation peut trouver la référence les fichiers du service.  
  
##### <a name="to-add-a-key-to-the-registry"></a>Pour ajouter une clé au Registre  
  
1.  Sur l’ordinateur de référence, cliquez sur **Démarrer**, entrez **regedit**, puis appuyez sur **entrée**.  
  
2.  Dans le volet gauche, développez **HKEY_LOCAL_MACHINE**, développez **logiciel**, développez **Microsoft**, développez **Windows Server**, développez **Domain Managers**, puis développez **fournisseurs**.  
  
3.  Avec le bouton droit le **E423C85D-6B1F-4583-95E0-449D8263BAC4** de la clé, puis cliquez sur **valeur de chaîne**.  
  
4.  Type **ReferralServerHttpsUri** pour le nom de la chaîne, puis appuyez sur **entrée**.  
  
5.  Cliquez sur le nouveau **ReferralServerHttpsUri** de chaîne dans le volet droit, puis cliquez sur **modifier**.  
  

6.  Tapez l’adresse URL HTTPS est utilisé pour accéder aux fichiers de référence que vous avez créé dans [créer des fichiers du service de la référence](Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles), puis cliquez sur **OK**.  

6.  Tapez l’adresse URL HTTPS est utilisé pour accéder aux fichiers de référence que vous avez créé dans [créer des fichiers du service de la référence](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles), puis cliquez sur **OK**.  

  
    > [!IMPORTANT]
    >  Une barre oblique (/) est nécessaire à la fin de l’URL.  
  
###  <a name="BKMK_ReplaceDomainNameProviders"></a>Problèmes d’état de nom de domaine  
 Si un partenaire ajoute des fournisseurs de noms de domaine et utilise une interface de programmation d’applications (API) dans le SDK WindowsServerEssentials pour définir les États Unknown, Failed et CertificateRequestNotSubmitted du certificat, le client reçoit un message incorrecte et le résultat de la configuration. Il s’agit dans la mesure où les cas sont gérés par des exceptions au lieu de renvoyer un état.  
  
 Les États de domaine suivants sont des échecs et doivent être signalés comme une erreur:  
  
-   N’a pas pu  
  
-   PendingCustomerInterventionRequired  
  
-   PurchaseFailed  
  
-   DomainNotFound  
  
-   InRenewalCustomerInterventionRequired  
  
-   RenewalFailed  
  
 Les États de domaine suivants sont réussies et doivent être signalés comme une réussite:  
  
-   Prêt  
  
-   En attente  
  
-   InRenewal  
  
## <a name="see-also"></a>Voir aussi  

 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](Testing-the-Customer-Experience.md)

 [Création et personnalisation de l’Image](../install/Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](../install/Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](../install/Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](../install/Testing-the-Customer-Experience.md)

