For API Spec, please refer to [API Spec](/apex.md)

## POST /projects/

### Request
```json
{
    "name": "Apex - The API Portal",
    "description":"Apex contains a set of guidelines, libraries and a protoal website. see https://yo/apex for details.", 
    "repoUrl":"git@git.corp.yahoo.com:apex/api.git#dogfood",
    "custodian":"ec-lightyear-dev@yahoo-inc.com",
    "isOpenApi" : true
}
```

### Responses

#### status 202
```json
{
    "createdTs": "2017-03-28T09:00:14.135Z",
    "custodian": "dev-projcreateSuccess@yahoo-inc.com",
    "description": "Description: createSuccess",
    "docUrl": "https://git.corp.yahoo.com/pages/ApexHosting/Projec-createSuccess",
    "id": "111",
    "name": "Name: createSuccess",
    "repoBranch": "devBranch",
    "repoName": "TestRepo_createSuccess",
    "repoOrg": "TestOrg",
    "repoUrl": "git@git.corp.yahoo.com:TestOrg/TestRepo_createSuccess.git#devBranch",
    "updateTs": "2017-03-28T09:00:14.135Z",
    "webHookId": 0,
    "isOpenApi" : true
}
```


## PUT /projects/{id}

### Request 

```json
{
    "name": "Apex - The API Portal",
    "description":"Apex contains a set of guidelines, libraries and a protoal website. see https://yo/apex for details.", 
    "repoUrl":"git@git.corp.yahoo.com:apex/api.git#dogfood",
    "custodian":"ec-lightyear-dev@yahoo-inc.com"
}
```

#### Responses

#### status 200
```json
{
    "createdTs": "2017-03-28T09:19:01.855Z",
     "custodian": "dev-projupdateSuccess@yahoo-inc.com",
     "description": "Description: updateSuccess",
     "docUrl": "https://git.corp.yahoo.com/pages/ApexHosting/Projec-updateSuccess",
     "id": "updateSuccess",
     "name": "Name: updateSuccess",
     "repoBranch": "devBranch",
     "repoName": "TestRepo_updateSuccess",
     "repoOrg": "TestOrg",
     "repoUrl": "git@git.corp.yahoo.com:TestOrg/TestRepo_updateSuccess.git#devBranch",
     "updateTs": "2017-03-28T09:19:01.855Z",
     "webHookId": 0,
     "isOpenApi" : true
}
```



## DELETE /projects/{id}

### Responses

#### status 204
NULL


## GET /projects/{id}

### Responses

#### status 200
```json
{
	"createdTs": "2017-04-07T02:19:32.01Z",
	"custodian": "dev-projgetSuccess@yahoo-inc.com",
	"description": "Description: getSuccess",
	"docUrl": "https://git.corp.yahoo.com/pages/ApexHosting/Projec-getSuccess",
	"id": 111,
	"name": "Name: getSuccess",
	"repoBranch": "devBranch",
	"repoName": "TestRepo_getSuccess",
	"repoOrg": "TestOrg",
	"repoUrl": "git@git.corp.yahoo.com:TestOrg/TestRepo_getSuccess.git#devBranch",
	"updateTs": "2017-04-07T02:19:32.01Z",
	"webHookId": 0,
	"isOpenApi" : true
}
```



## GET /projects

### Responses
```json
{
	"projectResponseList": [{
		"createdTs": "2017-03-28T09:19:01.423Z",
		"custodian": "dev-projlist1@yahoo-inc.com",
		"description": "Description: list1",
		"docUrl": "https://git.corp.yahoo.com/pages/ApexHosting/Projec-list1",
		"id": 111,
		"name": "Name: list1",
		"repoBranch": "devBranch",
		"repoName": "TestRepo_list1",
		"repoOrg": "TestOrg",
		"repoUrl": "git@git.corp.yahoo.com:TestOrg/TestRepo_list1.git#devBranch",
		"updateTs": "2017-03-28T09:19:01.423Z",
		"webHookId": 0,
		"isOpenApi" : true
	}, {
		"createdTs": "2017-03-28T09:19:01.423Z",
		"custodian": "dev-projlist2@yahoo-inc.com",
		"description": "Description: list2",
		"docUrl": "https://git.corp.yahoo.com/pages/ApexHosting/Projec-list2",
		"id": 222,
		"name": "Name: list2",
		"repoBranch": "devBranch",
		"repoName": "TestRepo_list2",
		"repoOrg": "TestOrg",
		"repoUrl": "git@git.corp.yahoo.com:TestOrg/TestRepo_list2.git#devBranch",
		"updateTs": "2017-03-28T09:19:01.423Z",
		"webHookId": 0,
		"isOpenApi" : false
	}, {
		"createdTs": "2017-03-28T09:19:01.423Z",
		"custodian": "dev-projlist3@yahoo-inc.com",
		"description": "Description: list3",
		"docUrl": "https://git.corp.yahoo.com/pages/ApexHosting/Projec-list3",
		"id": 333,
		"name": "Name: list3",
		"repoBranch": "devBranch",
		"repoName": "TestRepo_list3",
		"repoOrg": "TestOrg",
		"repoUrl": "git@git.corp.yahoo.com:TestOrg/TestRepo_list3.git#devBranch",
		"updateTs": "2017-03-28T09:19:01.423Z",
		"webHookId": 0,
		"isOpenApi" : false
	}]
}
```

## POST /projects/{id}/sync

### Responses

#### status 204
NULL


## POST /projects/{id}/hookCallback

### Request

```json
{
  "ref":"refs/heads/master",
  "after":"c6ae2887c8dc6e466dd95f9788b56996cc83c55d"
}

```

### Response

#### status 204
NULL



## Error Responses

Most of the error responses are in the following format, where `error.code` and `error.message` should provide some details on the root cause. 
Please refer to [Error Codes](/api/error.md) for the complete list of error codes and possible remediation.

```json
{
    "error": {
        "code": 50001,
        "message": "error message"
    }
}
```

Below are some examples categorized by HTTP Status code: 

#### status 400
```json
{
    "error": {
        "code": 0,
            "detail": [{
                "type": "validationError",
                "invalidValue": "  ",
                "message": "must match \"^[a-zA-Z0-9._%+-]+@yahoo-inc.com$\"",
                "messageTemplate": "{javax.validation.constraints.Pattern.message}",
                "path": "ApexResources.createProject.creationRequest.custodian"
            }, {
                "type": "validationError",
                "invalidValue": "  ",
                "message": "must match \"^.*\\S.*$\"",
                "messageTemplate": "{javax.validation.constraints.Pattern.message}",
                "path": "ApexResources.createProject.creationRequest.name"
            }, {
                "type": "validationError",
                "invalidValue": "  ",
                "message": "must match \"^.*\\S.*$\"",
                "messageTemplate": "{javax.validation.constraints.Pattern.message}",
                "path": "ApexResources.createProject.creationRequest.description"
            }, {
                "type": "validationError",
                "invalidValue": "  ",
                "message": "must match \"^.*\\S.*$\"",
                "messageTemplate": "{javax.validation.constraints.Pattern.message}",
                "path": "ApexResources.createProject.creationRequest.repoUrl"
            }],
            "message": "constraint violation validate error"
    }
}
```

#### status 401 
_TBD_

#### status 403
_TBD_

#### status 404
```json
{
	"error": {
		"code": 40400,
		"message": "Unable to find project with the given id. [getFailNotFound]"
	}
}
```

### status 409
```json
{
	"error": {
		"code": 40901,
		"message": "A project with the same repository url already exists."
	}
}
```

#### status 500
```json
{
    "error": {
        "code": 50001,
        "message": "Unable to create project with the given repo. [git@git.corp.yahoo.com:TestOrg/TestRepo_333.git#devBranch]"
    }
}

```
