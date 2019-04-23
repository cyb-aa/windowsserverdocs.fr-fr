---
ms.assetid: 0fd7b6aa-3e50-45a3-a3a6-56982844363e
title: ID d’événement 2088 - Échec de la recherche DNS s’est produite avec la réplication
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: cc090fa749a601e53b4347cce43245f22badc8ae
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840710"
---
# <a name="event-id-2088-dns-lookup-failure-occurred-with-replication-success"></a>ID d’événement 2088 : Échec de la recherche DNS s’est produite avec la réplication

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

    
    <developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd">
      <introduction>
    <para>When a destination domain controller running Windows Server 2003 with Service Pack 1 (SP1) receives Event ID 2088 in the Directory Service event log, attempts to resolve the globally unique identifier (GUID) in the alias (CNAME) resource record to an IP address for the source domain controller failed. However, the destination domain controller tried other means to resolve the name and succeeded by using either the fully qualified domain name (FQDN) or the NetBIOS name of the source domain controller. Although replication was successful, the Domain Name System (DNS) problem should be diagnosed and resolved. </para>
    <para>The following is an example of the event text: </para>
    <code>Log Name: Directory Service

    Source: Microsoft-Windows-ActiveDirectory_DomainService
    Date: 3/15/2008  9:20:11 AM
    Event ID: 2088
    Task Category: DS RPC Client 
    Level: Warning
    Keywords: Classic
    User: ANONYMOUS LOGON
    Computer: DC3.contoso.com
    Description:
    Active Directory could not use DNS to resolve the IP address of the 
    source domain controller listed below. To maintain the consistency 
    of Security groups, group policy, users and computers and their passwords, 
    Active Directory Domain Services successfully replicated using the NetBIOS 
    or fully qualified computer name of the source domain controller. 

Configuration DNS non valide peut-être affecter les autres opérations essentielles sur les ordinateurs membres, les contrôleurs de domaine ou les serveurs d’applications dans cette forêt Active Directory Domain Services, y compris l’authentification d’ouverture de session ou l’accès aux ressources réseau. 

Vous devez immédiatement résoudre cette erreur de configuration DNS pour que ce contrôleur de domaine peut résoudre l’adresse IP du contrôleur de domaine source à l’aide de DNS. 

Nom de remplacement : Nom d’hôte DC1 échec DNS : 4a8717eb-8e58-456c-995a-c92e4add7e8e._msdcs.contoso.com 

REMARQUE : Par défaut, jusqu'à 10 échecs DNS sont affichées pour une période donnée de 12 heures, même si plus de 10 échecs se produisent.  Pour consigner tous les événements d’échec individuels, affectez les tests de diagnostic suivants du Registre 1 : 

Chemin d’accès du Registre : HKLM\System\CurrentControlSet\Services\NTDS\Diagnostics\22 DS RPC Client 

Action de l’utilisateur : 

1) Si le contrôleur de domaine source n’est plu fonctionner ou son système d’exploitation a été réinstallé avec un autre nom d’ordinateur ou NTDSDSA GUID, de l’objet supprimer les métadonnées du contrôleur de domaine source avec ntdsutil.exe, à l’aide de la procédure décrite dans l’article correctif 216498. 

2) Vérifiez que le contrôleur de domaine source est en cours d’exécution Active Directory et est accessible sur le réseau en tapant « net view \\ &lt;nom de contrôleur de domaine source&gt;» ou « ping &lt;nom de contrôleur de domaine source&gt;». 

3) Vérifiez que le contrôleur de domaine source utilise un serveur DNS valide pour les services DNS et qu’enregistrement d’hôte et l’enregistrement CNAME du contrôleur de domaine source l'enregistrement sont correctement inscrit, à l’aide de la version améliorée du DNS de DCDIAG. EXE disponible sur https://www.microsoft.com/dns 

DCDiag/test : DNS 

4) Vérifiez que ce contrôleur de domaine de destination utilise un serveur DNS valide pour les services DNS, en exécutant la version améliorée du DNS de DCDIAG. Commande EXE sur la console du contrôleur de domaine de destination, comme suit : 

DCDiag/test : DNS 

5) Pour une analyse plus approfondie des échecs d’erreur DNS consultez 824449 de la base de connaissances : https://support.microsoft.com/?kbid=824449 

Données supplémentaires Valeur d'erreur : 11004 le nom demandé est valide, mais aucune donnée du type demandé a été trouvée.</code> </introduction>
  <section>
    <title>Diagnostic</title>
    <content>
      <para>Échec de résolution du nom de contrôleur de domaine source à l’aide de l’enregistrement de ressource alias (CNAME) dans DNS peut être en raison d’erreurs de configuration DNS ou des retards dans la propagation des données DNS.</para>
    </content>
  </section>
  <section>
    <title>Résolution</title>
    <content>
      <para>Procéder au test de DNS comme décrit dans «<link xlink:href="85b1d179-f53e-4f95-b0b8-5b1c096a8076">ID d’événement 2087 : Échec de la recherche DNS a provoqué l’échec des réplications</link>. »</para>
    </content>
  </section>
  <relatedTopics />
</developerConceptualDocument>


