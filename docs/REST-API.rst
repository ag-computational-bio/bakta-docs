Bakta API-Documentation
=======================

Bakta provides a open-access REST-API that can be used to annotate own genomes programmatically. The API and its Swagger documentation can be found `here <https://api.bakta.computational.bio>`__


Endpoints
----------

.. _jobinit:

/api/v1/job/init
~~~~~~~~~~~~~~~~~~~~
The init endpoint is used to initialize a new job. Initialized jobs can be started via the :ref:`start request <jobstart>`.

HTTP-Method: **POST**

Expected request body::

   {
     "repliconTableType": "CSV",
     "name": "string"
   }
   

``repliconTableType`` describes the file format of the provided replicontable, this should be either ``CSV`` or ``TSV``. ``name`` is an arbitrary name, usually the name of the fasta input file.
   
   
Expected response body::

   {
     "uploadLinkFasta": "string",
     "uploadLinkProdigal": "string",
     "uploadLinkReplicons": "string",
     "job": {
       "secret": "string",
       "jobID": "string"
     }
   }
   
   
The response contains three S3-URLs (``uploadLinkFasta``, ``uploadLinkProdigal``, ``uploadLinkReplicons``). These URLs are pre-authenticated and can be used to upload data to the internal storage using **PUT** requests. For a detailed, step-by-step guide to use these URLs see :ref:`apiusage`. Additionally the init-request-response contains a job description with an unique ``jobID`` and a corresponding ``secret`` that are used by future request to identify and authorize the initialized job.



.. _jobstart:

/api/v1/job/start
~~~~~~~~~~~~~~~~~~~~

This endpoint is used to start a job that has been initialized via the :ref:`init request <jobinit>`. 

HTTP-Method: **POST**

Expected request body::

   {
     "job": {
       "secret": "string",
       "jobID": "string"
     },
     "config": {
       "hasProdigal": true,
       "hasReplicons": true,
       "translationalTable": 0,
       "completeGenome": true,
       "keepContigHeaders": true,
       "minContigLength": "string",
       "dermType": "UNKNOWN",
       "genus": "string",
       "species": "string",
       "strain": "string",
       "plasmid": "string",
       "locus": "string",
       "locusTag": "string"
     },
   }
   
A successful response is indicated by a `200` status code and an empty response body.


    
.. _joblist:

/api/v1/job/list
~~~~~~~~~~~~~~~~~~~~

Endpoint to query the current status of one (or more) running jobs.

HTTP-Method: **POST**

Expected request body::

   {
     "jobs": [
       {
         "secret": "string",
         "jobID": "string"
       }
     ]
   }
   
   
Response::


   {
     "jobs": [
       {
         "jobID": "string",
         "jobStatus": "INIT",
         "started": "2021-07-02T11:41:10.675Z",
         "updated": "2021-07-02T11:41:10.675Z",
         "name": "string"
       }
     ],
     "failedJobs": [
       {
         "jobID": "string",
         "jobStatus": "NOT_FOUND"
       }
     ]
   }

.. _jobresult:

/api/v1/job/result
~~~~~~~~~~~~~~~~~~~~

Endpoint to query the results of a finished job.

HTTP-Method: **POST**

Request::

   {
     "secret": "string",
     "jobID": "string"
   }
   

Response::
  
  {
    "jobID": "string",
    "ResultFiles": 
      {
        "EMBL": "S3-URL",
        "FAA": "S3-URL",
        "FAAHypothetical": "S3-URL",
        "FNA": "S3-URL",
        "GBFF": "S3-URL",
        "GFF3": "S3-URL",
        "JSON": "S3-URL",
        "TSV": "S3-URL",
        "TSVHypothetical": "S3-URL"
      },
    "started": "2021-07-14T11:10:31.838Z",
    "updated": "2021-07-14T11:10:31.838Z",
    "name": "string"
  }


.. _versionreq:

/api/v1/version
~~~~~~~~~~~~~~~~~~~~

Method that can be used to determine the internal database and Bakta version.

HTTP-METHOD: **GET**

Response::

   {
     "toolVersion": "string",
     "dbVersion": "string",
     "backendVersion": "string"
   }


.. _apiusage:

Usage
-----

To use the REST-API a three staged process must be established. The requests have to be sent in order. The overall 


Initialization
~~~~~~~~~~~~~~

Initialization of a Bakta Job must start with a :ref:`init request <jobinit>`. The API responds to this request with a unique ``jobID`` and a corresponding ``secret`` as well as three pre-authenticated S3 urls (``uploadLinkFasta``, ``uploadLinkProdigal``, ``uploadLinkReplicons``).
``jobID`` and the corresponding ``secret`` must be stored locally and are used to identify the user in following requests.

To finish the initialization procedure the three S3-URLs (``uploadLinkFasta``, ``uploadLinkProdigal``, ``uploadLinkReplicons``) must be used to upload data to the internal storage system. For this ``PUT`` requests with data as request body should be used.

``uploadLinkFasta`` should be used to upload the (fasta) sequence data for annotation.
``uploadLinkProdigal`` (optional) can be used to upload an additional prodigal training file
``uploadLinkReplicons`` (optional) should be used to upload a replicon table in ``tsv`` format that describes the provided replicons in the fasta input file

.. note::
  A ``PUT`` request to all three S3-URLs is necessary to finish the initialization procedure, optional URLs should be satisfied with a request that has an empty body.


Job start & Monitoring
~~~~~~~~~~

After initialization the specific job (identified by ``jobID`` and a corresponding ``secret``) can be scheduled via the :ref:`start request <jobstart>`.

Scheduled jobs are monitored via the :ref:`job list request <joblist>`. This requests contains a list of all monitored ``jobIDs`` and ``secrets``. The API responds with a matching JSON list of ``jobStatuses`` .
The :ref:`job list request <joblist>` should be repeated until all jobs have either the ``SUCCESSFUL`` or ``ERROR`` status. Newly scheduled jobs have the ``INIT`` status, currently running jobs ``RUNNING``.

.. note::
  The ``INIT`` status refers to a successfully started job that has not been initialized for excecution. Depending on the current load of the underlying hardware and position in the scheduling queue it may take a while before a job transitions to the ``RUNNING`` status. A failed job is always indicated by the ``ERROR`` status.
  
If multiple jobs are monitored simultaneosly the finalization procedure can be started for a job with ``SUCCESSFUL`` status while others are still ``RUNNING``. In this case the monitoring should continue in parallel for the remaining jobs and the finished job can be removed from the list.


Finalization
~~~~~~~~~~~~

Results for jobs with a ``SUCCESFUL`` status can be retrieved via the :ref:`result request <jobresult>`. The response contains a list (ResultFiles) of different file-formats with corresponding Download URLs. The result files can be retrieved with ``GET`` requests to the URL, or via a regular Webbrowser.

Currently the following file formats for results are provided:

* **EMBL**
* **FAA** 
* **FAAHypothetical**
* **FNA** 
* **GBFF** 
* **GFF3** 
* **JSON** 
* **TSV** 
* **TSVHypothetical**

More information about the structure of these output formats can be found in the `CLI Documentation <https://bakta.readthedocs.io/en/latest/BAKTA.html#output>`_

.. note::
  The ``JSON`` output format can be visualized locally via the WebUI at `https://bakta.computational.bio <https://bakta.computational.bio>`_.



