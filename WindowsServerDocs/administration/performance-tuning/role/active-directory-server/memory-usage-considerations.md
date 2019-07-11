---
title: Considérations sur l’utilisation de mémoire de réglage des performances d’AD DS
description: Utilisation de la mémoire par le processus Lsass.exe sur les contrôleurs de domaine qui exécutent Windows Server 2012 R2, 2016 et 2019.
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; lindakup
author: Teresa-Motiv
ms.date: 7/3/2019
ms.openlocfilehash: dd33124d7d480f3670fa2781f567eaaa28abbce6
ms.sourcegitcommit: be243a92f09048ca80f85d71555ea6ee3751d712
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67799781"
---
# <a name="memory-usage-considerations-for-ad-ds-performance-tuning"></a>Considérations sur l’utilisation de mémoire pour le réglage des performances d’AD DS

Cet article décrit certains principes fondamentaux de Local Security Authority Subsystem Service LSASS (, également connu sous le processus Lsass.exe), meilleures pratiques pour la configuration de LSASS et attentes concernant l’utilisation de mémoire. Cet article doit être utilisé comme un guide pour l’analyse d’utilisation de performances et de la mémoire de processus LSASS sur les contrôleurs de domaine (DC). Les informations contenues dans cet article peuvent être utiles si vous avez des questions sur comment paramétrer et configurer les serveurs et contrôleurs de domaine pour optimiser ce moteur.  

LSASS est responsable de la gestion de l’authentification de domaine de sécurité locale LSA (autorité) et la gestion de Active Directory. LSASS gère l’authentification pour le client et le serveur, et elle détermine également le moteur d’Active Directory. LSASS est chargé pour les composants suivants :  

- Autorité de sécurité locale
- Service NetLogon
- Service de gestionnaire de comptes (SAM) de sécurité
- Service de serveur LSA
- SSL (Secure Sockets Layer)
- Protocole d’authentification Kerberos v5
- Protocole d’authentification NTLM
- Autres packages d’authentification qui chargent dans LSA

Les services de base de données Active Directory (NTDSAI.dll) fonctionnent avec le moteur de stockage Extensible (ESE, ESENT.dll).

Voici un schéma visuel de l’utilisation de la mémoire LSASS sur un contrôleur de domaine :

![Diagramme des composants qui utilisent la mémoire de processus LSASS](media/domain-controller-lsass-memory-usage.png)  

La quantité de mémoire utilisée par LSASS sur un contrôleur de domaine augmente suivant l’utilisation d’Active Directory. Lorsque les données sont interrogées, il est mis en mémoire cache. Par conséquent, il est normal que LSASS à l’aide d’une quantité de mémoire est supérieure à la taille du fichier de base de données Active Directory (NTDS.dit).

Comme illustré dans le diagramme, utilisation de la mémoire LSASS peut être divisée en plusieurs parties, y compris le cache des tampons de base de données ESE, la banque des versions ESE et autres. Le reste de cet article fournit un aperçu de chacune de ces parties.

## <a name="ese-database-buffer-cache"></a>Mémoire cache de la base de données ESE  
L’utilisation de mémoire de la variable plus grande dans LSASS est la mémoire cache de base de données ESE. La taille du cache peut varier de moins de 1 Mo à la taille de la base de données entière. Étant donné que la taille du cache améliore les performances, le moteur de base de données pour Active Directory (ESENT) tente de conserver le cache aussi grand que possible. Bien que la taille du cache varie selon la sollicitation de la mémoire de l’ordinateur, la taille maximale de la mémoire cache de base de données ESE est *uniquement* limité par la mémoire RAM physique installé sur l’ordinateur. Tant qu’il n’existe aucune autre sollicitation de la mémoire, le cache peut augmenter la taille du fichier de base de données Active Directory NTDS.dit. Plus de la base de données qui peut être mis en cache améliore les performances du contrôleur de domaine seront.  
  
> [!NOTE]
> En raison de la façon dont fonctionne de la base de données mise en cache d’algorithme, sur un système 64 bits sur lequel la taille de la base de données est inférieure à la mémoire RAM disponible, le cache de base de données peut dépasser la taille de la base de données de 30 à 40 %.

## <a name="ese-version-store"></a>Banque des versions ESE

Il est l’utilisation de mémoire de la variable par la banque des versions ESE (la partie rouge dans le diagramme ci-dessus). La quantité de mémoire utilisée varie selon que vous disposez de Windows Server 2019 ou les versions antérieures de Windows.

- Dans les versions de Windows Server antérieure de Windows Server 2019, par défaut que Lsass peut utiliser jusqu'à environ 400 Mo de mémoire (selon le nombre d’unités centrales) sur un ordinateur 64 bits pour la version ESE stocker. Pour plus d’informations sur l’utilisation de la banque des versions consultez le blog ASKDS suivant par Ryan Ries : [La Version Store appelé et ils sont tous les hors des compartiments](https://techcommunity.microsoft.com/t5/Ask-the-Directory-Services-Team/The-Version-Store-Called-and-They-8217-re-All-Out-of-Buckets/ba-p/400415).

- Dans Windows Server 2019, ce processus est simplifié et lorsque le premier démarrage du service de NTDS, la taille de magasin de version ESE est désormais calculée à 10 % de mémoire RAM physique, avec un minimum de 400 Mo et un maximum de 4 Go. Pour des informations à ce sujet et résolution des problèmes de magasin de version, consultez un autre excellent blog de Ryan Ries : [Immersion : Active Directory ESE Version Store change dans Server 2019](https://techcommunity.microsoft.com/t5/Ask-the-Directory-Services-Team/Deep-Dive-Active-Directory-ESE-Version-Store-Changes-in-Server/ba-p/400510).

## <a name="other-memory-use"></a>Utilisation de la mémoire

Enfin, il est code, les piles, les segments de mémoire et les différentes structures de données de taille fixe (par exemple, le cache de schéma). La quantité de mémoire utilisée par LSASS peut varier, selon la charge sur l’ordinateur. À mesure que le nombre de threads en cours d’exécution augmente, tout comme le fait le nombre de piles de mémoire. En moyenne, LSASS utilise 100 Mo à 300 Mo de mémoire pour ces composants fixes. Quand une plus grande quantité de RAM est installée, LSASS peut utiliser davantage de RAM et moins de mémoire virtuelle.

**Limiter ou de réduire le nombre de programmes sur votre contrôleur de domaine ou ajoutez de la RAM supplémentaire le cas échéant**

Pour des performances optimales, LSASS prend autant de RAM que possible sur un contrôleur de domaine donné. LSASS libère cette RAM comme d’autres processus le demande. L’idée est d’optimiser les performances de LSASS tout en prenant en compte pour les autres processus qui peuvent s’exécuter sur un ordinateur. La liste des programmes à surveiller inclut la surveillance des agents. Certains clients ont des agents distincts pour différentes fonctions de serveur qui peuvent consommer beaucoup de ressources RAM. Certains peuvent émettre de nombreuses requêtes WMI, pour lequel nous avons quelques détails ci-dessous.

Pour cette raison et pour augmenter les performances, il est conseillé de limiter ou de réduire le nombre de programmes sur un contrôleur de domaine. S’il n’y a aucune demande de mémoire, LSASS utilise cette mémoire pour mettre en cache la base de données Active Directory et obtenir ainsi des performances optimales.

Si vous remarquez qu’un contrôleur de domaine a des problèmes de performances, regardez également pour les processus avec utilisation de la mémoire significative. Peuvent avoir un problème que vous avez besoin pour résoudre les problèmes. Elles peuvent inclure des composants de Microsoft. Vérifiez que tenir informé des dernières mises à jour de maintenance&mdash;Microsoft inclut des solutions pour une utilisation excessive de la mémoire dans le cadre des qualité des mises à jour, qui peut également aider aux performances de votre contrôleur de domaine.

Il existe des fonctions intégrées de système d’exploitation qui peuvent consommer une RAM importante selon le profil d’utilisation :

- **Serveur de fichiers**. Contrôleurs de domaine sont également des serveurs de fichiers pour les partages SYSVOL et Netlogon, stratégie de groupe et de scripts pour les scripts de démarrage / d’ouverture de session et de la stratégie de maintenance.
  Toutefois, nous voyons les clients utilisent des contrôleurs de domaine pour tout autre contenu du fichier de service. Le serveur de fichiers SMB consomme puis RAM pour suivre les clients actifs, mais avant tout, le contenu du fichier rendrait le cache de fichiers du système d’exploitation augmente et sont en concurrence avec le cache de base de données ESE pour RAM.  

- **Requêtes WMI**. Solutions de surveillance apporter souvent de nombreuses requêtes WMI. Une requête individuelle peut être peu coûteuse à exécuter. Il est souvent le volume d’appels qui entraîne une certaine charge, en particulier que les solutions de surveillance extraire de nouveaux événements à partir de divers journaux d’événements qui gère Windows.  

  Le journal des événements qui produit le volume de la plupart des est généralement le journal des événements de sécurité. Et c’est aussi le journal des événements que les administrateurs de sécurité veulent à collecter, en particulier à partir de contrôleurs de domaine.  

  Le service WMI utilise un schéma d’allocation de mémoire dynamique qui optimise les requêtes. Par conséquent, le service WMI peut allouer une grande quantité de mémoire, à nouveau en concurrence avec le cache de base de données ESE.  
