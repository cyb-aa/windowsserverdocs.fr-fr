---
title: Optimisation des performances des serveurs Web
description: Recommandations concernant l’optimisation des performances pour les serveurs Web sur Windows Server 16
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: davso; ericam; yashi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: ec36d87957e5bbe897597e330e766c3193cd30d0
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80851692"
---
# <a name="performance-tuning-web-servers"></a>Optimisation des performances des serveurs Web


Cette rubrique décrit les méthodes d’optimisation des performances et les recommandations pour les serveurs Web Windows Server 2016.


## <a name="selecting-the-proper-hardware-for-performance"></a>Sélection du matériel approprié pour favoriser la performance


Il est important de sélectionner le matériel approprié pour répondre à la charge Web attendue, en prenant en compte la charge moyenne, la charge de pointe, la capacité, les plans de croissance et les temps de réponse. Les goulots d’étranglement matériels limitent l’efficacité du paramétrage de logiciels.

[L’optimisation des performances pour le matériel serveur](../../hardware/index.md) fournit des recommandations pour éviter les contraintes de performances suivantes :

-   Les processeurs lents offrent une puissance de traitement limitée pour les charges de travail gourmandes en ressources telles que les scénarios ASP, ASP.NET et TLS.

-   Un cache de processeur L2 ou L3/LLC de petite taille peut nuire aux performances.

-   Une quantité limitée de mémoire affecte le nombre de sites pouvant être hébergés, le nombre de scripts de contenu dynamique (tels que ASP.NET) pouvant être stockés et le nombre de pools d’applications ou de processus de travail.

-   La mise en réseau devient un goulot d’étranglement à cause d’une carte réseau inefficace.

-   Le système de fichiers devient un goulot d’étranglement en raison d’un sous-système de disque ou d’un adaptateur de stockage inefficace.

## <a name="operating-system-best-practices"></a>Meilleures pratiques du système d’exploitation


Si possible, installez une nouvelle fois le système d’exploitation. La mise à niveau du logiciel peut laisser des paramètres de registre obsolètes, indésirables ou sous-optimaux, ainsi que des services et applications précédemment installés, qui consomment des ressources si elles sont démarrées automatiquement. Si un autre système d’exploitation est installé et que vous devez le conserver, vous devez installer le nouveau système d’exploitation sur une partition différente. Sinon, la nouvelle installation remplace les paramètres sous %Program Files%\\Common Files.

Pour réduire les interférences d’accès au disque, placez le fichier de page système, le système d’exploitation, les données Web, le cache de modèle ASP et le journal IIS sur des disques physiques distincts, si possible.

Pour réduire les conflits de ressources système, installez Microsoft SQL Server et IIS sur des serveurs différents, si possible.

Évitez d’installer des services et des applications non essentiels. Dans certains cas, il peut être utile de désactiver les services qui ne sont pas requis sur un système.

## <a name="ntfs-file-system-settings"></a>Paramètres du système de fichiers NTFS

Le commutateur global système **NtfsDisableLastAccessUpdate** (REG \_DWORD) 1 se trouve sous **HKLM \\System \\CurrentControlSet \\Control\\ FileSystem** et est défini par défaut sur 1. Ce commutateur réduit la charge et les latences d’E/S du disque en désactivant la mise à jour de l’horodatage pour le dernier accès au fichier ou au répertoire. Les nouvelles installations de Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008 activent ce paramètre par défaut et vous n’avez pas besoin de le modifier. Les versions antérieures de Windows ne définissent pas cette clé. Si votre serveur exécute une version antérieure de Windows ou a été mis à niveau vers Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008, vous devez activer ce paramètre.

La désactivation des mises à jour est efficace lorsque vous utilisez des ensembles de données volumineux (ou de nombreux hôtes) contenant des milliers de répertoires. Nous vous recommandons d’utiliser plutôt la journalisation IIS si vous ne conservez ces informations que pour l’administration Web.

>[!Warning]
> Certaines applications, telles que les utilitaires de sauvegarde incrémentielle, s’appuient sur ces informations de mise à jour et ne fonctionnent pas correctement sans ces dernières.

## <a name="see-also"></a>Voir aussi
- [Optimisation des performances d’IIS 10.0](tuning-iis-10.md)
- [Optimisation de HTTP 1.1/2](http-performance.md)


