---
ms.assetid: 434fd617-373a-405e-bae4-da324ea83efc
title: "Guide de déploiement de Windows Server2012R2 ADFS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 13ce514dc5f3f70217a26c898cde6fe24d4967c6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="configure-a-federation-server"></a>Configurer un serveur de fédération

>S’applique à: Windows Server2016, Windows Server2012R2

Après avoir installé le service de rôle Services de fédération ActiveDirectory \(ADFS\) sur votre ordinateur, vous êtes prêt à configurer cet ordinateur pour qu’il devienne un serveur de fédération. Vous pouvez effectuer l’une des opérations suivantes:  
  
-   [Configurer le premier serveur de fédération dans une batterie de serveurs de fédération](assetId:///e340cf8f-acf3-4cba-8135-a9353b85e714#BKMK_1)  
  
-   [Ajouter un serveur de fédération à une batterie de serveurs de fédération existante](assetId:///e340cf8f-acf3-4cba-8135-a9353b85e714#BKMK_2)  
  
## <a name="BKMK_1"></a>Configurer le premier serveur de fédération dans une batterie de serveurs de fédération  
  
### <a name="to-configure-the-first-federation-server-in-a-new-federation-server-farm-by-using-the-active-directory-federation-service-configuration-wizard"></a>Pour configurer le premier serveur de fédération dans une batterie de serveurs de fédération à l’aide de l’Assistant Configuration du Service de fédération ActiveDirectory  
  
> [!NOTE]  
> Assurez-vous que vous avez autorisations d’administrateur de domaine ou d’administrateur de domaine disponible avant d’effectuer cette procédure.  
  
1.  Dans le Gestionnaire de serveur **tableau de bord**, cliquez sur le **Notifications** indicateur, puis cliquez sur **configurer le service de fédération sur le serveur**.  
  
    Le **Assistant Configuration de Service de fédération ActiveDirectory** s’ouvre.  
  
2.  Sur le **Bienvenue** page, sélectionnez **créer le premier serveur de fédération dans une batterie de serveurs de fédération**, puis cliquez sur **suivant**.  
  
3.  Sur le **se connecter aux services ADDS**, spécifiez un compte à l’aide des autorisations d’administrateur de domaine pour le domaine ActiveDirectory \(AD\) auquel cet ordinateur est joint, puis cliquez sur **suivant**.  
  
4.  Sur le **spécifier les propriétés de Service** page, procédez comme suit, puis cliquez sur **suivant**:  
  
    -   Importer le fichier .pfx qui contient le certificat Secure Socket Layer \(SSL\) et la clé que vous avez obtenu précédemment. Dans [étape2: inscrire un certificat SSL pour ADFS](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md), vous avez obtenu ce certificat et copié sur l’ordinateur que vous souhaitez configurer en tant qu’un serveur de fédération. Pour importer le fichier .pfx via l’Assistant, cliquez sur **importer**, puis accédez à l’emplacement du fichier. Entrez le mot de passe pour le fichier .pfx lorsque vous y êtes invité.  
  
    -   Fournir un nom pour votre service de fédération. Par exemple, **fs.contoso.com**. Ce nom doit correspondre à un des noms dans le certificat ou objet.  
  
    -   Fournir un nom d’affichage pour votre service de fédération. Par exemple, **Contoso Corporation**. Les utilisateurs voient ce nom sur les Services de fédération ActiveDirectory \(ADFS\) sign\-dans la page.  
  
5.  Sur le **spécifier un compte de Service**, spécifiez un compte de service. Vous pouvez créer ou utiliser un existant groupe compte de Service administré \(gMSA\) ou utiliser un compte d’utilisateur de domaine existant. Si vous sélectionnez l’option pour créer un nouveau compte de service administré de groupe, spécifiez un nom pour le nouveau compte. Si vous sélectionnez l’option pour utiliser un compte gMSA existant ou un compte de domaine, cliquez sur **sélectionnez** pour sélectionner un compte.  
  
    > [!NOTE]  
    > L’avantage de l’utilisation d’un compte de service administré de groupe est sa fonctionnalité de mise à jour automatique-négocié un mot de passe.  
  
    > [!WARNING]  
    > Si vous souhaitez utiliser un compte de service administré de groupe, vous devez disposer d’au moins un contrôleur de domaine dans votre environnement qui exécute le système d’exploitation Windows Server2012.  
    >   
    > Si l’option gMSA est désactivée, vous voyez un message d’erreur, tels que **la comptes de Service administrés de groupe ne sont pas disponible, car la clé racine KDS n’a pas été définie**, vous pouvez activer le compte gMSA dans votre domaine en exécutant la commande Windows PowerShell suivante sur un contrôleur de domaine qui exécute Windows Server2012 ou version ultérieure, dans votre domaine ActiveDirectory:`Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`. Puis revenez à l’Assistant, cliquez sur **précédent**, puis cliquez sur **suivant** à re\-Entrez le **spécifier un compte de Service** page. L’option gMSA doit maintenant être activée. Vous pouvez sélectionner et entrez un nom de compte de service administré de groupe que vous souhaitez utiliser.  
  
6.  Sur le **spécifier la base de données de Configuration** page, spécifiez une base de données de configuration ADFS, puis cliquez sur **suivant**. Vous pouvez créer une base de données sur cet ordinateur à l’aide de la base de données interne Windows \(WID\), ou vous pouvez spécifier l’emplacement et le nom de l’instance de MicrosoftSQLServer.  
  
    Pour plus d’informations, voir [le rôle de la base de données de Configuration ADFS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
    > [!IMPORTANT]  
    > Si vous souhaitez créer une batterie ADFS et utiliser SQLServer pour stocker vos données de configuration, vous pouvez utiliser SQLServer2008 et versions plus récentes, notamment SQLServer2012 et SQLServer2014.  
  
7.  Sur le **examiner les Options** page, vérifiez vos sélections de configuration, puis cliquez sur **suivant**.  
  
8.  Sur le **vérifications requises-Pre\**, vérifiez que toutes les vérifications sont terminées avec succès, puis cliquez sur **configurer**.  
  
9. Sur le **résultats** page, passez en revue les résultats et vérifiez si la configuration est terminée avec succès, puis cliquez sur **étapes ultérieures requises pour terminer le déploiement de votre service de fédération**. Pour plus d’informations, voir [étapes suivantes pour terminer votre installation ADFS](https://go.microsoft.com/fwlink/p/?LinkId=286704). Cliquez sur **fermer** pour quitter l’Assistant.  
  
### <a name="BKMK_3"></a>Pour configurer le premier serveur de fédération dans une nouvelle batterie de serveurs de fédération via Windows PowerShell  
Vous pouvez créer une batterie de serveurs de fédération à l’aide d’un compte de service administré de groupe nouveau ou existant ou un compte d’utilisateur de domaine existant.  
  
-   **Si vous souhaitez créer un nouveau serveur de fédération à l’aide d’un nouveau compte de service administré de groupe, procédez comme suit:**  
  
    > [!IMPORTANT]  
    > Vous devez disposer des autorisations d’administrateur de domaine pour créer le premier serveur de fédération dans une batterie de serveurs de fédération.  
  
    1.  Sur l’ordinateur que vous souhaitez configurer en tant qu’un serveur de fédération, assurez-vous que le certificat SSL requis a été importé dans le **Local de Store** active. Vous pouvez vérifier si le certificat SSL a été importé en exécutant la commande suivante dans la fenêtre de commande Windows PowerShell:`dir Cert:\LocalMachine\My`. Le certificat est répertorié par son empreinte numérique dans le **Local de Store** active.  
  
    2.  Sur votre contrôleur de domaine, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante pour vérifier si la clé racine KDS a été créée dans votre domaine:`Get-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`. S’il n'a pas été créé afin que la sortie n’affiche aucune information, exécutez la commande suivante pour créer la clé:`Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`.  
  
    3.  Sur l’ordinateur que vous souhaitez configurer en tant qu’un serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante:  
  
        ```  
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_Name>$  
        ```  
  
        > [!WARNING]  
        > Le `$`signe à la fin de la commande précédente est requis.  
  
        Pour obtenir la valeur de `<certificate_thumbprint>`, exécutez `dir Cert:\LocalMachine\My`, puis sélectionnez l’empreinte numérique de votre certificat SSL. La valeur de `<federation_service_name>`est le nom de votre service de fédération, par exemple, **fs.contoso.com**.  
  
        > [!NOTE]  
        > Si ce n’est pas la première fois que vous exécutez cette commande, ajoutez le `OverwriteConfiguration`paramètre.  
  
        > [!NOTE]  
        > La commande précédente crée une batterie WID. Si vous souhaitez créer une batterie de serveurs SQLServer, vous devez disposer d’une instance de SQLServer déjà installé et opérationnel.  
        >   
        > Vous pouvez utiliser la commande suivante pour créer le premier serveur de fédération dans une nouvelle batterie de serveurs qui utilise une instance de SQLServer: `Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name?\<SQL_instance_ name>;Integrated Security=True"`où **< sQL\_Host\_Name >** est le nom du serveur sur lequel SQLServer est en cours d’exécution, et **< sQL\_instance\_name >** est le nom de l’instance de SQLServer. Si vous utilisez l’instance par défaut de SQLServer, utilisez un **SQLConnectionString** valeur de «**Source\ données = < sQL\_Host\_Name >; sécurité\ intégrée = True**».  
  
        > [!IMPORTANT]  
        > Si vous souhaitez créer une batterie ADFS et utiliser SQLServer pour stocker vos données de configuration, vous pouvez utiliser SQLServer2008 et versions plus récentes, notamment SQLServer2012.  
  
-   **Si vous souhaitez créer un nouveau serveur de fédération à l’aide d’un compte d’utilisateur de domaine existant, procédez comme suit:**  
  
    1.  Sur l’ordinateur que vous souhaitez configurer en tant qu’un serveur de fédération, assurez-vous que le certificat SSL requis a été importé dans le **Local de Store** active. Vous pouvez vérifier si le certificat SSL a été importé en exécutant la commande suivante dans la fenêtre de commande Windows PowerShell:`dir Cert:\LocalMachine\My`. Le certificat est répertorié par son empreinte numérique dans le **Local de Store** active.  
  
    2.  Sur l’ordinateur que vous souhaitez configurer en tant qu’un serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell, puis exécutez la commande suivante:`$fscred = Get-Credential`. Entrez les domaine utilisateur informations d’identification que vous souhaitez utiliser pour le compte de service de fédération dans le format du nom option Domaine\\Nom d’utilisateur.  
  
    3.  Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante:  
  
        ```  
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscred  
        ```  
  
        Pour obtenir la valeur de **< certificate\_thumbprint >**, exécutez `dir Cert:\LocalMachine\My`, puis sélectionnez l’empreinte numérique de votre certificat SSL. La valeur de **< federation\_service\_name >** est le nom de votre service de fédération, par exemple, fs.contoso.com.  
  
        > [!NOTE]  
        > Si ce n’est pas la première fois que vous exécutez cette commande, ajoutez le `OverwriteConfiguration`paramètre.  
  
        > [!NOTE]  
        > La commande précédente crée une batterie WID. Si vous souhaitez créer une batterie de serveurs SQLServer, vous devez disposer de l’instance de SQLServer déjà installé et opérationnel.  
        >   
        > Vous pouvez utiliser la commande suivante pour créer le premier serveur de fédération dans une nouvelle batterie de serveurs qui utilise une instance de SQLServer: `Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"`où **SQL\_Host\_Name** est le nom du serveur sur lequel SQLServer est en cours d’exécution, et **SQL\_instance\_name** est le nom de l’instance de SQLServer. Si vous utilisez l’instance par défaut de SQLServer, utilisez un **SQLConnectionString** valeur de «**Source\ données = < sQL\_Host\_Name >; sécurité\ intégrée = True**».  
  
        > [!IMPORTANT]  
        > Si vous souhaitez créer une batterie ADFS et utiliser SQLServer pour stocker vos données de configuration, vous pouvez utiliser SQLServer2008 et versions plus récentes, notamment SQLServer2012 et SQLServer2014.  
  
## <a name="BKMK_2"></a>Ajouter un serveur de fédération à une batterie de serveurs de fédération existante  
  
> [!IMPORTANT]  
> Vérifiez que vous avez effectué [étape3: installer le Service de rôle ADFS](../../ad-fs/deployment/Install-the-AD-FS-Role-Service.md), avant de commencer les procédures de cette section.  
  
> [!IMPORTANT]  
> Assurez-vous que vous avez obtenu un serveur SSL valide certificat d’authentification avant d’effectuer cette procédure.  
  
### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-the-active-directory-federation-service-configuration-wizard"></a>Pour ajouter un serveur de fédération à une batterie de serveurs de fédération existante via l’Assistant Configuration du Service de fédération ActiveDirectory  
  
1.  Dans le Gestionnaire de serveur **tableau de bord**, cliquez sur le **Notifications** indicateur, puis cliquez sur **configurer le service de fédération sur le serveur**.  
  
    Le **Assistant Configuration de Service de fédération ActiveDirectory** s’ouvre.  
  
2.  Sur le **Bienvenue** page, sélectionnez **ajouter un serveur de fédération à une batterie de serveurs de fédération**, puis cliquez sur **suivant**.  
  
3.  Sur le **se connecter aux services ADDS**, spécifiez un compte à l’aide des autorisations d’administrateur de domaine pour le domaine ActiveDirectory auquel cet ordinateur est joint, puis cliquez sur **suivant**.  
  
4.  Sur le **spécifier la batterie de serveurs** page, indiquez le nom du serveur de fédération principal dans une batterie de serveurs qui utilise WID ou spécifiez le nom d’hôte de base de données et le nom de l’instance de base de données d’une batterie de serveurs de fédération existante qui utilise SQLServer.  
  
    > [!WARNING]  
    > Dans Windows Server® 2012R2, il existe une solution de contournement pour spécifier l’instance par défaut de SQLServer. La solution de contournement est de ne pas utiliser l’interface utilisateur. Au lieu de cela, utilisez les étapes de [pour configurer le premier serveur de fédération dans une nouvelle batterie de serveurs de fédération via Windows PowerShell](Configure-a-Federation-Server.md#BKMK_3).  
  
    > [!IMPORTANT]  
    > Si vous souhaitez créer une batterie ADFS et utiliser SQLServer pour stocker vos données de configuration, vous pouvez utiliser SQLServer2008 et versions plus récentes, notamment SQLServer2012.  
  
5.  Sur le **spécifier le certificat SSL** page, importez le fichier .pfx qui contient le certificat SSL et la clé que vous avez obtenu précédemment. Ce certificat est le certificat d’authentification de service requis. Dans [étape2: inscrire un certificat SSL pour ADFS](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md), vous avez obtenu ce certificat et copiée sur l’ordinateur que vous souhaitez configurer en tant qu’un serveur de fédération. Pour importer le fichier .pfx via l’Assistant, cliquez sur **importer** et accédez à l’emplacement du fichier. Entrez le mot de passe pour le fichier .pfx lorsque vous y êtes invité.  
  
6.  Sur le **spécifier un compte de Service**, spécifiez le même compte de service que vous avez configuré lorsque vous avez créé le premier serveur de fédération dans la batterie de serveurs. Vous pouvez utiliser un compte de Service administré de groupe existant ou un compte d’utilisateur de domaine existant.  
  
    > [!IMPORTANT]  
    > Le compte que vous spécifiez doit être le même compte que le compte qui a été utilisé sur le serveur de fédération principal de cette batterie de serveurs.  
  
7.  Sur le **examiner les Options** page, vérifiez vos sélections de configuration, puis cliquez sur **suivant**.  
  
8.  Sur le **vérifications requises-Pre\**, vérifiez que toutes les vérifications sont terminées avec succès, puis cliquez sur **configurer**.  
  
9. Sur le **résultats** page, passez en revue les résultats et vérifiez si la configuration est terminée avec succès, puis cliquez sur **étapes ultérieures requises pour terminer le déploiement de votre service de fédération**. Pour plus d’informations, voir [étapes suivantes pour terminer votre installation ADFS](https://go.microsoft.com/fwlink/p/?LinkId=286704). Cliquez sur **fermer** pour quitter l’Assistant.  
  
### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-windows-powershell"></a>Pour ajouter un serveur de fédération à une batterie de serveurs de fédération existante via Windows PowerShell  
Vous pouvez ajouter un serveur de fédération à une batterie existante à l’aide d’un compte de service administré de groupe existant ou un compte d’utilisateur de domaine existant.  
  
-   Si vous voulez joindre un serveur de fédération à une batterie de serveurs à l’aide d’un compte de service administré de groupe existant, procédez comme suit:  
  
    1.  Sur l’ordinateur que vous souhaitez configurer en tant qu’un serveur de fédération, assurez-vous que le certificat SSL requis a été importé dans le **Local de Store** active. Vous pouvez vérifier si le certificat SSL a été importé en exécutant la commande suivante dans la fenêtre de commande Windows PowerShell:`dir Cert:\LocalMachine\My`. Le certificat est répertorié par son empreinte numérique dans le **Local de Store** active.  
  
    2.  Sur l’ordinateur que vous souhaitez configurer en tant qu’un serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell et exécutez la commande suivante.  
  
        ```  
        Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>  
        ```  
  
        `<domain>\<GMSA_name>` Est le nom de votre compte de service administré de groupe dans ce domaine et de votre domaine ActiveDirectory. `<first_federation_server_hostname>` Est le nom d’hôte du serveur de fédération principal de cette batterie de serveurs existant.  
  
        Vous pouvez obtenir la valeur de `<certificate_thumbprint>`en exécutant `dir Cert:\LocalMachine\My`à l’étape précédente.  
  
        > [!NOTE]  
        > Si ce n’est pas la première fois que vous exécutez cette commande, ajoutez le `OverwriteConfiguration`paramètre.  
  
        > [!NOTE]  
        > La commande précédente crée un nœud de la batterie WID. Si vous souhaitez créer un nœud de batterie de serveurs des ordinateurs exécutant SQLServer, vous devez disposer de l’instance de SQLServer déjà installé et opérationnel.  
        >   
        > Vous pouvez utiliser la commande suivante pour ajouter un serveur de fédération à une batterie existante qui utilise une instance de SQLServer: `Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"`où **SQL\_Host\_Name** est le nom du serveur sur lequel SQLServer est en cours d’exécution, et **SQL\_instance\_name** est le nom de l’instance de SQLServer. Si vous utilisez l’instance par défaut de SQLServer, utilisez un **SQLConnectionString** valeur de «**Source\ données = < sQL\_Host\_Name >; sécurité\ intégrée = True**».  
  
        > [!IMPORTANT]  
        > Si vous souhaitez créer une batterie ADFS et utiliser SQLServer pour stocker vos données de configuration, vous pouvez utiliser SQLServer2008 et versions plus récentes, notamment SQLServer2012 et SQLServer2014.  
  
-   Si vous voulez joindre un serveur de fédération à une batterie de serveurs à l’aide d’un compte d’utilisateur de domaine existant, procédez comme suit:  
  
    1.  Sur l’ordinateur que vous souhaitez configurer en tant qu’un serveur de fédération, ouvrez la fenêtre PowerShellcommand Windows, puis exécutez la commande suivante:`$fscred = get-credential`. Entrez les domaine utilisateur informations d’identification que vous souhaitez utiliser pour le compte de service de fédération dans le format du nom option Domaine\\Nom d’utilisateur.  
  
    2.  Sur l’ordinateur que vous souhaitez configurer en tant qu’un serveur de fédération, assurez-vous que le certificat SSL requis a été importé dans le **Local de Store** active. Vous pouvez vérifier si le certificat SSL a été importé en exécutant la commande suivante dans la fenêtre Windows PowerShellcommand:`dir Cert:\LocalMachine\My`. Le certificat est répertorié par son empreinte numérique dans le **Local de Store** active.  
  
    3.  Dans la même fenêtre de commande Windows PowerShell, exécutez la commande suivante.  
  
        ```  
        Add-AdfsFarmNode -ServiceAccountCredential $fscred -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>  
        ```  
  
        > [!NOTE]  
        > Si ce n’est pas la première fois que vous exécutez cette commande, ajoutez le `OverwriteConfiguration`paramètre.  
  
        > [!NOTE]  
        > La commande précédente crée un nœud de la batterie WID. Si vous souhaitez créer un nœud de batterie de serveurs des ordinateurs exécutant SQLServer, vous devez disposer de l’instance de SQLServer déjà installé et opérationnel. Vous pouvez utiliser la commande suivante pour ajouter un serveur de fédération à une batterie existante à l’aide d’une instance de SQLServer: `Add-AdfsFarmNode -ServiceAccountCredential $fscred -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"`où **SQL\_Host\_Name** est le nom du serveur sur lequel l’instance de SQLServer est en cours d’exécution, et **SQL\_instance\_name** est le nom de l’instance de SQLServer. Si vous utilisez l’instance par défaut de SQLServer, utilisez un **SQLConnectionString** valeur de «**Source\ données = < sQL\_Host\_Name >; sécurité\ intégrée = True**».  
  
        > [!IMPORTANT]  
        > Si vous souhaitez créer une batterie ADFS et utiliser SQLServer pour stocker vos données de configuration, vous pouvez utiliser SQLServer2008 et versions plus récentes, notamment SQLServer2012 et SQLServer2014.  
  
## <a name="see-also"></a>Voir aussi 

[Déploiement d’ADFS](../../ad-fs/AD-FS-Deployment.md)  

[Guide de déploiement de Windows Server2012R2 ADFS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Déploiement d’une batterie de serveurs de fédération](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

