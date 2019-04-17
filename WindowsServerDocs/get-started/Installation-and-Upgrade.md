---
title: Installation et mise à niveau de WindowsServer
description: ''
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.date: 07/12/2018
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 98f876bd-63ff-4c3a-95d4-a8dd8d0d119c
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: c3b9070fc6cb9227ccfa445e23983d9e91fe5c82
ms.sourcegitcommit: 07ac08dea2b8f2763c2614a999dc7967018aa0b4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/07/2018
ms.locfileid: "6121478"
---
# Installation et mise à niveau de WindowsServer

>S’appliqueà: Windows Server2016, WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2, WindowsServer2008

> [!IMPORTANT]
> Le support étendu de Windows Server2008R2 et de Windows Server2008 prend fin en janvier2020. [En savoir plus sur les options de mise à niveau](#upgrading-from-windows-server-2008-r2-or-windows-server-2008).

Il est peut-être temps de passer à une version plus récente de Windows Server. Selon la version que exécutez actuellement, vous disposez de nombreuses options de mise à niveau.

## Installation
Si vous souhaitez effectuer une mise à niveau vers une version plus récente de WindowsServer sur le même matériel, une procédure infaillible consiste à effectuer une **nouvelle installation**, où vous installez simplement la dernière version du système d’exploitation directement sur l’ancienne version, sur le même matériel, en supprimant ainsi le système d’exploitation précédent. Cette procédure est la plus simple, mais vous devez commencer par sauvegarder vos données et il vous faudra ensuite réinstaller vos applications. Vous devez prendre en compte plusieurs éléments, notamment la configuration système requise. Par conséquent, veillez à consulter les informations relatives à [WindowsServer2016](https://go.microsoft.com/fwlink/?LinkID=825558), [WindowsServer2012R2](https://technet.microsoft.com/library/dn303418), et [WindowsServer2012](https://technet.microsoft.com/library/jj134246.aspx).

La mise à niveau d’une version précommerciale (par exemple, WindowsServer2016 Technical Preview) vers la version finale (WindowsServer2016) requiert toujours une nouvelle installation.

## Migration (recommandée pour WindowsServer2016)

La documentation relative à la migration de WindowsServer vous aide à migrer un rôle ou une fonctionnalité à la fois d’un ordinateur source qui exécute WindowsServer vers un autre ordinateur cible qui exécute la même version ou une version plus récente de WindowsServer. À ces fins, la migration est définie comme étant le déplacement d’un rôle ou d’une fonctionnalité et des données associées vers un autre ordinateur, et non pas la mise à niveau de la fonctionnalité sur le même ordinateur. Il s’agit de la méthode recommandée pour déplacer votre charge de travail existante et les données associées vers une version plus récente de WindowsServer. Pour commencer, vérifiez le [tableau sur la mise à niveau et la migration des rôles serveur](https://go.microsoft.com/fwlink/?LinkId=828595) pour WindowsServer2016.

## Mise à niveau propagée du système d’exploitation de cluster
La mise à niveau propagée de système d’exploitation de cluster est une nouvelle fonctionnalité de Windows Server2016 qui permet à un administrateur de mettre à niveau le système d’exploitation des nœuds de cluster Windows Server2016R2 vers Windows Server2012, sans arrêter les charges de travail du serveur de fichiers avec montée en puissance parallèle ou Hyper-V. Cette fonctionnalité élimine les temps d’arrêt susceptibles d’affecter les contrats de niveau de service. Cette nouvelle fonctionnalité est décrite plus en détail dans l’article [Mise à niveau propagée de système d’exploitation de cluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade).

## Conversion de licence
Dans certaines versions de systèmes d’exploitation, vous pouvez convertir une édition particulière de la version vers une autre édition de la même version en une seule étape à l’aide d’une simple commande et de la clé de licence appropriée. Cette opération est appelée **conversion de licence**. Par exemple, si votre serveur exécute WindowsServer2016Standard, vous pouvez effectuer une conversion vers WindowsServer2016Datacenter. Pour certaines versions de WindowsServer, vous pouvez également effectuer librement des conversions entre les versions OEM, de licence en volume et commerciales avec la même commande et la clé appropriée.

## Mise à niveau
Si vous souhaitez conserver le même matériel et tous les rôles serveur que vous avez configurés sans aplatir le serveur, la **mise à niveau** est une option et il existe de nombreuses façons de procéder. Lors de la mise à niveau standard, vous évoluez d’un ancien système d’exploitation vers une version plus récente, en conservant vos paramètres, rôles serveur et données inchangés. Par exemple, si votre serveur exécute WindowsServer2012R2, vous pouvez le mettre à niveau vers WindowsServer2016. Toutefois, il n’existe pas de chemin de mise à niveau vers toutes les versions plus récentes pour certains systèmes d’exploitation plus anciens.
 
>[!NOTE]
>La mise à niveau convient le mieux aux machines virtuelles qui ne nécessitent pas de pilotes de matériel OEM spécifiques pour que l’opération réussisse.
 
Vous pouvez effectuer une mise à niveau à partir d’une version d’évaluation du système d’exploitation vers une version commercialisée, d’une version commercialisée plus ancienne vers une version plus récente ou, dans certains cas, d’une édition de licence en volume du système d’exploitation vers une version commercialisée ordinaire.

Avant de commencer une mise à niveau, passez en revue les tableaux de cette page pour vérifier comment effectuer la mise à niveau souhaitée à partir de votre configuration actuelle.

Pour plus d’informations sur les différences entre les options d’installation disponibles pour WindowsServer2016Technical Preview, notamment les fonctionnalités installées avec chaque option et les options de gestion disponibles après l’installation, consultez [WindowsServer2016](https://go.microsoft.com/fwlink/?LinkId=828598).

>[!NOTE]
>Chaque fois que vous migrez ou effectuez une mise à niveau vers une version de WindowsServer, vous devez passer en revue et comprendre la [politique de support](https://support.microsoft.com/lifecycle) et le cycle de vie de cette version et planifier la procédure en conséquence. Vous pouvez [rechercher les informations de cycle de vie](https://support.microsoft.com/lifecycle) pour la version de WindowsServer qui vous intéresse.
 
 
## Mise à niveau vers WindowsServer2016
Pour plus d’informations, y compris les réserves importantes et les restrictions applicables à la mise à niveau, la conversion de licence entre éditions de WindowsServer2016 et la conversion de versions d’évaluation vers une version commerciale, consultez [Options de mise à niveau et de conversion pour WindowsServer2016](https://go.microsoft.com/fwlink/?LinkId=828602).
 
>[!NOTE]
>Remarque: les mises à niveau qui passent d’une installation minimale à une installation Serveur avec Expérience utilisateur (et vice versa) ne sont pas prises en charge. Si le système d’exploitation plus ancien à partir duquel vous effectuez la mise à niveau ou la conversion est une installation minimale, le résultat sera toujours une installation minimale du système d’exploitation plus récent.
 
Tableau de référence rapide des chemins de mise à niveau pris en charge pour des anciennes versions commerciales de WindowsServer vers des versions commerciales de WindowsServer2016:


|Si vous exécutez ces versions et éditions:|Vous pouvez effectuer une mise à niveau vers ces versions et éditions:|
|--------------------------------|---------------------------------------|
|WindowsServer2012Standard|WindowsServer2016 Standard ou Datacenter|
|WindowsServer2012 Datacenter|WindowsServer2016 Datacenter|
|WindowsServer2012R2 Standard|WindowsServer2016 Standard ou Datacenter|
|WindowsServer2012R2 Datacenter|WindowsServer2016Datacenter|
|Hyper-V Server2012R2|Hyper-V Server2016 (à l’aide de la fonctionnalité Mise à niveau propagée du système d’exploitation de cluster)|
|WindowsServer2012R2Essentials|WindowsServer2016 Essentials|
|Windows Storage Server2012 Standard|Windows Storage Server2016 Standard|
|Windows Storage Server2012 Workgroup|Windows Storage Server2016 Workgroup|
|Windows Storage Server2012R2 Standard|Windows Storage Server2016 Standard|
|Windows Storage Server2012R2 Workgroup|WindowsStorageServer2016Workgroup|
 
### Conversion de licence
Vous pouvez convertir WindowsServer2016Standard (version commerciale) vers WindowsServer2016Datacenter (version commerciale).

Vous pouvez convertir WindowsServer2016Essentials (version commerciale) vers WindowsServer2016Standard (version commerciale).

Vous pouvez convertir la version d’évaluation de WindowsServer2016Standard vers WindowsServer2016Standard (version commerciale) ou Datacenter(version commerciale).

Vous pouvez convertir la version d’évaluation de WindowsServer2016Datacenter vers WindowsServer2016Datacenter (version commerciale).
 
## Mise à niveau vers WindowsServer2012R2
Pour plus d’informations, y compris les réserves importantes et les restrictions applicables à la mise à niveau, la conversion de licence entre éditions de WindowsServer2012R2 et la conversion de versions d’évaluation vers une version commerciale, consultez [Options de mise à niveau pour WindowsServer2012R2](https://technet.microsoft.com/library/dn303416.aspx).

Tableau de référence rapide des chemins de mise à niveau pris en charge pour des anciennes versions commerciales de WindowsServer vers des versions commerciales de WindowsServer2012R2:

|Si votre ordinateur fonctionne sous:|Vous pouvez effectuer une mise à niveau vers ces éditions:|
|-------------------------|---------------------------|
|Windows Server2008R2 Datacenter avec SP1|WindowsServer 2012 R2 Datacenter|
|Windows Server2008R2 entreprise avec SP1|WindowsServer2012R2Standard ou WindowsServer2012R2Datacenter|
|Windows Server2008R2 Standard avec SP1|WindowsServer2012R2Standard ou WindowsServer2012R2Datacenter|
|Server2008R2 Web de Windows avec SP1|WindowsServer2012R2Standard|
|WindowsServer2012Datacenter|WindowsServer2012R2Datacenter|
|WindowsServer2012Standard|WindowsServer2012R2Standard ou WindowsServer2012R2Datacenter|
|Hyper-VServer2012|Hyper-V Server2012R2|

### Conversion de licence
Vous pouvez convertir WindowsServer2012Standard (version commerciale) vers WindowsServer2012Datacenter (version commerciale).

Vous pouvez convertir WindowsServer2012Essentials (version commerciale) vers WindowsServer2012Standard (version commerciale).

Vous pouvez convertir la version d’évaluation de WindowsServer2012Standard vers WindowsServer2012Standard (version commerciale) ou Datacenter(version commerciale).

## Mise à niveau vers WindowsServer2012
Pour plus d’informations, y compris les réserves importantes et les limitations applicables à la mise à niveau et la conversion de versions d’évaluation vers une version commerciale, consultez [Versions d’évaluation et options de mise à niveau pour WindowsServer2012](https://technet.microsoft.com/library/jj574204.aspx).
 
Tableau de référence rapide des chemins de mise à niveau pris en charge pour des anciennes versions commerciales de WindowsServer vers des versions commerciales de WindowsServer2012:

|Si votre ordinateur fonctionne sous:|Vous pouvez effectuer une mise à niveau vers ces éditions:|
|--------------------------|--------------------------|
|WindowsServer2008Standard avec SP2 ou WindowsServer2008Entreprise avec SP2|WindowsServer2012Standard ou WindowsServer2012Datacenter|
|WindowsServer2008Datacenter avec SP2|WindowsServer2012Datacenter|
|WindowsWebServer2008|WindowsServer2012Standard|
|Windows Server2008R2 Standard avec SP1 ou Windows Server2008R2 entreprise avec SP1|WindowsServer2012Standard ou WindowsServer2012Datacenter|
|Windows Server2008R2 Datacenter avec SP1|WindowsServer2012Datacenter|
|Windows Web Server2008R2|WindowsServer2012Standard|

### Conversion de licence
Vous pouvez convertir WindowsServer2012Standard (version commerciale) vers WindowsServer2012Datacenter (version commerciale).

Vous pouvez convertir WindowsServer2012Essentials (version commerciale) vers WindowsServer2012Standard (version commerciale).

Vous pouvez convertir la version d’évaluation de WindowsServer2012Standard vers WindowsServer2012Standard (version commerciale) ou Datacenter(version commerciale).

## La mise à niveau à partir de Windows Server 2008 R2 ou Windows Server 2008

Comme décrit dans la [mise à niveau de Windows Server 2008 et Windows Server 2008 R2](modernize-windows-server-2008.md), le support étendu pour Windows Server 2008 R2 ou Windows Server 2008 prend fin en janvier de 2020. Pour vous assurer sans intervalle prise en charge, vous devez mettre à niveau vers une version prise en charge de Windows Server, ou un réhébergement dans Azure en déplaçant vers [Spécialisé machines virtuelles Windows Server 2008 R2](uploading-specialized-WS08-image-to-azure.md). Consultez le [Guide de Migration de Windows Server](https://go.microsoft.com/fwlink/?linkid=872689) pour plus d’informations et planifier votre migration/mise à niveau.

Pour les serveurs locaux, il n’existe aucune mise à niveau directe à partir de Windows Server 2008 R2 vers Windows Server 2016 ou version ultérieure. Au lieu de cela, abord mettre à niveau vers Windows Server 2012 R2, puis [mettre à niveau vers Windows Server 2016](#Upgrading-to-Windows-Server-2016).

Que vous prévoyez de votre mise à niveau, tenez compte des recommandations suivantes pour l’étape du milieu de la mise à niveau vers Windows Server 2012 R2.

  - Vous ne peut pas effectuer une mise à niveau sur place à partir d’un architectures 32 bits vers 64 bits ou à partir d’un build type à un autre (fre chk, par exemple).

  - Mises à niveau sur place sont uniquement pris en charge dans la même langue. Vous ne peut pas mettre à niveau à partir d’une langue vers une autre.

  - Vous ne pouvez pas migrer à partir d’une installation server core de Windows Server 2008 vers Windows Server 2012 R2 avec l’interface utilisateur graphique de serveur (appelé «Serveur avec bureau complet» dans Windows Server). Vous pouvez basculer votre installation de core serveur mis à niveau vers serveur avec expérience utilisateur complète, mais uniquement sur Windows Server 2012 R2. Windows Server 2016 et versions ultérieur *ne le faites pas* prise en charge de passage du minimale au bureau complète, assurez-vous que le commutateur avant de passer à Windows Server 2016.
  
Pour plus d’informations, consultez les [Versions d’évaluation et Options de mise à niveau pour Windows Server 2012](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574204\(v=ws.11\)), qui inclut des détails de mise à niveau de rôle spécifique.

