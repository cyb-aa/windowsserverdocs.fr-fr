---
ms.assetid: 6f50476c-a1f1-48fb-999b-76c4c3816496
title: Planification des compromis
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7cb87e6b9d1ace15050dcbff8b3c6d864c2a18f0
ms.sourcegitcommit: f2e98f8b7828730b83a3cc8f0e33d4e4db1b16e1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/02/2018
---
# Planification des compromis

>S’applique à: WindowsServer2016, WindowsServer2012R2, WindowsServer2012

*Loi tout d’abord, personne ne pense que rien incorrecte peut se produire à ces jusqu'à ce que c’est le cas.* - [10 lois immuables de l’Administration de la sécurité](https://technet.microsoft.com/library/cc722488.aspx)  
  
Plans de reprise dans de nombreuses organisations se concentrer sur la restauration d’urgence régionaux ou défaillances entraînent la perte de services informatiques. Toutefois, lorsque vous travaillez avec les clients compromis, nous généralement trouver que la restauration d’un compromis voulu est absent dans leurs plans de reprise. Ceci est particulièrement vrai lorsque l’endommagement résultats de vol de propriété intellectuelle ou destruction volontaire qui se base sur les limites logiques (par exemple, de la destruction de tous les domaines Active Directory ou de tous les serveurs) plutôt que des limites physiques (tel que destruction d’un centre de données). Même si une organisation peut avoir des plans de réponse aux incidents définissant les activités initiales à prendre lorsqu’un problème est détecté, ces offres omettez souvent les étapes pour récupérer un compromis qui affecte toute l’infrastructure informatique.  
  
Étant donné que Active Directory fournit des riches fonctionnalités de gestion des identités et des accès pour les utilisateurs, les serveurs, les postes de travail et les applications, il est toujours ciblé par les pirates. Si un intrus accède hautement privilégié à un domaine Active Directory ou un contrôleur de domaine, par access peuvent être exploitées pour accéder, contrôler ou même destroy la forêt Active Directory.  
  
Ce document a examiné les attaques les plus courantes contre Windows et Active Directory et contre-mesures vous pouvez mettre en œuvre pour réduire votre surface d’attaque, mais uniquement que permet de récupérer en cas de problème complète d’Active Directory doit être prêt pour le compromis avant ce problème survient. Cette section concentre sur les détails d’implémentation techniques que les sections précédentes de ce document, et plus de recommandations de haut niveau que vous pouvez utiliser pour créer une approche holistique complète pour sécuriser et gérer votre organisation le critique entreprise et les ressources informatiques.  
  
Si votre infrastructure a jamais été attaque, a résista tenté violations, ou a succombé aux attaques et été entièrement compromis, vous devez prévoir la réalité inévitable que vous être attaque indéfiniment. Il n’est pas possible empêcher les attaques, mais il est bien possible empêcher les violations des règles significatives ou gros compromis. Toutes les organisations doivent attentivement évaluer leurs programmes de gestion des risques existants et apportez les modifications nécessaires afin de réduire leur niveau global faille d’en effectuant les investissements équilibrées dans prévention, détection, imbrication et récupération.  
  
Pour créer défense efficace tout en conservant les services aux utilisateurs et aux activités qui dépendent de votre infrastructure et vos applications, vous ont besoin à prendre en considération les nouvelles méthodes pour éviter, détecter et contiennent compromis dans votre environnement et puis récupérer le compromis. Les approches et les recommandations de ce document ne peuvent pas vous aider à réparer une installation Active Directory compromise, mais peuvent vous aider à sécuriser votre celui suivante.  
  
Recommandations pour la récupération d’une forêt Active Directory sont présentées dans [Windows Server 2012: planification de la récupération de forêt Active Directory](https://www.microsoft.com/download/details.aspx?id=16506). Vous pourrez peut-être empêcher votre nouvel environnement d’être compromis complètement, mais même si vous ne pouvez pas, vous aurez outils pour récupérer et reprendre le contrôle de votre environnement.  
  
## Repenser l’approche  
*Loi chiffre huit: la difficulté de défense d’un réseau est directement proportionnelle à sa complexité.* - [10 lois immuables de l’Administration de la sécurité](https://technet.microsoft.com/library/cc722488.aspx)  
  
Il est généralement bien acceptée que si un intrus a obtenu système, racine, accès administrateur ou équivalent à un ordinateur, quel que soit le système d’exploitation, cet ordinateur peut plus être considérés comme fiable, quel que soit le nombre des efforts sont apportées à «nettoyer» le système. Active Directory n’est pas différente. Si un utilisateur malveillant a obtenu dotés de privilèges d’accès à un contrôleur de domaine ou un compte hautement privilégié dans Active Directory, à moins d’avoir un enregistrement de chaque modification rend le pirate ou d’une sauvegarde correcte, vous pouvez restaurer jamais le répertoire à un complètement état digne de confiance.  
  
Lorsqu’un serveur membre ou un poste de travail est compromis et modifiée par les pirates, l’ordinateur n’est plus digne de confiance, mais voisines serveurs sans compromis et arecompromise postes de travail d’un ordinateur n’implique pas que tous les ordinateurs sont compromis.  
  
Toutefois, dans un domaine Active Directory, tous les superflus hébergent copies de la même base de données AD DS. Si un contrôleur de domaine est compromis et un intrus modifie la base de données AD DS, ces modifications répliquent sur chaque contrôleur de domaine dans le domaine et, selon la partition dans lequel les modifications sont apportées, la forêt. Même si vous réinstallez chaque contrôleur de domaine dans la forêt, vous réinstallez simplement les hôtes sur lequel se trouve la base de données AD DS. Modifications malveillantes à Active Directory réplique vers domaine nouvellement installés facilement qu’ils seront répliquées risquez exécutant ans.  
  
Pour évaluer les environnements compromis, nous retrouver fréquemment que ce qui a été susceptibles d’être le premier violation «événement» a été réellement déclenchée après semaines, mois, voire même des années après que intrus avaient compromis initiale de l’environnement. Les pirates obtenu généralement les informations d’identification pour les comptes hautement privilégiés long avant une violation a été détectée, et ils exploitées ces comptes pour compromettre le répertoire, domaine, les serveurs membres, postes de travail et même connecté non Windows systèmes.  
  
Ces résultats sont cohérentes avec plusieurs conclusions dans 2012 violation Investigations rapport de Verizon de données, qui indique que:  
  
-   98 pour cent des violations de données provient d’agents extérieurs  
  
-   85 % des violations de données a eu semaines ou plus à découvrir  
  
-   92 % des incidents ont été découvertes par un tiers, et  
  
-   97 pour cent de violations des règles ont été évitable bien que simple ou intermédiaire contrôles.  
  
Un compromis au degré décrit plus haut est efficacement irréparable et conseils standard de «fusionner et reconstruire» chaque système compromis n’est simplement pas réalisable ou même possible si Active Directory a été déchiffré ou destruction. En cours de restauration à un état correct connu ne supprime pas les défauts autorisés l’environnement à être compromis en premier lieu.  
  
Bien que vous devez protéger chaque facette de votre infrastructure, il suffit de trouver suffisamment défauts dans votre système de défense pour accéder à l’objectif souhaité. Si votre environnement est relativement simple et initial et traditionnellement bien gérée, puis les recommandations fournies précédemment dans ce document peut être une proposition simple.  
  
Toutefois, nous avons constaté que les plus anciens, plus grands et plus complexes l’environnement, plus il est probable que les recommandations de ce document sera impossibles ou même impossible à mettre en œuvre. Il est beaucoup plus difficile à sécuriser une infrastructure après le fait qu’il y a mettre à jour et pour créer un environnement résistant aux attaques et compromettre. Mais comme indiqué précédemment, il n’est aucune petite entreprise recréer un ensemble de la forêt Active Directory. Pour ces raisons, nous vous recommandons de plus axée, approche ciblée pour sécuriser vos forêts Active Directory.  
  
Au lieu de se concentrer sur et essayez de corriger toutes les opérations sont «rompus», envisagez d’une approche dans lequel vous classer par priorité en fonction de ce qui est plus important pour votre entreprise et de votre infrastructure. Au lieu d’essayer de corriger un environnement renseigné à partir des applications et systèmes obsolètes, incorrectement configurés, envisagez de créer un nouvel environnement petit et sécurisé dans lequel vous pouvez porter en toute sécurité les utilisateurs, systèmes et informations les plus importants de votre entreprise.  
  
Dans cette section, nous décrivent une approche par lequel vous pouvez créer une forêt AD DS initial qui sert de «bateau vie» ou «cellule sécurisé» pour votre infrastructure de base. Une forêt vierge est simplement une nouvellement installée forêt d’Active Directory qui est généralement limitée taille et la portée et qui est basé en utilisant les systèmes d’exploitation en cours, les applications et avec les principes décrits dans [réduisant l’attaque Active Directory Surface](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md).  
  
En mettant en œuvre les paramètres de configuration recommandée dans une forêt nouvellement créée, vous pouvez créer une installation AD DS intégré d’emblée avec des paramètres de sécurité et les pratiques, et vous pouvez encore réduire les défis qui accompagnent les systèmes de prise en charge et applications. Tandis que les instructions détaillées pour la création et l’exécution d’une installation AD DS initial sont en dehors de l’étendue de ce document, vous devez suivre quelques principes généraux et les instructions pour créer une «cellule sécurisée» dans laquelle vous pouvez héberger votre critique éléments. Ces instructions sont les suivantes:  
  
1.  Identifier les principes pour séparer et sécuriser les ressources critiques.  
  
2.  Définir un plan de migration limitée, en fonction du risque.  
  
3.  Exploiter les migrations «nonmigratory» le cas échéant.  
  
4.  Mettre en œuvre «destruction creative».  
  
5.  Isoler les applications et les systèmes existants.  
  
6.  Simplification de sécurité pour les utilisateurs finaux.  
  
### Identification des principes pour séparer et sécuriser les ressources critiques  

Les caractéristiques de l’environnement initial que vous créez à vos ressources critiques maison peuvent varier considérablement. Par exemple, vous pouvez choisir de créer une forêt vierge dans laquelle vous migrez uniquement les utilisateurs VIP et des données sensibles que seuls les utilisateurs peuvent accéder. Vous pouvez créer une forêt vierge dans laquelle vous migrez VIP non seulement les utilisateurs, mais que vous implémenter comme une forêt d’administration, mise en œuvre les principes décrits dans la [surface d’attaque Active Directory](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md) de créer des comptes administratifs sécurisés et héberge pouvant être utilisées pour gérer vos forêts héritées à partir de la forêt vierge. Vous pouvez implémenter une forêt «spécialisée» qui héberge les comptes VIP, les comptes dotés de privilèges et systèmes nécessitant une sécurité supplémentaire tels que les serveurs exécutant les Services de certificat Active Directory (AD SC) dans le but exclusif de séparer les de moins sécurisé forêts. Enfin, vous pourriez implémenter une forêt vierge devient l’emplacement de fait pour tous les nouveaux utilisateurs, systèmes, applications et des données, ce qui vous permet de finalement désaffecter votre forêt héritée via usure.  
  
Quel que soit votre forêt vierge contient quelques utilisateurs et systèmes ou elle constitue la base pour une migration plus agressif, vous devez suivre ces principes dans votre planification:  
  
1.  Part du principe que vos forêts héritées ont été déchiffrés.  
  
2.  Ne pas configurer un environnement initial pour faire confiance à une forêt héritée, bien que vous pouvez configurer un environnement hérité pour faire confiance à une forêt vierge.  
  
3.  Ne migrent pas les comptes d’utilisateurs ou des groupes à partir d’une forêt héritée vers un environnement vierge s’il existe une possibilité qu’appartenances du compte, historique de l’identificateur de sécurité ou d’autres attributs aient été à des fins malveillantes modifiées. À la place, utilisez approches «nonmigratory» pour remplir une forêt vierge. (Nonmigratory approches sont décrites plus loin dans cette section.)  
  
4.  Ne migrent pas ordinateurs à partir de forêts héritées aux forêts vierge. Mettre en œuvre des serveurs nouvellement installés dans la forêt vierge, installer des applications sur les serveurs nouvellement installés et migrer les données d’application pour les systèmes nouvellement installés. Pour les serveurs de fichier, copiez les données vers les serveurs nouvellement installés et définir des utilisateurs à l’aide des utilisateurs et groupes dans la nouvelle forêt puis créer des serveurs d’impression de la même manière.  
  
5.  Ne pas autoriser l’installation de systèmes d’exploitation ou des applications de la forêt vierge. Si une application ne peut pas être mis à jour et nouvellement installée, laissez ce champ dans la forêt héritée et en considération creative destruction à remplacer les fonctionnalités de l’application.  
  
### Définir un Plan de Migration limitée, en fonction du risque  
Création d’une limité, plan de migration en fonction du risque simplement signifie que lorsque vous décidez des utilisateurs, des applications et données migrer vers votre forêt vierge, vous devez identifier cibles migration selon le degré de risque auquel votre organisation est exposée si une des les utilisateurs ou les systèmes est compromis. Utilisateurs VIP dont les comptes sont susceptibles d’être ciblées par les pirates doivent être stockés dans la forêt vierge. Les applications qui fournissent des fonctions critiques doivent être installées sur des serveurs nouvellement créés dans la forêt vierge, et des données sensibles doivent être déplacées vers les serveurs de la forêt vierge sécurisé.  
  
Si vous n’avez pas déjà une image effacer des utilisateurs stratégiques, systèmes, des applications et des données dans votre environnement Active Directory, travailler avec divisions pour les identifier. N’importe quelle application requise pour les entreprises à employer doit être identifiée, ainsi que tous les serveurs sur lequel exécuteront des applications critiques ou de stockage des données critiques. En identifiant les utilisateurs et les ressources nécessaires pour votre organisation de continuer à fonctionner, vous créez une collection naturellement hiérarchisée du bien sur lequel afin de cibler vos efforts.  
  
### Exploiter les Migrations «Nonmigratory»  
Si vous savez que votre environnement a été déchiffré, suspect qu’il a été déchiffré, ou simplement vous préférez ne pas faire migrer les données héritées et des objets à partir d’une installation d’Active Directory héritée vers un nouvel, envisagez les approches de migration qui ne pas techniquement objets «migrer».  
  
### Comptes d’utilisateurs  
Dans une migration traditionnelle d’Active Directory d’une forêt à l’autre, l’attribut SIDHistory (historique de l’identificateur de sécurité) sur les objets utilisateur est utilisé pour stocker l’identificateur de sécurité des utilisateurs ainsi que ceux de groupes que les utilisateurs étaient membres dans la forêt héritée. Si les comptes d’utilisateurs sont migrés vers une nouvelle forêt et accéder à des ressources de la forêt héritée, les identificateurs de sécurité dans l’historique de l’identificateur de sécurité sont utilisés pour créer un jeton d’accès qui permet aux utilisateurs d’accéder aux ressources auxquelles ils avaient accès avant que les comptes ont été déplacées.  
  
La conservation de l’historique d’identificateur de sécurité, s’est toutefois, révélé problématique dans certains environnements, car les jetons d’accès des utilisateurs avec les identificateurs de sécurité et l’historique de remplissage peut entraîner une jeton augmentation. Gonflement jeton est un problème dans lequel le nombre d’identificateurs de sécurité qui doivent être stockés dans un jeton d’accès utilisateur utilise ou dépasse la quantité d’espace disponible dans le jeton.  
  
Bien que la taille des jetons peut être augmentée dans une certaine mesure, la solution parfaite à gonflement jeton est pour réduire le nombre d’identificateurs de sécurité associées aux comptes d’utilisateur, que ce soit par rationaliser les membres du groupe, éliminer historique de l’identificateur de sécurité, ou une combinaison des deux. Pour plus d’informations sur les jeton gonflement, voir [MaxTokenSize et dilatation jeton Kerberos](http://blogs.technet.com/b/shanecothran/archive/2010/07/16/maxtokensize-and-kerberos-token-bloat.aspx).  
  
Plutôt que de migration des utilisateurs à partir d’un environnement hérité (en particulier un dans lequel les appartenances et identificateur de sécurité historiques peuvent être compromis) en utilisant l’historique de l’identificateur de sécurité, pensez à tirer parti des applications méta-annuaire pour «migrer» des utilisateurs, sans historiques identificateur de sécurité de stockage dans la nouvelle forêt. Lorsque les comptes d’utilisateurs sont créés dans la nouvelle forêt, vous pouvez utiliser une application méta-annuaire pour établir une correspondance entre les comptes leurs comptes correspondantes dans la forêt héritée.  
  
Pour fournir le nouvel utilisateur comptes l’accès aux ressources de la forêt héritée, vous pouvez utiliser les outils méta-annuaire pour identifier les groupes de ressources dans lequel les anciens comptes des utilisateurs ont accès et ajoutez nouveaux comptes d’utilisateurs à ces groupes. Selon votre stratégie de groupe dans la forêt héritée, vous devrez peut-être créer des groupes de domaine local pour accéder aux ressources ou convertir les groupes existants à des groupes de domaine local pour autoriser les nouveaux comptes à ajouter aux groupes de ressources. En concentration tout d’abord sur les applications les plus critiques et les données et en les déplaçant vers le nouvel environnement (avec ou sans historique de l’identificateur de sécurité), vous pouvez limiter le volume de travail fiche dans l’ancien environnement.  
  

  
### Serveurs et les postes  
Dans une migration depuis un Active Directory traditionnelle forêt vers un autre, la migration ordinateurs est souvent relativement simple par rapport à la migration des utilisateurs, des groupes et des applications. En fonction du rôle de l’ordinateur, migration vers une nouvelle forêt peut être suffit disjonction un ancien domaine et participer à une nouvelle visualisation. Toutefois, migration comptes d’ordinateur intacts dans un annule forêt vierge l’objectif de création d’un environnement novateur. Plutôt que des comptes de migration (potentiellement compromis incorrectement configuré ordinateur ou obsolète) vers une nouvelle forêt, vous devez installer récemment serveurs et les postes de travail dans le nouvel environnement. Vous pouvez migrer les données à partir des systèmes de la forêt héritée vers des systèmes de la forêt vierge, mais pas les systèmes que les données de la maison.  
  
### Applications  

Applications peuvent présenter le défi plus significatif dans une migration à partir d’une forêt à une autre, mais dans le cas d’une migration «nonmigratory», un des principes de base, vous devez appliquer est que les applications de la forêt vierge doivent être en cours, prise en charge et installé récemment. Données peuvent être migrées à partir d’instances de l’application de la forêt ancienne autant que possible. Dans les situations dans lesquelles une application ne peut pas être «recréée» de la forêt vierge, vous devez envisager approches comme destruction creative ou isolement d’applications héritées comme décrit dans la section suivante.  
  
### Mise en œuvre Creative Destruction  
Destruction Creative est un terme de coût qui décrit le développement économique créée par la destruction d’une commande antérieure. Dans ces dernières années, le terme ont été appliqué à l’informatique. Il signifie généralement mécanismes par l’infrastructure ancien est supprimée, ne pas par la mise à niveau, mais en le remplaçant par quelque chose de complètement nouveau. 2011 [Gartner Symposium ITXPO](http://www.gartner.com/technology/symposium/orlando/) pour les responsables et dirigeants informatiques présenté creative destruction dans l’un de ses thèmes clés pour réduire les coûts et augmente l’efficacité. Améliorations de sécurité sont possibles comme une excroissance naturelle du processus.  

Par exemple, une organisation peut être composée de plusieurs divisions qui utilisent une autre application qui effectue une fonctionnalité similaire, avec différents degrés de prise en charge modernity et fournisseur. Traditionnellement, informatique peut être responsable de la maintenance d’application de chaque division séparément et efforts de consolidation doit comporter tente d’identifier quelle application proposée la fonctionnalité de meilleures et puis la migration des données dans qui application des autres.  
  
Dans destruction creative, plutôt que de gérer les applications redondantes ou obsolètes, vous implémentez des applications tout à fait nouvelles pour remplacer l’ancien, migrer les données dans les nouvelles applications et désaffecter les anciennes applications et les systèmes sur lequel elle est exécutée. Dans certains cas, vous pouvez implémenter creative destruction des applications héritées en déployant une nouvelle application dans votre propre infrastructure, mais aussi souvent que possible, vous devez prendre en compte porter à la place de l’application à une solution sur le nuage.  
  
En déployant applications basées sur le cloud à remplacer les anciennes applications internes, vous non seulement Réduisez les efforts de maintenance et des coûts, mais vous réduisez attaque de votre organisation en supprimant les systèmes hérités et applications qui présentent des vulnérabilités les pirates pour mettre à profit. Cette approche offre une méthode rapide pour une organisation obtenir des fonctionnalités souhaitées tout en supprimant simultanément héritées cibles dans l’infrastructure. Bien que le principe de destruction creative ne s’applique pas à toutes les ressources informatiques, il propose une option permettant de supprimer les systèmes hérités et applications lors du déploiement simultanément des applications sur le nuage, sécurisées robustes souvent viable.  
  
### Isoler des Applications et systèmes existants  
Une excroissance naturelle de migration de vos utilisateurs stratégiques et les systèmes vers un environnement initial et sécurisé est que votre forêt héritée être contiendra des informations moins importantes et les systèmes. Bien que les applications restant dans l’environnement moins sécurisé et systèmes hérités peuvent présenter des risques élevés, elles représentent également une réduction gravité du compromis. En rapatriement et modernisation vos ressources critiques, vous pouvez vous concentrer sur déploiement de la gestion efficace et le contrôle lors ne pas avoir à prendre en charge les protocoles et les paramètres hérités.  
  
Lorsque vous avez migrés vos données critiques à une forêt initial, vous pouvez évaluer les options à isoler plus anciens systèmes et applications dans votre forêt AD DS «principale». Bien que vous pouvez implémenter destruction creative pour remplacer une application et les serveurs sur lequel elle s’exécute, dans les autres cas, vous pouvez envisager d’isolement supplémentaire des systèmes et des applications moins sécurisé. Par exemple, une application qui est utilisé par un petit nombre d’utilisateurs, mais qui nécessite les informations d’identification héritées comme hachage LAN Manager peuvent être migrées vers un domaine de petite taille que vous créez pour prendre en charge les systèmes pour lesquels vous n’avez aucune option de remplacement.  
  
En supprimant ces systèmes de domaines où ils forcé implémentation des paramètres hérités, vous pouvez ensuite augmenter la sécurité des domaines en configurant les pour prendre en charge uniquement les systèmes d’exploitation actuels et les applications. Toutefois, il est préférable pour retirer des systèmes hérités et applications dès que possible. Si désaffecter n'est pas possible pour une petite partie de votre population héritée, séparer dans un domaine distinct (ou une forêt) vous permet d’effectuer des améliorations incrémentielles dans le reste de l’installation héritée.  
  
### Simplification de sécurité pour les utilisateurs finaux  
Dans la plupart des organisations, les utilisateurs qui ont accès à des informations sensibles plus en raison de la nature de leur rôle dans l’organisation souvent ont le moins de temps à consacrer à la formation des contrôles et les restrictions d’accès complexes. Bien que vous devez disposer d’un programme de formation sécurité complète pour tous les utilisateurs de votre organisation, vous devez également vous concentrer sur la création de sécurité comme simple à utiliser que possible en mettant en œuvre des contrôles intégrés à transparentes et simplification principes auxquels les utilisateurs supplémentaire.  
  
Par exemple, vous pouvez définir une stratégie dans laquelle les cadres et les autres VIP est requises pour utiliser des postes de travail sécurisés pour accéder à des données sensibles et systèmes, leur permettant d’utiliser leurs autres appareils pour accéder aux données moins sensibles. Il s’agit d’un principe simple pour les utilisateurs à retenir, mais vous pouvez mettre en œuvre un certain nombre de contrôles de serveur principal pour vous aider à appliquer l’approche.  

Vous pouvez utiliser [Assurance du mécanisme d’authentification](https://technet.microsoft.com/library/dd391847(v=WS.10).aspx) pour autoriser les utilisateurs à accéder aux données sensibles uniquement s’ils ont connectés à leurs systèmes sécurisés à l’aide de leur carte à puce et que vous pouvez utiliser des restrictions de droits de IPsec et de l’utilisateur pour contrôler les systèmes à partir de laquelle ils peuvent se connecter à des référentiels de données sensibles. Vous pouvez utiliser la [Boîte à outils de Classification de données Microsoft](https://www.microsoft.com/download/details.aspx?id=27123) pour créer une infrastructure de classification de fichiers de conception robuste, et vous pouvez implémenter le [Contrôle d’accès dynamique](http://blogs.technet.com/b/windowsserver/archive/2012/05/22/introduction-to-windows-server-2012-dynamic-access-control.aspx) pour restreindre l’accès aux données en fonction de caractéristiques d’une tentative d’accès, traduction règles d’entreprise dans les contrôles techniques.  
  
Du point de vue de l’utilisateur, l’accès à des données sensibles à partir d’un système sécurisé «fonctionne, tout simplement», puis essayez d’y parvenir à partir d’un système non sécurisé «simplement ne». Toutefois, du point de vue de la surveillance et la gestion de votre environnement, vous êtes vous aider à créer des modèles d’identification personnelle dans dont les utilisateurs accèdent des données sensibles et systèmes, ce qui facilite vous permettant de détecter les tentatives d’accès anormale.  
  
Dans les environnements dans laquelle l’utilisateur résistance aux mots de passe longs et complexes a permis de stratégies de mot de passe insuffisante, en particulier pour les utilisateurs VIP, envisagez les approches de remplacement à l’authentification, soit à l’aide des cartes à puce (qui arrivent dans un certain nombre de facteurs de forme et avec des fonctionnalités supplémentaires pour renforcer l’authentification), contrôles biométriques tels que les lecteurs doigt balayez ou des données d’authentification pair sont sécurisées par approuvés plateforme module de plateforme des puces dans les ordinateurs des utilisateurs. Bien que l’authentification multifacteur n’empêche pas les attaques de vol d’informations d’identification si un ordinateur est déjà compromis, en attribuant à vos utilisateurs contrôles facile à utiliser l’authentification, vous pouvez affecter plus performantes grâce aux mots de passe pour les comptes d’utilisateurs pour lesquels traditionnel utilisateur contrôles de nom et mot de passe sont encombrant.  
