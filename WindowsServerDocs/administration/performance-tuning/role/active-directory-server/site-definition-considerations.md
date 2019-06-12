---
title: Placement des contrôleurs de domaine et de définition de site de réglage des performances d’AD DS
description: Site definition domaine contrôleur placement considérations et de réglage des performances d’Active Directory.
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 9861703e5ae88dcaec5e76d9fab426b928d0cb9a
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811495"
---
# <a name="proper-placement-of-domain-controllers-and-site-considerations"></a>Positionnement correct de contrôleurs de domaine et les considérations de site

Définition de site correcte est essentielle pour les performances. Les clients en dehors de site peuvent rencontrer des performances médiocres des requêtes et les authentifications. En outre, avec l’introduction du protocole IPv6 sur les clients, la demande peut provenir de l’IPv4 ou l’adresse IPv6 et l’Active Directory doit avoir des sites correctement définis pour IPv6. Le système d’exploitation préfère IPv6 vers IPv4 lorsque les deux sont configurés.

À compter de Windows Server 2008, les tentatives de contrôleur de domaine à utiliser la résolution de nom pour effectuer une recherche inversée afin de déterminer le site du client doivent se trouver dans. Cela peut provoquer l’épuisement du Pool de threads ATQ et provoquer le contrôleur de domaine à cesser de répondre. La résolution appropriée pour cela consiste à définir correctement la topologie de site pour IPv6. Pour résoudre ce problème, un peut optimiser l’infrastructure de résolution de nom pour répondre rapidement aux demandes de contrôleur de domaine. Pour plus d’informations, consultez [Windows Server 2008 ou contrôleur de domaine Windows Server 2008 R2 retardée de réponse aux requêtes LDAP ou Kerberos](https://support.microsoft.com/kb/2668820).

La localisation des contrôleurs de domaine en lecture/écriture pour les scénarios où les RODC est en cours d’utilisation est une zone de considération supplémentaire.  Certaines opérations requièrent l’accès à un contrôleur de domaine accessible en écriture ou ciblent un contrôleur de domaine accessible en écriture lorsqu’un contrôleur de domaine en lecture seule suffit.  Optimisation de ces scénarios prendrait deux chemins d’accès :
-   Contacter les contrôleurs de domaine accessible en écriture lorsqu’un contrôleur de domaine en lecture seule suffit.  Cela nécessite un changement de code d’application.
-   Dans lequel un contrôleur de domaine accessible en écriture peut être nécessaire.  Place en lecture-écriture contrôleurs de domaine à des emplacements centraux pour réduire la latence.

Pour plus d’informations d’informations :
-   [Compatibilité des applications avec les contrôleurs](https://technet.microsoft.com/library/cc772597.aspx)
-   [Active Directory Service Interface (ADSI) et la lecture uniquement contrôleur de domaine (RODC) : pour éviter des problèmes de performances](https://blogs.technet.microsoft.com/fieldcoding/2012/06/24/active-directory-service-interface-adsi-and-the-read-only-domain-controller-rodc-avoiding-performance-issues/)

## <a name="optimize-for-referrals"></a>Optimiser pour les redirections

Les redirections sont comment les requêtes LDAP sont redirigés lorsque le contrôleur de domaine n’héberge pas une copie de la partition interrogée. Lorsqu’une référence est retournée, elle contient le nom unique de la partition, un nom DNS et un numéro de port. Le client utilise ces informations pour continuer la requête sur un serveur qui héberge la partition. Il s’agit d’un scénario DCLocator et toutes les recommandations les définitions de sites et placement des contrôleurs de domaine est conservée, mais les applications qui dépendent des références sont souvent négligées. Il est recommandé pour garantir la topologie Active Directory, y compris les définitions de site et le placement des contrôleurs de domaine reflète correctement les besoins du client. En outre, cela peut inclure d’avoir des contrôleurs de domaine à partir de plusieurs domaines dans un seul site, le réglage des paramètres DNS, ou le déplacement du site d’une application.

## <a name="optimization-considerations-for-trusts"></a>Considérations relatives à l’optimisation pour les approbations

Dans un scénario inter-forêts, les approbations sont traitées en fonction de la hiérarchie de domaine suivante : Domaine enfant-grand -&gt; domaine enfant -&gt; domaine - racine de la forêt&gt; domaine enfant -&gt; domaine enfant de Grand. Cela signifie que les canaux à la racine de forêt et chaque parent, sécurisés peut devenir surchargé en raison de l’agrégation des demandes d’authentification en transit les contrôleurs de domaine dans la hiérarchie d’approbation. Cela peut également entraîner des retards dans les annuaires Active Directory de la dispersion géographique volumineuse lors de l’authentification a également des liens à latence élevées pour affecter le flux ci-dessus de transit. Surcharges peuvent se produire dans des scénarios de confiance entre forêts et de bas niveau. Les recommandations suivantes s’appliquent à tous les scénarios :

-   Paramétrer correctement la MaxConcurrentAPI pour prendre en charge de la charge sur le canal sécurisé. Pour plus d’informations, consultez [comment effectuer le réglage des performances pour l’authentification NTLM en utilisant le paramètre MaxConcurrentApi](https://support.microsoft.com/kb/2688798/EN-US).

-   Créer des raccourcis d’approbation comme il convient en fonction lors du chargement.

-   Assurez-vous que chaque contrôleur de domaine dans le domaine est en mesure d’effectuer la résolution de noms et de communiquer avec les contrôleurs de domaine dans le domaine approuvé.

-   Vérifiez que les considérations de localité sont prises en compte pour les approbations.

-   Activer l’authentification Kerberos lorsque cela est possible et réduire l’utilisation du canal sécurisé pour réduire le risque d’exécution dans MaxConcurrentAPI les goulots d’étranglement.

Approbation scénarios correspondent à une zone qui a été de façon cohérente une problématique pour de nombreux clients inter-domaines. Nom résolution de problèmes et de connectivité, souvent en raison des pare-feux, provoquent un épuisement des ressources sur le contrôleur de domaine autorisé à approuver et avoir un impact sur tous les clients. En outre, un scénario souvent négligé est optimiser l’accès aux contrôleurs de domaine approuvé. Voici les zones clés pour garantir que cela fonctionne correctement :

-   Assurez-vous que la résolution de noms DNS et WINS à l’aide de contrôleurs de domaine autorisé à approuver peut résoudre une liste précise des contrôleurs de domaine pour le domaine approuvé.

    -   Les enregistrements ajoutés de manière statique ont tendance à deviennent obsolètes et réintroduire les problèmes de connectivité au fil du temps. DNS envoie, DNS dynamique, et fusionnées infrastructures WINS/DNS sont plus facile à gérer à long terme.

    -   Garantir une configuration correcte des redirecteurs, transfert conditionnel et les copies secondaires pour les zones de recherche directe et inversée pour chaque ressource dans l’environnement dans lequel un client peut devoir accéder. Là encore, cela nécessite une maintenance manuelle et a tendance à devenir obsolète. Consolidation des infrastructures est idéale.

-   Les contrôleurs de domaine du domaine d’approbation tente de localiser des contrôleurs de domaine dans le domaine approuvé qui se trouvent dans le même site tout d’abord, puis la restauration automatique pour les localisateurs génériques.

    -   Pour plus d’informations sur le fonctionne de DCLocator, consultez [trouver un contrôleur de domaine dans le Site le plus proche](https://technet.microsoft.com/library/cc978016.aspx).

    -   Faire converger les noms de sites entre les domaines approuvés et approbations afin de refléter le contrôleur de domaine dans le même emplacement. Vérifiez le sous-réseau et l’adresse IP mappages sont correctement liés à des sites dans les deux forêts. Pour plus d’informations, consultez [domaine localisateur sur une approbation de forêt](http://blogs.technet.com/b/askds/archive/2008/09/24/domain-locator-across-a-forest-trust.aspx).

    -   Vérifiez que les ports sont ouverts, en fonction des besoins de DCLocator, pour l’emplacement de contrôleur de domaine. S’il existe des pare-feu entre les domaines, vérifiez que les pare-feux sont correctement configurés pour toutes les approbations. Si les pare-feu ne sont pas ouverts, le contrôleur de domaine autorisé à approuver sera toujours essayer d’accéder au domaine approuvé. Si la communication échoue pour une raison quelconque, le contrôleur de domaine autorisé à approuver expire finalement la demande au contrôleur de domaine approuvé. Toutefois, ces délais d’expiration peut prendre plusieurs secondes par requête et peut épuiser les ports réseau sur le contrôleur de domaine autorisé à approuver si le volume de demandes entrantes est élevé. Le client peut rencontrer des attentes au délai d’attente au niveau du contrôleur de domaine comme des threads bloqués, ce qui peut signifier applications bloquées (si l’application s’exécute à la demande dans le thread de premier plan). Pour plus d’informations, consultez [comment configurer un pare-feu pour les domaines et approbations](https://support.microsoft.com/kb/179442).

    -   Utilisez DnsAvoidRegisterRecords pour éliminer les contrôleurs de domaine mal l’exécution ou à latence élevée, telles que celles dans les sites de satellite, de la publicité pour les localisateurs génériques. Pour plus d’informations, consultez [comment optimiser l’emplacement d’un contrôleur de domaine ou d’un catalogue global qui réside en dehors d’un site du client](https://support.microsoft.com/kb/306602).

        > [!NOTE]
        > Il existe une limite pratique d’environ 50 au nombre de contrôleurs de domaine que le client peut consommer. Ces valeurs doivent être le plus de capacité optimale de site et la plus élevée des contrôleurs de domaine.

    
    -  Envisagez de placer les contrôleurs de domaine à partir de domaines approuvés et approbations dans le même emplacement physique.

Pour tous les scénarios de confiance, les informations d’identification sont acheminées en fonction du domaine spécifié dans les demandes d’authentification. Cela vaut également pour les requêtes aux LookupAccountName et LsaLookupNames (ainsi que d’autres, elles sont simplement les plus couramment utilisés) API. Lorsque les paramètres de domaine de ces API sont transmis à une valeur NULL, le contrôleur de domaine tente de trouver le nom du compte spécifié dans chaque domaine de confiance.

-   Désactiver la vérification de toutes les approbations disponibles lorsque le domaine de la valeur NULL est spécifiée. [Comment faire pour restreindre la recherche de noms isolés dans des domaines approuvés externes à l’aide de l’entrée de Registre LsaLookupRestrictIsolatedNameLevel](https://support.microsoft.com/kb/818024)

-   Désactiver en passant des demandes d’authentification avec le domaine NULL spécifiée pour toutes les approbations disponibles. [Le processus Lsass.exe peut cesser de répondre si vous avez plusieurs approbations externes sur un contrôleur de domaine Active Directory](https://support.microsoft.com/kb/923241/EN-US)

## <a name="see-also"></a>Voir aussi
- [Les serveurs Active Directory de réglage des performances](index.md)
- [Considérations matérielles](hardware-considerations.md)
- [Considérations relatives au protocole LDAP](ldap-considerations.md)
- [Résoudre les problèmes de performances d’AD DS](troubleshoot.md) 
- [Planification de la capacité pour les services de domaine Active Directory](https://go.microsoft.com/fwlink/?LinkId=324566)