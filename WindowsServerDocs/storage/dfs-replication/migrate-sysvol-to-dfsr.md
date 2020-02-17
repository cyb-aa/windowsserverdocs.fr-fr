---
title: Migrer la réplication SYSVOL vers la réplication DFS
ms.date: 07/02/2012
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: d8d9d47ff8f14ce316d2352729247ab2dcf4acbc
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949704"
---
# <a name="migrate-sysvol-replication-to-dfs-replication"></a>Migrer la réplication SYSVOL vers la réplication DFS


Mise à jour : 25 août 2010

S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008

Les contrôleurs de domaine utilisent un dossier partagé spécial nommé SYSVOL pour répliquer des scripts d’ouverture de session et des fichiers objets de stratégie de groupe sur d’autres contrôleurs de domaine. Windows 2000 Server et Windows Server 2003 utilisent le service de réplication de fichiers (FRS) pour répliquer SYSVOL, alors que Windows Server 2008 utilise le service de réplication DFS plus récent dans des domaines qui utilisent le niveau fonctionnel de domaine Windows Server 2008, et FRS pour les domaines qui exécutent des niveaux fonctionnels de domaine plus anciens.

Pour utiliser la réplication DFS en vue de répliquer le dossier SYSVOL, vous pouvez soit créer un domaine qui utilise le niveau fonctionnel de domaine Windows Server 2008, soit utiliser la procédure décrite dans ce document pour mettre à niveau un domaine existant et migrer la réplication vers la réplication DFS.

Ce document suppose que vous possédez des connaissances de base sur Active Directory Domain Services (AD DS), sur le service FRS et sur la réplication du système de fichiers distribués (réplication DFS). Pour plus d’informations, consultez [Vue d’ensemble d’Active Directory Domain Services](https://go.microsoft.com/fwlink/?linkid=147787), [Vue d’ensemble du service FRS](https://go.microsoft.com/fwlink/?linkid=121763) ou [Vue d’ensemble de la réplication DFS](https://go.microsoft.com/fwlink/?linkid=121762).


> [!NOTE]
> Pour télécharger une version imprimable de ce guide, accédez au <a href="https://go.microsoft.com/fwlink/?linkid=150375">Guide de migration de la réplication SYSVOL : Réplication FRS vers la réplication DFS</a> (https://go.microsoft.com/fwlink/?LinkId=150375)
<br>


## <a name="in-this-guide"></a>Contenu de ce guide

[Informations conceptuelles sur la migration SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640170(v=ws.10))

  - [États de la migration SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641052(v=ws.10))  
      
  - [Vue d’ensemble de la procédure de migration SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639809(v=ws.10))  
      

[Procédure de migration SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639860(v=ws.10))

  - [Migration vers l’état Préparé](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641193(v=ws.10))  
      
  - [Migration vers l’état Redirigé](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641340(v=ws.10))  
      
  - [Migration vers l’état Éliminé](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640254(v=ws.10))  
      

[Résolution des problèmes de migration SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640395(v=ws.10))

  - [Résolution des problèmes liés à la migration SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639976(v=ws.10))  
      
  - [Restauration de la migration SYSVOL dans un état stable précédent](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640509(v=ws.10))  
      

[Informations de référence sur la migration SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640293(v=ws.10))

  - [Scénarios de migration SYSVOL pris en charge](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639854(v=ws.10))  
      
  - [Vérification de l’état de la migration SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639789(v=ws.10))  
      
  - [Dfsrmig](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641227(v=ws.10))  
      
  - [Actions de l’outil de migration SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639712(v=ws.10))  
      

## <a name="additional-references"></a>Références supplémentaires

[Séries de migration SYSVOL : Partie 1 - Introduction au processus de migration SYSVOL](https://go.microsoft.com/fwlink/?linkid=121756)

[Séries de migration SYSVOL : Partie 2 - Dfsrmig.exe : L’outil de migration SYSVOL](https://go.microsoft.com/fwlink/?linkid=121757)

[Séries de migration SYSVOL : Partie 3 - Migration vers l’état « PRÉPARÉ »](https://go.microsoft.com/fwlink/?linkid=121758)

[Séries de migration SYSVOL : Partie 4 - Migration vers l’état « REDIRIGÉ »](https://go.microsoft.com/fwlink/?linkid=121759)

[Séries de migration SYSVOL : Partie 5 - Migration vers l’état « ÉLIMINÉ »](https://go.microsoft.com/fwlink/?linkid=121760)

[Guide pas à pas pour les systèmes de fichiers distribués (DFS) dans Windows Server 2008](https://go.microsoft.com/fwlink/?linkid=85231)

[Informations techniques de référence sur le service FRS](https://go.microsoft.com/fwlink/?linkid=121764)

