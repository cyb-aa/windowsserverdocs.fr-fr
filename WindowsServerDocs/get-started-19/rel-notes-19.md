---
title: 'Notes de publication: problèmes importants dans Windows Server 2019'
description: Résume les problèmes critiques nécessitant des solutions de contournement pour éviter les blocages, retrait, la perte de données et de défaillance installation
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 134aab85-664f-4d55-87ef-9e5fd098371f
author: coreyp-at-msft
ms.author: coreyp
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: d5d84bb1cd204a5419271cc3668343f8ac9c9a54
ms.sourcegitcommit: dbb4738fdac3b7911952ff11f1eaed9649d6567a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/07/2019
ms.locfileid: "9149889"
---
# Notes de publication: problèmes importants dans Windows Server 2019

>S'applique à: WindowsServer2019

Ces notes de publication résument les problèmes les plus critiques dans Windows Server&reg; système d’exploitation de 2019, y compris les éviter ou les résoudre, s’il est connu. Pour plus d’informations sur les modifications de l’interface, les nouvelles fonctionnalités et les correctifs de cette version, voir les [Nouveautés de Windows Server 2019](whats-new-19.md) et les annonces publiées par les équipes de fonctionnalité spécifique. Sauf indication contraire, chaque problème signalé s’applique à toutes les éditions et options d’installation de Windows Server 2019.  

Ce document est continuellement mis à jour. Les problèmes critiques nécessitant une solution de contournement sont ajoutés dès qu’ils sont identifiés, tout comme les solutions de contournement et les correctifs.  
  
## Notes de publication
Problèmes connus suivants sont présentes dans Windows Server 2019. 
<table border="1" rules="rows">
  <thead align="left" valign="middle">
    <tr>
      <th>Titre</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody align="left" valign="middle">
    <tr>
      <td>Menu option d’installation lors de l’installation de serveur a tronqué texte en allemand</td>
      <td>Lorsque vous exécutez le programme d’installation à partir du support de serveur allemand, dans la fenêtre de sélection du système d’exploitation intitulée «Sélectionner le système d’exploitation que vous voulez installer,» la description d’options d’installation expérience utilisateur aura caractères manquantes et incorrectes à la fin de la phrase. Voici le texte complet allemand comme elle doit apparaître.  
      <br/>
      <p><i>Durch diese Option wird die vollständige grafische installiert Umgebung von Windows, wodurch zusätzlicher Speicherplatz verbraucht wird. SIE kann hilfreich sein, wenn Sie salon Windows de bureau verwenden möchten ou über eine application verfügen, matrice die grafische Umgebung benötigt.</i> </p>
      <p>Cela affecte uniquement le média allemand publié à disponibilité publique de Windows Server 2019, Windows Server, version 1809 et Microsoft Hyper-V Server 2019.</p></td>
    </tr>
    <tr>
      <td>Image de personnalisation de Windows Server incorrect pendant l’installation de Windows Server, version 1809  </td>
      <td>Pendant l’expérience d’installation de Windows Server, version 1809, l’image d’arrière-plan sur certains écrans initiales indique «Windows Server 2019».  En tant que Windows Server, version 1709 et 1803, il vous suffit de dire «Windows Server».  Il n’y a aucune autres impacts ailleurs dans le produit, et il n’existe aucun impact sur le produit Windows Server 2019.  Le problème est limité à cette image par image lors de l’installation de Windows Server, version 1809, disponible uniquement pour les clients de licence en volume accéder à du centre de gestion de licences en Volume.  
      </td>
    </tr>
  </tbody>
</table>


### Copyright  
Ce document est fourni «en l’état». Les informations contenues dans ce document, y compris les URL et autres références de sites Web, pourront faire l’objet de modifications sans préavis.  

Ce document ne vous fournit aucun droit légal de propriété intellectuelle de tout produit Microsoft. Vous pouvez copier le présent document pour une utilisation interne à des fins de référence.  

&copy;2019 Microsoft Corporation. Tous droits réservés.  

Microsoft, Active Directory, Hyper-V, Windows et WindowsServer sont soit des marques, soit des marques déposées de Microsoft Corporation aux États-Unis d’Amérique et/ou dans d’autres pays.  

Ce produit contient un logiciel de filtrage graphique qui est basé en partie sur le travail du groupe IndependentJPEGGroup.  


1.0  
