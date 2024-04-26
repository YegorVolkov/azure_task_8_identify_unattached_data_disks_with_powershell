# Indentify Unattached Data Disks With Powershell

While you are waiting for feedback on the new cost calculations for your infra, you continue looking for other cost optimization techniques. Once you heard from your colleague to proactively check for unattached data disks: 

- When you have a lot of VMs and more than one engineer working with the infrastructure, you can easily end up in a situation where someone deleted the VM but forgot to delete the data disk. 

- This data disk would still be included in your subscription, and you will need to pay for it even if you are not using it. For example, 1TB Premium with ZRS replication costs around $200/month (the exact price depends on the region). 

- So you would want to periodically check if there are any unattached disks in your subscription. 

In this task, you will implement a Powershell script that allows you to easily identify unattached data disks, review and delete them, and keep your cloud costs optimized. 

## Prerequisites

Before completing any task in the module, make sure that you followed all the steps described in the **Environment Setup** topic, in particular: 

1. Ensure you have an [Azure](https://azure.microsoft.com/en-us/free/) account and subscription.

2. Create a resource group called *"mate-resources"* in the Azure subscription.

3. In the *"mate-resources"* resource group, create a storage account (any name) and a *"task-artifacts"* container.

4. Install [PowerShell 7](https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell?view=powershell-7.4) on your computer. All tasks in this module use Powershell 7. To run it in the terminal, execute the following command: 
    ```
    pwsh
    ```

5. Install [Azure module for PowerShell 7](https://learn.microsoft.com/en-us/powershell/azure/install-azure-powershell?view=azps-11.3.0): 
    ```
    Install-Module -Name Az -Repository PSGallery -Force
    ```
If you are a Windows user, before running this command, please also run the following: 
    ```
    Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
    ```

6. Log in to your Azure account using PowerShell:
    ```
    Connect-AzAccount -TenantId <your Microsoft Entra ID tenant id>
    ```

## Requirements

In this task, you will need to work with the infrastructure from the previous task [previous task](https://github.com/mate-academy/azure_task_5_move_vm_to_new_region). In order to complete the task, you need to perform the following steps: 

1. Deatach the data disk from the VM, you worked with in the previous task. In order to complete this step, you need to [unmount the disk](https://learn.microsoft.com/en-us/azure/virtual-machines/linux/detach-disk#connect-to-the-vm-to-unmount-the-disk), and then detach it using the [Powershell](https://learn.microsoft.com/en-us/azure/virtual-machines/windows/detach-disk#detach-a-data-disk-using-powershell)

2. Write a Powershell script, which implements the task requirements below in the file `task.ps1` in this repo: 
    
    = The script should find all unattached disks in the resource group where the VM is deployed (`mate-azure-task-5`) and save information about them in JSON format to the file `result.json` in this repo.
    
    - to find disks resources, the script should use comandlet [Get-AzDisk](https://learn.microsoft.com/en-us/powershell/module/az.compute/get-azdisk?view=azps-11.5.0) from Az module. 

    - The script should filter disk objects to get only unattached ones. To check if a disk is attached, you can rely on either the 'DiskState' or 'ManagedBy' properties of the disk object, returned by the Get-AzDisk comandlet. When writing the script, explore the values of those properties to understand which value an unattached disk should have, and filter disk objects by that value in the script.  

3. Run the script to save the information about unattached data disks to the `result.json`. 

4. Run artifacts generation script `scripts/generate-artifacts.ps1`

## How to complete tasks in this module 

Tasks in this module are relying on 2 PowerShell scripts: 

- `scripts/generate-artifacts.ps1` generates the task "artifacts" and uploads them to cloud storage. An "artifact" is evidence of a task completed by you. Each task will have its own script, which will gather the required artifacts. The script also adds a link to the generated artifact in the `artifacts.json` file in this repository — make sure to commit changes to this file after you run the script. 
- `scripts/validate-artifacts.ps1` validates the artifacts generated by the first script. It loads information about the task artifacts from the `artifacts.json` file.

Here is how to complete tasks in this module:

1. Clone task repository

2. Make sure you completed the steps described in the Prerequisites section

3. Complete the task described in the Requirements section 

4. Run `scripts/generate-artifacts.ps1` to generate task artifacts. The script will update the file `artifacts.json` in this repo. 

5. Run `scripts/validate-artifacts.ps1` to test yourself. If tests are failing — follow the recommendation from the test script error message to fix or re-deploy your infrastructure. When you are ready to test yourself again — **re-generate the artifacts** (step 4) and re-run tests again. 

6. When all tests will pass - commit your changes and submit the solution for review. 

Pro tip: If you are stuck with any of the implementation steps, run `scripts/generate-artifacts.ps1` and `scripts/validate-artifacts.ps1`. The validation script might give you a hint on what to do.  



5. Test yourself using the script `scripts/validate-artifacts.ps1`

6. Make sure that changes to both `task.ps1` and `result.json` are committed to the repo, and submit the solution for review.

7. When the solution is validated, delete the resource group `mate-azure-task-5`— you won't need it anymore for the next tasks. 
