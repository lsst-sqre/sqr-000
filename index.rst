:tocdepth: 1

Technical notes (hereafter, *technotes*) are web-native documents that give `LSST Data Management (DM) <http://dm.lsst.org>`_ staff a standardized, yet flexible, platform to communicate their work.
Technotes are written in reStructuredText_, version controlled on GitHub_, built with the same stack as DM's software documentation, and made universally citeable with `digital object identifiers (DOIs) <https://en.wikipedia.org/wiki/Digital_object_identifier>`_ provided through Zenodo_.

This document describes the Technote platform and will be updated as the platform evolves.

DM team members can create a new technote today by following the instructions at https://github.com/lsst-sqre/lsst-technote-bootstrap.

We currently use two technote series. The :abbr:`DMTN (Data Management Technote)` is for general use within Data Management.
The :abbr:`SQR (SQuaRE Technote)` technote series is used for SQuaRE science quality verification and infrastructure work.

.. _niche:

Technotes' Niche in the Data Management Communication Landscape
===============================================================

DM already has a myriad of communication channels.
For conversations we use HipChat in real-time, and the `Community forum`_ for asynchronous long-form discussions, notices and community engagement.
We track work on JIRA.
We document our code in software docs (the platform for which is being re-imagined by SQuaRE).
We describe the design and architecture of the DM subsystem in change-controlled design documents.
We publish articles in journals and conference proceedings where appropriate.
Finally, we have several Confluence_ wikis that traditionally served as a catch-all for DM documentation.
Despite this array of platforms, a use case was still unserved and this technote platform was conceived to meet that need.

In SQuaRE, the Science Quality and Reliability Engineering group, we realized that we often wanted to write stand-alone documents to describe our work.
These documents didn't fit the conversational nature of `Community forum`_ topics, nor did they qualify for DM's change-controlled design document series, or fit into academic journals or the arXiv_.
Traditionally, Confluence_ was meant to fill this niche, and indeed, the wikis are well used by DM.
In practice, however, wikis have served DM poorly.
For readers, Confluence pages are hard to find and discover.
For authors, the editing and version control experience is also clumsy from the perspective of DM developers used to working on GitHub_.

In the fall of 2015, we experimented with moving several DM design documents from Word documents stored on Docushare_ to documents that are written in reStructuredText_, built with Sphinx_, hosted on GitHub_, and deployed to the web with `Read the Docs`_.
The impetus behind the transition was that documents written in plain text and hosted on GitHub were far more likely to be kept up-to-date by DM developers.

Technotes were thus built on the same technology stack as the reStructuredText_-based design documents, but re-purposed for general use by DM team members.\ [#]_ 

.. [#] In fact, this technote applies equally to describing the new reStructuredText_-based design document platform as it does the technote platform. The only meaningful difference is that design documents must be approved by a control board, and are ultimately deposited in LSST's Docushare_.

Some of the possible applications for technotes are:

- to report the results of a project, such as a data processing or software development experiment,
- to announce a new technology, serving as a high-level overview complementing software documentation,
- to propose an architecture, possibly becoming the subject of a request for comment (RFC).

Technote Platform Design Choices
=================================

Although technotes are still in an early, minimally-viable state, (see :ref:`implementation`), they also demonstrate several design decisions that characterize the platform.

Technotes are native to the web
-------------------------------

Although a preponderance of astronomy literature is published as PDFs, and many management documents as Word documents, those formats are built on the assumption that documents are read in print.
These platforms are out of step with modern expectations that documents should be Googleable, linkable, and instantly readable in the browser.
Modern HTML, CSS, JavaScript, and webfonts are delivering reading experiences that are beginning to surpass the printed page.
Technotes are built for this modern communication era.

Technotes are written in reStructuredText
-----------------------------------------

A fundamental requirement of the technote platform is that documents are written in a plain text format, and are then compiled into a website. This way, content is written in common text editors and hosted on GitHub for collaboration (the same workflow we already use for source code).
Many plain text formats have been invented for writing, however, not all formats are ideal for technotes.
For example, LaTeX, though common in astronomy, is tied too closely to PDF output and was never meant to be a web-native markup.
`Markdown <http://daringfireball.net/projects/markdown/>`_ is a hugely popular format among developers, in part because of its intuitive simplicity.
However, this simplicity has prompted developers to `add non-standard extensions to the format <http://commonmark.org>`_ to add semantics and style to content.

Instead, reStructuredText_ is an ideal markup format for technotes.
reStructuredText_\ 's syntax and build process was designed to be extended, and in fact, those extensions are made in DM's primary language, Python.
Because the Python community has adopted reStructuredText_ as its official markup language (including the scientific/astronomy Python community, led by `NumPy <http://www.numpy.org>`_, `SciPy <http://www.scipy.org>`_, `Matplotlib <http://matplotlib.org>`_ and `AstroPy <http://www.astropy.org>`_), our technote platform can take advantage of excellent open source tools, such as the Sphinx_ build system and the `Read the Docs`_ documentation hosting site.
For these same reasons, reStructuredText_ and the Sphinx_ build toolchain are also used by DM's new software documentation platform.
This allows both documentation platforms to benefit from the same bespoke infrastructure development, such as SQuaRE's documenteer_ package.

Technotes are single-page documents
-----------------------------------

This limitation is made purposefully to limit the scope of technotes, make it possible to easily print a technote in its entirely, and allow readers to use their browser's search feature to navigate a document.
We are designing technote presentation to accommodate on-page navigation within a long document.

Technotes are citeable
----------------------

Since they are standalone, self-encapsulated documents, technotes can be easily archived and assigned digital object identifiers, DOIs.
DOIs allow technotes to be easily cited in scientific literature.
The benefit of a citeable-trail of DM documentation is that LSST publications can more accurately describe DM's engineering work.
We use Zenodo_ as an archive and DOI provider, taking advantage of its GitHub integration.

Technotes are versioned
-----------------------

Technotes take a software development-inspired approach to publishing by allowing technotes to be updated in-place when appropriate.
The full version history is maintained by git and published on GitHub.
With GitHub's Zenodo_ integration, new releases are archived through Zenodo and given their own DOI (while also being linked to other versions).

.. _implementation:

Proof of Concept Implementation
===============================

We released a tool for creating and publishing technotes.
Authors can create a technote by following the instructions at https://github.com/lsst-sqre/lsst-technote-bootstrap.

Project automation
------------------

`lsst-technote-bootstrap`_ is built around the cookiecutter_ Python project.
cookiecutter_ allows code *projects* to be templated in the Jinja2_ template language.
Everything about the project can be templated: file contents, file names, and even directory structures.
By running

.. code-block:: bash

   cookiecutter https://github.com/lsst-sqre/lsst-technote-bootstrap.git

the author is prompted to answer questions that configure the document.
When that is done, the author is left with a working Sphinx_-based documentation project that can be immediately built with a ``make html`` command.
This level of configuration automation is crucial to the adoption of tech notes, and :ref:`we intend to only increase this level of automation <roadmap>`.

Document build configuration and metadata
-----------------------------------------

The Sphinx_ project prepared by `lsst-technote-bootstrap`_ appears conventional with the exception of how the Sphinx_ build is configured.
Most Sphinx_ projects have extensive :file:`conf.py` files, which are ``execfile()``'d Python code that configure Sphinx_ and prepare the data available to document templates.
The Sphinx_ :file:`conf.py` posed a maintenance threat to technotes: any infrastructural change to the Sphinx_ build system for technotes would require edits to the :file:`conf.py` files of every technote and DM design document.
Our solution was to strip nearly all logic from the :file:`conf.py` files, and centralize all configuration management in our documenteer_ Python package.
Now, single commits to documenteer_ are effectively deployed instantly to all technotes.

Of course, individual technotes need custom configuration, such as title and authorship information.
We keep this in a :file:`metadata.yaml` file in each technote repository.
By effectively refactoring metadata out of both :file:`conf.py` *and* the reStructuredText_ content, it is easy to develop a standardized schema for describing technotes.
See :ref:`metadata`.
Such a schema opens opportunities for indexing DM's technote library.

Deployment
----------

GitHub_ is the central infrastructure for hosting technotes.
The ``master`` branch is considered a live publication, but 'releases' can be made as well using git tags or the GitHub Release feature.

Technotes are published on `Read the Docs`_, a free and open-source platform for publishing Sphinx_-based documentation, such as technotes.
`Read the Docs`_ integrates with GitHub_ to rebuild the technote's webpage whenever commits are pushed to the technote's ``master`` branch on GitHub_.
We serve technotes as a subdomain of ``lsst.io``, e.g., http://sqr-000.lsst.io.

Finally, major versions of the technote can be granted DOIs.
The technote repository can be connected to Zenodo_.
When a major version of a technote is completed, a GitHub Release can be made, and the contents of the technote repository are uploaded and archived on Zenodo_.
`Following our instructions <https://github.com/lsst-sqre/lsst-technote-bootstrap/blob/master/README.rst#7-get-a-doi-with-zenodo>`_, a citeable DOI can be conveniently obtained.

.. _roadmap:

Roadmap for improvements
========================

Improved document creation and management automation
----------------------------------------------------

Although lsst-technote-bootstrap_ automates report creation, there are still many facets of technote authorship that would benefit from automation:

#. additional automation of technote configuration, beyond what cookiecutter_ provides (such as dynamic date suggestions)
#. creation of a GitHub_ repository
#. creation and configuration of a `Read The Docs`_ project
#. provisioning of an ``lsst.io`` domain
#. reStructuredText_ and metadata linting (using `Travis CI <https://travis-ci.org>`_ testing)
#. synchronization of metadata version tags and revision dates with git history
#. automatic local builds and browser updates (e.g., `Browsersync <http://www.browsersync.io>`_)
#. automation of releases and procurement of DOIs (leveraging :file:`metadata.yaml` to automate the technote's deposition on Zenodo_)

This likely demands a command line application to manage technotes, which would incorporate lsst-technote-bootstrap_.
Likely the most challenging aspect will be automating the creation of a `Read the Docs`_ project, since project creation is not part of `RTD's API <http://docs.readthedocs.org/en/latest/api.html>`_.

Improved presentation
---------------------

Technotes are currently published with Read the Doc's default theme (including minor additions to incorporate metadata from :file:`metadata.yaml`).
A new HTML/CSS theme is needed to

- establish a visual identity for DM documents
- provide allowances for navigation in long single page documents
- add facilities for styling elements created by an extended reStructuredText_ language (rather than retrofitting an existing theme)
- improve layout for print

Extensions to reStructuredText
------------------------------

DM authors need a richer reStructuredText_ language for technical writing.
One need is to have citations and bibliographies of the same quality as are possible with LaTeX and `natbib <http://ctan.org/pkg/natbib>`_.
We can achieve this by developing `Sphinx extensions <http://sphinx-doc.org/extdev/index.html>`_ within the documenteer_ package.
Development work done here will also benefit DM's software documentation.

A document index
----------------

From experience with Docushare_ and the Confluence_ wikis, we learnt that documentation can be easily buried if not indexed from a central, authoritative, reliable and highly visible place.
We need to provide a documentation index for DM, likely as part of http://dm.lsst.org.
The page could be automatically updated by leveraging the GitHub_ API and individual documents' :file:`metadata.yaml` information.
Ideally, the index would provide facilities for filtering or searching.

.. _metadata:

Metadata Standard
=================

Here we document the available keys in the :file:`metadata.yaml` schema.

series:
   A string identifying the technote series.
   Possible values are ``'DMTN'`` for DM Technotes and ``'SQR'`` for SQuaRE Technotes.
   Existing change-controlled document series can also be used, such as ``'LDM'``.

   Example:

   .. code-block:: yaml

      series: 'SQR'

series_number:
   Serial number of the document, as a string.
   For the :abbr:`DMTN (Data Management Technote)` and :abbr:`SQR (SQuaRE Technote)` series we use three digit serial numbers (with leading zeros).

   Example:

   .. code-block:: yaml

      serial_number: '000'

doc_id:
   **Planned for deprecation.** This is a string that joins ``series`` and ``serial_number`` with a dash.
    
   Example:

   .. code-block:: yaml

      doc_id: 'SQR-000'

authors:
   Author names, ordered as a list.
   Each author name should be formatted as 'First Last'.

   Example:

   .. code-block:: yaml

      authors:
          - 'Jonathan Sick'
          - 'Frossie Economou'

   An extended syntax for the ``authors`` key is planned.

version:
   Use semantic versioning, e.g., '1.0', including '.dev', as necessary.
   This version string should correspond to the git tag when the document is published on Zenodo_.

   Example of a '1.0' release:

   .. code-block:: yaml

      version: '1.0'

   Example of an early development version:

   .. code-block:: yaml

      version: '0.1.dev'

   This metadata may be replaced by tooling that uses git tags.

doi:
   Digital Object Identifier (DOI).
   Keep this DOI updated as new releases are pushed to Zenodo_.

   Example:

   .. code-block:: yaml

      doi: '10.5281/zenodo.12345'

   This field can be left commented (or omitted) if a DOI is unavailable:

   .. code-block:: yaml

      # doi: '10.5281/zenodo.#####'

last_revised:
   Document release date, as ``'YYYY-MM-DD'``.

   Example:

   .. code-block:: yaml

      '2015-11-18'

   This metadata may be replaced by tooling that uses git history.

copyright:
   Copyright statement.

   Example:

   .. code-block:: yaml

      copyright: '2015, AURA/LSST'

Planned metadata extensions
---------------------------

We plan to add the following fields to the :file:`metadata.yaml` schema.
These metadata fields are not currently in use, and are liable change prior to implementation.

description:
   A short 1-2 sentence description for document indices.

abstract:
   An abstract, if available.

   Example:

   .. code-block:: yaml

      abstract: >
                Write your paragraph
                here with multiple lines.
      
                You can have multiple paragraphs too.

url:
   The canonical URL where the document is published by `Read the Docs`_.

   Example:

   .. code-block:: yaml

      url: 'http://sqr-000.github.io'

docushare_url:
   If a canonical version of the document is archived in Docushare, the URL can be provided.

   Example:

   .. code-block:: yaml

      docushare_url: 'https://docushare.lsstcorp.org/docushare/{{ path }}'

github_url:
   The document's URL on GitHub_.

   Example:

   .. code-block:: yaml

      github_url: 'https://github.com/lsst-sqre/sqr-000'

deprecated:
   This field can be added if the document has been superseded.
   The deprecation notice may contain several fields, for example:

   .. code-block:: yaml

      deprecated:
         date: 'YYYY-MM-DD'
         superseded_by: 'http://{{url of new doc}}'

changes:
   A changelog.
   The DM design documents currently embed change tables in the content, but this would be more useful as independent metadata.

   .. code-block:: yaml

      changes:
         -
           tag: v1.0
           notes: 'First version'
         -
           tag: v2.0
           notes: 'Second version'


Leveraging ORCID for Author Information
---------------------------------------

The current authorship metadata is limited; the ``authors`` key is an ordered list of author names.
A better way to annotate authorship metadata would be through ORCID_ iDs, which unique identify researchers.
ORCID_ uses those identifiers to connect people to their work.

A possible revised syntax for declaring authorship metadata would be

.. code-block:: yaml

   authors:
     -
       name: Jonathan Sick
       orcid: 0000-0003-3001-676X
     -
       name: Second Author
       orcid: ####-####-####-####

ORCID_ iD integration would be used to improve the Zenodo_ submission process.

Acknowlegements
===============

J. S. would like to thank Frossie Economou, Tim Jenness and Josh Hoblitt for authoring early technotes and providing feedback on the platform.
Tim Jenness and JMatt Peterson provided valuable editorial feedback on this technote.

.. _Zenodo: https://zenodo.org
.. _GitHub: https://github.com
.. _Community forum: https://community.lsst.org
.. _arXiv: http://arxiv.org
.. _documenteer: https://github.com/lsst-sqre/documenteer
.. _lsst-technote-bootstrap: https://github.com/lsst-sqre/lsst-technote-bootstrap
.. _cookiecutter: http://cookiecutter.rtfd.org/
.. _Jinja2: http://jinja.pocoo.org
.. _ORCID: http://orcid.org/
.. _reStructuredText: http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html
.. _Sphinx: http://sphinx-doc.org
.. _Read the Docs: https://readthedocs.org
.. _Docushare: https://docushare.lsstcorp.org/docushare/dsweb
.. _Confluence: https://confluence.lsstcorp.org/
