---
title: Prise en charge par les ISV de la distribution des mises à jour Express
description: Rubrique Windows Server Update Services (WSUS) – Configuration de la distribution des mises à jour Express par les éditeurs de logiciels indépendants (ISV) à l’aide de WSUS
ms.prod: windows-server
ms.technology: manage-wsus
ms.topic: get-started article
author: sakitong
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: 60d01ef425ed96160cd76afdd7c27c081c778add
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80828772"
---
# <a name="express-update-delivery-isv-support"></a>Prise en charge par les ISV de la distribution des mises à jour Express

>S'applique à : Windows 10, Windows Server 2016

Les téléchargements de Windows 10 Update peuvent être volumineux car, à des fins de cohérence et de simplicité, chaque package contient l’ensemble des correctifs précédemment publiés.  

Depuis la version 7, Windows est parvenu à réduire la taille des téléchargements de Windows Update grâce à la mise en place d’une fonctionnalité appelée [Express](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx#Anchor_2). Bien que les appareils grand public le prennent en charge par défaut, les appareils Windows 10 Entreprise ont besoin de Windows Server Update Services (WSUS) pour tirer parti de la fonctionnalité Express.

## <a name="how-microsoft-supports-express"></a>Prise en charge de la fonctionnalité Express par Microsoft

- **Express sur WSUS en tant qu’application autonome**

    La distribution de mises à jour Express est déjà [disponible sur toutes les versions de WSUS prises en charge](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx).

- **Fonctionnalité Express sur des appareils directement connectés à Windows Update** 

    Les appareils grand public prennent en charge le téléchargement de la fonctionnalité Express : ils utilisent le client Windows Update (WU) pour analyser, télécharger et installer les mises à jour. Pendant la phase de téléchargement, le client WU demande des packages Express et télécharge les plages d’octets appropriées.

-  **Les appareils d’entreprise gérés à l’aide de [Windows Update for Business](https://technet.microsoft.com/itpro/windows/manage/waas-manage-updates-wufb)** bénéficient également de la distribution de mises à jour Express sans aucun changement de configuration.

## <a name="how-isvs-can-take-advantage-of-express"></a>Avantages de la fonctionnalité Express pour les éditeurs de logiciels indépendants

Les éditeurs de logiciels indépendants peuvent utiliser WSUS et le client WU pour prendre en charge la distribution des mises à jour Express. Microsoft recommande de suivre les trois étapes suivantes, ces dernières étant expliquées plus en détail dans les sections ci-dessous :

1.  [**Configurer WSUS**](#BKMK_1)

    Le serveur WSUS est requis pour les synchronisations d’analyse et de mise à jour (cliquez [ici](https://technet.microsoft.com/library/dn800972(v=ws.11).aspx) pour obtenir de plus amples informations)

2.  [**Spécifier et remplir un cache de fichiers ISV**](#BKMK_2)

    Il est recommandé d’utiliser un cache de fichiers ISV pour héberger le contenu de la mise à jour, qui comprend la mise à jour des fichiers CAB (fichiers.cab) et les packages Express (fichiers.psf).

3.  [**Configurer un agent client ISV pour diriger les opérations du client WU**](#BKMK_3)

>[!NOTE]
>Nécessite l’installation de la mise à jour cumulative de la version 1607 de Windows 10 sortie en janvier 2017 (ou après) ([KB3213986 [build de système d’exploitation 14393.693])](https://support.microsoft.com/help/4009938/january-10-2017-kb3213986-os-build-14393-693).
    
   - L’agent client ISV détermine quelles mises à jour doivent être approuvées et quand les télécharger et les installer.
   - Le client WU détermine les plages d’octets à télécharger et lance la demande de téléchargement

### <a name="step-1-configure-wsus"></a><a name=BKMK_1></a>Étape 1 : Configurer WSUS

WSUS est l’interface de Windows Update et gère toutes les métadonnées décrivant les packages Express à télécharger. Si vous devez effectuer un déploiement, consultez la section [**Vue d’ensemble de Windows Server Update Services 3.0 SP2**](https://technet.microsoft.com/library/dd939931(v=ws.10).aspx). Une fois le WSUS déployé, la première chose à faire et de décider si vous souhaitez stocker le contenu des mises à jour localement sur le serveur WSUS. Lors de la configuration de WSUS, nous vous recommandons de ne pas stocker les mises à jour localement. Cela suppose que vous disposez déjà d’un logiciel dirigeant le déploiement de ces packages dans votre environnement. Pour plus d’informations sur la configuration du stockage local de WSUS, consultez la Section[**Choisir l’emplacement du stockage des mises à jour**](https://technet.microsoft.com/library/cc720494(v=ws.10).aspx).

### <a name="step-2-specify-and-populate-the-isv-file-cache"></a><a name=BKMK_2></a>Étape 2 : Spécifier et remplir un cache de fichiers ISV 

#### <a name="specify-the-isv-file-cache"></a>Spécifier le cache des fichiers ISV

Les nouveaux paramètres de stratégie de groupe et de gestion des appareils mobiles (GAM) côté client détaillés dans la [**référence du fournisseur de services de configuration**](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/configuration-service-provider-reference) définissent l’emplacement du cache des fichiers ISV.

| **Nom**                                              | **Description**                                                                                                                                                      |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Configurer un autre emplacement de téléchargement pour les mises à jour. | Spécifie un autre serveur intranet permettant d’héberger les mises à jour de Microsoft Update. Vous pouvez ensuite utiliser ce service de mise à jour pour procéder à la mise à jour automatique des ordinateurs de votre réseau |

Vous pouvez choisir entre deux options lors de la configuration de l’autre emplacement de téléchargement pour le cache des fichiers ISV :

1. **Spécifiez un nom d’hôte de serveur HTTP ISV**, le cache de fichiers ISV
    
    Cette approche configure le client WU pour qu’il effectue des demandes de téléchargement auprès du serveur HTTP indiqué dans la stratégie

2. **Spécifiez localhost**
 
    Cette approche configure le client WU pour qu’il effectue des demandes de téléchargement auprès du localhost. Cela permet à l’agent client ISV de gérer ces requêtes et de les acheminer comme il convient pour répondre à la demande de téléchargement.

> [!IMPORTANT]
> Le cache des fichiers ISV requiert les éléments suivants :                                                          
> - Le serveur doit être conforme à la norme HTTP 1.1 pour chaque RFC : <http://www.w3.org/Protocols/rfc2616/rfc2616.html>                                                                                                                                                                
> Plus précisément, le serveur Web doit prendre en charge                                                                                                                                                                                                                                       les demandes[**HEAD**](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) et [**GET**](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.htm)<br>                                                                                                                                                                                                                                                                                                  – Requêtes de plages partielles<br>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   – Conservation d’activité<br>                                                                                                                                                                                                                                                                                                                                                                                                                            – Ne pas utiliser Transfer-Encoding:chunked                                                                                                 

#### <a name="populate-the-isv-file-cache"></a>Remplir le cache des fichiers ISV

Le cache des fichiers ISV doit être rempli avec les fichiers associés aux mises à jour devant être installées sur les clients gérés. 

**Pour remplir le cache des fichiers ISV :**

1. Utilisez les [API WSUS](https://msdn.microsoft.com/library/windows/desktop/microsoft.updateservices.administration.updatefile(v=vs.85).aspx) pour accéder au chemin d’accès et au nom de fichier de la mise à jour du service MU.

    Les métadonnées de chaque mise à jour effectuée sur le serveur WSUS contiennent le chemin d’accès et le nom de fichier de la mise à jour sur Microsoft Update (nom d’hôte Microsoft Update en gras suivi du chemin d’accès et du nom de fichier) : **<http://download.windowsupdate.com>** /c/msdownload/update/software/updt/2016/09/windows10.0-kb3195781-x64_0c06079bccc35cba35a48bd2b1ec46f818bd2e74.msu

2. Téléchargez des fichiers à partir de Microsoft Update et stockez-les dans le cache de fichiers ISV en utilisant l’une de ces deux méthodes : 

   - Stocker des fichiers à l’aide du **même chemin d’accès au dossier que celui du service MU**

   - Stocker des fichiers à l’aide d’un **chemin d’accès au dossier défini par ISV**

     Demander au serveur HTTP (ou à localhost) de rediriger les requêtes **HTTP GET** qui font référence au chemin du dossier MU et au nom du fichier vers l’emplacement du fichier ISV.

### <a name="step-3-set-up-an-isv-client-agent-to-direct-wu-client-operations"></a><a name=BKMK_3></a>Étape 3 : Configurer un agent client ISV pour diriger les opérations du client WU

L’agent client ISV gère le téléchargement et l’installation des mises à jour approuvées à l’aide du flux de travail suivant recommandé :

1.  L’agent client ISV demande au client WU d’analyser le serveur WSUS

2.  L’analyse renvoie l’ensemble des mises à jour applicables au client WU.

3.  Le client ISV détermine quelles mises à jour doivent être approuvées et quand les télécharger et les installer.

4.  L’agent client ISV demande au client WU de télécharger les mises à jour approuvées

5.  Une fois les mises à jour téléchargées, l’agent client ISV demande au client WU pour installer les mises à jour approuvées.

Consultez la section [Recherche, téléchargement et installation des mises à jour](https://msdn.microsoft.com/library/windows/desktop/aa387102(v=vs.85).aspx) pour plus d’informations sur l’utilisation du client WU pour l’analyse, le téléchargement et l’installation des mises à jour.

### <a name="download-workflow-options"></a>Télécharger les options de flux de travail

Voici deux illustrations des options de téléchargement de flux de travail à partir d’un cache de fichiers ISV :

![Flux de travail 1](../../media/express-update-delivery-isv-support/image1.png)

![Flux de travail 2](../../media/express-update-delivery-isv-support/image2.png)
### <a name="how-express-download-works"></a>Principe de fonctionnement du téléchargement Express

- Pour les mises à jour du système d’exploitation qui prennent en charge la fonctionnalité Express, deux versions de la charge utile de fichier sont stockées sur le service :

  - **Version complète du fichier** : remplace principalement les versions locales des binaires de mise à jour.

  - **Version Express** : contient les deltas nécessaires à la retouche des binaires existants sur l’appareil. 

    La version complète du fichier et la version Express sont référencées dans les métadonnées de la mise à jour qui ont été téléchargées vers le client lors de la phase d’analyse. 

    **Le téléchargement Express fonctionne comme suit :**

    Le client WU tente dans un premier temps de télécharger la version Express et, dans certains cas, il bascule sur la version complète si besoin (par exemple, en cas de proxy ne prenant pas en charge les requêtes de plages d’octets).

  1. Lorsque le client WU lance un téléchargement Express, **le client WU télécharge tout d’abord un stub** qui fait partie du package Express.

  2. **Le client WU transmet ce stub au programme Windows Installer** qui l’utilise pour générer un inventaire local, en comparant les deltas du fichier sur le périphérique avec ce qui est nécessaire pour accéder à la dernière version du fichier proposé.

  3. Le **programme Windows Installer demande ensuite au client WU de télécharger les plages** ayant été définies comme étant nécessaires.

  4. **Le client WU télécharge ces plages et les transmet au programme Windows Installer**. Ce dernier les applique et détermine ensuite si des plages supplémentaires sont nécessaires. Ce processus continue jusqu’à ce que le programme Windows Installer indique au client WU que toutes les plages nécessaires ont été téléchargées.

  À ce stade, le téléchargement est terminé et la mise à jour peut être installée.

### <a name="how-delivery-optimization-reduces-bandwidth-consumption"></a>Comment l’optimisation de la distribution réduit la consommation de bande passante

L’optimisation de la distribution est une solution de cache distribué avec organisation automatique pour les entreprises cherchant à réduire la consommation de bande passante pour les mises à jour et les mises à niveau du système d’exploitation ainsi que pour les applications. L’optimisation de la distribution permet aux clients de télécharger ces éléments à partir d’autres sources (comme d’autres pairs sur le réseau) en plus de l’emplacement de téléchargement spécifié (le cache des fichiers ISV dans ce scénario).

Par défaut, dans Windows 10 Entreprise et Windows 10 Education, l’optimisation de la distribution autorise le partage de pair à pair uniquement sur le réseau de l’organisation, mais vous pouvez la configurer différemment en utilisant les paramètres de stratégie de groupe et de gestion des appareils mobiles (GAM).

Pour plus d’informations sur l’optimisation de la distribution, consultez la section [Configurer l’optimisation de la distribution pour les mises à jour de Windows 10](https://technet.microsoft.com/itpro/windows/manage/waas-delivery-optimization).
