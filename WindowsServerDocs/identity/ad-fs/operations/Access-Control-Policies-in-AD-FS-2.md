---
ms.assetid: ''
title: Stratégies de Access Control client dans Services ADFS 2,0
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b0d6133a6fb43b8624dc1329db632fb5dd4aa070
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358454"
---
# <a name="client-access-control-policies-in-ad-fs-20"></a>Stratégies de Access Control client dans AD FS 2,0
Les stratégies d’accès client dans Services ADFS 2,0 vous permettent de restreindre ou d’accorder aux utilisateurs l’accès aux ressources.  Ce document explique comment activer les stratégies d’accès client dans AD FS 2,0 et comment configurer les scénarios les plus courants.

## <a name="enabling-client-access-policy-in-ad-fs-20"></a>Activation de la stratégie d’accès client dans AD FS 2,0

Pour activer la stratégie d’accès client, suivez les étapes ci-dessous.

### <a name="step-1-install-the-update-rollup-2-for-ad-fs-20-package-on-your-ad-fs-servers"></a>Étape 1 : Installer la mise à jour cumulative 2 pour le package AD FS 2,0 sur vos serveurs AD FS

Téléchargez le package [de correctif cumulatif 2 pour services ADFS (AD FS) 2,0](https://support.microsoft.com/en-us/help/2681584/description-of-update-rollup-2-for-active-directory-federation-services-ad-fs-2.0) et installez-le sur tous les serveurs proxys de Fédération et de serveur de Fédération.

### <a name="step-2-add-five-claim-rules-to-the-active-directory-claims-provider-trust"></a>Étape 2 : Ajouter cinq règles de revendication à l’Active Directory approbation de fournisseur de revendications

Une fois que le correctif cumulatif 2 a été installé sur tous les serveurs et proxys de AD FS, utilisez la procédure suivante pour ajouter un ensemble de règles de revendication qui rend les nouveaux types de revendication disponibles pour le moteur de stratégie.

Pour ce faire, vous allez ajouter cinq règles de transformation d’acceptation pour chacun des nouveaux types de revendication de contexte de requête à l’aide de la procédure suivante.

Sur l’Active Directory approbation de fournisseur de revendications, créez une nouvelle règle de transformation d’acceptation pour passer en revue chacun des nouveaux types de revendication de contexte de demande.

#### <a name="to-add-a-claim-rule-to-the-active-directory-claims-provider-trust-for-each-of-the-five-context-claim-types"></a>Pour ajouter une règle de revendication à l’Active Directory approbation de fournisseur de revendications pour chacun des cinq types de revendications de contexte :


1. Cliquez sur Démarrer, pointez sur programmes, sur outils d’administration, puis cliquez sur AD FS gestion de 2,0.
2. Dans l’arborescence de la console, sous AD FS 2.0 \ relations d’approbation, cliquez sur approbations de fournisseur de revendications, cliquez avec le bouton droit sur Active Directory, puis cliquez sur modifier les règles de revendication.
3. Dans la boîte de dialogue Modifier les règles de revendication, sélectionnez l’onglet Règles de transformation d’acceptation, puis cliquez sur Ajouter une règle pour démarrer l’Assistant règle.
4. Dans la page Sélectionner le modèle de règle, sous modèle de règle de revendication, sélectionnez transférer ou filtrer une revendication entrante dans la liste, puis cliquez sur suivant.
5. Dans la page Configurer la règle, sous nom de la règle de revendication, tapez le nom d’affichage de cette règle. dans type de revendication entrante, tapez l’URL de type de revendication suivante, puis sélectionnez transmettre toutes les valeurs de revendication.</br>
        `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`</br>
6. Pour vérifier la règle, sélectionnez-la dans la liste et cliquez sur modifier la règle, puis cliquez sur Afficher la langue de la règle. Le langage de règle de revendication doit se présenter comme suit :`c:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip"] => issue(claim = c);`
7. Cliquez sur Terminer.
8. Dans la boîte de dialogue Modifier les règles de revendication, cliquez sur OK pour enregistrer les règles.
9. Répétez les étapes 2 à 6 pour créer une règle de revendication supplémentaire pour chacun des quatre types de revendication restants indiqués ci-dessous jusqu’à ce que les cinq règles aient été créées.

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`


~~~
`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`
~~~

### <a name="step-3-update-the-microsoft-office-365-identity-platform-relying-party-trust"></a>Étape 3 : Mettre à jour l’approbation de partie de confiance Microsoft Office 365 Identity Platform

Choisissez l’un des exemples de scénarios ci-dessous pour configurer les règles de revendication sur l’approbation de partie de confiance Microsoft Office 365 Identity qui répond le mieux aux besoins de votre organisation.

## <a name="client-access-policy-scenarios-for-ad-fs-20"></a>Scénarios de stratégie d’accès client pour AD FS 2,0
Les sections suivantes décrivent les scénarios qui existent pour AD FS 2,0

### <a name="scenario-1-block-all-external-access-to-office-365"></a>Scénario 1: Bloquer tout accès externe à Office 365

Ce scénario de stratégie d’accès client autorise l’accès à partir de tous les clients internes et bloque tous les clients externes en fonction de l’adresse IP du client externe. L’ensemble de règles construit sur la règle d’autorisation d’émission par défaut autorise l’accès à tous les utilisateurs. Vous pouvez utiliser la procédure suivante pour ajouter une règle d’autorisation d’émission à l’approbation de partie de confiance Office 365.

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>Pour créer une règle pour bloquer tout accès externe à Office 365



1. Cliquez sur Démarrer, pointez sur programmes, sur outils d’administration, puis cliquez sur AD FS gestion de 2,0.
2. Dans l’arborescence de la console, sous AD FS 2.0 \ relations d’approbation, cliquez sur approbations de la partie de confiance, cliquez avec le bouton droit sur le Microsoft Office 365 identité de la plateforme d’identité, puis cliquez sur modifier les règles de revendication. 
3. Dans la boîte de dialogue Modifier les règles de revendication, sélectionnez l’onglet Règles d’autorisation d’émission, puis cliquez sur Ajouter une règle pour démarrer l’Assistant règle de revendication.
4. Dans la page Sélectionner un modèle de règle, sous modèle de règle de revendication, sélectionnez Envoyer des revendications à l’aide d’une règle personnalisée, puis cliquez sur suivant.
5. Dans la page Configurer la règle, sous nom de la règle de revendication, tapez le nom complet de cette règle. Sous règle personnalisée, tapez ou collez la syntaxe de langage de règle de revendication suivante :`exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");` 
6. Cliquez sur Terminer. Vérifiez que la nouvelle règle s’affiche immédiatement sous la règle autoriser l’accès à tous les utilisateurs dans la liste règles d’autorisation d’émission.
7. Pour enregistrer la règle, dans la boîte de dialogue Modifier les règles de revendication, cliquez sur OK.

>[!NOTE]
>Vous devrez remplacer la valeur ci-dessus pour « Regex adresse IP publique » par une expression IP valide. Pour plus d’informations, consultez génération de l’expression de plage d’adresses IP.


### <a name="scenario-2-block-all-external-access-to-office-365-except-exchange-activesync"></a>Scénario 2 : Bloquer tout accès externe à Office 365, à l’exception d’Exchange ActiveSync

L’exemple suivant autorise l’accès à toutes les applications Office 365, y compris Exchange Online, à partir des clients internes, y compris Outlook. Il bloque l’accès à partir de clients résidant en dehors du réseau d’entreprise, comme indiqué par l’adresse IP du client, à l’exception des clients Exchange ActiveSync tels que les téléphones intelligents. L’ensemble de règles s’appuie sur la règle d’autorisation d’émission par défaut intitulée autoriser l’accès à tous les utilisateurs. Procédez comme suit pour ajouter une règle d’autorisation d’émission à l’approbation de la partie de confiance Office 365 à l’aide de l’Assistant règle de revendication :

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>Pour créer une règle pour bloquer tout accès externe à Office 365



1. Cliquez sur Démarrer, pointez sur programmes, sur outils d’administration, puis cliquez sur AD FS gestion de 2,0.
2. Dans l’arborescence de la console, sous AD FS 2.0 \ relations d’approbation, cliquez sur approbations de la partie de confiance, cliquez avec le bouton droit sur le Microsoft Office 365 identité de la plateforme d’identité, puis cliquez sur modifier les règles de revendication. 
3. Dans la boîte de dialogue Modifier les règles de revendication, sélectionnez l’onglet Règles d’autorisation d’émission, puis cliquez sur Ajouter une règle pour démarrer l’Assistant règle de revendication.
4. Dans la page Sélectionner un modèle de règle, sous modèle de règle de revendication, sélectionnez Envoyer des revendications à l’aide d’une règle personnalisée, puis cliquez sur suivant.
5. Dans la page Configurer la règle, sous nom de la règle de revendication, tapez le nom complet de cette règle. Sous règle personnalisée, tapez ou collez la syntaxe de langage de règle de revendication suivante :`exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.Autodiscover"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.ActiveSync"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Cliquez sur Terminer. Vérifiez que la nouvelle règle s’affiche immédiatement sous la règle autoriser l’accès à tous les utilisateurs dans la liste règles d’autorisation d’émission.
7. Pour enregistrer la règle, dans la boîte de dialogue Modifier les règles de revendication, cliquez sur OK.

>[!NOTE]
>Vous devrez remplacer la valeur ci-dessus pour « Regex adresse IP publique » par une expression IP valide. Pour plus d’informations, consultez génération de l’expression de plage d’adresses IP.

### <a name="scenario-3-block-all-external-access-to-office-365-except-browser-based-applications"></a>Scénario 3 : Bloquer tout accès externe à Office 365 à l’exception des applications basées sur un navigateur

L’ensemble de règles s’appuie sur la règle d’autorisation d’émission par défaut intitulée autoriser l’accès à tous les utilisateurs. Procédez comme suit pour ajouter une règle d’autorisation d’émission à l’approbation de la partie de confiance Microsoft Office 365 Identity Platform à l’aide de l’Assistant règle de revendication :

>[!NOTE]
>Ce scénario n’est pas pris en charge avec un proxy tiers en raison de limitations sur les en-têtes de stratégie d’accès client avec des demandes passives (basées sur le Web).

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>Pour créer une règle pour bloquer tout accès externe à Office 365 à l’exception des applications basées sur un navigateur



1. Cliquez sur Démarrer, pointez sur programmes, sur outils d’administration, puis cliquez sur AD FS gestion de 2,0.
2. Dans l’arborescence de la console, sous AD FS 2.0 \ relations d’approbation, cliquez sur approbations de la partie de confiance, cliquez avec le bouton droit sur le Microsoft Office 365 identité de la plateforme d’identité, puis cliquez sur modifier les règles de revendication. 
3. Dans la boîte de dialogue Modifier les règles de revendication, sélectionnez l’onglet Règles d’autorisation d’émission, puis cliquez sur Ajouter une règle pour démarrer l’Assistant règle de revendication.
4. Dans la page Sélectionner un modèle de règle, sous modèle de règle de revendication, sélectionnez Envoyer des revendications à l’aide d’une règle personnalisée, puis cliquez sur suivant.
5. Dans la page Configurer la règle, sous nom de la règle de revendication, tapez le nom complet de cette règle. Sous règle personnalisée, tapez ou collez la syntaxe de langage de règle de revendication suivante :`exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value == "/adfs/ls/"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Cliquez sur Terminer. Vérifiez que la nouvelle règle s’affiche immédiatement sous la règle autoriser l’accès à tous les utilisateurs dans la liste règles d’autorisation d’émission.
7. Pour enregistrer la règle, dans la boîte de dialogue Modifier les règles de revendication, cliquez sur OK.

### <a name="scenario-4-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>Scénario 4 : Bloquer tout accès externe à Office 365 pour les groupes de Active Directory désignés

L’exemple suivant active l’accès à partir de clients internes en fonction de l’adresse IP. Il bloque l’accès à partir de clients résidant en dehors du réseau d’entreprise qui ont une adresse IP de client externe, à l’exception des individus d’un groupe de Active Directory spécifié. l’ensemble de règles s’appuie sur la règle d’autorisation d’émission par défaut intitulée autoriser l’accès à Tous les utilisateurs. Procédez comme suit pour ajouter une règle d’autorisation d’émission à l’approbation de la partie de confiance Microsoft Office 365 Identity Platform à l’aide de l’Assistant règle de revendication :

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>Pour créer une règle qui bloque tous les accès externes à Office 365 pour les groupes de Active Directory désignés



1. Cliquez sur Démarrer, pointez sur programmes, sur outils d’administration, puis cliquez sur AD FS gestion de 2,0.
2. Dans l’arborescence de la console, sous AD FS 2.0 \ relations d’approbation, cliquez sur approbations de la partie de confiance, cliquez avec le bouton droit sur le Microsoft Office 365 identité de la plateforme d’identité, puis cliquez sur modifier les règles de revendication. 
3. Dans la boîte de dialogue Modifier les règles de revendication, sélectionnez l’onglet Règles d’autorisation d’émission, puis cliquez sur Ajouter une règle pour démarrer l’Assistant règle de revendication.
4. Dans la page Sélectionner un modèle de règle, sous modèle de règle de revendication, sélectionnez Envoyer des revendications à l’aide d’une règle personnalisée, puis cliquez sur suivant.
5. Dans la page Configurer la règle, sous nom de la règle de revendication, tapez le nom complet de cette règle. Sous règle personnalisée, tapez ou collez la syntaxe de langage de règle de revendication suivante :`exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "Group SID value of allowed AD group"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Cliquez sur Terminer. Vérifiez que la nouvelle règle s’affiche immédiatement sous la règle autoriser l’accès à tous les utilisateurs dans la liste règles d’autorisation d’émission.
7. Pour enregistrer la règle, dans la boîte de dialogue Modifier les règles de revendication, cliquez sur OK.


### <a name="descriptions-of-the-claim-rule-language-syntax-used-in-the-above-scenarios"></a>Descriptions de la syntaxe du langage de règle de revendication utilisé dans les scénarios ci-dessus

|                                                                                                   Description                                                                                                   |                                                                     Syntaxe du langage de règle de revendication                                                                     |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              Règle de AD FS par défaut pour autoriser l’accès à tous les utilisateurs. Cette règle doit déjà exister dans la liste des règles d’autorisation d’émission de la plateforme d’identité Microsoft Office 365 Identity Platform.              |                                  = > problème (type = "<https://schemas.microsoft.com/authorization/claims/permit>", valeur = "true");                                   |
|                               L’ajout de cette clause à une nouvelle règle personnalisée spécifie que la demande provient du serveur proxy de Fédération (c’est-à-dire qu’elle a l’en-tête x-ms-proxy)                                |                                                                                                                                                                    |
|                                                                                 Il est recommandé d’inclure toutes les règles.                                                                                  |                                    Exists ([type = = "<https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy>"])                                    |
|                                                         Permet d’établir que la demande provient d’un client avec une adresse IP dans la plage acceptable définie.                                                         | N’existe pas ([type = = "<https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip>", valeur = ~ "Regex adresse IP publique fournie par le client"]) |
|                                    Cette clause permet de spécifier que si l’application faisant l’objet d’un accès n’est pas Microsoft. Exchange. ActiveSync, la demande doit être refusée.                                     |       N’existe pas ([type = = "<https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application>", valeur = = "Microsoft. Exchange. ActiveSync"])        |
|                                                      Cette règle vous permet de déterminer si l’appel s’est fait via un navigateur Web et ne sera pas refusé.                                                      |              N’existe pas ([type = = "<https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path>", valeur = = "/adfs/ls/"])               |
| Cette règle indique que les seuls utilisateurs d’un groupe de Active Directory particulier (basé sur la valeur SID) doivent être refusés. L’ajout de NOT à cette instruction signifie qu’un groupe d’utilisateurs est autorisé, quel que soit l’emplacement. |             Exists ([type = = "<https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid>", valeur = ~ "{valeur SID de groupe du groupe AD autorisé}"])              |
|                                                                Il s’agit d’une clause requise pour émettre un refus lorsque toutes les conditions précédentes sont remplies.                                                                 |                                   = > problème (type = "<https://schemas.microsoft.com/authorization/claims/deny>", valeur = "true");                                    |

### <a name="building-the-ip-address-range-expression"></a>Génération de l’expression de plage d’adresses IP

La revendication x-ms-forwarded-client-IP est remplie à partir d’un en-tête HTTP qui est actuellement défini uniquement par Exchange Online, qui remplit l’en-tête lors du passage de la demande d’authentification à AD FS. La valeur de la revendication peut être l’une des suivantes :

>[!Note] 
>Exchange Online ne prend actuellement en charge que les adresses IPV4 et non IPV6.

Une seule adresse IP : L’adresse IP du client qui est directement connecté à Exchange Online

>[!Note] 
>L’adresse IP d’un client sur le réseau d’entreprise s’affiche comme l’adresse IP de l’interface externe du proxy ou de la passerelle sortants de l’organisation.

Les clients qui sont connectés au réseau d’entreprise par un VPN ou par Microsoft DirectAccess (DA) peuvent apparaître comme des clients d’entreprise internes ou comme clients externes, en fonction de la configuration du VPN ou de DA.

Une ou plusieurs adresses IP : Quand Exchange Online ne peut pas déterminer l’adresse IP du client qui se connecte, il définit la valeur en fonction de la valeur de l’en-tête x-forwarded-for, d’un en-tête non standard qui peut être inclus dans les demandes HTTP et est pris en charge par de nombreux clients, équilibreurs de charge et les proxys sur le marché.

>[!Note]
>Plusieurs adresses IP, indiquant l’adresse IP du client et l’adresse de chaque proxy qui a transmis la demande, sont séparées par une virgule.

Les adresses IP associées à l’infrastructure Exchange Online n’apparaîtront pas dans la liste.


#### <a name="regular-expressions"></a>Expressions régulières

Lorsque vous devez faire correspondre une plage d’adresses IP, il devient nécessaire de construire une expression régulière pour effectuer la comparaison. Dans la prochaine série d’étapes, nous fournirons des exemples de construction d’une telle expression pour qu’elle corresponde aux plages d’adresses suivantes (Notez que vous devrez modifier ces exemples pour qu’ils correspondent à votre plage d’adresses IP publiques) :


- 192.168.1.1 – 192.168.1.25
- 10.0.0.1 – 10.0.0.14

Tout d’abord, le modèle de base qui correspondra à une seule adresse IP est le suivant :\.\b # # #######\.\.# # # \b

En étendant cela, nous pouvons faire correspondre deux adresses IP différentes avec une expression ou comme suit : \b\.# # #\. \.\. ######\.# # # \b | \b # # #### ######\b \.

Par conséquent, voici un exemple qui correspond à deux adresses (telles que 192.168.1.1 ou 10.0.0.1) : \b192\.168\.1\.1 \ b | \b10\.0\.0\.1 \ b

Cela vous donne la technique qui vous permet d’entrer un nombre quelconque d’adresses. Si une plage d’adresses doit être autorisée, par exemple 192.168.1.1 – 192.168.1.25, la correspondance doit être effectuée caractère par caractère : \b192 @ no__t-0168 @ no__t-11 @ no__t-2 ([1-9] | 1 [0-9] | 2 [0-5]) \b

>[!Note] 
>L’adresse IP est traitée comme une chaîne et non un nombre.


La règle est décomposée comme suit :\.\b192\.168 1\.

Cela correspond à toute valeur commençant par 192.168.1.

Les éléments suivants correspondent aux plages requises pour la partie de l’adresse après la virgule décimale finale :


- ([1-9] correspond aux adresses se terminant par 1-9
- | 1 [0-9] correspond aux adresses se terminant par 10-19
- | 2 [0-5]) correspond à des adresses se terminant par 20-25

>[!Note]
>Les parenthèses doivent être correctement positionnées, de sorte que vous ne commencez pas à faire correspondre d’autres parties d’adresses IP.

Une fois le bloc 192 mis en correspondance, nous pouvons écrire une expression similaire pour le bloc 10 : \b10 @ no__t-00 @ no__t-10 @ no__t-2 ([1-9] | 1 [0-4]) \b

Et en les plaçant ensemble, l’expression suivante doit correspondre à toutes les adresses pour « 192.168.1.1 ~ 25 » et « 10.0.0.1 ~ 14 » : \b192 @ no__t-0168 @ no__t-11 @ no__t-2 ([1-9] | 1 [0-9] | 2 [0-5]) \b | \b10 @ no__t-30 @ no__t-40 @ no__t-5 ([1-9] | 1 [0-4]) \b

#### <a name="testing-the-expression"></a>Test de l’expression

Les expressions Regex peuvent devenir assez difficiles. nous vous recommandons donc vivement d’utiliser un outil de vérification Regex. Si vous effectuez une recherche sur Internet pour « générateur d’expressions Regex en ligne », vous trouverez plusieurs utilitaires en ligne de qualité qui vous permettront de tester vos expressions par rapport à des exemples de données.

Lors du test de l’expression, il est important de comprendre ce qu’il faut s’attendre à trouver. Le système Exchange Online peut envoyer de nombreuses adresses IP, séparées par des virgules. Les expressions fournies ci-dessus fonctionnent pour cela. Toutefois, il est important de réfléchir à ce point lorsque vous testez vos expressions Regex. Par exemple, il est possible d’utiliser l’exemple d’entrée suivant pour vérifier les exemples ci-dessus : 

192.168.1.1, 192.168.1.2, 192.169.1.1. 192.168.12.1, 192.168.1.10, 192.168.1.25, 192.168.1.26, 192.168.1.30, 1192.168.1.20 

10.0.0.1, 10.0.0.5, 10.0.0.10, 10.0.1.0, 10.0.1.1, 110.0.0.1, 10.0.0.14, 10.0.0.15, 10.0.0.10, 10, 0.0.1 











## <a name="validating-the-deployment"></a>Validation du déploiement

### <a name="security-audit-logs"></a>Journaux d’audit de sécurité

Pour vérifier que les revendications du nouveau contexte de la demande sont envoyées et sont disponibles pour le pipeline de traitement des revendications AD FS, activez la journalisation d’audit sur le serveur de AD FS. Ensuite, envoyez des demandes d’authentification et recherchez les valeurs de revendication dans les entrées de journal d’audit de sécurité standard. 

Pour activer la journalisation des événements d’audit dans le journal de sécurité sur un serveur AD FS, suivez les étapes décrites dans configurer l’audit pour AD FS 2,0.

### <a name="event-logging"></a>Journalisation des événements

Par défaut, les demandes ayant échoué sont consignées dans le journal des événements de l’application situé sous journaux des applications et des services \ AD FS 2,0 \ admin. pour plus d’informations sur la journalisation des événements pour AD FS, consultez [configurer la journalisation des événements AD FS 2,0](https://technet.microsoft.com/library/adfs2-troubleshooting-configuring-computers.aspx).

### <a name="configuring-verbose-ad-fs-tracing-logs"></a>Configuration des journaux de suivi des AD FS détaillés

AD FS événements de suivi sont consignés dans le journal de débogage AD FS 2,0. Pour activer le suivi, consultez [configurer le suivi du débogage pour AD FS 2,0](https://technet.microsoft.com/library/adfs2-troubleshooting-configuring-computers.aspx).

Une fois que vous avez activé le suivi, utilisez la syntaxe de ligne de commande suivante pour activer le niveau de journalisation verbose : wevtutil. exe SL « AD FS 2,0 Tracing/Debug »/l : 5  

## <a name="related"></a>Liens apparentés
Pour plus d’informations sur les nouveaux types de revendication, consultez [AD FS types de revendications](AD-FS-Claims-Types.md).

