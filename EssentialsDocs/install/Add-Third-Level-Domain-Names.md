---
title: Ajout des noms de domaine de troisième niveau
description: Décrit comment utiliser Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: e5b4a362-1881-4024-ae4e-cc3b05e50103
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 659844ce4da6a7ec6311b36b4516875ed468733e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80817552"
---
# <a name="add-third-level-domain-names"></a>Ajout des noms de domaine de troisième niveau

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Vous pouvez donner aux utilisateurs la possibilité de demander des noms de domaine de troisième niveau dans l'Assistant Configuration du nom de domaine. Il suffit, pour cela, de créer et d'installer un assembly de code qui sera utilisé par le Gestionnaire de domaine dans le système d'exploitation.  
  
## <a name="create-a-provider-of-third-level-domain-names"></a>Création d'un fournisseur de noms de domaine de troisième niveau  
 Vous pouvez mettre des noms de domaine de troisième niveau à la disposition des utilisateurs en créant et en installant un assembly de code qui fournira les noms de domaine à l'assistant. Pour ce faire, effectuez les tâches suivantes :  
  
-   [Ajouter une implémentation de l’interface IDomainSignupProvider à l’assembly](Add-Third-Level-Domain-Names.md#BKMK_DomainSignup)  
  
-   [Ajouter une implémentation de l’interface IDomainMaintenanceProvider à l’assembly](Add-Third-Level-Domain-Names.md#BKMK_DomainMaintenance)  
  
-   [Signer l’assembly avec une signature Authenticode](Add-Third-Level-Domain-Names.md#BKMK_SignAssembly)  
  
-   [Installer l’assembly sur l’ordinateur de référence](Add-Third-Level-Domain-Names.md#BKMK_InstallAssembly)  
  
-   [Redémarrer le service de gestion de noms de domaine Windows Server](Add-Third-Level-Domain-Names.md#BKMK_RestartService)  
  
###  <a name="add-an-implementation-of-the-idomainsignupprovider-interface-to-the-assembly"></a><a name="BKMK_DomainSignup"></a>Ajouter une implémentation de l’interface IDomainSignupProvider à l’assembly  
 L'interface IDomainSignupProvider sert à ajouter des offres de domaine à l'Assistant.  
  
##### <a name="to-add-the-idomainsignupprovider-code-to-the-assembly"></a>Pour ajouter le code IDomainSignupProvider à l'assembly  
  
1.  Ouvrez Visual Studio 2008 en tant qu’administrateur en cliquant avec le bouton droit sur le programme dans le menu **Démarrer**, puis en sélectionnant **Exécuter en tant qu’administrateur**.  
  
2.  Cliquez sur **Fichier**, sur **Nouveau**, puis sur **Projet**.  
  
3.  Dans la boîte de dialogue **Nouveau projet**, cliquez sur **Visual C#** puis sur **Bibliothèque de classes**, donnez un nom à la solution, puis cliquez sur **OK**.  
  
4.  Renommez le fichier Class1.cs. Appelez-le, par exemple, MyDomainNameProvider.cs.  
  
5.  Ajoutez des références aux fichiers Wssg.Web.DomainManagerObjectModel.dll, CertManaged.dll, WssgCertMgmt.dll et WssgCommon.dll.  
  
6.  Ajoutez les instructions d'utilisation suivantes.  
  
    ```c#  
  
    using System.Collections.ObjectModel;  
    using System.Net;  
    using System.Net.Sockets;  
    using Microsoft.WindowsServerSolutions.Certificates;  
    using Microsoft.WindowsServerSolutions.CertificateManagement;  
    using Microsoft.WindowsServerSolutions.Common;  
    using Microsoft.Win32;  
    ```  
  
7.  Changez l'espace de noms et l'en-tête de classe pour qu'ils correspondent à l'exemple suivant.  
  
    ```c#  
    namespace Microsoft.WindowsServerSolutions.RemoteAccess.Domains  
    {  
        public class MyDomainNameProvider : IDomainSignupProvider  
        {  
        }  
    }  
    ```  
  
8.  Ajoutez la méthode Initialize et les variables requises à la classe afin de définir les offres présentées dans l'Assistant.  
  
    > [!NOTE]
    >  La méthode Initialize définit un identificateur pour le fournisseur de domaines. Cet identificateur doit être unique. Le moyen le plus simple pour s'en assurer est de choisir un GUID en guise d'identificateur. Pour plus d’informations sur la création d’un GUID, consultez [Créer un GUID (guidgen.exe)](https://go.microsoft.com/fwlink/?LinkId=116098).  
  
     L'exemple de code suivant affiche la méthode Initialize.  
  
    ```c#  
  
    static readonly Guid MyID = new Guid("8C999DF5-696A-47af-822D-94F1673D3397");  
    public Guid ID { get { return MyID; } }  
    public string Name { get { return "My Provider"; } }  
    List<Offering> offerings = new List<Offering>();  
  
    public void Initialize(DomainProviderSettings config)  
    {  
       var offer1 = new Offering()  
       {  
          Description = "My Domain Provider",  
          Name = "Offering 1",  
          ProviderID = ID,  
          MoreInfoUrl = new Uri("http://www.contoso.com"),  
          MembershipServiceName = "My Membership",  
          EulaUrl = new Uri("http://www.contoso.com"),  
       };  
  
       this.offerings.Add(offer1);  
       RegistryKey key =   
          Registry.LocalMachine.CreateSubKey(@"Software\Microsoft\Windows Server\Domain Manager\Settings");  
       key.Close();  
    }  
    ```  
  
9. Ajoutez la méthode GetOfferings afin de retourner la liste des offres initialisée au cours de l'étape précédente. L'exemple de code suivant affiche la méthode GetOfferings.  
  
    ```c#  
  
    public ReadOnlyCollection<Offering> GetOfferings()  
    {  
       return this.offerings.AsReadOnly();  
    }  
    ```  
  
10. Ajoutez la méthode FindOfferingForDomain afin de renvoyer l'offre de la liste. L'exemple de code suivant affiche la méthode FindOfferingForDomain.  
  
    ```  
  
    public Offering FindOfferingForDomain(string domain)  
    {  
       // Return the offering that has the domain name.  
       return offerings[0];  
    }  
  
    ```  
  
11. Ajoutez la méthode SetCredentials afin de définir les informations d'identification nécessaires pour accéder aux offres. L'exemple de code suivant affiche la méthode SetCredentials.  
  
    ```c#  
  
    private string currentUser { get; set; }  
    private string currentPassword { get; set; }  
  
    public bool SetCredentials(DomainNameRequest request,   
       DomainProviderCredentials credentials, bool validate)  
    {  
       currentUser = credentials.UserName;  
       currentPassword = credentials.Password;  
       if (validate)  
       {  
          return ValidateCredentials();  
       }  
  
       return true;  
    }  
    ```  
  
12. Ajoutez la méthode ValidateCredentials afin de valider les informations d'identification définies par SetCredentials. L'exemple de code suivant affiche la méthode ValidateCredentials.  
  
    ```c#  
  
    public static readonly string offerUser = "user1@contoso.com";  
    public static readonly string offerPassword = "password1";  
  
    public bool ValidateCredentials()  
    {  
       if (IsUser())  
          return string.Equals(currentPassword, offerPassword);  
       else  
          return false;  
    }   
  
    private bool IsUser()  
    {  
       return string.Equals(currentUser, offerUser, StringComparison.OrdinalIgnoreCase);  
    }  
    ```  
  
13. Ajoutez la méthode GetAvailableDomainRoots afin de renvoyer la liste des noms de domaine racine gérés par l'offre spécifiée dans la requête. La liste des noms de domaine racine ne doit pas être vide. L'exemple de code suivant affiche la méthode GetAvailableDomainRoots.  
  
    ```c#  
  
    public ReadOnlyCollection<string> GetAvailableDomainRoots(DomainNameRequest request)  
    {  
       List<string> list = new List<string>();  
       list.Add("domain1.com");  
       list.Add("domain1.org");  
  
       return list.AsReadOnly();  
    }  
    ```  
  
14. Ajoutez la méthode GetUserDomainNames afin de renvoyer la liste des noms de domaine déjà détenus par l'utilisateur actuel dans le cadre de l'offre en cours. Cette liste peut être vide. L'exemple de code suivant affiche la méthode GetUserDomainNames.  
  
    ```c#  
  
    public static readonly string AvailableDomain1 = "available.domain1.com",  
       AvailableDomain2 = "available.domain2.com";  
    public static readonly string OccupiedDomain1 = "occupied.domain1.com",  
       OccupiedDomain2 = "occupied.domain2.com";  
  
    public ReadOnlyCollection<string> GetUserDomainNames(DomainNameRequest request)  
    {  
       var userDomains = new List<string>();  
       userDomains.Add(OccupiedDomain1);  
       userDomains.Add(AvailableDomain1);  
  
       return userDomains.AsReadOnly();  
    }  
    ```  
  
15. Ajoutez la méthode GetUserDomainQuota afin de renvoyer le nombre maximum de domaines autorisé par l'offre spécifiée. Si aucun maximum n'est applicable, cette méthode renvoie 0. L'exemple de code suivant affiche la méthode GetUserDomainQuota.  
  
    ```c#  
  
    public int GetUserDomainQuota(DomainNameRequest request)  
    {  
       return 0;  
    }  
    ```  
  
16. Ajoutez la méthode CheckDomainAvailability afin de vérifier si le nom de domaine demandé est disponible et s'il est possible d'obtenir une liste de suggestions. L'exemple de code suivant affiche la méthode CheckDomainAvailability.  
  
    ```c#  
  
    public bool CheckDomainAvailability(DomainNameRequest request,   
       out ReadOnlyCollection<string> suggestions)  
    {  
       suggestions = null;  
  
       return true;  
    }  
    ```  
  
17. Ajoutez la méthode CommitDomain afin de valider le nom de domaine demandé. Pour que cette méthode ait des chances d'aboutir, le nom du domaine doit être associé au compte d'utilisateur. Le fournisseur chargé de la maintenance est contacté immédiatement afin de récupérer le certificat si l'état est opérationnel (FullyOperational). Le fournisseur et l'offre deviennent alors actifs. L'exemple de code suivant affiche la méthode CommitDomain.  
  
    ```c#  
  
    public DomainStatus CommitDomain(DomainNameRequest request)  
    {              
       ReadOnlyCollection<string> suggestions;  
       if (!CheckDomainAvailability(request, out suggestions))  
       {  
          throw new DomainException(FailureReason.InvalidDomainName, null, null);  
       }  
  
       return DomainStatus.Ready;  
    }  
    ```  
  
18. Ajoutez la méthode ReleaseDomain afin de prévenir le fournisseur que l'utilisateur souhaite publier le nom de domaine. L'exemple de code suivant affiche la méthode ReleaseDomain.  
  
    ```c#  
  
    public bool ReleaseDomain(DomainNameRequest request)  
    {  
       return true;  
    }  
    ```  
  
19. Ajoutez la méthode GetProviderLandingUrl afin de renvoyer l'adresse URL de la page d'arrivée dans le workflow d'inscription du domaine. L'exemple de code suivant affiche la méthode GetProviderLandingUrl.  
  
    ```c#  
  
    public Url GetProviderLandingUrl(DomainNameRequest request)  
    {  
       return new Url("www.contoso.com");  
    }  
    ```  
  
20. Ajoutez la méthode GetDomainMaintenanceProvider afin de renvoyer une instance de IDomainMaintenanceProvider destinée aux tâches de maintenance du domaine. Cette méthode est appelée au démarrage du Gestionnaire de domaine, une fois la méthode CommitDomain exécutée. L'exemple de code suivant affiche la méthode GetDomainMaintenanceProvider.  
  
    ```c#  
  
    public IDomainMaintenanceProvider GetDomainMaintenanceProvider()  
    {  
       return new MyDomainMaintenanceProvider();  
    }  
    ```  
  
21. Enregistrez le projet et laissez-le ouvert, car vous aurez l'occasion de le compléter au cours de la procédure suivante. Tant que vous n'aurez pas réalisé celle-ci, vous ne pourrez pas générer le projet.  
  
###  <a name="add-an-implementation-of-the-idomainmaintenanceprovider-interface-to-the-assembly"></a><a name="BKMK_DomainMaintenance"></a>Ajouter une implémentation de l’interface IDomainMaintenanceProvider à l’assembly  
 L'interface IDomainMaintenanceProvider assure la maintenance du domaine après sa création.  
  
##### <a name="to-add-the-idomainmaintenanceprovider-code-to-the-assembly"></a>Pour ajouter le code IDomainMaintenanceProvider à l'assembly  
  
1.  Ajoutez l'en-tête de classe pour le fournisseur chargé de la maintenance du domaine. Assurez-vous que le nom choisi pour le fournisseur correspond à celui indiqué dans la méthode GetDomainMaintenanceProvider définie précédemment.  
  
    ```c#  
  
    public class MyDomainMaintenanceProvider : IDomainMaintenanceProvider  
    {  
    }  
    ```  
  
2.  Ajoutez la méthode Activate afin de définir le fournisseur actif. L'exemple de code suivant affiche la méthode Activate.  
  
    ```c#  
  
    string DomainName { get; set; }  
    protected DomainProviderSettings Settings { get; set; }  
  
    public void Activate(DomainProviderSettings settings,   
       DomainNameConfiguration config, DomainProviderCredentials credentials)  
    {  
       Settings = settings;  
       SetCredentials(credentials);  
       DomainName = config.AutoConfiguredAnywhereAccessFullName.Punycode;  
    }  
    ```  
  
3.  Ajoutez la méthode Deactivate afin de désactiver toutes les actions. L'exemple de code suivant affiche la méthode Deactivate.  
  
    ```  
  
    public void Deactivate()  
    {  
       //Deactivate all actions  
    }  
  
    ```  
  
4.  Ajoutez la méthode SetCredentials afin de mettre à jour les informations d'identification de l'utilisateur. Il est possible, par exemple, d'appeler cette méthode pour mettre à jour des informations d'identification obsolètes. L'exemple de code suivant affiche la méthode SetCredentials.  
  
    ```c#  
  
    protected DomainProviderCredentials Credentials { get; set; }  
  
    public bool SetCredentials(DomainProviderCredentials credentials)  
    {  
       this.Credentials = credentials;  
  
       return true;  
    }  
    ```  
  
5.  Ajoutez la méthode ValidateCredentials afin de valider les informations d'identification spécifiées. L'exemple de code suivant affiche la méthode ValidateCredentials.  
  
    ```c#  
  
    public static readonly string offerUser = "user1@contoso.com";  
    public static readonly string offerPassword = "password1";  
  
    public bool ValidateCredentials()  
    {  
       if (string.Equals(this.Credentials.UserName,   
          offerUser,   
          StringComparison.OrdinalIgnoreCase) &&   
             string.Equals(this.Credentials.Password, offerPassword))  
          return true;  
  
       return false;  
    }  
    ```  
  
6.  Ajoutez la méthode GetPublicAddress afin de renvoyer l'adresse IP externe du serveur. L'exemple de code suivant affiche la méthode GetPublicAddress.  
  
    ```c#  
  
    public IPAddress GetPublicIPAddress()  
    {  
       string PublicIP = "0.0.0.0";  
       using (RegistryKey key = ProductInfo.RegKey.OpenSubKey("Domain Manager\\Settings", true))  
       {  
          PublicIP = (key == null) ? "0.0.0.0" : key.GetValue("PublicIP", "0.0.0.0").ToString();  
       }  
       IPAddress ip = IPAddress.Parse(PublicIP);  
  
       if (PublicIP == "0.0.0.0")  
       {  
          string strHostName = Dns.GetHostName();  
          IPHostEntry ipEntry = Dns.GetHostEntry(strHostName);  
  
          IPAddress[] addr = ipEntry.AddressList;  
          foreach (IPAddress add in addr)  
          {  
             if (add.AddressFamily == AddressFamily.InterNetwork)  
             {  
                return add;  
             }  
          }  
       }  
       else  
       {  
          return IPAddress.Parse(PublicIP);  
       }  
  
       return null;    
    }  
    ```  
  
7.  Ajoutez la méthode SubmitCertificateRequest afin de soumettre la demande de certificat pour le nom de domaine actuellement configuré.  
  
    ```c#  
  
    string cert=null;  
  
    public void SubmitCertificateRequest(string certificateRequest)  
    {  
       cert = CertManaged.SubmitRequest(certificateRequest, CertCommon.CAServerFQDN + "\\" +      
          CertCommon.CAName,   
          Microsoft.WindowsServerSolutions.CertificateManagement.CRFlags.Base64Header,   
          CertCommon.CATemplate,   
          EncodingFlags.Base64);  
    }  
    ```  
  
8.  Ajoutez la méthode GetCertificateResponse afin de renvoyer la réponse à propos du certificat à condition que l'état du domaine soit FullyOperational. Cette méthode concerne à la fois les demandes de création et les demandes de renouvellement de certificats. L'exemple de code suivant affiche la méthode GetCertificateResponse.  
  
    ```c#  
  
    public string GetCertificateResponse(bool renew)  
    {  
       return cert;  
    }  
    ```  
  
9. Ajoutez la méthode SubmitRenewCertificateRequest afin de traiter le renouvellement du certificat. L'exemple de code suivant affiche la méthode SubmitRenewCertificateRequest.  
  
    ```c#  
  
    public void SubmitRenewCertificateRequest()  
    {  
       // Add certificate renewal code   
    }  
    ```  
  
10. Ajoutez la méthode UpdateDNSRecords afin de mettre à jour les enregistrements DNS stockés par le fournisseur. L'exemple de code suivant affiche la méthode UpdateDNS.  
  
    ```c#  
  
    public bool UpdateDnsRecords(IList<DnsRecord> records)  
    {  
       string UpdateDNS = "true";  
       using (RegistryKey key = ProductInfo.RegKey.OpenSubKey("Domain Manager\\Settings", true))  
       {  
          UpdateDNS = (key == null) ? "true" : key.GetValue("UpdateDNS", "true").ToString();  
       }  
  
       return UpdateDNS == "true";  
    }  
  
    ```  
  
11. Ajoutez la méthode TestConnection pour essayer d'établir une connexion avec le service backend. Si cette méthode exige une authentification, une exception appropriée doit être levée. L'exemple de code suivant affiche la méthode TestConnection.  
  
    ```c#  
  
    public bool TestConnection()  
    {  
       // Add code to test connection  
  
       return true;  
    }  
    ```  
  
12. Ajoutez la méthode GetDomainState qui retourne l'état actuel du domaine. L'exemple de code suivant affiche la méthode GetDomainState.  
  
    ```c#  
  
    public DomainState GetDomainState()  
    {  
       string domainstatus = "FullyOperational";  
       long expirationDate = 365;  
       using (RegistryKey key = ProductInfo.RegKey.OpenSubKey("Domain Manager\\Settings", true))  
       {  
          domainstatus = (key == null) ? "Ready" : key.GetValue("DomainStatus", "Ready").ToString();  
          expirationDate = Int64.Parse(key.GetValue("ExpirationDate", "365").ToString());  
       }  
  
       switch (domainstatus)  
       {  
          case "Failed":  
             return new DomainState(DomainStatus.Failed,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "Ready":  
             return new DomainState(DomainStatus.Ready,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "InRenewal":  
             return new DomainState(DomainStatus.InRenewal,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "InRenewalCustomerInterventionRequired":  
             return new DomainState(DomainStatus.InRenewalCustomerInterventionRequired,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "Pending":  
             return new DomainState(DomainStatus.Pending,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "PendingCustomerInterventionRequired":  
             return new DomainState(DomainStatus.PendingCustomerInterventionRequired,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "RenewalFailed":  
             return new DomainState(DomainStatus.RenewalFailed,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          default:  
             return new DomainState(DomainStatus.Unknown,   
                null,   
                DateTime.Now.AddDays(expirationDate));                   
          }  
    }  
    ```  
  
13. Ajoutez la méthode GetCertificateState qui retourne l'état actuel du certificat. L'exemple de code suivant affiche la méthode GetCertificateState.  
  
    ```c#  
  
    public CertificateState GetCertificateState(bool renew)  
    {  
       return new CertificateState(CertificateStatus.Ready, string.Empty);  
    }  
    ```  
  
14. Enregistrez et générez la solution.  
  
###  <a name="sign-the-assembly-with-an-authenticode-signature"></a><a name="BKMK_SignAssembly"></a>Signer l’assembly avec une signature Authenticode  
 Vous devez signer l'assembly avec une signature Authenticode pour pouvoir l'utiliser dans le système d'exploitation. Pour plus d’informations sur la signature de l’assembly, consultez [Signature et vérification d’un code avec Authenticode](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode).  
  
###  <a name="install-the-assembly-on-the-reference-computer"></a><a name="BKMK_InstallAssembly"></a>Installer l’assembly sur l’ordinateur de référence  
 Placez l'assembly dans un dossier sur l'ordinateur de référence. Notez le chemin d'accès au dossier, car vous devrez entrer cette information dans le Registre au cours de la prochaine étape.  
  
### <a name="add-a-key-to-the-registry"></a>Ajout d'une clé au Registre  
 Il est nécessaire d'ajouter une entrée dans le Registre pour définir les caractéristiques et l'emplacement de l'assembly.  
  
##### <a name="to-add-a-key-to-the-registry"></a>Pour ajouter une clé au Registre  
  
1.  Sur l'ordinateur de référence, cliquez sur **Démarrer**, entrez **regedit**, puis appuyez sur **Entrée**.  
  
2.  Dans le volet gauche, développez successivement les entrées **HKEY_LOCAL_MACHINE**, **SOFTWARE**, **Microsoft**, **Windows Server**, **Domain Managers** et **Providers**.  
  
3.  Cliquez avec le bouton droit sur **Providers**, pointez sur **Nouveau**, puis cliquez sur **Clé**.  
  
4.  Saisissez l'identificateur du fournisseur en guise de nom de la clé. L'identificateur est le GUID que vous avez défini pour le fournisseur au cours de l'étape 8 de la rubrique [Ajout d'une implémentation de l'interface IDomainSignupProvider à l'assembly](Add-Third-Level-Domain-Names.md#BKMK_DomainSignup).  
  
5.  Cliquez avec le bouton droit sur la clé que vous venez de créer, puis cliquez sur **Nom de la valeur**.  
  
6.  Tapez **Assembly** comme nom de chaîne, puis appuyez sur **Entrée**.  
  
7.  Cliquez avec le bouton droit sur la nouvelle chaîne **Assembly** dans le volet droit, puis cliquez sur **Modifier**.  
  
8.  Indiquez le chemin d'accès complet au fichier d'assembly défini précédemment, puis cliquez sur **OK**.  
  
9. Cliquez de nouveau avec le bouton droit sur la clé, puis cliquez sur **Nom de la valeur**.  
  
10. Tapez **Enabled** en guise de nom de chaîne, puis appuyez sur **Entrée**.  
  
11. Cliquez avec le bouton droit sur la nouvelle chaîne **Enabled** dans le volet droit, puis cliquez sur **Modifier**.  
  
12. Tapez **True**, puis cliquez sur **OK**.  
  
13. Cliquez de nouveau avec le bouton droit sur la clé, puis cliquez sur **Nom de la valeur**.  
  
14. Tapez **Type** en guise de nom de chaîne, puis appuyez sur **Entrée**.  
  
15. Cliquez avec le bouton droit sur la nouvelle chaîne **Type** dans le volet droit, puis cliquez sur **Modifier**.  
  
16. Spécifiez le nom de classe complet de votre fournisseur défini dans l'assembly, puis cliquez sur **OK**.  
  
###  <a name="restart-the-windows-server-domain-name-management-service"></a><a name="BKMK_RestartService"></a>Redémarrer le service de gestion de noms de domaine Windows Server  
 Vous devez redémarrer le service Gestion de domaine Windows Server pour que le système d'exploitation ait accès au fournisseur.  
  
##### <a name="restart-the-service"></a>Redémarrer le service  
  
1.  Cliquez sur **Démarrer**, tapez **mmc**, puis appuyez sur **Entrée**.  
  
2.  Si le composant logiciel enfichable Services n'est pas répertorié dans la console, ajoutez-le en procédant de la façon suivante :  
  
    1.  Cliquez sur **Fichier**, puis cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**.  
  
    2.  Dans la liste **Composants logiciels enfichables disponibles**, cliquez sur **Services**, puis sur **Ajouter**.  
  
    3.  Dans la boîte de dialogue **Services**, assurez-vous que l'option **L'ordinateur local** est sélectionnée, puis cliquez sur **Terminer**.  
  
    4.  Cliquez sur **OK** pour fermer la boîte de dialogue **Ajouter/supprimer un composant logiciel enfichable**.  
  
3.  Double-cliquez sur **Services**, faites défiler la liste vers le bas, sélectionnez **Gestion de domaine Windows Server**, puis cliquez sur **Redémarrer le service**.  
  
## <a name="see-also"></a>Voir aussi  
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience utilisateur](Testing-the-Customer-Experience.md)