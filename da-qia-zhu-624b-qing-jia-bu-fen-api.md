## React Native

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



