---
ms.assetid: 9ee8a6cb-7550-46e2-9c11-78d0545c3a97
title: "Vue d’ensemble du contrôle d’accès dynamique"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5cf74042c9b511abb1fbeb88224dea0c7f2c8706
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="dynamic-access-control-overview"></a>Vue d’ensemble du contrôle d’accès dynamique

>S’applique à: Windows Server2012R2, Windows Server2012

Cette rubrique de présentation destinée aux professionnels de l’informatique décrit le contrôle d’accès dynamique et il est associé à des éléments qui ont été introduits dans Windows Server2012 et Windows8.  
  
Contrôle d’accès dynamique basé sur un domaine permet aux administrateurs d’appliquer des autorisations de contrôle d’accès et des restrictions basées sur les règles bien définies et peuvent inclure la sensibilité les ressources, la tâche ou le rôle de l’utilisateur et la configuration de l’appareil est utilisé pour accéder à ces ressources.  
  
Par exemple, un utilisateur peut avoir des autorisations différentes lorsqu’ils accèdent à une ressource à partir de leur ordinateur de bureau ou lorsqu’ils utilisent un ordinateur portable via un réseau privé virtuel. Ou accès peuvent être autorisé uniquement si un périphérique répond aux exigences de sécurité qui sont définies par les administrateurs réseau. Lorsque le contrôle d’accès dynamique est utilisé, les autorisations d’un utilisateur modifier dynamiquement sans intervention de l’administrateur si le travail ou le rôle de l’utilisateur change (entraînant des attributs de compte de l’utilisateur dans ADDS).  
  
Contrôle d’accès dynamique n’est pas prise en charge dans les systèmes d’exploitation Windows antérieurs à Windows Server2012 et Windows8. Lorsque le contrôle d’accès dynamique est configuré dans les environnements avec des versions prises en charge et non pris en charge de Windows, seules les versions prises en charge implémentent les modifications.  
  
Fonctionnalités et les concepts associés au contrôle d’accès dynamique sont les suivantes:  
  
-   [Règles d’accès centralisées](#BKMK_Rules)  
  
-   [Stratégies d’accès centralisées](#BKMK_Policies)  
  
-   [Revendications](#BKMK_Claims)  
  
-   [Expressions](#BKMK_Expressions2)  
  
-   [Autorisations proposées](#BKMK_Permissions2)  
  
### <a name="BKMK_Rules"></a>Règles d’accès centralisées  
Une règle d’accès central est une expression de règles d’autorisation qui peut inclure un ou plusieurs conditions pour des groupes d’utilisateurs, les revendications d’utilisateur, les revendications de périphérique et les propriétés de ressource. Plusieurs règles d’accès centralisées peuvent être combinées dans une stratégie d’accès centralisée.  
  
Si un ou plusieurs règles d’accès centralisées ont été définies pour un domaine, administrateurs de partage de fichiers peuvent correspondre à des règles spécifiques à des ressources et exigences professionnelles.  
  
### <a name="BKMK_Policies"></a>Stratégies d’accès centralisées  
Stratégies d’accès centralisées sont des stratégies d’autorisation qui comprennent des expressions conditionnelles. Par exemple, qu'une organisation a un besoin commercial à limiter l’accès aux informations d’identification personnelle (PII) dans les fichiers qu’au propriétaire du fichier et les membres du service ressources humaines (RH) qui sont autorisés à afficher les informations d’identification personnelle. Il s’agit d’une stratégie de l’organisation qui s’applique aux fichiers PII stockés sur les serveurs de fichiers dans l’organisation. Pour implémenter cette stratégie, une organisation doit être en mesure de:  
  
-   Identifier et marquer les fichiers qui contiennent les informations d’identification personnelle.  
  
-   Identifier le groupe de ressources humaines les membres qui sont autorisés à afficher les informations d’identification personnelle.  
  
-   Ajouter la stratégie d’accès centralisée à une règle d’accès central et appliquer la règle d’accès central à tous les fichiers qui contiennent les informations d’identification personnelle, où ils se trouvent entre les serveurs de fichiers dans l’organisation.  
  
Stratégies d’accès centralisées agissent comme parapluies de sécurité une organisation s’appliquent à ses serveurs. Ces stratégies complètent (mais ne remplacent pas) les stratégies d’accès local ou les listes de contrôle d’accès discrétionnaire (DACL) qui sont appliquées aux fichiers et dossiers.  
  
### <a name="BKMK_Claims"></a>Revendications  
Une revendication est un élément unique d’informations sur un utilisateur, un périphérique ou une ressource qui a été publié par un contrôleur de domaine. Titre de l’utilisateur, la classification de service d’un fichier ou de l’état d’intégrité d’un ordinateur sont des exemples de la revendication. Une entité peut impliquer plusieurs revendications, et n’importe quelle combinaison de revendications peut servir à autoriser l’accès aux ressources. Les types de revendications suivants sont disponibles dans les versions de Windows prises en charge:  
  
-   **Revendications d’utilisateur** attributs ActiveDirectory qui sont associés à un utilisateur spécifique.  
  
-   **Revendications de périphérique** attributs ActiveDirectory qui sont associés à un objet ordinateur spécifique.  
  
-   **Attributs de ressource** propriétés de ressources globales qui sont marquées pour une utilisation dans les décisions d’autorisation et publiées dans ActiveDirectory.  
  
Revendications permettent aux administrateurs d’effectuer des instructions de l’organisation ou entreprise précis sur les utilisateurs, appareils et ressources qui peuvent être incorporées dans les expressions, règles et stratégies.  
  
### <a name="BKMK_Expressions2"></a>Expressions  
Expressions conditionnelles sont une amélioration de la gestion du contrôle d’accès qui autorise ou refuse l’accès aux ressources uniquement lorsque certaines conditions sont remplies, par exemple, l’appartenance au groupe, emplacement ou l’état de sécurité de l’appareil. Les expressions sont gérées par le biais de la boîte de dialogue Paramètres de sécurité avancés de l’éditeur ACL ou l’éditeur de règles d’accès Central dans l’ActiveDirectory Administrative Center (ADAC).  
  
Expressions aider les administrateurs à gérer l’accès aux ressources sensibles avec les conditions dans les environnements d’entreprise plus en plus complexes.  
  
### <a name="BKMK_Permissions2"></a>Autorisations proposées  
Les autorisations proposées permettent à un administrateur plus précisément l’impact d’une modification éventuelle pour accéder aux paramètres de contrôle sans les modifier réellement.  
  
Anticipant l’accès effectif à une ressource vous pouvez mieux planifier et configurer des autorisations pour ces ressources avant d’implémenter ces modifications.  
  
## <a name="additional-changes"></a>Modifications supplémentaires  
Améliorations supplémentaires dans les versions prises en charge de Windows qui prennent en charge le contrôle d’accès dynamique:  
  
### <a name="support-in-the-kerberos-authentication-protocol-to-reliably-provide-user-claims-device-claims-and-device-groups"></a>Prendre en charge le protocole d’authentification Kerberos pour fournir de manière fiable les revendications d’utilisateur, les revendications de périphérique et les groupes de périphériques.  
Par défaut, les appareils exécutant les versions prises en charge de Windows sont en mesure de traiter les tickets Kerberos relevant du contrôle d’accès dynamique, ce qui incluent les données nécessaires pour l’authentification composée. Contrôleurs de domaine sont en mesure d’émettre et de répondre aux tickets Kerberos avec les informations d’authentification composées. Lorsqu’un domaine est configuré pour reconnaître le contrôle d’accès dynamique, les appareils reçoivent les revendications de contrôleurs de domaine lors de l’authentification initiale, et ils reçoivent les tickets de l’authentification composée lors de l’envoi des demandes de ticket de service. L’authentification composée génère un jeton d’accès qui contient l’identité de l’utilisateur et l’appareil sur les ressources qui reconnaissent le contrôle d’accès dynamique.  
  
### <a name="support-for-using-the-key-distribution-center-kdc-group-policy-setting-to-enable-dynamic-access-control-for-a-domain"></a>Prise en charge pour l’utilisation du paramètre de stratégie de groupe de Key Distribution Center (KDC) pour activer le contrôle d’accès dynamique pour un domaine.  
Chaque contrôleur de domaine doit avoir le même paramètre de stratégie du modèle d’administration, se trouve dans **contrôle d’accès dynamique ordinateur configuration administration\système\kdc\prendre en charge et le blindage Kerberos**.  
  
### <a name="support-for-using-the-key-distribution-center-kdc-group-policy-setting-to-enable-dynamic-access-control-for-a-domain"></a>Prise en charge pour l’utilisation du paramètre de stratégie de groupe de Key Distribution Center (KDC) pour activer le contrôle d’accès dynamique pour un domaine.  
Chaque contrôleur de domaine doit avoir le même paramètre de stratégie du modèle d’administration, se trouve dans **contrôle d’accès dynamique ordinateur configuration administration\système\kdc\prendre en charge et le blindage Kerberos**.  
  
### <a name="support-in-active-directory-to-store-user-and-device-claims-resource-properties-and-central-access-policy-objects"></a>Prise en charge dans ActiveDirectory pour stocker les revendications d’utilisateur et périphérique, les propriétés de ressource et objets de stratégie d’accès centralisée.  
  
### <a name="support-for-using-group-policy-to-deploy-central-access-policy-objects"></a>Prise en charge pour l’utilisation de la stratégie de groupe pour déployer des objets de stratégie d’accès centralisée.  
Le paramètre de stratégie de groupe suivant vous permet de déployer des objets de stratégie d’accès centralisée aux serveurs de fichiers dans votre organisation: **ordinateur Configuration ordinateur\Stratégies\Paramètres Windows\Paramètres sécurité\Système accès Fichiers\stratégie**.  
  
### <a name="support-for-claims-based-file-authorization-and-auditing-for-file-systems-by-using-group-policy-and-global-object-access-auditing"></a>Prise en charge de l’autorisation de fichiers basée sur les revendications et l’audit des systèmes de fichiers à l’aide de la stratégie de groupe et d’audit d’accès aux objets globaux  
Vous devez activer l’audit des stratégies accès centralisées intermédiaires auditer l’accès effectif d’une stratégie d’accès centralisée à l’aide des autorisations proposées. Vous configurez ce paramètre pour l’ordinateur sous **Configuration avancée de stratégie d’Audit** dans les **les paramètres de sécurité** d’un objet de stratégie de groupe (GPO). Après avoir configuré le paramètre de sécurité dans l’objet de stratégie de groupe, vous pouvez déployer l’objet de stratégie de groupe aux ordinateurs de votre réseau.  
  
### <a name="support-for-transforming-or-filtering-claim-policy-objects-that-traverse-active-directory-forest-trusts"></a>Prise en charge de la transformation ou du filtrage des objets de stratégie de revendication qui parcourent les approbations de forêt ActiveDirectory  
Vous pouvez filtrer ou transformer les revendications entrantes et sortantes qui parcourent une approbation de forêt. Il existe trois principaux scénarios de filtrage et de transformation des revendications:  
  
-   **Filtrage basé sur une valeur** filtres peuvent être basés sur la valeur de la revendication. Cela permet de la forêt approuvée empêcher que les revendications avec certaines valeurs soient envoyées à la forêt d’approbation. Contrôleurs de domaine dans les forêts de confiance peuvent utiliser le filtrage basé sur une valeur pour vous protéger contre une attaque par élévation de privilèges en filtrant les revendications entrantes ayant des valeurs spécifiques de la forêt approuvée.  
  
-   **Filtrage basé sur le type de revendication** filtres sont basés sur le type de revendication, plutôt que la valeur de la revendication. Vous identifiez le type de revendication par le nom de la revendication. Vous pouvez utiliser ce type basé le filtrage dans la forêt approuvée, et il empêche Windows d’envoyer des revendications qui divulguent des informations à la forêt d’approbation.  
  
-   **Transformation basée sur le type de revendication**: manipule une revendication avant de l’envoyer à la cible prévue. Vous utilisez la transformation basée sur le type de revendication dans la forêt approuvée pour généraliser une revendication connue qui contient des informations spécifiques. Vous pouvez utiliser les transformations pour généraliser le type de revendication, la valeur de revendication ou les deux.  
  
## <a name="software-requirements"></a>Configuration logicielle requise  
Étant donné que les revendications et l’authentification composée pour le contrôle d’accès dynamique nécessitent des extensions d’authentification Kerberos, n’importe quel domaine qui prend en charge le contrôle d’accès dynamique doit comporter suffisamment de contrôleurs de domaine exécutant les versions prises en charge de Windows pour prendre en charge l’authentification à partir de clients Kerberos avec contrôle d’accès dynamique. Par défaut, les périphériques doivent utiliser des contrôleurs de domaine dans d’autres sites. Si aucun ces contrôleurs de domaine ne sont disponibles, l’authentification échoue. Par conséquent, vous devez prendre en charge une des conditions suivantes:  
  
-   Chaque domaine qui prend en charge le contrôle d’accès dynamique doit comporter suffisamment de contrôleurs de domaine exécutant les versions prises en charge de Windows Server pour prendre en charge l’authentification à partir de tous les appareils exécutant les versions prises en charge de Windows ou Windows Server.  
  
-   Les appareils exécutant les versions prises en charge de Windows ou qui ne protègent pas les ressources à l’aide de revendications ou une identité composée, doit désactiver la prise en charge du protocole Kerberos pour le contrôle d’accès dynamique.  
  
Pour les domaines qui prennent en charge les revendications d’utilisateur, chaque contrôleur de domaine exécutant les versions prises en charge de Windows server doit être configuré avec le paramètre approprié pour prendre en charge les revendications et l’authentification composée et pour fournir le blindage Kerberos. Configurer les paramètres de la stratégie de modèle d’administration KDC comme suit:  
  
-   **Toujours fournir des revendications** utiliser ce paramètre si tous les contrôleurs de domaine exécutent les versions prises en charge de Windows Server. En outre, définissez le niveau fonctionnel du domaine vers Windows Server2012 ou version ultérieure.  
  
-   **Prise en charge** lorsque vous utilisez ce paramètre, les contrôleurs de domaine pour vous assurer que le nombre de contrôleurs de domaine exécutant les versions prises en charge de Windows Server est suffisant pour le nombre d’ordinateurs clients qui doivent accéder aux ressources protégées par le contrôle d’accès dynamique.  
  
Si le domaine de l’utilisateur et le domaine de serveur de fichiers se trouvent dans des forêts différentes, tous les contrôleurs de domaine racine de la forêt du serveur de fichiers doivent être définies au niveau fonctionnel supérieure ou Windows Server2012.  
  
Si les clients ne reconnaissent pas de contrôle d’accès dynamique, il doit être une relation d’approbation bidirectionnelle entre les deux forêts.  
  
Si les revendications sont transformées à leur une forêt, tous les contrôleurs de domaine racine de la forêt de l’utilisateur doivent être définis au niveau fonctionnel supérieure ou Windows Server2012.  
  
Un serveur de fichiers exécutant Windows Server2012 ou Windows Server2012R2 doit avoir un paramètre de stratégie de groupe qui spécifie s’il doit obtenir des revendications d’utilisateur pour les jetons d’utilisateur qui ne transmettent pas de revendications. Ce paramètre est défini par défaut à **automatique**, ce qui aboutit à ce paramètre de stratégie de groupe peut être activée **sur** s’il existe une stratégie centralisée contenant des revendications utilisateur ou périphérique pour ce serveur de fichiers. Si le serveur de fichiers contient des listes DACL qui incluent des revendications d’utilisateur, vous devez définir cette stratégie de groupe **sur** afin que le serveur qu’il doit pour demander des revendications pour les utilisateurs qui ne fournissent pas de revendications lorsqu’ils accèdent à du serveur.  
  
## <a name="additional-resource"></a>Ressources supplémentaires  
Pour plus d’informations sur l’implémentation des solutions basées sur cette technologie, consultez [contrôle d’accès dynamique: vue d’ensemble du scénario](Dynamic-Access-Control--Scenario-Overview.md).  
  


