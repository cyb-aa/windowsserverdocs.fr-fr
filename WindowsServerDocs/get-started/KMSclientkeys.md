---
title: Clés d’installation du client KMS
description: Clés nécessaires pour activer les produits Windows à partir d’un serveur KMS
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.date: 11/12/2019
ms.topic: get-started-article
ms.openlocfilehash: f2320b80fb372a227098f952dc8e7f0758420f34
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75947826"
---
# <a name="kms-client-setup-keys"></a>Clés d’installation du client KMS

>S'applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016, Windows 10

Les ordinateurs qui exécutent des éditions de licence en volume de Windows Server, Windows 10, Windows 8.1, Windows Server 2012 R2, Windows 8, Windows Server 2012, Windows 7, Windows Server 2008 R2, Windows Vista et Windows Server 2008 sont, par défaut, des clients KMS sans aucune configuration supplémentaire requise.

> [!NOTE]
> Dans les tableaux qui suivent, « LTSC » est l’acronyme de « Long-Term Servicing Channel » (Canal de maintenance à long terme), tandis que « LTSB » signifie « Long-Term Servicing Branch » (Branche de maintenance à long terme). 

**Pour utiliser les clés répertoriées ici (clés GVLK), vous devez avoir un hôte KMS s’exécutant déjà dans votre déploiement.** Si vous n’avez pas encore configuré d’hôte KMS, voir [Deploy KMS Activation](https://technet.microsoft.com/library/dn502531(v=ws.11).aspx) pour connaître la procédure de configuration à suivre.

Si vous convertissez un ordinateur depuis un hôte KMS, une clé d’activation multiple (MAK) ou une version commerciale de Windows en un client KMS, installez la clé d’installation applicable (GVLK) à partir des tableaux suivants. Pour installer une clé d’installation de client, ouvrez une invite de commandes d’administration sur le client, tapez **slmgr /ipk \<setup key\>** et appuyez sur **Entrée**.

| Si vous souhaitez...    | …utilisez ces ressources   |
|--------------------|------------------------|
| Activer Windows en dehors d’un scénario d’activation en volume (essayer d’activer une version commerciale de Windows), **ces clés ne fonctionneront pas**. | Utilisez ces liens pour les versions commerciales de Windows : |
| Corriger l’erreur suivante qui s’affiche quand vous tentez d’activer un système Windows 8.1, Windows Server 2012 R2 ou un système plus récent : « Erreur : 0xC004F050 Le service de gestion de licences a signalé que la clé de produit n’est pas valide. » | [Installer cette mise à jour](https://support.microsoft.com/help/3172614/july-2016-update-rollup-for-windows-8-1-and-windows-server-2012-r2) sur l’hôte KMS s’il exécute Windows 8.1, Windows Server 2012 R2, Windows 8 ou Windows Server 2012. |

-   [Obtenir Windows 10](https://www.microsoft.com/windows/get-windows-10)

-   [Obtenir une nouvelle clé de produit Windows](https://support.microsoft.com/help/10749/windows-product-key)

-   [Procédures et aide sur la copie authentique de Windows](https://support.microsoft.com/help/15087/windows-genuine)


>   Si vous exécutez Windows Server 2008 R2 ou Windows 7, recherchez une mise à jour permettant d’utiliser ces derniers comme hôtes KMS pour les clients Windows 10.

## <a name="windows-server-semi-annual-channel-versions"></a>Versions du canal semi-annuel de Windows Server

### <a name="windows-server-version-1909-version-1903-and-version-1809"></a>Windows Server, version 1909, version 1903 et version 1809

| Édition du système d’exploitation  | Clé d’installation du client KMS          |
|---------------------------|-------------------------------|
| Windows Server Datacenter | 6NMRW-2C8FM-D24W7-TQWMY-CWH2D |
| Windows Server Standard   | N2KJX-J94YW-TQVFB-DG9YT-724CC |

## <a name="windows-server-ltscltsb-versions"></a>Versions de Windows Server LTSC/LTSB

### <a name="windows-server-2019"></a>Windows Server 2019
| Édition du système d’exploitation       | Clé d’installation du client KMS          |
|--------------------------------|-------------------------------|
| Windows Server 2019 Datacenter | WMDGN-G9PQG-XVVXX-R3X43-63DFG  | 
| Windows Server 2019 Standard   | N69G4-B89J2-4G8F4-WWYCC-J464C  |
| Windows Server 2019 Essentials|WVDHN-86M7X-466P6-VHXV7-YY726|

### <a name="windows-server-2016"></a>Windows Server 2016

| Édition du système d’exploitation       | Clé d’installation du client KMS          |
|--------------------------------|-------------------------------|
| Windows Server 2016 Datacenter | CB7KF-BWN84-R7R2Y-793K2-8XDDG |
| Windows Server 2016 Standard   | WC2BQ-8NRM3-FDDYY-2BFGV-KHKQY |
| Windows Server 2016 Essentials | JCKRF-N37P4-C2D82-9YXRT-4M63B |

## <a name="windows-10-all-supported-semi-annual-channel-versions"></a>Windows 10, toutes les versions du canal semi-annuel prises en charge

Consultez la [fiche d’information du cycle de vie Windows](https://support.microsoft.com/help/13853/windows-lifecycle-fact-sheet) pour plus d’informations sur les versions prises en charge et les dates de fin de service.

| Édition du système d’exploitation          | Clé d’installation du client KMS          |
|-----------------------------------|-------------------------------|
|Windows 10 Professionnel|W269N-WFGWX-YVC9B-4J6C9-T83GX|
|Windows 10 Professionnel N|MH37W-N47XK-V7XM9-C7227-GCQG9|
|Windows 10 Professionnel pour les Stations de travail|NRG8B-VKK3Q-CXVCJ-9G2XF-6Q84J|
|Windows 10 Professionnel pour les Stations de travail N|9FNHH-K3HBT-3W4TD-6383H-6XYWF|
|Windows 10 Professionnel Éducation|6TP4R-GNPTD-KYYHQ-7B7DP-J447Y|
|Windows 10 Professionnel Éducation N|YVWGF-BXNMC-HTQYQ-CPQ99-66QFC|
|Windows 10 Éducation|NW6C2-QMPVW-D7KKK-3GKT6-VCFB2|
|Windows 10 Éducation N |2WH4N-8QGBV-H22JP-CT43Q-MDWWJ|
|Windows 10 Entreprise  |NPPR9-FWDCX-D2C8J-H872K-2YT43|
|Windows 10 Entreprise N    |DPH2V-TTNVB-4X9Q3-TJR4H-KHJW4|
|Windows 10 Entreprise G|YYVX9-NTFWV-6MDM3-9PT4T-4M68B|
|Windows 10 Entreprise G N|44RPN-FTY23-9VTTB-MP9BX-T84FV|

## <a name="windows-10-ltscltsb-versions"></a>Versions de Windows 10 LTSC/LTSB

### <a name="windows-10-ltsc-2019"></a>Windows 10 LTSC 2019

|Édition du système d’exploitation|Clé d’installation du client KMS|
|-|-|
|Windows 10 Entreprise LTSC 2019|M7XTQ-FN8P6-TTKYV-9D4CC-J462D|
|Windows 10 Entreprise N LTSC 2019|92NFX-8DJQP-P6BBQ-THF9C-7CG2H|

### <a name="windows-10-ltsb-2016"></a>Windows 10 LTSB 2016

|Édition du système d’exploitation|Clé d’installation du client KMS|
|-|-|
|Windows 10 Entreprise LTSB 2016|DCPHK-NFMTC-H88MJ-PFHPY-QJ4BJ|
|Windows 10 Entreprise N LTSB 2016|QFFDN-GRT3P-VKWWX-X7T3R-8B639|

### <a name="windows-10-ltsb-2015"></a>Windows 10 LTSB 2015 

| Édition du système d’exploitation          | Clé d’installation du client KMS          |
|-----------------------------------|-------------------------------|
| Windows 10 Enterprise 2015 LTSB   | WNMTR-4C88C-JK8YV-HQ7T2-76DF9 |
| Windows 10 Enterprise 2015 LTSB N | 2F77B-TNFGY-69QQF-B8YKP-D69TJ |

## <a name="earlier-versions-of-windows-server"></a>Versions antérieures de Windows Server

### <a name="windows-server-version-1803"></a>Windows Server, version 1803

| Édition du système d’exploitation       | Clé d’installation du client KMS          |
|--------------------------------|-------------------------------|
| Windows Server Datacenter | 2HXDN-KRXHB-GPYC7-YCKFJ-7FVDG  | 
| Windows Server Standard   | PTXN8-JFHJM-4WC78-MPCBR-9W4KR  |

### <a name="windows-server-version-1709"></a>Windows Server version 1709

| Édition du système d’exploitation       | Clé d’installation du client KMS          |
|--------------------------------|-------------------------------|
| Windows Server Datacenter | 6Y6KB-N82V8-D8CQV-23MJW-BWTG6  | 
| Windows Server Standard   | DPCNP-XQFKJ-BJF7R-FRC8D-GF6G4  |

### <a name="windows-server-2012-r2"></a>Windows Server 2012 R2

| Édition du système d’exploitation               | Clé d’installation du client KMS          |
|----------------------------------------|-------------------------------|
| Windows Server 2012 R2 Server Standard | D2N9P-3P6X9-2R39C-7RTCD-MDVJX |
| Windows Server 2012 R2 Datacenter      | W3GGN-FT8W3-Y4M27-J84CP-Q3VJ9 |
| Windows Server 2012 R2 Essentials      | KNC87-3J2TX-XB4WP-VCPJV-M4FWM |

### <a name="windows-server-2012"></a>Windows Server 2012

| Édition du système d’exploitation                | Clé d’installation du client KMS          |
|-----------------------------------------|-------------------------------|
| Windows Server 2012                     | BN3D2-R7TKB-3YPBD-8DRP2-27GG4 |
| Windows Server 2012 N                   | 8N2M2-HWPGY-7PGT9-HGDD8-GVGGY |
| Windows Server 2012 unilingue     | 2WN2H-YGCQR-KFX6K-CD6TF-84YXQ |
| Windows Server 2012 spécifique au pays    | 4K36P-JN4VD-GDC6V-KDT89-DYFKP |
| Windows Server 2012 Server Standard     | XC9B7-NBPP2-83J2H-RHMBY-92BT4 |
| Windows Server 2012 MultiPoint Standard | HM7DN-YVMH3-46JC3-XYTG7-CYQJJ |
| Windows Server 2012 MultiPoint Premium  | XNH6W-2V9GX-RGJ4K-Y8X6F-QGJ2G |
| Windows Server 2012 Datacenter          | 48HP8-DN98B-MYWDG-T2DCC-8W83P |


### <a name="windows-server-2008-r2"></a>Windows Server 2008 R2

| Édition du système d’exploitation                         | Clé d’installation du client KMS          |
|--------------------------------------------------|-------------------------------|
| Windows Server 2008 R2 Web                       | 6TPJF-RBVHG-WBW2R-86QPH-6RTM4 |
| Windows Server 2008 R2 HPC Edition               | TT8MH-CG224-D3D7Q-498W2-9QCTX |
| Windows Server 2008 R2 Standard                  | YC6KT-GKW9T-YTKYR-T4X34-R7VHC |
| Windows Server 2008 R2 Entreprise                | 489J6-VHDMP-X63PK-3K798-CPX3Y |
| Windows Server 2008 R2 Datacenter                | 74YFP-3QFB3-KQT8W-PMXWJ-7M648 |
| Windows Server 2008 R2 pour les systèmes Itanium | GT63C-RJFQ3-4GMB6-BRFB9-CB83V |

### <a name="windows-server-2008"></a>Windows Server 2008

| Édition du système d’exploitation                       | Clé d’installation du client KMS          |
|------------------------------------------------|-------------------------------|
| Windows Web Server 2008                        | WYR28-R7TFJ-3X2YQ-YCY4H-M249D |
| Windows Server 2008 Standard                   | TM24T-X9RMF-VWXK6-X8JC9-BFGM2 |
| Windows Server 2008 Standard sans Hyper-V   | W7VD6-7JFBR-RX26B-YKQ3Y-6FFFJ |
| Windows Server 2008 Entreprise                 | YQGMW-MPWTJ-34KDK-48M3W-X4Q6V |
| Windows Server 2008 Entreprise sans Hyper-V | 39BXF-X8Q23-P2WWT-38T2F-G3FPG |
| Windows Server 2008 HPC                        | RCTX3-KWVHP-BR6TB-RB6DM-6X7HP |
| Windows Server 2008 Datacenter                 | 7M67G-PC374-GR742-YH8V4-TCBY3 |
| Windows Server 2008 Datacenter sans Hyper-V | 22XQ2-VRXRG-P8D42-K34TD-G3QQC |
| Windows Server 2008 pour les systèmes Itanium  | 4DWFP-JF3DJ-B7DTH-78FJB-PDRHK |

## <a name="earlier-versions-of-windows"></a>Versions antérieures de Windows

### <a name="windows-81"></a>Windows 8.1

| Édition du système d’exploitation               | Clé d’installation du client KMS          |
|----------------------------------------|-------------------------------|
| Windows 8.1 Professionnel               | GCRJD-8NW9H-F2CDX-CCM8D-9D6T9 |
| Windows 8.1 Professionnel N             | HMCNV-VVBFX-7HMBH-CTY9B-B4FXY |
| Windows 8.1 Enterprise                 | MHF9N-XY6XB-WVXMC-BTDCT-MKKG7 |
| Windows 8.1 Entreprise N               | TT4HM-HN7YT-62K67-RGRQJ-JFFXW |

### <a name="windows-8"></a>Windows 8

| Édition du système d’exploitation                | Clé d’installation du client KMS          |
|-----------------------------------------|-------------------------------|
| Windows 8 Professionnel                  | NG4HW-VH26C-733KW-K6F98-J8CK4 |
| Windows 8 Professionnel N                | XCVCF-2NXM9-723PB-MHCB7-2RYQQ |
| Windows 8 Entreprise                    | 32JNW-9KQ84-P47T8-D8GGY-CWCK7 |
| Windows 8 Entreprise N                  | JMNMF-RHW7P-DMY6X-RF3DR-X2BQT |


### <a name="windows-7"></a>Windows 7 

| Édition du système d’exploitation                         | Clé d’installation du client KMS          |
|--------------------------------------------------|-------------------------------|
| Windows 7 Professionnel                           | FJ82H-XT6CR-J8D7P-XQJJ2-GPDD4 |
| Windows 7 Professionnel N                         | MRPKT-YTG23-K7D7T-X2JMM-QY7MG |
| Windows 7 Professionnel E                         | W82YF-2Q76Y-63HXB-FGJG9-GF7QX |
| Windows 7 Enterprise                             | 33PXH-7Y6KF-2VJC9-XBBR8-HVTHH |
| Windows 7 Entreprise N                           | YDRBP-3D83W-TY26F-D46B2-XCKRJ |
| Windows 7 Entreprise E                           | C29WB-22CC8-VJ326-GHFJW-H9DH4 |


Voir aussi

• [Planifier l’activation en volume](https://technet.microsoft.com/library/jj134042(v=ws.11).aspx)


