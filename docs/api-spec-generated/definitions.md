
## Objects 

  - [App](#app)
  - [AppCreate](#appcreate)
  - [Feature](#feature)
  - [FeatureCollection](#featurecollection)
  - [FeatureCreate](#featurecreate)
  - [FeatureOption](#featureoption)
  - [FeatureVersion](#featureversion)
  - [GrantCollection](#grantcollection)
  - [MemberGrant](#membergrant)
  - [MemberGrantCreate](#membergrantcreate)
  - [Owner](#owner)
  - [OwnerCreate](#ownercreate)
  - [ServiceGrant](#servicegrant)
  - [ServiceGrantCreate](#servicegrantcreate)
  - [Problem](#problem)


<a name="definitions"></a>
## Definitions

<a name="app"></a>
### App
An app houses a collection of features and who is granted access to them.

This representation corresponds the following protocol buffer:

```
  message App {
    string type = 8;
    string id = 1;
    string key = 2;
    string name = 3;
    string created = 6;
    string updated = 7;
    repeated Owner owners = 5;
    GrantCollection granted = 9;
  }
```


|Name|Description|Schema|
|---|---|---|
|**created**  <br>*required*|The server assigned create time.|string (date-time)|
|**granted**  <br>*required*|Which authenticated services and users are allowed to access the App's features. See<br>GrantCollection for more details.|[GrantCollection](definitions.md#grantcollection)|
|**id**  <br>*required*|The server's unique identifier for an App. However, clients typically use the key<br>to interact with an App.|string|
|**key**  <br>*required*|A client defined key for the App. Note that even though every App is given a unqiue<br>id, two Apps can't have the same key as the key is the idiomatic way owners access and<br>call Apps.|string|
|**name**  <br>*required*|The name of the App.|string|
|**owners**  <br>*required*|The owners of the App. Note the server tries to ensure every App has at least one owner.|< [Owner](definitions.md#owner) > array|
|**type**  <br>*required*||string|
|**updated**  <br>*required*|The server assigned update time.|string (date-time)|


<a name="appcreate"></a>
### AppCreate
An abbreviated form of App documenting the client defined fields to send.


|Name|Schema|
|---|---|
|**granted**  <br>*optional*|[GrantCollection](definitions.md#grantcollection)|
|**key**  <br>*required*|string|
|**name**  <br>*required*|string|
|**owners**  <br>*required*|< [Owner](definitions.md#owner) > array|


<a name="feature"></a>
### Feature
A feature.
This representation corresponds the following protocol buffer:

```
  message Feature {
      string id = 1;
      string key = 2;
      string appkey = 3;
      enum State {
          none = 0;
          off = 1;
          on = 2;
      }
      State state = 4;
      string description = 5;
      string created = 6;
      string updated = 7;
      Owner owner = 8;
      map<string, string> properties = 9;
      OptionType option = 10;
      repeated FeatureOption options = 11;
      FeatureVersion version = 12;
      string type = 13;
  }
```


|Name|Description|Schema|
|---|---|---|
|**appkey**  <br>*required*|The App's key identifier. Features cannot have their app key changed once created.|string|
|**created**  <br>*required*|The server assigned create time.|string (date-time)|
|**description**  <br>*required*|A description for the field. The server may choose to strip, escape or reject any<br>embedded markup.|string|
|**id**  <br>*required*|The server assigned id for the feature. Clients can use the key directly for identity.|string|
|**key**  <br>*required*|The client assigned key for the feature. This must be unique for a given App. Features<br>cannot have their keys changed once created.|string|
|**option**  <br>*required*|The kind of feature. These are the known values currently:<br><br>  - "flag": indicates a vanilla feature flag that has no follow on options, just state.<br>  - "bool": indicates a feature flag that has two fixed options, "true" and "false".<br>  - "string": indicates a feature flag that has n string valued options.<br>  - "long": indicates a feature flag that has n long valued (format int64) options.<br>  - "double": indicates a feature flag that has n double valued (format double) options.<br><br>This set may be extended in the future and should not be represented as a closed enum.|string|
|**options**  <br>*required*|The list of options for this feature. Note a "flag" option will have an empty array.|< [FeatureOption](definitions.md#featureoption) > array|
|**owner**  <br>*required*|The owner of the feature, useful for getting in touch.|[Owner](definitions.md#owner)|
|**properties**  <br>*required*|Client defined properties. The server may choose to strip, escape or reject any<br>embedded markup found in the names or values.|[FeatureProperties](definitions.md#featureproperties)|
|**state**  <br>*required*|The feature state. The enumeration is closed by design, but clients may treat unknown<br>states as being the 'off' type.<br><br>Features cannot be created with a starting state of on, they always start in the none<br>state (operationally none is treated the same as off) but none is used to indicate the<br>initial starting state. This means to turn on a feature, the client must send a subsequent<br>request. Clients may then change the feature state to on or off as needed, but can not<br>change the state back to none.|enum (on, off, none)|
|**type**  <br>*required*||string|
|**updated**  <br>*required*|The server assigned update time.|string (date-time)|
|**version**  <br>*required*|The version of the feature. This is updated each time the feature is changed.|[FeatureVersion](definitions.md#featureversion)|


<a name="featurecollection"></a>
### FeatureCollection
A collection of features belonging to an App.
This representation corresponds the following protocol buffer:

```
  message FeatureCollection {
      string type = 1;
      string appkey = 2;
      repeated Feature items = 3;
  }
```


|Name|Schema|
|---|---|
|**appkey**  <br>*required*|string|
|**items**  <br>*required*|< [Feature](definitions.md#feature) > array|
|**type**  <br>*required*|string|


<a name="featurecreate"></a>
### FeatureCreate
An abbreviated form of Feature documenting the client defined fields to send.


|Name|Description|Schema|
|---|---|---|
|**appkey**  <br>*required*||string|
|**description**  <br>*required*||string|
|**key**  <br>*required*||string|
|**option**  <br>*required*|The kind of feature. These are the known values currently:<br><br>  - "flag": indicates a vanilla feature flag that has no follow on options, just state.<br>  - "bool": indicates a feature flag that has two fixed options, "true" and "false".<br>  - "string": indicates a feature flag that has n string valued options.<br>  - "long": indicates a feature flag that has n long valued (format int64) options.<br>  - "double": indicates a feature flag that has n double valued (format double) options.<br><br>This set may be extended in the future and should not be represented as a closed enum.|string|
|**options**  <br>*required*|The list of options for this feature. Note a "flag" option will have an empty array.|< [FeatureOption](definitions.md#featureoption) > array|
|**owner**  <br>*required*||[Owner](definitions.md#owner)|
|**properties**  <br>*optional*||[FeatureProperties](definitions.md#featureproperties)|
|**state**  <br>*required*|The feature state. The enumaration is closed by design, but clients may treat unknown<br>states as being the 'none' type.|enum (on, off, none)|


<a name="featureoption"></a>
### FeatureOption
Indicates an option type of the feature.
This representation corresponds the following protocol buffer:

```
  message FeatureOption {
      string id = 1;
      OptionType option = 2;
      string name = 3;
      string value = 4;
      int32 weight = 5;
      string type = 6;
  }
```


|Name|Description|Schema|
|---|---|---|
|**id**  <br>*required*|The server assigned id for the feature option. Clients please note:<br><br>  - the id is not required when creating a feature; if sent it will be replaced<br>  - the id must be sent thereafter when adjusting the weights|string|
|**name**  <br>*required*|The name of the option. Note that for a bool option this must be either "true" or<br>"false". Other values cause a 422 response.|string|
|**option**  <br>*required*|The kind of feature. These are the known values currently:<br><br>  - "flag": indicates a vanilla feature flag that has no follow on options, just state.<br>  - "bool": indicates a feature flag that has two fixed options, "true" and "false".<br>  - "string": indicates a feature flag that has n string valued options.<br>  - "long": indicates a feature flag that has n long valued (format int64) options.<br>  - "double": indicates a feature flag that has n double valued (format double) options.<br><br>This set may be extended in the future and should not be represented as a closed enum.|string|
|**type**  <br>*required*||string|
|**value**  <br>*required*|The value of the feature. Clients may parse the string value as follows based on the<br>kind of option:<br><br>  - "bool": the literal value is one of "true" or "false" (OAI format boolean)<br>  - "string": the literal value is a string<br>  - "long": the literal value may be treated as an int64 signed long (OAI format int64)<br>  - "double": the literal value may be treated as a double  (OAI format double)<br><br>This set may be extended in the future and should not be represented as a closed enum.<br><br>Note that for a bool option this must be either "true" or "false". Other values<br>cause a 422 response. This value must correspond to the value of the name field. If<br>it doesn't that will cause a 422 response.|string|
|**weight**  <br>*required*|A number between 0 and 1000. Larger or smaller numbers will cause a 422 response. The<br>total sum of the weights across all options for a feature must equal 10000; if they<br>don't the  response will be a 422.|integer (int32)|


<a name="featureproperties"></a>
### FeatureProperties
*Type* : < string, string > map


<a name="featureversion"></a>
### FeatureVersion
Indicates the version of the feature. Versions are structured objects instead of strings
or numeric values as this allows them to be used in a fully distributed setting. In
other words, this API does not assume it is the only source of versions.

Versions are based on Hybrid Logical Clocks, which use a physical timestamp component
along with a logical counter. See "Logical Physical Clocks and Consistent Snapshots
in Globally Distributed Databases" by Kulkarni et al,
https://www.cse.buffalo.edu/tech-reports/2014-04.pdf, for more details on the model.

As a convenience an id string value is also sent with the timestamp and counter and
represents the same version information. The id value is lexically comparable for
version order - ids are based ULIDs with the difference that they are always lowercase.
See https://github.com/alizain/ulid for more details on ULIDs.

This representation corresponds the following protocol buffer:

```
  message FeatureVersion {
      string id = 1;
      int64 timestamp = 2;
      int64 counter = 3;
      string type = 4;
  }
```


|Name|Description|Schema|
|---|---|---|
|**counter**  <br>*required*|The logical counter for the version|integer (int32)|
|**id**  <br>*required*|The physical timestamp for the version  <br>**Pattern** : `"ver_[0-9abcdefghjkmnopqrstuvwxyz]{26}"`|string|
|**timestamp**  <br>*required*|The physical timestamp for the version|integer (int64)|
|**type**  <br>*required*||string|


<a name="grantcollection"></a>
### GrantCollection
A collection of member and service grants for an app.
This representation corresponds the following protocol buffer:

```
  message GrantCollection {
      string type = 1;
      repeated ServiceGrant services = 10;
      repeated MemberGrant members = 11;
  }
```

Services are typically just that, services or some runtime component of a system. Members
typically represent people such as an employee or team member, but a member can also be a
group or a team.

When making requests to see features, the service or member must present their credentials
and be authenticated against the server (for example via an OAuth Bearer token). Once they
are authenticated they are checked to see if they have a grant entry for the App. If they
do, the request is honored. If they don't the request is rejected.

The reason to support both services and members is that it's common for individuals to
want to work with features during development and for companies to issue tokens to
servcies and people separately. Allowng "people" grants avoids the need to do things like
create fake services for individuals. Also, operationally Outland supports the notion
of individuals examining features for operational or other reasons.

Grants are best thought of as a basic grouping mechanism to allow owners a level
of control over which services and people can access which features. This lets the server
provide a basic level of multitenancy. Grants do not provide strong security by themselves
- that will be acheived via the mechanisms you use to issue tokens and credentials to users
and systems to authenticate the named grantees, which is outside the scope of the Outland
API and project.


|Name|Description|Schema|
|---|---|---|
|**members**  <br>*required*|The list of members who can use this app's features|< [MemberGrant](definitions.md#membergrant) > array|
|**services**  <br>*required*|The list of services who can use this app's features|< [ServiceGrant](definitions.md#servicegrant) > array|
|**type**  <br>*required*||string|


<a name="membergrant"></a>
### MemberGrant
A member (person or team) that is allowed to access an App's features..
This representation corresponds the following protocol buffer:

```
  message MemberGrant {
      string type = 1;
      string id = 2;
      string name = 10;
      string username = 11;
      string email = 12;
  }
```


|Name|Description|Schema|
|---|---|---|
|**email**  <br>*required*|The email of the person or team|string|
|**id**  <br>*required*||string|
|**name**  <br>*required*|A name for the member.|string|
|**type**  <br>*required*||string|
|**username**  <br>*required*|The username of the person or team|string|


<a name="membergrantcreate"></a>
### MemberGrantCreate
An abbreviated form of MemberGrant documenting the client defined fields to send.


|Name|Description|Schema|
|---|---|---|
|**email**  <br>*required*|The email of the person or team|string|
|**name**  <br>*required*|A name for the member.|string|
|**username**  <br>*required*|The username of the person or team|string|


<a name="owner"></a>
### Owner
An owner is the responsible contact for a feature or app.
This representation corresponds the following protocol buffer:

```
  message Owner {
      string id = 1;
      string name = 2;
      string username = 3;
      string email = 4;
      string type = 7;
  }
```


|Name|Description|Schema|
|---|---|---|
|**email**  <br>*required*|The email of the owner or team.|string|
|**id**  <br>*required*|The server's unique identifier for an Owner. Owners are currently unique per App and<br> not neccessarily globally unique.|string|
|**name**  <br>*required*|A name for the owner.|string|
|**type**  <br>*required*||string|
|**username**  <br>*required*|The username of the owner or team.|string|


<a name="ownercreate"></a>
### OwnerCreate
An abbreviated form of Owner documenting the client defined fields to send.


|Name|Schema|
|---|---|
|**email**  <br>*required*|string|
|**name**  <br>*required*|string|
|**username**  <br>*required*|string|


<a name="problem"></a>
### Problem

|Name|Description|Schema|
|---|---|---|
|**detail**  <br>*optional*|A human readable explanation of the problem.|string|
|**instance**  <br>*optional*|An absolute URI that identifies the specific occurrence of the problem.|string (uri)|
|**status**  <br>*required*|The HTTP status code returned with this problem.|integer (int32)|
|**title**  <br>*required*|A short, summary of the problem type.|string|
|**type**  <br>*required*|A URI that identifies the problem type.|string (uri)|


<a name="servicegrant"></a>
### ServiceGrant
A service that is allowed to access an App's features..
This representation corresponds the following protocol buffer:

```
  message ServiceGrant {
      string type = 1;
      string id = 2;
      string name = 10;
      string key = 11;
  }
```


|Name|Description|Schema|
|---|---|---|
|**id**  <br>*required*||string|
|**key**  <br>*required*|A key identifiying the service|string|
|**name**  <br>*required*|A name for the service|string|
|**type**  <br>*required*||string|


<a name="servicegrantcreate"></a>
### ServiceGrantCreate
An abbreviated form of ServiceGrant documenting the client defined fields to send.


|Name|Schema|
|---|---|
|**key**  <br>*required*|string|
|**name**  <br>*required*|string|



