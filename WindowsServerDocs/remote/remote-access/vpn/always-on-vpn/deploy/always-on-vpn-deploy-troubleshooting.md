---
title: Résoudre les problèmes liés au VPN Toujours actif (AlwaysOn)
description: Cette rubrique fournit des instructions pour vérifier et résoudre les problèmes Always On le déploiement VPN dans Windows Server 2016.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 4d08164e-3cc8-44e5-a319-9671e1ac294a
ms.localizationpriority: medium
ms.date: 06/11/2018
ms.author: v-tea
author: Teresa-MOTIV
ms.openlocfilehash: 209567ccd88f4b20f98caecc2a13cc671ef09072
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854022"
---
# <a name="troubleshoot-always-on-vpn"></a>Résoudre les problèmes liés au VPN Toujours actif (AlwaysOn) 

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

Si votre configuration VPN Always On ne parvient pas à connecter les clients à votre réseau interne, la cause est probablement un certificat VPN non valide, des stratégies NPS incorrectes ou des problèmes liés aux scripts de déploiement du client ou au service Routage et accès distant. La première étape de dépannage et de test de votre connexion VPN consiste à comprendre les principaux composants de l’infrastructure VPN Always On. 

Vous pouvez résoudre les problèmes de connexion de plusieurs façons. Pour les problèmes liés au client et la résolution des problèmes généraux, les journaux des applications sur les ordinateurs clients sont inintéressants. Pour les problèmes spécifiques à l’authentification, le journal NPS sur le serveur NPS peut vous aider à déterminer la source du problème.

## <a name="error-codes"></a>Codes d’erreur

### <a name="error-code-800"></a>Code d’erreur : 800

- **Description de l’erreur.** La connexion à distance n’a pas été établie en raison de l’échec de la tentative des tunnels VPN. Le serveur VPN est peut-être inaccessible. Si cette connexion tente d’utiliser un tunnel L2TP/IPsec, les paramètres de sécurité requis pour la négociation IPsec ne sont peut-être pas configurés correctement.

- **Cause possible.** Cette erreur se produit lorsque le type de tunnel VPN est **automatique** et que la tentative de connexion échoue pour tous les tunnels VPN.

- **Solutions possibles :**

    - Si vous savez quel tunnel utiliser pour votre déploiement, définissez le type de VPN sur ce type de tunnel particulier côté client VPN.

    - En établissant une connexion VPN avec un type de tunnel particulier, votre connexion échouera toujours, mais cela entraînera une erreur plus spécifique au tunnel (par exemple, « GRE bloqué pour PPTP »).

    - Cette erreur se produit également lorsque le serveur VPN est inaccessible ou que la connexion du tunnel échoue.

- **S'assurer:**

    - Les ports IKE (ports UDP 500 et 4500) ne sont pas bloqués.

    - Les certificats valides pour IKE sont présents à la fois sur le client et sur le serveur.

### <a name="error-code-809"></a>Code d’erreur : 809

- **Description de l’erreur.**  La connexion réseau entre votre ordinateur et le serveur VPN n’a pas pu être établie car le serveur distant ne répond pas. Cela peut être dû au fait que l’un des périphériques réseau (par exemple, pare-feu, NAT, routeurs) entre votre ordinateur et le serveur distant n’est pas configuré pour autoriser les connexions VPN. Veuillez contacter votre administrateur ou votre fournisseur de services pour déterminer l’appareil qui est à l’origine du problème.

- **Cause possible.** Cette erreur est provoquée par les ports UDP 500 ou 4500 bloqués sur le serveur VPN ou le pare-feu.

- **Solution possible.** Assurez-vous que les ports UDP 500 et 4500 sont autorisés via tous les pare-feu entre le client et le serveur RRAS.

### <a name="error-code-812"></a>Code d’erreur : 812

- **Description de l’erreur.** Impossible de se connecter à Always On VPN. La connexion a été empêchée en raison d’une stratégie configurée sur votre serveur RAS/VPN. Plus précisément, la méthode d’authentification utilisée par le serveur pour vérifier votre nom d’utilisateur et votre mot de passe peut ne pas correspondre à la méthode d’authentification configurée dans votre profil de connexion. Veuillez contacter l’administrateur du serveur RAS et lui notifier cette erreur.

- **Causes possibles :**

    - La cause courante de cette erreur est que le serveur NPS a spécifié une condition d’authentification que le client ne peut pas satisfaire. Par exemple, le serveur NPS peut spécifier l’utilisation d’un certificat pour sécuriser la connexion PEAP, mais le client tente d’utiliser EAP-MSCHAPv2.

    - Le journal des événements 20276 est enregistré dans l’observateur d’événements lorsque le paramètre du protocole d’authentification de serveur VPN basé sur RRAS ne correspond pas à celui de l’ordinateur client VPN.

- **Solution possible.** Assurez-vous que la configuration de votre client correspond aux conditions spécifiées sur le serveur NPS.

### <a name="error-code-13806"></a>Code d’erreur : 13806

- **Description de l’erreur.** IKE n’a pas trouvé de certificat d’ordinateur valide. Contactez votre administrateur de sécurité réseau pour installer un certificat valide dans le magasin de certificats approprié.

- **Cause possible.** Cette erreur se produit généralement quand aucun certificat d’ordinateur ou certificat d’ordinateur racine n’est présent sur le serveur VPN.

- **Solution possible.** Assurez-vous que les certificats décrits dans ce déploiement sont installés sur l’ordinateur client et le serveur VPN.

### <a name="error-code-13801"></a>Code d’erreur : 13801

- **Description de l’erreur.** Les informations d’authentification IKE ne sont pas acceptables.

- **Causes possibles.** Cette erreur se produit généralement dans l’un des cas suivants :

    - Le certificat d’ordinateur utilisé pour la validation IKEv2 sur le serveur RAS ne dispose pas de **l’authentification du serveur** en cas d' **utilisation améliorée**de la clé.

    - Le certificat de l’ordinateur sur le serveur RAS est arrivé à expiration.

    - Le certificat racine pour valider le certificat de serveur RAS n’est pas présent sur l’ordinateur client.

    - Le nom du serveur VPN utilisé sur l’ordinateur client ne correspond pas au **SubjectName** du certificat de serveur.

- **Solution possible.** Vérifiez que le certificat de serveur comprend l' **authentification du serveur** en cas d' **utilisation améliorée**de la clé. Vérifiez que le certificat de serveur est toujours valide. Vérifiez que l’autorité de certification utilisée est indiquée sous **autorités de certification racines de confiance** sur le serveur RRAS. Vérifiez que le client VPN se connecte en utilisant le nom de domaine complet du serveur VPN comme indiqué sur le certificat du serveur VPN.

### <a name="error-code-0x80070040"></a>Code d’erreur : 0x80070040

- **Description de l’erreur.** Le certificat de serveur n’a pas **d’authentification de serveur** comme l’une de ses entrées d’utilisation de certificat.

- **Cause possible.** Cette erreur peut se produire si aucun certificat d’authentification serveur n’est installé sur le serveur RAS.

- **Solution possible.** Assurez-vous que le certificat d’ordinateur utilisé par le serveur RAS pour **IKEv2** utilise l' **authentification serveur** comme l’une des entrées d’utilisation de certificat.

### <a name="error-code-0x800b0109"></a>Code d’erreur : 0x800B0109

En règle générale, l’ordinateur client VPN est joint au domaine Active Directory. Si vous utilisez des informations d’identification de domaine pour ouvrir une session sur le serveur VPN, le certificat est automatiquement installé dans le magasin autorités de certification racines de confiance. Toutefois, si l’ordinateur n’est pas joint au domaine ou si vous utilisez une autre chaîne de certificats, vous pouvez rencontrer ce problème.

- **Description de l’erreur.** Une chaîne de certificats a été traitée mais s’est terminée dans un certificat racine que le fournisseur d’approbation n’approuve pas.

- **Cause possible.** Cette erreur peut se produire si le certificat d’autorité de certification racine de confiance approprié n’est pas installé dans le magasin autorités de certification racines de confiance sur l’ordinateur client.

- **Solution possible.** Assurez-vous que le certificat racine est installé sur l’ordinateur client dans le magasin autorités de certification racines de confiance.

## <a name="logs"></a>Journaux

### <a name="application-logs"></a>Journaux d’application

Les journaux des applications sur les ordinateurs clients enregistrent la plupart des détails de niveau supérieur des événements de connexion VPN.

Recherchez les événements à partir de la source RasClient. Tous les messages d’erreur retournent le code d’erreur à la fin du message. Certains des codes d’erreur les plus courants sont détaillés ci-dessous, mais une liste complète est disponible dans les [codes d’erreur de routage et d’accès à distance](https://msdn.microsoft.com/library/windows/desktop/bb530704.aspx).

## <a name="nps-logs"></a>Journaux NPS

NPS crée et stocke les journaux de gestion des comptes NPS. Par défaut, ceux-ci sont stockés dans% SYSTEMROOT%\\system32\\LogFiles\\ dans un fichier nommé*xxxx*. txt, où *xxxx* correspond à la date à laquelle le fichier a été créé.

Par défaut, ces journaux sont au format de valeurs séparées par des virgules, mais ils n’incluent pas de ligne d’en-tête. La ligne d’en-tête est :

```
ComputerName,ServiceName,Record-Date,Record-Time,Packet-Type,User-Name,Fully-Qualified-Distinguished-Name,Called-Station-ID,Calling-Station-ID,Callback-Number,Framed-IP-Address,NAS-Identifier,NAS-IP-Address,NAS-Port,Client-Vendor,Client-IP-Address,Client-Friendly-Name,Event-Timestamp,Port-Limit,NAS-Port-Type,Connect-Info,Framed-Protocol,Service-Type,Authentication-Type,Policy-Name,Reason-Code,Class,Session-Timeout,Idle-Timeout,Termination-Action,EAP-Friendly-Name,Acct-Status-Type,Acct-Delay-Time,Acct-Input-Octets,Acct-Output-Octets,Acct-Session-Id,Acct-Authentic,Acct-Session-Time,Acct-Input-Packets,Acct-Output-Packets,Acct-Terminate-Cause,Acct-Multi-Ssn-ID,Acct-Link-Count,Acct-Interim-Interval,Tunnel-Type,Tunnel-Medium-Type,Tunnel-Client-Endpt,Tunnel-Server-Endpt,Acct-Tunnel-Conn,Tunnel-Pvt-Group-ID,Tunnel-Assignment-ID,Tunnel-Preference,MS-Acct-Auth-Type,MS-Acct-EAP-Type,MS-RAS-Version,MS-RAS-Vendor,MS-CHAP-Error,MS-CHAP-Domain,MS-MPPE-Encryption-Types,MS-MPPE-Encryption-Policy,Proxy-Policy-Name,Provider-Type,Provider-Name,Remote-Server-Address,MS-RAS-Client-Name,MS-RAS-Client-Version
```

Si vous collez cette ligne d’en-tête en tant que première ligne du fichier journal, puis importez le fichier dans Microsoft Excel, les colonnes seront correctement étiquetées.

Les journaux NPS peuvent être utiles pour diagnostiquer les problèmes liés aux stratégies. Pour plus d’informations sur les journaux NPS, consultez [interpréter les fichiers journaux au format de base de données NPS](https://technet.microsoft.com/library/cc771748.aspx).

## <a name="vpn_profileps1-script-issues"></a>Problèmes liés aux scripts VPN_Profile. ps1

Les problèmes les plus courants lors de l’exécution manuelle du script VPN_ Profile. ps1 sont les suivants :

- Utilisez-vous un outil de connexion à distance ?  Veillez à ne pas utiliser RDP ou une autre méthode de connexion à distance, car elle est utilisée pour la détection des connexions utilisateur.

- L’utilisateur est-il un administrateur de cet ordinateur local ?  Assurez-vous que lors de l’exécution du script VPN_Profile. ps1, l’utilisateur dispose de privilèges d’administrateur.

- Avez-vous activé des fonctionnalités de sécurité PowerShell supplémentaires ? Assurez-vous que la stratégie d’exécution de PowerShell ne bloque pas le script. Vous pouvez envisager de désactiver le mode de langage restreint, s’il est activé, avant d’exécuter le script. Vous pouvez activer le mode de langage restreint une fois que le script s’est terminé avec succès.

## <a name="always-on-vpn-client-connection-issues"></a>Always On les problèmes de connexion du client VPN

Une petite configuration insuffisante peut provoquer l’échec de la connexion du client et peut être difficile à trouver.  Un client VPN Always On passe par plusieurs étapes avant d’établir une connexion. Lors du dépannage des problèmes de connexion du client, suivez le processus d’élimination avec les éléments suivants :

1. L’ordinateur de modèle est-il connecté en externe ? Une analyse **whatismyip** doit afficher une adresse IP publique qui ne vous appartient pas.

2. Pouvez-vous résoudre le nom du serveur d’accès à distance/VPN en adresse IP ? Dans le **panneau de configuration** > connexions réseau et **Internet** > **réseau**, ouvrez les propriétés de votre profil VPN. **Network** La valeur de l’onglet **général** doit pouvoir être résolue publiquement via DNS.

3. Pouvez-vous accéder au serveur VPN à partir d’un réseau externe ? Envisagez d’ouvrir le protocole ICMP (Internet Control Message Protocol) sur l’interface externe et d’exécuter une commande ping sur le nom à partir du client distant. Une fois le test ping réussi, vous pouvez supprimer la règle d’autorisation ICMP.

4. Les cartes réseau internes et externes sur le serveur VPN sont-elles configurées correctement ? Sont-elles dans des sous-réseaux différents ? La carte réseau externe se connecte-t-elle à l’interface appropriée sur votre pare-feu ?

5. Les ports UDP 500 et 4500 sont-ils ouverts à partir du client vers l’interface externe du serveur VPN ? Vérifiez le pare-feu du client, le pare-feu du serveur et tous les pare-feu matériels. IPSEC utilise le port UDP 500. Assurez-vous que vous n’avez pas de IPEC désactivé ou bloqué n’importe où.

6. La validation du certificat échoue-t-elle ? Vérifiez que le serveur NPS possède un certificat d’authentification serveur qui peut traiter les demandes IKE. Vérifiez que vous disposez de l’adresse IP de serveur VPN correcte spécifiée comme client NPS. Assurez-vous que vous vous authentifiez avec PEAP et que les propriétés EAP protégées doivent uniquement autoriser l’authentification avec un certificat. Vous pouvez rechercher les échecs d’authentification dans les journaux des événements NPS. Pour plus d’informations, consultez [installer et configurer le serveur NPS](vpn-deploy-nps.md) .

7. Êtes-vous connecté, mais vous ne disposez pas d’un accès au réseau local/Internet ? Recherchez les problèmes de configuration dans les pools d’adresses IP de votre serveur DHCP/VPN.

8. Connectez-vous et disposez d’une adresse IP interne valide, mais vous n’avez pas accès aux ressources locales ?  Vérifiez que les clients savent comment accéder à ces ressources. Vous pouvez utiliser le serveur VPN pour acheminer les demandes.

## <a name="azure-ad-conditional-access-connection-issues"></a>Azure AD les problèmes de connexion d’accès conditionnel

### <a name="oops---you-cant-get-to-this-yet"></a>Désolé, vous ne pouvez pas encore y accéder

- **Description de l’erreur.** Lorsque la stratégie d’accès conditionnel n’est pas satisfaite, bloque la connexion VPN, mais se connecte après que l’utilisateur a sélectionné **X** pour fermer le message.  La sélection de **OK** entraîne une autre tentative d’authentification, qui se termine par un autre message « Désolé ». Ces événements sont enregistrés dans le journal des événements opérationnels AAD du client.

- **Cause possible**

  - L’utilisateur dispose d’un certificat d’authentification client valide dans le magasin de certificats personnel qui n’a pas été émis par Azure AD.

  - La section du profil VPN \<la\> TLSExtensions est manquante ou ne contient pas les\<EKUName\>\<\>/EKUName \<\>1.3.6.1.4.1.311.87 </EKUOID\>**EKUName \<>/EKUName** < Azure\>\<\>EKUOID < 1.3.6.1.4.1.311.87\>/EKUOID. Les entrées \<EKUName > et \<EKUOID > indiquent au client VPN quel certificat récupérer dans le magasin de certificats de l’utilisateur lors de la transmission du certificat au serveur VPN. Sans cela, le client VPN utilise le certificat d’authentification client valide qui se trouve dans le magasin de certificats de l’utilisateur et l’authentification s’effectue correctement. 

  - Le serveur RADIUS (NPS) n’a pas été configuré pour accepter uniquement les certificats clients qui contiennent l’OID d' **accès conditionnel AAD** .

- **Solution possible.** Pour échapper à cette boucle, procédez comme suit :

  1. Dans Windows PowerShell, exécutez l’applet de commande **-WmiObject** pour vider la configuration du profil VPN. 
  2. Vérifiez que les sections **\<TLSExtensions >** , **\<EKUName >** et **\<EKUOID >** existent et affichent le nom et l’OID corrects.
      
      ```powershell
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

  3. Pour déterminer s’il existe des certificats valides dans le magasin de certificats de l’utilisateur, exécutez la commande **certutil** :

     ```powershell
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
     >Si un certificat de l’émetteur **CN = autorité de certification racine de Microsoft VPN GEN 1** est présent dans le magasin personnel de l’utilisateur, mais que l’utilisateur a obtenu l’accès en sélectionnant **X** pour fermer le message, collectez les journaux des événements CAPI2 pour vérifier que le certificat utilisé pour l’authentification était un certificat d’authentification client valide qui n’a pas été émis par l’autorité de certification racine

  4. S’il existe un certificat d’authentification de client valide dans le magasin personnel de l’utilisateur, la connexion échoue (comme c’est le cas) après que l’utilisateur a sélectionné le **X** et si les sections **\<TLSExtensions >** , **\<EKUName >** et **\<EKUOID >** existent et contiennent les informations correctes.
   
     Un message d’erreur indiquant « un certificat introuvable qui peut être utilisé avec le protocole d’authentification extensible » s’affiche.

### <a name="unable-to-delete-the-certificate-from-the-vpn-connectivity-blade"></a>Impossible de supprimer le certificat du panneau connectivité VPN

- **Description de l’erreur.** Impossible de supprimer les certificats du panneau connectivité VPN.

- **Cause possible.** Le certificat est défini sur **principal**.

- **Solution possible.**

    1. Dans le panneau connectivité VPN, sélectionnez le certificat.
    2. Sous **principal**, sélectionnez **non**, puis sélectionnez **Enregistrer**.
    3. Dans le panneau connectivité VPN, sélectionnez à nouveau le certificat.
    4. Sélectionnez **supprimer**.
