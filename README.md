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
