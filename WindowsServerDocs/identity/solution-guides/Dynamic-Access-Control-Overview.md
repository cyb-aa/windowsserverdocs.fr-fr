---
ms.assetid: 9ee8a6cb-7550-46e2-9c11-78d0545c3a97
title: Vue d’ensemble du contrôle d’accès dynamique
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 2374e2c8a1efb204dbae1ee633bc5ee41d049d57
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861172"
---
# <a name="dynamic-access-control-overview"></a>Vue d’ensemble du contrôle d’accès dynamique

>S’applique à : Windows Server 2012 R2, Windows Server 2012

Cette rubrique de présentation à destination des professionnels de l’informatique décrit la fonctionnalité de contrôle d’accès dynamique et les éléments associés, qui ont été inaugurés dans Windows Server 2012 et Windows 8.  
  
Le contrôle d’accès dynamique basé sur les domaines permet aux administrateurs d’établir des autorisations et des restrictions de contrôle d’accès fondées sur des règles précises. Ces règles peuvent être définies en fonction du niveau de confidentialité des ressources, du travail ou du rôle de l’utilisateur et de la configuration du périphérique utilisé pour accéder aux ressources.  
  
Par exemple, un utilisateur donné peut disposer d’autorisations distinctes selon qu’il accède à une ressource à partir de son ordinateur de bureau ou qu’il utilise un ordinateur portable via un réseau privé virtuel. Par ailleurs, l’accès peut être autorisé à la condition que l’appareil soit conforme aux exigences de sécurité définies par les administrateurs réseau. Lorsque des Access Control dynamiques sont utilisées, les autorisations d’un utilisateur changent de manière dynamique sans intervention de l’administrateur si le travail ou le rôle de l’utilisateur change (ce qui entraîne des modifications des attributs de compte de l’utilisateur dans AD DS).  
  
Le contrôle d’accès dynamique n’est pas pris en charge dans les systèmes d’exploitation Windows antérieurs à Windows Server 2012 et Windows 8. Quand le contrôle d’accès dynamique est configuré dans un environnement constitué à la fois de versions prises en charge et de versions non prises de Windows, seules les versions prises en charge implémentent les modifications.  
  
Les fonctionnalités et les concepts associés au contrôle d’accès dynamique sont les suivants :  
  
-   [Règles d’accès centralisées](#BKMK_Rules)  
  
-   [Stratégies d’accès centralisées](#BKMK_Policies)  
  
-   [Légitimité](#BKMK_Claims)  
  
-   [Manifestations](#BKMK_Expressions2)  
  
-   [Autorisations proposées](#BKMK_Permissions2)  
  
### <a name="central-access-rules"></a><a name="BKMK_Rules"></a>Règles d’accès centralisées  
Une règle d’accès centralisée est une expression de règles d’autorisation, qui comporte une ou plusieurs conditions pour des groupes d’utilisateurs, des revendications d’utilisateur ou de périphérique et des propriétés de ressources. Plusieurs règles d’accès centralisées peuvent être combinées dans une stratégie d’accès centralisée.  
  
Si une ou plusieurs règles d’accès centralisées ont été définies pour un domaine, les administrateurs des partages de fichiers peuvent associer des règles particulières à des ressources et des exigences professionnelles spécifiques.  
  
### <a name="central-access-policies"></a><a name="BKMK_Policies"></a>Stratégies d’accès centralisées  
Les stratégies d’accès centralisées sont des stratégies d’autorisation qui comportent des expressions conditionnelles. Supposons, par exemple, qu’une organisation a besoin de limiter l’accès aux informations d’identification personnelle (PII) dans les fichiers uniquement au propriétaire du fichier et aux membres du service des ressources humaines (RH) autorisés à consulter les informations d’identification personnelle. Il s’agit d’une stratégie à l’échelle de l’organisation qui s’applique aux fichiers PII, où qu’ils résident sur les serveurs de fichiers de l’organisation. Pour appliquer cette stratégie, l’organisation doit :  
  
-   identifier et marquer les fichiers qui contiennent les informations d’identification personnelle ;  
  
-   identifier le groupe d’employés du service des ressources humaines habilités à consulter ce type d’information ;  
  
-   ajouter la stratégie d’accès centralisée à une règle d’accès centralisée, puis appliquer cette règle à tous les fichiers qui contiennent les informations d’identification personnelle, où qu’elles se trouvent sur les serveurs de fichiers de l’organisation.  
  
Les stratégies d’accès centralisées agissent comme un processus de sécurité « parapluie » que l’organisation déploie sur tous ses serveurs. Ces stratégies complètent (mais ne remplacent pas) les stratégies d’accès locales ou les listes de contrôle d’accès discrétionnaire (DACL, Discretionary Access Control List) qui s’appliquent aux fichiers et dossiers.  
  
### <a name="claims"></a><a name="BKMK_Claims"></a>Légitimité  
Une revendication est un élément d’information unique relatif à un utilisateur, un périphérique ou une ressource qui a été publié par un contrôleur de domaine. Le titre de l’utilisateur, la classification du département d’un fichier ou l’état d’intégrité d’un ordinateur sont des exemples valides d’une revendication. Une entité peut faire l’objet de plusieurs revendications, et celles-ci peuvent être combinées pour autoriser l’accès aux ressources. Les types de revendication décrits ci-dessous sont disponibles dans les versions prises en charge de Windows :  
  
-   **Revendications d’utilisateur** : attributs Active Directory associés à un utilisateur spécifique.  
  
-   **Revendications de périphérique** : attributs Active Directory associés à un objet ordinateur spécifique.  
  
-   **Attributs de ressources** : propriétés de ressources globales marquées pour une utilisation dans les décisions d’autorisation et publiées dans Active Directory.  
  
Avec les revendications, les administrateurs peuvent définir de façon précise, à l’échelle de l’entreprise ou de l’organisation, les utilisateurs, appareils et ressources à intégrer dans les expressions, règles et stratégies.  
  
### <a name="expressions"></a><a name="BKMK_Expressions2"></a>Manifestations  
Les expressions conditionnelles améliorent la gestion du contrôle d’accès qui autorise ou refuse l’accès aux ressources selon que certaines conditions sont réunies ou pas, par exemple, l’appartenance à un groupe, l’emplacement ou l’état de la sécurité du périphérique. Les expressions sont gérées par le biais de la boîte de dialogue Paramètres de sécurité avancés dans l’Éditeur ACL, ou de l’Éditeur de règles d’accès centralisées dans le Centre d’administration Active Directory (ADAC).  
  
Avec les expressions, les administrateurs gèrent plus facilement l’accès aux ressources sensibles, car ils peuvent définir et modifier les conditions en fonction des besoins toujours plus complexes de leur environnement de travail.  
  
### <a name="proposed-permissions"></a><a name="BKMK_Permissions2"></a>Autorisations proposées  
Avec les autorisations proposées, un administrateur peut analyser plus précisément l’impact d’une modification éventuelle des paramètres de contrôle d’accès sans les modifier réellement.  
  
En anticipant l’accès effectif aux ressources, vous pouvez mieux planifier et configurer les autorisations pour ces ressources avant de procéder aux modifications.  
  
## <a name="additional-changes"></a>Autres modifications  
D’autres améliorations ont été apportées aux versions prises en charge de Windows compatibles avec le contrôle d’accès dynamique, à savoir :  
  
### <a name="support-in-the-kerberos-authentication-protocol-to-reliably-provide-user-claims-device-claims-and-device-groups"></a>Prise en charge du protocole d’authentification Kerberos pour fournir de manière fiable les revendications d’utilisateur, les revendications de périphérique et les groupes d’appareils.  
Par défaut, les appareils exécutant l’une des versions prises en charge de Windows peuvent traiter les tickets Kerberos relevant du contrôle d’accès dynamique, ce qui inclut les données nécessaires à l’authentification composée. Les contrôleurs de domaine peuvent émettre des tickets Kerberos avec des informations liées à l’authentification composée, et répondre à ces tickets. Quand un domaine est configuré pour reconnaître le contrôle d’accès dynamique, les appareils reçoivent les revendications des contrôleurs de domaine pendant l’authentification initiale, puis ils reçoivent les tickets de l’authentification composée pendant l’envoi des demandes de ticket de service. L’authentification composée génère un jeton d’accès qui indique l’identité de l’utilisateur et du périphérique utilisé pour accéder aux ressources qui reconnaissent le contrôle d’accès dynamique.  
  
### <a name="support-for-using-the-key-distribution-center-kdc-group-policy-setting-to-enable-dynamic-access-control-for-a-domain"></a>Prise en charge de l’utilisation du paramètre de stratégie de groupe du centre de distribution de clés (KDC) pour activer le contrôle d’accès dynamique sur un domaine.  
Chaque contrôleur de domaine doit avoir le même paramètre de stratégie de modèles d’administration (sous **Configuration ordinateur\Stratégies\Modèles d’administration\Système\KDC\Prendre en charge le contrôle d’accès dynamique et le blindage Kerberos**).  
  
### <a name="support-for-using-the-key-distribution-center-kdc-group-policy-setting-to-enable-dynamic-access-control-for-a-domain"></a>Prise en charge de l’utilisation du paramètre de stratégie de groupe du centre de distribution de clés (KDC) pour activer le contrôle d’accès dynamique sur un domaine.  
Chaque contrôleur de domaine doit avoir le même paramètre de stratégie de modèles d’administration (sous **Configuration ordinateur\Stratégies\Modèles d’administration\Système\KDC\Prendre en charge le contrôle d’accès dynamique et le blindage Kerberos**).  
  
### <a name="support-in-active-directory-to-store-user-and-device-claims-resource-properties-and-central-access-policy-objects"></a>Prise en charge dans Active Directory du stockage des revendications d’utilisateur et d’appareil, des propriétés de ressources ainsi que des objets de stratégie d’accès centralisée.  
  
### <a name="support-for-using-group-policy-to-deploy-central-access-policy-objects"></a>Prise en charge de l’utilisation d’une stratégie de groupe pour déployer les objets de stratégie d’accès centralisée.  
Le paramètre de stratégie de groupe suivant vous permet de déployer des objets de stratégie d’accès centralisée sur les serveurs de fichiers de votre organisation : **Configuration ordinateur\Stratégies\Paramètres Windows\Paramètres de sécurité\Système de fichiers\Stratégie d’accès centralisée**.  
  
### <a name="support-for-claims-based-file-authorization-and-auditing-for-file-systems-by-using-group-policy-and-global-object-access-auditing"></a>Prise en charge de l’autorisation de fichiers basée sur les revendications et de l’audit des systèmes de fichiers avec la stratégie de groupe et l’audit de l’accès global aux objets  
Vous devez activer l’audit des stratégies d’accès centralisées intermédiaires pour pouvoir évaluer l’accès effectif d’une stratégie d’accès centralisée à l’aide des autorisations proposées. Vous pouvez configurer ce paramètre de l’ordinateur sous **Configuration avancée de la stratégie d’audit** dans les **Paramètres de sécurité** d’un objet de stratégie de groupe (GPO). Une fois que vous avez configuré le paramètre de sécurité dans le GPO, vous pouvez déployer ce dernier sur les ordinateurs de votre réseau.  
  
### <a name="support-for-transforming-or-filtering-claim-policy-objects-that-traverse-active-directory-forest-trusts"></a>Prise en charge de la transformation ou du filtrage des objets de stratégie de revendication qui parcourent les approbations de forêt Active Directory  
Vous pouvez filtrer ou transformer les revendications entrantes et sortantes qui parcourent une approbation de forêt. Il existe trois principaux scénarios de filtrage et de transformation des revendications :  
  
-   **Filtrage basé sur les valeurs** : les filtres peuvent être basés sur la valeur d’une revendication. Dans ce cas, la forêt approuvée peut empêcher que les revendications avec certaines valeurs soient envoyées à la forêt d’approbation. Les contrôleurs de domaine situés dans les forêts d’approbation peuvent utiliser un filtrage basé sur les valeurs pour empêcher une attaque par élévation de privilège en filtrant les revendications entrantes ayant des valeurs spécifiques de la forêt approuvée.  
  
-   **Filtrage basé sur le type de la revendication** : les filtres sont basés sur le type de la revendication et non sur sa valeur. C’est le nom de la revendication qui vous permet d’identifier son type. Vous pouvez utiliser ce type de filtrage dans la forêt approuvée pour empêcher Windows d’envoyer des revendications qui divulguent des informations à la forêt d’approbation.  
  
-   **Transformation basée sur le type de la revendication** : manipule une revendication avant de l’envoyer à la cible prévue. Vous pouvez utiliser la transformation basée sur le type des revendications dans la forêt approuvée pour généraliser une revendication connue qui contient des informations spécifiques. Les transformations permettent de généraliser le type et/ou la valeur de la revendication.  
  
## <a name="software-requirements"></a>Configuration logicielle requise  
Les revendications et l’authentification composée pour le contrôle d’accès dynamique nécessitent des extensions d’authentification Kerberos. De ce fait, les domaines qui prennent en charge le contrôle d’accès dynamique doivent avoir suffisamment de contrôleurs de domaine exécutant les versions prises en charge de Windows pour assurer la prise en charge des demandes d’authentification provenant de clients Kerberos avec contrôle d’accès dynamique. Par défaut, les appareils doivent utiliser des contrôleurs de domaine d’autres sites. S’il n’existe pas de contrôleurs de domaine avec cette configuration, l’authentification échoue. En conséquence, vous devez prendre en charge l’une des situations suivantes :  
  
-   Chaque domaine qui prend en charge le contrôle d’accès dynamique doit avoir suffisamment de contrôleurs de domaine exécutant les versions prises en charge de Windows Server pour assurer la prise en charge des demandes d’authentification de tous les appareils exécutant les versions prises en charge de Windows ou Windows Server.  
  
-   Sur les appareils exécutant les versions prises en charge de Windows ou qui ne protègent pas les ressources au moyen de revendications ou d’une identité composée, la prise en charge du protocole Kerberos pour le contrôle d’accès dynamique doit être désactivée.  
  
Dans le cas des domaines qui prennent en charge les revendications d’utilisateur, chaque contrôleur de domaine exécutant les versions prises en charge de Windows Server doit être configuré de manière appropriée pour prendre en charge les revendications et l’authentification composée, et fournir le blindage Kerberos. Configurez les paramètres de la stratégie de modèles d’administration dans le centre de distribution de clés comme expliqué ci-dessous :  
  
-   **Toujours fournir des revendications** : utilisez ce paramètre si tous les contrôleurs de domaine exécutent les versions prises en charge de Windows Server. Par ailleurs, configurez le domaine au niveau fonctionnel Windows Server 2012 ou supérieur.  
  
-   **Pris en charge** : si vous utilisez ce paramètre, vérifiez que les contrôleurs de domaine exécutant les versions prises en charge de Windows Server sont en nombre suffisant par rapport au nombre d’ordinateurs clients ayant besoin d’accéder aux ressources protégées par le contrôle d’accès dynamique.  
  
Si le domaine de l’utilisateur et le domaine du serveur de fichiers se trouvent dans des forêts différentes, tous les contrôleurs de domaine dans la racine de la forêt du serveur de fichiers doivent être définis au niveau fonctionnel de Windows Server 2012 ou supérieur.  
  
Si les clients ne prennent pas en charge le contrôle d’accès dynamique, vous devez définir une relation d’approbation bidirectionnelle entre les deux forêts.  
  
Si les revendications sont transformées lors de la sortie d’une forêt, tous les contrôleurs de domaine dans la racine de la forêt de l’utilisateur doivent être définis au niveau fonctionnel de Windows Server 2012 ou supérieur.  
  
Un serveur de fichiers exécutant Windows Server 2012 ou Windows Server 2012 R2 doit avoir un paramètre de stratégie de groupe qui spécifie s’il doit obtenir des revendications d’utilisateur pour les jetons d’utilisateur qui ne transmettent pas de revendications. La valeur par défaut de ce paramètre de stratégie étant **Automatique**, il prend la valeur **Activé** si une stratégie centralisée contenant des revendications d’utilisateur ou de périphérique a été définie pour ce serveur de fichiers. Si le serveur de fichiers comporte des listes de contrôle d’accès discrétionnaire (DACL) avec des revendications d’utilisateur, vous devez définir ce paramètre de stratégie sur **Activé** afin d’indiquer au serveur qu’il doit demander des revendications pour les utilisateurs qui n’en fournissent pas lorsqu’ils accèdent au serveur.  
  
## <a name="additional-resource"></a>Ressource supplémentaire  
Pour plus d’informations sur l’implémentation de solutions basées sur cette technologie, consultez [Access Control dynamique : vue d’ensemble des scénarios](Dynamic-Access-Control--Scenario-Overview.md).  
  


