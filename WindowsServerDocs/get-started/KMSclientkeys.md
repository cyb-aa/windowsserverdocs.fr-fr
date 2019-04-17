---
title: Clés d’installation du client KMS
description: Clés nécessaires pour activer les produits Windows à partir d’un serveur KMS
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: jaimeo
ms.localizationpriority: medium
ms.date: 10/02/2018
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.openlocfilehash: 57ce4c4d7623c2a424efbdf0ff117ede8fad726b
ms.sourcegitcommit: c5373927c645a4d5e79bfff70666ccd10f034cbe
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/22/2018
ms.locfileid: "5425193"
---
# Clés d'installation du client KMS

>S’applique à: Windows Server 2019, canal semi-annuel de Windows Server, Windows Server 2016, Windows 10

Les ordinateurs qui exécutent des éditions de licence en volume de WindowsServer, Windows10, Windows8.1, WindowsServer2012R2, Windows8, WindowsServer2012, Windows7, WindowsServer2008R2, WindowsVista et WindowsServer2008 sont, par défaut, des clients KMS sans aucune configuration supplémentaire requise.

>[!NOTE]
> Dans les tableaux qui suivent, «LTSC» correspond à un «À long terme canal de maintenance,» tandis que «LTSB» fait référence aux «Branche maintenance à long terme». 

**Pour utiliser les clés répertoriées ici (clés GVLK), vous devez avoir un hôteKMS s’exécutant déjà dans votre déploiement.** Si vous n’avez pas encore configuré d’hôte KMS, voir [Déployer l’activation KMS](https://technet.microsoft.com/library/dn502531(v=ws.11).aspx) pour connaître la procédure de configuration à suivre.

Si vous convertissez un ordinateur depuis un hôte KMS, une clé d’activation multiple (MAK) ou une version commerciale de Windows en un client KMS, installez la clé d’installation applicable (GVLK) à partir des tableaux suivants. Pour installer une clé d’installation de client, ouvrez une invite de commandes d’administration sur le client, tapez **slmgr /ipk \<clé d’installation\>** et appuyez sur **Entrée**.

| Si vous souhaitez...                                                                                                                                                                                          | …utilisez ces ressources                                                                                                         |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
| Activer Windows en dehors d’un scénario d’activation en volume (essayer d’activer une version commerciale de Windows), **ces clés ne fonctionneront pas**.                                                     | Utilisez ces liens pour les versions commerciales de Windows:                                                                              |
| Corriger l’erreur suivante qui s’affiche quand vous tentez d’activer un système Windows8.1, WindowsServer2012R2 ou plus récent: «Erreur: 0xC004F050 Le service de gestion de licences a signalé que la clé de produit n’est pas valide.» | [Installer cette mise à jour](https://support.microsoft.com/en-us/help/3172614/july-2016-update-rollup-for-windows-8-1-and-windows-server-2012-r2) sur l’hôte KMS s’il exécute Windows8.1, WindowsServer2012R2, Windows8 ou WindowsServer2012. |

-   [Obtenir Windows10](https://www.microsoft.com/en-us/windows/get-windows-10)

-   [Obtenir une nouvelle clé de produit Windows](https://support.microsoft.com/help/10749/windows-product-key)

-   [Procédures et aide sur la copie authentique de Windows](https://support.microsoft.com/help/15087/windows-genuine)


>   Si vous exécutez WindowsServer 2008R2 ou Windows7, recherchez une mise à jour permettant d’utiliser ces derniers comme hôtes KMS pour les clients Windows10.


## Versions de Windows Server (canal semi-annuel)

### Windows Server, version 1809
| Édition du système d’exploitation       | Clé d’installation du client KMS          |
|--------------------------------|-------------------------------|
| WindowsServerDatacenter | 6NMRW-2C8FM-D24W7-TQWMY-CWH2D  | 
| WindowsServerStandard | N2KJX-J94YW-TQVFB-DG9YT-724CC  |


### WindowsServer, version1803

| Édition du système d’exploitation       | Clé d’installation du client KMS          |
|--------------------------------|-------------------------------|
| WindowsServerDatacenter | 2HXDN-KRXHB-GPYC7-YCKFJ-7FVDG  | 
| WindowsServerStandard   | PTXN8-JFHJM-4WC78-MPCBR-9W4KR  |

### WindowsServer, version1709

| Édition du système d’exploitation       | Clé d’installation du client KMS          |
|--------------------------------|-------------------------------|
| WindowsServerDatacenter | 6Y6KB-N82V8-D8CQV-23MJW-BWTG6  | 
| WindowsServerStandard   | DPCNP-XQFKJ-BJF7R-FRC8D-GF6G4  |

## Versions de Windows Server LTSC/LTSB

### Windows Server 2019
| Édition du système d’exploitation       | Clé d’installation du client KMS          |
|--------------------------------|-------------------------------|
| Windows Server 2019 Datacenter | WMDGN-G9PQG-XVVXX-R3X43-63DFG  | 
| Windows Server 2019 Standard   | N69G4-B89J2-4G8F4-WWYCC-J464C  |
| Windows Server 2019 Essentials|WVDHN-86M7X-466P 6-VHXV7-YY726|

### WindowsServer2016

| Édition du système d’exploitation       | Clé d’installation du client KMS          |
|--------------------------------|-------------------------------|
| WindowsServer2016Datacenter | CB7KF-BWN84-R7R2Y-793K2-8XDDG |
| WindowsServer2016 Standard   | WC2BQ-8NRM3-FDDYY-2BFGV-KHKQY |
| WindowsServer2016 Essentials | JCKRF-N37P4-C2D82-9YXRT-4M63B |

## Windows 10, toutes les versions prises en charge (canal semi-annuel)

Consultez la [fiche d’informations de cycle de vie de Windows](https://support.microsoft.com/en-us/help/13853/windows-lifecycle-fact-sheet) pour plus d’informations sur les versions prises en charge et à la fin des dates de service.

| Édition du système d’exploitation          | Clé d’installation du client KMS          |
|-----------------------------------|-------------------------------|
|Windows10Professionnel|W269N-WFGWX-YVC9B-4J6C9-T83GX|
|Windows 10 Professionnel N|MH37W-N47XK-V7XM9-C7227-GCQG9|
|Stations de travail Windows 10 Professionnel.|NRG8B-VKK3Q-CXVCJ-9G2XF-6Q84J|
|Stations de travail Windows 10 Professionnel N|9FNHH-K3HBT-3W4TD-6383H-6XYWF|
|Windows10 Professionnel Éducation|6TP4R-GNPTD-KYYHQ-7B7DP-J447Y|
|Windows 10 Professionnel éducation N|YVWGF-BXNMC-HTQYQ-CPQ99-66QFC|
|Windows10 Éducation|NW6C2-QMPVW-D7KKK-3GKT6-VCFB2|
|Windows10ÉducationN |2WH4N-8QGBV-H22JP-CT43Q-MDWWJ|
|Windows10 Entreprise  |NPPR9-FWDCX-D2C8J-H872K-2YT43|
|Windows10EntrepriseN    |DPH2V-TTNVB-4X9Q3-TJR4H-KHJW4|
|Windows10 Entreprise G|YYVX9-NTFWV-6MDM3-9PT4T-4M68B|
|Windows10 Entreprise G N|44RPN-FTY23-9VTTB-MP9BX-T84FV|

## Versions de Windows 10 LTSC/LTSB

### Windows 10 LTSC 2019

|Édition du système d’exploitation|Clé d’installation du client KMS|
|-|-|
|Windows 10 entreprise LTSC 2019|M7XTQ-FN8P6-TTKYV-9D4CC-J462D|
|Windows 10 entreprise N LTSC 2019|92NFX-8DJQP-P6BBQ-THF9C-7CG2H|

### Windows 10 LTSB 2016

|Édition du système d’exploitation|Clé d’installation du client KMS|
|-|-|
|Windows 10 entreprise 2016 LTSB|DCPHK-NFMTC-H88MJ-PFHPY-QJ4BJ|
|Windows 10 entreprise N LTSB 2016|QFFDN-GRT3P-VKWWX-X7T3R-8B639|

### Windows 10 LTSB 2015 

| Édition du système d’exploitation          | Clé d’installation du client KMS          |
|-----------------------------------|-------------------------------|
| Windows 10 Entreprise 2015 LTSB   | WNMTR-4C88C-JK8YV-HQ7T2-76DF9 |
| Windows 10 Entreprise 2015 LTSB N | 2F77B-TNFGY-69QQF-B8YKP-D69TJ |

## Versions antérieures de Windows Server
### WindowsServer2012R2

| Édition du système d’exploitation               | Clé d’installation du client KMS          |
|----------------------------------------|-------------------------------|
| WindowsServer2012R2 Server Standard | D2N9P-3P6X9-2R39C-7RTCD-MDVJX |
| WindowsServer 2012 R2 Datacenter      | W3GGN-FT8W3-Y4M27-J84CP-Q3VJ9 |
| WindowsServer2012R2Essentials      | KNC87-3J2TX-XB4WP-VCPJV-M4FWM |

### WindowsServer2012

| Édition du système d’exploitation                | Clé d’installation du client KMS          |
|-----------------------------------------|-------------------------------|
| WindowsServer2012                     | BN3D2-R7TKB-3YPBD-8DRP2-27GG4 |
| WindowsServer2012N                   | 8N2M2-HWPGY-7PGT9-HGDD8-GVGGY |
| WindowsServer2012 unilingue     | 2WN2H-YGCQR-KFX6K-CD6TF-84YXQ |
| WindowsServer2012 spécifique au pays    | 4K36P-JN4VD-GDC6V-KDT89-DYFKP |
| WindowsServer2012Server Standard     | XC9B7-NBPP2-83J2H-RHMBY-92BT4 |
| WindowsServer2012 MultiPoint Standard | HM7DN-YVMH3-46JC3-XYTG7-CYQJJ |
| WindowsServer2012MultiPointPremium  | XNH6W-2V9GX-RGJ4K-Y8X6F-QGJ2G |
| WindowsServer2012Datacenter          | 48HP8-DN98B-MYWDG-T2DCC-8W83P |


### Windows Server2008R2

| Édition du système d’exploitation                         | Clé d’installation du client KMS          |
|--------------------------------------------------|-------------------------------|
| WindowsServer2008R2 Web                       | 6TPJF-RBVHG-WBW2R-86QPH-6RTM4 |
| WindowsServer2008R2 HPC Edition               | TT8MH-CG224-D3D7Q-498W2-9QCTX |
| WindowsServer 2008R2Standard                  | YC6KT-GKW9T-YTKYR-T4X34-R7VHC |
| WindowsServer 2008R2Entreprise                | 489J6-VHDMP-X63PK-3K798-CPX3Y |
| WindowsServer 2008R2Datacenter                | 74YFP-3QFB3-KQT8W-PMXWJ-7M648 |
| WindowsServer2008R2 pour les systèmes Itanium | GT63C-RJFQ3-4GMB6-BRFB9-CB83V |

### WindowsServer2008

| Édition du système d’exploitation                       | Clé d’installation du client KMS          |
|------------------------------------------------|-------------------------------|
| WindowsWebServer2008                        | WYR28-R7TFJ-3X2YQ-YCY4H-M249D |
| WindowsServer2008Standard                   | TM24T-X9RMF-VWXK6-X8JC9-BFGM2 |
| WindowsServer 2008 Standard sans Hyper\-V   | W7VD6-7JFBR-RX26B-YKQ3Y-6FFFJ |
| WindowsServer2008 Entreprise                 | YQGMW-MPWTJ-34KDK-48M3W-X4Q6V |
| WindowsServer 2008 Entreprise sans Hyper\-V | 39BXF-X8Q23-P2WWT-38T2F-G3FPG |
| WindowsServer2008HPC                        | RCTX3-KWVHP-BR6TB-RB6DM-6X7HP |
| WindowsServer2008Datacenter                 | 7M67G-PC374-GR742-YH8V4-TCBY3 |
| WindowsServer 2008 Datacenter sans Hyper\-V | 22XQ2-VRXRG-P8D42-K34TD-G3QQC |
| WindowsServer 2008 pour les systèmes Itanium  | 4DWFP-JF3DJ-B7DTH-78FJB-PDRHK |

## Versions antérieures de Windows

### Windows8.1

| Édition du système d’exploitation               | Clé d’installation du client KMS          |
|----------------------------------------|-------------------------------|
| Windows8.1 Professionnel               | GCRJD-8NW9H-F2CDX-CCM8D-9D6T9 |
| Windows 8.1 Professionnel N             | HMCNV-VVBFX-7HMBH-CTY9B-B4FXY |
| Windows8.1Entreprise                 | MHF9N-XY6XB-WVXMC-BTDCT-MKKG7 |
| Windows8.1Entreprise N               | TT4HM-HN7YT-62K67-RGRQJ-JFFXW |

### Windows8

| Édition du système d’exploitation                | Clé d’installation du client KMS          |
|-----------------------------------------|-------------------------------|
| Windows8 Professionnel                  | NG4HW-VH26C-733KW-K6F98-J8CK4 |
| Windows 8 Professionnel N                | XCVCF-2NXM9-723PB-MHCB7-2RYQQ |
| Windows8Entreprise                    | 32JNW-9KQ84-P47T8-D8GGY-CWCK7 |
| Windows8EntrepriseN                  | JMNMF-RHW7P-DMY6X-RF3DR-X2BQT |


### Windows7 

| Édition du système d’exploitation                         | Clé d’installation du client KMS          |
|--------------------------------------------------|-------------------------------|
| Windows7 Professionnel                           | FJ82H-XT6CR-J8D7P-XQJJ2-GPDD4 |
| Windows7ProfessionnelN                         | MRPKT-YTG23-K7D7T-X2JMM-QY7MG |
| Windows7 ProfessionnelE                         | W82YF-2Q76Y-63HXB-FGJG9-GF7QX |
| Windows7 Entreprise                             | 33PXH-7Y6KF-2VJC9-XBBR8-HVTHH |
| Windows7EntrepriseN                           | YDRBP-3D83W-TY26F-D46B2-XCKRJ |
| Windows7 EntrepriseE                           | C29WB-22CC8-VJ326-GHFJW-H9DH4 |


Articles associés

• [Plan d’activation en volume](https://technet.microsoft.com/library/jj134042(v=ws.11).aspx)


