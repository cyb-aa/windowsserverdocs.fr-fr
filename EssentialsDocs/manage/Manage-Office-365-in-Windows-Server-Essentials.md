---
title: "Gérer Office 365dans WindowsServerEssentials"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3f8485e4-e10f-4f38-8a5e-d5227abd0d84
0author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: b45bddb657b96c7cc5f9291a6c887b9d0801974b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="manage-office-365-in-windows-server-essentials"></a>Gérer Office 365dans WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Lorsque vous intégrez votre serveur WindowsServerEssentials à MicrosoftOffice 365, vous pouvez gérer vos services Office 365 et les comptes en ligne avec vos ressources locales à partir du tableau de bord WindowsServerEssentials. Dans cette rubrique, vous tout savoir ce que vous pouvez obtenir à l’intégration de votre serveur à Office 365, comment procéder et comment gérer et dépanner l’intégration d’Office 365.  
  
  
  
> [!IMPORTANT]
>   Intégration d’Office 365 est uniquement pris en charge dans un environnement de contrôleur de domaine unique. En outre, l’Assistant d’intégration Office 365 doit s’exécuter sur un contrôleur de domaine.  
  
## <a name="in-this-topic"></a>Dans cette rubrique.  
  
-   [Pourquoi dois-je intégrer Office 365 avec mon serveur?](#BKMK_IntegrationOverview)  
  
-   [Configurer l’intégration d’Office 365](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Configure)  
  
-   [Gérer l’intégration d’Office 365](#BKMK_ManageIntegration)  
  
-   [Résoudre les problèmes d’intégration d’Office 365](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Troubleshoot)  
  
##  <a name="BKMK_IntegrationOverview"></a>Pourquoi dois-je intégrer Office 365 avec mon serveur?  
 Il existe de nombreuses raisons valables d’intégrer Office 365 à votre serveur WindowsServerEssentials. Si vous gérez certaines de vos ressources internes mais que vous utilisez Office 365 pour d’autres services, vous serez en mesure de gérer vos services Office 365 et les ressources à partir du tableau de bord, ainsi que vos ressources locales, au lieu de travailler à deux emplacements.  
  
-   Gérer les comptes en ligne qui permettent à vos utilisateurs d’accéder à Office 365 avec vos comptes d’utilisateur:  
  
    -   Créer des comptes MicrosoftOnline Services pour vos utilisateurs en une seule étape, ou créer des comptes d’utilisateur sur le serveur pour vos comptes en ligne existants. Vous pouvez également ajouter un compte en ligne pour un compte d’utilisateur nouveau ou existant.  
  
         Lorsque vous créez les comptes en ligne à partir du tableau de bord, les utilisateurs se connecter à Office 365 avec le même mot de passe qu’ils utilisent sur le serveur. S’ils modifient le mot de passe pour son compte d’utilisateur, le mot de passe en ligne change également. Et vous savez que leurs mots de passe du compte en ligne répondent toujours aux exigences de sécurité que vous définissez pour vos comptes d’utilisateur.  
  
    -   Gérer un compte en ligne, ainsi que le compte d’utilisateur tout au long du cycle de vie du compte d’utilisateur. Si vous désactivez un compte d’utilisateur, le compte en ligne est également désactivé. Si vous supprimez un compte d’utilisateur, le compte en ligne est également supprimé.  
  
    -   Sur un serveur WindowsServerEssentials, gérez les groupes de distribution Exchange Online pour le courrier électronique également.  
  
-   Envoyer et recevoir des messages électroniques à partir de votre domaine Internet de s l’organisation (par exemple, contoso.com) en reliant un domaine Internet personnalisé à votre abonnement Office 365.  
  
-   Gérer votre abonnement et l’intégration d’Office 365 à partir du tableau de bord.  
  
-   Si votre abonnement inclut des bibliothèques SharePoint Online, l’intégration d’Office 365 avec un serveur WindowsServerEssentials vous permet de:  
  
    -   Créer et gérer vos bibliothèques SharePoint Online à partir du tableau de bord.  
  
        > [!NOTE]
        >  Tout vous pourrez également utiliser l’application My Server2012R2 pour travailler avec des documents dans vos bibliothèques SharePoint Online à partir de votre ordinateur portable, appareil mobile ou Windows phone. Cette fonctionnalité est uniquement disponible pour WindowsServerEssentials. Pour plus d’informations, voir [utiliser l’application My Server](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md).  
  
    -   Modifier les autorisations pour un site d’équipe SharePoint Online à partir du tableau de bord, ou ouvrez le site d’équipe à partir du tableau de bord pour apporter d’autres modifications.  
  
     Pour plus d’informations, voir [gérer SharePoint Online](Manage-SharePoint-Online-in-Windows-Server-Essentials.md).  
  
-   Si vous vous abonnez à Exchange Online, l’intégration d’Office 365 avec un serveur WindowsServerEssentials vous permet de gérer les périphériques mobiles que vos utilisateurs utilisent pour se connecter au serveur de messagerie de votre entreprise:  
  
    -   Nécessitent une protection de mot de passe lorsque les périphériques mobiles se connectent au serveur de messagerie de la société. Définissez une longueur minimale de mot de passe, le nombre de tentatives de connexion ayant échouées pour autoriser et la durée minimale entre les tentatives de connexion.  
  
    -   Empêchez un appareil mobile de se connecter à Exchange Online Si vous connaissez des problèmes de sécurité avec le modèle d’appareil.  
  
    -   Si un périphérique mobile est perdu ou volé, effacez l’appareil pour supprimer toutes les données sensibles de l’entreprise la prochaine fois que l’appareil est allumé.  
  
-    Intégration d’Office 365vous offre de nouvelles façons pour se connecter aux ressources et services Office 365:  
  
    -   Ouvrez services Office 365 à partir de Launchpad de WindowsServerEssentials. Pour plus d’informations, consultez [Quick Start Guide to Using MicrosoftOffice 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md).  
  
    -   Utilisez l’application My Server2012R2 pour travailler avec des documents dans vos bibliothèques SharePoint Online à partir de votre ordinateur portable, appareil mobile ou Windows phone. Pour plus d’informations, consultez [utiliser l’application My Server](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md). Cette fonctionnalité est uniquement disponible dans WindowsServerEssentials.  
  
##  <a name="BKMK_Configure"></a>Configurer l’intégration d’Office 365  
 Vous pouvez intégrer votre serveur à Office 365 à tout moment après avoir terminé l’installation du serveur. Si vous ne pas avoir déjà un abonnement Office 365, vous pouvez acheter un ou vous inscrire pour un abonnement d’évaluation gratuit.  
  
 Vous exécuterez les tâches suivantes:  
  
-   [Étape1: Vérifier les conditions de l’intégration d’Office 365](#BKMK_StepOne_VERIFY)  
  
-   [Étape2: Intégrer le serveur à MicrosoftOffice 365](#BKMK_StepTwo)  
  
-   [Étape3: Relier votre nom de domaine Internet s d’organisation à Office 365 (facultatif)](#BKMK_StepThree)  
  
###  <a name="BKMK_StepOne_VERIFY"></a>Étape1: Vérifier les conditions de l’intégration d’Office 365  
 Avant de commencer, assurez-vous que le serveur répond aux exigences suivantes:  
  
-   Le serveur peut avoir un de ces systèmes d’exploitation: WindowsServerEssentials, WindowsServerEssentials ou le système d’exploitation Windows Server2012R2 Standard ou Windows Server2012R2 Datacenter avec le rôle expérience WindowsServerEssentials installé.  
  
-   L’environnement peut être un contrôleur de domaine, et vous devez effectuer l’intégration d’Office 365 sur le contrôleur de domaine.  
  
-   Vous devez être en mesure de se connecter à Internet à partir du serveur.  
  
-   Avant de commencer l’intégration d’Office 365, vous devez installer toutes les mises à jour critiques et importantes sur le serveur.  
  
-   Si vous souhaitez utiliser votre domaine Internet de s organisation dans les adresses de messagerie et avec vos ressources SharePoint Online, vous devez inscrire le nom de domaine afin que vous pouvez relier le domaine à Office 365lors de l’intégration. Pour plus d’informations, voir [comment acheter un nom de domaine](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660).  
  
> [!NOTE]
>  Vous n’avez pas besoin pour vous abonner à Office 365 à l’avance. Vous serez en mesure d’acheter un abonnement ou vous inscrire pour une version d’évaluation gratuite pendant l’intégration d’Office 365. Si vous d souhaitez jeter un coup de œil les offres et la tarification pour Office 365, [comparer les offres Office 365 pour les entreprises](https://office.microsoft.com/compare-office-365-for-business-plans-FX102918419.aspx?CR_CC=200061904&WT.srch=1&WT.mc_ID=PS_bing_O365Comm_subscribe-to-office-365_Text).  
  
###  <a name="BKMK_StepTwo"></a>Étape2: Intégrer le serveur à MicrosoftOffice 365  
 Effectuez la procédure suivante sur le contrôleur de domaine pour intégrer votre serveur WindowsServerEssentials à Office 365.  
  
> [!NOTE]
>  La procédure s le même si vous disposez de WindowsServerEssentials ou WindowsServerEssentials, mais tout vous démarrez à partir d’un autre emplacement sur la page d’accueil. Dans WindowsServerEssentials, vous intégrez le serveur à Office365 et d’autres Services en ligne Microsoft sur le **Services** onglet. Dans WindowsServerEssentials, l’intégration d’Office365 est effectuée sur le **messagerie** onglet.  
  
##### <a name="to-integrate-the-server-with-office-365"></a>Pour intégrer le serveur à Office 365  
  
1.  Ouvrez une session sur le serveur en tant qu’administrateur et ouvrez le tableau de bord WindowsServerEssentials.  
  
2.  Sur le **accueil**, cliquez sur **Services** (dans WindowsServerEssentials, cliquez sur **messagerie**), cliquez sur **intégration à MicrosoftOffice 365**, puis cliquez sur **configurer l’intégration de MicrosoftOffice 365**.  
  
     L’Assistant Intégration à MicrosoftOffice 365 s’affiche.  
  
3.  Sur le **main** page, effectuez l’une des actions suivantes:  
  
    -   Si vous ne pas avoir un abonnement à Office 365, cliquez sur **suivant**, suivez les instructions pour vous abonner à Office 365 ou signe pour un abonnement d’évaluation.  
  
         Vous devez connecter à Office 365 avant de retourner à l’Assistant. Mais vous n’avez pas besoin d’effectuer les tâches décrites dans le **Démarrer ici** section du portail Office 365.  
  
    -   Si vous avez déjà un abonnement à Office 365 et que vous souhaitez intégrer avec le serveur, sélectionnez **j’ai déjà un abonnement pour Office 365**, puis cliquez sur **suivant**.  
  
4.  Suivez les instructions pour terminer l’Assistant.  
  
 Une fois l’Assistant se termine correctement, tout vous notez les modifications suivantes au tableau de bord:  
  
-   Cet emplacement s une nouvelle **Office 365** page, qui est utilisé pour gérer l’intégration et votre abonnement Office 365.  
  
-   Sur le **utilisateurs** page, vous voyez un **comptes en ligne** onglet où vous pouvez créer et gérer les comptes MicrosoftOnline Services qui permettent à vos utilisateurs d’accéder à Office 365. Si vous utilisez Exchange Online et disposer d’un serveur WindowsServerEssentials, vous également voyez un **groupes de Distribution** onglet.  
  
-   Le **stockage** page sur un serveur WindowsServerEssentials a un **bibliothèques SharePoint** onglet pour gérer les bibliothèques SharePoint Online et modification des autorisations de vos sites d’équipe. Chaque offre commerciale d’Office 365 inclut les fonctionnalités de SharePoint Online de base.  
  
###  <a name="BKMK_StepThree"></a>Étape3: Relier votre nom de domaine Internet s d’organisation à Office 365 (facultatif)  
 Si vous souhaitez utiliser votre propre domaine Internet dans les messages électroniques adressés à votre organisation et les URL pour vos ressources SharePoint Online, vous pouvez relier un domaine personnalisé à votre abonnement Office 365. Si vous intégrez votre serveur WindowsServerEssentials à Office 365, vous pouvez le faire à partir du tableau de bord.  
  
 Il s plus facile de le faire avant de créer en ligne des comptes pour vos utilisateurs afin que vous pouvez utiliser le domaine lorsque vous créez en bloc les comptes en ligne.  
  
 Vous obtenez un nom de domaine lorsque vous vous inscrivez à Office365? par exemple, *contoso*. onmicrosoft.com. Si vous d plutôt utiliser un nom de domaine différent? dites simplement contoso.com? vous le pouvez. Vous devez acheter un nom de domaine si vous n’avez pas déjà propre et modifier certains enregistrements DNS.  
  
 Configuration d’un domaine personnalisé à utiliser avec Office 365 implique quatre tâches:  
  
1.  **Acheter un nom de domaine.** Autrement dit, enregistrez-le auprès d’un bureau d’enregistrement de domaine ou un fournisseur d’hébergement DNS.  
  
    -   Choisissez un nom de domaine qui fonctionne avec Office 365. Vous pouvez utiliser un nom de domaine de niveau 2? par exemple, buycontoso.com?, mais pas un nom de domaine de niveau 3? par exemple, marketing.contoso.com. Pour plus d’informations sur le choix d’un domaine à utiliser dans Office365, voir [domaines](https://technet.microsoft.com/library/office-365-domains.aspx).  
  
    -   Achetez-le auprès d’un bureau d’enregistrement de domaine qui autorise les enregistrements (DNS) du serveur de nom de domaine requis par Office 365. Pour savoir quels bureaux d’enregistrement autorise les enregistrements DNS requis, consultez [comment acheter un nom de domaine](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660). Si vous déjà inscrit votre domaine avec un autre bureau d’enregistrement, n’avez inquiétez; vous pouvez transférer le domaine à un autre bureau d’enregistrement lorsque vous reliez le domaine à Office 365.  
  
2.  **Configurer les enregistrements DNS qui permettent aux services Office 365 utiliser le nom de domaine.** Le moyen le plus simple consiste à laisser l’Assistant Configurer les enregistrements DNS lorsque vous reliez le domaine à votre abonnement Office 365 à l’étape3. Si vous d le faire vous-même, consultez [comment configurer manuellement le service DNS des enregistrements pour l’intégration d’Office 365](#BKMK_ManuallyConfigureDNS).  
  
3.  **Lier votre domaine Internet personnalisé à votre abonnement Office 365.** Vous utilisez tout le **relier un domaine à Office 365** action.  
  
4.  **Vérifiez que vos services Office 365 utilisent le nouveau nom de domaine.**  
  
 Si vous effectuez les étapes1 et 2 avant d’utiliser le **relier un domaine à Office 365** tâche, l’Assistant peut relier le nom de domaine à Office 365. Vous pouvez également avoir un Assistant vous aider avec certaines ou toutes les étapes1 et 2.  
  
##### <a name="to-link-your-organization-s-internet-domain-to-office-365"></a>Pour relier votre domaine Internet de l’organisation s à Office 365  
  
1.  Sur le tableau de bord, ouvrez le **Office 365** page, puis cliquez sur **relier un domaine à Office 365**.  
  
2.  Suivez les instructions pour terminer l’Assistant.  
  
     L’Assistant peut vous aider avec certaines ou toutes les étapes pour l’inscription, la configuration et la liaison d’un nom de domaine Internet nouveau ou existant à utiliser dans Office 365.  
  
     Cliquez sur le lien d’aide sur la page de l’Assistant pour obtenir les informations que vous avez besoin effectuer une tâche. Ou consultez l’une section du nom de domaine de configurer [gérer l’accès Web à distance dans WindowsServerEssentials](https://technet.microsoft.com/library/jj628152\(d=printer\).aspx) pour une vue d’ensemble du processus et la configuration requise.  
  
    > [!NOTE]
    >  Pour utiliser l’Assistant pour enregistrer un nouveau nom de domaine, vous devez utiliser un des fournisseurs de services de nom de domaine en partenariat avec Microsoft pour fournir une intégration transparente avec l’Assistant. Pour rechercher un bureau d’enregistrement de noms de domaine, voir [comment acheter un nom de domaine](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660).  
  
3.  Si l’Assistant détecte que votre nom de domaine n’est pas géré par le serveur, vous devez configurer manuellement les enregistrements DNS requis pour terminer la configuration. Pour obtenir des instructions, voir [comment configurer manuellement le service DNS des enregistrements pour l’intégration d’Office 365](#BKMK_ManuallyConfigureDNS), plus loin dans cette rubrique.  
  
4.  Vérifiez que le domaine est utilisé dans Office 365.  
  
     Il existe un peu d’attente une fois l’Assistant terminé, tandis que le Registre des noms de domaine vérifie les enregistrements DNS de s. Cela se produit automatiquement; don t avez rien à faire. Mais il prend généralement environ une heure? et parfois un peu plus. Lors de la vérification du domaine terminée, le **Office 365** page répertorie le domaine de votre organisation s.  
  
####  <a name="BKMK_ManuallyConfigureDNS"></a>Comment configurer manuellement les enregistrements DNS pour l’intégration d’Office 365  
 Si le lien Assistant votre domaine à Office 365 détecte que votre nom de domaine n’est pas géré par le serveur, pour terminer la configuration, vous devez configurer manuellement les enregistrements de serveur (DNS) de nom de domaine requis. Dans ce cas, tout vous trouver une liste des enregistrements DNS que vous devez configurer au **%username%\NewDNSRecords_ (n) .txt**, où *(n)* est un nombre aléatoire.  
  
 Le tableau suivant décrit les enregistrements DNS que vous devez ajouter. Les méthodes d’entrée peuvent varier avec l’enregistrement de domaines différents. Si vous avez des questions, demandez à votre bureau d’enregistrement de nom du domaine de l’aide.  
  
### <a name="required-dns-records-for-linking-a-custom-internet-domain-name-to-office-365"></a>Enregistrements DNS requis pour la liaison d’un nom de domaine Internet personnalisé à Office 365  
  
|Service|Enregistrements DNS requis|Objectif|  
|-------------|--------------------------|-------------|  
|(Plusieurs services)|MX| Office 365 utilise cet enregistrement pour vérifier que vous possédez un nom de domaine spécifique. Cet enregistrement MX n’interfère pas avec routage de messages électroniques.|  
|Exchange Online|MX|Fournit le routage de messages électroniques. **Important:** si vous ne migrez la messagerie électronique, n’attribuez pas une préférence de zéro (**0**) vers le nouvel enregistrement MX. Assurez-vous que la valeur de l’enregistrement est supérieure à la valeur qui est attribuée à l’enregistrement MX actif. Lors de la migration de courrier électronique est terminée et que vous êtes prêt à modifier le serveur de messagerie à Office 365, avoir votre bureau d’enregistrement nom de domaine de réinitialiser la valeur de préférence du nouvel enregistrement MX.|  
|Exchange Online|Alias (CNAME)|Découvrez automatiquement l’enregistrement qui est utilisé pour aider les utilisateurs facilement configurer une connexion entre Exchange Online et leur client de bureau Outlook ou un client de messagerie mobile. **Remarque:** si vous préférez accéder à Outlook Web Access en utilisant le nom de domaine de votre organisation s (par exemple, http://mail.contoso.com) au lieu de l’URL standard (https://outlook.com/owa/office365.com), vous pouvez configurer l’enregistrement d’Alias (CName) comme suit: **Type = CNAME, TTL = 01: 00:00, nom d’hôte = adresse de messagerie = mail.office365.com**|  
|Exchange Online|TXT|Spécifie que outlook.com, le domaine utilisé par les serveurs de messagerie Office 365, est autorisé à envoyer un courrier électronique au nom de votre domaine. Créez cet enregistrement pour empêcher que votre courrier électronique sortant soit marqué comme du courrier indésirable.|  
|Lync Online|SRV|Permet d’activer la fédération avec d’autres services de messagerie instantanée tels que Windows Live ou Yahoo!.|  
|Lync Online|SRV|Découvrez automatiquement l’enregistrement qui est utilisé pour aider les utilisateurs facilement configurer une connexion entre le client de bureau Lync et MicrosoftLync Online.|  
  
> [!IMPORTANT]
>  Domaine après la vérification est terminée, n’essayez pas d’ajouter ou apporter d’autres modifications aux enregistrements DNS à partir du portail Office 365.  
  
###  <a name="BKMK_StepFour_ACCOUNTS"></a>Étape suivante  
  
-   Créez des comptes MicrosoftOnline Services pour vos utilisateurs.  
  
     Pour utiliser les services Office 365, vos utilisateurs doivent disposer d’un compte MicrosoftOnline Services associé à leur compte d’utilisateur réseau. Le tableau de bord facilite cette opération. Si vous utilisez un nouvel abonnement Office 365, vous pouvez créer en bloc des comptes en ligne pour vos comptes d’utilisateur existant. Si vous faites l’intégration d’un nouveau serveur à un abonnement Office 365 que vous utilisez déjà, vous pouvez importer les comptes d’utilisateurs à partir en ligne existants comptes. Pour connaître les procédures, voir [gérer les comptes en ligne pour les utilisateurs](Manage-Online-Accounts-for-Users.md).  
  
> [!NOTE]
>  Sur le tableau de bord dans WindowsServerEssentials, les comptes MicrosoftOnline Services sont appelés comptes Office 365. Les comptes sont identiques; seule la terminologie modifiée.  
  
##  <a name="BKMK_ManageIntegration"></a>Gérer l’intégration d’Office 365  
 Après avoir intégré votre serveur à Office 365, le **Office 365** page du tableau de bord affiche des informations concernant votre abonnement Office 365 et rend ces tâches disponibles:  
  
-   [Gérer votre abonnement Office 365](#BKMK_ManageO365)? Modifier le compte d’administrateur que vous utilisez pour gérer l’abonnement. Ouvrez le tableau de bord Office 365 Admin pour gérer votre abonnement.  
  
-   [Relier votre domaine Internet de l’organisation s à Office 365](#BKMK_StepThree)? Si vous voulez être en mesure d’envoyer et recevoir des messages électroniques adressés à votre domaine, vous pouvez lier le domaine à Office 365. (Opération décrite ultérieurement, à [étape3: relier le domaine de votre organisation s à Office 365](#BKMK_StepThree).)  
  
-   [Désactiver l’intégration d’Office 365](#BKMK_Disable)? Si vous ne pas souhaitez gérer vos services Office 365, les abonnements et les comptes en ligne à partir du tableau de bord, vous pouvez désactiver l’intégration d’Office 365. Les services sont toujours disponibles sur le portail Office 365.  
  
###  <a name="BKMK_ManageO365"></a>Gérer votre abonnement Office 365  
 Si vous avez besoin apporter des modifications à votre abonnement Office 365 pendant que vous re travailler sur le serveur, vous pouvez ouvrir l’abonnement dans Office 365 à partir de la **Office 365** page du tableau de bord. Vous pouvez également modifier le compte administrateur que le serveur utilise pour apporter des modifications aux services Office 365.  
  
##### <a name="to-open-your-subscription-on-the-office-365-admin-dashboard"></a>Pour ouvrir votre abonnement dans le tableau de bord d’administrateur Office 365  
  
1.  Sur le tableau de bord WindowsServerEssentials, ouvrez le **Office 365** page.  
  
2.  Dans **tâches de Configuration**, cliquez sur **gérer Office 365**.  
  
3.  Connectez-vous à Office 365 avec le compte en ligne Microsoft que vous utilisez pour gérer votre abonnement.  
  
     Le tableau de bord d’administrateur Office 365 s’ouvre.  
  
##### <a name="to-change-the-office-365-administrator-account"></a>Pour modifier le compte d’administrateur Office 365  
  
1.  Dans le tableau de bord, cliquez sur **Office 365**.  
  
2.  Dans **tâches de Configuration**, cliquez sur **modifier le compte d’administrateur Office 365**. L’Assistant Modification du compte d’administrateur s’affiche. (Dans WindowsServerEssentials, l’Assistant s’appelle configurer le Office 365 compte d’administrateur).  
  
3.  Tapez les informations d’identification pour le compte que vous souhaitez utiliser pour se connecter à votre abonnement Office 365, puis cliquez sur **suivant**.  
  
4.  Cliquez sur **fermer **. Le tableau de bord redémarre.  
  
###  <a name="BKMK_Disable"></a>Désactiver l’intégration d’Office 365  
 Si vous décidez que vous n’avez pas à gérer vos services Office 365 et les comptes en ligne dans le tableau de bord, vous pouvez désactiver l’intégration d’Office 365. Votre abonnement Office 365 reste actif et toutes les modifications de configuration effectuées à partir du tableau de bord restent en vigueur. Par exemple, vous lle recevoir des messages électroniques adressés à un nom de domaine que vous avez relié à votre abonnement Office 365. Vous ne pas perdre tous les e-mails et les contrôles que vous définissez pour les périphériques mobiles sont toujours utilisés dans Exchange Online.  
  
 À l’avenir, vous allez gérer votre abonnement Office 365, services et ressources d’Office 365 et vos utilisateurs devront gérer les mots de passe de leurs comptes en ligne dans Office 365. Synchronisation de mot de passe ne s’effectue plus, et la désactivation ou la suppression d’un compte d’utilisateur n’a aucun effet sur le compte utilisateur en ligne.  
  
 Étant donné que le logiciel d’intégration d’Office 365 est installé sur le serveur local, vous pouvez désactiver la fonctionnalité même si le service d’intégration ne peut pas se connecter à Office 365.  
  
##### <a name="to-disable-office-365-integration-with-the-server"></a>Pour désactiver l’intégration d’Office 365 avec le serveur  
  
1.  Dans le tableau de bord, cliquez sur **Office 365**.  
  
2.  Cliquez sur **désactiver l’intégration d’Office 365**. L’Assistant désactiver Office 365 s’affiche.  
  
3.  Suivez les instructions pour terminer l’Assistant.  
  
> [!NOTE]
>  Pour activer l’intégration d’Office 365 à nouveau, utilisez le **intégration à Office 365** de tâches sur le **Service** onglet du tableau de bord s **accueil** page. Pour obtenir des instructions, voir [étape2: intégrer votre serveur WindowsServerEssentials à MicrosoftOffice 365](#BKMK_StepTwo), plus haut dans cette rubrique.  
  
##  <a name="BKMK_Troubleshoot"></a>Résoudre les problèmes d’intégration d’Office 365  
 Cette section fournit des informations pour vous aider à résoudre les problèmes courants que vous pouvez rencontrer lorsque vous utilisez les fonctionnalités d’intégration Office 365dans WindowsServerEssentials.  
  
###  <a name="BKMK_AcctsNotCreated"></a>Certains comptes MicrosoftOnline Services n’ont pas été créés.  
 **Description**  
  
 Une tentative de création d’un ou plusieurs comptes MicrosoftOnline Services à partir du tableau de bord n’était pas t réussie.  
  
 **Solution**  
  
1.  Cliquez sur le lien dans la page Fin Assistant pour ouvrir un fichier de résultats qui contient des informations détaillées sur chaque demande de création de compte n’a pas abouti. Par exemple, un résultat peut vous informer qu’un compte MicrosoftOnline Services existe déjà avec le nom d’un compte demandé.  
  
2.  Effectuez les actions recommandées pour résoudre chaque erreur.  
  
3.  Si le problème persiste, redémarrez le serveur et puis essayez à nouveau de créer les comptes en ligne.  
  
###  <a name="BKMK_ProblemUninstalling"></a>Il y a eu un problème de désinstallation de l’intégration d’Office 365  
 **Description**  
  
 Une erreur inconnue s’est produite lorsque vous avez tenté de désactiver l’intégration d’Office 365.  
  
 **Solution**  
  
1.  Assurez-vous que l’ordinateur est connecté à Internet, puis essayez à nouveau.  
  
2.  Si l’erreur se reproduit, redémarrez le serveur et réessayez.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Vue d’ensemble de l’intégration de services pour WindowsServerEssentials - partie 1](http://blogs.technet.com/b/sbs/archive/2013/11/04/services-integration-overview-for-windows-server-2012-r2-essentials-part-1.aspx)  
  
-   [Vue d’ensemble de l’intégration de services pour WindowsServerEssentials - partie 2](http://blogs.technet.com/b/sbs/archive/2013/11/06/services-integration-overview-for-windows-server-2012-r2-essentials-part-2.aspx)  
  
-   [Guide de démarrage rapide à l’aide de MicrosoftOffice 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md)  
  
-   [Gérer les Services en ligne Microsoft](../manage/Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)  
  
-   [Gérer WindowsServerEssentials](../manage/Manage-Windows-Server-Essentials.md)
