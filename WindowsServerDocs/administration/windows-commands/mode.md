---
title: mode
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 00dabdbeb7f0665c99714d0a97c7d3c78b22e04e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373612"
---
# <a name="mode"></a>mode



Affiche l’état du système, modifie les paramètres système ou reconfigure les ports ou les périphériques. En cas d’utilisation sans paramètre, le **mode** affiche tous les attributs contrôlables de la console et les périphériques com disponibles.

Vous pouvez utiliser le **mode** pour effectuer les tâches suivantes : chaque tâche utilise une syntaxe différente :
-   [Pour configurer un port de communication série](#BKMK_1)
-   [Pour afficher l’état de tous les appareils ou d’un seul appareil](#BKMK_2)
-   [Pour rediriger la sortie d’un port parallèle vers un port de communication série](#BKMK_3)
-   [Pour sélectionner, actualiser ou afficher le nombre de pages de codes de la console](#BKMK_4)
-   [Pour modifier la taille de la mémoire tampon de l’écran d’invite de commandes](#BKMK_5)
-   [Pour définir la vitesse de répétition du clavier](#BKMK_6)

## <a name="BKMK_1"></a>Pour configurer un port de communication série

### <a name="syntax"></a>Syntaxe

```
mode com<M>[:] [baud=<B>] [parity=<P>] [data=<D>] [stop=<S>] [to={on|off}] [xon={on|off}] [odsr={on|off}] [octs={on|off}] [dtr={on|off|hs}] [rts={on|off|hs|tg}] [idsr={on|off}]
```

### <a name="parameters"></a>Paramètres

|  Paramètre  |                                                                                                                                                                                     Description                                                                                                                                                                                     |
|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Com @ no__t-0M > [ :]  |                                                                                                                                                      Spécifie le numéro du port de communication asynchrone prncnfg. vbshronous.                                                                                                                                                      |
|  Baud = \<B >  | Spécifie la vitesse de transmission en bits par seconde. Le tableau suivant répertorie les abréviations valides pour *B* et leurs taux associés.</br>-   **11** = 110 bauds</br>-   **15** = 150 bauds</br>-   **30** = 300 bauds</br>-   **60** = 600 bauds</br>-   **12** = 1200 bauds</br>-   **24** = 2400 bauds</br>-   **48** = 4800 bauds</br>-   **96** = 9600 bauds</br>-   **19** = 19 200 bauds |
| Parity = \<P > |                              Spécifie la manière dont le système utilise le bit de parité pour vérifier les erreurs de transmission. Le tableau suivant répertorie les valeurs valides pour *P*. La valeur par défaut est **e**. Tous les ordinateurs ne prennent pas en charge les valeurs **m** et **s**.</br>-   **n** = aucun</br>-   **e** = même</br>-   **o** = impair</br>-   **m** = Mark</br>-   **s** = espace                              |
|  Data = \<D >  |                                                                                                    Spécifie le nombre de bits de données dans un caractère. Les valeurs valides pour **d** sont comprises dans la plage de 5 à 8. La valeur par défaut est 7. Tous les ordinateurs ne prennent pas en charge les valeurs 5 et 6.                                                                                                     |
|  Stop = \< >  |                                                                                  Spécifie le nombre de bits d’arrêt qui définissent la fin d’un caractère : 1, 1,5 ou 2. Si la vitesse de transmission est de 110, la valeur par défaut est 2. Dans le cas contraire, la valeur par défaut est 1. Tous les ordinateurs ne prennent pas en charge la valeur 1,5.                                                                                   |
|   to = {on    |                                                                                                                                                                                        préférable                                                                                                                                                                                         |
|   Xon = {on   |                                                                                                                                                                                        préférable                                                                                                                                                                                         |
|  ODSR = {on   |                                                                                                                                                                                        préférable                                                                                                                                                                                         |
|  PTOM = {on   |                                                                                                                                                                                        préférable                                                                                                                                                                                         |
|   DTR = {on   |                                                                                                                                                                                         le                                                                                                                                                                                         |
|   RTS = {on   |                                                                                                                                                                                         le                                                                                                                                                                                         |
|  IDSR = {on   |                                                                                                                                                                                        préférable                                                                                                                                                                                         |
|     /?      |                                                                                                                                                                        Affiche l'aide à l'invite de commandes.                                                                                                                                                                         |

## <a name="BKMK_2"></a>Pour afficher l’état de tous les appareils ou d’un seul appareil

### <a name="syntax"></a>Syntaxe

```
mode [<Device>] [/status]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|@no__t 0Device >|Spécifie le nom de l’appareil pour lequel vous souhaitez afficher l’État.|
|/status|Demande l’état de toutes les imprimantes parallèles redirigées. Vous pouvez abréger l’option de ligne de commande **/Status** en tant que **/STA**.|
|/?|Affiche l'aide à l'invite de commandes.|

### <a name="remarks"></a>Notes

En cas d’utilisation sans paramètre, le **mode** affiche l’état de tous les périphériques installés sur votre système.

## <a name="BKMK_3"></a>Pour rediriger la sortie d’un port parallèle vers un port de communication série

### <a name="syntax"></a>Syntaxe

```
mode lpt<N>[:]=com<M>[:]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|LPT @ no__t-0N > [ :]|Obligatoire. Spécifie le port parallèle. Les valeurs valides pour *N* sont comprises entre 1 et 3.|
|com @ no__t-0M > [ :]|Obligatoire. Spécifie le port série. Les valeurs valides pour *M* sont comprises entre 1 et 4.|
|/?|Affiche l'aide à l'invite de commandes.|

### <a name="remarks"></a>Notes

Vous devez être membre du groupe administrateurs pour rediriger l’impression.

### <a name="examples"></a>Exemples

Pour configurer votre système afin qu’il envoie une sortie d’imprimante parallèle à une imprimante série, vous devez utiliser la commande **mode** à deux reprises. La première fois, utilisez le **mode** pour configurer le port série. La deuxième fois, utilisez le **mode** pour rediriger la sortie de l’imprimante parallèle vers le port série que vous avez spécifié dans la commande du premier **mode** .

Par exemple, si votre imprimante série fonctionne à 4800 bauds avec parité paire et qu’elle est connectée au port COM1 (la première connexion série sur votre ordinateur), tapez :
```
mode com1 48,e,,,b
mode lpt1=com1
```
Si vous redirigez la sortie d’imprimante parallèle de LPT1 à COM1, mais que vous décidez ensuite de vouloir imprimer un fichier à l’aide de LPT1, tapez la commande suivante avant d’imprimer le fichier :
```
mode lpt1
```
Cette commande empêche la redirection du fichier de LPT1 vers COM1.

## <a name="BKMK_4"></a>Pour sélectionner, actualiser ou afficher le nombre de pages de codes de la console

### <a name="syntax"></a>Syntaxe

```
mode <Device> codepage select=<YYY>
mode <Device> codepage [/status]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|@no__t 0Device >|Obligatoire. Spécifie l’appareil pour lequel vous souhaitez sélectionner une page de codes. CON est le seul nom valide pour un appareil.|
|CodePage Select =|Obligatoire. Spécifie la page de codes à utiliser avec l’appareil spécifié. Vous pouvez abréger la **page de codes** en **sélectionnant** **CP** **sel**.|
|@NO__T 0YYY >|Obligatoire. Spécifie le numéro de la page de codes à sélectionner. La liste suivante affiche chaque page de codes prise en charge et son pays/région ou langue.</br>437 : États-Unis</br>850 : Multilingue (I latin)</br>852 : Slave (latin II)</br>855 : Cyrillique (russe)</br>857 : Turc</br>860 : Portugais</br>861 : Islandais</br>863 : Canada-français</br>865 : Nordique</br>866 : Russe</br>869 : Grec moderne|
|courante|Obligatoire. Affiche le nombre de pages de codes (le cas échéant) sélectionnées pour l’appareil spécifié.|
|/status|Affiche le nombre de pages de codes actuelles sélectionnées pour l’appareil spécifié. Vous pouvez abréger **/Status** en **/STA**. Que vous spécifiiez ou non **/Status**, le **mode CodePage** affiche le nombre de pages de codes sélectionnées pour l’appareil spécifié.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="BKMK_5"></a>Pour modifier la taille de la mémoire tampon de l’écran d’invite de commandes

### <a name="syntax"></a>Syntaxe

```
mode con[:] [cols=<C>] [lines=<N>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|con [ :]|Obligatoire. Indique que la modification s’applique à la fenêtre d’invite de commandes.|
|cols = \<C >|Spécifie le nombre de colonnes dans la mémoire tampon d’écran de l’invite de commandes.|
|lignes = \<N >|Spécifie le nombre de lignes dans la mémoire tampon d’écran de l’invite de commandes.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="BKMK_6"></a>Pour définir la vitesse de répétition du clavier

### <a name="syntax"></a>Syntaxe

```
mode con[:] [rate=<R> delay=<D>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|con [ :]|Obligatoire. Fait référence au clavier.|
|rate = \<R >|Spécifie la fréquence à laquelle un caractère est répété à l’écran lorsque vous maintenez une touche enfoncée.|
|Delay = \<D >|Spécifie la durée qui doit s’écouler après que vous avez appuyé sur une touche et maintenez-la enfoncée avant que la sortie de caractères ne se répète.|
|/?|Affiche l'aide à l'invite de commandes.|

### <a name="remarks"></a>Notes

- La vitesse de répétition est le taux de répétition d’un caractère lorsque vous maintenez la touche enfoncée pour ce caractère. Le taux de répétitions par répétition comporte deux composants : la vitesse et le délai. Certains claviers ne reconnaissent pas cette commande.
- Utilisation de **rate =** <em>R</em>

  Les valeurs valides sont comprises entre 1 et 32. Ces valeurs sont égales à entre 2 et 30 caractères par seconde. La valeur par défaut est 20 pour les claviers compatibles IBM AT et 21 pour les claviers compatibles IBM PS/2. Si vous définissez la fréquence, vous devez également définir le délai.
- Utilisation du **délai**=*D*

  Les valeurs valides pour *D* sont 1, 2, 3 et 4 (représentant 0,25, 0,50, 0,75 et 1 seconde). La valeur par défaut est 2. Si vous définissez le délai, vous devez également définir la fréquence.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)