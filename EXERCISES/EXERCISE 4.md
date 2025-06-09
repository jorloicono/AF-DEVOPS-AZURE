This lab guides you through creating a serverless solution in Azure using **Azure Functions** triggered by blob uploads to **Azure Blob Storage**. The function will automatically copy the uploaded blob from one container to another.

---

## Objectives

- Create two Azure Blob Storage containers (source and destination).
- Create an Azure Function triggered by blob upload events.
- Automatically copy the blob from the source container to the destination container.
- Verify the copied blob.

---

## Step 1: Create Two Azure Blob Storage Containers

1. In the Azure Portal, create a **Storage Account**:
   - Go to **Storage Accounts** > **+ Create**.
   - Fill in the details (unique name, region, performance, redundancy, etc.).
   - Create the storage account.

2. Inside the storage account, create two containers:
   - `source-container`
   - `destination-container`
   - Both with private access (default).

---

## Step 2: Create an Azure Function App

1. In the Azure Portal, search for **Function App** and create a new one.
2. Fill in:
   - Function App name (e.g., `myfunctionapp`).
   - Runtime stack: **Node.js** (recommended LTS version).
   - Region: same as your storage account.
   - Configure storage and networking as needed.
3. Create the Function App.

---

## Step 3: Create an Event Grid Triggered Function

1. Inside your Function App, add a new function using the **Event Grid trigger** template.
2. Name the function, e.g., `CopyBlobOnUpload`.

---

## Step 4: Add the Function Code

Replace the default code with the following Node.js code:

```javascript
const { BlobServiceClient } = require('@azure/storage-blob');

const sourceContainer = "source-container";
const destinationContainer = "destination-container";
const connectionString = process.env.AzureWebJobsStorage;

module.exports = async function (context, eventGridEvent) {
    const blobUrl = eventGridEvent.data.url;
    const blobName = blobUrl.split("/").pop();

    const blobServiceClient = BlobServiceClient.fromConnectionString(connectionString);
    const sourceClient = blobServiceClient.getContainerClient(sourceContainer);
    const destinationClient = blobServiceClient.getContainerClient(destinationContainer);

    const sourceBlobClient = sourceClient.getBlobClient(blobName);
    const destinationBlobClient = destinationClient.getBlockBlobClient(blobName);

    const downloadBlockBlobResponse = await sourceBlobClient.download();
    const downloadedContent = await streamToBuffer(downloadBlockBlobResponse.readableStreamBody);

    await destinationBlobClient.upload(downloadedContent, downloadedContent.length);

    context.log(`Copied blob ${blobName} to ${destinationContainer}`);
};

async function streamToBuffer(readableStream) {
    return new Promise((resolve, reject) => {
        const chunks = [];
        readableStream.on("data", (data) => chunks.push(data));
        readableStream.on("end", () => resolve(Buffer.concat(chunks)));
        readableStream.on("error", reject);
    });
}
````

---

## Step 5: Configure the Event Grid Trigger

1. Go to the `source-container` in your storage account.
2. Select **Events** > **+ Event Subscription**.
3. Fill out:

   * Name: `copy-on-upload`
   * Event Types: **Blob Created**
   * Endpoint Type: **Azure Function**
   * Select your Function App and the `CopyBlobOnUpload` function.
4. Create the subscription.

---

## Step 6: Test the Setup

1. Upload a file (image, document, etc.) to the `source-container`.
2. The Azure Function will trigger and copy the file automatically to the `destination-container`.
3. Verify the file is present in the destination container.

