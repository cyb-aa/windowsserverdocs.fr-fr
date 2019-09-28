---
title: Utiliser les compteurs de performance pour diagnostiquer les problèmes de réactivité des applications sur les hôtes de session Bureau à distance
description: Votre application s’exécute-t-elle lentement sur les services Bureau à distance ? Découvrez les compteurs de performance que vous pouvez utiliser pour diagnostiquer des problèmes de performance d’application sur les hôtes de session Bureau à distance
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 07/11/2019
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: b59d93d576967ee83b3efecc2630034eab919bf2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403908"
---
# <a name="use-performance-counters-to-diagnose-app-performance-problems-on-remote-desktop-session-hosts"></a>Utiliser les compteurs de performance pour diagnostiquer les problèmes de performance des applications sur les hôtes de session Bureau à distance

> S’applique à : Windows Server 2019, Windows 10

Des performances d’application médiocres, par exemple une application qui s’exécute lentement ou qui ne répond pas, représentent un des problèmes les plus difficiles à diagnostiquer. En règle générale, vous démarrez votre diagnostic par la collecte d’entrées et de sorties de disque, de mémoire, de processeur et d’autres métriques, puis vous utilisez des outils, tels que Windows Performance Analyzer, pour tenter de déterminer la cause du problème. Malheureusement, dans la plupart des cas, ces données ne vous permettent pas d’identifier l’origine, car les compteurs de consommation des ressources enregistrent des variations importantes et fréquentes. La lecture des données pour les corréler avec le problème signalé s’avère donc ardue. Pour vous aider à résoudre les problèmes de performance de vos applications rapidement, nous avons ajouté quelques nouveaux compteurs de performance (disponible [au téléchargement](#download-windows-server-insider-software) via le [Programme Windows Insider](https://insider.windows.com)) qui mesurent les flux de l’entrée utilisateur.

>[!NOTE]
>Le compteur de délai d’entrée utilisateur est compatible uniquement avec :
> - Windows Server 2019 ou ultérieur
> - Windows 10, version 1809 ou ultérieure

Le compteur du délai de l’entrée utilisateur peut vous aider à identifier rapidement l’origine de mauvaises expériences utilisateur final qui sont liées au protocole RDP (Remote Desktop Protocol). Ce compteur mesure la durée pendant laquelle une entrée utilisateur (par exemple, l’utilisation de la souris ou du clavier) reste dans la file d’attente avant d’être récupérée par un processus ; ce compteur fonctionne dans les sessions locales et à distance.

L’illustration suivante montre une représentation approximative du flux de l’entrée utilisateur, du client vers l’application.

![Bureau à distance - Flux de l’entrée utilisateur, du client Bureau à distance des utilisateurs vers l’application](./media/rds-user-input.png)

Le compteur du délai de l’entrée utilisateur mesure l’écart maximal (dans un intervalle de temps) entre le moment où l’entrée est mise en file d’attente et celui où elle est récupérée par l’application dans une [boucle de message classique](https://msdn.microsoft.com/library/windows/desktop/ms644927.aspx#loop), comme illustré dans l’organigramme suivant :

![Bureau à distance - Flux du compteur de performance du délai de l’entrée utilisateur](./media/rds-user-input-delay.png)

Une précision importante sur ce compteur est qu’il renseigne sur le délai de l’entrée utilisateur maximal dans un intervalle configurable. Il s’agit de la plus longue durée nécessaire à une entrée pour accéder à l’application, ce qui peut avoir un impact sur la vitesse d’actions importantes et tangibles, telles que la frappe au clavier.

Ainsi, dans le tableau suivant, le délai de l’entrée utilisateur est indiqué comme étant de 1 000 ms dans cet intervalle. Le compteur indique le délai de l’entrée utilisateur le plus lent dans l’intervalle, car la perception de « lent » par l’utilisateur est déterminée par la durée de l’entrée la plus lente (le maximum) qu’il rencontre, et non par la vitesse moyenne de toutes les entrées totalisées.

|Numéro| 0 | 1 | 2 |
|------|---|---|---|
|Délai |16 ms| 20 ms| 1 000 ms|

## <a name="enable-and-use-the-new-performance-counters"></a>Activer et utiliser les nouveaux compteurs de performance

Pour utiliser ces nouveaux compteurs de performance, vous devez d’abord activer une clé de Registre en exécutant cette commande :

```
reg add "HKLM\System\CurrentControlSet\Control\Terminal Server" /v "EnableLagCounter" /t REG_DWORD /d 0x1 /f
```

>[!NOTE]
> Si vous utilisez Windows 10 (version 1809 ou ultérieure) ou Windows Server 2019 (ou une version ultérieure), il est inutile d’activer la clé de Registre.

Redémarrez le serveur. Ensuite, ouvrez l’Analyseur de performances et sélectionnez le signe plus (+), comme indiqué dans la capture d’écran suivante.

![Bureau à distance - Capture d’écran illustrant l’ajout du compteur de performance du délai de l’entrée utilisateur](./media/rds-add-user-input-counter-screen.png)

La boîte de dialogue Ajouter des compteurs doit s’afficher ; dans celle-ci vous pouvez sélectionner **User Input Delay per Process** (Délai de l’entrée utilisateur par processus) ou **User Input Delay per Session** (Délai de l’entrée utilisateur par session).

![Bureau à distance - Capture d’écran illustrant l’ajout du délai de l’entrée utilisateur par session](./media/rds-user-delay-per-session.png)

![Bureau à distance - Capture d’écran illustrant l’ajout du délai de l’entrée utilisateur par processus](./media/rds-user-delay-per-process.png)

Si vous sélectionnez **Délai de l’entrée utilisateur par processus**, vous voyez les **Instances de l’objet sélectionné** (autrement dit, les processus) dans le format ```SessionID:ProcessID <Process Image>```.

Par exemple, si l’application Calculatrice est en cours d’exécution dans une [ID de session 1](https://msdn.microsoft.com/library/ms524326.aspx), vous voyez ```1:4232 <Calculator.exe>```.

> [!NOTE]
> Tous les processus ne sont pas inclus. Vous ne voyez aucun des processus exécutés en tant que SYSTÈME.

Aussitôt qu’il est ajouté, le compteur commence à donner des informations sur le délai de l’entrée utilisateur. Notez que l’échelle maximale est définie sur 100 (ms) par défaut. 

![Bureau à distance - Exemple d’activité pour le délai de l’entrée utilisateur par processus dans l’Analyseur de performances](./media/rds-sample-user-input-delay-perfmon.png)

Examinons à présent le **délai de l’entrée utilisateur par session**. Il y a des instances pour chaque ID de session, et leurs compteurs renseignent sur le délai de l’entrée utilisateur de n’importe quel processus au sein de la session spécifiée. Il existe par ailleurs deux instances appelées « Max » (le délai maximal de l’entrée utilisateur dans toutes les sessions) et « Average » (le délai moyen dans toutes les sessions).

Ce tableau montre un exemple visuel de ces instances. (Vous pouvez obtenir les mêmes informations dans Perfmon en basculant vers le type de graphe Rapport.)

|Type de compteur|Nom de l'instance|Délai signalé (ms)|
|---------------|-------------|-------------------|
|Délai de l’entrée utilisateur par processus|1:4232 <Calculator.exe>|  200|
|Délai de l’entrée utilisateur par processus|2:1000 <Calculator.exe>|  16|
|Délai de l’entrée utilisateur par processus|1:2000 <Calculator.exe>|  32|
|Délai de l’entrée utilisateur par session|1|    200|
|Délai de l’entrée utilisateur par session|2|    16|
|Délai de l’entrée utilisateur par session|Moyenne|  108|
|Délai de l’entrée utilisateur par session|Max.|  200|

## <a name="counters-used-in-an-overloaded-system"></a>Compteurs utilisés dans un système surchargé

Examinons maintenant le contenu du rapport pour voir comment il se présente si les performances d’une application se dégradent. Le graphe suivant montre les relevés d’utilisateurs travaillant à distance dans Microsoft Word. Dans le cas présent, les performances du serveur RDSH se détériorent au fil du temps, au fur et à mesure que d’autres utilisateurs se connectent.

![Bureau à distance - Exemple de graphe de performance du serveur RDSH exécutant Microsoft Word](./media/rds-user-input-perf-graph.png)

Voici comment interpréter les lignes du graphe :

- La trait rose indique le nombre de sessions ouvertes sur le serveur.
- Le trait rouge représente l’utilisation du processeur.
- Le trait vert symbolise le délai maximal de l’entrée utilisateur dans toutes les sessions.
- Le trait bleu (affiché en noir dans ce graphe) matérialise le délai moyen de l’entrée utilisateur dans toutes les sessions.

Vous pouvez remarquer qu’il existe une corrélation entre les pics du processeur et ceux du délai de l’entrée utilisateur : plus l’utilisation du processeur augmente, plus le délai de l’entrée utilisateur s’allonge. Qui plus est, au fur et à mesure que d’autres utilisateurs sont ajoutés au système, l’utilisation du processeur se rapproche de 100 %, ce qui se traduit par des pics plus fréquents du délai de l’entrée utilisateur. Même si ce compteur s’avère très utile dans des situations où le serveur ne dispose plus de ressources suffisantes, vous pouvez tout aussi bien vous en servir pour effectuer le suivi du délai de l’entrée utilisateur associé à une application particulière.

## <a name="configuration-options"></a>Options de configuration

L’indication du délai de l’entrée utilisateur d’après un intervalle par défaut de 1 000 ms est un point essentiel à retenir lorsque vous utilisez ce compteur de performance. Si vous définissez la propriété de l’intervalle d’échantillonnage du compteur de performance (comme indiqué dans la capture d’écran suivante) sur une autre valeur, l’information donnée sera incorrecte.

![Bureau à distance - Propriétés de votre analyseur de performances](./media/rds-user-input-perfmon-properties.png)

Pour résoudre ce problème, vous pouvez définir la clé de Registre suivante, afin qu’elle corresponde à l’intervalle (en millisecondes) que vous souhaitez utiliser. Par exemple, si nous modifions la valeur de Échantillonner toutes les x secondes, en optant pour « 5 » secondes, nous devons définir cette clé sur 5 000 ms.

```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server]

"LagCounterInterval"=dword:00005000
```

>[!NOTE]
>Si vous utilisez Windows 10 version 1809 ou ultérieure ou Windows Server 2019 ou ultérieur, il est inutile de définir LagCounterInterval pour corriger le compteur de performance.

Nous avons également ajouté deux clés qui peuvent vous être utiles, sous la même clé de Registre :

**LagCounterImageNameFirst** : définissez cette clé sur `DWORD 1` (valeur par défaut 0 ou la clé n’existe pas). Les noms de compteur sont alors modifiés pour « Nom d’image <SessionID:ProcessId> ». Par exemple, « explorer <1:7964> ». Ce procédé est utile si vous souhaitez procéder à un tri par nom d’image.

**LagCounterShowUnknown** : définissez cette clé sur `DWORD 1` (valeur par défaut 0 ou la clé n’existe pas). Vous affichez ainsi tous les processus qui sont exécutés en tant que services ou SYSTÈME. Certains processus peuvent s’afficher avec leur session définie comme « ? ».

Voici à quoi ressemble le résultat que vous obtenez si vous activez les deux clés :

![Bureau à distance - Analyseur de performances avec les deux clés activées](./media/rds-user-input-delay-with-two-counters.png)

## <a name="using-the-new-counters-with-non-microsoft-tools"></a>Utilisation des nouveaux compteurs avec des outils non-Microsoft

Des outils de supervision peuvent consommer ce compteur par le biais de l’[API Perfmon](https://msdn.microsoft.com/library/windows/desktop/aa371903.aspx).

## <a name="download-windows-server-insider-software"></a>Télécharger le logiciel Windows Server Insider

Les utilisateurs inscrits au programme Insider peuvent accéder directement à la [page de téléchargement de Windows Server Insider Preview](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewserver) pour obtenir les téléchargements des logiciels Insider les plus récents.  Pour savoir comment vous inscrire en tant que membre du programme Insider, consultez [Bien démarrer avec Server](https://insider.windows.com/en-us/for-business-getting-started-server/).

## <a name="share-your-feedback"></a>Partager vos commentaires

Vous pouvez envoyer des commentaires concernant cette fonctionnalité via le Hub de commentaires. Sélectionnez **Applications > Toutes les autres applications** et incluez « Compteurs de performance des services Bureau à distance—Analyseur de performances » dans le titre de votre billet.

Pour des idées de fonctionnalités générales, visitez la [page UserVoice des Services Bureau à distance](https://aka.ms/uservoice-rds).
