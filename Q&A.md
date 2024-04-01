### Q&A

#### 1.多次开启监测，是否会产生多份数据

答：不会，最后一次的会覆盖之前开启的监测，即只会保留一份

ps：不建议多次开启，即开启一份取收取一份

#### 2.token问题

答：第一次绑定连接后使用(0,0,0,0,0,0)，会需要摇晃绑定。绑定成功后回调出输出token。可以使用此token跳过摇晃

#### 3.为什么没有睡眠分期数据

答：睡眠分期数据只有睡眠超过四个小时才会输出

#### 4.报告收取流程

答：开启监测=>监测关闭=>收取数据

ps：监测关闭的几种情况1.低电结束 2.充电结束 3.异常结束

#### 5.开启后杀掉app、小程序数据是否存在

答：开启监测成功后，指环开始工作，只要未连接进行收取，数据会一直存储在指环

ps：再次开启会覆盖指环内的数据，即指环只会存储一份数据

#### 6.为什么安卓搜索到任何指环

答：因系统限制需要开启定位和蓝牙

#### 7.连接失败

答：指环需要和设备距离不要太远，并且不要有阻隔物

ps：受限于apple系统原因，连接距离更短，所以连接失败后离近一点再次尝试。或者冲一下电再次尝试

#### 8.指环低电电量是多少

答：不同批次不一致，大概是15%左右

ps：低电状态是指环返回的状态，要以返回的状态为准

#### 9.开启实时通道是什么意思

答：开启后可以实时（开启的实时血氧、睡眠监测的数据）收取数据信息（不是收取报告数据）

#### 10.mode模式都是什么

 答：睡眠模式（1） 实时⾎氧模式（4）⽇常模式（3）默认模式（0）

#### 11.电量模式

答： 0-电量正常 1-充电中 2-充满 3-低电 4-异常 5-休眠

#### 12.实时数据值是否可用

 答：0-实时值有效 1-值准备中 2-无效/离手

#### 13.实时睡眠监测的数据含义

答：duration 时长（s） pr：心率  spo：血氧  status：实时数据值是否可用

#### 14.实时血氧数据含义

答：pr：心率  spo：血氧  status：实时数据值是否可用

#### 15.app开启后，小程序也是那个状态

答：app开启监测，小程序连接后也会是这个监测。因为开启的是在指环，小程序连接后也是从指环拿取状态。

#### 16.为什么我拿到的脉诊不是解析后的数据

答：脉诊输出的是原始数据

### 备注

1. 低电禁止任何操作
2. 充电只可以进行收取数据，禁止其他任何操作
3. 实时血氧不会存储数据，即每次进入需要再次开启（需要注意备注1、备注2）
4. 不使用脉诊 必须要关闭掉(和其他模式不可同时开启)
5. 脉诊设置时间后，输出时间可能有延迟

### 文档

#### 1.脉率

```
SleepTimeStatic.prAvg 平均脉率(bpm)
SleepTimeStatic.prMax 最大脉率(bpm)
SleepTimeStatic.prMin 最小脉率(bpm)
```

#### 2.睡眠

```
SleepMinutes-总睡眠时长(分钟)
lightsleepMinutes-浅睡期时长(分钟)
deepsleepMinutes-深睡期时长(分钟)
REMMinutes-REM期时长(分钟)
```

#### 3.脉率图

```
pr
```

#### 4.血氧图

```
spo2
```

#### 5.睡眠分期图

````
status 1-深睡期 2-浅睡期 3-眼动期 4-清醒期 0-离床\清醒期
````

#### 6.睡眠分期统计

```
睡眠阶段 持续时间（分钟）总监测时间占比 总睡眠时间占比

清醒期 wakeMinutes  wakeMinutes/(SleepMinutes+wakeMinutes) --

眼动期 REMMinutes  REMMinutes /(SleepMinutes+wakeMinutes)  
REMMinutes/SleepMinutes

浅睡期 lightsleepMinutes lightsleepMinutes/(SleepMinutes+wakeMinutes) binData.lightsleepMinutes/binData.SleepMinutes

深睡期 deepsleepMinutes deepsleepMinutes/(SleepMinutes+wakeMinutes) deepsleepMinutes/binData.SleepMinutes
```

#### 7.氧减时间统计、氧减事件时间统计

```
ODI3SEDist:{
  //10-60
	Less100Cnt,//次数
	Less100Percent//占比
}
```

#### 8.氧减事件统计、氧减事件分布

````
//60-100
ODI3SEDist:{
	Less100Cnt,//次数
	Less100Percent//占比
}
````

#### 9.血氧饱和度统计、血氧饱和度占比

````
SleepTimeStatic:{
 	spo2Less60Time，时长
  spo2Less60TimePercent 占比
} 
````

#### 10.监测意见

![img](file:////Users/xiaobuding/Library/Containers/com.kingsoft.wpsoffice.mac/Data/tmp/wps-xiaobuding/ksohtml//wps1.jpg) 

##### 1.取值

```
总睡眠：SleepMinutes、

睡眠效率TST/TIB：(SleepMinutes / (SleepMinutes + wakeMinutes) * 100)、

深睡期占比：(deepsleepMinutes / SleepMinutes * 100)、

快速眼动期占比：(REMMinutes / SleepMinutes * 100)、

浅睡期占比：(lightsleepMinutes / SleepMinutes * 100)、

ODI：diffThdLge3Pr、

夜间平均血氧饱和度： SleepTimeStatic.Spo2Avg、

最齐血氧饱和度：SleepTimeStatic.Spo2Min、
```

##### 2.Spo2判断：

```
1. >=90 无低氧血症,
2. >=85&&<90 符合<轻度>低氧血症 ,
3. >=80&&<85 符合<中度>低氧血症,
4. <80 符合<重度>低氧血症
```

##### 3.ODI判断：

```
1. >=0&&<5 无睡眠呼吸暂停及低通气综合症 

2. >5&&<=15 符合<轻度>睡眠呼吸暂停及低通气综合症 

3. >=15&&<30 符合<中度>睡眠呼吸暂停及低通气综合症 

4. >=30 符合<重度>睡眠呼吸暂停及低通气综合症
```

#### 接口数据例子

```
{
    "duration": 1000,//总时长
    "status": [],//分期数组 array [ 60*24 ] 1分钟  0 / 1 清醒、2 眼动、3 浅睡、4 深睡、6 离手
    "length_status": 385,//分期数组长度
    "bin_start_sec": 1698262633,// 开始时间
    "bin_stop_sec": 1698285757,// 结束时间
    "startpos": 0, //有效数据偏移----开始时间
    "endpos": 13141, //有效数据偏移----结束时间
    "originDataType": 1,//转化前的报告类型
    "dataType": 1,
    "Spo2EvtVect4": [ //氧减事件的数组(ODI4)
        {
            "startpos": 4582,
            "len": 45
        }
    ],
    "Spo2EvtVect3": [//氧减事件的数组(ODI3)
        {
            "startpos": 4225,
            "len": 29
        }
    ],
    "ODI4SETime": {
        "Less10sCnt": 0, //时间少于10秒的事件个数
        "Less20sCnt": 0,
        "Less30sCnt": 1,
        "Less40sCnt": 1,
        "Less50sCnt": 1,
        "Less60sCnt": 0,
        "Longer60sCnt": 0,
        "Less10sPercent": 0,//时间大于60秒的事件占比
        "Less20sPercent": 0,
        "Less30sPercent": 33.33333206176758,
        "Less40sPercent": 33.33333206176758,
        "Less50sPercent": 33.33333206176758,
        "Less60sPercent": 0,
        "Longer60sPercent": 0
    },
    "ODI4SEDist": { //ODI4 结果
        "Less100Cnt": 0, //血氧低于100高于95的事件个数
        "Less95Cnt": 2, //血氧低于100高于95的事件占比
        "Less90Cnt": 1,
        "Less85Cnt": 0,
        "Less80Cnt": 0,
        "Less75Cnt": 0,
        "Less70Cnt": 0,
        "Less65Cnt": 0,
        "Less60Cnt": 0,
        "Less100Percent": 0,
        "Less95Percent": 66.66666412353516,
        "Less90Percent": 33.33333206176758,
        "Less85Percent": 0,
        "Less80Percent": 0,
        "Less75Percent": 0,
        "Less70Percent": 0,
        "Less65Percent": 0,
        "Less60Percent": 0
    },
    "ODI3SETime": {
        "Less10sCnt": 0, //时间少于10秒的事件个数
        "Less20sCnt": 0,
        "Less30sCnt": 2,
        "Less40sCnt": 1,
        "Less50sCnt": 2,
        "Less60sCnt": 0,
        "Longer60sCnt": 0,
        "Less10sPercent": 0,//时间少于10秒的事件占比
        "Less20sPercent": 0,
        "Less30sPercent": 40,
        "Less40sPercent": 20,
        "Less50sPercent": 40,
        "Less60sPercent": 0,
        "Longer60sPercent": 0
    },
    "ODI3SEDist": { //ODI3 结果
        "Less100Cnt": 1, //血氧低于100高于95的事件个数
        "Less95Cnt": 3,
        "Less90Cnt": 1,
        "Less85Cnt": 0,
        "Less80Cnt": 0,
        "Less75Cnt": 0,
        "Less70Cnt": 0,
        "Less65Cnt": 0,
        "Less60Cnt": 0,
        "Less100Percent": 20,//血氧低于100高于95的事件占比
        "Less95Percent": 60,
        "Less90Percent": 20,
        "Less85Percent": 0,
        "Less80Percent": 0,
        "Less75Percent": 0,
        "Less70Percent": 0,
        "Less65Percent": 0,
        "Less60Percent": 0
    },
    "diffThdLge3Cnts": 5, //氧减事件个数（氧减次数）
    "diffThdLge3Pr": 1.9607843160629272, // ODI3（氧减指数）
    "maxSpo2DownTime3": 48,////最大氧减时间
    "diffThdLge3Prw": 1.807228922843933,  //ODI3, 加w后缀的是对整晚数据，不做分期的统计
    "diffThdLge4Cnts": 3,//氧减事件个数
    "diffThdLge4Pr": 1.1764706373214722, //ODI4
    "maxSpo2DownTime4": 45,////最大氧减时间
    "diffThdLge4Prw": 1.0843373537063599,//ODI4, 加w后缀的是对整晚数据，不做分期的统计
    "wakeMinutes": 66, //清醒期时间(分钟)
    "REMMinutes": 31, //眼动期时间(分钟)
    "lightsleepMinutes": 112,//浅睡期时间(分钟)
    "deepsleepMinutes": 10,//深睡期时间(分钟)
    "SleepMinutes": 153, //睡眠时长
    "VldTestMinutes": 166,
    "FallSMinutes": 13,//入睡等待时长
    "wakeInSMinutes": 52,//入睡后觉醒时长
    "TotalTimeStatic": { // 总时间内产生的数据( 血氧饱和度数据,血氧统计,脉率统计 )
        "spo2Less100Time": 0,// 血氧低于100的时间
        "spo2Less95Time": 878, //血氧低于95的时间
        "spo2Less90Time": 9,
        "spo2Less85Time": 0,
        "spo2Less80Time": 0,
        "spo2Less75Time": 0,
        "spo2Less70Time": 0,
        "spo2Less65Time": 0,
        "spo2Less60Time": 0,
        "spo2Less100TimePercent": 0, 
        "spo2Less95TimePercent": 8.815260887145996, // 血氧低于100的占比
        "spo2Less90TimePercent": 0.09036144614219666,
        "spo2Less85TimePercent": 0,
        "spo2Less80TimePercent": 0,
        "spo2Less75TimePercent": 0,
        "spo2Less70TimePercent": 0,
        "spo2Less65TimePercent": 0,
        "spo2Less60TimePercent": 0,
        "Spo2Avg": 96.53956604003906,////平均血氧
        "Spo2Min": 89.20576477050781,	//最低血氧
        "prAvg": 65,//平均心率
        "prMax": 107, //最大心率
        "prMin": 31 //最小心率
    },
    "SleepTimeStatic": { // 睡眠期间产生的数据( 血氧饱和度数据,血氧统计,脉率统计 )
        "spo2Less100Time": 0, // 血氧低于100的时间
        "spo2Less95Time": 861,
        "spo2Less90Time": 9,
        "spo2Less85Time": 0,
        "spo2Less80Time": 0,
        "spo2Less75Time": 0,
        "spo2Less70Time": 0,
        "spo2Less65Time": 0,
        "spo2Less60Time": 0,
        "spo2Less100TimePercent": 0,// 血氧低于100的占比
        "spo2Less95TimePercent": 9.379084587097168,
        "spo2Less90TimePercent": 0.09803921729326248,
        "spo2Less85TimePercent": 0,
        "spo2Less80TimePercent": 0,
        "spo2Less75TimePercent": 0,
        "spo2Less70TimePercent": 0,
        "spo2Less65TimePercent": 0,
        "spo2Less60TimePercent": 0,
        "Spo2Avg": 96.55683898925781, //平均血氧
        "Spo2Min": 89.20576477050781,  //最低血氧
        "prAvg": 64, //平均心率
        "prMax": 107, //最大心率
        "prMin": 31 //最小心率
    },
    "time_start": 1698262633,//开始时间戳
    "error_tips": [],
    "length_time": 282,//时间戳数组长度
    "length_spo2": 23124,//血氧数组长度
    "time": [],//时间戳数组 
    "handoff": {
        "num": 5
    },
    "acc": [],//加速度计数组
    "moveflag": [],//离手标记数组
    "pr": [],//脉率趋势图数据 1s一个数据
    "spo2": [], //int 趋势图数据  1s一个数据（有效数据）
    "spo2f": [],//flot 血氧趋势图数据 1s一个数据
    "version": 6, // data format version(数据格式版本)
    "VldFrate": 0.7579549551010132, //血氧可信度
    "data_type": 1,//mode 类型
    "protocol": 1,
    "data_block_size": 256,
    "data_block_elenum": 82,
    "app_define": {
        "stage_N1_startime": 0,
        "stage_endtime": 0,
        "sw_ver": 11687
    }
}
```

