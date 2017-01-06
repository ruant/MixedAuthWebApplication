# MixedAuthWebApplication -
Forked from https://github.com/SergeySorokin/MixedAuthWebApplication


#Notes

## IIS issue
http://stackoverflow.com/questions/9794985/iis-this-configuration-section-cannot-be-used-at-this-path-configuration-lock

## IIS Express issue
(Credit: http://digitaldrummerj.me/iis-express-windows-authentication/)

__Error:__
The requested page cannot be accessed because the related configuration data for the page is invalid.

__Details:__
This configuration section cannot be used at this path. This happens when the section is locked at a parent level. Locking is either by default (overrideModeDefault=”Deny”), or set explicitly by a location tag with overrideMode=”Deny” or the legacy allowOverride=”false”.

__Solution:__
Every time I bring up a new machine I always forget to update the IIS Express setting to fix this error and have to do a google search to figure out where the IIS Express configuration is stored. So I figured I should finally document the fix for it.

The error is caused by this section in the web.config

    <system.webServer>
    	<security>
    		<authentication>
    			<windowsAuthentication enabled="true" />
    			<anonymousAuthentication enabled="false" />			
    		</authentication>
	    </security>
    </system.webServer>
    
To fix this open up the IIS Express applicationhost.config. This file is stored at C:\Users[your user name]\Documents\IISExpress\config\applicationhost.config

Update for VS2015+: config file location is $(solutionDir).vs\config\applicationhost.config

Look for the following lines:

    <section name="windowsAuthentication" overrideModeDefault="Deny" />
    <section name="anonymousAuthentication" overrideModeDefault="Deny" />
    <add name="WindowsAuthenticationModule" lockItem="true" />
    <add name="AnonymousAuthenticationModule" lockItem="true" />

Change those lines to:

    <section name="windowsAuthentication" overrideModeDefault="Allow" />
    <section name="anonymousAuthentication" overrideModeDefault="Allow" />
    <add name="WindowsAuthenticationModule" lockItem="false" />
    <add name="AnonymousAuthenticationModule" lockItem="false" />

Save the file and refresh your asp.net web page. It may take a moment to load as it load the new configurations into IIS Express.
