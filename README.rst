================
 base-chameleon
================

This repository contains the base-chameleon theme for the Nikola
static site generator and the nti.nikola_chameleon template engine.
This document will describe the structure of this theme for those that
need to extend it.

Conventions
===========

- Talk about conventions: naming, macro and viewlet use, etc.

.. _all-macros:

Macros
======

Naming Macros
-------------

Macros should be named in three parts: the name of the template that
uses them (if they are used by only one template), the selector-like
path to the part of the page where they are used, and lastly their
function. So a macro used in the base template directly inside the
``<html>`` element to produce the ``<heod>`` element would be called
``base_html_head``, while one used in the ``content`` ``<div>`` inside
the ``<body>`` tag (which is nested inside the ``<html>`` element) to
produce content header would be called
``base_html_body_content_header``. (Portions of the selector may be
left out if the name would be too unwieldy, if it's still
unambiguous.)

If a macro is globally defined and meant to be used from many
templates, it may omit the template name portion.

Global Macros
-------------

These may be used in different page types.

Navigation
~~~~~~~~~~

html_body_content_breadcrumbbar
    From crumbs.macro.pt. Produces a ``<nav>`` containing an ``<ul>``
    with a ``<li>`` for each item in the ``crumbs`` variable. Used by
    gallery.tmpl.pt and listing.tmpl.pt.
html_body_content_archive_navigation
    From archive_navigation_helper.pt. Produces a ``<nav>`` containing
    a ``<ul>`` with a ``<li>`` for the previous, next, and parent
    archive. Used in list.tmpl.pt and list_post.tmpl.pt for
    IArchivePageKind pages.
html_body_content_pager
    Produces a ``<ul class="pager">`` with entries for previous and
    next pages. There is a generic version used in indexes and a
    version specialized for use in post pages.

Comments
~~~~~~~~

.. note:: These do not yet follow the naming conventions. This may be changed.

There are three macros defined for comments:

comment_link
    Produce a link to the comments for a single post. This is used in
    index.tmpl.pt, where it is re-bound for every post:
    ``post/@@macros/comment_link``.

    This will set the global variable 'need_comment_script' to the context if
    they have ever been invoked and produced output while rendering a
    template. (Since we can only have one comment system at a time,
    this is idempotent.) You can then traverse that to get the script::

        <div tal:define="global need_comment_script nothing"
             tal:repeat="post posts">
          <metal:block metal:use-macro="post/@@macros/comment_link" />
        </div>
        <metal:block metal:use-macro="comment_link_post/@@macros/comment_link_script"
                     tal:condition="need_comment_script">
comment_link_script
    Used in the body of pages that will be displaying comments. This
    produces the correct scripts.
comment_form
     Produces the HTML for viewing and adding comments.

These macros are specialized for particular comment systems as needed.
(Currently only disqus is supported.)

Math
~~~~

There are two macros defined for dealing with math rendering:

math_html_extra_head_scripts
   Insert any necessary scripts for rendering into the head. This is
   typically included in the ``extra_head`` viewlet.
math_html_body_content_scripts
   Insert any extra scripts needed for rendering. This is usually
   placed at the end of the content section, next to the
   ``comment_link_script``

There are two variants of these macros, one defined for when the
context actually needs MathJax or KaTeX, and one for when it doesn't,
to avoid bloating pages when not needed.

Viewlets
========

Describe each global viewlet manager.

Templates
=========

Templates are described in more detail in templates.rst.
