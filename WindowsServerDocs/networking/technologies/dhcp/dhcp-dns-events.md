---
title: Enregistrement des événements DHCP pour des enregistrements DNS
description: Cette rubrique fournit des informations sur le serveur DHCP dans Windows Server2016, les événements de journalisation.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: beb8c188-6fcf-4520-8825-d17f8ee9fb04
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 61a5099cd5e1ef1d4687baa8c20411c96ea8f519
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="dhcp-logging-events-for-dns-registrations"></a>Enregistrement des événements DHCP pour les inscriptions DNS

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Journaux des événements DHCP server fournissent désormais des informations détaillées sur les échecs d’enregistrement DNS.

>[!NOTE]
>Dans de nombreux cas, la raison pour les échecs d’inscription de l’enregistrement DNS par les serveurs DHCP est qu’une Zone de recherche Reverse\ DNS est mal configurée ou non configurée du tout.

Les nouveaux événements DHCP suivants vous aident à identifier facilement lorsque les inscriptions DNS sont échoue en raison d’une configuration incorrecte ou manquante de Zone de recherche de Reverse\ DNS.

|ID|Événement|Valeur|
|-----|--------------------|--------------------------------------------------------|
|20317|DHCPv4.ForwardRecordDNSFailure|Inscription d’enregistrement de transfert pour l’adresse IPv4%1 et le nom de domaine complet %2 a échoué avec l’erreur %3. Cela est probablement parce que la zone de recherche directe pour cet enregistrement n’existe pas sur le serveur DNS.|
|20318|DHCPv4.ForwardRecordDNSTimeout|Inscription d’enregistrement de transfert pour l’adresse IPv4%1 et le nom de domaine complet %2 a échoué avec l’erreur %3.|
|20319|DHCPv4.PTRRecordDNSFailure|Inscription des enregistrements PTR pour l’adresse IPv4%1 et le nom de domaine complet %2 a échoué avec l’erreur %3. Cela est probablement parce que la zone de recherche inversée pour cet enregistrement n’existe pas sur le serveur DNS.|
|20320|DHCPv4.PTRRecordDNSTimeout|Inscription des enregistrements PTR pour l’adresse IPv4%1 et le nom de domaine complet %2 a échoué avec l’erreur %3.|
|20321|DHCPv6.ForwardRecordDNSFailure|Inscription d’enregistrement de transfert pour l’adresse IPv6%1 et le nom de domaine complet %2 a échoué avec l’erreur %3. Cela est probablement parce que la zone de recherche directe pour cet enregistrement n’existe pas sur le serveur DNS.|
|20322|DHCPv6.ForwardRecordDNSTimeout|Inscription d’enregistrement de transfert pour l’adresse IPv6%1 et le nom de domaine complet %2 a échoué avec l’erreur %3.|
|20323|DHCPv6.PTRRecordDNSFailure|Inscription des enregistrements PTR pour l’adresse IPv6%1 et le nom de domaine complet %2 a échoué avec l’erreur %3. Cela est probablement parce que la zone de recherche inversée pour cet enregistrement n’existe pas sur le serveur DNS.|
|20324|DHCPv6.PTRRecordDNSTimeout|Inscription des enregistrements PTR pour l’adresse IPv6%1 et le nom de domaine complet %2 a échoué avec l’erreur %3.|
|20325|DHCPv4.ForwardRecordDNSError|Inscription des enregistrements PTR pour l’adresse IPv4%1 et le nom de domaine complet %2 a échoué avec l’erreur %3 \(%4\).|
|20326|DHCPv6.ForwardRecordDNSError|Enregistrement vers l’avant pour l’adresse IPv6%1 et le nom de domaine complet %2 a échoué avec l’erreur %3 \(%4\)|
|20327|DHCPv6.PTRRecordDNSError|Inscription des enregistrements PTR pour l’adresse IPv6%1 et le nom de domaine complet %2 a échoué avec l’erreur %3 \(%4\).|

