---
title: "Mises à jour des composants des Services de domaine ActiveDirectory"
description: "Ce document décrit les mises à jour du composant de domaine ActiveDirectory pour Windows Server2012R2"
author: billmath
ms.author: billmath
manager: femila
ms.date: 09/08/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: a3a91034-a4da-4ad7-93f8-0cd2ec3e7824
ms.technology: identity-adds
ms.openlocfilehash: 52d3dab542b4250670067e927f42ddf1597fc852
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="active-directory-domain-services-component-updates"></a>Mises à jour des composants des Services de domaine ActiveDirectory

>S’applique à: Windows Server2016, Windows Server2012R2

Ce module présente les composants qui ont des mises à jour mineures dans les Services d’annuaire et les espaces de l’identité.  
  
|À propos de l’auteur|  
|--------------------|  
|**Auteur:**|Justin Turner|  
|**BIO:**|Justin est un ingénieur Support résolution Senior avec l’équipe des Services d’annuaire basée à Irving, Texas, États-Unis.  Il a créé ou contribué à de nombreux cours de formation et articles de la base de connaissances pour la Knowledgebase Microsoft au cours des 12dernières années. Il enseigne employés de Microsoft et de la nouvelle architecture de produits clients est enseigne MicrosoftCertified Master (MCM), MicrosoftCertified Trainer (MCT) et contient une maîtrise Degré dans les systèmes cognitifs et ordinateur.|  
|**Contributeurs**|Ce module de formation inclut des contributions de *Michiko Short*, *Dean Wells*, *Alan Jowett*, *Manu Pushpendran*, *Yashar Bahman*, *Anoosh Saboori*, *Rashmi Jha*, *Justin Hall* et *Herbert Mauerer*|  
|**Réviseurs**|Un grand merci aux personnes suivantes qui ont leur propre temps de réviser et de commentaires: *Joey Seifert*, *Justin Hall*|  
  
> [!NOTE]  
> Ce contenu est écrit par un ingénieur du support client Microsoft et est destiné aux administrateurs expérimentés et architectes système qui recherchent des explications techniques plus approfondies des fonctionnalités et des solutions Windows Server2012R2 que proposent généralement les rubriques sur TechNet. Toutefois, il n’a pas subi les mêmes passes, afin de la partie du langage peut sembler que moins finalisée que ce qui se trouvent généralement sur TechNet.  
  
### <a name="what-you-will-learn"></a>Ce que vous allez apprendre  
Après la fin de ce module, vous serez en mesure de:  
  
-   Expliquez les mises à jour des composants effectuées dans les domaines technologiques Services d’annuaire et identité dans Windows Server2012R2  
  
    -   [Unicité des noms SPN et UPN](../../../ad-ds/manage/component-updates/SPN-and-UPN-uniqueness.md)  
  
    -   [Connexion redémarrage automatique Winlogon sur et #40; aRSO et #41;](../../../ad-ds/manage/component-updates/Winlogon-Automatic-Restart-Sign-On--ARSO-.md)  
  
    -   [Attestation de clé TPM](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md)  
  
    -   [Applets de commande sauvegarde de l’autorité de certification et de restauration Windows PowerShell](../../../ad-ds/manage/component-updates/CA-Backup-and-Restore-Windows-PowerShell-cmdlets.md)  
  
    -   [Audit des processus de ligne de commande](../../../ad-ds/manage/component-updates/Command-line-process-auditing.md)  
  
    -   [Gestion et Protection des informations d’identification](https://technet.microsoft.com/library/dn408190.aspx)  
  
    -   [Mises à jour du composant Directory Services](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md)  
  
        -   [Niveaux fonctionnels de domaine et de forêt](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_FL)  
  
        -   [Désapprobation de NTFRS](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_NTFRS)  
  
        -   [Modifications de l’optimiseur de requête LDAP](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_LDAPQuery)  
  
        -   [Événement1644](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_1644)  
  
        -   [Amélioration du débit de réplication ActiveDirectory](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_ADRepl)  
  


