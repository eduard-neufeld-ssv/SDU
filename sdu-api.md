#### Topics
[Introduction](#sdu-server-api-v210)


[Maintainer](#maintainer)
* [Get a list of present BLOBs](#get-a-list-of-present-blobs)
* [Get info about existing BLOB](#get-info-about-existing-blob)
* [Upload a new BLOB](#upload-a-new-blob)
* [Delete an existing BLOB](#delete-an-existing-blob)
* [Get the delta between a known BLOB and a desired BLOB](#get-the-delta-between-a-known-blob-and-a-desired-blob)
* [Download the version&#x27;s manifest](#download-the-versions-manifest)
* [Upload the version&#x27;s manifest](#upload-the-versions-manifest)

[Devices](#devices)
* [Download an existing BLOB](#download-an-existing-blob)
* [Get the current version](#get-the-current-version)

[Manager](#manager)
* [Get a list of all products](#get-a-list-of-all-products)
* [Get the current version assignment](#get-the-current-version-assignment)
* [Append a version assignment](#append-a-version-assignment)
* [Insert a version assignment before the given assignment](#insert-a-version-assignment-before-the-given-assignment)
* [Remove the given assignment](#remove-the-given-assignment)
* [Modify the given assignment](#modify-the-given-assignment)
* [Get a list of all existing versions for a product](#get-a-list-of-all-existing-versions-for-a-product)
* [Get information about a version](#get-information-about-a-version)
* [Mark the version as available or unavailable](#mark-the-version-as-available-or-unavailable)

[Schema Definitions](#schemas)
* [Error](#error)
* [Success](#success)
* [Blob](#blob)
* [BlobList](#bloblist) 
* [BlobInfo](#blobinfo)
* [ProductList](#productlist)
* [Version](#version)
* [VersionInfo](#versioninfo)
* [VersionList](#versionlist)
* [VersionEdit](#versionedit)
* [CurrentVersion](#currentversion)
* [CurrentVersionAssignmentList](#currentversionassignmentlist)
* [CurrentVersionAssignment](#currentversionassignment)

<!-- Generator: Widdershins v4.0.1 -->

<h1 id="sdu-server-api">SDU Server API v2.1.0</h1>

> Scroll down for code samples, example requests and responses. Select a language for code samples from the tabs above or the mobile navigation menu.

The SDU API enables Maintainers, Managers and Devices to interact with the SDU server. The server manages updates with two types of data: The BLOB (Binary Large Object), which holds the data of an update, and a Manifest that assigns product type, version and signature to a BLOB.

Base URLs:

* <a href="https://[customer]-sdu.ssv-service.de/v2">https://[customer]-sdu.ssv-service.de/v2</a>

Email: <a href="mailto:jfi@ssv-embedded.de">Support</a>

<h1 id="sdu-server-api-maintainer">Maintainer</h1>

## Get a list of present BLOBs

`GET /blob`

> Example responses

> 200 Response

```json
{
  "code": 0,
  "blobs": [
    {
      "sha256": "string",
      "date": "2019-08-24T14:15:22Z",
      "size": 0,
      "maintainer": "string",
      "referencedByVersion": true
    }
  ]
}
```

<h3 id="get-a-list-of-present-blobs-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|List of present blobs|[BlobList](#schemabloblist)|

## Get info about existing BLOB

`GET /blob/{sha256}`

<h3 id="get-info-about-existing-blob-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|sha256|path|string|true|SHA256 of the BLOB|

> Example responses

> 200 Response

```json
{
  "code": 0,
  "blob": {
    "sha256": "string",
    "date": "2019-08-24T14:15:22Z",
    "size": 0,
    "maintainer": "string",
    "referencedByVersion": true
  }
}
```

<h3 id="get-info-about-existing-blob-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Version|[BlobInfo](#schemablobinfo)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Blob does not exist|[Error](#schemaerror)|

## Upload a new BLOB

`POST /blob/{sha256}`

<h3 id="upload-a-new-blob-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|sha256|path|string|true|SHA256 of the content|

> Example responses

> 200 Response

```json
{
  "code": 0
}
```

<h3 id="upload-a-new-blob-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|The data has been accepted|[Success](#schemasuccess)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|SHA256 mismatch|[Error](#schemaerror)|
|409|[Conflict](https://tools.ietf.org/html/rfc7231#section-6.5.8)|Blob already existing|[Error](#schemaerror)|
|413|[Payload Too Large](https://tools.ietf.org/html/rfc7231#section-6.5.11)|Blob too large|[Error](#schemaerror)|

## Delete an existing BLOB

`DELETE /blob/{sha256}`

<h3 id="delete-an-existing-blob-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|sha256|path|string|true|SHA256 of the content|

> Example responses

> 200 Response

```json
{
  "code": 0
}
```

<h3 id="delete-an-existing-blob-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|The blob has been delete|[Success](#schemasuccess)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Blob is referenced by a version|[Error](#schemaerror)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Blob does not exist|[Error](#schemaerror)|

## Get the delta between a known BLOB and a desired BLOB

`GET /blob/{sha256}/xdeltadiff/{knownfilesha256}`

<h3 id="get-the-delta-between-a-known-blob-and-a-desired-blob-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|sha256|path|string|true|SHA256 of the desired BLOB|
|knownfilesha256|path|string|true|SHA256 of the known BLOB|

<h3 id="get-the-delta-between-a-known-blob-and-a-desired-blob-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|The xdelta3 diff to the given hash|None|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Blob does not exist|None|
|428|[Precondition Required](https://tools.ietf.org/html/rfc6585#section-3)|Blob with the given knownfilesha256 does not exist|None|

## Download the version's manifest

`GET /product/{product}/version/{version}/manifest`

<h3 id="download-the-version's-manifest-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|product|path|string|true|Product Name|
|version|path|string|true|Version Name|

<h3 id="download-the-version's-manifest-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|The version's mainfest file|None|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Version not found|None|

### Response Headers

|Status|Header|Type|Format|Description|
|---|---|---|---|---|
|200|X-Upload-Date|integer||Upload date as Unix timestamp|

## Upload the version's manifest

`POST /product/{product}/version/{version}/manifest`

> Body parameter

<h3 id="upload-the-version's-manifest-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|product|path|string|true|Product Name|
|version|path|string|true|Version Name|
|body|body|string|true|none|

> Example responses

> 400 Response

<h3 id="upload-the-version's-manifest-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|The data has been accepted|None|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|The version already exists or the manifest is invalid|[Error](#schemaerror)|

<h1 id="sdu-server-api-devices">Devices</h1>

## Download an existing BLOB

`GET /blob/{sha256}/data`

<h3 id="download-an-existing-blob-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|sha256|path|string|true|SHA256 of the BLOB|

<h3 id="download-an-existing-blob-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Data|None|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Blob does not exist|None|

## Get the current version

`GET /product/{product}/currentVersion`

<h3 id="get-the-current-version-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|product|path|string|true|Product Name|
|serialnumber|query|string|false|Managers must state a serialnumber get the right version info. For devices this parameter is ignored and the common name of the device's certificate is used instead.|
|installedVersion|query|string|false|Give information about the currently installed version. This will be written to the SDU server's log.|

> Example responses

> 200 Response

```json
{
  "code": 0,
  "currentVersion": {
    "date": "2019-08-24T14:15:22Z",
    "manager": "string",
    "name": "string"
  }
}
```

<h3 id="get-the-current-version-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Current version assignment|[CurrentVersion](#schemacurrentversion)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|No assignment available for this product or serialnumber|[Error](#schemaerror)|

<h1 id="sdu-server-api-manager">Manager</h1>

## Get a list of all products

`GET /product`

> Example responses

> 200 Response

```json
{
  "code": 0,
  "products": [
    "string"
  ]
}
```

<h3 id="get-a-list-of-all-products-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Current version assignment|[ProductList](#schemaproductlist)|

## Get the current version assignment

`GET /product/{product}/currentVersionAssignment`

<h3 id="get-the-current-version-assignment-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|product|path|string|true|Product Name|

> Example responses

> 200 Response

```json
{
  "code": 0,
  "currentVersionAssignments": [
    {
      "id": "string",
      "date": "2019-08-24T14:15:22Z",
      "manager": "string",
      "serialnumberFilter": "string",
      "versionName": "string"
    }
  ]
}
```

<h3 id="get-the-current-version-assignment-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Current assignments|[CurrentVersionAssignmentList](#schemacurrentversionassignmentlist)|

## Append a version assignment

`POST /product/{product}/currentVersionAssignment`

> Body parameter

```json
{
  "currentVersionAssignment": {
    "serialnumberFilter": "string",
    "versionName": "string"
  }
}
```

<h3 id="append-a-version-assignment-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|product|path|string|true|Product Name|
|body|body|[CurrentVersionAssignment](#schemacurrentversionassignment)|true|Version assignment|

> Example responses

> 200 Response

<h3 id="append-a-version-assignment-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|New assignment accepted|[Success](#schemasuccess)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|New assignment rejected|[Error](#schemaerror)|

## Insert a version assignment before the given assignment

`POST /product/{product}/currentVersionAssignment/{id}`

> Body parameter

```json
{
  "currentVersionAssignment": {
    "serialnumberFilter": "string",
    "versionName": "string"
  }
}
```

<h3 id="insert-a-version-assignment-before-the-given-assignment-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|product|path|string|true|Product Name|
|id|path|string|true|Existing assignment ID|
|body|body|[CurrentVersionAssignment](#schemacurrentversionassignment)|true|Version assignment|

> Example responses

> 200 Response

```json
{
  "code": 0
}
```

<h3 id="insert-a-version-assignment-before-the-given-assignment-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|New assignment accepted|[Success](#schemasuccess)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid assignment|[Error](#schemaerror)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Unknown assignment|[Error](#schemaerror)|

## Remove the given assignment

`DELETE /product/{product}/currentVersionAssignment/{id}`

<h3 id="remove-the-given-assignment-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|product|path|string|true|Product Name|
|id|path|string|true|Assignment ID|

> Example responses

> 200 Response

```json
{
  "code": 0
}
```

<h3 id="remove-the-given-assignment-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Assignment removed|[Success](#schemasuccess)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Unknown assignment|[Error](#schemaerror)|

## Modify the given assignment

`PATCH /product/{product}/currentVersionAssignment/{id}`

> Body parameter

```json
{
  "currentVersionAssignment": {
    "serialnumberFilter": "string",
    "versionName": "string"
  }
}
```

<h3 id="modify-the-given-assignment-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|product|path|string|true|Product Name|
|id|path|string|true|Assignment ID|
|body|body|[CurrentVersionAssignment](#schemacurrentversionassignment)|true|Version assignment|

> Example responses

> 200 Response

```json
{
  "code": 0
}
```

<h3 id="modify-the-given-assignment-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|New assignment accepted|[Success](#schemasuccess)|
|400|[Bad Request](https://tools.ietf.org/html/rfc7231#section-6.5.1)|Invalid assignment|[Error](#schemaerror)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Unknown assignment|[Error](#schemaerror)|

## Get a list of all existing versions for a product

`GET /product/{product}/version`

<h3 id="get-a-list-of-all-existing-versions-for-a-product-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|product|path|string|true|Product Name|

> Example responses

> 200 Response

```json
{
  "code": 0,
  "versions": [
    {
      "date": "2019-08-24T14:15:22Z",
      "name": "string",
      "maintainer": "string",
      "available": true
    }
  ]
}
```

<h3 id="get-a-list-of-all-existing-versions-for-a-product-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Maintainer|[VersionList](#schemaversionlist)|

## Get information about a version

`GET /product/{product}/version/{version}`

<h3 id="get-information-about-a-version-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|product|path|string|true|Product Name|
|version|path|string|true|Version Name|

> Example responses

> 200 Response

```json
{
  "code": 0,
  "version": {
    "date": "2019-08-24T14:15:22Z",
    "name": "string",
    "maintainer": "string",
    "available": true
  }
}
```

<h3 id="get-information-about-a-version-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Version|[VersionInfo](#schemaversioninfo)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Version not found|[Error](#schemaerror)|

## Mark the version as available or unavailable

`PATCH /product/{product}/version/{version}`

> Body parameter

```json
{
  "version": {
    "available": true
  }
}
```

<h3 id="mark-the-version-as-available-or-unavailable-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|product|path|string|true|Product Name|
|version|path|string|true|Version Name|
|body|body|[VersionEdit](#schemaversionedit)|true|none|

> Example responses

> 200 Response

```json
{
  "code": 0
}
```

<h3 id="mark-the-version-as-available-or-unavailable-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|The version has been marked acordingly|[Success](#schemasuccess)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Version not found|[Error](#schemaerror)|

# Schemas

<h2 id="tocS_Error">Error</h2>
<!-- backwards compatibility -->
<a id="schemaerror"></a>
<a id="schema_Error"></a>
<a id="tocSerror"></a>
<a id="tocserror"></a>

```json
{
  "code": 0,
  "error": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|code|integer|false|none|Error number for machines|
|error|string|false|none|Human-readable error describing the problem|

<h2 id="tocS_Success">Success</h2>
<!-- backwards compatibility -->
<a id="schemasuccess"></a>
<a id="schema_Success"></a>
<a id="tocSsuccess"></a>
<a id="tocssuccess"></a>

```json
{
  "code": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|code|integer|false|none|Always zero|

<h2 id="tocS_Blob">Blob</h2>
<!-- backwards compatibility -->
<a id="schemablob"></a>
<a id="schema_Blob"></a>
<a id="tocSblob"></a>
<a id="tocsblob"></a>

```json
{
  "sha256": "string",
  "date": "2019-08-24T14:15:22Z",
  "size": 0,
  "maintainer": "string",
  "referencedByVersion": true
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|sha256|string|false|none|The content's SHA256|
|date|string(date-time)|false|none|Update date of this definition|
|size|integer|false|none|Size of the BLOB in bytes|
|maintainer|string|false|none|Maintainer who uploaded this BLOB|
|referencedByVersion|boolean|false|none|Indicates whether this BLOB is used by any uploaded version|

<h2 id="tocS_BlobList">BlobList</h2>
<!-- backwards compatibility -->
<a id="schemabloblist"></a>
<a id="schema_BlobList"></a>
<a id="tocSbloblist"></a>
<a id="tocsbloblist"></a>

```json
{
  "code": 0,
  "blobs": [
    {
      "sha256": "string",
      "date": "2019-08-24T14:15:22Z",
      "size": 0,
      "maintainer": "string",
      "referencedByVersion": true
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|code|integer|false|none|Always zero|
|blobs|[[Blob](#schemablob)]|false|none|none|

<h2 id="tocS_BlobInfo">BlobInfo</h2>
<!-- backwards compatibility -->
<a id="schemablobinfo"></a>
<a id="schema_BlobInfo"></a>
<a id="tocSblobinfo"></a>
<a id="tocsblobinfo"></a>

```json
{
  "code": 0,
  "blob": {
    "sha256": "string",
    "date": "2019-08-24T14:15:22Z",
    "size": 0,
    "maintainer": "string",
    "referencedByVersion": true
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|code|integer|false|none|Always zero|
|blob|[Blob](#schemablob)|false|none|none|

<h2 id="tocS_ProductList">ProductList</h2>
<!-- backwards compatibility -->
<a id="schemaproductlist"></a>
<a id="schema_ProductList"></a>
<a id="tocSproductlist"></a>
<a id="tocsproductlist"></a>

```json
{
  "code": 0,
  "products": [
    "string"
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|code|integer|false|none|Always zero|
|products|[string]|false|none|none|

<h2 id="tocS_Version">Version</h2>
<!-- backwards compatibility -->
<a id="schemaversion"></a>
<a id="schema_Version"></a>
<a id="tocSversion"></a>
<a id="tocsversion"></a>

```json
{
  "date": "2019-08-24T14:15:22Z",
  "name": "string",
  "maintainer": "string",
  "available": true
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|date|string(date-time)|false|none|Upload date of this version|
|name|string|false|none|Version name|
|maintainer|string|false|none|The maintainer who signed this version|
|available|boolean|false|none|Is this version downloadable by devices?|

<h2 id="tocS_VersionInfo">VersionInfo</h2>
<!-- backwards compatibility -->
<a id="schemaversioninfo"></a>
<a id="schema_VersionInfo"></a>
<a id="tocSversioninfo"></a>
<a id="tocsversioninfo"></a>

```json
{
  "code": 0,
  "version": {
    "date": "2019-08-24T14:15:22Z",
    "name": "string",
    "maintainer": "string",
    "available": true
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|code|integer|false|none|Always zero|
|version|[Version](#schemaversion)|false|none|none|

<h2 id="tocS_VersionList">VersionList</h2>
<!-- backwards compatibility -->
<a id="schemaversionlist"></a>
<a id="schema_VersionList"></a>
<a id="tocSversionlist"></a>
<a id="tocsversionlist"></a>

```json
{
  "code": 0,
  "versions": [
    {
      "date": "2019-08-24T14:15:22Z",
      "name": "string",
      "maintainer": "string",
      "available": true
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|code|integer|false|none|Always zero|
|versions|[[Version](#schemaversion)]|false|none|none|

<h2 id="tocS_VersionEdit">VersionEdit</h2>
<!-- backwards compatibility -->
<a id="schemaversionedit"></a>
<a id="schema_VersionEdit"></a>
<a id="tocSversionedit"></a>
<a id="tocsversionedit"></a>

```json
{
  "version": {
    "available": true
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|version|object|false|none|none|
|» available|boolean|false|none|Is this version downloadable by devices?|

<h2 id="tocS_CurrentVersion">CurrentVersion</h2>
<!-- backwards compatibility -->
<a id="schemacurrentversion"></a>
<a id="schema_CurrentVersion"></a>
<a id="tocScurrentversion"></a>
<a id="tocscurrentversion"></a>

```json
{
  "code": 0,
  "currentVersion": {
    "date": "2019-08-24T14:15:22Z",
    "manager": "string",
    "name": "string"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|code|integer|false|none|Always zero|
|currentVersion|object|false|none|none|
|» date|string(date-time)|false|none|Update date of this definition|
|» manager|string|false|none|Manager who stored this definition|
|» name|string|false|none|Version to install|

<h2 id="tocS_CurrentVersionAssignmentList">CurrentVersionAssignmentList</h2>
<!-- backwards compatibility -->
<a id="schemacurrentversionassignmentlist"></a>
<a id="schema_CurrentVersionAssignmentList"></a>
<a id="tocScurrentversionassignmentlist"></a>
<a id="tocscurrentversionassignmentlist"></a>

```json
{
  "code": 0,
  "currentVersionAssignments": [
    {
      "id": "string",
      "date": "2019-08-24T14:15:22Z",
      "manager": "string",
      "serialnumberFilter": "string",
      "versionName": "string"
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|code|integer|false|none|Always zero|
|currentVersionAssignments|[object]|false|none|none|
|» id|string|false|none|A random identifier for this assignment|
|» date|string(date-time)|false|none|Update date of this definition|
|» manager|string|false|none|Manager who stored this definition|
|» serialnumberFilter|string|false|none|Regular expression for matching serial numbers|
|» versionName|string|false|none|Version string|

<h2 id="tocS_CurrentVersionAssignment">CurrentVersionAssignment</h2>
<!-- backwards compatibility -->
<a id="schemacurrentversionassignment"></a>
<a id="schema_CurrentVersionAssignment"></a>
<a id="tocScurrentversionassignment"></a>
<a id="tocscurrentversionassignment"></a>

```json
{
  "currentVersionAssignment": {
    "serialnumberFilter": "string",
    "versionName": "string"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|currentVersionAssignment|object|false|none|none|
|» serialnumberFilter|string|false|none|Regular expression for matching serial numbers|
|» versionName|string|false|none|Version string|
