###### 请假

``` json

{
    "leave_id":"5812d96f84c24f749827ffad363f1b5a",
    "leave_type":"事假",
    "start_time":"2018-09-17 09:04",
    "end_time":"2018-09-17 09:06",
    "state":0, //0是未审核，1是已同意，-1是已拒绝
    "tag":1, //1是申请，2是审批，3是中间审批
    "name":"岳泉晨",
    "targetId":"13496"
}

变更：tag增加状态3

```

###### 补卡

``` json

{
    "time":"2018-09-17 06:30",
    "state":0, //0是未审核，1是已同意，2是已拒绝
    "tag":1, //1是申请，2是审批
    "name":"岳泉晨",
    "targetId":"131",
    "flag":"上班补卡",
    "leave_id":"45298472da874da5bf51dcc035324712"
}

```

###### 用车

``` json

{
    "leave_id":"5812d96f84c24f749827ffad363f1b5a",
    "state":0, //0是未审核，1是已同意，2是已拒绝
    "tag":1, //1是申请，2是审批，3是中间审批
    "name":"岳泉晨",
    "targetId":"13496"
}


```

###### 会议室

``` json

{
    "leave_id":"5812d96f84c24f749827ffad363f1b5a",
    "state":0, //0是未审核，1是已同意，2是已拒绝
    "tag":1, //1是申请，2是审批，3是中间审批
    "name":"岳泉晨",
    "targetId":"13496",
    "meeting_room_name":"402"
}


```

###### 通用

``` json

{
    "leave_id":"5812d96f84c24f749827ffad363f1b5a",
    "state":0, //0是未审核，1是已同意，2是已拒绝
    "tag":1, //1是申请，2是审批，3是中间审批
    "name":"岳泉晨",
    "targetId":"13496",
    "apply_title":"公文发布申请"
}


```