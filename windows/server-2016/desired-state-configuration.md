<!-- TITLE: Desired State Configuration -->
<!-- SUBTITLE: A quick summary of Desired State Configuration -->

# DSC -- Desired State Configuration

Idempotent -- Will only change what isn't in the desired state.

## Config

```
# Configuration
Configuration <config_name> {
	
	# Resources
	WindowsFeature <name> {
			
			Ensure = "Present"
			Name = "web-server"
		
	}

}

# Create the configuration (.mof)
<config_name> -ComputerName <server_name> -OutputPath <C:\my\path>

# Push Config
Start-DscConfiguration -Path <C:\my\path> -Wait -Verbose

```

## Terms

### LCM -- Local Configuration Manager

#### Configuration Mode

ApplyOnly: 

	Only Applies the configuration once.

ApplyAndMonitor:

	Apply the configuration and logs if it stays from what is configured.

ApplyAndAutocorrect:

	Apply the configuration and auto fix that are out of state.
	
	
	
#### Verify Configuration State


```
Get-DscConfigurationStatus
```

### .Mof -- Managed Object Framework