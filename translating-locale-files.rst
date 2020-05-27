Guide to Translating CSL Locale Files
=====================================

by `Rintze M. Zelle, PhD <https://twitter.com/rintzezelle>`_

|CCBYSA|_

.. |CCBYSA| image:: /media/cc-by-sa-80x15.png
.. _CCBYSA: http://creativecommons.org/licenses/by-sa/3.0/

.. contents:: **Table of Contents**

Preface
~~~~~~~

This document describes how you can help us improve the language support of
Citation Style Language (CSL) styles by translating the CSL locale file of your
favorite language.

CSL styles are either bound to one particular locale (e.g. the "British
Psychological Society" CSL style will always produce citations and
bibliographies in British English), or they (automatically) localize, e.g. to a
user-selected locale, or to the locale of the user's operating system.

All CSL styles, both those with and without a fixed locale, rely on locale files
for default localization data, which consists of translated terms commonly used
in citations and bibliographies, date formats, and grammar rules. Storing
localization data in separate files has several benefits: translations are
easier to maintain, styles are more compact (although styles can still include
their own localization data to override the defaults), and styles can be
(mostly) language-agnostic.

Below we will describe the structure of a locale file, give instructions on how
to translate all its parts, and explain how you can submit your translations.
See the `CSL specification
<specification.html>`_ for more in-depth
documentation on the structure and function of locale files.

Getting Started
~~~~~~~~~~~~~~~

The CSL locale files are kept in a GitHub repository at
https://github.com/citation-style-language/locales/.

Each locale file contains the localization data for one language. Locale files
are named "locales-xx-XX.xml", where "xx-XX" is a `BCP 47 language code
<https://r12a.github.io/app-subtags/>`_ (e.g. the locale code
for British English is "en-GB"). The `repository wiki
<https://github.com/citation-style-language/locales/wiki>`_ lists the locale
code, language, and translation status of all locale files in the repository.

If you find that a locale file already exists for your language, but that its
translations are inaccurate or incomplete, you can start translating that file.
If there is no locale file for your language, copy the "locales-en-US.xml" file
and start from there. Don't worry about finding the correct BCP 47 locale code
for a new language; we'll be happy to look it up when you submit your new locale
file.

Modifications to existing locale files can be made directly via the GitHub
website. New locale files can be submitted as a "gist" or through a pull
request. For details, see the `instructions on submitting CSL styles
<https://github.com/citation-style-language/styles/blob/master/CONTRIBUTING.md>`_. If
you edit a locale file on your own computer, use a suitable plain text editor
such as TextWrangler for OS X, Notepad++ for Windows, or jEdit.

Translating Locale Files
~~~~~~~~~~~~~~~~~~~~~~~~

When translating locale files, leave the overall structure of the locale file
untouched. It makes life easier for us if you don't remove existing elements,
introduce new ones, or move stuff around (exceptions are discussed below).

xml:lang
^^^^^^^^

At the top of the locale file you'll find the ``locale`` root element. The value
of its ``xml:lang`` attribute should be set to the same language code used in
the file name of the locale file. For the locales-en-US.xml locale file, this is
"en-US":

.. sourcecode:: xml

    <locale xmlns="http://purl.org/net/xbiblio/csl" version="1.0" xml:lang="en-US">


Locale File Metadata
^^^^^^^^^^^^^^^^^^^^

Directly below the ``locale`` root you'll find an ``info`` element. Here you can
list yourself as a translator using the ``translator`` element. Optionally you
can include contact information such as a website or email address. A locale
file can list multiple translators. The ``rights`` element indicates under which
license the locale file is released. All locale files in our repository use the
same Creative Commons license, so you don't have to change anything here. The
``updated`` element is used to keep track of when the locale file was last
updated. Feel free to ignore it if the format looks too intimidating.

.. sourcecode:: xml

    <info>
      <translator>
        <name>John Doe</name>
        <email>john.doe@citationstyles.org</email>
        <uri>http://citationstyles.org/</uri>
      </translator>
      <rights license="http://creativecommons.org/licenses/by-sa/3.0/">This work is licensed under a Creative Commons Attribution-ShareAlike 3.0 License</rights>
      <updated>2012-07-04T23:31:02+00:00</updated>
    </info>

Grammar Rules
^^^^^^^^^^^^^

Next up is the ``style-options`` element:

.. sourcecode:: xml

    <style-options punctuation-in-quote="true"/>

This element is used to define localized grammar rules, as described in the
`Locale Options
<specification.html#locale-options>`_
section in the CSL specification.

Date Formats
^^^^^^^^^^^^

CSL styles can render dates in either non-localizing or localizing formats:

.. sourcecode:: xml

    <style>

      <!-- use of non-localized date format -->
      <macro name="accessed">
        <date variable="accessed" suffix=", ">
          <date-part name="month" suffix=" "/>
          <date-part name="day" suffix=", "/>
          <date-part name="year"/>
        </date>
      </macro>

      <!-- use of localized date format -->
      <macro name="issued">
        <date variable="issued" form="text"/>
      </macro>

    </style>

Each locale file defines two localized date formats: a numeric format (e.g.
"2012/9/3"), and a textual format, where the month is written out in full (e.g.
"September 3, 2012").

To localize a date format, place the date-part elements for "day", "month", and
"year" in the desired order. Use the ``prefix`` and ``suffix`` attributes (on
the ``date-part`` elements), or the ``delimiter`` attribute (on the ``date``
element) to define punctuation before, after, and between the different
date-parts. When using affixes, make sure that dates that only consist of a year
and a month, or of only a year, still render correctly. For example, the US
English localized "text" date format,

.. sourcecode:: xml

    <date form="text">
      <date-part name="month" suffix=" "/>
      <date-part name="day" suffix=", "/>
      <date-part name="year"/>
    </date>

will produce dates like "September 3, 2012", "September 2012", and "2012".
Compare this to

.. sourcecode:: xml

    <date form="text">
      <date-part name="month"/>
      <date-part name="day" prefix=" "/>
      <date-part name="year" prefix=", "/>
    </date>

which gives the same correct complete date ("September 3, 2012"), but which
produces incorrect output for dates that don't have a day, or don't have a day
and month ("September, 2012" and ", 2012", respectively).

To read more about customizing date formats, see the `Localized Date Formats
<specification.html#localized-date-formats>`_
and `Date-part
<specification.html#date-part>`_ sections in
the CSL specification.

Terms
^^^^^

The ``terms`` element makes up the last section of the locale file, and contains
all the term translations. Below we discuss the different types of terms, and
how to translate them.

In its simplest form, a term consists only of a ``term`` element with the
``name`` attribute indicating the term name, and with the translation enclosed
between the start and end tag:

.. sourcecode:: xml

    <terms>
      <term name="et-al">et al.</term>
    </terms>

See the `Terms <specification.html#terms>`_
section in the CSL specification.


Abbreviations
'''''''''''''

When translating abbreviations such as "et al.", always include periods where
applicable.

Plurals
'''''''

Many terms have translations for both the singular and plural form. In this
case, the ``term`` element contains a ``single`` (for singular) and a
``multiple`` (for plural) element, which enclose the translations:

.. sourcecode:: xml

    <terms>
      <term name="edition">
        <single>edition</single>
        <multiple>editions</multiple>
      </term>
    </terms>

Forms
'''''

Terms can also vary in their 'form', which is indicated with the "form"
attribute on the ``term`` element. The different forms are "long" (the default),
"short" (abbreviated form of "long"), "verb", "verb-short" (abbreviated form of
"verb"), and "symbol". Examples of the different forms:

.. sourcecode:: xml

    <terms>
      <term name="editor">
        <single>editor</single>
        <multiple>editors</multiple>
      </term>

      <term name="editor" form="short">
        <single>ed.</single>
        <multiple>eds.</multiple>
      </term>

      <term name="editor" form="verb">edited by</term>
      <term name="editor" form="verb-short">ed.</term>

      <term name="paragraph">
        <single>paragraph</single>
        <multiple>paragraph</multiple>
      </term>

      <term name="paragraph" form="symbol">
        <single>¶</single>
        <multiple>¶¶</multiple>
      </term>
    </terms>

AD/BC
'''''

The "ad" and "bc" terms are used to format years before 1000. E.g. the year "79"
becomes "79AD", and "-2500" becomes "2500BC".

See the `AD and BC
<specification.html#ad-and-bc>`_ section in
the CSL specification.

Punctuation
'''''''''''

The terms "open-quote", "close-quote", "open-inner-quote", "close-inner-quote",
and "page-range-delimiter" define punctuation.

When a CSL style renders a title in quotes through the use of the ``quotes``
attribute, it uses the "open-quote" and "close-quote" terms. When the title
contains internal quotes, these are replaced by "open-inner-quote",
"close-inner-quote". For example, with

.. sourcecode:: xml

    <terms>
      <term name="open-quote">“</term>
      <term name="close-quote">”</term>
      <term name="open-inner-quote">‘</term>
      <term name="close-inner-quote">’</term>
      <term name="page-range-delimiter">–</term>
    </terms>

styles can render titles as

::

    “Moby-Dick”
    “Textual Analysis of ‘Moby-Dick’”

The "page-range-delimiter" terms is used to connect the first and last page of
page ranges, e.g. "15–18" (it's default value is an en-dash).

See the `Quotes
<specification.html#quotes>`_ and `Page
Ranges <specification.html#page-ranges>`_
sections in the CSL specification.

Ordinals
''''''''

CSL styles can render numbers (e.g., "2") in two ordinal forms: "long-ordinal"
("second") and "ordinal" ("2nd"). Both forms are localized through the use of
terms.

The "long-ordinal" form is limited to the numbers 1 through 10 (the fallback for
other numbers is the "ordinal" form). Each of these ten numbers has its own term
("long-ordinal-01" through "long-ordinal-10").

Things are different for the "ordinal" form. Here, terms are only used to define
the ordinal suffix ("nd" for "2nd"). Furthermore, terms and numbers don't
correspond one-to-one. For example, the "ordinal" term defines the default
suffix, which is used for all numbers (unless, as described below, exceptions
are introduced through the use of the terms "ordinal-00" through "ordinal-99").

CSL also supports gender-specific ordinals (both for "long-ordinal" and
"ordinal" forms). In languages such as French, ordinal numbers must match the
gender of the target noun, which can be feminine or masculine. E.g. "1re
édition" ("édition" is feminine) and "1er janvier" ("janvier" is masculine). See
the relevant section below.

Terms for "ordinal" numbers
|||||||||||||||||||||||||||

Terms for the "ordinal" form follow special rules to make it possible to render
any number in the "ordinal" form (e.g., "2nd", "15th", "231st"), without having
to define a term for each number.

The logic for defining ordinal suffixes with terms is described at `Ordinal Suffixes
<specification.html#ordinal-suffixes>`_, and
won't be revisited here. Instead, we'll look at an example.

In English, there are four different ordinal suffixes in use: "st", "nd", and
"rd" are used for numbers ending on 1, 2, and 3, respectively, while "th" is
used for numbers ending on 0 and 4 through 9. Exceptions are numbers ending on
"11", "12", and "13", which also use "th".

To capture this logic, we start by defining the "ordinal" term as "th", which is
the most common suffix. Then, we define the terms "ordinal-01", "ordinal-02",
and "ordinal-01" as "st", "nd", and "rd", respectively. By default (i.e., when
the ``term`` elements don't carry a ``match`` attribute), the terms "ordinal-00"
through "ordinal-09" repeat at intervals of 10. For example, the term
"ordinal-01" overrides the "ordinal" term for numbers 1, 11, 21, 31, etc. At
this point, we would get "ordinal" numbers such as "1st", "2nd", "3rd", "4th",
"21st", "67th", and "101st", but we would also get the incorrect "11st", "12nd"
and "13rd". For these cases, we define the terms "ordinal-11", "ordinal-12", and
""ordinal-13" as "th". By default, the terms "ordinal-10" though "ordinal-99"
repeat at intervals of 100. For example, the term "ordinal-11" overrides the
"ordinal" and "ordinal-01" terms for numbers "11", "111", "211", etc. So, in
total, we need the following seven terms:

.. sourcecode:: xml

    <terms>
      <term name="ordinal">th</term>
      <term name="ordinal-01">st</term>
      <term name="ordinal-02">nd</term>
      <term name="ordinal-03">rd</term>
      <term name="ordinal-11">th</term>
      <term name="ordinal-12">th</term>
      <term name="ordinal-13">th</term>
    </terms>

Fortunately, many languages have simpler "ordinal" numbers. E.g., for German all
"ordinal" numbers receive a period as the suffix, so it suffice to define the
"ordinal" term:

.. sourcecode:: xml

    <terms>
      <term name="ordinal">.</term>
    </terms>

Gender-specific Ordinals
||||||||||||||||||||||||

To use gender-specific ordinals, we first need to define the gender of several
target nouns: the terms accompanying the number variables (it is probably
sufficient to specify the gender for "edition", "issue", and "volume") and the
month terms ("month-01" through "month-12", corresponding to January through
December). This is done by setting the ``gender`` attribute on the "long"
(default) form of these terms to either "masculine" or "feminine".

Secondly, we need to define "feminine" and "masculine" variants of the ordinal
terms, which is done with the ``gender-variant`` attribute (set to "masculine"
or "feminine").

A minimal example for French:

.. sourcecode:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <locale xml:lang="fr-FR">
      <terms>
        <term name="edition" gender="feminine">
          <single>édition</single>
          <multiple>éditions</multiple>
        </term>
        <term name="edition" form="short">éd.</term>
        <term name="month-01" gender="masculine">janvier</term>

        <term name="long-ordinal-01" gender-form="masculine">premier</term>
        <term name="long-ordinal-01" gender-form="feminine">première</term>
        <term name="long-ordinal-01">premier</term>
        <term name="long-ordinal-02">deuxième</term>
        <term name="long-ordinal-03">troisième</term>
        <term name="long-ordinal-04">quatrième</term>
        <term name="long-ordinal-05">cinquième</term>
        <term name="long-ordinal-06">sixième</term>
        <term name="long-ordinal-07">septième</term>
        <term name="long-ordinal-08">huitième</term>
        <term name="long-ordinal-09">neuvième</term>
        <term name="long-ordinal-10">dixième</term>

        <term name="ordinal">e</term>
        <term name="ordinal-01" gender-form="feminine" match="whole-number">re</term>
        <term name="ordinal-01" gender-form="masculine" match="whole-number">er</term>
      </terms>
    </locale>

In this example, the "edition" term is defined as feminine, and the "month-01"
term ("janvier") is defined as masculine. For French, of the "long-ordinal"
terms, only "long-ordinal-01" has gender-variants ("premier" for masculine,
"première" for feminine). To cover cases where no gender is defined for the
target noun (e.g., a style author might redefine a term like "edition" but
forget to specify the gender), also a neuter variant of "long-ordinal-01" is
defined without the ``gender`` attribute. The "ordinal" term defines the default
suffix. The only exceptions are for the number 1 when the target noun is either
feminine or masculine (with ``match`` set to "whole-number", the term does not
repeat), e.g. "1re édition" but "11e édition".

For more information, see the `Gender-specific Ordinals
<specification.html#gender-specific-ordinals>`_
section in the CSL specification.

Submitting Contributions
~~~~~~~~~~~~~~~~~~~~~~~~

To submit changes to an existing locale file, or to submit a new locale file,
follow the `submission instructions for CSL styles
<https://github.com/citation-style-language/styles/blob/master/CONTRIBUTING.md>`_.

Questions?
~~~~~~~~~~

Questions? Contact us on Twitter at `@csl_styles`_, or create an issue on GitHub `here <https://github.com/citation-style-language/locales/issues>`_.

.. _@csl_styles: https://twitter.com/csl_styles
