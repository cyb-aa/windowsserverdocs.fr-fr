---
ms.assetid: 206b8072-1d0c-4a0b-ba8a-35a868d67b4c
title: "Création d’une conception de lien de Site"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9eb54781035424c9a5210e11fbdeafc55496c6c3
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="creating-a-site-link-design"></a>Création d’une conception de lien de Site

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Créer une conception de lien de site pour se connecter vos sites avec des liens de sites. Liens de sites reflètent la connectivité intersite et la méthode utilisée pour transférer le trafic de réplication. Vous devez connecter les sites avec des liens de sites afin que les contrôleurs de domaine sur chaque site peuvent répliquer les modifications d’ActiveDirectory.  
  
## <a name="connecting-sites-with-site-links"></a>Connexion de sites avec des liens de sites  
Pour connecter les sites avec des liens de sites, identifier les sites de membre que vous souhaitez vous connecter avec le lien de sites, créer un objet de lien de site dans le conteneur des Transports inter-sites respectif et nommez le lien de site. Après avoir créé le lien de sites, vous pouvez passer pour définir des propriétés de lien de site.  
  
Lorsque vous créez des liens de sites, assurez-vous que chaque site est incluse dans un lien de sites. En outre, assurez-vous que tous les sites sont connectés entre eux par le biais d’autres liens de sites afin que les modifications peuvent être répliquées à partir des contrôleurs de domaine dans n’importe quel site sur tous les autres sites. Si vous ne parvenez pas à effectuer cette opération, un message d’erreur est généré dans le journal du Service d’annuaire dans l’Observateur d’événements indiquant que la topologie de site n’est pas connectée.  
  
Chaque fois que vous ajoutez des sites à un lien de sites nouvellement créé, déterminer si le site que vous ajoutez est un membre des autres liens de sites et modifier l’appartenance au lien de site du site si nécessaire. Par exemple, si vous effectuez un site d’un membre du Default-First-Site-Link lorsque vous créez le site, veillez à supprimer le site à partir du Default-First-Site-Link après avoir ajouté le site à un lien de site. Si vous ne supprimez pas le site à partir du Default-First-Site-Link, le vérificateur de cohérence des connaissances (KCC) sera prendre des décisions de routage basées sur l’appartenance à ces deux liens de sites, qui peut entraîner un routage incorrect.  
  
Pour identifier les sites de membre que vous souhaitez vous connecter avec un lien de site, utilisez la liste des emplacements et les emplacements liés que vous avez enregistrée dans la feuille de calcul (DSSTOPO_1.doc) «Géographique emplacements et des liaisons de Communication». Si plusieurs sites ont la même connectivité et disponibilité entre eux, vous pouvez vous connecter avec le même lien de site.  
  
Le conteneur des Transports inter-sites fournit les moyens de mappage des liens de sites pour le transport qui utilise le lien. Lorsque vous créez un objet de lien de site, créez-la dans le conteneur IP, qui associe le lien de site à l’appel de procédure distante (RPC) via le transport IP, ou le conteneur SMTP Simple Mail Transfer Protocol (), qui associe le lien de site avec le transport SMTP.  
  
> [!NOTE]  
> La réplication SMTP n’est pas pris en charge dans les futures versions des Services de domaine ActiveDirectory (ADDS); Par conséquent, la création d’objets de liens de site dans le conteneur SMTP n’est pas recommandée.  
  
Lorsque vous créez un objet de lien de site dans le conteneur des Transports inter-sites respectif, les services ADDS utilisent RPC sur IP pour transférer la réplication intersite et intrasite entre les contrôleurs de domaine. Pour sécuriser les données en transit, réplication RPC sur IP utilise à la fois l’authentification protocole et les données chiffrement Kerberos.  
  
Lorsqu’une connexion IP directe n’est pas disponible, vous pouvez configurer la réplication entre sites pour utiliser le protocole SMTP. Toutefois, la fonctionnalité de réplication SMTP est limitée et nécessite une autorité de certification d’entreprise (CA). SMTP peut répliquer uniquement les partitions d’annuaire d’application, de schéma et de configuration et ne prend pas en charge la réplication des partitions d’annuaire de domaine.  
  
Pour nommer des liens de sites, utiliser un modèle cohérent d’attribution de noms, telles que name_of_site1-name_of_site2. Enregistrement de la liste des sites, les sites liés et les noms de connexion de ces sites dans une feuille de calcul de liens de sites. Pour une feuille de calcul pour vous aider à l’enregistrement de noms de site et les noms de lien de site associé, voir tâche identificateurs d’applet pour Windows Server2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), télécharger Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip, et Ouvrez (DSSTOPO_5.doc) "Sites et liens de sites associés".  
  
## <a name="in-this-guide"></a>Dans ce guide  
[Définition des propriétés de lien de Site](Setting-Site-Link-Properties.md)  
  


