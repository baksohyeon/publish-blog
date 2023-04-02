---
category: Project/API
share: true
title: WIP) ERD, API 설계
---
# ERD 설계하기


[Flowchart Maker & Online Diagram Software](https://app.diagrams.net/#Hbaksohyeon%2Ftutorial%2Fmaster%2F%EB%85%BC%EB%A6%AC%EC%A0%81%20%EB%8D%B0%EC%9D%B4%ED%84%B0%20%EB%AA%A8%EB%8D%B8%EB%A7%81)

> [!note]-  참고 페이지
> - [[ERD CLOUD] ☁️ ERD 다이어그램을 온라인에서 그려보자](https://inpa.tistory.com/entry/ERD-CLOUD-%E2%98%81%EF%B8%8F-ERD-%EB%8B%A4%EC%9D%B4%EC%96%B4%EA%B7%B8%EB%9E%A8%EC%9D%84-%EC%98%A8%EB%9D%BC%EC%9D%B8%EC%97%90%EC%84%9C-%EA%B7%B8%EB%A0%A4%EB%B3%B4%EC%9E%90#:~:text=ERD%20CLOUD%20%EC%82%AC%EC%9A%A9%EB%B2%95,-%EB%A8%BC%EC%A0%80%20%EC%82%AC%EC%9D%B4%ED%8A%B8%EC%97%90&text=%EB%A1%9C%EA%B7%B8%EC%9D%B8%EC%9D%84%20%ED%95%98%EB%A9%B4%20%EC%9D%B4%EC%A0%9C%20%EB%82%98%EB%A7%8C,%EC%B2%B4%ED%81%AC%EB%A5%BC%20%ED%95%B4%EC%A3%BC%EA%B3%A0%20%EC%A0%80%EC%9E%A5%ED%95%B4%EC%A4%80%EB%8B%A4.)
> - [[DB] 📈 데이터 모델링 개념 & ERD 다이어그램 그리기 💯 총정리](https://inpa.tistory.com/entry/DB-%F0%9F%93%9A-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EB%AA%A8%EB%8D%B8%EB%A7%81-1N-%EA%B4%80%EA%B3%84-%F0%9F%93%88-ERD-%EB%8B%A4%EC%9D%B4%EC%96%B4%EA%B7%B8%EB%9E%A8)

yaml 작성
api 중에서 2개 먼저 만들어보기
api 쉬워보이는거 만들어보기 test 코드랑 같이 짜면서 하기

---
## 초대 기능 관련 데이터 스키마 설계
- [Reddit - Dive into anything](https://www.reddit.com/r/Database/comments/x03332/help_with_database_design_for_group_invitations/)
- [database design - How do I create a user invitation model? - Database Administrators Stack Exchange](https://dba.stackexchange.com/questions/22660/how-do-i-create-a-user-invitation-model)
- [Group Invites | Bp-rest-api Handbook Handbook | BuddyPress Developer Resources](https://developer.buddypress.org/bp-rest-api/reference/user-groups/group-invites/)
- [Invite | Schoology Developers](https://developers.schoology.com/api-documentation/rest-api-v1/invite#Fields)
- [https://laracasts.com/discuss/channels/eloquent/database-schema-for-users-belonging-to-groups-with-roles](https://laracasts.com/discuss/channels/eloquent/database-schema-for-users-belonging-to-groups-with-roles)
- [database recommendation - Design for membership of a group - Database Administrators Stack Exchange](https://dba.stackexchange.com/questions/175516/design-for-membership-of-a-group)

This schema assumes that each user can belong to multiple groups, and each group can have multiple members.

A group can have multiple users, and a user can have multiple groups

```sql
SELECT users.*
FROM users
JOIN user_group ON users.id = user_group.user_id
WHERE user_group.group_id = :group_id AND user_group.role = 'member';
```


해당 테이블이 왜 필요한지
N:M 이 필요한건지
왜 그 그룹 멤버 테이블이 필요한지
실제 코드에서 어떻게 활용될 것인지

## Tag 관련 데이터 스키마 설계
[Tags: Database schemas | Random Howtos](http://howto.philippkeller.com/2005/04/24/Tags-Database-schemas/)

---

# api 설계

HTTP Status: `200 (OK)`
HTTP Status: `201 (Created)`
HTTP Status: `202 (Accepted)`
HTTP Status: `204 (No Content)`

| Status Code | Description                                                                                            |
| ----------- | ------------------------------------------------------------------------------------------------------ |
| 200         | The request was processed successfully.                                                                |
| 204         | The request was processed successfully without any data to return.                                     |
| 4xx         | The request was processed with an error. The request was invalid and should not be retried unmodified. |
| 5xx         | The request failed due to a server error, it's safe to retry later.                                    |




# Error codes
| Code   | Description                    |
|--------|--------------------------------|
| 401    | Unauthorized                   |
| 403    | Forbidden                      |
| 404    | Not Found                      |
| 422    | Unprocessable Entity           |
| 500    | Internal Server Error          |

## 401 - Unauthorized
| Description                                                            |     |
| ---------------------------------------------------------------------- | --- |
| Authentication failed: request expired                                 |     |
| Authentication failed: workspace missing                               |     | 
| Authentication failed: key missing                                     |     |
| Authentication failed: property 'iss' (issuer) missing in JWT          |     |
| Authentication failed: property 'sub' (subject) missing in JWT         |     |
| Authentication failed: property 'exp' (expiration time) missing in JWT |     |
| Authentication failed: incorrect signature                             |     |


## 403 - Forbidden
| Description                                                         |     | 
| ------------------------------------------------------------------- | --- |
| Your account has exceeded the monthly document generation limit.    |     |
| Access not granted: You cannot delete master workspace via API      |     |
| Access not granted: Template is not accessible by this organization |     |
| Your session has expired, please close and reopen the editor.       |     |
## 404 Entity not found
| Description                                           |     | 
| ----------------------------------------------------- | --- |
| Entity not found                                      |     |
| Resource not found                                    |     |
| None of the templates is available for the workspace. |     |
## 422 Unprocessable Entity
| Description                                                 |     | 
| ----------------------------------------------------------- | --- |
| Unable to parse JSON, please check formatting               |     |
| Required parameter missing                                  |     |
| Required parameter missing: template definition not defined |     |
| Required parameter missing: template not defined            |     |



v1) 딱! 집안일 목록 생성한 다음에 해당 집안일에 가정 내 멤버를 할당하고 모든 구성원끼리 공유해서 볼 수 있는 기능만 구성

Global Prefix: `api`
Header: `userId`

## auth
### GET auth/google/login 
> 로그인 요청 api



### GET auth/google/redirect
> 로그인 완료 후 리다이렉트 되는 api

**Response**: User
```json
HTTP status: 201 (Created)
JWT cookie
user entity return
```

### POST auth/logout
> 로그아웃

**Request**
`Header`: Bearer Token Header → <font style="color:#6082B6">userId: "uuid-example-user-1"</font>
```yaml
Header: "uuid-example-user-1"
```

- **Response**: OK
[openapi-directory/openapi.yaml at 26dfdf711308b7782f5c16517e93ea5ea6464d77 · APIs-guru/openapi-directory · GitHub](https://github.com/APIs-guru/openapi-directory/blob/26dfdf711308b7782f5c16517e93ea5ea6464d77/APIs/pdfgeneratorapi.com/3.1.1/openapi.yaml)

[https://github.dev/APIs-guru/openapi-directory/blob/26dfdf711308b7782f5c16517e93ea5ea6464d77/APIs/pdfgeneratorapi.com/3.1.1/openapi.yaml](https://github.dev/APIs-guru/openapi-directory/blob/26dfdf711308b7782f5c16517e93ea5ea6464d77/APIs/pdfgeneratorapi.com/3.1.1/openapi.yaml)


[트위터 api](https://github.com/APIs-guru/openapi-directory/blob/main/APIs/twitter.com/current/2.21/openapi.yaml)
## User 관리
### GET /user/profile
> 유저 로그인 정보를 가져옴

**Request**
`Header`: Bearer Token Header → <font style="color:#6082B6">userId: "uuid-example-user-1"</font>
```yaml
Header: "uuid-example-user-1"
```

**Response**: User
```json
{
    "user": {
        "id": "58eecdc0-e671-420c-bbd5-135e5d12c971",
        "email": "bori12370@gmail.com",
        "username": "박 소현                                              ",
        "createdAt": "2023-02-07T08:13:31.709Z",
        "updatedAt": "2023-02-07T08:13:31.709Z",
        "deletedAt": null
    },
    "memberInfo": [
        {
            "role": "Admin",
            "houseId": 6,
            "houseName": "test house name"
        },
        {
            "role": "Admin",
            "houseId": 8,
            "houseName": "new house name"
        },
        {
            "role": "Admin",
            "houseId": 9,
            "houseName": "가나다라"
        },
        {
            "role": "Admin",
            "houseId": 10,
            "houseName": "디스코"
        },
        {
            "role": "Admin",
            "houseId": 12,
            "houseName": "hello world"
        },
        {
            "role": "Admin",
            "houseId": 13,
            "houseName": "cup of tea"
        },
        {
            "role": "Member",
            "houseId": 14,
            "houseName": "bubble tea"
        }
    ]
}
```

### GET /user/house
> 유저가 속한 house 목록을 불러옴

**Request**
`Header`: Bearer Token Header → <font style="color:#6082B6">userId: "uuid-example-user-1"</font>
```yaml
Header: "uuid-example-user-1"
```

**Response**: 
```json
[
	{
		"houseId": 1,
		"name": "dorito's house",
		"role": "Admin",
		created_at: "2022-12-11T22:36:50.000000Z",
		updated_at: "2023-02-11T22:36:50.000000Z",
		deleted_at: null,
	},
]
```

![[Pasted image 20230222212525.png]]
## House 그룹 관리
### POST /house/create
> 로그인한 user가 admin인 house 그룹을 새로 만듬

**Request**
`Header`: Bearer Token Header → <font style="color:#6082B6">userId: "uuid-example-user-1"</font>
```yaml
Header: "a7899a69-c3c9-467b-af2b-095e0f082b7c"
```

```json
body: {
	house_name:  "only Dorito is Admin"
	}
```

**Response**: House
Status Code: `201 (Created)`
```json
{
    "role": "Admin",
    "houseId": 1,
    "userId": "a7899a69-c3c9-467b-af2b-095e0f082b7c",
    "house": {
        "id": 1,
        "name": "only Dorito is Admin",
        "createdAt": "2023-02-24T04:21:05.332Z",
        "updatedAt": "2023-02-24T04:21:05.332Z",
        "deletedAt": null
    },
    "user": {
        "id": "a7899a69-c3c9-467b-af2b-095e0f082b7c",
        "email": "bori12370@gmail.com",
        "username": "Dorito                                            ",
        "createdAt": "2023-02-24T03:34:28.159Z",
        "updatedAt": "2023-02-24T04:19:49.462Z",
        "deletedAt": null
    },
    "deletedAt": null,
    "id": 1,
    "createdAt": "2023-02-24T04:21:05.332Z",
    "updatedAt": "2023-02-24T04:21:05.332Z"
}
```

### GET /house/all
> 로그인한 유저의 House 전체 목록 불러옴

**Request**
```yaml
Header: "uuid-example-user-1"
```


**Response**: House
Status Code: `200 (OK)`
```json
[
	{
		house_id: 1234,
		house_name: "house name",
		created_at: "2022-02-01T22:36:50.000000Z",
		updated_at: "2022-02-10T22:36:50.000000Z",
		deleted_at: null,
	}
]
```

### GET /house/{:house_id}
> House 이름, 생성일자, 이름 정보 및 멤버 목록 불러옴

**Request**
```yaml
Header: "uuid-example-user-1"
parameter: house_id
	example: 1234
```

```json
body: {
	house_name: "updated house name"
}
```

**Response**: House
Status Code: `200 (OK)`
```json
{
    "id": 14,
    "name": "bubble tea",
    "createdAt": "2023-02-16T13:57:14.446Z",
    "updatedAt": "2023-02-16T13:57:14.446Z",
    "deletedAt": null,
    "houseMembers": [
        {
            "id": 15,
            "role": "Admin",
            "houseId": 14,
            "userId": "290a6cd6-79eb-48c4-ad58-82c4b82cfd4e",
            "createdAt": "2023-02-16T13:57:14.471Z",
            "updatedAt": "2023-02-16T13:57:14.471Z",
            "deletedAt": null,
            "user": {
                "id": "290a6cd6-79eb-48c4-ad58-82c4b82cfd4e",
                "email": "meetmigo@gmail.com",
                "username": "남극 미고                                             ",
                "createdAt": "2023-02-14T13:17:51.034Z",
                "updatedAt": "2023-02-14T13:17:51.034Z",
                "deletedAt": null
            }
        },
        {
            "id": 17,
            "role": "Member",
            "houseId": 14,
            "userId": "58eecdc0-e671-420c-bbd5-135e5d12c971",
            "createdAt": "2023-02-19T14:15:17.021Z",
            "updatedAt": "2023-02-19T14:15:17.021Z",
            "deletedAt": null,
            "user": {
                "id": "58eecdc0-e671-420c-bbd5-135e5d12c971",
                "email": "bori12370@gmail.com",
                "username": "박 소현                                              ",
                "createdAt": "2023-02-07T08:13:31.709Z",
                "updatedAt": "2023-02-07T08:13:31.709Z",
                "deletedAt": null
            }
        }
    ]
}
```


🔥 New
GET house/{:houseId}/admin
```json
{
    "id": 6,
    "role": "Admin",
    "houseId": 7,
    "backlog": "No Tasks",
    "createdAt": "2023-02-09T11:34:29.566Z",
    "updatedAt": "2023-02-09T11:34:29.566Z",
    "deletedAt": null,
    "user": {
        "id": "58eecdc0-e671-420c-bbd5-135e5d12c971",
        "email": "bori12370@gmail.com",
        "username": "박 소현                                              ",
        "createdAt": "2023-02-07T08:13:31.709Z",
        "updatedAt": "2023-02-07T08:13:31.709Z",
        "deletedAt": null
    }
}
```

```ts
  async getHouseAdmin(houseId: number) {
    return this.houseMemberRepository.findOneOrFail({
      relations: {
        user: true,
      },
      where: {
        houseId: houseId,
        role: 'Admin',
      },
    });
  }
```
### PUT /house/{:house_id}/update
> Admin인 유저가 house 이름을 변경함

**Request**
```yaml
Header: "uuid-example-user-1"
parameter: house_id
	example: 1234
```

```json
body: {
	house_name: "updated house name"
}
```

**Response**: House
Status Code: `200 (OK)`
```json
{
	house_id: 1234,
	house_name: "updated house name",
	created_at: "2022-02-01T22:36:50.000000Z",
	updated_at: "2022-02-10T22:36:50.000000Z",
	deleted_at: null,
}
```


### DELETE /house/{:house_id}
> Admin인 유저가 house 를 삭제함


```yaml
Header: "uuid-example-user-1"
parameter: house_id
	example: 1234
```

Status Code: `204 (No Content)`


## house 멤버 초대
### POST house/{:house_id}/member/invite
> 행동: 해당 house에 초대하고 싶은 유저의 email 을 입력하면 해당 email에 해당하는 user를 새로운 멤버로 초대한다.

**Request**
`Header`: Bearer Token Header → <font style="color:#6082B6">userId: "uuid-example-user-1"</font>
```yaml
Header: "uuid-example-user-1"
parameter: house_id
	example: 1234
```

- body
```json
"invite": {
	"email": "dorito@example.com", // userID : "uuid-example-user-2"
	"groupId": int,
}
```

**Response**: Invitation
```json
"invitation": {
	"id": 1,
	"house_id": 1234,
	"sender_user_id": "uuid-example-user-1",
	"reciever_user_id": "uuid-example-user-2",
	"status": "Pending",
	created_at: "2022-02-01T22:36:50.000000Z",
	updated_at: "2022-02-01T22:36:50.000000Z",
	deleted_at: null,
}
```

해당하는 유저가 없을 경우: 

`PERMISSION_DENIED`: 요청하는 사용자에게 이 과정 또는 액세스 오류에 대한 초대를 만들 수 없는 경우
`NOT_FOUND`: 과정 또는 사용자가 없는 경우
`FAILED_PRECONDITION`: 요청한 사용자의 계정이 사용 중지된 경우
사용자에게 이미 이 역할 또는 더 높은 권한을 가진 역할이 있는 경우
다음 요청 오류의 경우:
요건 미충족
`ALREADY_EXISTS`: 지정된 사용자 및 과정의 초대가 이미 있는 경우

### PUT house/{:house_id}/member/invite/join
> 초대에 대한 응답을 갱신 및 추가한다.  (Accept/Decline)
> Invitation DB status 업데이트
> House Member DB 업데이트

**Request**
`Header`: Bearer Token Header → <font style="color:#6082B6">userId: "uuid-example-user-1"</font>
```yaml
Header: "uuid-example-user-1"
parameter: 
	name: house_id
		type: int
		example: 1234
	name: invitation_id
		type: int
		example: 341
	
```

- body
```json
"body": {
	status: "Accepted"
}
```

**Response**:
HTTP Status: `200 (OK)`

```json
"invitation": {
	"id": 341,
	receiver_user_id: "uuid-example-user-2",
	"status": "Accepted",
}
```


## house 멤버 관리
### GET house/{:house_id}/members
> 	행동: house에 속한 멤버들을 모두 읽는다.

**Request**
`Header`: Bearer Token Header → <font style="color:#6082B6">userId: "uuid-example-user-1"</font>
```yaml
Header: "uuid-example-user-1"
parameter: house_id
	example: 1234
```

**Response**: 
```json
[
    {
	    "id": 1,
        "username": "dorito",
        "role": "Admin",
        "email": "dorito@email.com",
        "backlog": "Tasks to do",
		created_at: "2022-12-11T22:36:50.000000Z",
		updated_at: "2023-02-11T22:36:50.000000Z",
		deleted_at: null,
    },
    {
	    "id": 1,
        "username": "DudaDuda",
        "role": "Member",
        "email": "duda@email.com",
        "backlog": "No Tasks",
		created_at: "2022-12-11T22:36:50.000000Z",
		updated_at: "2023-02-11T22:36:50.000000Z",
		deleted_at: null,
    }
]
```



### DELETE /house/{:house_id}/members/{:user_id} 
> house 내 userId 멤버 제거 (또는 탈퇴) 
> 본인이 직접 나가야 함 (추가적으로 구현할 것: admin 권한이 있는 사람이 관리할 수 있음)


**Request**
`Header`: Bearer Token Header → <font style="color:#6082B6">userId: "uuid-example-user-1"</font>
```yaml
Header: "uuid-example-user-1"
parameter: 
	name: house_id
		type: number
		example: 1234
```

```json
body: {
	userId: "uuid-example-user-3"
}
```

**Response**
HTTP Status: `204 (No Content)`



fffffffffffdfdfdfffffffffffffffffffffffffffffffffffffdffdfffff## House housework 리스트 관리
### Get house/{:house_id}/house-chore
> 행동: 유저가 속한 House의 housework 가져옴

**Request**
`Header`: Bearer Token Header → <font style="color:#6082B6">userId: "uuid-example-user-1"</font>
```yaml
Header: "uuid-example-user-1"
parameter: 
	name: house_id
		type: number
		example: 1234
```

**Response**: 
```json
[
	{
		id: 1,
		house_id: 1234,
		title: '변기 청소',
		frequency: 'weekly',
		section: ["Bathroom"],
		created_at: "2022-12-11T22:36:50.000000Z",
		updated_at: "2023-02-11T22:36:50.000000Z",
		deleted_at: null,
	}
]
```

### POST /house/{:house_id}/housework
> 행동: house에 집안일 목록에 새로운 집안일을 추가한다.

- **Request**
`Header`: Bearer Token Header → <font style="color:#6082B6">userId: "uuid-example-user-1"</font>
```yaml
Header: "uuid-example-user-1"
parameter: 
	name: house_id
		type: number
		example: 1234
```

```json
{
	title: "설거지",
	frequency: "daily",
	section_id: [4],
}
```

- **Response**
HTTP Status: `201 (Created)`
```json
{
	id: 2,
	house_id: 1234,
	title: "설거지",
	frequency: 'weekly',
	section: ["kitchen"],
	created_at: "2022-12-11T22:36:50.000000Z",
	updated_at: "2023-02-11T22:36:50.000000Z",
	deleted_at: null,
}
```


### PUT /house/{:house_id}/housework/update
> 행동: house에 해당 집안일을 새롭게 업데이트 한다.

- **Request**
`Header`: Bearer Token Header → <font style="color:#6082B6">userId: "uuid-example-user-1"</font>
```yaml
Header: "uuid-example-user-1"
parameter: 
	name: house_id
			type: number
		example: 1234
```

```json
body: {
	house_chore_id: 1,
	title: "updated house chore",
	section_id:
	section: ["bathroom", "Living Room"],
}
```

- **Response**
HTTP Status: `200 (OK)`
```json
{
	house_chore_id: 1,
	house_id: 1234,
	creator: "uuid-example-user-1",
	title: "updated house chore",
	section: ["bathroom", "Living Room"],
	created_at: "2022-12-11T22:36:50.000000Z",
	updated_at: "2023-02-11T22:36:50.000000Z",
	deleted_at: null,
}
```

###  DELETE /house/{:house_id}/housework/{:houseworkId}
> 행동: house에 집안일 목록을 삭제한다.

- **Request**
`Header`: Bearer Token Header → <font style="color:#6082B6">userId: "uuid-example-user-1"</font>
```yaml
Header: "uuid-example-user-1"
parameter: 
	name: house_id
		type: number
		example: 1234
	name: houseworkId
		type: number
		expample: 1
```

- **Response**
HTTP Status: `204 (No Content)`



```
 {
  targetName: 'HouseParams',
  property: 'houseId',
  object: HouseParams { houseId: '14', _requestContext: { user: [User] } },
  value: '14',
  constraints: undefined
}


{
  targetName: 'HouseParams',
  property: 'houseId',
  object: HouseParams { houseId: '14', _requestContext: { user: [User] } },
  value: '14',
  constraints: undefined
}
```

## House 단위 Housework 관리

### GET /house/{:house_id}/housework
> 집안일 리스트 모두 가져오기

req
houseid = 14

resoponse
```json
[
    {
        "id": 1,
        "title": "바닥 닦기",
        "frequency": null,
        "houseId": 14,
        "createdAt": "2023-02-17T07:53:46.443Z",
        "updatedAt": "2023-02-17T07:53:46.443Z",
        "deletedAt": null
    },
    {
        "id": 2,
        "title": "화장실 청소",
        "frequency": null,
        "houseId": 14,
        "createdAt": "2023-02-17T07:58:19.434Z",
        "updatedAt": "2023-02-17T07:58:19.434Z",
        "deletedAt": null
    }
]
```

### GET /house/{:house_id}/housework/{:housework_id}

```json
{
    "id": 3,
    "title": "빨래 돌리기",
    "frequency": 3,
    "houseId": 14,
    "createdAt": "2023-02-17T08:14:59.452Z",
    "updatedAt": "2023-02-17T08:47:32.474Z",
    "deletedAt": null
}
```

### Put  /house/{:house_id}/housework
```json
{
    "houseworkId": 3,
    "title": "빨래 돌리기"   
}
```

### Post /house/{:house_id}/housework


```json
body: {
	{
	    "houseId": 14,
	    "title": "화장실 청소"
	}
}
```
response
```json
{
    "title": "화장실 청소",
    "houseId": 14,
    "deletedAt": null,
    "id": 2,
    "createdAt": "2023-02-17T07:58:19.434Z",
    "updatedAt": "2023-02-17T07:58:19.434Z"
}
```


### DELETE /house/{:house_id}/housework

```json
{
    "houseId": 14,
    "houseWorkId": 3
}
```


---
## House 단위 task 관리
### GET /house/{:house_id}/task
> 행동: house_id 의 house에서 유저에게 할당된 전체 Task 얻어옴 (user id = assginee_user_id)

**Request**
`Header`: Bearer Token Header → <font style="color:#6082B6">userId: "uuid-example-user-1"</font>
```yaml
Header: "uuid-example-user-1"
```

**Response**: Chore Task[]
```json
[
	{
		id: 1,
		house_chore_id: 1,
		creator_user_id: "uuid-example-user-1",
		assignee_user_id: "uuid-example-user-2",
		assigner_user_id: "uuid-example-user-3",
		house_id: 1234,
		is_completed: false,
		due_date: "2023-01-31T22:36:50.000000Z",
		created_at: "2022-12-11T22:36:50.000000Z",
		updated_at: "2023-02-11T22:36:50.000000Z",
		deleted_at: null
	  },
]
```


### POST /house/{:house_id}/task/create
> 행동: 해당 house 에서 house chore 목록을 보고 유저가 새로운 Task를 생성

**Request**
`Header`: Bearer Token Header → <font style="color:#6082B6">userId: "uuid-example-user-1"</font>
```yaml
Header: "uuid-example-user-1"
parameter: 
	name: house_id
		type: number
		example: 1234
```

```json
body: {
	house_chore_id: 20,
	}
```

**Response**: Chore Task
status: `201(create)`
```json
{
    "creatorUserId": "58eecdc0-e671-420c-bbd5-135e5d12c971",
    "houseworkId": 2,
    "isCompleted": false,
    "housework": {
        "id": 2,
        "title": "화장실 청소",
        "frequency": null,
        "houseId": 14,
        "createdAt": "2023-02-17T07:58:19.434Z",
        "updatedAt": "2023-02-17T08:35:17.628Z",
        "deletedAt": null
    },
    "deletedAt": null,
    "id": 1,
    "createdAt": "2023-02-20T07:31:24.046Z",
    "updatedAt": "2023-02-20T07:31:24.046Z"
}
```

### PUT /house/{:house_id}/task/update
> 행동: 기존 task 내용 업데이트 (due_date, house_chore_id, assgin 관련 정보)

**Request**
`Header`: Bearer Token Header → <font style="color:#6082B6">userId: "uuid-example-user-1"</font>

```yaml
Header: "uuid-example-user-1"
parameter: 
	name: house_id
		type: number
		example: 1234
```

```json
body: {
	task_id: 1,
	house_chore_id: 21,
	due_date: "2023-01-31T22:36:50.000000Z",
	assigner_id: "uuid-example-user-1",
	assignee_id: "uuid-example-user-2"
}
```

**Response**: Chore Task
status: `200(OK)`
```json
{
    "id": 1,
    "houseworkId": 3,
    "isCompleted": false,
    "housework": {
        "id": 3,
        "title": "빨래 돌리기",
        "frequency": 3,
        "houseId": 14,
        "createdAt": "2023-02-17T08:14:59.452Z",
        "updatedAt": "2023-02-17T08:47:32.474Z",
        "deletedAt": null
    },
    "assigneeUserId": "58eecdc0-e671-420c-bbd5-135e5d12c971",
    "assignerUserId": "290a6cd6-79eb-48c4-ad58-82c4b82cfd4e",
    "deletedAt": null,
    "updatedAt": "2023-02-20T07:55:21.911Z"
}
```

### POST /house/{:house_id}/task/close
> 행동: 기존 task의 is_completed 을 true 로 변경함




**Request**
`Header`: Bearer Token Header → <font style="color:#6082B6">userId: "uuid-example-user-1"</font>
```yaml
Header: "uuid-example-user-1"
parameter: 
	name: house_id
		type: number
		example: 1234
```

```json
body: {
	task_id: 1,
}
```

```json
body: {
	task_id: 1,
	is_completed: true,
}
```

**Response**
Status: `204 (No Content)`



### POST /house/{:house_id}/task/reopen
> 행동: 기존의 task의 is_completed을 true 에서 false로 다시 갱신함

**Request**
`Header`: Bearer Token Header → <font style="color:#6082B6">userId: "uuid-example-user-1"</font>
```yaml
Header: "uuid-example-user-1"
parameter: 
	name: house_id
		type: number
		example: 1234
```

```json
body: {
	task_id: 1,
}
```

**Response**
HTTP Status: `204 (No Content)`
```json
body: {
	task_id: 1,
	is_completed: false,
}
```



### DELETE /house/{:house_id}/task/
> 기존 task 삭제


**Request**
`Header`: Bearer Token Header → <font style="color:#6082B6">userId: "uuid-example-user-1"</font>
```yaml
Header: "uuid-example-user-1"
parameter: 
	name: house_id
		type: number
		example: 1234
```

```json
body: {
	task_id: 1,
	is_completed: false,
}
```

**Response**
HTTP Status: `204 (No Content)`

---
이후 추가할 기능
## Section 관리
> 부엌, 안방 등 섹션 관리


### GET /house/{:house_id}/housework/section
> 행동: sections 전체 얻어옴 

**Request**
`Header`: Bearer Token Header → <font style="color:#6082B6">userId: "uuid-example-user-1"</font>
```yaml
Header: "uuid-example-user-1"
```

**Response** Section
```json
[
    {
        "id": "2",
        "name": "Bathroom",
    }
    ...
]
```



### POST /house/{:house_id}/housework/section/create
> 행동: 새로운 Section 생성

**Request**
`Header`: Bearer Token Header → <font style="color:#6082B6">userId: "uuid-example-user-1"</font>
```yaml
Header: "uuid-example-user-1"
parameter: 
	name: house_id
		type: number
		example: 1234
```

```json
body: {
	title: "Laundry Room",
}
```

**Response**: 
HTTP Status: `201 (Created)`
```json
{
	id: 3,
	title: "Laundry Room",
}
```

### PUT /house/{:house_id}/housework/section
> 행동: section id 로 기존 Section 업데이트

**Request**
`Header`: Bearer Token Header → <font style="color:#6082B6">userId: "uuid-example-user-1"</font>
```yaml
Header: "uuid-example-user-1"
parameter: 
	name: house_id
		type: number
		example: 1234
```

```json
body: {
	id: 3,
	title: "update"
}
```

**Response**: 
HTTP Status: `200 (OK)`
```json
body: {
	id: 3,
	title: "update"
}
```


### DELETE /house/{:house_id}/housework/section
> 행동: section id 로 기존 Section 삭제

**Request**
`Header`: Bearer Token Header → <font style="color:#6082B6">userId: "uuid-example-user-1"</font>
```yaml
Header: "uuid-example-user-1"
parameter: 
	name: house_id
		type: number
		example: 1234
```

```json
body: {
	id: 3
}
```

**Response**: 
HTTP Status: `204 (No Content)`

# 추가로 생각해야 할 것

> [!info] 추가할 것
> - User 전체 Task 관리
> - House Member 각 개인 멤버 페이지에 들어가서 할 일 할당하는 api 구현하기

---
# Schema
## Chore Task
```json
{
	id: 1,
	house_chore_id: 1,
	creator_user_id: "uuid-example-user-1",
	assignee_user_id: null,
	assigner_user_id: null,
	house_id: 1234,
	is_completed: false,
	due_date: "2023-01-31T22:36:50.000000Z",
	created_at: "2022-12-11T22:36:50.000000Z",
	updated_at: "2023-02-11T22:36:50.000000Z",
	deleted_at: null
  }
```

## User
```json
{
	"id": 1,
	"userId": "uuid-example-user-1",
	"name": "Alice",
	"email": "alice@example.com",
	created_at: "2022-12-11T22:36:50.000000Z",
	updated_at: "2023-02-11T22:36:50.000000Z",
	deleted_at: null,
}
```

## Invitation
```json
"invitation": {
	"id": 1,
	"house_id": 1234,
	"sender_user_id": "uuid-example-user-1",
	"reciever_user_id": "uuid-example-user-2",
	"status": "Pending",
	created_at: "2022-02-01T22:36:50.000000Z",
	updated_at: "2022-02-01T22:36:50.000000Z",
	deleted_at: null,
}
```


## House Core
```json
{
	id: 1,
	house_id: 1234,
	title: 'cleaning the desk',
	frequency: 'weekly',
	created_at: "2022-12-11T22:36:50.000000Z",
	updated_at: "2023-02-11T22:36:50.000000Z",
	deleted_at: null,
}
```

# Open Api  참고 문서

> [!note]- 참고 자료
> - [OpenAPI 란? (feat. Swagger) - 호롤리한 하루](https://gruuuuu.github.io/programming/openapi/)
> - [OpenAPI 와 Swagger/redoc 란](https://www.lesstif.com/software-engineering/openapi-swagger-redoc-106857823.html)
> - [OpenAPI Documentation](https://oai.github.io/Documentation/)
> - [Simple Server · GitBook](https://goswagger.io/tutorial/todo-list.html)
> - [Get a user's projects – REST API Reference | Todoist Developer](https://developer.todoist.com/rest/v2/#get-a-user-39-s-projects)
> - [REST API 및 OpenAPI : 양자 택일이 아닙니다. - YouTube](https://youtu.be/pRS9LRBgjYg)
> - [OpenAPI 3.0: How to Design and Document APIs with the Latest OpenAPI Specification 3.0 - YouTube](https://youtu.be/6kwmW_p_Tig)

[Roomie-Backend-API-Documentation](https://documenter.getpostman.com/view/4337348/UUxwCov3)

---

house 가 존재하는지 계속 어떻게 보장하지?
일단은 간단하게 생각하자.

진짜 기본적인 CRUD 오늘안에 쳐내자.


UTC 를 클라이언트에서 해당 로컬 기준으로 바꿔주면 된다.
[mysql에서 9시간 차이날때(GCP)](https://velog.io/@taelee/mysql에서-9시간-차이날때GCP)

일단 이거 나중에 참고해보기.


유저 케이스 별로 유저 스토리를 만들어라고
사용사이클 예상 포인트 
그때 저장되어야 할 DB 받아야할 정보는 뭐가 있는지
 예측 되는게 뭐가 있는지
 실제 실행될 때 뭐가 있는지 확인

- role base 단계 `In NestJS, implement role authorization when roles differ by groups`
[Extensible and secure authorization with Nestjs and CASL | by Tri Minh | Money Forward i Engineering](https://mfi.engineering/extensible-and-secure-authorization-with-nestjs-and-casl-c6f6d1ceefd5)

Rather Than Doing this by associating roles in each entity, i would suggest to store Roles in another table, with their id's and make an association table for roles mapping to users, and add it to JwTAuthGuard, that whenever a user sign-in. the auth guards searches for roles that are accessible by the user, and thus limits the access.

[What is Role-Based Access Control | RBAC vs ACL & ABAC | Imperva](https://www.imperva.com/learn/data-security/role-based-access-control-rbac/)


- back office 기능
[Custom authentication · NestJS Admin](https://nestjs-admin.com/docs/custom-authentication)
[GitHub - Theodo-UK/nestjs-admin: A generic administration interface for TypeORM entities](https://github.com/Theodo-UK/nestjs-admin) 라이브러리 쓰면 될듯




attribute-based (ABAC) authorization techniques 



- 페이지네이션
[How to create pagination in Nest.js with TypeORM + Swagger | Adrian Pietrzak: Software Engineering Blog](https://pietrzakadrian.com/blog/how-to-create-pagination-in-nestjs-with-typeorm-swagger)


- backlog 
그냥 개수 업데이트 하면 되지 않나? 할당된 task 개수 세어주면 되잖아

enum으로 할게 아니라 걍 true 개수 세주면 되잖아

![[Pasted image 20230220210211.png]]


이거 backlog 수정해야겠다.