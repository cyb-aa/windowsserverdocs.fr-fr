---
title: Utiliser les compteurs de performances pour diagnostiquer les problèmes de réactivité d’application sur des hôtes de Session Bureau à distance
description: Votre application s’exécute lentement sur les services Bureau à distance? En savoir plus sur les compteurs de performance que vous pouvez utiliser pour diagnostiquer les problèmes de performances d’application sur des hôtes de service
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 09/19/2018
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 241b2b776a68cf5aec68a4d331201a07f0e5ea53
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133715"
---
# Utiliser les compteurs de performances pour diagnostiquer les problèmes de performances d’application sur des hôtes de Session Bureau à distance

Un des problèmes plus difficiles à diagnostiquer est des performances médiocres des applications, les applications sont en cours d’exécution lente ou ne répondez pas. En règle générale, vous démarrez votre diagnostic par collecte du processeur, mémoire, entrées/sorties disque et d’autres facteurs et ensuite utilisez des outils tels que Windows Performance Analyzer pour essayer de déterminer la cause du problème. Malheureusement dans la plupart des situations ces données ne vous aident à identifier la cause racine, car les compteurs de consommation de ressources variations fréquentes et grandes. Cela rend difficile à lire les données et en corrélation avec le problème signalé. Pour vous aider à résoudre rapidement les problèmes de performances de votre application, nous avons ajouté quelques nouveaux compteurs de performance (disponible [à télécharger](#download-windows-server-insider-software) via le [Programme Windows Insider](https://insider.windows.com)) qui mesurent le flux d’entrée utilisateur.

Le compteur de délai d’entrée utilisateur peut vous aider à identifier rapidement la cause racine pour mauvais utilisateur final que RDP expériences. Ce compteur mesure la durée pendant laquelle tout utilisateur d’entrée (par exemple, utilisation de la souris ou au clavier) reste dans la file d’attente avant qu’il est sélectionné par un processus, et le compteur de sessions locale et distantes.

L’image suivante montre une représentation approximative du flux d’entrée utilisateur à partir du client pour l’application.

![Services Bureau à distance - flux d’entrée utilisateur à partir du client de bureau à distance les utilisateurs à l’application](.\media\rds-user-input.png)

Le compteur de délai d’entrée utilisateur mesure le delta max (au sein d’un intervalle de temps) entre l’entrée en cours en file d’attente et lorsqu’il est sélectionné par l’application dans une [boucle de message de classique](https://msdn.microsoft.com/library/windows/desktop/ms644927.aspx#loop), comme illustré dans le diagramme suivant:

![Services Bureau à distance - flux de compteur de performances de délai d’entrée utilisateur](.\media\rds-user-input-delay.png)

Un détail important de ce compteur est qu’il signale le délai d’entrée maximal d’utilisateurs dans un intervalle configurable. Il s’agit de la plus longue durée pour une entrée d’atteindre l’application, qui peut avoir un impact sur la vitesse d’actions importantes et visibles tel que la saisie.

Par exemple, dans le tableau suivant, le délai d’entrée utilisateur serait signalé comme 1 000 ms dans cet intervalle. Le compteur signale les plus lent d’entrée utilisateur retard dans l’intervalle, car la perception de l’utilisateur de «lent» est déterminée par le temps d’entrée plus lente (au maximum) leur expérience, pas la vitesse moyenne de toutes les entrées totale.

|Nombre| 0 | 1 | 2 |
|------|---|---|---|
|Délai |16 ms| 20 ms| 1 000 ms|

## Activer et utiliser les nouveaux compteurs de performances

Pour utiliser ces nouveaux compteurs de performance, vous devez tout d’abord activer une clé de Registre en exécutant la commande suivante:

```
reg add "HKLM\System\CurrentControlSet\Control\Terminal Server" /v "EnableLagCounter" /t REG_DWORD /d 0x1 /f
```

>[!NOTE]
> Si vous utilisez Windows 10, version 1809 ou ultérieure ou Windows Server 2019 ou version ultérieure, vous n’avez pas besoin d’activer la clé de Registre.

Ensuite, redémarrez le serveur. Ensuite, ouvrez l’Analyseur de performances et sélectionnez le signe plus (+), comme illustré dans la capture d’écran suivante.

![Compteur de performance de délai d’entrée de bureau à distance - une capture d’écran montrant comment ajouter l’utilisateur](.\media\rds-add-user-input-counter-screen.png)

Après cela, vous devez voir la boîte de dialogue Ajouter des compteurs, où vous pouvez sélectionner le **Délai d’entrée utilisateur par processus** ou **Délai d’entrée utilisateur par Session**.

![Services Bureau à distance - une capture d’écran montrant comment ajouter le délai d’entrée utilisateur par session](.\media\rds-user-delay-per-session.png)

![Services Bureau à distance - une capture d’écran montrant comment ajouter le délai d’entrée utilisateur par processus](.\media\rds-user-delay-per-process.png)

Si vous sélectionnez le **Délai de l’entrée utilisateur par processus**, vous verrez les **Instances de l’objet sélectionné** (en d’autres termes, les processus) dans ```SessionID:ProcessID <Process Image>``` format.

Par exemple, si l’application Calculatrice s’exécute dans une [Session ID 1](https://msdn.microsoft.com/library/ms524326.aspx), vous verrez ```1:4232 <Calculator.exe>```.

> [!NOTE]
> Pas de tous les processus sont inclus. Vous ne verrez pas tous les processus qui sont exécutent en tant que système.

Le compteur démarre reporting délai d’entrée utilisateur dès que vous l’ajoutez. Notez que l’échelle maximale est défini sur 100 (ms) par défaut. 

![Services Bureau à distance - un exemple d’activité pour le report de l’entrée utilisateur par processus dans l’Analyseur de performances](.\media\rds-sample-user-input-delay-perfmon.png)

Ensuite, examinons le **Délai de l’entrée utilisateur par Session**. Il existe des instances pour chaque ID de session et leurs compteurs indiquent le délai d’entrée utilisateur de tous les processus au sein de la session spécifiée. En outre, il existe deux instances appelées «Max» (délai d’entrée l’utilisateur maximale sur toutes les sessions) et «Moyenne» (l’acorss moyenne toutes les sessions).

Ce tableau montre un exemple de ces instances visuel. (Vous pouvez obtenir les mêmes informations dans l’Analyseur de performances en basculant vers le type de graphique du rapport.)

|Type de compteur|Nom de l’instance|Délai signalé (ms)|
|---------------|-------------|-------------------|
|Délai d’entrée utilisateur par processus|1:4232 < Calculator.exe >|  200|
|Délai d’entrée utilisateur par processus|2:1000 < Calculator.exe >|  16|
|Délai d’entrée utilisateur par processus|1: 2 000 < Calculator.exe >|  32|
|Délai d’entrée utilisateur par session|1|    200|
|Délai d’entrée utilisateur par session|2|    16|
|Délai d’entrée utilisateur par session|Moyenne|  108|
|Délai d’entrée utilisateur par session|Max|  200|

## Compteurs de performance utilisés dans un système surchargé

Maintenant examinons de plus près ce que vous verrez dans le rapport si dégradation des performances d’une application. Le graphique suivant montre les lectures pour les utilisateurs qui travaillent à distance dans Microsoft Word. Dans ce cas, les performances des serveurs hôtes de service se dégradent au fil du temps que les utilisateurs plus se connecter.

![Services Bureau à distance - un graphique de performances d’exemple pour le serveur d’hôtes de service Microsoft Word en cours d’exécution](.\media\rds-user-input-perf-graph.png)

Voici comment lire les lignes du graphique:

- La ligne rose indique le nombre de sessions connecté sur le serveur.
- La ligne rouge est l’utilisation du processeur.
- La ligne verte est le délai d’entrée utilisateur maximale sur toutes les sessions.
- La ligne bleue (affichée comme noir dans ce graphique) représente le délai d’entrée utilisateur moyen sur toutes les sessions.

Vous remarquerez qu’il existe une corrélation entre l’UC et le délai d’entrée utilisateur, comme l’UC obtienne l’utilisation de plus, l’entrée utilisateur augmente de retard. En outre, car plus les utilisateurs seront ajoutés au système, l’utilisation du processeur obtient plus près à 100 %, conduisant à plus fréquents pics de délai d’entrée utilisateur. Alors que ce compteur est très utile dans les cas où le serveur manque de ressources, vous pouvez également l’utiliser pour effectuer le suivi de délai d’entrée utilisateur associé à une application spécifique.

## Options de configuration

Est important de garder à l’esprit lorsque vous utilisez ce compteur de performance qu’il signale délai d’entrée utilisateur sur un intervalle de 1 000 ms par défaut. Si vous définissez la propriété intervalle de performances compteur exemple (comme illustré dans la capture d’écran suivante), pour quelque chose de différent, la valeur signalée sera incorrecte.

![Services Bureau à distance - les propriétés de l’Analyseur de performances](.\media\rds-user-input-perfmon-properties.png)

Pour résoudre ce problème, vous pouvez définir la clé de Registre suivante pour correspondre à l’intervalle (en millisecondes) que vous souhaitez utiliser. Par exemple, si nous modifions exemple toutes les secondes x à 5 secondes, nous devons définir cette clé à 5000 ms.

```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server]

"LagCounterInterval"=dword:00005000
```

>[!NOTE]
>Si vous utilisez Windows 10, version 1809 ou ultérieure ou Windows Server 2019 ou plus tard, vous n’avez pas besoin définir LagCounterInterval pour corriger le compteur de performance.

Nous avons également ajouté deux clés que peuvent s’avérer utiles sous la même clé de Registre:

**LagCounterImageNameFirst** : cette clé de la valeur `DWORD 1` (valeur par défaut 0 ou la clé n’existe pas). Cela modifie les noms des compteurs de «Nom de l’Image de < SessionID:ProcessId >.» Par exemple, «explorer < 1:7964 >». Cela est utile si vous souhaitez trier par nom de l’image.

**LagCounterShowUnknown** : cette clé de la valeur `DWORD 1` (valeur par défaut 0 ou la clé n’existe pas). Cela affiche tous les processus qui sont exécutent en tant que services ou système. Certains processus seront affichent avec leur session définie comme «?.»

Voici à quoi elle ressemble si vous activez les deux clés:

![Services Bureau à distance - l’Analyseur de performances avec les deux clés sur](.\media\rds-user-input-delay-with-two-counters.png)

## À l’aide des compteurs de nouveau avec les outils non-Microsoft

Outils d’analyse peuvent consommer ce compteur à l’aide de l' [API de l’Analyseur de performances](https://msdn.microsoft.com/library/windows/desktop/aa371903.aspx).

## Télécharger le logiciel de Windows Server Insider

Insiders inscrits peuvent accéder directement à la [page de téléchargement de Windows Server Insider Preview](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewserver) pour obtenir les derniers téléchargements de logiciel Insider.  Pour savoir comment s’inscrire comme Insider, voir [mise en route avec le serveur](https://insider.windows.com/en-us/for-business-getting-started-server/).

## Partager des commentaires

Vous pouvez envoyer des commentaires pour cette fonctionnalité via le Hub de commentaires. Sélectionnez **applications > toutes les autres applications** et inclure «compteurs de performance des services Bureau à distance: l’Analyseur de performances» dans le titre de votre billet.

Pour les idées de fonctionnalité générale, visitez la [page de UserVoice des services Bureau à distance](https://aka.ms/uservoice-rds).
