---
title: Recommandations pour le passage à Windows Server 2016
description: Recommandations pour le passage à Windows Server 2016.
ms.prod: windows-server
ms.date: 10/18/2016
ms.technology: server-general
ms.topic: article
ms.assetid: 74aa1da3-7076-4a1f-ad5b-9e17bd46dba2
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 6b02a3caa0db2a66307754ebd95865d8ba10ef4f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80826752"
---
# <a name="recommendations-for-moving-to-windows-server-2016"></a>Recommandations pour le passage à Windows Server 2016

>S'applique à : Windows Server 2016


|Si votre ordinateur fonctionne sous :|Windows Server 2012 R2 ou Windows Server 2012|Windows Server 2008 R2 ou Windows Server 2008|  
|-------------------|----------|--------------|--------------|---------------------------------------|  
|**Infrastructure des rôles Windows Server**|Choisissez la mise à niveau ou la migration selon les [instructions relatives aux rôles spécifiques](https://technet.microsoft.com/windowsserver/jj554790).|- Pour tirer parti des nouvelles fonctionnalités de Windows Server 2016, déployez un nouveau matériel ou installez Windows Server 2016 dans une machine virtuelle sur un hôte existant. Certaines nouvelles fonctionnalités fonctionnent mieux sur un hôte physique Windows Server 2016 exécutant Hyper-V. <br>- Suivez les [instructions relatives aux rôles spécifiques](https://technet.microsoft.com/windowsserver/jj554790).|
|**Charges de travail d’applications et gestion des serveurs Microsoft**|- Les mises à niveau d’application doivent inclure la *migration* vers Windows Server 2016. Voir la [liste de compatibilité](Server-Application-Compatibility.md). <br>- Les mises à niveau vers Windows Server 2016 uniquement (c’est-à-dire sans mise à niveau des applications) doivent utiliser des instructions spécifiques à l’application.|- Pour tirer parti des nouvelles fonctionnalités de Windows Server 2016, déployez un nouveau matériel ou installez Windows Server 2016 dans une machine virtuelle sur un hôte existant. Certaines nouvelles fonctionnalités fonctionnent mieux sur un hôte physique Windows Server 2016 exécutant Hyper-V. Suivez les guides de migration le cas échéant. <br>- Vous pouvez également rester sur votre système d’exploitation actuel et utiliser une machine virtuelle en cours d’exécution sur un hôte Windows Server 2016 ou Microsoft Azure. Contactez votre revendeur Accord Entreprise, responsable technique de compte ou Microsoft pour connaître les options de support étendu via la [Software Assurance](https://www.microsoft.com/Licensing/licensing-programs/software-assurance-default.aspx).|
|**Charges de travail d’applications d’éditeurs de logiciels indépendants**|- Les mises à niveau vers Windows Server 2016 doivent utiliser des instructions spécifiques à l’application. <br>- Pour plus d’informations sur la compatibilité de Windows Server avec des applications autres que Microsoft, rendez-vous sur le [portail de certification du logo Windows Server](https://msdn.microsoft.com/enterprisecloudcertified).|- Pour tirer parti des nouvelles fonctionnalités de Windows Server 2016, déployez un nouveau matériel ou installez Windows Server 2016 dans une machine virtuelle sur un hôte existant. Certaines nouvelles fonctionnalités fonctionnent mieux sur un hôte physique Windows Server 2016 exécutant Hyper-V. Suivez les guides de migration le cas échéant. <br>- Vous pouvez également rester sur votre système d’exploitation actuel et utiliser une machine virtuelle en cours d’exécution sur un hôte Windows Server 2016 ou Microsoft Azure. Contactez votre revendeur Accord Entreprise, responsable technique de compte ou Microsoft pour connaître les options de support étendu via la [Software Assurance](https://www.microsoft.com/Licensing/licensing-programs/software-assurance-default.aspx).|
|**Charges de travail d’applications personnalisées**|- Renseignez-vous auprès des développeurs d’applications sur la compatibilité avec Windows Server 2016 et demandez des instructions de mise à niveau. <br>- Tirez parti de Microsoft Azure pour tester l’application sur Windows Server 2016 avant de basculer. <br>- Examinez l’ensemble des options dans la section suivante.|- Renseignez-vous auprès des développeurs d’applications sur la compatibilité avec Windows Server 2016 et demandez des instructions de mise à niveau. <br>- Tirez parti de Microsoft Azure pour tester votre application sur Windows Server 2016 avant de basculer. <br>- Pour tirer parti des nouvelles fonctionnalités de Windows Server 2016, déployez un nouveau matériel ou installez Windows Server 2016 dans une machine virtuelle sur un hôte existant. Certaines nouvelles fonctionnalités fonctionnent mieux sur un hôte physique Windows Server 2016 exécutant Hyper-V. <br>- Examinez l’ensemble des options dans la section suivante.|

## <a name="complete-options-for-moving-servers-running-custom-or-in-house-applications-on-older-versions-of-windows-server-to-windows-server-2016"></a>Ensemble des options pour la migration des serveurs exécutant des applications personnalisées ou internes sur des anciennes versions de Windows Server vers Windows Server 2016

Il existe plus d’options que jamais pour aider vos clients et vous-même à tirer parti des fonctionnalités de Windows Server 2016, avec un impact minimal sur les charges de travail et les services actifs.

- Essayez le dernier système d’exploitation avec votre application en téléchargeant la version d’évaluation de [Windows Server](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016) pour la tester en local. Une fois que le test est terminé et si vous en êtes satisfait, vous pouvez effectuer une simple conversion de licence avec une clé de licence vendue au détail (redémarrage requis).

- [Microsoft Azure](https://azure.microsoft.com) peut également servir à titre d’essai pour le test afin de garantir que votre application personnalisée fonctionne sur le dernier système d’exploitation serveur. Une fois que le test est terminé et si vous en êtes satisfait, [migrez vers la dernière version de Windows Server](https://docs.microsoft.com/windows-server/get-started/installation-and-upgrade#upgrade) en local. 

- Sinon, une fois que le test est terminé et si vous en êtes satisfait, [Microsoft Azure](https://azure.microsoft.com) peut aussi servir d’emplacement permanent pour votre application personnalisée ou service. Ainsi, l’ancien serveur reste disponible jusqu’à ce que vous soyez prêt à basculer vers le nouveau serveur dans Azure.

    - Si vous avez déjà la Software Assurance pour Windows Server, économisez de l’argent avec un déploiement à l’aide du programme [Azure Hybrid Use Benefit](https://azure.microsoft.com/pricing/hybrid-use-benefit/). 

- Dans la plupart des cas, [Microsoft Azure](https://azure.microsoft.com) peut être utilisé pour héberger la même application sur l’ancienne version de Windows Server exécutée aujourd’hui. Migrez l’application et la charge de travail vers une machine virtuelle avec le système d’exploitation de votre choix à l’aide des images de la [Place de marché Azure](https://azure.microsoft.com/marketplace/).

    - Si vous avez déjà la Software Assurance pour Windows Server, faites des économies avec un déploiement à l’aide du programme [Azure Hybrid Use Benefit](https://azure.microsoft.com/pricing/hybrid-use-benefit/). 

- Le programme [Software Assurance](https://www.microsoft.com/Licensing/licensing-programs/software-assurance-default.aspx) pour Windows Server propose de nouveaux avantages en matière de droits relatifs aux versions. Entre autres avantages, vous pouvez mettre à niveau les serveurs bénéficiant de la Software Assurance avec la dernière version de Windows Server au bon moment, sans acheter de nouvelle licence. 

## <a name="additional-resources"></a>Ressources supplémentaires

- [Fonctionnalités supprimées ou dépréciées dans Windows Server 2016](deprecated-features.md)
- Pour connaître les options générales de mise à niveau et de migration du serveur, visitez [Options de mise à niveau et de conversion pour Windows Server 2016](Supported-Upgrade-Paths.md).
- Pour plus d’informations sur le cycle de vie des produits et les niveaux de prise en charge, voir [Politique de support Microsoft - FAQ](https://support.microsoft.com/help/17140/support-lifecycle-policy-faq).

