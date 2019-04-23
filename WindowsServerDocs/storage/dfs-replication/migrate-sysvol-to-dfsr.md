---
Title: Migrer la réplication SYSVOL vers la réplication DFS
ms.date: 07/02/2012
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 7ea883730fcab83d064fa41f610bde4837510276
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836640"
---
# <a name="migrate-sysvol-replication-to-dfs-replication"></a>Migrer la réplication SYSVOL vers la réplication DFS


Mise à jour : 25 août 2010

S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008

Contrôleurs de domaine utilisent un dossier partagé spécial nommé SYSVOL pour répliquer les scripts d’ouverture de session et les fichiers d’objets de stratégie de groupe pour les autres contrôleurs de domaine. Windows 2000 Server et Windows Server 2003 utilisent le Service de réplication de fichiers (FRS) pour répliquer SYSVOL, tandis que Windows Server 2008 utilise le service de réplication DFS plus récente lorsque dans des domaines qui utilisent le niveau fonctionnel du domaine Windows Server 2008 et FRS pour les domaines qui exécuter des niveaux fonctionnels de domaine plus anciens.

Pour utiliser la réplication DFS pour répliquer le dossier SYSVOL, vous pouvez créer un nouveau domaine qui utilise le niveau fonctionnel de domaine Windows Server 2008, ou vous pouvez utiliser la procédure décrite dans ce document pour mettre à niveau un domaine existant et de migrer la réplication Réplication DFS.

Ce document suppose que vous disposez des connaissances de base sur les Services de domaine Active Directory (AD DS), FRS et réplication DFS (DFS Replication). Pour plus d’informations, consultez [présentation des Services de domaine Active Directory](http://go.microsoft.com/fwlink/?linkid=147787), [vue d’ensemble du service FRS](http://go.microsoft.com/fwlink/?linkid=121763), ou [vue d’ensemble de la réplication DFS](http://go.microsoft.com/fwlink/?linkid=121762)


> [!NOTE]
> Pour télécharger une version imprimable de ce guide, accédez à <a href="http://go.microsoft.com/fwlink/?linkid=150375">Guide de Migration de réplication SYSVOL : Réplication FRS vers DFS</a> ()http://go.microsoft.com/fwlink/?LinkId=150375)
<br>


## <a name="in-this-guide"></a>Dans ce guide

[Informations conceptuelles sur la Migration SYSVOL](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640170(v=ws.10))

  - [États de Migration SYSVOL](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641052(v=ws.10))  
      
  - [Vue d’ensemble de la procédure de Migration SYSVOL](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639809(v=ws.10))  
      

[Procédure de Migration SYSVOL](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639860(v=ws.10))

  - [Migration vers l’état préparé](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641193(v=ws.10))  
      
  - [Migration vers l’état redirigé](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641340(v=ws.10))  
      
  - [Migration vers l’état éliminé](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640254(v=ws.10))  
      

[Résolution des problèmes de Migration SYSVOL](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640395(v=ws.10))

  - [Résolution des problèmes de Migration SYSVOL](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639976(v=ws.10))  
      
  - [Restauration de la Migration SYSVOL dans un état Stable précédent](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640509(v=ws.10))  
      

[Informations de référence de Migration SYSVOL](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640293(v=ws.10))

  - [Scénarios de Migration SYSVOL pris en charge](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639854(v=ws.10))  
      
  - [Vérification de l’état de Migration SYSVOL](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639789(v=ws.10))  
      
  - [Dfsrmig](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641227(v=ws.10))  
      
  - [Actions de l’outil Migration SYSVOL](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639712(v=ws.10))  
      

## <a name="additional-references"></a>Références supplémentaires

[Série de Migration de SYSVOL : Partie 1 : présentation du processus de migration SYSVOL](http://go.microsoft.com/fwlink/?linkid=121756)

[Série de Migration de SYSVOL : Partie 2—Dfsrmig.exe : L’outil de migration SYSVOL](http://go.microsoft.com/fwlink/?linkid=121757)

[Série de Migration de SYSVOL : 3ème partie – migration vers l’état « Prêt »](http://go.microsoft.com/fwlink/?linkid=121758)

[Série de Migration de SYSVOL : Partie 4 : migration vers l’état 'REDIRIGÉ'](http://go.microsoft.com/fwlink/?linkid=121759)

[Série de Migration de SYSVOL : Partie 5 : migration vers l’état « Supprimée »](http://go.microsoft.com/fwlink/?linkid=121760)

[Guide pas à pas pour les systèmes de fichiers distribués dans Windows Server 2008](http://go.microsoft.com/fwlink/?linkid=85231)

[Référence technique de FRS](http://go.microsoft.com/fwlink/?linkid=121764)

