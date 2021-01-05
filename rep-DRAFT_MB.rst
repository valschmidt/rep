REP: XXX
Title: Standard Conventions for Multibeam Echosounders
Author: Val Schmidt
Status: Draft
Type: Standards Track
Content-Type: text/x-rst
Created: 22-Dec-2020
ROS-Version: 1.14.10
Post-History: 30-Aug-2002


Abstract
========

Multibeam echo sounders (MBES) are swath mapping sonar systems, that is, "push broom" type acoustic sensors common to surface and submerged marine robots for mapping and characterizing the seafloor and water column. This REP provides a standard set of conventions for encoding and interpreting data from these sensors. 


This REP provides a boilerplate or sample template for creating your
own reStructuredText REPs.  In conjunction with the content guidelines
in REP 1 [1]_, this should make it easy for you to conform your own
REPs to the format outlined below.

Note: if you are reading this REP via the web, you should first grab
the text (reStructuredText) source of this REP in order to complete
the steps below.  **DO NOT USE THE HTML FILE AS YOUR TEMPLATE!**

To get the source this (or any) REP, look at the top of the HTML page
and click on the date & time on the "Last-Modified" line.  It is a
link to the source text in the ROS repository.

If you would prefer not to use markup in your REP, please see REP 9,
"Sample Plaintext REP Template" [2]_.

This template is entirely based on the PEP 9 template by David Goodger
and Barry Warsaw.  The Author field of this document has been changed
in order to reflect reponsibility for maintenance.

Rationale
=========
There are many manufacturers of MBES and their capabilities can vary widely. The effort of this REP is to establish standard conventions for the essential common parameters for these systems, including reference frames and units.  

Introduction to Multibeam Echo Sounders (MBES) 
==========
Fundimentally, a MBES measures with each "ping" the two-way travel time to the seafloor along a fan of beams, typically downward looking, and oriented across-track to the direction of travel. Although other configurations are possible, the traditional MBES is composed of two linear arrays of transducers arranged in a "Mills Cross" configuration. In this configuration the sonar transmits one one linear array, oriented in the direction of travel of the vessel, and receives acoustic returns on a second linear array arranged orthogonal to the transmit array (in the across-track direction). 

The "beams" are synthetically generated at pre-determined angles through coherent combination of  signals recorded from individual receive elements along the transducer array. Correct generation of beam pointing angles requires realtime measurement of sound speed in the immediate vicinity of the array (more on this later). The pointing angles of each beam relative to the sonar can be fixed, but more often are varied by the system dynamically to compensate for the vessel (and therefore sonar's) pitch and roll. Beam pointing angles may have equal or varied beam-to-beam spacing, and a common arrangement is for the system to vary the angular spacing dynamically to produce equal across-track distance between measurements made on a flat seafloor. MBESs are often characterized by their "nadir" [See the note regarding this term.] transmit and receive beam widths, as these specs provide some indication of the physical size of the arrays for a given operating acoustic frequency, and also the nominal resolving capability of the system. (In a traditional installation, transmit and receive beam widths correspond to along-track and across-track beam widths.) The Nadir beam width is specified because beam widths increase for any beam pointed off nadir, (an artifact of beam steering) thus the nadir beam, provides a consistent comparison. 

> NOTE: The term *nadir* is a commonly used term in the seafloor mapping community, but can mean one of two different things. Nadir either means "in a direction orthogonal to the receive array", or "in a direction directly down toward the center of the earth." For an array that installed in the typical mapping configuration having zero roll and pitch, these two definitions are the same. But for a system oscillating with variations in pitch and roll or for a system installed in a non-traditional configuration, they may mean different things.   Unless otherwise stated otherwise, the term nadir will indicate the direction orthogonal to the array in this REP.

The calculation of depth relative to the echo-sounder from the two-way travel time measurement made along each beam has a big complicating factor, which is that the speed of sound in water is not constant. Variations in sound speed not only cause acoustic signals to speed up or slow down, but also to bend (or refract) as they propagate through the water column. This refraction must be modeled to obtain error free measurements of the seafloor. Sound speed varies with water temperature, salinity and depth (more precisely hydrostatic pressure), and so, in addition to the sound speed measured in real-time at the sonar head, MBES data is accompanied by vertical profiles of sound speed measurements made  through the water column while the survey is being conducted. These sound speed profiles are then used, either in real-time or post-processing to model the acoustic propagation of the signal. 

Some sonar systems allow an operator to load a sound speed profile into the acquisition software, which the system will then use to generate and report XYZ soundings in real-time as the data is collected. Other systems do not provide this capability, reporting only two-way travel time along each receive beam, leaving the calculation of XYZ points (and refraction correction) to other systems. Even for systems for which real-time generation of soundings is possible, it is not uncommon to re-correct for refraction in post-processing, possibly from sound speed profiles made after the sonar data was collected or in closer proximity to it. It is important to note that while it is possible to re-correct for refraction artifacts from sound speed profile data, because individual element-level data in the receive array is not retained, one cannot correct for errors in beam-pointing angle calculations that result from errors in sound speed measurement at the transducer head. None-the-less sound speed at the head is routinely recorded with the sonar data to provide some indication of the validity of the sound speed profile used for refraction correction.

In addition to two-way travel time along each beam, MBESs often record the acoustic intensity of the received signal associated with the target detect. There is no agreed upon method for measuring or reporting this value, and unfortunately each sonar system does it differently. This measurement has been of great interest for those doing seafloor characterization, but has been plagued by numerous challenges involving inconsistent reporting by sonar manufacturers, poor sonar design that produces inconsistent results, mis-treatment by scientists and engineers, and mis-understandings in the meaning of the measurement. The topic is very complex and those interested are referred to a recent report by the "Backscatter Working Group" here [1]_.  

A few terms are worth defining to prevent confusion, as outside the scientific community they are often misused and will be referenced in guidance provided below. Note that all of these are acoustic measurements which can be derived (sometimes only with great effort) from the received signal recorded by the echo-sounder, along with many other terms. 

* The term "acoustic backscatter" refers to an acoustic measurement of the ratio of the reflected acoustic energy off a surface (the seafloor) or volume (a portion of the water column) to the incident acoustic energy, when this ratio has been normalized by the ensonified area (or volume). Proper calculation of acoustic backscatter is extremely difficult, requiring corrections for the source level of the transmitted signal, transmit and receive radiation patterns (i.e. beam patterns), fixed and time varying gains applied by the system and acoustic absorption in the water column.  
* The term "acoustic target strength" is a measurement of the ratio of the reflected acoustic energy of a surface or volume to the ensonfying acoustic energy without normalization for the ensonified area or volume. 
* The term "acoustic intensity" is a measure of acoustic energy, of a  received signal. Acoustic intensity, when expressed in linear (as opposed to decibel units) is proportional to the mean square value of the received signal. 
* The term "acoustic magnitude" is proportional to acoustic pressure of the received acoustic signal. The SI unit for pressure is the Pascal. 
* Finally because these measurements range over many orders of magnitude, they are converted to deciBel Level in science and engineering. Decibels are 10 x the base 10 logarithm, of the **ratio** of a measurement to a reference value. Thus any measurement reported in decibels is meaningless without an explicit statement of the reference. The internationally agreed to reference value for underwater acoustics is 1 micro-Pascal, and one will see "120 dB re 1 micro-Pascal" in the acoustic literature. It is not uncommon in engineering to see measured voltages expressed in decibel form referenced to 1 Volt, or even 1 measurement step, where a measurement step is the maximum precision of an analog-to-digital converter. Unfortunately, it is also not uncommon for the reference value in these engineering measurements to be omitted or implied. This has historically caused no end of confusion.

Because of the complexity in calculating acoustic quantities properly, few sonar systems attempt to report them. They instead often report the received signal associated with the bottom detect (or voxel) in either decibel or linear units without corrections of any kind. This received signal level is neither acoustic backscatter nor target strength, although these terms are commonly misused to describe them. 

Finally, installation of a MBES aboard a vehicle is accompanied by a calibration procedure called a "Patch Test". A Patch Test is a collection of data sets whose collection is designed to isolate and measure specific errors in the sonar's coordinate reference frame with respect to the vessel (base_link). The results of a Patch Test are "bias" corrections to roll, pitch and yaw, which fine-tune nominal values provided by the reference frame itself. Patch Tests can also measure time delays between the navigation sonar temporal reference frames. The use of patch test values as correctors to nominal angular installation angles is a practical one, in use for decades in seafloor mapping. Because MBESs make measurements in polar coordinates, these systems are extremely sensitive to angular errors in the sensor's reference frame, as these are amplified by the range to the seafloor. Unfortunately, it is practically extremely difficult to make direct physical measurements of angular offsets to the required accuracy (generally less than 0.05 degree)   

Conventions:
=======
Units
------
ROS messages reporting the data or operation of a MBES shall adhere to REP 103 [2]_, using SI units throughout (e.g. seconds for travel time, radians for beam angles and beam widths, etc.) with the following exceptions for acoustic related values:

* Absolute acoustic measurements shall be reported in decibels re 1 micro Pascal.
* Relative acoustic values shall be reported in decibels re 1 Volt or unit of measure.   

ROS message definitions (including those defined here) shall include clear indications of whether values are absolute or relative. 


Coordinate Frame Conventions
-----
ROS messaging shall adopt REP 103 [2]_ standard for axis orientation and chirality (right handedness), with the following clarifications:

Consider a MBES with transmit and receive arrays in a Mill's Cross formation, with the long-axis of the transmit array aligned vertically and receive array orthogonal to  and centered above it. Looking down from the top. The coordinate definitions shall be defined as follows:

* x: 	Positive parallel to  the transmit array in the direction of the receive array.
* y:  Positive parallel to the receive array in the direction to the left
* z: Positive up, orthogonal as defined by the right-hand rule to the plane made by x and y.

Roll,  pitch and yaw shall indicate rotation about x, y and z axes, respectively, with positive direction in accordance with the right-hand rule. For a vessel with the sonar mounted with transmit array parallel to the fore/aft axis of the vessel and receive array across-ships, roll is then starboard down, pitch is bow down and yaw is positive counter clockwise when looking down from above. Note that yaw is to be reported using the standard ROS convention with zero along the x-axis. 

The MBES sensor reference frame shall be called ``mbes_XX`` where ``XX`` indicates a zero-padded index. 

Parameters
-----


Messages
----


ReStructuredText is offered as an alternative to plaintext REPs, to
allow REP authors more functionality and expressivity, while
maintaining easy readability in the source text.  The processed HTML
form makes the functionality accessible to readers: live hyperlinks,
styled text, tables, images, and automatic tables of contents, among
other advantages.  For an example of a REP marked up with
reStructuredText, see REP 287.


How to Use This Template
========================

To use this template you must first decide whether your REP is going
to be an Informational or Standards Track REP.  Most REPs are
Standards Track because they propose a new feature for the ROS
client libraries or standard libraries.  When in doubt, read REP 1 for details.

Once you've decided which type of REP yours is going to be, follow the
directions below.

- Make a copy of this file (``.rst`` file, **not** HTML!) and perform
  the following edits.

- Replace the "REP: 9" header with "REP: XXX" since you don't yet have
  a REP number assignment.

- Change the Title header to the title of your REP.

- Leave the Version and Last-Modified headers alone; we'll take care
  of those when we check your REP into ROS' Subversion repository.
  These headers consist of keywords ("Revision" and "Date" enclosed in
  "$"-signs) which are automatically expanded by the repository.
  Please do not edit the expanded date or revision text.

- Change the Author header to include your name, and optionally your
  email address.  Be sure to follow the format carefully: your name
  must appear first, and it must not be contained in parentheses.
  Your email address may appear second (or it can be omitted) and if
  it appears, it must appear in angle brackets.  It is okay to
  obfuscate your email address.

- If there is a mailing list for discussion of your new feature, add a
  Discussions-To header right after the Author header.  You should not
  add a Discussions-To header if the mailing list to be used is either
  ros-users@code.ros.org, or if discussions
  should be sent to you directly.  Most Informational REPs don't have
  a Discussions-To header.

- Change the Status header to "Draft".

- For Standards Track REPs, change the Type header to "Standards
  Track".

- For Informational REPs, change the Type header to "Informational".

- For Standards Track REPs, if your feature depends on the acceptance
  of some other currently in-development REP, add a Requires header
  right after the Type header.  The value should be the REP number of
  the REP yours depends on.  Don't add this header if your dependent
  feature is described in a Final REP.

- Change the Created header to today's date.  Be sure to follow the
  format carefully: it must be in ``dd-mmm-yyyy`` format, where the
  ``mmm`` is the 3 English letter month abbreviation, i.e. one of Jan,
  Feb, Mar, Apr, May, Jun, Jul, Aug, Sep, Oct, Nov, Dec.

- For Standards Track REPs, after the Created header, add a
  ROS-Version header and set the value to the next planned version
  of ROS, i.e. the one your new feature will hopefully make its
  first appearance in.  Do not use an unstable release here (e.g. 1.3.x). 
  Thus, if the last version of ROS was 1.2.2 and you're hoping to get 
  your new feature into ROS 1.4, set the header to::

      ROS-Version: 1.4

  You may also refer to a target ROS distribution, e.g. "Diamondback".

- Leave Post-History alone for now; you'll add dates to this header
  each time you post your REP.  If you posted your REP to the lists on
  August 14, 2001 and September 3, 2001, the Post-History header would
  look like::

      Post-History: 14-Aug-2001, 03-Sept-2001

  You must manually add new dates and check them in.  If you don't
  have check-in privileges, send your changes to the REP editors.

- Add a Replaces header if your REP obsoletes an earlier REP.  The
  value of this header is the number of the REP that your new REP is
  replacing.  Only add this header if the older REP is in "final"
  form, i.e. is either Accepted, Final, or Rejected.  You aren't
  replacing an older open REP if you're submitting a competing idea.

- Now write your Abstract, Rationale, and other content for your REP,
  replacing all this gobbledygook with your own text. Be sure to
  adhere to the format guidelines below, specifically on the
  prohibition of tab characters and the indentation requirements.

- Update your References and Copyright section.  Usually you'll place
  your REP into the public domain, in which case just leave the
  Copyright section alone.  Alternatively, you can use the `Open
  Publication License`__, but public domain is still strongly
  preferred.

  __ http://www.opencontent.org/openpub/

- Leave the Emacs stanza at the end of this file alone, including the
  formfeed character ("^L", or ``\f``).

- Send your REP submission to the ROS developers at ros-users@code.ros.org.


ReStructuredText REP Formatting Requirements
============================================

The following is a REP-specific summary of reStructuredText syntax.
For the sake of simplicity and brevity, much detail is omitted.  For
more detail, see `Resources`_ below.  `Literal blocks`_ (in which no
markup processing is done) are used for examples throughout, to
illustrate the plaintext markup.


General
-------

You must adhere to the Emacs convention of adding two spaces at the
end of every sentence.  You should fill your paragraphs to column 70,
but under no circumstances should your lines extend past column 79.
If your code samples spill over column 79, you should rewrite them.

Tab characters must never appear in the document at all.  A REP should
include the standard Emacs stanza included by example at the bottom of
this REP.


Section Headings
----------------

REP headings must begin in column zero and the initial letter of each
word must be capitalized as in book titles.  Acronyms should be in all
capitals.  Section titles must be adorned with an underline, a single
repeated punctuation character, which begins in column zero and must
extend at least as far as the right edge of the title text (4
characters minimum).  First-level section titles are underlined with
"=" (equals signs), second-level section titles with "-" (hyphens),
and third-level section titles with "'" (single quotes or
apostrophes).  For example::

    First-Level Title
    =================

    Second-Level Title
    ------------------

    Third-Level Title
    '''''''''''''''''

If there are more than three levels of sections in your REP, you may
insert overline/underline-adorned titles for the first and second
levels as follows::

    ============================
    First-Level Title (optional)
    ============================

    -----------------------------
    Second-Level Title (optional)
    -----------------------------

    Third-Level Title
    =================

    Fourth-Level Title
    ------------------

    Fifth-Level Title
    '''''''''''''''''

You shouldn't have more than five levels of sections in your REP.  If
you do, you should consider rewriting it.

You must use two blank lines between the last line of a section's body
and the next section heading.  If a subsection heading immediately
follows a section heading, a single blank line in-between is
sufficient.

The body of each section is not normally indented, although some
constructs do use indentation, as described below.  Blank lines are
used to separate constructs.


Paragraphs
----------

Paragraphs are left-aligned text blocks separated by blank lines.
Paragraphs are not indented unless they are part of an indented
construct (such as a block quote or a list item).


Inline Markup
-------------

Portions of text within paragraphs and other text blocks may be
styled.  For example::

    Text may be marked as *emphasized* (single asterisk markup,
    typically shown in italics) or **strongly emphasized** (double
    asterisks, typically boldface).  ``Inline literals`` (using double
    backquotes) are typically rendered in a monospaced typeface.  No
    further markup recognition is done within the double backquotes,
    so they're safe for any kind of code snippets.


Block Quotes
------------

Block quotes consist of indented body elements.  For example::

    This is a paragraph.

        This is a block quote.

        A block quote may contain many paragraphs.

Block quotes are used to quote extended passages from other sources.
Block quotes may be nested inside other body elements.  Use 4 spaces
per indent level.


Literal Blocks
--------------

..  
    In the text below, double backquotes are used to denote inline
    literals.  "``::``" is written so that the colons will appear in a
    monospaced font; the backquotes (``) are markup, not part of the
    text.  See "Inline Markup" above.

    By the way, this is a comment, described in "Comments" below.

Literal blocks are used for code samples or preformatted ASCII art. To
indicate a literal block, preface the indented text block with
"``::``" (two colons).  The literal block continues until the end of
the indentation.  Indent the text block by 4 spaces.  For example::

    This is a typical paragraph.  A literal block follows.

    ::

        for a in [5,4,3,2,1]:   # this is program code, shown as-is
            print a
        print "it's..."
        # a literal block continues until the indentation ends

The paragraph containing only "``::``" will be completely removed from
the output; no empty paragraph will remain.  "``::``" is also
recognized at the end of any paragraph.  If immediately preceded by
whitespace, both colons will be removed from the output.  When text
immediately precedes the "``::``", *one* colon will be removed from
the output, leaving only one colon visible (i.e., "``::``" will be
replaced by "``:``").  For example, one colon will remain visible
here::

    Paragraph::

        Literal block


Lists
-----

Bullet list items begin with one of "-", "*", or "+" (hyphen,
asterisk, or plus sign), followed by whitespace and the list item
body.  List item bodies must be left-aligned and indented relative to
the bullet; the text immediately after the bullet determines the
indentation.  For example::

    This paragraph is followed by a list.

    * This is the first bullet list item.  The blank line above the
      first list item is required; blank lines between list items
      (such as below this paragraph) are optional.

    * This is the first paragraph in the second item in the list.

      This is the second paragraph in the second item in the list.
      The blank line above this paragraph is required.  The left edge
      of this paragraph lines up with the paragraph above, both
      indented relative to the bullet.

      - This is a sublist.  The bullet lines up with the left edge of
        the text blocks above.  A sublist is a new list so requires a
        blank line above and below.

    * This is the third item of the main list.

    This paragraph is not part of the list.

Enumerated (numbered) list items are similar, but use an enumerator
instead of a bullet.  Enumerators are numbers (1, 2, 3, ...), letters
(A, B, C, ...; uppercase or lowercase), or Roman numerals (i, ii, iii,
iv, ...; uppercase or lowercase), formatted with a period suffix
("1.", "2."), parentheses ("(1)", "(2)"), or a right-parenthesis
suffix ("1)", "2)").  For example::

    1. As with bullet list items, the left edge of paragraphs must
       align.

    2. Each list item may contain multiple paragraphs, sublists, etc.

       This is the second paragraph of the second list item.

       a) Enumerated lists may be nested.
       b) Blank lines may be omitted between list items.

Definition lists are written like this::

    what
        Definition lists associate a term with a definition.

    how
        The term is a one-line phrase, and the definition is one
        or more paragraphs or body elements, indented relative to
        the term.


Tables
------

Simple tables are easy and compact::

    =====  =====  =======
      A      B    A and B
    =====  =====  =======
    False  False  False
    True   False  False
    False  True   False
    True   True   True
    =====  =====  =======

There must be at least two columns in a table (to differentiate from
section titles).  Column spans use underlines of hyphens ("Inputs"
spans the first two columns)::

    =====  =====  ======
       Inputs     Output
    ------------  ------
      A      B    A or B
    =====  =====  ======
    False  False  False
    True   False  True
    False  True   True
    True   True   True
    =====  =====  ======

Text in a first-column cell starts a new row.  No text in the first
column indicates a continuation line; the rest of the cells may
consist of multiple lines.  For example::

    =====  =========================
    col 1  col 2
    =====  =========================
    1      Second column of row 1.
    2      Second column of row 2.
           Second line of paragraph.
    3      - Second column of row 3.

           - Second item in bullet
             list (row 3, column 2).
    =====  =========================


Hyperlinks
----------

When referencing an external web page in the body of a REP, you should
include the title of the page in the text, with either an inline
hyperlink reference to the URL or a footnote reference (see
`Footnotes`_ below).  Do not include the URL in the body text of the
REP.

Hyperlink references use backquotes and a trailing underscore to mark
up the reference text; backquotes are optional if the reference text
is a single word.  For example::

    In this paragraph, we refer to the `ROS web site`_.

An explicit target provides the URL.  Put targets in a References
section at the end of the REP, or immediately after the reference.
Hyperlink targets begin with two periods and a space (the "explicit
markup start"), followed by a leading underscore, the reference text,
a colon, and the URL (absolute or relative)::

    .. _ROS web site: https://ros.org/

The reference text and the target text must match (although the match
is case-insensitive and ignores differences in whitespace).  Note that
the underscore trails the reference text but precedes the target text.
If you think of the underscore as a right-pointing arrow, it points
*away* from the reference and *toward* the target.

The same mechanism can be used for internal references.  Every unique
section title implicitly defines an internal hyperlink target.  We can
make a link to the Abstract section like this::

    Here is a hyperlink reference to the `Abstract`_ section.  The
    backquotes are optional since the reference text is a single word;
    we can also just write: Abstract_.

Footnotes containing the URLs from external targets will be generated
automatically at the end of the References section of the REP, along
with footnote references linking the reference text to the footnotes.

Text of the form "REP x" or "RFC x" (where "x" is a number) will be
linked automatically to the appropriate URLs.


Footnotes
---------

Footnote references consist of a left square bracket, a number, a
right square bracket, and a trailing underscore::

    This sentence ends with a footnote reference [1]_.

Whitespace must precede the footnote reference.  Leave a space between
the footnote reference and the preceding word.

When referring to another REP, include the REP number in the body
text, such as "REP 1".  The title may optionally appear.  Add a
footnote reference following the title.  For example::

    Refer to REP 1 [2]_ for more information.

Add a footnote that includes the REP's title and author.  It may
optionally include the explicit URL on a separate line, but only in
the References section.  Footnotes begin with ".. " (the explicit
markup start), followed by the footnote marker (no underscores),
followed by the footnote body.  For example::

    References
    ==========

    .. [2] REP 1, "REP Purpose and Guidelines", Conley
       (https://ros.org/reps/rep-0001.html)

If you decide to provide an explicit URL for a REP, please use this as
the URL template::

    https://ros.org/reps/rep-xxxx.html

REP numbers in URLs must be padded with zeros from the left, so as to
be exactly 4 characters wide, however REP numbers in the text are
never padded.

During the course of developing your REP, you may have to add, remove,
and rearrange footnote references, possibly resulting in mismatched
references, obsolete footnotes, and confusion.  Auto-numbered
footnotes allow more freedom.  Instead of a number, use a label of the
form "#word", where "word" is a mnemonic consisting of alphanumerics
plus internal hyphens, underscores, and periods (no whitespace or
other characters are allowed).  For example::

    Refer to REP 1 [#REP-1]_ for more information.

    References
    ==========

    .. [#REP-1] REP 1, "REP Purpose and Guidelines", Warsaw, Hylton

       https://ros.org/reps/rep-0001.html

Footnotes and footnote references will be numbered automatically, and
the numbers will always match.  Once a REP is finalized, auto-numbered
labels should be replaced by numbers for simplicity.


Images
------

If your REP contains a diagram, you may include it in the processed
output using the "image" directive::

    .. image:: diagram.png

Any browser-friendly graphics format is possible: .png, .jpeg, .gif,
.tiff, etc.

Since this image will not be visible to readers of the REP in source
text form, you should consider including a description or ASCII art
alternative, using a comment (below).


Graphs
------

ROS REPs support `mermaid diagrams`_


.. _mermaid diagrams: https://knsv.github.io/mermaid/ 

You can create flow charts: 
  
  
.. raw:: html
  
  <div class="mermaid">
  %% Example diagram
  graph LR
      A[Square Rect] -- Link text --> B((Circle))
      A --> C(Round Rect)
      B --> D{Rhombus}
      C --> D
  </div>

Gantt charts and sequences should also be possible but do not appear to be working.

Comments
--------

A comment block is an indented block of arbitrary text immediately
following an explicit markup start: two periods and whitespace.  Leave
the ".." on a line by itself to ensure that the comment is not
misinterpreted as another explicit markup construct.  Comments are not
visible in the processed document.  For the benefit of those reading
your REP in source form, please consider including a descriptions of
or ASCII art alternatives to any images you include.  For example::

     .. image:: dataflow.png

     ..
        Data flows from the input module, through the "black box"
        module, and finally into (and through) the output module.

The Emacs stanza at the bottom of this document is inside a comment.


Escaping Mechanism
------------------

reStructuredText uses backslashes ("``\``") to override the special
meaning given to markup characters and get the literal characters
themselves.  To get a literal backslash, use an escaped backslash
("``\\``").  There are two contexts in which backslashes have no
special meaning: `literal blocks`_ and inline literals (see `Inline
Markup`_ above).  In these contexts, no markup recognition is done,
and a single backslash represents a literal backslash, without having
to double up.

If you find that you need to use a backslash in your text, consider
using inline literals or a literal block instead.


Habits to Avoid
===============

Many programmers who are familiar with TeX often write quotation marks
like this::

    `single-quoted' or ``double-quoted''

Backquotes are significant in reStructuredText, so this practice
should be avoided.  For ordinary text, use ordinary 'single-quotes' or
"double-quotes".  For inline literal text (see `Inline Markup`_
above), use double-backquotes::

    ``literal text: in here, anything goes!``


Resources
=========

Many other constructs and variations are possible.  For more details
about the reStructuredText markup, in increasing order of
thoroughness, please see:

* `A ReStructuredText Primer`__, a gentle introduction.

  __ http://docutils.sourceforge.net/docs/rst/quickstart.html

* `Quick reStructuredText`__, a users' quick reference.

  __ http://docutils.sourceforge.net/docs/rst/quickref.html

* `reStructuredText Markup Specification`__, the final authority.

  __ http://docutils.sourceforge.net/spec/rst/reStructuredText.html

The processing of reStructuredText REPs is done using Docutils_.  If
you have a question or require assistance with reStructuredText or
Docutils, please `post a message`_ to the `Docutils-users mailing
list`_.  The `Docutils project web site`_ has more information.

.. _Docutils:
.. _Docutils project web site: http://docutils.sourceforge.net/
.. _post a message:
   mailto:docutils-users@lists.sourceforge.net?subject=REPs
.. _Docutils-users mailing list:
   http://docutils.sf.net/docs/user/mailing-lists.html#docutils-users


References
==========

[..1] Lamarche, G., Lurton, X. Introduction to the Special Issue “Seafloor backscatter data from swath mapping echosounders: from technological development to novel applications”. _Mar Geophys Res_  **39,** 1–3 (2018). https://doi.org/10.1007/s11001-018-9349-4

.. [1] REP 1, REP Purpose and Guidelines, Warsaw, Hylton
   (https://ros.org/reps/rep-0001.html)

.. [2] REP 9, Sample Plaintext REP Template, Warsaw
   (https://ros.org/reps/pep-0009.html)


Copyright
=========

This document has been placed in the public domain.



..
   Local Variables:
   mode: indented-text
   indent-tabs-mode: nil
   sentence-end-double-space: t
   fill-column: 70
   coding: utf-8
   End:
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExMTcwNTc5MDgsMTU3MjM3OTYxNCwxND
g4NDE5OTUwLDEzNTQ2NDI1ODIsLTE3MjE5MzMwMDEsNDc5MjY3
ODUyLDEwNzQ5NTUxMDEsMTc4MTYxNzM5NSw0ODQ3MDgzMzAsLT
EwMTE5ODU1ODgsNDg0NTEwNjY5LDUxMjU5OTcxNV19
-->