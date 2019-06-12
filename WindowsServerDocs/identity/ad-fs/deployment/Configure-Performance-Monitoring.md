---
ms.assetid: 67d8a8d7-2fbd-4ed7-bb41-75769f942024
title: Configurer la surveillance des performances
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6d651670976fca65ee517672c81dc6cebc42fff8
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442483"
---
# <a name="configure-performance-monitoring"></a>Configurer la surveillance des performances
  
## <a name="bkmk_ConfigurePerfMon"></a>  
AD FS comprend ses propres compteurs de performance dédiés pour vous aider à surveiller les performances des serveurs de fédération et d’ordinateurs de proxy de serveur de fédération. Pour utiliser l’Analyseur de performances pour surveiller les performances de vos serveurs AD FS, il est utile créer un nouvel ensemble de collecteurs de données et ajoutez les compteurs d’AD FS à cette vue. La procédure suivante décrit comment configurer la surveillance des performances pour AD FS.  
  
#### <a name="to-configure-performance-monitoring-for-ad-fs-using-performance-monitor"></a>Pour configurer la surveillance des performances pour AD FS à l’aide de l’Analyseur de performances  
  
1. Sur le **Démarrer** , tapez **Analyseur de performances**, puis appuyez sur ENTRÉE.  
  
2. Dans l’arborescence de la console, développez **des ensembles de collecteurs de données**, à droite\-cliquez sur **défini par l’utilisateur**, pointez sur **New**, puis cliquez sur **ensemble de collecteurs de données** .  
  
   L’Assistant créer un nouvel ensemble de collecteurs de données s’affiche.  
  
3. Dans **créer nouvel ensemble de collecteurs données**, pour **nom** tapez un nom pour le nouvel ensemble de collecteurs de données \(tels que « Performances AD FS »\), cliquez sur **créer manuellement \( Advanced\)** , puis cliquez sur **suivant**.  
  
4. Pour le type de données à inclure, vérifiez que **créer des journaux de données** est sélectionnée, puis cliquez sur les cases à cocher pour les types de données suivants : **Compteur de performances**, **données de trace d’événements**, **les informations de configuration système**.  
  
5. Pour les compteurs de performances, développez **AD FS** dans le **compteurs disponibles** liste, puis cliquez sur **ajouter**.  
  
   Les compteurs de performances AD FS doivent apparaître dans le **ajouté compteurs** liste.  
  
6. Lorsque vous êtes invité à ajouter des fournisseurs de suivi d’événements, cliquez sur **ajouter**, sélectionnez **événements AD FS** et **suivi AD FS** à partir de la liste des fournisseurs.  
  
7. Lorsque vous êtes invité à ajouter les clés de Registre à analyser, cliquez sur **suivant**.  
  
8. Lorsque vous êtes invité à spécifier l’emplacement pour enregistrer les données de performances, vous pouvez accepter l’emplacement par défaut \( * *% lecteur_système %\\PerfLogs\\administrateur\\*** <dedonnées\_collecteur\_définir >* , puis cliquez sur **suivant**.  
  
9. Lorsque vous êtes invité à créer l’ensemble de collecteurs de données, sélectionnez **enregistrer et fermer**, puis cliquez sur **Terminer**.  
  
    Le nouvel ensemble de collecteurs de données s’affiche dans l’arborescence de la console sous le **défini par l’utilisateur** nœud.  
  
10. Utilisez les étapes suivantes pour travailler avec les compteurs de performances AD FS :  
  
    -   Pour commencer l’analyse des performances à l’aide d’AD FS\-les compteurs, directement liés au\-cliquez sur l’ensemble de collecteurs de données que vous avez ajouté \(tels que « Performances AD FS »\), puis cliquez sur **Démarrer**.  
  
    -   Pour créer un rapport pour afficher les résultats d’analyse des performances, avec le bouton droit\-cliquez sur l’ensemble de collecteurs de données que vous avez ajouté \(tels que « Performances AD FS »\), puis cliquez sur **dernier rapport**.  
  
    -   Pour mettre fin à une capture de données de performances afin que vous puissiez afficher le dernier rapport, avec le bouton droit\-cliquez sur l’ensemble de collecteurs de données que vous avez ajouté \(tels que « Performances AD FS »\), puis cliquez sur **arrêter**.  
  
    Le dernier rapport est ajouté et numéroté automatiquement \(en commençant à 000001\) sous le **rapport\\défini par l’utilisateur**<em>\\< données\_collecteur \_définir ></em> nœud dans l’arborescence de la console.  
  
## <a name="ad-fs-performance-counters"></a>Compteurs de performances AD FS  
Le tableau suivant répertorie les compteurs de performances AD FS et décrit la façon dont elles sont utiles pour la surveillance de l’activité est liée à un serveur de fédération ou un serveur proxy de fédération.  
  
|Compteur|Description|Peut être utilisé sur : 
|-----------|---------------|------------------- 
|Demandes de jeton|Surveille le nombre de demandes de jeton envoyé au serveur de fédération, y compris les demandes de jeton SSOAuth.|Serveurs de fédération 
|Demande de jetons\/s|Surveille le nombre de demandes de jeton envoyé au serveur de fédération, y compris les demandes de jeton SSOAuth par seconde.|Serveurs de fédération  
|Demandes de métadonnées de fédération|Surveille le nombre de demandes de métadonnées de fédération entrantes envoyées au serveur de fédération.|Serveurs de fédération  
|Demandes de métadonnées de fédération\/s|Surveille le nombre de demandes entrantes de fédération métadonnées par seconde qui sont envoyés au serveur de fédération.|Serveurs de fédération  
|Demandes de résolution d'artefacts|Surveille le nombre de demandes entrantes de fédération métadonnées par seconde qui sont envoyés au serveur de fédération.|Serveurs de fédération  
|Demandes de résolution d’artefact\/s|Surveille le nombre de demandes au point de terminaison résolution artefact par seconde qui sont envoyés au serveur de fédération.|Serveurs de fédération  
|Demandes de proxy|Surveille le nombre de demandes entrantes envoyées au serveur proxy de fédération.|Serveurs proxy de fédération  
|Demandes de proxy\/s|Surveille le nombre de demandes entrantes par seconde qui sont envoyées au serveur proxy de fédération.|Serveurs proxy de fédération  
|Proxy des requêtes MEX|Surveille le nombre de WS entrant\-échange de métadonnées \(MEX\) demandes sont envoyées au serveur proxy de fédération.|Serveurs proxy de fédération 
|Proxy des requêtes MEX\/s|Surveille le nombre de demandes MEX entrantes par seconde qui sont envoyées au serveur proxy de fédération.|Serveurs proxy de fédération  
  

