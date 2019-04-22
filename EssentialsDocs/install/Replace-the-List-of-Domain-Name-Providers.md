---
title: Remplacement de la liste des fournisseurs de noms de domaine
description: Décrit comment utiliser Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820020"
---
# <a name="replace-the-list-of-domain-name-providers"></a>Remplacement de la liste des fournisseurs de noms de domaine

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Vous pouvez remplacer la liste des fournisseurs de noms de domaine affichée dans l’Assistant Configuration du nom de domaine en effectuant les tâches suivantes :  
  

-   [Créer des fichiers de service de la référence](Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)  
  
-   [Ajouter une entrée au Registre sur l’ordinateur de référence](Replace-the-List-of-Domain-Name-Providers.md#BKMK_AddRegistry)  

-   [Créer des fichiers de service de la référence](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)  
  
-   [Ajouter une entrée au Registre sur l’ordinateur de référence](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_AddRegistry)  

  
###  <a name="BKMK_ReferralFiles"></a> Créer des fichiers de service de la référence  
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
  
###  <a name="BKMK_AddRegistry"></a> Ajouter une entrée au Registre sur l’ordinateur de référence  
 Il est indispensable d’ajouter une entrée au Registre afin d’indiquer à quel endroit le système d’exploitation doit rechercher les fichiers du service de référence.  
  
##### <a name="to-add-a-key-to-the-registry"></a>Pour ajouter une clé au Registre  
  
1.  Sur l'ordinateur de référence, cliquez sur **Démarrer**, entrez **regedit**, puis appuyez sur **Entrée**.  
  
2.  Dans le volet gauche, développez successivement les entrées **HKEY_LOCAL_MACHINE**, **SOFTWARE**, **Microsoft**, **Windows Server**, **Domain Managers**et **Providers**.  
  
3.  Cliquez avec le bouton droit sur la clé **E423C85D-6B1F-4583-95E0-449D8263BAC4** , puis cliquez sur **Nom de la valeur**.  
  
4.  Tapez **ReferralServerHttpsUri** pour le nom de la chaîne, puis appuyez sur **Entrée**.  
  
5.  Cliquez avec le bouton droit sur la nouvelle chaîne **ReferralServerHttpsUri** dans le volet droit, puis cliquez sur **Modifier**.  
  

6.  Entrez l’adresse URL HTTPS permettant d’accéder aux fichiers de référence générés à l’étape[Création des fichiers du service de référence](Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles), puis cliquez sur **OK**.  

6.  Entrez l’adresse URL HTTPS permettant d’accéder aux fichiers de référence générés à l’étape[Création des fichiers du service de référence](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles), puis cliquez sur **OK**.  

  
    > [!IMPORTANT]
    >  N’oubliez pas la barre oblique (/) à la fin de l’adresse URL.  
  
###  <a name="BKMK_ReplaceDomainNameProviders"></a> Problèmes d’état de nom de domaine  
 Si un partenaire ajoute des fournisseurs de nom de domaine et utilise une interface de programmation d’applications (API) dans le Kit de développement logiciel de Windows Server Essentials pour définir les États Unknown, Failed et CertificateRequestNotSubmitted du certificat, le client reçoit un incorrect résultat de message et de configuration. Ces cas sont gérés, en effet, par des exceptions et ne donnent pas lieu à un renvoi d’état.  
  
 Les états de domaine suivants font figure d’échecs et doivent être signalés comme tels :  
  
-   Failed  
  
-   PendingCustomerInterventionRequired  
  
-   PurchaseFailed  
  
-   DomainNotFound  
  
-   InRenewalCustomerInterventionRequired  
  
-   RenewalFailed  
  
 Les états de domaine suivants font figure de réussites et doivent être signalés comme tels :  
  
-   Prêt  
  
-   Pending  
  
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

