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

.. _joblist:

/api/v1/job/list
~~~~~~~~~~~~~~~~~~~~

.. _jobresult:

/api/v1/job/result
~~~~~~~~~~~~~~~~~~~~

.. _jobdelete:

/api/v1/delete
~~~~~~~~~~~~~~~~~~~~

.. _versionreq:

/api/v1/version
~~~~~~~~~~~~~~~~~~~~


.. _apiusage:

Usage
-----

