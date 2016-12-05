<!-- -*-mode:markdown-*- -->

## 基本功能
```
服务环境 WeiXin
└─┬ 要接入服务的公众号 GitChat
  └─┬ 接入评论系统的页面 Article
    ├─┬ 从页面获得文章id
    │ ├── 读取(fetch / query)文章评论信息 包括各字段
    │
    ├── 从页面获得作者id
    ├─┬─ 从页面获得当前访问用户的信息 用户id, 用户名, 从页面获得文章id; 获得时间戳
    │ ├── 记录当前用户信息, 用于提交评论
    │
    ├─┬─ 用户提交评论
    │ ├── 无用户信息时, <记录当前页面状态,> 向用户显示登录页, 登录更新用户信息, <恢复当前页面状态>
    │ └── 提交评论信息
    │
    ├─┬─ 用户对评论投票 (up/like)
    │ ├── 读取该评论投票信息, 检查该用户id是否已对该评论投票, 如已投票返回提示信息
    │ ├── 提交评论投票信息
    │
    ├─┬─ 评论框
    │ ├── 显示当前用户的用户名, 头像等信息
    
评论后台管理系统

```

## 评论的存储
使用 sqlite

评论信息 用于直接展示
```
# comment table
| id | foreign_article_id | ordinal | user_id | up_count | timestamp | comment_content |

# ordinal: 帖子内评论编号
```

用户信息表
```
# user info
| id | image | state|
```

评论投票表
```
# comment_vote table
| id | foreign_articel_id | foreign_comment_id | up_user_id_list |
```

图片存储 (base64 或外部url)


## 接入
- *div.id = comment_root* 提供接入点
- 在接入点后载入js代码, 载入评论脚本.
- 根据所接入页面 location, 采用相应方法读取文章与用户信息.
    - \[新浪微博，QQ空间，腾讯微博，人人网，豆瓣，开心网，网易微博，搜狐微博，百度\] [ref: 多说](http://duoshuo.com/features/?feature_id=0)


## 评论 layout
<img alt='用户头像' style='width:128px,height:128px'> 用户名<br>

评论内容<br>
timestamp | up\_count | 操作 (if (comment.user\_id === current\_user\_id) 显示所有者操作项)


## 其他
- 对评论的评论
    - 回复评论

- 引用评论内容

- 被顶起来的评论 
    - if (被顶次数 > 设定的次数) 在顶部单独的div中显示
    

(待考虑怎么实现)
- 被@时的提醒功能 (谢贤哲)

- 实时提醒
- 评论流实时显示
- 单层、盖楼、层级模式


## 问题

不使用一个环境自带评论系统的原因?
是否因为没有自带的评论系统, 还是需要对评论系统有更多控制.

### 细节问题

多说
- 我们官网的评论会被恶意发贴，而且这个发贴人并不是注册的会员。网址是：[http://store.idx.com.cn/idxsstore]

评论系统权限
[畅言](http://changyan.kuaizhan.com/static/help/)
```
<div id="SOHUCS"></div>
<script ... src="changyan.sohu.com/upload/changyan.js"></script>
<script>window.changyan.api.config({ appid:'cyrHC88dv', conf:'prod_...' });</script>
```

## note from Wanglei
```
|--------+---------+---------|
| WeiXin | GitChat | Comment |
|--------+---------+---------|
|        | name    |         |
|        | id      |         |
|        | pic     |         |
|--------+---------+---------|

sync()
id
pic

                .
                getList
                postList

                                .
                                mysql
                                sqlite
                                (complex system)

Mapping from Named to comment system

host <-> group 群组
article 文章 <-> post 帖子
回复 <-> 回帖
```

<style>
    pre { background: #f7f7f7 }
</style>
