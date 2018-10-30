### 家庭成员管理类
ITuyaHomeMember提供了家庭成员管理接口，包括添加、删除成员，更新成员的控制权限、获取家庭成员列表等.调用方式:`TuyaHomeSdk.newMemberInstance(memberId)`(目前如果调用添加成员，此方法传参可传0，将在下个版本优化初始化和调用逻辑).家庭成员管理逻辑主要提供MemberBean用于获取成员信息的接口
```java
	private long homeId; //家庭id
    private String nickName;//备注名
    private boolean admin;//是否是管理员
    private long memberId;//成员id
    private String headPic;//头像地址
    private String account;//成员账户名称
    private String uid;//成员唯一标识id
```

#### Home下面添加成员

```java
    /**
     * 给这个Home下面添加成员
     *
     * @param countryCode 国家码
     * @param userAccount 用户名
     * @param name        昵称
     * @param admin       是否拥有管理员权限
     * @param callback
     */
    void addMember(long homeId ,int countryCode, String userAccount, String name, boolean admin, ITuyaMemberResultCallback callback);

```

#### 移除Home下面的成员

```java
   /**
     * 移除Home下面的成员
     *
     * @param id
     * @param callback
     */
    void removeMember(long memberId, IResultCallback callback);
```

#### 更新成员备注名和权限
```java
/**
 * 更新成员备注名和权限
 * @param name 备注名 如果不更改备注名，传入从memberBean获取的nickName
 * @param admin  是否是管理员
 * @param callback
 */
void updateMember(String name, boolean admin, IResultCallback callback);
```

#### 查询Home下面的成员列表

```java
   /**
     * 查询Home下面的成员列表
     *
     * @param callback
     */
    void queryMemberList(long homeId,ITuyaGetMemberListCallback callback);
```