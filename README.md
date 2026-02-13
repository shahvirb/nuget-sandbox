# HelloWorldLib - NuGet Sandbox

A minimal Hello World C# class library for demonstrating NuGet packaging and CI/CD workflows.

## Project Structure

```
├── src/
│   └── HelloWorldLib/
│       ├── HelloWorldLib.csproj
│       └── Class1.cs
├── .github/
│   └── workflows/
│       └── nuget.yml
├── HelloWorldLib.sln
└── README.md
```

## Requirements

- [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8.0)

## Quick Start

### Building Locally

Build the project:
```bash
dotnet build src/HelloWorldLib/HelloWorldLib.csproj
```

Or build using the solution file:
```bash
dotnet build HelloWorldLib.sln
```

### Packing Locally

Create a NuGet package:
```bash
dotnet pack src/HelloWorldLib/HelloWorldLib.csproj -c Release
```

The `.nupkg` file will be created in `src/HelloWorldLib/bin/Release/`.

### Using the Library

After building, you can reference the library in other projects:

```csharp
using HelloWorldLib;

var greeter = new Class1();
string message = greeter.Hello("World");
Console.WriteLine(message);  // Output: Hello, World!
```

## CI/CD Workflow

The repository includes a GitHub Actions workflow (`.github/workflows/nuget.yml`) that automatically:

### On Push to `main` or Pull Request:
- Sets up .NET 8
- Restores dependencies
- Builds the project in Release configuration
- Runs tests (if any exist)
- Packs the library into a `.nupkg`
- Uploads the package as a workflow artifact

### On Tag Push (`v*.*.*`):
- Performs all build steps above
- Publishes the package to NuGet.org using `NUGET_API_KEY` secret (with `--skip-duplicate`)

### Viewing Workflow Artifacts

1. Navigate to the **Actions** tab in the GitHub repository
2. Select a completed workflow run
3. Download the `nuget-package` artifact from the artifacts section

### Publishing to NuGet.org (Optional)

To enable publishing on tagged releases:

1. Create a NuGet API key at https://www.nuget.org/account/apikeys
2. Add the API key as a secret named `NUGET_API_KEY` in your repository settings
3. Create and push a version tag:
   ```bash
   git tag v1.0.0
   git push origin v1.0.0
   ```

## Package Metadata

The package is configured with the following metadata in `HelloWorldLib.csproj`:

- **PackageId**: HelloWorldLib
- **Authors**: shahvirb
- **Description**: A Hello World library
- **RepositoryUrl**: https://github.com/shahvirb/nuget-sandbox
- **TargetFramework**: net8.0

## License

This is a sandbox/demo repository.
