---
ms.assetid: 208928eb-bb17-4984-a312-23fff43133e3
title: "L’audit des améliorations apportées à ADFS dans Windows Server2016"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/25/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3d622686a3cc34316f0cf5187839785195c2f104
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="auditing-enhancements-to-ad-fs-in-windows-server-2016"></a>L’audit des améliorations apportées à ADFS dans Windows Server2016

>S’applique à: Windows Server2016

Actuellement, dans ADFS pour Windows Server2012R2 il sont nombreux événements d’audit générés pour une seule demande et les informations pertinentes sur une activité d’émission de connexion ou le jeton soient soit absent (dans certaines versions des services ADFS) ou réparti sur plusieurs événements d’audit. Par défaut, les services ADFS les événements d’audit sont désactivés en raison de leur nature verbose.  
    Avec la version des services ADFS dans Windows Server2016, l’audit est devenue plus simple et moins détaillés.  
  
## <a name="auditing-levels-in-ad-fs-for-windows-server-2016"></a>Niveaux d’audit dans ADFS pour Windows Server2016  
Par défaut, ADFS dans Windows Server2016 dispose d’audit de base est activé.  Avec l’audit de base, les administrateurs verrez les événements inférieur ou égal à 5 pour une seule demande.  Marque une diminution significative dans le nombre d’événements, les administrateurs disposent d’examiner, afin de voir une demande.   Le niveau d’audit peut être augmenté ou baissé à l’aide de l’applet de commande PowerShell: Set-AdfsProperties - AuditLevel.  Le tableau ci-dessous décrit les niveaux d’audit disponibles.  
  
||||  
|-|-|-|  
|Niveau d’audit|Syntaxe de PowerShell|Description|  
|None|Set-AdfsProperties - AuditLevel None|L’audit est désactivé et aucun événement n’est enregistré.|  
|Basic (par défaut)|Set-AdfsProperties - AuditLevel Basic|Pas plus de 5événements seront enregistrés pour une seule demande|  
|Commentaires|Set-AdfsProperties - AuditLevel documentée|Tous les événements seront enregistrés.  Elle enregistre une quantité importante d’informations par requête.|  
  
Pour afficher le niveau d’audit, vous pouvez utiliser l’applet de commande PowerShell: Get-AdfsProperties.  
  
![Améliorations d’audit](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_1.PNG)  
  
Le niveau d’audit peut être augmenté ou baissé à l’aide de l’applet de commande PowerShell: Set-AdfsProperties - AuditLevel.  
  
![Améliorations d’audit](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_2.png)  
  
## <a name="types-of-audit-events"></a>Types d’événements d’Audit  
Auditer les événements ADFS peut être de types différents, selon les différents types de demandes traitées par ADFS. Chaque type d’événement d’Audit a associé des données spécifiques.  Le type d’événements d’audit pouvant être distinguée entre les demandes de connexion (autrement dit, les demandes de jeton) par rapport aux demandes du système (appels de serveur, y compris l’extraction des informations de configuration).    
  Le tableau ci-dessous décrit les types d’événements d’audit de base.  
  
||||  
|-|-|-|  
|Type d’événement d’audit|ID d’événement|Description|  
|Réussite de Validation de nouvelles informations d’identification|1202|Une demande où les nouvelles informations d’identification sont validées correctement par le Service de fédération. Cela inclut WS-Trust, WS-Federation, SAML-P (première branche pour générer l’authentification unique) et OAuth autoriser les points de terminaison.|  
|Erreur de Validation de nouvelles informations d’identification|1203|Une demande où Échec de la validation des frais d’informations d’identification sur le Service de fédération. Cela inclut WS-Trust, WS-chargées, SAML-P (première branche pour générer l’authentification unique) et OAuth autoriser les points de terminaison.|  
|Réussite jeton d’application|1200|Une demande où un jeton de sécurité émis correctement par le Service de fédération. Pour WS-Federation, SAML-P, cette situation est enregistrée lorsque la demande est traitée avec les artefacts de l’authentification unique. (par exemple, le cookie SSO).|  
|Échec de jeton d’application|1201|Une demande où émission de jeton de sécurité a échoué sur le Service de fédération. Pour WS-Federation, SAML-P, cette situation est enregistrée lorsque la demande a été traitée avec les artefacts de l’authentification unique. (par exemple, le cookie SSO).|  
|Réussite de demande de modification de mot de passe|1204|Une transaction dans laquelle la demande de modification du mot de passe a été correctement traitée par le Service de fédération.|  
|Erreur de demande de modification de mot de passe|1205|Une transaction dans laquelle la demande de modification du mot de passe n’a pas pu être traités par le Service de fédération.| 
|Déconnectez-vous de réussite|1206|Décrit une demande de déconnexion réussie.|  
|Se déconnecter d’échec|1207|Décrit une demande de déconnexion a échoué.|  

  


