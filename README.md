TEI-NTMSS Schema
----------------

These are the schema files that are used to validate our transcriptions. The schema currently comply with version
1.6 of the IGNTP transcription guidelines. The files included in the repository are:

* TEI-NTMSS.odd - this is the TEI *One Document Does it All* format. it is the file that must be uploaded into Roma if the schema ever need to be changed.
* TEI-NTMSS.rng - this is a RelaxNG schema in XML syntax which is the easiest way to validate transcriptions in Oxygen.
* document.xsd - this is a w3c schema formatted version of the TEI-NTMSS schema. It is what we use to validate the transcriptions in our Django system as LXML cannot load the rng schema (I don't know why). This document includes a reference to xsl.xsd for the xml namespace validation.
* xml.xsd - this is inlcuded in the document.xsd file and is used for validate the xml namespaced attributes such as xml:lang and xml:id. It is never used on its own.


These schema were originally generated in 2011 using the first version of Roma which is still available but now known as Roma-antiqua (https://romaantiqua.tei-c.org/).
This version of Roma is currently broken and will not export schema. As it has now been replaced I do not know if it will be fixed so for the November 2023 revision of the schema the new version of Roma was used. 

The new version of Roma (https://roma.tei-c.org/) does not currently support the addition of user created elements and attributes, TEI does, it is just not supported in the new version of Roma yet. We have a single attribute which has been added to the hi element, the optional height attribute, which we use to indicate the height of initial capitals. So that we can still update the schema in the new version of Roma the additional attribute has been removed from the ODD file. The schema needs to be edited in Roma without this attribute included, the schema generated and then the attribute added direclty back into each of the schema following the instructions below.


To make changes to the schema use load the TEI-NTMSS.odd using the 
'upload ODD' option on the Roma homepage at https://roma.tei-c.org/.  

You can then customise the schema.

When you have finished you must export three things using the 'Download' option in the top right corner. The text is exactly the option you should choose from the download menu, there are lots of very similar options.

* Customization as ODD
* RELAX NG schema
* W3C schema

It is vital that all three are kept in synch with each other so always download all three.

The W3C schema will be a zip file. This should be unziped and the two files inside (document.xsd and xml.xsd should be added directly to the schema repository)

Next you need to add the height attribute back into the schema.

Open the **TEI-NTMSS.rng** file in Oxygen or a plain text editor.

Find the section that looks like this:

```xml
   <define name="tei_hi">
      <element name="hi">
         <a:documentation xmlns:a="http://relaxng.org/ns/compatibility/annotations/1.0">(highlighted) marks a word or phrase as graphically distinct from the surrounding text, for reasons concerning which no claim is made. [3.3.2.2. Emphatic Words and Phrases 3.3.2. Emphasis, Foreign Words, and Unusual Language]</a:documentation>
         <ref name="tei_macro.paraContent"/>
         <ref name="tei_att.global.attributes"/>
         <ref name="tei_att.written.attributes"/>   
         <empty/>
      </element>
   </define>
```

and after ```<ref name="tei_att.written.attributes"/>``` add 

```xml
 <optional>
    <attribute name="height">
        <a:documentation xmlns:a="http://relaxng.org/ns/compatibility/annotations/1.0">the height in lines of a larger character</a:documentation>
        <text/>
    </attribute>
  </optional> 
```

so that the section looks like this

```xml
   <define name="tei_hi">
      <element name="hi">
         <a:documentation xmlns:a="http://relaxng.org/ns/compatibility/annotations/1.0">(highlighted) marks a word or phrase as graphically distinct from the surrounding text, for reasons concerning which no claim is made. [3.3.2.2. Emphatic Words and Phrases 3.3.2. Emphasis, Foreign Words, and Unusual Language]</a:documentation>
         <ref name="tei_macro.paraContent"/>
         <ref name="tei_att.global.attributes"/>
         <ref name="tei_att.written.attributes"/> 
         <optional>
          <attribute name="height">
              <a:documentation xmlns:a="http://relaxng.org/ns/compatibility/annotations/1.0">the height in lines of a larger character</a:documentation>
              <text/>
          </attribute>
        </optional>   
         <empty/>
      </element>
   </define>
```


Open the **document.xsd** file in Oxygen or a plain text editor.

Find the section that looks like this:
 
```xml 
  <xs:element name="hi">
    <xs:annotation>
      <xs:documentation>(highlighted) marks a word or phrase as graphically distinct from the surrounding text, for reasons concerning which no claim is made. [3.3.2.2. Emphatic Words and Phrases 3.3.2. Emphasis, Foreign Words, and Unusual Language]</xs:documentation>
    </xs:annotation>
    <xs:complexType>
      <xs:complexContent>
        <xs:extension base="tei:tei_macro.paraContent">
          <xs:attributeGroup ref="tei:tei_att.global.attributes"/>
          <xs:attributeGroup ref="tei:tei_att.written.attributes"/>          
        </xs:extension>
      </xs:complexContent>
    </xs:complexType>
  </xs:element>
```

and after ```<xs:attributeGroup ref="tei:tei_att.written.attributes"/>``` add 

```xml
  <xs:attribute name="height">
    <xs:annotation>
      <xs:documentation>the height in lines of a larger character</xs:documentation>
    </xs:annotation>
  </xs:attribute>
```

so that the section looks like this

```xml
  <xs:element name="hi">
    <xs:annotation>
      <xs:documentation>(highlighted) marks a word or phrase as graphically distinct from the surrounding text, for reasons concerning which no claim is made. [3.3.2.2. Emphatic Words and Phrases 3.3.2. Emphasis, Foreign Words, and Unusual Language]</xs:documentation>
    </xs:annotation>
    <xs:complexType>
      <xs:complexContent>
        <xs:extension base="tei:tei_macro.paraContent">
          <xs:attributeGroup ref="tei:tei_att.global.attributes"/>
          <xs:attributeGroup ref="tei:tei_att.written.attributes"/>
          <xs:attribute name="height">
            <xs:annotation>
              <xs:documentation>the height in lines of a larger character</xs:documentation>
            </xs:annotation>
          </xs:attribute>
        </xs:extension>
      </xs:complexContent>
    </xs:complexType>
  </xs:element>
```