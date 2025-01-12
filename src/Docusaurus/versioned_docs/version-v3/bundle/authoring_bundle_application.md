---
title: Author Bootstrapper Application for a Bundle
layout: documentation
after: authoring_bundle_skeleton
---
# Author Bootstrapper Application for a Bundle

Every bundle requires a bootstrapper application to drive the Burn engine. The [&lt;BootstrapperApplication&gt;](../xsd/wix/bootstrapperapplication.md) element is used to define a new bootstrapper application. The [&lt;BootstrapperApplicationRef&gt;](../xsd/wix/bootstrapperapplicationref.md) element is used to refer to a bootstrapper application that exists in a [&lt;Fragment&gt;](../xsd/wix/fragment.md) or WiX extension.

The [WiX Standard Bootstrapper Application](wixstdba/index.md) exists in the WixBalExtension.dll. The following shows how to use it in a bundle:

```
    &lt;?xml version=&quot;1.0&quot;?&gt;
    &lt;Wix xmlns=&quot;http://schemas.microsoft.com/wix/2006/wi&quot;&gt;
      &lt;Bundle&gt;
<strong class="highlight">        &lt;BootstrapperApplicationRef Id=&quot;WixStandardBootstrapperApplication.RtfLicense&quot; /&gt;</strong>
        &lt;Chain&gt;
        &lt;/Chain&gt;
      &lt;/Bundle&gt;
    &lt;/Wix&gt;
```

The WiX Standard Bootstrapper Application may not provide all functionality a specialized bundle requires so a custom bootstrapper application DLL may be developed. The following example creates a bootstrapper application using a fictional &quot;ba.dll&quot;:

```
    &lt;?xml version=&quot;1.0&quot;?&gt;
    &lt;Wix xmlns=&quot;http://schemas.microsoft.com/wix/2006/wi&quot;&gt;
      &lt;Bundle&gt;
<strong class="highlight">        &lt;BootstrapperApplication SourceFile=&quot;path\to\ba.dll&quot; /&gt;</strong>
        &lt;Chain&gt;
        &lt;/Chain&gt;
      &lt;/Bundle&gt;
    &lt;/Wix&gt;
```

Inside the [&lt;BootstrapperApplication&gt;](../xsd/wix/bootstrapperapplication.md) element and [&lt;BootstrapperApplicationRef&gt;](../xsd/wix/bootstrapperapplicationref.md) element, you may add additional payload files such as resources files that are required by the bootstrapper application DLL as follows:

```
    &lt;?xml version=&quot;1.0&quot;?&gt;
    &lt;Wix xmlns=&quot;http://schemas.microsoft.com/wix/2006/wi&quot;&gt;
      &lt;Bundle&gt;
        &lt;BootstrapperApplication SourceFile=&quot;path\to\ba.dll&quot;&gt;
<strong class="highlight">          &lt;Payload SourceFile=&quot;path\to\en-us\resources.dll&quot; /&gt;
          &lt;PayloadGroupRef Id=&quot;ResourceGroupforJapanese&quot; /&gt;</strong>
        &lt;/BootstrapperApplication&gt;
        &lt;Chain&gt;
        &lt;/Chain&gt;
      &lt;/Bundle&gt;
    &lt;/Wix&gt;
```

This example references a payload file that is on the local machine named resources.dll, as well as a group of payload files that are defined in a [&lt;PayloadGroup&gt;](../xsd/wix/payloadgroup.md) element inside a [&lt;Fragment&gt;](../xsd/wix/fragment.md) elsewhere.

The next step is to [add installation packages to the chain](authoring_bundle_package_manifest.md).
