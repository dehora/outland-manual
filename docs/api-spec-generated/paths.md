
## API Resources

### Working with Apps, Grants, and Owners

  - [POST /apps](#post-apps)
  - [GET /apps{appkey}](#get-appsappkey)
  - [POST /apps/{appkey}/grants/members](#post-appsappkeygrantsmembers)
  - [DELETE /apps/{appkey}/grants/members/{member_key_or_email}](#delete-appsappkeygrantsmembersmember_key_or_email})
  - [POST /apps/{appkey}/grants/services](#post-appsappkeygrantsservices)
  - [DELETE /apps/{appkey}/grants/services/{service_key}](#delete-appsappkeygrantsservicesservice_key)
  - [POST /apps/{appkey}/owners](#post-appsappkeyowners)
  - [DELETE /apps/{appkey}/owners/{owner_username_or_email}](#delete-appsappkeyownersowner_username_or_email)
  
### Working with Features

  - [POST /features/{appkey}](#post-featuresappkey)
  - [GET /features/{appkey}/feed](#get-featuresappkeyfeed)
  - [POST /features/{appkey}/{feature_key}](#post-featuresappkeyfeature_key)




<a name="paths"></a>
## Paths

<a name="apps-post"></a>
### POST /apps

#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Header**|**Idempotency-Key**  <br>*optional*|Optional. Allows retrys with the same value to assure the request is processed once.|string|
|**Body**|**app**  <br>*optional*||[AppCreate](definitions.md#appcreate)|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**201**|Created  <br>**Headers** :   <br>`Location` (string) : The location of the created App. Always an absolute URI.  <br>`Idempotency-Key-Trace` (string) : Returned if the request is a duplicate based on the Idempotency-Key.|[App](definitions.md#app)|
|**422**|returned if the App is invalid|[Problem](definitions.md#problem)|


<a name="apps-appkey-get"></a>
### GET /apps/{appkey}

#### Parameters

|Type|Name|Schema|
|---|---|---|
|**Path**|**appkey**  <br>*required*|string|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Ok|[App](definitions.md#app)|


<a name="apps-appkey-grants-members-post"></a>
### POST /apps/{appkey}/grants/members

#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Header**|**Idempotency-Key**  <br>*optional*|Optional. Allows retrys with the same value to assure the request is processed once.|string|
|**Path**|**appkey**  <br>*required*||string|
|**Body**|**member**  <br>*optional*||[MemberGrantCreate](definitions.md#membergrantcreate)|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Ok  <br>**Headers** :   <br>`Idempotency-Key-Trace` (string) : Returned if the request is a duplicate based on the Idempotency-Key.|[App](definitions.md#app)|


<a name="apps-appkey-grants-members-member_key_or_email-delete"></a>
### DELETE /apps/{appkey}/grants/members/{member_key_or_email}

#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Path**|**appkey**  <br>*required*||string|
|**Path**|**member_key_or_email**  <br>*required*|The member's email or key.|string|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Ok|[App](definitions.md#app)|


<a name="apps-appkey-grants-services-post"></a>
### POST /apps/{appkey}/grants/services

#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Header**|**Idempotency-Key**  <br>*optional*|Optional. Allows retrys with the same value to assure the request is processed once.|string|
|**Path**|**appkey**  <br>*required*||string|
|**Body**|**service**  <br>*optional*||[ServiceGrantCreate](definitions.md#servicegrantcreate)|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Ok  <br>**Headers** :   <br>`Idempotency-Key-Trace` (string) : Returned if the request is a duplicate based on the Idempotency-Key.|[App](definitions.md#app)|


<a name="apps-appkey-grants-services-service_key-delete"></a>
### DELETE /apps/{appkey}/grants/services/{service_key}

#### Parameters

|Type|Name|Schema|
|---|---|---|
|**Path**|**appkey**  <br>*required*|string|
|**Path**|**service_key**  <br>*required*|string|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Ok|[App](definitions.md#app)|


<a name="apps-appkey-owners-post"></a>
### POST /apps/{appkey}/owners

#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Header**|**Idempotency-Key**  <br>*optional*|Optional. Allows retrys with the same value to assure the request is processed once.|string|
|**Path**|**appkey**  <br>*required*||string|
|**Body**|**owner**  <br>*optional*||[OwnerCreate](definitions.md#ownercreate)|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Ok  <br>**Headers** :   <br>`Idempotency-Key-Trace` (string) : Returned if the request is a duplicate based on the Idempotency-Key.|[App](definitions.md#app)|
|**409**|Returned if there would be no owners left after the delete|[Problem](definitions.md#problem)|


<a name="apps-appkey-owners-owner_username_or_email-delete"></a>
### DELETE /apps/{appkey}/owners/{owner_username_or_email}

#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Path**|**appkey**  <br>*required*||string|
|**Path**|**owner_username_or_email**  <br>*required*|The owner's email or username.|string|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Ok|[App](definitions.md#app)|


<a name="features-appkey-post"></a>
### POST /features/{appkey}

#### Description
Add a feature to the app.


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Header**|**Idempotency-Key**  <br>*optional*|Optional. Allows retrys with the same value to assure the request is processed once.|string|
|**Path**|**appkey**  <br>*required*||string|
|**Body**|**feature**  <br>*optional*||[FeatureCreate](definitions.md#featurecreate)|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Ok  <br>**Headers** :   <br>`Idempotency-Key-Trace` (string) : Returned if the request is a duplicate based on the Idempotency-Key.|[Feature](definitions.md#feature)|


<a name="features-appkey-feed-get"></a>
### GET /features/{appkey}/feed

#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Path**|**appkey**  <br>*required*||string|
|**Query**|**since**  <br>*optional*|A UNIX timestamp stating the point to fetch changes from. Always treated as UTC.|integer (int64)|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Ok|[FeatureCollection](definitions.md#featurecollection)|


<a name="features-appkey-feature_key-post"></a>
### POST /features/{appkey}/{feature_key}

#### Description
Update a feature in the app.


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Header**|**Idempotency-Key**  <br>*optional*|Optional. Allows retrys with the same value to assure the request is processed once.|string|
|**Path**|**appkey**  <br>*required*||string|
|**Path**|**feature_key**  <br>*required*||string|
|**Body**|**feature**  <br>*optional*||[FeatureCreate](definitions.md#featurecreate)|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Ok  <br>**Headers** :   <br>`Idempotency-Key-Trace` (string) : Returned if the request is a duplicate based on the Idempotency-Key.|[Feature](definitions.md#feature)|



