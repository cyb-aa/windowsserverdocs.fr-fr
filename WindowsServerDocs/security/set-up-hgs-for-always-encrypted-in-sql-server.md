---
title: Configuration du service Guardian hôte pour Always Encrypted dans SQL Server
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 11/03/2018
ms.openlocfilehash: 5d1396f609a425adcd87a41d3469f3aa55774851
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402280"
---
# <a name="setting-up-the-host-guardian-service-for-always-encrypted-with-secure-enclaves-in-sql-server"></a>Configuration du service Guardian hôte pour Always Encrypted avec des enclaves sécurisées dans SQL Server 

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, SQL Server 2019 preview
 
[Always Encrypted avec des enclaves sécurisées](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-enclaves) dans SQL Server version préliminaire 2019 est une fonctionnalité conçue pour activer des calculs confidentiels sur des données sensibles stockées dans une base de données. Le service Guardian hôte (SGH) joue un rôle important dans la sécurisation de vos données quand une enclave sécurisée, configurée pour Always Encrypted, est une enclave de mémoire de sécurité basée sur la virtualisation (VBS). La sécurité d’une enclave de mémoire VBS dépend de la sécurité de l’hyperviseur Windows et, plus largement, de la sécurité de l’ordinateur hébergeant SQL Server. 

Par conséquent, avant qu’une application cliente de base de données n’autorise l’enclave de mémoire VBS utilisée par Always Encrypted pour effectuer des calculs sur des données sensibles, l’application doit attester avec un SGH approuvé. L’attestation prouve que l’ordinateur hébergeant SQL Server, qui contient l’enclave, est dans l’état correct et peut être approuvé. Pour le reste de cette rubrique, nous allons faire référence à l’ordinateur hébergeant SQL Server en tant qu’ordinateur hôte.

Il existe deux méthodes, mutuellement exclusives, qui permettent à l’application d’attester : 

- L’attestation de clé hôte autorise un hôte en prouvant qu’elle possède une clé privée connue et approuvée. L’attestation de clé hôte et un ordinateur hôte physique ou un ordinateur virtuel de génération 2 exécutant SQL Server sont recommandés pour les environnements de pré-production.
- L’attestation TPM valide les mesures matérielles pour s’assurer qu’un ordinateur hôte exécute uniquement les fichiers binaires et les stratégies de sécurité appropriés. L’attestation TPM et un ordinateur hôte physique (pas une machine virtuelle) exécutant SQL Server sont recommandés pour les environnements de production.

Pour plus d’informations sur le service Guardian hôte et ce qu’il peut mesurer, consultez [vue d’ensemble de l’infrastructure protégée et des machines virtuelles protégées](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms). Notez que bien que les documents concernent les machines virtuelles protégées, les mêmes protections, architecture et meilleures pratiques s’appliquent à SQL Server Always Encrypted à l’aide des enclaves VBS. 

Cet article vous aidera à configurer SGH dans une configuration recommandée. 

## <a name="prerequisites"></a>Prérequis 

Cette section décrit les conditions préalables pour les ordinateurs hôtes et SGH. 

### <a name="hgs-servers"></a>Serveurs SGH

- 1-3 serveurs pour exécuter SGH. 

  > [!NOTE]
  > Seul 1 serveur SGH est requis pour un environnement de test ou de pré-production.

  Ces serveurs doivent être protégés avec précaution, car ils contrôlent les ordinateurs qui peuvent exécuter vos instances de SQL Server à l’aide de Always Encrypted avec des enclaves sécurisées. 
  Il est recommandé que les différents administrateurs gèrent le cluster SGH et que vous exécutiez le service SGH sur le matériel physique isolé du reste de votre infrastructure, ou dans des infrastructures de virtualisation ou des abonnements Azure distincts.

  - Windows Server 2019 standard ou Datacenter Edition.
  - 2 processeurs
  - 8 GO DE RAM
  - stockage de 100 Go

  Vous pouvez utiliser le [canal de maintenance à long terme (LTSC)](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#long-term-servicing-channel-ltsc) ou le [canal semi-annuel](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#semi-annual-channel). 
  Pour vous inscrire et télécharger Windows Server Insider Preview, consultez [prise en main du programme Windows Insider](https://insider.windows.com/for-business-getting-started-server/).

- Choisissez un nom pour la nouvelle forêt Active Directory créée par le service Guardian hôte. 
  SGH ne doit pas être joint à votre domaine d’entreprise existant et doit avoir des administrateurs distincts qui le gèrent.   

- Règles de pare-feu et de routage pour autoriser le trafic HTTP entrant (TCP 80) ou HTTPs (TCP 443) sur les nœuds du service Guardian hôte à partir de : 

  - Ordinateurs exécutant SQL Server
  - Les ordinateurs qui exécutent des applications clientes de base de données (telles que des serveurs Web) qui émettent des requêtes de base de données et utilisent Always Encrypted avec des enclaves sécurisées. 

### <a name="sql-server-host-machines"></a>Ordinateurs hôtes SQL Server

- Votre instance de SQL Server doit s’exécuter sur une machine qui répond aux exigences suivantes :

  - Windows 
    - Windows 10 entreprise, version 1809  
    - Windows Server 2019 Datacenter (canal semi-annuel), version 1809
    - Windows Server 2019 Datacenter
  - Machine physique pour la production ou un ordinateur virtuel de 2e génération pour le test (Notez qu’Azure ne prend pas en charge les machines virtuelles de 2e génération)
  - Configuration générale requise indiquée dans [Configuration matérielle et logicielle requise pour l’installation de SQL Server](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2017).   

  Vous pouvez utiliser le [canal de maintenance à long terme (LTSC)](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#long-term-servicing-channel-ltsc) ou le [canal semi-annuel](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#semi-annual-channel). 
  Pour vous inscrire et télécharger Windows Server Insider Preview, consultez [prise en main du programme Windows Insider](https://insider.windows.com/for-business-getting-started-server/).

- Configuration requise spécifique au mode d’attestation choisi :
  - Le **mode TPM** est le mode d’attestation le plus fort et utilise un module de plateforme sécurisée (TPM) (TPM) pour valider par chiffrement que vos ordinateurs hôtes sont connus de votre centre de code (à l’aide d’un ID unique de chaque TPM), en exécutant le matériel et le microprogramme de confiance. configurations (à l’aide d’une ligne de base du module de plateforme sécurisée) et exécution du code du noyau et du mode utilisateur dignes de confiance (à l’aide de Windows Defender application Control). Le matériel suivant est requis pour utiliser le mode TPM : 
    - Module TPM 2,0 installé et activé 
    - Démarrage sécurisé activé avec la stratégie de démarrage sécurisé Microsoft (n’activez pas la stratégie d’autorité de certification de démarrage sécurisé tierce ou les stratégies personnalisées)
    - IOMMU (Intel VT-d ou AMD IOV) pour empêcher les attaques d’accès direct à la mémoire 

  - Le **mode clé d’hôte** utilise une paire de clés asymétriques (comme les clés SSH) pour identifier et autoriser les ordinateurs hôtes qui souhaitent exécuter SQL Server. Ce mode est plus facile à configurer et n’a pas de configuration matérielle requise spécifique, mais ne vérifie pas le logiciel ou le microprogramme en cours d’exécution sur les ordinateurs hôtes.  

Pour vérifier si votre module de plateforme sécurisée est compatible, exécutez les commandes suivantes sur l’ordinateur hôte où vous avez l’intention d’exécuter SQL Server à l’aide de Always Encrypted avec des enclaves sécurisées. « 2,0 » doit apparaître dans la liste des SpecVersions prises en charge pour que vous utilisiez l’attestation TPM :

```powershell
Get-CimInstance -ClassName Win32_Tpm -Namespace root/cimv2/Security/MicrosoftTpm 
```

## <a name="set-up-the-first-hgs-node"></a>Configurer le premier nœud SGH 

Le service Guardian hôte fonctionne dans une configuration hautement disponible à l’aide d’un cluster à trois nœuds. Il est recommandé de configurer un nœud complètement avant d’ajouter d’autres nœuds. 

Exécutez toutes les commandes suivantes dans une session PowerShell avec élévation de privilèges.

1. [!INCLUDE [Install the HGS server role](../../includes/guarded-fabric-install-hgs-server-role.md)]

2. [!INCLUDE [Install HGS by default](../../includes/install-hgs-default.md)] 

3. [!INCLUDE [Determine a DNN](../../includes/guarded-fabric-initialize-hgs-default-step-one.md)]

   Pour les Always Encrypted avec des enclaves sécurisées, les ordinateurs hôtes qui exécutent des SQL Server et des ordinateurs qui exécutent des applications clientes de base de données doivent contacter SGH, bien que seuls les ordinateurs hôtes requièrent l’attestation.

4. Après le redémarrage de l’ordinateur, SGH est installé et le serveur est également un contrôleur de domaine avec Active Directory configuré. 
   Au cours de la configuration de Active Directory, le compte administrateur de l’ordinateur local est ajouté au groupe Admins du domaine, et seuls les membres de ce groupe peuvent se connecter à un contrôleur de domaine.
   Connectez-vous à l’aide du compte d’administrateur de administrator@bastion.local domaine (par exemple, ou bastion. local\administrator) ou créez un autre compte d’administrateur de domaine pour vous connecter, puis configurez le service d’attestation.
   Vous devez choisir le module de plateforme sécurisée ou l’attestation de clé hôte, puis exécuter la commande correspondante. 
   Pour HgsServiceName, spécifiez le DNN que vous avez choisi.

   Pour le mode TPM :

   ```powershell
   Initialize-HgsAttestation -HgsServiceName 'hgs' -TrustTpm
   ```

   Pour le mode de clé hôte :

   ```powershell
   Initialize-HgsAttestation -HgsServiceName 'hgs' -TrustHostKey 
   ```

5. Assurez-vous que les ordinateurs hôtes qui exécutent SQL Server seront en mesure de résoudre les noms DNS de vos nouveaux membres de domaine SGH en configurant un redirecteur de vos serveurs DNS d’entreprise sur le nouveau contrôleur de domaine SGH. Si vous utilisez le DNS Windows Server, vous pouvez configurer un redirecteur conditionnel en exécutant les commandes suivantes dans une console PowerShell avec élévation de privilèges sur un serveur DNS de votre organisation. Remplacez les noms et les adresses dans la syntaxe Windows PowerShell ci-dessous, selon les besoins de votre environnement. Après avoir ajouté d’autres nœuds SGH, réexécutez cette commande pour les ajouter en tant que serveurs maîtres.

   ```powershell
   Add-DnsServerConditionalForwarderZone -Name <HGS domain name> -ReplicationScope "Forest" -MasterServers <IP addresses of HGS servers>
   ```

## <a name="set-up-additional-hgs-nodes-for-production-deployments"></a>Configurer des nœuds SGH supplémentaires pour les déploiements de production

Pour ajouter des nœuds au cluster, exécutez les commandes suivantes dans une session PowerShell avec élévation de privilèges. 

1. [!INCLUDE [Install the HGS server role](../../includes/guarded-fabric-install-hgs-server-role.md)]

2. Définissez la résolution du client DNS pour pointer vers votre serveur SGH principal afin qu’il puisse résoudre votre nom de domaine SGH. Si vous utilisez le serveur avec expérience utilisateur, vous pouvez le faire dans le centre réseau et partage. Sur Server Core, vous pouvez utiliser l’outil **sconfig. exe** , l’option 8 ou **Set-DnsClientServerAddress** pour définir l’adresse DNS. 

3. Nous allons ensuite promouvoir ce serveur en contrôleur de domaine. Vous aurez besoin d’informations d’identification d’administrateur de domaine pour effectuer cette tâche et serez invité à entrer un mot de passe en mode de réparation des services d’annuaire après l’exécution de la commande. 

   ```powershell
   $HgsDomainName = 'bastion.local' 
   $HgsDomainCredential = Get-Credential 
 
   Install-HgsServer -HgsDomainName $HgsDomainName -HgsDomainCredential $HgsDomainCredential -Restart 
   ```

4. [!INCLUDE [Initialize HGS](../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="configure-hgs-for-https"></a>Configurer SGH pour HTTPs 

Par défaut, lorsque vous initialisez le serveur SGH, il configure les sites Web IIS pour les communications HTTP uniquement.

> [!NOTE]
> La configuration du protocole HTTPs à l’aide d’un certificat de serveur SGH bien connu et approuvé est nécessaire pour empêcher les attaques de l’intercepteur et est donc conseillée pour les déploiements de production.

[!INCLUDE [Configure HTTPS](../../includes/configure-hgs-for-https.md)] 

> [!NOTE]
> Pour les Always Encrypted avec des enclaves sécurisées, le certificat SSL doit être approuvé sur les deux ordinateurs hôtes qui exécutent SQL Server et les ordinateurs qui exécutent des applications clientes de base de données doivent contacter SGH. 

## <a name="collect-attestation-info-from-the-host-machines"></a>Collecter les informations d’attestation à partir des ordinateurs hôtes

Une fois que le service SGH est configuré, il doit être configuré avec les informations d’attestation de vos ordinateurs hôtes, afin qu’il sache quelles machines doivent être autorisées à effectuer des calculs confidentiels à l’aide de Always Encrypted et des enclaves sécurisées VBS. Ces étapes varient selon le mode d’attestation que vous utilisez. 

### <a name="collect-tpm-attestation-artifacts"></a>Collecter les artefacts d’attestation TPM 

Si vous utilisez le mode TPM, exécutez les commandes suivantes dans une session PowerShell avec élévation de privilèges sur chaque ordinateur hôte pour installer la prise en charge de l’attestation et collecter les informations que vous devez inscrire auprès du service Guardian hôte. 

1. Pour installer le client SGH sur votre ordinateur hôte, installez la fonctionnalité de l’hôte service Guardian, qui installe également Hyper-V. 
   Si vous n’exécutez pas de machines virtuelles sur cet ordinateur, l’hyperviseur est requis pour activer les fonctionnalités de sécurité basées sur la virtualisation qui isolent les enclaves VBS.

   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All 
   ```

2. Redémarrez votre ordinateur lorsque vous y êtes invité pour terminer l’installation d’Hyper-V. 
3. Composez une stratégie d’intégrité du code pour limiter les logiciels pouvant s’exécuter sur le système. 
   Toute stratégie Windows Defender application Control est suffisante. 
   Si vous exécutez uniquement des logiciels Microsoft sur le serveur, les commandes suivantes vous permettront de créer rapidement une stratégie. 
   La stratégie est en mode audit, ce qui signifie qu’elle enregistre un événement à propos du code non autorisé, mais ne l’empêche pas de s’exécuter.  

   ```powershell
   mkdir C:\artifacts
   Copy-Item C:\Windows\Schemas\CodeIntegrity\ExamplePolicies\AllowMicrosoft.xml C:\artifacts\AllowMicrosoft-Audit.xml 
   Set-RuleOption -FilePath C:\artifacts\AllowMicrosoft-Audit.xml -Option 3 
   ConvertFrom-CIPolicy -XmlFilePath C:\artifacts\AllowMicrosoft-Audit.xml -BinaryFilePath C:\artifacts\AllowMicrosoft-Audit.bin 
   Copy-Item C:\artifacts\AllowMicrosoft-Audit.bin C:\Windows\System32\CodeIntegrity\SIPolicy.p7b 
   Restart-Computer 
   ```

   Windows Defender application Control propose de nombreuses fonctionnalités pour couvrir une variété de postures de sécurité. 
   Si vous avez besoin d’autoriser des logiciels non-Microsoft ou de personnaliser la stratégie par défaut, se le [Guide de déploiement de Windows Defender application Control](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control-deployment-guide).   


4. Vérifiez que la sécurité basée sur la virtualisation s’exécute sur votre ordinateur à l’aide de la commande suivante. 
   Vous saurez que VBS s’exécute si le champ DeviceGuardSecurityServicesRunning contient « HypervisorEnforcedCodeIntegrity ». 
   S’il n’est pas en cours d’exécution, téléchargez l' [outil Device Guard Readiness](https://www.microsoft.com/download/details.aspx?id=53337) et exécutez « DG_Readiness. ps1-Enable-stratégie hvci » pour l’activer.  
   
   ```powershell
   Get-ComputerInfo -Property DeviceGuard* 
   ```

5. Collectez l’identificateur et la ligne de base du module de plateforme sécurisée :

   ```powershell 
   (Get-PlatformIdentifier -Name $env:computername).Save("C:\artifacts\TpmID-$env:computername.xml") 
   Get-HgsAttestationBaselinePolicy -SkipValidation -Path "C:\artifacts\TpmBaseline-$env:computername.tcglog" 
   ```
   
6. Copiez les fichiers XML, tcglog et bin de C:\artifacts sur votre serveur SGH.
7. S’il s’agit du premier hôte de module de plateforme sécurisée que vous ajoutez au serveur SGH, vous devrez installer les certificats racines TPM approuvés sur chaque serveur SGH. 
   Suivez les [instructions de la documentation SGH](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-install-trusted-tpm-root-certificates) pour effectuer cette étape.
8. Sur le serveur SGH, autorisez cet hôte à passer l’attestation. 
   Les scripts ci-dessous supposent que les 3 fichiers ont été copiés sur C:\temp sur le serveur SGH et que le nom d’ordinateur de votre serveur est « ServerA ». 
   Ajustez les chemins d’accès et les noms en fonction de votre propre environnement. 

   ```powershell
   Add-HgsAttestationTpmHost -Name ServerA -Path C:\temp\TpmID-ServerA.xml 
   Add-HgsAttestationTpmPolicy -Name ServerA-Baseline -Path C:\temp\TpmBaseline-ServerA.tcglog 
   Add-HgsAttestationCiPolicy -Name AllowMicrosoft-Audit -Path C:\temp\AllowMicrosoft-Audit.bin 
   ```

9. Votre premier serveur est maintenant prêt à attester ! 
   Sur l’ordinateur hôte, exécutez la commande suivante pour indiquer l’emplacement d’attestation (modifiez le nom DNS de votre cluster SGH, en général, vous utiliserez le nom du service SGH associé au nom de domaine SGH). 
   Si vous recevez une erreur HostUnreachable, vérifiez que vous pouvez résoudre et exécuter un test ping sur les noms DNS de vos serveurs SGH. 

   ```powershell
   Set-HgsClientConfiguration -AttestationServerUrl https://hgs.bastion.local/Attestation -KeyProtectionServerUrl https://hgs.bastion.local/KeyProtection/ 
   ```

10. Le résultat de la commande ci-dessus doit indiquer que AttestationStatus = réussite. Si ce n’est pas le cas, consultez [échecs d’attestation](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-hosts#attestation-failures) pour obtenir de l’aide sur la résolution de l’erreur.   
11. Répétez les étapes 1-10 pour chaque ordinateur hôte. 
    Si vous utilisez un matériel identique, vous n’aurez pas besoin de capturer une nouvelle stratégie de référence ou d’intégration continue pour chaque ordinateur. 
    La ligne de base de votre premier serveur couvre toutes les machines configurées de manière identique, et la stratégie CI peut être réutilisée sur plusieurs ordinateurs tant que le logiciel Microsoft est le seul logiciel sur l’ordinateur.

### <a name="collecting-host-keys"></a>Collecte des clés d’hôte 

> [!NOTE] 
> L’attestation de clé d’hôte est uniquement recommandée pour une utilisation dans les environnements de test. L’attestation TPM offre les garanties les plus fortes que les enclaves VBS qui traitent vos données sensibles sur SQL Server exécutent du code de confiance et que les ordinateurs sont configurés avec les paramètres de sécurité recommandés. 

Si vous avez choisi de configurer SGH en mode attestation de clé hôte, vous devez générer et collecter des clés à partir de chaque ordinateur hôte et les inscrire auprès du service Guardian hôte. 

1. Pour installer le client SGH sur votre ordinateur hôte, installez la fonctionnalité de l’hôte service Guardian, qui installe également Hyper-V. 
   Si vous n’exécutez pas de machines virtuelles sur cet ordinateur, l’hyperviseur est requis pour activer les fonctionnalités de sécurité basées sur la virtualisation qui isolent les enclaves VBS qui exécutent Always Encrypted requêtes. 

   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All 
   ```

2. Redémarrez votre ordinateur lorsque vous y êtes invité pour terminer l’installation d’Hyper-V.
3. Générez une clé d’hôte unique et exportez la clé publique résultante vers un fichier. 

   ```powershell
   Set-HgsClientHostKey 
   mkdir C:\artifacts 
   Get-HgsClientHostKey -Path C:\artifacts\$env:computername.cer 
   ```
   
   Vous pouvez également spécifier une empreinte numérique si vous souhaitez utiliser votre propre certificat. 
   Cela peut être utile si vous souhaitez partager un certificat sur plusieurs ordinateurs ou utiliser un certificat lié à un module de plateforme sécurisée (TPM) ou à un HSM. Voici un exemple de création d’un certificat lié au module de plateforme sécurisée (TPM) (ce qui empêche le vol et l’utilisation de la clé privée sur un autre ordinateur et requiert uniquement un TPM 1,2) :

   ```powershell
   $tpmBoundCert = New-SelfSignedCertificate -Subject “Host Key Attestation ($env:computername)” -Provider “Microsoft Platform Crypto Provider”
   Set-HgsClientHostKey -Thumbprint $tpmBoundCert.Thumbprint
   ```

4. Copiez la clé hôte sur le service Guardian hôte. 
5. Enregistrez la clé hôte avec un nœud SGH, en utilisant un nom et un chemin d’accès appropriés pour votre environnement : 

   ```powershell
   Add-HgsAttestationHostKey -Name 'ServerA' -Path C:\temp\ServerA.cer 
   ``` 

6. Votre premier serveur est maintenant prêt à attester ! 
   Sur l’ordinateur hôte, exécutez la commande suivante pour indiquer l’emplacement d’attestation (modifiez le nom DNS de votre cluster SGH, en général, vous utiliserez le nom du service SGH associé au nom de domaine SGH). 
   Si vous recevez une erreur HostUnreachable, vérifiez que vous pouvez résoudre et exécuter un test ping sur les noms DNS de vos serveurs SGH.    

   ```powershell
   Set-HgsClientConfiguration -AttestationServerUrl http://hgs.bastion.local/Attestation -KeyProtectionServerUrl http://hgs.bastion.local/KeyProtection/  
   ```

7. Le résultat de la commande ci-dessus doit indiquer que AttestationStatus = réussite. 
   Si vous recevez une erreur HostUnreachable, cela signifie que votre ordinateur hôte ne peut pas communiquer avec SGH. 
   Assurez-vous que la résolution DNS est configurée entre l’ordinateur hôte et les serveurs SGH et que vous pouvez effectuer un test ping sur les serveurs. 
   Une erreur UnauthorizedHost indique que la clé publique n’a pas été inscrite auprès du serveur SGH. Répétez les étapes 4 et 5 pour résoudre l’erreur. 
   Si tout le reste échoue, exécutez Clear-HgsClientHostKey et répétez les étapes 3-6.   

8. Répétez les étapes 1-7 pour chaque serveur qui exécutera SQL Server Always Encrypted à l’aide des enclaves VBS.     


