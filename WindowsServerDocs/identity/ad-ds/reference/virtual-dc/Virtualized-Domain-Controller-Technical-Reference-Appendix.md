---
ms.assetid: 73a4deba-7da6-4eae-8fdd-2a4d369f9cbb
title: Annexe des informations techniques de référence sur les contrôleurs de domaine virtualisés
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ee5a46781a61b8546fef113763c0d8ef9ca9f6cc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853982"
---
# <a name="virtualized-domain-controller-technical-reference-appendix"></a>Annexe des informations techniques de référence sur les contrôleurs de domaine virtualisés

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique traite des sujets suivants :  
  
-   [Terminologie](../../../ad-ds/reference/virtual-dc/../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_Terms)  
  
-   [FixVDCPermissions. ps1](../../../ad-ds/reference/virtual-dc/../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_FixPDCPerms)  
  
## <a name="terminology"></a><a name="BKMK_Terms"></a>Correspondance  
  
-   **Instantané** : état d’une machine virtuelle à un moment donné. Elle dépend de la chaîne des captures instantanées précédentes prises, sur le matériel et sur la plateforme de virtualisation.  
  
-   **Cloner** : copie complète et séparée d’une machine virtuelle. Il dépend du matériel virtuel (hyperviseur).  
  
-   **Clone complet** : un clone complet est une copie indépendante d’une machine virtuelle qui ne partage aucune ressource avec la machine virtuelle parente après l’opération de clonage. Le fonctionnement en cours d’un clone complet est entièrement distinct de l’ordinateur virtuel parent.  
  
-   **Disque de différenciation** : copie d’un ordinateur virtuel qui partage des disques virtuels avec la machine virtuelle parente de manière continue. Cela permet généralement d’économiser de l’espace disque et permet à plusieurs machines virtuelles d’utiliser la même installation de logiciel.  
  
-   **Copie de machine virtuelle**: copie du système de fichiers de tous les fichiers et dossiers associés d’un ordinateur virtuel.  
  
-   **Copie de fichier VHD** : copie du disque dur virtuel d’une machine virtuelle  
  
-   **ID de génération d’ordinateur virtuel** : entier 128 bits donné à la machine virtuelle par l’hyperviseur. Cet ID est stocké en mémoire et réinitialisé chaque fois qu’un instantané est appliqué. La conception utilise un mécanisme indépendant de l’hyperviseur pour exposer l’ID de génération d’ordinateur virtuel à l’ordinateur virtuel. L’implémentation d’Hyper-V expose l’ID dans la table ACPI de l’ordinateur virtuel.  
  
-   **Importation/exportation** : fonctionnalité Hyper-V qui permet à l’utilisateur d’enregistrer l’intégralité de l’ordinateur virtuel (fichiers de machine virtuelle, VHD et configuration de l’ordinateur). Il permet ensuite aux utilisateurs d’utiliser ce jeu de fichiers pour ramener l’ordinateur sur le même ordinateur que la même machine virtuelle (restauration), sur un autre ordinateur que la même machine virtuelle (déplacement) ou sur une nouvelle machine virtuelle (copie).  
  
## <a name="fixvdcpermissionsps1"></a><a name="BKMK_FixPDCPerms"></a>FixVDCPermissions. ps1  
  
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
  


