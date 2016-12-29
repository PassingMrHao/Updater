# Updater

基于DownloadManager封装的更新器，更简单的操作，更丰富的自定义，一行代码搞定应用更新，断点续传完全交给系统，再也不用自己操心。零依赖，不必再添加其他网络框架的依赖，DownloadManager在Android SDK中就已经集成。

## 样例

## 添加依赖

> compile 'simplepeng:updaterlibrary:1.0.0'

## 使用

### 开始下载

```java
updater = new Updater.Builder(getApplicationContext())
                        .setDownloadUrl(url)
                        .setApkName("test.apk")
                        .setNotificationTitle("updater")
                        .start();
```

默认下载路径在sd的Download目录，如果想自定义目录，可以调用
> setApkDir(String dirName)

示例：setApkDir("test")

或者自定义全路径
> setApkPath(String apkPath)

示例：setApkPath(Environment.getExternalStorageDirectory().getAbsolutePath())

### 监听下载完成安装apk

* 推荐在manifest中静态注册

```xml
<receiver android:name="com.simplepeng.updaterlibrary.DownloadReceiver">
            <intent-filter >
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
            </intent-filter>
</receiver>
```

* 动态注册（不推荐）

```java
updater.registerDownloadReceiver();
```

如果需要解绑监听器，记得调用该方法

```java
 updater.unRegisterDownloadReceiver();
```

### 监听下载进度

一般也不需要，看自己业务需求，notification上已经有进度显示了

```java
updater.addProgressListener(new ProgressListener() {
                    @Override
                    public void onProgressChange(long totalBytes, long curBytes, int progress) {
                        
                    }
                });
```

### 需要的权限

```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<!--调用hideNotification不显示notification需要的权限-->
<uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION" />
```

### 方法概括

Updater.Builder中的方法

* setApkName(String apkName) 设置下载下来的apk文件名
* setApkPath(String apkPath) 设置apk下载的路径（全路径）
* setApkDir(String dirName) 设置下载apk的文件目录
* setDownloadUrl(String downloadUrl)  设置下载的链接地址
* setNotificationTitle(String title) 通知栏显示的标题
* hideNotification() 隐藏通知栏
* debug() 是否为debug模式，会输出很多log信息（手动斜眼）
* allowedOverRoaming() 允许漫游网络可下载
* start() 开始下载

Updater中的方法

* registerDownloadReceiver 注册下载完成的监听
* unRegisterDownloadReceiver() 解绑下载完成的监听
* addProgressListener(ProgressListener progressListener) 添加下载进度回调
* removeProgressListener(ProgressListener progressListener) 移除下载进度回调
