# Notification

> 创建OS(操作系统)桌面通知

线程：[主线程](../glossary.md#main-process)

## 在渲染进程中使用

如果要显示来自渲染进程的通知, 你应该使用 [ HTML5 Notification API ](../tutorial/notifications.md)

## Class: Notification

> 创建OS(操作系统)桌面通知

线程：[主线程](../glossary.md#main-process)

`Notification` 是 [EventEmitter](http://nodejs.org/api/events.html#events_class_events_eventemitter)

通过 ` options ` 来设置的一个新的原生 ` Notification `。

### 静态方法

`Notification` 类有以下静态方法：

#### `Notification.isSupported()`

Returns ` Boolean `-无论当前系统是否支持桌面通知

### `new Notification([options])` *Experimental*

* `options` Object 
  * ` title `String - 通知的标题, 将在通知窗口的顶部显示
  * ` subtitle `String (可选) 通知的副标题, 将显示在标题下面。* macOS *
  * ` body `String 通知的正文文本, 将显示在标题或副标题下面
  * ` silent `Boolean (可选) 在显示通知时是否发出 OS 提示音
  * ` icon`[ NativeImage ](native-image.md) (可选) 该通知的图标
  * ` hasReply `Boolean (可选) 是否向通知中添加内联答复选项。 * macOS *
  * ` replyPlaceholder `String (可选) 内联答复输入字段中的占位符。* macOS *
  * `sound `String (可选) 显示通知时播放的声音文件的名称。* macOS *
  * ` actions `[ NotificationAction [] ](structures/notification-action.md) (可选) 要添加到通知中的操作。 请在 ` NotificationAction ` 文档 中的查看可用操作和限制* macOS *

### 事件

用 `new Notification` 创建的对象触发以下事件：

** 注意: **某些事件仅在特定的操作系统上可用, 这些方法会被标记出来。

#### 事件: 'show'

返回:

* `event` Event

当通知向用户显示时触发, 请注意, 这可能会多次触发, 因为「通知」可以通过 ` show() ` 方法多次显示。

#### Event: 'click'

返回:

* `event` Event

在用户单击通知时触发。

#### 事件：close

返回:

* `event` Event

当用户手动关闭通知时触发

当通知关闭后，这个事件不能保证在所有情况下都会触发。

#### Event: 'reply' *macOS*

返回:

* `event` Event
* ` reply `String-用户在内联答复字段中输入的字符串

当用户单击 ` hasReply: true ` 的通知上的 "Reply" 按钮时触发。

#### Event: 'action' *macOS*

返回:

* `event` Event
* `index` Number - The index of the action that was activated

### 实例方法

用`new Notification` 创建的对象有以下实例方法：

#### `notification.show()`

Immediately shows the notification to the user, please note this means unlike the HTML5 Notification implementation, simply instantiating a `new Notification` does not immediately show it to the user, you need to call this method before the OS will display it.

### Playing Sounds

On macOS, you can specify the name of the sound you'd like to play when the notification is shown. Any of the default sounds (under System Preferences > Sound) can be used, in addition to custom sound files. Be sure that the sound file is copied under the app bundle (e.g., `YourApp.app/Contents/Resources`), or one of the following locations:

* `~/Library/Sounds`
* `/Library/Sounds`
* `/Network/Library/Sounds`
* `/System/Library/Sounds`

See the [`NSSound`](https://developer.apple.com/documentation/appkit/nssound) docs for more information.