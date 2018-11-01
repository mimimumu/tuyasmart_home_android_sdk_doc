## Home Member Management Class

The ITuyaHomeMember provides the family member management interfaces including adding member, removing member, update the control permission of member and obtaining home member list ,etc. The invocation method is TuyaHomeSdk.newMemberInstance(memberId). (Currently, the parameter can be 0 if you want to add members. The initiation and invocation logics will be optimized in the next version.) In the home member management logic, the MemberBean is used mainly as the interface to obtain information of members. 
```java
private long homeId; //Home id
private String nickName;//Name 
private boolean admin;//Administrator or not
private long memberId;//Member id
private String headPic;//Directory of account picture
private String account;//Account of member
private String uid;//Unique id of members
```
## Add Members in a Home
```java
/**
* Add members in home
*
* @param countryCode Country code
* @param userAccount User name
* @param name     Nickname
* @param admin       Have the administrator permission or not.
* @param callback
*/
void addMember(long homeId ,int countryCode, String userAccount, String name, boolean admin, ITuyaMemberResultCallback callback);
```
## Remove Member of a Home
```java
/**
* Remove member of a home
*
* @param id
* @param callback
*/
void removeMember(long memberId, IResultCallback callback);
```
## Change Name and Permission of Members
```java
/**Change name and permission of members
* @param name Name of member. If the initial name is not changed, upload the nickName obtained from the memberBean.
* @param admin  administrator or not.
* @param callback
*/

void updateMember(String name, boolean admin, IResultCallback callback);
```
## Query the Member List of a Home
```java
/**
* Query the member list of a home
*
* @param callback
*/
void queryMemberList(long homeId,ITuyaGetMemberListCallback callback);
```