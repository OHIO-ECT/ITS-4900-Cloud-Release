
# Deliverable 1

# Task 1 - Gather your tools

Students should document the work done in the Introduction repo and document the tools selected to start this class. Include an explanation of each selection. Provide an additional justification and statement of confidence for not selecting the same toolkit as Professor Saunders.

## Large Language Model (LLM)

1. If you do not already have a preferred LLM, consider using Claude. Any of the free levels will work for this class, though students who aggressively use the selected LLM may find themselves running out of free AI resources. Justify your selection. The student may optionally use the LLM to set any of the following directions. Just know the what and why for anything that comes from an LLM.

## Programming language

2. Choose a programming language. It is generally true that most programs can be written in any language; however, some combinations work better than others. For example, it often makes more sense to use PHP and JavaScript for web applications than C++. Brandon is going to program and can provide support in Python this semester.

## Software Development Environment

3. Read Units 1 and 2 in [Prepare your development environment for Azure development](https://learn.microsoft.com/en-us/training/modules/prepare-your-dev-environment-for-azure-development/). There are many more software development environments. Visual Studio Code (VSCode) is very helpful for developing systems and integrating components, and will be what Professor Saunders develops in this semester.

4. Select a development venue. VSCode is available on the gHost, but it is possible to have your development environment on your personal computer as well. Professor Saunders is going to use VSCode on the gHost.

## Free Azure student account and web interface

The following procedure will set up a free student account at Azure. These free accounts are available and include $100. Students should be very cautious about the systems deployed in Azure that will consume this credit. The $100 budget may be used in later parts.

5. Browse to [Azure Student Infrastructure](https://azure.microsoft.com/en-us/free/students?icid=portal).

6. Scroll down and select the "Sign Up" button under "Azure for Students" with a $100 credit, and login with the student's OHIO id.

7. There will be an in-class demonstration of the web interface. This class will occasionally leverage the web interface, but the bulk of the work will be done from a command line and via software tools.

# Task 2 - Setup a VSCode development environment

8. Remote Desktop to the gHost and run VSCode from the start menu which can be accessed with the circle icon in the lower left corner of the gHost's desktop.

9. If prompted to install 'Container Tools', follow the defaults in the process.

10. Advance to Unit 3 of [Prepare your development environment for Azure development](https://learn.microsoft.com/en-us/training/modules/prepare-your-dev-environment-for-azure-development/) and scroll down to "Install Azure App Service extension".

11. VSCode is not direct about the next steps, but the user should see a new Azure A icon on the activity bar which is by default on the left side of VSCode.

12. This brings up a new block in the primary side bar. Select "Sign in to Azure", and follow the process.

# Task 3 - Setting up PowerShell

Students will need a very high level of fluency with Shell interfaces for this class. Use Microsoft's training guide [Automate Azure tasks with Azure PowerShell](https://learn.microsoft.com/en-us/training/modules/automate-azure-tasks-with-powershell/) to set up PowerShell on the gHost and/or student's computer.

13. Unit 2 - Do not run the example PowerShell commands.

14. Unit 3 - Is for informational purposes only, the real work is in the next unit.

15. Unit 4 - Make sure to select the intended platform.

16. Unit 5 - In the "Connect to Azure" step, use one of the following commands which will allow for a remote web-based login.

Login via a web browser instance that is on the local machine with PowerShell:

`Connect-AzAccount`

Login via code to an arbitrary (likely more trusted) web browser:

`Connect-AzAccount -UseDeviceAuthentication`

17. Later in Unit 5, list the Resource Groups before creating any. Set up a resource group for this class.

`New-AzResourceGroup -Name ITS-Cloud-Systems -Location eastus`

18. The remainder of Unit 5 is informational about the options of setting up a virtual machine. Do not run any of the remaining commands.

19. The following sequence will reload PowerShell and log into Azure in the future.

```
pwsh
Connect-AzAccount
```

20. Explore PowerShell on your own. Its verb-noun command naming and its multi-value data structures within piping make it conducive to managing complex environments like Azure. Here are some reasonable resources:

[Microsoft - PowerShell Documentation](https://learn.microsoft.com/en-us/powershell/)
[O'Reilly - Windows PowerShell Cookbook: The Complete Guide to Scripting Microsoft's Command Shell](https://ohiolink-ou.primo.exlibrisgroup.com/permalink/01OHIOLINK_OU/16n51lb/alma9926170658208506)
[The Lonely Administrator - Essential PowerShell Learning Resources](https://whatpixel.com/10-best-powershell-books/)

# Task 4 - Deploy a virtual server

The [Automate Azure tasks with Azure PowerShell](https://learn.microsoft.com/en-us/training/modules/automate-azure-tasks-with-powershell/) guide steps the reader through the configuration of a virtual machine in Unit 6.

21. The compute instance for this virtual machine is configured with the following hashtable. The following steps will break down the elements of these parameters and adjust them to suit this deliverable's objectives.

```
#DO NOT RUN THIS YET!!!
$azVmParams = @{
    ResourceGroupName   = 'myResourceGroupName'
    Name                = 'testvm-eus-01'
    Credential          = (Get-Credential)
    Location            = 'eastus'
    Image               = 'Canonical:0001-com-ubuntu-server-jammy:22_04-lts:latest'
    OpenPorts           = 22
    PublicIpAddressName = 'testvm-eus-01'
}
```
22. The suggested PowerShell command for creating a VM makes certain assumptions that need to be considered before blindly running the command. In particular, the Resource group name must equal `ITS-Cloud-Systems` to match the previous configurations. Students will need to be aware of this from the beginning.

23. This class will also extend the VM name and other resource names to make sure they are identified to a lab.

24. The Credential will be the username and password impressed into the system for initial access. The `(Get-Credential)` runs the PS command `Get-Credential` which prompts the user for input and then places the responses into the variable to be used later in the process.

25. This class will always use the `eastus` location; other locations would be relevant for use cases not in the eastern part of the United States.

```
#DO NOT RUN THIS YET!!!
$azVmParams = @{
    ResourceGroupName   = 'ITS-Cloud-Systems'
    Name                = 'del01-testvm-eus-01'
    Credential          = (Get-Credential)
    Location            = 'eastus'
    Image               = 'Canonical:0001-com-ubuntu-server-jammy:22_04-lts:latest'
    OpenPorts           = 22
    PublicIpAddressName = 'del01-testvm-eus-0101'
}
```

26. The machine image is a much more complex concept. These images have a uniform resource name (URN) with the style of `Publisher:Offer:SKU:Version`.

27. This list of publishers is quite extensive so it is generally necessary to have the beginning of a target. The following command pipe will show the names likely related to the maintainer of the Ubuntu Linux distribution, Canonical.

`Get-AzVMImagePublisher -Location 'eastus' | Where-Object {$_.PublisherName -like '*Canonical*'}`

28. The following command will list the disk image offerings by Canonical.

`Get-AzVMImageOffer -Location 'eastus' -PublisherName 'Canonical'`

29. Readers should notice that there are several variations in the type of image. Failure to understand a vendor's release and maintenance cycles for various offerings can open a system up to significant security and operational hazards. [Ubuntu Release Cycle and support](https://ubuntu.com/about/release-cycle). The automation principles to be studied in this class can mitigate many of those risks, IF DONE PROPERLY AND IN COLLABORATION WITH THE DEVELOPERS OF THE APPLICATION. In particular, "server" is a typical image, but there are other options for "minimized" environments or for subscription support services "pro", "daily" releases with the latest updates preinstalled, or "aks" images that are optimized for Kubernetes. Unless instructed otherwise, use a typical server version. While different vendors may choose other naming schemas, operational constructs like these are common across other vendors.

30. Canonical also maintains multiple releases of the images based on their code names. As a default, Long Term Support (LTS) releases have the most stability and the longest software support. Non-LTS releases might be necessary for bleeding edge services. Canonical releases a new LTS in April (.04) every 2 years, and supports the releases for 5 years, with extended support available.

| Version | Code Name |
|---------|-----------|
| 18.04 LTS | Bionic Beaver |
| 20.04 LTS | Focal Fossa |
| 22.04 LTS | Jammy Jellyfish |
| 24.04 LTS | Noble Numbat |

31. The flexibility and hierarchy in the image names allow for flexibility between vendors and an ability to dictate specific releases to suit the demands of the application. For this class, virtual machines will generally use the latest Ubuntu server image which would be denoted with the string `Canonical:ubuntu-24_04-lts:server:latest`.


```
#DO NOT RUN THIS YET!!!
$azVmParams = @{
    ResourceGroupName   = 'ITS-Cloud-Systems'
    Name                = 'del01-testvm-eus-01'
    Credential          = (Get-Credential)
    Location            = 'eastus'
    Image               = 'Canonical:ubuntu-24_04-lts:server:latest'
    OpenPorts           = 22
    PublicIpAddressName = 'del01-testvm-eus-0101'
}
```

32. These are not the only parameters that are available for the deployment of a new VM. There are also different sizes of machines that are available. There is a whole marketplace for different CPU, memory, and storage capacities that are available, but only a few of them are available for free use by the student. In particular, this course will generally use the `Standard_B1s` server size.

```
$azVmParams = @{
    ResourceGroupName   = 'ITS-Cloud-Systems'
    Name                = 'del01-testvm-eus-01'
    Credential          = (Get-Credential)
    Location            = 'eastus'
    Image               = 'Canonical:ubuntu-24_04-lts:server:latest'
    Size                = 'Standard_B1s'
    OpenPorts           = 22
    PublicIpAddressName = 'del01-testvm-eus-0101'
}
```

33. This configuration is executed with the following command:

```New-AzVm @azVmParams```

34. Yep, it breaks for Brandon at this point. It might be a licensing issue that unique to his user.  Please report to him via Teams if you are able to build a VM..


# A list of failed links and leads

[Deploy a website to Azure with Azure App Service](https://learn.microsoft.com/en-us/training/modules/stage-deploy-app-service-deployment-slots/) Assumes that the user has an account that has slots enabled.


