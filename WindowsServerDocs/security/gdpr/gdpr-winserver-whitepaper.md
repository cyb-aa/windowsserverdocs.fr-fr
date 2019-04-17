---
title: "À partir de votre voyage général données Protection règlement (PIBR) pour Windows Server2016"
description: "Cet article permet de comprendre les PIBR et sur les produits Microsoft fournit pour vous aider à commencer la conformité."
keywords: "Confidentialité, PIBR"
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.technology:
- techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 09/25/2017
ms.author: nirb
ms.openlocfilehash: e68b8d926f7749f7fcecf5f3752822c54db5ce76
ms.sourcegitcommit: c5aa1eedd1383b8b8f6b8fcb0af995c28277002a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2018
---
# <a name="beginning-your-general-data-protection-regulation-gdpr-journey-for-windows-server"></a>À partir de votre voyage général données Protection règlement (PIBR) pour Windows Server 

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cet article fournit des informations sur le PIBR, y compris qu’il est ainsi que les produits Microsoft pour vous aider à devenir compatibles.

## <a name="introduction"></a>Introduction
Sur le 25 mai2018, un droit de confidentialité européen est en raison de prendre effet qui définit une nouvelle barre globale pour les droits de confidentialité, sécurité et de conformité.

Le règlement de Protection de données général ou PIBR, est fondamentalement sur la protection et l’activation de droits de confidentialité des utilisateurs. Le PIBR établit les conditions de confidentialité global strict régissant comment gérer et protéger les données personnelles tout en respectant les choix individuel, quel que soit l’où les données sont envoyées, traitées ou stockées.

Microsoft et nos clients sont désormais sur un voyage pour atteindre les objectifs de confidentialité du PIBR. Chez Microsoft, nous pensons que la confidentialité est un droit fondamental, et nous pensons que le PIBR est une étape importante pour clarifier et l’activation de droits de confidentialité individuelles. Mais nous savons également que le PIBR nécessite des modifications importantes apportées par les organisations du monde entier.

Nous avons décrit notre engagement en le PIBR et comment nous sommes prise en charge de nos clients au sein du [PIBR obtenir compatible avec le Cloud de Microsoft](https://blogs.microsoft.com/on-the-issues/2017/02/15/get-gdpr-compliant-with-the-microsoft-cloud/#hv52B68OZTwhUj2c.99) billet de blog par notre directeur de confidentialité [Brendon Lynch](https://blogs.microsoft.com/on-the-issues/author/brendonlynch/) et [Earning votre relation d’approbation avec les engagements contractuels du règlement de Protection de données général](https://blogs.microsoft.com/on-the-issues/2017/04/17/earning-trust-contractual-commitments-general-data-protection-regulation/#6QbqoGWXCLavGM63.99)«billet de blog par [riche Sauer](https://blogs.microsoft.com/on-the-issues/author/rsauer/) -vice-président Microsoft et conseiller juridique et vice-président.

Bien que votre voyage dans PIBR-conformité peut sembler difficile, nous sommes là pour vous aider. Pour obtenir des informations spécifiques sur le PIBR, nos engagements et comment commencer votre voyage, visitez le [section PIBR de la MicrosoftTrust Center](https://www.microsoft.com/en-us/trustcenter/privacy/gdpr).

## <a name="gdpr-and-its-implications"></a>PIBR et ses implications
Le PIBR est un règlement complexe qui peut-être nécessiter des modifications importantes apportées à la façon dont vous recueillez, utiliserez et gérer les données personnelles. Microsoft a longtemps d’aider nos clients conformes aux réglementations complexes, et lorsqu’il s’agit de la préparation de la PIBR, nous sommes votre partenaire dans cette démarche.

Le PIBR impose des règles sur les organisations qui offrent des biens et services aux personnes de l’Union européenne (UE), ou qui collectent et analyser les données liées à résidents de l’Union européenne, quel que soit l’emplacement où se trouvent les entreprises. Parmi les éléments clés de la PIBR sont les suivants:

- **Droits de confidentialité améliorée.** En vous assurant qu’ils ont le droit d’accéder à leurs données personnelles, inexactitude correcte de ces données, pour effacer ces données, à l’objet de traitement des données personnelles et pour le déplacer de renforcer la protection des données pour les résidents de l’Union européenne.

- **Droit supérieur de la protection des données personnelles.** Renforcé plus de transparence pour les organisations qui traite les données personnelles, Clarifiez accrue de responsabilité de garantir la conformité.

- **Les rapports de violation des données personnelles obligatoires.** Les organisations qui contrôlent les données personnelles sont requises pour les violations de données personnelles de rapport qui présentent un risque pour les droits et les libertés des personnes à leurs autorités sans tarder et, si possible, ne postérieure à 72heures une fois qu’ils prennent connaissance de la violation.

Comme vous pouvez anticiper, le PIBR peut avoir un impact significatif sur votre entreprise, potentiellement vous obliger à mettre à jour les politiques de confidentialité, implémenter et renforcer les contrôles de protection de données et les procédures de notification de violation, déployer des stratégies hautement transparents, et d’investissement supplémentaire en informatique et de formation. MicrosoftWindows10 peut vous aider à efficacement et efficace de résoudre certains de ces exigences.

## <a name="personal-and-sensitive-data"></a>Données personnelles et confidentielles
Dans le cadre de vos efforts pour se conformer à la PIBR, vous devez comprendre comment le règlement définit les données personnelles et confidentielles et comment ces définitions se rapportent aux données détenues par votre organisation. Selon que vous serez en mesure de découvrir où ces données sont créées, traitement, de présentation gérés et stockés.

Le PIBR considère que des données personnelles toutes les informations relatives à une personne physique identifiée ou identifiable. Qui peut inclure une identification directe (par exemple, votre nom juridique) et identification indirecte (telles que des informations spécifiques qui le rend effacer vous êtes les références de données). Le PIBR montre clairement que le concept des données personnelles inclut des identificateurs en ligne (par exemple, les adresses IP, ID de périphérique mobile) et les données de localisation.

Le PIBR introduit les définitions spécifiques des données génétiques (par exemple, séquence de gène un individu) et des données biométriques. Données génétiques et les données biométriques avec d’autres sub catégories des données personnelles (les données personnelles révélant l’origine raciale ou ethnique, opinions politiques, convictions religieuses ou philosophiques ou l’appartenance au commerce union: données concernant la santé; ou données concernant un la personne vie sexuelle ou l’orientation sexuelle) sont traitées comme des données personnelles sensibles sous le PIBR. Les données personnelles sensibles celle dont les protections améliorées et nécessite généralement le consentement explicite d’un individu où ces données doivent être traitées.

### <a name="examples-of-info-relating-to-an-identified-or-identifiable-natural-person-data-subject"></a>Exemples d’informations relatives à une personne physique identifiée ou identifiable (objet de données)
Cette liste fournit des exemples des différents types d’informations est régie par le biais de PIBR. Cette liste n’est pas exhaustive.

-   Nom

-   Numéro d’identification (par exemple, SSN)

-   Données de localisation (par exemple, domicile)

-   Identificateur en ligne (par exemple, adresse de messagerie, les noms d’écran, adresse IP, ID de périphérique)

-   Données pseudonymes (par exemple, à l’aide d’une clé pour identifier des individus)

-   Données génétiques (par exemple, des échantillons biologiques à partir d’un individu)

-   Données biométriques (par exemple, empreintes digitales, la reconnaissance faciale)

## <a name="getting-started-on-the-journey-towards-gdpr-compliance"></a>Prise en main le parcours conformité PIBR
Étant donné la quantité est impliqué à devenir PIBR compatibles, nous recommandons vivement de ne pas attendre pour préparer jusqu'à ce que l’application commence. Vous devez passer en revue votre confidentialité et les pratiques de gestion des données maintenant. Nous vous conseillons de commencer votre voyage dans conformité PIBR en se concentrant sur quatre étapes principales:

-   **Découvrir.** Identifiez les données personnelles que vous avez et où il réside. 

-   **Gérer.** En définissant les données personnelles est utilisé et accessible.

-   **Protéger.** Établissez des contrôles de sécurité pour empêcher, détecter et répondre aux vulnérabilités et des violations de données.  

-   **Rapport.** Agir sur les demandes de données, des violations de données de rapport et conserver les documents requis.

    ![Diagramme sur le fonctionnement conjointement les principales étapes PIBR 4](../media/GDPR-Windows-Server-Overview/gdpr-steps-diagram.png)

Pour chacune des étapes, nous avons décrit les outils d’exemple, les ressources et les fonctionnalités dans les différentes solutions Microsoft, qui peuvent être utilisées pour vous aider à répondre aux besoins de cette étape. Alors que cet article n’est pas un guide complet «procédure», nous avons inclus des liens pour vous pour en savoir plus et plus d’informations est disponible dans le [section PIBR de la MicrosoftTrust Center](https://www.microsoft.com/en-us/trustcenter/privacy/gdpr).

## <a name="windows-server-security-and-privacy"></a>Confidentialité et la sécurité de Windows Server
Le PIBR, vous devez mettre en œuvre des mesures de sécurité techniques et d’organisation appropriées pour protéger les données personnelles et les systèmes de traitement. Dans le contexte de la PIBR, vos environnements de serveurs physiques et virtuels potentiellement traitement des données personnelles et confidentielles. Traitement peut signifier toute opération ou un ensemble d’opérations, telles que la collecte de données, de stockage et de récupération.

Votre capacité à répondre à cette exigence et à mettre en œuvre des mesures de sécurité technique approprié doit refléter les menaces, que vous êtes confronté dans les environnements informatiques plus en plus hostile d’aujourd'hui. Paysage de menaces de sécurité d’aujourd'hui est un des menaces agressives et tenaces. Ces dernières années, les pirates axée principalement sur l’obtention de reconnaissance de la Communauté par le biais de leurs attaques ou le frisson de temporairement mise hors connexion un système. Depuis, motivations l’attaquant ont glissé vers gagner de l’argent, y compris contenant les périphériques et otage de données jusqu'à ce que le propriétaire paie la rançon demandée.

Attaques modernes consistent plus en plus de vol de propriété intellectuelle à grande échelle; dégradation du système cible qui peut entraîner une perte financière; et maintenant même cyberterrorism qui menace la sécurité des personnes, les entreprises et les nationales centres d’intérêt dans le monde entier. Ces personnes malveillantes sont généralement des personnes extrêmement bien formés et des experts en sécurité, dont certains sont pour le compte de des États qui ont des budgets volumineux et des ressources humaines apparemment illimités. Menaces telles nécessitent une approche qui peut relever ce défi.

Non seulement sont ces menaces un risque pour votre capacité à garder le contrôle de toutes les données personnelles ou confidentielles, que vous devrez peut-être, mais ils représentent un risque de matériau à votre entreprise globale également. Tenez compte des données récentes McKinsey, Ponemon Institute, Verizon et Microsoft:

- Le coût moyen du type de violation de données le PIBR s’attendent à ce rapports est $3. 5M.

- 63% des violations de ces impliquent des mots de passe faibles ou volés que le PIBR attend adresse.

- Plus de 300000nouveaux exemples contre les programmes malveillants sont créés et propager à tous les jours qui rend votre tâche encore plus difficile à la protection des données d’adresse.

Comme avec les attaques Ransomware récentes, appelées une fois le noir peste d’Internet, les pirates vont après plus gros cibles qui peuvent se permettre de payer plus d’informations, avec les conséquences catastrophiques potentiellement. Le PIBR inclut des sanctions qui rendent vos systèmes, y compris les ordinateurs de bureau et ordinateurs portables, qui contiennent des cibles enrichie de données personnelles et confidentielles en effet.

Deux principes ont guidé et continuent guider le développement de Windows:

- **Sécurité.** Les données de que nos logiciels et services stockent pour le compte de nos clients doivent protégées contre les dommages et utilisées ou modifiées uniquement de manière appropriée. Modèles de sécurité doivent être faciles pour les développeurs à comprendre et à générer dans leurs applications.

- **Confidentialité.** Les utilisateurs doivent être dans le contrôle de l’utilisation de leurs données. Stratégies d’utilisation des informations doivent être claires à l’utilisateur. Les utilisateurs doivent être de contrôler quand et s’ils reçoivent des informations pour rendre le meilleur parti de leur temps. Il doit être facile aux utilisateurs de spécifier l’utilisation appropriée de leurs informations, y compris le contrôle de l’utilisation des e-mails qu’ils envoient.

Microsoft est resté permanent contre ces principes récemment indiqués par PDG de Microsoft, Satya Nadella, 

> «_Comme le monde continue à modifier et faire évoluer les besoins de l’entreprise, certains éléments sont cohérents: à la demande du client pour la sécurité et de confidentialité. _"

Comme vous travaillez pour se conformer à la PIBR, comprendre le rôle de vos serveurs physiques et virtuels dans la création, l’accès à, traitement, stockage et la gestion des données peuvent prendre en compte personnel et des données potentiellement sensibles sous le PIBR sont importantes. Windows Server fournit les fonctionnalités qui vous aidera à respecter les exigences PIBR à mettre en œuvre des mesures de sécurité techniques et d’organisation appropriées pour protéger les données personnelles.

La posture de sécurité de Windows Server2016 n’est pas une éclair-sur; il est un principe architectural. Et peuvent être mieux comprise dans quatre principaux:

- **Protéger.** Le focus en cours et innovation sur les mesures préventives; bloquer les attaques connues et contre les programmes malveillants connus.

- **Détecter.** Complète d’outils pour vous aider à repérer anomalies et répondre à des attaques plus rapides d’analyse.

- **Répondre.** Des technologies de réponse et de récupération plus profond conseils spécialisés.

- **Isoler.** Isoler les secrets de composants et les données du système d’exploitation et des privilèges d’administrateur de limiter rigoureusement mesurer contrôle d’intégrité de l’ordinateur hôte.

Avec Windows Server, votre capacité à protéger, de détecter et de se défendre contre les types d’attaques qui peuvent entraîner des violations de données est considérablement améliorée. Étant donné les exigences strictes autour de notification de violation dans le PIBR, garantissant que vos systèmes de bureau et portables sont bien défendaient diminuer les risques de que vous faire face qui peuvent entraîner la notification et d’analyse de la violation coûteux.

Dans la section qui suit, vous verrez comment Windows Server offre des fonctionnalités qui s’adapter à respecter lors de la phase «Protéger» de votre trajet de conformité PIBR. Ces fonctionnalités s’inscrivent dans les trois scénarios de protection:

- **Protéger vos informations d’identification et de limiter les privilèges d’administrateur.** Windows Server2016 permet d’implémenter ces modifications, pour aider à empêcher l’utilisation en tant que point de départ pour intrusions davantage votre système.

- **Sécuriser le système d’exploitation pour exécuter votre infrastructure et les applications.** Windows Server2016 offre des couches de protection, ce qui permet de bloquer les intrus externes à partir des logiciels malveillants en cours d’exécution ou d’exploiter les vulnérabilités.

- **Virtualisation sécurisée.** Windows Server2016 permet la virtualisation sécurisée, à l’aide de la protection des Machines virtuelles et structure protégée. Cela vous permet de chiffrer et exécuter vos ordinateurs virtuels sur des hôtes approuvés dans votre structure, une meilleure protection contre les attaques malveillantes.

Ces fonctionnalités, décrites plus en détail ci-dessous avec des références à des conditions spécifiques PIBR reposent sur la protection de périphérique avancée qui permet de maintenir l’intégrité et la sécurité du système d’exploitation et des données.

Une disposition dans le PIBR clée est la protection des données à la conception et, par défaut et aider avec votre capacité à répondre à cette disposition sont des fonctionnalités dans Windows10, telles que le chiffrement BitLocker. BitLocker utilise la technologie du Module de plateforme sécurisée (TPM), qui fournit des fonctions liées à la sécurité en fonction du matériel. Cette puce processeur de chiffrement comprend plusieurs mécanismes de sécurité physique pour rendre inviolable, et des logiciels malveillants ne peut pas falsifier les fonctions de sécurité du TPM.

La puce comprend plusieurs mécanismes de sécurité physique pour rendre inviolable, et des logiciels malveillants ne peut pas falsifier les fonctions de sécurité du TPM. Certains des avantages clés de l’utilisation de la technologie TPM sont que vous pouvez:

-   Générer, stocker et limiter l’utilisation des clés de chiffrement.

-   Utiliser la technologie TPM pour l’authentification des appareils plateforme à l’aide de clé RSA unique du TPM, qui est gravée sur elle-même.

-   Aider à garantir l’intégrité de la plateforme en réalisant et en stockant des mesures de sécurité.

Protection de périphérique avancée supplémentaires correspondant à votre exploitation sans les violations de données incluent démarrage sécurisé de Windows pour préserver l’intégrité du système en veillant à ce programme malveillant ne peut pas démarrer avant les défenses du système.

## <a name="windows-server-supporting-your-gdpr-compliance-journey"></a>Windows Server: Votre voyage de conformité PIBR de prise en charge
Les fonctionnalités clés dans Windows Server peuvent vous aider à efficacement implémenter les mécanismes de sécurité et de confidentialité que le PIBR requiert la conformité. Alors que l’utilisation de ces fonctionnalités ne garantit pas la conformité, ils prendront en charge vos efforts à le faire.

Le système d’exploitation se trouve sur une couche stratégique dans l’infrastructure d’une organisation, offrant de nouvelles opportunités pour créer des niveaux de protection contre les attaques de vol de données et votre activité d’interruption. Aspects clés de la PIBR telles que la confidentialité par conception, Protection des données et le contrôle d’accès doivent être traités au sein de votre infrastructure informatique au niveau du serveur.

Utilisation pour vous aider à protéger l’identité, système d’exploitation et les couches de virtualisation, Windows Server2016 permet de bloquer les vecteurs d’attaque courants utilisés pour obtenir un accès illicite sur vos systèmes: vol des informations d’identification, les logiciels malveillants et une structure de virtualisation compromis. En plus de ce qui réduit les risques pour l’entreprise, les composants de sécurité intégrée à Windows Server2016 aide répondre aux exigences de conformité pour la clé gouvernementales et de l’industrie règles de sécurité. 

Ces identité, système d’exploitation, les protections de virtualisation vous permettent de mieux protéger votre centre de données en cours d’exécution Windows Server comme un ordinateur virtuel dans n’importe quel cloud et limiter la capacité des pirates à compromettre les informations d’identification, lancer contre les programmes malveillants et échapper dans votre réseau. De même, lors du déploiement en tant qu’un ordinateur hôte Hyper-V, Windows Server2016 offre la garantie de sécurité de vos environnements de virtualisation via dotées d’ordinateurs virtuels et des fonctionnalités de pare-feu distribué. Avec Windows Server2016, le système d’exploitation devient un participant actif dans votre centre de données de sécurité.

### <a name="protect-your-credentials-and-limit-administrator-privileges"></a>Protéger vos informations d’identification et de limiter les privilèges d’administrateur 
Contrôler l’accès à des données personnelles et les systèmes qui traitent des données, est une zone avec le PIBR qui a des exigences spécifiques, y compris l’accès par les administrateurs. Identités privilégiées sont tous les comptes disposant de privilèges, tels que les comptes d’utilisateur qui sont membres du domaine Administrateurs, les administrateurs d’entreprise, Administrateurs local ou même les groupes d’utilisateurs avec pouvoir élevés. Ces identités peuvent également inclure des comptes qui ont reçu des privilèges directement, telle que l’exécution des sauvegardes, arrêt du système ou d’autres droits répertoriés dans le nœud de l’attribution des droits utilisateur dans la console Stratégie de sécurité locale.

Comme un principe de contrôle d’accès général et en ligne avec le PIBR, vous devez protéger ces identités privilégiées d’un compromis par des pirates potentiels. Tout d’abord, il est important de comprendre comment les identités sont compromises. Vous pouvez planifier empêcher les intrus d’accéder à ces identités privilégiées.

#### <a name="how-do-privileged-identities-get-compromised"></a>Comment identités privilégiées compromises?
Identités privilégiées peuvent compromises lorsque les organisations n’ont des recommandations pour les protéger. Voici quelques exemples:

- **Plus de privilèges que nécessaire.** Un des problèmes plus courants est que les utilisateurs ont plus de privilèges que nécessaire pour effectuer leur fonction. Par exemple, un utilisateur qui gère le DNS peut être un administrateur ActiveDirectory. Le plus souvent, cela pour éviter de devoir configurer différents niveaux d’administration. Toutefois, si ce compte est compromis, la personne malveillante automatiquement dispose de privilèges.

- **Constamment connecté avec des privilèges élevés.** Un autre problème courant est que les utilisateurs avec des privilèges élevés peuvent l’utiliser pour une durée illimitée. Cela est très courant avec les professionnels de l’informatique qui se connecter à un ordinateur de bureau à l’aide d’un compte privilégié de restent connecté et utilisent le compte privilégié pour parcourir le web et messagerie (classique travail informatique fonctions). Durée illimitée des comptes privilégiés rend le compte plus vulnérables aux attaques et augmente les chances que le compte sera compromis.

- **Recherche d’ingénierie sociale.** La plupart des menaces d’informations d’identification de commencer par la recherche de l’organisation et puis effectué par le biais d’ingénierie sociale. Par exemple, une personne malveillante peut effectuer une attaque de hameçonnage messagerie comptes légitime compromis (mais pas nécessairement avec élévation de privilèges comptes) qui ont accès à un réseau d’entreprise. La personne malveillante utilise ensuite ces comptes valides pour effectuer des recherches supplémentaires sur votre réseau et d’identifier les comptes privilégiés qui peuvent effectuer des tâches d’administration. 

- **Tirer parti des comptes avec des privilèges élevés.** Même avec un compte d’utilisateur normal, et non avec élévation de privilèges dans le réseau, des personnes malveillantes peuvent accéder aux comptes disposant des autorisations avec élévation de privilèges. Une des méthodes plus courantes de cette opération est donc en utilisant le Pass-the-Hash ou les attaques Pass-the-Token. Pour plus d’informations sur le Pass-the-Hash et autres techniques de vol d’informations d’identification, voir les ressources sur le [Pass-the-Hash (PtH)](https://technet.microsoft.com/en-us/dn785092.aspx).

Il existe bien entendu d’autres méthodes de personnes malveillantes peuvent utiliser pour identifier et compromettre les identités privilégiées (avec de nouvelles méthodes en cours de création tous les jours). Il est donc important d’établir des pratiques pour les utilisateurs de se connecter avec les privilèges des comptes afin de réduire la capacité des personnes malveillantes d’accéder aux identités privilégiées. Les sections suivantes abordent les fonctionnalités où Windows Server peut réduire ces risques.

#### <a name="just-in-time-admin-jit-and-just-enough-admin-jea"></a>L’administration juste-à-temps (JIT) et Just Enough administration (JEA)
Protection contre les attaques Pass-the-Ticket Pass-the-Hash est importante, informations d’identification de l’administrateur peuvent toujours être accaparées par d’autres moyens, notamment les techniques d’ingénierie sociale, aux employés mal intentionnés et en force brute. Par conséquent, en plus de l’isolation des informations d’identification autant que possibles, vous également un moyen de limiter la portée des privilèges de niveau administrateur au cas où ils sont compromis.

Aujourd'hui, trop de comptes d’administrateur sont trop privilégiés, même si elles n'ont qu’une seule zone de responsabilité. Par exemple, un administrateur DNS, qui requiert un ensemble très étroit de privilèges pour gérer les serveurs DNS, est accordé souvent des privilèges de niveau administrateur de domaine. En outre, étant donné que ces informations d’identification sont accordées pour perpétuité, est illimité sur la durée pendant laquelle ils peuvent être utilisés.

Chaque compte avec des privilèges de niveau administrateur de domaine inutiles augmente les risques de pirates cherchant à compromettre les informations d’identification. Pour réduire la surface d’attaque, que vous voulez fournir uniquement l’ensemble spécifique de droits d’administrateur doit effectuer la tâche – et uniquement pour la fenêtre de temps nécessaire pour terminer.

À l’aide de Just Enough Administration et juste-à-temps Administration, les administrateurs peuvent demander des privilèges spécifiques dont ils ont besoin pour la fenêtre exacte du temps nécessaire. Pour l’administrateur DNS, par exemple, à l’aide de PowerShell pour activer Just Enough Administration vous permet de créer un ensemble limité de commandes qui sont disponibles pour la gestion DNS.

Si l’administrateur DNS doit effectuer une mise à jour à un de ses serveurs, elle serait demander l’accès à gérer le système DNS à l’aide de MicrosoftIdentity Manager2016. Le workflow de demande peut inclure un processus d’approbation comme authentification à deux facteurs, ce qui peut appeler téléphone mobile de l’administrateur pour vérifier son identité avant d’accorder les privilèges requis. Une fois attribuée, ces privilèges DNS fournissent l’accès au rôle PowerShell pour DNS pour une période de temps spécifique.

Imaginons le scénario si les informations d’identification de l’administrateur DNS ont été volées. Tout d’abord, dans la mesure où les informations d’identification n’ont aucune associés à des privilèges d’administration, l’attaquant serait pas en mesure d’accéder au serveur DNS – ou d’autres systèmes – d’apporter des modifications. Si la personne malveillante a essayé de demander des privilèges pour le serveur DNS, l’authentification multifacteur invitait à confirmer leur identité. Dans la mesure où il est peu probable que la personne malveillante a téléphone mobile de l’administrateur DNS, l’authentification échoue. Cela aurait verrouiller la personne malveillante hors du système et le service informatique d’alertes que les informations d’identification peuvent être compromises.

En outre, de nombreuses organisations utilisent le libre [Solution de mot de passe d’administrateur Local (LAPS)](http://aka.ms/laps) comme un mécanisme d’administration JIT simple mais puissant pour leurs systèmes serveur et client. La fonctionnalité LAPS offre une gestion des mots de passe de compte local d’ordinateurs appartenant à un domaine. Mots de passe sont stockés dans ActiveDirectory (AD) et protégées par et liste de contrôle d’accès (ACL) par conséquent, les utilisateurs uniquement peuvent lire ou demander sa réinitialisation.

Comme indiqué dans le [Guide d’atténuation Windows Credential vol](https://www.microsoft.com/en-us/download/confirmation.aspx?id=54095), 

> «_criminelles les outils et techniques permettent de mener à bien le vol d’informations d’identification et améliorent la réutilisation d’attaques, les pirates trouvent qu’il est plus facile à atteindre leurs objectifs. Contre le vol d’informations d’identification s’appuie sur les pratiques opérationnelles ou l’exposition des informations d’identification utilisateur, souvent atténuations efficaces requiert une approche globale qui traite des personnes, des processus et des technologies. En outre, ces attaques s’appuient sur l’informations d’identification vol après compromettre un système pour développer ou de conserver l’accès, pour les organisations doivent contenir les violations rapidement en mettant en œuvre des stratégies qui empêchent les attaquants de déplacer librement et non détectée dans un réseau compromis. _"

Un aspect de conception importants pour Windows Server a été atténuation contre le vol d’informations d’identification, en particulier, les informations d’identification dérivées. Credential Guard offre améliore considérablement la sécurité contre le vol d’informations d’identification dérivées et réutilisation par l’implémentation d’une modification architecturale significative dans Windows conçues pour aider à éliminer les attaques d’isolation en fonction du matériel plutôt que de simplement de se défendre contre.

À l’aide de WindowsDefenderCredentialGuard, NTLM et Kerberos des informations d’identification dérivées sont protégées à l’aide de la sécurité basée sur la virtualisation, techniques d’attaque le vol d’informations d’identification et outils utilisés dans de nombreuses attaques ciblées sont bloqués. Contre les programmes malveillants en cours d’exécution dans le système d’exploitation avec des privilèges d’administration ne peut pas extraire les secrets protégés par la sécurité basée sur la virtualisation. Pendant que WindowsDefenderCredentialGuard est une solution puissante, les attaques de menace persistante seront susceptibles d’avoir recours à nouvelles techniques d’attaque et vous devez également d’intégrer Device Guard, comme décrit ci-dessous, ainsi que d’autres stratégies de sécurité et les architectures.

#### <a name="windows-defender-credential-guard"></a>WindowsDefenderCredentialGuard
WindowsDefenderCredentialGuard utilise la sécurité basée sur la virtualisation pour isoler les informations d’identification, empêchant les hachages de mot de passe ou les tickets Kerberos d’être intercepté. Elle utilise un processus autorité de sécurité locale (LSA) isolé entièrement nouveau, ce qui n’est pas accessible pour le reste du système d’exploitation. Utilisé par l’autorité LSA isolé de tous les fichiers binaires sont signés avec des certificats qui sont validées avant de les lancer dans l’environnement protégé, l’établissement des attaques de type Pass-the-Hash complètement inefficace.

WindowsDefenderCredentialGuard utilise:

- Sécurité basée sur la virtualisation (obligatoire). Également requis:

    - Processeur 64bits

    - Extensions de virtualisation du processeur, ainsi que des tables de pages étendues

    - Hyperviseur Windows

- Démarrage sécurisé (obligatoire)

- Module de plateforme sécurisée 2.0 soit discret ou microprogramme (de préférence - fournit une liaison au matériel)

Vous pouvez utiliser WindowsDefenderCredentialGuard pour vous aider à protéger les identités privilégiées en protégeant les informations d’identification de dérivés d’informations d’identification sur Windows Server2016. Pour plus d’informations sur la configuration requise pour WindowsDefenderCredentialGuard, voir [protéger les dérivées des informations d’identification de domaine avec la protection des informations d’identification Windows Defender](https://docs.microsoft.com/en-us/windows/access-protection/credential-guard/credential-guard).

#### <a name="windows-defender-remote-credential-guard"></a>Protection des informations d’identification à distance Windows Defender
WindowsDefenderCredentialGuard à distance sur Windows Server2016 et mise à jour anniversaire Windows10vous permet également de protéger les informations d’identification pour les utilisateurs avec les connexions Bureau à distance. Auparavant, tout le monde à l’aide des Services Bureau à distance devrait se connecter à leur ordinateur local et ensuite être requis pour se connecter à nouveau lorsqu’ils effectuée une connexion à distance à leur ordinateur cible. Cette deuxième connexion serait transmettre des informations d’identification à l’ordinateur cible, les exposer à des Pass-the-Hash ou les attaques Pass-the-Ticket.

Avec WindowsDefenderCredentialGuard à distance, Windows Server2016 implémente de l’authentification unique pour les sessions Bureau à distance, en éliminant la nécessité d’entrer de nouveau votre nom d’utilisateur et un mot de passe. Au lieu de cela, elle s’appuie sur les informations d’identification que vous avez déjà utilisé pour ouvrir une session sur votre ordinateur local. Pour utiliser WindowsDefenderCredentialGuard à distance, le client Bureau à distance et le serveur doivent répondre aux exigences suivantes:

- Doit être joint à un domaine ActiveDirectory et se trouver dans le même domaine ou d’un domaine avec une relation d’approbation.

- Doit utiliser l’authentification Kerberos.

- Doit être en cours d’exécution au moins Windows10 version1607 ou Windows Server2016.  

- L’application Windows classique de bureau à distance est requise. L’application à distance Bureau plateforme Windows universelle ne prennent pas en charge WindowsDefenderCredentialGuard à distance.

Vous pouvez activer WindowsDefenderCredentialGuard à distance à l’aide d’un paramètre de Registre sur le serveur des services Bureau à distance et d’une stratégie de groupe ou d’un paramètre de connexion Bureau à distance sur le client Bureau à distance. Pour plus d’informations sur l’activation de WindowsDefenderCredentialGuard à distance, voir [informations d’identification de protéger les services Bureau à distance avec WindowsDefenderCredentialGuard à distance](https://docs.microsoft.com/en-us/windows/access-protection/remote-credential-guard). Avec WindowsDefenderCredentialGuard, vous pouvez utiliser WindowsDefenderCredentialGuard à distance pour aider à protéger les identités privilégiées sur Windows Server2016.

### <a name="secure-the-operating-system-to-run-your-apps-and-infrastructure"></a>Sécuriser le système d’exploitation pour exécuter votre infrastructure et les applications
Empêcher les menaces requiert également une recherche et bloque les programmes malveillants et les attaques qui tentent de prendre le contrôle par Subversion les pratiques d’exploitation standards de votre infrastructure. Si les personnes malveillantes peuvent obtenir un système d’exploitation ou une application de s’exécuter de manière non prédéterminé, non viables, ils sont probablement à l’aide ce système de prendre des mesures malveillants. Windows Server2016 offre des couches de protection qui bloquent les intrus externes en cours d’exécution des logiciels malveillants ou d’exploiter les vulnérabilités. Le système d’exploitation prend un rôle actif dans la protection d’infrastructure et les applications par l’alerte aux administrateurs d’activité qui indique un système n’a pas été respecté.

#### <a name="windows-defender-device-guard"></a>WindowsDefenderDeviceGuard
Windows Server2016 inclut WindowsDefenderDeviceGuard pour vous assurer que seuls les logiciels approuvés peuvent être exécuté sur le serveur. À l’aide de la sécurité basée sur la virtualisation, il peut limiter les fichiers binaires peuvent s’exécutent sur le système en fonction de la stratégie de l’entreprise. Si rien, autres que les fichiers binaires spécifiés tente d’exécuter, Windows Server2016 bloque et enregistre la tentative a échoué afin que les administrateurs peuvent voir qu’il a été une faille potentielle. Notification de violation est une partie essentielle de la configuration requise pour la conformité PIBR.

WindowsDefenderDeviceGuard est également intégré avec PowerShell afin que vous pouvez autoriser les scripts peuvent s’exécuter sur votre système. Dans les versions antérieures de Windows Server, les administrateurs pourraient contourner la mise en œuvre de l’intégrité du code en supprimant simplement la stratégie à partir du fichier de code. Avec Windows Server2016, vous pouvez configurer une stratégie qui est signée par votre organisation afin que seule une personne ayant accès au certificat qui a signé la stratégie peut modifier la stratégie.

#### <a name="control-flow-guard"></a>Protection du flux de contrôle 
Windows Server2016 inclut également une protection intégrée contre certaines classes d’attaques de corruption de mémoire. Il est important de vos serveurs de mise à jour corrective, mais il est toujours possible qu’un logiciel malveillant pourrait être développé pour une vulnérabilité n’a pas encore été identifiée. Certaines des méthodes plus courantes pour ces vulnérabilités doivent fournir des données inhabituelles ou extrêmes à un programme en cours d’exécution. Par exemple, une personne malveillante peut exploiter une vulnérabilité de dépassement de capacité de mémoire tampon en fournissant plus d’entrée à un programme que prévu et un dépassement de la zone réservée par le programme pour contenir une réponse. Cela peut endommager située en regard de mémoire qui peut contenir un pointeur de fonction.

Lorsque le programme appelle par le biais de cette fonction, il peut ensuite atteindre un emplacement involontaire spécifié par la personne malveillante. Ces attaques sont également appelés orienté sur les raccourcis des attaques par programmation (JOP). Protection du flux de contrôle empêche les attaques JOP en plaçant sévères restrictions sur le code d’application peut être exécutée, notamment indirect d’instructions d’appel. Il ajoute les contrôles de sécurité léger pour identifier l’ensemble des fonctions dans l’application qui sont des cibles valides pour les appels indirects. Lorsqu’une application s’exécute, il vérifie que ces cibles appel indirect sont valides.

Si la vérification de la protection du flux contrôle échoue lors de l’exécution, Windows Server2016 arrête immédiatement le programme rupture toute attaque du mécanisme qui tente d’appeler indirectement une adresse non valide. Protection du flux de contrôle fournit une couche de protection supplémentaire importante à Device Guard. Si une application dans la liste blanche a été compromise, il serait en mesure d’exécuter non contrôlé par Device Guard, comme le Device Guard filtrage pourrait voir que l’application a été signée et qu’il est considéré comme approuvé.

Mais étant donné que la protection du flux de contrôle peut identifier si l’application s’exécute dans un ordre non viable non prédéfinis, l’attaque échoue, empêche l’application en cours d’exécution compromise. Ensemble, ces protections rendent très difficile pour les pirates d’injecter contre les programmes malveillants dans les logiciels s’exécutant sur Windows Server2016.

Les développeurs qui créent des applications dans lequel les données personnelles seront gérées sont invités à activer le contrôle de flux (CFG) dans leurs applications. Cette fonctionnalité est disponible dans MicrosoftVisual Studio2015 et s’exécute sur les versions de Windows «CFG prenant en charge», le x86 et x64 libère de bureau et serveur de Windows10 et mise à jour Windows8.1 (KB3000850). Vous n’êtes pas obligé d’activer CFG pour chaque partie de votre code, comme un mélange de CFG activée et le code non CFG activée s’exécute correctement. Mais ne peut pas activer la fonctionnalité CFG pour tout le code peut ouvrir les lacunes dans la protection. En outre, CFG activée code fonctionne correctement sur «Non CFG» versions de Windows et n’est donc entièrement compatible avec eux.

#### <a name="windows-defender-antivirus"></a>WindowsDefenderAntivirus
Windows Server2016 inclut les secteur principaux, les fonctionnalités de détection de Windows Defender pour bloquer les logiciels malveillants connus. WindowsDefenderAntivirus (AV) fonctionne avec WindowsDefenderDeviceGuard et de protection du flux de contrôle pour empêcher l’installation sur vos serveurs de programmes malveillants d’aucune sorte. Il est activé par défaut, l’administrateur n’a pas besoin aucune action pour qu’il puisse commencer à travailler. WindowsDefenderAV est également optimisé pour prendre en charge les différents rôles de serveur dans Windows Server2016. Dans le passé, les pirates utilisé interpréteurs de commandes tels que PowerShell pour lancer des programmes malveillants binaire. Dans Windows Server2016, PowerShell est désormais intégré à WindowsDefenderAV pour rechercher les programmes malveillants avant de lancer le code.

WindowsDefenderAV est une solution anti-programmes malveillants intégrée qui fournit une gestion de sécurité et de logiciel anti-programme malveillant aux ordinateurs de bureau, ordinateurs portables et les serveurs. WindowsDefenderAV a été considérablement améliorée dans la mesure où il a été introduit dans Windows8. L’Antivirus Windows Defender dans Windows Server utilise une approche complexes pour améliorer les logiciels anti-programme malveillant:

- **Protection cloud remis** vous aide à détecter et bloquer les nouveaux programmes malveillants en quelques secondes, même si le programme malveillant n’a jamais été rencontré avant.

- **Contexte local enrichi** améliore la façon dont les programmes malveillants sont identifié. Windows Server indique à WindowsDefenderAV non seulement sur le contenu, tel que des fichiers et des processus, mais également le contenu provenance, où il a été stocké et bien plus encore. 

- **Capteurs globaux** maintenir WindowsDefenderAV jour et même les plus récents contre les programmes malveillants. Cette opération est effectuée de deux manières: en collectant les données de contexte local enrichi à partir de points de terminaison et en analysant ces données de manière centralisée.

- **Anti-falsification** contribue à protéger Windows Defender violation d’accès lui-même contre les attaques contre les programmes malveillants. Par exemple, WindowsDefenderAV utilise des processus protégés, ce qui empêche les processus non approuvés de tenter de falsifier les composants de WindowsDefenderAV, ses clés de Registre et ainsi de suite.

- **Fonctionnalités de niveau entreprise** donner des professionnels de l’informatique les outils et options de configuration nécessaires pour que WindowsDefenderAV une solution anti-programme malveillant d’entreprise.

#### <a name="enhanced-security-auditing"></a>L’audit de sécurité renforcée 
Windows Server2016 alerte activement les administrateurs à des tentatives de violation potentielle avec l’audit de sécurité améliorée qui fournit des informations plus détaillées, ce qui peuvent être utilisées pour la détection des attaques plus rapide et l’analyse d’investigation. Il enregistre les événements à partir de la protection du flux de contrôle, WindowsDefenderDeviceGuard et autres fonctionnalités de sécurité dans un seul emplacement, rendant ainsi plus facile pour les administrateurs à déterminer quels systèmes peuvent être exposés.

Nouvelles catégories d’événements sont les suivantes:

- **Appartenance au groupe d’audit.** Vous permet d’auditer les informations d’appartenance de groupe dans le jeton d’ouverture de session d’un utilisateur. Événements sont générés lorsque des appartenances au groupe sont énumérées ou interrogées sur le PC sur lequel la session a été créée. 
 
- **Auditer l’activité Plug and Play.** Vous permet d’auditer lorsque plug-and-play détecte un appareil externe, ce qui peut contenir des programmes malveillants. Événements PnP peuvent être utilisés pour effectuer le suivi des modifications en fonction du matériel système. Une liste des codes de fournisseurs de matériel est incluse dans l’événement.

Windows Server2016 s’intègre facilement aux systèmes de gestion (SIEM) d’événements aux incidents de sécurité, tels que MicrosoftOperations Management Suite (OMS), ce qui peut intégrer les informations de rapports Asset intelligence sur les violations potentielles. La profondeur des informations fournies par l’audit amélioré permet aux équipes de sécurité identifier et répondre aux violations potentielles plus rapidement et efficacement.

### <a name="secure-virtualization"></a>Virtualisation sécurisée
Les entreprises aujourd'hui virtualiser tout ce dont ils peuvent, à partir de SQLServer pour SharePoint sur les contrôleurs de domaine ActiveDirectory. Machines virtuelles (VM) Assurez-vous simplement plus facile à déployer, gérer, service et automatiser votre infrastructure. Mais lorsqu’il s’agit de sécurité, les structures de virtualisation compromis sont devenus un nouveau vecteur d’attaque qui est difficile de se défendre contre – jusqu'à présent. À partir d’un point de vue PIBR, vous devez réfléchir à la protection des ordinateurs virtuels comme vous le feriez protéger serveurs physiques, notamment l’utilisation de la technologie TPM VM.

Windows Server2016 modifie radicalement la façon dont les entreprises peuvent sécuriser la virtualisation, en incluant plusieurs technologies qui vous permettent de créer des ordinateurs virtuels qui s’exécutent uniquement sur votre propre structure; contribue à protéger contre le stockage, de réseau et ils s’exécutent sur des périphériques hôte.

#### <a name="shielded-virtual-machines"></a>Machines virtuelles
Les mêmes opérations qui facilitent par conséquent, les ordinateurs virtuels à migrer, sauvegarde et réplication, facilitent également les modifier et de copier. Une machine virtuelle est juste un fichier, afin qu’il n’est pas protégé sur le réseau, stockage, dans les sauvegardes ou ailleurs. Un autre problème est que les administrateurs de structure – si elles sont un administrateur de stockage ou un administrateur réseau – ont accès à tous les ordinateurs virtuels.

Un administrateur compromis sur l’infrastructure peut entraîner facilement les données compromises entre les machines virtuelles. Tout ce que la personne malveillante doit faire est d’utiliser les informations d’identification compromises pour copier les fichiers de machine virtuelle qu’ils aiment sur un lecteur USB et parcourir hors de l’organisation, où les fichiers de machine virtuelle est accessible à partir de n’importe quel autre système. Si l’un de ces ordinateurs virtuels volés était un contrôleur de domaine ActiveDirectory, par exemple, la personne malveillante pourrait facilement afficher le contenu et utiliser techniques disponibles en force brute à déchiffrer les mots de passe dans la base de données ActiveDirectory, pour finir leur donnant accès pour tout le reste de votre infrastructure.

Windows Server2016 introduit dotées d’ordinateurs virtuels (dotées) pour vous protéger contre des scénarios tels que celui décrit précédemment. VM dotées d’incluent un périphérique virtuel module de plateforme sécurisée, ce qui permet aux organisations d’appliquer le chiffrement BitLocker pour les ordinateurs virtuels et garantir qu’ils s’exécutent uniquement sur des hôtes approuvés pour vous protéger contre les administrateurs d’hôtes, réseau et stockage compromis. Machines virtuelles protégées sont créés à l’aide d’ordinateurs virtuels génération 2, qui prend en charge du microprogramme Unified Extensible Firmware Interface (UEFI) et le module de plateforme sécurisée virtuels.

#### <a name="host-guardian-service"></a>Service Guardian hôte
À côté des machines virtuelles protégées, le Service Guardian hôte est un composant essentiel pour la création d’une structure de virtualisation sécurisée. Son rôle est d’attester de l’intégrité d’un ordinateur hôte Hyper-V avant qu’il autorise machine virtuelle protégée pour démarrer ou de migrer vers cet ordinateur hôte. Il conserve les clés pour les machines virtuelles protégées et ne les libère jusqu'à ce que l’intégrité de la sécurité est assurée. Il existe deux méthodes que vous pouvez exiger des ordinateurs hôtes Hyper-V d’attester du Service Guardian hôte.

La première et la plus sécurisée, sont attestation approuvée de matériel. Cette solution exige que vos machines virtuelles protégées sont en cours d’exécution sur les ordinateurs hôtes qui ont des puces de TPM 2.0 et UEFI 2.3.1. Ce matériel est requis pour fournir la fonctionnalité démarrage mesuré et informations de l’intégrité du noyau de système d’exploitation requises par le Service Guardian hôte pour garantir que l’ordinateur hôte Hyper-V n’a pas été falsifiées.

Les services informatiques ont d’utiliser l’attestation approuvée par l’administrateur, ce qui peut être souhaitable si le matériel du module de plateforme sécurisée 2.0 n’est pas en cours d’utilisation dans votre organisation. Ce modèle d’attestation est facile à déployer, car les ordinateurs hôtes sont simplement placés dans un groupe de sécurité et le Service Guardian hôte est configuré pour autoriser des machines virtuelles protégées pour s’exécuter sur les ordinateurs hôtes qui sont membres du groupe de sécurité. Avec cette méthode, il n’existe aucune mesure complexe pour vous assurer que l’ordinateur hôte n’a pas été falsifié. Toutefois, vous n’éliminez la possibilité de non chiffré disques de machines virtuelles à pied la porte sur USB ou que l’ordinateur virtuel s’exécute sur un ordinateur hôte non autorisé. Il s’agit dans la mesure où les fichiers de machine virtuelle ne s’exécutera pas sur un ordinateur autre que ceux du groupe désigné. Si vous n’avez pas encore de matériel TPM 2.0, vous pouvez démarrer avec l’attestation approuvée par l’administrateur et basculer vers l’attestation approuvée matériel lors de la mise à niveau votre matériel.

#### <a name="virtual-machine-trusted-platform-module"></a>Module de plateforme sécurisée de Machine virtuelle
Windows Server2016 prend en charge le module de plateforme sécurisée pour les ordinateurs virtuels, qui vous permet de prendre en charge des technologies de sécurité avancées telles que le chiffrement de lecteur BitLocker® dans les ordinateurs virtuels. Vous pouvez activer la prise en charge du module de plateforme sécurisée sur des ordinateurs virtuels de génération 2 Hyper-V à l’aide de Gestionnaire Hyper-V ou l’applet de commande WindowsPowerShellEnable-VMTPM.

Vous pouvez protéger le module de plateforme sécurisée virtuelle (vTPM) à l’aide des clés de chiffrement locales stockées sur l’ordinateur hôte ou stockés dans le Service Guardian hôte. Par conséquent, tandis que le Service Guardian hôte nécessite une infrastructure plus, il fournit également une protection plus.

#### <a name="distributed-network-firewall-using-software-defined-networking"></a>Pare-feu de réseau distribué à l’aide d’accès réseau défini par le logiciel
Une façon d’améliorer la protection dans des environnements virtualisés consiste à segmenter le réseau d’une manière qui permet d’ordinateurs virtuels à ne communiquer qu’avec les systèmes spécifiques requises pour fonctionner. Par exemple, si votre application n’a pas besoin pour se connecter à Internet, vous pouvez partitionner, éliminant ainsi ces systèmes en tant que cibles contre les personnes malveillantes externes. Défini par logiciel réseau (SDN) dans Windows Server2016 inclut un pare-feu de réseau distribué qui vous permet de créer dynamiquement les stratégies de sécurité qui peuvent protéger vos applications des attaques provenant à l’intérieur ou en dehors d’un réseau. Ce pare-feu de réseau distribué ajoute des couches de sécurité de votre en vous permettant d’isoler vos applications dans le réseau. Stratégies peuvent être appliquées n’importe où sur votre infrastructure de réseau virtuel, isoler le trafic VM-to-VM, le trafic VM-to-host ou VM-to-Internet le cas échéant, de trafic pour les systèmes individuels qui ont été compromis ou par programme sur plusieurs sous-réseaux. Fonctionnalités de mise en réseau définie par le logiciel Windows Server2016 permettent également de router ou de mise en miroir du trafic entrant non Microsoft des équipements virtuels. Par exemple, vous pouvez choisir d’envoyer tout le trafic des messages par le biais d’une appliance virtuelle Barracuda pour la protection de filtrage supplémentaire. Cela vous permet de facilement couche de sécurité supplémentaires à la fois local ou dans le cloud.

### <a name="other-gdpr-considerations-for-servers"></a>Autres considérations PIBR pour les serveurs
Le PIBR inclut des exigences explicites pour la notification de violation où une violation des données personnelles signifie, «_une faille de sécurité, ce qui conduit à la destruction accidentelle ou illicite, perte, modification, divulgation non autorisée, ou accès à, des données personnelles transmises, stockées ou traitées dans le cas contraire. _"  Bien évidemment, vous ne peut pas commencer à avancer exigences strictes PIBR notification au sein de 72heures si vous ne peut pas détecter la violation en premier lieu.

Comme indiqué dans le centre de sécurité Windows livre blanc, [Post violation: gestion avancée des menaces](http://wincom.blob.core.windows.net/documents/Post_Breach_Dealing_with_Advanced_Threats_Whitepaper.pdf,)

> «_Contrairement à une violation antérieurs, violation après suppose une brèche a déjà eu lieu – agissant en tant que noire et responsable des essais de scène Crime (CSI). Après Post-breach fournit les informations d’associations de sécurité et ensemble d’outils nécessaires pour identifier, examiner et répondre à des attaques qui sinon reste non détectés et en dessous de la détection. _"

Dans cette section, nous allons examiner comment Windows Server peut vous aider à répondre à vos obligations de notification de violation PIBR. Nous commençons par la présentation des données contre les menaces sous-jacentes disponibles pour Microsoft, qui est recueilli et analysé à votre avantage et comment, par le biais de Windows Defender avancée contre les menaces Protection (ATP), ces données peuvent être critiques pour vous.

#### <a name="insightful-security-diagnostic-data"></a>Données de diagnostic de sécurité pleines d’esprit
Depuis presque deux ans, Microsoft a été transformer les menaces en intelligence utile qui peut aider à renforcer sa plateforme et protéger les clients. Aujourd'hui, avec les avantages informatiques provoquerait offertes par le cloud, nous publions la recherche de nouvelles façons d’utiliser nos moteurs analytique enrichi pilotées par les renseignements pour protéger nos clients.

En appliquant une combinaison de processus automatisés et manuels, apprentissage machine et humaines experts, nous pouvons créer un graphique de sécurité Intelligent qui apprend à partir de lui-même et évolue en temps réel, réduisant notre temps collectif pour détecter et répondre aux incidents de nouveautés sur nos produits.

![Graphique de sécurité MicrosoftIntelligence](../media/GDPR-Windows-Server-Overview/gdpr-intelligent-security-graph.png)

L’étendue des renseignements de Microsoft s’étend sur, littéralement, des milliards de points de données: milliard de 35messages analysés tous les mois, 1 milliard des clients sur l’entreprise et segments consommateur l’accès à 200 cloud services et les authentifications 14milliards effectuées tous les jours. Toutes ces données sont rassemblées à votre place par Microsoft pour créer un graphe sécurité Intelligent qui peuvent vous aider à protéger votre porte de manière dynamique pour rester protégé, rester productifs et répondre aux exigences de la PIBR.

#### <a name="detecting-attacks-and-forensic-investigation"></a>Détection des attaques et complète d’examen
Même les meilleures défenses de point de terminaison peuvent présenter des failles finalement, comme les cyberattaques deviennent plus sophistiquées et plus ciblés. Deux fonctionnalités peuvent servir à faciliter la détection violation potentielle - WindowsDefenderAdvancedThreatProtection (ATP) et MicrosoftAdvanced Threat Analytique (ATA).

Protection contre les menaces avancées (ATP) de Windows Defender vous aide à détecter, d’étudier et de répondre aux attaques avancées et les violations de données sur vos réseaux. Les types de divulgation de données le PIBR attend vous protéger par le biais de mesures de sécurité techniques pour garantir la confidentialité en cours, l’intégrité et disponibilité des données personnelles et les systèmes de traitement.

Parmi les avantages de WindowsDefenderATP sont les suivants:

- **Détection de la non détectés.** Capteurs intégrés dans le noyau du système d’exploitation, experts en sécurité Windows et optique unique aux ordinateurs virtuels et les signaux 1 milliard sur tous les services Microsoft.

- **Intégrée, et non rajoutée.** Sans agent, des performances élevées et un impact minimal, fonctionnant sur le cloud; gestion facile avec aucun déploiement. 

- **Volet unique pour la sécurité Windows.** Explorez les 6mois riche,-chronologie de l’ordinateur, unification des événements de sécurité à partir de WindowsDefenderATP, WindowsDefenderAntivirus et WindowsDefenderDeviceGuard.

- **Alimentation du graphique de Microsoft.** Tire parti de la sécurité d’Asset Intelligence MicrosoftGraph pour intégrer la détection et l’exploration abonnement Office 365 ATP, de retour de suivre et de répondre à des attaques.

En savoir plus sur [quelles sont les nouveautés dans l’aperçu de la mise à jour de WindowsDefenderATP créateurs](https://blogs.microsoft.com/microsoftsecure/2017/03/13/whats-new-in-the-windows-defender-atp-creators-update-preview/).

ATA est un produit local qui permet de détecter toute compromission d’identité dans une organisation. ATA peut capturer et analyser le trafic réseau pour l’authentification, d’autorisation et de protocoles (par exemple, Kerberos, DNS, RPC, NTLM et d’autres protocoles) de collecte d’informations. ATA utilise ces données pour créer un profil de comportement sur les utilisateurs et d’autres entités sur un réseau afin qu’il puisse détecter les anomalies et les modèles d’attaque connues. Le tableau suivant répertorie les types d’attaques détectées par ATA.


|Type d’attaque |Description |
|---------|---------|
|Attaques malveillantes |Ces attaques sont détectés en recherchant les attaques dans une liste connue de types d’attaques, notamment:<ul><li>Pass-the-Ticket (PtT)</li><li>Pass-the-Hash (PtH)</li><li>Passage supérieur-the-Hash</li><li>Falsifiée PAC (MS14-068)</li><li>Ticket de référence</li><li>Réplications malveillantes</li><li>Reconnaissance</li><li>En force brute</li><li>Exécution à distance</li></ul>Pour une liste complète des attaques malveillantes qui peuvent être détectés et leur description, voir [détecter quelle suspects activités peuvent ATA?](https://docs.microsoft.com/en-us/advanced-threat-analytics/understand-explore/ata-threats).|
|Comportement anormal |Ces attaques sont détectées à l’aide d’analyse comportementale et utilisent apprentissage identifier les activités douteuses, y compris de l’ordinateur:<ul><li>Connexions anormales</li><li>Menaces inconnues</li><li>Partage de mot de passe</li><li>Mouvement latéral</li></ul>|
|Problèmes de sécurité et les risques |Ces attaques sont détectés en examinant réseau actuel et la configuration du système, notamment:<ul><li>Approbation rompue</li><li>Protocoles faibles</li><li>Vulnérabilités de protocole connus</li></ul>|

Vous pouvez utiliser ATA pour détecter les personnes malveillantes tentant de compromettre les identités privilégiées. Pour plus d’informations sur le déploiement d’ATA, consultez les rubriques de planification, conception et de déployer dans le [documentation Analytique de menace avancée](https://docs.microsoft.com/en-us/advanced-threat-analytics/).

## <a name="related-content-for-associated-windows-server-2016-solutions"></a>Contenu associé pour des solutions Windows Server2016associées

- **WindowsDefenderAntivirus:** https://www.youtube.com/watch?v=P1aNEy09NaI et https://docs.microsoft.com/en-us/windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10

- **Windows Defender la Protection contre les menaces avancées:** https://www.youtube.com/watch?v=qxeGa3pxIwg et https://docs.microsoft.com/en-us/windows/threat-protection/windows-defender-atp/configure-server-endpoints-windows-defender-advanced-threat-protection

- **WindowsDefenderDeviceGuard:** https://www.youtube.com/watch?v=F-pTkesjkhI et https://docs.microsoft.com/en-us/windows/device-security/device-guard/device-guard-deployment-guide

- **WindowsDefenderCredentialGuard:** https://www.youtube.com/watch?v=F-pTkesjkhI et https://docs.microsoft.com/en-us/windows/access-protection/credential-guard/credential-guard

- **Protection du flux de contrôle:** https://msdn.microsoft.com/en-us/library/windows/desktop/mt637065(v=vs.85).aspx

- **Sécurité et Assurance:** https://docs.microsoft.com/en-us/windows-server/security/security-and-assurance

## <a name="disclaimer"></a>Exclusion de responsabilité
Cet article est un commentaire sur le PIBR, comme Microsoft l’interprète, à compter de la date de publication. Nous avons passé beaucoup de temps avec PIBR et espérons que nous avons réfléchies sur son objectif et la signification. Mais l’application de PIBR est hautement fait spécifique, et pas tous les aspects et l’interprétation des PIBR est correctement réglées.

Par conséquent, cet article est fourni à titre d’information uniquement et ne doit pas servir à comme conseiller juridique ou pour déterminer comment PIBR peut-être s’appliquer à vous et votre organisation. Nous vous encourageons à travailler avec un professionnel légalement qualifié pour discuter des PIBR, elle s’applique spécifiquement à votre organisation et découvrez comment meilleures garantir la conformité.

MicrosoftEXCLUT TOUTE GARANTIE, EXPRESSE, IMPLICITE OU LÉGALE, POUR LES INFORMATIONS CONTENUES DANS CET ARTICLE. Cet article est fourni «en tant que-est.» Informations et idées exprimées dans cet article, y compris les URL et autres références à des sites Internet, peuvent changer sans préavis.

Cet article ne fournit pas vous concède à toute propriété intellectuelle sur les produits Microsoft.  Vous pouvez copier et utiliser cet article interne, à des fins de référence uniquement.  

Publication septembre2017<br>
Version1.0<br>
© 2017 Microsoft. Tous droits réservés.


