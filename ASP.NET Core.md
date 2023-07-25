## Project Structure

### `.csproj`
> uses .csproj file to manage projects

![image](https://github.com/yul154/CSharp/assets/27160394/27c7f2fd-4404-4ef2-bccf-a16d49a9ba05)

A file extension that reflects the type of project—for example, a C# project (.csproj), a Visual Basic.NET project (.vbproj), or a database project (.dbproj).

In order to build a project, MSBuild must process the project file associated with the project. 
The project file is an XML document that contains all the information and instructions that MSBuild needs in order to build your project

All .NET projects list their dependencies in the `.csproj` file

The `.csproj` file also contains all the information that .NET tooling needs to build the project

![image](https://github.com/yul154/CSharp/assets/27160394/987f8338-82f5-4b8e-b03b-ecd42f14f9cc)

1. The Project Element `<Project>`：root element of every project file,include attributes to specify the entry points for the build process
2. Properties and Conditions`<PropertyGroup>`:properties must be defined within a PropertyGroup
3. Different pieces of information in order to successfully build and deploy,To retrieve a property value, use the format $(PropertyName), (server names, connection strings, credentials, build configurations, source and destination file paths)
  - Properties are often used in conjunction with conditions:`<Version Condition=" '$(VersionSuffix)' != '' ">$(Version).$(VersionSuffix)</Version>`
4. Items and Item Groups:define the inputs to the build process
  - must specify an Include attribute to identify the file or wildcard that the item represents



### Dependencies
> The Dependencies in the ASP.NET Core 2.1 project contain all the installed server-side NuGet packages, as shown below.

install all other required server side dependencies as NuGet packages from Manage NuGte Packages window or using Package Manager Console


### Properties

The Properties node includes `launchSettings.json` file which includes Visual Studio profiles of debug settings.


### Program.cs

```
namespace xxx.xxx.WebService
{
    public static class Program
    {
        public static void Main(string[] args)
        {
            CreateHostBuilder(args).Build().Run();
        }

        public static IHostBuilder CreateHostBuilder(string[] args) => //creating an instance of IWebHost and IWebHostBuilder with pre-configured defaults
            Host.CreateDefaultBuilder(args)
                .ConfigureAppConfiguration((context, config) =>
                {
                    config
                        .AddJsonFile("appsettings.json")
                        .AddJsonFile($"appsettings.{context.HostingEnvironment.EnvironmentName.ToLower()}.json", optional: true)
                        .AddEnvironmentVariables();
                })
                .ConfigureLogging((context, logging) =>
                {
                    logging.ClearProviders();
                })
                .ConfigureWebHostDefaults(webBuilder =>
                {
                    webBuilder
                        .UseStartup<Startup>() // startup class can be configured using UseStartup<T>()
                        .UseUrls($"http://*:5000/");
                });
    }
}
```
* `ConfigureAppConfiguration()` to load configurations from `appsettings.json` files
* startup class can be configured using `UseStartup<T>()`


### `Startup.cs`
> configuring the application's services and middleware

* it is executed first when the application starts.

![image](https://github.com/yul154/CSharp/assets/27160394/5c443992-9efd-4cc6-b88a-353ba2983c64)

Startup class includes two public methods: `ConfigureServices` and `Configure`.
- The Startup class must include a `Configure` method
  - The Configure method is a place where you can configure application request pipeline for your application using IApplicationBuilder instance that is provided by the built-in IoC container.
  - ASP.NET Core introduced the middleware components to define a request pipeline, which will be executed on every request
  - the ConfigureServices method is called before the Configure method
- can optionally include `ConfigureService` method.
  - `ConfigureServices` method is a place where you can register your dependent classes with the built-in IoC container
  - ASP.NET Core refers dependent class as a Service
  - ConfigureServices method includes IServiceCollection parameter to register services to the IoC container.



