..
  Technote content.

  See https://developer.lsst.io/docs/rst_styleguide.html
  for a guide to reStructuredText writing.

  Do not put the title, authors or other metadata in this document;
  those are automatically added.

  Use the following syntax for sections:

  Sections
  ========

  and

  Subsections
  -----------

  and

  Subsubsections
  ^^^^^^^^^^^^^^

  To add images, add the image file (png, svg or jpeg preferred) to the
  _static/ directory. The reST syntax for adding the image is

  .. figure:: /_static/filename.ext
     :name: fig-label
     :target: http://target.link/url

     Caption text.

   Run: ``make html`` and ``open _build/html/index.html`` to preview your work.
   See the README at https://github.com/lsst-sqre/lsst-technote-bootstrap or
   this repo's README for more info.

   Feel free to delete this instructional comment.

:tocdepth: 1
.. sectnum::

.. warning::

   Work in progress! Authors of unsolicited comments will be tracked down and
   terminated with extreme prejudice including but not limited to drone
   strikes, voodoo curses, and reciting Vogon poetry.

Overview
========

Introduction
============

Generic pipeline
================

Expanded pipeline
=================

An **expanded pipeline** is a ordered collection of transformational steps with
actual resources (configuration, log, input, and output files) *explicitly*
specified.

Requirements
------------

An expanded pipeline shall be used to:

- determine input files that are needed to be pre-stage for a given pipeline,
- create a corresponding representation of the pipeline in the format
  recognized by the workflow management system used by Batch Production
  Services (e.g.  DAX).

An expanded pipeline shall be represented by a directed, acyclic **bipartie**
graph (or **bigraph**) as its elements, nodes, can be decomposed into two
*disjoint* sets **Files** and **Tasks** and edges only connect nodes from the
opposite sets.

.. figure:: /_static/graph.png

   An example expanded pipeline.

Any information other than order of execution and data dependency shall be
provided in form of node **properties** (key-value pairs).

An expanded pipeline shall be specified in lightweight, preferably
human-readable, data-interchange format (e.g. GraphML, JSON, YAML).

Task
----

id
    Unique node id.

name
    The name of the LSST task responsible for a given transformational step,
    e.g. ``processCcd``.

arguments
    Command line arguments required to run the LSST task for provided input data
    sets as a space, e.g. ``repo --calib repo --output repo --id visit=903334
    ccd=16``.

requirements (not implemented)
    Minimal values regarding memory, CPU and disk usage required for the task
    to run.

File
----

id
    Unique node id.

lfn
    Logical file name.

urls
    Coma-separated list of physical file names on available executions sites.
    The ordering is important! It is assumed that the first file name indicates
    the file on the first execution site (see below) and so on.

sites
    Coma-separated list of execution sites.  

scientific metadata (not implemented)
    Any information required by the `data butler`__ to ingest the file to a
    dataset repository, e.g.

    - `dataset type`__,
    - `dataId`__

operational metadata (not implemented)
    Any properties the Operating Organization deems important for providing 
    operation of the service including, but not limited to:

    - size,
    - mdsum,
    - path to shared filesystem
    - path to local scratch space
    - flags either for **campaign manager** or **workflow management system**


.. note::

   Current implementation is using consecutive integers as unique node ids
   though it is not required.  Any Python hashable object can be used.

.. __: https://ldm-463.lsst.io/v/draft/index.html#butler
.. __: https://ldm-463.lsst.io/v/draft/index.html#dataset-type
.. __: https://ldm-463.lsst.io/v/draft/index.html#dataid    
