---
title: Gérer Office 365 dans Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 3f8485e4-e10f-4f38-8a5e-d5227abd0d84
author: nnamuhcs
ms.author: daveba
ms.openlocfilehash: d8051431f55a7a3e05f0a1917a003df044533571
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311258"
---
# <a name="manage-office-365-in-windows-server-essentials"></a>Gérer Office 365 dans Windows Server Essentials

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Lorsque vous intégrez votre serveur Windows Server Essentials à Microsoft Office 365, vous pouvez gérer vos services 365 Office et vos comptes en ligne avec vos ressources locales à partir du tableau de bord Windows Server Essentials. Dans cette rubrique, vous allez découvrir ce que vous pouvez obtenir en intégrant votre serveur à Office 365, la procédure à suivre et comment gérer et résoudre les problèmes liés à votre intégration d’Office 365.  
  
  
  
> [!IMPORTANT]
>   L’intégration d’Office 365 est uniquement prise en charge dans un environnement de contrôleur de domaine unique. En outre, l’Assistant intégration d’Office 365 doit s’exécuter sur un contrôleur de domaine.  
  
## <a name="in-this-topic"></a>Dans cette rubrique  
  
-   [Pourquoi dois-je intégrer Office 365 à mon serveur ?](#BKMK_IntegrationOverview)  
  
-   [Configurer l’intégration d’Office 365](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Configure)  
  
-   [Gérer l’intégration d’Office 365](#BKMK_ManageIntegration)  
  
-   [Résoudre les problèmes d’intégration d’Office 365](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Troubleshoot)  
  
##  <a name="why-should-i-integrate-office-365-with-my-server"></a><a name="BKMK_IntegrationOverview"></a>Pourquoi dois-je intégrer Office 365 à mon serveur ?  
 Il existe de nombreuses bonnes raisons d’intégrer Office 365 à votre serveur Windows Server Essentials. Si vous gérez certaines de vos ressources en interne, mais que vous utilisez Office 365 pour d’autres services, vous pouvez gérer vos services et ressources Office 365 à partir du tableau de bord, avec vos ressources locales, au lieu de travailler à deux endroits.  
  
- Gérez les comptes en ligne qui permettent à vos utilisateurs d’accéder à Office 365 et à vos comptes d’utilisateur :  
  
  -   Créez des comptes Microsoft Online Services pour vos utilisateurs en une seule étape, ou créez des comptes d'utilisateur sur le serveur pour vos comptes en ligne existants. Vous pouvez également ajouter un compte en ligne à un compte d'utilisateur nouveau ou existant.  
  
       Lorsque vous créez les comptes en ligne à partir du tableau de bord, les utilisateurs se connectent à Office 365 avec le même mot de passe que celui qu’ils utilisent sur le serveur. S'ils changent le mot de passe de leur compte d'utilisateur, le mot de passe en ligne change également. Par ailleurs, vous savez que leurs mots de passe en ligne répondent toujours aux exigences de sécurité que vous définissez pour vos comptes d'utilisateur.  
  
  -   Gérez un compte en ligne avec le compte d'utilisateur tout au long du cycle de vie du compte d'utilisateur. Si vous désactivez un compte d'utilisateur, le compte en ligne est également désactivé. Si vous supprimez un compte d'utilisateur, le compte en ligne est également supprimé.  
  
  -   Sur un serveur Windows Server Essentials, gérez également les groupes de distribution Exchange Online pour les messages électroniques.  
  
- Envoyez et recevez des messages électroniques du domaine Internet de votre organisation (par exemple, contoso.com) en liant un domaine Internet personnalisé à votre abonnement Office 365.  
  
- Gérez votre abonnement et l’intégration d’Office 365 à partir du tableau de bord.  
  
- Si votre abonnement comprend des bibliothèques SharePoint Online, l’intégration d’Office 365 à un serveur Windows Server Essentials vous permet d’utiliser les éléments suivants :  
  
  - Créez et gérez vos bibliothèques SharePoint Online à partir du tableau de bord.  
  
    > [!NOTE]
    >  Vous pouvez également utiliser l’application my Server 2012 R2 pour travailler avec des documents dans vos bibliothèques SharePoint Online à partir de votre ordinateur portable, de votre appareil mobile ou de votre Windows Phone. Cette fonctionnalité n’est disponible que pour Windows Server Essentials. Pour plus d’informations, consultez [utiliser l’application my Server](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md).  
  
  - Modifiez les autorisations pour un site d’équipe SharePoint Online à partir du tableau de bord, ou ouvrez le site d’équipe à partir du tableau de bord pour apporter d’autres modifications.  
  
    Pour plus d’informations, consultez [gérer SharePoint Online](Manage-SharePoint-Online-in-Windows-Server-Essentials.md).  
  
- Si vous vous abonnez à Exchange Online, l’intégration d’Office 365 à un serveur Windows Server Essentials vous permet de gérer les appareils mobiles que vos utilisateurs utilisent pour se connecter au serveur de messagerie de votre entreprise :  
  
  -   Demandez la protection par mot de passe lorsque les appareils mobiles se connectent au serveur de messagerie de votre entreprise. Définissez une longueur de mot de passe minimale, le nombre de tentatives de connexion ayant échoué autorisées et la durée minimale entre les tentatives de connexion.  
  
  -   Empêchez un appareil mobile de se connecter à Exchange Online si vous avez connaissance de problèmes de sécurité avec le modèle de l'appareil.  
  
  -   Si un appareil mobile est perdu ou volé, effacez l'appareil pour supprimer toutes les données sensibles de l'entreprise la prochaine fois qu'il sera mis sous tension.  
  
- L’intégration d’Office 365 vous offre de nouvelles façons de vous connecter aux services et ressources Office 365 :  
  
  -   Ouvrez les services Office 365 à partir du Launchpad Windows Server Essentials. Pour plus d’informations, consultez [Guide de démarrage rapide à l’utilisation de Microsoft Office 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md).  
  
  -   Utilisez l’application my Server 2012 R2 pour travailler avec des documents dans vos bibliothèques SharePoint Online à partir de votre ordinateur portable, de votre appareil mobile ou de votre Windows Phone. Pour plus d’informations, consultez [utiliser l’application my Server](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md). Cette fonctionnalité est disponible uniquement dans Windows Server Essentials.  
  
##  <a name="set-up-office-365-integration"></a><a name="BKMK_Configure"></a>Configurer l’intégration d’Office 365  
 Vous pouvez intégrer votre serveur à Office 365 à tout moment une fois l’installation du serveur terminée. Si vous n’avez pas encore d’abonnement Office 365, vous pouvez en acheter un ou vous inscrire pour bénéficier d’un abonnement d’évaluation gratuit.  
  
 Vous exécuterez les tâches suivantes :  
  
-   [Étape 1 : vérifier les conditions d’intégration d’Office 365](#BKMK_StepOne_VERIFY)  
  
-   [Étape 2 : intégrer le serveur à Microsoft Office 365](#BKMK_StepTwo)  
  
-   [Étape 3 : lier le nom de domaine Internet de votre organisation à Office 365 (facultatif)](#BKMK_StepThree)  
  
###  <a name="step-1-verify-office-365-integration-requirements"></a><a name="BKMK_StepOne_VERIFY"></a>Étape 1 : vérifier les conditions d’intégration d’Office 365  
 Avant de commencer, assurez-vous que le serveur répond aux exigences suivantes :  
  
-   Le serveur peut avoir l’un des systèmes d’exploitation suivants : Windows Server Essentials, Windows Server Essentials ou le système d’exploitation Windows Server 2012 R2 Standard ou Windows Server 2012 R2 Datacenter avec le rôle expérience Windows Server Essentials installé.  
  
-   L’environnement ne peut avoir qu’un seul contrôleur de domaine, et vous devez effectuer l’intégration d’Office 365 sur le contrôleur de domaine.  
  
-   Vous devez être en mesure de vous connecter à Internet à partir du serveur.  
  
-   Vous devez installer toutes les mises à jour critiques et importantes sur le serveur avant de démarrer l’intégration d’Office 365.  
  
-   Si vous souhaitez utiliser le domaine Internet de votre organisation dans les adresses de messagerie et avec vos ressources SharePoint Online, vous devez inscrire le nom de domaine pour pouvoir lier le domaine à Office 365 lors de l’intégration. Pour plus d’informations, consultez [Procédure à suivre pour acheter un nom de domaine](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660).  
  
> [!NOTE]
>  Vous n’avez pas besoin de vous abonner à Office 365 à l’avance. Vous pouvez acheter un abonnement ou vous inscrire pour bénéficier d’une version d’évaluation gratuite pendant l’intégration d’Office 365. Si vous souhaitez examiner les plans et la tarification pour Office 365, [Comparez les plans office 365 pour les entreprises](https://office.microsoft.com/compare-office-365-for-business-plans-FX102918419.aspx?CR_CC=200061904&WT.srch=1&WT.mc_ID=PS_bing_O365Comm_subscribe-to-office-365_Text).  
  
###  <a name="step-2-integrate-the-server-with-microsoft-office-365"></a><a name="BKMK_StepTwo"></a>Étape 2 : intégrer le serveur à Microsoft Office 365  
 Exécutez la procédure suivante sur le contrôleur de domaine pour intégrer votre serveur Windows Server Essentials à Office 365.  
  
> [!NOTE]
>  La procédure est la même que vous ayez Windows Server Essentials ou Windows Server Essentials, mais vous commencerez à partir d’un emplacement différent sur la page d’accueil. Dans Windows Server Essentials, vous intégrez le serveur à Office 365 et d’autres services en ligne Microsoft sous l’onglet **services** . Dans Windows Server Essentials, l’intégration d’Office 365 est effectuée sous l’onglet **courrier électronique** .  
  
##### <a name="to-integrate-the-server-with-office-365"></a>Pour intégrer le serveur à Office 365  
  
1. Connectez-vous au serveur en tant qu’administrateur, puis ouvrez le tableau de bord Windows Server Essentials.  
  
2. Sur la **page** d’installation, cliquez sur **services** (dans Windows Server Essentials, cliquez sur **messagerie**), cliquez sur **intégrer à Microsoft Office 365**, puis cliquez sur **configurer Microsoft Office intégration 365**.  
  
    L'Assistant Intégration à Microsoft Office 365 s'affiche.  
  
3. Dans la page **Commencer**, effectuez l'une des actions suivantes :  
  
   -   Si vous n’avez pas d’abonnement à Office 365, cliquez sur **suivant**, puis suivez les instructions pour vous abonner à Office 365 ou vous inscrire pour obtenir un abonnement d’évaluation.  
  
        Vous devez vous connecter à Office 365 avant de revenir à l’Assistant. Mais vous n’avez pas besoin d’effectuer les tâches décrites dans la section **Démarrer ici** du portail Office 365.  
  
   -   Si vous disposez déjà d’un abonnement à Office 365 que vous souhaitez intégrer au serveur, sélectionnez **j’ai déjà un abonnement pour office 365**, puis cliquez sur **suivant**.  
  
4. Suivez les instructions pour exécuter l'Assistant.  
  
   Une fois l’Assistant terminé, vous remarquerez les modifications suivantes apportées au tableau de bord :  
  
-   Une nouvelle page **office 365** est utilisée pour gérer l’intégration et votre abonnement Office 365.  
  
-   Dans la page **utilisateurs** , vous pouvez voir un onglet **comptes en ligne** où vous pouvez créer et gérer les comptes Microsoft Online Services qui permettent à vos utilisateurs d’accéder à Office 365. Si vous utilisez Exchange Online et que vous disposez d’un serveur Windows Server Essentials, vous verrez également un onglet **groupes de distribution** .  
  
-   La page **stockage** sur un serveur Windows Server Essentials dispose d’un onglet **bibliothèques SharePoint** pour la gestion des bibliothèques SharePoint Online et la modification des autorisations pour vos sites d’équipe. Chaque plan d’activité pour Office 365 comprend ces fonctionnalités SharePoint Online de base.  
  
###  <a name="step-3-link-your-organizations-internet-domain-name-to-office-365-optional"></a><a name="BKMK_StepThree"></a>Étape 3 : lier le nom de domaine Internet de votre organisation à Office 365 (facultatif)  
 Si vous souhaitez utiliser votre propre domaine Internet dans le courrier électronique adressé à votre organisation et les URL de vos ressources SharePoint Online, vous pouvez lier un domaine personnalisé à votre abonnement Office 365. Si vous intégrez votre serveur Windows Server Essentials à Office 365, vous pouvez le faire à partir du tableau de bord.  
  
 Il est plus facile d’effectuer cette opération avant de créer des comptes en ligne pour vos utilisateurs afin de pouvoir utiliser le domaine lorsque vous créez en bloc des comptes en ligne.  
  
 Vous recevez un nom de domaine lorsque vous vous inscrivez à Office 365 ? par exemple, *contoso*. onmicrosoft.com. Si vous préférez utiliser un autre nom de domaine, disons simplement contoso.com ? vous pouvez. Vous devez acheter un nom de domaine si vous n’en possédez pas déjà un, et modifier certains des enregistrements DNS.  
  
 La configuration d’un domaine personnalisé à utiliser avec Office 365 implique quatre tâches :  
  
1. **Achetez un nom de domaine.** Autrement dit, enregistrez-le auprès d'un bureau d'enregistrement de domaines ou d'un fournisseur d'hébergement DNS.  
  
   -   Choisissez un nom de domaine qui fonctionne avec Office 365. Vous pouvez utiliser un nom de domaine de 2e niveau ? par exemple, buycontoso.com ? mais pas un nom de domaine de troisième niveau ? par exemple, marketing.contoso.com. Pour plus d’informations sur le choix d’un domaine à utiliser dans Office 365, consultez [domaines](https://technet.microsoft.com/library/office-365-domains.aspx).  
  
   -   Achetez-le auprès d’un bureau d’enregistrement de domaines qui autorise les enregistrements DNS (Domain Name Server) requis par Office 365. Pour savoir quel bureau d’enregistrement de domaines autorise les enregistrements DNS requis, consultez [Comment acheter un nom de domaine](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660). Si vous avez déjà inscrit votre domaine auprès d’un bureau d’enregistrement différent, ne vous inquiétez pas ; vous pouvez transférer le domaine vers un autre bureau d’enregistrement lorsque vous liez le domaine à Office 365.  
  
2. **Configurez des enregistrements DNS qui autorisent les services Office 365 à utiliser le nom de domaine.** Le moyen le plus simple consiste à laisser l’Assistant Configurer les enregistrements DNS pour vous lorsque vous liez le domaine à votre abonnement Office 365 à l’étape 3. Si vous préférez le faire vous-même, consultez [Comment configurer manuellement des enregistrements DNS pour l’intégration d’Office 365](#BKMK_ManuallyConfigureDNS).  
  
3. **Liez votre domaine Internet personnalisé à votre abonnement Office 365.** Vous allez utiliser l’action **lier un domaine à Office 365** .  
  
4. **Vérifiez que vos services Office 365 utilisent le nouveau nom de domaine.**  
  
   Si vous effectuez les étapes 1 et 2 avant d’utiliser la tâche **lier un domaine à office 365** , l’Assistant peut lier le nom de domaine à Office 365. Un Assistant peut également vous aider à effectuer partiellement ou en totalité les étapes 1 et 2.  
  
##### <a name="to-link-your-organizations-internet-domain-to-office-365"></a>Pour lier le domaine Internet de votre organisation à Office 365  
  
1.  Dans le tableau de bord, ouvrez la page **Office 365** , puis cliquez sur **Relier un domaine à Office 365**.  
  
2.  Suivez les instructions pour exécuter l'Assistant.  
  
     L’Assistant peut vous aider à effectuer une partie ou l’ensemble des étapes de l’inscription, de la configuration et de la liaison d’un nom de domaine Internet nouveau ou existant à utiliser dans Office 365.  
  
     Cliquez sur le lien d'aide affiché sur la page de l'Assistant pour obtenir les informations nécessaires à l'exécution d'une tâche. Ou consultez la section configurer un nom de domaine de [gérer les accès Web distants dans Windows Server Essentials](https://technet.microsoft.com/library/jj628152\(d=printer\).aspx) pour obtenir une vue d’ensemble du processus et des exigences.  
  
    > [!NOTE]
    >  Pour utiliser l'Assistant pour inscrire un nouveau nom de domaine, vous devez utiliser un des fournisseurs de noms de domaine partenaires de Microsoft pour fournir une intégration transparente à l'aide de l'Assistant. Pour rechercher un bureau d’enregistrement de domaines, consultez [Procédure à suivre pour acheter un nom de domaine](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660).  
  
3.  Si l’Assistant détecte que votre nom de domaine n’est pas géré par le serveur, vous devez configurer manuellement les enregistrements DNS requis pour terminer la configuration. Pour obtenir des instructions, voir [Comment configurer manuellement des enregistrements DNS pour l'intégration d'Office 365](#BKMK_ManuallyConfigureDNS), plus loin dans cette rubrique.  
  
4.  Vérifiez que le domaine est utilisé dans Office 365.  
  
     Il y a un peu d’attente une fois l’Assistant terminé, tandis que le Bureau d’enregistrement de noms de domaine vérifie les enregistrements DNS. Cela se produit automatiquement. vous n’avez rien à faire. Mais cela prend généralement une heure environ, et parfois un peu plus longtemps. Une fois la vérification du domaine terminée, la page **Office 365** répertorie le domaine de votre organisation.  
  
####  <a name="how-to-manually-configure-dns-records-for-office-365-integration"></a><a name="BKMK_ManuallyConfigureDNS"></a>Comment configurer manuellement des enregistrements DNS pour l’intégration d’Office 365  
 Si l'Assistant Relier votre domaine à Office 365 détecte que votre nom de domaine n'est pas géré par le serveur, vous devez, pour terminer la configuration, configurer manuellement les enregistrements DNS (Domain Name Server) requis. Dans ce cas, vous trouverez la liste des enregistrements DNS que vous devez configurer sur **% username% \ NewDNSRecords_ (n). txt**, où *(n)* est un nombre aléatoire.  
  
 Le tableau suivant décrit les enregistrements DNS que vous devez ajouter. Les méthodes d’entrée peuvent varier en fonction des différents bureaux d’enregistrement de domaines. Si vous avez des questions, demandez de l'aide à votre bureau d'enregistrement de noms de domaine.  
  
### <a name="required-dns-records-for-linking-a-custom-internet-domain-name-to-office-365"></a>Enregistrements DNS requis pour la liaison d'un nom de domaine Internet personnalisé à Office 365  
  
|Service|Enregistrements DNS requis|Objectif|  
|-------------|--------------------------|-------------|  
|(Plusieurs services)|MX| Office 365 utilise cet enregistrement pour vérifier que vous possédez un nom de domaine spécifique. Cet enregistrement MX n'interfère pas avec le routage de messages électroniques.|  
|Exchange Online|MX|Assure le routage de messages électroniques. **Important :**  Si vous migrez des e-mails, n’affectez pas de valeur zéro (**0**) au nouvel enregistrement MX. Assurez-vous que la valeur de l'enregistrement est supérieure à celle affectée à l'enregistrement MX actif. Lorsque la migration de la messagerie électronique est terminée et que vous êtes prêt à remplacer le serveur de messagerie par Office 365, contactez votre bureau d’enregistrement de noms de domaine de façon à réinitialiser la valeur de préférence du nouvel enregistrement MX.|  
|Exchange Online|Alias (CNAME)|Découvrez automatiquement l'enregistrement qui est utilisé pour aider les utilisateurs à configurer facilement une connexion entre Exchange Online et leur client de bureau Outlook ou un client de messagerie mobile. **Remarque :**  Si vous préférez accéder à Outlook Accès web à l’aide du nom de domaine de votre organisation (par exemple, http://mail.contoso.com) au lieu de l’URL standard (https://outlook.com/owa/office365.com), vous pouvez configurer l’enregistrement d’alias (CNAME) comme suit : **type = CNAME, TTL = 01:00:00, nomhôte = mail, Address = mail. 365. com**|  
|Exchange Online|TXT|Spécifie que outlook.com, le domaine utilisé par les serveurs de messagerie Office 365, est autorisé à envoyer des courriers électroniques au nom de votre domaine. Créez cet enregistrement pour empêcher que votre courrier électronique sortant soit marqué en tant que courrier indésirable.|  
|Lync Online|SRV|Permet d'activer la fédération avec d'autres services de messagerie instantanée tels que Windows Live ou Yahoo!.|  
|Lync Online|SRV|Découvrez automatiquement l'enregistrement qui est utilisé pour aider les utilisateurs à configurer facilement une connexion entre le client de bureau Lync et Microsoft Lync Online.|  
  
> [!IMPORTANT]
>  Une fois la vérification du domaine terminée, n’essayez pas d’ajouter ou d’apporter d’autres modifications aux enregistrements DNS à partir du portail Office 365.  
  
###  <a name="next-step"></a><a name="BKMK_StepFour_ACCOUNTS"></a>Étape suivante  
  
-   Créez des comptes Microsoft Online Services pour vos utilisateurs.  
  
     Pour utiliser les services Office 365, vos utilisateurs doivent disposer d’un compte Microsoft Online Services associé à leur compte d’utilisateur réseau. Le tableau de bord facilite cette opération. Si vous utilisez un nouvel abonnement Office 365, vous pouvez créer en bloc des comptes en ligne pour vos comptes d’utilisateur existants. Si vous intégrez un nouveau serveur à un abonnement Office 365 que vous utilisez déjà, vous pouvez importer des comptes d’utilisateur à partir de vos comptes en ligne existants. Pour obtenir les procédures, consultez [gérer les comptes en ligne pour les utilisateurs](Manage-Online-Accounts-for-Users.md).  
  
> [!NOTE]
>  Dans le tableau de bord de Windows Server Essentials, les comptes Microsoft Online Services sont appelés comptes Office 365. Les comptes sont identiques ; seule la terminologie a changé.  
  
##  <a name="manage-office-365-integration"></a><a name="BKMK_ManageIntegration"></a>Gérer l’intégration d’Office 365  
 Une fois que vous avez intégré votre serveur à Office 365, la page **office 365** du tableau de bord affiche des informations sur votre abonnement Office 365 et rend ces tâches disponibles :  
  
-   [Gérer votre abonnement Office 365](#BKMK_ManageO365) ? Modifiez le compte d’administrateur que vous utilisez pour gérer l’abonnement. Ouvrez le tableau de bord d’administration Office 365 pour gérer votre abonnement.  
  
-   [Lier le domaine Internet de votre organisation à Office 365](#BKMK_StepThree) ? Si vous souhaitez être en mesure d’envoyer et de recevoir des messages électroniques adressés à votre propre domaine, vous pouvez lier le domaine à Office 365. (Décrit précédemment, à l' [étape 3 : lier le domaine de votre organisation à Office 365](#BKMK_StepThree).)  
  
-   [Désactiver l’intégration d’Office 365](#BKMK_Disable) ? Si vous ne souhaitez pas gérer vos services Office 365, votre abonnement et vos comptes en ligne à partir du tableau de bord, vous pouvez désactiver l’intégration d’Office 365. Les services sont toujours disponibles sur le portail Office 365.  
  
###  <a name="manage-your-office-365-subscription"></a><a name="BKMK_ManageO365"></a>Gérer votre abonnement Office 365  
 Si vous devez apporter des modifications à votre abonnement Office 365 pendant que vous travaillez sur le serveur, vous pouvez ouvrir l’abonnement dans Office 365 à partir de la page **office 365** du tableau de bord. Vous pouvez également modifier le compte administrateur que le serveur utilise pour apporter des modifications aux services Office 365.  
  
##### <a name="to-open-your-subscription-on-the-office-365-admin-dashboard"></a>Pour ouvrir votre abonnement dans le tableau de bord Administrateur d'Office 365  
  
1.  Dans le tableau de bord Windows Server Essentials, ouvrez la page **Office 365** .  
  
2.  Dans **Tâches Configuration**, cliquez sur **Gérer Office 365**.  
  
3.  Connectez-vous à Office 365 avec le compte en ligne Microsoft que vous utilisez pour gérer votre abonnement.  
  
     Le tableau de bord d’administration Office 365 s’ouvre.  
  
##### <a name="to-change-the-office-365-administrator-account"></a>Pour modifier le compte Administrateur Office 365  
  
1.  Dans le tableau de bord, cliquez sur **Office 365**.  
  
2.  Dans **Tâches Configuration**, cliquez sur **Changer de compte Administrateur Office 365**. L'Assistant Changer de compte Administrateur s'affiche. (Dans Windows Server Essentials, l’Assistant s’appelle configurer le compte d’administrateur Office 365.)  
  
3.  Tapez les informations d’identification du compte que vous souhaitez utiliser pour vous connecter à votre abonnement Office 365, puis cliquez sur **suivant**.  
  
4.  Cliquez sur **Fermer**. Le tableau de bord redémarre.  
  
###  <a name="disable-office-365-integration"></a><a name="BKMK_Disable"></a>Désactiver l’intégration d’Office 365  
 Si vous décidez que vous ne souhaitez pas gérer vos services Office 365 et vos comptes en ligne à partir du tableau de bord, vous pouvez désactiver l’intégration d’Office 365. Votre abonnement Office 365 reste actif et toutes les modifications de configuration que vous avez apportées à partir du tableau de bord restent en vigueur. Par exemple, vous recevrez un courrier électronique adressé à un nom de domaine que vous avez lié à votre abonnement Office 365. Vous ne perdrez pas de courrier électronique, et les contrôles que vous définissez pour les appareils mobiles sont toujours utilisés par Exchange Online.  
  
 À l’avenir, vous allez gérer votre abonnement Office 365, vos services et vos ressources dans Office 365, et vos utilisateurs devront gérer les mots de passe de leurs comptes en ligne dans Office 365. La synchronisation des mots de passe ne se produit plus, et la désactivation ou la suppression d’un compte d’utilisateur n’aura aucun effet sur le compte en ligne de l’utilisateur.  
  
 Étant donné que le logiciel d’intégration Office 365 est installé sur le serveur local, vous pouvez désactiver la fonctionnalité même si le service d’intégration ne peut pas se connecter à Office 365.  
  
##### <a name="to-disable-office-365-integration-with-the-server"></a>Pour désactiver l'intégration d'Office 365 avec le serveur  
  
1.  Dans le tableau de bord, cliquez sur **Office 365**.  
  
2.  Cliquez sur **Désactiver l'intégration d'Office 365**. L'Assistant Désactiver l'intégration d'Office 365 s'affiche.  
  
3.  Suivez les instructions pour exécuter l'Assistant.  
  
> [!NOTE]
>  Pour réactiver l’intégration Office 365, utilisez la tâche **intégrer à office 365** dans l’onglet **service** de la page d' **hébergement** du tableau de bord. Pour obtenir des instructions, voir [Étape 2 : Intégrer votre serveur Windows Server Essentials à Microsoft Office 365](#BKMK_StepTwo), précédemment dans cette rubrique.  
  
##  <a name="troubleshoot-office-365-integration"></a><a name="BKMK_Troubleshoot"></a>Résoudre les problèmes d’intégration d’Office 365  
 Cette section fournit des informations pour vous aider à résoudre les problèmes courants que vous pouvez rencontrer lors de l’utilisation des fonctionnalités d’intégration d’Office 365 dans Windows Server Essentials.  
  
###  <a name="some-microsoft-online-services-accounts-were-not-created"></a><a name="BKMK_AcctsNotCreated"></a>Certains comptes Microsoft Online Services n’ont pas été créés  
 **Description**  
  
 Une tentative de création d’un ou plusieurs comptes Microsoft Online Services à partir du tableau de bord a échoué.  
  
 **Solution**  
  
1.  Cliquez sur le lien figurant sur la page de fin de l'Assistant pour ouvrir un fichier de résultats qui contient des informations détaillées sur chaque demande de création de compte ayant échoué. Par exemple, un résultat peut vous informer qu'un compte Microsoft Online Services portant le nom d'un compte demandé existe déjà.  
  
2.  Effectuez les actions recommandées pour résoudre chaque erreur.  
  
3.  Si le problème persiste, redémarrez le serveur, puis essayez à nouveau de créer les comptes en ligne.  
  
###  <a name="there-was-a-problem-uninstalling-office-365-integration"></a><a name="BKMK_ProblemUninstalling"></a>Un problème est survenu lors de la désinstallation de l’intégration d’Office 365  
 **Description**  
  
 Une erreur inconnue s’est produite lors de la tentative de désactivation de l’intégration d’Office 365.  
  
 **Solution**  
  
1.  Assurez-vous que l'ordinateur est connecté à Internet, puis réessayez.  
  
2.  Si l'erreur se reproduit, redémarrez le serveur, puis réessayez.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Présentation de l’intégration des services pour Windows Server Essentials-partie 1](https://blogs.technet.com/b/sbs/archive/2013/11/04/services-integration-overview-for-windows-server-2012-r2-essentials-part-1.aspx)  
  
-   [Présentation de l’intégration des services pour Windows Server Essentials-partie 2](https://blogs.technet.com/b/sbs/archive/2013/11/06/services-integration-overview-for-windows-server-2012-r2-essentials-part-2.aspx)  
  
-   [Guide de démarrage rapide de l’utilisation de Microsoft Office 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md)  
  
-   [Gérer Microsoft Online Services](../manage/Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)  
  
-   [Gérer Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)
