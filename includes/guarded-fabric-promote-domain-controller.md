1.  Exécutez [Install-HgsServer](https://technet.microsoft.com/itpro/powershell/windows/hgsserver/install-hgsserver) pour joindre le domaine et promouvoir le nœud à un contrôleur de domaine.

    ```powershell
    $adSafeModePassword = ConvertTo-SecureString -AsPlainText '<password>' -Force

    $cred = Get-Credential 'relecloud\Administrator'

    Install-HgsServer -HgsDomainName 'bastion.local' -HgsDomainCredential $cred -SafeModeAdministratorPassword $adSafeModePassword -Restart
    ```

2.  Lorsque le serveur redémarre, connectez-vous avec un compte d’administrateur de domaine.

<!-- Appears twice in guarded-fabric-configure-additional-hgs-nodes.md 
-->