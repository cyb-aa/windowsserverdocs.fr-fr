---
title: Résoudre les problèmes liés à VPN Toujours actif (AlwaysOn)
description: Cette rubrique fournit des instructions pour vérifier et résolution des problèmes de déploiement toujours actif (AlwaysOn) dans Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 4d08164e-3cc8-44e5-a319-9671e1ac294a
ms.localizationpriority: medium
ms.date: 06/11/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 47d22d566407f45fe6ac78931ffea7b5b2854a1c
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067156"
---
# Résoudre les problèmes liés à VPN Toujours actif (AlwaysOn) 

>S’applique à: Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

Si votre programme d’installation toujours actif (AlwaysOn) ne parvient pas à connecter les clients à votre réseau interne, la cause est probablement un certificat VPN non valide, des stratégies NPS incorrectes ou des problèmes avec les scripts de déploiement du client ou dans le routage et d’accès à distance. La première étape de résolution des problèmes et le test de votre connexion VPN consiste à comprendre les principaux composants de l’infrastructure toujours actif (AlwaysOn). 

Vous pouvez résoudre les problèmes de connexion de plusieurs façons. Pour les problèmes de côté client et la résolution des problèmes généraux, les journaux des applications sur les ordinateurs clients sont extrêmement utiles. Problèmes spécifiques à l’authentification, le journal NPS sur le serveur NPS peut vous aider à déterminer la source du problème.


## Codes d’erreur


### Code d’erreur: 800

-   **Description de l’erreur.** La connexion à distance a été effectuée pas, car les tunnels VPN tentées a échoué. Le serveur VPN peut être inaccessible. Si cette connexion tente d’utiliser un tunnel IPsec/L2TP, les paramètres de sécurité requis pour IPsec négociation ne peut pas être configurée correctement.


-   **Cause possible.** Cette erreur se produit lorsque le type de tunnel VPN est **automatique** et la tentative de connexion échoue pour tous les tunnels VPN.

-   **Solutions possibles:**

    -   Si vous savez quelle tunnel à utiliser pour votre déploiement, définissez le type de VPN sur ce type de tunnel particulier côté client VPN.

    -   En effectuant une connexion VPN avec un type de tunnel particulier, votre connexion échoue malgré tout, mais il se traduit par une erreur de tunnel plus spécifique (par exemple, «GRE bloqué pour PPTP»).

    -   Cette erreur se produit également lorsque le serveur VPN ne peut pas être contacté ou d’échec de la connexion tunnel.

-   **S'assurer:**

    -   Les ports IKE (UDP ports500 et 4500) ne sont pas bloquées.

    -   Les certificats appropriés pour IKE sont présents sur le client et le serveur.

### Code d’erreur: 809

-   **Description de l’erreur.**  La connexion réseau entre votre ordinateur et le serveur VPN n’a pas pu être établie car le serveur distant ne répond pas. Il peut s’agir, car un des périphériques réseau \ (par exemple, pare-feu, NAT, routers\) entre votre ordinateur et le serveur distant n’est pas configuré pour autoriser les connexions VPN. Contactez votre administrateur ou votre fournisseur de services pour déterminer quel périphérique peut-être provoquer le problème.

-   **Cause possible.** Cette erreur est due bloqués UDP 500 ou 4500 ports sur le serveur VPN ou le pare-feu.

-   **Solutions possibles.** Assurez-vous que 4500 et UDP ports500 sont autorisés par le biais de tous les pare-feu entre le client et le serveur RRAS. 
    
    
### Code d’erreur: 812

-   **Description de l’erreur.** Impossible de se connecter à toujours actif (AlwaysOn). La connexion n’a pas pu en raison d’une stratégie configurée sur votre serveur RAS/VPN. Plus précisément, la méthode d’authentification du serveur utilisé pour vérifier votre nom d’utilisateur et mot de passe ne peut pas correspondre à la méthode d’authentification configurée dans votre profil de connexion. Contactez l’administrateur du serveur RAS et informer de cette erreur.

-   **Causes possibles:**

    -  La cause typique de cette erreur est que le serveur NPS a indiqué une condition d’authentification que le client ne peut pas répondre aux. Par exemple, le serveur NPS peut spécifier l’utilisation d’un certificat pour sécuriser la connexion PEAP, mais le client tente d’utiliser le protocole EAP-MSCHAPv2.

    -  Journal des événements 20276 est enregistré dans l’Observateur d’événements lorsque le paramètre de protocole d’authentification du serveur RRAS-in VPN ne correspond pas à celle de l’ordinateur client VPN.

-   **Solutions possibles.** Assurez-vous que votre configuration du client correspond aux conditions spécifiées sur le serveur NPS.


### Code d’erreur: 13806

-   **Description de l’erreur.** IKE n’a pas pu trouver un certificat d’ordinateur valide. Contactez votre administrateur de sécurité réseau sur l’installation d’un certificat valide dans le magasin de certificats approprié.

-   **Cause possible.** Cette erreur se produit généralement lorsque aucun certificat d’ordinateur ou de certificat racine de l’ordinateur est présent sur le serveur VPN.

-   **Solutions possibles.** Assurez-vous que les certificats décrites dans ce déploiement sont installés sur l’ordinateur client et le serveur VPN.

### Code d’erreur: 13801

-   **Description de l’erreur.** Informations d’authentification IKE ne sont pas acceptables.

-   **Causes possibles.** Cette erreur se produit généralement dans l’un des cas suivants:

    -   Le certificat d’ordinateur utilisé pour la validation IKEv2 sur le serveur RAS ne dispose pas **D’authentification serveur** sous **Utilisation avancée de la clé**.

    -   Le certificat d’ordinateur sur le serveur RAS a expiré.

    -   Le certificat racine pour valider le certificat du serveur RAS n’est pas présent sur l’ordinateur client.

    -   Le nom du serveur VPN utilisé sur l’ordinateur client ne correspond pas à la **SubjectName et pour** du certificat du serveur.

-   **Solutions possibles.** Vérifiez que le certificat de serveur inclut **L’authentification du serveur** sous **Utilisation avancée de la clé**. Vérifiez que le certificat de serveur est toujours valide. Vérifiez que l’autorité de certification utilisée est répertoriée sous **Autorités de Certification racine de confiance** sur le serveur RRAS. Vérifiez que le client VPN se connecte à l’aide du nom de domaine complet du serveur VPN présentée sur le certificat du serveur VPN.


### Code d’erreur: 0 x 80070040

-   **Description de l’erreur.** Le certificat de serveur n’a pas **d’Authentification du serveur** comme l’une de ses entrées de l’utilisation de certificats.

-   **Cause possible.** Cette erreur peut se produire si aucun certificat d’authentification serveur n’est installé sur le serveur d’accès distant.

-   **Solutions possibles.** Assurez-vous que le certificat d’ordinateur que du serveur RAS utilise pour **IKEv2** dispose **d’Authentification du serveur** comme l’une des entrées de l’utilisation du certificat.

### Code d’erreur: 0x800B0109

En règle générale, l’ordinateur du client VPN est joint au domaine basée sur Active Directory. Si vous utilisez des informations d’identification de domaine pour se connecter au serveur VPN, le certificat est installé automatiquement dans les autorités de Certification racine de confiance stocker. Toutefois, si l’ordinateur n’est pas joint au domaine ou si vous utilisez une autre chaîne de certificat, vous pouvez rencontrer ce problème.

-   **Description de l’erreur.** Une chaîne de certificats traitée mais s’est terminée par un certificat racine que le fournisseur d’approbation n’approuve pas.

-   **Cause possible.** Cette erreur peut se produire si le certificat d’autorité de certification de racine de confiance approprié n’est pas installé dans les autorités de Certification racine de confiance stocker sur l’ordinateur client.

-   **Solutions possibles.** Assurez-vous que le certificat racine est installé sur l’ordinateur client dans le magasin d’autorités de Certification racine de confiance.

## Journaux

### Journaux des applications

Les journaux des applications sur les ordinateurs clients enregistrent la plupart des détails de niveau supérieur d’événements de connexion VPN.

Recherchez des événements à partir de la source client RAS. Tous les messages d’erreur retournent le code d’erreur à la fin du message. Certains des codes d’erreur plus courants sont indiqués ci-dessous, mais une liste complète est disponible dans le [routage et les Codes d’erreur de l’accès à distance](https://msdn.microsoft.com/library/windows/desktop/bb530704.aspx).

## Journaux NPS
NPS crée et stocke les journaux de gestion des comptes de serveur NPS. Par défaut, ils sont stockés dans %SYSTEMROOT%\\System32\\Logfiles\\ dans un fichier nommé dans*XXXX.* txt, où *XXXX* est la date de création du fichier.

Par défaut, ces journaux sont au format de valeurs séparées par des virgules, mais ils n’incluent pas une ligne d’en-tête. La ligne d’en-tête est la suivante:

```
ComputerName,ServiceName,Record-Date,Record-Time,Packet-Type,User-Name,Fully-Qualified-Distinguished-Name,Called-Station-ID,Calling-Station-ID,Callback-Number,Framed-IP-Address,NAS-Identifier,NAS-IP-Address,NAS-Port,Client-Vendor,Client-IP-Address,Client-Friendly-Name,Event-Timestamp,Port-Limit,NAS-Port-Type,Connect-Info,Framed-Protocol,Service-Type,Authentication-Type,Policy-Name,Reason-Code,Class,Session-Timeout,Idle-Timeout,Termination-Action,EAP-Friendly-Name,Acct-Status-Type,Acct-Delay-Time,Acct-Input-Octets,Acct-Output-Octets,Acct-Session-Id,Acct-Authentic,Acct-Session-Time,Acct-Input-Packets,Acct-Output-Packets,Acct-Terminate-Cause,Acct-Multi-Ssn-ID,Acct-Link-Count,Acct-Interim-Interval,Tunnel-Type,Tunnel-Medium-Type,Tunnel-Client-Endpt,Tunnel-Server-Endpt,Acct-Tunnel-Conn,Tunnel-Pvt-Group-ID,Tunnel-Assignment-ID,Tunnel-Preference,MS-Acct-Auth-Type,MS-Acct-EAP-Type,MS-RAS-Version,MS-RAS-Vendor,MS-CHAP-Error,MS-CHAP-Domain,MS-MPPE-Encryption-Types,MS-MPPE-Encryption-Policy,Proxy-Policy-Name,Provider-Type,Provider-Name,Remote-Server-Address,MS-RAS-Client-Name,MS-RAS-Client-Version
```

Si vous collez cette ligne d’en-tête en tant que la première ligne du fichier journal, puis importer le fichier dans Microsoft Excel, les colonnes seront identifiées correctement.

Les journaux NPS peuvent être utiles pour diagnostiquer les problèmes liés à la stratégie. Pour plus d’informations sur les journaux NPS, voir [Interpréter les fichiers journaux NPS base de données de Format](https://technet.microsoft.com/library/cc771748.aspx).

## Problèmes de script VPN_Profile.ps1
Les problèmes les plus courants lorsque vous exécutez manuellement le script VPN_ Profile.ps1 sont les suivantes:

- Utiliser un outil de connexion à distance?  Assurez-vous que ne pas d’utiliser RDP ou une autre méthode de connexion à distance comme il mess avec la détection d’ouverture de session utilisateur.

- L’utilisateur n’est un administrateur de l’ordinateur local?  Assurez-vous que tout en exécutant le script VPN_Profile.ps1, l’utilisateur dispose des privilèges d’administrateur.

- Vous disposez d’autres fonctionnalités de sécurité de PowerShell activées? Assurez-vous que la stratégie d’exécution PowerShell ne bloque pas le script. Vous pouvez envisager la désactivation de mode de langage limitée, si activé, avant d’exécuter le script. Vous pouvez activer le mode de langage contraint une fois que le script se termine correctement.

## Problèmes de connexion client toujours actif (AlwaysOn)
Une configuration incorrecte petite peut provoquer l’échec de connexion client et peut être difficile pour trouver la cause.  Un client toujours actif (AlwaysOn) passe par plusieurs étapes avant d’établir une connexion. Lors de la résolution des problèmes de connexion client, passer par le processus d’élimination par ce qui suit:


1. L’ordinateur de modèle en externe est connecté? Une analyse **whatismyip** doit indiquer une adresse IP publique qui n’appartient pas à vous.

2. Peut résoudre le nom du serveur VPN/accès à distance à une adresse IP? Dans le **Panneau** \> **réseau** et **Internet** \> **Connexions réseau**, ouvrez les propriétés de votre profil VPN. La valeur dans l’onglet **Général** doit être publiquement peut être résolue via DNS.

3. Peut accéder le serveur VPN à partir d’un réseau externe? Envisagez ouvrant contrôle Message ICMP (Internet Protocol) à l’interface externe et en utilisant la commande ping à partir du client distant. Après une commande ping est réussie, vous pouvez supprimer la ICMP autoriser la règle.

4. Avez-vous les cartes réseau internes et externes sur le serveur VPN configuré correctement? Ils sont dans des sous-réseaux différents? La carte réseau externe se connecte à l’interface appropriée sur votre pare-feu?

5. Sont UDP 500 et 4500 ports ouvert à partir du client à l’interface externe du serveur VPN? Vérifiez le pare-feu du client, le serveur pare-feu et n’importe quel pare-feu matériels. IPSEC utilise marque tel est le cas 500, de port UDP Assurez-vous que vous n’avez pas IPEC désactivé ou bloqué n’importe où.

7. Validation du certificat ne réussit pas? Vérifiez que le serveur NPS dispose d’un certificat d’authentification serveur qui peut effectuer la maintenance demandes IKE. Assurez-vous que vous disposez de l’IP du serveur VPN approprié spécifié en tant que serveur NPS client. Vérifiez que vous authentifiez avec PEAP et les propriétés EAP protégé doivent autoriser uniquement l’authentification avec un certificat. Vous pouvez vérifier les journaux des événements NPS pour les échecs d’authentification. Pour plus d’informations, voir [installer et configurer le serveur NPS](vpn-deploy-nps.md)

8. Vous vous connectez sont, mais n’ont pas accès au réseau de Internet/local? Vérifiez vos pools IP du serveur DHCP/VPN les problèmes de configuration.

9.  Se connectent et avoir une adresse IP interne valide mais ne pas avoir accès aux ressources locales?  Vérifiez que les clients sachent comment obtenir à ces ressources. Vous pouvez utiliser le serveur VPN pour acheminer les demandes.


## Problèmes de connexion Azure AD l’accès conditionnel

### Mince - vous ne parvenez pas à cette encore

-   **Description de l’erreur.** Lorsque la stratégie d’accès conditionnel n’est pas satisfaite, bloquer la connexion VPN, mais se connecte après l’utilisateur clique sur **X** pour fermer le message.  En cliquant sur **OK** entraîne une autre tentative d’authentification, qui se termine par un autre message _Oops_ . Ces événements sont enregistrés dans le journal des événements opérationnel AAD du client. 

-   **Cause possible.** 

    - L’utilisateur possède un certificat d’authentification client valide dans leurs certificats personnels de stockage qui n’a pas été émis par Azure AD.

    - Le profil VPN \ < TLSExtensions\ > section est manquant ou il ne contient pas le **\ < EKUName\ > Access\ conditionnel AAD < / EKUName\ > \ < EKUOID\ > 1.3.6.1.4.1.311.87 < / EKUOID\ > \ < EKUName > accès conditionnel AAD < / EKUName\ > \ < EKUOID\ > 1.3.6.1.4.1.311.87 < / EKUOID\ >** entrées. Le \ < EKUName > et \ entrées < EKUOID > indiquent le client VPN le certificat à récupérer à partir du magasin de certificats de l’utilisateur lors du passage du certificat vers le serveur VPN. Sans cela, le client VPN utilise le certificat d’authentification de Client valide se trouve dans le magasin de certificats de l’utilisateur et l’authentification réussit. 

    - Le serveur RADIUS (NPS) n’a pas été configuré pour accepter uniquement les certificats clients qui contiennent l' **Accès conditionnel AAD** OID.

-   **Solutions possibles.** Pour quitter cette boucle, procédez comme suit:

    1. Dans Windows PowerShell, exécutez l’applet de commande **Get-WmiObject** pour vider la configuration du profil VPN. 
    2. Vérifiez que le **\ < TLSExtensions >**, **\ < EKUName >**, et **\ < EKUOID >** sections existent et affiche le nom et corrects OID. 
        ```
        PS C:\> Get-WmiObject -Class MDM_VPNv2_01 -Namespace root\cimv2\mdm\dmmap

        __GENUS                 : 2
        __CLASS                 : MDM_VPNv2_01
        __SUPERCLASS            :
        __DYNASTY               : MDM_VPNv2_01
        __RELPATH               : MDM_VPNv2_01.InstanceID="AlwaysOnVPN",ParentID="./Vendor/MSFT/VPNv2"
        __PROPERTY_COUNT        : 10
        __DERIVATION            : {}
        __SERVER                : DERS2
        __NAMESPACE             : root\cimv2\mdm\dmmap
        __PATH                  : \\DERS2\root\cimv2\mdm\dmmap:MDM_VPNv2_01.InstanceID="AlwaysOnVPN",ParentID="./Vendor/MSFT/VP
                                    Nv2"
        AlwaysOn                :
        ByPassForLocal          :
        DnsSuffix               :
        EdpModeId               :
        InstanceID              : AlwaysOnVPN
        LockDown                :
        ParentID                : ./Vendor/MSFT/VPNv2
        ProfileXML              : <VPNProfile><RememberCredentials>false</RememberCredentials><DeviceCompliance><Enabled>true</
                                    Enabled><Sso><Enabled>true</Enabled></Sso></DeviceCompliance><NativeProfile><Servers>derras2.
                                    corp.deverett.info;derras2.corp.deverett.info</Servers><RoutingPolicyType>ForceTunnel</Routin
                                    gPolicyType><NativeProtocolType>Ikev2</NativeProtocolType><Authentication><UserMethod>Eap</Us
                                    erMethod><MachineMethod>Eap</MachineMethod><Eap><Configuration><EapHostConfig
                                    xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><EapMethod><Type
                                    xmlns="https://www.microsoft.com/provisioning/EapCommon">25</Type><VendorId
                                    xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorId><VendorType
                                    xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorType><AuthorId
                                    xmlns="https://www.microsoft.com/provisioning/EapCommon">0</AuthorId></EapMethod><Config
                                    xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><Eap xmlns="https://www.microsoft.
                                    com/provisioning/BaseEapConnectionPropertiesV1"><Type>25</Type><EapType xmlns="https://www.mic
                                    rosoft.com/provisioning/MsPeapConnectionPropertiesV1"><ServerValidation><DisableUserPromptFor
                                    ServerValidation>true</DisableUserPromptForServerValidation><ServerNames></ServerNames></Serv
                                    erValidation><FastReconnect>true</FastReconnect><InnerEapOptional>false</InnerEapOptional><Ea
                                    p xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>13</Type>
                                    <EapType xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV1"><Credenti
                                    alsSource><CertificateStore><SimpleCertSelection>true</SimpleCertSelection></CertificateStore
                                    ></CredentialsSource><ServerValidation><DisableUserPromptForServerValidation>true</DisableUse
                                    rPromptForServerValidation><ServerNames></ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b
                                    1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><DifferentUsername>fal
                                    se</DifferentUsername><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/E
                                    apTlsConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://ww
                                    w.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">false</AcceptServerName><TLSExtens
                                    ions
                                    xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2"><FilteringInfo xml
                                    ns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV3"><EKUMapping><EKUMap><
                                    EKUName>AAD Conditional
                                    Access</EKUName><EKUOID>1.3.6.1.4.1.311.87</EKUOID></EKUMap></EKUMapping><ClientAuthEKUList
                                    Enabled="true"><EKUMapInList><EKUName>AAD Conditional Access</EKUName></EKUMapInList></Client
                                    AuthEKUList></FilteringInfo></TLSExtensions></EapType></Eap><EnableQuarantineChecks>false</En
                                    ableQuarantineChecks><RequireCryptoBinding>false</RequireCryptoBinding><PeapExtensions><Perfo
                                    rmServerValidation xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2"
                                    >false</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisionin
                                    g/MsPeapConnectionPropertiesV2">false</AcceptServerName></PeapExtensions></EapType></Eap></Co
                                    nfig></EapHostConfig></Configuration></Eap></Authentication></NativeProfile></VPNProfile>
        RememberCredentials     : False
        TrustedNetworkDetection :
        PSComputerName          : DERS2
        ```

    3. Pour déterminer s’il y a des certificats valides dans magasin de certificats de l’utilisateur, exécutez la commande **Certutil** :

       ```
       C:\>certutil -store -user My

        My "Personal"
        ================ Certificate 0 ================
        Serial Number: 32000000265259d0069fa6f205000000000026
        Issuer: CN=corp-DEDC0-CA, DC=corp, DC=deverett, DC=info
         NotBefore: 12/8/2017 8:07 PM
         NotAfter: 12/8/2018 8:07 PM
        Subject: E=winfed@deverett.info, CN=WinFed, OU=Users, OU=Corp, DC=corp, DC=deverett, DC=info
        Certificate Template Name (Certificate Type): User
        Non-root Certificate
        Template: User
        Cert Hash(sha1): a50337ab015d5612b7dc4c1e759d201e74cc2a93
          Key Container = a890fd7fbbfc072f8fe045e680c501cf_5834bfa9-1c4a-44a8-a128-c2267f712336
          Simple container name: te-User-c7bcc4bd-0498-4411-af44-da2257f54387
          Provider = Microsoft Enhanced Cryptographic Provider v1.0
        Encryption test passed
        
        ================ Certificate 1 ================
        Serial Number: 367fbdd7e6e4103dec9b91f93959ac56
        Issuer: CN=Microsoft VPN root CA gen 1
         NotBefore: 12/8/2017 6:24 PM
         NotAfter: 12/8/2017 7:29 PM
        Subject: CN=WinFed@deverett.info
        Non-root Certificate
        Cert Hash(sha1): 37378a1b06dcef1b4d4753f7d21e4f20b18fbfec
          Key Container = 31685cae-af6f-48fb-ac37-845c69b4c097
          Unique container name: bf4097e20d4480b8d6ebc139c9360f02_5834bfa9-1c4a-44a8-a128-c2267f712336
          Provider = Microsoft Software Key Storage Provider
        Private key is NOT exportable
        Encryption test passed
       ```
       >[!NOTE]
       >Si un certificat d’émetteur **CN = génération d’autorité de certification racine VPN Microsoft 1** est présent dans le magasin personnel de l’utilisateur, mais l’utilisateur obtenu l’accès en cliquant sur **X** pour fermer le message Oops, collecter des journaux des événements de CAPI2 pour vérifier le certificat utilisé pour authentifier a un certificat d’authentification Client valide qui n’a pas été publié à partir de l’autorité de certification racine VPN Microsoft.

   4. Si un certificat d’authentification de Client valide existe dans le magasin personnel de l’utilisateur, la connexion échoue (telle qu’elle ne devrait) une fois que l’utilisateur clique sur le **X** et si le **\ < TLSExtensions >**, **\ < EKUName >**, et **\ < EKUOID >** les sections existent et contiennent les informations correctes.<p>Le _un certificat est introuvable qui peut être utilisé avec le protocole d’authentification Extensible._ message d’erreur s’affiche.

### Impossible de supprimer le certificat dans le panneau de connectivité VPN

-   **Description de l’erreur.** Certificats sur le panneau de connectivité VPN ne peut pas être supprimés.

-   **Cause possible.** Le certificat est défini sur **principal**.

-   **Solutions possibles.** 

    1. Dans le panneau de connectivité VPN, sélectionnez le certificat.
    2. Sous **principal**, sélectionnez **non** et cliquez sur **Enregistrer**.
    3. Dans le panneau de connectivité VPN, sélectionnez le certificat à nouveau.
    4. Cliquez sur **Supprimer**.


---
