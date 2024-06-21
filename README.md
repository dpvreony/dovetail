# Dovetail

## Mission Statement

* To provide a mechanism to embed Syncfusion license keys into applications as part of the build process.

## Introduction

Dovetail is a Roslyn Source Generator that picks up the license key in a way that:
* allows the key to not be stored in source control
* allows the embedding without the direct modification of the source code, again preventing the risk of the key accidentally ending up in source control.
  * Syncfusion has a couple of batch scripts that can be embedded into your csproj but these do alter the source code and do it a slightly blind fashion.

## Getting Started

You will need a Syncfusion license key

### 1. Create an application (such as WPF)
### 2. Add a nuget package reference to "Dovetail" in your project

```xml
<ItemGroup>
    <PackageReference Include="Dovetail" Version="1.0.0" />
</ItemGroup>
```

This will add an attributes package and the source generator package.

### 3. Add the following initialisation for the Syncfusion license manager

```cs
   Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense(SYNCFUSION_LICENSE_KEY);
```

### 4. Mark the class hosting the logic as partial and add the attribute to the class

```cs
    /// <summary>
    /// WPF Application entry point.
    /// </summary>
    [Dovetail.EmbedSyncfusionLicenseKey]
    public partial class App : Application
    {
      // YOUR CODE HERE
    }
```

### 5. Add the license key to a CI secret or your local environment variables.

???

## longer examples

### Full example that will fail if the license key isn't present

The source generator is written to set a Diagnostic Error if the attribute is included but the license key isn't present in 

```cs
    /// <summary>
    /// WPF Application entry point.
    /// </summary>
    [Dovetail.EmbedSyncfusionLicenseKey]
    public partial class App : Application
    {
        /// <summary>
        /// Initializes a new instance of the <see cref="App"/> class.
        /// </summary>
        public App()
        {
            Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense(SYNCFUSION_LICENSE_KEY);
        }
    }
```
```

### Full example that will not include the Syncfusion license manager if they license key isn't present.

The source generator has a .props file includes the compiler directive SYNCFUSION_LICENSE_KEY.

```cs
    /// <summary>
    /// WPF Application entry point.
    /// </summary>
#if DOVETAIL_SYNCFUSION_LICENSE_KEY
    [Dovetail.EmbedSyncfusionLicenseKey]
#endif
    public partial class App : Application
    {
        /// <summary>
        /// Initializes a new instance of the <see cref="App"/> class.
        /// </summary>
        public App()
        {
#if DOVETAIL_SYNCFUSION_LICENSE_KEY
            Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense(SYNCFUSION_LICENSE_KEY);
#endif
        }
    }
```

## Viewing the documentation

The documentation can be found at https://docs.dpvreony.com/projects/dovetail/

## Contributing to the code

See the [contribution guidelines](CONTRIBUTING.md).
