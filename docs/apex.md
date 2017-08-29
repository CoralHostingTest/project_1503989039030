# The Apex API


**APEX API** provides endpoints to onboard, update, delete a project and
retrieve project(s) information. It also provides other operations to sync
with github or receive github events. <br/> <br/> ***Terminology*** <br/> *
**Project**: A project is an API that is registered with APEX through the
onboarding process. <br/>

This API has the following attributes:

| Attribute | Value                 |
|-----------|-----------------------|
| namespace | com.yahoo.apex.parsec |
| version   | 1                     |


## Resources

### [SearchResponseList](#SearchResponseList)

#### GET /search/

#### Request parameters:

| Name   | Type   | Source        | Options | Description                                                                                                                              |
|--------|--------|---------------|---------|------------------------------------------------------------------------------------------------------------------------------------------|
| query  | String | query: q      |         | query, separate by "," this naming follows http://yo/ecrest                                                                              |
| sortBy | String | query: sortBy |         | sort search responses, which allows: projectName: projectName ascending -projectName: projectName descending (no value): default sorting |
| limit  | String | query: limit  |         | record limit in one response, default 10                                                                                                 |
| offset | String | query: offset |         | page offset, nullable, default 0                                                                                                         |


#### Responses:

Expected:

| Code   | Type               |
|--------|--------------------|
| 200 OK | SearchResponseList |


Exception:

| Code                      | Type          | Comment |
|---------------------------|---------------|---------|
| 400 Bad Request           | ResourceError |         |
| 401 Unauthorized          | ResourceError |         |
| 500 Internal Server Error | ResourceError |         |


### [ProjectResponse](#ProjectResponse)

#### POST /projects/

Get WSSID for an authentication in request's header <br/> marked this following
/wssids section because YBYCookieWSSIDIssuingServlet at DefaultWebListener
has already added this following mapping /apex/v1/wssids resource
ProjectResponseList GET "/wssids" { expected OK; exceptions { ResourceError
INTERNAL_SERVER_ERROR; ResourceError BAD_REQUEST; ResourceError UNAUTHORIZED;
} } Create/Onboard a project. <br/> Please refer to  [Sample
Request/Responses](/api/req_n_resp.md) sections for examples.


#### Request parameters:

| Name            | Type                                          | Source                             | Options | Description                 |
|-----------------|-----------------------------------------------|------------------------------------|---------|-----------------------------|
| creationRequest | [ProjectCreateRequest](#projectcreaterequest) | body                               |         | Required request parameter. |
| wssid           | String                                        | header: X-YahooWSSID-Authorization |         |                             |
| cookie          | String                                        | header: Cookie                     |         |                             |


#### Responses:

Expected:

| Code         | Type                                |
|--------------|-------------------------------------|
| 202 Accepted | [ProjectResponse](#projectresponse) |


Exception:

| Code                      | Type          | Comment |
|---------------------------|---------------|---------|
| 400 Bad Request           | ResourceError |         |
| 401 Unauthorized          | ResourceError |         |
| 403 Forbidden             | ResourceError |         |
| 409 Conflict              | ResourceError |         |
| 500 Internal Server Error | ResourceError |         |


#### PUT /projects/{id}

Edit/Modify a project. <br/> Please refer to  [Sample
Request/Responses](/api/req_n_resp.md) sections for examples.


#### Request parameters:

| Name    | Type                              | Source                             | Options | Description                    |
|---------|-----------------------------------|------------------------------------|---------|--------------------------------|
| id      | UUID                              | path                               |         | The id of the project to edit. |
| project | [ProjectRequest](#projectrequest) | body                               |         | The fields to edit.            |
| wssid   | String                            | header: X-YahooWSSID-Authorization |         |                                |
| cookie  | String                            | header: Cookie                     |         |                                |


#### Responses:

Expected:

| Code   | Type            |
|--------|-----------------|
| 200 OK | ProjectResponse |


Exception:

| Code                      | Type          | Comment |
|---------------------------|---------------|---------|
| 400 Bad Request           | ResourceError |         |
| 401 Unauthorized          | ResourceError |         |
| 403 Forbidden             | ResourceError |         |
| 404 Not Found             | ResourceError |         |
| 409 Conflict              | ResourceError |         |
| 500 Internal Server Error | ResourceError |         |


#### GET /projects/{id}

Get the detail of the project with the given project id. <br/> Please refer to
[Sample Request/Responses](/api/req_n_resp.md) sections for examples.


#### Request parameters:

| Name | Type | Source | Options | Description                                |
|------|------|--------|---------|--------------------------------------------|
| id   | UUID | path   |         | The id of the project to retrieve details. |


#### Responses:

Expected:

| Code   | Type            |
|--------|-----------------|
| 200 OK | ProjectResponse |


Exception:

| Code                      | Type          | Comment |
|---------------------------|---------------|---------|
| 400 Bad Request           | ResourceError |         |
| 401 Unauthorized          | ResourceError |         |
| 404 Not Found             | ResourceError |         |
| 500 Internal Server Error | ResourceError |         |


### [ProjectResponseList](#ProjectResponseList)

#### GET /projects

Get all projects. <br/> Please refer to [Sample
Request/Responses](/api/req_n_resp.md) sections for examples.


#### Responses:

Expected:

| Code   | Type                |
|--------|---------------------|
| 200 OK | ProjectResponseList |


Exception:

| Code                      | Type          | Comment |
|---------------------------|---------------|---------|
| 400 Bad Request           | ResourceError |         |
| 401 Unauthorized          | ResourceError |         |
| 500 Internal Server Error | ResourceError |         |


### [NullResponse](#NullResponse)

#### DELETE /projects/{id}

Delete the project with the given project id. <br/> Please refer to [Sample
Request/Responses](/api/req_n_resp.md) sections for examples.


#### Request parameters:

| Name   | Type   | Source                             | Options | Description                      |
|--------|--------|------------------------------------|---------|----------------------------------|
| id     | UUID   | path                               |         | The id of the project to delete. |
| wssid  | String | header: X-YahooWSSID-Authorization |         |                                  |
| cookie | String | header: Cookie                     |         |                                  |


#### Responses:

Expected:

| Code           | Type |
|----------------|------|
| 204 No Content |      |


Exception:

| Code                      | Type          | Comment |
|---------------------------|---------------|---------|
| 400 Bad Request           | ResourceError |         |
| 401 Unauthorized          | ResourceError |         |
| 404 Not Found             | ResourceError |         |
| 409 Conflict              | ResourceError |         |
| 500 Internal Server Error | ResourceError |         |


#### POST /projects/{id}/hookCallback

For github Webhook callback <br/> When the request is for the event type we
can’t support. <br/> Currently we only support `push`.


#### Request parameters:

| Name  | Type                                  | Source | Options | Description            |
|-------|---------------------------------------|--------|---------|------------------------|
| id    | UUID                                  | path   |         | The id of the project. |
| event | [PushEventRequest](#pusheventrequest) | body   |         | github webhook payload |


#### Responses:

Expected:

| Code           | Type |
|----------------|------|
| 204 No Content |      |


Exception:

| Code                      | Type          | Comment |
|---------------------------|---------------|---------|
| 401 Unauthorized          | ResourceError |         |
| 404 Not Found             | ResourceError |         |
| 409 Conflict              | ResourceError |         |
| 500 Internal Server Error | ResourceError |         |
| 501 Not Implemented       | ResourceError |         |


#### POST /projects/{id}/sync

Force Apex to sync with the project data on git. <br/> Please refer to [Sample
Request/Responses](/api/req_n_resp.md) sections for examples.


#### Request parameters:

| Name   | Type   | Source                             | Options | Description            |
|--------|--------|------------------------------------|---------|------------------------|
| id     | UUID   | path                               |         | The id of the project. |
| wssid  | String | header: X-YahooWSSID-Authorization |         |                        |
| cookie | String | header: Cookie                     |         |                        |


#### Responses:

Expected:

| Code           | Type |
|----------------|------|
| 204 No Content |      |


Exception:

| Code                      | Type          | Comment |
|---------------------------|---------------|---------|
| 400 Bad Request           | ResourceError |         |
| 401 Unauthorized          | ResourceError |         |
| 404 Not Found             | ResourceError |         |
| 409 Conflict              | ResourceError |         |
| 500 Internal Server Error | ResourceError |         |


#### POST /projects/{id}/promotion

#### Request parameters:

| Name    | Type                                              | Source                             | Options | Description |
|---------|---------------------------------------------------|------------------------------------|---------|-------------|
| id      | UUID                                              | path                               |         |             |
| request | [PromotionCreateRequest](#promotioncreaterequest) | body                               |         |             |
| wssid   | String                                            | header: X-YahooWSSID-Authorization |         |             |
| cookie  | String                                            | header: Cookie                     |         |             |


#### Responses:

Expected:

| Code           | Type |
|----------------|------|
| 204 No Content |      |


Exception:

| Code                      | Type          | Comment |
|---------------------------|---------------|---------|
| 400 Bad Request           | ResourceError |         |
| 401 Unauthorized          | ResourceError |         |
| 403 Forbidden             | ResourceError |         |
| 404 Not Found             | ResourceError |         |
| 409 Conflict              | ResourceError |         |
| 500 Internal Server Error | ResourceError |         |


## Types

### <a name="DateTime"></a> DateTime

DateTime string that follows ISO 8601 UTC format, e.g.2013-03-06T11:00:00Z

`DateTime` is a `String` type.


### <a name="ProjectRequest"></a> ProjectRequest

The request parameter used in project onboarding/creation and update.
<br/><br/> - Each and every field are required for creation (not null), and
can not be blank. <br/> - The fields are allowed to be null on update but not
blank. If the field value is null for update, the field will be kept
unchanged. <br/> - Please refer to  [Sample
Request/Responses](/api/req_n_resp.md) sections for examples. <br/> -
**Implementation note**: Regex is used to verify the input is not blank and
escape '\\' twice for rdl & java

`ProjectRequest` is a `Struct` type with the following fields:

| Name        | Type   | Options | Description                                                                                                                                                                  | Notes |
|-------------|--------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------|
| repoUrl     | String |         | The ssh url to the repository where the RDL source and documents resides. <br/> Should be in the format of `git@git.corp.yahoo.com:${orgname}/${reponame}.git#${branchName}` |       |
| name        | String |         | The name of the api.                                                                                                                                                         |       |
| custodian   | String |         | The custodian/contact of the api, should be a `@yahoo-inc` address. An ilist is preferred.                                                                                   |       |
| description | String |         | Long description of the api.                                                                                                                                                 |       |


### <a name="ProjectCreateRequest"></a> ProjectCreateRequest
`ProjectCreateRequest` is a `Struct` type with the following fields:

| Name        | Type   | Options | Description                                                                                                                                                                  | Notes                                    |
|-------------|--------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------|
| repoUrl     | String |         | The ssh url to the repository where the RDL source and documents resides. <br/> Should be in the format of `git@git.corp.yahoo.com:${orgname}/${reponame}.git#${branchName}` | [from [ProjectRequest](#projectrequest)] |
| name        | String |         | The name of the api.                                                                                                                                                         | [from [ProjectRequest](#projectrequest)] |
| custodian   | String |         | The custodian/contact of the api, should be a `@yahoo-inc` address. An ilist is preferred.                                                                                   | [from [ProjectRequest](#projectrequest)] |
| description | String |         | Long description of the api.                                                                                                                                                 | [from [ProjectRequest](#projectrequest)] |
| isOpenApi   | Bool   |         | Indicate this document will be published or not. It can't be changed on update request.                                                                                      |                                          |


### <a name="ProjectResponse"></a> ProjectResponse

The project response object used in GET operation

`ProjectResponse` is a `Struct` type with the following fields:

| Name               | Type                  | Options | Description                                                                                                              | Notes |
|--------------------|-----------------------|---------|--------------------------------------------------------------------------------------------------------------------------|-------|
| id                 | UUID                  |         | System generated unique id.                                                                                              |       |
| name               | String                |         | The name of the project. As specified on project creation.                                                               |       |
| custodian          | String                |         | The custodian of the project. As specified on project creation.                                                          |       |
| description        | String                |         | The description of the project. As specified on project creation.                                                        |       |
| repoUrl            | String                |         | The repository of the project. As specified on project creation.                                                         |       |
| repoOrg            | String                |         | The organization of the project repository, specified in `repoUrl` on project creation.                                  |       |
| repoName           | String                |         | The name of the project repository, specified in `repoUrl` on project creation.                                          |       |
| repoBranch         | String                |         | The branch of the project repository, specified in `repoUrl` on project creation.                                        |       |
| repoHeadSha        | String                |         | The sha of the project repository.                                                                                       |       |
| docUrl             | String                |         | The url to the document-hosting page, ex: https://git.corp.yahoo.com/pages/ApexHosting/project_cat/                      |       |
| reviewUrl          | String                |         | The url to the api review jive page, ex: https://yahoo.jiveon.com/thread/17141, as specified in the project config file. |       |
| splunkDashboardUrl | String                |         | The url to the api splunk dashboard, as specified in the project config file.                                            |       |
| yamasUrl           | String                |         | The url to the yamas dashboard, as specified in the project config file.                                                 |       |
| testHostUrl        | String                |         | The test host url, as specified in the project config file.                                                              |       |
| webHookId          | Int64                 |         |                                                                                                                          |       |
| status             | String                |         | The status of the project.                                                                                               |       |
| message            | String                |         | Error messages if encounters document generation error                                                                   |       |
| docGeneratedTs     | [DateTime](#datetime) |         | The date time when the doc is generated.                                                                                 |       |
| isOpenApi          | Bool                  |         | Indicate this document will be published or not                                                                          |       |
| createdTs          | [DateTime](#datetime) |         | The date time when the project is created.                                                                               |       |
| updateTs           | [DateTime](#datetime) |         | The date time when the project is updated.                                                                               |       |


### <a name="ProjectResponseList"></a> ProjectResponseList

A collection of [ProjectResponse](#projectresponse)

`ProjectResponseList` is a `Struct` type with the following fields:

| Name     | Type                                             | Options | Description | Notes |
|----------|--------------------------------------------------|---------|-------------|-------|
| projects | Array&lt;[ProjectResponse](#projectresponse)&gt; |         |             |       |


### <a name="NullResponse"></a> NullResponse

Represents response when there is no content.

`NullResponse` is a `Struct` with no specified fields


### <a name="PushEventRequestRepoInfo"></a> PushEventRequestRepoInfo

Used only in PushEventRequest reference:
https://developer.github.com/v3/activity/events/types/#pushevent

`PushEventRequestRepoInfo` is a `Struct` type with the following fields:

| Name           | Type   | Options | Description | Notes |
|----------------|--------|---------|-------------|-------|
| default_branch | String |         |             |       |


### <a name="PushEventRequest"></a> PushEventRequest

Represents the github webhook payload. Currently we only need these 2 fields.
<br/> reference:
https://developer.github.com/v3/activity/events/types/#pushevent

`PushEventRequest` is a `Struct` type with the following fields:

| Name       | Type                                                  | Options | Description                                     | Notes |
|------------|-------------------------------------------------------|---------|-------------------------------------------------|-------|
| ref        | String                                                |         |                                                 |       |
| after      | String                                                |         | The latest sha                                  |       |
| repository | [PushEventRequestRepoInfo](#pusheventrequestrepoinfo) |         | repository, need to get the default branch info |       |


### <a name="SearchResponse"></a> SearchResponse
`SearchResponse` is a `Struct` type with the following fields:

| Name               | Type   | Options | Description | Notes |
|--------------------|--------|---------|-------------|-------|
| projectId          | String |         |             |       |
| projectName        | String |         |             |       |
| projectDescription | String |         |             |       |
| projectDocUrl      | String |         |             |       |
| projectRepoUrl     | String |         |             |       |
| sectionName        | String |         |             |       |
| sectionUrl         | String |         |             |       |
| content            | String |         |             |       |
| custodian          | String |         |             |       |


### <a name="SearchResponseList"></a> SearchResponseList
`SearchResponseList` is a `Struct` type with the following fields:

| Name            | Type                                           | Options | Description | Notes |
|-----------------|------------------------------------------------|---------|-------------|-------|
| searchResponses | Array&lt;[SearchResponse](#searchresponse)&gt; |         |             |       |
| resultsTotal    | Int32                                          |         |             |       |
| nextOffset      | Int32                                          |         |             |       |


### <a name="PromotionCreateRequest"></a> PromotionCreateRequest
`PromotionCreateRequest` is a `Struct` type with the following fields:

| Name  | Type | Options | Description | Notes |
|-------|------|---------|-------------|-------|
| cmrId | UUID |         | the cmr id. |       |


[Swagger URL](https://git.corp.yahoo.com/pages/ApexTest/Swagger-UI/parsec/swagger-ui/?url=https://git.corp.yahoo.com/pages/ApexTest/project_1503989039030/swagger-json/apex_swagger.json)
