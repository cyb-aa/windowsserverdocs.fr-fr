---
ms.assetid: 399a8bbe-3375-4bb0-b55b-5f46e7050028
title: 'Erreur de réplication1396 Échec d’ouverture de session: le nom du compte cible est incorrect'
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ef3da06dd348b804f538d37cafbfabdd8bf45beb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844500"
---
# <a name="replication-error-1396-logon-failure-the-target-account-name-is-incorrect"></a>Erreur de réplication1396 Échec d’ouverture de session: le nom du compte cible est incorrect

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


<developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd"> <introduction>
    <para>Cet article décrit les symptômes, causes et la résolution de la réplication Active Directory échoue avec l’erreur Win32 1396 : « Échec de la connexion : Le nom du compte cible est incorrect. » </para>
    <list class="bullet">
      <listItem>
        <para>
          <link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Symptoms">Symptômes</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Causes">Causes</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Resolutions">Résolutions</link>
        </para>
      </listItem>
    </list>
  </introduction>
  <section address="BKMK_Symptoms">
    <title>Symptômes</title>
    <content>
      <para />
      <list class="ordered">
<listItem><para>Rapports DCDIAG le test de Active Directory réplications a échoué avec l’erreur 1396 : Échec de l’ouverture de session : Le nom du compte cible est incorrect. »</para><code>Testing server: &lt;Site name&gt;&lt;DC Name&gt;
Starting test: Replications
[Replications Check,&lt;DC Name&gt;] A recent replication attempt failed:
From &lt;source DC&gt; to &lt;destination DC&gt;
Naming Context: CN=&lt;DN path of naming context&gt;
<codeFeaturedElement>The replication generated an error (1396):
Logon Failure: The target account name is incorrect.</codeFeaturedElement>
The failure occurred at &lt;date&gt; &lt;time&gt;.
The last success occurred at &lt;date&gt; &lt;time&gt;.
XX failures have occurred since the last success</code></listItem><listItem><para>REPADMIN. EXE signale que la dernière tentative de réplication a échoué avec l’état 1396.</para><para>REPADMIN commandes qui citent généralement l’état 1396 incluent, mais ne sont pas limités à :</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><tbody><tr><TD><list class="bullet"><listItem><para>REPADMIN /ADD</para></listItem><listItem><para>REPADMIN /REPLSUM</para></listItem><listItem><para>REPADMIN /REHOST</para></listItem><listItem><para>REPADMIN /SHOWVECTOR /LATENCY</para></listItem></list></TD><TD><list class="bullet"><listItem><para>REPADMIN /SHOWREPS</para></listItem><listItem><para>REPADMIN /SHOWREPL</para></listItem><listItem><para>REPADMIN /SYNCALL</para></listItem></list></TD></tr></tbody></table><para>Exemple de sortie à partir de « REPADMIN /SHOWREPS » illustrant une réplication entrante à partir de CONTOSO-CD2 à CONTOSO-CD1 échoue avec le « Échec d’ouverture de session : Le nom du compte cible est incorrect. » erreur est indiquée ci-dessous :</para><code>Default-First-Site-NameCONTOSO-DC1
DSA Options: IS_GC 
Site Options: (none)
DSA object GUID: b6dc8589-7e00-4a5d-b688-045aef63ec01
DSA invocationID: b6dc8589-7e00-4a5d-b688-045aef63ec01
==== INBOUND NEIGHBORS ======================================
DC=contoso,DC=com
Default-First-Site-NameCONTOSO-DC2 via RPC
DSA object GUID: 74fbe06c-932c-46b5-831b-af9e31f496b2
Last attempt @ &lt;date&gt; &lt;time&gt; failed, <codeFeaturedElement>result 1396 (0x574):
Logon Failure: The target account name is incorrect.</codeFeaturedElement>
&lt;#&gt; consecutive failure(s).
Last success @ &lt;date&gt; &lt;time&gt;.
</code></listItem><listItem><para>Le <ui>Répliquer maintenant</ui> commande dans les Sites Active Directory et Services renvoie « Échec d’ouverture de session : Le nom du compte cible est incorrect. »</para><para>Clic droit sur l’objet de connexion à partir d’une contrôleur de domaine source et en choisissant <ui>Répliquer maintenant</ui> échoue avec « échec d’ouverture de session : Le nom du compte cible est incorrect. » Le visuel le message d’erreur est indiqué ci-dessous :</para><para>Texte du titre de boîte de dialogue :</para><para>Répliquer maintenant</para><para>Texte du message de boîte de dialogue : </para><para>L’erreur suivante s’est produite pendant la tentative de synchronisation du contexte d’appellation &lt;le chemin d’accès de partition DNS&gt; à partir du contrôleur de domaine &lt;DC de source&gt; au contrôleur de domaine &lt;destination DC&gt;: Échec de l’ouverture de session : Le nom du compte cible est incorrect. Cette opération ne continuera pas. </para></listItem><listItem><para>Événements de KCC NTDS générales de NTDS ou Microsoft-Windows-ActiveDirectory_DomainService avec l’état 1396 sont consignés dans le journal Services d’annuaire dans l’Observateur d’événements.</para><para>Les événements de Active Directory qui citent généralement l’état 1396 incluent, mais ne sont pas limités à :</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><thead><tr><TD><para>ID d’événement</para></TD><TD><para>Source de l'événement</para></TD><TD><para>Chaîne d’événement</para></TD></tr></thead><tbody><tr><TD><para>1125</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>L’Assistant domaine Active Directory Services Installation (Dcpromo) n’a pas pu établir de connexion auprès du contrôleur de domaine suivant.</para></TD></tr><tr><TD><para>1645</para><para>Cet événement répertorie le SPN de trois parties.</para></TD><TD><para>Réplication NTDS</para></TD><TD><para>Active Directory n'a pas effectué un appel de procédure distante vers un autre contrôleur de domaine car le nom principal de service désiré pour le contrôleur de domaine cible n'est pas inscrit sur le contrôleur de domaine Centre de distribution de clés qui résout les noms principaux de service.</para></TD></tr><tr><TD><para>1655</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>Les Services de domaine Active Directory a tenté de communiquer avec le catalogue global suivant et les tentatives ont échoué.</para></TD></tr><tr><TD><para>2847</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>Le vérificateur de cohérence trouve une connexion de réplication pour le service d’annuaire en lecture seule local et a tenté de mettre à jour à distance sur l’instance de service d’annuaire suivante. L’opération a échoué. Il va être retentée.</para></TD></tr><tr><TD><para>1925</para></TD><TD><para>KCC NTDS</para></TD><TD><para>Échec de la tentative pour établir un lien de réplication pour la partition d’annuaire accessible en écriture suivante.</para></TD></tr><tr><TD><para>1926</para></TD><TD><para>KCC NTDS</para></TD><TD><para>La tentative d’établissement d’un lien de réplication pour une partition d’annuaire en lecture seule avec les paramètres suivants a échoué.</para></TD></tr><tr><TD><para>5781</para></TD><TD><para>ACCÈS RÉSEAU</para></TD><TD><para> Le serveur ne peut pas inscrire son nom dans DNS.</para></TD></tr></tbody></table></listItem><listItem><para>DCPROMO échoue avec une erreur à l’écran</para><para>Texte du titre de boîte de dialogue :</para><para>Échoué de l’Installation Active Directory</para><para>Texte du Message de boîte de dialogue :</para><para>Échec de l'opération. Le Service d’annuaire Impossible de créer l’objet serveur pour CN = NTDS Settings, CN = ServerBeingPromoted, CN = Servers, CN = sites, CN = Sites, CN = Configuration, DC = contoso, DC = com sur le serveur ReplicationSourceDC.contoso.com. </para><para>Vérifiez que les informations d’identification réseau fournies ont un accès suffisant pour ajouter un réplica. </para><para>
« Échec de la connexion : Le nom du compte cible est incorrect. "</para><para>Dans ce cas, l’ID d’événement 1645 et 1125 1168 sont enregistrés sur le serveur qui est promu.</para></listItem><listItem><para>Mapper un lecteur en utilisant <embeddedLabel>utilisation de net</embeddedLabel>:</para><code>C:&gt;net use z: &lt;server_name&gt;c$
System error 1396 has occurred.
Logon Failure: The target account name is incorrect.</code><para>Dans ce cas, le serveur peut également journalisation 333 d’ID d’événement dans le journal des événements système et utiliser une grande quantité de mémoire virtuelle pour une application telle que SQL Server.</para></listItem><listItem><para>L’heure du contrôleur de domaine est incorrect.</para></listItem><listItem><para>Le KDC ne démarrera pas sur un RODC après une restauration du compte krbtgt pour le RODC, ce qui a été supprimé. Par exemple, après une restauration, erreur 1396 s’affiche. </para><para>
ID d’événement 1645 est enregistré sur le RODC. </para><para>
Dcdiag signale également une erreur qu’il ne peut pas mettre à jour le compte krbtgt RODC. </para></listItem>
</list>
    </content>
  </section>
  <section address="BKMK_Causes">
    <title>Causes</title>
    <content>
      <para />
      <list class="ordered">
        <listItem>
          <para>Le SPN n’existe pas sur le catalogue global recherché par le KDC pour le compte client qui tente de s’authentifier à l’aide de Kerberos.</para>
          <para>Dans le contexte de la réplication Active Directory, le client Kerberos est le DC de destination, le KDC effectuer la recherche de nom principal de service est probablement la destination du contrôleur de domaine lui-même, mais peut être un contrôleur de domaine distant.</para>
        </listItem>
        <listItem>
          <para>L’utilisateur ou le compte de service qui doit contenir le nom de principal du service en cours de recherche n’existe pas sur le catalogue global recherché par le KDC pour le compte de destination tente de répliquer un contrôleur de domaine.</para>
          <para>Dans le contexte de la réplication Active Directory, le compte d’ordinateur du contrôleur de domaine source n’existe pas sur le catalogue global recherché par le contrôleur de domaine pour le compte de la réplication entrante performant DC de destination.</para>
        </listItem>
        <listItem>
          <para>Le DC de destination ne dispose pas d’un secret LSA pour le domaine de contrôleurs de domaine source.</para>
        </listItem>
        <listItem>
          <para>Le SPN recherché existe sur un compte d’ordinateur autre que le contrôleur de domaine source.</para>
        </listItem>
      </list>
    </content>
  </section>
  <section address="BKMK_Resolutions">
    <title>Résolutions</title>
    <content>
      <list class="ordered">
        <listItem>
          <para>Vérifiez le journal des événements Service d’annuaire sur le DC de destination pour l’événement NTDS réplication 1645 et notez les points suivants :</para>
          <para>Le nom de la destination du contrôleur de domaine</para>
          <para>Le SPN recherchée (E3514235-4B06-11D1-AB04-00C04FC2DCD2 /&lt;guid pour l’objet Paramètres NTDS de contrôleurs de domaine de source de l’objet&gt;/&lt;domaine cible&gt;.&lt; domaine de premier niveau&gt;@&lt;domaine cible&gt;.&lt; domaine de premier niveau&gt;</para>
          <para>Le KDC utilisé par le DC de destination</para>
        </listItem>
        <listItem>
          <para>À partir de la console du KDC identifié à l’étape 1, tapez : </para>
          <code>nltest /dsgetdc &lt;forest root DNS domain name &gt; /gc</code>
          <para>Exécutez le test du localisateur NLTEST immédiatement après une tentative de réplication échoue avec l’erreur 1396 sur le DC de destination. </para>
          <para>Cela doit identifier ce catalogue global qui le KDC effectue des recherches SPN sur. </para>
          <para>Le GC recherché par le KDC peut-être également être capturé dans l’événement Microsoft-Windows-ActiveDirectory_DomainService 1655.</para>
        </listItem>
        <listItem>
          <para>Recherche le SPN découvert à l’étape 1 sur le catalogue global détecté à l’étape 2.</para>
          <code>C:&gt;repadmin /showattr Server_Name DC=corp,DC=contoso,dc=com &lt;GC used by KDC&gt; &lt;DN path of forest root domain&gt; /filter:"(serviceprincipalname=&lt;SPN cited in the NTDS Replication event 1645&gt;)" /gc /subtree /atts:cn,serviceprincipalname</code>
          <para>OU</para>
          <code>C:&gt;dsquery * forestroot -scope subtree -filter "(serviceprincipalname=E3514235-4B06-11D1-AB04-00C04FC2DCD2/65cead9f-4949-46a3-a49a-f1fbfe13d2b3*)" -attr * -s Server_Name.europe.corp.contoso.com</code>
          <para>Vérifiez que l’objet hôte pour le SPN existe.</para>
          <para>Vérifiez le chemin d’accès de nom unique pour l’hôte objet, notamment si l’objet est CNF / conflit tronqué ou réside dans le conteneur LostAndFound.</para>
          <para>Vérifiez que la source de contrôleurs de domaine Active Directory Replication SPN est inscrit uniquement sur le compte d’ordinateur de contrôleurs de domaine source.</para>
          <para>Si la nom principal de service de réplication est manquante, déterminez si le contrôleur de domaine source a inscrit son SPN avec elle-même, et si le SPN est manquant dans le catalogue global utilisé par le KDC en raison de la latence de réplication ou d’un échec de la réplication.</para>
        </listItem>
        <listItem>
          <para>Vérifier l’intégrité du canal sécurisé et d’intégrité d’approbation.</para>
        </listItem>
      </list>
    </content>
  </section>
  <relatedTopics>
    <externalLink>
      <linkText>Résolution des problèmes d’opérations Active Directory qui échouent avec l’erreur 1396 : Échec de l’ouverture de session : Le nom du compte cible est incorrect.</linkText>
      <linkUri>https://support.microsoft.com/kb/2183411/en-gb</linkUri>
    </externalLink>
  </relatedTopics>
</developerConceptualDocument>


