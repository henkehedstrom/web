# How To: Create a Shortcut on the Start Menu

When installing applications it is a common requirement to place a shortcut on the user&apos;s Start Menu to provide a launching point for the program. This how to walks through how to create a shortcut on the start menu. It assumes you have a WiX source file based on the concepts described in [How To: Add a file to your installer](add_a_file.md).

## Step 1: Define the directory structure
Start Menu shortcuts are installed in a different directory than regular application files, so modifications to the installer&apos;s directory structure are required. The following WiX fragment should be placed inside a [&lt;Directory&gt;](../../xsd/wix/directory.md) element with the TARGETDIR ID and adds directory structure information for the Start Menu:

```
<font size="2" color="#0000FF">&lt;</font><font size="2" color="#A31515">Directory</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">ProgramMenuFolder</font><font size="2">"</font><font size="2" color="#0000FF">&gt;
    &lt;</font><font size="2" color="#A31515">Directory</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">ApplicationProgramsFolder</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Name</font><font size="2" color="#0000FF">=</font><font size="2">"My Application Name"</font><font size="2" color="#0000FF">/&gt;
&lt;/</font><font size="2" color="#A31515">Directory</font><font size="2" color="#0000FF">&gt;</font>
```

The <a href="http://msdn.microsoft.com/library/aa370882.aspx" target="_blank">ProgramMenuFolder</a> Id is a standard Windows Installer property that points to the Start Menu folder on the target machine. The second Directory element creates a subfolder on the Start Menu called My Application Name, and gives it an id for use later in the WiX project.

## Step 2: Add the shortcut to your installer package
A shortcut is added to the installer using three elements: a [&lt;Component&gt;](../../xsd/wix/component.md) element to specify an atomic unit of installation, a [&lt;Shortcut&gt;](../../xsd/wix/shortcut.md) element to specify the shortcut that should be installed, and a [&lt;RemoveFolder&gt;](../../xsd/wix/removefolder.md) element to ensure proper cleanup when your application is uninstalled.

The following sample uses the directory structure defined in Step 1 to create the Start Menu shortcut.

```
<font size="2" color="#0000FF">&lt;</font><font size="2" color="#A31515">DirectoryRef</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">ApplicationProgramsFolder</font><font size="2">"</font><font size="2" color="#0000FF">&gt;
    &lt;</font><font size="2" color="#A31515">Component</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">ApplicationShortcut</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Guid</font><font size="2" color="#0000FF">=</font><font size="2">"<a href="../../howtos/general/generate_guids">PUT-GUID-HERE</a>"</font><font size="2" color="#0000FF">&gt;
        &lt;</font><font size="2" color="#A31515">Shortcut</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">ApplicationStartMenuShortcut</font><font size="2">"</font><font size="2" color="#0000FF"> 
                  </font><font size="2" color="#FF0000">Name</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">My Application Name</font><font size="2">"</font><font size="2" color="#FF0000">
                  Description</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">My Application Description</font><font size="2">"
                  </font><font size="2" color="#FF0000">Target</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">[#myapplication.exe]"
                  </font><font size="2" color="#FF0000">WorkingDirectory</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">APPLICATIONROOTDIRECTORY</font><font size="2">"</font><font size="2" color="#0000FF">/&gt;
        &lt;</font><font size="2" color="#A31515">RemoveFolder</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">CleanUpShortCut</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Directory</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">ApplicationProgramsFolder</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">On</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">uninstall</font><font size="2">"</font><font size="2" color="#0000FF">/&gt;
        &lt;</font><font size="2" color="#A31515">RegistryValue</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Root</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">HKCU</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Key</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">Software\MyCompany\MyApplicationName</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Name</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">installed</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Type</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">integer</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Value</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">1</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">KeyPath</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">yes</font><font size="2">"</font><font size="2" color="#0000FF">/&gt;
    &lt;/</font><font size="2" color="#A31515">Component</font><font size="2" color="#0000FF">&gt;
&lt;/</font><font size="2" color="#A31515">DirectoryRef</font><font size="2" color="#0000FF">&gt;</font>
```

The [&lt;DirectoryRef&gt;](../../xsd/wix/directoryref.md) element is used to refer to the directory structure created in step 1. By referencing the ApplicationProgramsFolder directory the shortcut will be installed into the user&apos;s Start Menu inside the My Application Name folder.

Underneath the DirectoryRef is a single Component to group the elements used to install the Shortcut. The first element is Shortcut and it creates the actual shortcut in the Start Menu. The Id attribute is a unique id for the shortcut. The Name attribute is the text that will be displayed in the Start Menu. The description is an optional attribute for an additional application description. The Target attribute points to the executable to launch on disk. Notice how it references the full path using the `[#FileId]` syntax where [`myapplication.exe` was previously defined](add_a_file.md). The WorkingDirectory attribute sets the working directory for the shortcut.

To set an optional icon for the shortcut you need to first include the icon in your installer using the [&lt;Icon&gt;](../../xsd/wix/icon.md) element, then reference it using the Icon attribute on the Shortcut element.

In addition to creating the shortcut the component contains two other important pieces. The first is a RemoveFolder element, which ensures the ApplicationProgramsFolder is correctly removed from the Start Menu when the user uninstalls the application. The second creates a registry entry on install that indicates the application is installed. This is required as a Shortcut cannot serve as the KeyPath for a component when installing non-advertised shortcuts for the current users. For more information on creating registry entries see [How To: Write a registry entry during installation](write_a_registry_entry.md).

## Step 3: Tell Windows Installer to install the shortcut
After defining the directory structure and listing the shortcuts to package into the installer, the last step is to tell Windows Installer to actually install the shortcut. The [&lt;Feature&gt;](../../xsd/wix/feature.md) element is used to do this. The following snippet adds a reference to the shortcut component, and should be inserted inside a parent Feature element.

```
<font size="2" color="#A31515">&lt;ComponentRef</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"ApplicationShortcut</font><font size="2">"</font><font size="2" color="#0000FF"> /&gt;</font>
```

The [&lt;ComponentRef&gt;](../../xsd/wix/componentref.md) element is used to reference the component created in Step 2 via the Id attribute.

## The Complete Sample
The following is a complete sample that uses the above concepts. This example can be inserted into a WiX project and compiled, or compiled and linked from the command line, to generate an installer.

```
<font size="2" color="#0000FF">&lt;?</font><font size="2" color="#A31515">xml</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">version</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">1.0</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">encoding</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">UTF-8</font><font size="2">"</font><font size="2" color="#0000FF">?&gt;
&lt;<font size="2" color="#A31515">Wix</font> <font size="2" color="#FF0000">xmlns</font>=<font size="2">"</font>http://schemas.microsoft.com/wix/2006/wi<font size="2">"</font>&gt;
    &lt;<font size="2" color="#A31515">Product</font> <font size="2" color="#FF0000">Id</font>=<font size="2">"*"</font> <font size="2" color="#FF0000">UpgradeCode</font>=<font size="2">"</font></font><font size="2"><a href="../../howtos/general/generate_guids">PUT-GUID-HERE</a></font><font size="2" color="#0000FF"><font size="2">"</font> <font size="2" color="#FF0000">Version</font>=<font size="2">"1.0.0.0" </font><font size="2" color="#FF0000">Language</font>=<font size="2">"</font>1033<font size="2">" </font><font size="2" color="#FF0000">Name</font>=<font size="2">"My Application Name" </font><font size="2" color="#FF0000">Manufacturer</font>=<font size="2">"My Manufacturer Name"</font>&gt;
        &lt;<font size="2" color="#A31515">Package</font> <font size="2" color="#FF0000">InstallerVersion</font>=<font size="2">"</font>300<font size="2">"</font> <font size="2" color="#FF0000">Compressed</font>=<font size="2">"</font>yes<font size="2">"</font>/&gt;
        &lt;<font size="2" color="#A31515">Media</font> <font size="2" color="#FF0000">Id</font>=<font size="2">"</font>1<font size="2">"</font> <font size="2" color="#FF0000">Cabinet</font>=<font size="2">"myapplication</font>.cab<font size="2">"</font> <font size="2" color="#FF0000">EmbedCab</font>=<font size="2">"</font>yes<font size="2">"</font> /&gt;

        &lt;</font><font size="2" color="#A31515">Directory</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">TARGETDIR</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Name</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">SourceDir</font><font size="2">"</font><font size="2" color="#0000FF">&gt;
            &lt;</font><font size="2" color="#A31515">Directory</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">ProgramFilesFolder</font><font size="2">"</font><font size="2" color="#0000FF">&gt;
                &lt;</font><font size="2" color="#A31515">Directory</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">APPLICATIONROOTDIRECTORY</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Name</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">My Application Name</font><font size="2">"</font><font size="2" color="#0000FF">/&gt;
            &lt;/</font><font size="2" color="#A31515">Directory</font><font size="2" color="#0000FF">&gt;
            &lt;!--</font><font size="2" color="#008000"> Step 1: Define the directory structure </font><font size="2" color="#0000FF">--&gt;
            &lt;</font><font size="2" color="#A31515">Directory</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">ProgramMenuFolder</font><font size="2">"</font><font size="2" color="#0000FF">&gt;
                &lt;</font><font size="2" color="#A31515">Directory</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">ApplicationProgramsFolder</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Name</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">My Application Name</font><font size="2">"</font><font size="2" color="#0000FF">/&gt;
            &lt;/</font><font size="2" color="#A31515">Directory</font><font size="2" color="#0000FF">&gt;
        &lt;/</font><font size="2" color="#A31515">Directory</font><font size="2" color="#0000FF">&gt;

        &lt;</font><font size="2" color="#A31515">DirectoryRef</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">APPLICATIONROOTDIRECTORY</font><font size="2">"</font><font size="2" color="#0000FF">&gt;
            &lt;</font><font size="2" color="#A31515">Component</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">myapplication.exe</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Guid</font><font size="2" color="#0000FF">=</font><font size="2">"<a href="../../howtos/general/generate_guids">PUT-GUID-HERE</a>"</font><font size="2" color="#0000FF">&gt;
                &lt;</font><font size="2" color="#A31515">File</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">myapplication.exe</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Source</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">MySourceFiles\MyApplication.exe</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">KeyPath</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">yes</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Checksum</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">yes</font><font size="2">"</font><font size="2" color="#0000FF">/&gt;
            &lt;/<font size="2" color="#A31515">Component</font>&gt;
            &lt;<font size="2" color="#A31515">Component</font> <font size="2" color="#FF0000">Id</font>=<font size="2">"d</font>ocumentation.<font size="2">html"</font> <font size="2" color="#FF0000">Guid</font>=<font size="2">"</font></font><font size="2"><a href="../../howtos/general/generate_guids">PUT-GUID-HERE</a></font><font size="2" color="#0000FF"><font size="2">"</font>&gt;
                &lt;</font><font size="2" color="#A31515">File</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">documentation.html</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Source</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">MySourceFiles\documentation.html</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">KeyPath</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">yes</font><font size="2">"</font><font size="2" color="#0000FF">/&gt;
            &lt;/<font size="2" color="#A31515">Component</font>&gt;
        &lt;/</font><font size="2" color="#A31515">DirectoryRef</font><font size="2" color="#0000FF">&gt;

        &lt;!--</font><font size="2" color="#008000"> Step 2: Add the shortcut to your installer package </font><font size="2" color="#0000FF">--&gt;
        &lt;</font><font size="2" color="#A31515">DirectoryRef</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">ApplicationProgramsFolder</font><font size="2">"</font><font size="2" color="#0000FF">&gt;
            &lt;</font><font size="2" color="#A31515">Component</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">ApplicationShortcut</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Guid</font><font size="2" color="#0000FF">=</font><font size="2">"<a href="../../howtos/general/generate_guids">PUT-GUID-HERE</a>"</font><font size="2" color="#0000FF">&gt;
                &lt;</font><font size="2" color="#A31515">Shortcut</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">ApplicationStartMenuShortcut</font><font size="2">"</font><font size="2" color="#0000FF"> <br />                     </font><font size="2" color="#FF0000">Name</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">My Application Name</font><font size="2">"</font><font size="2" color="#0000FF"> <br />                   </font><font size="2" color="#FF0000">Description</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">My Application Description</font><font size="2">"<br />                    </font><font size="2" color="#FF0000">Target</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">[#myapplication.exe]</font><font size="2">"
                          </font><font size="2" color="#FF0000">WorkingDirectory</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">APPLICATIONROOTDIRECTORY</font><font size="2">"</font><font size="2" color="#0000FF">/&gt;
                &lt;</font><font size="2" color="#A31515">RemoveFolder</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">ApplicationProgramsFolder</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">On</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">uninstall</font><font size="2">"</font><font size="2" color="#0000FF">/&gt;
                &lt;</font><font size="2" color="#A31515">RegistryValue</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Root</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">HKCU</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Key</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">Software\MyCompany\MyApplicationName</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Name</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">installed</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Type</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">integer</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Value</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">1</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">KeyPath</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">yes</font><font size="2">"</font><font size="2" color="#0000FF">/&gt;<br />           &lt;/</font><font size="2" color="#A31515">Component</font><font size="2" color="#0000FF">&gt;
        &lt;/</font><font size="2" color="#A31515">DirectoryRef</font><font size="2" color="#0000FF">&gt;

        &lt;</font><font size="2" color="#A31515">Feature</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">MainApplication</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Title</font><font size="2" color="#0000FF">=</font><font size="2">"Main Application"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Level</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">1</font><font size="2">"</font><font size="2" color="#0000FF">&gt;
            &lt;</font><font size="2" color="#A31515">ComponentRef</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">myapplication.exe</font><font size="2">"</font><font size="2" color="#0000FF"> /&gt;
            &lt;</font><font size="2" color="#A31515">ComponentRef</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">documentation.html</font><font size="2">"</font><font size="2" color="#0000FF"> /&gt;
            &lt;!--</font><font size="2" color="#008000"> Step 3: Tell WiX to install the shortcut </font><font size="2" color="#0000FF">--&gt;
            &lt;</font><font size="2" color="#A31515">ComponentRef</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">ApplicationShortcut</font><font size="2">"</font><font size="2" color="#0000FF"> /&gt;   
        &lt;/</font><font size="2" color="#A31515">Feature</font><font size="2" color="#0000FF">&gt;
    &lt;/</font><font size="2" color="#A31515">Product</font><font size="2" color="#0000FF">&gt;
&lt;/</font><font size="2" color="#A31515">Wix</font><font size="2" color="#0000FF">&gt;</font>
```