---
title: Migrer la réplication SYSVOL vers la réplication DFS
ms.date: 07/02/2012
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 56877bc5ddb3ea5f24f4057051775094654d8bbf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386041"
---
# <a name="migrate-sysvol-replication-to-dfs-replication"></a>Migrer la réplication SYSVOL vers la réplication DFS


Mise à jour : 25 août 2010

S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008

Les contrôleurs de domaine utilisent un dossier partagé spécial nommé SYSVOL pour répliquer des scripts d’ouverture de session et des fichiers objets stratégie de groupe sur d’autres contrôleurs de domaine. Windows 2000 Server et Windows Server 2003 utilisent le service de réplication de fichiers (FRS) pour répliquer SYSVOL, alors que Windows Server 2008 utilise le service de réplication DFS plus récent dans des domaines qui utilisent le niveau fonctionnel de domaine Windows Server 2008, et FRS pour les domaines qui exécuter des niveaux fonctionnels de domaine plus anciens.

Pour utiliser réplication DFS pour répliquer le dossier SYSVOL, vous pouvez soit créer un nouveau domaine qui utilise le niveau fonctionnel de domaine Windows Server 2008, soit utiliser la procédure décrite dans ce document pour mettre à niveau un domaine existant et migrer la réplication vers Réplication DFS.

Ce document suppose que vous avez une connaissance de base de Active Directory Domain Services (AD DS), de la réplication FRS et de la résystème de fichiers DFS (réplication DFS). Pour plus d’informations, consultez [Active Directory Domain Services vue d’ensemble](http://go.microsoft.com/fwlink/?linkid=147787), vue d’ensemble de [FRS](http://go.microsoft.com/fwlink/?linkid=121763)ou [vue d’ensemble de réplication DFS](http://go.microsoft.com/fwlink/?linkid=121762)


> [!NOTE]
> Pour télécharger une version imprimable de ce guide, consultez <a href="http://go.microsoft.com/fwlink/?linkid=150375">le Guide de migration de la réplication SYSVOL : Réplication FRS vers DFS</a> (http://go.microsoft.com/fwlink/?LinkId=150375)
<br>


## <a name="in-this-guide"></a>Dans ce guide

[Informations conceptuelles sur la migration SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640170(v=ws.10))

  - [États de migration de SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641052(v=ws.10))  
      
  - [Vue d’ensemble de la procédure de migration SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639809(v=ws.10))  
      

[Procédure de migration SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639860(v=ws.10))

  - [Migration vers l’état préparé](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641193(v=ws.10))  
      
  - [Migration vers l’État Redirigé](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641340(v=ws.10))  
      
  - [Migration vers l’État éliminé](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640254(v=ws.10))  
      

[Résolution des problèmes de migration SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640395(v=ws.10))

  - [Résolution des problèmes de migration de SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639976(v=ws.10))  
      
  - [Restauration de la migration SYSVOL vers un état stable antérieur](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640509(v=ws.10))  
      

[Informations de référence sur la migration SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640293(v=ws.10))

  - [Scénarios de migration SYSVOL pris en charge](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639854(v=ws.10))  
      
  - [Vérification de l’état de la migration de SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639789(v=ws.10))  
      
  - [Dfsrmig](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641227(v=ws.10))  
      
  - [Actions de l’outil de migration SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639712(v=ws.10))  
      

## <a name="additional-references"></a>Références supplémentaires

[Série de migration SYSVOL : Partie 1 : présentation du processus de migration de SYSVOL](http://go.microsoft.com/fwlink/?linkid=121756)

[Série de migration SYSVOL : Partie 2 — dfsrmig. exe : Outil de migration SYSVOL](http://go.microsoft.com/fwlink/?linkid=121757)

[Série de migration SYSVOL : Partie 3 : migration vers l’état préparé](http://go.microsoft.com/fwlink/?linkid=121758)

[Série de migration SYSVOL : Partie 4 : migration vers l’État « Redirigé »](http://go.microsoft.com/fwlink/?linkid=121759)

[Série de migration SYSVOL : Partie 5 : migration vers l’État « éliminé »](http://go.microsoft.com/fwlink/?linkid=121760)

[Guide pas à pas des systèmes de fichiers distribués dans Windows Server 2008](http://go.microsoft.com/fwlink/?linkid=85231)

[Informations techniques de référence sur FRS](http://go.microsoft.com/fwlink/?linkid=121764)

