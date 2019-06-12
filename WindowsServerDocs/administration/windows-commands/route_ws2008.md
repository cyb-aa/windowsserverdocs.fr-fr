---
title: route_ws2008
description: Découvrez comment modifier et afficher les entrées dans la table de routage IP locale.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: afcd666c-0cef-47c2-9bcc-02d202b983b3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 30843fe94ac7a4dc60092adcede60120bc9e627f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441759"
---
# <a name="routews2008"></a>route_ws2008

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche et modifie les entrées dans la table de routage IP locale. Utilisée sans paramètres, **itinéraire** affiche l’aide.   

## <a name="syntax"></a>Syntaxe  
```  
route [/f] [/p] [<Command> [<Destination>] [mask <Netmask>] [<Gateway>] [metric <Metric>]] [if <Interface>]]  
```  

### <a name="parameters"></a>Paramètres  

|Paramètre|Description|  
|-------|--------|  
|/f|Efface la table de routage de toutes les entrées qui ne sont pas des itinéraires d’hôte (itinéraires avec un masque de réseau 255.255.255.255), l’itinéraire de réseau de bouclage (itinéraires avec une destination 127.0.0.0 et un masque de sous-réseau 255.0.0.0) ou un itinéraire multidiffusion (itinéraires avec une destination 224.0.0.0 et un masque de sous-réseau 240.0.0.0). Si elle est utilisée conjointement avec une des commandes (telles qu’ajouter, modifier ou supprimer), la table est effacée avant d’exécuter la commande.|  
|/p|Lorsqu’il est utilisé avec la commande Ajouter, l’itinéraire spécifié est ajouté au Registre et est utilisée pour initialiser la table de routage IP chaque fois que le protocole TCP/IP est démarré. Par défaut, les itinéraires ajoutés ne sont pas conservés lorsque le protocole TCP/IP est démarré. Lorsqu’il est utilisé avec la commande d’impression, la liste des itinéraires s’affiche. Ce paramètre est ignoré pour toutes les autres commandes. Itinéraires persistants sont stockés dans l’emplacement de Registre **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\PersistentRoutes**.|  
|\<Command>|Spécifie la commande à exécuter. Le tableau suivant répertorie les commandes valides :<br /><br />-   **ajouter :** ajoute un itinéraire.<br />-   **modifier :** modifie un itinéraire existant.<br />-   **Supprimer :** supprime un ou plusieurs itinéraires.<br />-   **imprimer :** imprime un ou plusieurs itinéraires.|  
|\<Destination>|Spécifie la destination réseau de l’itinéraire. La destination peut être une adresse de réseau IP (où les bits de l’hôte de l’adresse réseau sont définis sur 0), une adresse IP pour un itinéraire hôte ou 0.0.0.0 pour l’itinéraire par défaut.|  
|masque \<masque réseau >|Spécifie la destination réseau de l’itinéraire. La destination peut être une adresse de réseau IP (où les bits de l’hôte de l’adresse réseau sont définis sur 0), une adresse IP pour un itinéraire hôte ou 0.0.0.0 pour l’itinéraire par défaut.|  
|\<Passerelle >|Spécifie l’adresse IP du tronçon suivant ou de transfert de messages sur laquelle le jeu d’adresses défini par le masque de sous-réseau et de destination du réseau sont accessibles. Pour les itinéraires de sous-réseau connecté localement, l’adresse de passerelle est l’adresse IP affectée à l’interface qui est attaché au sous-réseau. Pour les itinéraires distants, disponibles sur un ou plusieurs routeurs, l’adresse de passerelle est une adresse IP directement accessible qui est affectée à un routeur voisin.|  
|métrique \<métrique >|Spécifie une métrique de coût entier (compris entre 1 et 9999) pour l’itinéraire, qui est utilisé lors du choix parmi plusieurs itinéraires dans la table de routage qui correspondent le plus étroitement à l’adresse de destination d’un paquet transféré. L’itinéraire avec la métrique la plus faible est choisi. La métrique peut refléter le nombre de sauts, la vitesse du chemin d’accès, la fiabilité, débit de chemin d’accès ou les propriétés d’administration.|  
|if \<Interface>|Spécifie l’index de l’interface sur laquelle la destination est joignable. Pour obtenir la liste des interfaces et les index correspondants, utilisez l’affichage de la commande route print. Vous pouvez utiliser des valeurs décimales ou hexadécimales pour l’index d’interface. Pour les valeurs hexadécimales, faites précéder la valeur 0 x. Lorsque le si le paramètre est omis, l’interface est déterminé à partir de l’adresse de passerelle.|  
|/?|Affiche l'aide à l'invite de commandes.|  

## <a name="remarks"></a>Notes  
- Les valeurs élevées dans le **métrique** colonne de la table de routage sont le résultat de l’autorisation de TCP/IP déterminer automatiquement la métrique des itinéraires dans la table de routage basé sur la configuration d’adresse IP, masque de sous-réseau et passerelle par défaut pour chaque interface LAN. Détermination automatique de la métrique de l’interface, activée par défaut, détermine la vitesse de chaque interface et ajuste les métriques d’itinéraires pour chaque interface afin que l’interface la plus rapide crée l’itinéraire avec la métrique la plus faible. Pour supprimer les métriques de grande taille, désactivez le calcul automatique de la métrique de l’interface à partir des propriétés avancées du protocole TCP/IP pour chaque connexion de réseau local.  
- Noms peuvent être utilisés pour *Destination* si une entrée appropriée existe dans le fichier de réseaux locaux stocké dans le <strong>systemroot\System32\Drivers\\</strong>etc. dossier. Noms peuvent être utilisés pour le *passerelle* tant qu’ils peuvent être résolues en une adresse IP via des techniques de résolution de nom hôte standard telles que les requêtes du système DNS (Domain Name), utilisez du fichier Hosts local stocké dans le  <strong>systemroot\system32\drivers\\</strong>dossier etc et la résolution de nom NetBIOS.  
- Si la commande est **imprimer** ou **supprimer**, le *passerelle* paramètre peut être omis et des caractères génériques peuvent être utilisés pour la destination et la passerelle. Le *Destination* valeur peut être une valeur générique spécifiée par un astérisque (*). Si la destination spécifiée contient un astérisque (\*) ou un point d’interrogation ( ?), il est traité comme un caractère générique et seuls les itinéraires de destination correspondants sont imprimés ou supprimés. L’astérisque correspond à n’importe quelle chaîne, et le point d’interrogation correspond à n’importe quel caractère unique. Par exemple, 10. \*.1, 192.168. \*, 127. \*, et \*224\* sont tous valides utilise du caractère générique astérisque.  
- À l’aide d’une combinaison non valide d’une valeur de masque de sous-réseau et de destination affiche un « itinéraire : masque réseau adresse de passerelle incorrecte » message d’erreur. Ce message d’erreur s’affiche lorsque la destination contient un ou plusieurs bits définis sur 1 dans les emplacements de bits où le bit de masque de sous-réseau correspondant est défini sur 0. Pour tester cette condition, exprimez la destination et masque de sous-réseau à l’aide de la notation binaire. Le masque de sous-réseau en notation binaire se compose d’une série de bits 1, représentant la partie adresse de réseau de la destination et une série de bits 0, qui représente la partie hôte de destination. Vérification pour déterminer s’il existe dans la destination des bits qui sont définis sur 1 pour la partie de la destination est l’adresse d’hôte (tel que défini par le masque de sous-réseau).  
- Le **/p** paramètre est uniquement pris en charge sur la commande de l’itinéraire pour Windows NT 4.0, Windows 2000, Windows Millennium edition, Windows XP et Windows Server 2003. Ce paramètre n’est pas pris en charge par le **itinéraire** commande pour Windows 95 ou Windows 98.  
- Cette commande est disponible uniquement si le protocole TCP/IP (Internet Protocol) est installé en tant que composant dans les propriétés d’une carte réseau dans Connexions réseau.  

## <a name="BKMK_Examples"></a>Exemples  
Pour afficher tout le contenu de la table de routage IP, tapez :  
```  
route print  
```  
Pour afficher les itinéraires dans la table de routage IP qui commencent par 10, tapez :  
```  
route print 10.*  
```  
Pour ajouter un itinéraire par défaut avec l’adresse de passerelle par défaut de 192.168.12.1, tapez :  
```  
route add 0.0.0.0 mask 0.0.0.0 192.168.12.1  
```  
Pour ajouter un itinéraire à la destination 10.41.0.0 avec le masque de sous-réseau 255.255.0.0 et l’adresse de tronçon suivant 10.27.0.1, tapez :  
```  
route add 10.41.0.0 mask 255.255.0.0 10.27.0.1  
```  
Pour ajouter un itinéraire persistant à la destination 10.41.0.0 avec le masque de sous-réseau 255.255.0.0 et l’adresse de tronçon suivant 10.27.0.1, tapez :  
```  
route /p add 10.41.0.0 mask 255.255.0.0 10.27.0.1  
```  
Pour ajouter un itinéraire à la destination 10.41.0.0 avec le masque de sous-réseau 255.255.0.0, l’adresse de tronçon suivant 10.27.0.1 et la métrique de coût 7, tapez :  
```  
route add 10.41.0.0 mask 255.255.0.0 10.27.0.1 metric 7  
```  
Pour ajouter un itinéraire à la destination 10.41.0.0 avec le masque de sous-réseau 255.255.0.0, l’adresse de tronçon suivant de 10.27.0.1 et à l’aide de l’index d’interface 0 x 3, tapez :  
```  
route add 10.41.0.0 mask 255.255.0.0 10.27.0.1 if 0x3  
```  
Pour supprimer l’itinéraire à la destination 10.41.0.0 avec le masque de sous-réseau 255.255.0.0, tapez :  
```  
route delete 10.41.0.0 mask 255.255.0.0  
```  
Pour supprimer tous les itinéraires dans la table de routage IP qui commencent avec 10, tapez :  
```  
route delete 10.*  
```  
Pour modifier l’adresse de tronçon suivant de l’itinéraire avec la destination 10.41.0.0 et le masque de sous-réseau 255.255.0.0 de 10.27.0.1 par 10.27.0.25, tapez :  
```  
route change 10.41.0.0 mask 255.255.0.0 10.27.0.25  
```  

## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
