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
                Id：1
		    Title: '好好学习',   
		    Content:'cdjcndjnvjbfvkbfskvbfkjvbfkv',
	         },{},{}]
    }

##### 7 书评详情页 /bookreview/details
    Metohd: Post
    Request：  
       参数名        必选M/可选O         类型           说明
	   Id               M              int           书评id
    Response:{  
                 Id:1,
		     User:'门户名',
		     Title: '好好学习',   
		     Content:'cdjcndjnvjbfvkbfskvbfkjvbfkv',
	   	     Time: 2019_03_11 17:51，
                 Live: 10,
                 Comment:[ 
                           {
                             people: 'lxp'
		                 content:'bbbbbb'
		                 time：2019_03_11 18:05:12
                           },{
                             people: 'xps'
		                 content:'cccccc'
		                 time：2019_03_11 18:00:23
                           },{},{}.... 
                         ]
    }
    
#####  8 点赞 /like/creata
	Metohd: POST
	Request：  
	      参数名        必选M/可选O      类型           说明
		Id               M           int           书评id
	Response:{
	       Msg: "ok"
	}

##### 9 取消点赞 /like/cancel
	Metohd: POST
	Request：  
	      参数名        必选M/可选O      类型           说明
		 Id              M           int           书评id
	Response:{
	       Msg: "ok"
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
                bookmark：[ {
                             Id:1,
				 User:'门户名',
				 Title: '好好学习',   
				 Content:'cdjcndjnvjbfvkbfskvbfkjvbfkv',
				 Time: 2019_03_11 17:51，
			         Live: 10
                            },{},{}
			              ] 
                或者
                bookreview：[{ 
                              Id:1,
			          User:'门户名',
	 			  Source: '人民公报',   
				  Content:'cdjcndjnvjbfvkbfskvbfkjvbfkv',
				  Time: 2019_03_11 17:51
                            },{},{}
                          ]
    }

##### 12 个人资料 /user/details/get
    Metohd: Get
    Request：  
       无
    Response:{ 
                 Name:'dd'       
		     Sex: 1          
		     Email:'983316419@qq.com'       
		     Phone:15521501175        
		     Birth:1990-10       
		     Signature:"do ffff"
                           
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
        [ {用户1的所有信息}，{用户2}，{用户3} ]
    }
