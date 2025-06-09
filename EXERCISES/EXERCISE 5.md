## Create a Storage Account

In this task, you will create a storage account.

1. Sign in to the Azure Portal at https://portal.azure.com.

2. Select **Create a resource**.

3. Under **Categories**, select **Storage**.

4. Select **Storage account**, then select **Create**.

5. On the **Basics** tab of the **Create storage account** panel, fill in the following information. Leave all other values as default.

| Setting         | Value                                |
|-----------------|------------------------------------|
| Subscription    | Concierge Subscription              |
| Resource group  | Select the resource group starting with "learn" |
| Storage account name | Create a unique storage account name |
| Region          | Leave the default value             |
| Performance     | Standard                           |
| Redundancy      | Locally-redundant storage (LRS)    |

6. On the **Advanced** tab of the **Create storage account** panel, fill in the following. Leave all other values as default.

| Setting                                                        | Value     |
|----------------------------------------------------------------|-----------|
| Allow blob public access to containers                          | Enabled   |

7. Select **Review + create** to review your storage account configuration and allow Azure to validate the settings.

8. Once validated, select **Create**. Wait for the notification that the account was created successfully.

9. Click **Go to resource**.

---

## Use with Blob Storage

In this section, you will create a blob container and upload an image.

1. Under **Data storage**, select **Containers**.

2. Select **+ Container** and complete the information.

| Setting           | Value                          |
|-------------------|--------------------------------|
| Name              | Enter a name for the container |
| Public access level | Private (no anonymous access)  |

3. Select **Create**.

> **Note:**  
> Step 4 requires an image. If you want to upload an image from your computer, proceed to Step 4. Otherwise, open a new browser window and search for a flower image on Bing. Save the image to your computer.

4. Back in the Azure Portal, select the container you created, then select **Upload**.

5. Browse for the image file you want to upload. Select it, then select **Upload**.

> **Note:**  
> You can upload as many blobs as you want this way. The new blobs will be listed inside the container.

6. Select the blob (file) you just uploaded. It should open in the **Properties** tab.

7. Copy the URL from the **URL** field and paste it into a new browser tab. You should receive an error message similar to the following:

```xml
<Error>
  <Code>ResourceNotFound</Code>
  <Message>The specified resource does not exist. RequestId:4a4bd3d9-101e-005a-1a3e-84bd42000000</Message>
</Error>
````

8. Change the blob access level.

9. Go back to the Azure Portal.

10. Select **Change access level**.

11. Set the **Public access level** to **Blob (anonymous read access for blobs only)**.

12. Select **OK**.

13. Refresh the tab where you previously tried to access the file.


