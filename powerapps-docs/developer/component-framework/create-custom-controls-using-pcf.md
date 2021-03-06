---
title: Create custom component using PowerApp Component Framework Tooling| Microsoft Docs
description: Start creating a component using the PowerApps component framework Tooling
keywords: PowerApps component framework, Custom components, Component Framework
ms.author: nabuthuk
manager: kvivek
ms.date: 04/23/2019
ms.service: "powerapps"
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: d2cbf58a-9112-45c2-b823-2c07a310714c
---

# Create, debug and deploy a component using  Microsoft PowerApps CLI

[!INCLUDE[cc-beta-prerelease-disclaimer](../../includes/cc-beta-prerelease-disclaimer.md)]

Use PowerApps Command Line Interface (CLI) to create, debug and deploy custom PowerApps component framework components. PowerApps CLI will enable developers to quickly create PowerApps component framework components and will in the future be expanded to include support for additional development and Application Lifecycle Management (ALM) experiences. 

## What is Microsoft PowerApps CLI? 

Microsoft PowerApps CLI is a simple, single-stop developer command line interface enabling you to create custom component. PowerApps CLI is also the first step towards a comprehensive ALM story where enterprise developers and ISVs can create, build, debug and publish their PowerApps and Dynamics 365 for Customer Engagement apps extensions and customizations quickly and efficiently.  

> [!IMPORTANT]
> - Microsoft PowerApps CLI tools are a pre-release version and may be different from the commercially released version.
> - [!INCLUDE[cc_preview_features_definition](../../includes/cc-preview-features-definition.md)] 
> - If you give feedback about the software to Microsoft, you give to Microsoft, without charge, the right to use, share and commercialize your feedback in any way and for any purpose. 
> - Microsoft doesn't provide support for this preview feature. Microsoft Technical Support won’t be able to help you with issues or questions.

## Prerequisites to use PowerApps CLI

To use PowerApps CLI you will need the following:

- Install [Npm](https://www.npmjs.com/get-npm)(comes with Node.js) or install [Node.js](https://nodejs.org/en/) (comes with npm). We recommend LTS (Long Term Support) version 10.15.3 LTS as it seems to be most stable.
- If you don’t already have Visual Studio 2017 or later, follow one of the options below:
   - Option 1: Install Visual Studio 2017 or later
   - Option 2: Install .NET Core 2.2 SDK and install Visual Studio Code
- Install Microsoft PowerApps CLI from [here](http://download.microsoft.com/download/D/B/E/DBE69906-B4DA-471C-8960-092AB955C681/powerapps-cli-0.1.51.msi)



> [!NOTE]
> To deploy your custom component, you will need Common Data Service environment with System administrator or System customizer privileges.

> [!NOTE]
> Currently PowerApps CLI is supported only on Windows 10.

## Creating a new PowerApps component framework component

To get started, open a new Developer Command Prompt for VS 2017 after installing PowerApps CLI.

1. In the Developer Command Prompt for VS 2017, create a new folder on your local hard drive for example, *C:\Users\<your name>\Documents\My_PCF_Control*
2. Go to the newly created folder using the command `cd <specify your new folder path>`
3. Run the following command to create a new component project by passing some basic parameters
 `pac pcf init --namespace <specify your namespace here> --name <put component name here> --template <component type>`
 
   > [!NOTE]
   > Today we offer two types of components **field** and **dataset**.

4. To retrieve all the required project dependencies, run the command `npm install`.
5. Open your project folder (`C:\Users\<your name>\Documents\My_PCF_Control\<component name>`) in any developer environment of your choice and get started with your custom component development. If you would like a to follow a step-by-step tutorial please scroll down see how a sample linear component is implemented.

## Building your components

To build your component you can open the folder in Visual Studio Code and use the (Ctrl-Shift-B) command, then select your build options. Alternately, you can build your control quickly using  `npm run build` command in your Developer Command Prompt for VS 2017 window.

## Debugging your PowerApps component framework component

Once you are done implementing your custom component logic, run the following command to start the debugging process
`npm start`

> [!NOTE]
> Today you can only visualize your field component, but dataset support is coming soon. Below image shows a sample component implemented in the tutorial below just as an example. 

> [!div class="mx-imgBorder"]
> ![local-host](media/local-host.png "local host")

As shown in the image above, the browse window will open with 3 sections. Your component will be rendered in the left pane while the right pane consists of **Inputs** and **Outputs** sections

  - **Inputs** section is an interactive UI that displays all properties and their types/type-groups defined in the manifest.xml. It allows you to key in mock data for each property. 
  - **Outputs** section renders the output whenever a component's `getOutputs` method gets called.  
 
> [!NOTE]
> If you want to modify the `manifest.xml` or create additional properties, you will need to restart the debug process before they appear in the inputs section.

As you are inputting mock data, you can use the browser’s debugging capabilities to observe the component behavior. Each browser provides you with a debugging tool to help you debug your code natively in the browser. Typically, you can activate debugging in your browser by pressing the **F12** key to display the native developer tool used for debugging. Today both Chrome and Edge browsers are supported.

For example, on **Microsoft Edge**,

- Press **F12** to open inspector.
- Click on your component
- On top bar, go to **Debugger**, and then start searching for the component name described in the Manifest file in the search bar. For example, type your component name like `Hello World component`.

     > [!div class="mx-imgBorder"]
     > ![debug-component](media/debug-control.png "Debug component")

> [!NOTE]
> It is always a good practice to set breakpoints on the component's life cycle methods like `init` and `updateView`

You can also interact with the component locally in real time and observe elements in the DOM by setting a breakpoint in the sources tab as follows:

> [!div class="mx-imgBorder"]
> ![local-host](media/local-host.png "local host")

> [!div class="mx-imgBorder"]
> ![debug-component](media/debug-control-1.png "Debug component 1")


 > [!NOTE]
 > You can also use the following steps to perform outer loop debugging using fiddler.
 >    1. Install [Fiddler](https://www.telerik.com/download/fiddler)
 >    2. Follow the steps to configure [AutoResponder](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/developer/streamline-javascript-development-fiddler-autoresponder)

## Deploying your PowerApps component framework components

Once the debugging and development is finished, you just have one step remaining - to deploy your new component.  

Follow the steps below to create and import a [solution](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/customize/solutions-overview) file:

1. Create a new directory and go to it 'cd <new directory name>'
2. Create a new solution project in the directory of your choice by using the command 
 `pac solution init --publisherName <enter your publisher name> --customizationPrefix <enter your publisher name>` after `cd <your new folder>`.

   > [!NOTE]
   > The [publisherName](https://docs.microsoft.com/en-us/powerapps/developer/common-data-service/reference/entities/publisher) and [cutomizationPrefix](https://docs.microsoft.com/en-us/powerapps/maker/common-data-service/change-solution-publisher-prefix) values must be unique to your environment.
 
3. Once the new solution project is created, you need to refer to the location where the created component is located. You can add the reference by using the command
`pac solution add-reference --path <path or relative path of your PowerApps component framework project on disk>`
4. To generate a zip file from your solution project, you will need to `cd` into your solution project directory and build the project using the command `msbuild /t:restore` then `msbuild`

    > [!NOTE]
    > If msbuild 15 is not in the path, open Developer Command Prompt for Vs 2017 to run the `msbuild` commands.

    > [!NOTE]
    > Building the solution in the debug configuration, generates an unmanaged solution package. A managed solution package is generated by building the solution in release configuration. These settings can be overridden by specifying SolutionPackageType property in `cdsproj` file.
    
    > [!NOTE]
    > If you would like your project build to emit a managed solution or both managed and unmanaged, open the folder where you created your solution project, edit the `cdsproj` file and uncomment the below property group:
      ```XML
         <PropertyGroup>
          <SolutionPackageType>Managed</SolutionPackageType>
           </PropertyGroup>
      ```

    > [!NOTE]
    > You can also enable additional solution packaging [capabilities](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/developer/compress-extract-solution-file-solutionpackager) by adding any of the property group elements below:

     ```XML
       <PropertyGroup>
         <SolutionPackageErrorLevel />
         <SolutionPackageEnableLocalization />
        <SolutionPackagerWorkingDirectory />
        <SolutionPackageLogFilePath />
        <SolutionPackageZipFilePath />
        <SolutionPackageMapFilePath />
       </PropertyGroup>
     ```

5. The generated solution zip file is located in `\bin\debug\`.
6. You should manually [import the solution](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/customize/import-update-export-solutions) using the web portal once the zip file is ready.

## Adding custom controls to entity or a field

To add a custom component like data-set component or simple table component to a grid or view, follow the steps mentioned in the topic [Add controls to fields and entities](add-custom-controls-to-a-field-or-entity.md). 

## Telemetry

The feature team is aggregating anonymized telemetry in order to understand which features or capabilities in the PowerApps CLI tool are most often used by the developers. The aggregated data allows us to provide the best experience to the customers by focusing on what’s truly is important.

> [!NOTE]
> To disable the telemetry collection, run the command `pac telemetry --enable false`. To turn the telemetry back, use the command `pac telemetry --enable true`.

## How to Uninstall Microsoft PowerApps CLI

To uninstall the CLI tool please run the MSI from [here](http://download.microsoft.com/download/D/B/E/DBE69906-B4DA-471C-8960-092AB955C681/powerapps-cli-0.1.51.msi). If you are Private Preview Participant and have an older version of CLI, please follow below manual steps:

1. To find out where PowerApps CLI is installed, open a command prompt and type `where pac`
1. Delete the PowerAppsCLI folder.
1. Open Environment Variables tool by running command `rundll32 sysdm.cpl,EditEnvironmentVariables` in the command prompt
1. Double-click on `Path` under `User variable for...` section
1. Select the row containing PowerAppsCLI path and click the Delete button on the right-hand side
1. Click OK twice

## Known Configuration Issues and Workarounds

**Msbuild error MSB4036:**

1. The name of the task in the project file is the same as the name of the task class.
2. The task class is public and implements the Microsoft.Build.Framework.ITask interface.
3. The task is correctly declared with <UsingTask> in the project file or in the *.tasks files located in the <path> directory

**Resolution:**

1. Open Visual Studio Installer 
1. For VS 2017, click on modify 
1. Click on Individual Components
1. Under Code Tools, check **NuGet targets & Build Tasks**

### See also

[Implementing components in TypeScript](implementing-controls-using-typescript.md)<br/>
[Updating existing components into new tools format](updating-existing-controls.md)<br/>
[PowerApps component framework API Reference](reference/index.md)<br/>
[PowerApps component framework Overview](overview.md)
