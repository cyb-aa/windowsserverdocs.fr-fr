---
ms.assetid: 
title: "Stratégies de contrôle d’accès client dans ActiveDirectory Federation Services 2.0"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 00b9cd17c7a5c206e06bea12fd90762adb336715
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="client-access-control-policies-in-ad-fs-20"></a>Stratégies de contrôle d’accès client dans ADFS 2.0
Une stratégies d’accès client dans ActiveDirectory Federation Services 2.0 permettent de restreindre ou accorder aux utilisateurs d’accéder aux ressources.  Ce document décrit comment activer des stratégies d’accès client dans ADFS 2.0 et comment configurer les scénarios les plus courants.

## <a name="enabling-client-access-policy-in-ad-fs-20"></a>L’activation de stratégie d’accès Client dans ADFS 2.0

Pour activer la stratégie d’accès client, suivez les étapes ci-dessous.

### <a name="step-1-install-the-update-rollup-2-for-ad-fs-20-package-on-your-ad-fs-servers"></a>Étape1: Installer le correctif cumulatif 2 pour ADFS 2.0 de package sur vos serveurs ADFS

Télécharger le [correctif cumulatif 2 pour ActiveDirectory Federation Services (ADFS) 2.0](https://support.microsoft.com/en-us/help/2681584/description-of-update-rollup-2-for-active-directory-federation-services-ad-fs-2.0) du package et l’installer sur tout serveur de fédération et les serveurs proxy de fédération.

### <a name="step-2-add-five-claim-rules-to-the-active-directory-claims-provider-trust"></a>Étape2: Ajouter que cinq règles de revendication à l’approbation de fournisseur de revendications ActiveDirectory

Une fois le correctif cumulatif 2 a été installé sur tous les serveurs ADFS et les serveurs proxy de fédération, utilisez la procédure suivante pour ajouter un ensemble de règles de revendications qui rend les nouveaux types de revendication disponibles pour le moteur de stratégie.

Pour ce faire, vous allez ajouter cinq règles de transformation d’acceptation pour chacun des types de revendication nouveau du contexte de demande à l’aide de la procédure suivante.

Sur l’approbation de fournisseur de revendications ActiveDirectory, créer une nouvelle règle de transformation d’acceptation à passer par le biais de chacun des nouveaux types de revendication contexte demande.

#### <a name="to-add-a-claim-rule-to-the-active-directory-claims-provider-trust-for-each-of-the-five-context-claim-types"></a>Pour ajouter une règle de revendication à ActiveDirectory approbation de fournisseur de revendications pour chaque du contexte cinq types de revendication:


1. Cliquez sur Démarrer, pointez sur Programmes, pointez sur Outils d’administration, puis cliquez sur ADFS 2.0 Management.
2. Dans l’arborescence de la console, sous 2 AD. 0\relations FS, cliquez sur les approbations de fournisseur de revendications, faites un clic droit ActiveDirectory, puis cliquez sur Modifier les règles de revendication.
3. Dans la boîte de dialogue Modifier les règles de revendication, sélectionnez l’onglet Règles de transformation d’acceptation, puis cliquez sur Ajouter une règle pour démarrer l’Assistant règle.
4. Sur la page Sélectionner un modèle de règle, sous le modèle de règle de revendication, sélectionnez passer ou filtrer une revendication entrante à partir de la liste, puis cliquez sur Suivant.
5. Sur la page Configurer la règle, sous le nom de règle de revendication, tapez le nom d’affichage pour cette règle; en entrant le type de revendication, tapez l’URL de type de revendication suivante, puis sélectionnez passe par le biais de toutes les valeurs de revendication.</br>
        `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`</br>
6. Pour vérifier la règle, sélectionnez-le dans la liste et cliquez sur Modifier la règle, puis cliquez sur Afficher le langage de règle. Le langage de règle de revendication doit apparaître comme suit: `c:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip"] => issue(claim = c);`
7. Cliquez sur Terminer.
8. Dans la boîte de dialogue Modifier les règles de revendication, cliquez sur OK pour enregistrer les règles.
9. Répétez les étapes2 à 6 pour créer une règle de revendication supplémentaires pour chacun des types de quatre revendication restants ci-dessous jusqu'à ce que toutes les cinq règles ont été créés.

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`


    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`

### <a name="step-3-update-the-microsoft-office-365-identity-platform-relying-party-trust"></a>Étape3: Mise à jour de la plate-forme d’identité de MicrosoftOffice 365 de confiance

Choisir l’un des exemples de scénarios ci-dessous pour configurer les règles de revendication sur la plate-forme d’identité de MicrosoftOffice 365 de confiance qui répond le mieux aux besoins de votre organisation.

## <a name="client-access-policy-scenarios-for-ad-fs-20"></a>Scénarios de stratégie d’accès client pour ADFS 2.0
Les sections suivantes décrivent les scénarios qui existent pour ADFS 2.0

### <a name="scenario-1-block-all-external-access-to-office-365"></a>Scénario 1: Bloquer tout accès externe à Office 365

Ce scénario de stratégie d’accès client permet d’accéder à partir de tous les clients internes et les blocs de tous les clients externes basés sur l’adresse IP du client externe. L’ensemble de règles s’appuie sur la règle d’autorisation d’émission par défaut autoriser l’accès à tous les utilisateurs. Vous pouvez utiliser la procédure suivante pour ajouter une règle d’autorisation d’émission à Office 365 de confiance.

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>Pour créer une règle pour bloquer tout accès externe à Office 365



1. Cliquez sur Démarrer, pointez sur Programmes, pointez sur Outils d’administration, puis cliquez sur ADFS 2.0 Management.
2. Dans l’arborescence de la console, sous 2 AD. 0\relations FS, cliquez sur les approbations de partie de confiance, cliquez sur l’approbation de la plate-forme d’identité MicrosoftOffice 365, puis cliquez sur Modifier les règles de revendication. 
3. Dans la boîte de dialogue Modifier les règles de revendication, sélectionnez l’onglet Règles d’autorisation d’émission, puis cliquez sur Ajouter une règle pour démarrer l’Assistant règle de revendication.
4. Sur la page Sélectionner un modèle de règle, sous le modèle de règle de revendication, sélectionnez Envoyer des revendications en utilisant une règle personnalisée, puis cliquez sur Suivant.
5. Sur la page Configurer la règle, sous le nom de règle de revendication, tapez le nom d’affichage pour cette règle. Sous règle personnalisée, tapez ou collez la syntaxe du langage de règle revendication suivantes: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");` 
6. Cliquez sur Terminer. Vérifiez que la nouvelle règle apparaît immédiatement sous Autoriser l’accès à la règle de tous les utilisateurs dans la liste des règles d’autorisation d’émission.
7. Pour enregistrer la règle, dans la boîte de dialogue Modifier les règles de revendication, cliquez sur OK.

>[!NOTE]
>Vous devez remplacer la valeur ci-dessus pour «regex d’adresse ip publique» avec une expression IP valide; consultez la construction de l’expression de plage d’adresses IP pour plus d’informations.


### <a name="scenario-2-block-all-external-access-to-office-365-except-exchange-activesync"></a>Scénario 2: Bloquer tout accès externe à Office 365 à l’exception d’Exchange ActiveSync

L’exemple suivant permet d’accéder à toutes les applications Office 365, notamment Exchange Online à partir des clients internes, y compris Outlook. Il bloque l’accès à partir de clients qui se trouvent en dehors du réseau d’entreprise, tel qu’indiqué par l’adresse IP du client, à l’exception des clients d’Exchange ActiveSync tels que des Smartphones. L’ensemble de règles s’appuie sur la règle d’autorisation d’émission par défaut intitulée Autoriser l’accès à tous les utilisateurs. Pour ajouter une règle d’autorisation d’émission à Office 365 de confiance à l’aide de l’Assistant règle de revendication, procédez comme suit:

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>Pour créer une règle pour bloquer tout accès externe à Office 365



1. Cliquez sur Démarrer, pointez sur Programmes, pointez sur Outils d’administration, puis cliquez sur ADFS 2.0 Management.
2. Dans l’arborescence de la console, sous 2 AD. 0\relations FS, cliquez sur les approbations de partie de confiance, cliquez sur l’approbation de la plate-forme d’identité MicrosoftOffice 365, puis cliquez sur Modifier les règles de revendication. 
3. Dans la boîte de dialogue Modifier les règles de revendication, sélectionnez l’onglet Règles d’autorisation d’émission, puis cliquez sur Ajouter une règle pour démarrer l’Assistant règle de revendication.
4. Sur la page Sélectionner un modèle de règle, sous le modèle de règle de revendication, sélectionnez Envoyer des revendications en utilisant une règle personnalisée, puis cliquez sur Suivant.
5. Sur la page Configurer la règle, sous le nom de règle de revendication, tapez le nom d’affichage pour cette règle. Sous règle personnalisée, tapez ou collez la syntaxe du langage de règle revendication suivantes: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
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
>Vous devez remplacer la valeur ci-dessus pour «regex d’adresse ip publique» avec une expression IP valide; consultez la construction de l’expression de plage d’adresses IP pour plus d’informations.

### <a name="scenario-3-block-all-external-access-to-office-365-except-browser-based-applications"></a>Scénario 3: Bloquer tout accès externe à Office 365 à l’exception des applications basées sur le navigateur

L’ensemble de règles s’appuie sur la règle d’autorisation d’émission par défaut intitulée Autoriser l’accès à tous les utilisateurs. Pour ajouter une règle d’autorisation d’émission à la plate-forme d’identité de MicrosoftOffice 365 de confiance à l’aide de l’Assistant règle de revendication, procédez comme suit:

>[!NOTE]
>Ce scénario n’est pas prise en charge avec un proxy tiers en raison des limitations sur les en-têtes de stratégie d’accès client avec des requêtes passives (basée sur le Web).

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>Pour créer une règle pour bloquer tout accès externe à Office 365 à l’exception des applications basées sur le navigateur



1. Cliquez sur Démarrer, pointez sur Programmes, pointez sur Outils d’administration, puis cliquez sur ADFS 2.0 Management.
2. Dans l’arborescence de la console, sous 2 AD. 0\relations FS, cliquez sur les approbations de partie de confiance, cliquez sur l’approbation de la plate-forme d’identité MicrosoftOffice 365, puis cliquez sur Modifier les règles de revendication. 
3. Dans la boîte de dialogue Modifier les règles de revendication, sélectionnez l’onglet Règles d’autorisation d’émission, puis cliquez sur Ajouter une règle pour démarrer l’Assistant règle de revendication.
4. Sur la page Sélectionner un modèle de règle, sous le modèle de règle de revendication, sélectionnez Envoyer des revendications en utilisant une règle personnalisée, puis cliquez sur Suivant.
5. Sur la page Configurer la règle, sous le nom de règle de revendication, tapez le nom d’affichage pour cette règle. Sous règle personnalisée, tapez ou collez la syntaxe du langage de règle revendication suivantes: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value == "/adfs/ls/"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Cliquez sur Terminer. Vérifiez que la nouvelle règle apparaît immédiatement sous Autoriser l’accès à la règle de tous les utilisateurs dans la liste des règles d’autorisation d’émission.
7. Pour enregistrer la règle, dans la boîte de dialogue Modifier les règles de revendication, cliquez sur OK.

### <a name="scenario-4-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>Scénario 4: Bloquer tout accès externe à Office 365 pour les groupes ActiveDirectory désignés

L’exemple suivant permet d’accéder à partir de clients internes basé sur l’adresse IP. Il bloque l’accès à partir de clients résidant en dehors du réseau d’entreprise qui ont une adresse IP de clients externes, à l’exception des personnes dans une règle d’ActiveDirectory Group.The spécifiée jeu s’appuie sur la règle d’autorisation d’émission par défaut intitulée Autoriser l’accès à tous les utilisateurs. Pour ajouter une règle d’autorisation d’émission à la plate-forme d’identité de MicrosoftOffice 365 de confiance à l’aide de l’Assistant règle de revendication, procédez comme suit:

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>Pour créer une règle pour bloquer tout accès externe à Office 365 pour les groupes ActiveDirectory désignés



1. Cliquez sur Démarrer, pointez sur Programmes, pointez sur Outils d’administration, puis cliquez sur ADFS 2.0 Management.
2. Dans l’arborescence de la console, sous 2 AD. 0\relations FS, cliquez sur les approbations de partie de confiance, cliquez sur l’approbation de la plate-forme d’identité MicrosoftOffice 365, puis cliquez sur Modifier les règles de revendication. 
3. Dans la boîte de dialogue Modifier les règles de revendication, sélectionnez l’onglet Règles d’autorisation d’émission, puis cliquez sur Ajouter une règle pour démarrer l’Assistant règle de revendication.
4. Sur la page Sélectionner un modèle de règle, sous le modèle de règle de revendication, sélectionnez Envoyer des revendications en utilisant une règle personnalisée, puis cliquez sur Suivant.
5. Sur la page Configurer la règle, sous le nom de règle de revendication, tapez le nom d’affichage pour cette règle. Sous règle personnalisée, tapez ou collez la syntaxe du langage de règle revendication suivantes: `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "Group SID value of allowed AD group"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. Cliquez sur Terminer. Vérifiez que la nouvelle règle apparaît immédiatement sous Autoriser l’accès à la règle de tous les utilisateurs dans la liste des règles d’autorisation d’émission.
7. Pour enregistrer la règle, dans la boîte de dialogue Modifier les règles de revendication, cliquez sur OK.


### <a name="descriptions-of-the-claim-rule-language-syntax-used-in-the-above-scenarios"></a>Descriptions de la syntaxe du langage de règle revendication utilisées dans les scénarios ci-dessus

|Description|Syntaxe du langage de règle de revendication|
|-----|-----| 
|Règle de FS par défaut ActiveDirectory pour autoriser l’accès à tous les utilisateurs. Cette règle doit déjà exister dans la plate-forme d’identité de MicrosoftOffice 365 confiance liste des règles d’autorisation d’émission.|= > problème (Type = "https://schemas.microsoft.com/authorization/claims/permit", valeur = "true");| 
|Ajout de cette clause à une nouvelle règle personnalisée spécifie que la demande provient d’un serveur proxy de fédération (par exemple, il a l’en-tête x-ms-proxy)
Il est recommandé que toutes les règles incluent.|Il existe ([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"])| 
|Utilisé pour établir que la demande provient d’un client avec une adresse IP de la plage acceptable définie.|PAS existe ([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", valeur = ~ "regex d’adresse ip publique fournie par le client"])| 
|Cette clause est utilisée pour spécifier que si l’application de l’accès n’est pas Microsoft.Exchange.ActiveSync la demande doit être refusée.|PAS existe ([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value=="Microsoft.Exchange.ActiveSync"])| 
|Cette règle vous permet de déterminer si l’appel a été via un navigateur Web et ne sera pas possible.|PAS existe ([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", valeur == «/ adfs/ls /»])| 
|Cette règle indique que seuls les utilisateurs dans un groupe ActiveDirectory particulier (basé sur la valeur de SID) doivent être refusés. Ajout de pas à cette déclaration signifie qu'un groupe d’utilisateurs sera autorisé, quel que soit l’emplacement.|Existe ([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", valeur = ~ «{groupe valeur du SID de groupe d’ActiveDirectory autorisée}»])| 
|Il s’agit d’une clause requise pour émettre un refus lorsque toutes les conditions précédentes sont remplies.|= > problème (Type = "https://schemas.microsoft.com/authorization/claims/deny", valeur = "true");|

### <a name="building-the-ip-address-range-expression"></a>Création de l’expression de plage d’adresses IP

La revendication x-ms-transmis-client-ip est complétée à partir d’un en-tête HTTP qui est actuellement défini par Exchange Online, qui remplit l’en-tête lors du passage de la demande d’authentification pour ADFS. La valeur de la revendication peut être une des opérations suivantes:

>[!Note] 
>Exchange Online prend actuellement en charge uniquement les adresses IPV4 et IPV6 pas.

Une seule adresse IP: adresse IP du client qui est connecté directement à Exchange Online

>[!Note] 
>L’adresse IP d’un client sur le réseau d’entreprise s’affiche comme adresse IP externe d’interface de passerelle ou de proxy sortantes de l’organisation.

Les clients qui sont connectés au réseau d’entreprise via un VPN ou par MicrosoftDirectAccess (DA) peuvent apparaître en tant que clients d’entreprise internes ou externes clients en fonction de la configuration du VPN ou DA.

Une ou plusieurs adresses IP: lorsque Exchange Online ne peut pas déterminer l’adresse IP du client qui se connecte, il définit la valeur en fonction de la valeur de l’en-tête x transmis pour, un en-tête non standard qui peut être inclus dans les demandes HTTP et est pris en charge par de nombreux clients, les équilibreurs de charge et les proxys sur le marché.

>[!Note]
>Plusieurs adresses IP, ce qui indique l’adresse IP du client et l’adresse de chaque serveur proxy qui a transmis la demande, seront séparées par une virgule.

Les adresses IP liés à l’infrastructure Exchange Online n’apparaîtront pas dans la liste.


#### <a name="regular-expressions"></a>Expressions régulières

Lorsque vous disposez correspondre à une plage d’adresses IP, il devient nécessaire construire une expression régulière pour effectuer la comparaison. Dans la prochaine série d’étapes, nous fournirons des exemples pour savoir comment construire une telle expression pour faire correspondre les plages d’adresses suivantes (Notez que vous devrez modifier ces exemples pour faire correspondre votre plage IP public):


- 192.168.1.1 – 192.168.1.25
- 10.0.0.1 – 10.0.0.14

Tout d’abord, le modèle de base correspondant à une seule adresse IP est le suivant: \b###\.###\.###\.###\b

L’extension de cela, nous pouvons correspondent à deux adresses IP avec une expression ou comme suit: \b###\.###\.###\.###\b|\b###\.###\.###\.###\b

Ainsi, est un exemple pour correspondre à seulement deux adresses (comme 192.168.1.1 ou 10.0.0.1): \b192\.168\.1\.1\b|\b10\.0\.0\.1\b

Cela vous donne la technique par laquelle vous pouvez entrer n’importe quel nombre d’adresses. Lorsqu’une plage d’adresses doivent autorisée, par exemple 192.168.1.1 – 192.168.1.25, la mise en correspondance doit être réalisée caractère par caractère: \b192\.168\.1\. ([1-9] | 1 [0-9] | 2 [0-5]) \b

>[!Note] 
>L’adresse IP est considérée comme chaîne et non un nombre.


La règle est répartie comme suit: \b192\.168\.1\.

Cela correspond à n’importe quel 192.168.1 à compter de valeur.

Les plages requis pour la partie de l’adresse après la virgule décimale finale correspond aux éléments suivants:


- ([1-9] correspond à des adresses se terminant par 1-9
- | 1 [0-9] correspond à 10-19 se terminant par des adresses
- Adresses de correspondances |2[0-5]) se terminant par 20-25

>[!Note]
>Les parenthèses doivent être positionnés correctement, afin que vous ne démarrez pas correspondance d’autres parties d’adresses IP.

Avec le bloc de 192mis en correspondance, nous pouvons écrire une expression semblable pour le bloc de 10: \b10\.0\.0\. ([1-9] | 1 [0-4]) \b

Et les assembler, l’expression suivante doit correspondre à toutes les adresses de «192.168.1.1~25» et «10.0.0.1~14»: \b192\.168\.1\. ([1-9]|1[0-9]|2[0-5])\b|\b10\.0\.0\. ([1-9] | 1 [0-4]) \b

#### <a name="testing-the-expression"></a>L’Expression de test

Expressions Regex peuvent devenir très difficile, nous vous recommandons vivement d’à l’aide d’un outil de vérification de regex. Si vous effectuez une recherche sur internet pour «générateur d’expressions regex en ligne», vous trouverez plusieurs utilitaires en ligne bons qui vous permet de tester vos expressions contre les exemples de données.

Lorsque vous testez l’expression, il est important de comprendre à quoi s’attendre à correspondre à. Le système en ligne Exchange peut envoyer le nombre d’adresses IP, séparé par des virgules. Les expressions ci-dessus fonctionnera pour cela. Toutefois, il est important d’aborder ce sujet lors du test de vos expressions de regex. Par exemple, un peut utiliser l’exemple suivant d’entrée pour vérifier les exemples ci-dessus: 

192.168.1.1, 192.168.1.2, 192.169.1.1. 192.168.12.1, 192.168.1.10, 192.168.1.25, 192.168.1.26, 192.168.1.30, 1192.168.1.20 

10.0.0.1, 10.0.0.5, 10.0.0.10, 10.0.1.0, 10.0.1.1, 110.0.0.1, 10.0.0.14, 10.0.0.15, 10.0.0.10, 10,0.0.1 











## <a name="validating-the-deployment"></a>Validation du déploiement

### <a name="security-audit-logs"></a>Journaux d’Audit de sécurité

Pour vérifier que le nouveau contexte de demande de revendications sont envoyées et sont disponibles pour les services ADFS de pipeline de traitement des revendications, activer l’enregistrement sur le serveur ADFS d’audit. Envoyer des demandes d’authentification, ainsi à cocher pour les valeurs de revendication dans les entrées du journal d’audit de sécurité standard. 

Pour activer la journalisation d’audit des événements à la sécurité sont consignés sur un serveur ADFS, suivez les étapes de configurer l’audit pour ADFS 2.0.

### <a name="event-logging"></a>Journalisation des événements

Par défaut, les demandes ayant échoué sont consignés dans le journal des événements application situé sous journaux des Applications et Services \ ADFS 2.0 \ Admin.For plus d’informations sur la journalisation des événements pour ADFS, consultez [configurer ADFS 2.0 la journalisation des événements](https://technet.microsoft.com/library/adfs2-troubleshooting-configuring-computers.aspx).

### <a name="configuring-verbose-ad-fs-tracing-logs"></a>La configuration détaillée ADFS journaux de suivi

Événements de suivi ADFS sont consignés dans le journal de débogage 2.0 ADFS. Pour activer le suivi, voir [configurer suivi de débogage pour ADFS 2.0](https://technet.microsoft.com/en-us/library/adfs2-troubleshooting-configuring-computers.aspx).

Une fois que vous avez activé le suivi, utilisez la syntaxe de ligne de commande suivante pour activer le niveau de journalisation documentée: wevtutil.exe sl «ADFS 2.0 suivi/débogage» /l: 5  

## <a name="related"></a>Liées
Pour plus d’informations sur les nouveaux types de revendication [Types de revendications ADFS](AD-FS-Claims-Types.md).
 
