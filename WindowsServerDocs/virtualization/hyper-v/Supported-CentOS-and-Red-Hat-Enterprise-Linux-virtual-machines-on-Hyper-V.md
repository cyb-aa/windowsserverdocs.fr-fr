---
title: Ordinateurs virtuels CentOS et Red Hat Enterprise Linux pris en charge sur Hyper-V
description: Répertorie les versions de Linux Integration Services pour les distributions CentOS et Red Hat Enterprise prises en charge
ms.prod: windows-server
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: 4bf8783d-dee5-4b3e-8cce-2b11b117c189
author: danihalfin
ms.author: vichen
ms.date: 04/06/2020
ms.openlocfilehash: 5d30f373390ffa61ebe8710de82d20107456462b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855982"
---
# <a name="supported-centos-and-red-hat-enterprise-linux-virtual-machines-on-hyper-v"></a>Ordinateurs virtuels CentOS et Red Hat Enterprise Linux pris en charge sur Hyper-V

>S’applique à : Windows Server 2019, Hyper-V Server 2019, Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows 10, Windows 8.1

Les cartes de distribution de fonctionnalités suivantes indiquent les fonctionnalités présentes dans les versions intégrées et téléchargeables de Linux Integration Services. Les problèmes connus et les solutions de contournement pour chaque distribution sont répertoriés après les tables.

Les pilotes de Integration Services Red Hat Enterprise Linux intégrés pour Hyper-V (disponibles depuis Red Hat Enterprise Linux 6,4) sont suffisants pour permettre à Red Hat Enterprise Linux invités de s’exécuter à l’aide des appareils synthétiques hautes performances sur les hôtes Hyper-V. Ces pilotes intégrés sont certifiés par Red Hat pour cette utilisation. Les configurations certifiées peuvent être affichées sur cette page Web Red Hat : [catalogue de certification Red Hat](https://access.redhat.com/ecosystem/search/#/ecosystem/Red%20Hat%20Enterprise%20Linux?sort=sortTitle%20asc&vendors=Microsoft&category=Server). Il n’est pas nécessaire de télécharger et d’installer des packages de Integration Services Linux à partir du centre de téléchargement Microsoft, et cela peut limiter votre prise en charge de Red Hat, comme décrit dans l’article 1067 de la base de connaissances Red Hat : [Red Hat knowledgebase 1067](https://access.redhat.com/articles/1067).

En raison de conflits potentiels entre la prise en charge de LIS intégrée et la prise en charge des LIS téléchargeables lorsque vous mettez à niveau le noyau, désactivez les mises à jour automatiques, désinstallez les packages installables LIS, mettez à jour le noyau, redémarrez, puis installez la dernière version de LIS, puis redémarrez.

> [!NOTE]
> Les informations de certification officielles Red Hat Enterprise Linux sont disponibles via le [portail client Red Hat](https://access.redhat.com/ecosystem/search/#/category/Server?sort=sortTitle%20asc&query=windows%20server&ecosystem=Red%20Hat%20Enterprise%20Linux).

Dans cette section :

* [Série RHEL/CentOS 8. x](#rhelcentos-8x-series)

* [Série RHEL/CentOS 7. x](#rhelcentos-7x-series)

* [Série RHEL/CentOS 6. x](#rhelcentos-6x-series)

* [Série RHEL/CentOS 5. x](#rhelcentos-5x-series)

* [Notes](#notes)

## <a name="table-legend"></a>Légende de la table

* Les lis intégrés sont inclus **dans** le cadre de cette distribution Linux. Les numéros de version du module de noyau pour la LIS intégrée (comme indiqué par **lsmod**, par exemple) sont différents du numéro de version du package de téléchargement lis fourni par Microsoft. Une incompatibilité n’indique pas que la LIS intégrée est obsolète.

* &#10004;-Fonctionnalité disponible

* (*vide*)-fonctionnalité non disponible

## <a name="rhelcentos-8x-series"></a>Série RHEL/CentOS 8. x

|       **Fonctionnalité**     |       **Version de Windows Server**      |       **8,1**     |       **8,0**     | 
|-----------------------|---------------------------------------|-------------------|-------------------|
|       **Disponibilité**        |   |   |
|       **[Ebauche](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**      | 2019, 2016, 2012 R2 | &#10004; | &#10004;
|       Heure précise de Windows Server 2016       | 2019, 2016 | &#10004; | &#10004; 
|       **[Mise](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**      |   |  |
|       Trames Jumbo        | 2019, 2016, 2012 R2 | &#10004; | &#10004;|
|       Balisage et Trunking VLAN       | 2019, 2016, 2012 R2 | &#10004;  | &#10004; |
|       Migration dynamique      | 2019, 2016, 2012 R2 | &#10004; | &#10004;|
|       Injection d’adresses IP statiques     |  2019, 2016, 2012 R2 | &#10004;Remarque 2 | &#10004;Remarque 2|
|       vRSS     | 2019, 2016, 2012 R2 | &#10004; | &#10004;|
|       Segmentation TCP et déchargements de somme de contrôle | 2019, 2016, 2012 R2 | &#10004;|  &#10004; |
|       SR-IOV  | 2019, 2016 |  &#10004;   | &#10004; |
|       **[Rangement](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)** |  |  |
|       Redimensionnement VHDX  | 2019, 2016, 2012 R2 | &#10004; | &#10004; |
|       Fibre Channel virtuel | 2019, 2016, 2012 R2 | &#10004;Remarque 3  | &#10004;Remarque 3 |
|       Sauvegarde de machine virtuelle en direct  | 2019, 2016, 2012 R2 | &#10004;Remarque 5 | &#10004;Remarque 5|
|       SUPPRIMER la prise en charge | 2019, 2016, 2012 R2 | &#10004;  | &#10004; |
|       WWN SCSI | 2019, 2016, 2012 R2 | &#10004;  | &#10004; |
|       **[Capacité](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)** | |  |
|       Prise en charge du noyau PAE  | 2019, 2016, 2012 R2 |  N/A | N/A
|       Configuration de l’intervalle MMIO  | 2019, 2016, 2012 R2 | &#10004; | &#10004;  |
|       Mémoire dynamique-ajout à chaud | 2019, 2016, 2012 R2  | &#10004;Remarque 8, 9, 10 | &#10004;Remarque 8, 9, 10 |
|       Mémoire dynamique-bulles | 2019, 2016, 2012 R2 | &#10004;Remarque 8, 9, 10 | &#10004;Remarque 8, 9, 10 |
|       Redimensionnement de la mémoire d’exécution | 2019, 2016  | &#10004;  | &#10004; |
|       **[Vidéosurveillance](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)** | | |
|       Périphérique vidéo spécifique à Hyper-V | 2019, 2016, 2012 R2 | &#10004;   | &#10004; |
|       **[Diverses](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)** | | |
|       Paire clé-valeur  | 2019, 2016, 2012 R2 | &#10004;   | &#10004;  |
|       Interruption non masquable | 2019, 2016, 2012 R2 | &#10004;  | &#10004; |
|       Copie de fichiers de l’hôte vers l’invité | 2019, 2016, 2012 R2 | &#10004;  | &#10004; |
|       commande lsvmbus | 2019, 2016, 2012 R2 | &#10004;  | &#10004; |
|       Sockets Hyper-V | 2019, 2016 | &#10004;  | &#10004; |
|       Passthrough/DDA PCI | 2019, 2016 | &#10004; | &#10004; |
| **[Ordinateurs virtuels de 2e génération](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)** | |  |
|       Démarrer à l’aide d’UEFI | 2019, 2016, 2012 R2 |  &#10004;Remarque 14  | &#10004;Remarque 14   
|       Démarrage sécurisé | 2019, 2016 |  &#10004; |  &#10004; |


## <a name="rhelcentos-7x-series"></a>Série RHEL/CentOS 7. x

Cette série n’a que des noyaux 64 bits.


|                                                                 **Fonctionnalité**                                                                  |     **Version de Windows Server**     |                             **7,5-7.8**                             |                             **7.3-7.4**                             |                             **7.0-7.2**                             |     **7,5-7.8**     |       **7,4**       |       **7,3**       |       **7,2**       |       **7,1**       |        **7,0**         |
|----------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------|---------------------------------------------------------------------|---------------------------------------------------------------------|---------------------------------------------------------------------|---------------------|---------------------|---------------------|---------------------|---------------------|------------------------|
|                                                               **Disponibilité**                                                               |                                    | [LIS 4,3](https://www.microsoft.com/download/details.aspx?id=55106) | [LIS 4,3](https://www.microsoft.com/download/details.aspx?id=55106) | [LIS 4,3](https://www.microsoft.com/download/details.aspx?id=55106) |      Intégré       |      Intégré       |      Intégré       |      Intégré       |      Intégré       |        Intégré        |
|                          **[Ebauche](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**                          | 2019, 2016, 2012 R2 |                              &#10004;                               |                              &#10004;                               |                              &#10004;                               |      &#10004;       |      &#10004;       |      &#10004;       |      &#10004;       |      &#10004;       |        &#10004;        |
|                                                      Heure précise de Windows Server 2016                                                       |             2019, 2016             |                              &#10004;                               |                              &#10004;                               |                                                                     |                     |                     |                     |                     |                     |                        |
|                    **[Mise](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**                    |                                    |                                                                     |                                                                     |                                                                     |                     |                     |                     |                     |                     |                        |
|                                                                 Trames Jumbo                                                                 | 2019, 2016, 2012 R2 |                              &#10004;                               |                              &#10004;                               |                              &#10004;                               |      &#10004;       |      &#10004;       |      &#10004;       |      &#10004;       |      &#10004;       |        &#10004;        |
|                                                          Balisage et Trunking VLAN                                                           | 2019, 2016, 2012 R2 |                              &#10004;                               |                              &#10004;                               |                              &#10004;                               |      &#10004;       |      &#10004;       |      &#10004;       |      &#10004;       |      &#10004;       |        &#10004;        |
|                                                                Migration dynamique                                                                | 2019, 2016, 2012 R2 |                              &#10004;                               |                              &#10004;                               |                              &#10004;                               |      &#10004;       |      &#10004;       |      &#10004;       |      &#10004;       |      &#10004;       |        &#10004;        |
|                                                             Injection d’adresses IP statiques                                                              |     2019, 2016, 2012 R2      |                           &#10004;Remarque 2                           |                           &#10004;Remarque 2                           |                           &#10004;Remarque 2                           |   &#10004;Remarque 2   |   &#10004;Remarque 2   |   &#10004;Remarque 2   |   &#10004;Remarque 2   |   &#10004;Remarque 2   |    &#10004;Remarque 2     |
|                                                                     vRSS                                                                     |        2019, 2016, 2012 R2         |                              &#10004;                               |                              &#10004;                               |                              &#10004;                               |      &#10004;       |      &#10004;       |      &#10004;       |      &#10004;       |      &#10004;       |                        |
|                                                    Segmentation TCP et déchargements de somme de contrôle                                                    | 2019, 2016, 2012 R2 |                              &#10004;                               |                              &#10004;                               |                              &#10004;                               |      &#10004;       |      &#10004;       |      &#10004;       |      &#10004;       |      &#10004;       |                        |
|                                                                    SR-IOV                                                                    |             2019, 2016             |                              &#10004;                               |                              &#10004;                               |                                                                     |      &#10004;       |      &#10004;       |      &#10004;       |                     |                     |                        |
|                       **[Rangement](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**                       |                                    |                                                                     |                                                                     |                                                                     |                     |                     |                     |                     |                     |                        |
|                                                                 Redimensionnement VHDX                                                                  |        2019, 2016, 2012 R2         |                              &#10004;                               |                              &#10004;                               |                              &#10004;                               |      &#10004;       |      &#10004;       |      &#10004;       |      &#10004;       |                     |                        |
|                                                            Fibre Channel virtuel                                                             |        2019, 2016, 2012 R2         |                           &#10004;Remarque 3                           |                           &#10004;Remarque 3                           |                           &#10004;Remarque 3                           |   &#10004;Remarque 3   |   &#10004;Remarque 3   |   &#10004;Remarque 3   |   &#10004;Remarque 3   |   &#10004;Remarque 3   |    &#10004;Remarque 3     |
|                                                         Sauvegarde de machine virtuelle en direct                                                          |        2019, 2016, 2012 R2         |                           &#10004;Remarque 5                           |                           &#10004;Remarque 5                           |                           &#10004;Remarque 5                           |  &#10004;Remarque 4, 5  | &#10004;Remarque 4, 5  | &#10004;Remarque 4, 5  | &#10004;Remarque 4, 5  | &#10004;Remarque 4, 5  |   &#10004;Remarque 4, 5   |
|                                                                 SUPPRIMER la prise en charge                                                                 |        2019, 2016, 2012 R2         |                              &#10004;                               |                              &#10004;                               |                              &#10004;                               |      &#10004;       |      &#10004;       |      &#10004;       |      &#10004;       |                     |                        |
|                                                                   WWN SCSI                                                                   |        2019, 2016, 2012 R2         |                              &#10004;                               |                              &#10004;                               |                              &#10004;                               |      &#10004;       |                     |                     |                     |                     |                        |
|                        **[Capacité](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**                        |                                    |                                                                     |                                                                     |                                                                     |                     |                     |                     |                     |                     |                        |
|                                                              Prise en charge du noyau PAE                                                              | 2019, 2016, 2012 R2 |                                 N/A                                 |                                 N/A                                 |                                 N/A                                 |         N/A         |         N/A         |         N/A         |         N/A         |         N/A         |          N/A           |
|                                                          Configuration de l’intervalle MMIO                                                           |        2019, 2016, 2012 R2         |                              &#10004;                               |                              &#10004;                               |                              &#10004;                               |      &#10004;       |      &#10004;       |      &#10004;       |      &#10004;       |      &#10004;       |        &#10004;        |
|                                                           Mémoire dynamique-ajout à chaud                                                           |     2019, 2016, 2012 R2      |                       &#10004;Remarque 8, 9, 10                        |                       &#10004;Remarque 8, 9, 10                        |                       &#10004;Remarque 8, 9, 10                        | &#10004;Remarque 9, 10 | &#10004;Remarque 9, 10 | &#10004;Remarque 9, 10 | &#10004;Remarque 9, 10 | &#10004;Remarque 9, 10 | &#10004;Remarque 8, 9, 10 |
|                                                         Mémoire dynamique-bulles                                                          |     2019, 2016, 2012 R2      |                       &#10004;Remarque 8, 9, 10                        |                       &#10004;Remarque 8, 9, 10                        |                       &#10004;Remarque 8, 9, 10                        | &#10004;Remarque 9, 10 | &#10004;Remarque 9, 10 | &#10004;Remarque 9, 10 | &#10004;Remarque 9, 10 | &#10004;Remarque 9, 10 | &#10004;Remarque 8, 9, 10 |
|                                                            Redimensionnement de la mémoire d’exécution                                                             |             2019, 2016             |                              &#10004;                               |                              &#10004;                               |                              &#10004;                               |                     |                     |                     |                     |                     |                        |
|                         **[Vidéosurveillance](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**                         |                                    |                                                                     |                                                                     |                                                                     |                     |                     |                     |                     |                     |                        |
|                                                        Périphérique vidéo spécifique à Hyper-V                                                         | 2019, 2016, 2012 R2 |                              &#10004;                               |                              &#10004;                               |                              &#10004;                               |      &#10004;       |      &#10004;       |      &#10004;       |      &#10004;       |      &#10004;       |        &#10004;        |
|                 **[Diverses](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**                 |                                    |                                                                     |                                                                     |                                                                     |                     |                     |                     |                     |                     |                        |
|                                                                Paire clé-valeur                                                                | 2019, 2016, 2012 R2 |                              &#10004;                               |                              &#10004;                               |                              &#10004;                               |      &#10004;       |      &#10004;       |      &#10004;       |      &#10004;       |      &#10004;       |        &#10004;        |
|                                                            Interruption non masquable                                                            |        2019, 2016, 2012 R2         |                              &#10004;                               |                              &#10004;                               |                              &#10004;                               |      &#10004;       |      &#10004;       |      &#10004;       |      &#10004;       |      &#10004;       |        &#10004;        |
|                                                         Copie de fichiers de l’hôte vers l’invité                                                         |        2019, 2016, 2012 R2         |                              &#10004;                               |                              &#10004;                               |                              &#10004;                               |      &#10004;       |      &#10004;       |      &#10004;       |      &#10004;       |      &#10004;       |                        |
|                                                               commande lsvmbus                                                                | 2019, 2016, 2012 R2 |                              &#10004;                               |                              &#10004;                               |                              &#10004;                               |                     |                     |                     |                     |                     |                        |
|                                                               Sockets Hyper-V                                                                |             2019, 2016             |                              &#10004;                               |                              &#10004;                               |                              &#10004;                               |                     |                     |                     |                     |                     |                        |
|                                                             Passthrough/DDA PCI                                                              |             2019, 2016             |                              &#10004;                               |                              &#10004;                               |                                                                     |      &#10004;       |      &#10004;       |      &#10004;       |                     |                     |                        |
| **[Ordinateurs virtuels de 2e génération](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)** |                                    |                                                                     |                                                                     |                                                                     |                     |                     |                     |                     |                     |                        |
|                                                               Démarrer à l’aide d’UEFI                                                                |        2019, 2016, 2012 R2         |                          &#10004;Remarque 14                           |                          &#10004;Remarque 14                           |                          &#10004;Remarque 14                           |  &#10004;Remarque 14   |  &#10004;Remarque 14   |  &#10004;Remarque 14   |  &#10004;Remarque 14   |  &#10004;Remarque 14   |    &#10004;Remarque 14    |
|                                                                 Démarrage sécurisé                                                                  |             2019, 2016             |                              &#10004;                               |                              &#10004;                               |                              &#10004;                               |      &#10004;       |      &#10004;       |      &#10004;       |      &#10004;       |      &#10004;       |        &#10004;        |


## <a name="rhelcentos-6x-series"></a>Série RHEL/CentOS 6. x

Le noyau 32 bits pour cette série est compatible PAE. Il n’existe aucune prise en charge intégrée de la Banque d’aide locale pour RHEL/CentOS 6.0 à 6.3.

|  **Fonctionnalité**  | **Version de Windows Server** |  **6.7-6.10** |  **6.4-6.6** | **6.0-6.3** |   **6,10, 6,9, 6,8**   |       **6,6, 6,7**        |          **6,5**          |          **6,4**           |
|---------------|----------------------------|---------------|--------------|-------------|------------------------|---------------------------|----------------------------|---------------------------|
|  **Disponibilité** |   | [LIS 4,3](https://www.microsoft.com/download/details.aspx?id=55106) | [LIS 4,3](https://www.microsoft.com/download/details.aspx?id=55106) | [LIS 4,3](https://www.microsoft.com/download/details.aspx?id=55106) |        Intégré        |         Intégré          |         Intégré          |          Intégré          |
|  **[Ebauche](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)** | 2019, 2016, 2012 R2 |  &#10004; | &#10004;  | &#10004;  | &#10004; | &#10004; | &#10004;| &#10004; |
|  Heure précise de Windows Server 2016 | 2019, 2016 | |  | |  |  |   |   |
|  **[Mise](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**  | |   |  | |  |  |   |  |
|  Trames Jumbo | 2019, 2016, 2012 R2 | &#10004; |  &#10004; | &#10004; | &#10004; | &#10004; | &#10004; | &#10004; |
|  Balisage et Trunking VLAN | 2019, 2016, 2012 R2 | &#10004;Remarque 1 | &#10004;Remarque 1 | &#10004;Remarque 1 | &#10004;Remarque 1 | &#10004;Remarque 1|  &#10004;Remarque 1 |&#10004;Remarque 1 |
|  Migration dynamique | 2019, 2016, 2012 R2 | &#10004; | &#10004; | &#10004;| &#10004; | &#10004; | &#10004;  | &#10004; |
|  Injection d’adresses IP statiques |  2019, 2016, 2012 R2 | &#10004;Remarque 2 |  &#10004;Remarque 2  | &#10004;Remarque 2 | &#10004;Remarque 2 | &#10004;Remarque 2 | &#10004;Remarque 2  | &#10004;Remarque 2 |
|  vRSS  | 2019, 2016, 2012 R2 | &#10004; | &#10004; |  &#10004; | &#10004; | &#10004;  |   |   |
|  Segmentation TCP et déchargements de somme de contrôle | 2019, 2016, 2012 R2 | &#10004; | &#10004; | &#10004; | &#10004; | &#10004;  |   |    |
|  SR-IOV   | 2019, 2016 |    |  |   |   |   |   |  |
|  **[Rangement](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**  |  |   |   |  | |  |  | |
|  Redimensionnement VHDX | 2019, 2016, 2012 R2| &#10004; | &#10004; | &#10004; | &#10004; | &#10004; | &#10004; |  |
|  Fibre Channel virtuel  | 2019, 2016, 2012 R2 |  &#10004;Remarque 3  | &#10004;Remarque 3  | &#10004;Remarque 3  | &#10004;Remarque 3 | &#10004;Remarque 3  | &#10004;Remarque 3  |    |
|  Sauvegarde de machine virtuelle en direct | 2019, 2016, 2012 R2 | &#10004;Remarque 5  | &#10004;Remarque 5  | &#10004;Remarque 5 | &#10004;Remarque 4, 5 | &#10004;Remarque 4, 5 | &#10004;Remarque 4, 5, 6 | &#10004;Remarque 4, 5, 6 |
|  SUPPRIMER la prise en charge  | 2019, 2016, 2012 R2  | &#10004; | &#10004; | &#10004; | &#10004; |  |  |  |
|  WWN SCSI  | 2019, 2016, 2012 R2  | &#10004; | &#10004;  | &#10004;  |  |   |  |  |
|  **[Capacité](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)** | |  | | |  | |  |  |
|  Prise en charge du noyau PAE | 2019, 2016, 2012 R2 | &#10004;  | &#10004;  | &#10004; | &#10004; |  &#10004;  |  &#10004; |  &#10004; |
|  Configuration de l’intervalle MMIO  | 2019, 2016, 2012 R2 | &#10004;  | &#10004;  | &#10004;  |  &#10004;  | &#10004;  |  &#10004; |  &#10004; |
|  Mémoire dynamique-ajout à chaud | 2019, 2016, 2012 R2 |  &#10004;Remarque 7, 9, 10  | &#10004;Remarque 7, 9, 10 | &#10004;Remarque 7, 9, 10 | &#10004;Remarque 7, 9, 10 | &#10004;Remarque 7, 8, 9, 10 | &#10004;Remarque 7, 8, 9, 10 | |
|  Mémoire dynamique-bulles | 2019, 2016, 2012 R2 | &#10004;Remarque 7, 9, 10  | &#10004;Remarque 7, 9, 10  | &#10004;Remarque 7, 9, 10 | &#10004;Remarque 7, 9, 10 |  &#10004;Remarque 7, 9, 10   |  &#10004;Remarque 7, 9, 10   | &#10004;Remarque 7, 9, 10, 11 |
| Redimensionnement de la mémoire d’exécution | 2019, 2016  |  |   |  |  | |  |  |
| **[Vidéosurveillance](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)** || | | |  |  |  | |
| Périphérique vidéo spécifique à Hyper-V | 2019, 2016, 2012 R2 | &#10004;  | &#10004;  | &#10004;  | &#10004; | &#10004; | &#10004; | |
| **[Diverses](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)** |  |  |  |   |  | |  |  |
| Paire clé-valeur | 2019, 2016, 2012 R2 | &#10004; | &#10004;| &#10004; | &#10004;Remarque 12 | &#10004;Remarque 12 | &#10004;Remarque 12, 13 | &#10004;Remarque 12, 13 |
| Interruption non masquable | 2019, 2016, 2012 R2 | &#10004;  | &#10004;  | &#10004; | &#10004; | &#10004;  |  &#10004;| &#10004; |
| Copie de fichiers de l’hôte vers l’invité | 2019, 2016, 2012 R2 | &#10004;  |  &#10004; | &#10004; | &#10004; | &#10004; | |  |
| commande lsvmbus | 2019, 2016, 2012 R2 |  &#10004; |  &#10004;  |  &#10004; |   |   |    |  |
| Sockets Hyper-V | 2019, 2016  | &#10004;  |  &#10004;  |  &#10004; |  |   |   |  |
| Passthrough/DDA PCI | 2019, 2016 | &#10004;  | |   |  |  | |   |
| **[Ordinateurs virtuels de 2e génération](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)** | |  | | | | | | |
| Démarrer à l’aide d’UEFI  | 2012 R2 |  |  |  |  |  | |
|  |2019, 2016 | &#10004;Remarque 14 | &#10004;Remarque 14| &#10004;Remarque 14 |    &#10004;Remarque 14    |  | |  |
|Démarrage sécurisé| 2019, 2016|  ||  |  |  | ||



## <a name="rhelcentos-5x-series"></a>Série RHEL/CentOS 5. x

Cette série a un noyau PAE 32 bits pris en charge disponible. Il n’existe aucune prise en charge intégrée de la Banque d’aide locale pour RHEL/CentOS avant 5,9.


|                                                                 **Fonctionnalité**                                                                  |     **Version de Windows Server**     |                              5,2-5,11                              |                            **5.2-5.11**                             |    **5,9-5,11**     |
|----------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------|---------------------------------------------------------------------|---------------------------------------------------------------------|-----------------------|
|                                                               **Disponibilité**                                                               |                                    | [LIS 4,3](https://www.microsoft.com/download/details.aspx?id=55106) | [LIS 4,1](https://www.microsoft.com/download/details.aspx?id=51612) |       Intégré        |
|                          **[Ebauche](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**                          | 2019, 2016, 2012 R2 |                              &#10004;                               |                              &#10004;                               |       &#10004;        |
|                                                      Heure précise de Windows Server 2016                                                       |             2019, 2016             |                                                                     |                                                                     |                       |
|                    **[Mise](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**                    |                                    |                                                                     |                                                                     |                       |
|                                                                 Trames Jumbo                                                                 | 2019, 2016, 2012 R2 |                              &#10004;                               |                              &#10004;                               |       &#10004;        |
|                                                          Balisage et Trunking VLAN                                                           | 2019, 2016, 2012 R2 |                           &#10004;Remarque 1                           |                           &#10004;Remarque 1                           |    &#10004;Remarque 1    |
|                                                                Migration dynamique                                                                | 2019, 2016, 2012 R2 |                              &#10004;                               |                              &#10004;                               |       &#10004;        |
|                                                             Injection d’adresses IP statiques                                                              |     2019, 2016, 2012 R2      |                           &#10004;Remarque 2                           |                           &#10004;Remarque 2                           |    &#10004;Remarque 2    |
|                                                                     vRSS                                                                     |        2019, 2016, 2012 R2         |                                                                     |                                                                     |                       |
|                                                    Segmentation TCP et déchargements de somme de contrôle                                                    | 2019, 2016, 2012 R2 |                              &#10004;                               |                              &#10004;                               |                       |
|                                                                    SR-IOV                                                                    |             2019, 2016             |                                                                     |                                                                     |                       |
|                       **[Rangement](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**                       |                                    |                                                                     |                                                                     |                       |
|                                                                 Redimensionnement VHDX                                                                  |        2019, 2016, 2012 R2         |                              &#10004;                               |                              &#10004;                               |                       |
|                                                            Fibre Channel virtuel                                                             |        2019, 2016, 2012 R2         |                           &#10004;Remarque 3                           |                           &#10004;Remarque 3                           |                       |
|                                                         Sauvegarde de machine virtuelle en direct                                                          |        2019, 2016, 2012 R2         |                         &#10004;Remarque : 5, 15                         |                           &#10004;Remarque 5                           | &#10004;Remarque 4, 5, 6 |
|                                                                 SUPPRIMER la prise en charge                                                                 |        2019, 2016, 2012 R2         |                                                                     |                                                                     |                       |
|                                                                   WWN SCSI                                                                   |        2019, 2016, 2012 R2         |                                                                     |                                                                     |                       |
|                        **[Capacité](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**                        |                                    |                                                                     |                                                                     |                       |
|                                                              Prise en charge du noyau PAE                                                              | 2019, 2016, 2012 R2 |                              &#10004;                               |                              &#10004;                               |       &#10004;        |
|                                                          Configuration de l’intervalle MMIO                                                           |        2019, 2016, 2012 R2         |                              &#10004;                               |                              &#10004;                               |       &#10004;        |
|                                                           Mémoire dynamique-ajout à chaud                                                           |     2019, 2016, 2012 R2      |                                                                     |                                                                     |                       |
|                                                         Mémoire dynamique-bulles                                                          |     2019, 2016, 2012 R2      |                     &#10004;Remarque 7, 9, 10, 11                      |                     &#10004;Remarque 7, 9, 10, 11                      |                       |
|                                                            Redimensionnement de la mémoire d’exécution                                                             |             2019, 2016             |                                                                     |                                                                     |                       |
|                         **[Vidéosurveillance](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**                         |                                    |                                                                     |                                                                     |                       |
|                                                        Périphérique vidéo spécifique à Hyper-V                                                         | 2019, 2016, 2012 R2 |                              &#10004;                               |                              &#10004;                               |                       |
|                 **[Diverses](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**                 |                                    |                                                                     |                                                                     |                       |
|                                                                Paire clé-valeur                                                                | 2019, 2016, 2012 R2 |                              &#10004;                               |                              &#10004;                               |                       |
|                                                            Interruption non masquable                                                            |        2019, 2016, 2012 R2         |                              &#10004;                               |                              &#10004;                               |       &#10004;        |
|                                                         Copie de fichiers de l’hôte vers l’invité                                                         |        2019, 2016, 2012 R2         |                              &#10004;                               |                              &#10004;                               |                       |
|                                                               commande lsvmbus                                                                | 2019, 2016, 2012 R2 |                                                                     |                                                                     |                       |
|                                                               Sockets Hyper-V                                                                |             2019, 2016             |                                                                     |                                                                     |                       |
|                                                             Passthrough/DDA PCI                                                              |             2019, 2016             |                                                                     |                                                                     |                       |
| **[Ordinateurs virtuels de 2e génération](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)** |                                    |                                                                     |                                                                     |                       |
|                                                               Démarrer à l’aide d’UEFI                                                                |        2019, 2016, 2012 R2         |                                                                     |                                                                     |                       |
|                                                                 Démarrage sécurisé                                                                  |             2019, 2016             |                                                                     |                                                                     |                       |

## <a name="notes"></a>Remarques

1. Pour cette version RHEL/CentOS, le balisage VLAN fonctionne, mais pas le Trunking VLAN.

2. L’injection d’adresses IP statiques peut ne pas fonctionner si le gestionnaire de réseau a été configuré pour une carte réseau synthétique donnée sur la machine virtuelle. Pour un bon fonctionnement de l’injection d’adresses IP statiques, assurez-vous que le gestionnaire de réseau est entièrement désactivé ou a été désactivé pour une carte réseau spécifique via son fichier ifcfg-ethX.

3. Sur Windows Server 2012 R2 lors de l’utilisation de périphériques Fibre Channel virtuels, assurez-vous que le numéro d’unité logique 0 (LUN 0) a été rempli. Si le LUN 0 n’a pas été rempli, une machine virtuelle Linux peut ne pas être en mesure de monter des appareils Fibre Channel en mode natif.

4. Pour les LIS intégrés, le package « HyperV-daemons » doit être installé pour cette fonctionnalité.

5. Si des descripteurs de fichiers sont ouverts pendant une opération de sauvegarde de machine virtuelle en direct, dans certains cas, les VHD sauvegardés devront peut-être subir une vérification de cohérence du système de fichiers (fsck) lors de la restauration. Les opérations de sauvegarde en direct peuvent échouer en mode silencieux si l’ordinateur virtuel dispose d’un périphérique iSCSI attaché ou d’un stockage en attachement direct (également appelé disque direct).

6. Pendant que le téléchargement de la Integration Services Linux est préféré, la prise en charge de la sauvegarde en direct pour RHEL/CentOS 5,9-5.11/6.4/6.5 est également disponible via [Hyper-V Backup Essentials pour Linux](https://github.com/LIS/backupessentials/tree/1.0).

7. La prise en charge de la mémoire dynamique est disponible uniquement sur les ordinateurs virtuels 64 bits.

8. La prise en charge de l’ajout à chaud n’est pas activée par défaut dans cette distribution. Pour activer la prise en charge de l’ajout à chaud, vous devez ajouter une règle udev sous/etc/udev/rules.d/comme suit :

   1. Créez un fichier **/etc/udev/rules.d/100-Balloon.Rules**. Vous pouvez utiliser n’importe quel autre nom souhaité pour le fichier.

   2. Ajoutez le contenu suivant au fichier : `SUBSYSTEM=="memory", ACTION=="add", ATTR{state}="online"`

   3. Redémarrez le système pour activer la prise en charge de l’ajout à chaud.

   Tandis que le téléchargement de la Integration Services Linux crée cette règle lors de l’installation, la règle est également supprimée lorsque la LIS est désinstallée, de sorte que la règle doit être recréée si la mémoire dynamique est nécessaire après la désinstallation.

9. Les opérations de mémoire dynamique peuvent échouer si le système d’exploitation invité s’exécute trop peu de mémoire. Voici quelques-unes des meilleures pratiques :

   * La mémoire de démarrage et la mémoire minimale doivent être supérieures ou égales à la quantité de mémoire recommandée par le fournisseur de distribution.

   * Les applications qui ont tendance à consommer toute la mémoire disponible sur un système se limitent à consommer jusqu’à 80% de la mémoire RAM disponible.

10. Si vous utilisez Mémoire dynamique sur un système d’exploitation Windows Server 2016 ou Windows Server 2012 R2, spécifiez les paramètres de **mémoire de démarrage**, de **mémoire minimale**et de **mémoire maximale** en multiples de 128 mégaoctets (Mo). Si vous ne le faites pas, vous risquez d’obtenir des erreurs d’ajout à chaud. vous ne verrez peut-être aucune augmentation de la mémoire dans un système d’exploitation invité.

11. Certaines distributions, y compris celles utilisant LIS 4,0 et 4,1, ne fournissent que la prise en charge des bulles et ne fournissent pas de prise en charge de l’ajout à chaud. Dans ce scénario, la fonctionnalité de mémoire dynamique peut être utilisée en affectant à la mémoire de démarrage la valeur qui est égale au paramètre de mémoire maximal. En conséquence, toute la mémoire requise est ajoutée à chaud à la machine virtuelle au moment du démarrage, puis, en fonction des besoins en mémoire de l’ordinateur hôte, Hyper-V peut librement allouer ou libérer de la mémoire de l’invité à l’aide de l’info-bulle. Configurez la **mémoire de démarrage** et la **mémoire minimale** au niveau ou au-dessus de la valeur recommandée pour la distribution.

12. Pour activer l’infrastructure de paires clé/valeur (KVP), installez le package rpm hypervkvpd ou HyperV-daemons à partir de votre fichier ISO RHEL. Le package peut également être installé directement à partir de dépôts RHEL.

13. L’infrastructure de paires clé/valeur (KVP) peut ne pas fonctionner correctement sans mise à jour logicielle Linux. Contactez votre fournisseur de distribution pour obtenir la mise à jour logicielle en cas de problème avec cette fonctionnalité.

14. Sur les ordinateurs virtuels Windows Server 2012 R2 génération 2, le démarrage sécurisé est activé par défaut et certaines machines virtuelles Linux ne démarrent pas, sauf si l’option de démarrage sécurisé est désactivée. Vous pouvez désactiver le démarrage sécurisé dans la section **microprogramme** des paramètres de l’ordinateur virtuel dans le **Gestionnaire Hyper-V** , ou vous pouvez le désactiver à l’aide de PowerShell :

    ```Powershell
    Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off
    ```

    Le téléchargement de Integration Services Linux peut être appliqué aux machines virtuelles de 2e génération, mais ne pas communiquer avec la génération 2.

15. Dans Red Hat Enterprise Linux ou CentOS 5,2, 5,3 et 5,4, la fonctionnalité de blocage du système de fichiers n’est pas disponible. par conséquent, la sauvegarde de la machine virtuelle active n’est pas disponible.

Voir aussi

* [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [Machines virtuelles Debian prises en charge sur Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Ordinateurs virtuels Oracle Linux pris en charge sur Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Ordinateurs virtuels SUSE pris en charge sur Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Machines virtuelles Ubuntu prises en charge sur Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Ordinateurs virtuels FreeBSD pris en charge sur Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Descriptions des fonctionnalités pour les machines virtuelles Linux et FreeBSD sur Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Meilleures pratiques pour l’exécution de Linux sur Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [Certification matérielle Red Hat](https://hardware.redhat.com/&quicksearch=Microsoft)
