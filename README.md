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

### Viewing and Downloading Workflow Artifacts

After a workflow run completes, you can download the built NuGet package:

1. Navigate to the **[Actions](https://github.com/shahvirb/nuget-sandbox/actions)** tab in the GitHub repository
2. Click on a completed workflow run (e.g., "NuGet Build and Pack")
3. Scroll down to the **Artifacts** section at the bottom of the page
4. Click on **nuget-package** to download the `.nupkg` file as a ZIP archive
5. Extract the ZIP file to access the `.nupkg` file

**Alternative:** The workflow summary will also display a direct link and instructions for downloading the artifact.

**Where is the package built?**
- During the workflow, the package is built in the `./artifacts/` directory
- The file is named `HelloWorldLib.<version>.nupkg` (e.g., `HelloWorldLib.1.0.0.nupkg`)
- This artifact is then uploaded and available for download for 30 days

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
