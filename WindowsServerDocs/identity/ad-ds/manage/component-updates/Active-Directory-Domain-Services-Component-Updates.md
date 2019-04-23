---
title: Mises à jour des composants des services de domaine Active Directory
description: Ce document décrit les mises à jour du composant AD DS pour Windows Server 2012 R2
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 09/08/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: a3a91034-a4da-4ad7-93f8-0cd2ec3e7824
ms.technology: identity-adds
ms.openlocfilehash: 300bf7dafe0ef0ede754302f2c0aa8c36a7adbfd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889880"
---
# <a name="active-directory-domain-services-component-updates"></a>Mises à jour des composants des services de domaine Active Directory

>S'applique à : Windows Server 2016, Windows Server 2012 R2

Ce module présente les composants qui ont subi des mises à jour mineures dans les espaces Services d'annuaire et Identité.  
  
|À propos de l'auteur|  
|--------------------|  
|**Auteur :**|Justin Turner|  
|**Biographie :**|Justin est un ingénieur support résolution senior de l'équipe Services d'annuaire basée à Irving, Texas, États-Unis.  Il a créé ou contribué à de nombreux cours de formation et articles de la Base de connaissances Microsoft au cours des 12 dernières années. Il enseigne les employés de Microsoft et de la nouvelle architecture des produits aux clients, est une charte Microsoft Certified Master (MCM), Microsoft Certified Trainer (MCT) et le maintient une maîtrise degré dans les systèmes cognitifs et ordinateur.|  
|**Contributeurs**|Ce module de formation inclut des contributions de *Michiko Short*, *Dean Wells*, *Alan Jowett*, *Manu Pushpendran*, *Yashar Bahman*, *Anoosh Saboori*, *Rashmi Jha*, *Justin Hall* et *Herbert Mauerer*|  
|**Réviseurs**|Un grand merci aux personnes suivantes qui ont pris le temps de réviser et émettre des commentaires : *Joey Seifert* et *Justin Hall*|  
  
> [!NOTE]  
> Ce contenu est écrit par un ingénieur du support client Microsoft et est destiné aux administrateurs expérimentés et aux architectes système qui recherchent des explications techniques plus approfondies des fonctionnalités et des solutions Windows Server 2012 R2 que n'en proposent généralement les rubriques de TechNet. Toutefois, il n'a pas subi les mêmes passes de correction. De ce fait, une partie du langage peut sembler moins finalisée que le contenu de TechNet.  
  
### <a name="what-you-will-learn"></a>Ce que vous allez apprendre  
À la fin de ce module, vous serez en mesure de :  
  
-   Expliquer les mises à jour des composants effectuées dans les domaines technologiques Services d'annuaire et Identité dans Windows Server 2012 R2  
  
    -   [Unicité des noms SPN et UPN](../../../ad-ds/manage/component-updates/SPN-and-UPN-uniqueness.md)  
  
    -   [Redémarrage automatique Sign-On &#40;ARSO&#41;](../../../ad-ds/manage/component-updates/Winlogon-Automatic-Restart-Sign-On--ARSO-.md)  
  
    -   [Attestation de clé TPM](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md)  
  
    -   [Applets de commande de sauvegarde et de restauration de l'autorité de certification Windows PowerShell](../../../ad-ds/manage/component-updates/CA-Backup-and-Restore-Windows-PowerShell-cmdlets.md)  
  
    -   [Audit des processus de ligne de commande](../../../ad-ds/manage/component-updates/Command-line-process-auditing.md)  
  
    -   [Gestion et protection des informations d'identification](https://technet.microsoft.com/library/dn408190.aspx)  
  
    -   [Mises à jour des composants Services d'annuaire](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md)  
  
        -   [Niveaux fonctionnels de domaines et de forêt](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_FL)  
  
        -   [Désapprobation de NTFRS](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_NTFRS)  
  
        -   [Modifications de l'optimiseur de requête LDAP](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_LDAPQuery)  
  
        -   [Améliorations de l'événement 1644](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_1644)  
  
        -   [Amélioration du débit de réplication Active Directory](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_ADRepl)  
  


