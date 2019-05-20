### 模型类
##### 1. 书签
		type BookMark struct{
		         Id           int  
			 UserID       int
			 UserName     string
		         Source       string
		         Content      string
			 Time         string
               }


##### 2. 书评
		type BookReview struct{
                     Id           int  
			 UserID       int
			 UserName     string
		         Title        string
		         Content      string
			 Time         string
			 LikeNum      int       `orm:"default(0)"`
               }



##### 3. 评论
		type Comment struct{
			 Id           int 
			 BookReview   *BookReview   `orm:"rel(fk)"`             评论的书评
			 PeopleId     int                                       评论人id
			 PeopleName   string                                    评论人名字
		         Content      string                                    内容
			 Time         time.Time     `orm:"auto_now_add;type(datetime)"`   时间
        }


##### 4. 用户
		type UserMQ struct{
			Id           int                                用户id
			Num          string                             账号
			Pwd          string                             密码
			Name         string     `orm:"-"`               名字
			Role         int        `orm:"default(0)"`      角色，默认普通用户   0:普通用户  1：管理员
			Sex          int        `orm:"-"`               性别   0：男   1：女
			Email        string     `orm:"-"`               邮箱
			Phone        string     `orm:"-"`               手机号
		        Birth        time.Time  `orm:"-;type(date)"`    生日
		        Signature    string     `orm:"-"`               签名
			Time         string                             创建时间
			
			
		}


### 接口
##### 1 注册 /user/register
    Metohd: POST
    Request：  
	   参数名        必选M/可选O          类型         说明
       Num              M               string       账号 
	   Pwd              M               string       密码 
	   Name             M               string       姓名
    Response:{
         Msg: "ok"
	}

##### 2 登录 /user/enter
    验证，信息正确则跳转到首页，否则提示错误仍保留在登录页
    Metohd: POST
    Request：   
	   参数名        必选M/可选O          类型         说明
	   Num              M               string       账号
	   Pwd              M               string       密码
    Response:{
         Msg: "ok"
	}

##### 3 创建书签 /bookmark/create
    Metohd: POST
    Request：  
	   参数名        必选M/可选O         类型           说明
	   Source           M               string       出处
	   Content          M               string       内容
    Response:{
         Msg: "ok"
	}

###### 4 首页获取书签 /bookmark/get
    Metohd: Get
    Request：  
	   无
    Response:{
	  "List": [
		    {
		      "Id": 1,
		      "UserID": 1,
		      "UserName": "",
		      "Source": "Linux",
		      "Content": "Linux大法红火火恍恍惚惚",
		      "Time": "2019-05-20 20:49:18"
		    },{},{}
	  ]
    }


##### 5 创建书评 /bookreview/create
    Metohd: POST
    Request：  
	   参数名        必选M/可选O         类型           说明
	   Title            M               string       题目
	   Content          M               string       内容
    Response:{
         Msg: "ok"
	}

##### 6 书评页获取书评 /bookreview/get
    Metohd: Get
    Request：  
	   无
    Response:{
	  "List": [
		    {
		      "Id": 1,
		      "UserID": 1,
		      "UserName": "",
		      "Title": "毕业咯",
		      "Content": "\u003cp\u003e毕业咯咯咯咯咯咯咯咯咯咯\u003c/p\u003e\n",
		      "Time": "2019-05-20 20:52:16",
		      "LikeNum": 0
		    },{},{}
	  ]
    }

##### 7 书评详情页 /bookreview/details
    Metohd: Post
    Request：  
       参数名        必选M/可选O         类型           说明
	   Id               M              int           书评id
    Response:{
		  "BookReview": {
			    "Id": 1,
			    "UserID": 1,
			    "UserName": "",
			    "Title": "毕业咯",
			    "Content": "\u003cp\u003e毕业咯咯咯咯咯咯咯咯咯咯\u003c/p\u003e\n",
			    "Time": "2019-05-20 20:52:16",
			    "LikeNum": -1
		  },
		  "Comment": [
		    {
		      "Time": "2019-05-20 21:12:26",
		      "content": "毕业快乐",
		      "people": ""
		    }
		  ]
    }

    
#####  8 点赞 /article/like/creata
	Metohd: POST
	Request：  
	      参数名        必选M/可选O      类型           说明
		Id               M           int           书评id
	Response:{
	       Msg: "ok",
	       Num: 1
	}

##### 9 取消点赞 /article/like/cancel
	Metohd: POST
	Request：  
	      参数名        必选M/可选O      类型           说明
		 Id              M           int           书评id
	Response:{
	       Msg: "ok",
	       Num: 0
	}

##### 10 评论/article/comment/create
	Metohd: POST
	Request：  
	      参数名        必选M/可选O      类型           说明
		Id     M            int            书评id
		Content        M            string         内容
	Response:{
	       Msg: "ok"
	}

##### 11 个人文墨 /user/article/get
    Metohd: Post
    Request：  
       参数名        必选M/可选O         类型           说明
	   Kind             M              int           0:书签 1：书评
    Response:{  
                "List": [
			    {
			      "Id": 1,
			      "UserID": 1,
			      "UserName": "",
			      "Source": "Linux",
			      "Content": "Linux大法红火火恍恍惚惚",
			      "Time": "2019-05-20 20:49:18"
			    },{},{}
		  ]
                或者
                "List": [
			    {
			      "Id": 1,
			      "UserID": 1,
			      "UserName": "",
			      "Title": "毕业咯",
			      "Content": "\u003cp\u003e毕业咯咯咯咯咯咯咯咯咯咯\u003c/p\u003e\n",
			      "Time": "2019-05-20 20:52:16",
			      "LikeNum": 0
			    },{},{}
                ]
    }

##### 12 个人资料 /user/details/get
    Metohd: Get
    Request：  
       无
    Response:{ 
             "Msg": "ok",
	         "Data": {
		    "Id": 1,
		    "Num": "gbt",
		    "Pwd": "123456",
		    "Name": "",
		    "Role": 0,
		    "Sex": 0,
		    "Email": "",
		    "Phone": "",
		    "Birth": "",
		    "Signature": "",
		    "Time": "2019-05-20 20:41:59"
             }                         
    }

##### 13 角色设置 /user/role/set
    Metohd: Post
    Request：  
       Uid         M              int           用户id
       Role        M              int           角色
    Response:{ 
	      Msg:"ok"
    }

##### 14 用户列表 /user/list
    Metohd: Get
    Request：  
       无
    Response:{
	  "List": [
		    {
		      "Id": 1,
		      "Num": "gbt",
		      "Pwd": "123456",
		      "Name": "",
		      "Role": 0,
		      "Sex": 0,
		      "Email": "",
		      "Phone": "",
		      "Birth": "",
		      "Signature": "",
		      "Time": "2019-05-20 20:41:59"
		    },{},{}
	  ]
}

##### 15 修改用户信息 /user/detail/modify
    Metohd: Post
    Request：  
       参数名        必选M/可选O         类型           说明
       Email            M               string
       Birth            M               string
       Phone            M               string
       Pwd              M               string
       Role             M               int            0：普通用户    1：管理员
       Sex              M               int            0：男          1：女
       Signature        M               string  
    Response:{
	  Msg:"ok"
}
