############################
``citeproc-js``
############################
||||||||||||||||||||||||||||||||||||||||||||
:small-caps:`CSL-m` Specification Supplement
||||||||||||||||||||||||||||||||||||||||||||

.. class:: fixed

   .. raw:: html

      <p><a href="http://citationstyles.org" target="_blank">citationstyles.org</a></p>
      <p><a href="http://citationstylist.org"><span style="font-variant:small-caps;">CitationStylist.org</span></a></p>

.. class:: contributors

   Frank G. Bennett, Jr.

.. class:: date

   October 23, 2013

.. |citeproc-js| replace:: ``citeproc-js``
.. |link| image:: link.png
.. |(multilingual)| image:: multilingual-required-90.png
.. |(modification)| image:: modification.png
.. |(extension)| image:: extension.png
.. |(approved for CSL)| image:: csl-approved-90.png
.. |ndash|  unicode:: U+02013 .. EN DASH
.. |mdash|  unicode:: U+02014 .. EM DASH
.. |para|   unicode:: U+000B6 .. PILCROW SIGN

*[Note: this document is undergoing revision. When complete, code samples will be provided under each major heading.]* 

========

This supplement is a companion to the `CSL 1.0.1 Specification`__. It is aimed at
style authors, and documents differences between official CSL and the
CSL-m schema recognised by Multilingual Zotero.

The changes are of two types:

* **Modifications** alter the behaviour or validation rules of
  existing CSL elements or attributes. Changes of this kind can cause
  official CSL repository to fail validation under the CSL-m schema:
  such styles will run, but may produce unexpected results.  The
  potential effects of this category of changes are indicated where
  relevant.

* **Extensions** add entirely new elements or attributes to the
  schema. Styles making use of extended syntax will fail validation
  under the official CSL schema, and can be used only with
  Multilingual Zotero.

__ http://citationstyles.org/downloads/specification.html

========

.. contents:: Table of Contents

========


%%%%%%%%%%
Item Types
%%%%%%%%%%

=========================
``classic`` |(extension)|
=========================

Use the ``classic`` type for sources commonly cited within a field. Cites
of this type do not appear in the bibliography, and can be completely
reformatted to a compact, style-specific form using the **Classic**
abbreviation list in the **Abbreviation Filter**.

When a short form is supplied for a **Classic** item, the ``title``
variable is suppressed, and the short form entirely replaces the
``author`` variable. When a volume number or other details are included,
these can be rendered on either side of the composite abbreviated form,
but not of course within it.

.. sourcecode:: xml

   <choose>
     <if type="classic">
       <group delimiter=" ">
         <text variable="volume"/>
         <group delimiter=", ">
           <names variable="author"/>
           <text variable="title"/>
         </group>
     </if>
   </choose>



=========================
``gazette`` |(extension)|
=========================

Use the ``gazette`` type for instruments published through an official
gazette. Typical use cases would be cites to amending acts, to the initial
version of legislation, or to orders and other instruments that are not
available from other official sources. For consolidated acts or codified
statutes or regulations, use the ``legislation`` (MLZ **Statute**) or
``regulation`` (MLZ **Regulation**) types instead.

.. sourcecode:: xml

   <choose>
     <if type="gazette">
        <text macro="gazette-mac"/>
     </if>
   </choose>

The format of gazette citations may vary among jurisdictions. Test the
``jurisdiction`` variable (see below) to discriminate between citation
forms.

=========================
``hearing`` |(extension)|
=========================

The ``hearing`` type is primarily intended for transcripts of official
hearings by government committees and the like (other documents
produced by a committee should be cast as the ``report`` type
instead). The body conducting a hearing is set in the ``authority``
variable (**Legis. Body** in MLZ).

.. sourcecode:: xml

   <choose>
     <if type="hearing">
        <text macro="hearing-mac"/>
     </if>
   </choose>
   
============================
``regulation`` |(extension)|
============================

Use the ``regulation`` type for administrative orders at all
levels of government.

.. sourcecode:: xml

   <choose>
     <if type="regulation">
        <text macro="hearing-mac"/>
     </if>
   </choose>
   
=======================
``video`` |(extension)|
=======================

The ``video`` type is appropriate for video works that are not
disseminated through an access-restricted distribution channel. For
example, content hosted on YouTube should be set to ``video``, while
a DVD release of "The Wizard of Oz" should be set to ``motion_picture``.

.. sourcecode:: xml

   <choose>
     <if type="regulation">
        <text macro="hearing-mac"/>
     </if>
   </choose>
   
%%%%%%%%
Elements
%%%%%%%%

===============================
``cs:conditions`` |(extension)|
===============================

Condition statements in official CSL take a single "match" attribute,
which determines how the tests will be combined. The match attribute
value (``all``, ``any``, ``none``) applies to all tests within the
statement: grouping of tests with separate match values is not possible.
To simplify the coding of complex styles, CSL-m introduces an optional
alternative syntax for condition statements.

The alternative syntax may be applied to ``cs:if`` or ``cs:else-if``
elements (the "parent node" in this description). The parent node must
have no attributes, and a single ``cs:conditions`` node as its first
child element. The ``cs:conditions`` node must have one or more
``cs:condition`` children. The ``cs:condition`` children each define a
conditional statement with attributes specified in the CSL 1.0.1
schema and in this Supplement. The ``cs:condition`` statements are
joined according to a mandatory "match" attribute on
``cs:conditions``.

Note that CSL-m adds a "nand" match value (true if at least one of
the tests or condition statements to which it applies returns false),
in addition to "all", "any", and "none".

.. sourcecode:: xml

   <choose>
     <if>
       <conditions match="any">
         <condition type="chapter"/>
         <condition variable="container-title collection-title" match="nand"/>
       </conditions>
       <text macro="some-chapter-mac"/>
     </if>
   </choose>
   


================================
``cs:institution`` |(extension)|
================================

Institutional names are fundamentally different in structure from
personal names. CSL provides quite robust support for the presentation
and sorting of personal names, but in CSL 1.0.1, institutional names
have just one fixed form, and are otherwise treated the same as
personal names in a list of creators.

Some publishing environments require greater flexibility.  Institution
names can consist of multiple subunits. Individuals may be credited
together with the institution to which they belong. Unaffiliated
personal authors may be cited together with an institution or with
individuals affiliated with it.  Some examples:

1. Research & Pub. Policy Dep't, Nat'l Urban League
2. United Nations - ECLAC
3. ECLAC (Economic Commission for Latin America and the Carribean)
4. Canadian Conservation Institute (CCI)
5. Nolan J. Malone and others, U.S. Bureau of the Census
6. World Trade Organization and World Health Organization
7. Smith with Jones, Bureau of Sloth, Ministry of Fear
8. Doe et al. with Roe et al., Ministry of Fear & Noakes, Ministry of Destruction

Examples 3 and 4 render both the full form and the acronym of a single
institution name, with arbitrary ordering of the two name parts.
Example 1 begins with the smallest subunit in a list of related
institutions, and example 2 does the opposite.  Examples 1 and 2 are
pure organizations, while example 5 is a mix of personal and
institutional names.  Examples 1, 2, 3 and 4 would be entered as
literal strings currently, which has obvious drawbacks.  Example 5
would require that the authorship information be spread across two
variables, although all parties listed are equally authors of the
resource.  Example 6 can be produced in CSL 0.8, but examples 7 and 8
cannot.

The MLZ extensions to CSL 1.0.1 provide a cs:institution element, which
can be used to produce any of the above forms, without interfering
with the formatting of ordinary personal names. The extension is
always enabled in |citeproc-js|, but the application calling
|citeproc-js| (i.e. Zotero) must specially flag institutional names
for it to take effect. MLZ provides this flag, while the official
Zotero client does not. For this reason, this extension only works
with the multilingual client at present.

-----------------
Entry conventions
-----------------

In multilingual Zotero, names entered in two-field mode are personal,
and those entered in single-field mode are treated as
organizations. Names should be entered in the order in which they
should appear in citations, with one (extremely rare) exception: when
an unaffiliated author is included in a list of names that includes
one or more institutions, the name of the unaffiliated author(s)
should come *after* that of the last institution in the list.

Subunits of an organizational name should be separated with a
field separator character ``|``.


^^^^^^^^^^^^^^^^^^
Affiliated authors
^^^^^^^^^^^^^^^^^^

Single or multiple personal Names that are co-authors with an
organization would be entered above the relevant organization name.


.. image:: affiliated-authors.png

In a very simple style, the sample above might be rendered as: *Clarke,
Ministry of Fear and Smith & Brown, Large Corporation*.

^^^^^^^^^^^^^^^^^^^^
Unaffiliated authors
^^^^^^^^^^^^^^^^^^^^

Authors with no affiliation would be listed after any organizational
names:

.. image:: unaffiliated-authors.png


In a very simple style, the sample above might be rendered as: *Doe &
Roe with Clarke, Ministry of Fear and Smith & Brown, Large Corporation*
(note the reverse ordering in this case, with the names at the end
placed at the front of the rendered list of names). 

The structure of mixed personal and organizational names can thus be
expressed in the current Zotero UI. We now turn to the extended
CSL syntax used to control the appearance of such names.

-------------
CSL Extension
-------------

^^^^^^^^^^^^^^^^^^^^^^^^^^^
Element: ``cs:institution``
^^^^^^^^^^^^^^^^^^^^^^^^^^^

A ``cs:institution`` element can be placed immediately after the
``cs:name`` element to control the formatting of organization
names. 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Attributes: ``delimiter`` and ``and``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The value of the ``delimiter`` attribute on ``cs:institution``
is used in the following locations:

* between organization names;
* between the subunits of an organization;
* between affiliated authors and their institution.

The ``and`` attribute on ``cs:institution``, if any, is used for the
final join between two or more author/organization units.

A simple use of ``cs:institution`` might read as follows:

.. sourcecode:: xml

   <names variable="author">
     <name and="symbol" initialize-with=". "/>
     <institution and="text" delimiter=", ">
   </names>

With this CSL, all of the delimiters in the following string would be
drawn from attributes on ``cs:institution``: *R. Smith, Small
Committee, Large Corporation, G. Brown, Busy Group, Active Laboratory,
and S. Noakes, Powerful Ministry*.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Attributes: ``use-first``, ``substitute-use-first`` and ``use-last``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To control the omission of names from the middle of the list of
organizational subunits, either of ``use-first`` or
``substitute-use-first`` may be used to pick names from the front of
the list. The ``use-last`` attribute picks names from the end.  The
``substitute-use-first`` attribute includes the leading (smallest)
subunit if and only if no personal names are associated with the
organization.

The following CSL code would format both example 1 and example 5 from
the list of samples at the top of this section:

.. sourcecode:: xml

    <names variable="author" delimiter=", ">
        <name 
          and="symbol" 
          delimiter-precedes-last="never"
          et-al-min="3"
          et-al-use-first="1"/>
        <et-al term="and others"/>
        <institution 
          delimiter=", "
          substitute-use-first="1"
          use-last="1"/>
    </names>

~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Attribute: ``reverse-order``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

By convention, organizational names are rendered in "big endian"
order, from the smallest to the largest organizational unit.  To
provide for cases such as example 2 in the list of samples, a
``reverse-order`` attribute can be applied on ``cs:institution``:

.. sourcecode:: xml

    <names variable="author" delimiter=", ">
        <name/>
        <institution 
          delimiter=" - "
          use-first="1"
          use-last="1"
          reverse-order="true"/>
    </names>
    
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Attribute: ``institution-parts``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    
The components of organization names are normally rendered in their
long form only.  When the `Zotero Abbreviations Gadget`__ is used
with Zotero, abbreviated forms for these names may be available
to the processor.

To use the short form, or combinations of the long and short form, an
``institution-parts`` attribute is available on ``cs:institution``.
The attribute accepts values of ``long``, ``short``, ``short-long``
and ``long-short``.  This attribute would be used to produce examples
3 and 4 in the list of samples, with values of ``short-long`` and
``long-short`` respectively.  A value of ``short`` behaves in the same
way as ``form="short"`` in other contexts in CSL, using the short form
if it is available, and falling back to the long form otherwise.

__ http://onezotero.org/tools/

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Element: ``cs:institution-part``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

One or more cs:institution-part elements can be used to control the
formatting of long and short forms of organization names.  Like
``cs:name-part``, these elements are unordered, and affect only the
formatting of the target name element, specified (as on ``cs:name-part``)
with a required ``name`` attribute.

~~~~~~~~~~~~~~~~~~~~~~~
Attribute: ``if-short``
~~~~~~~~~~~~~~~~~~~~~~~

In example 3, the parentheses should be included only if a short form
of the institution name is available.  The ``if-short`` attribute,
available on ``cs:institution-part`` only when applied to the long
form of an organization name, makes the formatting in the element
conditional on the availability of a short form of the name.  The
following CSL would render example 3 in the list of samples:

.. sourcecode:: xml

    <names variable="author">
        <name/>
        <institution institution-parts="short-long">
            <institution-part name="long" if-short="true" prefix=" (" suffix=")"/>
        </institution>
    </names>

^^^^^^^^^^^^^^^^^^^^
Element: ``cs:with``
^^^^^^^^^^^^^^^^^^^^

In rendered output, unaffiliated personal names are joined to a
following organizational name using an implicit localizable term
``with``.  Styling of this term is permitted through an optional
``cs:with`` element, placed immediately above ``cs:institution``:

.. sourcecode:: xml

    <names variable="author">
        <name/>
        <with font-style="italic" prefix=" " suffix=" "/>
        <institution institution-parts="short-long">
            <institution-part name="long" if-short="true" prefix=" (" suffix=")"/>
        </institution>
    </names>

^^^^^^^^^^^^^^^^^^^^
Simple style example
^^^^^^^^^^^^^^^^^^^^


The simple style used in the illustrated examples in the `Entry conventions`_ section
above would look like this in CSL:

.. sourcecode:: xml

    <names variable="author">
        <name form="short" and="symbol" delimiter=", "/>
        <institution use-last="1" and="text" delimiter=", "/>
    </names>


===========================
``cs:layout`` |(extension)|
===========================

In publishing outside of the English language domain, citation
of foreign material in the style of the originating language
is the norm. For example, a Japanese publication might include
the following references in a single work:

* D. H. McQueen, "Patents and Swedish University Spin-off
  Companies: Patent Ownership and Economic Health", *Patent World*,
  March 1996, pp.22\ |ndash|\ 27.
* 北川善太郎「著作権法１００年記念講演会／著作権制度の未来像」コピーマート No.465, 7頁 (2000年)。

To meet such requirements, the MLZ extensions to CSL permit multiple
``cs:layout`` elements within ``cs:citation`` and ``cs:bibliography``.
Each ``cs:layout`` element but the last must include a ``locale`` attribute
specifying one or more recognized CSL locales, and the final element must
not carry a ``locale`` attribute. The locale applied to an item is determined
by matching it against the locale set in the ``language`` variable of
the item (this value is passed by Zotero). An example:

.. sourcecode:: xml

   <citation>
     <layout locale="en es de">
         <text macro="layout-citation-roman"/>
     </layout>
     <layout locale="ru">
         <text macro="layout-citation-cyrillic"/>
     </layout>
     <layout>
         <text macro="layout-citation-ja"/>
     </layout>
   </citation>

In the example above, an item with ``en``, ``es``
or ``de`` (or ``de-AT``) set in the ``language``
variable will be render by the ``layout-citation-roman``
macro, with locale terms set to the appropriate language.



%%%%%%%%%
Variables
%%%%%%%%%

==============================
``authority`` |(modification)|
==============================

In CSL-m, ``authority`` is handled as an institutional creator, not as an ordinary
variable. It is rendered with a ``cs:names`` element.

.. sourcecode:: xml

   <macro name="std-authority-child">
     <names variable="authority">
       <name suppress-min="0"/>
       <institution institution-parts="short" use-first="1"/>
     </names>
   </macro>

The use of ``suppress-min="0"`` in this example is documented below
under `suppress-min (extension)`_.

================================
``available-date`` |(extension)|
================================

The CSL-m ``available-date`` variable is appropriate for the date on
which a ``treaty`` was made available for signing.

.. sourcecode:: xml

   <group delimiter=" ">
     <text value="opened for signature" font-style="italic"/>
     <date date-parts="year-month-day" form="text" variable="available-date"/>
   </group>


=======================
``dummy`` |(extension)|
=======================

The ``dummy`` name variable is always empty. Use it to force *all*
name variables called through a ``cs:names`` node to render through
``cs:substitute``, and so suppress whichever is chosen for rendering
to be suppressed through the remainder of the current cite.

.. sourcecode:: xml

   <names variable="dummy">
     <name/>
     <label/>
     <substitute>
       <names variable="author"/>
       <names variable="editor"/>
     </substitute>
   </names>


=============================
``hereinafter`` |(extension)|
=============================

The ``hereinafter`` variable is a backreference form specific to a
particular item and style. In MLZ, it can be set only through the
Abbreviation Filter. The role of the variable in a given cite
(i.e. whether it provides an alternative title, an acronym or a
more complete formatted citation) depends on the the style and context.

.. sourcecode:: xml

   <choose>
     <if match="all" type="bill gazette legislation" variable="hereinafter">
       <text variable="hereinafter"/>
     </if>
     <else>
       <text variable="title" form="short"/>
     </else>
   </choose>

The Abbreviation Filter will offer an entry in the ``hereinafter`` list
for every item cited in the document. It is not necessary to use ``form="short"``
on the ``cs:text`` node that renders the variable.


=======================================================
``locator-date`` and ``locator-revision`` |(extension)|
=======================================================

The variable "locator-date" is parsed out from the user-supplied
locator, using the following syntax:

.. sourcecode:: csh

   123|2010-12-01

In this example, "123" is the value of the ``locator`` variable
(a page or other pinpoint string), the ``|`` character marks the
end of the pinpoint, and the ten-character string immediately
following is a full date. If supplied, dates must be given as shown above,
zero-padded, in year-month-day order, and with no space between
the date and the ``|`` character. Non-conforming strings following
the ``|`` marker will be treated as a ``locator-revision`` variable.

The ``locator-revision`` variable consists of a string that is not a
date, following the ``locator-date`` string (if any) as described
above.  If supplied without a ``locator-date``, the
``locator-revision`` string must be preceded by a ``|`` field
separator character.  This variable can be used for version
descriptions associated with some looseleaf services.

These extensions are useful with looseleaf services, because the dates
of the content in these services varies depending on the page cited
and the time at which the resource was referenced. These extensions
permit a single item in the calling application's database to
represent the volume on the library shelf, the page date being
optionally supplied by the user when citing into a document.


============================================
``page`` and ``page-first`` |(modification)|
============================================

The ``page`` and ``page-first`` variables are numeric in CSL-m.
The validator requires that they be rendered with ``cs:number``.

.. sourcecode:: xml

   <number variable="page"/>

==================================
``publication-date`` |(extension)|
==================================

CSL-m adds a ``publication-date`` variable to the language schema.
It is available on the ``gazette``, ``legal_case``, ``legislation``, ``patent``
and ``regulation`` types, and provides the date on which the instrument
was published in the given reporter. On the ``patent`` type, it represents
the date on which the patent was published for opposition (applicable only in certain
jurisdictions).

.. sourcecode:: xml

   <date date-parts="year-month-day" form="text" variable="publication-date"/>


====================================
``publication-number`` |(extension)|
====================================

The ``publication-number`` variable is available on the ``patent`` type,
and provides the number assigned to a patent published for opposition.
It is a numeric variable, and validation requires that it be rendered
with ``cs:number``.

.. sourcecode:: xml

   <number variable="publication-number"/>

============================
``supplement`` |(extension)|
============================

The ``supplement`` variable and its associated locale term are useful
for secondary sources that are regularly updated between fresh
editions. Such fine-grained updates are found in secondary legal
publications. Although a supplement may be identified by number or by
name, ``supplement`` is a numeric variable, and validation requires
that it be rendered with ``cs:number``.

.. sourcecode:: xml

   <choose>
     <if is-numeric="supplement">
       <group delimiter=" ">
         <label variable="supplement"/>
         <number variable="supplement"/>
       </group>
     </if>
     <else>
       <number variable="supplement"/>
     </else>
   </choose>

==============================
``volume-title`` |(extension)|
==============================

The ``volume-title`` variable is available on items of the ``book``
and ``chapter`` type. Use it to identify the name of a volume within a
larger work known by an umbrella title (which may in turn be a part of
a publisher's series, described  by ``collection-title``).

.. sourcecode:: xml

   <text variable="volume-title"/>


%%%%%%%%%%
Attributes
%%%%%%%%%%

=====================================
``default-locale-sort`` |(extension)|
=====================================

Use the ``default-locale-sort`` attribute on the ``cs:style`` node to specify
the language collation to govern sorting behaviour. The sort locale has no effect
on the language of standard terms and labels.

.. sourcecode:: xml

   <style xmlns="http://purl.org/net/xbiblio/csl" 
          class="note" version="1.1mlz1"
          default-locale="en-US"
          default-locale-sort="zh-TW">


If this attribute is *not* set, the sort locale is aligned with the default locale
of the style or processor instance.

=================================
``form="short"`` |(modification)|
=================================

In CSL 1.0.1, rendering the ``title`` variable with the attribute
``form="short"`` produces the same result with any item type: if the
``title-short`` variable has a value, that it used; otherwise the
``title`` variable is rendered as a fallback.

In CSL-m, on the ``legal_case`` type only, the ``form="short"``
attribute does not attempt to render ``title-short``, but instead
renders the ``title`` variable, transformed by the Abbreviation Filter
if an entry for it exists in the list there. This permits the
application of style-specific abbreviation rules, as required by
law-specific style guides such as *The Bluebook: A Uniform System
of Citation*.

.. sourcecode:: xml

   <text variable="title" form="short"/>

=============================
``is-parallel`` |(extension)|
=============================

Set on a ``cs:group`` node, the ``is-parallel`` attribute includes
or suppresses the content of the group node depending on whether it
is rendered in a trailing parallel cite. Four values are recognised
on the attribute:

``false``
    Renders when the cite is not one of a series of
    parallel cites.

``true``
    Renders when the cite is one of several cites in a parallel
    series.

``master``
    Renders when the cite is one of several in a parallel series,
    and is the first cite in the series.

``servant``
    Renders when the cite is one of several in a parallel series,
    and is *not* the first in the series.


.. sourcecode:: xml

   <group delimiter=" ">
     <text font-style="italic" value="supra"/>
     <text value="note"/>
     <text variable="first-reference-note-number"/>
   </group>
   <group delimiter=" " is-parallel="false">
     <text value="at"/>
     <number variable="page-first"/>
   </group>
   <group delimiter=" " is-parallel="true">
     <number variable="volume"/>
     <text variable="container-title"/>
     <number variable="page-first"/>
   </group>

============================
``label-form`` |(extension)|
============================

The ``label-form`` attribute can be used on ``cs:number`` and on
``cs:text`` with the ``macro`` attribute. It accepts the same arguments
as the ``form`` attribute for localised terms: ``long``, ``verb``,
``short``, ``verb-short`` and ``symbol``. Its effect is to override
the ``form`` attribute applicable to terms called via the parent ``cs:text``
or ``cs:number`` node. This can be useful where macros are copied
across styles that require different label forms.

.. sourcecode:: xml

   <text macro="locator-mac" label-form="symbol"/>

=====================================
``leading-noise-words`` |(extension)|
=====================================

When set on a ``cs:style-options`` node in a locale file or in a
style, the ``leading-noise-words`` attribute takes a comma-delimited
list of words as its argument.

When a list is set, the same attribute on a ``cs:text`` node with
``variable="title" takes an argument of ``demote`` or ``drop``.  With
the ``demote`` attribute, noise words at the start of the field are
rendered after the remainder of the title field, delimited by a comma.
With the ``drop`` attribute the leading noise words are simply
removed.

.. sourcecode:: xml

   <style-options leading-noise-words="a,an,the"/>

and

.. sourcecode:: xml

   <text variable="title" leading-noise-words="demote"/>


============================
``locator`` |(modification)|
============================

In CSL-m, ``locator`` is a numeric variable. Validation requires that
it be rendered with ``cs:number``.

.. sourcecode:: xml

   <number variable="locator"/>

==============================
``match="nand"`` |(extension)|
==============================

With the ``nand`` argument to the ``match`` attribute, a node test is true
if at least one of the tests it invokes is false.

.. sourcecode:: xml

   <choose>
     <if variable="volume issue" match="nand">
       <text macro="volume-issue-mac"/>
     </if>
   </choose>

======================
``oops`` |(extension)|
======================

This attribute is deprecated. Use ``is-parallel`` instead.

============================================
``prefix`` and ``suffix`` |(modification)|
============================================

Ordinary affixes in CSL-m are subject to a restriction: a ``prefix``
attribute may not begin with a space, and a ``suffix`` attribute may
not end with a space. Affixes on ``cs:label`` within a ``cs:names``
element, and affixes within the scope of a ``cs:date`` element are
not subject to this constraint.

The purpose of this requirement is to assure that styles are incapable
of rendering cites with stray punctuation or multiple spaces. Where
spaces are required between elements, they should be applied using
a ``delimiter`` attribute value on a ``cs:group`` element.

.. sourcecode:: xml

   <group delimiter=", ">
     <number variable="volume"/>
     <text variable="container-title"/>
   </group>

============================
``skip-words`` |(extension)|
============================

The processor carries a list of prepositions and other terms that
will not be capitalised when rendering a field with ``text-case="title"``.
Within a locale, the ``skip-words`` attribute on ``cs:style-options``
can be used to replace this list of terms with another. The attribute
value should be a comma-delimited list of words or phrases.

.. sourcecode:: xml

   <style-options skip-list="a,an,the,or,and,over,under"/>

====================================================================================
``subgroup-delimiter``,  ``subgroup-delimiter-precedes-last``, ``and`` |(extension)|
====================================================================================

The ``subgroup-delimiter`` attribute is a field-parsing hack coded
into the ``citeproc-js`` processor, enabled when the processor is run
in CSL-m mode. In a group containing *only* ``cs:text`` elements rendering
the ``publisher`` and ``publisher-place`` variables, the processor will
split the content of both fields on a semicolon. If the length of the
resulting list objects is equal, each ``publisher``/``publisher-place``
pair will be joined with the ``delimiter`` string set on the enclosing
``cs:group`` element. The composed pairs are then joined using the
``subgroup-delimiter`` value.

The ``subgroup-delimiter-precedes-last`` attribute controls the use of
the delimiter between the last and the penultimate pair in the same
manner as ``delimiter-precedes-last`` on a ``cs:name`` element. The
``and`` attribute with an argument of ``text`` or ``symbol`` may be
used on the ``cs:group`` element to join the final item with the
specified term.

.. sourcecode:: xml

   <group delimiter=" " subgroup-delimiter=", "
          subgroup-delimiter-precedes-last="always">
     <text variable="publisher"/>
     <text variable="publisher-place"/>
   </group>

or

.. sourcecode:: xml

   <group delimiter=" " subgroup-delimiter=", " 
          subgroup-delimiter-precedes-last="never"
          and="symbol">
     <text variable="publisher"/>
     <text variable="publisher-place"/>
   </group>



==============================
``suppress-min`` |(extension)|
==============================

In the MLZ extended schema, names can be suppressed in two ways.
First, using ``suppress-min`` and ``suppress-max`` with values of
``1`` or above, names rendered via a ``cs:name`` element can be
suppressed entirely when the number of individual names is at or below
a minimum, or at or above a maximum.

Second, with a value of ``0``, ``suppress-min`` can be used
on a ``cs:name`` *or* ``cs:institution`` element to suppress
*only* names of that type. See the description of ``suppress-min``
below for an example of how that works and why it might sometimes
be useful.

An example of ``suppress-min`` with a value of ``4``:

.. sourcecode:: xml

  <locale xml:lang="en">
    <terms>
      <term name="and others"></term>
    </terms>
  </locale>
  <macro name="first-position-author">
    <names variable="author">
      <name et-al-min="2" et-al-use-first="1" 
            suppress-min="4" 
            name-as-sort-order="first"/>
      <et-al term="and others"/>
    </names>
  </macro>
  <macro name="second-position-author">
    <names variable="author">
      <name et-al-min="4" et-al-use-first="1" delimiter=", "/>
    </names>
  </macro>
  <citation>
    <layout>
      <group delimiter=" / ">
        <group delimiter=" ">
          <text macro="first-position-author"/>
          <text variable="title"/>
        </group>
        <text macro="second-position-author"/>
      </group>
    </layout>
  </citation>

In the above example, an item with two authors will render as
follows:

   Stamou, A.I. Title of the Article / A.I. Stamou, I. Katsiris

An item with four authors, however, will render as follows:

   Title of the Article / A.I. Stamou et al.

--------------------------------------
``suppress-min`` with a value of ``0``
--------------------------------------

When set to zero, the ``suppress-min`` attribute is specific to the
``cs:name`` or ``cs:institution`` node only (for clarity, the
attribute with this value should always be set directly on the
affected node, rather than relying on inheritance).  The effect of the
setting is to suppress all institution or all personal names, leaving
a list of the remaining names in place.  This can be useful where
personal and institutional authors must be listed in separate places
in a citation\ |mdash|\ one example of such formatting being Rule
21.7.3 of the Bluebook 18th ed.  (applicable to U.N. reports) which
provides the following guidance and example:

    If a personal author is given along with the institutional
    author, the author [sic] should be included in a
    parenthetical at the end of the citation.

        U.N. Econ. & Soc. Council [ECOSOC], Sub-Comm. on Prevention
        of Discrimination & Prot. of Minorities, Working Group on
        Minorities, *Working Paper: Universal and Regional Mechanisms
        for Minority Protection*, |para| 17, U.N. Doc. E/CN.4/Sub.2/AC.5/1999/WP.6
        (May 5, 1999) (*prepared by* Vladimir Kartashkin).

==============================
``suppress-max`` |(extension)|
==============================

.. sourcecode:: xml

   <macro name="authors">
     <group delimiter=" ">
       <names variable="author">
         <name name-as-sort-order="all"
               et-al-min="11" et-al-use-first="3"
               and="text"/>
       </names>
       <group delimiter=" " prefix="(" suffix=")">
         <names variable="author">
           <name suppress-max="10" form="count"/>
         </names>
         <text value="co-authors"/>
       </group>
     </group>
   </macro>
   <citation>
     <layout>
       <text macro="authors"/>
     </layout>
   </citation>

In this example, an item with four authors would render as
follows:

   Doe, J, Roe, J, Noakes, R, and Snoakes, H

An item with eleven authors, on the other hand, would 
render like this:

   Doe, J, Roe, J, Noakes, R, et al. (11 co-authors)

===========================
``text-case`` |(extension)|
===========================

CSL-m adds a ``normal`` argument to the possible values of
``text-case``.  This is mainly useful when rendering a ``cs:number``
element in the scope of an element that applies a ``text-case``
transform, to prevent text content in the rendered variable from being
affected by the transform.

(The need for this attribute value is open to question.)

.. sourcecode:: xml

   <number variable="number" text-case="normal"/>

===================================
``year-range-format`` |(extension)|
===================================

%%%%%%%%%%
Conditions
%%%%%%%%%%

=========================
``context`` |(extension)|
=========================



=======================
``genre`` |(extension)|
=======================

=========================
``has-day`` |(extension)|
=========================

========================================
``has-to-month-or-season`` |(extension)|
========================================

===============================
``has-year-only`` |(extension)|
===============================

==============================
``jurisdiction`` |(extension)|
==============================

When citing primary legal resources, the form of citation is often
fixed, for ease of reference, by the issuing 
jurisdiction\ |mdash|\  "jurisdiction" referring in this case to
international rule-making bodies as well as national governments.
CSL 1.0.1 provides a ``jurisdiction`` variable, but it cannot be used
because Zotero does not currently have a corresponding field.

The particular requirement for this variable is that it be tested in a
``cs:if`` and ``cs:else-if`` condition, so that citations can be
varied according to the issuing jurisdiction. Testing of field content
is contrary to the design of CSL, so the approach of the MLZ extended
CSL schema is strictly circumscribed to address this particular need,
without opening a door to uncontrolled general testing of field
content.

The solution is in two parts, described below.

----------------------------
Jurisdiction constraint list
----------------------------

The CSL schema has been extended
in accordance with the proposed `URN:LEX`_ standard for a uniform
resource namespace for sources of law. This standard provides a
concept of "jurisdiction" that suits the requirements of legal
citation, including both national jurisdictions and international
rule-making bodies. Following `URN:LEX`_, the schema has been extended
with an explicit list of the national jurisdictions of the world, plus
selected rule-making international organizations designated by their
permanent domain name. The former are drawn from `ISO 3166 Alpha-2`_.
The latter do not yet have official sanction, as `URN:LEX`_ is still
at the proposal stage, but the list in the schema extension is
conservative, including only a few of the most stable (and widely
cited) organizations.

.. _`URN:LEX`: http://tools.ietf.org/html/draft-spinosa-urn-lex-03

.. _`ISO 3166 Alpha-2`: http://en.wikipedia.org/wiki/ISO_3166-1_alpha-2



---------------
Controlled list
---------------

The list of acceptable jurisdictions codes is coupled with an
extension of the ``cs:if`` and ``cs:else-if`` elements, providing a
``jurisdiction`` test attribute. In styles, the value set on the
attribute *must* be present in the list of acceptable jurisdiction
values. A style that uses other values is invalid.

When the ``jurisdiction`` test attribute is used, its value is
compared with the value of the ``jurisdiction`` variable on the item
being processed. If the values match, the test returns true, otherwise
false.

========================
``locale`` |(extension)|
========================

======================
``page`` |(extension)|
======================

==================================
``subjurisdictions`` |(extension)|
==================================

%%%%%
Terms
%%%%%

=============================
``unpublished`` |(extension)|
=============================

%%%%%%%%%%%%%
Locator Terms
%%%%%%%%%%%%%

=========================
``article`` |(extension)|
=========================

=========================
``Chapter`` |(extension)|
=========================

======================
``rule`` |(extension)|
======================

=========================
``Section`` |(extension)|
=========================

=======================
``title`` |(extension)|
=======================

