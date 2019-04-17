---
ms.assetid: 67d8a8d7-2fbd-4ed7-bb41-75769f942024
title: "Configurer l’analyse des performances"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6a7602cddcaee274d42213cd9365f6d1722dab79
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="configure-performance-monitoring"></a>Configurer l’analyse des performances

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012
  
## <a name="bkmk_ConfigurePerfMon"></a>  
ADFS comprend ses propres compteurs de performance dédiés pour vous aider à surveiller les performances des serveurs de fédération et ordinateurs de proxy de serveur de fédération. Pour utiliser l’Analyseur de performances pour surveiller les performances de vos serveurs ADFS, il est utile de créer un nouvel ensemble de collecteurs de données et ajoutez les compteurs ADFS à cette vue. La procédure suivante décrit comment configurer l’analyse des performances pour ADFS.  
  
#### <a name="to-configure-performance-monitoring-for-ad-fs-using-performance-monitor"></a>Pour configurer l’analyse des performances pour ADFS à l’aide de l’Analyseur de performances  
  
1.  Sur le **Démarrer**, tapez **Analyseur de performances**, puis appuyez sur ENTRÉE.  
  
2.  Dans l’arborescence de la console, développez **ensembles de collecteurs de données**, clic droit **défini par l’utilisateur**, pointez sur **New**, puis cliquez sur **ensemble de collecteurs de données**.  
  
    L’Assistant créer un nouvel ensemble de collecteurs de données s’affiche.  
  
3.  Dans **Créer données ensemble de collecteurs de**, pour **nom** tapez un nom pour le nouvel ensemble de collecteurs de données \ (par exemple, «Performances ADFS» \), cliquez sur **créer manuellement \(Advanced\)**, puis cliquez sur **suivant**.  
  
4.  Pour le type de données à inclure, vérifiez que **créer des journaux de données** est sélectionné, puis cliquez sur les cases à cocher pour les types de données suivants: **compteur de Performance**, **données de suivi d’événements**, **informations de configuration système**.  
  
5.  Pour les compteurs de performances, développez **ADFS** dans les **compteurs disponibles** liste, puis cliquez sur **ajouter**.  
  
    Les compteurs de performances ADFS doivent apparaître dans le **ajouté compteurs** liste.  
  
6.  Lorsque vous êtes invité à ajouter des fournisseurs de suivi d’événements, cliquez sur **ajouter**, sélectionnez **ADFS Eventing** et **suivi ADFS** à partir de la liste des fournisseurs.  
  
7.  Lorsque vous êtes invité à ajouter les clés de Registre à analyser, cliquez sur **suivant**.  
  
8.  Lorsque vous êtes invité à spécifier l’emplacement pour enregistrer les données de performances, vous pouvez accepter l’emplacement par défaut \ (**%systemdrive%\\PerfLogs\\Admin\\***< data\_collector\_set >*, puis cliquez sur **suivant**.  
  
9. Lorsque vous êtes invité à créer l’ensemble de collecteurs de données, sélectionnez **enregistrer et fermer**, puis cliquez sur **Terminer**.  
  
    Le nouvel ensemble de collecteurs de données s’affiche dans l’arborescence de la console sous le **défini par l’utilisateur** nœud.  
  
10. Pour travailler avec les compteurs de performances ADFS, procédez comme suit:  
  
    -   Pour commencer l’analyse des performances à l’aide des compteurs ADFS\, clic droit du collecteur de données défini que vous avez ajouté \ (par exemple, «Performances ADFS» \), puis cliquez sur **Démarrer**.  
  
    -   Pour créer un rapport pour afficher les résultats d’analyse des performances, cliquez droit sur le collecteur de données défini que vous avez ajouté \ (par exemple, «Performances ADFS» \), puis cliquez sur **dernier rapport**.  
  
    -   Pour mettre fin à une capture des données de performances afin que vous pouvez afficher le rapport le plus récent, le collecteur de données clic droit définie que vous avez ajouté \ (par exemple, «Performances ADFS» \), puis cliquez sur **arrêter**.  
  
    Le rapport le plus récent est ajouté et numéroté automatiquement \ (commençant à 000001\) sous le **Report\\User défini***\\ < data\_collector\_set >* nœud dans l’arborescence de la console.  
  
## <a name="ad-fs-performance-counters"></a>Compteurs de performances ADFS  
Le tableau suivant répertorie les compteurs de performances ADFS et décrit comment ils sont utiles pour analyser l’activité se rapporte à un serveur de fédération ou un serveur proxy de fédération.  
  
|Compteur|Description|Peut être utilisé sur: 
|-----------|---------------|------------------- 
|Demandes de jeton|Analyse le nombre de demandes de jeton envoyé au serveur de fédération, y compris les demandes de jeton SSOAuth.|Serveurs de fédération 
|Jeton Requests\/s|Analyse le nombre de demandes de jeton envoyé au serveur de fédération, y compris les demandes de jeton SSOAuth par seconde.|Serveurs de fédération  
|Demandes de métadonnées de fédération|Analyse le nombre de demandes de métadonnées de fédération entrantes envoyées au serveur de fédération.|Serveurs de fédération  
|Requests\ des métadonnées de fédération/s|Analyse le nombre de fédération métadonnées demandes entrantes par seconde sont envoyés au serveur de fédération.|Serveurs de fédération  
|Demandes de résolution d’artefacts|Analyse le nombre de fédération métadonnées demandes entrantes par seconde sont envoyés au serveur de fédération.|Serveurs de fédération  
|Artefacts résolution Requests\/s|Analyse le nombre de demandes au point de terminaison résolution artefact par seconde qui sont envoyés au serveur de fédération.|Serveurs de fédération  
|Demandes de proxy|Analyse le nombre de demandes envoyées à un serveur proxy de fédération.|Serveurs proxy de fédération  
|Proxy Requests\/s|Analyse le nombre de demandes par seconde qui sont envoyés au serveur proxy de fédération.|Serveurs proxy de fédération  
|Demandes de proxy MEX|Analyse le nombre de demandes entrantes \(MEX\) WS-Metadata Exchange qui sont envoyés au serveur proxy de fédération.|Serveurs proxy de fédération 
|Proxy MEX Requests\/s|Analyse le nombre de demandes MEX entrantes par seconde qui sont envoyés au serveur proxy de fédération.|Serveurs proxy de fédération  
  

