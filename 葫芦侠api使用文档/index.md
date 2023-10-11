# 

# 葫芦侠api使用文档

*文档整理 by七折（tccc）*

## 登录

**提交接口:**http://floor.huluxia.com/user/info/ANDROID/4.1.8

**需提交参数:**

- platform=2[X]
- app_version=APP版本[建议最新版][!]
- gkey=000000[X]
- market_id=登录应用[X][floor_web葫芦侠三楼,floor_tool][更改请自换接口]
- floor_webversioncode=20141481[X]
- _key=[X]
- phone_brand_type=手机品牌类型[!]
- account=账号[!]
- login_type=登录类型[!][推荐2]
- password=密码[请用md5加密]
- sign=符号?

**返回参数说明:**

```json
{
  msg=提交状态
  _key=登录后key
  user=用户信息
  {
    userID=UID
    nick=名称
    avatar=头像地址
    birthday=生日
    age=年龄
    gender=性别id[0未知1女2男]
    level=等级
    identityTitle=称号?
  },
  session_key=用于会话的密钥
  status=状态
}
```



## 个人信息

**提交接口:**http://floor.huluxia.com/user/info/ANDROID/4.1.8

**需提交参数:**

- _key=你的key
- user_id=对方用户UID

**返回参数说明:**

```json
{
  msg=成功值
  userID=对方UID
  nick=用户名称
  avatar=头像
  birthday=生日时间[时间戳]
  age=年龄
  gender=性别
  credits=贡献值
  integral=积分
  integralNick=贡献值名称
  identityTitle=貌似是称号
  level=等级
  exp=经验数量
  praiseCount=点赞数量
  followerCount=关注数量
  postCount=帖子数量
  favoriteCount=收藏数量
  ipAddr=ip地址
  gameCommentCount=游评
  medalList=奖章数
  photos=照骗[滑稽]
    {
      id=照骗序号1
      url=http://cdn.u1.huluxia.com/g4照骗1
      fid=g4/照骗2
      //两个完全一样
    }
    {
      id=照骗序号2
      url=http://cdn.u1.huluxia.com/照骗1
      fid=g4/照骗2
    }
    //一直循环,直到没有图片
  ]
  signature=简介
  hometown=家乡{
    hometown_province_id=家乡省份id
    hometown_city_id=家乡城市id
    hometown_province=家乡省份
    hometown_city=家乡城市
  }
  schoolInfo=学校信息{
    school_name=学校名
    school_enteryear=学校入学年
  }
  workinfo=工作信息{
    work_company=工作公司
    work_position=工作职位
    work_industry=工作行业
    work_industry_detail=工作行业详细信息
  }
  tags=标签
    {
      selected=挑选
      isCustom=0
      status=null
      title=
      seq=0
      fid=1
      id=23
      //这个不要获取,是用来新建替换的
    }
    {
      selected=选择的标签id
      status=状态[null就是无,正常显示]
      title=标签名称
      seq=序列号
      id=ID
    }
    {
      selected=选择的标签id
      status=状态[null就是无,正常显示]
      title=标签名称
      seq=序列号
      id=ID
    }
    //一直循环,直到没有
  ]
  beenLocations=好像是位置信息
  location=位置距离[貌似在开发中?]
  lastLoginTime=登录时间戳
  userRemark=用户备注
  status=状态
}
```


