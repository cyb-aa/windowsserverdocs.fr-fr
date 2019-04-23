---
title: gpresult
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dfaa3adf-2c83-486c-86d6-23f93c5c883c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e0acd4563225bc1c413b2096387ad8c14f6009a4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835970"
---
# <a name="gpresult"></a>gpresult

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche les informations du jeu de stratégie (résultant RSoP) pour un utilisateur distant et un ordinateur.
Pour utiliser les rapports RSoP pour des ordinateurs ciblés à distance via le pare-feu, vous devez disposer de règles de pare-feu qui permettent le trafic réseau entrant sur les ports.
## <a name="syntax"></a>Syntaxe
```
gpresult [/s <system> [/u <USERNAME> [/p [<PASSWOrd>]]]] [/user [<TARGETDOMAIN>\]<TARGETUSER>] [/scope {user | computer}] {/r | /v | /z | [/x | /h] <FILENAME> [/f] | /?}
```
## <a name="parameters"></a>Paramètres
> [!NOTE]
> Excepté lorsque vous utilisez **/ ?**, vous devez inclure une option de sortie, soit **/r**, **/v**, **/z.**, **/x**, ou **/h**.

|Paramètre|Description|
|-------|--------|
|/s \<système\>|Spécifie le nom ou l’adresse IP d’un ordinateur distant. N’utilisez pas de barres obliques inverses. La valeur par défaut est l'ordinateur local.|
|/u \<nom d’utilisateur\>|Utilise les informations d’identification de l’utilisateur spécifié pour exécuter la commande. L’utilisateur par défaut est l’utilisateur qui a ouvert une session l’ordinateur qui exécute la commande.|
|/p [\<PASSWOrd\>]|Spécifie le mot de passe du compte d’utilisateur qui est fourni dans le **/u** paramètre. Si **/p** est omis, **gpresult** vous invite à entrer le mot de passe. **/p** ne peut pas être utilisé avec **/x** ou **/h**.|
|/User [\<domaine cible\>\\]\<TARGETUSER\>|Spécifie l’utilisateur distant dont les données RSoP doit être affichée.|
|l’étendue / {utilisateur &#124; ordinateur}|Affiche les données RSoP pour l’utilisateur ou l’ordinateur. Si **étendue/** est omis, **gpresult** affiche des données RSoP pour l’utilisateur et l’ordinateur.|
|[/x &#124; /h] <FILENAME>|Enregistre le rapport dans un code XML (**/x**) ou HTML (**/h**) format à l’emplacement et avec le nom de fichier spécifié par le *FILENAME* paramètre. Ne peut pas être utilisé avec **/u**, **/p**, **/r**, **/v**, ou **/z**.|
|/f|force **gpresult** pour remplacer le nom de fichier spécifié dans le **/x** ou **/h** option.|
|/r|Affiche les données de synthèse de RSoP.|
|/v|Affiche des informations détaillées. Cela inclut les paramètres détaillés qui ont été appliquées avec une priorité de 1.|
|/z|Affiche toutes les informations disponibles sur la stratégie de groupe. Cela inclut les paramètres détaillés qui ont été appliquées avec une priorité de 1 et versions ultérieures.|
|/?|Affiche l'aide à l'invite de commandes.|
## <a name="remarks"></a>Notes
-   Stratégie de groupe est l’outil d’administration principal pour définir et de contrôler le fonctionnement des programmes, des ressources réseau et le système d’exploitation pour les utilisateurs et ordinateurs d’une organisation. Dans un environnement active directory, stratégie de groupe est appliquée aux utilisateurs ou ordinateurs en fonction de leur appartenance à des sites, domaines ou unités d’organisation.
-   Étant donné que vous pouvez appliquer les paramètres de stratégie qui se chevauchent à n’importe quel ordinateur ou un utilisateur, la fonctionnalité de stratégie de groupe génère un jeu de paramètres de stratégie résultant quand l’utilisateur se connecte. **Gpresult** affiche l’ensemble de paramètres de stratégie qui étaient appliqués obtenus sur l’ordinateur pour l’utilisateur spécifié lors de l’utilisateur connecté.
-   Étant donné que **/v** et **/z.** produisent un grand nombre d’informations, il est utile de rediriger la sortie vers un fichier texte (par exemple, **gpresult/z > policy.txt**).
-   Le **gpresult** commande est disponible dans Windows Server 2012, Windows Server 2008 R2, Windows 2008, Windows 8, Windows 7 et Windows Vista.
## <a name="BKMK_Examples"></a>Exemples
L’exemple suivant récupère des données RSoP pour l’utilisateur distant **nom_utilisateur_cible** de l’ordinateur **srvmain**et affiche les données RSoP sur l’utilisateur uniquement. La commande est exécutée avec les informations d’identification de l’utilisateur **maindom\hiropln**, et **p@ssW23** est entré en tant que le mot de passe pour cet utilisateur.
```
gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /user targetusername /scope user /r
```
L’exemple suivant enregistre toutes les informations disponibles sur la stratégie de groupe pour l’utilisateur distant **nom_utilisateur_cible** de l’ordinateur **srvmain** dans un fichier nommé **policy.txt**. Aucune donnée n’est incluse sur l’ordinateur. La commande est exécutée avec les informations d’identification de l’utilisateur **maindom\hiropln**, et **p@ssW23** est entré en tant que le mot de passe pour cet utilisateur.
```
gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /user targetusername /z > policy.txt
```
L’exemple suivant affiche les données RSoP de l’ordinateur **srvmain** et l’utilisateur connecté. Les données sont incluses sur l’utilisateur et l’ordinateur. La commande est exécutée avec les informations d’identification de l’utilisateur **maindom\hiropln**, et **p@ssW23** est entré en tant que le mot de passe pour cet utilisateur.
```
gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /r
```
## <a name="additional-references"></a>Références supplémentaires
-   [TechCenter stratégie de groupe](https://go.microsoft.com/fwlink/?LinkID=145531)

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
