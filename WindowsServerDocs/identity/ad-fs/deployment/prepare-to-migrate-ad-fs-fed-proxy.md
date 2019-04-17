---
title: "Préparer la migration AD FS 2.0 serveur Proxy de fédération"
description: "Fournit des informations sur la préparation à la migration du serveur proxy ADFS vers Windows Server2012."
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f207993580e6fd06c9ff185e58e5b7e81af60252
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="prepare-to-migrate-the-ad-fs-20-federation-server-proxy"></a>Préparer la migration AD FS 2.0 serveur Proxy de fédération

Pour préparer la migration d’un serveur proxy de fédération 2.0 ADFS vers Windows Server2012, vous devez exporter et sauvegarder les données de configuration ADFS à partir de ce serveur proxy.  Les étapes décrites dans cette rubrique s’appliquent à un scénario avec un serveur proxy de fédération ou de plusieurs serveurs proxy de fédération.  
  
 Pour exporter les données de configuration ADFS, effectuez les tâches suivantes:  
  
-   [Étape1: Exporter les paramètres de service proxy](#step-1-export-proxy-service-settings)  
  
-   [Étape2: Sauvegarder les personnalisations de page Web](#step-2-back-up-webpage-customizations)  
  
##  <a name="step-1-export-proxy-service-settings"></a>Étape1: Exporter les paramètres de service proxy  
 Pour exporter les paramètres de service de fédération serveur proxy, effectuez la procédure suivante:  
  
### <a name="to-export-proxy-service-settings"></a>Pour exporter les paramètres de service proxy  
  
1.  Exporter le certificat Secure Sockets Layer (SSL) et sa clé privée dans un fichier .pfx. Pour plus d’informations, voir [exporter la partie clé privée d’un certificat d’authentification serveur](export-the-private-key-portion-of-a-server-authentication-certificate.md).  
  
> [!NOTE]
>  Cette étape est facultative, car ce certificat est préservé pendant la mise à niveau du système d’exploitation.  
  
2.  Exporter les propriétés du proxy ADFS 2.0 de fédération vers un fichier. Que faire à l’aide de Windows PowerShell.  
  
Ouvrez Windows PowerShell et exécutez la commande suivante pour ajouter les applets de commande AD FS à votre session Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Puis exécutez la commande suivante pour exporter les propriétés de proxy de fédération dans un fichier:`PSH:> Get-ADFSProxyProperties | out-file “.\proxyproperties.txt”`.  
  
3.  Vous devez connaître les informations d’identification d’un compte qui est soit un administrateur du serveur de fédération ADFS ou le compte de service sous lequel s’exécute le service de fédération ADFS.  Ces informations sont requises pour la configuration d’approbation de proxy.  
  
 La réalisation de cette étape permet de rassembler les informations suivantes requises pour configurer votre serveur proxy de fédération ADFS:  
  
-   Nom de service de fédération ADFS  
  
-   Nom du compte de domaine qui est requis pour la configuration d’approbation de proxy  
  
-   L’adresse et le port du proxy HTTP (s’il existe un proxy HTTP entre le serveur proxy de fédération ADFS et les serveurs de fédération ADFS)  
  
##  <a name="step-2-back-up-webpage-customizations"></a>Étape2: Sauvegarder les personnalisations de page Web  
 Pour sauvegarder les personnalisations de page Web, copiez les pages Web du proxy ADFS et le **web.config** fichier à partir du répertoire qui est mappé sur le chemin d’accès virtuel **«/ adfs/ls»** dans IIS.  Par défaut, il est dans le **%systemdrive%\inetpub\adfs\ls** active.  
  
## <a name="next-steps"></a>Étapes suivantes
 [Préparer la migration du serveur AD FS 2.0 de fédération](prepare-to-migrate-ad-fs-fed-server.md)   
 [Préparer la migration AD FS 2.0 serveur Proxy de fédération](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrer le serveur AD FS 2.0 de fédération](migrate-the-ad-fs-fed-server.md)   
 [Migrer le serveur Proxy AD FS 2.0 de fédération](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrer les Agents de 1.1 Web AD FS](migrate-the-ad-fs-web-agent.md)