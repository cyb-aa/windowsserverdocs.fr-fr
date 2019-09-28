---
title: Prise en charge par les ISV de la distribution des mises à jour Express
description: 'Rubrique Windows Server Update Service (WSUS) : comment les éditeurs de logiciels indépendants (ISV) peuvent configurer la remise des mises à jour Express à l’aide de WSUS'
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
author: sakitong
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: a4880a1a66d9c722cfda9e194c4eff38c5058674
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361722"
---
# <a name="express-update-delivery-isv-support"></a>Prise en charge par les ISV de la distribution des mises à jour Express

>S'applique à : Windows 10, Windows Server 2016

Les téléchargements de mises à jour Windows 10 peuvent être volumineux, car chaque package contient tous les correctifs précédemment publiés pour garantir la cohérence et la simplicité.  

Depuis la version 7, Windows a pu réduire la taille des téléchargements de Windows Update avec une fonctionnalité appelée [Express](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx#Anchor_2), et bien que les appareils grand public le prennent en charge par défaut, les appareils Windows 10 entreprise requièrent que Windows Server Update Services (WSUS) prenne avantage d’Express.

## <a name="how-microsoft-supports-express"></a>Comment Microsoft prend en charge la fonctionnalité Express

- **Express sur WSUS autonome**

    La remise des mises à jour Express est déjà [disponible sur toutes les versions prises en charge de WSUS](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx).

- **Express sur les appareils directement connectés à Windows Update** 

    Les appareils grand public prennent en charge Express Download : ils utilisent le client Windows Update (WU) pour analyser, télécharger et installer les mises à jour. Pendant la phase de téléchargement, le client WU demande des packages Express et télécharge les plages d’octets appropriées.

-  **Les périphériques d'entreprise gérés via [Windows Update for Business](https://technet.microsoft.com/itpro/windows/manage/waas-manage-updates-wufb)** bénéficient également de la distribution de mises à jour Express, le tout sans modification de la configuration.

## <a name="how-isvs-can-take-advantage-of-express"></a>Comment les éditeurs de logiciels indépendants peuvent tirer parti d’Express

Les éditeurs de logiciels indépendants peuvent utiliser WSUS et le client WU pour prendre en charge la remise des mises à jour rapide. Microsoft recommande les trois étapes suivantes, chacune étant traitée plus en détail dans les sections ci-dessous :

1.  [**Configurer WSUS**](#BKMK_1)

    Le serveur WSUS est requis pour l’analyse & les synchronisations de mise à jour (des informations supplémentaires sont disponibles [ici](https://technet.microsoft.com/library/dn800972(v=ws.11).aspx))

2.  [**Spécifier et remplir un cache de fichiers ISV**](#BKMK_2)

    Un cache de fichiers ISV est recommandé pour héberger le contenu de la mise à jour, qui comprend les fichiers CAB de mise à jour (fichiers. cab) et les packages Express (fichiers. polyesters).

3.  [**Configurer un agent du client ISV pour diriger les opérations du client WU**](#BKMK_3)

>[!NOTE]
>Requiert une mise à jour cumulative pour la version 1607 de Windows 10 (ou après) le 2017 janvier ([KB3213986 (version du système d’exploitation 14393,693)](https://support.microsoft.com/en-us/help/4009938/january-10-2017-kb3213986-os-build-14393-693) à installer.
    
   - L’agent du client ISV détermine les mises à jour à approuver, ainsi que le téléchargement et l’installation des mises à jour.
   - Le client WU détermine les plages d’octets à télécharger et lance la demande de téléchargement

### <a name="BKMK_1"></a>Étape 1 : Configurer WSUS

WSUS sert d’interface pour Windows Update et gère toutes les métadonnées décrivant les packages Express qui doivent être téléchargés. Si vous devez effectuer le déploiement, consultez [**vue d’ensemble de Windows Server Update Services 3,0 SP2**](https://technet.microsoft.com/library/dd939931(v=ws.10).aspx). Une fois que WSUS a été déployé, il est primordial de stocker le contenu de mise à jour localement sur le serveur WSUS. Lors de la configuration de WSUS, nous vous recommandons de ne pas stocker les mises à jour localement. Cela suppose que vous avez déjà un logiciel dirigeant le déploiement de ces packages dans votre environnement. Pour plus d’informations sur la configuration du stockage local WSUS, consultez [**déterminer où stocker les mises à jour**](https://technet.microsoft.com/library/cc720494(v=ws.10).aspx).

### <a name="BKMK_2"></a>Étape 2 : Spécifier et remplir le cache des fichiers ISV 

#### <a name="specify-the-isv-file-cache"></a>Spécifier le cache des fichiers ISV

Les nouveaux paramètres de stratégie de groupe côté client et de gestion des appareils mobiles (MDM) détaillés dans la [**Référence du fournisseur de services de configuration**](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/configuration-service-provider-reference) définissent l’emplacement du cache des fichiers ISV.

| **Nom**                                              | **Description**                                                                                                                                                      |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Configurez un autre emplacement de téléchargement pour les mises à jour. | Spécifie un autre serveur intranet pour héberger les mises à jour à partir de Microsoft Update. Vous pouvez ensuite utiliser ce service de mise à jour pour mettre à jour automatiquement les ordinateurs sur votre réseau. |

Il existe deux options lors de la configuration de l’emplacement de téléchargement alternatif pour le cache des fichiers ISV :

1. **Spécifiez un nom d’hôte du serveur http ISV**, qui est le cache des fichiers ISV.
    
    Cette approche configure le client WU pour qu’il effectue des demandes de téléchargement au serveur HTTP spécifié dans la stratégie.

2. **Spécifier localhost**
 
    Cette approche configure le client WU pour qu’il effectue des demandes de téléchargement à localhost. Cela permet à l’agent du client ISV de gérer ces requêtes et de les acheminer de manière appropriée pour répondre à la demande de téléchargement.

> [!IMPORTANT]
> Le cache des fichiers ISV requiert les éléments suivants :                                                          
> - Le serveur doit être conforme à la norme HTTP 1,1 pour chaque RFC :<http://www.w3.org/Protocols/rfc2616/rfc2616.html>                                                                                                                                                                
> Plus précisément, le serveur Web doit prendre en charge                                                                                                                                                                                                                                       Requêtes [**Head**](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) et [**obten**](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.htm)<br>                                                                                                                                                                                                                                                                                                  -Requêtes de plage partielles<br>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   -Keep-Alive<br>                                                                                                                                                                                                                                                                                                                                                                                                                            -N’utilisez pas l’encodage de transfert : mémorisé en bloc                                                                                                 

#### <a name="populate-the-isv-file-cache"></a>Remplir le cache des fichiers ISV

Le cache des fichiers ISV doit être rempli avec les fichiers associés aux mises à jour à installer sur les clients gérés. 

**Pour remplir le cache des fichiers ISV :**

1. Utilisez les [API WSUS](https://msdn.microsoft.com/library/windows/desktop/microsoft.updateservices.administration.updatefile(v=vs.85).aspx) pour accéder au chemin d’accès et au nom de fichier de la mise à jour pour le service MU.

    Les métadonnées de chaque mise à jour sur le serveur WSUS contiennent le chemin d’accès et le nom de fichier de la mise à jour sur Microsoft Update comme suit (Microsoft Update nom d’hôte en gras, suivi du chemin d’accès et du nom de fichier) : **<http://download.windowsupdate.com>** /c/msdownload/Update/Software/UPDT/2016/09/ Windows 10.0-kb3195781-x64_0c06079bccc35cba35a48bd2b1ec46f818bd2e74. msu

2. Téléchargez les fichiers à partir de Microsoft Update et stockez-les dans le cache des fichiers ISV à l’aide de l’une des deux méthodes suivantes : 

   - Stocker les fichiers en utilisant le **même chemin d’accès de dossier que sur le service MU**

   - Stocker des fichiers à l’aide **d’un chemin de dossier défini par ISV**

     Faites en sorte que le serveur HTTP (ou localhost) redirige les requêtes **http obten** , qui font référence au chemin d’accès du dossier MU et au nom du fichier, à l’emplacement du fichier ISV.

### <a name="BKMK_3"></a>Étape 3 : Configurer un agent du client ISV pour diriger les opérations du client WU

L’agent du client ISV orchestre le téléchargement et l’installation des mises à jour approuvées à l’aide du flux de travail recommandé suivant :

1.  L’agent du client ISV appelle le client WU pour analyser le serveur WSUS

2.  L’analyse renvoie l’ensemble des mises à jour applicables au client WU.

3.  Le client ISV détermine les mises à jour à approuver, télécharger et installer.

4.  L’agent du client ISV appelle WU client pour télécharger les mises à jour approuvées

5.  Une fois que les mises à jour ont été téléchargées, l’agent du client ISV appelle le client WU pour installer les mises à jour approuvées.

Pour plus d’informations sur l’utilisation du client WU pour analyser, télécharger et installer des mises à jour, consultez [recherche, téléchargement et installation des mises à jour](https://msdn.microsoft.com/library/windows/desktop/aa387102(v=vs.85).aspx) .

### <a name="download-workflow-options"></a>Télécharger les options de flux de travail

Voici deux illustrations des options de téléchargement de flux de travail à partir d’un cache de fichiers ISV :

![Flux de travail 1](../../media/express-update-delivery-isv-support/image1.png)

![Flux de travail 2](../../media/express-update-delivery-isv-support/image2.png)
### <a name="how-express-download-works"></a>Principe de fonctionnement du téléchargement Express

- Pour les mises à jour du système d’exploitation qui prennent en charge le téléchargement Express, deux versions de la charge utile de fichier sont stockées sur le service :

  - **Version complète** : remplacement des versions locales des fichiers binaires de mise à jour

  - **Version Express** : contient les deltas nécessaires pour corriger les fichiers binaires existants sur l’appareil. 

    La version de fichier complet et la version Express sont référencées dans les métadonnées de la mise à jour, qui ont été téléchargées sur le client dans le cadre de la phase d’analyse. 

    **Express Download fonctionne comme suit :**

    Le client WU tentera de Télécharger Express Express, et dans certains cas, repassera à fichier complet si nécessaire (par exemple, si vous parcourez un proxy qui ne prend pas en charge les demandes de plages d’octets).

  1. Lorsque le client WU lance un téléchargement Express, **le client Wu télécharge d’abord un stub**, qui fait partie du package Express.

  2. **Le client Wu transmet ce stub à Windows Installer**, qui utilise le stub pour effectuer un inventaire local, en comparant les deltas du fichier sur l’appareil avec ce qui est nécessaire pour accéder à la version la plus récente du fichier proposé.

  3. Le **programme d’installation de Windows demande ensuite au client Wu de télécharger les plages** qui ont été jugées nécessaires.

  4. **Le client Wu télécharge ces plages et les transmet à Windows Installer**, qui applique les plages, puis détermine si des plages supplémentaires sont nécessaires. Cela se répète jusqu’à ce que Windows Installer indique au client WU que toutes les plages nécessaires ont été téléchargées.

  À ce stade, le téléchargement est terminé et la mise à jour est prête à être installée.

### <a name="how-delivery-optimization-reduces-bandwidth-consumption"></a>Comment l’optimisation de la distribution réduit la consommation de bande passante

L’optimisation de la distribution (DO) est une solution de mise en cache distribuée auto-organisable pour les entreprises cherchant à réduire la consommation de bande passante pour les mises à jour du système d’exploitation, les mises à niveau du système d’exploitation et les applications. Permet aux clients de télécharger ces éléments à partir d’autres sources (par exemple, d’autres homologues sur le réseau), conjointement avec l’emplacement de téléchargement spécifié (le cache des fichiers ISV dans ce scénario).

Par défaut, dans Windows 10 entreprise et éducation, autorise le partage d’égal à égal uniquement sur le réseau de l’organisation, mais vous pouvez le configurer différemment à l’aide des paramètres de stratégie de groupe et de gestion des appareils mobiles (MDM).

Pour plus d’informations sur les actions, consultez [configurer l’optimisation de la distribution pour les mises à jour de Windows 10](https://technet.microsoft.com/itpro/windows/manage/waas-delivery-optimization) .
