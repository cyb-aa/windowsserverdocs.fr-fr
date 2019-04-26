---
title: Gérer Office 365 dans Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860500"
---
# <a name="manage-office-365-in-windows-server-essentials"></a>Gérer Office 365 dans Windows Server Essentials

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Lorsque vous intégrez votre serveur Windows Server Essentials à Microsoft Office 365, vous pouvez gérer vos services Office 365 et les comptes en ligne, ainsi que vos ressources locales à partir du tableau de bord Windows Server Essentials. Dans cette rubrique, vous trouverez ce que vous pouvez obtenir en intégrant votre serveur Office 365, comment le faire et comment gérer et dépanner votre intégration d’Office 365.  
  
  
  
> [!IMPORTANT]
>   Intégration d’Office 365 est uniquement pris en charge dans un environnement de contrôleur de domaine unique. En outre, l’Assistant d’intégration Office 365 doit s’exécuter sur un contrôleur de domaine.  
  
## <a name="in-this-topic"></a>Dans cette rubrique  
  
-   [Pourquoi dois-je intégrer Office 365 avec mon serveur ?](#BKMK_IntegrationOverview)  
  
-   [Configurer l’intégration d’Office 365](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Configure)  
  
-   [Gérer l’intégration d’Office 365](#BKMK_ManageIntegration)  
  
-   [Résoudre les problèmes d’intégration d’Office 365](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Troubleshoot)  
  
##  <a name="BKMK_IntegrationOverview"></a> Pourquoi dois-je intégrer Office 365 avec mon serveur ?  
 Il existe de nombreuses raisons valables d’intégrer Office 365 à votre serveur Windows Server Essentials. Si vous gérez certaines de vos ressources internes mais que vous utilisez Office 365 pour les autres services, vous serez en mesure de gérer vos services Office 365 et les ressources du tableau de bord, ainsi que vos ressources locales, au lieu de travailler dans deux emplacements.  
  
-   Gérer les comptes en ligne qui permettent à vos utilisateurs un accès à Office 365 avec vos comptes d’utilisateur :  
  
    -   Créez des comptes Microsoft Online Services pour vos utilisateurs en une seule étape, ou créez des comptes d'utilisateur sur le serveur pour vos comptes en ligne existants. Vous pouvez également ajouter un compte en ligne à un compte d'utilisateur nouveau ou existant.  
  
         Lorsque vous créez les comptes en ligne du tableau de bord, les utilisateurs se connecter à Office 365 avec le même mot de passe qu’ils utilisent sur le serveur. S'ils changent le mot de passe de leur compte d'utilisateur, le mot de passe en ligne change également. Par ailleurs, vous savez que leurs mots de passe en ligne répondent toujours aux exigences de sécurité que vous définissez pour vos comptes d'utilisateur.  
  
    -   Gérez un compte en ligne avec le compte d'utilisateur tout au long du cycle de vie du compte d'utilisateur. Si vous désactivez un compte d'utilisateur, le compte en ligne est également désactivé. Si vous supprimez un compte d'utilisateur, le compte en ligne est également supprimé.  
  
    -   Sur un serveur Windows Server Essentials, gérez également des groupes de distribution Exchange Online pour le courrier électronique.  
  
-   Envoyer et recevoir du courrier électronique à partir de votre domaine Internet d’entreprise s (par exemple, contoso.com) en liant un domaine Internet personnalisé à votre abonnement Office 365.  
  
-   Gérer votre abonnement et l’intégration d’Office 365 à partir du tableau de bord.  
  
-   Si votre abonnement inclut des bibliothèques SharePoint Online, intégration d’Office 365 avec un serveur Windows Server Essentials vous permet de :  
  
    -   Créez et gérez vos bibliothèques SharePoint Online à partir du tableau de bord.  
  
        > [!NOTE]
        >  Vous allez également pouvoir utiliser l’application My Server 2012 R2 pour travailler avec des documents dans vos bibliothèques SharePoint Online à partir de votre ordinateur portable, appareil mobile ou Windows phone. Cette fonctionnalité est uniquement disponible pour Windows Server Essentials. Pour plus d’informations, consultez [utiliser l’application My Server](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md).  
  
    -   Modifier les autorisations pour un site d’équipe SharePoint Online à partir du tableau de bord, ou ouvrez le site d’équipe à partir du tableau de bord pour apporter d’autres modifications.  
  
     Pour plus d’informations, consultez [gérer SharePoint Online](Manage-SharePoint-Online-in-Windows-Server-Essentials.md).  
  
-   Si vous vous abonnez à Exchange Online, intégration d’Office 365 avec un serveur Windows Server Essentials vous permet de gérer les appareils mobiles que vos utilisateurs utilisent pour se connecter à votre serveur de messagerie d’entreprise :  
  
    -   Demandez la protection par mot de passe lorsque les appareils mobiles se connectent au serveur de messagerie de votre entreprise. Définissez une longueur de mot de passe minimale, le nombre de tentatives de connexion ayant échoué autorisées et la durée minimale entre les tentatives de connexion.  
  
    -   Empêchez un appareil mobile de se connecter à Exchange Online si vous avez connaissance de problèmes de sécurité avec le modèle de l'appareil.  
  
    -   Si un appareil mobile est perdu ou volé, effacez l'appareil pour supprimer toutes les données sensibles de l'entreprise la prochaine fois qu'il sera mis sous tension.  
  
-    Intégration d’Office 365 offre de nouvelles méthodes pour se connecter aux ressources et aux services Office 365 :  
  
    -   Ouvrez les services Office 365 à partir de Launchpad de Windows Server Essentials. Pour plus d’informations, consultez [Quick Start Guide to Using Microsoft Office 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md).  
  
    -   Utilisez l’application My Server 2012 R2 pour travailler avec des documents dans vos bibliothèques SharePoint Online à partir de votre ordinateur portable, appareil mobile ou Windows phone. Pour plus d’informations, consultez [utiliser l’application My Server](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md). Cette fonctionnalité est uniquement disponible dans Windows Server Essentials.  
  
##  <a name="BKMK_Configure"></a> Configurer l’intégration d’Office 365  
 Vous pouvez intégrer votre serveur avec Office 365 à tout moment après avoir terminé l’installation du serveur. Si vous n t disposez déjà d’un abonnement Office 365, vous pouvez acheter un ou souscrire un abonnement d’essai gratuit.  
  
 Vous exécuterez les tâches suivantes :  
  
-   [Étape 1 : Vérifier les conditions d’intégration Office 365](#BKMK_StepOne_VERIFY)  
  
-   [Étape 2 : Intégrer le serveur à Microsoft Office 365](#BKMK_StepTwo)  
  
-   [Étape 3 : Relier votre nom de domaine Internet organisation s à Office 365 (facultatif)](#BKMK_StepThree)  
  
###  <a name="BKMK_StepOne_VERIFY"></a> Étape 1 : Vérifier les conditions d’intégration Office 365  
 Avant de commencer, assurez-vous que le serveur répond aux exigences suivantes :  
  
-   Le serveur peut disposer de l'un des systèmes d'exploitation suivants :  Windows Server Essentials, Windows Server Essentials ou le système d’exploitation Windows Server 2012 R2 Standard ou Windows Server 2012 R2 Datacenter avec le rôle expérience Windows Server Essentials installé.  
  
-   L’environnement ne peut comporter un contrôleur de domaine, et vous devez effectuer l’intégration d’Office 365 sur le contrôleur de domaine.  
  
-   Vous devez être en mesure de vous connecter à Internet à partir du serveur.  
  
-   Avant de commencer l’intégration Office 365, vous devez installer les mises à jour critiques et importantes sur le serveur.  
  
-   Si vous souhaitez utiliser votre domaine Internet d’organisation s dans les adresses de messagerie et avec vos ressources SharePoint Online, vous devez inscrire le nom de domaine afin de pouvoir relier le domaine à Office 365 lors de l’intégration. Pour plus d’informations, consultez [Procédure à suivre pour acheter un nom de domaine](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660).  
  
> [!NOTE]
>  Vous n’avez pas besoin pour vous abonner à Office 365 à l’avance. Vous serez en mesure d’acheter un abonnement ou vous inscrire pour un essai gratuit pendant l’intégration d’Office 365. Si vous d comme jeter un coup de œil offres et la tarification pour Office 365, [comparer les offres Office 365 pour les entreprises](https://office.microsoft.com/compare-office-365-for-business-plans-FX102918419.aspx?CR_CC=200061904&WT.srch=1&WT.mc_ID=PS_bing_O365Comm_subscribe-to-office-365_Text).  
  
###  <a name="BKMK_StepTwo"></a> Étape 2 : Intégrer le serveur à Microsoft Office 365  
 Effectuez la procédure suivante sur le contrôleur de domaine pour intégrer votre serveur Windows Server Essentials à Office 365.  
  
> [!NOTE]
>  La procédure s le même si vous disposez de Windows Server Essentials ou Windows Server Essentials, mais vous allez démarrer à partir d’un autre emplacement de la page d’accueil. Dans Windows Server Essentials, vous intégrez le serveur avec Office 365 et d’autres Services en ligne Microsoft sur le **Services** onglet. Dans Windows Server Essentials, intégration d’Office 365 est effectuée sur le **E-mail** onglet.  
  
##### <a name="to-integrate-the-server-with-office-365"></a>Pour intégrer le serveur à Office 365  
  
1.  Connectez-vous sur le serveur en tant qu’administrateur, puis ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Sur le **accueil** , cliquez sur **Services** (dans Windows Server Essentials, cliquez sur **E-mail**), cliquez sur **intégration à Microsoft Office 365**, puis cliquez sur **configurer l’intégration de Microsoft Office 365**.  
  
     L'Assistant Intégration à Microsoft Office 365 s'affiche.  
  
3.  Dans la page **Commencer** , effectuez l'une des actions suivantes :  
  
    -   Si vous ne pas disposer d’un abonnement à Office 365, cliquez sur **suivant**et suivez les instructions pour s’abonner à Office 365 ou de connexion d’un abonnement d’évaluation.  
  
         Vous devez vous connecter à Office 365 avant de retourner à l’Assistant. Mais vous n’avez pas besoin d’effectuer les tâches décrites dans le **Démarrer ici** section du portail Office 365.  
  
    -   Si vous avez déjà un abonnement à Office 365 que vous souhaitez intégrer au serveur, sélectionnez **j’ai déjà un abonnement pour Office 365**, puis cliquez sur **suivant**.  
  
4.  Suivez les instructions pour exécuter l'Assistant.  
  
 Une fois l’Assistant terminé, vous allez remarquer les modifications suivantes au tableau de bord :  
  
-   Cet emplacement s un nouveau **Office 365** page, qui est utilisé pour gérer l’intégration et votre abonnement Office 365.  
  
-   Sur le **utilisateurs** page, vous voyez un **comptes en ligne** onglet où vous pouvez créer et gérer les comptes Microsoft Online Services qui permettent à vos utilisateurs d’accéder à Office 365. Si vous utilisez Exchange Online et vous disposez d’un serveur Windows Server Essentials, vous allez également voir un **groupes de Distribution** onglet.  
  
-   Le **stockage** page sur un serveur Windows Server Essentials a un **bibliothèques SharePoint** onglet pour la gestion des bibliothèques SharePoint Online et la modification des autorisations pour vos sites d’équipe. Chaque offre commerciale d’Office 365 inclut les fonctionnalités de SharePoint Online de base.  
  
###  <a name="BKMK_StepThree"></a> Étape 3 : Relier votre nom de domaine Internet organisation s à Office 365 (facultatif)  
 Si vous souhaitez utiliser votre propre domaine Internet dans un message électronique adressé à votre organisation et les URL pour vos ressources SharePoint Online, vous pouvez relier un domaine personnalisé à votre abonnement Office 365. Si vous intégrez votre serveur Windows Server Essentials à Office 365, nous vous conseillons de vous faire à partir du tableau de bord.  
  
 Il s le plus simple pour ce faire avant de créer en ligne des comptes pour vos utilisateurs afin de pouvoir utiliser le domaine lorsque vous créer en bloc les comptes en ligne.  
  
 Vous obtenez un nom de domaine lorsque vous vous inscrivez pour Office 365 ? par exemple, *contoso*. onmicrosoft.com. Si vous d plutôt utiliser un autre nom de domaine ? par exemple, contoso.com simplement ? vous pouvez. Vous devez acheter un nom de domaine si vous n t déjà propre et modifier certains des enregistrements DNS.  
  
 Configuration d’un domaine personnalisé à utiliser avec Office 365 implique quatre tâches :  
  
1.  **Achetez un nom de domaine.** Autrement dit, enregistrez-le auprès d'un bureau d'enregistrement de domaines ou d'un fournisseur d'hébergement DNS.  
  
    -   Choisir un nom de domaine qui fonctionne avec Office 365. Vous pouvez utiliser un nom de domaine de 2e niveau ? buycontoso.com, par exemple, ?, mais pas un nom de domaine de 3e niveau ? par exemple, marketing.contoso.com. Pour plus d’informations sur le choix d’un domaine à utiliser dans Office 365, consultez [domaines](https://technet.microsoft.com/library/office-365-domains.aspx).  
  
    -   Achetez-le auprès d’un bureau d’enregistrement de domaine qui autorise les enregistrements de serveur (DNS) de nom de domaine requis par Office 365. Pour savoir quel bureau d’enregistrement de domaines autorise les enregistrements DNS requis, consultez [Comment acheter un nom de domaine](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660). Si vous déjà inscrit votre domaine avec un autre bureau d’enregistrement, n inquiétez ; Lorsque vous reliez le domaine à Office 365, vous pouvez transférer le domaine à un autre bureau d’enregistrement.  
  
2.  **Configurez des enregistrements DNS qui autorisent les services Office 365 à utiliser le nom de domaine.** Le moyen le plus simple consiste à laisser l’Assistant Configurer les enregistrements DNS pour vous lorsque vous reliez le domaine à votre abonnement Office 365 à l’étape 3. Si vous d le faire vous-même, consultez [comment configurer manuellement le service DNS des enregistrements pour l’intégration d’Office 365](#BKMK_ManuallyConfigureDNS).  
  
3.  **Liez votre domaine Internet personnalisé à votre abonnement Office 365.** Vous utilisez ll le **relier un domaine à Office 365** action.  
  
4.  **Vérifiez que vos services Office 365 utilisent le nouveau nom de domaine.**  
  
 Si vous effectuez les étapes 1 et 2 avant d’utiliser le **relier un domaine à Office 365** tâche, l’Assistant peut relier le nom de domaine à Office 365. Un Assistant peut également vous aider à effectuer partiellement ou en totalité les étapes 1 et 2.  
  
##### <a name="to-link-your-organization-s-internet-domain-to-office-365"></a>Pour relier votre domaine Internet d’entreprise s à Office 365  
  
1.  Dans le tableau de bord, ouvrez la page **Office 365** , puis cliquez sur **Relier un domaine à Office 365**.  
  
2.  Suivez les instructions pour exécuter l'Assistant.  
  
     L’Assistant peut vous aider avec certaines ou toutes les étapes de l’inscription, la configuration et la liaison d’un nom de domaine Internet nouveau ou existant à utiliser dans Office 365.  
  
     Cliquez sur le lien d'aide affiché sur la page de l'Assistant pour obtenir les informations nécessaires à l'exécution d'une tâche. Ou consultez l’une section de nom de domaine de configurer [gérer l’accès Web à distance dans Windows Server Essentials](https://technet.microsoft.com/library/jj628152\(d=printer\).aspx) pour une vue d’ensemble du processus et la configuration requise.  
  
    > [!NOTE]
    >  Pour utiliser l'Assistant pour inscrire un nouveau nom de domaine, vous devez utiliser un des fournisseurs de noms de domaine partenaires de Microsoft pour fournir une intégration transparente à l'aide de l'Assistant. Pour rechercher un bureau d’enregistrement de domaines, consultez [Procédure à suivre pour acheter un nom de domaine](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660).  
  
3.  Si l’Assistant détecte que votre nom de domaine n’est pas géré par le serveur, vous devez configurer manuellement les enregistrements DNS requis pour terminer la configuration. Pour obtenir des instructions, voir [Comment configurer manuellement des enregistrements DNS pour l'intégration d'Office 365](#BKMK_ManuallyConfigureDNS), plus loin dans cette rubrique.  
  
4.  Vérifiez que le domaine est utilisé dans Office 365.  
  
     Il existe un peu d’attente une fois l’Assistant terminé, tandis que le bureau d’enregistrement du nom de domaine vérifie les enregistrements DNS de s. Cela se produit automatiquement ; ne pas avoir rien à faire. Mais cela prend généralement environ une heure ? et parfois un peu plus. Lors de la vérification de domaine est terminée, le **Office 365** page répertorie le domaine de votre organisation s.  
  
####  <a name="BKMK_ManuallyConfigureDNS"></a> Comment configurer manuellement les enregistrements DNS pour l’intégration d’Office 365  
 Si l'Assistant Relier votre domaine à Office 365 détecte que votre nom de domaine n'est pas géré par le serveur, vous devez, pour terminer la configuration, configurer manuellement les enregistrements DNS (Domain Name Server) requis. Dans ce cas, vous trouverez une liste d’enregistrements DNS que vous devez configurer dans **%username%\NewDNSRecords_ (n) .txt**, où *(n)* est un nombre aléatoire.  
  
 Le tableau suivant décrit les enregistrements DNS que vous devez ajouter. Les méthodes d’entrée peuvent varier en fonction des différents bureaux d’enregistrement de domaines. Si vous avez des questions, demandez de l'aide à votre bureau d'enregistrement de noms de domaine.  
  
### <a name="required-dns-records-for-linking-a-custom-internet-domain-name-to-office-365"></a>Enregistrements DNS requis pour la liaison d'un nom de domaine Internet personnalisé à Office 365  
  
|Service|Enregistrements DNS requis|Objectif|  
|-------------|--------------------------|-------------|  
|(Plusieurs services)|MX| Office 365 utilise cet enregistrement pour vérifier que vous possédez un nom de domaine spécifique. Cet enregistrement MX n'interfère pas avec le routage de messages électroniques.|  
|Exchange Online|MX|Assure le routage de messages électroniques. **Important :**  Si vous migrez la messagerie électronique, n'affectez pas zéro (**0**) comme préférence au nouvel enregistrement MX. Assurez-vous que la valeur de l'enregistrement est supérieure à celle affectée à l'enregistrement MX actif. Quand migrer la messagerie électronique est terminée et vous êtes prêt à modifier le serveur de messagerie pour Office 365, avoir votre bureau d’enregistrement du nom de domaine de réinitialiser la valeur de préférence du nouvel enregistrement MX.|  
|Exchange Online|Alias (CNAME)|Découvrez automatiquement l'enregistrement qui est utilisé pour aider les utilisateurs à configurer facilement une connexion entre Exchange Online et leur client de bureau Outlook ou un client de messagerie mobile. **Remarque :**  Si vous préférez accéder à Outlook Web Access en utilisant le nom de votre propre domaine organisation s (par exemple, http://mail.contoso.com) au lieu de l’URL standard (https://outlook.com/owa/office365.com), vous pouvez configurer l’enregistrement d’Alias (CName) comme suit : **Type=CNAME, TTL=01:00:00, HostName=mail, Address=mail.office365.com**|  
|Exchange Online|TXT|Spécifie qu’outlook.com, le domaine utilisé par les serveurs de messagerie Office 365, est autorisé à envoyer du courrier électronique au nom de votre domaine. Créez cet enregistrement pour empêcher que votre courrier électronique sortant soit marqué en tant que courrier indésirable.|  
|Lync Online|SRV|Permet d'activer la fédération avec d'autres services de messagerie instantanée tels que Windows Live ou Yahoo!.|  
|Lync Online|SRV|Découvrez automatiquement l'enregistrement qui est utilisé pour aider les utilisateurs à configurer facilement une connexion entre le client de bureau Lync et Microsoft Lync Online.|  
  
> [!IMPORTANT]
>  Après le domaine, la vérification est terminée, n’essayez pas d’ajouter ou apporter d’autres modifications aux enregistrements DNS à partir du portail Office 365.  
  
###  <a name="BKMK_StepFour_ACCOUNTS"></a> Étape suivante  
  
-   Créez des comptes Microsoft Online Services pour vos utilisateurs.  
  
     Pour utiliser les services Office 365, vos utilisateurs doivent disposer d’un compte Microsoft Online Services associé à leur compte d’utilisateur réseau. Le tableau de bord facilite cette opération. Si vous utilisez un nouvel abonnement Office 365, vous pouvez créer en bloc des comptes en ligne pour vos comptes d’utilisateur existants. Si vous faites l’intégration d’un nouveau serveur à un abonnement Office 365 que vous utilisez déjà, vous pouvez importer des comptes d’utilisateur à partir de vos en ligne des comptes. Pour connaître les procédures, consultez [gérer les comptes en ligne pour les utilisateurs](Manage-Online-Accounts-for-Users.md).  
  
> [!NOTE]
>  Sur le tableau de bord dans Windows Server Essentials, les comptes Microsoft Online Services sont appelés comptes Office 365. Les comptes sont identiques ; seule la terminologie a changé.  
  
##  <a name="BKMK_ManageIntegration"></a> Gérer l’intégration d’Office 365  
 Une fois que vous intégrez votre serveur avec Office 365, le **Office 365** page sur le tableau de bord affiche des informations sur votre abonnement Office 365 et permet d’exécuter ces tâches :  
  
-   [Gérer votre abonnement Office 365](#BKMK_ManageO365) ? Modifier le compte d’administrateur que vous utilisez pour gérer l’abonnement. Ouvrez le tableau de bord Office 365 Admin pour gérer votre abonnement.  
  
-   [Relier votre domaine Internet d’entreprise s à Office 365](#BKMK_StepThree) ? Si vous souhaitez être en mesure d’envoyer et recevoir des messages électroniques adressés à votre propre domaine, vous pouvez lier le domaine à Office 365. (Nous l’avons vu précédemment, dans [étape 3 : Reliez votre domaine de l’organisation s à Office 365](#BKMK_StepThree).)  
  
-   [Désactiver l’intégration d’Office 365](#BKMK_Disable) ? Si vous ne pas souhaitez gérer vos services Office 365, abonnement et les comptes en ligne du tableau de bord, vous pouvez désactiver l’intégration d’Office 365. Les services sont toujours disponibles sur le portail Office 365.  
  
###  <a name="BKMK_ManageO365"></a> Gérer votre abonnement Office 365  
 Si vous avez besoin apporter des modifications à votre abonnement Office 365 pendant que vous re travailler sur le serveur, vous pouvez ouvrir l’abonnement dans Office 365 à partir de la **Office 365** page du tableau de bord. Vous pouvez également modifier le compte d’administrateur que le serveur utilise pour apporter des modifications aux services Office 365.  
  
##### <a name="to-open-your-subscription-on-the-office-365-admin-dashboard"></a>Pour ouvrir votre abonnement dans le tableau de bord Administrateur d'Office 365  
  
1.  Sur le tableau de bord Windows Server Essentials, ouvrez le **Office 365** page.  
  
2.  Dans **Tâches Configuration**, cliquez sur **Gérer Office 365**.  
  
3.  Connectez-vous à Office 365 avec le compte en ligne Microsoft que vous utilisez pour gérer votre abonnement.  
  
     Le tableau de bord d’administrateur Office 365 s’ouvre.  
  
##### <a name="to-change-the-office-365-administrator-account"></a>Pour modifier le compte Administrateur Office 365  
  
1.  Dans le tableau de bord, cliquez sur **Office 365**.  
  
2.  Dans **Tâches Configuration**, cliquez sur **Changer de compte Administrateur Office 365**. L'Assistant Changer de compte Administrateur s'affiche. (Dans Windows Server Essentials, l’Assistant s’appelle configurer le Office 365 compte d’administrateur).  
  
3.  Tapez les informations d’identification pour le compte que vous souhaitez utiliser pour se connecter à votre abonnement Office 365, puis cliquez sur **suivant**.  
  
4.  Cliquez sur **Fermer**. Le tableau de bord redémarre.  
  
###  <a name="BKMK_Disable"></a> Désactiver l’intégration d’Office 365  
 Si vous décidez que vous n’avez pas à gérer vos services Office 365 et les comptes en ligne du tableau de bord, vous pouvez désactiver l’intégration d’Office 365. Votre abonnement Office 365 reste actif et les modifications de configuration effectuées à partir du tableau de bord restent en vigueur. Par exemple, vous recevrez un message électronique adressé à un nom de domaine que vous avez relié à votre abonnement Office 365. Vous ne voyez pas t perdez aucun message électronique, et les contrôles que vous définissez pour les appareils mobiles sont toujours utilisés dans Exchange Online.  
  
 À l’avenir, vous allez gérer votre abonnement Office 365, les services et les ressources d’Office 365 et vos utilisateurs devront gérer les mots de passe pour leurs comptes en ligne dans Office 365. Synchronisation de mot de passe ne s’effectue plus, et la désactivation ou suppression d’un compte d’utilisateur aura aucun effet sur le compte utilisateur en ligne.  
  
 Étant donné que le logiciel d’intégration d’Office 365 est installé sur le serveur local, vous pouvez désactiver la fonctionnalité même si le service d’intégration ne peut pas se connecter à Office 365.  
  
##### <a name="to-disable-office-365-integration-with-the-server"></a>Pour désactiver l'intégration d'Office 365 avec le serveur  
  
1.  Dans le tableau de bord, cliquez sur **Office 365**.  
  
2.  Cliquez sur **Désactiver l'intégration d'Office 365**. L'Assistant Désactiver l'intégration d'Office 365 s'affiche.  
  
3.  Suivez les instructions pour exécuter l'Assistant.  
  
> [!NOTE]
>  Pour activer l’intégration d’Office 365, utilisez la **intégration à Office 365** de tâches sur le **Service** onglet du tableau de bord s **accueil** page. Pour obtenir des instructions, consultez [étape 2 : Intégrer votre serveur Windows Server Essentials à Microsoft Office 365](#BKMK_StepTwo), plus haut dans cette rubrique.  
  
##  <a name="BKMK_Troubleshoot"></a> Résoudre les problèmes d’intégration d’Office 365  
 Cette section fournit des informations pour vous aider à résoudre les problèmes courants que vous pouvez rencontrer lorsque vous utilisez les fonctionnalités d’intégration Office 365 dans Windows Server Essentials.  
  
###  <a name="BKMK_AcctsNotCreated"></a> Certains comptes Microsoft Online Services n’ont pas été créés.  
 **Description**  
  
 Une tentative de création d’un ou plusieurs comptes Microsoft Online Services à partir du tableau de bord n’a pas été t réussie.  
  
 **Solution**  
  
1.  Cliquez sur le lien figurant sur la page de fin de l'Assistant pour ouvrir un fichier de résultats qui contient des informations détaillées sur chaque demande de création de compte ayant échoué. Par exemple, un résultat peut vous informer qu'un compte Microsoft Online Services portant le nom d'un compte demandé existe déjà.  
  
2.  Effectuez les actions recommandées pour résoudre chaque erreur.  
  
3.  Si le problème persiste, redémarrez le serveur, puis essayez à nouveau de créer les comptes en ligne.  
  
###  <a name="BKMK_ProblemUninstalling"></a> Un problème est survenu désinstallation d’intégration d’Office 365  
 **Description**  
  
 Une erreur inconnue s’est produite lors de la tentative de désactivation d’intégration d’Office 365.  
  
 **Solution**  
  
1.  Assurez-vous que l'ordinateur est connecté à Internet, puis réessayez.  
  
2.  Si l'erreur se reproduit, redémarrez le serveur, puis réessayez.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Services Integration Overview for Windows Server Essentials - partie 1](http://blogs.technet.com/b/sbs/archive/2013/11/04/services-integration-overview-for-windows-server-2012-r2-essentials-part-1.aspx)  
  
-   [Services Integration Overview for Windows Server Essentials - partie 2](http://blogs.technet.com/b/sbs/archive/2013/11/06/services-integration-overview-for-windows-server-2012-r2-essentials-part-2.aspx)  
  
-   [Guide de démarrage rapide à l’aide de Microsoft Office 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md)  
  
-   [Gérer les Services en ligne Microsoft](../manage/Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)  
  
-   [Gérer Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)
