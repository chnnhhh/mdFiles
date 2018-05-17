
## 接口文档 raw：
### 前台接口
#### 1.筛选查询今日赛程 path：todo

Params:
```
当前时间 currentTime： //暂定不传 使用后台系统时间

范围 scope: 全部 "ALL" | 可投注 "BETTING" //不填默认为ALL

分组 category: 							//不填默认全查询

	赛程分组 A组~H组：GROUP_A~GROUP_H;
	1/8决赛 EIGHTH_FINAL;
	1/4决赛 QUARTER_FINAL;
	半决赛 SEMI_FINAL;
	三四名决赛 BRONZE_FINAL;
	决赛 FINAL;
```

```
example Params:
{
  "scope": "BETTING",
  "category": "QUARTER_FINAL"
};

Returns:
{
  "homeName": "葡萄牙",				//主队名称
  "homePicUrl": "http://xxx.xxx/pty.jpg", //主队图片
  "homeScore": null					//主队得分 为null时表示比赛未结束  比赛结束后会有整数值
  "awayName": "西班牙",				//客队名称
  "awayPicUrl": "http://xxx.xxx/xby.jpg",//客队图片
  "awayScore": null					//客队得分 逻辑同主队得分
  "homeOdds": 1.7,					//主胜赔率
  "awayOdds": 0.9,					//客胜赔率
  "drawOdds": 1.9,					//平局赔率
  "matchStartTime": 1526473886000,  //比赛开始时间
  "bettingStartTime": 1526473886000,//投注开始时间
  "bettingEndTime": 1526473886000,	//投注截止时间
  "matchStatus":"NORMAL"			//HIDE为隐藏,NORMAL为正常 为HIDE时不显示在页面上 只在后台界面显示
}
```
2.用户投注接口

```
Params:
{
  "userId": 96659903984542329, //用户id 必填
  "matchId": 43,  				//比赛赛程id 必填
  "currentOdds":1.7,			//当前赔率 必填
  "bettingPoints": 200 			//投注积分 必填
}

Returns:
//todo 需参考其他写操作的成功返回值
{"isSuccess":true}
```
3.用户查询投注记录
```
Params:
{
	"userId":"96659903984542329" //必填
}
Returns:
[
  {
    "homeName": "英国",
    "awayName": "德国",
    "matchStartTime": 1526473886000,
    "records": [
      {
      	"bettingTo" : "HOME", //投注方向 主场HOME、客场AWAY、平局DRAW
      	"bettingPoints":100,//投注积分
      	"bettingOdds":1.3,//赔率
      	"status": "SETTLED",//状态：待开WAITING、已结算SETTLED、已退回ROLLED
      	"earnings":130 //收益 积分×赔率 大于投注积分时显示 小于显示0
       },
      {
      	"bettingTo":"AWAY", //投注方向 主场HOME、客场AWAY、平局DRAW
      	"bettingPoints":200,//投注积分
      	"bettingOdds":0.8,//赔率
      	"status": "SETTLED",//状态：待开WAITING、已结算SETTLED、已退回ROLLED
      	"earnings":0
      }
    ]
  },
  {
    "homeName": "法国",
    "awayName": "巴西",
    "matchStartTime": 1526473836000,
    "records": [
      {
      	"bettingTo":"HOME", //投注方向 主场HOME、客场AWAY、平局DRAW
      	"bettingPoints":500,//投注积分
      	"bettingOdds":1.3,//赔率
      	"status": "WAITING",//状态：待开WAITING、已结算SETTLED、已退回ROLLED
      	"earnings": null //收益 积分×赔率 大于投注积分时显示 小于显示0 未结算返回null
       }
    ]
  }
]
]
```
4.获取前七条投注记录
```
Params
{}
Returns
{
  "userName": "张xxx",  //投注人姓名 后端进行匿名处理
  "homeName": "德国",   //主场队名
  "awayName": "法国",   //客场队名
  "bettingPoints": 300 //投注积分
}
```
========================================
### 后台接口:
####列出所有赛程 使用筛选查询今日赛程接口 不传参数即可

1.添加比赛赛程
```
Params
{
	"category":"QUARTER_FINAL", 				//分组 同筛选查询中的category取值 必填
	"homeName":"德国",							//主场队名 必填 
	"homePicUrl":"http://xxx.xxx/pty.jpg",		// 必填
	"awayName":"法国", 							// 必填
	"awayPicUrl":"http://xxx.xxx/pty.jpg",		// 必填
	"matchStartTime":1526473836000,				// 必填
	"matchEndTime":1526473886000,				// 必填
	"bettingStartTime":1526473836000,			// 必填
	"bettingEndTime":1526473796000,				// 必填
	"homeOdds":1.7,								//主胜赔率 非必填
	"awayOdds":0.7,								//客胜赔率 非必填
	"drawOdds":1.0 								//平局赔率 非必填
}
Returns:
//todo 需参考其他写操作的成功返回值
{"isSuccess":true}
```
2.修改比赛赛程
```
Params
{	
	"matchId":35, //赛程id 必填项 其他都是非必填项
	"category":"QUARTER_FINAL", //分组 同筛选查询中的category取值
	"homeName":"德国",			
	"homePicUrl":"http://xxx.xxx/pty.jpg",
	"awayName":"法国",
	"awayPicUrl":"http://xxx.xxx/pty.jpg",
	"matchStartTime":1526473836000,
	"matchEndTime":1526473886000,
	"bettingStartTime":1526473836000,
	"bettingEndTime":1526473796000,
	"homeOdds":1.7,
	"awayOdds":0.7,
	"drawOdds":1.0
}
Returns:
//todo 需参考其他写操作的成功返回值
{"isSuccess":true}
```
3.显示和隐藏比赛赛程
```
Params:
{
	"matchId":34,
	"matchStatus":"HIDE"//操作标记 HIDE为隐藏,NORMAL为正常
}
Returns:
//todo 需参考其他写操作的成功返回值
{"isSuccess":true}
```
4.投注结算
```
Params: //全必填
{
	"matchId":12,		 
	"matchResult":"HOME",//比赛结果 主场胜HOME、客场胜AWAY、平局DRAW
	"homeScore":3,		 //主队得分
	"awayScore":4		 //客队得分
}
Returns:
//todo 需参考其他写操作的成功返回值
{"isSuccess":true}
```
5.导出参与用户明细
```
Params:
{
	"matchId":13,//非必填 不传或者传0 认为导出所有比赛的投注记录
	"matchStartTime":1526473886000,
	"matchEndTime":1526478086000
}
Returns:
//todo 文件流
```