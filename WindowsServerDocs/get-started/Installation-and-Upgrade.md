---
title: Installation et mise à niveau de Windows Server
description: Comment installer, mettre à niveau ou migrer vers une version plus récente de Windows Server.
ms.prod: windows-server
ms.date: 05/14/2019
ms.technology: server-general
ms.topic: article
ms.assetid: 98f876bd-63ff-4c3a-95d4-a8dd8d0d119c
author: jasongerend
ms.author: jgerend
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 140f67a9dab5cf1f10cdb0c5c51a031a0dfb9dd3
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "79323641"
---
# <a name="windows-server-installation-and-upgrade"></a>Installation et mise à niveau de Windows Server

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Vous cherchez Windows Server 2019 ? Consultez [Installer, mettre à niveau ou migrer vers Windows Server 2019](../get-started-19/install-upgrade-migrate-19.md).

> [!IMPORTANT]
> Le support étendu de Windows Server 2008 R2 et de Windows Server 2008 prend fin en janvier 2020. [En savoir plus sur vos options de mise à niveau](#upgrading-from-windows-server-2008-r2-or-windows-server-2008).

Il est peut-être temps de passer à une version plus récente de Windows Server. Selon la version exécutée, vous disposez de nombreuses options de mise à niveau.

## <a name="installation"></a>Installation

Si vous souhaitez effectuer une mise à niveau vers une version plus récente de Windows Server sur le même matériel, une procédure infaillible consiste à effectuer une **nouvelle installation**, où vous installez simplement la dernière version du système d’exploitation directement sur l’ancienne version, sur le même matériel, en supprimant ainsi le système d’exploitation précédent. Cette procédure est la plus simple, mais vous devez commencer par sauvegarder vos données et il vous faudra ensuite réinstaller vos applications. Vous devez prendre en compte plusieurs éléments, notamment la configuration système requise. Par conséquent, veillez à consulter les informations relatives à [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkID=825558), [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418), et [Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).

La mise à niveau d’une version précommerciale (par exemple, Windows Server 2016 Technical Preview) vers la version finale (Windows Server 2016) requiert toujours une nouvelle installation.

## <a name="migration-recommended-for-windows-server-2016"></a>Migration (recommandée pour Windows Server 2016)

La documentation relative à la migration de Windows Server vous aide à migrer un rôle ou une fonctionnalité à la fois d’un ordinateur source qui exécute Windows Server vers un autre ordinateur cible qui exécute la même version ou une version plus récente de Windows Server. À ces fins, la migration est définie comme étant le déplacement d’un rôle ou d’une fonctionnalité et des données associées vers un autre ordinateur, et non pas la mise à niveau de la fonctionnalité sur le même ordinateur. Il s’agit de la méthode recommandée pour déplacer votre charge de travail existante et les données associées vers une version plus récente de Windows Server. Pour commencer, vérifiez le [tableau sur la mise à niveau et la migration des rôles serveur](https://go.microsoft.com/fwlink/?LinkId=828595) pour Windows Server.

## <a name="cluster-os-rolling-upgrade"></a>Mise à niveau propagée du système d’exploitation de cluster
La mise à niveau propagée de système d’exploitation de cluster est une nouvelle fonctionnalité de Windows Server 2016 qui permet à un administrateur de mettre à niveau le système d’exploitation des nœuds de cluster Windows Server 2016 R2 vers Windows Server 2012, sans arrêter les charges de travail du serveur de fichiers avec montée en puissance parallèle ou Hyper-V. Cette fonctionnalité élimine les temps d’arrêt susceptibles d’affecter les contrats de niveau de service. Cette nouvelle fonctionnalité est décrite plus en détail dans l’article [Mise à niveau propagée de système d’exploitation de cluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade).

## <a name="license-conversion"></a>Conversion de licence
Dans certaines versions de systèmes d’exploitation, vous pouvez convertir une édition particulière de la version vers une autre édition de la même version en une seule étape à l’aide d’une simple commande et de la clé de licence appropriée. Cette opération est appelée **conversion de licence**. Par exemple, si votre serveur exécute Windows Server 2016 Standard, vous pouvez effectuer une conversion vers Windows Server 2016 Datacenter. Pour certaines versions de Windows Server, vous pouvez également effectuer librement des conversions entre les versions OEM, de licence en volume et commerciales avec la même commande et la clé appropriée.

## <a name="upgrade"></a>Mettre à niveau/Mise à niveau
Si vous souhaitez conserver le même matériel et tous les rôles serveur que vous avez configurés sans aplatir le serveur, la **mise à niveau** est une option et il existe de nombreuses façons de procéder. Lors de la mise à niveau standard, vous évoluez d’un ancien système d’exploitation vers une version plus récente, en conservant vos paramètres, rôles serveur et données inchangés. Par exemple, si votre serveur exécute Windows Server 2012 R2, vous pouvez le mettre à niveau vers Windows Server 2016. Toutefois, il n’existe pas de chemin de mise à niveau vers toutes les versions plus récentes pour certains systèmes d’exploitation plus anciens.
 
>[!NOTE]
>La mise à niveau convient le mieux aux machines virtuelles qui ne nécessitent pas de pilotes de matériel OEM spécifiques pour que l’opération réussisse.
 
Vous pouvez effectuer une mise à niveau à partir d’une version d’évaluation du système d’exploitation vers une version commercialisée, d’une version commercialisée plus ancienne vers une version plus récente ou, dans certains cas, d’une édition de licence en volume du système d’exploitation vers une version commercialisée ordinaire.

Avant de commencer une mise à niveau, passez en revue les tableaux de cette page pour vérifier comment effectuer la mise à niveau souhaitée à partir de votre configuration actuelle.

Pour plus d’informations sur les différences entre les options d’installation disponibles pour Windows Server 2016 Technical Preview, notamment les fonctionnalités installées avec chaque option et les options de gestion disponibles après l’installation, consultez [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkId=828598).

>[!NOTE]
>Chaque fois que vous migrez ou effectuez une mise à niveau vers une version de Windows Server, vous devez passer en revue et comprendre la [stratégie de support](https://support.microsoft.com/lifecycle) et le cycle de vie de cette version et planifier la procédure en conséquence. Vous pouvez [rechercher les informations relatives au cycle de vie](https://support.microsoft.com/lifecycle) de la version de Windows Server qui vous intéresse.
 
 
## <a name="upgrading-to-windows-server-2016"></a>Mise à niveau vers Windows Server 2016
Pour plus d’informations, y compris les réserves importantes et les restrictions applicables à la mise à niveau, la conversion de licence entre éditions de Windows Server 2016 et la conversion de versions d’évaluation vers une version commerciale, consultez [Options de mise à niveau et de conversion pour Windows Server 2016](https://go.microsoft.com/fwlink/?LinkId=828602).
 
>[!NOTE]
>Remarque : Les mises à niveau qui passent d’une installation minimale à une installation Serveur avec Expérience utilisateur (et vice versa) ne sont pas prises en charge. Si le système d’exploitation plus ancien à partir duquel vous effectuez la mise à niveau ou la conversion est une installation minimale, le résultat sera toujours une installation minimale du système d’exploitation plus récent.
 
Tableau de référence rapide des chemins de mise à niveau pris en charge pour des anciennes versions commerciales de Windows Server vers des versions commerciales de Windows Server 2016 :


|Si vous exécutez ces versions et éditions :|Vous pouvez effectuer une mise à niveau vers ces versions et éditions :|
|--------------------------------|---------------------------------------|
|Windows Server 2012 Standard|Windows Server 2016 Standard ou Datacenter|
|Windows Server 2012 Datacenter|Windows Server 2016 Datacenter|
|Windows Server 2012 R2 Standard|Windows Server 2016 Standard ou Datacenter|
|Windows Server 2012 R2 Datacenter|Windows Server 2016 Datacenter|
|Hyper-V Server 2012 R2|Hyper-V Server 2016 (à l’aide de la fonctionnalité Mise à niveau propagée du système d’exploitation de cluster)|
|Windows Server 2012 R2 Essentials|Windows Server 2016 Essentials|
|Windows Storage Server 2012 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 Workgroup|Windows Storage Server 2016 Workgroup|
|Windows Storage Server 2012 R2 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 R2 Workgroup|Windows Storage Server 2016 Workgroup|
 
### <a name="license-conversion"></a>Conversion de licence
Vous pouvez convertir Windows Server 2016 Standard (version commerciale) vers Windows Server 2016 Datacenter (version commerciale).

Vous pouvez convertir Windows Server 2016 Essentials (version commerciale) en Windows Server 2016 Standard (version commerciale).

Vous pouvez convertir la version d’évaluation de Windows Server 2016 Standard vers Windows Server 2016 Standard (version commerciale) ou Datacenter (version commerciale).

Vous pouvez convertir la version d’évaluation de Windows Server 2016 Datacenter vers Windows Server 2016 Datacenter (version commerciale).
 
## <a name="upgrading-to-windows-server-2012-r2"></a>Mise à niveau vers Windows Server 2012 R2
Pour plus d’informations, y compris les réserves importantes et les restrictions applicables à la mise à niveau, la conversion de licence entre éditions de Windows Server 2012 R2 et la conversion de versions d’évaluation vers une version commerciale, consultez [Options de mise à niveau pour Windows Server 2012 R2](https://technet.microsoft.com/library/dn303416.aspx).

Tableau de référence rapide des chemins de mise à niveau pris en charge pour des anciennes versions commerciales de Windows Server vers des versions commerciales de Windows Server 2012 R2 :

|Si votre ordinateur fonctionne sous :|Vous pouvez effectuer une mise à niveau vers ces éditions :|
|-------------------------|---------------------------|
|Windows Server 2008 R2 Datacenter avec SP1|Windows Server 2012 R2 Datacenter|
|Windows Server 2008 R2 Entreprise avec SP1|Windows Server 2012 R2 Standard ou Windows Server 2012 R2 Datacenter|
|Windows Server 2008 R2 Standard avec SP1|Windows Server 2012 R2 Standard ou Windows Server 2012 R2 Datacenter|
|Windows Web Server 2008 R2 avec SP1|Windows Server 2012 R2 Standard|
|Windows Server 2012 Datacenter|Windows Server 2012 R2 Datacenter|
|Windows Server 2012 Standard|Windows Server 2012 R2 Standard ou Windows Server 2012 R2 Datacenter|
|Hyper-V Server 2012|Hyper-V Server 2012 R2|

### <a name="license-conversion"></a>Conversion de licence
Vous pouvez convertir Windows Server 2012 Standard (version commerciale) en Windows Server 2012 Datacenter (version commerciale).

Vous pouvez convertir Windows Server 2012 Essentials (version commerciale) en Windows Server 2012 Standard (version commerciale).

Vous pouvez convertir la version d’évaluation de Windows Server 2012 Standard en Windows Server 2012 Standard (version commerciale) ou Datacenter (version commerciale).

## <a name="upgrading-to-windows-server-2012"></a>Mise à niveau vers Windows Server 2012
Pour plus d’informations, y compris les réserves importantes et les limitations applicables à la mise à niveau et la conversion de versions d’évaluation vers une version commerciale, consultez [Versions d’évaluation et options de mise à niveau pour Windows Server 2012](https://technet.microsoft.com/library/jj574204.aspx).
 
Tableau de référence rapide des chemins de mise à niveau pris en charge pour des anciennes versions commerciales de Windows Server vers des versions commerciales de Windows Server 2012 :

|Si votre ordinateur fonctionne sous :|Vous pouvez effectuer une mise à niveau vers ces éditions :|
|--------------------------|--------------------------|
|Windows Server 2008 Standard avec SP2 ou Windows Server 2008 Entreprise avec SP2|Windows Server 2012 Standard, Windows Server 2012 Datacenter|
|Windows Server 2008 Datacenter avec SP2|Windows Server 2012 Datacenter|
|Windows Web Server 2008|Windows Server 2012 Standard|
|Windows Server 2008 R2 Standard avec SP1 ou Windows Server 2008 R2 Entreprise avec SP1|Windows Server 2012 Standard, Windows Server 2012 Datacenter|
|Windows Server 2008 R2 Datacenter avec SP1|Windows Server 2012 Datacenter|
|Windows Web Server 2008 R2|Windows Server 2012 Standard|

### <a name="license-conversion"></a>Conversion de licence
Vous pouvez convertir Windows Server 2012 Standard (version commerciale) en Windows Server 2012 Datacenter (version commerciale).

Vous pouvez convertir Windows Server 2012 Essentials (version commerciale) en Windows Server 2012 Standard (version commerciale).

Vous pouvez convertir la version d’évaluation de Windows Server 2012 Standard en Windows Server 2012 Standard (version commerciale) ou Datacenter (version commerciale).

## <a name="upgrading-from-windows-server-2008-r2-or-windows-server-2008"></a>Mise à niveau depuis Windows Server 2008 R2 ou Windows Server 2008

Comme décrit dans [Mise à niveau de Windows Server 2008 et Windows Server 2008 R2](modernize-windows-server-2008.md), le support étendu pour Windows Server 2008 R2/Windows Server 2008 se termine de janvier 2020. Pour vous assurer qu’il n’y a aucune interruption dans le support, vous devez effectuer une mise à niveau vers une version prise en charge de Windows Server ou procéder à un ré-hébergement dans Azure en migrant vers des [machines virtuelles Windows Server 2008 R2 spécialisées](uploading-specialized-WS08-image-to-azure.md). Consultez le [Guide de migration de Windows Server](https://go.microsoft.com/fwlink/?linkid=872689) pour obtenir des informations et des conseils pour la planification de votre migration/mise à niveau.

Pour les serveurs locaux, il n’y a aucun parcours de mise à niveau direct de Windows Server 2008 R2 vers Windows Server 2016 ou version ultérieure. Pour cela, vous devez d’abord procéder à la mise à niveau vers Windows Server 2012 R2, puis effectuer une [mise à niveau vers Windows Server 2016](#upgrading-to-windows-server-2016).

Lorsque vous prévoyez votre mise à niveau, tenez compte des instructions suivantes pour l’étape intermédiaire de la mise à niveau vers Windows Server 2012 R2.

  - Vous ne pouvez pas effectuer de mise à niveau sur place d’une architecture 32 bits vers 64 bits ou d’un type de version à une autre (français vers tchèque, par exemple).

  - Les mises à niveau sur place sont uniquement possibles dans la même langue. Vous ne pouvez pas procéder à une mise à niveau d’une langue à une autre.

  - Vous ne pouvez pas migrer à partir d’une installation minimale de Windows Server 2008 vers Windows Server 2012 R2 avec l’interface utilisateur graphique de serveur (appelée « Serveur avec bureau complet » dans Windows Server). Vous pouvez basculer votre installation minimale mise à niveau vers Server avec bureau complet, mais uniquement sur Windows Server 2012 R2. Windows Server 2016 et les versions ultérieures *ne prennent pas en charge* le passage de Server au bureau complet, assurez-vous donc que le basculement soit effectué avant la mise à niveau vers Windows Server 2016.
  
Pour plus d’informations, consultez [Versions d’évaluation et options de mise à niveau pour Windows Server 2012](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574204\(v=ws.11\)), qui comprend des détails de mise à niveau spécifiques aux rôles.

