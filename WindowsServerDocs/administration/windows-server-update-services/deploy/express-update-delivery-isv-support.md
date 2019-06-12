---
title: Prise en charge par les ISV de la distribution des mises à jour Express
description: Rubrique de Windows Server Update Service (WSUS) - fournisseurs de logiciels comment indépendant (ISV) permettre configurer la livraison de mise à jour Express à l’aide de WSUS
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
author: sakitong
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: 7331418c1926958da07c94bca9ff9f871134f3fa
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439869"
---
# <a name="express-update-delivery-isv-support"></a>Prise en charge par les ISV de la distribution des mises à jour Express

>S'applique à : Windows 10, Windows Server 2016

Téléchargements de la mise à jour de Windows 10 peuvent être volumineux, car chaque package contient tous les correctifs précédemment publiés pour garantir la cohérence et la simplicité.  

Depuis la version 7, Windows a été en mesure de réduire la taille des téléchargements de Windows Update avec une fonctionnalité appelée [Express](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx#Anchor_2), et bien que les périphériques grand public prennent en charge par défaut, les appareils Windows 10 entreprise nécessitent la mise à jour de Windows Server Services (WSUS) pour tirer parti d’Express.

## <a name="how-microsoft-supports-express"></a>Comment Microsoft prend en charge la fonctionnalité Express

- **Express sur WSUS autonome**

    Remise de la mise à jour Express est déjà [disponible sur toutes les versions prises en charge de WSUS](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx).

- **Express sur les périphériques directement connectés à Windows Update** 

    Périphériques grand public prennent en charge le téléchargement Express : ils utilisent le client de Windows Update (WU) pour analyser, télécharger et installer les mises à jour. Pendant la phase de téléchargement, le client WU demande des packages d’Express et télécharge les plages d’octets approprié.

-  **Les périphériques d'entreprise gérés via [Windows Update for Business](https://technet.microsoft.com/itpro/windows/manage/waas-manage-updates-wufb)** bénéficient également de la distribution de mises à jour Express, le tout sans modification de la configuration.

## <a name="how-isvs-can-take-advantage-of-express"></a>Comment les éditeurs de logiciels indépendants peuvent tirer parti d’Express

Éditeurs de logiciels indépendants peuvent utiliser WSUS et le client WU pour prendre en charge la remise de mise à jour d’Express. Microsoft recommande les trois étapes suivantes, chacune abordée plus en détail dans les sections ci-dessous :

1.  [**Configurer WSUS**](#BKMK_1)

    Serveur WSUS est nécessaire pour analyser et mettre à jour de synchronisations (vous pouvez trouver des informations supplémentaires [ici](https://technet.microsoft.com/library/dn800972(v=ws.11).aspx))

2.  [**Spécifiez et remplir un cache de fichier ISV**](#BKMK_2)

    Un cache de fichier ISV est recommandé d’héberger le contenu de mise à jour, ce qui inclut les fichiers CAB de mise à jour (fichiers .cab) et rapide de packages (fichiers .psf).

3.  [**Configurer un agent de client ISV pour diriger les opérations du client WU**](#BKMK_3)

>[!NOTE]
>Nécessite la version Cumulative mise à jour pour Windows 10 Version 1607 dans (ou après) janvier 2017 ([KB3213986 (14393.693 de Build du système d’exploitation)](https://support.microsoft.com/en-us/help/4009938/january-10-2017-kb3213986-os-build-14393-693) doit être installé.
    
   - L’agent du client ISV détermine quelles mises à jour à approuver, puis lorsque télécharger et installer des mises à jour
   - Le client Windows Update détermine les plages d’octets à télécharger et lance la demande de téléchargement

### <a name="BKMK_1"></a>Étape 1 : Configurer WSUS

WSUS sert d’interface pour la mise à jour de Windows et gère toutes les métadonnées décrivant les packages d’Express qui doivent être téléchargés. Si vous avez besoin déployer, consultez [ **vue d’ensemble de Windows Server Update Services 3.0 SP2**](https://technet.microsoft.com/library/dd939931(v=ws.10).aspx). Une fois que WSUS a été déployé, la considération principale est ou non stocker le contenu de la mise à jour localement sur le serveur WSUS. Lors de la configuration WSUS, nous vous recommandons de ne pas stocker les mises à jour localement. Cela suppose que vous avez déjà logiciel dirigeant le déploiement de ces packages dans votre environnement. Pour plus d’informations sur la façon de configurer le stockage local de WSUS, consultez [ **déterminer où les mises à jour Store**](https://technet.microsoft.com/library/cc720494(v=ws.10).aspx).

### <a name="BKMK_2"></a>Étape 2 : Spécifiez et remplir le Cache de fichier ISV 

#### <a name="specify-the-isv-file-cache"></a>Spécifiez le cache du fichier ISV

Nouveaux paramètres de stratégie de groupe et de gestion des appareils mobiles (MDM) côté client détaillées dans le [ **référence de fournisseur de service de Configuration** ](https://msdn.microsoft.com/en-us/windows/hardware/commercialize/customize/mdm/configuration-service-provider-reference) définissent l’emplacement du cache de fichier ISV.

| **Nom**                                              | **Description**                                                                                                                                                      |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Configurer un autre emplacement de téléchargement des mises à jour. | Spécifie un serveur intranet de remplacement pour les mises à jour de l’hôte à partir de Microsoft Update. Vous pouvez ensuite utiliser ce service de mise à jour pour mettre à jour automatiquement les ordinateurs sur votre réseau |

Il existe deux options lorsque vous configurez l’emplacement de téléchargement différent pour le cache de fichier ISV :

1. **Spécifiez un nom d’hôte du serveur HTTP de l’éditeur de logiciels indépendant**, qui est le cache de fichier ISV
    
    Cette approche configure le client Windows Update pour effectuer des demandes de téléchargement pour le serveur HTTP spécifié dans la stratégie

2. **Spécifiez localhost**
 
    Cette approche configure le client Windows Update pour effectuer des demandes de téléchargement à localhost. Ainsi, l’agent du client ISV gérer ces demandes et l’itinéraire comme il convient de répondre à la demande de téléchargement.

> [!IMPORTANT]
> Le cache de fichier ISV nécessite les éléments suivants :                                                          
> - Le serveur doit être compatible avec la RFC HTTP 1.1 : <http://www.w3.org/Protocols/rfc2616/rfc2616.html>                                                                                                                                                                
> Plus précisément, le serveur web doit prendre en charge                                                                                                                                                                                                                                       [ **HEAD** ](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) et [ **obtenir** ](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.htm) demandes<br>                                                                                                                                                                                                                                                                                                  -Demandes de plage de partielle<br>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   Persistant<br>                                                                                                                                                                                                                                                                                                                                                                                                                            -N’utilisez pas « Transfer-Encoding : chunked »                                                                                                 

#### <a name="populate-the-isv-file-cache"></a>Remplir le Cache de fichier ISV

Le cache de fichiers des éditeurs de logiciels indépendants doit être rempli avec les fichiers associés avec les mises à jour à installer sur les clients gérés. 

**Pour remplir le cache de fichier ISV :**

1. Utilisez [API WSUS](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.updateservices.administration.updatefile(v=vs.85).aspx) pour accéder à la mise à jour le chemin d’accès au fichier et nom de fichier pour le service Microsoft Update.

    Les métadonnées pour chaque mise à jour sur le serveur WSUS contient le chemin d’accès et nom de fichier sur Microsoft Update de la mise à jour comme suit (nom d’hôte de Microsoft Update en gras, suivie du chemin d’accès et nom de fichier) : **<http://download.windowsupdate.com>** /c/msdownload/mise à jour / Software/UPDT/2016/09/windows10.0-kb3195781-x64_0c06079bccc35cba35a48bd2b1ec46f818bd2e74.msu

2. Télécharger des fichiers à partir de Microsoft Update et les stocker dans le cache de fichier ISV en utilisant l’une de ces deux méthodes : 

   - Fichiers de Store à l’aide de la **même chemin de dossier que sur le service MU**

   - Fichiers de Store à l’aide un **chemin d’accès du dossier défini par l’éditeur de logiciels indépendant**

     Ont de redirection de serveur (ou localhost) HTTP **HTTP GET** demandes, qui font référence à la MU dossier chemin d’accès et nom de fichier à l’emplacement du fichier ISV.

### <a name="BKMK_3"></a>Étape 3 : Configurer un agent de client ISV pour diriger les opérations du client WU

L’agent du client ISV orchestre le téléchargement et l’installation de mises à jour approuvées à l’aide de flux de travail recommandé suivant :

1.  L’agent du client ISV appelle le client Windows Update pour rechercher sur le serveur WSUS

2.  L’analyse retourne le jeu de mises à jour applicables au client WU

3.  Le client ISV détermine quelles mises à jour à approuver, téléchargez et installez

4.  Le client ISV client agent appelle Windows Update pour télécharger les mises à jour approuvées

5.  Une fois que les mises à jour ont été téléchargées, l’agent du client ISV appelle le client Windows Update pour installer les mises à jour approuvées

Consultez [rechercher, téléchargement et installation des mises à jour](https://msdn.microsoft.com/en-us/library/windows/desktop/aa387102(v=vs.85).aspx) pour plus d’informations sur l’utilisation du client WU à analyser, télécharger et installer les mises à jour.

### <a name="download-workflow-options"></a>Options de flux de travail de téléchargement

Voici les deux illustrations des options de flux de travail de téléchargement à partir d’un cache de fichier ISV :

![Flux de travail 1](../../media/express-update-delivery-isv-support/image1.png)

![Flux de travail 2](../../media/express-update-delivery-isv-support/image2.png)
### <a name="how-express-download-works"></a>Principe de fonctionnement du téléchargement Express

- Pour les mises à jour du système d’exploitation qui prennent en charge le téléchargement Express, deux versions de la charge utile de fichier sont stockées sur le service :

  - **Version complète** --essentiellement en remplaçant les versions locales des fichiers binaires de mise à jour

  - **Version Express** --contenant les deltas nécessaires pour corriger les fichiers binaires existants sur l’appareil. 

    La version complète et la version Express sont référencés dans les métadonnées de la mise à jour, ce qui a été téléchargée sur le client en tant que partie de la phase d’analyse. 

    **Téléchargement rapide fonctionne comme suit :**

    Le client WU tente de télécharger Express tout d’abord et dans certaine situations fall fichier complet si nécessaire (par exemple, si la connexion via un proxy qui ne prennent pas en charge les demandes de plage d’octets).

  1. Lorsque le client WU lance un téléchargement rapide, **le client Windows Update télécharge d’abord un stub**, qui fait partie du package Express.

  2. **Le client WU transmet ce stub pour le programme d’installation de Windows**, qui utilise le stub pour effectuer un inventaire local, en comparant les deltas du fichier sur l’appareil avec ce qui est nécessaire pour accéder à la dernière version du fichier proposé.

  3. Le **programme d’installation de Windows demande ensuite le client Windows Update pour télécharger les plages** qui ont été déterminé comme obligatoire.

  4. **Le client Windows Update télécharge ces plages d’adresses et les transmet au programme d’installation de Windows**, qui s’applique les plages et détermine ensuite si des plages supplémentaires sont nécessaires. Ce processus se répète jusqu'à ce que le programme d’installation Windows indique le client WU que toutes les plages nécessaires ont été téléchargés.

  À ce stade, le téléchargement est terminé et la mise à jour est prête à être installée.

### <a name="how-delivery-optimization-reduces-bandwidth-consumption"></a>Comment l’optimisation de livraison réduit la consommation de bande passante

Remise DO (optimisation) est une solution de cache distribué autonome pour les entreprises cherchent à réduire la consommation de bande passante des mises à jour du système d’exploitation, les mises à niveau du système d’exploitation et les applications. Permet aux clients de télécharger ces éléments à partir d’autres sources (telles que les autres pairs sur le réseau) en association avec l’emplacement de téléchargement spécifié (le cache de fichier ISV dans ce scénario).

Par défaut dans Windows 10 entreprise et éducation, effectuez l’autorise le partage de peer-to-peer sur le réseau de l’organisation uniquement, mais vous pouvez configurer différemment à l’aide de stratégie de groupe et les paramètres de gestion des appareils mobiles.

Reportez-vous à [met à jour de l’optimisation de livraison configurer pour Windows 10](https://technet.microsoft.com/itpro/windows/manage/waas-delivery-optimization) pour plus d’informations sur.
