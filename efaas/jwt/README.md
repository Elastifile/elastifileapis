# Generate JWT

The script generate a Google ID JWT token for authenticating requests with Elastifile Cloud File Service API

## Supported Python Versions:
Python >= 3.4


## Install the dependencies:

```bash
$ virtualenv .env
$ source .env/bin/activate
$ pip install -r requirements.txt
```

## Run the script to generate API JWT token
Set the path to Google Service Account json file in ELASTIFILE_APPLICATION_CREDENTIALS environment variable
```bash
(.env)$ export ELASTIFILE_APPLICATION_CREDENTIALS="/path/to/service_account.json"
(.env)$ python main.py

```
or

```bash
(.env)$ python main.py -f/--file /path/to/service_account.json
```