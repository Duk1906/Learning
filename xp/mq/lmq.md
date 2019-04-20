## 刘美琪

### 模型类
##### 1. 书签
		type BookMark struct{
		     Id           int                             唯一标识
			 User         *UserMQ    `orm:"rel(fk)"`      作者
		     Source       string                          出处 
		     Content      string                          内容
		     Pic          string    `orm:"-"`             配图/可选
			 Time         time.Time `orm:"auto_now_add;type(datetime)"`     创建时间 
		}


##### 2. 书评
		type BookReview struct{
		     Id           int                              唯一标识
			 User         *UserMQ    `orm:"rel(fk)"`       作者
		     Title        string                           题目
		     Content      string                           内容
		     Pic          string    `orm:"-"`              配图/可选
			 Time         time.Time `orm:"auto_now_add;type(datetime)"`     创建时间
			 Live         int       `orm:"default(0)"`     点赞数
        }



##### 3. 评论
		type Comment struct{
			 Id           int 
			 BookReview   *BookReview   `orm:"rel(fk)"`             评论的书评
			 People       *UserMQ       `orm:"rel(fk)"`             评论人
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
		    Pic          string     `orm:"-"`               图片
		    Signature    string     `orm:"-"`               签名
		}


### 接口
##### 1. 1 注册 /user/register
    Metohd: POST
    Request：  
	   参数名        必选M/可选O          类型         说明
       num              M               string       账号 
	   pwd              M               string       密码 
    Response:
	    data :{
           msg: "ok"
	    }

### 2 登录 /user/enter
    验证，信息正确则跳转到首页，否则提示错误仍保留在登录页
    Metohd: POST
    Request：   
	   参数名        必选M/可选O          类型         说明
	   num              M               string       账号
	   pwd              M               string       密码
    Response:
	    data :{
           msg: "验证通过"
	    }

### 3 创建书签 /bookmark/create
    Metohd: POST
    Request：  
	   参数名        必选M/可选O         类型           说明
	   source           M               string       出处
	   content          M               string       内容
	   pic              O                            配图/可选/暂时不处理
    Response:
	    data :{
           msg: "ok"
	    }

### 4 首页获取书签 /bookmark/get
    Metohd: Get
    Request：  
	   无
    Response:
	    data :{
               bookmark：[ {  id:1,
					          user:{用户对应的信息},
					          source: '人民公报',   
					          content:'cdjcndjnvjbfvkbfskvbfkjvbfkv',
					          pic:"一个路径，暂时不理",
					          time: 2019_03_11 17:51},{},{}]
               }


### 5 创建书评 /bookreview/create
    Metohd: POST
    Request：  
	   参数名        必选M/可选O         类型           说明
	   title            M               string       题目
	   content          M               string       内容
	   pic              O                            配图/可选/暂时不处理
    Response:
	    data :{
           msg: "ok"
	    }

### 6 书评页获取书评 /bookreview/get
    Metohd: Get
    Request：  
	   无
    Response:
	    data :{ bookreview：[{ id：1
						       title: '好好学习',   
						       content:'cdjcndjnvjbfvkbfskvbfkjvbfkv',
						       pic:"一个路径，暂时不理"},{},{}]
               }

### 7 书评详情页 /bookreview/details
    Metohd: Post
    Request：  
	   id        M               int          书评id
    Response:
	    data :{  id:1,
			     user:{用户对应的信息},
			     title: '好好学习',   
			     content:'cdjcndjnvjbfvkbfskvbfkjvbfkv',
	             pic:"一个路径，暂时不理",
	   	         time: 2019_03_11 17:51，
                 live: 10,
                 comment:[{
		                     people: 'lxp'
		                     content:'bbbbbb'
		                     time：2019_03_11 18:05
                          },{
                             people: 'xps'
		                     content:'cccccc'
		                     time：2019_03_11 18:00
                          },{},{}....]
               }

### 8 个人文墨 /user/article/get
    Metohd: Post
    Request：  
	   kind        M              int           0:书签 1：书评
    Response:
	   data :{  bookmark：[ {id:1,
						     user:{用户对应的信息},
						     title: '好好学习',   
						     content:'cdjcndjnvjbfvkbfskvbfkjvbfkv',
				             pic:"一个路径，暂时不理",
				   	         time: 2019_03_11 17:51，
			                 live: 10},{},{}
			              ] 
                或者
                bookreview：[{ id:1,
					          user:{用户对应的信息},
					          source: '人民公报',   
					          content:'cdjcndjnvjbfvkbfskvbfkjvbfkv',
					          pic:"一个路径，暂时不理",
					          time: 2019_03_11 17:51},{},{}
                          ]
              }

### 9 个人资料 /user/details/get
    Metohd: Get
    Request：  
       无
    Response:
	   data :{ UserDatails：{ name:'dd'       
					          sex: 1          
					          email:'983316419@qq.com'       
					          phone:15521501175        
					          birth:1990-10       
					          signature:"do ffff"
                             }
               }

### 10 角色设置 /user/role/set
    Metohd: Post
    Request：  
       uid         M              int           用户id
       role        M              int           角色
    Response:
	   data :{ 
		      msg:"ok"
               }

### 11 用户列表 /user/list
    Metohd: Get
    Request：  
       无
    Response:
	   data :{users： [ {用户1的所有信息}，{用户2}，{用户3} ]
             }
