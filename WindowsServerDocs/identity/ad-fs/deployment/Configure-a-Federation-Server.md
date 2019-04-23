---
ms.assetid: 434fd617-373a-405e-bae4-da324ea83efc
title: Guide de déploiement des services AD FS Windows Server 2012 R2
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 13ce514dc5f3f70217a26c898cde6fe24d4967c6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847380"
---
# <a name="configure-a-federation-server"></a>Configurer un serveur de fédération

>S'applique à : Windows Server 2016, Windows Server 2012 R2

Après avoir installé les Services de fédération Active Directory \(AD FS\) service de rôle sur votre ordinateur, vous êtes prêt à configurer cet ordinateur pour qu’il devienne un serveur de fédération. Vous pouvez effectuer l’une des opérations suivantes :  
  
-   [Configurer le premier serveur de fédération dans une nouvelle batterie de serveurs de fédération](assetId:///e340cf8f-acf3-4cba-8135-a9353b85e714#BKMK_1)  
  
-   [Ajouter un serveur de fédération à une batterie de serveurs de fédération existante](assetId:///e340cf8f-acf3-4cba-8135-a9353b85e714#BKMK_2)  
  
## <a name="BKMK_1"></a>Configurer le premier serveur de fédération dans une nouvelle batterie de serveurs de fédération  
  
### <a name="to-configure-the-first-federation-server-in-a-new-federation-server-farm-by-using-the-active-directory-federation-service-configuration-wizard"></a>Configurer le premier serveur de fédération dans une nouvelle batterie de serveurs de fédération à l’aide de l’Assistant Configuration des services de fédération Active Directory  
  
> [!NOTE]  
> Veillez à ce que vous disposez des autorisations d’administrateur de domaine d’administrateur de domaine disponibles avant d’effectuer cette procédure.  
  
1.  Dans la page **Tableau de bord** du Gestionnaire de serveur, cliquez sur le drapeau **Notifications** , puis sur **Configurer le service FS (Federation Service) sur le serveur**.  
  
    L'**Assistant Configuration des services AD FS (Active Directory Federation Services)** s'ouvre.  
  
2.  Dans la page **Bienvenue**, sélectionnez **Créer le premier serveur de fédération dans une batterie de serveurs de fédération**, puis cliquez sur **Suivant**.  
  
3.  Sur le **connexion à AD DS** , spécifiez un compte à l’aide d’autorisations d’administrateur de domaine pour Active Directory \(AD\) auquel cet ordinateur est joint à un domaine, puis cliquez sur **suivant**.  
  
4.  Dans la page **Spécifier les propriétés de service**, effectuez la procédure suivante, puis cliquez sur **Suivant** :  
  
    -   Importez le fichier .pfx qui contient le Secure Socket Layer \(SSL\) certificat et la clé que vous avez obtenu précédemment. Dans [étape 2 : Inscrire un certificat SSL pour AD FS](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md), vous avez obtenu ce certificat et copié sur l’ordinateur que vous souhaitez configurer comme serveur de fédération. Pour importer le fichier .pfx via l’Assistant, cliquez sur **importer**, puis accédez à l’emplacement du fichier. Entrez le mot de passe pour le fichier .pfx lorsque vous y êtes invité.  
  
    -   Fournissez un nom pour votre service de fédération. Par exemple, **fs.contoso.com**. Ce nom doit correspondre à l’un de l’objet ou les autres noms d’objet du certificat.  
  
    -   Fournissez un nom d’affichage pour votre service de fédération. Par exemple, **Contoso Corporation**. Les utilisateurs voient ce nom dans les Services de fédération Active Directory \(AD FS\) connexion\-dans la page.  
  
5.  Sur le **spécifier un compte de Service** , spécifiez un compte de service. Vous pouvez créer ou utiliser un compte de Service administré de groupe existant \(gMSA\) ou utiliser un compte d’utilisateur de domaine existant. Si vous sélectionnez l’option permettant de créer un nouveau compte de service administré de groupe, spécifiez un nom pour le nouveau compte. Si vous sélectionnez l’option pour utiliser un compte de domaine ou un service administré de groupe existant, cliquez sur **sélectionnez** à sélectionner un compte.  
  
    > [!NOTE]  
    > L’avantage d’utiliser un compte de service administré de groupe est son automatiquement\-négocié la fonctionnalité de mise à jour de mot de passe.  
  
    > [!WARNING]  
    > Si vous souhaitez utiliser un compte de service administré de groupe, vous devez disposer d’au moins un contrôleur de domaine dans votre environnement qui exécute le système d’exploitation Windows Server 2012.  
    >   
    > Si l’option de service administré de groupe est désactivée, et vous voyez un message d’erreur, tel que **groupe comptes de Service administrés ne sont pas disponibles, car la clé racine KDS n’a pas été définie**, vous pouvez activer le service administré de groupe dans votre domaine en exécutant le Windows suivantes Commande PowerShell sur un contrôleur de domaine qui exécute Windows Server 2012 ou version ultérieure, dans votre domaine Active Directory : `Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`. Puis revenez à l’Assistant, cliquez sur **précédent**, puis cliquez sur **suivant** à re\-Entrez le **spécifier un compte de Service** page. L’option gMSA doit maintenant être activée. Vous pouvez la sélectionner et entrez un nom de compte de service administré de groupe que vous souhaitez utiliser.  
  
6.  Sur le **spécifier la base de données de Configuration** page, spécifiez une base de données de configuration AD FS, puis cliquez sur **suivant**. Vous pouvez créer une base de données sur cet ordinateur à l’aide de la base de données interne Windows \(WID\), ou vous pouvez spécifier l’emplacement et le nom de l’instance de Microsoft SQL Server.  
  
    Pour plus d'informations, consultez [Rôle de la base de données de configuration AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
    > [!IMPORTANT]  
    > Si vous souhaitez créer une batterie de serveurs AD FS et utiliser SQL Server pour stocker vos données de configuration, vous pouvez utiliser SQL Server 2008 et les versions plus récentes, y compris SQL Server 2012 et SQL Server 2014.  
  
7.  Dans la page **Examiner les options**, vérifiez les choix que vous avez effectués pour la configuration, puis cliquez sur **Suivant**.  
  
8.  Sur le **Pre\-vérifications des conditions préalables** , vérifiez que toutes les vérifications terminées, puis cliquez sur **configurer**.  
  
9. Sur le **résultats** page, passez en revue les résultats et vérifiez si la configuration est terminée avec succès, puis cliquez sur **étapes requis pour terminer le déploiement de votre service de fédération**. Pour plus d’informations, consultez [étapes suivantes pour l’installation d’AD FS](https://go.microsoft.com/fwlink/p/?LinkId=286704). Cliquez sur **Fermer** pour quitter l'Assistant.  
  
### <a name="BKMK_3"></a>Pour configurer le premier serveur de fédération dans une nouvelle batterie de serveurs de fédération via Windows PowerShell  
Vous pouvez créer une nouvelle batterie de serveurs de fédération à l’aide d’un compte de service administré de groupe nouveau ou existant ou un compte d’utilisateur de domaine existant.  
  
-   **Si vous souhaitez créer un nouveau serveur de fédération à l’aide d’un nouveau compte de service administré de groupe, procédez comme suit :**  
  
    > [!IMPORTANT]  
    > Vous devez disposer des autorisations d’administrateur de domaine pour créer le premier serveur de fédération dans une nouvelle batterie de serveurs de fédération.  
  
    1.  Sur l’ordinateur que vous souhaitez configurer comme serveur de fédération, vérifiez que le certificat SSL requis a été importé dans le **ordinateur Local\\Store My** directory. Vous pouvez vérifier si le certificat SSL a été importé en exécutant la commande suivante dans la fenêtre de commande Windows PowerShell : `dir Cert:\LocalMachine\My`. Le certificat est répertorié par son empreinte numérique dans le **ordinateur Local\\Store My** directory.  
  
    2.  Sur votre contrôleur de domaine, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante pour vérifier si la clé racine KDS a été créée dans votre domaine : `Get-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`. Si elle n'a pas été créé afin que la sortie n’affiche aucune information, exécutez la commande suivante pour créer la clé : `Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`.  
  
    3.  Sur l’ordinateur que vous souhaitez configurer comme serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante :  
  
        ```  
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_Name>$  
        ```  
  
        > [!WARNING]  
        > Le `$` signe à la fin de la commande précédente est requis.  
  
        Pour obtenir la valeur de `<certificate_thumbprint>`, exécutez `dir Cert:\LocalMachine\My`, puis sélectionnez l’empreinte numérique de votre certificat SSL. La valeur de `<federation_service_name>` est le nom de votre service de fédération, par exemple, **fs.contoso.com**.  
  
        > [!NOTE]  
        > Si ce n’est pas la première fois que vous exécutez cette commande, ajoutez le `OverwriteConfiguration` paramètre.  
  
        > [!NOTE]  
        > La commande précédente crée une batterie WID. Si vous souhaitez créer une batterie de serveurs SQL Server, vous devez disposer d’une instance de SQL Server déjà installé et opérationnel.  
        >   
        > Vous pouvez utiliser la commande suivante pour créer le premier serveur de fédération dans une nouvelle batterie de serveurs qui utilise une instance de SQL Server : `Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name?\<SQL_instance_ name>;Integrated Security=True"` où **< SQL\_hôte\_nom >** est le nom du serveur sur lequel SQL Server est en cours d’exécution, et **< SQL\_instance\_nom >** est le nom de l’instance de SQL Server. Si vous utilisez l’instance par défaut de SQL Server, utilisez un **SQLConnectionString** valeur de «**Source de données\=< SQL\_hôte\_nom > ; Integrated Security\=True** ".  
  
        > [!IMPORTANT]  
        > Si vous voulez créer une batterie AD FS et utiliser SQL Server pour stocker vos données de configuration, vous pouvez utiliser SQL Server 2008 et versions plus récentes, notamment SQL Server 2012.  
  
-   **Si vous souhaitez créer un nouveau serveur de fédération à l’aide d’un compte d’utilisateur de domaine existant, procédez comme suit :**  
  
    1.  Sur l’ordinateur que vous souhaitez configurer comme serveur de fédération, vérifiez que le certificat SSL requis a été importé dans le **ordinateur Local\\Store My** directory. Vous pouvez vérifier si le certificat SSL a été importé en exécutant la commande suivante dans la fenêtre de commande Windows PowerShell : `dir Cert:\LocalMachine\My`. Le certificat est répertorié par son empreinte numérique dans le **ordinateur Local\\Store My** directory.  
  
    2.  Sur l’ordinateur que vous souhaitez configurer comme serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell, puis exécutez la commande suivante : `$fscred = Get-Credential`. Entrez les domaine compte informations d’identification utilisateur que vous souhaitez utiliser pour le compte de service de fédération dans le domaine de format\\nom d’utilisateur.  
  
    3.  Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante :  
  
        ```  
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscred  
        ```  
  
        Pour obtenir la valeur de **< certificat\_empreinte >**, exécutez `dir Cert:\LocalMachine\My`, puis sélectionnez l’empreinte numérique de votre certificat SSL. La valeur de **< fédération\_service\_nom >** est le nom de votre service de fédération, par exemple, fs.contoso.com.  
  
        > [!NOTE]  
        > Si ce n’est pas la première fois que vous exécutez cette commande, ajoutez le `OverwriteConfiguration` paramètre.  
  
        > [!NOTE]  
        > La commande précédente crée une batterie WID. Si vous souhaitez créer une batterie de serveurs SQL Server, vous devez disposer de l’instance de SQL Server déjà installé et opérationnel.  
        >   
        > Vous pouvez utiliser la commande suivante pour créer le premier serveur de fédération dans une nouvelle batterie de serveurs qui utilise une instance de SQL Server : `Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"` où **SQL\_hôte\_nom** est le nom du serveur sur lequel SQL Server est en cours d’exécution, et **SQL\_instance\_nom** est le nom de l’instance de SQL Server. Si vous utilisez l’instance par défaut de SQL Server, utilisez un **SQLConnectionString** valeur de «**Source de données\=< SQL\_hôte\_nom > ; Integrated Security\=True** ".  
  
        > [!IMPORTANT]  
        > Si vous souhaitez créer une batterie de serveurs AD FS et utiliser SQL Server pour stocker vos données de configuration, vous pouvez utiliser SQL Server 2008 et les versions plus récentes, y compris SQL Server 2012 et SQL Server 2014.  
  
## <a name="BKMK_2"></a>Ajouter un serveur de fédération à une batterie de serveurs de fédération existante  
  
> [!IMPORTANT]  
> Vérifiez que vous avez effectué [étape 3 : Installez le Service de rôle AD FS](../../ad-fs/deployment/Install-the-AD-FS-Role-Service.md), avant de commencer les procédures dans cette section.  
  
> [!IMPORTANT]  
> Vérifiez que vous avez obtenu de serveur SSL valide un certificat d’authentification avant d’effectuer cette procédure.  
  
### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-the-active-directory-federation-service-configuration-wizard"></a>Pour ajouter un serveur de fédération à une batterie de serveurs de fédération existante via l’Assistant Configuration des services de fédération Active Directory  
  
1.  Dans la page **Tableau de bord** du Gestionnaire de serveur, cliquez sur le drapeau **Notifications** , puis sur **Configurer le service FS (Federation Service) sur le serveur**.  
  
    L'**Assistant Configuration des services AD FS (Active Directory Federation Services)** s'ouvre.  
  
2.  Sur le **Bienvenue** page, sélectionnez **ajouter un serveur de fédération à une batterie de serveurs de fédération**, puis cliquez sur **suivant**.  
  
3.  Sur le **connexion à AD DS** , spécifiez un compte à l’aide d’autorisations d’administrateur de domaine pour le domaine AD auquel cet ordinateur est joint, puis cliquez sur **suivant**.  
  
4.  Sur le **spécifier la batterie de serveurs** page, indiquez le nom du serveur de fédération principal dans une batterie de serveurs qui utilise WID ou spécifiez le nom d’hôte de base de données et le nom d’instance de base de données d’une batterie de serveurs de fédération existante qui utilise SQL Server.  
  
    > [!WARNING]  
    > Dans Windows Server® 2012 R2, il existe une solution de contournement pour spécifier l’instance par défaut de SQL Server. La solution de contournement consiste à ne pas utiliser l’interface utilisateur. Au lieu de cela, utilisez les étapes de [pour configurer le premier serveur de fédération dans une nouvelle batterie de serveurs de fédération via Windows PowerShell](Configure-a-Federation-Server.md#BKMK_3).  
  
    > [!IMPORTANT]  
    > Si vous voulez créer une batterie AD FS et utiliser SQL Server pour stocker vos données de configuration, vous pouvez utiliser SQL Server 2008 et versions plus récentes, notamment SQL Server 2012.  
  
5.  Sur le **spécifier le certificat SSL** page, importez le fichier .pfx qui contient le certificat SSL et la clé que vous avez obtenu précédemment. Ce certificat est le certificat d'authentification de service requis. Dans [étape 2 : Inscrire un certificat SSL pour AD FS](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md), vous avez obtenu ce certificat et copiée sur l’ordinateur que vous souhaitez configurer comme serveur de fédération. Pour importer le fichier .pfx via l’Assistant, cliquez sur **importer** et accédez à l’emplacement du fichier. Entrez le mot de passe pour le fichier .pfx lorsque vous y êtes invité.  
  
6.  Sur le **spécifier un compte de Service** , spécifiez le même compte de service que vous avez configuré lorsque vous avez créé le premier serveur de fédération dans la batterie de serveurs. Vous pouvez utiliser un compte de Service administré de groupe existant ou un compte d’utilisateur de domaine existant.  
  
    > [!IMPORTANT]  
    > Le compte que vous spécifiez doit être le même compte que le compte qui a été utilisé sur le serveur de fédération principal dans cette batterie de serveurs.  
  
7.  Dans la page **Examiner les options**, vérifiez les choix que vous avez effectués pour la configuration, puis cliquez sur **Suivant**.  
  
8.  Sur le **Pre\-vérifications des conditions préalables** , vérifiez que toutes les vérifications terminées, puis cliquez sur **configurer**.  
  
9. Sur le **résultats** page, passez en revue les résultats et vérifiez si la configuration est terminée avec succès, puis cliquez sur **étapes requis pour terminer le déploiement de votre service de fédération**. Pour plus d’informations, consultez [étapes suivantes pour l’installation d’AD FS](https://go.microsoft.com/fwlink/p/?LinkId=286704). Cliquez sur **Fermer** pour quitter l'Assistant.  
  
### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-windows-powershell"></a>Pour ajouter un serveur de fédération à une batterie de serveurs de fédération existante via Windows PowerShell  
Vous pouvez ajouter un serveur de fédération à une batterie existante à l’aide d’un compte de service administré de groupe existant ou un compte d’utilisateur de domaine existant.  
  
-   Si vous souhaitez joindre un serveur de fédération à une batterie de serveurs à l’aide d’un compte de service administré de groupe existant, procédez comme suit :  
  
    1.  Sur l’ordinateur que vous souhaitez configurer comme serveur de fédération, vérifiez que le certificat SSL requis a été importé dans le **ordinateur Local\\Store My** directory. Vous pouvez vérifier si le certificat SSL a été importé en exécutant la commande suivante dans la fenêtre de commande Windows PowerShell : `dir Cert:\LocalMachine\My`. Le certificat est répertorié par son empreinte numérique dans le **ordinateur Local\\Store My** directory.  
  
    2.  Sur l’ordinateur que vous souhaitez configurer comme serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  
  
        ```  
        Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>  
        ```  
  
        `<domain>\<GMSA_name>` est votre domaine Active Directory et le nom de votre compte de service administré de groupe dans ce domaine. `<first_federation_server_hostname>` est le nom d’hôte du serveur de fédération principal dans cette batterie de serveurs existante.  
  
        Vous pouvez obtenir la valeur de `<certificate_thumbprint>` en exécutant `dir Cert:\LocalMachine\My` à l’étape précédente.  
  
        > [!NOTE]  
        > Si ce n’est pas la première fois que vous exécutez cette commande, ajoutez le `OverwriteConfiguration` paramètre.  
  
        > [!NOTE]  
        > La commande précédente crée un nœud de batterie WID. Si vous souhaitez créer un nœud de batterie de serveur des ordinateurs qui exécutent SQL Server, vous devez disposer de l’instance de SQL Server déjà installé et opérationnel.  
        >   
        > Vous pouvez utiliser la commande suivante pour ajouter un serveur de fédération à une batterie de serveurs existante qui utilise une instance de SQL Server : `Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"` où **SQL\_hôte\_nom** est le nom du serveur sur lequel SQL Server est en cours d’exécution, et **SQL\_instance\_nom** est le nom de l’instance de SQL Server. Si vous utilisez l’instance par défaut de SQL Server, utilisez un **SQLConnectionString** valeur de «**Source de données\=< SQL\_hôte\_nom > ; Integrated Security\=True** ".  
  
        > [!IMPORTANT]  
        > Si vous souhaitez créer une batterie de serveurs AD FS et utiliser SQL Server pour stocker vos données de configuration, vous pouvez utiliser SQL Server 2008 et les versions plus récentes, y compris SQL Server 2012 et SQL Server 2014.  
  
-   Si vous souhaitez joindre un serveur de fédération à une batterie de serveurs à l’aide d’un compte d’utilisateur de domaine existant, procédez comme suit :  
  
    1.  Sur l’ordinateur que vous souhaitez configurer comme serveur de fédération, ouvrez la fenêtre Windows PowerShellcommand, puis exécutez la commande suivante : `$fscred = get-credential`. Entrez les domaine compte informations d’identification utilisateur que vous souhaitez utiliser pour le compte de service de fédération dans le domaine de format\\nom d’utilisateur.  
  
    2.  Sur l’ordinateur que vous souhaitez configurer comme serveur de fédération, vérifiez que le certificat SSL requis a été importé dans le **ordinateur Local\\Store My** directory. Vous pouvez vérifier si le certificat SSL a été importé en exécutant la commande suivante dans la fenêtre Windows PowerShellcommand : `dir Cert:\LocalMachine\My`. Le certificat est répertorié par son empreinte numérique dans le **ordinateur Local\\Store My** directory.  
  
    3.  Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante.  
  
        ```  
        Add-AdfsFarmNode -ServiceAccountCredential $fscred -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>  
        ```  
  
        > [!NOTE]  
        > Si ce n’est pas la première fois que vous exécutez cette commande, ajoutez le `OverwriteConfiguration` paramètre.  
  
        > [!NOTE]  
        > La commande précédente crée un nœud de batterie WID. Si vous souhaitez créer un nœud de batterie de serveur des ordinateurs qui exécutent SQL Server, vous devez disposer de l’instance de SQL Server déjà installé et opérationnel. Vous pouvez utiliser la commande suivante pour ajouter un serveur de fédération à une batterie existante à l’aide d’une instance de SQL Server : `Add-AdfsFarmNode -ServiceAccountCredential $fscred -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"` où **SQL\_hôte\_nom** est le nom du serveur sur lequel l’instance de SQL Server est en cours d’exécution, et **SQL\_instance\_nom** est le nom de l’instance de SQL Server. Si vous utilisez l’instance par défaut de SQL Server, utilisez un **SQLConnectionString** valeur de «**Source de données\=< SQL\_hôte\_nom > ; Integrated Security\=True** ".  
  
        > [!IMPORTANT]  
        > Si vous souhaitez créer une batterie de serveurs AD FS et utiliser SQL Server pour stocker vos données de configuration, vous pouvez utiliser SQL Server 2008 et les versions plus récentes, y compris SQL Server 2012 et SQL Server 2014.  
  
## <a name="see-also"></a>Voir aussi 

[Déploiement d’AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guide de déploiement de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Déploiement d’une batterie de serveurs de fédération](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

