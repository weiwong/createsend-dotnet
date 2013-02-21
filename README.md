# createsend-dotnet

A .NET library which implements the complete functionality of the Campaign Monitor API.

This library is supported on .NET 2, 3.5, and 4. You will find solution files which target the different .NET runtime versions: `createsend-dotnet.net20.sln`, `createsend-dotnet.net35.sln`, and `createsend-dotnet.sln`.

## Installation

Using NuGet (recommended):

1. Run `Install-Package campaignmonitor-api` from the Package Manager Console in Visual Studio. See the [NuGet documentation](http://nuget.codeplex.com/documentation) for further details.

If you don't want to use NuGet:

1. Open the solution file which targets your preferred version of the runtime. 
2. Build the solution in Release mode. You will see a sub-directory in the `createsend-dotnet/bin/Release directory`, which contains the assemblies targeting your preferred version of the runtime. 
3. Add the resulting assemblies as references in your project. 

## Authenticating

The Campaign Monitor API supports authentication using either OAuth or an API key.

### Using OAuth

TODO: Add full instructions...

Once you have an access token and refresh token for your user, you can authenticate and make further API calls like so:

```csharp
using System;
using System.Collections.Generic;
using createsend_dotnet;

namespace dotnet_api_client
{
    class Program
    {
        static void Main(string[] args)
        {
            AuthenticationDetails auth = new OAuthAuthenticationDetails(
                "your access token", "your refresh token");
            var general = new General(auth);
            var clients = general.Clients();
        }
    }
}
```

### Using an API key

```csharp
using System;
using System.Collections.Generic;
using createsend_dotnet;

namespace dotnet_api_client
{
    class Program
    {
        static void Main(string[] args)
        {
            AuthenticationDetails auth = new ApiKeyAuthenticationDetails(
                "your api key");
            var general = new General(auth);
            var clients = general.Clients();
        }
    }
}
```

## Basic usage

This example of listing all your clients and their campaigns demonstrates basic usage of the library and the data returned from the API:

```csharp
using System;
using System.Collections.Generic;
using createsend_dotnet;

namespace dotnet_api_client
{
    class Program
    {
        static void Main(string[] args)
        {
            AuthenticationDetails auth = new OAuthAuthenticationDetails(
                "your access token", "your refresh token");
            var general = new General(auth);
            IEnumerable<BasicClient> clients = general.Clients();
            foreach (BasicClient c in clients)
            {
                Console.WriteLine(string.Format("Client: {0}", c.Name));
                var cl = new Client(auth, c.ClientID);
                Console.WriteLine("- Campaigns:");
                foreach (CampaignDetail cm in cl.Campaigns())
                    Console.WriteLine(string.Format("  - {0}", cm.Subject));
            }
        }
    }
}
```

## Contributing
1. Fork the repository
2. Make your changes.
3. Ensure that the build passes for all three solutions (`createsend-dotnet.sln`, `createsend-dotnet.net35.sln`, and `createsend-dotnet.net20.sln`)
4. It should go without saying, but do not increment the version number in your commits.
5. Submit a pull request.