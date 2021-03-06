## React Native

#### 请假详情:

```jsx
navigation.navigate('LeaveDetail',{
    leave_id: item.leave_id,
    name: item.leave_user_name,
    refresh: ()=>{
        this.listView.manuallyRefresh()
    }
})
```

| routerName | leave\_id （必填） | name \(非必填\) | refresh （非必填） |
| :--- | :--- | :--- | :--- |
| LeaveDetail | 请假id | 请假人名称 | 回调刷新函数 |

---

## Native

* ### native端跳转react native界面时，携带参数统一`PARAMS`，是字符型json。如`bundle("PARAMS","{"id":1,"name":"test","photo":"xxx.png"}")`

#### 请假混合审批界面：

| `moduleName` or `HeadlessJsTaskConfig.TaskName` | initialProperties |
| :--- | :--- |
| `AddLeaveManager` | `null` |

---

#### 只能请假界面：

| `moduleName` or `HeadlessJsTaskConfig.TaskName` | PARAMS |
| :--- | :--- |
| `AddLeave` | approver （如下） |

#### `approver`:

| parmas | type | description | required/optional |
| :--- | :--- | :---: | :---: |
| employee\_id | string/number | 审批人id | required |
| name | string | 审批人名称 | required |
| photo | number | 审批人头像 | required |

---

#### 请假详情界面：

| `moduleName` or `HeadlessJsTaskConfig.TaskName` |
| :--- |
| `LeaveDetail` |

| parmas | type | description | required/optional |
| :--- | :--- | :---: | :---: |
| leave\_id | string/number | 请假id | required |
| name | string | 请假人名称 | optional |



