===========
 Templates
===========

This document will discuss the various templates. In general, the
templates are named the same and used for the same purposes as
described in `the Nikola documentation
<https://getnikola.com/theming.html#templates>`_.

We will mention specific templates when there is something important
to say about them.

Missing Templates
=================

Some of the default templates that come with Nikola do not exist on
disk in this theme. Usually this means that the templates were just
slight variations of each other. Here we've been able to parameterize them
into macros and viewlets so that they can be based on a common
template. This makes maintenance and extensions easier.

The templates that are missing are:

archiveindex.tmpl
  Replaced with index.tmpl.pt
tag.tmpl, author.tmpl
  Replaced with generic_post_list.pt
page.tmpl, story.tmpl
  Combined into post.tmpl.pt.

base.tmpl.pt
============

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
=====

doctype
    The complete value to use for the opening doctype statement.
    Defaults to ``<!DOCTYPE html>``.
content
    The main slot where body content will go. Most templates will fill
    this slot.

Macros Used
===========

These macros are all defined in base.macro.pt.

base_html_head
    Produces the complete ``<head>`` element. See `Header Macros`_ for
    more details.
base_html_body_nav_menu
    Inside the ``<body>``, write navigation menu.
base_html_body_page_header
    After navigation, produce any header information before the main
    body content. The default implementation does nothing.
base_html_body_content_header
    After the page header, in the section of the body devoted to
    content, before the ``content`` slot is filled. See `Content
    Header Macros`_ for more details. It also shows the search form,
    if one is defined.
base_html_body_content_footer
    After the ``content`` slot, within the section of the body devoted
    to content.

Viewlets Used
=============

base_html_body_footer
    After the content footer, but immediately before the close of the
    body. Viewlets will commonly put extra JavaScript tags here.
    Defined for the IHtmlBodyFooterViewletManager interface.



Header Macros
=============

The macro ``html_head`` is used to produce the entire ``head``
element. It delegates some of its work to other macros and viewlets.


Macros Used By HTML Head
------------------------

base_html_head_stylesheets
   This macro is used to insert the necessary style and link tags near the top of the head.
   This macro in turn delegates to up to three more macros, depending
   on the site configuration:

   base_html_head_stylesheets_bundled_cdn
       Used only when we're using bundles and a CDN.
   base_html_head_stylesheets_non_bundled_cdn
       Used only when we're not using bundles but are using a CDN.
   base_html_head_stylesheets_non_bundled_nocdn
       Used only when we're not using bundles and are not using a CDN.
   base_html_head_stylesheets_non_bundled
       Used when we're not using bundles. Always included regardless
       of the CDN status.
   base_html_head_stylesheets_extensions
       Always included regardless of other settings. This is the last
       macro executed by ``base_html_head_stylesheets.``

Viewlets Used In The HTML Head
------------------------------

extra_head
   This viewlet is registered for IHtmlHeadViewletManager. It is
   inserted just before the closing ``</head>`` tag.

Content Header Macros
=====================

The macro ``base_html_body_content_header`` uses the following three
macros, in order:

base_html_body_content_header_site_title
   Shows the site title and logo.
base_html_body_content_header_translation
   Produces a list of translations for the entire site. Delegates to
   the ``html_translations`` macro.
base_html_body_content_header_navigation_links
   Produces a ``<nav id="menu">`` containing nested ``<ul>`` for the
   defined navigation links.

index.tmpl.pt
=============

The following macros are used:

index_html_body_content_page_navigation
    Defined in pagination_helper.macro.pt. Produces a ``<div
    class="page-navigation">`` showing index page numbers.

post.tmpl.pt
============

This is also used for ``page.tmpl`` and ``story.tmpl``, with appropriate macro
definitions.

The following macros are used:

post_html_body_content_header
    The very first thing rendered inside the content slot, just above
    the actual text of the post.

    This in turn calls two macros:

    post_html_body_content_title_author_meta
        Render the title, author, date and other metadata. This uses
        the macro ``post_html_body_content_title``, which is also used
        in ``page.tmpl`` and ``story.tmpl``.
    post_html_translations
        Present a visible list of translations for this post.

    For pages and stories, this calls ``post_html_body_content_title``
    instead. They get a css class of 'pagepage' and 'storypage', respectively.

post_html_body_content_tags
    The tag list for the post. Rendered below it.
post_html_body_content_pager
    Paging for posts.
post_comment_form
    Wraps the generic ``comment_form``. Rendered below the text, tags,
    and pagers.

The default viewlet extra head used for posts includes these macros;
it is not expected that these will need to be replaced.

- post_html_head_open_graph_metadata
- post_html_head_twitter_card_information
- post_html_head_meta_translations
