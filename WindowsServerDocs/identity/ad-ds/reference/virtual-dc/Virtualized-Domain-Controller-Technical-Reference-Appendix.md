---
ms.assetid: 73a4deba-7da6-4eae-8fdd-2a4d369f9cbb
title: Annexe des informations techniques de référence sur les contrôleurs de domaine virtualisés
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9e3a5cc2c71455bb040f1311bdbfed1ac7e213fb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832230"
---
# <a name="virtualized-domain-controller-technical-reference-appendix"></a>Annexe des informations techniques de référence sur les contrôleurs de domaine virtualisés

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique traite des sujets suivants :  
  
-   [Terminologie](../../../ad-ds/reference/virtual-dc/../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_Terms)  
  
-   [FixVDCPermissions.ps1](../../../ad-ds/reference/virtual-dc/../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_FixPDCPerms)  
  
## <a name="BKMK_Terms"></a>Terminologie  
  
-   **Instantané** -l’état d’un ordinateur virtuel à un moment précis dans le temps. Il est dépendant de la chaîne de prise de captures instantanées, sur le matériel et sur la plateforme de virtualisation.  
  
-   **Clone** : un terminer et de séparer la copie d’une machine virtuelle. Il est dépendant sur le matériel virtuel (hyperviseur).  
  
-   **Total Clone** -un clone complet est une copie indépendante d’une machine virtuelle qui ne partage aucune ressource avec la machine virtuelle parent après l’opération de clonage. Opération en cours d’un clone complet est totalement distincte à partir de la machine virtuelle parente.  
  
-   **Disque de différenciation** -une copie d’une machine virtuelle qui partage des disques virtuels avec la machine virtuelle parente de manière continue. Généralement, cela permet d’économiser de l’espace disque et permet à plusieurs machines virtuelles à utiliser la même installation de logiciel.  
  
-   **Copie de la machine virtuelle**- une copie des fichiers système de tous les fichiers connexes et les dossiers d’un ordinateur virtuel.  
  
-   **Copie de fichiers de disque dur virtuel** -une copie du disque dur virtuel d’une machine virtuelle  
  
-   **ID de génération de machine virtuelle** : un entier de 128 bits donné à la machine virtuelle par l’hyperviseur. Cet ID est stocké en mémoire et réinitialiser chaque fois qu’un instantané est appliqué. La conception utilise un mécanisme indépendant de l’hyperviseur pour exposer l’ID de génération d’ordinateur virtuel dans la machine virtuelle. L’implémentation de Hyper-V expose l’ID de la table ACPI de la machine virtuelle.  
  
-   **Import/Export** -fonctionnalité de Hyper-V, A, qui permet à l’utilisateur enregistrer la machine virtuelle complète (fichiers machine virtuelle, disque dur virtuel et la configuration d’ordinateur). Il permet ensuite aux utilisateurs de mettre l’ordinateur sur le même ordinateur que la même machine virtuelle (restauration), à l’aide de cet ensemble de fichiers sur un autre ordinateur en tant que la même machine virtuelle (déplacer) ou une nouvelle machine virtuelle (copie)  
  
## <a name="BKMK_FixPDCPerms"></a>FixVDCPermissions.ps1  
  
```  
# Unsigned script, requires use of set-executionpolicy remotesigned -force  
# You must run the Windows PowerShell console as an elevated administrator  
  
# Load Active Directory Windows PowerShell Module and switch to AD DS drive  
import-module activedirectory  
cd ad:  
  
## Get Domain NC  
$domainNC = get-addomain  
  
## Get groups and obtain their SIDs   
$dcgroup = get-adgroup "Cloneable Domain Controllers"  
  
$sid1 = (get-adgroup $dcgroup).sid  
  
## Get the DACL of the domain  
$acl = get-acl $domainNC  
  
## The following object specific ACE grants extended right 'Allow a DC to create a clone of itself' for the CDC group to the Domain NC  
## 3e0f7e18-2c7a-4c10-ba82-4d926db99a3e is the schemaIDGuid for 'DS-Clone-Domain-Controller"  
  
$objectguid = new-object Guid 3e0f7e18-2c7a-4c10-ba82-4d926db99a3e  
$ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid1,"ExtendedRight","Allow",$objectguid  
  
## Add the ACE in the ACL and set the ACL on the object   
  
$acl.AddAccessRule($ace1)  
set-acl -aclobject $acl $domainNC  
write-host "Done writing new VDC permissions."  
cd c:   
```  
  


