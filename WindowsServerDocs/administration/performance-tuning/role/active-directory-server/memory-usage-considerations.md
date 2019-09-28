---
title: Considérations sur l’utilisation de la mémoire dans AD DS le réglage des performances
description: Utilisation de la mémoire par le processus Lsass. exe sur les contrôleurs de domaine qui exécutent Windows Server 2012 R2, 2016 et 2019.
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; lindakup
author: Teresa-Motiv
ms.date: 7/3/2019
ms.openlocfilehash: 55ac47d835874ddb8e160603f08cbafa985aad2a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370291"
---
# <a name="memory-usage-considerations-for-ad-ds-performance-tuning"></a>Considérations sur l’utilisation de la mémoire pour le réglage des performances AD DS

Cet article décrit quelques-uns des principes de base du service LSASS (Local Security Authority Subsystem Service) (LSASS, également connu sous le nom de processus Lsass. exe), les meilleures pratiques pour la configuration d’LSASS et les attentes en matière d’utilisation de la mémoire. Cet article doit être utilisé comme guide pour l’analyse des performances et de l’utilisation de la mémoire LSASS sur les contrôleurs de domaine (DC). Les informations contenues dans cet article peuvent être utiles si vous avez des questions sur l’optimisation et la configuration des serveurs et des contrôleurs de l’information pour optimiser ce moteur.  

LSASS est responsable de la gestion de l’authentification de domaine LSA (autorité de sécurité locale) et de la gestion des Active Directory. LSASS gère l’authentification pour le client et le serveur, et il gouverne également le moteur de Active Directory. LSASS est responsable des composants suivants :  

- Autorité de sécurité locale
- Service Netlogon
- Service SAM (Security Accounts Manager)
- Service serveur LSA
- SSL (Secure Sockets Layer)
- Protocole d’authentification Kerberos V5
- Protocole d’authentification NTLM
- Autres packages d’authentification chargés dans LSA

Les services de base de données Active Directory (NTDSAI. dll) fonctionnent avec le moteur de stockage extensible (ESE, ESENT. dll).

Voici un diagramme visuel de l’utilisation de la mémoire LSASS sur un contrôleur de domaines :

![Diagramme des composants qui utilisent la mémoire LSASS](media/domain-controller-lsass-memory-usage.png)  

La quantité de mémoire utilisée par LSASS sur un contrôleur de base augmente en fonction de l’utilisation de Active Directory. Lorsque les données sont interrogées, elles sont mises en cache dans la mémoire. Par conséquent, il est normal de voir LSASS à l’aide d’une quantité de mémoire supérieure à la taille du fichier de base de données Active Directory (NTDS. dit).

Comme illustré dans le diagramme, l’utilisation de la mémoire LSASS peut être divisée en plusieurs parties, notamment le cache des tampons de la base de données ESE, la Banque des versions ESE et d’autres. Le reste de cet article fournit des informations sur chacune de ces parties.

## <a name="ese-database-buffer-cache"></a>Cache de tampons de base de données ESE  
La plus grande utilisation de la mémoire variable dans LSASS est le cache des tampons de la base de données ESE. La taille du cache peut être comprise entre moins de 1 Mo et la taille de la base de données entière. Dans la mesure où un plus grand cache améliore les performances, le moteur de base de données pour Active Directory (ESENT) tente de garder le cache aussi grand que possible. Bien que la taille du cache varie avec la sollicitation de la mémoire dans l’ordinateur, la taille maximale du cache des tampons de la base de données ESE n’est limitée *que* par la RAM physique installée sur l’ordinateur. Tant qu’il n’y a pas d’autre sollicitation de la mémoire, le cache peut croître jusqu’à la taille du fichier de base de données Active Directory NTDS. dit. Plus la base de données qui peut être mise en cache est grande, plus les performances du contrôleur de périphérique sont élevées.  
  
> [!NOTE]
> En raison de la façon dont l’algorithme de mise en cache de base de données fonctionne, sur un système 64 bits sur lequel la taille de la base de données est inférieure à la mémoire RAM disponible, le cache de la base de données peut croître de 30 à 40%.

## <a name="ese-version-store"></a>Banque des versions ESE

L’utilisation de la mémoire par la Banque des versions ESE (la partie rouge dans le diagramme ci-dessus) est utilisée par la variable. La quantité de mémoire utilisée varie selon que vous disposez de Windows Server 2019 ou de versions antérieures de Windows.

- Dans les versions de Windows Server qui prémettent à jour Windows Server 2019, par défaut, LSASS peut utiliser jusqu’à environ 400 Mo de mémoire (selon le nombre de processeurs) sur un ordinateur 64 bits pour la Banque des versions ESE. Pour plus d’informations sur l’utilisation de la Banque des versions, consultez le billet de blog, voir suivant de Ryan Ries : [La Banque des versions a appelé et elles sont toutes en dehors des compartiments](https://techcommunity.microsoft.com/t5/Ask-the-Directory-Services-Team/The-Version-Store-Called-and-They-8217-re-All-Out-of-Buckets/ba-p/400415).

- Dans Windows Server 2019, cette valeur est simplifiée et, lorsque le service NTDS démarre pour la première fois, la taille de la Banque des versions ESE est maintenant calculée comme 10% de RAM physique, avec un minimum de 400 Mo et un maximum de 4 Go. Pour plus d’informations sur le dépannage et la résolution des problèmes du magasin de versions, consultez un autre blog de Ryan Ries : [Présentation approfondie : Active Directory modifications de la Banque des versions ESE dans le serveur 2019 @ no__t-0.

## <a name="other-memory-use"></a>Autre utilisation de la mémoire

Enfin, le code, les piles, les segments de mémoire et les différentes structures de données de taille fixe (par exemple, le cache de schéma). La quantité de mémoire utilisée par LSASS peut varier en fonction de la charge sur l’ordinateur. À mesure que le nombre de threads en cours d’exécution augmente, le nombre de piles de mémoire est donc atteint. En moyenne, LSASS utilise 100 Mo à 300 Mo de mémoire pour ces composants fixes. Quand une plus grande quantité de RAM est installée, LSASS peut utiliser plus de RAM et moins de mémoire virtuelle.

**Limitez ou réduisez le nombre de programmes sur votre contrôleur de domaine ou ajoutez de la mémoire RAM supplémentaire, le cas échéant.**

Pour des performances optimales, LSASS prend autant de RAM que possible sur un contrôleur de périphérique donné. LSASS abandonne cette RAM lorsque d’autres processus le demandent. L’idée est d’optimiser les performances de LSASS tout en tenant compte des autres processus qui peuvent s’exécuter sur un ordinateur. La liste des programmes à surveiller pour comprend des agents d’analyse. Certains clients possèdent des agents distincts pour différentes fonctions serveur qui peuvent consommer des ressources RAM considérables. Certains peuvent émettre de nombreuses requêtes WMI, pour lesquelles nous avons quelques détails ci-dessous.

Pour cette raison et pour améliorer les performances, il est recommandé de limiter ou de réduire le nombre de programmes sur un contrôleur de périphérique. S’il n’y a aucune demande de mémoire, LSASS utilise cette mémoire pour mettre en cache la base de données Active Directory et, par conséquent, obtenir des performances optimales.

Lorsque vous remarquez qu’un contrôleur de périphérique présente des problèmes de performances, surveillez également les processus avec une utilisation importante de la mémoire. Ces problèmes peuvent être liés à un problème que vous devez résoudre. Ils peuvent inclure des composants Microsoft. Veillez à suivre les dernières mises à jour de maintenance @ no__t-0Microsoft fournit des solutions pour une utilisation excessive de la mémoire dans le cadre des mises à jour qualité, qui peuvent également vous aider à optimiser les performances de votre contrôleur de périphérique.

Il existe des fonctionnalités de système d’exploitation intégrées qui peuvent consommer une RAM importante en fonction du profil d’utilisation :

- **Serveur de fichiers**. Les contrôleurs de réseau sont également des serveurs de fichiers pour les partages SYSVOL et Netlogon, la stratégie de groupe de maintenance et les scripts pour les scripts de stratégie et de démarrage/ouverture de session.
  Toutefois, nous voyons que les clients utilisent des contrôleurs de service pour traiter un autre contenu de fichier. Le serveur de fichiers SMB consomme alors la RAM pour suivre les clients actifs, mais au-dessus, le contenu du fichier rend le cache du fichier du système d’exploitation plus grand et en concurrence avec le cache de la base de données ESE pour la RAM.  

- **Requêtes WMI**. Les solutions de surveillance effectuent souvent de nombreuses requêtes WMI. Une requête individuelle peut être peu coûteuse à exécuter. Il s’agit souvent du volume d’appels qui entraîne une surcharge, en particulier lorsque les solutions de surveillance extraient de nouveaux événements à partir des différents journaux des événements gérés par Windows.  

  Le journal des événements qui produit le plus de volume est généralement le journal des événements de sécurité. Il s’agit également du journal des événements que les administrateurs de la sécurité souhaitent collecter, en particulier à partir des contrôleurs de l’un.  

  Le service WMI utilise un schéma d’allocation de mémoire dynamique qui optimise les requêtes. Par conséquent, le service WMI peut allouer une grande quantité de mémoire, qui est de nouveau en concurrence avec le cache de base de données ESE.  
