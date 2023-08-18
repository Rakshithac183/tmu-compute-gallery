---
sidebar_position: 9
slug: /LabDeveloper/CreatingVMImages
---

# Creating VM Images

## Overview

The intent of this document is to know how you can use **Azure Compute Galleries** service to create VM Image Defintion and utilize it.

Follow the below steps to get started:-

## Prerequisites

1. An Azure VM which is configured as per your needs.

2. VM Image Reference details (Publisher, Offer, SKU).

## Create Azure Compute Gallery


1. Navigate to [Azure Portal](https://portal.azure.com/) and login with your work account, which CloudLabs has granted Azure access to.

![](/img/LabDeveloper/ManageImage/img01.png)

2. Once you're logged in to Azure successfully, click on **+ Create a Resource** button available on home page **OR** you can also get it from the menu which is avialable on the top left corner.

![](/img/LabDeveloper/ManageImage/img02.png)

3. Now, type **Compute gallery** in the search bar and select **Azure Compute Gallery** option from the list. 

![](/img/LabDeveloper/ManageImage/acg-search-select.jpg)

4. In the **Marketplace** page, select **Create** ***(1)*** on the **Azure Compute Gallery** service, then again select **Azure Compute Gallery** ***(2)***. 

![](/img/LabDeveloper/ManageImage/market-select-acg.jpg)


5. In the **Azure compute galleries** page, select **+ Create**, which will lead you to the resource wizard from where you can provide the details for the resource.

![](/img/LabDeveloper/ManageImage/acg-create-blade.jpg)


6. Once within the **Create Azure compute gallery** wizard, provide the values for the properties as below:

    - **Subscription** ***(1)*****:** Choose the subscription where you want your Compute Gallery to be deployed.

    - **Resource group** ***(2)*****:**  Choose the resource group **cloudlabs-mgmt**, else create one if you do not have already.

    - **Name** ***(3)*****:** Provide a name for the service.

    - **Region** ***(4)*****:** Select a region for the deployment of your Compute Gallery that is optimally situated in proximity to either yourself or your user base.

    After providing the above details, click **Review + create** ***(5)***. 
    
![](/img/LabDeveloper/ManageImage/acg-create-details-01.jpg)

7. Post the validation is passed, verify the details in the **Review + create** tab and proceed with creating the resource by clicking **Create** button.

> **Note:** It will take for around 30 seconds for Azure to create the Compute Gallery.

![](/img/LabDeveloper/ManageImage/acg-final-create.jpg)

8. Upon successful deployment, you can directly access your resource by selecting **Go to resource**.  

![](/img/LabDeveloper/ManageImage/acg-go-to-resource.jpg)


9. Following the aforementioned step will lead you to the **icg** Compute Gallery that you created.

![](/img/LabDeveloper/ManageImage/acg-homepage.jpg)


## Capturing VM and Creating Image Definition

1. Firstly, login into the VM and ensure there is no window left open inside it. Then, shut down the VM by choosing **Other[Planned]** ***(1)*** and then click **Continue** ***(2)***.

![](/img/LabDeveloper/ManageImage/shutdown-config-vm.jpg)


2. Now go back to **Azure portal** and check the status of the VM, you might need to wait for couple of minutes until the VM **Status** transitions to **Stopped (deallocated state)**.

![](/img/LabDeveloper/ManageImage/vm-status.jpg)

3. Once the VM is stopped, you can capture the new image using the **Capture** button available on the VM overview page.

![](/img/LabDeveloper/ManageImage/vm-capture.jpg)

> You already have the **Compute Gallery** and now you are just creating a new Image Definition and Image Version which will be stored in the Compute Gallery.

4. Next, in the **Create an image** page, you need to associate it with the Subscription and Resource Group where the Compute Gallery resides, which is **cloudlabs-mgmt** ***(1)*** resource group and also set the **Share image to Azure compute gallery** property to **Yes, share it to a gallery as a VM image version** ***(2)*** if it is not already.

![](/img/LabDeveloper/ManageImage/createimg-subsrg-select.jpg)


5. In the Gallery details section, select the **Target Azure compute gallery** ***(1)*** you created previously from the dropdown, and set the **Operating system state** property to **Specialized: VMs created from this image are completely configured and do not require parameters such as hostname and admin user/password** ***(2)***. Then, click **Create new** ***(3)*** to create a new image definition. 

![](/img/LabDeveloper/ManageImage/createimg-gallery-details.jpg)

6. In the  the **Create a VM image definition** wizard, provide the values for the properties as below:

    - **VM image definition** ***(1)*****:** Provide a name for your VM image. Ideally it should relate to lab name.

    - **OS type** ***(2)*****:**  Choose OS type Windows or Linux, if not auto populated.

    - **Publisher** ***(3)*****:** Provide the image Publisher name.
    > **Note:** If you are capturing the image of a VM created from Azure market image, then you can find the image **Publisher, Offer and SKU** details by going to the respective VM page, selecting **Export template** in the VM blade and searching for **imageReference** under which you can find the details image details. For non-Azure market image you can provide custom details.
 
    - **Offer** ***(4)*****:**  Input the image Offer name.

    - **SKU** ***(5)*****:** Enter the image SKU.

After entering the above details, click on **OK** ***(6)*** to save the details. 

![](/img/LabDeveloper/ManageImage/createimg-definition.jpg)

7. Scroll down, and in the **Version details** section you need to provide version and image replication details as below:

    - **Version number** ***(1)*****:** Provide the version number. The VM image version number should follow Major(int).Minor(int).Patch(int) format. For example: 1.0.0, 1.2.2

    - **End of life date** ***(2)*****:** Here you need to put an End of life date for image as well, say we can put after 2 years.

    - **Default storage sku** ***(3)*****:** Choose **Standard HDD LRS** as it will optimize the storage cost.

    - **Default replica count** ***(4)*****:** Keep it as **1**, as one copy is sufficient.

    - **Storage account type** ***(5)*****:** Set it to **Standard HDD LRS** for optimized storage cost. 

    - **Target regions** ***(6)*****:** Select a region from the dropdown if you want to replicate the image to an additional region.

Select **Review + create** ***(7)*** to proceed further.

![](/img/LabDeveloper/ManageImage/createimg-version-details.jpg)

8. Lastly, in the **Review + create** tab, verify te details and click **Create** to create the image definition and version.

![](/img/LabDeveloper/ManageImage/createimg-verify-img-details.jpg)


> **Note:** Creating image definition and version could take 15 minutes or more.

## Utilizing the image

1. Post the deployment succeeds, click on **Go to resource** which will direct you to the image version you created.

![](/img/LabDeveloper/ManageImage/createimg-go-to-resource.jpg)

2. In the **VM image version** page, select **Properties** in the left blade and copy the **Resoruce ID** property so that we can use it in the ARM template.

![](/img/LabDeveloper/ManageImage/createimg-copy-resource-id.jpg)

3. In the ARM template, under  **imageReference**, provide the image **Resoruce ID** value for **id**.

![](/img/LabDeveloper/ManageImage/createimg-vm-ref-arm.jpg)

4. Add the updated ARM template in the CloudLabs template and provide the same region(s) where the image is replicated in both CloudLabs template and ODL.

![](/img/LabDeveloper/ManageImage/createimg-odl-regions.jpg)

5. Finally, verify if the configurations are made correctly by adding a new user deployment from any existing/new **On Demand Lab**.


You can connect to the VM and verify the changes once the deployment is completed.

Please delete the Resource Group that was created to Capture a VM image after the changes have been confirmed (manually on Azure portal).

If you require any support, please contact the CloudLabs team. 

============================================
