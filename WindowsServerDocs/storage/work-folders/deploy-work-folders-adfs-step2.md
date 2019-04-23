---
title: 'Déployer Dossiers de travail avec AD FS et le proxy d’application Web : Étape 2, Tâches post-configuration AD FS'
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 4/5/2017
ms.assetid: 0a48852e-48cc-4047-ae58-99f11c273942
ms.openlocfilehash: 87fdcf06c601d3362488eaf6a83e4f88ad191305
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828230"
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-2-ad-fs-post-configuration-work"></a>Déployer des dossiers de travail avec AD FS et Proxy d’Application Web : Étape 2, le travail de post-configuration AD FS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit la deuxième étape de déploiement de Dossiers de travail avec les services de fédération Active Directory (AD FS) et le proxy d’application Web. Vous pouvez trouver les autres étapes de ce processus dans ces rubriques :  
  
-   [Déployer des dossiers de travail avec AD FS et Proxy d’Application Web : Vue d’ensemble](deploy-work-folders-adfs-overview.md)  
  
-   [Déployer des dossiers de travail avec AD FS et Proxy d’Application Web : Étape 1, configurer AD FS](deploy-work-folders-adfs-step1.md)  
  
-   [Déployer des dossiers de travail avec AD FS et Proxy d’Application Web : Étape 3, configurer des dossiers de travail](deploy-work-folders-adfs-step3.md)  
  
-   [Déployer des dossiers de travail avec AD FS et Proxy d’Application Web : Étape 4 : configurer le Proxy d’Application Web](deploy-work-folders-adfs-step4.md)  
  
-   [Déployer des dossiers de travail avec AD FS et Proxy d’Application Web : L’étape 5, configurez les Clients](deploy-work-folders-adfs-step5.md)  
  
> [!NOTE]
>   Les instructions présentées dans cette section ont été conçues pour un environnement Windows Server 2016. Si vous utilisez Windows Server 2012 R2, suivez les [instructions pour Windows Server 2012 R2](https://technet.microsoft.com/library/dn747208(v=ws.11).aspx).

À l’étape 1, vous avez installé et configuré les services AD FS. Vous devez maintenant effectuer les étapes de post-configuration suivantes pour AD FS.  
  
## <a name="configure-dns-entries"></a>Configurer les entrées DNS  
Vous devez créer deux entrées DNS pour AD FS. Il s’agit des mêmes entrées que celles qui étaient utilisées dans les étapes de pré-installation lorsque vous avez créé le certificat SAN.  
  
Les entrées DNS se présentent sous la forme suivante :  
  
-   Service AD FS nom.domaine  
  
-   enterpriseregistration.domain  
  
-   nom du serveur AD FS.domaine   (l’entrée DNS doit déjà exister. par exemple, 2016-ADFS.contoso.com)
  
Dans l’exemple de test, les valeurs sont :  
  
-   **blueadfs.contoso.com**  
  
-   **enterpriseregistration.contoso.com**  
  
## <a name="create-the-a-and-cname-records-for-ad-fs"></a>Créer les enregistrements A et CNAME pour AD FS  
Pour créer des enregistrements A et CNAME pour AD FS, procédez comme suit :  
  
1.  Sur votre contrôleur de domaine, ouvrez le Gestionnaire DNS.  
  
2.  Développez le dossier Zones de recherche directe, cliquez avec le bouton droit sur votre domaine, puis sélectionnez **Nouvel hôte (A)**.  
  
3.  La fenêtre **Nouvel hôte** s’ouvre. Dans le champ du **Nom**, entrez l’alias du nom du service AD FS. Dans l’exemple de test, ce nom est **blueadfs**.  
  
    L’alias doit être identique à l’objet dans le certificat qui a été utilisé pour AD FS. Par exemple, si l’objet était adfs.contoso.com, l’alias entré ici serait **adfs**.  
  
    > [!IMPORTANT]  
    > Lorsque vous configurez des services AD FS à l’aide de l’interface utilisateur de Windows Server au lieu de Windows PowerShell, vous devez créer un enregistrement A au lieu d’un enregistrement CNAME pour AD FS. Cela s’explique par le fait que le nom principal de service (SPN) qui est créé via l’interface utilisateur contient uniquement l’alias qui est utilisé pour configurer le service AD FS en tant qu’hôte.  
    >   
4.  Dans **Adresse IP**, entrez l’adresse IP du serveur AD FS. Dans l’exemple de test, cette adresse est **192.168.0.160**. Cliquez sur **Ajouter un hôte**.  
  
5.  Dans le dossier Zones de recherche directe, cliquez à nouveau avec le bouton droit sur votre domaine, puis sélectionnez **Nouvel Alias (CNAME)**.  
  
6.  Dans la fenêtre **Nouvel enregistrement de ressource**, ajoutez le nom d’alias **enterpriseregistration** et entrez le nom de domaine complet (FQDN) du serveur AD FS. Cet alias est utilisé pour la jonction d’appareils et doit être appelé **enterpriseregistration**.
  
7.  Cliquez sur **OK**.  
  
Pour effectuer les mêmes étapes via Windows PowerShell, utilisez la commande suivante. La commande doit être exécutée sur le contrôleur de domaine.  
  
```powershell  
Add-DnsServerResourceRecord  -ZoneName "contoso.com" -Name blueadfs -A -IPv4Address 192.168.0.160   
Add-DnsServerResourceRecord  -ZoneName "contoso.com" -Name enterpriseregistration -CName  -HostNameAlias 2016-ADFS.contoso.com   
```  
  
## <a name="set-up-the-ad-fs-relying-party-trust-for-work-folders"></a>Configurer l’approbation de partie de confiance AD FS pour Dossiers de travail  
Vous pouvez installer et configurer l’approbation de partie de confiance pour Dossiers de travail, même si Dossiers de travail n’a pas encore été configuré. L’approbation de partie de confiance doit être configurée pour permettre à Dossiers de travail d’utiliser les services AD FS. Dans la mesure où vous êtes en train de configurer AD FS, il s’agit du moment idéal pour effectuer cette étape.  
  
Pour configurer l’approbation de partie de confiance :  
  
1.  Ouvrez le **Gestionnaire de serveur**, puis dans le menu **Outils**, sélectionnez **Gestion AD FS**.  
  
2.  Dans le volet droit, sous **Actions**, cliquez sur **Ajouter une approbation de partie de confiance**.  
  
3.  Dans la page **Bienvenue**, sélectionnez **Prise en charge des revendications** et cliquez sur **Démarrer**.  
  
4.  Dans la page **Sélectionner une source de données**, sélectionnez **Entrer manuellement les données concernant la partie de confiance**, puis cliquez sur **Suivant**.  
  
5.  Dans le champ **Nom complet**, tapez **WorkFolders**, puis cliquez sur **Suivant**.  
  
6.  Dans la page **Configurer le certificat**, cliquez sur **Suivant**. Les certificats de chiffrement de jetons sont facultatifs et ne sont pas nécessaires pour la configuration de test.  
  
7.  Dans la page **Configurer l’URL**, cliquez sur **Suivant**.  
  
8. Sur le **configurer les identificateurs** page, ajoutez l’identificateur suivant : **https://windows-server-work-folders/V1**. Cet identificateur est une valeur codée en dur utilisée par Dossiers de travail, et il est envoyé par le service Dossiers de travail lorsqu’il communique avec AD FS. Cliquez sur **Suivant**.  
  
9. Dans la page Sélectionner une stratégie de contrôle d’accès, sélectionnez **Autoriser tout le monde**, puis cliquez sur **Suivant**.  
  
10. Dans la page **Prêt à ajouter l'approbation** , cliquez sur **Suivant**.  
  
11. Une fois la configuration terminée, la dernière page de l’assistant indique que la configuration a réussi. Cochez la case permettant de modifier les règles de revendication, puis cliquez sur **Fermer**.  
  
12. Dans le composant logiciel enfichable AD FS, sélectionnez l’approbation de partie de confiance **WorkFolders**, puis cliquez sur **Éditer la stratégie d’émission de revendication** dans Actions.

13. La fenêtre **Éditer la stratégie d’émission de revendication pour WorkFolders** s’ouvre. Cliquez sur **Ajouter une règle**.  
  
14. Dans la liste déroulante **Modèle de règle de revendication**, sélectionnez **Envoyer les attributs LDAP en tant que revendications**, puis cliquez sur **Suivant**.  
  
15. Dans la page **Configurer la règle de revendication**, dans le champ **Nom de la règle de revendication**, tapez **WorkFolders**.  
  
16. Dans la liste déroulante **Magasin d’attributs**, sélectionnez **Active Directory**.  
  
17. Dans la table de mappage, entrez ces valeurs :  
  
    -   Principal-nom d’utilisateur : UPN  
  
    -   Nom complet : Nom  
  
    -   Nom de famille : Nom  
  
    -   Given Name : Prénom  
  
18. Cliquez sur **Terminer**. La règle WorkFolders sera répertoriée sous l’onglet Règles de transformation d’émission. Cliquez ensuite sur **OK**.  
  
### <a name="set-relying-part-trust-options"></a>Définir les options d’approbation de partie de confiance  
Après que l’approbation de partie de confiance a été configurée pour AD FS, vous devez terminer la configuration en exécutant cinq commandes dans Windows PowerShell. Ces commandes définissent les options nécessaires pour que Dossiers de travail puisse communiquer correctement avec AD FS, et elles ne peuvent pas être définies via l’interface utilisateur. Ces options sont les suivantes :  
  
-   Activer l’utilisation des clés d’authentification Web JSON (JWT)  
  
-   Désactiver les revendications chiffrées  
  
-   Activer la mise à jour automatique  
  
-   Définir l’émission de jetons d’actualisation Oauth sur tous les appareils.  

-   Accorder l’accès à l’approbation de partie de confiance aux clients

Pour définir ces options, utilisez les commandes suivantes :  
  
```powershell  
Set-ADFSRelyingPartyTrust -TargetIdentifier "https://windows-server-work-folders/V1" -EnableJWT $true   
Set-ADFSRelyingPartyTrust -TargetIdentifier "https://windows-server-work-folders/V1" -Encryptclaims $false   
Set-ADFSRelyingPartyTrust -TargetIdentifier "https://windows-server-work-folders/V1" -AutoupdateEnabled $true   
Set-ADFSRelyingPartyTrust -TargetIdentifier "https://windows-server-work-folders/V1" -IssueOAuthRefreshTokensTo AllDevices
Grant-AdfsApplicationPermission -ServerRoleIdentifier "https://windows-server-work-folders/V1" -AllowAllRegisteredClients -ScopeNames openid,profile  
```  
  
## <a name="enable-workplace-join"></a>Activer Workplace Join  
L’activation de Workplace Join est facultative, mais peut être utile lorsque vous souhaitez que les utilisateurs soient en mesure d’utiliser leurs appareils personnels pour accéder aux ressources de l’espace de travail.  
  
Pour activer l’inscription de l’appareil à Workplace Join, vous devez exécuter les commandes Windows PowerShell suivantes, qui permettront de configurer l’inscription de l’appareil et de définir la stratégie d’authentification globale :  
  
```powershell  
Initialize-ADDeviceRegistration -ServiceAccountName <your AD FS service account>
    Example: Initialize-ADDeviceRegistration -ServiceAccountName contoso\adfsservice$
Set-ADFSGlobalAuthenticationPolicy -DeviceAuthenticationEnabled $true   
```  
  
## <a name="export-the-ad-fs-certificate"></a>Exporter le certificat AD FS  
Ensuite, exportez le certificat AD FS auto\-signé pour qu’il puisse être installé sur les ordinateurs suivants dans l’environnement de test :  
  
-   Le serveur qui est utilisé pour Dossiers de travail  
  
-   Le serveur qui est utilisé pour le proxy d’application Web  
  
-   Le client Windows joint au domaine  
  
-   Le client Windows qui n’est pas joint au domaine  
  
Pour exporter le certificat, procédez comme suit :  
  
1.  Cliquez sur **Démarrer**, puis sur **Exécuter**.  
  
2.  Tapez **MMC**.  
  
3.  Dans le menu **Fichier** , cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**.  
  
4.  Dans la zone **Composants logiciels enfichables disponibles**, cliquez sur **Certificats**, puis sur **Ajouter**. L’assistant Composant logiciel enfichable Certificats démarre.  
  
5.  Sélectionnez **Un compte d’ordinateur**, puis cliquez sur **Suivant**.  
  
6.  Sélectionnez **Ordinateur local (l’ordinateur sur lequel cette console s’exécute)**, puis cliquez sur **Terminer**.  
  
7.  Cliquez sur **OK**.  
  
8.  Développez le dossier **Racine de la console\Certificats\(Ordinateur local)\Personnel\Certificats**.  
  
9.  Cliquez avec le bouton droit sur le **certificat AD FS**, cliquez sur **Toutes les tâches**, puis sélectionnez **Exporter...**.  
  
10. L'Assistant Exportation de certificat s'ouvre. Sélectionnez **Oui, exporter la clé privée**.  
  
11. Dans la page **Format de fichier d’exportation**, conservez les sélections par défaut, puis cliquez sur **Suivant**.  
  
12. Créez un mot de passe pour le certificat. Il s’agit du mot de passe que vous utiliserez ultérieurement lorsque vous importerez le certificat vers d’autres appareils. Cliquez sur **Suivant**.  
  
13. Indiquez un emplacement et un nom pour le certificat, puis cliquez sur **Terminer**.  
  
L’installation du certificat est traitée plus loin dans la procédure de déploiement.  
  
## <a name="manage-the-private-key-setting"></a>Gérer le paramètre de clé privé  
Vous devez accorder au compte de service AD FS l’autorisation d’accéder à la clé privée du nouveau certificat. Vous devrez de nouveau accorder cette autorisation lorsque vous remplacerez le certificat de communication après son expiration. Pour accorder l’autorisation, procédez comme suit :  
  
1.  Cliquez sur **Démarrer**, puis sur **Exécuter**.  
  
2.  Tapez **MMC**.  
  
3.  Dans le menu **Fichier** , cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**.  
  
4.  Dans la zone **Composants logiciels enfichables disponibles**, cliquez sur **Certificats**, puis sur **Ajouter**. L’assistant Composant logiciel enfichable Certificats démarre.  
  
5.  Sélectionnez **Un compte d’ordinateur**, puis cliquez sur **Suivant**.  
  
6.  Sélectionnez **Ordinateur local (l’ordinateur sur lequel cette console s’exécute)**, puis cliquez sur **Terminer**.  
  
7.  Cliquez sur **OK**.  
  
8.  Développez le dossier **Racine de la console\Certificats\(Ordinateur local)\Personnel\Certificats**.  
  
9.  Cliquez avec le bouton droit sur le **certificat AD FS**, cliquez sur **Toutes les tâches**, puis cliquez sur **Gérer les clés privées**.  
  
10. Dans la fenêtre **Autorisations**, cliquez sur **Ajouter**.  
  
11. Dans la fenêtre **Types d’objets**, sélectionnez **Comptes de service** , puis cliquez sur **OK**.  
  
12. Tapez le nom du compte qui exécute les services AD FS. Dans l’exemple de test, ce nom est ADFSService. Cliquez sur **OK**.  
  
13. Dans la fenêtre **Autorisations**, accordez au moins les autorisations de lecture au compte, puis cliquez sur **OK**.  
  
Si vous n’avez pas l’option pour gérer les clés privées, vous devrez peut-être exécuter la commande suivante : `certutil -repairstore my *`  
  
## <a name="verify-that-ad-fs-is-operational"></a>Vérifiez que les services AD FS sont opérationnels  
Pour vérifier que les services AD FS est opérationnel, ouvrez une fenêtre de navigateur et accédez à https://blueadfs.contoso.com/federationmetadata/2007-06/federationmetadata.xml 
  
La fenêtre du navigateur affiche les métadonnées du serveur de fédération sans mise en forme. Si les données s’affichent sans avertissements ou erreurs SSL, votre serveur de fédération est opérationnel.  
  
Étape suivante : [Déployer des dossiers de travail avec AD FS et Proxy d’Application Web : Étape 3, configurer des dossiers de travail](deploy-work-folders-adfs-step3.md)  
  
## <a name="see-also"></a>Voir aussi  
[Vue d’ensemble des dossiers de travail](Work-Folders-Overview.md)  
  

