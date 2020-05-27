Primer — An Introduction to CSL
===============================

by `Rintze M. Zelle, PhD <https://twitter.com/rintzezelle>`_

|CCBYSA|_

.. |CCBYSA| image:: /media/cc-by-sa-80x15.png
.. _CCBYSA: http://creativecommons.org/licenses/by-sa/3.0/

.. contents:: **Table of Contents**

Preface
~~~~~~~

This primer is an introduction to the `Citation Style Language`_ (CSL), an open XML-based language to describe the formatting of citations and bibliographies. For a more technical and in-depth description of CSL, see the `CSL Specification`_.

.. _Citation Style Language: http://citationstyles.org
.. _CSL Specification: specification.html

What is CSL?
~~~~~~~~~~~~

If you ever wrote a research paper, you probably included references to other works. Referencing is important in scholarly communication, as it provides attribution, and links together published research. However, manually formatting references can become very time-consuming, especially when you're dealing with multiple journals that each have their own citation style.

Luckily, reference management software can help out. Programs like Zotero, Mendeley, and Papers not only help you manage your research library, but can also automatically generate citations and bibliographies. But to format references in the desired style, these programs need descriptions of each citation style in a language the computer can understand. As you might have guessed, the Citation Style Language (CSL) is such a language.

Citation Formats
~~~~~~~~~~~~~~~~

There are hundreds, if not thousands of different citation styles in use around the world. Fortunately, most citation styles fall within a few basic categories. CSL distinguishes between the following types:

In-text Styles
^^^^^^^^^^^^^^

Citation styles can be divided in two main categories. The first category consists of **in-text** styles, where a *citation* in the sentence directly points to one or multiple entries in the *bibliography*. CSL subdivides this category in **author-date**, **author**, **numeric** and **label** styles.

Each citation points to one or more *bibliographic entries*. In CSL, each individual pointer is called a *cite*. For example, the citation "(Doe et al. 2002, Smith 1997)" contains two cites: one to a 2002 publication by Doe et al., and one to a 1997 publication by Smith. In the context of CSL, bibliographic entries are sometimes also called *references*.

"author-date" & "author" Styles
'''''''''''''''''''''''''''''''

Cites of **author-date** styles show author names and the date of publication, e.g. "(Van der Klei et al. 1991; Zwart et al. 1983)", whereas cites of **author** styles only show names, e.g. "(Gidijala et al.)". Bibliographic entries are typically sorted alphabetically by author.

Note that many style guides use the confusing term "Harvard" to refer to **author-date** formats, even though most of these styles have no connection to Harvard University. There is also no such thing as a single official "Harvard" style.

**Example Bibliography**

Gidijala L, Bovenberg RA, Klaassen P, van der Klei IJ, Veenhuis M, et al. (2008) Production of functionally active *Penicillium chrysogenum* isopenicillin N synthase in the yeast *Hansenula polymorpha*. BMC Biotechnol 8: 29.

van der Klei IJ, Harder W, Veenhuis M (1991) Methanol metabolism in a peroxisome-deficient mutant of *Hansenula polymorpha*: a physiological study. Arch Microbiol 156: 15-23.

Zwart KB, Veenhuis M, Harder W (1983) Significance of yeast peroxisomes in the metabolism of choline and ethanolamine. Antonie van Leeuwenhoek 49: 369-385.

"numeric" Styles
''''''''''''''''

Cites of **numeric** styles consist of numbers, e.g. "[1, 2]" and "[3]". Bibliographic entries are typically sorted either alphabetically by author, or put in the order by which they have been first cited.

**Example Bibliography**

1. Gidijala L, Bovenberg RA, Klaassen P, van der Klei IJ, Veenhuis M, et al. (2008) Production of functionally active *Penicillium chrysogenum* isopenicillin N synthase in the yeast *Hansenula polymorpha*. BMC Biotechnol 8: 29.

2. Zwart KB, Veenhuis M, Harder W (1983) Significance of yeast peroxisomes in the metabolism of choline and ethanolamine. Antonie van Leeuwenhoek 49: 369-385.

3. van der Klei IJ, Harder W, Veenhuis M (1991) Methanol metabolism in a peroxisome-deficient mutant of *Hansenula polymorpha*: a physiological study. Arch Microbiol 156: 15-23.

"numeric" Compound Styles
'''''''''''''''''''''''''

Compound styles are a variation of the **numeric** in-text style format. With these styles, popular in the field of chemistry, bibliographic entries may contain multiple references. Once a citation has defined such a bibliographic entry (e.g, "[2]"), it becomes possible to cite individual items within the entry (e.g., "[2b]"). This format is not yet supported by CSL.

**Example Bibliography**

1. Gidijala L, et al. (2008) BMC Biotechnol 8: 29.

2. \a) Zwart KB, et al. (1983) Antonie van Leeuwenhoek 49: 369-385, b) van der Klei IJ, et al. (1991) Arch Microbiol 156: 15-23.

"label" Styles
''''''''''''''

Cites of **label** styles consist of short keys, e.g. "[GBKv2008]" and "[ZwVH1983; vaHV1991]". These keys are also included in the bibliographic entries. CSL has limited support for this format, since it currently doesn't allow for (style-specific) customisation of the key format.

**Example Bibliography**

[GBKv2008] Gidijala L, Bovenberg RA, Klaassen P, van der Klei IJ, Veenhuis M, et al. (2008) Production of functionally active *Penicillium chrysogenum* isopenicillin N synthase in the yeast *Hansenula polymorpha*. BMC Biotechnol 8: 29.

[vaHV1991] van der Klei IJ, Harder W, Veenhuis M (1991) Methanol metabolism in a peroxisome-deficient mutant of *Hansenula polymorpha*: a physiological study. Arch Microbiol 156: 15-23.

[ZwVH1983] Zwart KB, Veenhuis M, Harder W (1983) Significance of yeast peroxisomes in the metabolism of choline and ethanolamine. Antonie van Leeuwenhoek 49: 369-385.

Note Styles
^^^^^^^^^^^

The second category of citation styles consists of **note** styles. Here a *marker*, which can be a number or a symbol, is added to the sentence when works are cited, e.g. "[*]_" and "[*]_". Each marker points to a footnote or endnote. CSL styles do not control which number formats or symbols are used for the markers, which is left to the word processor. In contrast to **in-text** citations, footnotes and endnotes typically contain all information required to identify the cited works. Some **note** styles include a bibliography to give an overview of all cited works, and to describe the works in more detail.

    .. [*] 'Voyage to St. Kilda' (3rd edit. 1753), p. 37.
    .. [*] Sir J. E. Tennent, 'Ceylon,' vol. ii. 1859, p. 107.

The CSL Ecosystem
~~~~~~~~~~~~~~~~~

To understand how CSL works, let's start by taking a look at the various bits and pieces of the CSL ecosystem:

|csl-infrastructure|

.. |csl-infrastructure| image:: /media/csl-infrastructure.png
   :width: 257pt

Independent and Dependent Styles
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Styles! Everything in the world of CSL revolves around styles. But not all CSL styles are alike. There are two types: **independent styles** and **dependent styles**.

An **independent CSL style** has two functions: first, it needs to define a citation format. What does the format look like? Is it an "author-date" style, or a "note" style? Are cites ordered alphabetically, or by date? Should bibliographic entries include DOIs? What punctuation and capitalization should be used? Does the year of publication come before or after the title? Etcetera, etcetera. Secondly, the CSL style must describe itself. We call this self-describing information **style metadata**, and it can include the title of the journal for which the CSL style was created, a link to that journal's website, the name of the creator of the CSL style, etc.

A **dependent CSL style**, on the other hand, only contains **style metadata**. Instead of providing a definition of a citation format, a dependent style simply refers to an independent CSL style (its "parent"), whose citation format will be used instead.

Dependent styles come in handy when multiple CSL styles share the same citation format. Take a publisher which uses a single citation format for all its journals. If we were limited to using independent CSL styles, every journal's CSL style would need to contain a full definition of the citation format, even though it would be the same for each journal. This would produce a collection of bulky styles that are hard to maintain. If the publisher makes a change to its citation format, we would have to update every single independent CSL style.

Dependent styles solve these problems. For example, the journals "Nature Biotechnology", "Nature Chemistry", and "Nature" all use the same citation format. We therefore created dependent CSL styles for "Nature Biotechnology" and "Nature Chemistry" that both point to our independent CSL style for "Nature". Since they don't define a citation format, dependent styles are a fraction of the size of an independent style. And, if the Nature Publishing Group ever decides to change the "Nature" citation format across its journals, we only have to correct the citation format in the "Nature" CSL style, without having to touch any of its dependents.

Locale Files
^^^^^^^^^^^^

I have a little secret to share with you: most independent styles aren't fully independent.

Take the reference below:

    Hartman, P., Bezos, J. P., Kaphan, S., & Spiegel, J. (1999, September 28). Method and system for placing a purchase order via a communications network. Retrieved from https://www.google.com/patents/US5960411

You can describe this citation format in an independent CSL style by hard-coding all language-specific information into the style. For example, you can put the text "Retrieved from" before the URL, and use "YYYY, Month DD" as the date format. However, such a style would only be usable in US English. If you later need a German variant of this citation format, you would have to change all the translations and date formats within the style.

Fortunately, independent CSL styles can rely on the CSL **locale files** for translations of common terms, localized date formats, and grammar. For example, we can rewrite our CSL style to use the "retrieved" and "from" CSL terms, and to use the localized "text" date format. If we then set the locale of the style to US English, this style will retrieve the term translations and localized date format from the US English CSL locale file, which will produce the reference as written above. But if we switch the style locale to German, the German locale file will be used instead, producing:

    Hartman, P., Bezos, J. P., Kaphan, S., & Spiegel, J. (28. September 1999). Method and system for placing a purchase order via a communications network. Abgerufen von https://www.google.com/patents/US5960411

So with CSL locale files, it becomes possible to write CSL styles that are largely language-agnostic. As illustrated above, such styles can easily switch between different languages. However, languages are complex, and CSL's automatic localization doesn't support the peculiarities of all languages for which we have locale files. But even if you find that you need to modify a CSL style to adapt it to your language of preference, language-agnostic styles have value, since they're easier to translate.

Locale files have the added benefit that we only need to define common translations, date formats, and grammar once per language. This keeps styles compact, and makes locale data easier to maintain. Since citation formats for a given language don't always agree on a translation or date format, CSL styles can selectively overwrite any locale data that is defined in the locale files.

Item Metadata
^^^^^^^^^^^^^

Next up are the bibliographic details of the items you wish to cite: the **item metadata**.

For example, the bibliographic entry for a journal article may show the names of the authors, the year in which the article was published, the article title, the journal title, the volume and issue in which the article appeared, the page numbers of the article, and the article's Digital Object Identifier (DOI). All these details help the reader identify and find the referenced work.

Reference managers make it easy to create a library of items. While many reference managers have their own way of storing item metadata, most support common bibliographic exchange formats such as BibTeX and RIS. The citeproc-js CSL processor introduced a JSON-based format for storing item metadata in a way citeproc-js could understand. Several other CSL processors have since adopted this "CSL JSON" format (also known as "citeproc JSON").

Citing Details
^^^^^^^^^^^^^^

For a given citation format, the way citations and bibliographies look not only depends on the metadata of the cited items, but also on the context in which these items are cited. We refer to this type of context-specific information as the **citing details**.

For instance, the order in which items are cited in a document can affect their order in the bibliography. And in "note" styles, subsequent cites to a previously cited item are often written in a more compact form. Another example is the use of locators, which guide the reader to a specific location within a cited work, such as the page numbers within a chapter where a certain argument is made, e.g. "(Doe 2000, pp. 43-44)".

CSL Processors
^^^^^^^^^^^^^^

With CSL styles, locale files, item metadata and citing details in hand, we now need a piece of software to parse all this information, and generate citations and bibliographies in the correct format: the **CSL processor**.

Most reference managers use one of the freely available open source CSL processors, such as citeproc-js.

Understanding CSL Styles
~~~~~~~~~~~~~~~~~~~~~~~~

By now you've learned what CSL is, how it can be used, and how its different parts and pieces fit together. We're now ready to dive into the CSL styles themselves, and look at their XML code.

XML Basics
^^^^^^^^^^

If you're new to XML, this section gives a short overview of what you need to know about XML in order to read and edit CSL styles and locale files. For more background, just check one of the many XML tutorials online.

Let's take a look at the following dependent CSL style:

.. sourcecode:: xml

    <?xml version="1.0" encoding="utf-8"?>
    <style xmlns="http://purl.org/net/xbiblio/csl" version="1.0" default-locale="en-US">
      <!-- Generated with https://github.com/citation-style-language/utilities/tree/master/generate_dependent_styles/data/asm -->
      <info>
        <title>Applied and Environmental Microbiology</title>
        <id>http://www.zotero.org/styles/applied-and-environmental-microbiology</id>
        <link href="http://www.zotero.org/styles/applied-and-environmental-microbiology" rel="self"/>
        <link href="http://www.zotero.org/styles/american-society-for-microbiology" rel="independent-parent"/>
        <link href="http://aem.asm.org/" rel="documentation"/>
        <category citation-format="numeric"/>
        <category field="biology"/>
        <issn>0099-2240</issn>
        <eissn>1098-5336</eissn>
        <updated>2014-04-30T03:45:36+00:00</updated>
        <rights license="http://creativecommons.org/licenses/by-sa/3.0/">This work is licensed under a Creative Commons Attribution-ShareAlike 3.0 License</rights>
      </info>
    </style>

There are several concepts and terms you need to be familiar with. These are:

- **XML Declaration**. The first line of each style and locale file is usually the XML declaration. In most cases, this will be ``<?xml version="1.0" encoding="utf-8"?>``. This declaration makes it clear that the document consists of XML, and specifies the XML version ("1.0") and character encoding ("utf-8") used.

- **Elements and Hierarchy**. Elements are the basic building blocks of XML documents. Each XML document contains a single root element (for CSL styles this is ``<style/>``). If an element contains other elements, this parent element is split into a start tag (``<style>``) and an end tag (``</style>``). In our example, the ``<style/>`` element has one child element, ``<info/>``. This element has several children of its own, which are grandchildren of the grandparent ``<style/>`` element.

  Element tags are always wrapped in less-than ("<") and greater-than (">") characters (e.g., ``<style>``). For empty-element tags, ">" is preceded by a forward-slash (e.g., ``<category/>``), while for end tags, "<" is followed by a forward-slash (e.g., ``</style>``). Child elements are typically indented with spaces or tabs to show the different hierarchical levels. We use 2 spaces per level in our CSL styles and locale files.

  In the rest of this primer we will use the prefix "cs:" when referring to CSL elements (e.g., ``cs:style`` instead of ``<style/>``).

- **Attributes and Element Content**. There are two ways to add additional information to elements.

  First, XML elements can carry one or more attributes. The order of attributes on an element is arbitrary, but every attribute needs a value. For example, the ``<style/>`` element carries the ``version`` attribute, set to a value of "1.0" (this indicates that the style is compatible with the latest CSL 1.0.x release).

  Secondly, elements can store non-element content between their start and end tags. For example, the title of the style, "Applied and Environmental Microbiology", is stored as the content of the ``<title/>`` element.

- **Escaping**. To avoid ambiguity in defining the structure of XML files, some characters need to be substituted when used for other purposes, e.g. when used in attribute values or element content. The escape sequences are:

  * ``&lt;`` for ``<``
  * ``&gt;`` for ``>``
  * ``&amp;`` for ``&``
  * ``&apos;`` for ``'``
  * ``&quot;`` for ``"``

  For example, the link ``http://domain.com/?tag=a&id=4`` is escaped as ``<link href="http://domain.com/?tag=a&amp;id=4"/>``.

- **XML Comments**. You can use XML comments to add clarifying information to a XML file. Comments start with ``<!--`` and end with ``-->``, and are ignored by the CSL processor.

- **Well-formedness and Schema Validity**. Unlike HTML, XML is unforgiving when it comes to markup errors. Any error, like forgetting an end tag, having more than one root element, or incorrect escaping will break the entire XML document, and prevent it from being processed.

  To make sure that a CSL style works correctly, it must follow the XML specification. An error-free XML file is called "well-formed". But to be considered "valid" CSL, a well-formed CSL style must also follow the rules specified by the CSL schema. This schema describes all the various CSL elements and attributes, and how they must be used.

  You can use a CSL validator to check a CSL style for any errors. Remember that only well-formed and valid CSL files can be expected to work properly.

Anatomy of a Dependent Style
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

As explained above, dependent CSL styles are much more compact that their independent counterparts, since they don't actually have to define a citation format. Dependent styles are also very common, and their style metadata is similar to that of independent styles, so they are a good starting point for learning CSL. Let's take a closer look at the dependent style above, line by line.

.. sourcecode:: xml

    <?xml version="1.0" encoding="utf-8"?>

The XML declaration.

.. sourcecode:: xml

    <style xmlns="http://purl.org/net/xbiblio/csl" version="1.0" default-locale="en-US">
        ...
    </style>

The start and end tags of the ``cs:style`` root element. Its ``xmlns`` attribute specifies that all elements in the style are part of CSL, while the ``version`` attribute indicates CSL version compatibility. The ``default-locale`` attribute tells the style to generate citations and bibliographies in a certain language (in this case US English).

.. sourcecode:: xml

      <!-- Generated with https://github.com/citation-style-language/utilities/tree/master/generate_dependent_styles/data/asm -->

Most of our dependent styles are automatically generated from spreadsheet data. This XML comment makes it clear that this style has been generated, and contains a link to the spreadsheet.

.. sourcecode:: xml

      <info>
        ...
      </info>

The ``cs:info`` section is used to store most of the style's metadata.

.. sourcecode:: xml

    <title>Applied and Environmental Microbiology</title>

The title of the style.

.. sourcecode:: xml

    <id>http://www.zotero.org/styles/applied-and-environmental-microbiology</id>

The style ID, which is used by reference managers to identify styles and tell them apart.

.. sourcecode:: xml

    <link href="http://www.zotero.org/styles/applied-and-environmental-microbiology" rel="self"/>

The style's "self" link. This URL links to an online copy of the style. For simplicity, we use the same URL as style ID and "self" link for our repository styles.

.. sourcecode:: xml

    <link href="http://www.zotero.org/styles/american-society-for-microbiology" rel="independent-parent"/>

Dependent styles need to link to an independent parent style, whose citation format will be used. Here we use the citation format from the CSL style for the American Society for Microbiology.

.. sourcecode:: xml

    <link href="http://aem.asm.org/" rel="documentation"/>

It's much easier to maintain our collection of CSL styles if each style's purpose is clear. We therefore require that all our repository styles contain at least one "documentation" link. In this case, to the journal's home page.

.. sourcecode:: xml

    <category citation-format="numeric"/>
    <category field="biology"/>

To help cataloguing our styles, we specify the citation format with the ``citation-format`` attribute on ``cs:category``. Similarly, we assign each style to one or more fields of study, using the ``field`` attribute.

.. sourcecode:: xml

    <issn>0099-2240</issn>
    <eissn>1098-5336</eissn>

When a CSL styles is created for a journal, we store the journal's print ISSN and electronic ISSN in the ``cs:issn`` and ``cs:eissn`` elements, respectively.

.. sourcecode:: xml

    <updated>2014-04-30T03:45:36+00:00</updated>

A time stamp to indicate when the style was last updated.

.. sourcecode:: xml

    <rights license="http://creativecommons.org/licenses/by-sa/3.0/">This work is licensed under a Creative Commons Attribution-ShareAlike 3.0 License</rights>

Last, but certainly not least, the license under which the style is released.

Anatomy of an Independent Style
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Finally, a real independent CSL style, one that actually defines a citation format! Well, okay, maybe it's not exactly a realistic style. Most independent styles in our repository are quite a bit bigger than the simplified example style below. But our "author-date" style below is valid CSL, and still has the same overall design as any other independent style.

.. sourcecode:: xml

    <?xml version="1.0" encoding="utf-8"?>
    <style xmlns="http://purl.org/net/xbiblio/csl" class="in-text" version="1.0">
      <info>
        <title>Example Style</title>
        <id>http://www.zotero.org/styles/example</id>
        <link href="http://www.zotero.org/styles/example" rel="self"/>
        <link href="http://www.zotero.org/styles/apa" rel="template"/>
        <link href="http://www.example.com/style-guide/" rel="documentation"/>
        <author>
          <name>John Doe</name>
          <email>JohnDoe@example.com</email>
        </author>
        <contributor>
          <name>Jane Doe</name>
        </contributor>
        <contributor>
          <name>Bill Johnson</name>
        </contributor>
        <category citation-format="author-date"/>
        <category field="science">
        <updated>2014-10-15T18:17:09+00:00</updated>
        <rights license="http://creativecommons.org/licenses/by-sa/3.0/">This work is licensed under a Creative Commons Attribution-ShareAlike 3.0 License</rights>
      </info>
      <locale xml:lang="en">
        <terms>
          <term name="no date">without date</term>
        </terms>
      </locale>
      <macro name="author">
        <names variable="author">
          <name initialize-with="."/>
        </names>
      </macro>
      <macro name="issued-year">
        <choose>
          <if variable="issued">
            <date variable="issued">
              <date-part name="year"/>
            </date>
          </if>
          <else>
            <text term="no date"/>
          </else>
        </choose>
      </macro>
      <citation et-al-min="3" et-al-use-first="1">
        <sort>
          <key macro="author"/>
          <key macro="issued-year"/>
        </sort>
        <layout prefix="(" suffix=")" delimiter="; ">
          <group delimiter=", ">
            <text macro="author"/>
            <text macro="issued-year"/>
          </group>
        </layout>
      </citation>
      <bibliography>
        <sort>
          <key macro="author"/>
          <key macro="issued-year"/>
          <key variable="title"/>
        </sort>
        <layout suffix="." delimiter=", ">
          <group delimiter=". ">
            <text macro="author"/>
            <text macro="issued-year"/>
            <text variable="title"/>
            <text variable="container-title"/>
          </group>
          <group>
            <text variable="volume"/>
            <text variable="issue" prefix="(" suffix=")"/>
          </group>
          <text variable="page"/>
        </layout>
      </bibliography>
    </style>

Style Structure
'''''''''''''''

To understand the style above, lets first look at the child elements of the ``cs:style`` root element:

.. sourcecode:: xml

    <?xml version="1.0" encoding="utf-8"?>
    <style>
      <info/>
      <locale/>
      <macro/>
      <macro/>
      <citation/>
      <bibliography/>
    </style>

Compared to a dependent style, which only has the ``cs:info`` child element, we see several additional elements here. In additional to ``cs:info``, we see ``cs:locale``, ``cs:macro``, ``cs:citation``, and ``cs:bibliography``.

What do these elements do?

- The required ``cs:info`` element fulfills the same function in independent styles as it does in dependent styles: it stores the style metadata.
- The optional ``cs:locale`` elements can be used to overwrite the locale data from the locale files.
- The optional ``cs:macro`` elements can be used to store CSL code for use by ``cs:citation``, ``cs:bibliography``, or other ``cs:macro`` elements.
- The required ``cs:citation`` element defines the format of citations.
- The optional ``cs:bibliography`` element defines the format of the bibliography.

With this in mind, let's step through the style, starting with the ``cs:style`` element.

``cs:style`` Root Element
'''''''''''''''''''''''''

.. sourcecode:: xml

    <style xmlns="http://purl.org/net/xbiblio/csl" class="in-text" version="1.0">
      ...
    </style>

We've already come across the ``xmlns`` and ``version`` attributes when we looked at the ``cs:style`` element of our dependent style. The ``class`` attribute is new. It tells the CSL processor whether it is an "in-text" or "note" style.

``cs:info`` Element
'''''''''''''''''''

The style metadata for independent styles is usually more expansive than for dependent styles:

.. sourcecode:: xml

    <info>
      <title>Example Style</title>
      <id>http://www.zotero.org/styles/example</id>
      <link href="http://www.zotero.org/styles/example" rel="self"/>
      <link href="http://www.zotero.org/styles/apa" rel="template"/>
      <link href="http://www.example.com/style-guide/" rel="documentation"/>
      <author>
        <name>John Doe</name>
        <email>JohnDoe@example.com</email>
      </author>
      <contributor>
        <name>Jane Doe</name>
      </contributor>
      <contributor>
        <name>Bill Johnson</name>
      </contributor>
      <category citation-format="author-date"/>
      <category field="science">
      <updated>2014-10-15T18:17:09+00:00</updated>
      <rights license="http://creativecommons.org/licenses/by-sa/3.0/">This work is licensed under a Creative Commons Attribution-ShareAlike 3.0 License</rights>
    </info>

The title, style ID, "self" link, categories, time stamp, and license work the same, but there are differences. First, independent styles don't depend on a parent style. Instead we usually provide a "template" link to indicate which style was used as a starting point for creating the current style (CSL styles are rarely written from scratch, since it's usually much faster to adapt an existing one). In this case, the template was the APA style. We also like to include one or more "documentation" links that point to an online description of the citation format in question.

To acknowledge the creators of CSL styles, their names and contact information can be added to the style. In this case, we have one author and two contributors. Authors usually have done most of the work in creating the style, whereas contributors have provided small improvements.

``cs:citation`` and ``cs:macro`` Elements
'''''''''''''''''''''''''''''''''''''''''

Let's jump down now to the macros and ``cs:citation`` element. The purpose of the ``cs:citation`` element is to describe the format of citations (or, for "note" styles, the format of footnotes or endnotes).

.. sourcecode:: xml

    <macro name="author">
      <names variable="author">
        <name initialize-with="."/>
      </names>
    </macro>
    <macro name="issued-year">
      <choose>
        <if variable="issued">
          <date variable="issued">
            <date-part name="year"/>
          </date>
        </if>
        <else>
          <text term="no date"/>
        </else>
      </choose>
    </macro>
    <citation et-al-min="3" et-al-use-first="1">
      <sort>
        <key macro="author"/>
        <key macro="issued-year"/>
      </sort>
      <layout prefix="(" suffix=")" delimiter="; ">
        <group delimiter=", ">
          <text macro="author"/>
          <text macro="issued-year"/>
        </group>
      </layout>
    </citation>

The code above generates citations like "(A.C. Smith et al., 2002; W. Wallace, J. Snow, 1999)". To understand how this citation format is encoded in CSL, let's first focus on the ``cs:layout`` element of ``cs:citation``. Its ``prefix`` and ``suffix`` attributes define the parentheses around the citation, while the value of the ``delimiter`` attribute ("; ") separates neighboring cites. The format of each individual cite is defined by the contents of ``cs:layout``, which consists of the output of the "author" and "issued-year" macros, separated by the value of the "delimiter" attribute (", ") on the ``cs:group`` element.

The "author" macro prints the names stored in the "author" name variable of the cited item. The ``initialize-with`` attribute on ``cs:name`` specifies that given names should appear as initials, and that each initial is followed by the attribute's value (".").

The "issued-year" macro starts with a test, defined with the ``cs:choose`` element. If the cited item has a date stored in its "issued" date variable, the year of this date is printed. Otherwise, the style prints the value of the "no date" term.

You might wonder why we didn't just put the CSL code from the two macros directly into the ``cs:citation`` element. What are the advantages of using macros? Well, in the example above, the use of macros simplifies the structure of ``cs:citation``, making it easier to follow. In addition, both macros are called a total of four times in the style (twice in ``cs:citation``, and twice in ``cs:bibliography``). Without macros, we'd have to repeat the CSL code of these macros multiple times. Macros thus allow for more compact styles.

We're not done yet. The ``cs:citation`` element carries two attributes, ``et-al-min`` and ``et-al-use-first``. Together, they specify that if an item has three or more "author" names, only the first name is printed, followed by the value of the "et al" term.

Finally, ``cs:citation`` contains the ``cs:sort`` element, which itself contains two ``cs:key`` elements. This section specifies how cites within a citation are sorted. The first sorting key consists of the output of the "author" macro (CSL is smart enough to sort names by the family name first, and by initials second). Any cites with the same output for the first key are then sorted by the second sorting key, which is the output of the "issue-year" macro.

``cs:bibliography`` Element
'''''''''''''''''''''''''''

Whereas ``cs:citation`` is responsible for citations and cites, the ``cs:bibliography`` element is used to define the format of bibliographic entries.

.. sourcecode:: xml

    <macro name="author">
      <names variable="author">
        <name initialize-with="."/>
      </names>
    </macro>
    <macro name="issued-year">
      <choose>
        <if variable="issued">
          <date variable="issued">
            <date-part name="year"/>
          </date>
        </if>
        <else>
          <text term="no date"/>
        </else>
      </choose>
    </macro>
    ...
    <bibliography>
      <sort>
        <key macro="author"/>
        <key macro="issued-year"/>
        <key variable="title"/>
      </sort>
      <layout suffix="." delimiter=", ">
        <group delimiter=". ">
          <text macro="author"/>
          <text macro="issued-year"/>
          <text variable="title"/>
          <text variable="container-title"/>
        </group>
        <group>
          <text variable="volume"/>
          <text variable="issue" prefix="(" suffix=")"/>
        </group>
        <text variable="page"/>
      </layout>
    </bibliography>

The ``cs:bibliography`` section of our example style really only works well for a single type of items: journal articles. It generates bibliographic entries in the form of:

    A.C. Smith, D. Williams, T. Johnson. 2002. Story of my life. Journal of Biographies, 12(2), 24—27.
    W. Wallace, J. Snow. 1999. Winter is coming. Journal of Climate Dynamics, 6(9), 97—102.

How were we able to define this format? First, the structure of ``cs:bibliography`` is very similar to that of ``cs:citation``, but here ``cs:layout`` defines the format of each individual bibliographic entry. In addition to the "author" and "issued-year" macro, the bibliographic entries also show each item's "title" and "container-title" (for journal articles, the "container-title" is the title of the journal), the "volume" and "issue" in which the article was printed, and the pages ("page") on which the article appeared. The style uses the ``prefix`` and ``suffix`` attributes to wrap the journal issue number in parentheses, and relies on the ``suffix`` and ``delimiter`` attributes on the ``cs:layout`` and ``cs:group`` elements to place the rest of the punctuation.

The ``cs:bibliography`` element also contains a ``cs:sort`` element, with three keys: the "author" and "issued-year" macros, and, as a third key, the item's "title".

``cs:locale`` Element
'''''''''''''''''''''

The last section of our style is ``cs:locale``. As we wrote above, CSL locale files allow CSL styles to quickly translate into different languages. However, sometimes it's desirable to overwrite the default translations.

.. sourcecode:: xml

    <locale xml:lang="en">
      <terms>
        <term name="no date">without date</term>
      </terms>
    </locale>

The translation for the "no date" term in the CSL locale file for US English is, not very surprising, "no date". However, for our example style, I wanted to use "without date" instead. To overwrite the default translation, we can use the ``cs:locale`` element as shown above. For an item without an issued date, this would result in a citation like "(D. Williams, without date)".

The ``xml:lang`` attribute on ``cs:locale`` is set to "en", which tells the style to overwrite the "no date" translation whenever the style is used in English. If we used the style in German, the style would still print the translation from the German locale file ("ohne Datum").

Diving Deeper
~~~~~~~~~~~~~

You finished the primer. Good job! If you're interested in learning more about CSL, you're now well prepared to start reading the `CSL Specification`_ and our other documentation on the `Citation Style Language`_ website.

Feedback
~~~~~~~~

Questions or feedback? Contact us on Twitter at `@csl_styles`_.

.. _@csl_styles: https://twitter.com/csl_styles
