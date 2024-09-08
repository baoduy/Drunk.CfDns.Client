# Drunk.Cf.Dns

Drunk.Cf.Dns is a .NET library that provides a client for interacting with Cloudflare's DNS API. It leverages Refit to create a strongly-typed, easy-to-use HTTP client.

## Features

- List DNS records
- Find DNS records by name
- Create new DNS records
- Update existing DNS records
- Delete DNS records

## Installation

You can install the package via NuGet:

```sh
dotnet add package Drunk.Cf.Dns
```

## Usage

### Configuration

First, you need to configure the Cloudflare DNS client in your `IServiceCollection`:

```csharp
using Microsoft.Extensions.DependencyInjection;
using Drunk.Cf.Dns;

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddCloudflareDnsClient(provider => ("your-email@example.com", "your-api-token"));
    }
}
```

### Creating the Client

You can create the client manually if you prefer:

```csharp
using Drunk.Cf.Dns;

var client = CfDnsConfigs.Create("your-email@example.com", "your-api-token");
```

### Using the Client

Here are some examples of how to use the client:

#### List DNS Records

```csharp
var zoneId = "your-zone-id";
var dnsRecords = await client.ListAsync(zoneId);

if (dnsRecords.Success)
{
    foreach (var record in dnsRecords.Result)
    {
        Console.WriteLine($"{record.Name} - {record.Type} - {record.Content}");
    }
}
```

#### Find DNS Records by Name

```csharp
var zoneId = "your-zone-id";
var name = "example.com";
var dnsRecords = await client.FindByNameAsync(zoneId, name);

if (dnsRecords.Success)
{
    foreach (var record in dnsRecords.Result)
    {
        Console.WriteLine($"{record.Name} - {record.Type} - {record.Content}");
    }
}
```

#### Create a DNS Record

```csharp
var zoneId = "your-zone-id";
var newRecord = new DnsRecord
{
    Type = RecordType.A,
    Name = "example.com",
    Content = "192.0.2.1",
    Ttl = 3600,
    Proxied = false
};

var createResponse = await client.CreateAsync(zoneId, newRecord);

if (createResponse.Success)
{
    Console.WriteLine($"Created record with ID: {createResponse.Result.Id}");
}
```

#### Update a DNS Record

```csharp
var zoneId = "your-zone-id";
var recordId = "your-record-id";
var updatedRecord = new DnsRecord
{
    Type = RecordType.A,
    Name = "example.com",
    Content = "192.0.2.2",
    Ttl = 3600,
    Proxied = false
};

var updateResponse = await client.UpdateAsync(zoneId, recordId, updatedRecord);

if (updateResponse.Success)
{
    Console.WriteLine($"Updated record with ID: {updateResponse.Result.Id}");
}
```

#### Delete a DNS Record

```csharp
var zoneId = "your-zone-id";
var recordId = "your-record-id";

await client.DeleteAsync(zoneId, recordId);

Console.WriteLine("Record deleted successfully.");
```

## License

This project is licensed under the MIT License.
