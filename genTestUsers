 # EDIT THE FOLLOWING FIRST BEFORE RUNNING THE SCRIPT
$Company = 'Pear Inc'
$UserCount = 100


# User Account Settings
$RandomPassword = $false
$DefaultPassword = 'P@ssw0rd'
# User name prefix
# New user object will be named TestUser1, TestUser2, ...
$TestUserPrefix = 'TestUser'

# User object properties
$GivenName = 'Test'
$Surname = 'User'
$JobTitle = @('Junior Consultant', 'Senior Consultant', 'Technical Consultant', 'Business Consultant')
$PreferredLanguage = 'en-EN'

# Name of the new organizational unit for test user object
$TestOU = 'Research'

# Target OU path where the script creates the new OU 
$TargetOU = get-addomain | select-object -expandproperty distinguishedname
$UserOUPath = 'OU=' + $TestOU + ',' + $TargetOU

write-output $UserOUPath

# Import Active Directory PowerShell Module
Import-Module -Name ActiveDirectory 

# Build OU Path$UserOUPath = ("OU={0},{1}" -f $TestOU, $TargetOU)
# Check if OU exists
$OUExists = $false
try {   
    $OUExists = [adsi]::Exists("LDAP://$UserOUPath")
}
catch { $OUExists = $true }
if (-not $OUExists) {    
    # Create new organizational unit for test users   
    New-ADOrganizationalUnit -Name $TestOU -Path $TargetOU -ProtectedFromAccidentalDeletion:$false -Confirm:$false
    New-ADOrganizationalUnit -Name "Computers" -Path $UserOUPath -ProtectedFromAccidentalDeletion:$false -Confirm:$false
}
else {
    Write-Warning ('OU {0} exists please delete the OU and user objects manually, before running this script.' -f $UserOUPath)   
    Exit
}
Write-Output ("Creating {0} user object in {1}" -f $UserCount, $UserOUPath)

# Create new user objects
1..$UserCount | ForEach-Object {   
    # Get a random number for selecting a job title   
    $random = Get-Random -Minimum 0 -Maximum (($JobTitle | Measure-Object). Count - 1)   
    # Set user password   
    if ($RandomPassword) {     
        # Create a random password      
        $UserPassword = ConvertTo-SecureString -String ( -join ((33..93) + (97..125) | Get-Random -Count 25 | ForEach-Object { [char]$_ })) -AsPlainText -Force   
    }   
    else {      
        # Use a fixed password      
        $UserPassword = ConvertTo-SecureString -String $DefaultPassword -AsPlainText -Force   
    }   
    # Create a new user object   
    # Adjust user name template and other attributes as needed   
    New-ADUser -Name ("{0}{1}" -f $TestUserPrefix, $_) -DisplayName ("{0} {1}" -f $TestUserPrefix, $_) -GivenName $GivenName -Surname ("$Surname{0}" -f $_) `
        -OtherAttributes @{title = $JobTitle[$random]; company = $Company; preferredLanguage = $PreferredLanguage } `
        -Path $UserOUPath `
        -AccountPassword $UserPassword `
        -Enabled:$True `
        -Confirm:$false
}

#Create Privileged Users

New-ADUser -Name ("David.Ang") -DisplayName ("David Ang") -GivenName "David" -Surname "Ang" `
        -OtherAttributes @{title = "IT Administrator"; company = $Company; preferredLanguage = $PreferredLanguage } `
        -Path $UserOUPath `
        -AccountPassword $UserPassword `
        -Enabled:$True `
        -Confirm:$false `
        -PasswordNeverExpires:$True 

New-ADUser -Name ("Daisy.Tan") -DisplayName ("Daisy Tan") -GivenName "Daisy" -Surname "Tan" `
        -OtherAttributes @{title = "Senior IT Administrator"; company = $Company; preferredLanguage = $PreferredLanguage } `
        -Path $UserOUPath `
        -AccountPassword $UserPassword `
        -Enabled:$True `
        -Confirm:$false `
        -PasswordNeverExpires:$True 

New-ADUser -Name ("Danny.Chan") -DisplayName ("Danny Chan") -GivenName "Danny" -Surname "Chan" `
        -OtherAttributes @{title = "DB Administrator"; company = $Company; preferredLanguage = $PreferredLanguage } `
        -Path $UserOUPath `
        -AccountPassword $UserPassword `
        -Enabled:$True `
        -Confirm:$false `
        -PasswordNeverExpires:$True 

Add-ADGroupMember -Identity "Domain Admins" -Members David.Ang, Daisy.Tan, Danny.Chan


New-ADUser -Name ("Drew.Tay") -DisplayName ("Drew Tay") -GivenName "Drew" -Surname "Tay" `
        -OtherAttributes @{title = "Systems Analyst"; company = $Company; preferredLanguage = $PreferredLanguage } `
        -Path $UserOUPath `
        -AccountPassword $UserPassword `
        -Enabled:$True `
        -Confirm:$false 

New-ADUser -Name ("Daniel.Tay") -DisplayName ("Daniel Tay") -GivenName "Daniel" -Surname "Tay" `
        -OtherAttributes @{title = "Senior Systems Analyst"; company = $Company; preferredLanguage = $PreferredLanguage } `
        -Path $UserOUPath `
        -AccountPassword $UserPassword `
        -Enabled:$True `
        -Confirm:$false 

New-ADUser -Name ("Dylan.Sim") -DisplayName ("Dylan Sim") -GivenName "Dylan" -Surname "Sim" `
        -OtherAttributes @{title = "Systems Analyst"; company = $Company; preferredLanguage = $PreferredLanguage } `
        -Path $UserOUPath `
        -AccountPassword $UserPassword `
        -Enabled:$True `
        -Confirm:$false

New-ADUser -Name ("Damien.Ong") -DisplayName ("Damien Ong") -GivenName "Damien" -Surname "Ong" `
        -OtherAttributes @{title = "Systems Analyst"; company = $Company; preferredLanguage = $PreferredLanguage } `
        -Path $UserOUPath `
        -AccountPassword $UserPassword `
        -Enabled:$True `
        -Confirm:$false 

Add-ADGroupMember -Identity "Account Operators" -Members Drew.Tay, Daniel.Tay, Dylan.Sim, Damien.Ong

New-ADComputer -Name "httpServer01" -SamAccountName "httpServer01" -Path ("OU=Computers,"+$UserOUPath)
New-ADComputer -Name "httpServer02" -SamAccountName "httpServer02" -Path ("OU=Computers,"+$UserOUPath)
New-ADComputer -Name "httpServer03" -SamAccountName "httpServer03" -Path ("OU=Computers,"+$UserOUPath)

setspn -S ("MSSQLSvc/httpServer01"+ $env:USERDNSDOMAIN) ($env:USERDNSDOMAIN + "\Damien.Ong")
setspn -S ("MSSQLSvc/httpServer02"+ $env:USERDNSDOMAIN) ($env:USERDNSDOMAIN + "\Damien.Ong")
setspn -S ("MSSQLSvc/httpServer02"+ $env:USERDNSDOMAIN) ($env:USERDNSDOMAIN + "\Dylan.Sim")
setspn -S ("MSSQLSvc/httpServer01"+ $env:USERDNSDOMAIN) ($env:USERDNSDOMAIN + "\Dylan.Sim")

Set-ADDefaultDomainPasswordPolicy -Identity $env:USERDNSDOMAIN -LockoutDuration 00:40:00 -LockoutObservationWindow 00:20:00 `
-ComplexityEnabled $False -ReversibleEncryptionEnabled $True -MaxPasswordAge 10.00:00:00 -MinPasswordLength 6
 
