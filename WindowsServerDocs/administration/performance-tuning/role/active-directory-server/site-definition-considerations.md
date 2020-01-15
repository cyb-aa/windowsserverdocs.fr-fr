---
title: Définition de site et placement de contrôleur de domaine dans ajoute l’optimisation des performances
description: Définition de site et considérations relatives au placement des contrôleurs de domaine dans Active Directory le réglage des performances.
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 66c6f94f1f3fee924ba0d9a3bfa0c712d62bb095
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75947100"
---
# <a name="proper-placement-of-domain-controllers-and-site-considerations"></a>Positionnement approprié des contrôleurs de domaine et des considérations sur les sites

La définition de site appropriée est essentielle pour les performances. Les clients qui se trouvent hors site peuvent présenter des performances médiocres pour les authentifications et les requêtes. En outre, avec l’introduction de IPv6 sur les clients, la requête peut provenir de l’adresse IPv4 ou IPv6, et Active Directory doit disposer de sites correctement définis pour IPv6. Le système d’exploitation préfère IPv6 à IPv4 lorsque les deux sont configurés.

À partir de Windows Server 2008, le contrôleur de domaine tente d’utiliser la résolution de noms pour effectuer une recherche inversée afin de déterminer le site dans lequel le client doit se trouver. Cela peut entraîner l’épuisement du pool de threads ATQ et provoquer le blocage du contrôleur de domaine. Pour résoudre ce problème, il convient de définir correctement la topologie de site pour IPv6. En guise de solution de contournement, vous pouvez optimiser l’infrastructure de résolution de noms pour répondre rapidement aux demandes de contrôleur de domaine. Pour plus d’informations, consultez la page [réponse différée du contrôleur de domaine Windows server 2008 ou Windows server 2008 R2 aux demandes LDAP ou Kerberos](https://support.microsoft.com/kb/2668820).

La recherche de contrôleurs de domaine en lecture/écriture pour les scénarios où les RODC sont en cours d’utilisation est un domaine supplémentaire à prendre en compte.  Certaines opérations requièrent l’accès à un contrôleur de domaine accessible en écriture ou ciblent un contrôleur de domaine accessible en écriture lorsqu’un contrôleur de domaine en lecture seule suffit.  L’optimisation de ces scénarios prendrait deux chemins d’accès :
-   Contacter les contrôleurs de domaine accessibles en écriture lorsqu’un contrôleur de domaine en lecture seule suffit.  Cela nécessite une modification du code de l’application.
-   Lorsqu’un contrôleur de domaine accessible en écriture peut être nécessaire.  Placez les contrôleurs de domaine en lecture-écriture dans des emplacements centraux pour réduire la latence.

Pour plus d’informations, consultez :
-   [Compatibilité des applications avec RODC](https://technet.microsoft.com/library/cc772597.aspx)
-   [Interface de service Active Directory (ADSI) et contrôleur de domaine en lecture seule (RODC) : éviter les problèmes de performances](https://blogs.technet.microsoft.com/fieldcoding/2012/06/24/active-directory-service-interface-adsi-and-the-read-only-domain-controller-rodc-avoiding-performance-issues/)

## <a name="optimize-for-referrals"></a>Optimiser pour les références

Les références sont la manière dont les requêtes LDAP sont redirigées lorsque le contrôleur de domaine n’héberge pas une copie de la partition interrogée. Lorsqu’une référence est retournée, elle contient le nom unique de la partition, un nom DNS et un numéro de port. Le client utilise ces informations pour continuer la requête sur un serveur qui héberge la partition. Il s’agit d’un scénario du localisateur, et toutes les recommandations relatives aux définitions de site et au placement du contrôleur de domaine sont conservées, mais les applications qui dépendent des références sont souvent négligées. Il est recommandé de s’assurer que la topologie Active Directory, notamment les définitions de site et le placement du contrôleur de domaine, reflète correctement les besoins du client. Il peut également s’agir d’avoir des contrôleurs de domaine de plusieurs domaines dans un même site, de paramétrer des paramètres DNS ou de déplacer le site d’une application.

## <a name="optimization-considerations-for-trusts"></a>Considérations relatives à l’optimisation pour les approbations

Dans un scénario intra-forêts, les approbations sont traitées en fonction de la hiérarchie de domaine suivante : domaine racine-enfant&gt; domaine racine&gt; domaine racine de la forêt-&gt; domaine enfant&gt; domaine enfant. Cela signifie que les canaux sécurisés au niveau de la racine de la forêt, et de chaque parent, peuvent être surchargés en raison de l’agrégation des demandes d’authentification qui transitent par les contrôleurs de l’utilisateur dans la hiérarchie d’approbation. Cela peut également entraîner des retards dans les répertoires actifs de la grande dispersion géographique lorsque l’authentification doit également transiter des liaisons très latentes pour affecter le processus ci-dessus. Des surcharges peuvent se produire dans des scénarios d’approbation de niveau inter-forêts et de niveau supérieur. Les recommandations suivantes s’appliquent à tous les scénarios :

-   Réglez correctement les MaxConcurrentAPI pour prendre en charge la charge sur le canal sécurisé. Pour plus d’informations, consultez [Comment effectuer le réglage des performances pour l’authentification NTLM à l’aide du paramètre MaxConcurrentApi](https://support.microsoft.com/kb/2688798/EN-US).

-   Créez des approbations raccourcies selon les besoins en fonction de la charge.

-   Assurez-vous que chaque contrôleur de domaine dans le domaine est en mesure d’effectuer la résolution de noms et de communiquer avec les contrôleurs de domaine du domaine approuvé.

-   Vérifiez que les considérations relatives à la localité sont prises en compte pour les approbations.

-   Activez Kerberos dans la mesure du possible et réduisez l’utilisation du canal sécurisé pour réduire le risque d’exécution dans des goulots d’étranglement MaxConcurrentAPI.

Les scénarios d’approbation inter-domaines sont une zone qui a été constamment un point délicat pour de nombreux clients. La résolution de noms et les problèmes de connectivité, souvent dus aux pare-feu, provoquent l’épuisement des ressources sur le contrôleur de domaine d’approbation et affectent tous les clients. En outre, un scénario souvent négligé est l’optimisation de l’accès aux contrôleurs de domaine approuvés. Les domaines clés pour garantir le bon fonctionnement sont les suivants :

-   Vérifiez que la résolution de noms DNS et WINS utilisée par les contrôleurs de domaine d’approbation peut résoudre une liste exacte de contrôleurs de domaine pour le domaine approuvé.

    -   Les enregistrements ajoutés de manière statique ont tendance à devenir obsolètes et à réintroduire des problèmes de connectivité au fil du temps. Les transferts DNS, le DNS dynamique et la fusion des infrastructures WINS/DNS sont plus gérables à long terme.

    -   Assurez la configuration appropriée des redirecteurs, des transferts conditionnels et des copies secondaires pour les zones de recherche directe et inversée pour chaque ressource de l’environnement auquel un client peut avoir besoin d’accéder. Là encore, cela nécessite une maintenance manuelle et a tendance à devenir obsolète. La consolidation des infrastructures est idéale.

-   Les contrôleurs de domaine du domaine d’approbation tentent de localiser d’abord les contrôleurs de domaine du domaine approuvé qui se trouvent dans le même site, puis d’effectuer une restauration automatique vers les localisateurs génériques.

    -   Pour plus d’informations sur le fonctionnement de du localisateur, consultez [recherche d’un contrôleur de domaine sur le site le plus proche](https://technet.microsoft.com/library/cc978016.aspx).

    -   Convergez les noms de site entre les domaines de confiance et d’approbation pour refléter le contrôleur de domaine dans le même emplacement. Vérifiez que les mappages d’adresses IP et de sous-réseau sont correctement liés aux sites dans les deux forêts. Pour plus d’informations, consultez [localisateur de domaine dans une approbation de forêt](https://blogs.technet.com/b/askds/archive/2008/09/24/domain-locator-across-a-forest-trust.aspx).

    -   Vérifiez que les ports sont ouverts, en fonction des besoins du localisateur, pour l’emplacement du contrôleur de domaine. S’il existe des pare-feu entre les domaines, assurez-vous que les pare-feu sont correctement configurés pour toutes les approbations. Si les pare-feu ne sont pas ouverts, le contrôleur de domaine d’approbation tente toujours d’accéder au domaine approuvé. Si la communication échoue pour une raison quelconque, le contrôleur de domaine d’approbation finit par expirer la requête auprès du contrôleur de domaine approuvé. Toutefois, ces délais peuvent durer plusieurs secondes par demande et peuvent épuiser les ports réseau sur le contrôleur de domaine autorisé à approuver si le volume de demandes entrantes est élevé. Le client peut rencontrer les attentes de délai d’attente sur le contrôleur de domaine en tant que threads bloqués, ce qui peut se traduire par des applications bloquées (si l’application exécute la demande dans le thread de premier plan). Pour plus d’informations, consultez [Comment configurer un pare-feu pour les domaines et les approbations](https://support.microsoft.com/kb/179442).

    -   Utilisez DnsAvoidRegisterRecords pour éliminer les contrôleurs de domaine à haut niveau de performances ou à latence élevée, tels que ceux des sites satellites, de la publicité aux localisateurs génériques. Pour plus d’informations, consultez [Comment optimiser l’emplacement d’un contrôleur de domaine ou d’un catalogue global qui réside en dehors du site d’un client](https://support.microsoft.com/kb/306602).

        > [!NOTE]
        > Il existe une limite pratique d’environ 50 pour le nombre de contrôleurs de domaine que le client peut consommer. Il doit s’agir des contrôleurs de domaine les plus performants et les plus performants.

    
    -  Envisagez de placer les contrôleurs de domaine à partir de domaines approuvés et approuvés dans le même emplacement physique.

Pour tous les scénarios d’approbation, les informations d’identification sont routées en fonction du domaine spécifié dans les demandes d’authentification. Cela est également vrai pour les requêtes à LookupAccountName et LsaLookupNames (ainsi que d’autres, il s’agit simplement des API les plus couramment utilisées). Lorsque la valeur NULL est passée aux paramètres de domaine pour ces API, le contrôleur de domaine tente de trouver le nom de compte spécifié dans chaque domaine approuvé disponible.

-   Désactive la vérification de toutes les approbations disponibles lorsque le domaine NULL est spécifié. [Comment limiter la recherche de noms isolés dans des domaines approuvés externes à l’aide de l’entrée de Registre LsaLookupRestrictIsolatedNameLevel](https://support.microsoft.com/kb/818024)

-   Désactivez le passage des demandes d’authentification avec un domaine NULL spécifié dans toutes les approbations disponibles. [Le processus Lsass. exe peut cesser de répondre si vous avez de nombreuses approbations externes sur un contrôleur de domaine Active Directory](https://support.microsoft.com/kb/923241/EN-US)

## <a name="see-also"></a>Articles associés
- [Réglage des performances Active Directory serveurs](index.md)
- [Considérations matérielles](hardware-considerations.md)
- [Considérations relatives au protocole LDAP](ldap-considerations.md)
- [Résoudre les problèmes de performances d’AD DS](troubleshoot.md) 
- [Planification de la capacité pour les services de domaine Active Directory](https://go.microsoft.com/fwlink/?LinkId=324566)