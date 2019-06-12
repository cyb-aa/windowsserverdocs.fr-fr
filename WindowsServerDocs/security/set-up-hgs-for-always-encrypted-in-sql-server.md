---
title: Configurer le Service Guardian hôte pour Always Encrypted dans SQL Server
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 11/03/2018
ms.openlocfilehash: 70f6f8c2db742361deecaa216b053d8b1d057a3d
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812603"
---
# <a name="setting-up-the-host-guardian-service-for-always-encrypted-with-secure-enclaves-in-sql-server"></a>Configuration du Service Guardian hôte pour Always Encrypted avec enclaves sécurisés dans SQL Server 

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, version préliminaire de SQL Server 2019
 
[Always Encrypted avec enclaves sécurisés](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-enclaves) dans SQL Server 2019 preview est une fonctionnalité conçue pour permettre des calculs sur les données sensibles stockées dans une base de données confidentielles. Le Service Guardian hôte (SGH) joue un rôle important dans la sécurisation de vos données quand une enclave sécurisée, configurée pour Always Encrypted, est une enclave de mémoire de sécurité basés sur la virtualisation (VBS). La sécurité d’une enclave de mémoire VBS dépend de la sécurité de l’hyperviseur Windows et, plus généralement, la sécurité de l’ordinateur qui héberge SQL Server. 

Par conséquent, avant d’une application cliente de base de données autorise l’enclave de mémoire VBS utilisé pour Always Encrypted pour effectuer des calculs sur les données sensibles, l’application doit attester avec un service SGH approuvé. Attestation prouve la machine hébergeant SQL Server, qui contient l’enclave, est dans l’état correct et peut être approuvée. Pour le reste de cette rubrique, nous allons faire référence à l’ordinateur qui héberge SQL Server en tant que simplement l’ordinateur hôte.

Il existe deux moyens mutuellement exclusifs pour l’application d’attester : 

- L’attestation de clé hôte autorise un hôte en prouvant qu’il possède une clé privée connue et approuvée. L’attestation de clé hôte et un ordinateur hôte physique ou une machine virtuelle de génération 2 qui exécute SQL Server est recommandée pour les environnements de préproduction.
- L’attestation TPM valide des mesures de matériel pour assurer qu'un hôte s’exécute uniquement les fichiers binaires corrects et les stratégies de sécurité. L’attestation TPM et un ordinateur hôte physique (pas une machine virtuelle) exécutant SQL Server est recommandée pour les environnements de production.

Pour plus d’informations sur le Service Guardian hôte et il peut mesurer, consultez [protégées de vue d’ensemble des machines virtuelles et service Guardian fabric](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms). Notez que bien que les documents parlent de machines virtuelles protégées, les mêmes protections, architecture et meilleures pratiques s’appliquent à SQL Server Always Encrypted à l’aide des enclaves VBS. 

Cet article vous aidera à configurer le service SGH dans une configuration recommandée. 

## <a name="prerequisites"></a>Prérequis 

Cette section aborde les conditions requises pour les machines SGH et hôte. 

### <a name="hgs-servers"></a>Serveurs SGH

- 1-3 : serveurs pour exécuter le service SGH. 

  > [!NOTE]
  > Seulement 1 serveur SGH est requis pour un environnement de test ou de préproduction.

  Ces serveurs doivent être soigneusement protégés dans la mesure où elles contrôlent les ordinateurs peuvent s’exécuter vos instances de SQL Server à l’aide d’Always Encrypted avec enclaves sécurisés. 
  Il est recommandé que différents administrateurs gèrent le cluster de SGH et que vous exécutez le service SGH sur du matériel physique isolé du reste de votre infrastructure, ou dans les abonnements Azure ou les structures de virtualisation distinct.

  - Windows Server 2019 Standard ou Datacenter edition.
  - 2 processeurs
  - 8 GO DE RAM
  - 100 Go de stockage

  Vous pouvez utiliser la [canal de maintenance à long terme (LTSC)](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#long-term-servicing-channel-ltsc) ou [canal semi-annuel](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#semi-annual-channel). 
  Pour vous inscrire et télécharger Windows Server Insider Preview, consultez [mise en route avec le programme Insider de Windows](https://insider.windows.com/for-business-getting-started-server/).

- Choisissez un nom pour la nouvelle forêt Active Directory créé par le Service Guardian hôte. 
  SGH ne doit pas être joints à votre domaine d’entreprise existant et doit avoir sa gestion des administrateurs distincts.   

- Pare-feu et les règles de routage pour autoriser HTTP entrantes (TCP 80) ou le trafic HTTPS (TCP 443) sur les nœuds de Service Guardian hôte à partir de : 

  - Les ordinateurs qui exécutent SQL Server
  - Les ordinateurs qui exécutent les applications de client de base de données (par exemple, des serveurs web) qui émettent des requêtes de base de données et utilisent Always Encrypted avec enclaves sécurisés. 

### <a name="sql-server-host-machines"></a>Ordinateurs hôtes SQL Server

- Votre instance de SQL Server doit s’exécuter sur un ordinateur qui remplit les conditions suivantes :

  - Windows : 
    - Windows 10 entreprise version 1809  
    - Windows Server Datacenter 2019 (canal semi-annuel), version 1809
    - Centre de données Windows Server 2019
  - Ordinateur physique pour la production, ou une machine virtuelle de génération 2 pour le test (Notez que Azure ne prend pas en charge les machines virtuelles de génération 2)
  - Exigences générales répertoriées dans [matérielle et logicielle requise pour l’installation de SQL Server](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2017).   

  Vous pouvez utiliser la [canal de maintenance à long terme (LTSC)](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#long-term-servicing-channel-ltsc) ou [canal semi-annuel](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#semi-annual-channel). 
  Pour vous inscrire et télécharger Windows Server Insider Preview, consultez [mise en route avec le programme Insider de Windows](https://insider.windows.com/for-business-getting-started-server/).

- Configuration requise spécifique pour le mode d’attestation choisi :
  - **Mode de module de plateforme sécurisée** est le mode d’attestation plus fort et utilise un Module de plateforme approuvés (TPM) pour valider par chiffrement que vos ordinateurs hôtes sont connus pour votre centre de données (à l’aide d’un ID unique à partir de chaque module de plateforme sécurisée) en cours d’exécution approuvé matériel et microprogramme configurations (à l’aide d’une ligne de base du module de plateforme sécurisée) et l’exécution du noyau digne de confiance et le code en mode utilisateur (à l’aide de Windows Defender Application Control). Le matériel suivant est requis pour utiliser le mode de module de plateforme sécurisée : 
    - Module TPM 2.0 installé et activé 
    - Démarrage sécurisé activé avec la stratégie de démarrage sécurisé de Microsoft (n’activez pas la stratégie d’autorité de certification le démarrage sécurisé tiers 3e et les stratégies personnalisées)
    - IOMMU (Intel VT-d ou IOV d’AMD) pour empêcher des attaques d’accès direct à la mémoire 

  - **Mode clé hôte** utilise une paire de clés asymétriques (un peu comme les clés SSH) pour identifier et autoriser les ordinateurs hôtes qui souhaitent exécuter SQL Server. Ce mode est plus facile de configurer et ne dispose pas des exigences de matériel spécifique, mais ne vérifie pas le logiciel ou le microprogramme en cours d’exécution sur les ordinateurs hôtes.  

Pour vérifier si votre module de plateforme sécurisée est compatible, exécutez les commandes suivantes sur l’ordinateur hôte où vous envisagez d’exécuter SQL Server à l’aide d’Always Encrypted avec enclaves sécurisés. « 2.0 » doit apparaître dans la liste des SpecVersions pris en charge que vous pouvez utiliser l’attestation TPM :

```powershell
Get-CimInstance -ClassName Win32_Tpm -Namespace root/cimv2/Security/MicrosoftTpm 
```

## <a name="set-up-the-first-hgs-node"></a>Configurer le premier nœud SGH 

Le Service Guardian hôte fonctionne dans une configuration hautement disponible à l’aide d’un cluster à 3 nœuds. Il est recommandé de configurer un seul nœud complètement avant d’ajouter d’autres nœuds. 

Exécutez toutes les commandes suivantes dans une session PowerShell avec élévation de privilèges.

1. [!INCLUDE [Install the HGS server role](../../includes/guarded-fabric-install-hgs-server-role.md)]

2. [!INCLUDE [Install HGS by default](../../includes/install-hgs-default.md)] 

3. [!INCLUDE [Determine a DNN](../../includes/guarded-fabric-initialize-hgs-default-step-one.md)]

   Pour Always Encrypted avec enclaves sécurisés, les ordinateurs hôtes qui exécutent SQL Server et les machines qui exécutent des applications de client de base de données que les deux doivent contacter SGH, même si seuls les ordinateurs hôtes nécessitent d’attestation.

4. Une fois que l’ordinateur redémarré, SGH sera installé et le serveur sera également être un contrôleur de domaine avec Active Directory configuré. 
   Lors de la configuration d’Active Directory, le compte d’administrateur local d’ordinateur est ajouté au groupe Admins du domaine, et seuls les membres de ce groupe peuvent se connecter à un contrôleur de domaine.
   Connectez-vous à l’aide de compte administrateur du domaine (par exemple, administrator@bastion.local ou bastion.local\administrator) ou créer un autre compte d’administrateur de domaine pour vous connecter, puis configurez le service d’attestation.
   Vous devez choisir l’attestation de clé hôte ou le module de plateforme sécurisée et exécutez la commande correspondante. 
   Pour le HgsServiceName, spécifiez le réseau de neurones profond que vous avez choisi.

   Pour le mode de module de plateforme sécurisée :

   ```powershell
   Initialize-HgsAttestation -HgsServiceName 'hgs' -TrustTpm
   ```

   Pour le mode de clé hôte :

   ```powershell
   Initialize-HgsAttestation -HgsServiceName 'hgs' -TrustHostKey 
   ```

5. Vérifiez que les ordinateurs hôtes qui exécutent SQL Server sera en mesure de résoudre les noms DNS de vos nouveaux membres de domaine SGH en configurant un redirecteur de vos serveurs DNS d’entreprise pour le nouveau contrôleur de domaine SGH. Si vous utilisez le DNS de Windows Server, vous pouvez configurer un redirecteur conditionnel en exécutant les commandes suivantes dans une console PowerShell avec élévation de privilèges sur un serveur DNS de votre organisation. Remplacez les noms et adresses dans la syntaxe Windows PowerShell ci-dessous autant que nécessaire pour votre environnement. Une fois que vous ajoutez plus de nœuds SGH, exécutez cette commande à nouveau pour les ajouter comme serveurs maîtres.

   ```powershell
   Add-DnsServerConditionalForwarderZone -Name <HGS domain name> -ReplicationScope "Forest" -MasterServers <IP addresses of HGS servers>
   ```

## <a name="set-up-additional-hgs-nodes-for-production-deployments"></a>Configurer des nœuds SGH supplémentaires pour les déploiements de production

Pour ajouter des nœuds au cluster, exécutez les commandes suivantes dans une session PowerShell avec élévation de privilèges. 

1. [!INCLUDE [Install the HGS server role](../../includes/guarded-fabric-install-hgs-server-role.md)]

2. Définir le programme de résolution client DNS pour pointer vers votre serveur SGH principal afin qu’il peut résoudre votre nom de domaine SGH. Si vous utilisez un serveur avec expérience, nous vous conseillons de vous faire dans le Centre réseau et partage. Sur Server Core, vous pouvez utiliser la **sconfig.exe** outil, l’option 8, ou **Set-DnsClientServerAddress** pour définir l’adresse DNS. 

3. Ensuite, nous sera promouvoir ce serveur en contrôleur de domaine. Vous aurez besoin des informations d’identification d’administrateur de domaine pour effectuer cette tâche et serez invité à entrer un mot de passe du Mode de réparation des Services annuaire après avoir exécuté la commande. 

   ```powershell
   $HgsDomainName = 'bastion.local' 
   $HgsDomainCredential = Get-Credential 
 
   Install-HgsServer -HgsDomainName $HgsDomainName -HgsDomainCredential $HgsDomainCredential -Restart 
   ```

4. [!INCLUDE [Initialize HGS](../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="configure-hgs-for-https"></a>Configurer le service SGH pour HTTPS 

Par défaut, lorsque vous initialisez un serveur SGH il configurera les sites web IIS pour les communications HTTP uniquement.

> [!NOTE]
> Configuration HTTPS à l’aide d’un certificat de serveur SGH bien connu et approuvé est nécessaire pour empêcher les attaques man-in-the-middle et est par conséquent conseillé pour les déploiements de production.

[!INCLUDE [Configure HTTPS](../../includes/configure-hgs-for-https.md)] 

> [!NOTE]
> Pour Always Encrypted avec enclaves sécurisés, le certificat SSL doit être approuvé sur les deux ordinateurs hôtes qui exécutent SQL Server et les machines qui exécutent des applications de client de base de données doivent contacter SGH. 

## <a name="collect-attestation-info-from-the-host-machines"></a>Collecter des informations de l’attestation à partir des ordinateurs hôtes

Une fois que SGH est configuré, il doit être configuré avec les informations de l’attestation à partir de vos ordinateurs hôtes afin qu’il sache quels ordinateurs doivent être autorisés à effectuer des calculs confidentielles à l’aide d’Always Encrypted et VBS enclaves sécurisés. Ces étapes varient selon le mode d’attestation que vous utilisez. 

### <a name="collect-tpm-attestation-artifacts"></a>Collecter les artefacts de l’attestation TPM 

Si vous utilisez le mode de module de plateforme sécurisée, exécutez les commandes suivantes dans une session PowerShell avec élévation de privilèges sur chaque ordinateur hôte pour installer la prise en charge pour l’attestation et de collecter les informations que vous devrez inscrire avec le Service Guardian hôte. 

1. Pour installer le client HGS sur votre ordinateur hôte, installez la fonctionnalité de l’hôte service Guardian, qui sera également installer Hyper-V. 
   Bien que vous n’allez pas exécuter des machines virtuelles sur cet ordinateur, l’hyperviseur est nécessaire pour activer les fonctionnalités de sécurité basés sur la virtualisation qui isolent les enclaves VBS.

   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All 
   ```

2. Redémarrez votre ordinateur lorsque vous êtes invité à terminer l’installation d’Hyper-V. 
3. Composer une stratégie d’intégrité de code pour restreindre les logiciels peuvent s’exécuter sur le système. 
   Toute stratégie Windows Defender Application Control est suffisante. 
   Si vous exécutez uniquement des logiciels Microsoft sur le serveur, les commandes suivantes crée rapidement une stratégie pour vous. 
   La stratégie sera en mode audit, ce qui signifie qu’il enregistrera un événement concernant le code non autorisé, mais ne l’empêchera pas de s’exécuter.  

   ```powershell
   mkdir C:\artifacts
   Copy-Item C:\Windows\Schemas\CodeIntegrity\ExamplePolicies\AllowMicrosoft.xml C:\artifacts\AllowMicrosoft-Audit.xml 
   Set-RuleOption -FilePath C:\artifacts\AllowMicrosoft-Audit.xml -Option 3 
   ConvertFrom-CIPolicy -XmlFilePath C:\artifacts\AllowMicrosoft-Audit.xml -BinaryFilePath C:\artifacts\AllowMicrosoft-Audit.bin 
   Copy-Item C:\artifacts\AllowMicrosoft-Audit.bin C:\Windows\System32\CodeIntegrity\SIPolicy.p7b 
   Restart-Computer 
   ```

   Windows Defender Application Control comporte de nombreuses fonctionnalités pour couvrent une variété de postures de sécurité. 
   Si vous avez besoin autoriser les logiciels non Microsoft ou de personnaliser la stratégie par défaut, se la [guide de déploiement de Windows Defender Application Control](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control-deployment-guide).   


4. Vérifiez que la sécurité en fonction de la virtualisation s’exécute sur votre ordinateur avec la commande suivante. 
   Vous saurez que VBS est en cours d’exécution si le champ de DeviceGuardSecurityServicesRunning a « HypervisorEnforcedCodeIntegrity » répertorié dans celui-ci. 
   Si elle n’est pas en cours d’exécution, téléchargez le [Device Guard Readiness Tool](https://www.microsoft.com/download/details.aspx?id=53337) et exécutez « DG_Readiness.ps1-activer - HVCI » pour l’activer.  
   
   ```powershell
   Get-ComputerInfo -Property DeviceGuard* 
   ```

5. Collecter l’identificateur de module de plateforme sécurisée et de la ligne de base :

   ```powershell 
   (Get-PlatformIdentifier -Name $env:computername).Save("C:\artifacts\TpmID-$env:computername.xml") 
   Get-HgsAttestationBaselinePolicy -SkipValidation -Path "C:\artifacts\TpmBaseline-$env:computername.tcglog" 
   ```
   
6. Copier le code xml, tcglog et aux fichiers bin C:\artifacts à votre serveur SGH.
7. Si c’est le premier hôte de module de plateforme sécurisée, que vous ajoutez à un serveur SGH, vous devez installer les certificats racines de TPM approuvés sur chaque serveur SGH. 
   Suivez le [obtenir des conseils sur la documentation SGH](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-install-trusted-tpm-root-certificates) pour effectuer cette étape.
8. Sur le serveur SGH, autoriser cet hôte pour passer une attestation. 
   Les scripts ci-dessous supposent les 3 fichiers ont été copiés sur le serveur SGH pour C:\temp et que le nom de l’ordinateur de votre serveur est « ServerA ». 
   Ajuster les chemins d’accès et les noms à votre propre environnement. 

   ```powershell
   Add-HgsAttestationTpmHost -Name ServerA -Path C:\temp\TpmID-ServerA.xml 
   Add-HgsAttestationTpmPolicy -Name ServerA-Baseline -Path C:\temp\TpmBaseline-ServerA.tcglog 
   Add-HgsAttestationCiPolicy -Name AllowMicrosoft-Audit -Path C:\temp\AllowMicrosoft-Audit.bin 
   ```

9. Votre premier serveur est maintenant prêt à attester ! 
   Sur l’ordinateur hôte, exécutez la commande suivante pour lui indiquer où d’attester (modifier que le nom DNS à celle de votre cluster SGH, en général, vous utiliserez le nom du Service HGS associée au nom de domaine SGH). 
   Si vous recevez une erreur HostUnreachable, garantir la résoudre un test ping sur les noms DNS de vos serveurs SGH. 

   ```powershell
   Set-HgsClientConfiguration -AttestationServerUrl https://hgs.bastion.local/Attestation -KeyProtectionServerUrl https://hgs.bastion.local/KeyProtection/ 
   ```

10. Le résultat de la commande ci-dessus doit indiquer que AttestationStatus = réussite. Si elle n’est pas le cas, consultez [d’attestation d’échecs](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-hosts#attestation-failures) pour obtenir des conseils sur la façon de résoudre l’erreur.   
11. Répétez les étapes 1 à 10 pour chaque ordinateur hôte. 
    Si vous utilisez un matériel identique, vous devrez pas capturer une nouvelle ligne de base ou d’une stratégie d’intégrité pour chaque ordinateur. 
    La ligne de base à partir de votre premier serveur couvre tous les ordinateurs configurés de manière identique, et la stratégie de l’élément de configuration peut être réutilisée sur plusieurs ordinateurs tant que logiciel de Microsoft est le seul logiciel sur l’ordinateur.

### <a name="collecting-host-keys"></a>Collecte des clés d’hôte 

> [!NOTE] 
> L’attestation de clé hôte est recommandée uniquement pour une utilisation dans des environnements de test. L’attestation TPM fournit les garanties plus fortes qu’enclaves VBS traiter vos données sensibles sur SQL Server sont en cours d’exécution du code fiable et les ordinateurs sont configurés avec les paramètres de sécurité recommandée. 

Si vous avez choisi de configurer le service SGH en mode de l’attestation de clé d’hôte, vous devrez générer et de collecter les clés à partir de chaque ordinateur hôte et de les enregistrer avec le Service Guardian hôte. 

1. Pour obtenir le client HGS installé sur votre ordinateur hôte, installez la fonctionnalité de l’hôte service Guardian, qui sera également installer Hyper-V. 
   Bien que vous n’allez pas exécuter des machines virtuelles sur cet ordinateur, l’hyperviseur est nécessaire pour activer les fonctionnalités de sécurité basés sur la virtualisation qui isolent les enclaves VBS qui exécutent des requêtes Always Encrypted. 

   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All 
   ```

2. Redémarrez votre ordinateur lorsque vous êtes invité à terminer l’installation d’Hyper-V.
3. Générer une clé d’hôte unique et exporter la clé publique qui en résulte dans un fichier. 

   ```powershell
   Set-HgsClientHostKey 
   mkdir C:\artifacts 
   Get-HgsClientHostKey -Path C:\artifacts\$env:computername.cer 
   ```
   
   Vous pouvez également spécifier une empreinte numérique si vous souhaitez utiliser votre propre certificat. 
   Cela peut être utile si vous souhaitez partager un certificat sur plusieurs ordinateurs, ou utiliser un certificat lié à un module de plateforme sécurisée ou d’un module HSM. Voici un exemple de création d’un certificat lié à du module de plateforme sécurisée (ce qui empêche de générer la clé privée volé et utilisé sur un autre ordinateur et nécessite un TPM 1.2) :

   ```powershell
   $tpmBoundCert = New-SelfSignedCertificate -Subject “Host Key Attestation ($env:computername)” -Provider “Microsoft Platform Crypto Provider”
   Set-HgsClientHostKey -Thumbprint $tpmBoundCert.Thumbprint
   ```

4. Copiez la clé d’hôte pour le Service Guardian hôte. 
5. Enregistrez la clé d’hôte avec n’importe quel nœud SGH, à l’aide d’un nom approprié et le chemin d’accès pour votre environnement : 

   ```powershell
   Add-HgsAttestationHostKey -Name 'ServerA' -Path C:\temp\ServerA.cer 
   ``` 

6. Votre premier serveur est maintenant prêt à attester ! 
   Sur l’ordinateur hôte, exécutez la commande suivante pour lui indiquer où d’attester (modifier que le nom DNS à celle de votre cluster SGH, en général, vous utiliserez le nom du Service HGS associée au nom de domaine SGH). 
   Si vous recevez une erreur HostUnreachable, garantir la résoudre un test ping sur les noms DNS de vos serveurs SGH.    

   ```powershell
   Set-HgsClientConfiguration -AttestationServerUrl http://hgs.bastion.local/Attestation -KeyProtectionServerUrl http://hgs.bastion.local/KeyProtection/  
   ```

7. Le résultat de la commande ci-dessus doit indiquer que AttestationStatus = réussite. 
   Si vous obtenez une erreur HostUnreachable, cela signifie que votre ordinateur hôte ne peut pas communiquer avec SGH. 
   Vérifiez que la résolution DNS est configurée entre l’ordinateur hôte et les serveurs SGH et que vous pouvez solliciter les serveurs. 
   Une erreur UnauthorizedHost indique que la clé publique n’est pas inscrit avec le serveur SGH, répétez les étapes 4 et 5 pour résoudre l’erreur. 
   Si tout le reste échoue, exécutez Clear-HgsClientHostKey et répétez les étapes 3 à 6.   

8. Répétez les étapes 1 à 7 pour chaque serveur qui exécute SQL Server Always Encrypted à l’aide des enclaves VBS.     


