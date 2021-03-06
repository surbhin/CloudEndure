# CloudEndure

I developed this module when working at a former job when we were using CloudEndure a lot to do migrations for a customer. That was in 2016/2017. I haven't maintained this module since then, except for minor bug fixes and updates that have been asked for. The CE APIs have been continually updated and have introduced a number of breaking changes. I no longer have access to a CE account, so I don't have a way to test the changes I make either. At some point I may refactor the whole module to standardize on the latest release of CE and remove the backwards compatibility. Who knows. If you're interested in the module, or use it and want to see development/updates continue, let me know.

## Information
The cmdlets have been tested against v12 of the CloudEndure API. The "latest" version depends on your CE account. The formerly labeled v14/v15 API has now been renamed to v3 and the v12 API version has been renamed to v2, thus these cmdlets are tested against v2.

The new APIs are not backwards compatible with the old APIs, so you need to be know which versions or options of the cmdlets you should use since this module supports both v2 and v3 in the same module. It will provide an error if you try using a cmdlet against an API that is not supported for the version associated with your CE account.

## Usage

**Import the module**

    Import-Module CloudEndure

**Creates a new CloudEndure credential profile to use.**

    New-CEProfile -ProfileName "Lab" -Credential (New-Object -TypeName System.Management.Automation.PSCredential("john.smith@contoso.com", (ConvertTo-SecureString -String "My$ecureP@$$w0rd!" -AsPlainText -Force)))

**Gets a list of the existing profiles.**

    Get-CEProfile 

**Sets up a new persistent session to CloudEndure, you need to perform this before using any other cmdlets.
The passthru returns the key value the session information is stored with, it is the username/email used for the account.**

    $Email = New-CESession -ProfileName Lab -PassThru

**Alternatively, we could have specified a non-profile credential like this.**

    New-CESession -Credential (Get-Credential)

**Get license information.**

    Get-CELicense

**Gets the current blueprints.**

    Get-CEBlueprint

**Sets a new replication configuration, which will delete all existing replicated instances in the CloudEndure console.**

    New-CEReplicationConfiguration -Source 'Generic' -Target 'AWS US East (Northern Virginia)'

**Get info from multiple accounts, this assumes you have already performed a New-CESession for each of these email addresses.
The commands will get all of the blueprints in both accounts.**

    $Users = @("john.smith@contoso.com", "jane.smith@tailspintoys.com")

    foreach ($User in $Users)
	{
	    Get-CEBlueprint -Session $User
    }

**Removes all saved sessions. When you run New-CESession, whether you specify a credential or profile, there is an existing session.**

    Remove-CESession

## Revision History

### 1.2.1.0
Updated Get-CEWindowsInstaller so that the web response is returned to the pipeline.

Updated Start-CEDataReplication verbose message content.
		
Fixed Remove-CESession when a specific Session Id is provided so that the logout works.

### 1.2.0.3
Updated the Set-CEMachine cmdlet to comply with the launch time updates restriction. Converted the new version, v3, from the old names for that version, v14/v15 and translated the old version v12 to v2.

Added XSRF token support for v3 and later.

### 1.2.0.2
Fixed Remove-CEProfile.

### 1.2.0.1
Fixed bug in Invoke-CEMachineCutover adding the Ids to a collection.

### 1.2.0.0
Update the module to comply with the new version (v15) of the API. Added new cmdlets:

		Suspend-CEDataReplication
		Invoke-CELaunchTargetMachine
		Move-CEMachine
		Set-CEMachine
		Get-CEMachineBandwidth
		Set-CEMachineBandwidth

All cmdlets that are version specific will throw a runtime exception if the version the CE account is tied to is not supported by the cmdlet.

### 1.1.0.2
Removed the Get-CEAccountSummary cmdlet because it was using an undocumented API. Added the Get-CEAccountExtendedInfo cmdlet. Updated the error handling for all of the Invoke-WebRequest calls to make errors more accessible.

### 1.1.0.1
Fixed numerous bugs in the Blueprint cmdlets.

### 1.1.0.0
Updated module for 100% API coverage including all target cloud environments, AWS, Azure, GCP, and Generic. This includes several additional cmdlets:

Get-CEMachineRecoveryPoints
Get-CEAccount
Set-CEProject
New-CECloudCredential
Get-CECloudCredential
Get-CECloud
Get-CESourceCloud
Get-CEMachineReplica
Invoke-CEMachineFailover
Invoke-CEReplicaCleanup

All of the List operations now support Offset and Limit parameters. You can also specify a non-default ProjectId or CloudCredential Id for each cmdlet that supports it.

The New-CEReplicationConfiguration cmdlet allows for the input of the SubnetId by both the long name (as presented in the CE console with VPC info) and as the normal subnet id.

This is really a BETA release, many of the cmdlets were refactored or updated with added validation or capabilities, but I may have typos or copy/paste errors I didn't catch through testing. 
Please report any bugs or errors, it's just me working on it.

** NOTE : Many of the CE APIs are case sensitive, you may receive an error response if one of the JSON keys in a POST, PATCH, or DELETE body is not capitalized correctly,
		use the -Verbose option to view additional details about the request to see if this may be an issue. Send me the details if you think this is the case.

### 1.0.0.3
Fixed typo in New-CEReplicationConfiguration.

### 1.0.0.2
Updated New-CESession cmdlet to reflect changes CloudEndure made to the API. Added Get-CEAccountSummary and Get-CESourceCloud cmdlets. Changed name of Get-CECloudRegions to Get-CECloudRegion and added an Id parameter.

### 1.0.0.1
Updated the Get-CEMachine cmdlet.

### 1.0.0.0
Initial Release.
