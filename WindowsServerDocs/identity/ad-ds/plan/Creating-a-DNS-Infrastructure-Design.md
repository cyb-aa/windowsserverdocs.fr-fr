---
ms.assetid: cd70b0aa-0a67-4966-a041-4dd3f302c98b
title: Création d’une conception d’infrastructure DNS
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 080c36f8410be4d6b1933c74730e2b55ce8d0a0b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856130"
---
# <a name="creating-a-dns-infrastructure-design"></a>Création d’une conception d’infrastructure DNS

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Après avoir créé vos conceptions de forêt et le domaine Active Directory, vous devez concevoir une infrastructure de système DNS (Domain Name) pour prendre en charge de la structure logique d’Active Directory. DNS permet aux utilisateurs d’utiliser des noms conviviaux qui sont faciles à mémoriser pour se connecter aux ordinateurs et autres ressources sur les réseaux IP. Services de domaine Active Directory (AD DS) dans Windows Server 2008 nécessite DNS.  
  
Le processus de conception de DNS pour prendre en charge les services AD DS varie selon que votre organisation possède déjà un service de serveur DNS existant ou que vous déployez un nouveau service de serveur DNS :  
  
- Si vous avez déjà une infrastructure DNS existante, vous devez intégrer l’espace de noms Active Directory dans cet environnement. Pour plus d’informations, consultez [l’intégration AD DS dans une Infrastructure DNS existante](../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md).  
- Si vous n’avez pas d’une infrastructure DNS en place, vous devez concevoir et déployer une nouvelle infrastructure DNS pour prendre en charge les services AD DS. Pour plus d’informations, consultez [déploiement de système DNS (Domain Name)](https://go.microsoft.com/fwlink/?LinkId=93656).  
  
Si votre organisation possède une infrastructure DNS existante, il se peut que vous devez vous assurer que vous comprenez comment votre infrastructure DNS vont interagir avec l’espace de noms Active Directory. Pour une feuille de calcul pour vous aider à documenter votre conception d’infrastructure DNS existante, téléchargez Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip à partir de [travail aides pour Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558) et Ouvrez « Inventaire DNS » (DSSLOGI_8.doc).  
  
> [!NOTE]  
> En plus du protocole IP version 4 (IPv4) des adresses, corrige également prend en charge IP version 6 (IPv6) de Windows Server. Pour une feuille de calcul pour vous aider à la liste des adresses IPv6 lors de la documentation de la méthode de résolution de noms récursive de la structure actuelle de votre DNS, consultez [annexe a : Inventaire DNS](../../ad-ds/plan/Appendix-A--DNS-Inventory.md).
  
Avant de concevoir votre infrastructure DNS pour prendre en charge les services AD DS, il peut être utile d’en savoir plus sur la hiérarchie DNS, le processus de résolution de nom DNS, et comment DNS prend en charge les services AD DS. Pour plus d’informations sur le processus de résolution de hiérarchie et le nom DNS, consultez la référence technique DNS ([https://go.microsoft.com/fwlink/?LinkID=48145](https://go.microsoft.com/fwlink/?LinkID=48145)). Pour plus d’informations sur la manière dont DNS prend en charge les services AD DS, consultez la prise en charge de DNS pour les informations de référence technique Active Directory ([https://go.microsoft.com/fwlink/?LinkID=48147](https://go.microsoft.com/fwlink/?LinkID=48147)).  
  
## <a name="in-this-section"></a>Dans cette section  

- [Examen des Concepts DNS](../../ad-ds/plan/Reviewing-DNS-Concepts.md)  
- [DNS et AD DS](../../ad-ds/plan/DNS-and-AD-DS.md)  
- [Affectez le DNS pour le rôle de propriétaire du service d’annuaire AD](../../ad-ds/deploy/Assigning-the-DNS-for-AD-DS-Owner-Role.md)  
- [L’intégration des services AD DS dans une Infrastructure DNS existante](../../ad-ds/plan/../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md)  
