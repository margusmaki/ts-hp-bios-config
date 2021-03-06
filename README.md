# Task Sequence HP-Bios Configuration
Script to configure HP-BIOS in SCCM-task sequence environment.
## Motivation
The script is created to ease managment of HP-BIOS configuration during SCCM-OSD.
All that script requires is a computer model folder that contains configuration .REPSET-file. 
## Getting Started
### Prerequisites
* WinPE PowerShell Support
### Installation
1. Download "HP BIOS Configuration Utility (BCU)".
2. Unpack BCU-files to Tools\ConfigurationUtility.
3. (optional) Generate HP-password using "HPQPswd.exe".
    * Move password file to Tools\BiosPassword.
4. Move your .REPSET(mandatory file type)-files to BiosFiles\Computer Model, eg. BiosFiles\EliteBook 840 G5\BiosConfig.REPSET.
    * Make sure that your regular config.REPSET does not contain EFI in name, eg. BiosConfig.REPSET.
    * If you want to configure legacy to UEFI make sure that this config file is tagged with EFI in name, eg. BiosFiles\EliteBook 840 G5\EFI_BiosConfig.REPSET.
5. Edit first line of "Set-HpBiosConfiguration.bat" with parameters you want to use.
    * Eg. "powershell.exe -WindowStyle Hidden -ExecutionPolicy ByPass -file "%~DP0Set-HpBiosConfiguration.ps1" -BiosPasswordFileName BiosPassword.bin -SetBiosPassword"
6. Copy all files to CM-share.
7. Create CM-package without program.
8. Open task sequence.
9. Just below "Online USMT" add new "Run Command Line"-step(you can add an manufacturer group if you want too).
    * Command line: cmd /C Set-HpBiosConfiguration.bat
    * Package: Package you created.
10. (optional) If you want to convert legacy to UEFI, repeat step 5 and 7 but use "Set-HpLegacyToUefi.bat" and add the run command line step to your convert to UEFI-group.

### Parameters
Parameter  | Description
------------- | -------------
ApprovedExitCodes  | Exit codes that are considered as success, see "BIOS Configuration Utility User's Guide" for more information
BiosPasswordFileName | Specify name of the BIOS-password file
SetBiosPassword | Specify this if you want to set BIOS-password on script execution
DontUseBiosPassword  | Specify this if you are not using BIOS-password
ConvertToUefi | Specify this if you are converting legacy to UEFI(seperate step), the script will look for .REPSET-file that contains EFI in name
DebugMode | Specify this if you want to run the script from Windows (it will set log path to script directory)

### Why bat-wrapper? 
Because it is the most reliable way to manage exit codes from PowerShell that never(?) fails. 
### Tests (so far)
##### Tested on 
- [x] WinPE 1803 x64
- [x] Windows 10 x64
##### Scenarios tested
- [x] -ApprovedExitCodes
- [x] -BiosPasswordFileName
- [x] -SetBiosPassword
- [x] -DebugMode
- [ ] -DontUseBiosPassword
- [ ] -ConvertToUefi

## Planned updates\features
* BIOS-update script.
* Make the script more readable.
## Disclaimer
The scripts are provided "as is" without liability or warranty of any kind and should be tested in a controlled environment.
