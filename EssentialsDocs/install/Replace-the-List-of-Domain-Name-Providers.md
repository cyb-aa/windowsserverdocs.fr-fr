---
title: Remplacement de la liste des fournisseurs de noms de domaine
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 104d0412-2d77-4cd4-99f7-65a885522850
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: adbadcd08bb6867cbc7f1da8b08e01250f5186a6
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311538"
---
# <a name="replace-the-list-of-domain-name-providers"></a>Remplacement de la liste des fournisseurs de noms de domaine

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Vous pouvez remplacer la liste des fournisseurs de noms de domaine affichée dans l’Assistant Configuration du nom de domaine en effectuant les tâches suivantes :  


-   [Créer les fichiers du service de référence](Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)  

-   [Ajouter une entrée au registre sur l’ordinateur de référence](Replace-the-List-of-Domain-Name-Providers.md#BKMK_AddRegistry)  

-   [Créer les fichiers du service de référence](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)  

-   [Ajouter une entrée au registre sur l’ordinateur de référence](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_AddRegistry)  


###  <a name="create-the-referral-service-files"></a><a name="BKMK_ReferralFiles"></a>Créer les fichiers du service de référence  
 L’outil Administration du service de référence génère un jeu de fichiers servant à définir la liste des fournisseurs de noms de domaine présentée dans l’Assistant Configuration du nom de domaine. À chaque région du globe correspond un fichier au format XML contenant les informations spécifiques aux fournisseurs de noms de domaine spécifiés dans l’outil. Les fichiers créés par l’outil doivent figurer dans un dossier accessible via une liaison sécurisée (HTTPS) que vous gérez sur Internet.  

##### <a name="to-create-the-referral-files"></a>Pour créer les fichiers de référence  

1.  Ouvrez l’outil Administration du service de référence.  

2.  Cliquez sur **Ajouter**.  

3.  Dans la boîte de dialogue Ajouter un fournisseur de noms de domaine, indiquez le nom du fournisseur de noms de domaine.  

4.  Ajoutez les domaines de niveau supérieur pris en charge par le fournisseur de noms de domaine. Il suffit pour cela de cliquer sur **Ajouter**, d’entrer l’identificateur de domaine de niveau supérieur, puis de sélectionner les régions compatibles. Vous pouvez sélectionner **Toutes les régions**.  

5.  Entrez la description du fournisseur de noms de domaine.  

6.  Ajoutez les adresses URL de tous les sites Web associés au fournisseur de noms de domaine.  

7.  Si un logo est disponible pour le fournisseur de noms de domaine, ajoutez-le en cliquant sur **Modifier le logo**.  

8.  Cliquez sur **Enregistrer**.  

9. Recommencez les étapes 2 à 8 pour chaque fournisseur de noms de domaine que vous comptez répertorier dans l’assistant.  

10. Lorsque vous avez terminé, choisissez le dossier réservé aux fichiers de référence. Rappelez-vous que ces fichiers doivent être accessibles via une liaison HTTPS. Le choix du dossier est donc primordial.  

11. Cliquez sur **Générer des fichiers dans le système de fichiers**.  

###  <a name="add-an-entry-to-the-registry-on-the-reference-computer"></a><a name="BKMK_AddRegistry"></a>Ajouter une entrée au registre sur l’ordinateur de référence  
 Il est indispensable d’ajouter une entrée au Registre afin d’indiquer à quel endroit le système d’exploitation doit rechercher les fichiers du service de référence.  

##### <a name="to-add-a-key-to-the-registry"></a>Pour ajouter une clé au Registre  

1.  Sur l'ordinateur de référence, cliquez sur **Démarrer**, entrez **regedit**, puis appuyez sur **Entrée**.  

2.  Dans le volet gauche, développez successivement les entrées **HKEY_LOCAL_MACHINE**, **SOFTWARE**, **Microsoft**, **Windows Server**, **Domain Managers** et **Providers**.  

3.  Cliquez avec le bouton droit sur la clé **E423C85D-6B1F-4583-95E0-449D8263BAC4**, puis cliquez sur **Nom de la valeur**.  

4.  Tapez **ReferralServerHttpsUri** pour le nom de la chaîne, puis appuyez sur **Entrée**.  

5.  Cliquez avec le bouton droit sur la nouvelle chaîne **ReferralServerHttpsUri** dans le volet droit, puis cliquez sur **Modifier**.  


6.  Entrez l’adresse URL HTTPS permettant d’accéder aux fichiers de référence générés à l’étape[Création des fichiers du service de référence](Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles), puis cliquez sur **OK**.  

6.  Entrez l’adresse URL HTTPS permettant d’accéder aux fichiers de référence générés à l’étape[Création des fichiers du service de référence](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles), puis cliquez sur **OK**.  


~~~
> [!IMPORTANT]
>  A slash (/) is required at the end of the URL.  
~~~

###  <a name="domain-name-status-issues"></a><a name="BKMK_ReplaceDomainNameProviders"></a>Problèmes d’état de nom de domaine  
 Si un partenaire ajoute des fournisseurs de noms de domaine et utilise une interface de programmation d’applications (API) dans le kit de développement logiciel (SDK) Windows Server Essentials pour définir les États Unknown, failed et CertificateRequestNotSubmitted pour le certificat, le client reçoit une erreur résultat du message et de la configuration. Ces cas sont gérés, en effet, par des exceptions et ne donnent pas lieu à un renvoi d’état.  

 Les états de domaine suivants font figure d’échecs et doivent être signalés comme tels :  

- Échec  

- PendingCustomerInterventionRequired  

- PurchaseFailed  

- DomainNotFound  

- InRenewalCustomerInterventionRequired  

- RenewalFailed  

  Les états de domaine suivants font figure de réussites et doivent être signalés comme tels :  

- Prêt  

- En attente  

- InRenewal  

## <a name="see-also"></a>Voir aussi  

 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience utilisateur](Testing-the-Customer-Experience.md)

 [Création et personnalisation de l’Image](../install/Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](../install/Additional-Customizations.md)   
 [Préparation de l’image pour le déploiement](../install/Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience utilisateur](../install/Testing-the-Customer-Experience.md)

