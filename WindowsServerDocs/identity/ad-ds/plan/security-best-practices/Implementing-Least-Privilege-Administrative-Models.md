---
ms.assetid: 7a7ab95c-9cb3-4a7b-985a-3fc08334cf4f
title: "Implémentation de modèles d’administration de privilèges"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9716d442446b2705dfd2803d061cb884a5e72fbe
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="implementing-least-privilege-administrative-models"></a>Implémentation de modèles d’administration de privilèges

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

L’extrait suivant provient [Guide de planification de l’administrateur de comptes de sécurité](https://technet.microsoft.com/library/cc162797.aspx), la première publication du 1eravril1999:  
  
> «Formations plus liées à la sécurité et la documentation abordent l’implémentation d’un principe des moindres privilèges encore organisations suivent rarement. Le principe est simple, et l’impact du appliquer correctement considérablement améliore la sécurité et réduit les risques. Le principe stipule que tous les utilisateurs doivent se connecter avec un compte d’utilisateur qui a les autorisations minimales nécessaires pour terminer la tâche en cours et rien de plus. Cela offre une protection contre les programmes malveillants, entre autres attaques. Il s’applique aux ordinateurs et les utilisateurs de ces ordinateurs.   
> «Ce principe fonctionne aussi bien est qu’il vous oblige à faire des recherches interne. Par exemple, vous devez déterminer les privilèges d’accès qui a un ordinateur ou un utilisateur a réellement besoin et puis de les implémenter. Pour de nombreuses organisations, cette tâche peut initialement sembler beaucoup de travail; toutefois, il est une étape essentielle pour sécuriser correctement votre environnement réseau.   
> «Vous devez accorder tous les utilisateurs d’administrateur de domaine leurs privilèges de domaine le principe du privilège minimum. Par exemple, si un administrateur ouvre une session avec un compte privilégié et exécute par inadvertance un programme antivirus, le virus avec un accès administratif à l’ordinateur local et à l’ensemble du domaine. Si l’administrateur au lieu de cela était connecté avec un compte non privilégié (non administrateur), l’étendue des dommages de virus serait uniquement l’ordinateur local, car elle s’exécute en tant qu’un utilisateur de l’ordinateur local.   
> «Dans un autre exemple, les comptes auxquels vous accordez des droits d’administrateur au niveau du domaine ne doivent pas disposer de droits élevés dans une autre forêt, même s’il existe une relation d’approbation entre les forêts. Cette méthode permet de limiter les dégâts si une personne malveillante parvient à compromettre une seule forêt gérée. Les organisations doivent vérifier régulièrement leur réseau pour vous protéger contre l’élévation non autorisée des privilèges.»  
  
L’extrait suivant est à partir de la [Kit de ressources MicrosoftWindows Security](https://www.microsoft.com/learning/en/us/book.aspx?ID=6815&locale=en-us), première 2005 publiée dans:  
  
> «Toujours pensez de sécurité en termes d’accorder le moins de privilèges nécessaires pour effectuer la tâche. Si une application qui a trop de privilèges doit être compromise, la personne malveillante peut être en mesure d’étendre l’attaque au-delà qu’elle le ferait si l’application n’a pas sous le moins de privilèges possible. Par exemple, examinez les conséquences d’un administrateur réseau involontairement ouverture d’une pièce jointe qui lance un virus. Si l’administrateur est connecté à l’aide du compte administrateur du domaine, le virus aura des privilèges d’administrateur sur tous les ordinateurs dans le domaine et par conséquent un accès illimité à presque toutes les données sur le réseau. Si l’administrateur est connecté à l’aide d’un compte d’administrateur local, le virus aura des privilèges d’administrateur sur l’ordinateur local et par conséquent pourra accéder aux données sur l’ordinateur et installer des logiciels malveillants, tels que les logiciels de journalisation de trait de clé sur l’ordinateur. Si l’administrateur est connecté à l’aide d’un compte d’utilisateur normal, le virus ont accès qu’aux données de l’administrateur et ne sera pas en mesure d’installer des logiciels malveillants. À l’aide des privilèges minimum nécessaires pour lire le message électronique, dans cet exemple, l’étendue potentielle de la compromission est considérablement réduit.»  
  
## <a name="the-privilege-problem"></a>Le problème de privilèges  
Les principes décrits dans les extraits précédentes n’ont pas changé, mais pour l’évaluation des installations d’ActiveDirectory, nous invariablement trouver un nombre excessif de comptes qui disposent de droits et autorisations bien au-delà de celles qui sont requises pour effectuer les tâches quotidiennes. La taille de l’environnement affecte les numéros des comptes privilégiés trop brutes, mais pas les répertoires proportionmidsized peuvent avoir des dizaines de comptes dans les groupes dotés de privilèges plus élevés, tandis que les installations de grande taille peuvent avoir des centaines ou milliers. À quelques exceptions près, quelle que soit la sophistication des compétences et d’arsenal, l’attaquant les pirates suivre le chemin d’accès empruntent. Ils améliorent la complexité de leurs outils et l’approche uniquement si plus simple mécanismes sont en panne ou déjouer en défenses.  
  
Malheureusement, le chemin d’accès empruntent dans de nombreux environnements s’est révélé l’utilisation excessive de comptes avec des privilèges très vaste. Privilèges étendus sont les droits et autorisations qui permettent à un compte effectuer des activités spécifiques sur une partie importante de l’environnement, par exemple, le personnel du support technique peut disposer des autorisations qui leur permettent de réinitialiser les mots de passe sur plusieurs comptes d’utilisateur.  
  
Privilèges approfondies des privilèges élevés qui sont appliqués à un segment étroit de la population, telle donnant un ingénieur droits d’administrateur sur un serveur afin qu’ils puissent effectuer des réparations. Privilèges large ni privilège approfondie est toujours dangereux, mais lorsque le nombre de comptes dans le domaine est accordée définitivement privilège très vaste, si seul un des comptes est compromis, il peut être utilisé rapidement pour reconfigurer l’environnement à des fins de la personne malveillante ou même détruire des segments de grande taille de l’infrastructure.  
  
Attaques pass-the-hash, qui sont un type d’attaque de vol d’informations d’identification, sont omniprésents car les outils pour effectuer les sont librement disponible et facile à utiliser, et de nombreux environnements sont vulnérables aux attaques. Attaques pass-the-hash, toutefois, ne sont pas le problème. L’essentiel de ce problème est double:  
  
1.  Il est généralement facile pour un intrus d’obtenir des privilèges approfondie sur un seul ordinateur, puis propagez ce privilège largement à d’autres ordinateurs.  
  
2.  Il existe généralement un trop grand nombre de comptes permanentes avec des privilèges élevés sur le paysage informatique.  
  
Même si les attaques pass-the-hash sont éliminés, les pirates utiliseriez simplement tactiques, pas une autre stratégie. Plutôt que de plantation contre les programmes malveillants qui contient le vol d’informations d’identification des outils, ils peuvent plantes contre les programmes malveillants qui enregistre les frappes de touches ou tirer parti de n’importe quel nombre d’autres approches pour capturer les informations d’identification qui sont puissantes au sein de l’environnement. Quel que soit les tactiques, les cibles restent les mêmes: comptes avec des privilèges très vaste.  
  
Octroi d’un privilège excessif n’est pas uniquement trouvé dans ActiveDirectory dans les environnements compromis. Lorsqu’une organisation a développé l’habitude d’accorder plus de privilèges que nécessaire, il se trouve généralement dans l’ensemble de l’infrastructure, comme indiqué dans les sections suivantes.  
  
## <a name="in-active-directory"></a>Dans ActiveDirectory  
Dans ActiveDirectory, il est courant de trouver que les groupes EA, DA et BA contient un nombre excessif de comptes. Le plus souvent, groupe d’une organisation EA contient les membres moins DA groupes contiennent généralement un multiplicateur du nombre d’utilisateurs dans le groupe EA et groupes d’administrateurs contiennent généralement plus de membres que les populations d’autres groupes combinés. Cela est souvent dû à une croyance administrateurs sont d’une certaine manière «moins privilégié» que DAs ou EAs. Alors que les droits et autorisations accordées à chacun de ces groupes diffèrent, ils doivent être efficacement considérés tout aussi puissants groupes car un membre d’un peut apporter identifié un membre des deux autres.  
  
## <a name="on-member-servers"></a>Sur les serveurs membres  
Lorsque nous récupérer les membres du groupe Administrateurs locaux sur les serveurs membres dans de nombreux environnements, nous avons trouvé d’appartenance comprises entre un petit nombre de comptes locaux et de domaine, et des dizaines de groupes imbriqués qui, lorsqu’elle est développée, des centaines d’affichage, même des milliers de comptes avec des privilèges d’administrateur local sur les serveurs. Dans de nombreux cas, les groupes de domaine avec des appartenances grand sont imbriqués dans groupe Administrateurs locaux des serveurs membres du, sans en considération le fait que tout utilisateur peut modifier les membres de ces groupes dans le domaine permettre prendre le contrôle d’administration de tous les systèmes sur lesquels le groupe a été imbriqué dans un groupe d’administrateurs local.  
  
## <a name="on-workstations"></a>Sur les stations de travail  
Bien que les stations de travail ont généralement beaucoup moins de membres dans leur groupe d’administrateurs locaux à faire des serveurs membres, dans de nombreux environnements, l’appartenance au groupe Administrateurs local sur leurs ordinateurs personnels sont accordées aux utilisateurs. Lorsque cela se produit, même si le compte d’utilisateur est activé, ces utilisateurs présentent un risque pour l’intégrité de leurs stations de travail avec élévation de privilèges.  
  
> [!IMPORTANT]  
> Vous devez envisager avec soin si les utilisateurs ont besoin de droits d’administration sur leurs stations de travail, et ce cas, une meilleure approche peut être pour créer un compte local distinct sur l’ordinateur qui est membre du groupe Administrateurs. Lorsque les utilisateurs requièrent une élévation, ils peuvent présenter les informations d’identification de ce compte local d’élévation, mais étant donné que le compte est local, il ne peut pas servir à compromettre les autres ordinateurs ou d’accéder aux ressources du domaine. Comme avec tous les comptes locaux, toutefois, les informations d’identification pour le compte privilégié local doivent être uniques. Si vous créez un compte local avec les mêmes informations d’identification sur plusieurs stations de travail, vous exposer les ordinateurs à des attaques pass-the-hash.  
  
## <a name="in-applications"></a>Dans les Applications  
Dans les attaques dans lequel la cible est la propriété intellectuelle d’une organisation, les comptes qui ont reçu des privilèges élevés dans les applications peuvent être ciblés pour permettre une exfiltration de données. Bien que les comptes qui ont accès aux données sensibles peuvent ont été accordées sans privilèges élevés dans le domaine ou le système d’exploitation, les comptes qui permettent de manipuler la configuration d’une application ou d’un accès aux informations de l’application fournit des risques présents.  
  
## <a name="in-data-repositories"></a>Dans les référentiels de données  
Comme c’est le cas avec d’autres cibles, des attaquants qui l’accès à la propriété intellectuelle sous la forme de documents et d’autres fichiers peuvent cibler les comptes, contrôler l’accès au fichier stocke, les comptes qui ont un accès direct aux fichiers, ou les groupes ou rôles qui ont accès aux fichiers. Par exemple, si un serveur de fichiers est utilisé pour stocker des documents de contrat, et l’accès aux documents à l’aide d’un groupe ActiveDirectory, un attaquant peut modifier l’appartenance du groupe peut ajouter des comptes compromis au groupe et accéder aux documents de contrat. Dans les cas dans lesquels un accès aux documents est fourni par les applications telles que SharePoint, les personnes malveillantes peuvent cibler les applications comme décrit précédemment.  
  
## <a name="reducing-privilege"></a>Réduction des privilèges  
La plus grande et plus complexe un environnement, plus il est difficile elle consiste à gérer et sécuriser. Dans les petites entreprises, examen et en réduisant les privilèges peuvent être une proposition relativement simple, mais chaque serveur supplémentaire, une station de travail, un compte d’utilisateur et une application en cours d’utilisation dans une organisation ajoute un autre objet qui doit être sécurisé. Dans la mesure où il peut être difficile, voire impossible correctement sécurisé tous les aspects de l’organisation votre infrastructure informatique, vous devez vous concentrer efforts tout d’abord sur les comptes dont les privilèges créer le plus grand risque, qui sont généralement intégrés privilégié des comptes et groupes dans ActiveDirectory et des comptes privilégié local sur les stations de travail et les serveurs membres.  
  
### <a name="securing-local-administrator-accounts-on-workstations-and-member-servers"></a>Sécurisation des comptes d’administrateur Local sur les stations de travail et les serveurs membres  
Bien que ce document se concentre sur la sécurisation d’ActiveDirectory, comme a été indiqué précédemment, la plupart des attaques par rapport au répertoire commence en tant que les attaques contre les ordinateurs hôtes individuels. Des instructions complètes pour la sécurisation des groupes locaux sur des systèmes membres ne peut pas être fournies, mais les recommandations suivantes peuvent être utilisées pour vous aider à sécuriser les comptes d’administrateur locales sur les stations de travail et les serveurs membres.  
  
#### <a name="securing-local-administrator-accounts"></a>Sécurisation des comptes d’administrateur Local  
Toutes les versions de Windows actuellement dans le support standard, le compte administrateur local est désactivé par défaut, ce qui rend le compte inutilisables pour pass-the-hash et autres attaques de vol d’informations d’identification. Toutefois, dans les domaines contenant des systèmes d’exploitation hérités ou les comptes d’administrateur locales ont été activées, ces comptes peuvent être utilisés comme décrit précédemment pour propager les compromis entre les stations de travail et les serveurs membres. Pour cette raison, les contrôles suivants sont recommandés pour tous les comptes d’administrateur locales sur les systèmes joints au domaine.  
  
Obtenir des instructions détaillées pour l’implémentation de ces contrôles sont fournies dans [annexe h: sécurisation des comptes administrateur locaux et groupes](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md). Avant d’implémenter ces paramètres, toutefois, assurez-vous que les comptes administrateur locaux ne sont pas actuellement utilisés dans l’environnement pour exécuter les services sur des ordinateurs ou effectuer d’autres activités dont ces comptes ne doivent pas être utilisés. Testez minutieusement ces paramètres avant de les mettre en œuvre dans un environnement de production.  
  
#### <a name="controls-for-local-administrator-accounts"></a>Contrôles pour les comptes d’administrateur Local  
Comptes d’administrateur intégrés ne doivent jamais être utilisés en tant que comptes de service sur les serveurs membres, ni ils doivent être utilisés pour vous connecter à des ordinateurs locaux (sauf en Mode sans échec, ce qui est autorisé même si le compte est désactivé). L’objectif de la mise en œuvre les paramètres décrits ici consiste à empêcher le compte d’administrateur local de chaque ordinateur ne sont pas utilisables, sauf si les contrôles de protection sont inversées tout d’abord. Par l’implémentation de ces contrôles et la surveillance des comptes d’administrateur pour les modifications, vous pouvez réduire considérablement le risque de réussite d’une attaque que comptes administrateur locaux de cibles.  
  
##### <a name="configuring-gpos-to-restrict-administrator-accounts-on-domain-joined-systems"></a>La configuration de stratégie de groupe pour limiter les comptes d’administrateur sur les systèmes joints au domaine  
Dans un ou plusieurs objets stratégie de groupe que vous créez et liez à une station de travail et le serveur membre unités d’organisation dans chaque domaine, ajoutez le compte d’administrateur pour les droits d’utilisateur suivants dans **ordinateur Configuration ordinateur\Stratégies\Paramètres Windows\Paramètres sécurité\Stratégies Settings\nom droits**:  
  
-   Refuser l’accès à cet ordinateur à partir du réseau  
  
-   Refuse la connexion en tant que tâche  
  
-   Refuse la connexion en tant que service  
  
-   Refuse la connexion par le biais des Services Bureau à distance  
  
Lorsque vous ajoutez des comptes d’administrateur à ces droits d’utilisateur, spécifiez si vous ajoutez le compte administrateur local ou l’administrateur du domaine de compte à la façon que vous nommez le compte. Par exemple, pour ajouter le compte d’administrateur du domaine NWTRADERS à ces refuser des droits, vous devez taper le compte en tant que **NWTRADERS\Administrateur**, ou accédez au compte administrateur du domaine NWTRADERS. Pour vous assurer que vous limitez le compte administrateur local, tapez **administrateur** dans ces paramètres dans l’éditeur d’objets de stratégie de groupe des droits utilisateur.  
  
> [!NOTE]  
> Même si les comptes d’administrateur locales sont renommés, les stratégies seront appliqueront.  
  
Ces paramètres garantit que compte d’administrateur d’un ordinateur ne peut pas être utilisé pour se connecter à d’autres ordinateurs, même si elle est à des fins malveillantes ou par inadvertance activée. Ouvertures de session locale à l’aide du compte administrateur local ne peut pas être complètement désactivée, ni vous devez essayer pour ce faire, car le compte d’administrateur local d’un ordinateur est conçu pour être utilisé dans les scénarios de récupération d’urgence.  
  
Doit un serveur membre ou une station de travail deviennent disjoint du domaine avec aucun autre comptes locaux disposent des privilèges d’administrateur, l’ordinateur peut être démarré en mode sans échec, le compte d’administrateur peut être activé et le compte peut ensuite être utilisé pour effectuer des réparations sur l’ordinateur. Lorsque les réparations effectuées, le compte d’administrateur doit être désactivé à nouveau.  
  
### <a name="securing-local-privileged-accounts-and-groups-in-active-directory"></a>Sécurisation des comptes privilégiés locaux et groupes dans ActiveDirectory  
*La loi numéro Six: un ordinateur ne peut être aussi sécurisé que l’administrateur est digne de confiance.* - [Dix immuables de sécurité (Version2.0)](https://technet.microsoft.com/security/hh278941.aspx)  
  
Les informations fournies ici sont destinées à donner des instructions générales pour la sécurisation des groupes dans ActiveDirectory et des comptes intégrés privilège plus élevés. Obtenir des instructions pas à pas détaillées sont également fournies dans [annexe d: sécurisation des comptes d’administrateur intégrés dans ActiveDirectory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md), [annexe e: sécurisation de l’entreprise groupes d’administrateurs dans ActiveDirectory](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md), [annexe f: sécurisation des administrateurs de groupes de domaine dans ActiveDirectory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)et dans [annexe g: sécurisation des groupes d’administrateurs dans ActiveDirectory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md).  
  
Avant d’implémenter ces paramètres, vous devez également tester tous les paramètres de manière approfondie pour déterminer si elles sont appropriées pour votre environnement. Les entreprises pourront implémenter ces paramètres.  
  
#### <a name="securing-built-in-administrator-accounts-in-active-directory"></a>Sécurisation des comptes d’administrateur intégrés dans ActiveDirectory  
Dans chaque domaine dans Active Directory, un compte d’administrateur est créé dans le cadre de la création du domaine. Ce compte est par défaut un membre des groupes d’administrateur dans le domaine et administrateurs du domaine, et si le domaine est le domaine racine de forêt, le compte est également un membre du groupe Administrateurs de l’entreprise. Utilisation du compte d’administrateur local d’un domaine doit être réservée uniquement pour les activités de génération initiale et, éventuellement, des scénarios de reprise après sinistre. Pour garantir qu’un compte administrateur intégré peut être utilisé pour effectuer des réparations dans le cas où aucun autre compte ne peut être utilisé, vous ne devez pas modifier l’appartenance par défaut du compte d’administrateur dans n’importe quel domaine de la forêt. Au lieu de cela, vous devez suivant les recommandations pour contribuer à sécuriser le compte d’administrateur dans chaque domaine dans la forêt. Obtenir des instructions détaillées pour l’implémentation de ces contrôles sont fournies dans [annexe d: sécurisation des comptes d’administrateur intégrés dans ActiveDirectory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
#### <a name="controls-for-built-in-administrator-accounts"></a>Contrôles pour les comptes d’administrateur intégré  
L’objectif de la mise en œuvre les paramètres décrits ici est d’empêcher le compte d’administrateur de chaque domaine (et non un groupe) ne sont pas utilisables, sauf si un certain nombre de contrôles est inversé. En implémentant ces contrôles et les comptes d’administrateur pour les modifications d’analyse, vous pouvez réduire considérablement la probabilité d’une attaque réussie en tirant parti de compte d’administrateur d’un domaine. Pour le compte administrateur dans chaque domaine dans votre forêt, vous devez configurer les paramètres suivants.  
  
##### <a name="enable-the-account-is-sensitive-and-cannot-be-delegated-flag-on-the-account"></a>Activer l’indicateur «Compte est sensible et ne peut pas être délégué» sur le compte  
Par défaut, tous les comptes dans ActiveDirectory peuvent être déléguées. La délégation permet à un ordinateur ou un service pour présenter les informations d’identification d’un compte qui est authentifié auprès de l’ordinateur ou à d’autres ordinateurs pour obtenir des services au nom du compte. Lorsque vous activez le **compte est sensible et ne peut pas être délégué** attribut sur un compte de domaine, les informations d’identification du compte ne peuvent être présentées à d’autres ordinateurs ou les services sur le réseau, ce qui limite les attaques qui exploitent de délégation pour utiliser les informations d’identification du compte sur d’autres systèmes.  
  
##### <a name="enable-the-smart-card-is-required-for-interactive-logon-flag-on-the-account"></a>Activer l’indicateur «carte à puce est requise pour l’ouverture de session interactive» sur le compte  
Lorsque vous activez le **carte à puce est requise pour l’ouverture de session interactive** attribut sur un compte, Windows réinitialise le mot de passe du compte sur une valeur aléatoire de 120caractères. En définissant cet indicateur sur des comptes d’administrateur intégrés, vous vérifiez que le mot de passe pour le compte est non seulement longs et complexes, mais n’est pas connu à n’importe quel utilisateur. Il n’est pas techniquement nécessaire de créer des cartes à puce pour les comptes avant l’activation de cet attribut, mais si possible, les cartes à puce doit être créés pour chaque compte d’administrateur avant de configurer les restrictions de compte et les cartes à puce doivent être stockés dans des emplacements sécurisés.  
  
Bien que la configuration du **carte à puce est requise pour l’ouverture de session interactive** indicateur réinitialise le mot de passe du compte, il n’empêche pas un utilisateur avec des droits pour réinitialiser le mot de passe du compte à partir de la définition du compte à une valeur connue et à l’aide de nom du compte et le nouveau mot de passe pour accéder aux ressources sur le réseau. Pour cette raison, vous devez implémenter les contrôles supplémentaires suivants sur le compte.  
  
##### <a name="disable-the-account"></a>Désactiver le compte  
Si le compte d’administrateur n’est pas déjà désactivé, désactivez-la lorsque vous avez terminé la configuration des propriétés du compte. Cela empêche le compte utilisé dans n’importe quel but, sauf si elle est d’abord activé. Dans un scénario de récupération d’urgence dans lequel aucun compte n’est disponible pour effectuer des réparations de l’environnement ADDS, vous pouvez démarrer un contrôleur de domaine en mode sans échec, une session localement avec le compte administrateur intégré (qui n’est jamais bloqué à partir de l’ouverture de session locale) et activer le compte d’administrateur du domaine, si nécessaire.  
  
##### <a name="configuring-gpos-to-restrict-domains-administrator-accounts-on-domain-joined-systems"></a>La configuration de stratégie de groupe pour limiter les comptes d’administrateur de domaines sur les systèmes joints au domaine  
Bien que la désactivation du compte administrateur dans un domaine définit le compte est inutilisable efficacement, vous devez implémenter des restrictions supplémentaires sur le compte dans le cas où le compte est activé par inadvertance ou à des fins malveillantes. Bien que ces contrôles peuvent être inversées en fin de compte par le compte d’administrateur, l’objectif est de créer des contrôles de progression de la personne malveillante de ralentir et limite les dommages le compte peut causer des dommages.  
  
Dans un ou plusieurs objets stratégie de groupe que vous créez et liez à une station de travail et le serveur membre unités d’organisation dans chaque domaine, ajouter le compte d’administrateur de chaque domaine pour les droits d’utilisateur suivants dans **ordinateur Configuration ordinateur\Stratégies\Paramètres Windows\Paramètres sécurité\Stratégies Settings\nom droits**:  
  
-   Refuser l’accès à cet ordinateur à partir du réseau  
  
-   Refuse la connexion en tant que tâche  
  
-   Refuse la connexion en tant que service  
  
-   Refuse la connexion par le biais des Services Bureau à distance  
  
> [!NOTE]  
> Lorsque vous ajoutez des comptes d’administrateur locales à ce paramètre, vous devez spécifier si vous configurez les comptes administrateur locaux ou les comptes d’administrateur de domaine. Par exemple, pour ajouter le compte d’administrateur local du domaine NWTRADERS à ces refuser des droits, vous devez taper le compte en tant que **NWTRADERS\Administrateur**, ou accédez au compte d’administrateur local pour le domaine NWTRADERS. Si vous tapez **administrateur** dans ces paramètres de droits d’utilisateur dans l’éditeur d’objets de stratégie de groupe, vous allez restreindre le compte administrateur local sur chaque ordinateur sur lequel l’objet de stratégie de groupe est appliquée.  
>   
> Nous vous recommandons de restreindre les comptes d’administrateur locales sur les serveurs membres et stations de travail dans la même manière que les comptes administrateur de domaine. Par conséquent, vous devez ajouter généralement le compte d’administrateur pour chaque domaine dans la forêt et le compte d’administrateur pour les ordinateurs locaux à ces paramètres de droits d’utilisateur. La capture d’écran suivante montre un exemple de configuration de ces droits d’utilisateur à bloquer les comptes d’administrateur locales et le compte d’administrateur d’un domaine d’effectuer des ouvertures de session qui ne doit pas être nécessaire pour ces comptes.  

>   
> ![Modèles d’administration privilège minimum](media/Implementing-Least-Privilege-Administrative-Models/SAD_20.gif)  
  
##### <a name="configuring-gpos-to-restrict-administrator-accounts-on-domain-controllers"></a>La configuration de stratégie de groupe pour limiter les comptes d’administrateur sur les contrôleurs de domaine  
Dans chaque domaine dans la forêt, la stratégie des contrôleurs de domaine par défaut ou une stratégie liée à l’unité d’organisation des contrôleurs de domaine doit être modifiée pour ajouter le compte d’administrateur de chaque domaine pour les droits d’utilisateur suivants dans **ordinateur Configuration ordinateur\Stratégies\Paramètres Windows\Paramètres sécurité\Stratégies Settings\nom droits**:  
  
-   Refuser l’accès à cet ordinateur à partir du réseau  
  
-   Refuse la connexion en tant que tâche  
  
-   Refuse la connexion en tant que service  
  
-   Refuse la connexion par le biais des Services Bureau à distance  
  
> [!NOTE]  
> Ces paramètres garantit que le compte administrateur local ne peut pas être utilisé pour se connecter à un contrôleur de domaine, même si le compte, s’il est activé, peut ouvrir une session localement aux contrôleurs de domaine. Étant donné que ce compte seulement doit être activé et utilisé dans les scénarios de récupération d’urgence, il est prévu que l’accès physique au moins un contrôleur de domaine sera disponible, ou que les autres comptes avec des autorisations d’accès à distance des contrôleurs de domaine peuvent être utilisés.  
  
##### <a name="configure-auditing-of-built-in-administrator-accounts"></a>Configurer l’audit des comptes d’administrateur intégrés  
Lorsque vous avez sécurisé du compte d’administrateur de chaque domaine et désactivée, vous devez configurer l’audit pour surveiller les modifications apportées au compte. Si le compte est activé, son mot de passe est réinitialisé ou toutes les autres modifications sont apportées au compte, les alertes doivent être envoyées aux utilisateurs ou équipes responsables de l’administration des services ADDS, en plus des équipes de réponse aux incidents de votre organisation.  
  
### <a name="securing-administrators-domain-admins-and-enterprise-admins-groups"></a>Sécurisation des administrateurs, Admins du domaine et les groupes Administrateurs de l’entreprise  
  
#### <a name="securing-enterprise-admin-groups"></a>Sécurisation des groupes administrateurs d’entreprise  
Le groupe Administrateurs de l’entreprise, qui est hébergé dans le domaine racine de forêt, ne doit contenir aucun utilisateur sur une base quotidienne, à l’exception du compte d’administrateur local du domaine, il est sécurisé comme décrit précédemment et dans [annexe d: sécurisation des comptes d’administrateur intégrés dans ActiveDirectory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
Quand l’accès EA est requis, les utilisateurs dont les comptes nécessitent des autorisations et droits EA doivent être placés temporairement dans le groupe Administrateurs de l’entreprise. Bien que les utilisateurs utilisent les comptes dotés de privilèges élevés, leurs activités doivent être vérifiées et effectuées de préférence avec un utilisateur qui effectue les modifications et un autre utilisateur en observant les modifications pour réduire la probabilité d’une mauvaise utilisation accidentelle ou une configuration incorrecte. Lorsque les activités ont été effectuées, les comptes doivent être supprimés du groupe EA. Cela peut être obtenue via les procédures manuelles à et documenté des processus, les logiciels de gestion (PIM/PAM) identité/accès privilégié tiers ou une combinaison des deux. Instructions pour créer des comptes qui peut être utilisé pour contrôler l’appartenance des groupes privilégiés dans ActiveDirectory sont fournies dans [attrayantes comptes au vol d’informations d’identification](../../../ad-ds/plan/security-best-practices/Attractive-Accounts-for-Credential-Theft.md) et obtenir des instructions détaillées sont fournies dans [annexe i: création de gestion des comptes pour des comptes protégés et les groupes dans ActiveDirectory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
Administrateurs de l’entreprise sont, par défaut, les membres du groupe Administrateurs intégré dans chaque domaine dans la forêt. Suppression du groupe d’administrateurs de l’entreprise dans les groupes d’administrateurs dans chaque domaine d’est une modification inappropriée, car en cas d’un scénario de reprise après sinistre forêt, droits EA sera probablement requises. Si le groupe Administrateurs de l’entreprise a été supprimé à partir de groupes d’administrateurs dans une forêt, il doit être ajouté au groupe Administrateurs dans chaque domaine, et les contrôles supplémentaires suivants doivent être implémentées:  
  
-   Comme indiqué précédemment, le groupe Administrateurs de l’entreprise ne doit contenir aucun utilisateur sur une base quotidienne, à l’exception du compte administrateur du domaine racine de forêt, qui doit être sécurisé, comme décrit dans [annexe d: sécurisation des comptes d’administrateur intégrés dans ActiveDirectory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
-   Dans la stratégie de groupe liés aux unités d’organisation contenant des serveurs membres et les stations de travail dans chaque domaine, le groupe EA doit être ajouté aux droits d’utilisateur suivants:  
  
    -   Refuser l’accès à cet ordinateur à partir du réseau  
  
    -   Refuse la connexion en tant que tâche  
  
    -   Refuse la connexion en tant que service  
  
    -   Refuse la connexion locale  
  
    -   Refuse la connexion par le biais des Services Bureau à distance.  
  
Cela empêchera les membres du groupe EA à partir de la connexion aux stations de travail et les serveurs membres. Si les serveurs de renvoi sont utilisés pour administrer des contrôleurs de domaine et ActiveDirectory, assurez-vous que les serveurs de renvoi sont trouvent dans une unité d’organisation à laquelle les objets de stratégie restrictives ne sont pas liés.  
  
-   L’audit doit être configuré pour envoyer des alertes si des modifications sont apportées aux propriétés ou l’appartenance au groupe EA. Ces alertes doivent être envoyées, au minimum, à des utilisateurs ou équipes responsables de la réponse d’incidents et d’administration ActiveDirectory. Vous devez également définir des processus et procédures pour temporairement remplir le groupe EA, y compris les procédures de notification lors de la population légitime du groupe est effectuée.  
  
#### <a name="securing-domain-admins-groups"></a>Sécurisation des groupes d’administrateurs de domaine  
Comme c’est le cas avec le groupe Administrateurs de l’entreprise, l’appartenance aux groupes Admins du domaine doit être requis uniquement dans les scénarios de build ou la récupération d’urgence. Il ne doit être aucun compte d’utilisateur quotidiennes dans le groupe DA à l’exception du compte administrateur local pour le domaine, si elle a été sécurisée comme décrit dans [annexe d: sécurisation des comptes d’administrateur intégrés dans ActiveDirectory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
Quand l’accès DA est requis, les comptes d’avoir besoin de ce niveau d’accès doivent être placés temporairement dans le groupe DA pour le domaine en question. Bien que les utilisateurs utilisent les comptes dotés de privilèges élevés, les activités doivent être vérifiées et effectuées de préférence avec un utilisateur qui effectue les modifications et un autre utilisateur en observant les modifications pour réduire la probabilité d’une mauvaise utilisation accidentelle ou une configuration incorrecte. Lorsque les activités ont été effectuées, les comptes doivent être retirés du groupe Admins du domaine. Cela peut être obtenue via les procédures manuelles et processus documentés, via le logiciel de gestion (PIM/PAM) tiers identité/accès privilégié, ou une combinaison des deux. Instructions pour créer des comptes qui peut être utilisé pour contrôler l’appartenance des groupes privilégiés dans ActiveDirectory sont fournies dans [annexe i: création de gestion des comptes pour des comptes protégés et les groupes dans ActiveDirectory](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
Admins du domaine est, par défaut, les membres du groupe Administrateurs local sur tous les serveurs membres et les stations de travail de leurs domaines respectifs. Cette imbrication par défaut ne doit pas être modifiée, car cela affecte les options de récupération d’urgence et de prise en charge. Si les groupes Admins du domaine ont été supprimées à partir du groupe Administrateurs locaux sur les serveurs membres, ils doivent être ajoutés au groupe Administrateurs sur chaque serveur membre et la station de travail dans le domaine par le biais des paramètres de groupe restreint dans les objets GPO. Les contrôles générales suivantes, lesquelles sont décrites en détail dans [annexe f: sécurisation des administrateurs de groupes de domaine dans ActiveDirectory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md) doit également être implémenté.  
  
Pour le groupe Admins du domaine dans chaque domaine dans la forêt:  
  
1.  Supprimez tous les membres du groupe DA, à l’exception du compte administrateur intégré pour le domaine, à condition qu’il a été sécurisé comme décrit dans [annexe d: sécurisation des comptes d’administrateur intégrés dans ActiveDirectory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
2.  Dans la stratégie de groupe liés aux unités d’organisation contenant des serveurs membres et les stations de travail dans chaque domaine, le groupe DA doit être ajouté aux droits d’utilisateur suivants:  
  
    -   Refuser l’accès à cet ordinateur à partir du réseau  
  
    -   Refuse la connexion en tant que tâche  
  
    -   Refuse la connexion en tant que service  
  
    -   Refuse la connexion locale  
  
    -   Refuse la connexion par le biais des Services Bureau à distance  
  
    Cela empêchera les membres du groupe DA à partir de la connexion aux stations de travail et les serveurs membres. Si les serveurs de renvoi sont utilisés pour administrer des contrôleurs de domaine et ActiveDirectory, assurez-vous que les serveurs de renvoi sont trouvent dans une unité d’organisation à laquelle les objets de stratégie restrictives ne sont pas liés.  
  
3.  L’audit doit être configuré pour envoyer des alertes si des modifications sont apportées aux propriétés ou l’appartenance au groupe DA. Ces alertes doivent être envoyées, au minimum, à des utilisateurs ou équipes responsables de réponse d’incidents et d’administration de domaine ActiveDirectory. Vous devez également définir des processus et procédures pour temporairement remplir le groupe DA, y compris les procédures de notification lors de la population légitime du groupe est effectuée.  
  
#### <a name="securing-administrators-groups-in-active-directory"></a>Sécurisation des groupes d’administrateurs dans ActiveDirectory  
Comme c’est le cas avec les groupes EA et DA, appartenance au groupe Administrateurs (BA) doit être requis uniquement dans les scénarios de build ou la récupération d’urgence. Il ne doit être aucun compte d’utilisateur quotidiennes dans le groupe administrateurs à l’exception du compte administrateur local pour le domaine, si elle a été sécurisée comme décrit dans [annexe d: sécurisation des comptes d’administrateur intégrés dans ActiveDirectory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
Lorsque les administrateurs accès est requis, les comptes d’avoir besoin de ce niveau d’accès doivent être placés temporairement dans le groupe Administrateurs du domaine en question. Bien que les utilisateurs utilisent les comptes dotés de privilèges élevés, activités doivent être vérifiées et, de préférence, effectuées avec un utilisateur qui effectue les modifications et un autre utilisateur en observant les modifications pour réduire la probabilité d’une mauvaise utilisation accidentelle ou une configuration incorrecte. Lorsque les activités ont été effectuées, les comptes doivent être immédiatement supprimés du groupe Administrateurs. Cela peut être obtenue via les procédures manuelles et processus documentés, via le logiciel de gestion (PIM/PAM) tiers identité/accès privilégié, ou une combinaison des deux.  
  
Les administrateurs sont, par défaut, les propriétaires de la plupart des objets de domaine Active Directory dans leurs domaines respectifs. L’appartenance à ce groupe peut-être être nécessaires dans les scénarios de récupération build et de reprise après sinistre dans lequel la propriété ou la possibilité de prendre possession d’objets est nécessaire. En outre, DAs et EAs héritent un nombre de leurs droits et autorisations appartenance par défaut dans le groupe Administrateurs. Par défaut groupe imbrication de groupes privilégiés dans ActiveDirectory ne doivent pas être modifiés et groupe d’administrateurs de chaque domaine doit être sécurisé, comme décrit dans [annexe g: sécurisation des groupes d’administrateurs dans ActiveDirectory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)et dans les instructions générales ci-dessous.  
  
1.  Supprimez tous les membres du groupe Administrateurs, à l’exception du compte d’administrateur local pour le domaine, à condition qu’il a été sécurisé comme décrit dans [annexe d: sécurisation des comptes d’administrateur intégrés dans ActiveDirectory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
2.  Les membres du groupe d’administrateurs du domaine ne doivent jamais besoin pour vous connecter à des serveurs membres ou les stations de travail. Dans un ou plusieurs objets stratégie de groupe lié à une station de travail et le serveur membre unités d’organisation dans chaque domaine, du groupe Administrateurs doit être ajouté aux droits d’utilisateur suivants:  
  
    -   Refuser l’accès à cet ordinateur à partir du réseau  
  
    -   Refuse la connexion en tant que tâche,  
  
    -   Refuse la connexion en tant que service  
  
    -   Cela empêche les membres du groupe Administrateurs utilisé pour se connecter ou se connecter à des serveurs membres ou les stations de travail (sauf si plusieurs contrôles sont tout d’abord violation), où leurs informations d’identification peuvent être mis en cache et ainsi de compromis. Un compte privilégié ne doit jamais être utilisé pour ouvrir une session sur un système de moins de privilèges, et en appliquant ces contrôles offre une protection contre un nombre d’attaques.  
  
3.  À des droits de l’unité d’organisation dans chaque domaine dans la forêt, du groupe Administrateurs doit être accordée à l’utilisateur suivant des contrôleurs de domaine (s’ils n’ont pas déjà de ces droits), qui autorise les membres du groupe Administrateurs pour effectuer les fonctions nécessaires pour un scénario de récupération d’urgence forêt:  
  
    -   Accéder à cet ordinateur à partir du réseau  
  
    -   Autoriser  
  
    -   Permettre le par le biais des Services Bureau à distance  
  
4.  L’audit doit être configuré pour envoyer des alertes si des modifications sont apportées aux propriétés ou l’appartenance au groupe Administrateurs. Ces alertes doivent être envoyées, au minimum, aux membres de l’équipe chargée de l’administration des services ADDS. Les alertes doivent également être envoyées aux membres de l’équipe de sécurité et procédures doivent être définis pour la modification de l’appartenance du groupe Administrateurs. Plus précisément, ces processus doivent inclure une procédure par laquelle l’équipe de sécurité est averti du groupe Administrateurs va être modifié de sorte que lorsque les alertes sont envoyées, qu’elles sont tenues et une alarme n’est pas déclenchée. En outre, les processus pour informer l’équipe de sécurité lors de l’utilisation du groupe Administrateurs est terminée et les comptes utilisés ont été supprimés du groupe doivent être implémentés.  
  
> [!NOTE]  
> Lorsque vous implémentez des restrictions sur le groupe Administrateurs dans la stratégie de groupe, Windows applique les paramètres aux membres du groupe Administrateurs local de l’ordinateur en plus du groupe d’administrateurs du domaine. Par conséquent, vous soyez prudent lors de l’implémentation des restrictions sur le groupe Administrateurs. Bien que l’interdiction des ouvertures de session réseau, de traitement par lots et de service pour les membres du groupe Administrateurs est informé partout où il est possible d’implémenter, ne limitez pas les ouvertures de session locale ou les ouvertures de session par le biais des Services Bureau à distance. Blocage de ces types d’ouverture de session peut bloquer l’administration légitime d’un ordinateur par les membres du groupe Administrateurs local. La capture d’écran suivante montre les paramètres de configuration qui bloquent la mauvaise utilisation d’intégré local et les comptes d’administrateur de domaine, en plus de l’utilisation abusive du groupe Administrateurs intégré au domaine ou local. Notez que le **refuse la connexion par le biais des Services Bureau à distance** droit d’utilisateur n’inclut pas le groupe Administrateurs, dans la mesure où inclus dans ce paramètre est également bloquer les ouvertures de session pour les comptes qui sont membres du groupe Administrateurs de l’ordinateur local. Si les services sur les ordinateurs sont configurés pour s’exécuter dans le contexte d’un des groupes privilégiés décrites dans cette section, mise en œuvre de ces paramètres peut entraîner des services et applications en échec. Par conséquent, avec toutes les recommandations de cette section, vous devez tester les paramètres de mise en application dans votre environnement.  

>   
> ![Modèles d’administration privilège minimum](media/Implementing-Least-Privilege-Administrative-Models/SAD_3.gif)  
  
### <a name="role-based-access-controls-rbac-for-active-directory"></a>Contrôles d’accès basé sur un rôle (RBAC) pour ActiveDirectory  
En règle générale, les contrôles d’accès basé sur un rôle (RBAC) sont un mécanisme de regroupement d’utilisateurs et de fournir un accès aux ressources en fonction des règles d’entreprise. Dans le cas d’ActiveDirectory, l’implémentation de RBAC pour les services ADDS est le processus de création de rôles auquel les droits et autorisations sont déléguées pour permettre aux membres du rôle d’effectuer des tâches d’administration quotidiennes sans leur accordant un privilège excessif. RBAC pour ActiveDirectory peut être conçue et implémentée par le biais des outils natifs et d’interfaces, en tirant profit des logiciels que vous possède déjà, en achetant des produits tiers, ou n’importe quelle combinaison de ces approches. Cette section ne fournit pas d’instructions pas à pas pour mettre en œuvre de RBAC pour ActiveDirectory, mais décrit à la place des facteurs, que vous devez envisager de choisir une approche à l’implémentation de RBAC dans vos installations ADDS.  
  
#### <a name="native-approaches-to-rbac-for-active-directory"></a>Approches natifs de RBAC pour ActiveDirectory  
Dans l’implémentation de RBAC plus simple, vous pouvez implémenter des rôles en tant que les groupes ADDS et déléguer les droits et autorisations pour les groupes de les autoriser à effectuer l’administration quotidienne dans l’étendue désigné du rôle.  
  
Dans certains cas, les groupes de sécurité existant dans ActiveDirectory peuvent être utilisés pour accorder des droits et autorisations appropriées pour une fonction. Par exemple, si certains employés de votre organisation informatique sont responsables de la gestion et maintenance des zones DNS et des enregistrements, délégation de ces responsabilités peut être aussi simple que la création d’un compte pour chaque administrateur DNS, puis ajoutez-le au groupe Administrateurs DNS dans ActiveDirectory. Le groupe Administrateurs DNS, contrairement aux groupes dotés de privilèges plus élevés, a peu de droits puissant sur ActiveDirectory, bien que les membres de ce groupe ont été délégué des autorisations qui leur permettent d’administrer le DNS.  
  
Dans d’autres cas, vous devrez peut-être créer des groupes de sécurité et de déléguer les droits et autorisations aux objets ActiveDirectory, les objets de système de fichiers et les objets de Registre pour permettre aux membres des groupes pour effectuer des tâches d’administration désignés. Par exemple, si vos opérateurs de support technique sont responsables de la réinitialisation des mots de passe oubliés, aider les utilisateurs avec des problèmes de connectivité et le dépannage de paramètres d’application, vous devrez peut-être combiner les paramètres de délégation sur des objets utilisateur dans ActiveDirectory avec des privilèges qui permettent aux utilisateurs du support technique pour se connecter à distance aux ordinateurs des utilisateurs pour afficher ou modifier les paramètres de configuration des utilisateurs. Pour chaque rôle que vous définissez, vous devez identifier:  
  
1.  Les membres de tâches du rôle d’effectuer sur une base journalière et les tâches sont exécutées moins fréquemment.  
  
2.  Sur quels systèmes et dans les applications membres d’un rôle doivent être droits et autorisations accordés.  
  
3.  Les utilisateurs qui doivent bénéficier de l’appartenance à un rôle.  
  
4.  La gestion des appartenances de rôle sera effectuée.  
  
Dans de nombreux environnements, la création manuelle des contrôles d’accès basé sur les rôles pour l’administration d’un environnement ActiveDirectory peut être difficile pour implémenter et gérer. Si vous avez clairement défini des rôles et des responsabilités pour l’administration de votre infrastructure informatique, vous voudrez peut-être tirer parti des outils supplémentaires pour vous aider à créer un déploiement de RBAC natif facile à gérer. Par exemple, si Forefront Identity Manager (FIM) est en cours d’utilisation dans votre environnement, vous pouvez utiliser FIM pour automatiser la création et le remplissage des rôles d’administration, qui peuvent faciliter l’administration en cours. Si vous utilisez SystemCenter ConfigurationManager (SCCM) et SystemCenter Operations Manager (SCOM), vous pouvez utiliser des rôles spécifiques à l’application pour déléguer la gestion et les fonctions de surveillance et également appliquer une configuration cohérente et audit sur les systèmes dans le domaine. Si vous avez implémenté une infrastructure à clé publique (PKI), vous pouvez émettre et nécessitent des cartes à puce pour le personnel informatique chargé d’administrer l’environnement. Avec la gestion des informations d’identification FIM (FIM CM), vous pouvez même combiner la gestion des rôles et des informations d’identification pour le personnel d’administration.  
  
Dans d’autres cas, il peut être préférable d’une organisation à prendre en compte le déploiement de logiciels de RBAC tiers qui fournit les fonctionnalités de «out-of-box». Les solutions (COTS) standard de RBAC pour ActiveDirectory, Windows et non Windows répertoires et les systèmes d’exploitation sont offertes par un certain nombre de fournisseurs. Lors du choix entre les solutions natives et des produits tiers, tenez compte des facteurs suivants:  
  
1.  Budget: En investir dans du développement de RBAC à l’aide de logiciels et les outils que vous possédez peut-être déjà, vous pouvez réduire les coûts de logiciels impliqués dans le déploiement d’une solution. Toutefois, si vous n’avez personnel ayant une expérience de création et déploiement de solutions de RBAC natives, vous devrez peut-être d’engager des ressources conseils pour développer votre solution. Vous devez soigneusement peser les coûts prévus pour une solution personnalisées avec les coûts de déploiement d’une solution «out-of-box», en particulier si votre budget est limité.  
  
2.  Composition de l’environnement informatique: Si votre environnement se compose principalement des systèmes Windows, ou si vous êtes déjà tirant parti d’ActiveDirectory pour la gestion des comptes et des systèmes autres que Windows, des solutions personnalisées natives peuvent fournir la solution optimale à vos besoins. Si votre infrastructure contient de nombreux systèmes qui n’exécutent pas Windows et ne sont pas gérés par ActiveDirectory, vous devrez peut-être envisager d’options pour la gestion des systèmes autres que Windows séparément à partir de l’environnement ActiveDirectory.  
  
3.  Modèle de privilèges dans la solution: si un produit s’appuie sur la sélection élective de ses comptes de service dans des groupes ActiveDirectory disposant de privilèges élevés et sont offre pas les options qui ne nécessitent pas un privilège excessif être accordée au logiciel de RBAC, vous n’avez pas vraiment réduit votre ActiveDirectory attaque surfaceyou avez modifié uniquement la composition des groupes plus privilégiés dans le répertoire. Sauf si un fournisseur de l’application peut fournir des contrôles pour les comptes de service qui réduisent la probabilité que les comptes d’être compromise et utilisé à des fins malveillantes, vous voudrez peut-être envisager d’autres options.  

  
### <a name="privileged-identity-management"></a>Privileged Identity Management  
Privilégié de gestion des identités (PIM), parfois appelée en tant que compte privilégié (PAM) de gestion ou d’informations d’identification privilégié (PCM) est la conception, la construction, et implémentation des approches pour la gestion des privilégié des comptes dans votre infrastructure. En règle générale, PIM fournit des mécanismes par lequel les comptes bénéficient de droits temporaires et les autorisations requises pour effectuer le saut build ou résoudre les fonctions, plutôt que de laisser des privilèges définitivement associés aux comptes. Si les fonctionnalités PIM sont créée manuellement ou sont mis en œuvre le déploiement d’un logiciel tiers un ou plusieurs des fonctionnalités suivantes sont disponible:  
  

-   «Coffres», où des mots de passe pour les comptes privilégiés sont «extrait» et affectés un mot de passe initial, puis «archivés» quand les activités ont été effectuées, à quel moment les mots de passe sont réinitialisés à nouveau sur les comptes des informations d’identification.  
  
-   Restrictions de temps sur l’utilisation des informations d’identification privilégiées  
  
-   Informations d’identification de l’usage unique  
  
-   Générés par le flux de travail de l’octroi de privilèges avec analyse et rapports d’activités effectuées et la suppression automatique des privilèges lorsque les activités sont terminées ou période a expiré  
  
-   Remplacement de codés en dur les informations d’identification telles que les noms d’utilisateur et mots de passe dans les scripts avec les interfaces de programmation d’application (API) qui autorisent les informations d’identification doivent être récupérés à partir des coffres-forts en fonction des besoins  
  
-   Gestion automatique des informations d’identification du compte de service  
  
### <a name="creating-unprivileged-accounts-to-manage-privileged-accounts"></a>Création de comptes non privilégié pour gérer les comptes privilégiés  
Les défis liés à la gestion des comptes privilégiés est que, par défaut, les comptes qui peuvent gérer les comptes privilégiés et protégés et groupes sont privilégiés et des comptes protégés. Si vous implémentez des solutions de RBAC et PIM appropriées pour votre installation d’Active Directory, les solutions peuvent comprendre des approches qui vous permettent effectivement devez vider l’appartenance des groupes plus privilégiés dans le répertoire, remplir les groupes que temporairement les besoins.  
  
Toutefois, si vous implémentez de RBAC natif et PIM, vous devez envisager la création de comptes d’aucun privilège et avec la fonction de remplissage et le vidage des privilèges uniquement des groupes dans Active Directory si nécessaire. [AnnexeI Des comptes d’administration de la création des comptes protégés et des groupes dans Active Directory](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md) fournit des instructions pas à pas que vous pouvez utiliser pour créer des comptes à cet effet.  
  
### <a name="implementing-robust-authentication-controls"></a>L’implémentation de contrôles d’authentification fiable  
*La loi numéro Six: il y a vraiment quelqu'un hors il tente de deviner vos mots de passe.* - [10lois immuables de l’Administration de la sécurité](https://technet.microsoft.com/library/cc722488.aspx)  
  
Pass-the-hash et autres attaques de vol d’informations d’identification ne sont pas spécifiques aux systèmes d’exploitation Windows, ils ne sont pas nouveau. La première attaque pass-the-hash a été créée en 1997. Historiquement, toutefois, ces attaques requis des outils personnalisés, ont été hit-or-miss dans leur réussite et requises des personnes malveillantes d’avoir un degré relativement élevé de compétence. L’introduction des outils disponible gratuitement, faciles à utiliser qui extrait des informations d’identification en mode natif a entraîné une augmentation exponentielle du nombre et le succès des attaques de vol d’informations d’identification de ces dernières années. Toutefois, les attaques de vol d’informations d’identification sont en aucun cas les mécanismes uniquement par lequel les informations d’identification sont ciblées et compromises.  
  
Bien que vous devez implémenter des contrôles pour vous protéger contre les attaques de vol d’informations d’identification, vous devez également identifier les comptes dans votre environnement qui sont plus susceptibles d’être ciblés par les personnes malveillantes et implémenter des contrôles d’authentification robustes pour ces comptes. Si vos comptes privilégiés plus sont à l’aide de l’authentification unique facteur tels que les noms d’utilisateur et mots de passe (les deux sont «quelque chose que vous connaissez,» qui est un facteur d’authentification), ces comptes sont protégés faiblement. Tout ce qui a besoin d’une personne malveillante est le nom d’utilisateur et du mot de passe associé au compte et les attaques pass-the-hash ne sont pas requiredthe attaquant peut authentifier l’utilisateur pour les systèmes qui acceptent les informations d’identification d’un facteur.  
  
Bien que l’implémentation de l’authentification multifacteur ne vous protège pas contre les attaques pass-the-hash, en combinaison avec les systèmes protégés peuvent l’implémentation de l’authentification multifacteur. Vous trouverez plus d’informations sur l’implémentation de systèmes protégés dans [implémentation de sécuriser les ordinateurs hôtes d’administration](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md), et les options d’authentification sont décrites dans les sections suivantes.  
  
#### <a name="general-authentication-controls"></a>Contrôles d’authentification général  
Si vous n’avez pas déjà implémenté l’authentification multifacteur telles que des cartes à puce, effectuez cette opération. Cartes à puce implémentent une protection matérielle des clés privées dans une paire de clés publique-privée, empêchant la clé privée d’un utilisateur à partir d’accès ou utilisée à moins que l’utilisateur présente le code confidentiel correct, code secret ou identificateur biométrique à la carte à puce. Même si le code confidentiel ou le mot de passe d’un utilisateur est intercepté par un enregistreur de frappe sur un ordinateur compromis, pour une personne malveillante de réutiliser le code confidentiel ou mot de passe, la carte doit également être physiquement présente.  
  
Dans les cas dans lesquels les mots de passe longs et complexes se sont avérés difficiles à implémenter en raison de la résistance des utilisateurs, les cartes à puce fournissent un mécanisme par lequel les utilisateurs peuvent implémenter relativement simples codes confidentiels ou secrets sans les informations d’identification soient exposées à des attaques en force ou des tables arc-en-ciel. Codes confidentiels de carte à puce ne sont pas stockés dans Active Directory ou dans les bases de données SAM locales, même si les hachages d’informations d’identification peuvent encore être stockés dans la mémoire LSASS protégés sur des ordinateurs sur lesquels les cartes à puce ont été utilisés pour l’authentification.  
  
#### <a name="additional-controls-for-vip-accounts"></a>Contrôles supplémentaires pour les comptes d’adresse IP virtuelle  
Un autre avantage de l’implémentation des cartes à puce ou autres mécanismes d’authentification par certificat est la possibilité de tirer parti de l’Assurance du mécanisme d’authentification pour protéger les données sensibles qui est accessibles aux utilisateurs de l’adresse IP virtuelle. L’Assurance du mécanisme d’authentification est disponible dans les domaines dans lesquels le niveau fonctionnel est défini à Windows Server 2012 ou Windows Server 2008 R2. Lorsqu’elle est activée, l’Assurance du mécanisme d’authentification ajoute un appartenance au groupe global désignés par l’administrateur à un jeton d’utilisateur Kerberos lorsque les informations d’identification de l’utilisateur sont authentifiées au cours de l’ouverture de session à l’aide d’une méthode basée sur le certificat d’ouverture de session.  
  
Cela permet aux administrateurs de ressources contrôler l’accès aux ressources, telles que des fichiers, dossiers et des imprimantes, selon si l’utilisateur se connecte à l’aide d’une méthode d’ouverture de session basées sur certificat, outre le type de certificat utilisé. Par exemple, lorsqu’un utilisateur ouvre une session à l’aide d’une carte à puce, l’accès aux ressources sur le réseau de l’utilisateur peut être spécifié comme différents à partir de l’accès est lorsque l’utilisateur n’utilise pas d’une carte à puce (autrement dit, lorsque l’utilisateur ouvre une session en entrant un nom d’utilisateur et un mot de passe). Pour plus d’informations sur l’Assurance du mécanisme d’authentification, consultez le [l’Assurance du mécanisme d’authentification pour AD DS dans le Guide pas à pas de Windows Server 2008 R2](https://technet.microsoft.com/library/dd378897.aspx).  
  
#### <a name="configuring-privileged-account-authentication"></a>Configuration de l’authentification de compte privilégié  
Dans Active Directory pour tous les comptes d’administration, vous devez activer le **carte à puce est nécessaire pour l’ouverture de session interactive** attribut et l’audit des modifications apportées aux (au minimum), tous les attributs sur le **compte** onglet pour le compte (par exemple, cn, nom, sAMAccountName, userPrincipalName et userAccountControl) des objets utilisateur administratif.  
  
Bien que la configuration du **carte à puce est nécessaire pour l’ouverture de session interactive** sur des comptes réinitialise le mot de passe à une valeur aléatoire de 120 caractères et nécessite des cartes à puce pour les ouvertures de session interactive, l’attribut peuvent toujours être remplacés par les utilisateurs disposant des autorisations pour les autoriser à modifier les mots de passe sur les comptes et les comptes peuvent ensuite être utilisés pour établir des ouvertures de session non interactif avec uniquement le nom d’utilisateur et mot de passe.  
  
Dans d’autres cas, selon la configuration de comptes dans les paramètres Active Directory et le certificat dans les Services de certificats Active Directory (AD CS) ou une infrastructure à clé publique tiers, le nom d’utilisateur Principal (UPN) pour les attributs d’administration ou comptes VIP peuvent être ciblés pour un type spécifique d’attaque, comme décrit ici.  
  
##### <a name="upn-hijacking-for-certificate-spoofing"></a>Piratage UPN pour l’usurpation d’identité du certificat  
Même si un examen détaillé des attaques contre les infrastructures à clé publiques (PKI) est en dehors de la portée de ce document, les attaques contre les infrastructures à clé publique publiques et privées ont augmenté de manière exponentielle depuis 2008. Violations d’infrastructures à clé publique publics ont été largement objet d’une publicité, mais les attaques d’infrastructure à clé publique interne d’une organisation sont peut-être encore plus prolifiques. Une telle attaque exploite Active Directory et des certificats pour permettre à un attaquant d’usurper les informations d’identification d’autres comptes d’une manière qui peuvent être difficiles à détecter.  
  
Lorsqu’un certificat est présenté pour l’authentification à un système à un domaine, le contenu de l’objet ou l’attribut de l’autre nom de l’objet du certificat est utilisé pour mapper le certificat à un objet utilisateur dans Active Directory. Selon le type de certificat et comment elle est construite, l’attribut d’objet dans un certificat contient généralement un nom d’utilisateur commun (CN), comme illustré dans la capture d’écran suivante.  
  
![Modèles d’administration privilège minimum](media/Implementing-Least-Privilege-Administrative-Models/SAD_4.gif)  
  
Par défaut, Active Directory construit CN d’un utilisateur en concaténant le nom du compte premier +» «+ nom. Toutefois, les composants CN des objets utilisateur dans Active Directory ne sont pas requises ou uniques, et déplacement d’un compte d’utilisateur vers un autre emplacement dans le répertoire modifie le nom du compte unique (DN), qui est le chemin d’accès complet à l’objet dans le répertoire, comme indiqué dans le volet inférieur de la capture d’écran précédente.  
  
Étant donné que les noms de sujets de certificat ne sont pas garantis doit être statiques ou unique, le contenu de l’autre nom de sujet est souvent utilisé pour localiser l’objet utilisateur dans Active Directory. L’attribut SAN des certificats émis pour les utilisateurs à partir d’autorités de certification d’entreprise (autorités de certification intégrées à Active Directory) contient généralement l’adresse de l’utilisateur UPN ou par courrier électronique. Étant donné que les noms UPN est garantis pour être unique dans une forêt AD DS, localisation d’un objet utilisateur par nom d’utilisateur principal est généralement effectuée dans le cadre de l’authentification, avec ou sans impliqués dans le processus d’authentification de certificats.  
  
L’utilisation de noms UPN dans les attributs SAN dans les certificats d’authentification peut être exploitée par des pirates d’obtenir des certificats frauduleux. Si une personne malveillante a compromis un compte qui a la possibilité de lire et écrire l’UPN sur des objets utilisateur, l’attaque est implémentée comme suit:  
  
L’attribut de nom d’utilisateur principal sur un objet utilisateur (par exemple, un utilisateur VIP) est modifié temporairement sur une autre valeur. L’attribut de nom de compte SAM et CN peuvent également être modifié à ce stade, même si cela n’est généralement pas nécessaire pour les raisons décrites précédemment.  
  
Lors de l’attribut de nom UPN du compte cible a été modifié, un compte d’utilisateur obsolètes, activé ou l’attribut de nom d’utilisateur principal d’un compte d’utilisateur récemment créé est remplacée par la valeur qui a été affectée au compte cible. Comptes d’utilisateur obsolètes, activés sont des comptes que vous n’êtes pas connecté pendant de longues périodes de temps, mais n’ont pas été désactivées. Ils sont ciblés par les personnes malveillantes souhaitant «masquer en clair» pour les raisons suivantes:  
  
1.  Étant donné que le compte est activé, mais n’a pas été utilisé récemment, en utilisant le compte est peu susceptible de déclencher des alertes la façon dont l’activation d’un compte d’utilisateur désactivé peut.  
  
2.  Utilisation d’un compte existant ne nécessite pas la création d’un nouveau compte d’utilisateur qui peut être remarqué par le personnel d’administration.  
  
3.  Comptes d’utilisateur obsolètes qui sont toujours activées sont généralement des membres de différents groupes de sécurité et sont autorisés à accéder aux ressources sur le réseau, ce qui simplifie l’accès et «fusion» pour une population d’utilisateurs existants.  
  
Le compte d’utilisateur sur lequel la cible UPN est maintenant configurée est utilisé pour demander un ou plusieurs certificats à partir des Services de certificats Active Directory.  
  
Lorsque les certificats ont été obtenues pour le compte du pirate, l’UPN du compte «nouveaux» et le compte cible est retournés à leurs valeurs d’origine.  
  
La personne malveillante dispose maintenant d’un ou plusieurs certificats qui peuvent être présentées pour l’authentification aux ressources et applications comme si l’utilisateur est l’adresse IP virtuelle dont le compte a été modifié temporairement. Bien qu’une description complète de toutes les méthodes dans lequel les certificats et infrastructure à clé publique peuvent être ciblés par les personnes malveillantes est en dehors de la portée de ce document, ce mécanisme d’attaque est fourni pour illustrer pourquoi vous devez surveiller privilégiés et comptes VIP dans AD DS pour les modifications, en particulier pour les modifications apportées à tous les attributs sur le **compte** onglet (par exemple pour le compte cn, nom, sAMAccountName, userPrincipalName et userAccountControl). Outre les comptes de surveillance, vous devez restreindre qui peut modifier les comptes en tant que petites un ensemble d’utilisateurs administratifs que possible. De même, les comptes d’utilisateurs administratifs doivent être protégées et contrôler les modifications non autorisées de.  
  


