# 电源监视器

> Monitor power state changes.

线程：[主线程](../glossary.md#main-process)

在发出 ` app ` 模块的 ` ready ` 事件之前, 您不能 `require` 或使用此模块。

例如：

```javascript
const electron = require('electron')
const {app} = electron

app.on('ready', () => {
  electron.powerMonitor.on('suspend', () => {
    console.log('The system is going to sleep')
  })
})
```

## 事件

` powerMonitor ` 模块触发以下事件:

### Event: 'suspend'

在系统挂起时触发。

### Event: 'resume'

在系统恢复时触发。

### Event: 'on-ac' *Windows*

当系统变为交流电源时触发。

### Event: 'on-battery' *Windows*

当系统更改为电池电量时触发。