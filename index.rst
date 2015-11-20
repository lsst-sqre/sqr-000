:tocdepth: 1

Technical notes (hereafter, *technotes*) are a new documentation medium for `LSST Data Management <http://dm.lsst.org>`_ staff to publish stand-alone documents that are native to the web.
Technotes are written in reStructuredText, version controlled on GitHub, built with the same stack as DM's software documentation, and made universally citeable `digital object identifiers (DOIs) <http://www.doi.org>`_ through Zenodo_.

This document describes the Technote platform and will be updated as the platform evolves.

DM team members can create a new technote today by following the instructions at https://github.com/lsst-sqre/lsst-technote-bootstrap

We currently use two technote series. The 'DMTN' is for general use within Data Management.
The 'SQR' technote series is used for SQuaRE science quality verification and infrastructure work.

.. _Zenodo: https://zenodo.org

.. _niche:

Technotes' niche in the Data Management communication landscape
===============================================================

DM already has a myriad of communication channels.
For conversations we use HipChat in real-time, and the `Community forum`_ for asynchronous long-form discussions, notices and community engagement.
We track work on JIRA.
We document our code in software docs (the platform for which is being re-imagined by SQuaRE).
We describe the design and architecture of the DM subsystem in change-controlled design documents.
We publish articles in journals and conference proceedings where appropriate.
Finally, we have several Confluence wikis that traditionally served as a catch-all for DM documentation.
Despite this array of platforms, a use case was still unserved this technote platform was born organically to meet that need.

In SQuaRE, the Science Quality and Reliability Engineering, group, we realized that we often wanted to write stand-alone documents to describe our work.
These documents didn't fit the conversational nature of `Community forum`_ topics, nor did they qualify for DM's change-controlled design document series, or fit into academic journals or the `arXiv`.
Traditionally, Confluence was meant to fill this niche, and indeed, the wikis are well used by DM.
In practice, however, wikis have served DM poorly.
Confluence pages are hard to find and discover.
The editing and version control experience is also clumsy from the perspective of DM developers used to working on GitHub.

In the fall of 2015, we experimented with moving several DM design documents from Word documents and stored on Docushare to documents that are written in reStructuredText, built with Sphinx, hosted on GitHub, and deployed to the web with Read the Docs.
The impetus behind the transition was that documents written in plain text and hosted on GitHub were far more likely to be kept up-to-date by DM developers.

Technotes were thus built on the same technology stack as the reStructuredText-based design documents, but re-purposed for general use by DM team members.\ [#]_ 

.. [#] In fact, this technote applies equally to describing the new reStructuredText-based design document platform as it does the technote platform. The only meaningful difference is that design documents must be approved by a control board, and are ultimately deposited in LSST's Docushare.

Some of the possible applications for technotes are:

- to report the results of a project, such as a data processing or software development experiment,
- to announce a new technology, serving as a high-level wrapper to software documentation,
- to propose an architecture, possibly becoming the subject of a request for comment (RFC).

Although there is a clear need for technotes, there may inevitably be uncertainty in deciding which platform should be used for a given piece of content.
In particular, technotes are not a replacement for user-oriented documentation of software.
A technote can be written with how-to documentation in support of software experiments, once a software product is fully developed we should strive to provide proper user documentation.
That said, if the software product is backend infrastructure, a technote describing the product's architecture to DM colleagues along with 'README' files in the code may be entirely sufficient.

.. _Community forum: https://community.lsst.org
.. _arXiv: http://arxiv.org

Technote Platform Design Choices
=================================

Although technotes are still in an early, minimally-viable state, (see :ref:`implementation`), they also demonstrate several design decisions that characterize the platform.

Technotes are native to the web
-------------------------------

Although a preponderance of astronomy literature is published as PDFs, and many management documents as Word documents, those formats are archaic solutions from an era when documents were read in print.
Modern HTML, CSS and JavaScript allow for better, more innovative, and frictionless reading experiences than PDFs and Word documents can provide.
Modern readers expect to be able to search for content, click and link, and read it immediately in the browser.

Technotes are written in reStructuredText
-----------------------------------------

A fundamental requirement of the technote platform is that documents are written in a plain text format, and are then compiled into a website so that content can be written in common text editors and hosted on GitHub for collaboration.
Many plain text formats have been invented for writing, however, not all formats are ideal for technotes.
For example, LaTeX, though common in astronomy, is tied too closely to PDF output and makes little sense for a web-native syntax.
Markdown is a hugely population format among developers, in part because of its intuitive simplicity.
However, this simplicity has prompted developers to add non-standard extensions to the language to their markup to add semantics and style to content.

Instead, reStructuredText is an ideal markup format for technotes.
reStructuredText's syntax and build process was designed to be extended, and in fact, those extensions are made in DM's primary language, Python.
Because the Python community has adopted reStructuredText as its official markup language, a our technote platform can take advantage of excellent open source tools, such as the Sphinx build system and the Read the Docs documentation hosting site.
For these same reasons, reStructuredText and the Sphinx build toolchain are also used by DM's new software documentation platform.
This allows both documentation platforms to benefit from the same bespoke infrastructure development, such as SQuaRE's `documenteer`_ package.

.. _documenteer: https://github.com/lsst-sqre/documenteer

Technotes are single-page documents
-----------------------------------

This limitation is made purposefully to limit the scope of technotes, make it possible to easily print a technote in its entirely, and allow readers to their browser's search feature to navigate a document.
We are designing technote presentation to accommodate on-page navigation within a long document.

Technotes are citeable
----------------------

Since they are standalone, self-encapsulated documents, technotes can be easily archived and assigned digital object identifiers, DOIs.
DOIs allow technotes to be easily cited in scientific literature.
The benefit of a citeable-trail of DM implementation work is that LSST publications can more accurately describe DM's engineering work.
We use Zenodo_ as an archive and DOI provider, taking advantage of its GitHub integration.

Technotes are versioned
-----------------------

Unlike scientific publications that are published once, technotes can and should be updated when appropriate.
The full version history is maintained by git and published on GitHub.

.. _implementation:

Proof of concept implementation
===============================

We released a mininum-viable product for creating publishing tech notes.
Authors can create a technote by following the instructions at https://github.com/lsst-sqre/lsst-technote-bootstrap.

.. _lsst-technote-bootstrap: https://github.com/lsst-sqre/lsst-technote-bootstrap
.. _cookiecutter: http://cookiecutter.rtfd.org/
.. _Jinja2: http://jinja.pocoo.org

Project Automation
------------------

`lsst-technote-bootstrap`_ is built around the cookiecutter_ Python project.
cookiecutter_ allows code *projects* to be templated in the Jinja2_ template language.
Everything about the project can be templated: file contents, file names, and even directory structures.
By running

.. code-block:: bash

   cookiecutter https://github.com/lsst-sqre/lsst-technote-bootstrap.git

the author is prompted to answer questions that configure the document.
When that is done, the author is left with a working Sphinx-based documentation project that can be immediately built with a ``make html`` command.
This level of configuration automation is crucial to the adoption of tech notes, and :ref:`we intend to only increase this level of automation <roadmap>`.

Document Build Configuration and Metadata
-----------------------------------------

The Sphinx project prepared by `lsst-technote-bootstrap`_ appears conventional with the exception of how the Sphinx build is configured.
Most Sphinx projects have extensive ``conf.py`` files, which are ``execfile()``'d Python code that configure Sphinx and prepare the data available to document templates.
The Sphinx ``conf.py`` posed a maintenance threat to technotes: any infrastructural change to the Sphinx build system for technotes would require edits to the ``conf.py`` files of every technote and DM design document.
Our solution was to strip nearly all logic from the ``conf.py`` files, and centralize all configuration management in our documenteer_ Python package.
Now, single commits to documenteer_ are effectively deployed instantly to all technotes.

Of course, individual technotes need custom configuration, such as title and authorship information.
We keep this in a ``metadata.yaml`` file in each technote repository.
By effectively refactoring metadata out of both ``conf.py`` *and* the reStructuredText content, it is easy to develop a standardized schema for describing technotes.
See :ref:`metadata`.
Such a schema opens opportunities for indexing DM's technote library.

Deployment
----------

GitHub is the central infrastructure for hosting technotes.
The ``master`` branch is considered a live publication, but 'releases' can be made as well using git tags or the GitHub release feature.

Technotes are published on `Read the Docs`_ a free and open-source platform for publishing Sphinx-based documentation, such as technotes.
`Read the Docs`_ integrates with GitHub to rebuild the technote's webpage whenever commits are pushed to the technote's ``master`` branch on GitHub.
We serve technotes as a subdomain of ``lsst.io``, e.g., http://sqr-000.lsst.io.

.. _`Read the docs`: http://readthedocs.org

Finally, major versions of the technote can be granted DOIs.
The technote repository can be connected to Zenodo_.
When a major version of a technote is completed, a GitHub Release can be made, and the contents of the technote repository are uploaded and archived on Zenodo_.
`Following our instructions <https://github.com/lsst-sqre/lsst-technote-bootstrap/blob/master/README.rst#7-get-a-doi-with-zenodo>`_, a citeable DOI can be conveniently obtained.

.. _roadmap:

Roadmap for improvements
========================

Improved document creation and management automation
----------------------------------------------------

Improved presentation
---------------------

A document index
----------------

.. _metadata:

Metadata standard
=================
