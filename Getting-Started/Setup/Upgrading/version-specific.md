#Version specific upgrades

*For now, this document will only list updates from the past two years, but it will eventually be improved to go further back.*

Most of the time you will be able to upgrade directly from your current version to the latest version. Follow the steps in the [general upgrade guide](general.md), then these additional instructions for the specific versions. (Remember that new config files are not mentioned because they are already covered in the general upgrade guide.)

##Version 4.6.1 to 4.7.1.1
* Delete bin/Iron*.dll (all dll files starting with "Iron")
* Delete bin/RazorEngine*.dll (all dll files starting with "RazorEngine")
* Delete bin/umbraco.MacroEngines.Legacy.dll
* Delete bin/Microsoft.Scripting.Debugging.dll
* Delete bin/Microsoft.Dynamic.dll

##Version 4.7.1.1 to 4.7.2
* Delete the bin/umbraco.MacroEngines.Legacy.dll file

##Version 4.7.2 to 4.8.0
* Delete the bin/App_Browsers.dll file
* Delete the bin/App_global.asax.dll file
* Delete the bin/Fizzler.Systems.HtmlAgilityPack.dll file
* For people using uComponents 3.1.2 or below, 4.8.0 breaks support for it. Either upgrade to a newer version beforehand or follow the workaround [posted here](http://our.umbraco.org/projects/backoffice-extensions/ucomponents/questionssuggestions/33021-Upgrading-to-Umbraco-48-breaks-support-for-uComponents)

##Version 4.8.0 to 4.10.0
* Delete the bin/umbraco.linq.core.dll file
* Copy the new files and folders from the zip file into your site's folder
 * /App_Plugins
 * /Views
 * global.asax
* Remove the Config/formHandlers.config file

##Version 4.10.x to 4.11.x
* If your site was ever a version between 4.10.0 and 4.11.4 install the [fixup package](http://our.umbraco.org/projects/developer-tools/path-fixup) and run it after the upgrade process is finished.

##Version 4.10.x/4.11.x to 6.0.0
* If your site was ever a version between 4.10.0 and 4.11.4 and you have just upgraded to 6.0.0 install the [fixup package](http://our.umbraco.org/projects/developer-tools/path-fixup) and run it after the upgrade process is finished.
* The DocType Mixins package is **NOT** compatible with v6+ and will cause problems in your document types.

##Version 6 to 7.0.0
Read and follow [the full v7 upgrade guide](upgrading-to-v7.md)

##Version 7.0.0 to 7.0.1
* Remove all uGoLive dlls from /bin
 * These are not compatible with V7
* Move appSettings/connectionStrings back to web.config
 * If you are on 7.0.0 you should migrate these settings into the web.config instead of having them in separate files in /config/
 * The keys in config/AppSettings.config need to be moved back to the web.config <appSettings> section and similarly, the config/ConnectionStrings.config holds the Umbraco database connections in v7.0.0 and they should be moved back to the web.config <connectionStrings> section.
 * /config/AppSettings.config and /config/ConnectionString.config can be removed after the contents have been moved back to web.config. (Make backups just in case)
* Delete all files in ~/App_Data/TEMP/Razor/*
 * Related to issues with razor macros

##Version 7.0.1 to 7.0.2

* There was an update to the /umbraco/config/create/ui.xml which needs to be manually updated, the original element had this text:

		<nodeType alias="users">
			<header>User</header>
			<usercontrol>/create/simple.ascx</usercontrol>
			<tasks>
			  <create assembly="umbraco" type="userTasks" />
			  <delete assembly="umbraco" type="userTasks" />
			</tasks>
		</nodeType>

	* The &lt;usercontrol&gt; value has changed to: **/create/user.ascx**, this is a required change otherwise creating a new user will not work.
* There is a breaking change to be aware of, full details can be found [here](http://umbraco.com/follow-us/blog-archive/2014/1/17/heads-up,-breaking-change-coming-in-702-and-62.aspx).


##Version 7.1.0
* Remove the /Install folder.

##Version 7.2.0
* Copy in the /Views/Partials/Grid (contains Grid rendering views)

##Version 7.3.0
Make sure to manually clear your cookies after updating all the files, otherwise you might an error relating to `Umbraco.Core.Security.UmbracoBackOfficeIdentity.AddUserDataClaims()`. The error looks like: `Value cannot be null. Parameter name: value`.

NuGet will do the following for you but if you're upgrading manually:

* Delete `bin/Microsoft.Web.Helpers.dll`
* Delete `bin/Microsoft.Web.Mvc.FixedDisplayModes.dll`
* Delete `bin/System.Net.Http.dll`
* Delete `bin/System.Net.Http.*.dll` (all dll files starting with `System.Net.Http`) **EXCEPT** for `System.Net.Http.Formatting.dll`
* Delete `bin/umbraco.XmlSerializers.dll`
* In your `web.config` file, add this in the `appSetting` section: `<add key="owin:appStartup" value="UmbracoDefaultOwinStartup" />`

Other considerations:

* WebApi has been updated, normally you don’t have to do anything unless you have custom webapi configuration:
  * See this article if you are using `WebApiConfig.Register`: [http://www.asp.net/mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2](http://www.asp.net/mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2) 
  * You need to update your `web.config` file to have the correct WebApi version references - this should be done by doing a compare/merge of your `~/web.config` file with the `~/web.config` file in the release
* MVC has been updated to MVC5
  * You need to update your `web.config` file to have the correct MVC version references - this should be done by doing a compare/merge of your `~/web.config` file with the `~/web.config` file in the release
  * The upgrader will take care of updating all other web.config’s (in all other folders, for example, the `Views` and `App_Plugins` folders) to have the correct settings
  * For general ASP.Net MVC 5 upgrade details see: [http://www.asp.net/mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2](http://www.asp.net/mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2) 
* It is not required that you merge the changes for the Examine index paths in the ExamineIndex.config file. However, if you do, your indexes will be rebuilt on startup because Examine will detect that they don’t exist at the new location.
* It's highly recommended to clear browser cache - the ClientDependency version is automatically bumped during install which should force browser cache to refresh, however in some edge cases this might not be enough.

##Version 7.4.0
For manual upgrades: 

* Copy the new folder `~/App_Plugins/ModelsBuilder` into the site
* Do not forget to merge `~/Config/trees.config` and `~/Config/Dashboard.config` - they contain new and updated entries that are required to be there
  * If you forget `trees.config` you will either not be able to browse the Developer section or you will be logged out immediately when trying to go to the developer section
* You may experience an error saying `Invalid object name 'umbracoUser'` - this can be fixed by [clearing your cookies on localhost](http://issues.umbraco.org/issue/U4-8031)
  
