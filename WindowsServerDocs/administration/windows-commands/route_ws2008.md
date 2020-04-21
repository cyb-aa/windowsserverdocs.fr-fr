---
title: route_ws2008
description: Découvrez comment modifier et afficher des entrées dans la table de routage IP locale.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: afcd666c-0cef-47c2-9bcc-02d202b983b3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: a0287fed8452cb155ea858ff0a544962dd765c3a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835602"
---
# <a name="route_ws2008"></a>route_ws2008

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche et modifie les entrées dans la table de routage IP locale. Utilisé sans paramètres, **route** affiche l’aide.   

## <a name="syntax"></a>Syntaxe  
```  
route [/f] [/p] [<Command> [<Destination>] [mask <Netmask>] [<Gateway>] [metric <Metric>]] [if <Interface>]]  
```  

#### <a name="parameters"></a>Paramètres  

|Paramètre|Description|  
|-------|--------|  
|/f|Efface la table de routage de toutes les entrées qui ne sont pas des itinéraires hôtes (itinéraires avec un masque réseau 255.255.255.255), l’itinéraire réseau de bouclage (itinéraires avec une destination de 127.0.0.0 et un masque réseau 255.0.0.0), ou un itinéraire de multidiffusion (itinéraires avec une destination de 224.0.0.0 et un masque réseau de 240.0.0.0). Si cette option est utilisée conjointement avec l’une des commandes (telles que Add, change ou Delete), la table est effacée avant l’exécution de la commande.|  
|/p|Lorsqu’il est utilisé avec la commande Add, l’itinéraire spécifié est ajouté au registre et est utilisé pour initialiser la table de routage IP chaque fois que le protocole TCP/IP est démarré. Par défaut, les itinéraires ajoutés ne sont pas conservés lorsque le protocole TCP/IP est démarré. Lorsqu’elle est utilisée avec la commande Imprimer, la liste des itinéraires persistants s’affiche. Ce paramètre est ignoré pour toutes les autres commandes. Les itinéraires persistants sont stockés à l’emplacement du Registre **HKEY_LOCAL_MACHINE \system\currentcontrolset\services\tcpip\parameters\persistentroutes**.|  
|> de commande \<|Spécifie la commande que vous souhaitez exécuter. Le tableau suivant répertorie les commandes valides :<p>-   **Ajouter :** ajoute un itinéraire.<br />-   **modifier :** modifie un itinéraire existant.<br />-   **Delete :** supprime un itinéraire ou des itinéraires.<br />-   **Imprimer :** imprime un itinéraire ou des itinéraires.|  
|> de destination \<|Spécifie la destination réseau de l’itinéraire. La destination peut être une adresse réseau IP (où les bits d’hôte de l’adresse réseau sont définis sur 0), une adresse IP pour un itinéraire hôte ou 0.0.0.0 pour l’itinéraire par défaut.|  
|masque \<réseau de masque >|Spécifie la destination réseau de l’itinéraire. La destination peut être une adresse réseau IP (où les bits d’hôte de l’adresse réseau sont définis sur 0), une adresse IP pour un itinéraire hôte ou 0.0.0.0 pour l’itinéraire par défaut.|  
|> de la passerelle \<|Spécifie l’adresse IP de transfert ou de tronçon suivant sur laquelle le jeu d’adresses défini par la destination réseau et le masque de sous-réseau sont accessibles. Pour les itinéraires de sous-réseau attachés localement, l’adresse de la passerelle est l’adresse IP affectée à l’interface attachée au sous-réseau. Pour les itinéraires distants, disponibles sur un ou plusieurs routeurs, l’adresse de la passerelle est une adresse IP directement accessible qui est affectée à un routeur voisin.|  
|mesure \<métrique >|Spécifie une mesure de coût d’entier (entre 1 et 9999) pour l’itinéraire, qui est utilisé lors du choix entre plusieurs itinéraires dans la table de routage qui correspondent le plus à l’adresse de destination d’un paquet transféré. L’itinéraire avec la métrique la plus basse est choisi. La métrique peut refléter le nombre de tronçons, la vitesse du chemin d’accès, la fiabilité du chemin d’accès, le débit du chemin d’accès ou les propriétés d’administration.|  
|Si \<interface >|Spécifie l’index d’interface pour l’interface sur laquelle la destination est accessible. Pour obtenir la liste des interfaces et leurs index d’interface correspondants, utilisez l’affichage de la commande route print. Vous pouvez utiliser des valeurs décimales ou hexadécimales pour l’index d’interface. Pour les valeurs hexadécimales, faites précéder le nombre hexadécimal de 0x. Lorsque le paramètre if est omis, l’interface est déterminée à partir de l’adresse de la passerelle.|  
|/?|Affiche l'aide à l'invite de commandes.|  

## <a name="remarks"></a>Remarks  
- Les valeurs élevées de la colonne **métrique** de la table de routage sont le résultat de l’autorisation de TCP/IP à déterminer automatiquement la métrique des itinéraires dans la table de routage en fonction de la configuration de l’adresse IP, du masque de sous-réseau et de la passerelle par défaut pour chaque interface LAN. La détermination automatique de la métrique de l’interface, activée par défaut, détermine la vitesse de chaque interface et ajuste les métriques des itinéraires pour chaque interface afin que l’interface la plus rapide crée les itinéraires avec la métrique la plus faible. Pour supprimer les métriques volumineuses, désactivez la détermination automatique de la métrique de l’interface à partir des propriétés avancées du protocole TCP/IP pour chaque connexion LAN.  
- Les noms peuvent être utilisés pour la *destination* s’il existe une entrée appropriée dans le fichier de réseaux locaux stocké dans le dossier <strong>systemroot\System32\Drivers\\</strong>etc. Les noms peuvent être utilisés pour la *passerelle* tant qu’ils peuvent être résolus en adresse IP par le biais de techniques de résolution de noms d’hôte standard, telles que les requêtes DNS (Domain Name System), l’utilisation du fichier hosts local stocké dans le dossier <strong>systemroot\System32\Drivers\\</strong>etc et la résolution de noms NetBIOS.  
- Si la commande est **Print** ou **Delete**, le paramètre de la *passerelle* peut être omis et des caractères génériques peuvent être utilisés pour la destination et la passerelle. La valeur de *destination* peut être une valeur générique spécifiée par un astérisque (*). Si la destination spécifiée contient un astérisque (\*) ou un point d’interrogation ( ?), elle est traitée comme un caractère générique et seuls les itinéraires de destination correspondants sont imprimés ou supprimés. L’astérisque correspond à n’importe quelle chaîne et le point d’interrogation correspond à n’importe quel caractère unique. Par exemple, 10.\*. 1, 192,168.\*, 127.\*et \*224\* sont toutes des utilisations valides du caractère générique astérisque.  
- Si vous utilisez une combinaison non valide d’une destination et d’une valeur de masque de sous-réseau (masque de sous-réseau), un message d’erreur « route : adresse de passerelle incorrecte » s’affiche. Ce message d’erreur s’affiche lorsque la destination contient un ou plusieurs bits définis sur 1 dans des emplacements de bits où le bit de masque de sous-réseau correspondant est défini sur 0. Pour tester cette condition, exprimez la destination et le masque de sous-réseau à l’aide de la notation binaire. Le masque de sous-réseau en notation binaire se compose d’une série de 1 bits, représentant la partie de l’adresse du réseau de la destination, et d’une série de 0 bits, représentant la partie de l’adresse hôte de la destination. Vérifiez si des bits de la destination ont la valeur 1 pour la partie de la destination qui est l’adresse d’hôte (telle que définie par le masque de sous-réseau).  
- Le paramètre **/p** est pris en charge uniquement sur la commande de routage pour windows NT 4,0, Windows 2000, Windows Millennium Edition, Windows XP et windows Server 2003. Ce paramètre n’est pas pris en charge par la commande de **routage** pour Windows 95 ou Windows 98.  
- Cette commande est disponible uniquement si le protocole TCP/IP (Internet Protocol) est installé en tant que composant dans les propriétés d’une carte réseau dans connexions réseau.  

## <a name="examples"></a><a name="BKMK_Examples"></a>Illustre  
Pour afficher tout le contenu de la table de routage IP, tapez :  
```  
route print  
```  
Pour afficher les itinéraires dans la table de routage IP qui commencent par 10, tapez :  
```  
route print 10.*  
```  
Pour ajouter un itinéraire par défaut avec l’adresse de passerelle par défaut 192.168.12.1, tapez :  
```  
route add 0.0.0.0 mask 0.0.0.0 192.168.12.1  
```  
Pour ajouter un itinéraire au 10.41.0.0 de destination avec le masque de sous-réseau 255.255.0.0 et l’adresse de tronçon suivant 10.27.0.1, tapez :  
```  
route add 10.41.0.0 mask 255.255.0.0 10.27.0.1  
```  
Pour ajouter un itinéraire persistant au 10.41.0.0 de destination avec le masque de sous-réseau 255.255.0.0 et l’adresse de tronçon suivant 10.27.0.1, tapez :  
```  
route /p add 10.41.0.0 mask 255.255.0.0 10.27.0.1  
```  
Pour ajouter un itinéraire au 10.41.0.0 de destination avec le masque de sous-réseau 255.255.0.0, l’adresse de tronçon suivant 10.27.0.1 et la métrique de coût 7, tapez :  
```  
route add 10.41.0.0 mask 255.255.0.0 10.27.0.1 metric 7  
```  
Pour ajouter un itinéraire au 10.41.0.0 de destination avec le masque de sous-réseau 255.255.0.0, l’adresse de tronçon suivant 10.27.0.1 et l’index d’interface 0x3, tapez :  
```  
route add 10.41.0.0 mask 255.255.0.0 10.27.0.1 if 0x3  
```  
Pour supprimer l’itinéraire vers le 10.41.0.0 de destination avec le masque de sous-réseau 255.255.0.0, tapez :  
```  
route delete 10.41.0.0 mask 255.255.0.0  
```  
Pour supprimer tous les itinéraires de la table de routage IP qui commencent par 10, tapez :  
```  
route delete 10.*  
```  
Pour modifier l’adresse de tronçon suivant de l’itinéraire avec la destination 10.41.0.0 et le masque de sous-réseau de 255.255.0.0 de 10.27.0.1 à 10.27.0.25, tapez :  
```  
route change 10.41.0.0 mask 255.255.0.0 10.27.0.25  
```  

## <a name="additional-references"></a>Références supplémentaires  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
