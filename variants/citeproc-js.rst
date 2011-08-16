===============================================
CSL Variant: Multilingual and Law (citeproc-js)
===============================================

This currently ragged document will become a companion to the CSL Specification,
for use by style authors making use of extensions to the CSL language available
in citeproc-js.




=====

Institution Names

Status as of 2010.05.23

This proposal has been raised on the CSL development list (xbiblio-devel), and the lead designer Bruce D'Arcus has contacted the Bibo RDF group concerning the problem of the structured description of institutional names.  The clear decision in the CSL group has been to push this issue to version 1.1, since the proposal would introduce significant addtional complexity to the CSL schema, while the basic shape of the upcoming 1.0 release was settled some time ago.  Discussion of the issue wil continue following the 1.0 release.  The result is not predetermined, but I think we can hope to see something like this in the schema for a CSL 1.1 schema release later this year.

Note: This page has been updated to reflect the entry conventions recognized by the citeproc-js processor.
Objective
Institutional names typically use different joins and follow separate conventions for limiting the names listed.  Currently, CSL makes no distinction between personal and institutional names, which makes it difficult to achieve correct formatting for several common use cases.  This is a proposal for a backward-compatible extension to the cs:names element to provide formatting appropriate to institutional names.

Support for institutional names must account for the fact that, as the examples below illustrate, institution names can consist of multiple subunits, individuals may be credited together with the institution to which they belong, unaffiliated persons may be cited together with an institution or with individuals affiliated with a credited institution.
Example use cases

Research & Pub. Policy Dep't, Nat'l Urban League
United Nations - ECLAC
ECLAC (Economic Commission for Latin America and the Carribean)
Canadian Conservation Institute (CCI)
Nolan J. Malone and others, U.S. Bureau of the Census
World Trade Organization and World Health Organization
Smith with Jones, Bureau of Sloth, Ministry of Fear
Doe et al. with Roe et al., Ministry of Fear & Noakes, Ministry of Destruction
Examples 3 and 4 render both the full form and the acronym of a single institution name, with arbitrary ordering of the two name parts.  Example 1 begins with the smallest subunit in a list of related institutions, and example 2 does the opposite.  Examples 1 and 2 are pure organizations, while example 5 is a mix of personal and institutional names.  Examples 1, 2, 3 and 4 would be entered as literal strings currently, which has obvious drawbacks.  Example 5 would require that the authorship information be spread across two variables, although all parties listed are equally authors of the resource.  Example 6 can be produced in CSL 0.8, but examples 7 and 8 cannot.
Data expectations

There are two types of possible name object: personal or organizational, which correspond to the current two-field and one-field entry modes for creator names in the current Zotero interface.  This proposal requires no changes to the Zotero UI; it is sufficient to adopt a set of conventions for the use of the existing fields, and to extend CSL to recognize hierarchical relationships between individuals and the organizations they represent, as outlined below.  This proposal takes no position on the issue of whether special measures should be taken in Zotero RDF to express the hierarchical relations implicit in the conventions below.
Personal authors

The proposal would not alter the normal entry of individual authors:



In a very simple style, the sample above might be rendered as: Doe & Roe.
Organizational names

Organizational names may include the subunits of a larger organization.  Such names would be entered as a comma-delimited list of subelements in the current Zotero interface:



In a very simple style, the sample above might be rendered as: Ministry of Fear and Large Corporation.
Authors as representatives

Single or multiple personal Names that are co-authors with an organization would be entered above the relevant organization name:



In a very simple style, the sample above might be rendered as: Clarke, Ministry of Fear and Smith & Brown, Large Corporation.
Unaffiliated authors

Authors with no affiliation would be listed after any organizational names:



In a very simple style, the sample above might be rendered as: Doe & Roe with Clarke, Ministry of Fear and Smith & Brown, Large Corporation (note the reversal of ordering in this case, with the names at the end placed at the front of the rendered list of names).
CSL proposal

The structure of mixed personal and organizational names can thus be expressed in the current Zotero UI.  In CSL, handling such structured names gracefully would require an extension to the CSL syntax.

An (optional?) cs:institution element placed (between cs:name + cs:et-al and cs:substitute), would impose separate formatting on organizational name elements and on joins between them.  The "delimiter" attribute on cs:institution would be used to join whole institution names, the subunits of which they are composed, and affiliated authors.  The "and" attribute on cs:institution would be used for the final join between two or more organizatonal authors (with their affiliated personal names).  The joins in the following string would be entirely derived from attributes on cs:institution: "R. Smith, Small Committee, Large Corporation, G. Brown, Busy Group, Active Laboratory, and S. Noakes, Powerful Ministry".
Attributes: use-first, substitute-use-first, use-last

To control the omission of names from the middle of the list of organizational subunits, the cs:institution element would use one of two attributes "use-first" or "substitute-use-first" to pick names from the front of the list, and "use-last" to pick names from the end.  The "substitute-use-first" attribute includes the leading (smallest) subunit only if no personal names are associated with the organization.

The following CSL code would format both example 1 and example 5 above, and would not interfere with existing formatting behavior for personal names.
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
Attribute: reverse-order

By convention, organizational names should be listed in "big endian" order, from the smallest to the largest organizational unit.  To provide for case such as example 2 above, a "reverse-order" attribute would be used on cs:institution:
<names variable="author" delimiter=", ">
    <name/>
    <institution 
      delimiter=" - "
      use-first="1"
      use-last="1"
      reverse-order="true"/>
</names>
Attribute: institution-parts

Organizational names are normally rendered in their long form only.  To use the short form, or combinations of the long and short form, an "institution-parts" attribute would be used on cs:institution.  This attribute would accept values of "long", "short", "short-long" and "long-short".  This attribute would be used for examples 3 and 4 above, with values of "short-long" and "long-short" respectively.  A value of "short" behaves in the same way as form="short" in other contexts within CSL, using the short form if it is available, and falling back to the long form otherwise.
Element: cs:institution-part

One or more cs:institution-part elements could be used to control the formatting of the long and short forms of organizational names.  Like cs:name-part, these elements are unordered, and affect only the formatting of the target name element, specified as on the name-part element with a required "name" attribute.  Unlike cs:name-part, the cs:institution-part element allows affix attributes to be set.
Attribute: if-short

In example 3, the parentheses should be included only if a short form of the institution name is available.  The "if-short" attribute, available on cs:institution-part only when applied to the long form, makes the formatting in the element conditional on the existence of a short form of the name.  The following CSL would render example 3:

<names variable="author">
    <name/>
    <institution institution-parts="short-long">
        <institution-part name="long" if-short="true" prefix=" (" suffix=")"/>
    </institution>
</names>
Element: with

Unaffiliated personal names would be joined to a following organizational name using an implicit localizable term "with".  Styling of this term would be permitted through an optional cs:with element, placed immediately above the cs:institution element:
<names variable="author">
    <name/>
    <with font-style="italic" prefix=" " suffix=" "/>
    <institution institution-parts="short-long">
        <institution-part name="long" if-short="true" prefix=" (" suffix=")"/>
    </institution>
</names>
Simple style example

The simple style used in the illustrated examples above would look like this in CSL:

<names variable="author">
    <name form="short" and="symbol" delimiter=", "/>
    <institution use-last="1" and="text" delimiter=", "/>
</names>
