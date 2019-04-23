---
title: Résoudre les problèmes liés au VPN Toujours actif (AlwaysOn)
description: Cette rubrique fournit des instructions de la vérification et résolution des problèmes de déploiement VPN Always On dans Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 4d08164e-3cc8-44e5-a319-9671e1ac294a
ms.localizationpriority: medium
ms.date: 06/11/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 47d22d566407f45fe6ac78931ffea7b5b2854a1c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839090"
---
# <a name="troubleshoot-always-on-vpn"></a>Résoudre les problèmes liés au VPN Toujours actif (AlwaysOn) 

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

Si votre configuration VPN Always On ne peut pas connecter des clients à votre réseau interne, la cause est probablement un certificat VPN non valide, les stratégies NPS incorrects ou des problèmes avec les scripts de déploiement du client ou dans Routage et accès distant. La première étape de résolution des problèmes et de test de votre connexion VPN consiste à comprendre les principaux composants de l’infrastructure VPN Always On. 

Vous pouvez résoudre les problèmes de connexion de plusieurs façons. Pour les problèmes côté client et le dépannage général, les journaux des applications sur les ordinateurs clients sont très utiles. Pour les problèmes spécifiques à l’authentification, le journal de serveur NPS sur le serveur NPS peut vous aider à déterminer la source du problème.


## <a name="error-codes"></a>Codes d’erreur


### <a name="error-code-800"></a>Code d'erreur : 800

-   **Description de l’erreur.** La connexion à distance n’a pas a été effectuée, car les tunnels VPN essayés a échoué. Le serveur VPN peut être inaccessible. Si cette connexion tente d’utiliser un tunnel L2TP/IPsec, les paramètres de sécurité requis pour IPsec négociation ne peut pas être configurée correctement.


-   **Cause possible.** Cette erreur se produit lorsque le type de tunnel VPN est **automatique** et la tentative de connexion échoue pour tous les tunnels VPN.

-   **Solutions possibles :**

    -   Si vous savez quel tunnel à utiliser pour votre déploiement, définissez le type de VPN pour ce type de tunnel particulier sur le client VPN.

    -   En effectuant une connexion VPN avec un type de tunnel particulier, votre connexion échoue toujours, mais cela entraînerait une erreur plus spécifique au tunnel (par exemple, « GRE bloqué pour PPTP »).

    -   Cette erreur se produit également lorsque le serveur VPN ne peut pas être atteinte ou la connexion de tunnel échoue.

-   **S'assurer :**

    -   Les ports IKE (ports UDP 500 et 4500) ne sont pas bloqués.

    -   Les certificats appropriés pour IKE sont présents sur le client et le serveur.

### <a name="error-code-809"></a>Code d'erreur : 809

-   **Description de l’erreur.**  La connexion réseau entre votre ordinateur et le serveur VPN n’a pas pu être établie car le serveur distant ne répond pas. Cela peut signifier qu’un des périphériques réseau \(par exemple, les routeurs de pare-feux, NAT,\) entre votre ordinateur et le serveur distant n’est pas configuré pour autoriser les connexions VPN. Veuillez contacter votre administrateur ou votre fournisseur de services pour déterminer quel périphérique susceptible de causer le problème.

-   **Cause possible.** Cette erreur est due en bloqué UDP 500 ou 4500 ports sur le serveur VPN ou le pare-feu.

-   **Solution possible.** Vérifiez que les ports UDP 500 et 4500 sont autorisées par tous les pare-feu entre le client et le serveur RRAS. 
    
    
### <a name="error-code-812"></a>Code d'erreur : 812

-   **Description de l’erreur.** Impossible de se connecter au VPN Always On. La connexion a été empêchée en raison d’une stratégie configurée sur votre serveur RAS/VPN. Plus précisément, le serveur de la méthode d’authentification utilisé pour vérifier votre nom d’utilisateur et mot de passe ne peut ne pas correspondre à la méthode d’authentification configurée dans votre profil de connexion. Contactez l’administrateur du serveur RAS et informer de cette erreur.

-   **Causes possibles :**

    -  La cause classique de cette erreur est que le serveur NPS a spécifié une condition d’authentification qui ne satisfait pas le client. Par exemple, le serveur NPS peut spécifier l’utilisation d’un certificat pour sécuriser la connexion PEAP, mais le client tente d’utiliser EAP-MSCHAPv2.

    -  Journal des événements 20276 est consignée dans l’Observateur d’événements lorsque le paramètre de protocole d’authentification du serveur VPN basés sur RRAS ne correspond pas à celle de l’ordinateur client VPN.

-   **Solution possible.** Assurez-vous que votre configuration du client correspond aux conditions qui sont spécifiées sur le serveur NPS.


### <a name="error-code-13806"></a>Code d'erreur : 13806

-   **Description de l’erreur.** IKE Impossible de trouver un certificat d’ordinateur valide. Contactez votre administrateur de sécurité réseau sur l’installation d’un certificat valide dans le magasin de certificats approprié.

-   **Cause possible.** Cette erreur se produit généralement lorsque aucun certificat d’ordinateur ou le certificat racine de l’ordinateur est présent sur le serveur VPN.

-   **Solution possible.** Assurez-vous que les certificats décrites dans ce déploiement sont installés sur l’ordinateur client et le serveur VPN.

### <a name="error-code-13801"></a>Code d'erreur : 13801

-   **Description de l’erreur.** Informations d’identification de l’authentification IKE sont inacceptables.

-   **Causes possibles.** Cette erreur se produit généralement dans un des cas suivants :

    -   Le certificat d’ordinateur utilisé pour la validation IKEv2 sur le serveur RAS n’a pas **l’authentification du serveur** sous **Enhanced Key Usage**.

    -   Le certificat d’ordinateur sur le serveur RAS a expiré.

    -   Le certificat racine pour valider le certificat de serveur RAS n’est pas présent sur l’ordinateur client.

    -   Le nom du serveur VPN utilisé sur l’ordinateur client ne correspond pas à la **subjectName** du certificat du serveur.

-   **Solution possible.** Vérifiez que le certificat de serveur inclut **l’authentification du serveur** sous **Enhanced Key Usage**. Vérifiez que le certificat de serveur est toujours valide. Vérifiez que l’autorité de certification utilisée est répertoriée sous **Trusted Root Certification Authorities** sur le serveur RRAS. Vérifiez que le client VPN se connecte en utilisant le nom de domaine complet du serveur VPN s’affiche sur le certificat du serveur VPN.


### <a name="error-code-0x80070040"></a>Code d'erreur : 0x80070040

-   **Description de l’erreur.** Le certificat de serveur n’a pas **l’authentification du serveur** comme l’un de ses entrées de l’utilisation du certificat.

-   **Cause possible.** Cette erreur peut se produire si aucun certificat d’authentification serveur n’est installé sur le serveur RAS.

-   **Solution possible.** Assurez-vous que le certificat d’ordinateur au serveur RAS utilise pour **IKEv2** a **l’authentification du serveur** comme l’une des entrées de l’utilisation du certificat.

### <a name="error-code-0x800b0109"></a>Code d'erreur : 0x800B0109

En règle générale, l’ordinateur du client VPN est joint au domaine basée sur Active Directory. Si vous utilisez des informations d’identification de domaine pour ouvrir une session le serveur VPN, le certificat est automatiquement installé dans les autorités de Certification racine de confiance stocker. Toutefois, si l’ordinateur n’est pas joint au domaine ou si vous utilisez une autre chaîne de certificat, vous risquez de rencontrer ce problème.

-   **Description de l’erreur.** Une chaîne de certificat traitée mais s’est terminée par un certificat racine non approuvé par le fournisseur d’approbation.

-   **Cause possible.** Cette erreur peut se produire si le certificat d’autorité de certification de racine de confiance approprié n’est pas installé dans les autorités de Certification racine approuvée stocker sur l’ordinateur client.

-   **Solution possible.** Assurez-vous que le certificat racine est installé sur l’ordinateur client dans le magasin d’autorités de Certification racine de confiance.

## <a name="logs"></a>Journaux

### <a name="application-logs"></a>Journaux d’application

Les journaux des applications sur les ordinateurs clients enregistrent la plupart des détails de niveau supérieurs d’événements de connexion VPN.

Recherchez des événements à partir de la source client RAS. Tous les messages d’erreur renvoyer le code d’erreur à la fin du message. Certains des codes d’erreur plus courants sont décrits ci-dessous, mais une liste complète est disponible dans [routage et accès à distance les Codes d’erreur](https://msdn.microsoft.com/library/windows/desktop/bb530704.aspx).

## <a name="nps-logs"></a>Journaux de serveur NPS
NPS crée et stocke les journaux de gestion de serveur NPS. Par défaut, ceux-ci sont stockés dans % SystemRoot%\\System32\\Logfiles\\ dans un fichier nommé dans*XXXX.* txt, où *XXXX* correspond à la date de création du fichier.

Par défaut, ces journaux sont au format de valeurs séparées par des virgules, mais ils n’incluent pas une ligne d’en-tête. La ligne d’en-tête est :

```
ComputerName,ServiceName,Record-Date,Record-Time,Packet-Type,User-Name,Fully-Qualified-Distinguished-Name,Called-Station-ID,Calling-Station-ID,Callback-Number,Framed-IP-Address,NAS-Identifier,NAS-IP-Address,NAS-Port,Client-Vendor,Client-IP-Address,Client-Friendly-Name,Event-Timestamp,Port-Limit,NAS-Port-Type,Connect-Info,Framed-Protocol,Service-Type,Authentication-Type,Policy-Name,Reason-Code,Class,Session-Timeout,Idle-Timeout,Termination-Action,EAP-Friendly-Name,Acct-Status-Type,Acct-Delay-Time,Acct-Input-Octets,Acct-Output-Octets,Acct-Session-Id,Acct-Authentic,Acct-Session-Time,Acct-Input-Packets,Acct-Output-Packets,Acct-Terminate-Cause,Acct-Multi-Ssn-ID,Acct-Link-Count,Acct-Interim-Interval,Tunnel-Type,Tunnel-Medium-Type,Tunnel-Client-Endpt,Tunnel-Server-Endpt,Acct-Tunnel-Conn,Tunnel-Pvt-Group-ID,Tunnel-Assignment-ID,Tunnel-Preference,MS-Acct-Auth-Type,MS-Acct-EAP-Type,MS-RAS-Version,MS-RAS-Vendor,MS-CHAP-Error,MS-CHAP-Domain,MS-MPPE-Encryption-Types,MS-MPPE-Encryption-Policy,Proxy-Policy-Name,Provider-Type,Provider-Name,Remote-Server-Address,MS-RAS-Client-Name,MS-RAS-Client-Version
```

Si vous collez cette ligne d’en-tête de la première ligne du fichier journal, puis importer le fichier dans Microsoft Excel, les colonnes seront intitule correctement.

Les journaux de serveur NPS peuvent être utiles pour diagnostiquer les problèmes liés à la stratégie. Pour plus d’informations sur les journaux de serveur NPS, consultez [interpréter des fichiers journaux NPS de base de données au Format](https://technet.microsoft.com/library/cc771748.aspx).

## <a name="vpnprofileps1-script-issues"></a>Problèmes de script VPN_Profile.ps1
Problèmes les plus courants lorsque vous exécutez manuellement le script de fichier Profile.ps1 VPN_ sont les suivantes :

- Vous utilisez un outil de connexion à distance ?  Veillez ne pas à utiliser RDP ou une autre méthode de connexion à distance comme il assainir la situation avec la détection de connexion utilisateur.

- Est l’utilisateur administrateur de l’ordinateur local ?  Assurez-vous que lors de l’exécution du script VPN_Profile.ps1 que l’utilisateur dispose des privilèges d’administrateur.

- Vous avez des fonctionnalités supplémentaires de sécurité PowerShell activées ? Assurez-vous que la stratégie d’exécution PowerShell ne bloque pas le script. Vous pouvez envisager de désactivation de mode de langage de contrainte, si activé, avant d’exécuter le script. Vous pouvez activer le mode de langage contraint une fois le script terminé avec succès.

## <a name="always-on-vpn-client-connection-issues"></a>Problèmes de connexion client VPN Always On
Une configuration incorrecte de petites peut entraîner l’échec de connexion client et peut être difficile de déterminer la cause.  Un client VPN Always On passe par plusieurs étapes avant d’établir une connexion. Lors de la résolution des problèmes de connexion client, passent par le processus d’élimination avec les éléments suivants :


1. L’ordinateur de modèle en externe est connecté ? Un **whatismyip** analyse doit afficher une adresse IP publique qui n’appartient pas à vous.

2. Vous résoudre le nom du serveur accès distant/VPN à une adresse IP ? Dans **le panneau de configuration** \> **réseau** et **Internet** \> **connexions réseau**, ouvrez les propriétés pour votre profil VPN. La valeur dans le **général** onglet doit être résolu publiquement via DNS.

3. Pouvez-vous accéder au serveur VPN à partir d’un réseau externe ? Envisagez ouvrant contrôle Message ICMP (Internet Protocol) à l’interface externe et en utilisant la commande ping à partir du client distant. Une fois un test ping réussi, vous pouvez supprimer le protocole ICMP règle d’autorisation.

4. Vous avez les cartes réseau internes et externes sur le serveur VPN configuré correctement ? Ils sont, dans des sous-réseaux différents ? La carte réseau externe se connecte à l’interface correcte sur votre pare-feu ?

5. Sont UDP 500 et 4500 ports ouverts à partir du client à l’interface externe du serveur VPN ? Vérifiez le pare-feu du client, les pare-feu de serveur et les éventuels pare-feu matériel. IPSEC utilise make c’est le cas de UDP port 500, bien sûr que vous n’avez pas IPEC désactivé ou bloqué n’importe où.

7. Validation du certificat échoue ? Vérifiez que le serveur NPS dispose d’un certificat d’authentification de serveur qui peut traiter les demandes d’IKE. Assurez-vous que vous disposez de l’IP du serveur VPN correct est spécifié comme un client du serveur NPS. Assurez-vous que vous vous authentifiez avec PEAP et propriétés EAP protégées doivent autoriser uniquement l’authentification avec un certificat. Vous pouvez vérifier les journaux des événements NPS pour les échecs d’authentification. Pour plus d’informations, consultez [installer et configurer le serveur NPS](vpn-deploy-nps.md)

8. Vous vous connectez sont mais n’ont pas accès de réseau local/Internet ? Vérifiez vos pools d’IP de serveur DHCP/VPN pour les problèmes de configuration.

9.  Se connectent et ont une adresse IP interne valide mais ne pas avoir accès aux ressources locales ?  Vérifiez que les clients sachent comment accéder à ces ressources. Vous pouvez utiliser le serveur VPN pour acheminer les demandes.


## <a name="azure-ad-conditional-access-connection-issues"></a>Problèmes de connexion Azure AD l’accès conditionnel

### <a name="oops---you-cant-get-to-this-yet"></a>Désolé, vous ne parvenez pas à ce encore

-   **Description de l’erreur.** Lorsque la stratégie d’accès conditionnel n’est pas satisfaite, bloque la connexion VPN, mais se connecte une fois que l’utilisateur clique sur **X** pour fermer le message.  En cliquant sur **OK** entraîne une autre tentative d’authentification, qui se termine par un autre _Désolé_ message. Ces événements sont enregistrés dans le journal des événements opérationnels AAD du client. 

-   **Cause possible.** 

    - L’utilisateur a un certificat d’authentification client valide dans leur certificat personnel stocker qui n’a pas été émis par Azure AD.

    - Le profil VPN \<TLSExtensions\> section est manquant ou il ne contient pas le **\<EKUName\>accès conditionnel AAD\</EKUName\> \< EKUOID\>1.3.6.1.4.1.311.87 < / EKUOID\>\<EKUName > accès conditionnel AAD < / EKUName\>\<EKUOID\>1.3.6.1.4.1.311.87 < / EKUOID\>** entrées. Le \<EKUName > et \<EKUOID > entrées indiquent le client VPN le certificat à récupérer à partir du magasin de certificats de l’utilisateur lors du passage du certificat au serveur VPN. Sans cela, le client VPN utilise le certificat d’authentification de Client valide est dans le magasin de certificats de l’utilisateur et l’authentification réussit. 

    - Le serveur RADIUS (NPS) n’a pas été configuré pour accepter uniquement les certificats de client qui contiennent le **accès conditionnel AAD** OID.

-   **Solution possible.** Pour abandonner cette boucle, procédez comme suit :

    1. Dans Windows PowerShell, exécutez le **Get-WmiObject** applet de commande pour faire un dump la configuration du profil VPN. 
    2. Vérifiez que le  **\<TLSExtensions >**,  **\<EKUName >**, et  **\<EKUOID >** sections existent et montre la bonne nom et les OID. 
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

    3. Pour déterminer s’il existe des certificats valides dans le magasin de certificats de l’utilisateur, exécutez le **Certutil** commande :

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
       >Si un certificat d’émetteur **CN = génération d’autorité de certification racine Microsoft VPN 1** est présent dans le magasin personnel de l’utilisateur, mais l’utilisateur obtenu un accès en cliquant sur **X** pour fermer le message Oups, collecter des journaux des événements CAPI2 pour vérifier le certificat utilisé pour l’authentification a été un certificat d’authentification de Client valide qui n’a pas été émis à partir de l’autorité de certification de racine VPN Microsoft.

   4. Si un certificat d’authentification de Client valide existe dans le magasin personnel de l’utilisateur, la connexion échoue (comme il le devrait) une fois que l’utilisateur clique sur le **X** et si le  **\<TLSExtensions >**,  **\<EKUName >**, et  **\<EKUOID >** sections existe et contient les informations appropriées.<p>Le _un certificat est introuvable qui peut être utilisé avec le protocole d’authentification Extensible._ message d’erreur s’affiche.

### <a name="unable-to-delete-the-certificate-from-the-vpn-connectivity-blade"></a>Impossible de supprimer le certificat à partir du Panneau de la connectivité VPN

-   **Description de l’erreur.** Impossible de supprimer les certificats dans le panneau de la connectivité VPN.

-   **Cause possible.** Le certificat est défini sur **principal**.

-   **Solution possible.** 

    1. Dans le panneau de connectivité VPN, sélectionnez le certificat.
    2. Sous **principal**, sélectionnez **non** et cliquez sur **enregistrer**.
    3. Dans le panneau de connectivité VPN, sélectionnez à nouveau le certificat.
    4. Cliquez sur **Supprimer**.


---
