---
title: mode
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b59b04f2-b41d-42df-b5be-19c3721445b1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cde0e9786f72823f446202f1c87ad8e9e181d29c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848200"
---
# <a name="mode"></a>mode



Affiche l’état du système, modifie les paramètres du système ou reconfigure des ports ou des appareils. Si utilisée sans paramètres, **mode** affiche tous les attributs contrôlables de la console et les périphériques COM disponibles.

Vous pouvez utiliser **mode** pour effectuer les tâches suivantes : chaque tâche utilise une syntaxe différente :
-   [Pour configurer un port de communication série](#BKMK_1)
-   [Pour afficher l’état de tous les périphériques ou d’un appareil unique](#BKMK_2)
-   [Pour rediriger la sortie d’un port parallèle à un port de communication série](#BKMK_3)
-   [Pour sélectionner, actualiser ou afficher les numéros des pages de codes pour la console](#BKMK_4)
-   [Pour modifier la taille de la mémoire tampon d’écran invite de commandes](#BKMK_5)
-   [Pour définir la vitesse de répétition du clavier](#BKMK_6)

## <a name="BKMK_1"></a>Pour configurer un port de communication série

### <a name="syntax"></a>Syntaxe

```
mode com<M>[:] [baud=<B>] [parity=<P>] [data=<D>] [stop=<S>] [to={on|off}] [xon={on|off}] [odsr={on|off}] [octs={on|off}] [dtr={on|off|hs}] [rts={on|off|hs|tg}] [idsr={on|off}]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Com\<M > [ ::]|Spécifie le numéro de port de communication Prncnfg.vbshronous asynchrones.|
|baud=\<B>|Spécifie la vitesse de transmission en bits par seconde. Le tableau suivant répertorie les abréviations valides pour *B* et leurs tarifs associés.</br>-   **11** = 110 baud</br>-   **15** = 150 baud</br>-   **30** = 300 baud</br>-   **60** = 600 baud</br>-   **12** = 1200 baud</br>-   **24** = 2400 baud</br>-   **48** = 4800 baud</br>-   **96** = 9600 baud</br>-   **19** = 19 200 bauds|
|parité =\<P >|Spécifie comment le système utilise le bit de parité pour rechercher les erreurs de transmission. Le tableau suivant répertorie les valeurs valides pour *P*. La valeur par défaut est **e**. Tous les ordinateurs ne prennent en charge les valeurs **m** et **s**.</br>-   **n** = none</br>-   **e** = even</br>-   **o** = odd</br>-   **m** = marque</br>-   **s** = space|
|data=\<D>|Spécifie le nombre de bits de données dans un caractère. Les valeurs valides pour **d** se trouvent dans la plage de 5 à 8. La valeur par défaut est 7. Tous les ordinateurs ne prennent en charge les valeurs 5 et 6.|
|stop=\<S>|Spécifie le nombre de bits d’arrêt qui définissent la fin d’un caractère : 1, 1,5 ou 2. Si la vitesse de transmission est 110, la valeur par défaut est 2. Sinon, la valeur par défaut est 1. Tous les ordinateurs ne prennent en charge la valeur 1,5.|
|to={on | off}|Spécifie si le traitement de délai d’attente infini est activé ou désactivé. La valeur par défaut est désactivé.|
|xon={on | off}|Spécifie si le protocole xon / xoff pour le contrôle de flux de données est activé ou désactivé.|
|odsr={on | off}|Spécifie si le protocole de sortie qui utilise le circuit prêt DSR (Data Set) est activée ou désactivée.|
|PTOM = {on | off}|Spécifie si le protocole de sortie qui utilise le circuit à envoyer CTS (Clear) est activé ou désactivé.|
|dtr={on | le | hs}|Spécifie si le circuit prêt DTR (Data Terminal) est activée ou désactivée pour la négociation.|
|rts={on | le | hs | tg}|Spécifie si le circuit de demande à envoyer (RTS) est activé, désactivé, protocole ou bascule.|
|idsr={on | off}|Spécifie si le niveau de sensibilité DSR circuit est activé ou désactivé.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="BKMK_2"></a>Pour afficher l’état de tous les périphériques ou d’un appareil unique

### <a name="syntax"></a>Syntaxe

```
mode [<Device>] [/status]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<APPAREIL >|Spécifie le nom de l’appareil pour lequel vous souhaitez afficher l’état.|
|/status|Demande l’état de toutes les imprimantes parallèles redirigés. Vous pouvez abréger la **/Status** une option de ligne de commande en tant que **/sta**.|
|/?|Affiche l'aide à l'invite de commandes.|

### <a name="remarks"></a>Notes

Si utilisée sans paramètres, **mode** affiche l’état de tous les périphériques qui sont installés sur votre système.

## <a name="BKMK_3"></a>Pour rediriger la sortie d’un port parallèle à un port de communication série

### <a name="syntax"></a>Syntaxe

```
mode lpt<N>[:]=com<M>[:]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|LPT\<N > [ ::]|Obligatoire. Spécifie le port parallèle. Les valeurs valides pour *N* se trouvent dans la plage 1 à 3.|
|com\<M > [ ::]|Obligatoire. Spécifie le port série. Les valeurs valides pour *M* se trouvent dans la plage 1 à 4.|
|/?|Affiche l'aide à l'invite de commandes.|

### <a name="remarks"></a>Notes

Vous devez être membre du groupe Administrateurs de rediriger l’impression.

### <a name="examples"></a>Exemples

Pour configurer votre système pour qu’il envoie la sortie de l’imprimante parallèle à une imprimante série, vous devez utiliser le **mode** commande à deux reprises. La première fois, utilisez **mode** pour configurer le port série. La deuxième fois, utilisez **mode** pour rediriger la sortie de l’imprimante parallèle vers le port série que vous avez spécifié dans la première **mode** commande.

Par exemple, si votre imprimante série travaille à 4800 bauds avec parité, et il est connecté au port COM1 (première connexion série sur votre ordinateur), tapez :
```
mode com1 48,e,,,b
mode lpt1=com1
```
Si vous redirigez la sortie d’imprimante parallèle de LPT1 à COM1, mais que vous décidez ensuite que vous souhaitez imprimer un fichier à l’aide de LPT1, tapez la commande suivante avant d’imprimer le fichier :
```
mode lpt1
```
Cette commande empêche la redirection du fichier de LPT1 à COM1.

## <a name="BKMK_4"></a>Pour sélectionner, actualiser ou afficher les numéros des pages de codes pour la console

### <a name="syntax"></a>Syntaxe

```
mode <Device> codepage select=<YYY>
mode <Device> codepage [/status]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<APPAREIL >|Obligatoire. Spécifie le périphérique pour lequel vous souhaitez sélectionner une page de codes. CON est le seul nom valide pour un appareil.|
|page de codes sélectionnez =|Obligatoire. Spécifie la page de codes à utiliser avec le périphérique spécifié. Vous pouvez abréger **page de codes** **sélectionnez** comme **cp** **sel**.|
|\<YYY &GT;|Obligatoire. Spécifie le numéro de page de codes à sélectionner. La liste suivante affiche chaque code page qui est pris en charge et son pays/région ou la langue.</br>437: États-Unis</br>850: Multilingue (Latin I)</br>852: Slave (Latin II)</br>855: Cyrillique (russe)</br>857: Turc</br>860: Portugais</br>861: Islandais</br>863: Français (Canada)</br>865: Europe du Nord</br>866: Russe</br>869: Grec moderne|
|codepage|Obligatoire. Affiche les numéros du code des pages (le cas échéant) qui sont sélectionnés pour le périphérique spécifié.|
|/status|Affiche les numéros des pages de codes en cours sélectionnées pour le périphérique spécifié. Vous pouvez abréger **/Status** à **/sta**. Si vous spécifiez **/Status**, **page de codes de mode** affiche les numéros des pages de codes qui sont sélectionnées pour le périphérique spécifié.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="BKMK_5"></a>Pour modifier la taille de la mémoire tampon d’écran invite de commandes

### <a name="syntax"></a>Syntaxe

```
mode con[:] [cols=<C>] [lines=<N>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|CON [ :]|Obligatoire. Indique que la modification s’applique à la fenêtre d’invite de commandes.|
|cols=\<C>|Spécifie le nombre de colonnes dans l’invite de commandes.|
|lignes =\<N >|Spécifie le nombre de lignes dans la mémoire tampon d’écran invite de commandes.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="BKMK_6"></a>Pour définir la vitesse de répétition du clavier

### <a name="syntax"></a>Syntaxe

```
mode con[:] [rate=<R> delay=<D>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|CON [ :]|Obligatoire. Fait référence à l’aide du clavier.|
|rate=\<R>|Spécifie la fréquence à laquelle un caractère est répété sur l’écran lorsque vous maintenez une touche enfoncée.|
|délai =\<D >|Spécifie la quantité de temps qui doit s’écouler une fois que vous appuyez sur une touche enfoncée avant la sortie de caractères se répète.|
|/?|Affiche l'aide à l'invite de commandes.|

### <a name="remarks"></a>Notes

-   La vitesse de répétition est la vitesse à laquelle un caractère se répète lorsque vous maintenez la touche enfoncée pour ce caractère. La vitesse de répétition a deux composants, la vitesse et le délai. Certains claviers ne reconnaissent pas cette commande.
-   À l’aide de **taux = *** R*

    Les valeurs valides sont compris entre 1 et 32. Ces valeurs sont égales à environ 2 à 30 caractères par seconde. La valeur par défaut est 20 pour les claviers compatibles IBM AT et 21 pour les claviers compatibles IBM PS/2. Si vous définissez la fréquence, vous devez également définir le délai.
-   À l’aide de **délai**=*D*

    Les valeurs valides pour *D* sont 1, 2, 3 et 4 (représentant 0,25, 0,50, 0,75 et 1 seconde). La valeur par défaut est 2. Si vous définissez le délai, vous devez également définir le taux.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)