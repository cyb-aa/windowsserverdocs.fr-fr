---
ms.assetid: c20231dd-2b83-4494-9385-1172272e00d6
title: Décision de mettre à niveau des domaines existants ou d’en déployer de nouveaux
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 1e058240bc971d949de279407701e57cd021712c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822596"
---
# <a name="determining-whether-to-upgrade-existing-domains-or-deploy-new-domains"></a>Décision de mettre à niveau des domaines existants ou d’en déployer de nouveaux

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Chaque domaine de votre conception sera soit un nouveau domaine, soit un domaine mis à niveau existant. Les utilisateurs de domaines existants que vous ne mettez pas à niveau doivent être déplacés vers de nouveaux domaines.  
  
Le déplacement de comptes entre domaines peut avoir un impact sur les utilisateurs finaux. Avant de décider de déplacer des utilisateurs vers un nouveau domaine ou de mettre à niveau des domaines existants, évaluez les avantages administratifs à long terme d’un nouveau domaine AD DS par rapport au coût du déplacement des utilisateurs dans le domaine.  
  
Pour plus d’informations sur la mise à niveau des domaines de Active Directory vers Windows Server 2008, consultez [mise à niveau de domaines Active Directory vers des domaines Windows server 2008 et Windows server 2008 R2 AD DS](https://technet.microsoft.com/library/cc731188.aspx).  
  
Pour plus d’informations sur la restructuration AD DS les domaines dans et entre les forêts, consultez Active Directory Migration Tool version 3,1 Guide de migration ([https://go.microsoft.com/fwlink/?LinkId=93678](https://go.microsoft.com/fwlink/?LinkId=93678)).  
  
Pour obtenir une feuille de calcul qui vous aide à documenter vos plans pour les domaines nouveaux et mis à niveau, téléchargez Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip à partir des outils d’aide pour le kit de déploiement de Windows Server 2003 ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)) et ouvrez « planification de domaine » (DSSLOGI_5. doc).  
  


