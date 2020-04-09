---
ms.assetid: cd70b0aa-0a67-4966-a041-4dd3f302c98b
title: Création d’une conception d’infrastructure DNS
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 90c670d5f59702ce932c004a016adb855b040f77
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822802"
---
# <a name="creating-a-dns-infrastructure-design"></a>Création d’une conception d’infrastructure DNS

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Une fois que vous avez créé votre Active Directory conception de forêt et de domaine, vous devez concevoir une infrastructure DNS (Domain Name System) pour prendre en charge votre structure logique de Active Directory. DNS permet aux utilisateurs d’utiliser des noms conviviaux faciles à mémoriser pour se connecter aux ordinateurs et autres ressources sur les réseaux IP. Active Directory Domain Services (AD DS) dans Windows Server 2008 requiert DNS.  
  
Le processus de conception du DNS pour prendre en charge les AD DS varie selon que votre organisation dispose déjà d’un service serveur DNS existant ou que vous déployez un nouveau service serveur DNS :  
  
- Si vous disposez déjà d’une infrastructure DNS existante, vous devez intégrer l’espace de noms Active Directory dans cet environnement. Pour plus d’informations, consultez [intégration de AD DS dans une infrastructure DNS existante](../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md).  
- Si vous n’avez pas d’infrastructure DNS en place, vous devez concevoir et déployer une nouvelle infrastructure DNS pour prendre en charge les AD DS. Pour plus d’informations, consultez [déploiement du système DNS (Domain Name System)](https://go.microsoft.com/fwlink/?LinkId=93656).  
  
Si votre organisation dispose d’une infrastructure DNS existante, vous devez vous assurer que vous comprenez comment votre infrastructure DNS interagit avec l’espace de noms Active Directory. Pour obtenir une feuille de calcul qui vous aide à documenter votre conception d’infrastructure DNS existante, téléchargez Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip à partir des [Outils d’aide pour le kit de déploiement de Windows Server 2003](https://go.microsoft.com/fwlink/?LinkID=102558) et ouvrez « inventaire DNS » (DSSLOGI_8. doc).  
  
> [!NOTE]  
> Outre les adresses IP version 4 (IPv4), Windows Server prend également en charge les adresses IP version 6 (IPv6). Pour obtenir une feuille de calcul qui vous aidera à répertorier les adresses IPv6 tout en documentant la méthode de résolution de noms récursive de votre structure DNS actuelle, consultez [l’annexe a : inventaire DNS](../../ad-ds/plan/Appendix-A--DNS-Inventory.md).
  
Avant de concevoir votre infrastructure DNS pour prendre en charge AD DS, il peut être utile de lire la hiérarchie DNS, le processus de résolution de noms DNS et la façon dont DNS prend en charge les AD DS. Pour plus d’informations sur la hiérarchie DNS et le processus de résolution de noms, consultez la référence technique DNS ([https://go.microsoft.com/fwlink/?LinkID=48145](https://go.microsoft.com/fwlink/?LinkID=48145)). Pour plus d’informations sur la façon dont DNS prend en charge AD DS, consultez la page de référence technique sur la prise en charge de DNS pour Active Directory ([https://go.microsoft.com/fwlink/?LinkID=48147](https://go.microsoft.com/fwlink/?LinkID=48147)).  
  
## <a name="in-this-section"></a>Dans cette section  

- [Examen des concepts DNS](../../ad-ds/plan/Reviewing-DNS-Concepts.md)  
- [DNS et AD DS](../../ad-ds/plan/DNS-and-AD-DS.md)  
- [Affectation du DNS pour le rôle de propriétaire AD DS](../../ad-ds/deploy/Assigning-the-DNS-for-AD-DS-Owner-Role.md)  
- [Intégration d’AD DS à une infrastructure DNS existante](../../ad-ds/plan/../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md)  
