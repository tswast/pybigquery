SQLAlchemy dialect and API client for BigQuery.


Usage
=====


SQLAchemy
_________

.. code-block:: python

    from sqlalchemy import *
    from sqlalchemy.engine import create_engine
    from sqlalchemy.schema import *
    engine = create_engine('bigquery://project')
    table = Table('dataset.table', MetaData(bind=engine), autoload=True)
    print(select([func.count('*')], from_obj=table).scalar())

API Client
__________

.. code-block:: python

    from pybigquery.api import ApiClient
    api_client = ApiClient()
    print(api_client.dry_run_query(query=sqlstr).total_bytes_processed)

Project
_______

``project`` in ``bigquery://project`` is used to instantiate BigQuery client with the specific project ID. To infer project from the environment, use ``bigquery://`` – without ``project``

Authentication
______________

Follow the `Google Cloud library guide <https://google-cloud-python.readthedocs.io/en/latest/core/auth.html>`_ for authentication. Alternatively, you can provide the path to a service account JSON file in ``create_engine()``:

.. code-block:: python

    engine = create_engine('bigquery://', credentials_path='/path/to/keyfile.json')


Location
________

To specify location of your datasets pass ``location`` to ``create_engine()``:

.. code-block:: python

    engine = create_engine('bigquery://project', location="asia-northeast1")


Table names
___________

To query tables from non-default projects, use the following format for the table name: ``project.dataset.table``, e.g.:

.. code-block:: python

    sample_table = Table('bigquery-public-data.samples.natality')

Batch size
__________

By default, ``arraysize`` is set to ``5000``. ``arraysize`` is used to set the batch size for fetching results. To change it, pass ``arraysize`` to ``create_engine()``:

.. code-block:: python

    engine = create_engine('bigquery://project', arraysize=1000)


Requirements
============

Install using

- ``pip install pybigquery``


Testing
============

Load sample tables::

    ./scripts/load_test_data.sh

This will create a dataset ``test_pybigquery`` with tables named ``sample_one_row`` and ``sample``.

Set up an environment and run tests::

    pyvenv .env
    source .env/bin/activate
    pip install -r dev_requirements.txt
    pytest
