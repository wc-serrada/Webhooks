# Webhooks
This is an implementation of a standalone generic publish/subscribe notification system using webhooks and Hangfire as an event dispatcher.

Documentation: https://webhooks.hyprsoft.com/docs

## .NET 5 Upgrade Notes
In order to support the classic .NET framework, we use .NET standard 2.0 libraries.  Apparently the latest EF Nugets for .NET 5 only support .NET Standard 2.1 which doesn't support the classic .NET Framework.  Reference https://docs.microsoft.com/en-us/dotnet/standard/net-standard.  For now we have a separate branch.

## Sample Code
``` csharp
// Subscribe
var client = new WebhooksClient(new WebhooksHttpClientOptions { ServerBaseUri = new Uri("http://webhooks.hyprsoft.com/") });
var webhookUri = new Uri("http://webhooks.hyprsoft.com/webhooks/v1/samplecreated");
await client.SubscribeAsync<SampleCreatedWebhookEvent>(webhookUri, x => x.SampleType == 2);

// Publish
var client = new WebhooksClient(new WebhooksHttpClientOptions { ServerBaseUri = new Uri("http://webhooks.hyprsoft.com/") });
await client.PublishAsync(new SampleCreatedWebhookEvent { SampleId = 1, SampleType = 2, UserId = 3, ReferenceId = 4 });
```

## Install as Windows Service
``` 
sc create HyprsoftWebhooksServer binpath= "D:\Source\Hyprsoft.Webhooks.Solution\Hyprsoft.Webhooks.Server.Web\bin\Release\netcoreapp3.1\win-x64\publish\Hyprsoft.Webhooks.Server.Web.exe  --service 1" DisplayName= "Hyprsoft Webhooks Server" start= auto
sc description HyprsoftWebhooksServer "Hyprsoft Webhooks Server" 
net start HyprsoftWebhooksServer

sc query HyprsoftWebhooksServer 
net stop HyprsoftWebhooksServer 
sc delete HyprsoftWebhooksServer 
```

## Sample Client Web Command Line
```
Hyprsoft.Webhooks.Client.Web.exe ServerBaseUri="https://webhooks.hyprsoft.com/" WebhooksBaseUri="http://office.hyprsoft.com/" Role=PubSub AutoUnsubscribe=true
```