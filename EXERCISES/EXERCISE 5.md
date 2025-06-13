# Deploying to Azure App Service using GitHub Actions

This guide will help you deploy a web application (Node.js, Python, .NET, etc.) to **Azure App Service** using **GitHub Actions**.

---

## Prerequisites

1. An Azure account: [Sign up](https://azure.com)
2. Azure App Service created (Linux or Windows).
3. Your app source code in a GitHub repository.
4. GitHub repository linked to your Azure App.

---

## Step 1: Create Azure App Service and Get Publish Profile

1. Go to the **Azure Portal** → your **App Service**.
2. In the left menu, select **Deployment Center**.
3. Under **Settings > GitHub**, set up the source if needed.
4. Click on **"Get publish profile"** under **Overview** → save the downloaded `.PublishSettings` file securely.

---

## Step 2: Add Publish Profile to GitHub Secrets

1. Go to your GitHub repo.
2. Click on **Settings > Secrets and variables > Actions > New repository secret**.
3. Name it: `AZURE_WEBAPP_PUBLISH_PROFILE`
4. Paste the content of the downloaded publish profile.

---

## Step 3: Add Your Application Code

# Azure .NET Minimal API App

## Program.cs

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;

var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapGet("/", () => "Hello from Azure!");

app.Run();
````

## AzureDotNetApp.csproj

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>net8.0</TargetFramework>
    <Nullable>enable</Nullable>
    <ImplicitUsings>enable</ImplicitUsings>
  </PropertyGroup>

</Project>
```

---

## Step 4: Add GitHub Actions Workflow File

Create this file in your repo:

```yaml
name: Deploy .NET app to Azure Web App

on:
  push:
    branches:
      - main  # o tu rama principal

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
    - name: 'Checkout code'
      uses: actions/checkout@v3

    - name: 'Set up .NET'
      uses: actions/setup-dotnet@v4
      with:
        dotnet-version: '8.0.x'  # O la versión que estés usando

    - name: 'Restore dependencies'
      run: dotnet restore

    - name: 'Build app'
      run: dotnet build --configuration Release

    - name: 'Publish app'
      run: dotnet publish --configuration Release --output ./publish

    - name: 'Deploy to Azure Web App'
      uses: azure/webapps-deploy@v2
      with:
        app-name: <YOUR_A_

```

Replace `<YOUR_APP_NAME>` with the **name of your Azure Web App**.

---

## Step 5: Commit and Push

```bash
git add .
git commit -m "Add Azure deployment workflow"
git push origin main
```

This will trigger the GitHub Action to build and deploy your app.

---

## Step 6: Verify the Deployment

1. Go to your Azure App Service URL (e.g., `https://<YOUR_APP_NAME>.azurewebsites.net`)
2. You should see your app running!

---

## 🧼 Cleanup

To remove GitHub Actions deployments:

* Delete the workflow file.
* Remove the `AZURE_WEBAPP_PUBLISH_PROFILE` secret from GitHub.


