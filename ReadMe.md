# PDF Page Break Plugin for DITA Open Toolkit

This plugin is based on a blog post by Radu Coravu of Syncro Soft, the makers of [oXygen XML Editor][1].

For more information, see <http://blog.oxygenxml.com/2015/04/dita-pdf-publishing-force-page-breaks.html>.

The instructions from the original post are included here for convenience:

---  

## DITA PDF publishing - Force page breaks between two block elements

[blog.oxygenxml.com](http://blog.oxygenxml.com/2015/04/dita-pdf-publishing-force-page-breaks.html) * by Radu Coravu * April 29, 2015 

Let's say that at some point in your DITA content you have two block level elements, like for example two paragraphs:

```xml
<p>First para</p>
<p>Second para</p>
```

and you want to force in the PDF output a page break between them.

Here's how a DITA Open Toolkit plugin which would achieve this could be implemented:

 1. You define your custom processing instruction which marks the place where a page break should be inserted in the PDF, for example:
    
    ```xml
    <p>First para</p>
    <?pagebreak?>
    <p>Second para</p>
    ```

 2. In the **DITA Open Toolkit** distribution in the plugins directory you create a new plugin folder named for example `pdf-page-break`.

 3. In this new folder create a new `plugin.xml` file with the content:
    
    ```xml
    <plugin id="com.yourpackage.pagebreak">
      <feature extension="package.support.name" value="Force Page Break Plugin"/>
      <feature extension="package.support.email" value="support@youremail.com"/>
      <feature extension="package.version" value="1.0.0"/>
      <feature extension="dita.xsl.xslfo" value="pageBreak.xsl" type="file"/>
    </plugin>
    ```

    The most important feature in the plugin is that it will add a new XSLT stylesheet to the XSL processing which produces the PDF content.

 4. Create in the same folder an **XSLT** stylesheet named `pageBreak.xsl` with the content:
    
    ```xml
    <xsl:stylesheet xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
        xmlns:fo="http://www.w3.org/1999/XSL/Format"
        version="1.0">
      <xsl:template match="processing-instruction('pagebreak')">
        <fo:block break-after="page"/>
      </xsl:template>
    </xsl:stylesheet>
    ```

 5. Install your plugin in the **DITA Open Toolkit** by running the DITA-OT Ant integrator task. 

    If you are running the publishing from **Oxygen XML Editor** you can use the predefined transformation scenario: [http://www.oxygenxml.com/doc/ug-oxygen/#topics/dita-ot-install-plugin.html](http://www.oxygenxml.com/doc/ug-oxygen/#topics/dita-ot-install-plugin.html). 

    If you run DITA OT from the command line please follow these guidelines:  
    [http://www.dita-ot.org/2.0/dev_ref/plugins-installing.html](http://www.dita-ot.org/2.0/dev_ref/plugins-installing.html).


---

[1]:    [http://www.oxygenxml.com/]
