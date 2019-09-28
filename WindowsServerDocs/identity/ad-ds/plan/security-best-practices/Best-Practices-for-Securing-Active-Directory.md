---
ms.assetid: e2651dc8-4b31-4cd8-a400-3b8123890210
title: Meilleures pratiques pour la sécurisation d'Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 0d6f4abbf5dd071a2e229acbda2057c1f81851e6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408621"
---
# <a name="best-practices-for-securing-active-directory"></a>Meilleures pratiques pour la sécurisation d'Active Directory

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ce document fournit le point de vue d’un praticien et contient un ensemble de techniques pratiques pour aider les responsables informatiques à protéger un environnement de Active Directory d’entreprise. Active Directory joue un rôle essentiel dans l'infrastructure informatique et garantit l'harmonie et la sécurité des différentes ressources réseau dans un environnement global interconnecté. Les méthodes présentées reposent largement sur l’expérience de l’organisation Microsoft Information Security and Risk Management (ISRM), qui est responsable de la protection des ressources de Microsoft IT et d’autres divisions de l’entreprise Microsoft, en plus de conseiller un nombre sélectionné de clients Microsoft Global 500.  
  
-   [Résumé](../../../ad-ds/manage/component-updates/Executive-Summary.md)  
  
-   [Introduction](../../../ad-ds/manage/component-updates/Introduction.md)  
  
-   [Voies de compromis](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md)  
  
-   [Comptes attrayants pour le vol d’informations d’identification](../../../ad-ds/plan/security-best-practices/Attractive-Accounts-for-Credential-Theft.md)  
  
-   [Réduction de la surface d’attaque Active Directory](../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md)  
  
-   [Implémentation de modèles d’administration à privilèges faibles](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)  
  
-   [Implémentation des hôtes d’administration sécurisés](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md)  
  
-   [Sécurisation des contrôleurs de domaine contre les attaques](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md)  
  
-   [Surveillance des Active Directory pour les signes de compromission](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md)  
  
-   [Recommandations en matière de stratégie d’audit](../../../ad-ds/plan/security-best-practices/Audit-Policy-Recommendations.md)  
  
-   [Planification de la compromission](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md)  
  
-   [Maintenance d’un environnement plus sécurisé](../../../ad-ds/plan/security-best-practices/Maintaining-a-More-Secure-Environment.md)  
  
-   [Annexes](../../../ad-ds/plan/security-best-practices/Appendices.md)  
   
-   [Annexe B : Groupes et comptes privilégiés dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)  
  
-   [Annexe C : Groupes et comptes protégés dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)  
  
-   [Annexe D : Sécurisation des comptes d’administrateurs intégrés dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)  
  
-   [Annexe E : Sécurisation des groupes d’administrateurs de l’entreprise dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)  
  
-   [Annexe F : Sécurisation des groupes d’administrateurs de domaine dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)  
  
-   [Annexe G : Sécurisation des groupes d’administrateurs dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)  
  
-   [Annexe H : Sécurisation des groupes et des comptes des administrateurs locaux](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)  
  
-   [Annexe I : Création de comptes de gestion pour les groupes et les comptes protégés dans Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)   
  
-   [Annexe L : événements à analyser](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)  
  
-   [Annexe M : Liens vers des documents et lecture recommandée](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)  
  


