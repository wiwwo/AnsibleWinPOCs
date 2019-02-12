https://medium.com/the-sysadmin/managing-windows-machines-with-ansible-60395445069f

PRE:
```
pip install pywinrm requests-ntlm requests-kerberos requests-credssp
```

On windows CMD (Admin):
```
powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((new-object net.webclient).DownloadString('https://github.com/ansible/ansible/raw/devel/examples/scripts/ConfigureRemotingForAnsible.ps1'))"

powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((new-object net.webclient).DownloadString('file:///Cosimo/myConfigureRemotingForAnsible.ps1'))"

powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((new-object net.webclient).DownloadString('file:///Cosimo/myStopWinRM.ps1'))"
powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((new-object net.webclient).DownloadString('file:///Cosimo/myStartWinRM.ps1'))"

```

Play and plug!

```
ansible windows -i hostsFile -m win_ping -vvv
ansible windows -i hostsFile -m setup

ansible windows -i hostsFile -m  win_command -a "cmd.exe /c dir c:\\"

ansible windows -i hostsFile -m win_say -a "msg='Hi! This is a demo' start_sound_path='C:\\windows\\media\\ding.wav' speech_speed=2"

ansible-playbook  -i hostsFile playbooks/playbook-install-7zip.yml

ansible-playbook  -i hostsFile  playbooks/run_hello.ps1.yml

ansible-playbook  -i hostsFile  playbooks/run_getInstalled.ps1.yml

ansible-playbook -i hostsFile playbooks/upgradeWin.yml

# ansible windows -i hostsFile -m win_reboot
```

----------------------------
Firewall Using PowerShell
PowerShell:
```
  Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False
  Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled True
```

Command Prompt
```
  netsh advfirewall set allprofiles state off
  netsh advfirewall set allprofiles state on
```

Funny firewall error might be fixed by:
```
PS C:\Windows\system32>
$nlm = [Activator]::CreateInstance([Type]::GetTypeFromCLSID([Guid]"{DCB00C01-570F-4A9B-8D69-199FDBA5723B}"))
$connections = $nlm.getnetworkconnections()
$connections |foreach {
 if ($_.getnetwork().getcategory() -eq 0)
 {
    $_.getnetwork().setcategory(1)
 }
}

PS C:\Windows\system32> winRM quickconfig
```

Various:
```
winrm get winrm/config/service

winrm set winrm/config/Service/auth @{Basic="true"}

```

Study those:
```
PS C:\Windows\system32> Enable-PSRemoting


PS C:\Windows\system32> Disable-PSRemoting
```
----------------------------------
TO-READ:

https://fabianlee.org/2017/06/05/ansible-managing-a-windows-host-using-ansible/

https://argonsys.com/microsoft-cloud/articles/configuring-ansible-manage-windows-servers-step-step/
