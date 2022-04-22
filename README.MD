#### 1. upload the input.pdf into the gcs bucket
#### 2. config the request.json as the api payload
#### 3. call the batch translation api [reference doc](https://cloud.google.com/translate/docs/advanced/batch-translation#batch-request)

```
curl -X POST \
-H "Authorization: Bearer "$(gcloud auth application-default print-access-token) \
-H "Content-Type: application/json; charset=utf-8" \
-d @request.json \
"https://translation.googleapis.com/v3/projects/<PROJECT_NUMBER_OR_ID>/locations/<us-central1>:batchTranslateText"
```

```
# example
curl -X POST \
-H "Authorization: Bearer "$(gcloud auth application-default print-access-token) \
-H "Content-Type: application/json; charset=utf-8" \
-d @request.json \
"https://translation.googleapis.com/v3/projects/cliu201/locations/us-central1:batchTranslateDocument"
```

get the operation id from the output
```json
{
  "name": "projects/148965607901/locations/us-central1/operations/20220421-22281650605302-625e066e-0000-29a2-9043-089e0820de50",
  "metadata": {
    "@type": "type.googleapis.com/google.cloud.translation.v3.BatchTranslateDocumentMetadata",
    "state": "RUNNING"
  }
}
```

#### 4. get the operation status [reference doc](https://cloud.google.com/translate/docs/advanced/long-running-operation)
```
curl -X GET \
-H "Authorization: Bearer "$(gcloud auth application-default print-access-token) \
"https://translation.googleapis.com/v3/projects/PROJECT_NUMBER_OR_ID/locations/location-id/operations/operation-id"
```

```
# example
curl -X GET \
-H "Authorization: Bearer "$(gcloud auth application-default print-access-token) \
"https://translation.googleapis.com/v3/projects/cliu201/locations/us-central1/operations/20220421-22281650605302-625e066e-0000-29a2-9043-089e0820de50"
```

successful output for example: 
```json
{
  "name": "projects/148965607901/locations/us-central1/operations/20220421-22281650605302-625e066e-0000-29a2-9043-089e0820de50",
  "metadata": {
    "@type": "type.googleapis.com/google.cloud.translation.v3.BatchTranslateDocumentMetadata",
    "state": "SUCCEEDED",
    "totalPages": "27",
    "translatedPages": "27",
    "totalBillablePages": "27",
    "totalCharacters": "4194",
    "translatedCharacters": "4194",
    "submitTime": "2022-04-22T05:28:22Z"
  },
  "done": true,
  "response": {
    "@type": "type.googleapis.com/google.cloud.translation.v3.BatchTranslateDocumentResponse",
    "totalPages": "27",
    "translatedPages": "27",
    "totalBillablePages": "27",
    "totalCharacters": "4194",
    "translatedCharacters": "4194",
    "submitTime": "2022-04-22T05:28:22Z",
    "endTime": "2022-04-22T05:39:13Z"
  }
}
```
