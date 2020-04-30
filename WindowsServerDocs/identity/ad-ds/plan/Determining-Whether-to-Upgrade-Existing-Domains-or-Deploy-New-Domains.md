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
ms.openlocfilehash: 60f3dca9f4930fe9d1a717d741a818194a3db0f0
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81624277"
---
# <a name="determining-whether-to-upgrade-existing-domains-or-deploy-new-domains"></a>Décision de mettre à niveau des domaines existants ou d’en déployer de nouveaux

> S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Chaque domaine de votre conception sera soit un nouveau domaine, soit un domaine mis à niveau existant. Les utilisateurs de domaines existants que vous ne mettez pas à niveau doivent être déplacés vers de nouveaux domaines.

Le déplacement de comptes entre domaines peut avoir un impact sur les utilisateurs finaux. Avant de décider de déplacer des utilisateurs vers un nouveau domaine ou de mettre à niveau des domaines existants, évaluez les avantages administratifs à long terme d’un nouveau domaine AD DS par rapport au coût du déplacement des utilisateurs dans le domaine.

Pour plus d’informations sur la mise à niveau des domaines de Active Directory vers Windows Server 2008, consultez [mise à niveau de domaines Active Directory vers des domaines Windows server 2008 et Windows server 2008 R2 AD DS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731188(v=ws.10)).

Pour plus d’informations sur la restructuration des domaines AD DS dans et entre les forêts, consultez Guide de l' [outil ADMT : migration et restructuration de Active Directory domaines](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc974332(v=ws.10)).

Pour obtenir une feuille de calcul qui vous aide à documenter vos plans pour les domaines nouveaux et mis à niveau, téléchargez Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip à partir des [Outils d’aide pour le kit de déploiement de Windows Server 2003](https://microsoft.com/download/details.aspx?id=9608) et ouvrez « planification de domaine » (DSSLOGI_5. doc).
