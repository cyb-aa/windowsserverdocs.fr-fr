---
title: Procédures de dépannage courantes pour les problèmes KMS et DNS
description: ''
ms.topic: article
ms.date: 07/22/2019
ms.technology: server-general
ms.assetid: ''
author: Teresa-Motiv
ms.author: v-tea
ms.localizationpriority: medium
ms.openlocfilehash: 12e3c1fa82a567c43507df2f2ffd72595c3903ba
ms.sourcegitcommit: af80963a1d16c0b836da31efd9c5caaaf6708133
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68664891"
---
# <a name="common-troubleshooting-procedures-for-kms-and-dns-issues"></a>Procédures de dépannage courantes pour les problèmes KMS et DNS

Vous devrez peut-être utiliser certaines de ces méthodes si une ou plusieurs des conditions suivantes sont remplies :

- Vous utilisez un média sous licence en volume et une&nbsp;clé de produit générique de licence en volume pour installer l’un des systèmes d’exploitation suivants :
   - Windows Server 2019
   - Windows Server 2016
   - Windows Server 2012 R2
   - Windows Server 2012
   - Windows Server 2008 R2
   - Windows Server 2008
   - Windows 10
   - Windows 8.1
   - Windows 8
- L’Assistant Activation ne parvient pas à se connecter à un ordinateur hôte KMS.

Lorsque vous essayez d’activer un système client, l’Assistant Activation utilise le DNS pour localiser un ordinateur correspondant exécutant le logiciel KMS. Si l’Assistant interroge le DNS et ne trouve pas l’entrée DNS pour l’ordinateur hôte KMS, il signale une erreur.   

<a id="list"></a>Consultez la liste suivante pour trouver une approche adaptée à votre situation :

- Si vous ne parvenez pas à installer un hôte KMS ou à utiliser l’activation KMS, essayez la procédure [Remplacer la clé de produit par une clé MAK](#change-the-product-key-to-an-mak).
- Si vous devez installer et configurer un hôte KMS, utilisez la procédure [Configurer un hôte KMS sur lequel activer les clients](#configure-a-kms-host-for-the-clients-to-activate-against).
- Si le client ne parvient pas à localiser votre hôte KMS existant, utilisez les procédures suivantes pour résoudre les problèmes liés à vos configurations de routage. Ces procédures sont organisées de la plus simple à la plus complexe.
  - [Vérifier la connectivité IP de base au serveur DNS](#verify-basic-ip-connectivity-to-the-dns-server)
  - [Vérifier la configuration de l’hôte KMS](#verify-the-configuration-of-the-kms-host)  
  - [Déterminer le type d’un problème de routage](#determine-the-type-of-routing-issue)
  - [Vérifier la configuration DNS](#verify-the-dns-configuration)
  - [Créer manuellement un enregistrement SRV KMS](#manually-create-a-kms-srv-record)
  - [Attribuer manuellement un hôte KMS à un client KMS](#manually-assign-a-kms-host-to-a-kms-client)
  - [Configurer l’hôte KMS pour publier dans plusieurs domaines DNS](#configure-the-kms-host-to-publish-in-multiple-dns-domains)

## <a name="change-the-product-key-to-an-mak"></a>Remplacer la clé de produit par une clé MAK

Si vous ne parvenez pas à installer un hôte KMS ou à utiliser l’activation KMS, pour toute autre raison, remplacez la clé de produit par une clé MAK. Si vous avez téléchargé des images Windows à partir de MSDN (Microsoft Developer Network) ou de TechNet, les références SKU répertoriées sous le média sont généralement des médias sous licence en volume, et la clé de produit fournie est une clé MAK.

Pour remplacer la clé de produit par une clé MAK, procédez comme suit :

1. Ouvrez une fenêtre d'invite de commandes avec privilèges élevés. Pour ce faire, appuyez sur la touche du logo Windows + X, cliquez avec le bouton droit sur **Invite de commandes**, puis sélectionnez **Exécuter en tant qu’administrateur**. Le cas échéant, le système vous invite à taper un mot de passe administrateur ou à le confirmer.
2. À l'invite de commandes, exécutez la commande suivante :
   ```cmd
    slmgr -ipk xxxxx-xxxxx-xxxxx-xxxxx-xxxxx
   ```
   > [!NOTE]
   > L’espace réservé **xxxxx-xxxxx-xxxxx-xxxxx-xxxxx** représente votre clé de produit MAK.  

[Revenez à la liste des procédures.](#list)

## <a name="configure-a-kms-host-for-the-clients-to-activate-against"></a>Configurer un hôte KMS sur lequel activer les clients

L’activation KMS nécessite la configuration d’un hôte KMS sur lequel activer les clients. Si aucun hôte KMS n’est configuré dans votre environnement, installez et activez-en un en utilisant une clé d’hôte KMS appropriée. Après avoir configuré un ordinateur du réseau pour héberger le logiciel KMS, publiez les paramètres DNS (Domain Name System).

Pour plus d’informations sur le processus de configuration d’un hôte KMS, consultez les sections [Activer à l’aide du service Gestion des clés](https://docs.microsoft.com/windows/deployment/volume-activation/activate-using-key-management-service-vamt) et [Installer et configurer VMAT](https://docs.microsoft.com/windows/deployment/volume-activation/install-configure-vamt).

[Revenez à la liste des procédures.](#list)

## <a name="verify-basic-ip-connectivity-to-the-dns-server"></a>Vérifier la connectivité IP de base au serveur DNS

Vérifiez la connectivité IP de base au serveur DNS à l’aide de la commande ping. Pour ce faire, procédez comme suit sur le client KMS qui rencontre l’erreur et sur l’ordinateur hôte KMS :

1. Ouvrez une fenêtre d'invite de commandes avec privilèges élevés.
1. À l'invite de commandes, exécutez la commande suivante :
   ```cmd
   ping <DNS_Server_IP_address>
   ```
   > [!NOTE]
   > Si la sortie de cette commande n’inclut pas l’expression « Réponse de », cela indique un problème réseau ou DNS que vous devez résoudre avant de pouvoir utiliser les autres procédures de cet article. Pour plus d’informations sur la résolution des problèmes de TCP/IP si vous ne parvenez pas à exécuter une commande ping sur le serveur DNS, consultez la section [Résolution avancée des problèmes de TCP/IP](https://docs.microsoft.com/windows/client-management/troubleshoot-tcpip).

[Revenez à la liste des procédures.](#list)

## <a name="verify-the-configuration-of-the-kms-host"></a>Vérifier la configuration de l’hôte KMS

Vérifiez le Registre du serveur hôte KMS pour déterminer s’il est inscrit auprès du DNS. Par défaut, un serveur hôte KMS inscrit dynamiquement un enregistrement SRV DNS une fois toutes les 24 heures. 
> [!IMPORTANT]
> Suivez attentivement les étapes décrites dans cette section. De graves problèmes peuvent se produire si vous modifiez le Registre de façon incorrecte. Avant de le modifier, [sauvegardez le Registre afin de pouvoir le restaurer](https://support.microsoft.com/en-us/help/322756) en cas de problème.  

Pour vérifier ce paramètre, procédez comme suit :
1. Démarrez l'Éditeur du Registre. Pour ce faire, cliquez avec le bouton droit sur **Démarrer**, sélectionnez **Exécuter**, tapez **regedit**, puis appuyez sur Entrée.
1. Recherchez la sous-clé **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SL** et vérifiez la valeur de l’entrée **DisableDnsPublishing**. Cette entrée a les valeurs possibles suivantes :
   - **0** ou non défini(e) (valeur par défaut) : Le serveur hôte KMS inscrit un enregistrement SRV une fois toutes les 24 heures.
   - **1** : Le serveur hôte KMS n’inscrit pas automatiquement les enregistrements SRV. Si votre implémentation ne prend pas en charge les mises à jour dynamiques, consultez la section [Créer manuellement un enregistrement SRV KMS](#manually-create-a-kms-srv-record).  
1. Si l’entrée **DisableDnsPublishing** est manquante, créez-la (le type est DWORD). Si l’inscription dynamique est acceptable, laissez la valeur non définie ou affectez-lui la valeur **0**.

[Revenez à la liste des procédures.](#list)

## <a name="determine-the-type-of-routing-issue"></a>Déterminer le type d’un problème de routage

Vous pouvez utiliser les commandes suivantes pour déterminer s’il s’agit d’un problème de résolution de noms ou d’un problème d’enregistrement SRV.  

1. Sur un client KMS, ouvrez une fenêtre d’invite de commandes avec élévation de privilèges.  
1. À l’invite de commandes, exécutez les commandes suivantes :
   ```cmd
   cscript \windows\system32\slmgr.vbs -skms <KMS_FQDN>:<port>
   cscript \windows\system32\slmgr.vbs -ato
   ```
   > [!NOTE]
   > Dans cette commande, <KMS_FQDN> représente le nom de domaine complet (FQDN) de l’ordinateur hôte KMS et \<port\> représente le port TCP utilisé par KMS.  

   Si ces commandes résolvent le problème, il s’agit d’un problème d’enregistrement SRV. Vous pouvez le résoudre à l’aide de l’une des commandes décrites dans la procédure [Attribuer manuellement un hôte KMS à un client KMS](#manually-assign-a-kms-host-to-a-kms-client).  

1. Si le problème persiste, exécutez les commandes suivantes :
   ```cmd
   cscript \windows\system32\slmgr.vbs -skms <IP Address>:<port>
   cscript \windows\system32\slmgr.vbs -ato
   ```
   > [!NOTE]
   > Dans cette commande, \<adresse IP\> représente l’adresse IP de l’ordinateur hôte KMS et \<port\> représente le port TCP utilisé par KMS.  

   Si ces commandes résolvent le problème, il s’agit probablement d’un problème de résolution de noms. Pour plus d’informations sur la résolution des problèmes, consultez la procédure [Vérifier la configuration DNS](#verify-the-dns-configuration).

1. Si aucune de ces commandes ne résout le problème, vérifiez la configuration du pare-feu de l’ordinateur. Toutes les communications d’activation qui se produisent entre les clients KMS et l’hôte KMS utilisent le port TCP 1688. Les pare-feu sur le client KMS et l’hôte KMS doivent autoriser la communication sur le port 1688.

[Revenez à la liste des procédures.](#list)

## <a name="verify-the-dns-configuration"></a>Vérifier la configuration DNS

>[!NOTE]
> Sauf indication contraire, procédez comme suit sur un client KMS qui a rencontré l’erreur applicable.

1. Ouvrez une fenêtre d’invite de commandes avec élévation de privilèges
1. À l'invite de commandes, exécutez la commande suivante :
   ```cmd
   IPCONFIG /all
   ```
1. À partir des résultats de la commande, notez les informations suivantes :
   - Adresse IP affectée à l’ordinateur client KMS
   - L’adresse IP du serveur DNS principal que l’ordinateur client KMS utilise
   - L’adresse IP de la passerelle par défaut que l’ordinateur client KMS utilise
   - La liste de recherche de suffixes DNS que l’ordinateur client KMS utilise
1. Vérifiez que les enregistrements SRV de l’hôte KMS sont inscrits dans le DNS. Pour cela, procédez comme suit:  
   1. Ouvrez une fenêtre d'invite de commandes avec privilèges élevés.
   1. À l'invite de commandes, exécutez la commande suivante :
      ```cmd
      nslookup -type=all _vlmcs._tcp>kms.txt
      ```
   1. Ouvrez le fichier KMS.txt généré par la commande. Ce fichier doit contenir une ou plusieurs entrées qui ressemblent à l’entrée suivante :
       ```
       _vlmcs._tcp.contoso.com SRV service location:
       priority = 0
       weight = 0
       port = 1688 svr hostname = kms-server.contoso.com
       ```
       > [!NOTE]
       > Dans cette entrée, contoso.com représente le domaine de l’hôte KMS.
      1. Vérifiez l’adresse IP, le nom d’hôte, le port et le domaine de l’hôte KMS.
      1. Si ces entrées **_vlmcs** existent et si elles contiennent les noms d’hôte KMS attendus, accédez à la section [Attribuer manuellement un hôte KMS à un client KMS](#manually-assign-a-kms-host-to-a-kms-client).  
      > [!NOTE]
      > Si la commande [**nslookup**](https://docs.microsoft.com/windows-server/administration/windows-commands/nslookup) trouve l’hôte KMS, cela ne signifie pas que le client DNS peut trouver l’hôte KMS. Si la commande **nslookup** trouve l’hôte KMS, mais que vous ne parvenez toujours pas à l’activer à l’aide de l’hôte KMS, vérifiez les autres paramètres DNS, tels que le suffixe DNS principal et la liste de recherche du suffixe DNS.
1. Vérifiez que la liste de recherche du suffixe DNS principal contient le suffixe de domaine DNS associé à l’hôte KMS. Si la liste de recherche n’inclut pas ces informations, accédez à la procédure [Configurer l’hôte KMS pour publier dans plusieurs domaines DNS](#configure-the-kms-host-to-publish-in-multiple-dns-domains).

[Revenez à la liste des procédures.](#list)

## <a name="manually-create-a-kms-srv-record"></a>Créer manuellement un enregistrement SRV KMS

Pour créer manuellement un enregistrement SRV pour un hôte KMS qui utilise un serveur DNS Microsoft, procédez comme suit :

1. Sur le serveur DNS, ouvrez le Gestionnaire DNS. Pour ouvrir le Gestionnaire DNS, sélectionnez **Démarrer**, **Outils d’administration**, puis **DNS**.
1. Sélectionnez le serveur DNS sur lequel vous devez créer l’enregistrement de ressource SRV.
1. Dans l’arborescence de la console, développez **Zones de recherche directe**, cliquez avec le bouton droit sur le domaine, puis sélectionnez **Nouveaux enregistrements**.
1. Faites défiler la liste, sélectionnez **Emplacement du service(SRV)** , puis sélectionnez **Créer un enregistrement**.
1. Entrez les informations suivantes :
   - Service : **_VLMCS**
   - Protocole : **_TCP**
   - Numéro de port : **1688**
   - Hôte offrant ce service : **&lt;*Nom complet de l’hôte KMS*&gt;**
1. Lorsque vous avez terminé, sélectionnez **OK**, puis **Terminé**.

Pour créer manuellement un enregistrement SRV pour un hôte KMS qui utilise un serveur DNS compatible BIND 9.x, suivez les instructions pour ce serveur DNS et fournissez les informations suivantes pour l’enregistrement SRV :

- Nom :&nbsp; **_vlmcs._TCP**
- Type : &nbsp;**SRV**
- Priorité : **0**
- Poids : **0**
- Port : **1688**
- Nom d’hôte : **&lt;*Nom complet ou nom de l’hôte KMS*&gt;**

> [!NOTE]
> KMS n’utilise pas les valeurs **Priorité** ou **Poids**. Toutefois, l’enregistrement doit les inclure.

Pour configurer un serveur DNS compatible BIND 9.x afin de prendre en charge la publication automatique KMS, configurez le serveur DNS pour activer les mises à jour des enregistrements de ressources à partir des hôtes KMS. Par exemple, ajoutez la ligne suivante à la définition de zone dans Named.conf ou dans Named.conf.local :

```cmd
allow-update { any; };
```
## <a name="manually-assign-a-kms-host-to-a-kms-client"></a>Attribuer manuellement un hôte KMS à un client KMS

Par défaut, les clients KMS utilisent le processus de détection automatique. D’après ce processus, un client KMS interroge le DNS pour obtenir la liste des serveurs qui ont publié des enregistrements SRV _vlmcs dans la zone d’appartenance du client. Le DNS retourne la liste des hôtes KMS dans un ordre aléatoire. Le client choisit un hôte KMS et tente d’établir une session sur celui-ci. Si cette tentative aboutit, le client met en cache le nom de l’hôte KMS et tente de l’utiliser pour la tentative de renouvellement suivante. Si la configuration de la session échoue, le client choisit de façon aléatoire un autre hôte KMS. Nous vous recommandons vivement d’utiliser le processus de détection automatique.  

Toutefois, vous pouvez affecter manuellement un hôte KMS à un client KMS particulier. Pour ce faire, procédez comme suit.

1. Sur un client KMS, ouvrez une fenêtre d’invite de commandes avec élévation de privilèges.
1. Selon votre implémentation, effectuez l’une des étapes suivantes:
   - Pour affecter un hôte KMS à l’aide du nom de domaine complet de l’hôte, exécutez la commande suivante :
     ```cmd
     cscript \windows\system32\slmgr.vbs -skms <KMS_FQDN>:<port>
     ```
   - Pour affecter un hôte KMS à l’aide de la version 4 de l’adresse IP de l’hôte, exécutez la commande suivante :
     ```cmd
     cscript \windows\system32\slmgr.vbs -skms <IPv4Address>:<port>
     ```
    - Pour affecter un hôte KMS à l’aide de la version 6 de l’adresse IP de l’hôte, exécutez la commande suivante :
      ```cmd
      cscript \windows\system32\slmgr.vbs -skms <IPv6Address>:<port>
      ```
    - Pour affecter un hôte KMS à l’aide du nom NETBIOS de l’hôte, exécutez la commande suivante :
      ```cmd
      cscript \windows\system32\slmgr.vbs -skms <NETBIOSName>:<port>
      ```
   - Pour revenir à la détection automatique sur un client KMS, exécutez la commande suivante :
     ```cmd
     cscript \windows\system32\slmgr.vbs -ckms
     ```
     > [!NOTE]
     > Ces commandes utilisent les espaces réservés suivants :
     >- **<KMS_FQDN>** représente le nom de domaine complet de l’ordinateur hôte KMS
     >- **\<IPv4Address\>** représente la version 4 de l’adresse IP de l’ordinateur hôte KMS
     >- **\<IPv6Address\>** représente la version 6 de l’adresse IP de l’ordinateur hôte KMS
     >- **\<NETBIOSName\>** représente le nom NETBIOS de l’ordinateur hôte KMS
     >- **\<port\>** représente le port TCP que KMS utilise.  

## <a name="configure-the-kms-host-to-publish-in-multiple-dns-domains"></a>Configurer l’hôte KMS pour publier dans plusieurs domaines DNS

> [!IMPORTANT]
> Suivez attentivement les étapes décrites dans cette section. De graves problèmes peuvent se produire si vous modifiez le Registre de façon incorrecte. Avant de le modifier, [sauvegardez le Registre afin de pouvoir le restaurer](https://support.microsoft.com/help/322756) en cas de problème.

Comme décrit dans [Attribuer manuellement un hôte KMS à un client KMS](#manually-assign-a-kms-host-to-a-kms-client), les clients KMS utilisent généralement le processus de détection automatique pour identifier les hôtes KMS. Ce processus nécessite que les enregistrements SRV _vlmcs soient disponibles dans la zone DNS de l’ordinateur client KMS. La zone DNS correspond au suffixe DNS principal de l’ordinateur ou à l’un des éléments suivants :
- Pour les ordinateurs joints à un domaine, le domaine de l’ordinateur affecté par le système DNS (par exemple, DNS Active Directory Domain Services (AD DS)).
- Pour les ordinateurs de groupe de travail, le domaine de l’ordinateur affecté par le protocole DHCP (Dynamic Host Configuration Protocol). Ce nom de domaine est défini par l’option qui a la valeur de code 15, comme défini dans le document RFC (Request For Comments) 2132.

Par défaut, un hôte KMS inscrit ses enregistrements SRV dans la zone DNS qui correspond au domaine de l’ordinateur hôte KMS. Par exemple, supposons qu’un hôte KMS est joint au domaine contoso.com. Dans ce scénario, l’hôte KMS inscrit son enregistrement SRV _vmlcs sous la zone DNS contoso.com. Par conséquent, l’enregistrement identifie le service en tant que VLMCS._TCP.CONTOSO.COM.

Si l’hôte KMS et les clients KMS utilisent des zones DNS différentes, vous devez configurer l’hôte KMS pour qu’il publie automatiquement ses enregistrements SRV dans plusieurs domaines DNS. Pour cela, procédez comme suit:

1. Sur l’hôte KMS, démarrez l’Éditeur du Registre. 
1. Recherchez puis sélectionnez la sous-clé **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SL**.
1. Dans le volet **Détails**, cliquez avec le bouton droit sur une zone vide, sélectionnez **Nouveau**, puis sélectionnez **Valeur de chaînes multiples**.
1. Pour le nom de la nouvelle entrée, entrez **DnsDomainPublishList**.
1. Cliquez avec le bouton droit sur la nouvelle entrée **DnsDomainPublishList**, puis sélectionnez **Modifier**.
1. Dans la boîte de dialogue **Modifier les chaînes multiples**, tapez chaque suffixe de domaine DNS que KMS publie sur une ligne distincte, puis sélectionnez **OK**.
   > [!NOTE]
   > Pour Windows Server 2008 R2, le format de **DnsDomainPublishList** diffère. Pour plus d’informations, voir le Guide des informations techniques de référence sur l’activation en volume.
1. Utilisez l’outil d’administration des services pour redémarrer le service de gestion de licences des logiciels. Cette opération crée les enregistrements SRV.
1. Vérifiez qu’en utilisant une méthode standard, le client KMS peut contacter l’hôte KMS que vous avez configuré. Vérifiez que le client KMS identifie correctement l’hôte KMS à la fois par le nom et par l’adresse IP. Si l’une de ces vérifications échoue, examinez ce problème de résolution du client DNS.
1. Pour effacer les noms d’hôte KMS précédemment mis en cache sur le client KMS, ouvrez une fenêtre d’invite de commandes avec élévation de privilèges sur le client KMS, puis exécutez la commande suivante :
   ```cmd
   cscript C:\Windows\System32\slmgr.vbs -ckms
   ```
