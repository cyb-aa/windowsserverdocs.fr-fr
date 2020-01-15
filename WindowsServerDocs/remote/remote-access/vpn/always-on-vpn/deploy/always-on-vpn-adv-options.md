---
title: Fonctionnalités avancées de VPN Toujours actif (AlwaysOn)
description: Au-delà du scénario de déploiement fourni dans ce déploiement, vous pouvez ajouter d’autres fonctionnalités VPN avancées pour améliorer la sécurité et la disponibilité de votre connexion VPN.
ms.assetid: 51a1ee61-3ffe-4f65-b8de-ff21903e1e74
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.date: 07/24/2019
ms.author: pashort, v-tea
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 73d64cd143b7bbd13e0eb9bb5fadfbb1e7c65416
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950446"
---
# <a name="advanced-features-of-always-on-vpn"></a>Fonctionnalités avancées de Always On VPN

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Précédent :** En savoir plus sur la technologie VPN Always On](../always-on-vpn-technology-overview.md)
- [**Ensuite :** Commencer à planifier le déploiement de Always On VPN](always-on-vpn-deploy-planning.md)

Au-delà des scénarios de déploiement fournis, vous pouvez ajouter d’autres fonctionnalités VPN avancées pour améliorer la sécurité et la disponibilité de votre connexion VPN. Par exemple, le serveur VPN peut utiliser ces fonctionnalités pour s’assurer que le client qui se connecte est sain avant d’autoriser une connexion.

## <a name="high-availability"></a>Haute disponibilité

Vous trouverez ci-dessous des options supplémentaires pour la haute disponibilité.

|Option  |Description  |
|---------|---------|
|Résilience de serveur et équilibrage de charge     |Dans les environnements qui nécessitent une haute disponibilité ou qui prennent en charge un grand nombre de requêtes, vous pouvez augmenter les performances et la résilience de l’accès à distance en utilisant l’équilibrage de charge entre plusieurs serveurs qui exécutent le serveur NPS (Network Policy Server) et l’activation de. Clustering de serveurs d’accès à distance.<p>Documents associés :<ul><li>[Équilibrage de charge du serveur proxy NPS](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md)</li><li>[Déployer l’accès à distance dans un cluster](https://docs.microsoft.com/windows-server/remote/remote-access/ras/cluster/deploy-remote-access-in-cluster)</li></ul>        |
|Résilience de site géographique     |Pour la géolocalisation basée sur IP, vous pouvez utiliser des Traffic Manager globales avec DNS dans Windows Server 2016. Pour un équilibrage de charge géographique plus robuste, vous pouvez utiliser des solutions d’équilibrage de charge de serveur globales, telles que Microsoft Azure Traffic Manager.<p>Documents associés :<ul><li>[Vue d’ensemble de Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview)</li><li>[Qu’est-ce que Traffic Manager ?](https://azure.microsoft.com/services/traffic-manager)</li></ul>         |

## <a name="advanced-authentication"></a>Authentification avancée

Vous trouverez ci-dessous des options supplémentaires pour l’authentification.

|Option  |Description  |
|---------|---------|
|Windows Hello Entreprise     |Dans Windows 10, Windows Hello entreprise remplace les mots de passe en fournissant une authentification forte à deux facteurs sur les PC et les appareils mobiles. Cette authentification se compose d’un nouveau type d’informations d’identification de l’utilisateur qui est lié à un appareil et qui utilise un code confidentiel ou un numéro d’identification personnel.<p>Le client VPN Windows 10 est compatible avec Windows Hello entreprise. Une fois que l’utilisateur se connecte à l’aide d’un geste, la connexion VPN utilise le certificat Windows Hello entreprise pour l’authentification basée sur les certificats.<p>Documents associés :<ul><li>[Windows Hello Entreprise](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification)</li><li>Étude de cas technique : activation de l' [accès à distance avec Windows Hello entreprise dans Windows 10](https://msdn.microsoft.com/library/mt728163.aspx)</li></ul>         |
|Authentification multifacteur Azure (MFA)     |Azure MFA offre des versions Cloud et locales que vous pouvez intégrer avec le mécanisme d’authentification VPN Windows.<p>Pour plus d’informations sur le fonctionnement de ce mécanisme, consultez [intégrer l’authentification RADIUS à Azure serveur multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius).         |

## <a name="advanced-vpn-features"></a>Fonctionnalités VPN avancées

Vous trouverez ci-dessous des options supplémentaires pour les fonctionnalités avancées.

|Option  |Description  |
|---------|---------|
|Filtrage du trafic     |Si vous devez appliquer le choix des applications auxquelles les clients VPN peuvent accéder, vous pouvez activer les filtres de trafic VPN.<p>Pour plus d’informations, consultez [fonctionnalités de sécurité VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features).         |
|VPN déclenché par l’application     |Vous pouvez configurer des profils VPN pour qu’ils se connectent automatiquement au démarrage de certaines applications ou types d’applications.<p>Pour plus d’informations sur cette option et d’autres options de déclenchement, consultez [options de profil déclenchés automatiquement par VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile).         |
|Accès conditionnel VPN   |L’accès conditionnel et la conformité des appareils peuvent exiger que les appareils gérés respectent les normes avant de pouvoir se connecter au VPN. L’une des fonctionnalités avancées pour l’accès conditionnel VPN vous permet de limiter les connexions VPN à celles sur lesquelles le certificat d’authentification client contient l’OID d’accès conditionnel AAD de **1.3.6.1.4.1.311.87**.<p>Pour restreindre les connexions VPN, vous devez effectuer les opérations suivantes :<ol><li>Sur le serveur NPS, ouvrez le composant logiciel enfichable **serveur de stratégie réseau** .</li><li>Développez **stratégies** > **stratégies réseau**.</li><li>Cliquez avec le bouton droit sur la stratégie réseau des **connexions de réseau privé virtuel (VPN)** et sélectionnez **Propriétés**.</li><li>Sélectionnez l’onglet **Settings** (Paramètres).</li><li>Sélectionnez **spécifique au fournisseur**, puis cliquez sur **Ajouter**.</li><li>Sélectionnez l’option **allowed-Certificate-OID** , puis sélectionnez **Ajouter**.</li><li>Collez l’OID d’accès conditionnel AAD de **1.3.6.1.4.1.311.87** en tant que valeur d’attribut, puis sélectionnez **OK** deux fois.</li><li>Sélectionnez **Fermer**, puis **appliquer**.<p>Une fois que vous avez suivi ces étapes, lorsque les clients VPN essaient de se connecter à l’aide d’un certificat autre que le certificat Cloud à courte durée de vie, la connexion échoue.</li></ol>Pour plus d’informations sur l’accès conditionnel, consultez [VPN et accès conditionnel](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access).   |


---
## <a name="blocking-vpn-clients-that-use-revoked-certificates"></a>Blocage des clients VPN qui utilisent des certificats révoqués
  
Après l’installation des mises à jour, le serveur RRAS peut appliquer la révocation de certificat pour les VPN qui utilisent les certificats IKEv2 et les certificats d’ordinateur pour l’authentification, tels que les VPN Always on pour appareil tunnel. Cela signifie que pour ces VPN, le serveur RRAS peut refuser des connexions VPN aux clients qui essaient d’utiliser un certificat révoqué.

**Disponibilité**

Le tableau suivant répertorie les mises en production qui contiennent les correctifs pour chaque version de Windows.

|Version du système d’exploitation |Release  |
|---------|---------|
|Windows Server, version 1903  |[KB4501375](https://support.microsoft.com/help/4501375/windows-10-update-kb4501375) |
|Windows Server 2019<br />Windows Server, version 1809  |[KB4505658](https://support.microsoft.com/help/4505658/windows-10-update-kb4505658)  |
|Windows Server, version 1803  |[KB4507466](https://support.microsoft.com/help/4507466/windows-10-update-kb4507466)  |
|Windows Server version 1709  |[KB4507465](https://support.microsoft.com/help/4507465/windows-10-update-kb4507465)  |
|Windows Server 2016, version 1607  |[KB4503294](https://support.microsoft.com/help/4503294/windows-10-update-kb4503294) |

**Procédure de configuration des composants requis** 

1. Installez les mises à jour Windows dès qu’elles sont disponibles.
1. Assurez-vous que tous les certificats du client VPN et du serveur RRAS que vous utilisez comportent des entrées CDP et que le serveur RRAS peut atteindre les listes de révocation de certificats respectives.
1. Sur le serveur RRAS, utilisez l’applet de commande PowerShell **Set-VpnAuthProtocol** pour configurer le paramètre **RootCertificateNameToAccept** .<br /><br />
   L’exemple suivant répertorie les commandes permettant d’effectuer cette opération. Dans l’exemple, **CN = autorité de certification racine contoso** représente le nom unique de l’autorité de certification racine. 
   ``` powershell
   $cert1 = ( Get-ChildItem -Path cert:LocalMachine\root | Where-Object -FilterScript { $_.Subject -Like "*CN=Contoso Root Certification Authority*" } )
   Set-VpnAuthProtocol -RootCertificateNameToAccept $cert1 -PassThru
   ```
**Comment configurer le serveur RRAS pour appliquer la révocation de certificat pour les connexions VPN basées sur des certificats d’ordinateur IKEv2**

1. Dans une fenêtre d’invite de commandes, exécutez la commande suivante : 
   ```
   reg add HKLM\SYSTEM\CurrentControlSet\Services\RemoteAccess\Parameters\Ikev2 /f /v CertAuthFlags /t REG_DWORD /d "4"
   ```

1. Redémarrez le service **routage et accès distant** .
  
Pour désactiver la révocation de certificat pour ces connexions VPN, définissez **CertAuthFlags = 2** ou supprimez la valeur **CertAuthFlags** , puis redémarrez le service **routage et accès distant** . 

**Comment révoquer un certificat de client VPN pour une connexion VPN basée sur un certificat d’ordinateur IKEv2**
1. Révoquez le certificat du client VPN de l’autorité de certification.
1. Publiez une nouvelle liste de révocation de certificats à partir de l’autorité de certification.
1. Sur le serveur RRAS, ouvrez une fenêtre d’invite de commandes d’administration, puis exécutez les commandes suivantes :
   ```
   certutil -urlcache * delete
   certutil -setreg chain\ChainCacheResyncFiletime @now
   ```

**Comment vérifier que la révocation de certificat pour les connexions VPN basées sur un certificat d’ordinateur IKEv2 fonctionne**  
>[!Note]  
> Avant d’utiliser cette procédure, assurez-vous d’activer le journal des événements de fonctionnement CAPI2.
1. Suivez les étapes précédentes pour révoquer un certificat de client VPN.
1. Essayez de vous connecter au VPN à l’aide d’un client qui a le certificat révoqué. Le serveur RRAS doit refuser la connexion et afficher un message tel que « informations d’authentification IKE inacceptables ».
1. Sur le serveur RRAS, ouvrez observateur d’événements et accédez à **journaux des applications et des services/Microsoft/Windows/CAPI2**. 
1. Recherchez un événement qui contient les informations suivantes :
   * Nom du journal : **Microsoft-Windows-CAPI2/Operational Microsoft-Windows-CAPI2/Operational**
   * ID d’événement : **41** 
   * L’événement contient le texte suivant : **Subject = "** nom de domaine complet du client" (*FQDN client* représente le nom de domaine complet du client qui a le certificat révoqué). 

   Le champ **<Result>** des données d’événement doit inclure **le certificat est révoqué**. Par exemple, consultez les extraits suivants d’un événement :
   ```xml
   Log Name:      Microsoft-Windows-CAPI2/Operational Microsoft-Windows-CAPI2/Operational  
   Source:        Microsoft-Windows-CAPI2  
   Date:          5/20/2019 1:33:24 PM  
   Event ID:      41  
   ...  
   Event Xml:
   <Event xmlns="https://schemas.microsoft.com/win/2004/08/events/event">
    <UserData>  
     <CertVerifyRevocation>  
      <Certificate fileRef="C97AE73E9823E8179903E81107E089497C77A720.cer" subjectName="client01.corp.contoso.com" />  
      <IssuerCertificate fileRef="34B1AE2BD868FE4F8BFDCA96E47C87C12BC01E3A.cer" subjectName="Contoso Root Certification Authority" />
      ...
      <Result value="80092010">The certificate is revoked.</Result>
     </CertVerifyRevocation>
    </UserData>
   </Event>
   ```

---
## <a name="additional-protection"></a>Protection supplémentaire

### <a name="trusted-platform-module-tpm-key-attestation"></a>Attestation de clé de Module de plateforme sécurisée (TPM) (TPM)

Un certificat d’utilisateur doté d’une clé de module de plateforme sécurisée (TPM) fournit une garantie de sécurité accrue, sauvegardée par la non-exportabilité, l’anti-marteaurie et l’isolation des clés fournies par le module de plateforme sécurisée.

Pour plus d’informations sur l’attestation de clé TPM dans Windows 10, consultez [attestation de clé TPM](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation).

## <a name="next-step"></a>Étape suivante

[Commencez à planifier le déploiement de Always on VPN](always-on-vpn-deploy-planning.md): avant d’installer le rôle serveur d’accès à distance sur l’ordinateur que vous envisagez d’utiliser en tant que serveur VPN, effectuez les tâches suivantes. Après la planification appropriée, vous pouvez déployer Always On VPN et, éventuellement, configurer l’accès conditionnel pour la connectivité VPN à l’aide de Azure AD.  

## <a name="related-topics"></a>Rubriques connexes
- [Équilibrage de charge du serveur proxy NPS](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md): les clients protocole RADIUS (Remote Authentication Dial-in User Service) (RADIUS), qui sont des serveurs d’accès réseau tels que des serveurs de réseau privé virtuel (VPN) et des points d’accès sans fil, créent des demandes de connexion et les envoient à des serveurs RADIUS tels que NPS. Dans certains cas, un serveur NPS peut recevoir un trop grand nombre de demandes de connexion à la fois, ce qui entraîne une dégradation des performances ou une surcharge.

- [Vue d’ensemble de Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview): cette rubrique fournit une vue d’ensemble d’Azure Traffic Manager, qui vous permet de contrôler la distribution du trafic utilisateur pour les points de terminaison de service. Traffic Manager utilise le DNS (Domain Name System) pour diriger les requêtes des clients vers le point de terminaison approprié en fonction de la méthode de routage du trafic et de l’intégrité des points de terminaison. 

- [Windows Hello entreprise](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification): cette rubrique fournit les conditions préalables, telles que les déploiements de Cloud uniquement et les déploiements hybrides.  Cette rubrique répertorie également les questions fréquemment posées sur Windows Hello entreprise.

- [Étude de cas technique : activation de l’accès à distance avec Windows Hello entreprise dans Windows 10](https://msdn.microsoft.com/library/mt728163.aspx): dans cette étude de cas technique, vous découvrez comment Microsoft implémente l’accès à distance avec Windows Hello entreprise.  Windows Hello Entreprise est une approche d’authentification par clé publique/privée ou basée sur les certificats pour les organisations et les consommateurs qui ne nécessite pas de passer par un mot de passe. Cette forme d’authentification s’appuie sur des informations d’identification constituées d’une paire de clés, qui peuvent remplacer les mots de passe et sont résistantes aux failles, aux vols et au phishing. 

- [Intégrer l’authentification RADIUS à azure serveur multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius): cette rubrique vous guide tout au long de l’ajout et de la configuration d’une authentification client RADIUS avec Azure serveur multi-Factor Authentication. RADIUS est un protocole standard qui permet d’accepter les demandes d’authentification et de traiter ces demandes. Le serveur Azure Multi-Factor Authentication peut fonctionner comme un serveur RADIUS. 

- [Fonctionnalités de sécurité VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features): cette rubrique fournit des instructions de sécurité VPN pour le VPN de verrouillage, l’intégration de Windows information protection (WIP) avec VPN et les filtres de trafic. 

- [Options de profil de déclenchement automatique VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile): cette rubrique fournit des options de profil de déclenchement automatique VPN, telles que le déclencheur d’application, le déclencheur basé sur le nom et l’Always on.

- [VPN et accès conditionnel](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): cette rubrique fournit une vue d’ensemble de la plateforme d’accès conditionnel basé sur le Cloud pour fournir une option de conformité d’appareil pour les clients distants. L’accès conditionnel est un moteur d’évaluation basé sur les stratégies qui vous permet de créer des règles d’accès pour n’importe quelle application connectée Azure Active Directory (Azure AD). 

- [Attestation de clé TPM](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation): cette rubrique fournit une vue d’ensemble de module de plateforme sécurisée (TPM) (TPM) et des étapes de déploiement de l’attestation de clé TPM. Vous pouvez également trouver des informations de dépannage et des étapes pour résoudre les problèmes.
