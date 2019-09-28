---
ms.assetid: ad3f0480-99f7-428a-ab33-6d165a440840
title: Scénario pour obtenir des informations sur vos données à l’aide de la classification
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: cd6a6e9d3cb452a2cd0a48c6207aea181d85dc7b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357446"
---
# <a name="scenario-get-insight-into-your-data-by-using-classification"></a>Scénario : classification des données pour mieux les comprendre

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Le recours à des données et des ressources de stockage prend une importance croissante dans la plupart des organisations. Les administrateurs informatiques sont confrontés à un défi sans cesse grandissant : contrôler des infrastructures de stockage toujours plus volumineuses et complexes et, simultanément, assumer le coût total de possession et le maintenir à des niveaux raisonnables. La gestion des ressources de stockage n'est pas seulement une affaire de volume ou de disponibilité des données. Il s'agit aussi de mettre en place des stratégies d'entreprise et de savoir comment le stockage est exploité pour favoriser une utilisation et une conformité efficaces avec des risques réduits. L’infrastructure de classification des fichiers aide à mieux appréhender vos données en automatisant des processus de classification qui vous permettront de les gérer avec plus d’efficacité. Les méthodes de classification disponibles dans l'infrastructure de classification des fichiers sont les suivantes : manuelle, par programmation et automatique. Cette rubrique est axée sur la méthode de classification automatique des fichiers.  
  
## <a name="BKMK_OVER"></a>Description du scénario  
L'infrastructure de classification des fichiers utilise des règles de classification pour analyser automatiquement les fichiers et les classer en fonction de leur contenu. Les propriétés de classification sont définies de manière centralisée dans Active Directory pour que ces définitions puissent être partagées par les serveurs de fichiers de l'organisation. Vous pouvez créer des règles de classification qui analysent les fichiers à la recherche d'une chaîne standard ou qui correspond à un modèle (expression régulière). Quand un paramètre de classification configuré est détecté dans un fichier, celui-ci est classifié tel que configuré dans la règle de classification. Voici quelques exemples de règles de classification :  
  
-   Classer tout fichier contenant la chaîne « contoso Confidential » comme ayant un impact commercial élevé  
  
-   classifier tout fichier qui contient au moins 10 numéros de sécurité sociale comme comportant des informations d'identification personnelle.  
  
Quand un fichier est classifié, vous pouvez utiliser une tâche de gestion de fichiers pour exécuter une action sur tous les fichiers classifiés d'une certaine manière. Les actions d'une tâche de gestion de fichiers peuvent consister à protéger les droits associés au fichier, à faire expirer le fichier et à exécuter une action personnalisée (par exemple publier des informations sur un service web).  
  
Vous trouverez des informations sur la planification de la configuration de la classification automatique des fichiers dans [Planifier la classification automatique des fichiers](assetId:///e3c3bb4b-3034-42b7-b391-8ef5f5851955).  
  
Vous trouverez la procédure de classement automatique des fichiers dans les [étapes&#41;de démonstration déployer &#40;la classification automatique des fichiers](Deploy-Automatic-File-Classification--Demonstration-Steps-.md).  
  
## <a name="in-this-scenario"></a>Dans ce scénario  
Ce scénario fait partie du scénario de contrôle d'accès dynamique. Pour plus d'informations sur le contrôle d'accès dynamique, voir :  
  
-   [Contrôle d’accès dynamique : Vue d’ensemble du scénario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_APP"></a>Applications pratiques  
Infrastructure de classification des fichiers dans Windows Server 2012 contribue à la Access Control dynamique en permettant aux propriétaires de données métiers de classer et d’étiqueter facilement les données. Les informations de classification stockées dans la stratégie d'accès centralisée vous permettent de définir des stratégies d'accès pour des classes de données qui sont essentielles aux activités de l'entreprise.  
  
## <a name="BKMK_NEW"></a>Fonctionnalités incluses dans ce scénario  
Le tableau ci-dessous répertorie les fonctionnalités incluses dans ce scénario et détaille la manière dont elles prennent en charge ce dernier.  
  
|Fonctionnalité|Prise en charge de ce scénario|  
|-----------|---------------------------------|  
|[Vue d’ensemble du serveur de fichiers Gestionnaire des ressources](https://technet.microsoft.com/library/hh831701.aspx)|L'infrastructure de classification des fichiers est une fonctionnalité du Gestionnaire de ressources du serveur de fichiers.|  
|[Vue d’ensemble des services de stockage et de fichiers](https://technet.microsoft.com/library/hh831487.aspx)|Le Gestionnaire de ressources du serveur de fichiers est une fonctionnalité du rôle serveur Services de fichiers.|  
  


