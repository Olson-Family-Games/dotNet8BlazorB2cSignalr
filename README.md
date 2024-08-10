# .NET 8 Blazor Sample App with AAD B2C & SignalR
Foundational .NET 8 sample app that was able to get Blazor working with SignalR and AAD B2C

Pulled together many publicly available/posted information to get features working for me to build a card game for my family (located around the country) to play. The working version of my site is located at https://olsongames.azurewebsites.net

Key features that work:
 - Blazor InteractiveServer rendermode
 - Blazor InteractiveWebAssembly rendermode
 - Azure AD (AAD) B2C login (as proven via AuthorizeView menu features)
 - SignalR functionality (as proven via a basic Chat feature) 

There are multiple areas of the code that could/should be done better, in particular:
 - The Weather page double renders
 - The NavMenu collapsing submenus
 - The UI is not pretty

Also, I haven't actually connected it to a SignalR Service yet; at this point, I'm not sure I want or need to.

Azure Resources that are needed for public deployment:
 - Web App App Service (Works on Linux or Windows)
 - Azure AD B2C Tenant with:
   * Necessary User Flows
   * App Registration with Web Redirect URIs: ![image](https://github.com/user-attachments/assets/fc30c5ed-1979-4fec-9232-cf12974396a1)

I also have this working with Github Actions for CI/CD deployment. 
```
name: Build and deploy ASP.Net Core app to Azure Web App - olsongames

on:
  push:
    branches:
      - yourbranch
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Set up .NET Core
        uses: actions/setup-dotnet@v4
        with:
          dotnet-version: '8.x'

      - name: Build with dotnet
        run: dotnet build --configuration Release

      - name: dotnet publish
        run: dotnet publish -c Release -o ${{env.DOTNET_ROOT}}/myapp

      - name: Upload artifact for deployment job
        uses: actions/upload-artifact@v4
        with:
          name: .net-app
          path: ${{env.DOTNET_ROOT}}/myapp

  deploy:
    runs-on: ubuntu-latest
    needs: build
    environment:
      name: 'your-environment'
      url: ${{ steps.deploy-to-webapp.outputs.webapp-url }}
    permissions:
      id-token: write #This is required for requesting the JWT

    steps:
      - name: Download artifact from build job
        uses: actions/download-artifact@v4
        with:
          name: .net-app
      
      - name: Login to Azure
        uses: azure/login@v2
        with:
          client-id: ${{ secrets.yoursecret }}
          tenant-id: ${{ secrets.yoursecret }}
          subscription-id: ${{ secrets.yoursecret }}

      - name: Deploy to Azure Web App
        id: deploy-to-webapp
        uses: azure/webapps-deploy@v3
        with:
          app-name: 'yourapp'
          slot-name: 'yourslot'
          package: .
          
```
