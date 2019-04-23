---
ms.assetid: 206b8072-1d0c-4a0b-ba8a-35a868d67b4c
title: Création d'une conception de lien de site
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4e0607cf66d41e1747b108a3ecc10562120d9174
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861930"
---
# <a name="creating-a-site-link-design"></a>Création d'une conception de lien de site

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Créer une conception de lien de site pour connecter vos sites avec des liens de sites. Liens de sites reflètent la connectivité intersite et la méthode utilisée pour transférer le trafic de réplication. Vous devez connecter les sites avec des liens de sites afin que les contrôleurs de domaine sur chaque site peuvent répliquer les modifications de Active Directory.  
  
## <a name="connecting-sites-with-site-links"></a>Connexion de sites à des liens de sites

Pour connecter les sites avec des liens de sites, identifier les sites de membre que vous souhaitez vous connecter avec le lien de sites, créer un objet de lien de site dans le conteneur Transports inter-sites respectif, puis nommez le lien de site. Une fois que vous créez le lien de site, vous pouvez passer pour définir les propriétés de lien de site.  
  
Lorsque vous créez des liens de sites, assurez-vous que chaque site est incluse dans un lien de sites. En outre, assurez-vous que tous les sites sont connectés entre eux via d’autres liens de sites afin que les modifications puissent être répliquées à partir des contrôleurs de domaine dans n’importe quel site pour tous les autres sites. Si vous ne le faites pas, un message d’erreur est généré dans le journal du Service d’annuaire dans l’Observateur d’événements indiquant que la topologie de site n’est pas connectée.  
  
Chaque fois que vous ajoutez des sites à un lien de site qui vient d’être créé, déterminer si le site que vous ajoutez est un membre d’autres liens de sites et modifier l’appartenance de lien de site du site si nécessaire. Par exemple, si vous apportez un site d’un membre de la valeur par défaut-Premier-Site-Link lorsque vous créez le site, veillez à supprimer le site de la valeur par défaut-Premier-Site-Link après avoir ajouté le site à un lien de site. Si vous ne supprimez pas le site à partir de la valeur par défaut-Premier-Site-Link, le vérificateur de cohérence des connaissances (KCC) sera décisions de routage basé sur l’appartenance à la fois des liens de sites, ce qui peut aboutir dans le routage incorrect.  
  
Pour identifier les sites de membre que vous souhaitez vous connecter avec un lien de site, utilisez la liste des emplacements et des emplacements liés que vous avez enregistrée dans la feuille de calcul « Géographique emplacements et liaisons de Communication » (DSSTOPO_1.doc). Si plusieurs sites ont la même connectivité et la disponibilité entre eux, vous pouvez vous connecter avec le même lien de site.  
  
Le conteneur Transports inter-sites fournit les moyens de mappage de liens de sites pour le transport utilise le lien. Lorsque vous créez un objet de lien de site, vous les créez dans le conteneur IP, qui associe la liaison de site à l’appel de procédure distante (RPC) via le transport IP, ou le conteneur SMTP Simple Mail Transfer Protocol (), qui associe la liaison de site avec le protocole SMTP transport.  
  
> [!NOTE]  
> La réplication SMTP n’est pas pris en charge dans les futures versions des Services de domaine Active Directory (AD DS) ; Par conséquent, la création d’objets de liens de site dans le conteneur SMTP n’est pas recommandée.  
  
Lorsque vous créez un objet de lien de site dans le conteneur Transports inter-sites respectif, les services AD DS utilisent RPC sur IP pour transférer la réplication intersite et intrasite entre les contrôleurs de domaine. Pour sécuriser les données en cours de transit, la réplication RPC sur IP utilise à la fois le Kerberos authentication protocol et chiffrement des données.  
  
Lorsqu’une connexion IP directe n’est pas disponible, vous pouvez configurer la réplication entre sites pour utiliser le protocole SMTP. Toutefois, la fonctionnalité de réplication SMTP est limitée et nécessite une autorité de certification d’entreprise (CA). SMTP permettre uniquement répliquer la configuration, schéma et partitions d’annuaire d’application et ne prend pas en charge la réplication des partitions d’annuaire de domaine.  
  
Pour nommer les liens de sites, d’utiliser un schéma d’affectation de noms cohérent, telles que name_of_site1-name_of_site2. Enregistrement de la liste des sites, sites liés et les noms des liaisons de sites qui connecte ces sites dans une feuille de calcul. Pour une feuille de calcul pour vous aider à l’enregistrement des noms de site et les noms de lien de site associé, consultez [travail aides pour Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), téléchargez Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip, et Ouvrez « Sites et liens de sites associés » (DSSTOPO_5.doc).  
  
## <a name="in-this-guide"></a>Dans ce guide

[Définition des propriétés de lien de Site](Setting-Site-Link-Properties.md)  
