---
ms.assetid: cd70b0aa-0a67-4966-a041-4dd3f302c98b
title: "Création d’une conception d’Infrastructure DNS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 6e5093b05fd81a693cec87ddb00d39e70483df23
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="creating-a-dns-infrastructure-design"></a>Création d’une conception d’Infrastructure DNS

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Après avoir créé vos conceptions de forêt et le domaine ActiveDirectory, vous devez concevoir une infrastructure de système DNS (Domain Name) pour prendre en charge de la structure logique d’ActiveDirectory. DNS permet aux utilisateurs d’utiliser des noms conviviaux faciles à retenir pour se connecter aux ordinateurs et autres ressources sur des réseaux IP. Services de domaine ActiveDirectory (ADDS) dans Windows Server2008 nécessite le système DNS.  
  
Le processus de conception DNS pour prendre en charge les services ADDS varie selon que votre organisation possède déjà un service de serveur DNS existant ou que vous déployez un nouveau service de serveur DNS:  
  
-   Si vous disposez déjà d’une infrastructure DNS existante, vous devez intégrer l’espace de noms ActiveDirectory dans cet environnement. Pour plus d’informations, voir [intégration ADDS dans une Infrastructure DNS existante](../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md).  
  
-   Si vous n’avez pas d’une infrastructure DNS en place, vous devez concevoir et déployer une nouvelle infrastructure DNS pour prendre en charge les services ADDS. Pour plus d’informations, voir le déploiement de système DNS (Domain Name) ([https://go.microsoft.com/fwlink/?LinkId=93656](https://go.microsoft.com/fwlink/?LinkId=93656)).  
  
Si votre organisation dispose d’une infrastructure DNS existante, il se peut que vous devez vous assurer que vous comprenez comment votre infrastructure DNS vont interagir avec l’espace de noms ActiveDirectory. Pour une feuille de calcul pour vous aider à documenter votre conception d’infrastructure DNS existante, téléchargez Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip à partir de la tâche des identificateurs d’applet pour Windows Server2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)) et ouvrez "Inventaire DNS" (DSSLOGI_8.doc).  
  
> [!NOTE]  
> En plus de IP version4 (IPv4) des adresses, adresses de Windows Server2008 également prend en charge IP version6 (IPv6). Pour plus d’une feuille de calcul pour vous aider à la liste des adresses IPv6 lors de la documentation de la méthode de résolution de noms récursive de votre structure DNS en cours, consultez [annexe a: DNS inventaire](../../ad-ds/plan/Appendix-A--DNS-Inventory.md).  
  
Avant de concevoir votre infrastructure DNS pour prendre en charge les services ADDS, il peut être utile pour en savoir plus sur la hiérarchie DNS, le processus de résolution de nom DNS et comment DNS prend en charge les services ADDS. Pour plus d’informations sur le processus de résolution de nom et la hiérarchie DNS, voir la référence technique DNS ([https://go.microsoft.com/fwlink/?LinkID=48145](https://go.microsoft.com/fwlink/?LinkID=48145)). Pour plus d’informations sur comment DNS prend en charge les services ADDS, voir la prise en charge de DNS pour la référence technique ActiveDirectory ([https://go.microsoft.com/fwlink/?LinkID=48147](https://go.microsoft.com/fwlink/?LinkID=48147)).  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Examen des Concepts DNS](../../ad-ds/plan/Reviewing-DNS-Concepts.md)  
  
-   [DNS et les services ADDS](../../ad-ds/plan/DNS-and-AD-DS.md)  
  
-   [Affectation du DNS pour le rôle de propriétaire du service d’annuaire ActiveDirectory](../../ad-ds/deploy/Assigning-the-DNS-for-AD-DS-Owner-Role.md)  
  
-   [Intégration des services ADDS dans une Infrastructure DNS existante](../../ad-ds/plan/../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md)  
  


