---
ms.assetid: ''
title: Stratégies de contrôle d’accès client dans Active Directory Federation Services 2.0
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 036d6d0543687e7f82caf3dfd2c3bb0b4a981181
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445052"
---
# <a name="client-access-control-policies-in-ad-fs-20"></a>Stratégies de contrôle d’accès client dans AD FS 2.0
Une stratégie d’accès client dans Active Directory Federation Services 2.0 permettre de restreindre ou accorder l’accès des utilisateurs aux ressources.  Ce document décrit comment activer des stratégies d’accès client dans AD FS 2.0 et comment configurer des scénarios les plus courants.

## <a name="enabling-client-access-policy-in-ad-fs-20"></a>Activation de la stratégie d’accès Client dans AD FS 2.0

Pour activer la stratégie d’accès client, suivez les étapes ci-dessous.

### <a name="step-1-install-the-update-rollup-2-for-ad-fs-20-package-on-your-ad-fs-servers"></a>Étape 1 : Installation du package du correctif cumulatif 2 pour AD FS 2.0 sur vos serveurs AD FS

Téléchargez le [correctif cumulatif 2 pour Active Directory Federation Services (ADFS) 2.0](https://support.microsoft.com/en-us/help/2681584/description-of-update-rollup-2-for-active-directory-federation-services-ad-fs-2.0) empaqueter et installez-le sur tous les serveur de fédération et les serveurs proxy de fédération.

### <a name="step-2-add-five-claim-rules-to-the-active-directory-claims-provider-trust"></a>Étape 2 : Ajoutez que cinq règles de revendication à l’approbation de fournisseur de revendications Active Directory

Une fois le correctif cumulatif 2 a été installé sur tous les serveurs AD FS et proxys, procédez comme suit pour ajouter un ensemble de règles de revendications qui rend les nouveaux types de revendication disponibles pour le moteur de stratégie.

Pour ce faire, vous allez ajouter cinq règles de transformation d’acceptation pour chacun des types de revendication nouvelle du contexte de demande à l’aide de la procédure suivante.

Sur l’approbation de fournisseur de revendications Active Directory, créez une nouvelle règle de transformation d’acceptation à passer par chacun des nouveaux types de revendication de contexte demande.

#### <a name="to-add-a-claim-rule-to-the-active-directory-claims-provider-trust-for-each-of-the-five-context-claim-types"></a>Pour ajouter une règle de revendication à l’annuaire Active Directory approbation de fournisseur de revendications pour chaque du contexte cinq types de revendications :


1. Cliquez sur Démarrer, pointez sur Programmes, pointez sur Outils d’administration, puis cliquez sur AD FS 2.0 Management.
2. Dans l’arborescence de la console, sous les relations de 2.0\Trust AD FS, cliquez sur les approbations de fournisseur de revendications, cliquez sur Active Directory, puis cliquez sur Modifier les règles de revendication.
3. Dans la boîte de dialogue Modifier les règles de revendication, sélectionnez l’onglet Règles de transformation d’acceptation, puis cliquez sur Ajouter une règle pour démarrer l’Assistant règle.
4. Dans la page Sélectionner un modèle de règle, sous le modèle de règle de revendication, sélectionnez passthrough ou filtrer une revendication entrante à partir de la liste, puis cliquez sur Suivant.
5. Dans la page Configurer la règle, sous le nom de règle de revendication, tapez le nom complet de cette règle ; dans entrant type de revendication, tapez l’URL de type de revendication suivante, puis sélectionnez Pass toutes les valeurs de revendication.</br>
        `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`</br>
6. Pour vérifier la règle, sélectionnez-la dans la liste et cliquez sur Modifier la règle, puis cliquez sur Afficher le langage de règle. Le langage de règle de revendication doit apparaître comme suit : `c:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip"] => issue(claim = c);`
7. Cliquez sur Terminer.
8. Dans la boîte de dialogue Modifier les règles de revendication, cliquez sur OK pour enregistrer les règles.
9. Répétez les étapes 2 à 6 pour créer une règle de revendication supplémentaires pour chacun des types de quatre revendication restantes ci-dessous jusqu'à ce que toutes les cinq règles ont été créés.

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`


~~~
`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`
~~~

### <a name="step-3-update-the-microsoft-office-365-identity-platform-relying-party-trust"></a>Étape 3 : Mettre à jour de la plateforme d’identité Microsoft Office 365 de confiance

Choisissez un des exemples de scénarios ci-dessous pour configurer les règles de revendication sur la plateforme d’identité Microsoft Office 365 de confiance qui répond le mieux aux besoins de votre organisation.

## <a name="client-access-policy-scenarios-for-ad-fs-20"></a>Scénarios de stratégie d’accès client pour AD FS 2.0
Les sections suivantes décrivent les scénarios qui existent pour AD FS 2.0

### <a name="scenario-1-block-all-external-access-to-office-365"></a>Scénario 1 : Bloquer tous les accès externes à Office 365

Ce scénario de stratégie d’accès client autorise l’accès à partir de tous les clients internes et les blocs de tous les clients externes, en fonction de l’adresse IP du client externe. L’ensemble de règles s’appuie sur la règle d’autorisation d’émission par défaut autoriser l’accès à tous les utilisateurs. Vous pouvez utiliser la procédure suivante pour ajouter une règle d’autorisation d’émission à la confiance Office 365.

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>Pour créer une règle pour bloquer tous les accès externes à Office 365



1. Cliquez sur Démarrer, pointez sur Programmes, pointez sur Outils d’administration, puis cliquez sur AD FS 2.0 Management.
2. Dans l’arborescence de la console, sous les relations de 2.0\Trust AD FS, cliquez sur la partie de confiance, cliquez sur l’approbation de la plateforme d’identité Microsoft Office 365, puis cliquez sur Modifier les règles de revendication. 
3. Dans la boîte de dialogue Modifier les règles de revendication, sélectionnez l’onglet Règles d’autorisation d’émission, puis cliquez sur Ajouter une règle pour démarrer l’Assistant règle de revendication.
4. Dans la page Sélectionner un modèle de règle, sous le modèle de règle de revendication, sélectionnez d’envoyer des revendications à l’aide d’une règle personnalisée, puis cliquez sur Suivant.
5. Dans la page Configurer la règle, sous le nom de règle de revendication, tapez le nom d’affichage pour cette règle. Sous règle personnalisée, tapez ou collez la syntaxe de langage de règle de revendication suivants : `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");` 
6. Cliquez sur Terminer. Vérifiez que la nouvelle règle apparaît immédiatement sous Autoriser l’accès à la règle de tous les utilisateurs dans la liste des règles d’autorisation d’émission.
7. Pour enregistrer la règle, dans la boîte de dialogue Modifier les règles de revendication, cliquez sur OK.

>[!NOTE]
>Vous devrez remplacer la valeur ci-dessus pour « regex d’adresse ip publique » avec une expression valide de l’adresse IP ; consultez la création de l’expression de plage d’adresses IP pour plus d’informations.


### <a name="scenario-2-block-all-external-access-to-office-365-except-exchange-activesync"></a>Scénario 2 : Bloquer tous les accès externes à Office 365 à l’exception d’Exchange ActiveSync

L’exemple suivant autorise l’accès à toutes les applications Office 365, notamment Exchange Online, à partir de clients internes, notamment Outlook. Il bloque l’accès à partir de clients résidant en dehors du réseau d’entreprise, comme indiqué par l’adresse IP de client, à l’exception des clients Exchange ActiveSync tels que des téléphones intelligents. L’ensemble de règles s’appuie sur la règle d’autorisation d’émission par défaut intitulée Autoriser l’accès à tous les utilisateurs. Utilisez les étapes suivantes pour ajouter une règle d’autorisation d’émission à Office 365 de confiance à l’aide de l’Assistant règle de revendication :

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>Pour créer une règle pour bloquer tous les accès externes à Office 365



1. Cliquez sur Démarrer, pointez sur Programmes, pointez sur Outils d’administration, puis cliquez sur AD FS 2.0 Management.
2. Dans l’arborescence de la console, sous les relations de 2.0\Trust AD FS, cliquez sur la partie de confiance, cliquez sur l’approbation de la plateforme d’identité Microsoft Office 365, puis cliquez sur Modifier les règles de revendication. 
3. Dans la boîte de dialogue Modifier les règles de revendication, sélectionnez l’onglet Règles d’autorisation d’émission, puis cliquez sur Ajouter une règle pour démarrer l’Assistant règle de revendication.
4. Dans la page Sélectionner un modèle de règle, sous le modèle de règle de revendication, sélectionnez d’envoyer des revendications à l’aide d’une règle personnalisée, puis cliquez sur Suivant.
5. Dans la page Configurer la règle, sous le nom de règle de revendication, tapez le nom d’affichage pour cette règle. Sous règle personnalisée, tapez ou collez la syntaxe de langage de règle de revendication suivants : `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.Autodiscover"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.ActiveSync"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Cliquez sur Terminer. Vérifiez que la nouvelle règle apparaît immédiatement sous Autoriser l’accès à la règle de tous les utilisateurs dans la liste des règles d’autorisation d’émission.
7. Pour enregistrer la règle, dans la boîte de dialogue Modifier les règles de revendication, cliquez sur OK.

>[!NOTE]
>Vous devrez remplacer la valeur ci-dessus pour « regex d’adresse ip publique » avec une expression valide de l’adresse IP ; consultez la création de l’expression de plage d’adresses IP pour plus d’informations.

### <a name="scenario-3-block-all-external-access-to-office-365-except-browser-based-applications"></a>Scénario 3 : Bloquer tous les accès externes à Office 365 à l’exception des applications basées sur le navigateur

L’ensemble de règles s’appuie sur la règle d’autorisation d’émission par défaut intitulée Autoriser l’accès à tous les utilisateurs. Utilisez les étapes suivantes pour ajouter une règle d’autorisation d’émission à la plateforme d’identité Microsoft Office 365 de confiance à l’aide de l’Assistant règle de revendication :

>[!NOTE]
>Ce scénario n’est pas pris en charge avec un proxy tiers en raison des limitations sur les en-têtes de stratégie d’accès client avec des requêtes passives (basé sur le Web).

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>Pour créer une règle pour bloquer tous les accès externes à Office 365 à l’exception des applications basées sur le navigateur



1. Cliquez sur Démarrer, pointez sur Programmes, pointez sur Outils d’administration, puis cliquez sur AD FS 2.0 Management.
2. Dans l’arborescence de la console, sous les relations de 2.0\Trust AD FS, cliquez sur la partie de confiance, cliquez sur l’approbation de la plateforme d’identité Microsoft Office 365, puis cliquez sur Modifier les règles de revendication. 
3. Dans la boîte de dialogue Modifier les règles de revendication, sélectionnez l’onglet Règles d’autorisation d’émission, puis cliquez sur Ajouter une règle pour démarrer l’Assistant règle de revendication.
4. Dans la page Sélectionner un modèle de règle, sous le modèle de règle de revendication, sélectionnez d’envoyer des revendications à l’aide d’une règle personnalisée, puis cliquez sur Suivant.
5. Dans la page Configurer la règle, sous le nom de règle de revendication, tapez le nom d’affichage pour cette règle. Sous règle personnalisée, tapez ou collez la syntaxe de langage de règle de revendication suivants : `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value == "/adfs/ls/"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Cliquez sur Terminer. Vérifiez que la nouvelle règle apparaît immédiatement sous Autoriser l’accès à la règle de tous les utilisateurs dans la liste des règles d’autorisation d’émission.
7. Pour enregistrer la règle, dans la boîte de dialogue Modifier les règles de revendication, cliquez sur OK.

### <a name="scenario-4-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>Scénario 4 : Bloquer tous les accès externes à Office 365 pour les groupes Active Directory désignés

L’exemple suivant permet d’accéder à partir de clients internes en fonction de l’adresse IP. Il bloque l’accès à partir de clients résidant en dehors du réseau d’entreprise qui ont une adresse IP de client externe, sauf pour les personnes dans une règle d’Active Directory groupe spécifiée ensemble s’appuie sur la règle d’autorisation d’émission par défaut intitulée Autoriser l’accès à Tous les utilisateurs. Utilisez les étapes suivantes pour ajouter une règle d’autorisation d’émission à la plateforme d’identité Microsoft Office 365 de confiance à l’aide de l’Assistant règle de revendication :

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>Pour créer une règle pour bloquer tous les accès externes à Office 365 pour les groupes Active Directory désignés



1. Cliquez sur Démarrer, pointez sur Programmes, pointez sur Outils d’administration, puis cliquez sur AD FS 2.0 Management.
2. Dans l’arborescence de la console, sous les relations de 2.0\Trust AD FS, cliquez sur la partie de confiance, cliquez sur l’approbation de la plateforme d’identité Microsoft Office 365, puis cliquez sur Modifier les règles de revendication. 
3. Dans la boîte de dialogue Modifier les règles de revendication, sélectionnez l’onglet Règles d’autorisation d’émission, puis cliquez sur Ajouter une règle pour démarrer l’Assistant règle de revendication.
4. Dans la page Sélectionner un modèle de règle, sous le modèle de règle de revendication, sélectionnez d’envoyer des revendications à l’aide d’une règle personnalisée, puis cliquez sur Suivant.
5. Dans la page Configurer la règle, sous le nom de règle de revendication, tapez le nom d’affichage pour cette règle. Sous règle personnalisée, tapez ou collez la syntaxe de langage de règle de revendication suivants : `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "Group SID value of allowed AD group"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Cliquez sur Terminer. Vérifiez que la nouvelle règle apparaît immédiatement sous Autoriser l’accès à la règle de tous les utilisateurs dans la liste des règles d’autorisation d’émission.
7. Pour enregistrer la règle, dans la boîte de dialogue Modifier les règles de revendication, cliquez sur OK.


### <a name="descriptions-of-the-claim-rule-language-syntax-used-in-the-above-scenarios"></a>Descriptions de la syntaxe de langage de règle de revendication utilisée dans les scénarios ci-dessus

|                                                                                                   Description                                                                                                   |                                                                     Syntaxe de langage de règle de revendication                                                                     |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              Règle de FS Active Directory par défaut pour autoriser l’accès à tous les utilisateurs. Cette règle doit déjà exister dans la plateforme d’identité Microsoft Office 365 confiance liste des règles d’autorisation d’émission.              |                                  => issue(Type = "<https://schemas.microsoft.com/authorization/claims/permit>", Value = "true");                                   |
|                               Ajout de cette clause pour une nouvelle règle personnalisée spécifie que la demande provient d’un serveur proxy de fédération (par exemple, elle a l’en-tête x-ms-proxy)                                |                                                                                                                                                                    |
|                                                                                 Il est recommandé que toutes les règles incluent cela.                                                                                  |                                    exists([Type == "<https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy>"])                                    |
|                                                         Utilisé pour établir que la demande provient d’un client avec une adresse IP dans la plage acceptable définie.                                                         | N’existe pas ([Type == "<https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip>», valeur = ~ « regex d’adresse ip publique fourni par le client »]) |
|                                    Cette clause est utilisée pour spécifier que si l’application en cours d’accès n’est pas Microsoft.Exchange.ActiveSync la demande doit être refusée.                                     |       NOT exists([Type == "<https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application>", Value=="Microsoft.Exchange.ActiveSync"])        |
|                                                      Cette règle permet de déterminer si l’appel a été via un navigateur Web et ne sera pas possible.                                                      |              NOT exists([Type == "<https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path>", Value == "/adfs/ls/"])               |
| Cette règle stipule que les seuls utilisateurs dans un groupe Active Directory particulier (basé sur la valeur du SID) doivent être refusés. Ajout de pas à cette instruction signifie qu'un groupe d’utilisateurs sera autorisé, indépendamment de l’emplacement. |             existe ([Type == "<https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid>», valeur = ~ « {groupe valeur SID de groupe d’Active Directory autorisée} »])              |
|                                                                Il s’agit d’une clause requise pour émettre une instruction deny lorsque toutes les conditions précédentes sont remplies.                                                                 |                                   => issue(Type = "<https://schemas.microsoft.com/authorization/claims/deny>", Value = "true");                                    |

### <a name="building-the-ip-address-range-expression"></a>Création de l’expression de plage d’adresses IP

La revendication de x-ms-transférés-client-ip est remplie à partir d’un en-tête HTTP qui est actuellement configuré par Exchange Online, qui remplit l’en-tête lors du passage de la demande d’authentification pour AD FS. La valeur de la revendication peut être une des opérations suivantes :

>[!Note] 
>Exchange Online prend en charge uniquement les adresses IPV4 et IPV6 pas.

Une seule adresse IP : L’adresse IP du client qui est directement connecté à Exchange Online

>[!Note] 
>L’adresse IP d’un client sur le réseau d’entreprise s’affiche comme adresse IP externe d’interface de passerelle ou proxy sortant de l’organisation.

Les clients qui sont connectés au réseau d’entreprise via un VPN ou par Microsoft DirectAccess (DA) peuvent apparaître en tant que clients d’entreprise internes ou en tant que clients externes, en fonction de la configuration de VPN ou DA.

Une ou plusieurs adresses IP : Lorsque Exchange Online ne peut pas déterminer l’adresse IP du client qui se connecte, il définit la valeur basée sur la valeur de l’en-tête x-forwarded-for, un en-tête non standard qui peut être inclus dans les demandes HTTP et est pris en charge par le nombre de clients, les équilibreurs de charge et proxys sur le marché.

>[!Note]
>Plusieurs adresses IP, indiquant l’adresse IP du client et l’adresse de chaque proxy qui a transmis la demande, doivent être séparés par une virgule.

Les adresses IP liées à l’infrastructure Exchange Online n’apparaîtra pas dans la liste.


#### <a name="regular-expressions"></a>Expressions régulières

Lorsque vous devez faire correspondre une plage d’adresses IP, il devient nécessaire construire une expression régulière pour effectuer la comparaison. Dans la prochaine série d’étapes, nous vous proposons des exemples pour savoir comment construire une telle expression pour faire correspondre les plages d’adresses suivante (Notez que vous devrez modifier ces exemples pour correspondre à votre plage IP publique) :


- 192.168.1.1 – 192.168.1.25
- 10.0.0.1 – 10.0.0.14

Tout d’abord, le modèle de base qui correspond à une seule adresse IP est la suivante : \b###\.###\.###\.### \b

Cette extension, nous pouvons correspond à deux adresses IP différentes avec une expression OR comme suit : \b###\.###\.###\.### \b|\b###\. ### \. ### \.### \b

Par conséquent, un exemple pour faire correspondre deux adresses (par exemple, 192.168.1.1 ou 10.0.0.1) serait : \b192\.168\.1\.1\b | \b10\.0\.0\.1\b

Cela vous donne la technique par laquelle vous pouvez entrer n’importe quel nombre d’adresses. Où une plage d’adresses doivent autorisés, par exemple 192.168.1.1 – 192.168.1.25, la correspondance doit être effectuée caractère par caractère : \b192\.168\.1\.([1-9] | [0-9] de 1 | 2 [0-5]) \b

>[!Note] 
>L’adresse IP est traité en tant que chaîne et pas un nombre.


La règle est répartie comme suit : \b192\.168\.1\.

Cela correspond à n’importe quel valeurs commençant par 192.168.1.

Les plages requis pour la partie de l’adresse après la virgule décimale finale correspond à ce qui suit :


- ([1-9] correspond à des adresses se terminant par 1-9
- | 1 [0-9] correspond à des adresses se terminant par 10-19
- Adresses de correspondances |2[0-5]) se terminant par 20-25

>[!Note]
>Les parenthèses doivent être positionnés correctement, afin que vous ne démarrez pas autres parties d’adresses IP correspondantes.

Avec le bloc 192 mis en correspondance, nous pouvons écrire une expression similaire pour le bloc de 10 : \b10\.0\.0\.([1-9] | [0-4] de 1) \b

Et en les plaçant entre eux, l’expression suivante doit correspondre à toutes les adresses pour « 192.168.1.1~25 » et « 10.0.0.1~14 » : \b192\.168\.1\.([1-9] | [0-9] de 1 | 2 [0-5]) \b|\b10\.0\.0\. ([1-9] | [0-4] de 1) \b

#### <a name="testing-the-expression"></a>L’Expression de test

Expressions d’expression régulière peuvent devenir assez délicate, nous vous recommandons fortement d’à l’aide d’un outil de vérification d’expression régulière. Si vous effectuez une recherche sur internet pour « générateur d’expressions regex en ligne », vous trouverez plusieurs utilitaires en ligne bon qui vous permettra de tester vos expressions par rapport à des exemples de données.

Lorsque vous testez l’expression, il est important de comprendre à quoi s’attendre à doivent correspondre. Le système en ligne Exchange peut envoyer le nombre d’adresses IP, séparée par des virgules. Les expressions fournies ci-dessus fonctionne pour cela. Toutefois, il est important de réfléchir à ce sujet lorsque vous testez vos expressions regex. Par exemple, il peut utiliser l’exemple suivant d’entrée pour vérifier les exemples ci-dessus : 

192.168.1.1, 192.168.1.2, 192.169.1.1. 192.168.12.1, 192.168.1.10, 192.168.1.25, 192.168.1.26, 192.168.1.30, 1192.168.1.20 

10.0.0.1, 10.0.0.5, 10.0.0.10, 10.0.1.0, 10.0.1.1, 110.0.0.1, 10.0.0.14, 10.0.0.15, 10.0.0.10, 10,0.0.1 











## <a name="validating-the-deployment"></a>Validation du déploiement

### <a name="security-audit-logs"></a>Journaux d’Audit de sécurité

Pour vérifier que le nouveau contexte de demande de revendications sont envoyées et sont disponibles pour les services AD FS de pipeline de traitement des revendications, activer l’audit de journalisation sur le serveur AD FS. Ensuite envoyer certaines demandes d’authentification et la vérification pour les valeurs de revendication dans les entrées de journal d’audit de sécurité standard. 

Pour activer la journalisation d’audit de journal des événements à la sécurité sur un serveur AD FS, suivez les étapes à configurer l’audit pour AD FS 2.0.

### <a name="event-logging"></a>Journalisation des événements

Par défaut, les demandes ayant échoué sont enregistrés dans le journal des événements application situé sous journaux des Applications et Services \ AD FS 2.0 \ Admin.For plus d’informations sur la journalisation des événements pour AD FS, consultez [configurer AD FS 2.0 la journalisation des événements](https://technet.microsoft.com/library/adfs2-troubleshooting-configuring-computers.aspx).

### <a name="configuring-verbose-ad-fs-tracing-logs"></a>Configuration détaillée AD FS journaux de suivi

Événements de suivi AD FS sont enregistrés dans le journal de débogage 2.0 AD FS. Pour activer le suivi, consultez [configurer le suivi de débogage pour AD FS 2.0](https://technet.microsoft.com/library/adfs2-troubleshooting-configuring-computers.aspx).

Une fois que vous avez activé le traçage, utilisez la syntaxe de ligne de commande suivante pour activer le niveau de journalisation verbose : wevtutil.exe sl « AD FS 2.0 suivi/débogage « /l:5  

## <a name="related"></a>Liens apparentés
Pour plus d’informations sur les nouveaux types de revendication, consultez [Types de revendications AD FS](AD-FS-Claims-Types.md).

