 .. Licensed to the Apache Software Foundation (ASF) under one
    or more contributor license agreements.  See the NOTICE file
    distributed with this work for additional information
    regarding copyright ownership.  The ASF licenses this file
    to you under the Apache License, Version 2.0 (the
    "License"); you may not use this file except in compliance
    with the License.  You may obtain a copy of the License at

 ..   http://www.apache.org/licenses/LICENSE-2.0

 .. Unless required by applicable law or agreed to in writing,
    software distributed under the License is distributed on an
    "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
    KIND, either express or implied.  See the License for the
    specific language governing permissions and limitations
    under the License.


Changelog
---------

3.0.0
.....

Breaking changes
~~~~~~~~~~~~~~~~

Integration with the ``apache.beam`` provider
`````````````````````````````````````````````

In 3.0.0 version of the provider we've changed the way of integrating with the ``apache.beam`` provider.
The previous versions of both providers caused conflicts when trying to install them together
using PIP > 20.2.4. The conflict is not detected by PIP 20.2.4 and below but it was there and
the version of ``Google BigQuery`` python client was not matching on both sides. As the result, when
both ``apache.beam`` and ``google`` provider were installed, some features of the ``BigQuery`` operators
might not work properly. This was cause by ``apache-beam`` client not yet supporting the new google
python clients when ``apache-beam[gcp]`` extra was used. The ``apache-beam[gcp]`` extra is used
by ``Dataflow`` operators and while they might work with the newer version of the ``Google BigQuery``
python client, it is not guaranteed.

This version introduces additional extra requirement for the ``apache.beam`` extra of the ``google`` provider
and symmetrically the additional requirement for the ``google`` extra of the ``apache.beam`` provider.
Both ``google`` and ``apache.beam`` provider do not use those extras by default, but you can specify
them when installing the providers. The consequence of that is that some functionality of the ``Dataflow``
operators might not be available.

Unfortunately the only ``complete`` solution to the problem is for the ``apache.beam`` to migrate to the
new (>=2.0.0) Google Python clients.

This is the extra for the ``google`` provider:

.. code-block:: python

        extras_require={
            ...
            'apache.beam': ['apache-airflow-providers-apache-beam', 'apache-beam[gcp]'],
            ....
        },

And likewise this is the extra for the ``apache.beam`` provider:

.. code-block:: python

        extras_require={'google': ['apache-airflow-providers-google', 'apache-beam[gcp]']},

You can still run this with PIP version <= 20.2.4 and go back to the previous behaviour:

.. code-block:: shell

  pip install apache-airflow-providers-google[apache.beam]

or

.. code-block:: shell

  pip install apache-airflow-providers-apache-beam[google]

But be aware that some ``BigQuery`` operators functionality might not be available in this case.

2.2.0
.....

Features
~~~~~~~~

* ``Adds 'Trino' provider (with lower memory footprint for tests) (#15187)``
* ``update remaining old import paths of operators (#15127)``
* ``Override project in dataprocSubmitJobOperator (#14981)``
* ``GCS to BigQuery Transfer Operator with Labels and Description parameter (#14881)``
* ``Add GCS timespan transform operator (#13996)``
* ``Add job labels to bigquery check operators. (#14685)``
* ``Use libyaml C library when available. (#14577)``
* ``Add Google leveldb hook and operator (#13109) (#14105)``

Bug fixes
~~~~~~~~~

* ``Google Dataflow Hook to handle no Job Type (#14914)``

2.1.0
.....

Features
~~~~~~~~

* ``Corrects order of argument in docstring in GCSHook.download method (#14497)``
* ``Refactor SQL/BigQuery/Qubole/Druid Check operators (#12677)``
* ``Add GoogleDriveToLocalOperator (#14191)``
* ``Add 'exists_ok' flag to BigQueryCreateEmptyTable(Dataset)Operator (#14026)``
* ``Add materialized view support for BigQuery (#14201)``
* ``Add BigQueryUpdateTableOperator (#14149)``
* ``Add param to CloudDataTransferServiceOperator (#14118)``
* ``Add gdrive_to_gcs operator, drive sensor, additional functionality to drive hook  (#13982)``
* ``Improve GCSToSFTPOperator paths handling (#11284)``

Bug Fixes
~~~~~~~~~

* ``Fixes to dataproc operators and hook (#14086)``
* ``#9803 fix bug in copy operation without wildcard  (#13919)``

2.0.0
.....

Breaking changes
~~~~~~~~~~~~~~~~

Updated ``google-cloud-*`` libraries
````````````````````````````````````

This release of the provider package contains third-party library updates, which may require updating your
DAG files or custom hooks and operators, if you were using objects from those libraries.
Updating of these libraries is necessary to be able to use new features made available by new versions of
the libraries and to obtain bug fixes that are only available for new versions of the library.

Details are covered in the UPDATING.md files for each library, but there are some details
that you should pay attention to.


+-----------------------------------------------------------------------------------------------------+----------------------+---------------------+---------------------------------------------------------------------------------------------------------------------------------------+
| Library name                                                                                        | Previous constraints | Current constraints | Upgrade Documentation                                                                                                                 |
+=====================================================================================================+======================+=====================+=======================================================================================================================================+
| `google-cloud-automl <https://pypi.org/project/google-cloud-automl/>`_                              | ``>=0.4.0,<2.0.0``   | ``>=2.1.0,<3.0.0``  | `Upgrading google-cloud-automl <https://github.com/googleapis/python-automl/blob/master/UPGRADING.md>`_                               |
+-----------------------------------------------------------------------------------------------------+----------------------+---------------------+---------------------------------------------------------------------------------------------------------------------------------------+
| `google-cloud-bigquery-datatransfer <https://pypi.org/project/google-cloud-bigquery-datatransfer>`_ | ``>=0.4.0,<2.0.0``   | ``>=3.0.0,<4.0.0``  | `Upgrading google-cloud-bigquery-datatransfer <https://github.com/googleapis/python-bigquery-datatransfer/blob/master/UPGRADING.md>`_ |
+-----------------------------------------------------------------------------------------------------+----------------------+---------------------+---------------------------------------------------------------------------------------------------------------------------------------+
| `google-cloud-datacatalog <https://pypi.org/project/google-cloud-datacatalog>`_                     | ``>=0.5.0,<0.8``     | ``>=3.0.0,<4.0.0``  | `Upgrading google-cloud-datacatalog <https://github.com/googleapis/python-datacatalog/blob/master/UPGRADING.md>`_                     |
+-----------------------------------------------------------------------------------------------------+----------------------+---------------------+---------------------------------------------------------------------------------------------------------------------------------------+
| `google-cloud-dataproc <https://pypi.org/project/google-cloud-dataproc/>`_                          | ``>=1.0.1,<2.0.0``   | ``>=2.2.0,<3.0.0``  | `Upgrading google-cloud-dataproc <https://github.com/googleapis/python-dataproc/blob/master/UPGRADING.md>`_                           |
+-----------------------------------------------------------------------------------------------------+----------------------+---------------------+---------------------------------------------------------------------------------------------------------------------------------------+
| `google-cloud-kms <https://pypi.org/project/google-cloud-kms>`_                                     | ``>=1.2.1,<2.0.0``   | ``>=2.0.0,<3.0.0``  | `Upgrading google-cloud-kms <https://github.com/googleapis/python-kms/blob/master/UPGRADING.md>`_                                     |
+-----------------------------------------------------------------------------------------------------+----------------------+---------------------+---------------------------------------------------------------------------------------------------------------------------------------+
| `google-cloud-logging <https://pypi.org/project/google-cloud-logging/>`_                            | ``>=1.14.0,<2.0.0``  | ``>=2.0.0,<3.0.0``  | `Upgrading google-cloud-logging <https://github.com/googleapis/python-logging/blob/master/UPGRADING.md>`_                             |
+-----------------------------------------------------------------------------------------------------+----------------------+---------------------+---------------------------------------------------------------------------------------------------------------------------------------+
| `google-cloud-monitoring <https://pypi.org/project/google-cloud-monitoring>`_                       | ``>=0.34.0,<2.0.0``  | ``>=2.0.0,<3.0.0``  | `Upgrading google-cloud-monitoring <https://github.com/googleapis/python-monitoring/blob/master/UPGRADING.md)>`_                      |
+-----------------------------------------------------------------------------------------------------+----------------------+---------------------+---------------------------------------------------------------------------------------------------------------------------------------+
| `google-cloud-os-login <https://pypi.org/project/google-cloud-os-login>`_                           | ``>=1.0.0,<2.0.0``   | ``>=2.0.0,<3.0.0``  | `Upgrading google-cloud-os-login <https://github.com/googleapis/python-oslogin/blob/master/UPGRADING.md>`_                            |
+-----------------------------------------------------------------------------------------------------+----------------------+---------------------+---------------------------------------------------------------------------------------------------------------------------------------+
| `google-cloud-pubsub <https://pypi.org/project/google-cloud-pubsub>`_                               | ``>=1.0.0,<2.0.0``   | ``>=2.0.0,<3.0.0``  | `Upgrading google-cloud-pubsub <https://github.com/googleapis/python-pubsub/blob/master/UPGRADING.md>`_                               |
+-----------------------------------------------------------------------------------------------------+----------------------+---------------------+---------------------------------------------------------------------------------------------------------------------------------------+
| `google-cloud-tasks <https://pypi.org/project/google-cloud-tasks>`_                                 | ``>=1.2.1,<2.0.0``   | ``>=2.0.0,<3.0.0``  | `Upgrading google-cloud-task <https://github.com/googleapis/python-tasks/blob/master/UPGRADING.md>`_                                  |
+-----------------------------------------------------------------------------------------------------+----------------------+---------------------+---------------------------------------------------------------------------------------------------------------------------------------+

The field names use the snake_case convention
`````````````````````````````````````````````

If your DAG uses an object from the above mentioned libraries passed by XCom, it is necessary to update the
naming convention of the fields that are read. Previously, the fields used the CamelSnake convention,
now the snake_case convention is used.

**Before:**

.. code-block:: python

    set_acl_permission = GCSBucketCreateAclEntryOperator(
        task_id="gcs-set-acl-permission",
        bucket=BUCKET_NAME,
        entity="user-{{ task_instance.xcom_pull('get-instance')['persistenceIamIdentity']"
        ".split(':', 2)[1] }}",
        role="OWNER",
    )


**After:**

.. code-block:: python

    set_acl_permission = GCSBucketCreateAclEntryOperator(
        task_id="gcs-set-acl-permission",
        bucket=BUCKET_NAME,
        entity="user-{{ task_instance.xcom_pull('get-instance')['persistence_iam_identity']"
        ".split(':', 2)[1] }}",
        role="OWNER",
    )


Features
~~~~~~~~

* ``Add Apache Beam operators (#12814)``
* ``Add Google Cloud Workflows Operators (#13366)``
* ``Replace 'google_cloud_storage_conn_id' by 'gcp_conn_id' when using 'GCSHook' (#13851)``
* ``Add How To Guide for Dataflow (#13461)``
* ``Generalize MLEngineStartTrainingJobOperator to custom images (#13318)``
* ``Add Parquet data type to BaseSQLToGCSOperator (#13359)``
* ``Add DataprocCreateWorkflowTemplateOperator (#13338)``
* ``Add OracleToGCS Transfer (#13246)``
* ``Add timeout option to gcs hook methods. (#13156)``
* ``Add regional support to dataproc workflow template operators (#12907)``
* ``Add project_id to client inside BigQuery hook update_table method (#13018)``

Bug fixes
~~~~~~~~~

* ``Fix four bugs in StackdriverTaskHandler (#13784)``
* ``Decode Remote Google Logs (#13115)``
* ``Fix and improve GCP BigTable hook and system test (#13896)``
* ``updated Google DV360 Hook to fix SDF issue (#13703)``
* ``Fix insert_all method of BigQueryHook to support tables without schema (#13138)``
* ``Fix Google BigQueryHook method get_schema() (#13136)``
* ``Fix Data Catalog operators (#13096)``


1.0.0
.....

Initial version of the provider.
