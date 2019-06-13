---
ms.assetid: 0f21951c-b1bf-43bb-a329-bbb40c58c876
title: Erreur de réplication 1753 Le mappeur de point final n’a plus de point final disponible
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e7d2b47c9c14af22cdcf29fb388779e7639e38cb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442770"
---
# <a name="replication-error-1753-there-are-no-more-endpoints-available-from-the-endpoint-mapper"></a>Erreur de réplication 1753 Le mappeur de point final n’a plus de point final disponible

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


<developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd"> <introduction>
    <para>Cette rubrique explique les symptômes, causes et résolution Active Directory replication erreur 8524 de l’opération DSA ne peut pas continuer en raison d’un échec de la recherche DNS.</para>
    <list class="bullet">
      <listItem>
        <para>
          <link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_Symptoms">Symptômes</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_Cause">Cause</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_Resolutions">Résolutions</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_MoreInfo">Plus d’informations</link>
        </para>
      </listItem>
    </list>
  </introduction>
  <section address="BKMK_Symptoms">
    <title>Symptômes</title>
    <content>
      <para>Cet article décrit les symptômes, causes et les étapes de résolution pour les opérations Active Directory qui échouent avec l’erreur Win32 1753 : &quot;Aucun point de terminaison plus ne sont disponibles sur l’endpoint mapper.&quot;</para>
      <list class="ordered">
        <listItem>
          <para>Rapports DCDIAG le test de connectivité, Active Directory réplications test ou KnowsOfRoleHolders a échoué avec l’erreur 1753 : &quot;Aucun point de terminaison plus ne sont disponibles sur l’endpoint mapper.&quot;</para>
          <code>Testing server: &lt;site&gt;&lt;DC Name&gt;
Starting test: Connectivity
* Active Directory LDAP Services Check
* Active Directory RPC Services Check
[&lt;DC Name&gt;] <codeFeaturedElement>DsBindWithSpnEx() failed with error 1753,
There are no more endpoints available from the endpoint mapper..</codeFeaturedElement>
Printing RPC Extended Error Info:
Error Record 1, ProcessID is &lt;process ID&gt; (DcDiag) 
System Time is: &lt;date&gt; &lt;time&gt;
Generating component is 2 (RPC runtime)
Status is 1753: There are no more endpoints available from the endpoint mapper. Detection location is 500
NumberOfParameters is 4
Unicode string: ncacn_ip_tcp
Unicode string: &lt;source DC object GUID&gt;._msdcs.contoso.com
Long val: -481213899
Long val: 65537
Error Record 2, ProcessID is 700 (DcDiag) 
System Time is: &lt;date&gt; &lt;time&gt;
Generating component is 2 (RPC runtime)
<codeFeaturedElement>Status is 1753: There are no more endpoints available from the endpoint mapper.</codeFeaturedElement>
NumberOfParameters is 1
Unicode string: 1025
[Replications Check,&lt;DC Name&gt;] A recent replication attempt failed:
From &lt;source DC&gt; to &lt;destination DC&gt;
Naming Context: &lt;DN path of directory partition&gt;
The replication generated an error <codeFeaturedElement>(1753):
There are no more endpoints available from the endpoint mapper.</codeFeaturedElement> 
The failure occurred at &lt;date&gt; &lt;time&gt;.
The last success occurred at &lt;date&gt; &lt;time&gt;.
3 failures have occurred since the last success.
The directory on &lt;DC name&gt; is in the process.
of starting up or shutting down, and is not available.
Verify machine is not hung during boot.
</code>
        </listItem>
<listItem><para>REPADMIN. EXE signale que cette tentative de réplication a échoué avec l’état 1753.</para><para>REPADMIN commandes qui citent généralement l’état 1753 incluent, mais ne sont pas limités à :</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><tbody><tr><TD><list class="bullet"><listItem><para>REPADMIN /REPLSUM</para></listItem><listItem><para>REPADMIN /SHOWREPL</para></listItem></list></TD><TD><list class="bullet"><listItem><para>REPADMIN /SHOWREPS</para></listItem><listItem><para>REPADMIN /SYNCALL</para></listItem></list></TD></tr></tbody></table><para>Exemple de sortie à partir de &quot;REPADMIN /SHOWREPS&quot; illustrant une réplication entrante à partir de CONTOSO-CD2 à CONTOSO-CD1 échoue avec le &quot;accès à la réplication a été refusé&quot; erreur est indiquée ci-dessous :</para><code>Default-First-Site-NameCONTOSO-DC1
DSA Options: IS_GC 
Site Options: (none)
DSA object GUID: b6dc8589-7e00-4a5d-b688-045aef63ec01
DSA invocationID: b6dc8589-7e00-4a5d-b688-045aef63ec01
==== INBOUND NEIGHBORS ======================================
DC=contoso,DC=com
Default-First-Site-NameCONTOSO-DC2 via RPC
DSA object GUID: 74fbe06c-932c-46b5-831b-af9e31f496b2
Last attempt @ &lt;date&gt; &lt;time&gt; failed, <codeFeaturedElement>result 1753 (0x6d9):
There are no more endpoints available from the endpoint mapper.</codeFeaturedElement>
&lt;#&gt; consecutive failure(s).
Last success @ &lt;date&gt; &lt;time&gt;.

</code></listItem><listItem><para>Le <ui>vérifier la topologie de réplication</ui> commande dans les Services et Sites Active Directory renvoie &quot;aucun point de terminaison plus ne sont disponibles dans le mappeur de point de terminaison.&quot;</para><para>Clic droit sur l’objet de connexion à partir d’une contrôleur de domaine source et en choisissant <ui>vérifier la topologie de réplication</ui> échoue avec &quot;aucun point de terminaison plus ne sont disponibles dans le mappeur de point de terminaison.&quot; Le visuel le message d’erreur est indiqué ci-dessous :</para><para>Texte du titre de boîte de dialogue : Vérification de la topologie de réplication</para><para>Texte du message de boîte de dialogue : </para><para>L’erreur suivante s’est produite pendant la tentative de contacter le contrôleur de domaine : Aucun point de terminaison plus ne sont disponibles sur l’endpoint mapper.</para></listItem><listItem><para>Le <ui>Répliquer maintenant</ui> commande dans les Services et Sites Active Directory renvoie &quot;aucun point de terminaison plus ne sont disponibles dans le mappeur de point de terminaison.&quot;</para><para>Clic droit sur l’objet de connexion à partir d’une contrôleur de domaine source et en choisissant <ui>Répliquer maintenant</ui> échoue avec &quot;aucun point de terminaison plus ne sont disponibles dans le mappeur de point de terminaison.&quot; Le visuel le message d’erreur est indiqué ci-dessous :</para><para>Texte du titre de boîte de dialogue : Répliquer maintenant</para><para>Texte du message de boîte de dialogue : L’erreur suivante s’est produite pendant la tentative de synchronisation du contexte d’appellation &lt;nom de partition de répertoire %&gt; à partir du contrôleur de domaine &lt;contrôleur de domaine Source&gt; au contrôleur de domaine &lt;DC de Destination&gt;:</para><para>

Aucun point de terminaison plus ne sont disponibles sur l’endpoint mapper.</para><para>L’opération ne continuera pas</para></listItem><listItem><para>Événements KCC NTDS générales de NTDS ou Microsoft-Windows-ActiveDirectory_DomainService avec l’état-2146893022 sont enregistrés dans le journal Services d’annuaire dans l’Observateur d’événements.</para><para>Les événements de Active Directory qui citent généralement l’état-2146893022 incluent, mais ne sont pas limités à :</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><thead><tr><TD><para>ID d’événement</para></TD><TD><para>Source de l'événement</para></TD><TD><para>Chaîne d’événement</para></TD></tr></thead><tbody><tr><TD><para>1655</para></TD><TD><para>NTDS Général</para></TD><TD><para>Active Directory a tenté de communiquer avec le catalogue global suivant et les tentatives ont échoué.</para></TD></tr><tr><TD><para>1925</para></TD><TD><para>KCC NTDS</para></TD><TD><para>Échec de la tentative pour établir un lien de réplication pour la partition d’annuaire accessible en écriture suivante.</para></TD></tr><tr><TD><para>1265</para></TD><TD><para>KCC NTDS</para></TD><TD><para>Une tentative par le vérificateur de cohérence des connaissances (KCC) pour ajouter un contrat de réplication pour le répertoire source et la partition contrôleur de domaine suivant a échoué.</para></TD></tr></tbody></table></listItem>
</list>
    </content>
  </section>
  <section address="BKMK_Cause">
    <title>Cause</title>
    <content>
      <para>Le diagramme ci-dessous illustre le flux de travail RPC commençant par l’inscription de l’application de serveur avec le mappeur de point de terminaison RPC (EPM) à l’étape 1 pour la transmission de données à partir du client RPC à l’application cliente à l’étape 7. </para>
      <para>&lt;ADDS_RPCWorkflow&gt;</para>
      <para>Étapes 1 à 7 de mappage pour les opérations suivantes :</para>
      <list class="ordered">
        <listItem>
          <para>Application de serveur inscrit ses points de terminaison avec le mappeur de point de terminaison RPC (EPM) </para>
        </listItem>
        <listItem>
          <para>Client effectue un appel RPC (pour le compte d’un utilisateur, système d’exploitation ou d’application lancé une opération) </para>
        </listItem>
        <listItem>
          <para>Côté client RPC contacte les ordinateurs cibles EPM et demandez du point de terminaison terminer l’appel du client </para>
        </listItem>
        <listItem>
          <para>Machine de serveur&#39;s EPM répond avec un point de terminaison </para>
        </listItem>
        <listItem>
          <para>Côté client RPC contacte l’application serveur </para>
        </listItem>
        <listItem>
          <para>Application serveur s’exécute l’appel, retourne le résultat au client RPC </para>
        </listItem>
        <listItem>
          <para>Côté client RPC repasse le résultat à l’application cliente</para>
        </listItem>
      </list>
      <para>Échec 1753 sont générées par une défaillance entre les étapes #3 et #4. Plus précisément, erreur 1753 signifie que le client RPC (DC de destination) a été en mesure de contacter le serveur RPC (contrôleur de domaine source) sur le port 135, mais EPM sur le serveur RPC (contrôleur de domaine source) n’a pas pu localiser l’application RPC d’intérêt et a retourné l’erreur du côté serveur 1753. La présence de l’erreur 1753 indique que le client RPC (DC de destination) a reçu la réponse d’erreur côté serveur à partir du serveur RPC (source de réplication AD DC) sur le réseau. </para>
      <para>Les causes spécifiques de l’erreur 1753 incluent : </para>
      <list class="ordered">
        <listItem>
          <para>L’application serveur n’a jamais démarrée (autrement dit, étape 1 de la &quot;plus d’informations&quot; situé au-dessus de diagramme a été tentée jamais).</para>
        </listItem>
        <listItem>
          <para>L’application de serveur a démarré, mais il y a un échec lors de l’initialisation qui l’a empêché de s’inscrire avec le mappeur de point de terminaison RPC (autrement dit, étape 1 de la &quot;plus d’informations&quot; diagramme ci-dessus a été tenté mais a échoué).</para>
        </listItem>
        <listItem>
          <para>L’application serveur a démarré mais a été arrêtée par la suite. (par exemple, étape 1 de la &quot;plus d’informations&quot; diagramme ci-dessus s’est déroulé avec succès, mais a été annulée ultérieurement, car le serveur a été arrêtée).</para>
        </listItem>
        <listItem>
          <para>L’application serveur désinscrit manuellement ses points de terminaison (semblables à 3, mais il est intentionnel. Mais probablement pas incluse par souci d’exhaustivité.)</para>
        </listItem>
        <listItem>
          <para>Le client RPC (DC de destination) de contacter un autre serveur RPC celui prévu en raison d’un nom de l’erreur de mappage d’adresse IP dans DNS, WINS ou le fichier hôte/Lmhosts.</para>
        </listItem>
      </list>
      <para>Erreur 1753 n’est pas due à : </para>
      <list class="bullet">
        <listItem>
          <para>Un manque de connectivité réseau entre le client RPC (DC de destination) et le serveur RPC (contrôleur de domaine source) sur le port 135</para>
        </listItem>
        <listItem>
          <para>Un manque de connectivité réseau entre le serveur RPC (contrôleur de domaine source) via le port 135 et le client RPC (DC de destination) via le port éphémère. </para>
        </listItem>
        <listItem>
          <para>Une incompatibilité de mot de passe ou l’impossibilité de déchiffrer un paquet chiffré Kerberos par le contrôleur de domaine source </para>
        </listItem>
      </list>
      <para> </para>
    </content>
  </section>
  <section address="BKMK_Resolutions">
    <title>Résolutions</title>
    <content>
      <list class="ordered">
        <listItem>
          <para>
            <embeddedLabel>Vérifiez que le service de l’inscription de son service avec le mappeur de point de terminaison a démarré.</embeddedLabel>
          </para>
          <para>Pour Windows 2000 et les contrôleurs de domaine Windows Server 2003 : Assurez-vous que le contrôleur de domaine source est démarré en mode normal. </para>
          <para>
Pour Windows Server 2008 ou Windows Server 2008 R2 : à partir de la console du contrôleur de domaine source, démarrez le Gestionnaire de Services (services.msc) et vérifiez que le <embeddedLabel>Active Directory Domain Services</embeddedLabel> service est en cours d’exécution. </para>
        </listItem>
        <listItem>
          <para>
            <embeddedLabel>Vérifiez que le client RPC (DC de destination) connecté au serveur RPC prévue (contrôleur de domaine source)</embeddedLabel>
          </para>
          <para>Tous les contrôleurs de domaine dans une forêt Active Directory commune inscrire un enregistrement CNAME du contrôleur de domaine dans la zone _msdcs. &lt;domaine racine de forêt&gt; zone DNS, quel que soit le quel domaine qu’ils résident dans la forêt. L’enregistrement CNAME du contrôleur de domaine est dérivé le <embeddedLabel>objectGUID</embeddedLabel> attribut de l’objet Paramètres NTDS pour chaque contrôleur de domaine. </para>
          <para>Lorsque vous effectuez des opérations basées sur la réplication, une contrôleur de domaine de destination demande à DNS l’enregistrement CNAME de contrôleurs de domaine source. L’enregistrement CNAME contient le nom d’ordinateur complet de contrôleur de domaine source qui est utilisé pour dériver l’adresse IP des contrôleurs de domaine source par le biais de recherche dans le cache client DNS, héberger /file LMHost recherche, l’hôte A / AAAA enregistrer dans DNS ou WINS. </para>
          <para>Périmées objets Paramètres NTDS et des mappages nom-IP incorrectes dans les fichiers DNS, WINS, hôte et LMHOST peuvent entraîner le client RPC (DC de destination) pour se connecter au serveur RPC incorrect (contrôleur de domaine Source). En outre, le mappage de nom IP incorrect peut entraîner le client RPC (DC de destination) pour se connecter à un ordinateur qui n’a pas encore de l’Application de serveur RPC d’intérêt (dans ce cas, le rôle d’Active Directory) est installé. (Exemple : un enregistrement de l’ordinateur hôte périmés pour DC2 contient l’adresse IP de DC3 ou sur un ordinateur membre). </para>
          <para>Vérifiez que l’attribut objectGUID pour le contrôleur de domaine source qui existe dans la destination de copie des contrôleurs de domaine d’Active Directory correspond à l’objectGUID de contrôleur de domaine source stockée dans la source de copie de contrôleurs de domaine d’Active Directory. S’il existe une incohérence, utilisez repadmin /showobjmeta sur l’objet paramètres ntds pour voir celui qui correspond à la dernière promotion de contrôleur de domaine source (indicateur : comparer les cachets de date pour la date de création de l’objet Paramètres NTDS de /showobjmeta par rapport à la dernière date de promotion dans le fichier dcpromo.log de contrôleurs de domaine source. Vous devrez peut-être utiliser la dernière modification / date de création de la DCPROMO. Fichier journal lui-même). Si l’objet GUID n’est pas identique, le DC susceptible de destination a un objet Paramètres NTDS obsolète pour le contrôleur de domaine source dont l’enregistrement CNAME fait référence à un enregistrement d’hôte avec un nom incorrect pour le mappage IP. </para>
          <para>Sur le DC de destination, exécutez IPCONFIG /ALL pour déterminer quels serveurs DNS la destination à l’aide de contrôleur de domaine pour la résolution de noms :</para>
          <code>c:&gt;ipconfig /all</code>
          <para>Sur le DC de destination, exécutez NSLOOKUP sur l’enregistrement CNAME du contrôleur de domaine complet de contrôleurs de domaine source :</para>
          <code>c:&gt;nslookup -type=cname &lt;fully qualified cname of source DC&gt; &lt;destination DCs primary DNS Server IP &gt;
c:&gt;nslookup -type=cname &lt;fully qualified cname of source DC&gt; &lt;destination DCs secondary DNS Server IP&gt;</code>
          <para>Vérifiez que l’adresse IP renvoyée par NSLOOKUP &quot;possède&quot; le nom d’hôte / identité de sécurité de contrôleur de domaine source :</para>
          <code>C:&gt;NBTSTAT -A &lt;IP address returned by NSLOOKUP in the step above&gt;</code>
          <para>ou</para>
          <para>Connectez-vous à la console de la source de contrôleur de domaine, exécutez &quot;IPCONFIG&quot; à partir de la commande invite et vérifier que le contrôleur de domaine source possède l’adresse IP renvoyée par la commande NSLOOKUP ci-dessus</para>
          <para>Recherchez un hôte obsolète / en double pour les mappages d’adresses IP dans DNS</para>
          <code>NSLOOKUP -type=hostname &lt;single label hostname of source DC&gt; &lt;primary DNS Server IP on destination DC&gt;
NSLOOKUP -type=hostname &lt;single label hostname of source DC&gt; &lt;secondary DNS Server IP on destination DC&gt;

NSLOOKUP -type=hostname &lt;fully qualified computer name of source DC&gt; &lt;primary DNS Server IP on destination DC&gt;
NSLOOKUP -type=hostname &lt;fully qualified computer name of source DC&gt; &lt;secondary DNS Server IP on dest. DC&gt;</code>
<para>S’il existe des adresses IP non valides dans les enregistrements d’hôte, examinez si le nettoyage DNS est activé et correctement configuré. </para><para>Si les tests ci-dessus ou un réseau de trace ne&#39;afficher une requête de nom qui renvoie une adresse IP non valide, envisagez les entrées obsolètes dans les fichiers d’hôte, les fichiers LMHOSTS et les serveurs WINS. Notez que les serveurs DNS peuvent également être configurés pour effectuer une résolution de nom de secours de WINS.</para>
</listItem>
        <listItem>
          <para>
            <embeddedLabel>Vérifier que l’application serveur (Active Directory et coll.) a inscrit avec le mappeur de point de terminaison sur le serveur RPC (contrôleur de domaine source)</embeddedLabel>
          </para>
          <para>Active Directory utilise une combinaison de ports bien connus et inscrites dynamiquement. Ce tableau répertorie les ports bien connus et les protocoles utilisés par les contrôleurs de domaine Active Directory.</para>
          <table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11">
            <thead>
              <tr>
                <TD>
                  <para>Application de serveur RPC</para>
                </TD>
                <TD>
                  <para>Port</para>
                </TD>
                <TD>
                  <para>TCP</para>
                </TD>
                <TD>
                  <para>UDP</para>
                </TD>
              </tr>
            </thead>
            <tbody>
              <tr>
                <TD>
                  <para>Serveur DNS</para>
                </TD>
                <TD>
                  <para>53</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>Kerberos</para>
                </TD>
                <TD>
                  <para>88</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>Serveur LDAP</para>
                </TD>
                <TD>
                  <para>389</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>Microsoft-DS</para>
                </TD>
                <TD>
                  <para>445</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>SSL LDAP</para>
                </TD>
                <TD>
                  <para>636</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>Serveur du catalogue global</para>
                </TD>
                <TD>
                  <para>3268</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para />
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>Serveur du catalogue global</para>
                </TD>
                <TD>
                  <para>3269</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para />
                </TD>
              </tr>
            </tbody>
          </table>
          <para>Les ports connus ne sont pas inscrits avec le mappeur de point de terminaison. </para>
          <para>Active Directory et autres applications également inscrivent les services qui reçoivent des ports affectés dynamiquement dans la plage de ports éphémères de RPC. Ces applications de serveur RPC sont attribuées dynamiquement des ports TCP entre 1024 et 5000 sur les ordinateurs Windows 2000 et Windows Server 2003 et les ports entre 49152 et 65535 plage sur les ordinateurs Windows Server 2008 et Windows Server 2008 R2. Le port RPC utilisé par la réplication peut être codé en dur dans le Registre à l’aide de la procédure décrite dans <externalLink> <linkText>l’article 224196</linkText> <linkUri> <a href="https://support.microsoft.com/kb/224196" data-raw-source="https://support.microsoft.com/kb/224196"> https://support.microsoft.com/kb/224196 </a> </linkUri> </externalLink>. Active Directory continue à s’inscrire avec EPM lorsque configuré pour utiliser un port codé en dur. </para>
          <para>Vérifiez que l’application serveur RPC d’intérêt s’est inscrit avec le mappeur de point de terminaison RPC sur le serveur RPC (contrôleur de domaine source dans le cas de la réplication Active Directory). </para>
          <para>Il existe plusieurs façons d’accomplir cette tâche, mais une consiste à installer et exécuter PORTQRY à partir d’une invite de commandes administrateur privilégié sur la console de la source de contrôleur de domaine à l’aide de la syntaxe : </para>
          <code>c:&amp;gt;portquery -n &lt;source DC&gt; -e 135 &gt;file.txt</code>
          <para>Dans la sortie de portqry, notez les numéros de port inscrits dynamiquement par le &quot;MS NT Directory DRS Interface&quot; (UUID =... 351) pour le <embeddedLabel>ncacn_ip_tcp protocole</embeddedLabel>. L’extrait de code ci-dessous illustre une sortie portquery à partir d’un contrôleur de domaine Windows Server 2008 R2 et l’UUID / paire protocole spécifiquement utilisés par Active Directory mis en surbrillance dans <embeddedLabel>gras</embeddedLabel>: </para>
          <code>UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_np:CONTOSO-DC01[\pipe\lsass] 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_np:CONTOSO-DC01[\PIPE\protected_storage] 
<codeFeaturedElement>UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_ip_tcp:CONTOSO-DC01[49156]</codeFeaturedElement> 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_http:CONTOSO-DC01[49157] 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_http:CONTOSO-DC01[6004]</code>
          <para />
        </listItem>
        <listItem>
          <para>Autres méthodes permettant de résoudre cette erreur :</para>
          <list class="ordered">
            <listItem>
              <para>Vérifiez que le contrôleur de domaine source est démarré en mode normal et que le rôle de système d’exploitation et le contrôleur de domaine sur le contrôleur de domaine source ont entièrement démarré.</para>
            </listItem>
            <listItem>
              <para>Vérifiez que le Service de domaine Active Directory est en cours d’exécution. Si le service est actuellement arrêté ou n’a pas été configuré avec les valeurs de démarrage par défaut, réinitialisez les valeurs de démarrage par défaut, redémarrez le contrôleur de domaine modifié, puis recommencez l’opération.</para>
            </listItem>
            <listItem>
              <para>Vérifiez que l’état de valeur et le service de démarrage pour le service RPC et le Localisateur RPC est correct pour la version du système d’exploitation du Client RPC (DC de destination) et du serveur RPC (contrôleur de domaine source). Si le service est actuellement arrêté ou n’a pas été configuré avec les valeurs de démarrage par défaut, réinitialisez les valeurs de démarrage par défaut, redémarrez le contrôleur de domaine modifié, puis recommencez l’opération.</para>
              <para>En outre, assurez-vous que le contexte de service correspond aux paramètres par défaut répertoriés dans le tableau suivant.</para>
              <table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11">
                <thead>
                  <tr>
                    <TD>
                      <para>Service</para>
                    </TD>
                    <TD>
                      <para>État par défaut (type de démarrage) dans Windows Server 2003 et versions ultérieures </para>
                    </TD>
                    <TD>
                      <para>État par défaut (type de démarrage) dans Windows Server 2000</para>
                    </TD>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <TD>
                      <para>Appel de procédure distante</para>
                    </TD>
                    <TD>
                      <para>Démarré (automatique)</para>
                    </TD>
                    <TD>
                      <para>Démarré (automatique)</para>
                    </TD>
                  </tr>
                  <tr>
                    <TD>
                      <para>Localisateur d’appel de procédure distante</para>
                    </TD>
                    <TD>
                      <para>NULL ou arrêté (manuel)</para>
                    </TD>
                    <TD>
                      <para>Démarré (automatique)</para>
                    </TD>
                  </tr>
                </tbody>
              </table>
            </listItem>
            <listItem>
              <para>Vérifiez que la taille de la plage de ports dynamiques n’a pas été contraint. La syntaxe de Windows Server 2008 et Windows Server 2008 R2 NETSH pour énumérer la plage de ports RPC est indiquée ci-dessous :</para>
              <code>&gt;netsh int ipv4 show dynamicport tcp
&gt;netsh int ipv4 show dynamicport udp
&gt;netsh int ipv6 show dynamicport tcp
&gt;netsh int ipv6 show dynamicport udp</code>
            </listItem>
            <listItem>
              <para>Vérifiez que les définitions de port codé en dur définies dans la base de connaissances 224196 tombent dans la plage de ports dynamiques pour la version de système d’exploitation des contrôleurs de domaine source.</para>
              <para>Révision <externalLink> <linkText>l’article 224196</linkText> <linkUri> <a href="https://support.microsoft.com/kb/224196" data-raw-source="https://support.microsoft.com/kb/224196"> https://support.microsoft.com/kb/224196 </a> </linkUri> </externalLink> et vérifiez que le port codé en dur est comprise dans la plage de ports éphémères pour le contrôleur de domaine source&#39;version de système d’exploitation de s.</para>
            </listItem>
            <listItem>
              <para>Vérifiez que la clé ClientProtocols existe sous HKLM\Software\Microsoft\Rpc et contient les valeurs par 5 défaut suivantes :</para>
              <code>ncacn_http REG_SZ rpcrt4.dll
ncacn_ip_tcp REG_SZ rpcrt4.dll
<codeFeaturedElement>ncacn_nb_tcp REG_SZ rpcrt4.dll</codeFeaturedElement>
ncacn_np REG_SZ rpcrt4.dll
ncacn_ip_udp REG_SZ rpcrt4.dll</code>
            </listItem>
          </list>
        </listItem>
      </list>
    </content>
  </section>
  <section address="BKMK_MoreInfo">
    <title>Plus d’informations</title>
    <content>
      <para>
        <embeddedLabel>Exemple d’un nom incorrect pour l’IP mappage à l’origine de l’erreur RPC 1753 et-2146893022 : le nom du principal cible est incorrect</embeddedLabel>
      </para>
      <para>Le domaine contoso.com se compose de DC1 et DC2 avec x.x.1.1 d’adresses IP et x.x.1.2. L’hôte &quot;A&quot; / &quot;AAAA&quot; enregistrements pour DC2 sont inscrits correctement sur tous les serveurs DNS configurés pour DC1. En outre, le fichier HOSTS sur DC1 contient une entrée de mappage de nom d’hôte entièrement qualifié de DC2s à x.x.1.2 d’adresse IP. Plus tard, DC2&#39;adresse IP par X.X.1.2 X.X.1.3 et un nouvel ordinateur membre est joint au domaine avec x.x.1.2 d’adresse IP. La réplication Active Directory tentatives déclenchement par le <ui>Répliquer maintenant</ui> commande dans le composant logiciel enfichable Services et Sites Active Directory échoue avec l’erreur 1753, comme indiqué dans la trace ci-dessous :</para>
      <code>F# SRC    DEST    Operation 
1 x.x.1.1 x.x.1.2 ARP:Request, x.x.1.1 asks for x.x.1.2
2 x.x.1.2 x.x.1.1 ARP:Response, x.x.1.2 at 00-13-72-28-C8-5E
3 x.x.1.1 x.x.1.2 TCP:Flags=......S., SrcPort=50206, DstPort=DCE endpoint resolution(135)
4 x.x.1.2 x.x.1.1 ARP:Request, x.x.1.2 asks for x.x.1.1
5 x.x.1.1 x.x.1.2 ARP:Response, x.x.1.1 at 00-15-5D-42-2E-00
6 x.x.1.2 x.x.1.1 TCP:Flags=...A..S., SrcPort=DCE endpoint resolution(135)
7 x.x.1.1 x.x.1.2 TCP:Flags=...A...., SrcPort=50206, DstPort=DCE endpoint resolution(135)
8 x.x.1.1 x.x.1.2 MSRPC:c/o Bind: UUID{E1AF8308-5D1F-11C9-91A4-08002B14A0FA} EPT(EPMP) 
9 x.x.1.2 x.x.1.1 MSRPC:c/o Bind Ack: Call=0x2 Assoc Grp=0x5E68 Xmit=0x16D0 Recv=0x16D0 
<codeFeaturedElement>10</codeFeaturedElement> x.x.1.1 x.x.1.2 EPM:Request: ept_map: NDR, DRSR(DRSR) {E3514235-4B06-11D1-AB04-00C04FC2DCD2} [DCE endpoint resolution(135)]
<codeFeaturedElement>11</codeFeaturedElement> x.x.1.2 x.x.1.1 EPM:Response: ept_map: 0x16C9A0D6 - EP_S_NOT_REGISTERED
</code>
      <para>Image <embeddedLabel>10</embeddedLabel>, le DC de destination interroge le mappeur de point de terminaison de contrôleurs de domaine source sur le port 135 pour la classe de service de réplication Active Directory UUID E351... </para>
      <para>Dans le cadre <embeddedLabel>11</embeddedLabel>, la source de contrôleur de domaine, dans ce cas, un ordinateur membre qui n’héberge pas encore le rôle de contrôleur de domaine et n’a donc pas enregistré la E351... UUID pour le service de réplication avec ses EPM local répond avec l’erreur symbolique EP_S_NOT_REGISTERED qui mappe à l’erreur décimal 1753, hex 0x6d9 et erreur convivial &quot;aucun point de terminaison plus ne sont disponibles dans le mappeur de point de terminaison&quot; .</para>
      <para>Une version ultérieure, l’ordinateur membre avec x.x.1.2 d’adresse IP est promue en tant que réplica &quot;MayberryDC&quot; dans le domaine contoso.com. Là encore, le <ui>Répliquer maintenant</ui> commande est utilisée pour déclencher la réplication, mais cette fois échoue avec l’à l’écran erreur &quot;le nom du principal cible est incorrect.&quot; L’ordinateur dont la carte réseau est affectée l’adresse IP d’adresses x.x.1.2 <placeholder>est</placeholder> un contrôleur de domaine est actuellement démarré en mode normal et a enregistré le E351... le service de réplication UUID avec son EPM local, mais il ne possède pas l’identité de nom ou la sécurité de DC2 et ne peut pas déchiffrer la requête Kerberos de DC1 la demande échoue désormais avec l’erreur &quot;le nom du principal cible est incorrect.&quot; L’erreur est mappé à l’erreur décimal-2146893022 / hex erreur 0x80090322. </para>
      <para>Ces mappages IP de l’hôte non valides peut être dû à des entrées obsolètes dans hôte / fichiers lmhost, héberger un / des enregistrements AAAA dans DNS ou WINS. </para>
      <para>Résumé : Cet exemple a échoué, car un mappage d’adresse IP hôte non valide (dans le fichier d’hôte dans le cas présent) a provoqué la destination du contrôleur de domaine à résoudre en un &quot;source&quot; contrôleur de domaine qui n’avait pas l’exécution du service Active Directory Domain Services (ou même installés pour ailleurs) pour la réplication SPN n’a pas encore été inscrit et le contrôleur de domaine source a renvoyé erreur 1753. Dans le deuxième cas, un mappage d’adresse IP hôte non valide (dans le fichier hôte) a provoqué la destination du contrôleur de domaine pour se connecter à un contrôleur de domaine qui a inscrit la E351... nom principal de service de réplication, mais cette source avait une autre identité de nom d’hôte et de sécurité que le contrôleur de domaine source prévue afin des tentatives a échoué avec l’erreur-2146893022 : Le nom de principal de cible est incorrect.</para>
    </content>
  </section>
  <relatedTopics>
    <externalLink>
      <linkText>Résolution des problèmes d’opérations Active Directory qui échouent avec l’erreur 1753 : Aucun point de terminaison plus ne sont disponibles sur l’endpoint mapper. </linkText> 
      <linkUri> <a href="https://support.microsoft.com/kb/2089874" data-raw-source="https://support.microsoft.com/kb/2089874"> https://support.microsoft.com/kb/2089874 </a> </linkUri> 
    </externalLink> 
<externalLink> <linkText>Ko article 839880 erreurs de résolution des problèmes de mappeur de point de terminaison RPC à l’aide de la prise en charge de Windows Server 2003 Outils à partir du CD du produit</linkText><linkUri><a href="https://support.microsoft.com/kb/839880" data-raw-source="https://support.microsoft.com/kb/839880">https://support.microsoft.com/kb/839880</a></linkUri></externalLink>
<externalLink><linkText>Ko article 832017 réseau et la vue d’ensemble du port du Service configuration requise pour le système de Windows Server</linkText><linkUri><a href="https://support.microsoft.com/kb/832017/" data-raw-source="https://support.microsoft.com/kb/832017/">https://support.microsoft.com/kb/832017/</a></linkUri></externalLink>
<externalLink><linkText>Ko article 224196 restriction Active Le trafic de réplication de répertoire et de client RPC le trafic vers un port spécifique</linkText><linkUri><a href="https://support.microsoft.com/kb/224196/" data-raw-source="https://support.microsoft.com/kb/224196/">https://support.microsoft.com/kb/224196/</a></linkUri></externalLink>
<externalLink><linkText>article de la base de connaissances 154596 comment configurer l’allocation de port dynamique RPC avec un pare-feu</linkText><linkUri><a href="https://support.microsoft.com/kb/154596" data-raw-source="https://support.microsoft.com/kb/154596">https://support.microsoft.com/kb/154596</a></linkUri></externalLink><externalLink><linkText>comment RPC Works</linkText><linkUri><a href="https://msdn.microsoft.com/library/aa373935(VS.85).aspx" data-raw-source="https://msdn.microsoft.com/library/aa373935(VS.85).aspx">https://msdn.microsoft.com/library/aa373935(VS.85).aspx</a></linkUri></externalLink><externalLink><linkText>comment le serveur se prépare pour une connexion</linkText> <linkUri> <a href="https://msdn.microsoft.com/library/aa373938(VS.85).aspx" data-raw-source="https://msdn.microsoft.com/library/aa373938(VS.85).aspx"> https://msdn.microsoft.com/library/aa373938(VS.85).aspx </a> </linkUri> </externalLink> 
<externalLink> <linkText>Comment le Client établit une connexion</linkText> <linkUri> <a href="https://msdn.microsoft.com/library/aa373937(VS.85).aspx" data-raw-source="https://msdn.microsoft.com/library/aa373937(VS.85).aspx"> https://msdn.microsoft.com/library/aa373937(VS.85).aspx </a> </linkUri> </externalLink> <externalLink> <linkText>L’inscription de l’Interface</linkText> <linkUri> <a href="https://msdn.microsoft.com/library/aa375357(VS.85).aspx" data-raw-source="https://msdn.microsoft.com/library/aa375357(VS.85).aspx"> https://msdn.microsoft.com/library/aa375357(VS.85).aspx </a> </linkUri> </externalLink> <externalLink> <linkText>Rendre le serveur soit disponible sur le réseau</linkText> <linkUri> <a href="https://msdn.microsoft.com/library/aa373974(VS.85).aspx" data-raw-source="https://msdn.microsoft.com/library/aa373974(VS.85).aspx"> https://msdn.microsoft.com/library/aa373974(VS.85).aspx </a> </linkUri> </externalLink> <externalLink> <linkText>L’enregistrement des points de terminaison</linkText> <linkUri> <a href="https://msdn.microsoft.com/library/aa375255(VS.85).aspx" data-raw-source="https://msdn.microsoft.com/library/aa375255(VS.85).aspx"> https://msdn.microsoft.com/library/aa375255(VS.85).aspx </a> </linkUri> </externalLink> <externalLink> <linkText>à l’écoute pour les appels clients</linkText> <linkUri> <a href="https://msdn.microsoft.com/library/aa373966(VS.85).aspx" data-raw-source="https://msdn.microsoft.com/library/aa373966(VS.85).aspx"> https://msdn.microsoft.com/library/aa373966(VS.85).aspx </a> </linkUri> </externalLink> <externalLink> <linkText>Comment le Client établit une connexion</linkText><linkUri><a href="https://msdn.microsoft.com/library/aa373937(VS.85).aspx" data-raw-source="https://msdn.microsoft.com/library/aa373937(VS.85).aspx">https://msdn.microsoft.com/library/aa373937(VS.85).aspx</a></linkUri></externalLink><span class=" class=""></span class="></linkText><linkUri><a href="https://msdn.microsoft.com/library/dd207688(PROT.13).aspx" data-raw-source="https://msdn.microsoft.com/library/dd207688(PROT.13).aspx">https://msdn.microsoft.com/library/dd207688(PROT.13).aspx</a></linkUri></externalLink></relatedTopics>
</developerConceptualDocument>


