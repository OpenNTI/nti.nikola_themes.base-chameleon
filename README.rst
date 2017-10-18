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
templates, it will omit the template name portion.

Macros
======

Describe each global macro if not otherwise described.

html_body_content_breadcrumbbar
    From crumbs.macro.pt. Produces a ``<nav>`` containing an ``<ul>``
    with a ``<li>`` for each item in the ``crumbs`` variable. Used by
    gallery.tmpl.pt and listing.tmpl.pt.

Viewlets
========

Describe each global viewlet manager.

Templates
=========

Describe each template.

base.tmpl.pt
------------

This template is the main "layout" template. It defines one macro
called "base". Other templates are generally expected to use this like
so::

  <html metal:use-macro="context/@@base.tmpl/index/macros/base"
        xmlns:tal="http://xml.zope.org/namespaces/tal"
        xmlns:i18n="http://xml.zope.org/namespaces/i18n"
	    xmlns:metal="http://xml.zope.org/namespaces/metal">
     ...
  </html>

Slots
~~~~~

doctype
    The complete value to use for the opening doctype statement.
    Defaults to ``<!DOCTYPE html>``.
content
    The main slot where body content will go. Most templates will fill
    this slot.

Macros Used
~~~~~~~~~~~

base_html_head
    Produces the complete ``<head>`` element.
base_html_body_nav_menu
    Inside the ``<body>``, write navigation menu.
base_html_body_page_header
    After navigation, produce any header information before the main
    body content.
base_html_body_content_header
    After the page header, in the section of the body devoted to
    content, before the ``content`` slot is filled.
base_html_body_content_footer
    After the ``content`` slot, within the section of the body devoted
    to content.

Viewlets Used
~~~~~~~~~~~~~

base_html_body_footer
    After the content footer, but immediately before the close of the
    body. Viewlets will commonly put extra JavaScript tags here.
