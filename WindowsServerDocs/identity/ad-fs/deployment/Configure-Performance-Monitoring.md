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
ms.openlocfilehash: 82added5018d83aeb9fe7d8033204a0d19bd047a
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868097"
---
# <a name="configure-performance-monitoring"></a>Configurer la surveillance des performances
  
## <a name="bkmk_ConfigurePerfMon"></a>  
AD FS comprend ses propres compteurs de performances dédiés pour vous aider à surveiller les performances des serveurs de Fédération et des ordinateurs proxy de serveur de Fédération. Pour utiliser l’analyseur de performances afin de surveiller les performances de vos serveurs de AD FS, il est utile de créer un ensemble de collecteurs de données et d’ajouter les compteurs AD FS à cette vue. La procédure suivante décrit comment configurer la surveillance des performances pour AD FS.  
  
#### <a name="to-configure-performance-monitoring-for-ad-fs-using-performance-monitor"></a>Pour configurer l’analyse des performances pour AD FS à l’aide de l’analyseur de performances  
  
1. Dans l’écran d' **Accueil** , tapez **Analyseur de performances**, puis appuyez sur entrée.  
  
2. Dans l’arborescence de la console, développez ensembles de\- **collecteurs de données**, cliquez avec le bouton droit sur **défini par l’utilisateur**, pointez sur **nouveau**, puis cliquez sur **ensemble de collecteurs de données**.  
  
   L’Assistant Création d’un ensemble de collecteurs de données s’affiche.  
  
3. Dans **créer un ensemble de collecteurs de données**, pour **nom** , tapez un nom pour le \(nouvel ensemble de collecteurs de\)données, tel que « performances de AD FS », cliquez sur  **\(créer manuellement avancé\)** , puis sur  **Suivant**.  
  
4. Pour le type de données à inclure, vérifiez que l’option **créer des journaux de données** est sélectionnée, puis activez les cases à cocher correspondant aux types de données suivants : **Compteur de performances**, **données de trace d’événements**, informations de configuration du **système**.  
  
5. Pour les compteurs de performances, développez **AD FS** dans la liste **compteurs disponibles** , puis cliquez sur **Ajouter**.  
  
   Les compteurs de performance AD FS doivent apparaître dans la liste des **compteurs ajoutés** .  
  
6. Lorsque vous êtes invité à ajouter des fournisseurs de suivi d’événements, cliquez sur **Ajouter**, sélectionnez **AD FS eventing** et **AD FS Tracing** dans la liste des fournisseurs.  
  
7. Lorsque vous êtes invité à ajouter des clés de Registre à surveiller, cliquez sur **suivant**.  
  
8. Lorsque vous êtes invité à spécifier l’emplacement d’enregistrement des données de performances, vous pouvez accepter l' \(emplacement par défaut **% systemdrive\\%\\perflogs\\** _< Admin\_Data Collector définissez\_>_ , puis cliquez sur **suivant**.  
  
9. Lorsque vous êtes invité à créer l’ensemble de collecteurs de données, sélectionnez **enregistrer et fermer**, puis cliquez sur **Terminer**.  
  
    Le nouvel ensemble de collecteurs de données s’affiche dans l’arborescence de la console, sous le nœud **défini par l’utilisateur** .  
  
10. Utilisez les étapes suivantes pour utiliser les compteurs de performance AD FS :  
  
    -   Pour commencer l’analyse des performances\-à l’aide de AD FS\-compteurs associés, cliquez avec le bouton droit \(sur l’ensemble de collecteurs\)de données que vous avez ajouté, par exemple « AD FS performance », puis cliquez sur **Démarrer**.  
  
    -   Pour créer un rapport afin d’afficher les résultats de l’analyse\-des performances, cliquez avec le bouton droit \(sur l’ensemble de collecteurs de données que vous avez ajouté, par exemple « AD FS performance »\), puis cliquez sur le **dernier rapport**.  
  
    -   Pour mettre fin à une capture de données de performances afin de pouvoir afficher le rapport le\-plus récent, cliquez avec le bouton droit \(sur l’ensemble de collecteurs de données que vous avez ajouté, par exemple « AD FS performance »\), puis cliquez sur **arrêter**.  
  
    Le rapport le plus récent est ajouté et numéroté \(automatiquement à partir\) de 000001 sous le **rapport\\défini par**<em>\\l'\_utilisateur < ensemble de collecteurs de données\_></em> nœud dans l’arborescence de la console.  
  
## <a name="ad-fs-performance-counters"></a>AD FS des compteurs de performances  
Le tableau suivant répertorie les compteurs de performance AD FS et décrit comment ils sont utiles pour surveiller l’activité qui concerne un serveur de Fédération ou un serveur proxy de Fédération.  
  
|Compteur|Description|Peut être utilisé sur : 
|-----------|---------------|------------------- 
|Demandes de jeton|Analyse le nombre de demandes de jetons envoyées au serveur de Fédération, y compris les demandes de jeton SSOAuth.|Serveurs de fédération 
|Demandes\/de jeton (s)|Analyse le nombre de demandes de jetons envoyées au serveur de Fédération, y compris les demandes de jetons SSOAuth par seconde.|Serveurs de fédération  
|Demandes de métadonnées de fédération|Analyse le nombre de demandes de métadonnées de Fédération entrantes envoyées au serveur de Fédération.|Serveurs de fédération  
|Demandes\/de métadonnées de Fédération s|Analyse le nombre de demandes de métadonnées de Fédération entrantes par seconde envoyées au serveur de Fédération.|Serveurs de fédération  
|Demandes de résolution d'artefacts|Analyse le nombre de demandes de métadonnées de Fédération entrantes par seconde envoyées au serveur de Fédération.|Serveurs de fédération  
|Demandes\/de résolution d’artefact s|Analyse le nombre de demandes au point de terminaison de résolution d’artefact par seconde qui sont envoyées au serveur de Fédération.|Serveurs de fédération  
|Demandes de proxy|Analyse le nombre de demandes entrantes envoyées au serveur proxy de Fédération.|Serveurs proxys de Fédération  
|Requêtes\/de proxy s|Analyse le nombre de demandes entrantes par seconde envoyées au serveur proxy de Fédération.|Serveurs proxys de Fédération  
|Demandes de proxy MEX|Analyse le nombre de\-\) demandes entrantes de \(serveur d’échange de métadonnées WS envoyées au serveur proxy de Fédération.|Serveurs proxys de Fédération 
|Demandes\/de proxy MEX s|Analyse le nombre de requêtes MEX entrantes par seconde qui sont envoyées au serveur proxy de Fédération.|Serveurs proxys de Fédération  
  

