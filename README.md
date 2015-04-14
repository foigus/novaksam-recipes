# Recipes

This is the repo for Sam Novak's modifed JSS recipes that are setup to be used with the latest version of JSSImporter

The recipes must be kept in the root folder so that it can correctly access template, script and image folders.

<h2>Keys:</h2>

Please note, case sensitivity.
* CATEGORY - The category the pkg will be uploaded into.
* EXTENSION_PATH - (Extension attributes only) The folder path pointing to the folder where Info.plist resides.
* EXTENSION_TEMPLATE - (Extension attributes only) The template file to use when creating the extension attribute.
* GROUP_NAME - The name of the group in JSS that will keep track of the software version.
* GROUP_TEMPLATE - The template file to use when creating the group.
* ICON - The path to the icon file to upload for self service.
* NAME - The name you want to give to the software (ex. {NAME}-{VERSION}.pkg)
* POLICY_CATEGORY - The category the installtion policy will be located in.
* POLICY_TEMPLATE - The template file to be used when creating the installation policy.
* POLICY_TRIGGER - The event that can be called to run the policy outside of self service (ex. jamf policy -event {POLICY_TRIGGER})

<h2>Notes:</h2>

My recipes overall are pretty flexible, so you should be able to create overrides and modify them to fit your needs pretty easily.

I've made the template files pretty flexible too, so you can modify them how you'd like to use them in your shop.

<h3>Example:</h3>

You want a policy to run at checkin.
Duplicate PolicyTemplate.xml to MyPolicyTemplate.xml (or some other name) and open it
Modify the line 

    <trigger_checkin>false</trigger_checkin>
to

    <trigger_checkin>true</trigger_checkin>
Now open the recipe/override you want to run at checkin and modify

    <string>%RECIPE_DIR%/Templates/PolicyTemplate.xml</string>
to

    <string>%RECIPE_DIR%/Templates/MyPolicyTemplate.xml</string>

**There** are some pieces of software that require Extension Attributes to keep track of software versions, and those recipes are constructed a slightly different way.

<h3>Example:</h3>

You want to keep track of flash
In the recipe/override modify the GROUP_TEMPLATE to use

    <string>%RECIPE_DIR%/Templates/SmartGroupTemplate-Extension.xml</string>
and add the following Keys/strings to the input section

    <key>EXTENSION_PATH</key>
    <string>/Library/Internet\ Plug-Ins/Flash\ Player.plugin/Contents</string>
    <key>EXTENSION_TEMPLATE</key>
    <string>%RECIPE_DIR%/Templates/ExtensionAttributeTemplate.xml</string>
The EXTENSION_PATH key is used to give the path where the Info.plist file resides for a particular piece of software.


**There** are some pieces of software that have minimum operating system requirements (iMovie, iPhoto, etc) so in the case of these recpies the GROUP_TEMPLATE string must be changed to the appropriate template.

<h3>Example:</h3>

iMovie get updated and requires 10.10
In the iMovie recipe, change 

    %RECIPE_DIR%/Templates/SmartGroupTemplate-Application.xml
to

    %RECIPE_DIR%/Templates/SmartGroupTemplate-Application-Yose.xml

**There** are also some extension atributes that are a pain to get because of the variable they use in the Info.plist file, so there is another EA template for this scenario.

Github uses a funky CFBundleShortVersion string in their Info.plist file for version tracking
to work around this, there is a specific template used to track the software that reads the CFBundleVersion string instead.
Now you'll be able to track github version with an Extension attribute that returns a version number.
    

<h2>Requires the following repos:</h2>

autopkg: AdobeAIR, FlashPlayerExtractPackage, AdobeReader, Dropbox, Evernote, Firefox, GoogleChrome, Handbrake, OracleJava7, OracleJava8, sassafras-k2client, Silverlight, Skype, TheUnarchiver, VLC

hansen-m: AdobeDigitalEditions, OracleJava8JDK

    hansen-m is currently the only repo offering pkg for AdobeDigitalEditions and Oracle JDK 8

jleggat: AdobeShockwavePlayer

    Jleggat is the only repo currently offering Shockwave Player.

scriptingosx: audacity, Casper Suite, garageband, iMovie, iPhoto_, keynote_, Numbers_, Pages_, xcode_, XQuartz
    
    Scriptingosx offers a number of AppStore pieces of software, along with some additional pkg recipes that are only offered there (currently)

nmcspadden: appstore

    Nmcspadden offers the appstore recipe, which is super useful when packaging appstore software. I use it multiple times.

cgerke-recipes: GoogleDrive, Wireshark

    Cgerke-recipes offers a PKG for GoogleDrive and Wireshark, which I would otherwise have to pull from at least 2 other repos.

arubdesu: LyncInstaller

    Arubdesu offers the only Lync downloader currently offered and offers a pkg recipe for it. I sort or re-package it to display the version number in the installer.

jazzace: processing

    Jazzace is the only repo currently offering processing.

justinrummel: VMwareFusion

    Justinrummel is the only repo currently offering a PKG of VMWareFusion

kelleysam: Github
    
    Kelleysam offer both the download and pkg recipe for github.