---
title: Configure a Multi-Forest Deployment
description: Cette rubrique fait partie du guide déployer l’accès à distance dans un environnement à plusieurs forêts dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 3c8feff2-cae1-4376-9dfa-21ad3e4d5d99
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f4675e8f465cc44597e16b0312911cae28bd7a1a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860492"
---
# <a name="configure-a-multi-forest-deployment"></a>Configure a Multi-Forest Deployment

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit comment configurer un déploiement à forêts multiples de l’accès à distance dans plusieurs scénarios possibles. Tous les scénarios partent du principe que DirectAccess est actuellement déployé sur une seule forêt nommée Forest1 et que vous êtes en train de configurer DirectAccess à des fins d’utilisation d’une nouvelle forêt nommée Forest2.  
  
## <a name="access-resources-from-forest2"></a><a name="AccessForest2"></a>Accéder aux ressources à partir de Forest2  
Dans ce scénario, DirectAccess est déjà déployé sur Forest1 et est configuré de sorte à permettre aux clients provenant de Forest1 d’accéder au réseau d’entreprise. Par défaut, les clients connectés via DirectAccess peuvent accéder uniquement aux ressources incluses dans Forest1 et ils ne peuvent accéder à aucun serveur de Forest2.  
  
#### <a name="to-enable-directaccess-clients-to-access-resources-from-forest2"></a>Pour permettre aux clients DirectAccess d’accéder aux ressources depuis Forest2  
  
1.  Si le suffixe DNS de Forest2 ne fait pas partie du suffixe DNS de Forest1, ajoutez des règles de table de stratégie de résolution de noms avec les suffixes des domaines dans Forest2, puis ajoutez éventuellement les suffixes des domaines de Forest2 à la liste de recherche de suffixes DNS.  
  
2.  Ajoutez les préfixes IPv6 internes appropriés dans Forest2 si le protocole IPv6 est déployé sur le réseau interne.  
  
## <a name="enable-clients-from-forest2-to-connect-via-directaccess"></a><a name="EnableForest2DA"></a>Autoriser les clients de Forest2 à se connecter via DirectAccess  
Dans ce scénario, vous configurez le déploiement de l’accès à distance afin de permettre aux clients de Forest2 d’accéder au réseau d’entreprise. Vous êtes supposé avoir créé les groupes de sécurité requis pour les ordinateurs clients dans Forest2.   
  
#### <a name="to-allow-clients-from-forest2-to-access-the-corporate-network"></a>Pour autoriser les clients Forest2 à accéder au réseau d’entreprise  
  
1.  Ajoutez le groupe de sécurité des clients depuis Forest2.  
  
2.  Si le suffixe DNS de Forest2 ne fait pas partie du suffixe DNS de Forest1, ajoutez des règles NRPT avec les suffixes du domaine des clients dans Forest2 pour permettre l’accès aux contrôleurs de domaine pour l’authentification et, éventuellement, ajoutez les suffixes des domaines de Forest2 au DNS liste de recherche de suffixes. 
  
3.  Ajoutez les préfixes IPv6 internes dans Forest2 pour permettre à DirectAccess de créer le tunnel IPsec vers les contrôleurs de domaine destinés à l’authentification.  
  
4.  Actualisez la liste des serveurs d’administration.  
  
## <a name="add-entry-points-from-forest2"></a><a name="AddEPForest2"></a>Ajouter des points d’entrée à partir de Forest2  
Dans ce scénario, DirectAccess est déployé dans une configuration multisite sur Forest1 et vous voulez ajouter un serveur d’accès à distance, nommé DA2, depuis Forest2 en tant que point d’entrée vers le déploiement multisite DirectAccess existant.  
  
#### <a name="to-add-a-remote-access-server-from-forest2-as-an-entry-point"></a>Pour ajouter un serveur d’accès à distance depuis Forest2 en tant que point d’entrée  
  
1.  Assurez-vous que l’administrateur de l’accès à distance dispose des autorisations suffisantes pour écrire des objets de stratégie de groupe sur le domaine de DA2 et qu’il est administrateur local sur DA2.  
  
2.  Ajoutez DA2 comme point d’entrée.   
  
3.  Ajoutez des règles de table de stratégie de résolution de noms avec les suffixes des domaines dans Forest2 afin de permettre l’accès aux contrôleurs de domaine destinés à l’authentification, puis ajoutez éventuellement les suffixes des domaines de Forest2 à la liste de recherche de suffixes DNS.  
  
4.  Ajoutez les préfixes IPv6 internes appropriés dans Forest2, si besoin, afin de permettre à l’accès à distance d’établir le tunnel IPsec vers les ressources d’entreprise et pour veiller à ce que les sondes NCSI fonctionnent correctement.  
  
5.  Actualisez la liste des serveurs d’administration.  
  
## <a name="configure-otp-in-a-multi-forest-deployment"></a><a name="OTPMultiForest"></a>Configurer le mot de passe à usage unique dans un déploiement à forêts multiples  
Notez les termes suivants lors de la configuration du mot de passe à usage unique dans un déploiement à forêts multiples :  
  
-   Autorité de certification racine-forêt (s) principale d’autorité de certification PKI.  
  
-   Autorité de certification d’entreprise : toutes les autres autorités de certification.  
  
-   Forêt de ressources-la forêt qui contient l’autorité de certification racine et est considérée comme la « gestion des administration\domaine ».  
  
-   Forêt de comptes : toutes les autres forêts dans la topologie.  
  
Le script PowerShell, PKISync.ps1, est requis pour cette procédure. Voir [AD CS :  script PKISync.ps1 pour l’inscription de certificats inter-forêts](https://technet.microsoft.com/library/ff961506.aspx).  
  
> [!NOTE]  
> Cette rubrique comprend des exemples d'applets de commande Windows PowerShell que vous pouvez utiliser pour automatiser certaines des procédures décrites. Pour plus d’informations, consultez [Utilisation des applets de commande](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
### <a name="configure-cas-as-certificate-publishers"></a><a name="BKMK_CertPub"></a>Configurer les autorités de certification en tant qu’éditeurs de certificats  
  
1.  Activez la prise en charge de la référence LDAP sur toutes les autorités de certification d’entreprise dans toutes les forêts en exécutant la commande suivante à partir d’une invite de commandes avec élévation de privilèges :  
  
    ```  
    certutil -setreg Policy\EditFlags +EDITF_ENABLELDAPREFERRALS  
    ```  
  
2.  Ajoutez tous les comptes d’ordinateurs des autorités de certification d’entreprise aux groupes de sécurité des éditeurs de certificats Active Directory dans chacune des forêts de comptes.  
  
3.  Redémarrez tous les services certsvc sur tous les ordinateurs des autorités de certification dans toutes les forêts en exécutant la commande suivante à partir d’une invite de commandes avec élévation de privilèges :  
  
    ```  
    net stop certsvc && net start certsvc  
    ```  
  
4.  Extrayez le certificat de l’autorité de certification racine en exécutant la commande suivante à partir d’une invite de commandes avec élévation de privilèges :  
  
    ```  
    certutil -config <Computer-Name>\<Root-CA-Name> -ca.cert <root-ca-cert-filename.cer>  
    ```  
  
    (Si vous exécutez la commande sur l’autorité de certification racine, vous pouvez omettre les informations de connexion,-config < Computer-Name >\\< Root-CA-name >)  
  
    1.  Importez le certificat de l’autorité de certification racine de l’étape précédente dans l’autorité de certification de la forêt de comptes en exécutant la commande suivante à partir d’une invite de commandes avec élévation de privilèges :  
  
        ```  
        certutil -dspublish -f <root-ca-cert-filename.cer> RootCA  
        ```  
  
    2.  Accordez des autorisations de lecture/écriture de modèles de certificat de forêt de ressources à la forêt de comptes \<\>\\< compte administrateur\>.  
  
    3.  Extrayez tous les certificats d’autorité de certification d’entreprise de la forêt de ressources en exécutant la commande suivante à partir d’une invite de commandes avec élévation de privilèges :  
  
        ```  
        certutil -config <Computer-Name>\<Enterprise-CA-Name> -ca.cert <enterprise-ca-cert-filename.cer>  
        ```  
  
        (Si vous exécutez la commande sur l’autorité de certification racine, vous pouvez omettre les informations de connexion,-config < Computer-Name >\\< Root-CA-name >)  
  
    4.  Importez les certificats de l’autorité de certification d’entreprise de l’étape précédente dans l’autorité de certification de la forêt de comptes en exécutant les commandes suivantes à partir d’une invite de commandes avec élévation de privilèges :  
  
        ```  
        certutil -dspublish -f <enterprise-ca-cert-filename.cer> NTAuthCA  
        certutil -dspublish -f <enterprise-ca-cert-filename.cer> SubCA  
        ```  
  
    5.  Supprimez de la liste des modèles de certificat émis les modèles de certificat de mot de passe à usage unique de la forêt de comptes.  
  
### <a name="delete-and-import-otp-certificate-templates"></a><a name="BKMK_DelImp"></a>Supprimer et importer des modèles de certificats avec mot de passe à usage unique  
  
1.  Supprimer les modèles de certificat de mot de passe à usage unique de la forêt de comptes, à savoir, Forest2.  
  
2.  Copiez les modèles de certificat et les objets OID depuis la forêt de ressources vers chacune des forêts de comptes à l’aide des commandes PowerShell suivantes :  
  
    ```  
    .\PKISync.ps1 -sourceforest <resource forest DNS> -targetforest <account forest DNS> -type Template -cn <DA OTP registration authority template common name>.  
    .\PKISync.ps1 -sourceforest <resource forest DNS> -targetforest <account forest DNS> -type Template -cn <Secure DA OTP logon certificate template common name>.  
    .\PKISync.ps1 -sourceforest <resource forest DNS> -targetforest <account forest DNS> -type Oid -f  
    ```  
  
### <a name="publish-otp-certificate-templates"></a><a name="BKMK_Publish"></a>Publier les modèles de certificats OTP  
  
-   Émettez les modèles de certificat récemment importés sur toutes les autorités de certification des forêts de comptes.  
  
### <a name="extract-and-synchronize-the-ca"></a><a name="BKMK_Extract"></a>Extraire et synchroniser l’autorité de certification  
  
1.  Extrayez tous les certificats d’autorité de certification d’entreprise des forêts de comptes en exécutant les commandes suivantes à partir d’une invite de commandes avec élévation de privilèges :  
  
    ```  
    certutil -config <Computer-Name>\<Enterprise-CA-Name> -ca.cert <enterprise-ca-cert-filename.cer>  
    ```  
  
2.  Synchronisez les autorités de certification entre les forêts depuis les forêts de comptes vers la forêt de ressources à l’aide de la commande PowerShell suivante :  
  
    ```  
    .\PKISync.ps1 -sourceforest <account forest DNS> -targetforest <resource forest DNS> -type CA -cn <enterprise CA sanitized name> -f  
    ```  
  
3.  Synchronisez les autorités de certification entre les forêts depuis la forêt de ressources vers les forêts de comptes à l’aide de la commande PowerShell suivante :  
  
    ```  
    .\PKISync.ps1 -sourceforest <resource forest DNS> -targetforest <account forest DNS> -type CA -cn <enterprise CA sanitized name> -f  
    ```  
  
## <a name="configuration-procedures"></a>Procédures de configuration  
Les sections suivantes contiennent les procédures de configuration requises pour les déploiements des scénarios ci-dessus. Après avoir accompli une procédure, revenez au scénario pour continuer.  
  
### <a name="add-nrpt-rules-and-dns-suffixes"></a><a name="NRPT_DNSSearchSuffix"></a>Ajouter des règles NRPT et des suffixes DNS  
Les clients qui se connectent via DirectAccess au réseau d’entreprise utilisent la table de stratégie de résolution de noms pour déterminer quel serveur DNS doit être utilisé pour résoudre l’adresse des différentes ressources. Cela permet au client de résoudre les adresses des ressources d’entreprise et de conserver une classification interne à l’entreprise/extérieure à l’entreprise propre, laquelle est requise pour que DirectAccess continue de fonctionner. Les outils de configuration de DirectAccess détectent automatiquement le suffixe DNS racine de Forest1 et l’ajoute à la table de stratégie de résolution de noms. Cependant, les suffixes des noms de domaine complets de Forest2 ne sont pas ajoutés automatiquement à la table de stratégie de résolution de noms et l’administrateur de l’accès à distance doit les ajouter manuellement.  
  
La liste de recherche de suffixes DNS permet aux clients d’utiliser des noms d’étiquette courts au lieu des noms de domaine complets. Les outils de configuration de l’accès à distance ajoutent automatiquement tous les domaines de Forest1 à la liste de recherche de suffixes DNS. Si vous voulez permettre aux clients d’utiliser des noms d’étiquette courts pour les ressources incluses dans Forest2, vous devez les ajouter manuellement.  
  
##### <a name="to-add-a-dns-suffix-to-the-nrpt-table-and-domain-suffixes-to-the-dns-suffix-search-list"></a>Pour ajouter un suffixe DNS à la table de stratégie de résolution de noms et des suffixes de domaine à la liste de recherche de suffixes DNS  
  
1.  Dans le volet central de la console Gestion de l’accès à distance, dans la zone **Étape 3 : serveurs d’infrastructure**, cliquez sur **Modifier**.  
  
2.  Dans la page **Serveur Emplacement réseau**, cliquez sur **Suivant**.  
  
3.  Dans la page **DNS**, dans la table, entrez tous les suffixes de noms supplémentaires qui font partie du réseau d’entreprise dans Forest2. Dans **Adresse du serveur DNS**, entrez l’adresse du serveur DNS manuellement ou en cliquant sur **Détecter**. Si vous n’entrez pas l’adresse, les nouvelles entrées sont appliquées en tant qu’exemptions NRPT. Ensuite, cliquez sur **Suivant**.  
  
4.  Facultatif : Sur la page **Liste de recherche de suffixes DNS** , ajoutez un suffixe DNS en l’entrant dans la zone **Nouveau suffixe** , puis en cliquant sur **Ajouter**. Ensuite, cliquez sur **Suivant**.  
  
5.  Dans la page **Gestion**, cliquez sur **Terminer**.  
  
6.  Dans le volet central de la console Gestion de l’accès à distance, cliquez sur **Terminer**.  
  
7.  Dans la boîte de dialogue **Vérification de l’accès DirectAccess**, cliquez sur **Appliquer**.  
  
8.  Dans la boîte de dialogue **Application des paramètres de l’Assistant Configuration de l’accès à distance**, cliquez sur **Fermer**.  
  
### <a name="add-internal-ipv6-prefix"></a><a name="IPv6Prefix"></a>Ajouter un préfixe IPv6 interne  
  
> [!NOTE]  
> L’ajout d’un préfixe IPv6 interne convient uniquement lorsque le protocole IPv6 est déployé sur le réseau interne.  
  
L’accès à distance gère la liste des préfixes IPv6 pour les ressources d’entreprise. Seules les ressources dotées de ces préfixes IPv6 sont accessibles par les clients connectés via DirectAccess. Étant donné que la console de gestion de l’accès à distance et les commandes Windows PowerShell ajoutent automatiquement les préfixes IPv6 de Forest1, et peuvent ne pas ajouter les préfixes d’autres forêts, vous devez ajouter manuellement les préfixes manquants de Forest2.  
  
##### <a name="to-add-an-ipv6-prefix"></a>Pour ajouter un préfixe IPv6  
  
1.  Dans le volet central de la console Gestion de l’accès à distance, dans la zone **Étape 2 : serveur d’accès à distance**, cliquez sur **Modifier**.  
  
2.  Dans l’Assistant Installation du serveur d’accès à distance, cliquez sur **Configuration du préfixe**.  
  
3.  Dans la page **Configuration du préfixe**, dans **Préfixes IPv6 utilisés du réseau interne**, ajoutez tous les préfixes IPv6 supplémentaires en les séparant par des points-virgules, par exemple, 2001:db8:1::/64;2001:db8:2::/64. Ensuite, cliquez sur **Suivant**.  
  
4.  Dans la page **Authentification**, cliquez sur **Terminer**.  
  
5.  Dans le volet central de la console Gestion de l’accès à distance, cliquez sur **Terminer**.  
  
6.  Dans la boîte de dialogue **Vérification de l’accès DirectAccess**, cliquez sur **Appliquer**.  
  
7.  Dans la boîte de dialogue **Application des paramètres de l’Assistant Configuration de l’accès à distance**, cliquez sur **Fermer**.  
  
### <a name="add-client-security-groups"></a><a name="SGs"></a>Ajouter des groupes de sécurité client  
Pour permettre aux ordinateurs clients Windows 8 de Forest2 d’accéder aux ressources via DirectAccess, vous devez ajouter le groupe de sécurité de Forest2 au déploiement de l’accès à distance.  
  
##### <a name="to-add-windows-8-client-security-groups"></a>Pour ajouter des groupes de sécurité clients Windows 8  
  
1.  Dans le volet central de la console Gestion de l’accès à distance, dans la zone **Étape 1 : clients distants**, cliquez sur **Modifier**.  
  
2.  Dans l’Assistant Installation des clients DirectAccess, cliquez sur **Sélectionner des groupes**, puis dans la page **Sélectionner des groupes**, cliquez sur **Ajouter**.  
  
3.  Dans la boîte de dialogue **Sélectionner des groupes**, sélectionnez les groupes de sécurité contenant des ordinateurs clients DirectAccess. Ensuite, cliquez sur **Suivant**.  
  
4.  Dans la page **Assistant Connectivité réseau**, cliquez sur **Terminer**.  
  
5.  Dans le volet central de la console Gestion de l’accès à distance, cliquez sur **Terminer**.  
  
6.  Dans la boîte de dialogue **Vérification de l’accès DirectAccess**, cliquez sur **Appliquer**.  
  
7.  Dans la boîte de dialogue **Application des paramètres de l’Assistant Configuration de l’accès à distance**, cliquez sur **Fermer**.  
  
Pour permettre aux ordinateurs clients Windows 7 de Forest2 d’accéder aux ressources via DirectAccess lorsque le multisite est activé, vous devez ajouter le groupe de sécurité de Forest2 au déploiement de l’accès à distance pour chaque point d’entrée. Pour plus d’informations sur l’ajout de groupes de sécurité Windows 7, consultez la description de la page de **support client** dans 3,6. Activez le déploiement multisite.  
  
### <a name="refresh-the-management-servers-list"></a><a name="RefreshMgmtServers"></a>Actualiser la liste des serveurs d’administration  
L’accès à distance découvre automatiquement les serveurs d’infrastructure dans toutes les forêts qui contiennent des objets de stratégie de groupe de configuration DirectAccess. Si DirectAccess a été déployé sur un serveur depuis Forest1, l’objet de stratégie de groupe du serveur est écrit sur son domaine dans Forest1. Si vous avez activé l’accès à DirectAccess pour les clients depuis Forest2, l’objet de stratégie de groupe du client est écrit sur un domaine dans Forest2.  
  
Le processus de découverte automatique des serveurs d’infrastructure est requis pour autoriser l’accès via DirectAccess aux contrôleurs de domaine et aux Configuration Manager de point de terminaison Microsoft. Vous devez démarrer manuellement le processus de découverte.  
  
##### <a name="to-refresh-the-management-servers-list"></a>Pour actualiser la liste des serveurs d’administration  
  
1.  Dans la console Gestion de l’accès à distance, cliquez sur **Configuration**, puis dans le volet **Tâches**, cliquez sur **Actualiser les serveurs d’administration**.  
  
2.  Dans la boîte de dialogue **Actualisation des serveurs d’administration**, cliquez sur **Fermer**.  
  


