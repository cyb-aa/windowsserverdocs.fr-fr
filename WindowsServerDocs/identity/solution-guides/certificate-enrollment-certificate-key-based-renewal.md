---
title: Configuration de service Web Inscription de certificats pour le renouvellement basé sur les clés de certificat sur un port personnalisé
author: Deland-Han
ms.author: delhan
manager: dcscontentpm
ms.date: 11/12/2019
ms.topic: article
ms.prod: windows-server
ms.openlocfilehash: a21a34448248658d2ceffcad07d2a4e6e17b9348
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856342"
---
# <a name="configuring-certificate-enrollment-web-service-for-certificate-key-based-renewal-on-a-custom-port"></a>Configuration de service Web Inscription de certificats pour le renouvellement basé sur les clés de certificat sur un port personnalisé

> Auteurs : Jitesh Thakur, Meera Mohideen, conseillers techniques avec le groupe Windows.
Ingénieur du support Ankit Tyagi avec le groupe Windows

## <a name="summary"></a>Résumé

Cet article fournit des instructions pas à pas pour implémenter les service Web Stratégie d’inscription de certificats (CEP) et service Web Inscription de certificats (EC) sur un port personnalisé autre que 443 pour le renouvellement basé sur les clés de certificat afin de tirer parti de la fonctionnalité de renouvellement automatique de CEP et EC.

Cet article explique également comment fonctionne CEP et EC et fournit des instructions de configuration.

> [!Note]
> Le flux de travail inclus dans cet article s’applique à un scénario spécifique. Le même flux de travail peut ne pas fonctionner dans une situation différente. Toutefois, les principes restent identiques.
>
> Exclusion de responsabilité : cette configuration est créée pour une condition spécifique dans laquelle vous ne souhaitez pas utiliser le port 443 pour la communication HTTPs par défaut pour les serveurs CEP et EC. Bien que cette configuration soit possible, elle offre une prise en charge limitée. Le service clientèle et le support technique peuvent mieux vous aider si vous suivez soigneusement ce guide en utilisant un écart minimal de la configuration du serveur Web fourni.

## <a name="scenario"></a>Scénario

Pour cet exemple, les instructions sont basées sur un environnement qui utilise la configuration suivante :

- Forêt Contoso.com disposant d’une infrastructure de clé publique (PKI) Active Directory des services de certificats (AD CS).

- Deux instances CEP/EC configurées sur un serveur qui s’exécute sous un compte de service. Une instance utilise le nom d’utilisateur et le mot de passe pour l’inscription initiale. L’autre utilise l’authentification basée sur les certificats pour le renouvellement basé sur les clés en mode renouvellement uniquement.

- Un utilisateur dispose d’un groupe de travail ou d’un ordinateur non joint à un domaine pour lequel il inscrit le certificat d’ordinateur à l’aide des informations d’identification du nom d’utilisateur et du mot de passe.

- La connexion de l’utilisateur à CEP et EC sur HTTPs se produit sur un port personnalisé, tel que 49999. (Ce port est sélectionné à partir d’une plage de ports dynamiques et n’est pas utilisé comme port statique par un autre service.)

- Lorsque la durée de vie du certificat est proche de sa fin, l’ordinateur utilise le renouvellement basé sur les clés par certificat pour renouveler le certificat sur le même canal.

![déploiement](media/certificate-enrollment-certificate-key-based-renewal-1.png)

## <a name="configuration-instructions"></a>Instructions de configuration

### <a name="overview"></a>Overview 

1. Configurez le modèle pour le renouvellement basé sur les clés.

2. Comme condition préalable, configurez un serveur CEP et EC pour l’authentification du nom d’utilisateur et du mot de passe.   
   Dans cet environnement, nous faisons référence à l’instance en tant que « CEPCES01 ».

3.  Configurez une autre instance CEP et EC à l’aide de PowerShell pour l’authentification basée sur les certificats sur le même serveur. L’instance EC utilise un compte de service.

    Dans cet environnement, nous faisons référence à l’instance en tant que « CEPCES02 ». Le compte de service utilisé est « cepcessvc ».

4.  Configurez les paramètres côté client.

### <a name="configuration"></a>Configuration

Cette section décrit les étapes à suivre pour configurer l’inscription initiale.

> [!Note]
> Vous pouvez également configurer tout compte de service utilisateur, MSA ou GMSA pour que ces fonctionnent.

Comme condition préalable, vous devez configurer CEP et EC sur un serveur à l’aide de l’authentification par nom d’utilisateur et mot de passe.

#### <a name="configure-the-template-for-key-based-renewal"></a>Configurer le modèle pour le renouvellement basé sur les clés

Vous pouvez dupliquer un modèle d’ordinateur existant et configurer les paramètres suivants du modèle :

1. Sous l’onglet nom d’objet du modèle de certificat, assurez-vous que l’option **fournir dans les options demander** et **utiliser les informations d’objet des certificats existants pour les demandes de renouvellement d’inscription automatique** est sélectionnée.
   ![de nouveaux modèles](media/certificate-enrollment-certificate-key-based-renewal-2.png) 

2. Basculez vers l’onglet **conditions d’émission** , puis activez la case à cocher approbation du gestionnaire de certificat de l' **autorité de certification** .
   Conditions requises pour l’émission de ![](media/certificate-enrollment-certificate-key-based-renewal-3.png) 

3. Attribuez l’autorisation de **lecture** et d' **inscription** au compte de service **cepcessvc** pour ce modèle.

4. Publiez le nouveau modèle sur l’autorité de certification.

> [!Note]
> Assurez-vous que les paramètres de compatibilité sur le modèle sont définis sur **Windows server 2012 R2** , car il existe un problème connu dans lequel les modèles ne sont pas visibles si la compatibilité est définie sur windows server 2016 ou version ultérieure. Pour plus d’informations, consultez [Impossible de sélectionner les modèles de certificats compatibles avec l’autorité de certification Windows server 2016 sur les serveurs ca ou CEP basés sur Windows server 2016 ou version ultérieure ](https://support.microsoft.com/en-in/help/4508802/cannot-select-certificate-templates-in-windows-server-2016).


#### <a name="configure-the-cepces01-instance"></a>Configurer l’instance CEPCES01

##### <a name="step-1-install-the-instance"></a>Étape 1 : installer l’instance

Pour installer l’instance CEPCES01, utilisez l’une des méthodes suivantes.

**Méthode 1**

Consultez les articles suivants pour obtenir des instructions pas à pas sur l’activation de CEP et EC pour l’authentification par nom d’utilisateur et mot de passe :

[Aide service Web Stratégie d’inscription de certificats](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831625(v=ws.11))

[Aide service Web Inscription de certificats](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831822(v=ws.11)#configure-a-ca-for-the-certificate-enrollment-web-service)

> [!Note]
> Veillez à ne pas sélectionner l’option « Activer le renouvellement basé sur les clés » si vous configurez les instances CEP et EC de nom d’utilisateur et authentification par mot de passe.

**Méthode 2**

Vous pouvez utiliser les applets de commande PowerShell suivantes pour installer les instances CEP et EC :

```PowerShell
Import-Module ServerManager
Add-WindowsFeature Adcs-Enroll-Web-Pol
Add-WindowsFeature Adcs-Enroll-Web-Svc
```

```PowerShell
Install-AdcsEnrollmentPolicyWebService -AuthenticationType Username -SSLCertThumbprint "sslCertThumbPrint"
```

Cette commande installe le service Web Stratégie d’inscription de certificats (CEP) en spécifiant qu’un nom d’utilisateur et un mot de passe sont utilisés pour l’authentification. 

> [!Note]
> Dans cette commande, \<**SSLCertThumbPrint**\> est l’empreinte numérique du certificat qui sera utilisé pour la liaison des services Internet (IIS).

```PowerShell
Install-AdcsEnrollmentWebService -ApplicationPoolIdentity -CAConfig "CA1.contoso.com\contoso-CA1-CA" -SSLCertThumbprint "sslCertThumbPrint" -AuthenticationType Username
```

Cette commande installe les service Web Inscription de certificats (ces) pour utiliser l’autorité de certification pour le nom d’ordinateur **CA1.contoso.com** et le nom commun de l’autorité de certification **contoso-CA1-ca**. L’identité du EC est spécifiée en tant qu’identité du pool d’applications par défaut. Le type d’authentification est **username**. SSLCertThumbPrint est l’empreinte numérique du certificat qui sera utilisé pour la liaison des services Internet (IIS).

##### <a name="step-2-check-the-internet-information-services-iis-manager-console"></a>Étape 2 vérifier la console du gestionnaire de Internet Information Services (IIS)

Une fois l’installation réussie, vous vous attendez à voir l’affichage suivant dans la console du gestionnaire de Internet Information Services (IIS).
Gestionnaire des services Internet ![](media/certificate-enrollment-certificate-key-based-renewal-4.png) 

Sous **site Web par défaut**, sélectionnez **ADPolicyProvider_CEP_UsernamePassword**, puis ouvrez **paramètres**de l’application. Notez l' **ID** et l' **URI**.

Vous pouvez ajouter un **nom convivial** pour la gestion.

#### <a name="configure-the-cepces02-instance"></a>Configurer l’instance CEPCES02

##### <a name="step-1-install-the-cep-and-ces-for-key-based-renewal-on-the-same-server"></a>Étape 1 : installez CEP et EC pour le renouvellement basé sur les clés sur le même serveur. 

Exécutez la commande suivante dans PowerShell :

```PowerShell
Install-AdcsEnrollmentPolicyWebService -AuthenticationType Certificate -SSLCertThumbprint "sslCertThumbPrint" -KeyBasedRenewal
```

Cette commande installe le service Web Stratégie d’inscription de certificats (CEP) et spécifie qu’un certificat est utilisé pour l’authentification. 

> [!Note]
> Dans cette commande, \<SSLCertThumbPrint\> est l’empreinte numérique du certificat qui sera utilisé pour la liaison des services Internet (IIS). 

Le renouvellement basé sur les clés permet aux clients de certificats de renouveler leurs certificats à l’aide de la clé de leur certificat existant pour l’authentification. En mode de renouvellement basé sur les clés, le service retourne uniquement les modèles de certificats qui sont définis pour le renouvellement basé sur les clés.

```PowerShell
Install-AdcsEnrollmentWebService -CAConfig "CA1.contoso.com\contoso-CA1-CA" -SSLCertThumbprint "sslCertThumbPrint" -AuthenticationType Certificate -ServiceAccountName "Contoso\cepcessvc" -ServiceAccountPassword (read-host "Set user password" -assecurestring) -RenewalOnly -AllowKeyBasedRenewal
```

Cette commande installe les service Web Inscription de certificats (ces) pour utiliser l’autorité de certification pour le nom d’ordinateur **CA1.contoso.com** et le nom commun de l’autorité de certification **contoso-CA1-ca**. 

Dans cette commande, l’identité de l’service Web Inscription de certificats est spécifiée en tant que compte de service **cepcessvc** . Le type d’authentification est **Certificate**. **SSLCertThumbPrint** est l’empreinte numérique du certificat qui sera utilisé pour la liaison des services Internet (IIS).

L’applet de commande **RenewalOnly** autorise EC à s’exécuter en mode renouvellement uniquement. L’applet de commande **AllowKeyBasedRenewal** spécifie également que le EC accepte les demandes de renouvellement basées sur les clés pour le serveur d’inscription. Il s’agit de certificats clients valides pour l’authentification qui ne sont pas directement mappés à un principal de sécurité.

> [!Note]
> Le compte de service doit faire partie du groupe **IISUsers** sur le serveur.

##### <a name="step-2-check-the-iis-manager-console"></a>Étape 2 vérifier la console du gestionnaire des services Internet

Une fois l’installation réussie, vous vous attendez à voir apparaître l’affichage suivant dans la console du gestionnaire des services Internet.
Gestionnaire des services Internet ![](media/certificate-enrollment-certificate-key-based-renewal-5.png) 

Sélectionnez **KeyBasedRenewal_ADPolicyProvider_CEP_Certificate** sous **site Web par défaut** et ouvrez paramètres de l' **application**. Prenez note de l' **ID** et de l' **URI**. Vous pouvez ajouter un **nom convivial** pour la gestion.

> [!Note]
> Si l’instance est installée sur un nouveau serveur, vérifiez l’ID pour vous assurer que l’ID est le même que celui qui a été généré dans l’instance CEPCES01. Vous pouvez copier et coller la valeur directement si elle est différente.

#### <a name="complete-certificate-enrollment-web-services-configuration"></a>Terminer la configuration des services Web inscription de certificats

Pour pouvoir inscrire le certificat pour le compte des fonctionnalités de CEP et EC, vous devez configurer le compte d’ordinateur du groupe de travail dans Active Directory puis configurer la délégation avec restriction sur le compte de service.

##### <a name="step-1-create-a-computer-account-of-the-workgroup-computer-in-active-directory"></a>Étape 1 : créer un compte d’ordinateur de l’ordinateur de groupe de travail dans Active Directory

Ce compte sera utilisé pour l’authentification vers le renouvellement basé sur les clés et l’option « publier sur Active Directory » sur le modèle de certificat.

> [!Note]
> Vous n’avez pas besoin de joindre le domaine à l’ordinateur client. Ce compte se trouve dans l’image lors de l’authentification par certificat dans le service dsmapper.

![Nouvel objet](media/certificate-enrollment-certificate-key-based-renewal-6.png) 
 
##### <a name="step-2-configure-the-service-account-for-constrained-delegation-s4u2self"></a>Étape 2 : configurer le compte de service pour la délégation avec restriction (S4U2Self)

Exécutez la commande PowerShell suivante pour activer la délégation avec restriction (S4U2Self ou n’importe quel protocole d’authentification) :

```PowerShell
Get-ADUser -Identity cepcessvc | Set-ADAccountControl -TrustedToAuthForDelegation $True
Set-ADUser -Identity cepcessvc -Add @{'msDS-AllowedToDelegateTo'=@('HOST/CA1.contoso.com','RPCSS/CA1.contoso.com')}
```

> [!Note]
> Dans cette commande, \<cepcessvc\> est le compte de service, et < CA1. contoso. com > est l’autorité de certification.

> [!Important]
> Nous n’autorisons pas l’indicateur RENEWALONBEHALOF sur l’autorité de certification dans cette configuration, car nous utilisons la délégation avec restriction pour effectuer la même tâche pour nous. Cela nous permet d’éviter d’ajouter l’autorisation pour le compte de service à la sécurité de l’autorité de certification.

##### <a name="step-3-configure-a-custom-port-on-the-iis-web-server"></a>Étape 3 : configurer un port personnalisé sur le serveur Web IIS

1. Dans la console du gestionnaire des services Internet, sélectionnez site Web par défaut.

2. Dans le volet Actions, sélectionnez Modifier la liaison de site. 

3. Remplacez le paramètre de port par défaut 443 par votre port personnalisé. L’exemple de capture d’écran montre un paramètre de port de 49999.
   ![de changement de port](media/certificate-enrollment-certificate-key-based-renewal-7.png) 

##### <a name="step-4-edit-the-ca-enrollment-services-object-on-active-directory"></a>Étape 4 : modifier l’objet services d’inscription de l’autorité de certification sur Active Directory

1. Sur un contrôleur de domaine, ouvrez Adsiedit. msc.

2. [Connectez-vous à la partition de configuration](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/ff730188(v=ws.10)), puis accédez à l’objet services d’inscription de l’autorité de certification :
   
   CN = ENTCA, CN = Services d’inscription, CN = Public Key Services, CN = Services, CN = Configuration, DC = contoso, DC = com

3. Cliquez avec le bouton droit et modifiez l’objet de l’autorité de certification. Modifiez l’attribut **attribut mspki-inscription-** Servers à l’aide du port personnalisé avec les URI de serveur CEP et EC qui ont été trouvés dans les paramètres de l’application. Par exemple :

   ```
   140https://cepces.contoso.com:49999/ENTCA_CES_UsernamePassword/service.svc/CES0   
   181https://cepces.contoso.com:49999/ENTCA_CES_Certificate/service.svc/CES1
   ```
   
   ![Éditeur ADSI](media/certificate-enrollment-certificate-key-based-renewal-8.png) 

#### <a name="configure-the-client-computer"></a>Configuration de l’ordinateur client

Sur l’ordinateur client, configurez les stratégies d’inscription et la stratégie d’inscription automatique. Pour cela, procédez comme suit:

1. Sélectionnez **démarrer** > **exécuter**, puis entrez **gpedit. msc**.

2. Accédez à **Configuration ordinateur** > **Paramètres Windows** > **paramètres de sécurité**, puis cliquez sur **stratégies de clé publique**.

3. Activez la **stratégie client des services de certificats-inscription automatique** pour qu’elle corresponde aux paramètres de la capture d’écran suivante.
   ![](media/certificate-enrollment-certificate-key-based-renewal-9.png) de stratégie de groupe de certificats
 
4. Activer **les services de certificats client-stratégie d’inscription de certificats**.

   a. Cliquez sur **Ajouter** pour ajouter une stratégie d’inscription et entrez l’URI CEP avec **UserNamePassword** que nous avons modifié dans ADSI.
   
   b. Pour **type d’authentification**, sélectionnez **nom d’utilisateur/mot de passe**.
   
   c. Définissez une priorité de **10**, puis validez le serveur de stratégie.
      Stratégie d’inscription de ![](media/certificate-enrollment-certificate-key-based-renewal-10.png)

   > [!Note]
   > Assurez-vous que le numéro de port est ajouté à l’URI et est autorisé sur le pare-feu.

5. Inscrivez le premier certificat de l’ordinateur par le biais de certlm. msc.
   Stratégie d’inscription de ![](media/certificate-enrollment-certificate-key-based-renewal-11.png)

   Sélectionnez le modèle KBR et inscrivez le certificat.
   Stratégie d’inscription de ![](media/certificate-enrollment-certificate-key-based-renewal-12.png)

6. Ouvrez de nouveau **gpedit. msc** . Modifiez la **stratégie client des services de certificats – inscription de certificats**, puis ajoutez la stratégie d’inscription de renouvellement basée sur les clés :

   a. Cliquez sur **Ajouter**, entrez l’URI CEP avec le **certificat** que nous avons modifié dans ADSI. 
   
   b. Définissez une priorité de **1**, puis validez le serveur de stratégie. Vous êtes invité à vous authentifier et à choisir le certificat que nous avons inscrit initialement.

   ![Stratégie d’inscription](media/certificate-enrollment-certificate-key-based-renewal-13.png) 

> [!Note]
> Assurez-vous que la valeur de priorité de la stratégie d’inscription de renouvellement par clé est inférieure à la priorité de la stratégie d’inscription de mot de passe du nom d’utilisateur. La première préférence est donnée à la priorité la plus basse.

## <a name="testing-the-setup"></a>Test de l’installation

Pour vous assurer que le renouvellement automatique fonctionne, vérifiez que le renouvellement manuel fonctionne en renouvelant le certificat avec la même clé à l’aide de MMC. En outre, vous devez être invité à sélectionner un certificat lors du renouvellement. Vous pouvez choisir le certificat que nous avons inscrit précédemment. L’invite est attendue.

Ouvrez le magasin de certificats personnels de l’ordinateur et ajoutez la vue « certificats archivés ». Pour ce faire, ajoutez le composant logiciel enfichable compte d’ordinateur local à MMC. exe, mettez en surbrillance **certificats (ordinateur local)** en cliquant dessus, cliquez sur **Afficher** dans l' **onglet action** situé à droite ou en haut de la console MMC, cliquez sur **afficher les options**, sélectionnez **certificats archivés**, puis cliquez sur **OK**.

### <a name="method-1"></a>Méthode 1 : 

Exécutez la commande suivante :

```PowerShell
certreq -machine -q -enroll -cert <thumbprint> renew
```

![commande](media/certificate-enrollment-certificate-key-based-renewal-14.png)

### <a name="method-2"></a>Méthode 2

Avancez la date et l’heure sur la machine cliente dans la durée de renouvellement du modèle de certificat.

Par exemple, le modèle de certificat a un paramètre de validité de 2 jours et un paramètre de renouvellement de 8 heures. L’exemple de certificat a été émis à 4:00 h 00. le 18e jour du mois, expire à 4:00 A.M. le 20. Le moteur d’inscription automatique est déclenché au redémarrage et à chaque intervalle de 8 heures (approximativement).

Par conséquent, si vous avancez le délai à 8:10 h 00 le 19 h puisque notre fenêtre de renouvellement était définie à 8 heures sur le modèle, l’exécution de Certutil-Pulse (pour déclencher le moteur AE) inscrit le certificat pour vous.

![commande](media/certificate-enrollment-certificate-key-based-renewal-15.png)
 
Une fois le test terminé, rétablissez la valeur d’origine du paramètre d’heure, puis redémarrez l’ordinateur client.

> [!Note]
> La capture d’écran précédente est un exemple qui démontre que le moteur d’inscription automatique fonctionne comme prévu, car la date de l’autorité de certification est toujours définie sur la valeur 18. Par conséquent, il continue d’émettre des certificats. Dans une situation réelle, ce nombre important de renouvellements ne se produit pas.

## <a name="references"></a>Références

[Guide de laboratoire de test : démonstration du renouvellement basé sur les clés de certificat](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj590165(v%3Dws.11))

[Services Web inscription de certificats](https://techcommunity.microsoft.com/t5/Ask-the-Directory-Services-Team/Certificate-Enrollment-Web-Services/ba-p/397385)

[Install-AdcsEnrollmentPolicyWebService](https://docs.microsoft.com/powershell/module/adcsdeployment/install-adcsenrollmentpolicywebservice?view=win10-ps)

[Install-AdcsEnrollmentWebService](https://docs.microsoft.com/powershell/module/adcsdeployment/install-adcsenrollmentwebservice?view=win10-ps)

Voir aussi

[Forum sur la sécurité de Windows Server](https://aka.ms/adcsforum)

[Forum aux questions (FAQ) sur l’infrastructure de clé publique (PKI) des services de certificats Active Directory (AD CS)](https://aka.ms/adcsfaq)

[Bibliothèque et références de documentation sur l’infrastructure PKI de Windows](https://social.technet.microsoft.com/wiki/contents/articles/987.windows-pki-documentation-reference-and-library.aspx)

[Blog sur l’infrastructure PKI de Windows](https://blogs.technet.com/b/pki/)

[Comment configurer la délégation Kerberos avec restriction (S4U2Proxy ou Kerberos uniquement) sur un compte de service personnalisé pour les pages proxy d’inscription par le Web](https://support.microsoft.com/help/4494313/configuring-web-enrollment-proxy-for-s4u2proxy-constrained-delegation)