# 自定义消息

###### 请假

- 申请：daka:LeaveMessage；推送提示文字：[请假申请]
- 中间审批：daka:LeaveTransferMessage；推送提示文字：[请假审批]
- 审批：daka:LeaveApprovalMessage；推送提示文字：[请假审批]

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

- 申请：daka:PatchCardMessage；推送提示文字：[补卡申请]
- 审批：daka:PatchCardApprovalMessage；推送提示文字：[补卡审批]

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

- 申请，中间审批，审批：daka:UseCarMessage；推送提示文字：[用车申请]，[用车审批]

``` json

{
    "apply_id":"5812d96f84c24f749827ffad363f1b5a", //原leave_id
    "state":0, //0是未审核，1是已同意，2是已拒绝
    "tag":1, //1是申请，2是审批，3是中间审批
    "name":"岳泉晨",
    "targetId":"13496"
}


```

###### 会议室

- 申请，中间审批，审批：daka:UseMeetingRoomMessage；推送提示文字：[会议室预订申请]，[会议室预订审批]

``` json

{
    "apply_id":"5812d96f84c24f749827ffad363f1b5a",
    "state":0, //0是未审核，1是已同意，2是已拒绝
    "tag":1, //1是申请，2是审批，3是中间审批
    "name":"岳泉晨",
    "targetId":"13496",
    "meeting_room_name":"402"
}


```

###### 通用

- 申请，中间审批，审批：daka:OthersApplyMessage；推送提示文字：[显示RN传过来的参数apply_title,根据tag拼加字符串'申请'或者'审批']

``` json

{
    "apply_id":"5812d96f84c24f749827ffad363f1b5a",
    "state":0, //0是未审核，1是已同意，2是已拒绝
    "tag":1, //1是申请，2是审批，3是中间审批
    "name":"岳泉晨",
    "targetId":"13496",
    "apply_title":"公文发布" //RN只需传递title内容，原生拼加字符串'申请'或者'审批'
}


```

***

# 页面跳转



