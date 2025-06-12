# Deploying to Azure App Service using Azure Pipelines (with Azure Repos)

This guide walks you through deploying an application to **Azure App Service** using **Azure Pipelines** with code stored in **Azure Repos**.


## Project Structure Example (Node.js)

```plaintext
my-app/
├── index.js
├── package.json
├── .azure-pipelines/
│   └── azure-pipelines.yml
````

---

## Step 1: Sample App Code (Node.js)

`index.js`:

```js
const express = require('express');
const app = express();
const port = process.env.PORT || 3000;
app.get('/', (req, res) => res.send('Hello from Azure Pipelines!'));
app.listen(port, () => console.log(`App listening on port ${port}`));
```

`package.json`:

```json
{
  "name": "azure-node-app",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "start": "node index.js"
  },
  "dependencies": {
    "express": "^4.18.2"
  }
}
```

---

## Step 2: Create Azure Resource Manager (ARM) Service Connection

1. Go to **Azure DevOps Portal** → your project.
2. Navigate to **Project Settings > Service connections**
3. Click **New service connection** → choose **Azure Resource Manager**
4. Choose:

   * **Scope level:** Subscription
   * **Authentication method:** Service principal (automatic)
5. Select your **Azure subscription** and App Service resource group.
6. Give it a name (e.g., `AzureConnection`) and save.

---

## Step 3: Create Azure Pipeline

You can create a file `.azure-pipelines/azure-pipelines.yml` (or define it in root if preferred):

```yaml
# azure-pipelines.yml
trigger:
  branches:
    include:
      - main  # or your chosen branch

pool:
  vmImage: 'ubuntu-latest'

variables:
  azureSubscription: 'AzureConnection'  # Name of your service connection
  appName: '<YOUR_APP_NAME>'            # e.g. my-node-webapp
  packageDir: '.'                       # Root folder or 'dist' if you have a build step

steps:

- task: NodeTool@0
  inputs:
    versionSpec: '18.x'
  displayName: 'Install Node.js'

- script: |
    npm install
  displayName: 'Install Dependencies'

- script: |
    echo "No build step defined"  # Change if using React/Angular/etc.
  displayName: 'Build App'

- task: ArchiveFiles@2
  inputs:
    rootFolderOrFile: '$(packageDir)'
    includeRootFolder: false
    archiveType: 'zip'
    archiveFile: '$(Build.ArtifactStagingDirectory)/app.zip'
    replaceExistingArchive: true
  displayName: 'Archive App'

- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(Build.ArtifactStagingDirectory)'
    ArtifactName: 'drop'
    publishLocation: 'Container'
  displayName: 'Publish Artifact'

- task: AzureWebApp@1
  inputs:
    azureSubscription: '$(azureSubscription)'
    appName: '$(appName)'
    package: '$(Build.ArtifactStagingDirectory)/app.zip'
  displayName: 'Deploy to Azure Web App'
```

Replace `<YOUR_APP_NAME>` with your actual App Service name.

---

## Step 4: Create Pipeline in Azure DevOps

1. Go to **Pipelines > Create Pipeline**
2. Select **Azure Repos Git**
3. Choose your repo
4. Select **YAML** and point to your `azure-pipelines.yml` file
5. Run the pipeline

---

## Step 5: Deploy and Verify

Once the pipeline runs successfully:

1. Open `https://<YOUR_APP_NAME>.azurewebsites.net` in a browser
2. You should see: `Hello from Azure Pipelines!`

---

## Add a Build Step (e.g., for React, Angular)

```yaml
- script: |
    npm run build
  displayName: 'Build frontend'

- task: CopyFiles@2
  inputs:
    SourceFolder: 'build'
    Contents: '**'
    TargetFolder: '$(Build.ArtifactStagingDirectory)'
  displayName: 'Copy build output'
```

---

## Cleanup

To remove the deployment integration:

* Delete the pipeline from Azure DevOps
* Remove the Azure Resource Manager service connection

```
