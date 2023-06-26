# Apache Airflow Kettle Operator

[![PyPI](https://img.shields.io/pypi/v/apache-airflow-providers-kettle)](https://pypi.org/project/apache-airflow-providers-kettle/)
[![PyPI - Downloads](https://img.shields.io/pypi/dm/apache-airflow-providers-kettle)](https://pypi.org/project/apache-airflow-providers-kettle/)

KettleOperator which consists of KettleRunJobOperator and KettleRunTransformationOperator responsible for running Hitachi Vantara's PDI (Pentaho Data Integration) jobs and transformations from .kjb and .ktr files.

Currently, there's no support for CarteServer and the only way to make it work is to make sure that PDI is deployed within the same container as your Airflow installation and the operator is deployed locally (no repository support either, yet).

# Requirements

- Python 3.7+
- Apache Airflow 2.2.5+
- Hitachi Vantara PDI (within the same container)

# Setup

### Installation

Installation can be done via pip, and the package itself will be installed in your site-packages directory (just like any other pip installation).

`python -m site` to find out where are your site-packages installed. This package will be located in the `kettle_provider` directory.

```
pip install apache-airflow-providers-kettle
```

### Usage

To use the Operators you first have to import them within your Airflow .py files (presumably your DAG).

```
from kettle_provider.operators.kettle_operator import KettleRunJobOperator, KettleRunTransformationOperator
```

The Operators can then be used just like any other. 

```
run_job = KettleRunJobOperator(
    task_id='kettle-run-job',
    file='test-job.kjb',
    params={
        'test-parameter-1': 'test-value-1',
        'test-parameter'2': 'test-value-2',
    },
)
```
```
run_transformation = KettleRunTransformationOperator(
    task_id='kettle-run-transformation',
    file='test-job.ktr',
    params={
        'test-parameter-1': 'test-value-1',
        'test-parameter'2': 'test-value-2',
    },
)
```

### Available parameters

Below are the parameters you can use when defining the tasks with their default values.
Below list excludes base parameters inherited from BaseOperator class (such as `task_id`, etc.)

```
KettleRunJobOperator(
    pdipath: str = '/opt/pentaho/data-integration/'                             # PDI installation location
    filepath: str = '/opt/pentaho/data-integration/jobs/'                       # jobs/ directory within PDI installation
    file: str | None = None                                                     # .kjb filename to run (with file extension)
    logfile: str = '/opt/pentaho/data-integration/logs/pdi.kitchen.log',        # logfile for kitchen runs
    maxloglines: int = 0,                                                       # max log lines for kitchen logfile (0 = no limit)
    maxlogtimeout: int = 0,                                                     # max log age in seconds for kitchen logfile (0 = no limit)
    loglevel: str = 'Basic',                                                    # log level (Basic, Detailed, Debug, Rowlevel, Error, Nothing)
    params: dict[str, str] | None = None,                                       # dictionary of parameters
    output_encoding: str = 'utf-8',                                             # output encoding for exit commands
    **kwargs
)
```

```
KettleRunTransformationOperator(
    pdipath: str = '/opt/pentaho/data-integration/'
    filepath: str = '/opt/pentaho/data-integration/transformations/'
    file: str | None = None
    logfile: str = '/opt/pentaho/data-integration/logs/pdi.pan.log',
    maxloglines: int = 0,
    maxlogtimeout: int = 0,
    loglevel: str = 'Basic',
    params: dict[str, str] | None = None,
    output_encoding: str = 'utf-8',
    **kwargs
)
```
# To-dos

- CarteServer support (for PDI deployed in a different container)
- PDI Repository support
- Better exit codes (currently all we get are bash exit-codes)
- Support for Windows machines (currently, the commands are all executed with bash)
