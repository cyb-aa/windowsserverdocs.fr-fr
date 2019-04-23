---
ms.assetid: 208928eb-bb17-4984-a312-23fff43133e3
title: Améliorations de l’audit apportées à AD FS dans Windows Server 2016
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/25/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3d622686a3cc34316f0cf5187839785195c2f104
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880230"
---
# <a name="auditing-enhancements-to-ad-fs-in-windows-server-2016"></a>Améliorations de l’audit apportées à AD FS dans Windows Server 2016

>S'applique à : Windows Server 2016

Actuellement, dans AD FS pour Windows Server 2012 R2 il sont nombreux événements d’audit générés pour une seule requête et les informations pertinentes sur un journal dans ou les activités d’émission de jeton soient absent (dans certaines versions d’AD FS) ou répartie sur plusieurs événements d’audit. Par défaut, les services AD FS, les événements d’audit sont désactivées en raison de leur nature détaillée.  
    Avec la version des services AD FS dans Windows Server 2016, l’audit est devenue plus simple et moins détaillé.  
  
## <a name="auditing-levels-in-ad-fs-for-windows-server-2016"></a>Niveaux d’audit dans AD FS pour Windows Server 2016  
Par défaut, AD FS dans Windows Server 2016 a base l’audit est activé.  Avec l’audit de base, les administrateurs voient maximum 5 événements pour une demande unique.  Cela marque une réduction significative du nombre d’événements, les administrateurs disposent d’examiner, afin de voir une demande unique.   Le niveau d’audit peut être augmentée ou abaissée à l’aide de l’applet de commande PowerShell :  Set-AdfsProperties - AuditLevel.  Le tableau ci-dessous explique les niveaux d’audit disponibles.  
  
||||  
|-|-|-|  
|Niveau d'audit|Syntaxe de PowerShell|Description|  
|Aucune|Set-AdfsProperties - AuditLevel None|L’audit est désactivé et aucun événement ne sera consigné.|  
|Basic (valeur par défaut)|Set-AdfsProperties - AuditLevel Basic|Pas plus de 5 événements seront enregistrés pour une demande unique|  
|Verbose|Set-AdfsProperties - AuditLevel détaillée|Tous les événements seront enregistrés.  Ceci enregistrera une quantité significative d’informations par demande.|  
  
Pour afficher le niveau d’audit, vous pouvez utiliser l’applet de commande PowerShell :  Get-AdfsProperties.  
  
![améliorations d’audit](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_1.PNG)  
  
Le niveau d’audit peut être augmentée ou abaissée à l’aide de l’applet de commande PowerShell :  Set-AdfsProperties - AuditLevel.  
  
![améliorations d’audit](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_2.png)  
  
## <a name="types-of-audit-events"></a>Types d’événements d’Audit  
Événements d’Audit AD FS peut être de types différents, selon les différents types de demandes traitées par AD FS. Chaque type d’événement d’Audit a des données spécifiques associées.  Le type d’événements d’audit peut être différencié entre les demandes de connexion (par exemple, les demandes de jeton) par rapport aux demandes du système (appels y compris l’extraction des informations de configuration de serveur).    
  Le tableau ci-dessous décrit les types d’événements d’audit de base.  
  
||||  
|-|-|-|  
|Type d’événement d’audit|ID d’événement|Description|  
|Réussite de la Validation des informations d’identification fraîches|1202|Une demande où les nouvelles informations d’identification sont validées avec succès par le Service de fédération. Cela inclut WS-Trust, WS-Federation, SAML-P (premier tronçon pour générer l’authentification unique) et les points de terminaison autoriser OAuth.|  
|Erreur de Validation des informations d’identification fraîches|1203|Une demande où Échec de la validation de nouvelles informations d’identification sur le Service de fédération. Cela inclut WS-Trust, WS-Fed, SAML-P (premier tronçon pour générer l’authentification unique) et les points de terminaison autoriser OAuth.|  
|Jeton de l’application réussi|1200|Une demande où un jeton de sécurité est émis avec succès par le Service de fédération. Pour WS-Federation, SAML-P, elle est consignée lorsque la demande est traitée avec l’artefact d’authentification unique. (par exemple, le cookie d’authentification unique).|  
|Échec de jeton d’application|1201|Une demande où les d’émission de jeton de sécurité a échoué sur le Service de fédération. Pour WS-Federation, SAML-P, elle est consignée lorsque la demande a été traitée avec l’artefact d’authentification unique. (par exemple, le cookie d’authentification unique).|  
|Demande de modification de mot de passe réussie|1204|Une transaction où la demande de modification du mot de passe a été correctement traitée par le Service de fédération.|  
|Erreur de demande de modification de mot de passe|1205|Une transaction où la demande de modification du mot de passe a échoué doivent être traités par le Service de fédération.| 
|Déconnectez-vous de réussite|1206|Décrit une demande de déconnexion réussie.|  
|Déconnectez-vous de défaillance|1207|Décrit une demande de déconnexion a échoué.|  

  


