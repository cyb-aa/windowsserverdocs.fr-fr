---
ms.assetid: 208928eb-bb17-4984-a312-23fff43133e3
title: Améliorations de l’audit apportées à AD FS dans Windows Server 2016
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/25/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 191ecf5b3c7bf6c8c44d4d3553cd6e98b5543351
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853802"
---
# <a name="auditing-enhancements-to-ad-fs-in-windows-server-2016"></a>Améliorations de l’audit apportées à AD FS dans Windows Server 2016


Actuellement, dans AD FS pour Windows Server 2012 R2, de nombreux événements d’audit sont générés pour une requête unique et les informations pertinentes sur une activité de connexion ou d’émission de jetons sont absentes (dans certaines versions de AD FS) ou réparties sur plusieurs événements d’audit. Par défaut, les événements d’audit AD FS sont désactivés en raison de leur nature détaillée.  
    Avec la publication de AD FS dans Windows Server 2016, l’audit est devenu plus rationalisé et moins détaillé.  
  
## <a name="auditing-levels-in-ad-fs-for-windows-server-2016"></a>Niveaux d’audit dans AD FS pour Windows Server 2016  
Par défaut, les AD FS dans Windows Server 2016 sont activés pour l’audit de base.  Avec l’audit de base, les administrateurs verront 5 événements ou moins pour une requête unique.  Cela marque une diminution significative du nombre d’événements que les administrateurs doivent examiner pour afficher une requête unique.   Le niveau d’audit peut être augmenté ou diminué à l’aide de PowerShell applet : Set-AdfsProperties-AuditLevel.  Le tableau ci-dessous décrit les niveaux d’audit disponibles.  
  
||||  
|-|-|-|  
|Niveau d'audit|Syntaxe PowerShell|Description|  
|Aucune|Set-AdfsProperties-AuditLevel aucun|L’audit est désactivé et aucun événement n’est enregistré.|  
|De base (par défaut)|Set-AdfsProperties-AuditLevel de base|Plus de 5 événements seront journalisés pour une demande unique|  
|Verbose|Set-AdfsProperties-AuditLevel verbose|Tous les événements sont consignés.  Cela permet de consigner une quantité importante d’informations par demande.|  
  
Pour afficher le niveau d’audit actuel, vous pouvez utiliser PowerShell applet : obtenir-AdfsProperties.  
  
![améliorations de l’audit](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_1.PNG)  
  
Le niveau d’audit peut être augmenté ou diminué à l’aide de PowerShell applet : Set-AdfsProperties-AuditLevel.  
  
![améliorations de l’audit](media/Auditing-Enhancements-to-AD-FS-in-Windows-Server-2016/ADFS_Audit_2.png)  
  
## <a name="types-of-audit-events"></a>Types d’événements d’audit  
AD FS événements d’audit peuvent être de types différents, en fonction des différents types de demandes traités par AD FS. Chaque type d’événement d’audit est associé à des données spécifiques.  Le type d’événements d’audit peut être différencié entre les demandes de connexion (par exemple, les demandes de jeton) et les requêtes système (appels serveur-serveur, y compris la récupération des informations de configuration).    
  Le tableau ci-dessous décrit les types de base des événements d’audit.  
  
||||  
|-|-|-|  
|Type d’événement d’audit|ID d’événement|Description|  
|Réussite de la validation des informations d’identification|1202|Demande dans laquelle les nouvelles informations d’identification sont validées correctement par le service FS (Federation Service). Cela comprend WS-Trust, WS-Federation, SAML-P (premier tronçon pour générer l’authentification unique) et des points de terminaison d’autorisation OAuth.|  
|Nouvelle erreur de validation des informations d’identification|1203|Demande dans laquelle la validation des informations d’identification a échoué sur le service FS (Federation Service). Cela comprend WS-Trust, WS-FED, SAML-P (premier tronçon pour générer l’authentification unique) et des points de terminaison d’autorisation OAuth.|  
|Jeton d’application réussi|1200|Demande dans laquelle un jeton de sécurité est émis avec succès par le service FS (Federation Service). Pour WS-Federation, SAML-P est consigné lors du traitement de la demande avec l’artefact SSO. (par exemple, le cookie SSO).|  
|Échec du jeton d’application|1201|Demande dans laquelle l’émission du jeton de sécurité a échoué sur le service FS (Federation Service). Pour WS-Federation, SAML-P est consigné lorsque la requête a été traitée avec l’artefact SSO. (par exemple, le cookie SSO).|  
|Réussite de la demande de modification de mot de passe|1204|Transaction dans laquelle la demande de modification de mot de passe a été traitée avec succès par le service FS (Federation Service).|  
|Erreur de demande de modification de mot de passe|1205|Transaction dans laquelle la demande de modification de mot de passe n’a pas pu être traitée par le service FS (Federation Service).| 
|Déconnexion réussie|1206|Décrit une demande de déconnexion réussie.|  
|Échec de la déconnexion|1207|Décrit une demande de déconnexion ayant échoué.|  

  


