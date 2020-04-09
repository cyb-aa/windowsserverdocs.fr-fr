---
ms.assetid: 434fd617-373a-405e-bae4-da324ea83efc
title: Guide de déploiement des services AD FS Windows Server 2012 R2
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b4cf161d9dfcbeeb467ccb0a83cc260c5299057e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855522"
---
# <a name="configure-a-federation-server"></a>Configurer un serveur de fédération


Après avoir installé le Services ADFS \(AD FS\) service de rôle sur votre ordinateur, vous êtes prêt à configurer cet ordinateur pour qu’il devienne un serveur de Fédération. Vous pouvez effectuer l'une des opérations suivantes :  
  
-   [Configurer le premier serveur de Fédération dans une nouvelle batterie de serveurs de Fédération](Configure-a-Federation-Server.md#configure-the-first-federation-server-in-a-new-federation-server-farm)  
  
-   [Ajouter un serveur de Fédération à une batterie de serveurs de fédération existante](Configure-a-Federation-Server.md#add-a-federation-server-to-an-existing-federation-server-farm)  
  
## <a name="configure-the-first-federation-server-in-a-new-federation-server-farm"></a>Configurer le premier serveur de fédération dans une nouvelle batterie de serveurs de fédération  
  
### <a name="to-configure-the-first-federation-server-in-a-new-federation-server-farm-by-using-the-active-directory-federation-service-configuration-wizard"></a>Pour configurer le premier serveur de Fédération dans une nouvelle batterie de serveurs de Fédération à l’aide de l’Assistant Configuration de service FS (Federation Service) Active Directory  
  
> [!NOTE]  
> Avant d’effectuer cette procédure, assurez-vous que vous disposez des autorisations d’administrateur de domaine ou que vous disposez des informations d’identification d’administrateur de domaine.  
  
1.  Dans la page **Tableau de bord** du Gestionnaire de serveur, cliquez sur le drapeau **Notifications**, puis sur **Configurer le service FS (Federation Service) sur le serveur**.  
  
    L'**Assistant Configuration des services AD FS (Active Directory Federation Services)** s'ouvre.  
  
2.  Dans la page **Bienvenue**, sélectionnez **Créer le premier serveur de fédération dans une batterie de serveurs de fédération**, puis cliquez sur **Suivant**.  
  
3.  Sur la page **se connecter à AD DS** , spécifiez un compte à l’aide des autorisations d’administrateur de domaine pour le domaine Active Directory \(ad\) auquel cet ordinateur est joint, puis cliquez sur **suivant**.  
  
4.  Dans la page **Spécifier les propriétés de service**, effectuez la procédure suivante, puis cliquez sur **Suivant** :  
  
    -   Importez le fichier. pfx qui contient le certificat et la clé SSL\) \(SSL que vous avez obtenus précédemment. À l' [étape 2 : inscrire un certificat SSL pour AD FS](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md), vous avez obtenu ce certificat et vous l’avez copié sur l’ordinateur que vous souhaitez configurer en tant que serveur de Fédération. Pour importer le fichier. pfx à l’aide de l’Assistant, cliquez sur **Importer**, puis accédez à l’emplacement du fichier. Lorsque vous y êtes invité, entrez le mot de passe du fichier. pfx.  
  
    -   Indiquez le nom de votre service FS (Federation Service). Exemple : **fs.contoso.com**. Ce nom doit correspondre à l'un des noms d'objets ou autres noms d'objets du certificat.  
  
    -   Indiquez le nom d'affichage de votre service FS (Federation Service). Exemple : **Contoso Corporation**. Les utilisateurs voient ce nom sur le Services ADFS \(AD FS\) signe\-dans la page.  
  
5.  Dans la page **Spécifier un compte de service**, indiquez un compte de service. Vous pouvez créer ou utiliser un compte de service géré de groupe existant \(gMSA\) ou utiliser un compte d’utilisateur de domaine existant. Si vous sélectionnez l’option de création d’un nouveau compte gMSA, spécifiez un nom pour le nouveau compte. Si vous sélectionnez l’option permettant d’utiliser un compte de domaine ou un gMSA existant, cliquez sur **Sélectionner** pour sélectionner un compte.  
  
    > [!NOTE]  
    > L’avantage de l’utilisation d’un compte gMSA est sa fonctionnalité de mise à jour automatique de mot de passe\-négocié.  
  
    > [!WARNING]  
    > Si vous souhaitez utiliser un compte gMSA, vous devez disposer d’au moins un contrôleur de domaine dans votre environnement qui exécute le système d’exploitation Windows Server 2012.  
    >   
    > Si l’option gMSA est désactivée et qu’un message d’erreur s’affiche, tel que les **comptes de service administrés de groupe ne sont pas disponibles, car la clé racine KDS n’a pas été définie**, vous pouvez activer gMSA dans votre domaine en exécutant la commande Windows PowerShell suivante sur un contrôleur de domaine qui exécute Windows Server 2012 ou une version ultérieure, dans votre Active Directory domaine : `Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`. Revenez ensuite à l’Assistant, cliquez sur **précédent**, puis cliquez sur **suivant** pour\-entrez la page spécifier le **compte de service** . L’option gMSA doit maintenant être activée. Vous pouvez le sélectionner et entrer le nom du compte gMSA que vous souhaitez utiliser.  
  
6.  Dans la page **spécifier la base de données de configuration** , spécifiez une base de données de configuration AD FS, puis cliquez sur **suivant**. Vous pouvez créer une base de données sur cet ordinateur à l’aide de la base de données interne Windows \(\)WID. vous pouvez aussi spécifier l’emplacement et le nom de l’instance de Microsoft SQL Server.  
  
    Pour plus d'informations, consultez [Rôle de la base de données de configuration AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
    > [!IMPORTANT]  
    > Si vous souhaitez créer une batterie de AD FS et utiliser SQL Server pour stocker vos données de configuration, vous pouvez utiliser SQL Server 2008 et versions plus récentes, y compris SQL Server 2012 et SQL Server 2014.  
  
7.  Dans la page **Examiner les options**, vérifiez les choix que vous avez effectués pour la configuration, puis cliquez sur **Suivant**.  
  
8.  Sur la page vérifications préalables **requises pour\-** , vérifiez que toutes les vérifications de la configuration requise sont correctement effectuées, puis cliquez sur **configurer**.  
  
9. Sur la page **résultats** , passez en revue les résultats et vérifiez si la configuration est terminée, puis cliquez sur **étapes suivantes requises pour effectuer votre déploiement du service de Fédération**. Pour plus d’informations, consultez [étapes suivantes pour terminer l’installation de votre AD FS](https://go.microsoft.com/fwlink/p/?LinkId=286704). Cliquez sur **Fermer** pour quitter l'Assistant.  
  
### <a name="to-configure-the-first-federation-server-in-a-new-federation-server-farm-via-windows-powershell"></a>Pour configurer le premier serveur de fédération dans une nouvelle batterie de serveurs de fédération via Windows PowerShell  
Vous pouvez créer une nouvelle batterie de serveurs de Fédération à l’aide d’un compte gMSA nouveau ou existant ou d’un compte d’utilisateur de domaine existant.  
  
-   **Si vous souhaitez créer un nouveau serveur de Fédération à l’aide d’un nouveau compte gMSA, procédez comme suit :**  
  
    > [!IMPORTANT]  
    > Vous devez avoir des autorisations d'administrateur de domaine pour créer le premier serveur de fédération dans une nouvelle batterie de serveurs de fédération.  
  
    1.  Sur l’ordinateur que vous souhaitez configurer en tant que serveur de Fédération, assurez-vous que le certificat SSL requis a été importé dans l' **ordinateur Local\\répertoire mon magasin** . Vous pouvez vérifier si le certificat SSL a été importé en exécutant la commande suivante dans la fenêtre de commande Windows PowerShell : `dir Cert:\LocalMachine\My`. Le certificat est listé par son empreinte numérique dans l' **ordinateur Local\\répertoire mon magasin** .  
  
    2.  Sur votre contrôleur de domaine, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante pour vérifier si la clé racine KDS a été créée dans votre domaine : `Get-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`. S’il n’a pas été créé afin que la sortie n’affiche pas d’informations, exécutez la commande suivante pour créer la clé : `Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`.  
  
    3.  Sur l'ordinateur à configurer en tant que serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante :  
  
        ```  
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_Name>$  
        ```  
  
        > [!WARNING]  
        > Le signe `$` à la fin de la commande précédente est requis.  
  
        Pour obtenir la valeur de `<certificate_thumbprint>`, exécutez `dir Cert:\LocalMachine\My`, puis sélectionnez l’empreinte numérique de votre certificat SSL. La valeur de `<federation_service_name>` est le nom de votre service de Fédération, par exemple, **FS.contoso.com**.  
  
        > [!NOTE]  
        > Si ce n’est pas la première fois que vous exécutez cette commande, ajoutez le paramètre `OverwriteConfiguration`.  
  
        > [!NOTE]  
        > La commande précédente crée une batterie de serveurs WID. Si vous souhaitez créer une batterie de serveurs SQL Server, vous devez disposer d’une instance de SQL Server déjà installée et opérationnelle.  
        >   
        > Vous pouvez utiliser la commande suivante pour créer le premier serveur de Fédération dans une nouvelle batterie de serveurs qui utilise une instance de SQL Server : `Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name?\<SQL_instance_ name>;Integrated Security=True"` où **< nom de\_d’ordinateur hôte sql\_>** est le nom du serveur sur lequel SQL Server s’exécute, et **< instance SQL\_\_nom** > est le nom de l’instance de SQL Server. Si vous utilisez l’instance par défaut de SQL Server, utilisez une valeur **sqlConnectionString** de «**Source de données\=< le nom de l’hôte SQL\_\_>; sécurité intégrée\=true**».  
  
        > [!IMPORTANT]  
        > Si vous voulez créer une batterie AD FS et utiliser SQL Server pour stocker vos données de configuration, vous pouvez utiliser SQL Server 2008 et versions plus récentes, notamment SQL Server 2012.  
  
-   **Si vous souhaitez créer un nouveau serveur de Fédération à l’aide d’un compte d’utilisateur de domaine existant, procédez comme suit :**  
  
    1.  Sur l’ordinateur que vous souhaitez configurer en tant que serveur de Fédération, assurez-vous que le certificat SSL requis a été importé dans l' **ordinateur Local\\répertoire mon magasin** . Vous pouvez vérifier si le certificat SSL a été importé en exécutant la commande suivante dans la fenêtre de commande Windows PowerShell : `dir Cert:\LocalMachine\My`. Le certificat est listé par son empreinte numérique dans l' **ordinateur Local\\répertoire mon magasin** .  
  
    2.  Sur l’ordinateur que vous souhaitez configurer en tant que serveur de Fédération, ouvrez la fenêtre de commande Windows PowerShell, puis exécutez la commande suivante : `$fscred = Get-Credential`. Entrez les informations d’identification du compte d’utilisateur de domaine que vous souhaitez utiliser pour le compte de service de Fédération au format domaine\\nom d’utilisateur.  
  
    3.  Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante :  
  
        ```  
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscred  
        ```  
  
        Pour obtenir la valeur de **<\_d’empreinte numérique de certificat >** , exécutez `dir Cert:\LocalMachine\My`, puis sélectionnez l’empreinte numérique de votre certificat SSL. La valeur de **< federation\_service\_nom >** est le nom de votre service de Fédération, par exemple, FS.contoso.com.  
  
        > [!NOTE]  
        > Si ce n’est pas la première fois que vous exécutez cette commande, ajoutez le paramètre `OverwriteConfiguration`.  
  
        > [!NOTE]  
        > La commande précédente crée une batterie de serveurs WID. Si vous souhaitez créer une batterie de serveurs SQL Server, vous devez disposer de l’instance de SQL Server déjà installée et opérationnelle.  
        >   
        > Vous pouvez utiliser la commande suivante pour créer le premier serveur de Fédération dans une nouvelle batterie de serveurs qui utilise une instance de SQL Server : `Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"` où le nom de\_de l' **hôte sql\_** est le nom du serveur sur lequel SQL Server s’exécute, et le nom de l' **instance SQL\_** est le nom de l’instance de\_. Si vous utilisez l’instance par défaut de SQL Server, utilisez une valeur **sqlConnectionString** de «**Source de données\=< le nom de l’hôte SQL\_\_>; sécurité intégrée\=true**».  
  
        > [!IMPORTANT]  
        > Si vous souhaitez créer une batterie de AD FS et utiliser SQL Server pour stocker vos données de configuration, vous pouvez utiliser SQL Server 2008 et versions plus récentes, y compris SQL Server 2012 et SQL Server 2014.  
  
## <a name="add-a-federation-server-to-an-existing-federation-server-farm"></a>Ajouter un serveur de fédération à une batterie de serveurs de fédération existante  
  
> [!IMPORTANT]  
> Vérifiez que vous avez terminé [l’étape 3 : installer le service de rôle AD FS](../../ad-fs/deployment/Install-the-AD-FS-Role-Service.md), avant de commencer l’une des procédures décrites dans cette section.  
  
> [!IMPORTANT]  
> Avant d’effectuer cette procédure, assurez-vous que vous avez obtenu un certificat d’authentification de serveur SSL valide.  
  
### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-the-active-directory-federation-service-configuration-wizard"></a>Pour ajouter un serveur de fédération à une batterie de serveurs de fédération existante via l'Assistant Configuration des services AD FS (Active Directory Federation Services)  
  
1.  Dans la page **Tableau de bord** du Gestionnaire de serveur, cliquez sur le drapeau **Notifications**, puis sur **Configurer le service FS (Federation Service) sur le serveur**.  
  
    L'**Assistant Configuration des services AD FS (Active Directory Federation Services)** s'ouvre.  
  
2.  Sur la page **Bienvenue** , sélectionnez **Ajouter un serveur de Fédération à une batterie de serveurs de Fédération**, puis cliquez sur **suivant**.  
  
3.  Sur la page **se connecter à AD DS** , spécifiez un compte en utilisant des autorisations d’administrateur de domaine pour le domaine Active Directory auquel cet ordinateur est joint, puis cliquez sur **suivant**.  
  
4.  Dans la page **spécifier une batterie** de serveurs, indiquez le nom du serveur de Fédération principal dans une batterie de serveurs qui utilise wid ou spécifiez le nom d’hôte de la base de données et le nom de l’instance de base de données d’une batterie de serveurs de fédération existante qui utilise SQL Server.  
  
    > [!WARNING]  
    > Dans Windows Server&reg; 2012 R2, il existe une solution de contournement pour spécifier l’instance par défaut de SQL Server. Elle consiste à ne pas utiliser l'interface utilisateur. Au lieu de cela, suivez les étapes de la section [pour configurer le premier serveur de Fédération dans une nouvelle batterie de serveurs de Fédération à l’aide de Windows PowerShell](Configure-a-Federation-Server.md#BKMK_3).  
  
    > [!IMPORTANT]  
    > Si vous voulez créer une batterie AD FS et utiliser SQL Server pour stocker vos données de configuration, vous pouvez utiliser SQL Server 2008 et versions plus récentes, notamment SQL Server 2012.  
  
5.  Dans la page **spécifier le certificat SSL** , importez le fichier. pfx qui contient le certificat et la clé SSL que vous avez obtenus précédemment. Ce certificat est le certificat d'authentification de service requis. À l' [étape 2 : inscrire un certificat SSL pour AD FS](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md), vous avez obtenu ce certificat et vous l’avez copié sur l’ordinateur que vous souhaitez configurer en tant que serveur de Fédération. Pour importer le fichier. pfx à l’aide de l’Assistant, cliquez sur **Importer** et accédez à l’emplacement du fichier. Lorsque vous y êtes invité, entrez le mot de passe du fichier. pfx.  
  
6.  Sur la page **spécifier un compte de service** , spécifiez le même compte de service que celui que vous avez configuré lors de la création du premier serveur de Fédération de la batterie de serveurs. Vous pouvez soit utiliser un compte de service administré de groupe existant, soit utiliser un compte d'utilisateur de domaine existant.  
  
    > [!IMPORTANT]  
    > Le compte que vous spécifiez doit être le même que le compte utilisé sur le serveur de Fédération principal dans cette batterie de serveurs.  
  
7.  Dans la page **Examiner les options**, vérifiez les choix que vous avez effectués pour la configuration, puis cliquez sur **Suivant**.  
  
8.  Sur la page vérifications préalables **requises pour\-** , vérifiez que toutes les vérifications de la configuration requise sont correctement effectuées, puis cliquez sur **configurer**.  
  
9. Sur la page **résultats** , passez en revue les résultats et vérifiez si la configuration est terminée, puis cliquez sur **étapes suivantes requises pour effectuer votre déploiement du service de Fédération**. Pour plus d’informations, consultez [étapes suivantes pour terminer l’installation de votre AD FS](https://go.microsoft.com/fwlink/p/?LinkId=286704). Cliquez sur **Fermer** pour quitter l'Assistant.  
  
### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-windows-powershell"></a>Pour ajouter un serveur de fédération à une batterie de serveurs de fédération existante via Windows PowerShell  
Vous pouvez ajouter un serveur de Fédération à une batterie de serveurs existante à l’aide d’un compte gMSA existant ou d’un compte d’utilisateur de domaine existant.  
  
-   Si vous souhaitez joindre un serveur de Fédération à une batterie de serveurs à l’aide d’un compte gMSA existant, procédez comme suit :  
  
    1.  Sur l’ordinateur que vous souhaitez configurer en tant que serveur de Fédération, assurez-vous que le certificat SSL requis a été importé dans l' **ordinateur Local\\répertoire mon magasin** . Vous pouvez vérifier si le certificat SSL a été importé en exécutant la commande suivante dans la fenêtre de commande Windows PowerShell : `dir Cert:\LocalMachine\My`. Le certificat est listé par son empreinte numérique dans l' **ordinateur Local\\répertoire mon magasin** .  
  
    2.  Sur l’ordinateur que vous souhaitez configurer en tant que serveur de Fédération, ouvrez la fenêtre de commande Windows PowerShell, puis exécutez la commande suivante.  
  
        ```  
        Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>  
        ```  
  
        `<domain>\<GMSA_name>` est votre domaine Active Directory et le nom de votre compte gMSA dans ce domaine. `<first_federation_server_hostname>` est le nom d’hôte du serveur de Fédération principal dans cette batterie de serveurs existante.  
  
        Vous pouvez obtenir la valeur de `<certificate_thumbprint>` en exécutant `dir Cert:\LocalMachine\My` à l’étape précédente.  
  
        > [!NOTE]  
        > Si ce n’est pas la première fois que vous exécutez cette commande, ajoutez le paramètre `OverwriteConfiguration`.  
  
        > [!NOTE]  
        > La commande précédente crée un nœud de batterie de serveurs WID. Si vous souhaitez créer un nœud de batterie de serveurs d’ordinateurs exécutant SQL Server, vous devez disposer de l’instance de SQL Server déjà installée et opérationnelle.  
        >   
        > Vous pouvez utiliser la commande suivante pour ajouter un serveur de Fédération à une batterie de serveurs existante qui utilise une instance de SQL Server : `Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"` où le nom de\_de l' **hôte sql\_** est le nom du serveur sur lequel SQL Server est en cours d’exécution, et le nom de l' **instance SQL\_\_nom** est le nom de l’instance de SQL Server. Si vous utilisez l’instance par défaut de SQL Server, utilisez une valeur **sqlConnectionString** de «**Source de données\=< le nom de l’hôte SQL\_\_>; sécurité intégrée\=true**».  
  
        > [!IMPORTANT]  
        > Si vous souhaitez créer une batterie de AD FS et utiliser SQL Server pour stocker vos données de configuration, vous pouvez utiliser SQL Server 2008 et versions plus récentes, y compris SQL Server 2012 et SQL Server 2014.  
  
-   Si vous souhaitez joindre un serveur de Fédération à une batterie de serveurs à l’aide d’un compte d’utilisateur de domaine existant, procédez comme suit :  
  
    1.  Sur l’ordinateur que vous souhaitez configurer en tant que serveur de Fédération, ouvrez la fenêtre PowerShellcommand de Windows, puis exécutez la commande suivante : `$fscred = get-credential`. Entrez les informations d’identification du compte d’utilisateur de domaine que vous souhaitez utiliser pour le compte de service de Fédération au format domaine\\nom d’utilisateur.  
  
    2.  Sur l’ordinateur que vous souhaitez configurer en tant que serveur de Fédération, assurez-vous que le certificat SSL requis a été importé dans l' **ordinateur Local\\répertoire mon magasin** . Vous pouvez vérifier si le certificat SSL a été importé en exécutant la commande suivante dans la fenêtre PowerShellcommand de Windows : `dir Cert:\LocalMachine\My`. Le certificat est listé par son empreinte numérique dans l' **ordinateur Local\\répertoire mon magasin** .  
  
    3.  Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante.  
  
        ```  
        Add-AdfsFarmNode -ServiceAccountCredential $fscred -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>  
        ```  
  
        > [!NOTE]  
        > Si ce n’est pas la première fois que vous exécutez cette commande, ajoutez le paramètre `OverwriteConfiguration`.  
  
        > [!NOTE]  
        > La commande précédente crée un nœud de batterie de serveurs WID. Si vous souhaitez créer un nœud de batterie de serveurs d’ordinateurs exécutant SQL Server, vous devez disposer de l’instance de SQL Server déjà installée et opérationnelle. Vous pouvez utiliser la commande suivante pour ajouter un serveur de Fédération à une batterie de serveurs existante à l’aide d’une instance de SQL Server : `Add-AdfsFarmNode -ServiceAccountCredential $fscred -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"` où le nom de\_de l' **hôte sql\_** est le nom du serveur sur lequel l’instance de SQL Server s’exécute et le nom de l’instance **SQL\_\_nom** est le nom de l’instance de SQL Server. Si vous utilisez l’instance par défaut de SQL Server, utilisez une valeur **sqlConnectionString** de «**Source de données\=< le nom de l’hôte SQL\_\_>; sécurité intégrée\=true**».  
  
        > [!IMPORTANT]  
        > Si vous souhaitez créer une batterie de AD FS et utiliser SQL Server pour stocker vos données de configuration, vous pouvez utiliser SQL Server 2008 et versions plus récentes, y compris SQL Server 2012 et SQL Server 2014.  
  

Après avoir installé le Services ADFS \(AD FS\) service de rôle sur votre ordinateur, vous êtes prêt à configurer cet ordinateur pour qu’il devienne un serveur de Fédération. Vous pouvez effectuer l'une des opérations suivantes :

-   [Configurer le premier serveur de Fédération dans une nouvelle batterie de serveurs de Fédération](Configure-a-Federation-Server.md#BKMK_1)

-   [Ajouter un serveur de Fédération à une batterie de serveurs de fédération existante](Configure-a-Federation-Server.md#BKMK_2)

## <a name="configure-the-first-federation-server-in-a-new-federation-server-farm"></a><a name="BKMK_1"></a>Configurer le premier serveur de Fédération dans une nouvelle batterie de serveurs de Fédération

### <a name="to-configure-the-first-federation-server-in-a-new-federation-server-farm-by-using-the-active-directory-federation-service-configuration-wizard"></a>Pour configurer le premier serveur de Fédération dans une nouvelle batterie de serveurs de Fédération à l’aide de l’Assistant Configuration de service FS (Federation Service) Active Directory

> [!NOTE]
> Avant d’effectuer cette procédure, assurez-vous que vous disposez des autorisations d’administrateur de domaine ou que vous disposez des informations d’identification d’administrateur de domaine.

1.  Dans la page **Tableau de bord** du Gestionnaire de serveur, cliquez sur le drapeau **Notifications**, puis sur **Configurer le service FS (Federation Service) sur le serveur**.

    L'**Assistant Configuration des services AD FS (Active Directory Federation Services)** s'ouvre.

2.  Dans la page **Bienvenue**, sélectionnez **Créer le premier serveur de fédération dans une batterie de serveurs de fédération**, puis cliquez sur **Suivant**.

3.  Sur la page **se connecter à AD DS** , spécifiez un compte à l’aide des autorisations d’administrateur de domaine pour le domaine Active Directory \(ad\) auquel cet ordinateur est joint, puis cliquez sur **suivant**.

4.  Dans la page **Spécifier les propriétés de service**, effectuez la procédure suivante, puis cliquez sur **Suivant** :

    -   Importez le fichier. pfx qui contient le certificat et la clé SSL\) \(SSL que vous avez obtenus précédemment. À l' [étape 2 : inscrire un certificat SSL pour AD FS](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md), vous avez obtenu ce certificat et vous l’avez copié sur l’ordinateur que vous souhaitez configurer en tant que serveur de Fédération. Pour importer le fichier. pfx à l’aide de l’Assistant, cliquez sur **Importer**, puis accédez à l’emplacement du fichier. Lorsque vous y êtes invité, entrez le mot de passe du fichier. pfx.

    -   Indiquez le nom de votre service FS (Federation Service). Exemple : **fs.contoso.com**. Ce nom doit correspondre à l'un des noms d'objets ou autres noms d'objets du certificat.

    -   Indiquez le nom d'affichage de votre service FS (Federation Service). Exemple : **Contoso Corporation**. Les utilisateurs voient ce nom sur le Services ADFS \(AD FS\) signe\-dans la page.

5.  Dans la page **Spécifier un compte de service**, indiquez un compte de service. Vous pouvez créer ou utiliser un compte de service géré de groupe existant \(gMSA\) ou utiliser un compte d’utilisateur de domaine existant. Si vous sélectionnez l’option de création d’un nouveau compte gMSA, spécifiez un nom pour le nouveau compte. Si vous sélectionnez l’option permettant d’utiliser un compte de domaine ou un gMSA existant, cliquez sur **Sélectionner** pour sélectionner un compte.

    > [!NOTE]
    > L’avantage de l’utilisation d’un compte gMSA est sa fonctionnalité de mise à jour automatique de mot de passe\-négocié.

    > [!WARNING]
    > Si vous souhaitez utiliser un compte gMSA, vous devez disposer d’au moins un contrôleur de domaine dans votre environnement qui exécute le système d’exploitation Windows Server 2012.
    >
    > Si l’option gMSA est désactivée et qu’un message d’erreur s’affiche, tel que les **comptes de service administrés de groupe ne sont pas disponibles, car la clé racine KDS n’a pas été définie**, vous pouvez activer gMSA dans votre domaine en exécutant la commande Windows PowerShell suivante sur un contrôleur de domaine qui exécute Windows Server 2012 ou une version ultérieure, dans votre Active Directory domaine : `Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`. Revenez ensuite à l’Assistant, cliquez sur **précédent**, puis cliquez sur **suivant** pour\-entrez la page spécifier le **compte de service** . L’option gMSA doit maintenant être activée. Vous pouvez le sélectionner et entrer le nom du compte gMSA que vous souhaitez utiliser.

6.  Dans la page **spécifier la base de données de configuration** , spécifiez une base de données de configuration AD FS, puis cliquez sur **suivant**. Vous pouvez créer une base de données sur cet ordinateur à l’aide de la base de données interne Windows \(\)WID. vous pouvez aussi spécifier l’emplacement et le nom de l’instance de Microsoft SQL Server.

    Pour plus d'informations, consultez [Rôle de la base de données de configuration AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).

    > [!IMPORTANT]
    > Si vous souhaitez créer une batterie de AD FS et utiliser SQL Server pour stocker vos données de configuration, vous pouvez utiliser SQL Server 2008 et versions plus récentes, y compris SQL Server 2012 et SQL Server 2014.

7.  Dans la page **Examiner les options**, vérifiez les choix que vous avez effectués pour la configuration, puis cliquez sur **Suivant**.

8.  Sur la page vérifications préalables **requises pour\-** , vérifiez que toutes les vérifications de la configuration requise sont correctement effectuées, puis cliquez sur **configurer**.

9. Sur la page **résultats** , passez en revue les résultats et vérifiez si la configuration est terminée, puis cliquez sur **étapes suivantes requises pour effectuer votre déploiement du service de Fédération**. Pour plus d’informations, consultez [étapes suivantes pour terminer l’installation de votre AD FS](https://go.microsoft.com/fwlink/p/?LinkId=286704). Cliquez sur **Fermer** pour quitter l'Assistant.

### <a name="to-configure-the-first-federation-server-in-a-new-federation-server-farm-via-windows-powershell"></a><a name="BKMK_3"></a>Pour configurer le premier serveur de Fédération dans une nouvelle batterie de serveurs de Fédération à l’aide de Windows PowerShell
Vous pouvez créer une nouvelle batterie de serveurs de Fédération à l’aide d’un compte gMSA nouveau ou existant ou d’un compte d’utilisateur de domaine existant.

-   **Si vous souhaitez créer un nouveau serveur de Fédération à l’aide d’un nouveau compte gMSA, procédez comme suit :**

    > [!IMPORTANT]
    > Vous devez avoir des autorisations d'administrateur de domaine pour créer le premier serveur de fédération dans une nouvelle batterie de serveurs de fédération.

    1.  Sur l’ordinateur que vous souhaitez configurer en tant que serveur de Fédération, assurez-vous que le certificat SSL requis a été importé dans l' **ordinateur Local\\répertoire mon magasin** . Vous pouvez vérifier si le certificat SSL a été importé en exécutant la commande suivante dans la fenêtre de commande Windows PowerShell : `dir Cert:\LocalMachine\My`. Le certificat est listé par son empreinte numérique dans l' **ordinateur Local\\répertoire mon magasin** .

    2.  Sur votre contrôleur de domaine, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante pour vérifier si la clé racine KDS a été créée dans votre domaine : `Get-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`. S’il n’a pas été créé afin que la sortie n’affiche pas d’informations, exécutez la commande suivante pour créer la clé : `Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`.

    3.  Sur l'ordinateur à configurer en tant que serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante :

        ```
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_Name>$
        ```

        > [!WARNING]
        > Le signe `$` à la fin de la commande précédente est requis.

        Pour obtenir la valeur de `<certificate_thumbprint>`, exécutez `dir Cert:\LocalMachine\My`, puis sélectionnez l’empreinte numérique de votre certificat SSL. La valeur de `<federation_service_name>` est le nom de votre service de Fédération, par exemple, **FS.contoso.com**.

        > [!NOTE]
        > Si ce n’est pas la première fois que vous exécutez cette commande, ajoutez le paramètre `OverwriteConfiguration`.

        > [!NOTE]
        > La commande précédente crée une batterie de serveurs WID. Si vous souhaitez créer une batterie de serveurs SQL Server, vous devez disposer d’une instance de SQL Server déjà installée et opérationnelle.
        >
        > Vous pouvez utiliser la commande suivante pour créer le premier serveur de Fédération dans une nouvelle batterie de serveurs qui utilise une instance de SQL Server : `Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name?\<SQL_instance_ name>;Integrated Security=True"` où **< nom de\_d’ordinateur hôte sql\_>** est le nom du serveur sur lequel SQL Server s’exécute, et **< instance SQL\_\_nom** > est le nom de l’instance de SQL Server. Si vous utilisez l’instance par défaut de SQL Server, utilisez une valeur **sqlConnectionString** de «**Source de données\=< le nom de l’hôte SQL\_\_>; sécurité intégrée\=true**».

        > [!IMPORTANT]
        > Si vous voulez créer une batterie AD FS et utiliser SQL Server pour stocker vos données de configuration, vous pouvez utiliser SQL Server 2008 et versions plus récentes, notamment SQL Server 2012.

-   **Si vous souhaitez créer un nouveau serveur de Fédération à l’aide d’un compte d’utilisateur de domaine existant, procédez comme suit :**

    1.  Sur l’ordinateur que vous souhaitez configurer en tant que serveur de Fédération, assurez-vous que le certificat SSL requis a été importé dans l' **ordinateur Local\\répertoire mon magasin** . Vous pouvez vérifier si le certificat SSL a été importé en exécutant la commande suivante dans la fenêtre de commande Windows PowerShell : `dir Cert:\LocalMachine\My`. Le certificat est listé par son empreinte numérique dans l' **ordinateur Local\\répertoire mon magasin** .

    2.  Sur l’ordinateur que vous souhaitez configurer en tant que serveur de Fédération, ouvrez la fenêtre de commande Windows PowerShell, puis exécutez la commande suivante : `$fscred = Get-Credential`. Entrez les informations d’identification du compte d’utilisateur de domaine que vous souhaitez utiliser pour le compte de service de Fédération au format domaine\\nom d’utilisateur.

    3.  Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante :

        ```
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscred
        ```

        Pour obtenir la valeur de **<\_d’empreinte numérique de certificat >** , exécutez `dir Cert:\LocalMachine\My`, puis sélectionnez l’empreinte numérique de votre certificat SSL. La valeur de **< federation\_service\_nom >** est le nom de votre service de Fédération, par exemple, FS.contoso.com.

        > [!NOTE]
        > Si ce n’est pas la première fois que vous exécutez cette commande, ajoutez le paramètre `OverwriteConfiguration`.

        > [!NOTE]
        > La commande précédente crée une batterie de serveurs WID. Si vous souhaitez créer une batterie de serveurs SQL Server, vous devez disposer de l’instance de SQL Server déjà installée et opérationnelle.
        >
        > Vous pouvez utiliser la commande suivante pour créer le premier serveur de Fédération dans une nouvelle batterie de serveurs qui utilise une instance de SQL Server : `Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"` où le nom de\_de l' **hôte sql\_** est le nom du serveur sur lequel SQL Server s’exécute, et le nom de l' **instance SQL\_** est le nom de l’instance de\_. Si vous utilisez l’instance par défaut de SQL Server, utilisez une valeur **sqlConnectionString** de «**Source de données\=< le nom de l’hôte SQL\_\_>; sécurité intégrée\=true**».

        > [!IMPORTANT]
        > Si vous souhaitez créer une batterie de AD FS et utiliser SQL Server pour stocker vos données de configuration, vous pouvez utiliser SQL Server 2008 et versions plus récentes, y compris SQL Server 2012 et SQL Server 2014.

## <a name="add-a-federation-server-to-an-existing-federation-server-farm"></a><a name="BKMK_2"></a>Ajouter un serveur de Fédération à une batterie de serveurs de fédération existante

> [!IMPORTANT]
> Vérifiez que vous avez terminé [l’étape 3 : installer le service de rôle AD FS](../../ad-fs/deployment/Install-the-AD-FS-Role-Service.md), avant de commencer l’une des procédures décrites dans cette section.

> [!IMPORTANT]
> Avant d’effectuer cette procédure, assurez-vous que vous avez obtenu un certificat d’authentification de serveur SSL valide.

### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-the-active-directory-federation-service-configuration-wizard"></a>Pour ajouter un serveur de fédération à une batterie de serveurs de fédération existante via l'Assistant Configuration des services AD FS (Active Directory Federation Services)

1.  Dans la page **Tableau de bord** du Gestionnaire de serveur, cliquez sur le drapeau **Notifications**, puis sur **Configurer le service FS (Federation Service) sur le serveur**.

    L'**Assistant Configuration des services AD FS (Active Directory Federation Services)** s'ouvre.

2.  Sur la page **Bienvenue** , sélectionnez **Ajouter un serveur de Fédération à une batterie de serveurs de Fédération**, puis cliquez sur **suivant**.

3.  Sur la page **se connecter à AD DS** , spécifiez un compte en utilisant des autorisations d’administrateur de domaine pour le domaine Active Directory auquel cet ordinateur est joint, puis cliquez sur **suivant**.

4.  Dans la page **spécifier une batterie** de serveurs, indiquez le nom du serveur de Fédération principal dans une batterie de serveurs qui utilise wid ou spécifiez le nom d’hôte de la base de données et le nom de l’instance de base de données d’une batterie de serveurs de fédération existante qui utilise SQL Server.

    > [!WARNING]
    > Dans Windows Server&reg; 2012 R2, il existe une solution de contournement pour spécifier l’instance par défaut de SQL Server. Elle consiste à ne pas utiliser l'interface utilisateur. Au lieu de cela, suivez les étapes de la section [pour configurer le premier serveur de Fédération dans une nouvelle batterie de serveurs de Fédération à l’aide de Windows PowerShell](Configure-a-Federation-Server.md#BKMK_3).

    > [!IMPORTANT]
    > Si vous voulez créer une batterie AD FS et utiliser SQL Server pour stocker vos données de configuration, vous pouvez utiliser SQL Server 2008 et versions plus récentes, notamment SQL Server 2012.

5.  Dans la page **spécifier le certificat SSL** , importez le fichier. pfx qui contient le certificat et la clé SSL que vous avez obtenus précédemment. Ce certificat est le certificat d'authentification de service requis. À l' [étape 2 : inscrire un certificat SSL pour AD FS](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md), vous avez obtenu ce certificat et vous l’avez copié sur l’ordinateur que vous souhaitez configurer en tant que serveur de Fédération. Pour importer le fichier. pfx à l’aide de l’Assistant, cliquez sur **Importer** et accédez à l’emplacement du fichier. Lorsque vous y êtes invité, entrez le mot de passe du fichier. pfx.

6.  Sur la page **spécifier un compte de service** , spécifiez le même compte de service que celui que vous avez configuré lors de la création du premier serveur de Fédération de la batterie de serveurs. Vous pouvez soit utiliser un compte de service administré de groupe existant, soit utiliser un compte d'utilisateur de domaine existant.

    > [!IMPORTANT]
    > Le compte que vous spécifiez doit être le même que le compte utilisé sur le serveur de Fédération principal dans cette batterie de serveurs.

7.  Dans la page **Examiner les options**, vérifiez les choix que vous avez effectués pour la configuration, puis cliquez sur **Suivant**.

8.  Sur la page vérifications préalables **requises pour\-** , vérifiez que toutes les vérifications de la configuration requise sont correctement effectuées, puis cliquez sur **configurer**.

9. Sur la page **résultats** , passez en revue les résultats et vérifiez si la configuration est terminée, puis cliquez sur **étapes suivantes requises pour effectuer votre déploiement du service de Fédération**. Pour plus d’informations, consultez [étapes suivantes pour terminer l’installation de votre AD FS](https://go.microsoft.com/fwlink/p/?LinkId=286704). Cliquez sur **Fermer** pour quitter l'Assistant.

### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-windows-powershell"></a>Pour ajouter un serveur de fédération à une batterie de serveurs de fédération existante via Windows PowerShell
Vous pouvez ajouter un serveur de Fédération à une batterie de serveurs existante à l’aide d’un compte gMSA existant ou d’un compte d’utilisateur de domaine existant.

-   Si vous souhaitez joindre un serveur de Fédération à une batterie de serveurs à l’aide d’un compte gMSA existant, procédez comme suit :

    1.  Sur l’ordinateur que vous souhaitez configurer en tant que serveur de Fédération, assurez-vous que le certificat SSL requis a été importé dans l' **ordinateur Local\\répertoire mon magasin** . Vous pouvez vérifier si le certificat SSL a été importé en exécutant la commande suivante dans la fenêtre de commande Windows PowerShell : `dir Cert:\LocalMachine\My`. Le certificat est listé par son empreinte numérique dans l' **ordinateur Local\\répertoire mon magasin** .

    2.  Sur l’ordinateur que vous souhaitez configurer en tant que serveur de Fédération, ouvrez la fenêtre de commande Windows PowerShell, puis exécutez la commande suivante.

        ```
        Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>
        ```

        `<domain>\<GMSA_name>` est votre domaine Active Directory et le nom de votre compte gMSA dans ce domaine. `<first_federation_server_hostname>` est le nom d’hôte du serveur de Fédération principal dans cette batterie de serveurs existante.

        Vous pouvez obtenir la valeur de `<certificate_thumbprint>` en exécutant `dir Cert:\LocalMachine\My` à l’étape précédente.

        > [!NOTE]
        > Si ce n’est pas la première fois que vous exécutez cette commande, ajoutez le paramètre `OverwriteConfiguration`.

        > [!NOTE]
        > La commande précédente crée un nœud de batterie de serveurs WID. Si vous souhaitez créer un nœud de batterie de serveurs d’ordinateurs exécutant SQL Server, vous devez disposer de l’instance de SQL Server déjà installée et opérationnelle.
        >
        > Vous pouvez utiliser la commande suivante pour ajouter un serveur de Fédération à une batterie de serveurs existante qui utilise une instance de SQL Server : `Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"` où le nom de\_de l' **hôte sql\_** est le nom du serveur sur lequel SQL Server est en cours d’exécution, et le nom de l' **instance SQL\_\_nom** est le nom de l’instance de SQL Server. Si vous utilisez l’instance par défaut de SQL Server, utilisez une valeur **sqlConnectionString** de «**Source de données\=< le nom de l’hôte SQL\_\_>; sécurité intégrée\=true**».

        > [!IMPORTANT]
        > Si vous souhaitez créer une batterie de AD FS et utiliser SQL Server pour stocker vos données de configuration, vous pouvez utiliser SQL Server 2008 et versions plus récentes, y compris SQL Server 2012 et SQL Server 2014.

-   Si vous souhaitez joindre un serveur de Fédération à une batterie de serveurs à l’aide d’un compte d’utilisateur de domaine existant, procédez comme suit :

    1.  Sur l’ordinateur que vous souhaitez configurer en tant que serveur de Fédération, ouvrez la fenêtre PowerShellcommand de Windows, puis exécutez la commande suivante : `$fscred = get-credential`. Entrez les informations d’identification du compte d’utilisateur de domaine que vous souhaitez utiliser pour le compte de service de Fédération au format domaine\\nom d’utilisateur.

    2.  Sur l’ordinateur que vous souhaitez configurer en tant que serveur de Fédération, assurez-vous que le certificat SSL requis a été importé dans l' **ordinateur Local\\répertoire mon magasin** . Vous pouvez vérifier si le certificat SSL a été importé en exécutant la commande suivante dans la fenêtre PowerShellcommand de Windows : `dir Cert:\LocalMachine\My`. Le certificat est listé par son empreinte numérique dans l' **ordinateur Local\\répertoire mon magasin** .

    3.  Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante.

        ```
        Add-AdfsFarmNode -ServiceAccountCredential $fscred -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>
        ```

        > [!NOTE]
        > Si ce n’est pas la première fois que vous exécutez cette commande, ajoutez le paramètre `OverwriteConfiguration`.

        > [!NOTE]
        > La commande précédente crée un nœud de batterie de serveurs WID. Si vous souhaitez créer un nœud de batterie de serveurs d’ordinateurs exécutant SQL Server, vous devez disposer de l’instance de SQL Server déjà installée et opérationnelle. Vous pouvez utiliser la commande suivante pour ajouter un serveur de Fédération à une batterie de serveurs existante à l’aide d’une instance de SQL Server : `Add-AdfsFarmNode -ServiceAccountCredential $fscred -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"` où le nom de\_de l' **hôte sql\_** est le nom du serveur sur lequel l’instance de SQL Server s’exécute et le nom de l’instance **SQL\_\_nom** est le nom de l’instance de SQL Server. Si vous utilisez l’instance par défaut de SQL Server, utilisez une valeur **sqlConnectionString** de «**Source de données\=< le nom de l’hôte SQL\_\_>; sécurité intégrée\=true**».

        > [!IMPORTANT]
        > Si vous souhaitez créer une batterie de AD FS et utiliser SQL Server pour stocker vos données de configuration, vous pouvez utiliser SQL Server 2008 et versions plus récentes, y compris SQL Server 2012 et SQL Server 2014.

## <a name="see-also"></a>Voir aussi

[Déploiement d’AD FS](../../ad-fs/AD-FS-Deployment.md)

[Guide de déploiement de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)

[Déploiement d’une batterie de serveurs de fédération](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)



