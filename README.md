# Zodiac for Sitecore 10
## A starter kit for Sitecore implementation projects.
## [Sponsored by Verndale](https://www.verndale.com)
# What's in the Box
A "complete" Visual Studio 2019 solution that you can fork/clone and use as the basis of your net-new Sitecore implementation project.

Features include:
- ASP.NET MVC implementation
- No external build frameworks required, just Visual Studio 2019
- Helix-compatible
- IIS Url Rewrite 2.1 compatible with "Sitecore safe" default rules
- "Must Have" overrides for Sitecore configuration defaults that provide improved performance during development and production.
- Drop-in Rules for organizing Content Tree Items alphabetically or by date
- A framework for creating navigation within the Content Tree and retrieving it on the presentation layer
- Drop-in solution for managing analytics scripts within the CMS
- Drop-in solution for page meta tags including basic support for social network tagging
- Drop-in solution for managing 301/302 redirects within the CMS
- Drop-in Rules for managing the URLs of Page Items
- A framework for accessing the Sitecore Cache
- Drop-in semantics and supporting framework for organizing page and page-fragment Items
- Drop-in solution for managing link options on a site-by-site basis
- Lightweight Item to ViewModel mapping framework
- Base MVC components to enforce Item-Repository-Controller-ViewModel-View design pattern
- Drop-in solution for ensuring a Sitecore Package.Zip file is installed in a given instance
- A framework for creating dynamic robots.txt and sitemap.xml responses for each site in the installation


# Installation
There are several options, depending upon what you want to do. In all cases, you need a target Sitecore 10 installation! so...


## Step 1: Compatibility Check!
### Workstation Assumptions:
- You are running Windows 10, 64-bit.
- IIS 10 is installed.
- IIS Url Rewrite 2.1 https://www.iis.net/downloads/microsoft/url-rewrite is installed.
- Your Windows instance meets Sitecore's minimum requirements of 4 cores and > 16 GB of RAM
- If you're running SOLR and SQL Server locally as well, we recommend having even more cores & RAM available. (at least 8 threads)

___If your machine does not meet these minimums, you will likely experience resource-related problems that are virtually impossible to troubleshoot.___


### Development Enviornment Assumptions:
- For Sitecore 10, you must have the .NET Framework SDK version 4.8 installed.
- This solution was tested on Visual Studio 2019 v16.6.5. 
- You must run Visual Studio in "as Administrator" mode to ensure you have sufficient local privileges.
- Your NuGet feeds should include the Sitecore package feeds. You can get the feed links here: https://sitecore.myget.org/gallery/sc-packages 


## Step 2: Install Sitecore 10
Since these instructions are intended for developers expecting to use Zodiac for development, we assume you are installing a "Standalone" local instance of Sitecore on your personal development machine.

- Follow the instructions on https://dev.sitecore.net/
- Note the hostname and the installation folder of your new install, you'll need these facts later.


## Step 3: Verify Sitecore 10 is Installed Correctly
It is essential that you have a "stock" copy of Sitecore running before proceeding.
- If you load your supplied hostname, do you get the default Sitecore web page?
- Can you log into Sitecore?
- Can you load the Content Editor and browse the Content Tree?
- Can you see & rebuild all of the SOLR indexes?
- Rebuild all the SOLR indexes.
- Can you run a "site publish?"
- Can you create users?
- Can you load the default web page in Experience Editor?
- Can you edit the content in Experience Editor?
- Have you resolved all errors in your log file?

If the answer to anything above is "no", stop now and address these problems. 

___IMPORTANT: Zodiac will not be stable if Sitecore is not stable. Fix any install issues now.___

- Run a full Site Publish
- Rebuild all Indexes

___IMPORTANT: Rebuilding the Indexes makes sure your otherwise "new" Sitecore installation is fully primed. Buckets and Content Search will not function until you have rebuilt the Indexes. Zodiac REQUIRES these Indexes to be built and available.___


## Step 4: Clone or Fork the Repository:
- https://github.com/constellation4sitecore/zodiac-sitecore10.git

Go ahead & set up a Working Copy on your local machine. We recommend creating a new Branch based on the "master" branch of the repo.


## Step 5: Load the Solution in Visual Studio
- Zodiac-Sitecore10.sln

Assuming Visual Studio fires up and everything looks OK, click on the Solution in Solution Explorer and choose "Restore NuGet Packages".
After NuGet Packages have been restored, run a Solution Build. Verify that the solution does build. _Building will not deploy, and you should *not* deploy at this stage._

___IMPORTANT: Do not perform a TDS sync at this time, Just run the Build to verify all your Nuget packages installed.___

## Step 6: AppSettings.Config
In the "Website" project there is an AppSettings.Config file located at /App_Config/AppSettings.config. You ___must___ manually copy this file into your local installation. It contains essential pre-Sitecore config patch settings that must be set to parse the config files correctly.

        Example = C:\Inetpub\wwwroot\zodiac.local.sc\App_Config\AppSettings.config

___IMPORTANT: After Deployment, your install will not start unless this file exists at the above location.___


## Step 7: Configure Visual Studio Publishing
- In the Solution Explorer select the "Website" project. 
- If it's not already visible, add the "Web One-Click Publish" toolbar by right-clicking outside any existing Toolbars in Visual Studio, and selecting it from the menu. 
- In the (now visible) "Publish" toolbar, there should be a "Debug" target selected. Click the cog/gear icon to edit the publishing profile. 
- Within the "Publish" panel that is now exposed, ensure the "Target Location" matches the location of your IIS Web App's root. (Use the Configure link to change it.)

        Example Target Location = "C:\inetpub\wwwroot\zodiac.local.sc"

## Step 8: Deploy!
With the "Website" project selected in Solution Explorer, hit the "publish" button in the One-Click Publish toolbar.

___IMPORTANT: Do not perform a TDS sync at this time, Just run the Publish to push your code to your Sitecore installation___


## Step 9: Run Sitecore
Access your Sitecore installation in a browser, but don't log into Sitecore yet. You should get the default "Sitecore Girl" page.

___IMPORTANT: After Visual Studio deploys your solution for the first time, you _must_ run Sitecore immediately. This will install a number of prerequisite Items that allow you to start development.___


## Step 10: Log into Sitecore
Access the /sitecore login page and authenticate. Run a Full Site publish and then Rebuild all Indexes.

___IMPORTANT: Rebuilding the Indexes makes sure your otherwise "new" Sitecore installation is fully primed. Buckets and Content Search will not function until you have rebuilt the Indexes.


## Step 11: Configure Sitecore to load the Example Site
If you are using one of the solutions that includes the Example Site, you need to adjust the configuration files so that Sitecore will resolve the site properly.

Assuming you want the example site to be the "default" site for your install, you need to add the hostname you supplied in the install to your site config. 

In the "Website" project in Visual Studio:
- Open the following file: Website/App_Config/Include/Project/ExampleSite/_SiteDefinition.local.config
- Adjust the "hostName" and "targetHostName" attribute values to match your install's hostname.

Note that if you want the example site, but you don't want it to be the "primary" site on your install, you can always add another hostname to your IIS website bindings. The above instructions still apply.

## Step 12: Deploy and Publish!
- With the "Website" project selected in Solution Explorer, hit the "publish" button in the One-Click Publish toolbar.
- Log into Sitecore Desktop.
- Run a Full Publish (Smart or Republish)
- Using your selected hostname, visit the front end. You should see the Example Site home page.

## Step 13: Season to Taste
At this point you should have a running installation using Zodiac (with Constellation) as the backbone. You can now start development of your solution!

For details on how to use various features of this installation, visit https://constellation4sitecore.com/ the official documentation site for Constellation and Zodiac.

# Credits
A big thank you to [Verndale](https://www.verndale.com) for providing the time to assemble this kit.

### Sponsors/Developers/Testers
(in no particular order)
- Hetal Dave
- Davan Etelamaki
- Sandy Foley
- Joe Fusco
- Deepthi Katta
- Richard Leiva
- Ozell McBride
- Santiago Morla
- Mike Reynolds
- Liz Spranzani
- Steve Striga
