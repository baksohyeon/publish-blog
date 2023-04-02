---
category: Project/인증 및 인가
share: true
---
# Athorization Role 구현
 출처 [Using CASL and roles with persisted permissions - Rule of Tech](https://ruleoftech.com/2022/using-casl-and-roles-with-persisted-permissions?utm_source=rss&utm_medium=rss&utm_campaign=using-casl-and-roles-with-persisted-permissions)
 [RBAC와 ABAC 의 특징 비교: 정의 및 사용 사례 | Okta Identity Korea](https://www.okta.com/kr/identity-101/role-based-access-control-vs-attribute-based-access-control/)
 Attributes-based access control (ABAC) in addition with roles (RBAC).

In short: you've groups, groups have roles and roles have permission attributes. Well, read forward and I'll explain how to tie things together using `CASL`: an isomorphic authorization JavaScript library which restricts what resources a given client is allowed to access.

[The Hidden Costs of User Authorization](https://cerbos.dev/blog/the-hidden-costs-of-user-authorization)

Two popular types of authorization are: 
1. Role-based access control (RBAC) 
2. Attributes-based access control (ABAC)


**Role based access control (RBAC)** is a simple and efficient solution to manage user permissions in an organization. We have to create roles for each user group and assign permissions for certain operations to each role. This is commonly used practice with roles suchs as "System admin", "Administrator", "Editor" and "User". 
- The drawbacks of RBAC, especially in a complex system, is that a user can have various roles, which require more time and effort to organize. Also the more roles you create, the more security holes will be born.

![Pasted image 20230221224217.png](../../Attachments/Pasted%20image%2020230221224217.png)

[사진 출처](https://cerbos.dev/blog/the-hidden-costs-of-user-authorization) 

**Attributes-based access control (ABAC)** is an authorization mechanism granting access dynamically based on user characteristics, environment context, action types, and more. Here we can have some action which is permitted to certain user, e.g. "

```
{ action: 'edit', subject: 'Registration' }", "{ action: 'manage', subject: 'User', fields: ['givenName', 'familyName', 'email', 'phone', 'role'] }
```

For example, a manager can view only the users in their department; an user can not access his project’s information. 

ABAC is sometimes referred to as claims-based access control (CBAC) or policy-based access control (PBAC). 
You often see ABAC authorization mechanism in cloud computing services such as AWS and Azure.


![Pasted image 20230221224229.png](../../Attachments/Pasted%20image%2020230221224229.png)

- RBAC: 그룹에서 정해진 Role 따라 할 수 있는 일이 달라짐

- ABAC: Role + 유저 속성 등등 에 따라 동적으로 권한 제어할 수 있음



The difference between ABAC and RBAC is that ABAC supports Boolean logic, if the user meets this condition he is allowed to do that action. 

Attribute-based Access Control (ABAC) enables more flexible control of access permissions as you can grant, adjust or revoke permissions instantly.

RBAC is a simple but sufficient solution to manage access in an uncomplicated system. 
However, it is not flexible and scalable and that is why **ABAC becomes an efficient approach in this case**. 
In the contrast, **ABAC may be costly to implement.** 
There is no all-in-one solution and it depends on your business requirements. 
Remember that use the right tool for the right job.

 ---

참고할 코드
- [NetJS + PostgreSQL + JWT + RBAC Policy + Docker](https://www.linkedin.com/pulse/netjs-postgresql-jwt-rbac-policy-docker-cleuber-biango)


- [https://github.dev/Cleuber/nestjs-rbac-postgres-docker](https://github.dev/Cleuber/nestjs-rbac-postgres-docker)

![Pasted image 20230221204042.png](../../Attachments/Pasted%20image%2020230221204042.png)

[IBM Documentation](https://www.ibm.com/docs/en/coss/3.14.5?topic=administration-rbac-model)



---
- Role Based Authorization vs. Claim Based Authorization

[authentication - Role Based Authorization vs. Claim Based Authorization - Information Security Stack Exchange](https://security.stackexchange.com/questions/45270/role-based-authorization-vs-claim-based-authorization)


Claims are a method of providing information about a user, and roles are a description of a user by way of which roles they belong.

Claims are generally more useful because they can contain arbitrary data -- including role membership information. E.g. whatever is useful for the given application.

Claim Based identities are more useful, but tend to be trickier to use because there's a lot of setup involved for acquiring the claims in the first place. RBAC identities are less useful because they are just a collection of roles, but they are generally easier to setup.

The .NET stack, and Windows as a whole, is going claims. Windows authn tickets are claims, and Active Directory now has the ability to use claims for certain functions. The .NET stack uses a claims identity as the base identity object now by default.

[CASL.js](https://casl.js.org/v4/en/cookbook/claim-authorization)


---

[Role-Based Access Control for a Complex Enterprise](https://delinea.com/blog/role-based-access-control-for-a-complex-enterprise)
![Pasted image 20230221213605.png](../../Attachments/Pasted%20image%2020230221213605.png)

How to start or improve an RBAC implementation
To set up an RBAC system, you must:

1. Inventory systems: Create a comprehensive list of every server, database, application, external website, etc. that users might need access to. Optionally, include physical security in this list, covering access to server rooms or other sensitive physical spaces and resources. Create a map of who (or what) currently has access to those systems and resources.

2. Create roles that reflect your organization: Assess current usage patterns throughout your organization and create roles that align with them. Consider when and whether to use hierarchical or constrained RBAC (e.g. determining when to have permission inheritance between roles, vs. enforcing separation of duties). Determine whether to create roles that align with current usage or use newly defined roles to reflect how your organization aspires to work.
   Ensure that roles are aligned with the work that people in that role need to get done so that the roles don’t need to be modified frequently. Don’t forget to create roles for contractors or other third-party users who may need temporary or restricted access and roles for non-human service accounts. As a general rule, create the least amount of access a user or role needs to get their job done and limit the time they have access to perform the tasks.

3. Articulate the RBAC policy: Document the implemented RBAC policy, even if you’re using an automated tool to implement it. Many compliance schemes require access control policies to be thoroughly documented, and to have that documentation undergo a change control process to ensure it remains current. Then, extend that governance to include RBAC within each ap With the shift to cloud infrastructures, SaaS applications, and the convenience of SSO, users and groups commonly inherit roles with excessive permissions. RBAC helps to mitigate this risk.
4. Assign people and accounts to roles: Pragmatically assign users to roles that match their job function, prescribing access with least privilege in mind, and resist pressure to provide users with more access than they need. Implement granular access approval controls that include auditable logs to govern just-in-time access requests.

 5. Audit regularly: Conduct periodic reviews to ensure users are assigned to proper roles (since roles can change over time) and each role has appropriate permissions. Don’t make one-off changes for one employee to satisfy their unique needs. Either update permissions for their role or create a new role as appropriate. Doing so will enable you to manage the system over the long haul.





[node.js - How to Change Reponse Acc to User Role in NestJs - Stack Overflow](https://stackoverflow.com/questions/74549743/how-to-change-reponse-acc-to-user-role-in-nestjs)


---

[TypeScript: Documentation - Mixins](https://www.typescriptlang.org/docs/handbook/mixins.html)
Mixins 가 몬데
[코드 재사용을 위한 Mixin – StonzeBlog](https://stonzeteam.github.io/%EC%BD%94%EB%93%9C-%EC%9E%AC%EC%82%AC%EC%9A%A9%EC%9D%84-%EC%9C%84%ED%95%9C-Mixin/)
[믹스인(MIXIN | TypeScript)](https://velog.io/@from_numpy/믹스인-타입스크립트)
Mixin이란?
Mixin은 전통적인 다중상속을 클래스의 부분부분을 쪼개어 조립하는 것입니다. 하지만 Mixin이 다중상속의 대안으로만 사용되는 것은 아닙니다.

전통적인 클래스 상속을 이용할 때에는 필요하지 않은 부분도 모조리 상속되는 그런 불편함이 있습니다. 하지만 Mixin으로 클래스를 조립한다면 더 깔끔한 구현이 가능하게 됩니다.



믹스인은 다음과 같은 함수이다.
- 생성자(constructor)를 받음
- 생성자를 확장하여 새 기능을 추가한 클래스 생성
- 새 클래스 반환

원리는 단순한데, 클래스 A가 클래스 B를 확장해서 그 기능을 받는 것이 아니라 함수 B가 클래스 A를 받고 기능이 추가된 새 클래스를 반환하는 것이다. 함수 B가 믹스인인 것이다.

---


[API with NestJS #56. Authorization with roles and claims](https://wanago.io/2021/11/15/api-nestjs-authorization-roles-claims/)


[API with NestJS #56. Authorization with roles and claims](https://wanago.io/2021/11/15/api-nestjs-authorization-roles-claims/)


[GitHub - rangle/jwt-role-based-authentication-examples: Implement the same backend using graphql, nestjs and deno.](https://github.com/rangle/jwt-role-based-authentication-examples)


일단 다음에 함