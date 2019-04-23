---
title: rundll32 printui.dll,PrintUIEntry
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 12fb48b6-5dd8-4cc0-8808-e6a681aceb84 jpjofre
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/25/2018
ms.openlocfilehash: c90641820bfa01c19ae7bf587c5467d3f9c5a01c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836840"
---
# <a name="rundll32-printuidllprintuientry"></a>rundll32 printui.dll,PrintUIEntry

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Automatise les tâches de configuration d’imprimante. printui.dll est le fichier exécutable qui contient les fonctions utilisées par les boîtes de dialogue de configuration imprimante. Ces fonctions peuvent également être appelées à partir d’un script ou un fichier de commandes de ligne de commande, ou ils peuvent ensuite être exécutés de manière interactive à partir de l’invite de commandes. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).  
## <a name="syntax"></a>Syntaxe  
```  
rundll32 printui.dll PrintUIEntry [BaseParameter] [ModificationParameter1] [ModificationParameter2] [ModificationParameterN]  
```  
Vous pouvez également utiliser d’autres syntaxes suivantes, bien que les exemples de cette rubrique utilisent la syntaxe précédente :  
```  
rundll32 printui.dll,PrintUIEntry [BaseParameter] [ModificationParameter1] [ModificationParameter2] [ModificationParameterN]  
```  
```  
rundll32 printui PrintUIEntry [BaseParameter] [ModificationParameter1] [ModificationParameter2] [ModificationParameterN]  
```  
```  
rundll32 printui,PrintUIEntry [BaseParameter] [ModificationParameter1] [ModificationParameter2] [ModificationParameterN]  
```  
## <a name="parameters"></a>Paramètres  
Il existe deux types de paramètres : paramètres et les paramètres de modification de base. Les paramètres de base spécifient la fonction de la commande consiste à effectuer. Ces paramètres peuvent apparaître dans une ligne de commande donnée. Ensuite, vous pouvez modifier le paramètre de base à l’aide d’un ou plusieurs des paramètres de modification s’ils s’appliquent pour le paramètre de base (pas tous les paramètres de modification sont pris en charge par tous les paramètres de base).  
|Paramètres de base|Description|  
|----------|--------|  
|/dl|Supprime l’imprimante locale.|  
|/dn|Supprime une connexion d’imprimante réseau.|  
|/dd|Supprime un pilote d’imprimante.|  
|/e|Affiche les préférences d’impression pour l’imprimante donnée.|  
|/ga|Ajoute une connexion par ordinateur imprimante (la connexion est disponible pour n’importe quel utilisateur sur cet ordinateur lorsqu’ils se connectent).|  
|/ge|Affiche par les connexions d’imprimante ordinateur sur un ordinateur.|  
|/gd|Supprime une connexion par ordinateur imprimante (la connexion est supprimée de la prochaine fois qu’un utilisateur ouvre une session).|  
|/ia|Installe un pilote d’imprimante à l’aide d’un fichier .inf.|  
|/id|Installe un pilote d’imprimante à l’aide de l’imprimante ajouter Assistant Importation de pilote.|  
|/if|Installe une imprimante à l’aide d’un fichier .inf.|  
|/ii|Installe une imprimante à l’aide de l’Assistant Ajout d’imprimante avec un fichier .inf.|  
|/il|Installe une imprimante à l’aide de l’Assistant Ajout d’imprimante.|  
|/in|Se connecte à une imprimante réseau à distance.|  
|/ip|Installe une imprimante à l’aide de l’Assistant d’Installation (disponible à partir de l’interface utilisateur à partir de la gestion de l’impression) d’imprimante réseau.|  
|/k|Imprime une page de test sur une imprimante.|  
|/o|Affiche la file d’attente d’une imprimante.|  
|/p|Affiche les propriétés d’une imprimante. Lorsque vous utilisez ce paramètre, vous devez également spécifier une valeur pour le paramètre modification **/n [nom]**.|  
|/s|Affiche les propriétés d’un serveur d’impression. Si vous souhaitez afficher le serveur d’impression local, il est inutile d’utiliser un paramètre de modification. Toutefois, si vous souhaitez afficher un serveur d’impression à distance, vous devez spécifier le **/c [nom]** paramètre de modification.|  
|/Ss|Indique le type d’informations pour une imprimante est stocké. Si aucune des valeurs pour **/Ss** est spécifié, le comportement par défaut est comme si tous les ont été spécifiés. Utilisez ce paramètre de base avec les valeurs suivantes est placés à la fin de la ligne de commande :<br /><br />-   **2**: Utiliser pour stocker les informations contenues dans la structure de printER_INFO_2 imprimante s. Cette structure contient des informations de base sur l’imprimante telles que son nom, le nom du serveur, le nom de port et le nom de partage.<br />-   **7**: Utilisez cette option pour stocker les informations de service de répertoire contenues dans la structure printER_INFO_7.<br />-   **c**: Utilisez cette option pour stocker les informations de profil de couleur pour une imprimante.<br />-   **d**: Utiliser pour stocker les données spécifiques de l’imprimante tels que l’ID de matériel d’imprimante s.<br />-   **s**: Utiliser pour stocker le descripteur de sécurité de l’imprimante s.<br />-   **g**: Utilisez cette option pour stocker les informations de la structure DEVmode globale de l’imprimante s.<br />-   **m**: Utiliser pour stocker les paramètres minimaux pour l’imprimante. Cela revient à spécifier **2** **d**, et **g**.<br />-   **u**: Utilisez cette option pour stocker les informations dans l’imprimante s par utilisateur structure DEVmode.|  
|/SR|Spécifie les informations relatives à une imprimante sont restaurées et la gestion des conflits dans les paramètres. Utiliser avec les valeurs suivantes est placés à la fin de la ligne de commande :<br /><br />-   **2**: Utiliser pour restaurer les informations contenues dans la structure de printER_INFO_2 imprimante s. Cette structure contient des informations de base sur l’imprimante telles que son nom, le nom du serveur, le nom de port et le nom de partage.<br />-   **7**: Utiliser pour restaurer les informations de service de répertoire contenues dans la structure printER_INFO_7.<br />-   **c**: Utiliser pour restaurer les informations de profil de couleur pour une imprimante.<br />-   **d**: Permet de restaurer des données spécifiques de l’imprimante, telles que l’ID de matériel d’imprimante s.<br />-   **s**: Utiliser pour restaurer le descripteur de sécurité de l’imprimante s.<br />-   **g**: Utiliser pour restaurer les informations contenues dans la structure DEVmode globale de l’imprimante s.<br />-   **m**: Utiliser pour restaurer les paramètres minimaux pour l’imprimante. Cela revient à spécifier **2**, **d**, et **g**.<br />-   **u** permet de restaurer les informations contenues dans le s Print par utilisateur structure DEVmode.<br />-   **r**: si le nom d’imprimante stocké dans le fichier est différent du nom de l’imprimante en cours de restauration, utilisez le nom de l’imprimante actuelle. Cela ne peut pas être spécifié avec **f**. Si ni **r** ni **f** est spécifié et les noms ne correspondent pas, les paramètres de restauration échoue.<br />-   **f**: si le nom d’imprimante stocké dans le fichier est différent du nom de l’imprimante en cours de restauration, utilisez le nom de l’imprimante dans le fichier. Cela ne peut pas être spécifié avec **r**. Si ni **f** ni **r** est spécifié et les noms ne correspondent pas, les paramètres de restauration échoue.<br />-   **p**: si le nom de port dans le fichier en cours de restauration à partir de ne correspond pas le nom de port actuel de l’imprimante en cours de restauration, le nom du port imprimante s actuel est utilisé.<br />-   **h**: si l’imprimante en cours de restauration n’a pas pu être partagé en utilisant le nom de partage de ressources dans le fichier de paramètres enregistrés, puis essayez de partager l’imprimante avec le nom du partage ou un nom de partage généré si ni **H**ni **h** est spécifié et l’imprimante en cours de restauration ne peut pas être partagé avec le nom du partage enregistré, puis restauration échoue.<br />-   **h**: si l’imprimante en cours de restauration ne peut pas être partagée avec le nom du partage enregistré, ne pas partager l’imprimante. Si ni **H** ni **h** est spécifié et l’imprimante en cours de restauration ne peut pas être partagé avec le nom du partage enregistré, puis restauration échoue.<br />-   **J’ai**: si le pilote dans le fichier de paramètres enregistrés ne correspond pas au pilote d’imprimante en cours de restauration, la restauration échoue.|  
|/Xg|Récupère les paramètres pour une imprimante.|  
|/Xs|Définit les paramètres pour une imprimante.|  
|/y|Définit l’imprimante en cours d’installation en tant que l’imprimante par défaut.|  
|/?|Affiche l’aide de produit pour la commande et ses paramètres associés.|  
|@[file]|Spécifie un fichier d’argument de ligne de commande et directement insère le texte dans ce fichier dans la ligne de commande.|  
|Paramètres de modification|Description|  
|--------------|--------|  
|/a [fichier]|Spécifie le nom de fichier binaire.|  
|/b[name]|Spécifie le nom de l’imprimante de base.|  
|/c[name]|Spécifie le nom d’ordinateur si l’action à effectuer est sur un ordinateur distant.|  
|/f[file]|Spécifie le chemin d’accès UNC Universal Naming Convention () et le nom du nom du fichier .inf ou le nom de fichier de sortie, selon la tâche que vous effectuez. Utilisez **/F [fichier]** pour spécifier un fichier .inf dépendants.|  
|/F [fichier]|Spécifie le chemin d’accès UNC et le nom d’un fichier .inf que le fichier .inf spécifié avec **/f [fichier]** dépend.|  
|/h[architecture]|Spécifie l’architecture du pilote. Utilisez une des opérations suivantes : **x86**, **x64**, ou **Itanium**.|  
|/j[provider]|Spécifie le nom du fournisseur d’impression.|  
|/l [chemin]|Spécifie le chemin d’accès UNC où se trouvent les fichiers de pilote d’imprimante que vous utilisez.|  
|/m[model]|Spécifie le nom de modèle de pilote. (Cette valeur peut être spécifiée dans le fichier .inf).|  
|/n[name]|Spécifie le nom de l’imprimante.|  
|/q|Exécute la commande sans notification à l’utilisateur.|  
|/r[port]|Spécifie le nom de port.|  
|/u|Spécifie d’utiliser le pilote d’imprimante existant s’il est déjà installé.|  
|/t[#]|Spécifie la page d’index de base zéro pour démarrer sur.|  
|/v[version]|Spécifie la version du pilote. Si vous ne spécifiez pas une valeur pour **/K**, vous devez spécifier une des valeurs suivantes : **type 2 - mode noyau** ou **type 3 - mode utilisateur**.|  
|/w|invite l’utilisateur à un pilote si le pilote est introuvable dans le fichier .inf qui est spécifié par **/f**.|  
|/Y|Spécifie que les noms d’imprimantes ne doivent pas être générés automatiquement.|  
|/z|Spécifie pour pas automatiquement partager l’imprimante en cours d’installation.|  
|/K|Modifie la signification du paramètre **/h [architecture]** pour accepter **2** à la place de **x86**, **3** à la place de **x64**, ou **4** à la place de **Itanium**. Il modifie également la valeur du paramètre **/v [version]** pour accepter **2** à la place de **type 2 - mode noyau** et **3** à la place de **type 3 - mode utilisateur**.|  
|/Z|Partage de l’imprimante est en cours d’installation. Utiliser uniquement avec le **/if** paramètre.|  
|/MW [message]|Affiche un message d’avertissement à l’utilisateur avant de valider les modifications spécifiées dans la ligne de commande.|  
|/Mq[message]|Affiche un message de confirmation à l’utilisateur avant de valider les modifications spécifiées dans la ligne de commande.|  
|/W [flags]|Spécifie les paramètres ou les options de l’Assistant Installation pour l’Assistant Ajout d’imprimante, l’ajouter imprimante Assistant pilote et l’imprimante réseau.<br /><br />**R**: Permet les Assistants être redémarré à partir de la dernière page.|  
|/G [flags]|Spécifie les paramètres globaux et les options que vous souhaitez utiliser.<br /><br />**w**: Supprime les avertissements de pilote le programme d’installation à l’utilisateur.|  
## <a name="remarks"></a>Notes  
-   Le **PrintUIEntry** mot clé respecte la casse, et vous devez entrer la syntaxe de cette commande avec la mise en majuscules exact indiqué dans les exemples dans cette rubrique.  
-   Consultez [exemples](#BKMK_Examples) dans ce document pour connaître la syntaxe de certaines tâches courantes. Pour plus d’exemples, à une invite de commandes, tapez : **rundll32 PrintUIEntry / ?**  
## <a name="BKMK_Examples"></a>Exemples  
Pour ajouter une nouvelle imprimante à distance, printer1, pour un ordinateur, Client1, qui est visible pour le compte d’utilisateur exécutant cette commande, tapez :  
```  
rundll32 printui.dll PrintUIEntry /in /n\\client1\printer1  
```  
Pour ajouter une imprimante à l’aide de l’imprimante ajouter Assistant et à l’aide d’un fichier .inf, InfFile.inf, se trouve sur le lecteur c: à Infpath, type :  
```  
rundll32 printui.dll PrintUIEntry /ii /f c:\Infpath\InfFile.inf  
```  
Pour supprimer une imprimante, printer1, sur un ordinateur, Client1, tapez :  
```  
rundll32 printui.dll PrintUIEntry /dn /n\\client1\printer1  
```  
Pour ajouter une connexion par ordinateur imprimante, printer2, pour tous les utilisateurs d’un type d’ordinateur, Client2, (la connexion s’appliqueront lorsqu’un utilisateur ouvre une session) :  
```  
rundll32 printui.dll PrintUIEntry /ga /n\\client2\printer2  
```  
Pour supprimer une connexion par ordinateur imprimante, printer2, pour tous les utilisateurs d’un type d’ordinateur, Client2, (la connexion est supprimée quand un utilisateur ouvre une session) :  
```  
rundll32 printui.dll PrintUIEntry /gd /n\\client2\printer2  
```  
Pour afficher les propriétés du serveur d’impression, printServer1, tapez :  
```  
rundll32 printui.dll PrintUIEntry /s /t1 /c\\printserver1  
```  
Pour afficher les propriétés d’une imprimante, printer3, tapez :  
```  
rundll32 printui.dll PrintUIEntry /p /n\\printer3  
```  
## <a name="additional-references"></a>Références supplémentaires  
  
-   [rundll32](rundll32.md)  
-   [Référence des commandes d’impression](print-command-reference.md)  
