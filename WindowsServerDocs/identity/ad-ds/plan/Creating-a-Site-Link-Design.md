---
ms.assetid: 206b8072-1d0c-4a0b-ba8a-35a868d67b4c
title: Création d'une conception de lien de site
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: fdff477a1fb7cbe42402b2bb608eea55f2f9ec09
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402711"
---
# <a name="creating-a-site-link-design"></a>Création d'une conception de lien de site

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Créez une conception de lien de sites pour connecter vos sites aux liens de sites. Les liens de sites reflètent la connectivité intersite et la méthode utilisée pour transférer le trafic de réplication. Vous devez connecter des sites avec des liens de sites afin que les contrôleurs de domaine de chaque site puissent répliquer les modifications de Active Directory.  
  
## <a name="connecting-sites-with-site-links"></a>Connexion de sites à des liens de sites

Pour connecter des sites avec des liens de sites, identifiez les sites membres auxquels vous souhaitez vous connecter avec le lien de site, créez un objet de lien de sites dans le conteneur de Transports inter-sites respectifs, puis nommez le lien de site. Après avoir créé le lien de site, vous pouvez définir les propriétés des liens de sites.  
  
Lorsque vous créez des liens de sites, assurez-vous que chaque site est inclus dans un lien de sites. En outre, assurez-vous que tous les sites sont connectés entre eux par le biais d’autres liens de sites afin que les modifications puissent être répliquées à partir de contrôleurs de domaine dans n’importe quel site vers tous les autres sites. Si vous ne le faites pas, un message d’erreur est généré dans le journal du service d’annuaire dans observateur d’événements indiquant que la topologie de site n’est pas connectée.  
  
Chaque fois que vous ajoutez des sites à un lien de sites nouvellement créé, déterminez si le site en cours d’ajout est membre d’autres liens de sites et modifiez l’appartenance au lien du site si nécessaire. Par exemple, si vous faites d’un site un membre du lien par défaut-premier-site lors de la création initiale du site, veillez à supprimer le site du lien par défaut-premier-site après avoir ajouté le site à un nouveau lien de sites. Si vous ne supprimez pas le site du lien par défaut-premier-site-, le vérificateur de cohérence des connaissances (KCC) prend des décisions de routage en fonction de l’appartenance des deux liens de sites, ce qui peut entraîner un routage incorrect.  
  
Pour identifier les sites membres auxquels vous souhaitez vous connecter à l’aide d’un lien de sites, utilisez la liste des emplacements et des emplacements liés que vous avez enregistrés dans la feuille de calcul « emplacements géographiques et liens de communication » (DSSTOPO_1. doc). Si plusieurs sites ont la même connectivité et la même disponibilité entre eux, vous pouvez les connecter avec le même lien de sites.  
  
Le conteneur de Transports inter-sites permet de mapper des liens de site vers le transport utilisé par le lien. Lorsque vous créez un objet de lien de sites, vous le créez dans le conteneur IP, qui associe le lien de site à l’appel de procédure distante (RPC) sur le transport IP ou le conteneur SMTP (Simple Mail Transfer Protocol), qui associe le lien de site au protocole SMTP. transport.  
  
> [!NOTE]  
> La réplication SMTP ne sera pas prise en charge dans les futures versions de Active Directory Domain Services (AD DS); par conséquent, il n’est pas recommandé de créer des objets de liens de sites dans le conteneur SMTP.  
  
Lorsque vous créez un objet de lien de sites dans le conteneur de Transports inter-sites respectifs, AD DS utilise RPC sur IP pour transférer la réplication intersite et intrasite entre les contrôleurs de domaine. Pour sécuriser les données en transit, la réplication RPC sur IP utilise le protocole d’authentification Kerberos et le chiffrement des données.  
  
Lorsqu’une connexion IP directe n’est pas disponible, vous pouvez configurer la réplication entre les sites pour utiliser SMTP. Toutefois, la fonctionnalité de réplication SMTP est limitée et requiert une autorité de certification d’entreprise. SMTP peut uniquement répliquer les partitions de configuration, de schéma et d’annuaire d’applications et ne prend pas en charge la réplication des partitions d’annuaire de domaine.  
  
Pour nommer les liens de sites, utilisez un schéma de nommage cohérent, tel que name_of_site1-name_of_site2. Enregistrez la liste des sites, des sites liés et des noms des liens de sites qui connectent ces sites dans une feuille de calcul. Pour obtenir une feuille de calcul qui vous aide à enregistrer des noms de site et des noms de liens de sites associés, consultez [les outils d’aide pour le kit de déploiement Windows Server 2003](https://go.microsoft.com/fwlink/?LinkID=102558), téléchargez Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip et ouvrez «sites et Liens de sites associés» (DSSTOPO_5. doc).  
  
## <a name="in-this-guide"></a>Dans ce guide

[Définition des propriétés des liens de sites](Setting-Site-Link-Properties.md)  
