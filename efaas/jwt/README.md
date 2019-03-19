# Generate JWT

The script generate a Google ID JWT token for authenticating requests with Elastifile Cloud File Service API

##Install requirements
```
pip install -r requirements
```

## Generate API JWT token
Set the path to Google Service Account json file in ELASTIFILE_APPLICATION_CREDENTIALS environment variable
```bash
export ELASTIFILE_APPLICATION_CREDENTIALS="/path/to/service_account.json"
python main.py

```