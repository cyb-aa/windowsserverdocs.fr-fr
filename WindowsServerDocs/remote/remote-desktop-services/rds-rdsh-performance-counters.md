---
title: Utiliser des compteurs de performances pour diagnostiquer les problèmes de réactivité des applications sur des hôtes de Session Bureau à distance
description: Votre application s’exécute lentement sur les services Bureau à distance ? En savoir plus sur les compteurs de performances que vous pouvez utiliser pour diagnostiquer les problèmes de performances d’application sur RDSH
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 09/19/2018
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: f9aafaa34d5c16e45681e88b1ce60e99a9ad2842
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447093"
---
# <a name="use-performance-counters-to-diagnose-app-performance-problems-on-remote-desktop-session-hosts"></a>Utiliser des compteurs de performances pour diagnostiquer les problèmes de performances d’application sur des hôtes de Session Bureau à distance

Un des problèmes plus difficiles à diagnostiquer est performances médiocres des applications, les applications s’exécutent trop lentement ou ne répondent pas. En règle générale, vous démarrez votre diagnostic par collecte d’UC, mémoire, entrée/sortie disque et d’autres mesures, puis outils tels que Windows Performance Analyzer pour tenter de déterminer ce qui provoque le problème. Malheureusement dans la plupart des situations ces données ne vous aider à identifier la cause racine, car les compteurs de consommation de ressources ont des variations fréquentes et de grande taille. Cela rend difficile à lire les données et le corréler avec le problème signalé. Pour vous aider à plus rapidement de résoudre vos problèmes de performances d’application, nous avons ajouté certains nouveaux compteurs de performance (disponible [pour télécharger](#download-windows-server-insider-software) via la [programme Insider de Windows](https://insider.windows.com)) que l’utilisateur mesure les flux d’entrée.

Le compteur de délai d’entrée utilisateur peut vous aider à identifier rapidement la cause de mauvais utilisateur final que rencontre de RDP. Ce compteur mesure la durée pendant laquelle n’importe quel utilisateur d’entrée (par exemple, l’utilisation de la souris ou clavier) reste dans la file d’attente avant qu’il est récupéré par un processus, et le compteur fonctionne dans les sessions locales et distantes.

L’illustration suivante montre une représentation approximative de flux d’entrée d’utilisateur à partir du client à l’application.

![Bureau à distance - flux d’entrée utilisateur à partir du client de bureau à distance des utilisateurs à l’application](./media/rds-user-input.png)

Le compteur de délai d’entrée utilisateur mesure le delta max (dans un intervalle de temps) entre l’entrée en file d’attente et quand elle est récupérée par l’application dans un [boucle de message traditionnel](https://msdn.microsoft.com/library/windows/desktop/ms644927.aspx#loop), comme illustré dans l’organigramme suivant :

![Bureau à distance - flux de compteur de performances délai d’entrée utilisateur](./media/rds-user-input-delay.png)

Un détail important de ce compteur est qu’il indique le délai d’entrée maximal d’utilisateurs dans un intervalle configurable. Il s’agit de la plus longue durée pour une entrée accéder à l’application, ce qui peut avoir un impact sur la vitesse des actions importantes et visibles telles que la saisie.

Par exemple, dans le tableau suivant, le délai d’entrée utilisateur serait signalé en tant que 1 000 ms dans cet intervalle. Le compteur signale à l’utilisateur les plus lentes d’entrée délai dans l’intervalle, car la perception de « lent » est déterminée par l’heure la plus lente d’entrée (maximum) ils rencontrent, pas la vitesse moyenne de toutes les entrées de total.

|Numéro| 0 | 1 | 2 |
|------|---|---|---|
|Délai |16 ms| 20 ms| 1 000 ms|

## <a name="enable-and-use-the-new-performance-counters"></a>Activer et utiliser les nouveaux compteurs de performances

Pour utiliser ces nouveaux compteurs de performance, vous devez d’abord activer une clé de Registre en exécutant cette commande :

```
reg add "HKLM\System\CurrentControlSet\Control\Terminal Server" /v "EnableLagCounter" /t REG_DWORD /d 0x1 /f
```

>[!NOTE]
> Si vous utilisez Windows 10, version 1809 ou version ultérieure ou Windows Server 2019 ou une version ultérieure, vous ne devez activer la clé de Registre.

Ensuite, redémarrez le serveur. Ensuite, ouvrez l’Analyseur de performances et sélectionnez le signe plus (+), comme indiqué dans la capture d’écran suivante.

![Bureau à distance - capture d’écran montrant comment ajouter l’utilisateur d’entrée de compteur de performances délai](./media/rds-add-user-input-counter-screen.png)

Après cela, vous devez voir la boîte de dialogue Ajouter des compteurs, dans laquelle vous pouvez sélectionner **délai d’entrée utilisateur par processus** ou **délai d’entrée utilisateur par Session**.

![Bureau à distance - capture d’écran montrant comment ajouter le délai d’entrée utilisateur par session](./media/rds-user-delay-per-session.png)

![Bureau à distance - capture d’écran montrant comment ajouter le délai d’entrée utilisateur par processus](./media/rds-user-delay-per-process.png)

Si vous sélectionnez **délai d’entrée utilisateur par processus**, vous verrez la **Instances de l’objet sélectionné** (en d’autres termes, les processus) dans ```SessionID:ProcessID <Process Image>``` format.

Par exemple, si l’application de calculatrice est en cours d’exécution un [Session ID 1](https://msdn.microsoft.com/library/ms524326.aspx), vous verrez ```1:4232 <Calculator.exe>```.

> [!NOTE]
> Pas tous les processus sont inclus. Vous ne voyez pas tous les processus qui sont exécutent en tant que système.

Le compteur commence à signaler de délai d’entrée utilisateur dès que vous l’ajoutez. Notez que la mise à l’échelle maximale est définie sur 100 (ms) par défaut. 

![Bureau à distance - un exemple d’activité pour le délai d’entrée utilisateur par processus dans l’Analyseur de performances](./media/rds-sample-user-input-delay-perfmon.png)

Ensuite, nous allons examiner la **délai d’entrée utilisateur par Session**. Il existe des instances pour chaque ID de session et leurs compteurs indiquent le délai d’entrée d’utilisateur de n’importe quel processus au sein de la session spécifiée. En outre, il existe deux instances sont appelées « Max » (le maximal d’utilisateurs d’entrée délai dans toutes les sessions) et « Average » (l’acorss moyenne toutes les sessions).

Ce tableau montre un exemple visuel de ces instances. (Vous pouvez obtenir les mêmes informations dans l’Analyseur de performances en basculant vers le type de graphique du rapport.)

|Type de compteur|Nom de l'instance|Délai signalée (ms)|
|---------------|-------------|-------------------|
|Délai d’entrée utilisateur par processus|1:4232 < Calculator.exe >|  200|
|Délai d’entrée utilisateur par processus|2:1000 < Calculator.exe >|  16|
|Délai d’entrée utilisateur par processus|1 : 2 000 < Calculator.exe >|  32|
|Délai d’entrée utilisateur par session|1|    200|
|Délai d’entrée utilisateur par session|2|    16|
|Délai d’entrée utilisateur par session|Moyenne|  108|
|Délai d’entrée utilisateur par session|Max.|  200|

## <a name="counters-used-in-an-overloaded-system"></a>Compteurs utilisés dans un système surchargé

Maintenant nous allons voir ce que vous verrez dans le rapport si la dégradation de performances pour une application. Le graphique suivant montre les relevés aux utilisateurs qui travaillent à distance dans Microsoft Word. Dans ce cas, les performances du serveur RDSH se dégradent au fil du temps en tant que davantage d’utilisateurs.

![Bureau à distance - un exemple de graphique de performances pour le serveur RDSH Microsoft Word en cours d’exécution](./media/rds-user-input-perf-graph.png)

Voici comment lire les lignes du graphique :

- Le trait rose indique le nombre de sessions ouvertes sur le serveur.
- La ligne rouge est l’utilisation du processeur.
- La ligne verte est le délai d’entrée maximal d’utilisateurs dans les sessions.
- La ligne bleue (affichée en noir dans ce graphique) représente le délai d’entrée utilisateur moyen dans toutes les sessions.

Vous remarquerez qu’il existe une corrélation entre les pics du processeur et de délai d’entrée utilisateur, à mesure que l’UC augmente l’utilisation de plus, l’entrée utilisateur augmente de délai. En outre, comme davantage d’utilisateurs est ajoutés au système, l’utilisation du processeur est proche de l’à 100 %, ce qui conduit à des pics de délai d’entrée utilisateur plus fréquents. Bien que ce compteur est très utile dans les cas où le serveur manque de ressources, vous pouvez également l’utiliser pour effectuer le suivi de délai d’entrée utilisateur liée à une application spécifique.

## <a name="configuration-options"></a>Options de configuration

Un point essentiel à retenir lors de l’utilisation de ce compteur de performance est qu’il indique le délai d’entrée utilisateur un intervalle de 1 000 ms par défaut. Si vous définissez la propriété d’intervalle performances compteur exemple (comme indiqué dans la capture d’écran suivante), à quelque chose de différent, la valeur signalée sera incorrecte.

![Bureau à distance - les propriétés de l’Analyseur de performances](./media/rds-user-input-perfmon-properties.png)

Pour résoudre ce problème, vous pouvez définir la clé de Registre suivante pour faire correspondre l’intervalle (en millisecondes) que vous souhaitez utiliser. Par exemple, si nous modifions exemple toutes les x secondes à 5 secondes, nous devons définir cette clé à 5 000 ms.

```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server]

"LagCounterInterval"=dword:00005000
```

>[!NOTE]
>Si vous utilisez Windows 10, version 1809 ou version ultérieure ou Windows Server 2019 ou une version ultérieure, vous n’avez pas besoin définir LagCounterInterval pour résoudre le compteur de performances.

Nous avons également ajouté deux clés que vous être utiles sous la même clé de Registre :

**LagCounterImageNameFirst** : définissez cette clé sur `DWORD 1` (valeur par défaut 0 ou la clé n’existe pas). Cela modifie les noms de compteur à « Nom de l’Image de < SessionID:ProcessId >. » Par exemple, « explorer < 1:7964 >. » Cela est utile si vous souhaitez trier par nom de l’image.

**LagCounterShowUnknown** : définissez cette clé sur `DWORD 1` (valeur par défaut 0 ou la clé n’existe pas). Cela affiche tous les processus qui sont exécutent en tant que services ou système. Certains processus seront afficheront avec leur session définie comme « ?. »

Voici à quoi elle ressemble si vous activez les deux clés :

![Bureau à distance - l’Analyseur de performances avec les deux clés sur](./media/rds-user-input-delay-with-two-counters.png)

## <a name="using-the-new-counters-with-non-microsoft-tools"></a>À l’aide de nouveaux compteurs avec les outils non Microsoft

Outils de surveillance peuvent consommer ce compteur à l’aide de la [Perfmon API](https://msdn.microsoft.com/library/windows/desktop/aa371903.aspx).

## <a name="download-windows-server-insider-software"></a>Télécharger le logiciel de Windows Server Insider

Initiés inscrits peuvent accéder directement à la [page de téléchargement de Windows Server Insider Preview](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewserver) pour obtenir les derniers logiciels Insider téléchargements.  Pour savoir comment enregistrer en tant qu’une personne interne, consultez [mise en route avec le serveur](https://insider.windows.com/en-us/for-business-getting-started-server/).

## <a name="share-your-feedback"></a>Partagez vos commentaires

Vous pouvez envoyer des commentaires pour cette fonctionnalité via le Hub de commentaires. Sélectionnez **applications > toutes les autres applications** et inclure « compteurs de performances des services Bureau à distance, l’Analyseur de performances » dans le titre de votre billet.

Pour les idées de fonctionnalités générales, visitez le [page Services Bureau à distance UserVoice](https://aka.ms/uservoice-rds).
