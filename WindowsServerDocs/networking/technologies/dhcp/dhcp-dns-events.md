---
title: Journalisation des événements DHCP pour inscrire les enregistrements DNS
description: Cette rubrique fournit des informations sur le serveur DHCP dans Windows Server 2016, les événements de journalisation.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: beb8c188-6fcf-4520-8825-d17f8ee9fb04
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7f4ce217b19cfd8a63bff1ae504362d4fc24fcd0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816900"
---
# <a name="dhcp-logging-events-for-dns-registrations"></a>Journalisation des événements DHCP pour les inscriptions DNS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Journaux des événements DHCP server fournissent désormais des informations détaillées sur les échecs d’inscription DNS.

>[!NOTE]
>Dans de nombreux cas, les échecs d’inscription des enregistrements DNS par les serveurs DHCP au fait qu’un DNS inversé\-Zone de recherche n’est pas configurée correctement ou configurée du tout.

Les nouveaux événements DHCP suivants vous aident à identifier facilement lorsque les enregistrements DNS sont échoue en raison d’une configuration incorrecte ou manquante DNS inversée\-Zone de recherche.

|ID|Événement|Value|
|-----|--------------------|--------------------------------------------------------|
|20317|DHCPv4.ForwardRecordDNSFailure|L’inscription d’enregistrement vers l’avant pour l’adresse IPv4 %1 et le nom de domaine complet %2 a échoué avec l’erreur %3. Cela est susceptible d’être, car la zone de recherche directe pour cet enregistrement n’existe pas sur le serveur DNS.|
|20318|DHCPv4.ForwardRecordDNSTimeout|L’inscription d’enregistrement vers l’avant pour l’adresse IPv4 %1 et le nom de domaine complet %2 a échoué avec l’erreur %3.|
|20319|DHCPv4.PTRRecordDNSFailure|Inscription des enregistrements PTR pour l’adresse IPv4 %1 et le nom de domaine complet %2 a échoué avec l’erreur %3. Cela est susceptible d’être, car la zone de recherche inversée pour cet enregistrement n’existe pas sur le serveur DNS.|
|20320|DHCPv4.PTRRecordDNSTimeout|Inscription des enregistrements PTR pour l’adresse IPv4 %1 et le nom de domaine complet %2 a échoué avec l’erreur %3.|
|20321|DHCPv6.ForwardRecordDNSFailure|L’inscription d’enregistrement vers l’avant pour l’adresse IPv6 %1 et le nom de domaine complet %2 a échoué avec l’erreur %3. Cela est susceptible d’être, car la zone de recherche directe pour cet enregistrement n’existe pas sur le serveur DNS.|
|20322|DHCPv6.ForwardRecordDNSTimeout|L’inscription d’enregistrement vers l’avant pour l’adresse IPv6 %1 et le nom de domaine complet %2 a échoué avec l’erreur %3.|
|20323|DHCPv6.PTRRecordDNSFailure|Inscription des enregistrements PTR pour l’adresse IPv6 %1 et le nom de domaine complet %2 a échoué avec l’erreur %3. Cela est susceptible d’être, car la zone de recherche inversée pour cet enregistrement n’existe pas sur le serveur DNS.|
|20324|DHCPv6.PTRRecordDNSTimeout|Inscription des enregistrements PTR pour l’adresse IPv6 %1 et le nom de domaine complet %2 a échoué avec l’erreur %3.|
|20325|DHCPv4.ForwardRecordDNSError|Inscription des enregistrements PTR pour l’adresse IPv4 %1 et le nom de domaine complet %2 a échoué avec l’erreur %3 \(%4\).|
|20326|DHCPv6.ForwardRecordDNSError|L’inscription d’enregistrement vers l’avant pour l’adresse IPv6 %1 et le nom de domaine complet %2 a échoué avec l’erreur %3 \(%4\)|
|20327|DHCPv6.PTRRecordDNSError|Inscription des enregistrements PTR pour l’adresse IPv6 %1 et le nom de domaine complet %2 a échoué avec l’erreur %3 \(%4\).|

