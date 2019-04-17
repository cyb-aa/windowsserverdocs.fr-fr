---
ms.assetid: 73a4deba-7da6-4eae-8fdd-2a4d369f9cbb
title: "Annexe des informations techniques de référence contrôleurs de domaine virtualisés"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2e7f264a098b6f67d98c9aa47ec5794374b8920d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="virtualized-domain-controller-technical-reference-appendix"></a>Annexe des informations techniques de référence contrôleurs de domaine virtualisés

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique couvre:  
  
-   [Terminologie](../../../ad-ds/reference/virtual-dc/../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_Terms)  
  
-   [FixVDCPermissions.ps1](../../../ad-ds/reference/virtual-dc/../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_FixPDCPerms)  
  
## <a name="BKMK_Terms"></a>Terminologie  
  
-   **Capture instantanée** -l’état d’un ordinateur virtuel à un moment donné dans le temps. Il est dépendant de la chaîne de prise de captures instantanées, sur le matériel et sur la plateforme de virtualisation.  
  
-   **Clone** - un terminer et séparer la copie d’un ordinateur virtuel. Il dépend du matériel virtuel (hyperviseur).  
  
-   **Total Clone** -un clone complet est une copie d’un ordinateur virtuel qui ne partage aucune ressource avec la machine virtuelle parent après l’opération de clonage indépendante. Opération en cours d’un clone complet est complètement distincte de l’ordinateur virtuel parent.  
  
-   **Disque de différenciation** -une copie d’un ordinateur virtuel qui partage des disques virtuels avec l’ordinateur virtuel parent de manière permanente. Généralement, cela permet d’économiser l’espace disque et permet à plusieurs ordinateurs virtuels utiliser la même installation de logiciel.  
  
-   **Copie de la machine virtuelle**- une copie des fichiers système de tous les fichiers associés et les dossiers d’un ordinateur virtuel.  
  
-   **Copie des fichiers de disque dur virtuel** -une copie du disque dur virtuel d’un ordinateur virtuel  
  
-   **ID de génération d’ordinateur virtuel** - un entier de 128 bits spécifié à l’ordinateur virtuel par l’hyperviseur. Cet ID est stocké dans la mémoire et réinitialiser chaque fois qu’une capture instantanée est appliquée. La conception utilise un mécanisme d’indépendante du hyperviseur pour exposer l’ID de génération d’ordinateur virtuel sur l’ordinateur virtuel. L’implémentation Hyper-V expose l’ID de la table ACPI de l’ordinateur virtuel.  
  
-   **Importation/exportation** -fonctionnalité A Hyper-V qui permet à l’utilisateur enregistrer la machine virtuelle complète (fichiers VM, disque dur virtuel et la configuration d’ordinateur). Puis les utilisateurs peuvent mettre l’ordinateur sur le même ordinateur que l’ordinateur virtuel même (restauration), à l’aide de cet ensemble de fichiers sur un autre ordinateur en tant que l’ordinateur virtuel même (déplacement), ou un nouvel ordinateur virtuel (copier)  
  
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
  


