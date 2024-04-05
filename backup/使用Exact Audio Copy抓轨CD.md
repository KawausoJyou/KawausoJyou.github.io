抓轨CD对我来说并没什么用，只是单纯想有这么一个过程。抓轨后发扬互联网精神分享出来也不会有人下载，毕竟我自购的CD在网络上都能很容易找到抓轨资源。

### 准备

- 需抓轨的CD
- 抓轨软件[Exact Audio Copy](https://www.exactaudiocopy.de/)
- 元数据软件[Player](http://www.vuplayer.com/player.php)

安装EAC（Exact Audio Copy）时取消选择`GD3 Metadata Plugin`，首次打开EAC关闭「EAC配置向导」，将CD放入光驱进入「读取采样偏移校正值」设置。若所使用光驱在AccurateRip有记录可直接配置采样偏移校正值，若无记录需使用多张「参校碟（Key Disc）」确认光驱读取采样偏移校正值，在此不表。

### 配置EAC

下载[EAC配置文件](https://raw.githubusercontent.com/KawausoJyou/KawausoJyou.github.io/main/files/EACProfile.cfg)于`EAC - 配置文件 - 读取配置文件`中读取下载的配置文件。
> 若系统安装了WinFsp在运行EAC读取配置文件时（或是任何调用选择文件/目录的操作）都会引起EAC闪退，可使用[WinFsp提供的脚本](https://github.com/winfsp/winfsp/blob/master/tools/fix162.bat)解决，需要「以管理员身份运行」。

#### 压缩选项
`外部压缩程序`配置如下：
<img width="468" alt="PixPin_2024-04-05_10-49-55" src="https://github.com/KawausoJyou/KawausoJyou.github.io/assets/92703641/c4d0c521-fe2c-4541-a363-7e067dfdfb94">
附加命令行选项：
`-T "artist=%artist%" -T "title=%title%" -T "album=%albumtitle%" -T "date=%year%" -T "tracknumber=%tracknr%" -T "genre=%genre%" -5 %source%`

除压缩为FLAC外，我还需要压缩为ALAC上传到Apple Music，为此配置了`额外的外部压缩程序`：
<img width="468" alt="PixPin_2024-04-05_10-53-37" src="https://github.com/KawausoJyou/KawausoJyou.github.io/assets/92703641/75c5cc74-f1ca-4023-a9e7-ff2de3e7f884">
附加命令行选项：
`-i %source% -metadata "ARTIST=%artist%" -metadata "TITLE=%title%" -metadata "ALBUM=%albumtitle%" -metadata "DATE=%year%" -metadata "TRACK=%tracknr%/%numtracks%" -metadata "GENRE=%genre%" -metadata "ALBUM_ARTIST=%albumartist%" -metadata "COMPOSER=%composer%" -metadata "DISC=%cdnumber%/%totalcds%" -metadata "COMMENT=CRC:%TRACKCRC%" -acodec alac %dest%`

两者`压缩程序及路径`按对应编码器路径设置即可。

#### EAC选项

`文件名`可配置抓轨文件文件名格式，「文件名」「附加文件名」分别与「外部压缩程序」「额外的外部压缩程序」相对应。我个人分别设置为`%albumartist% - %albumtitle% (%year%)\FLAC\%tracknr2%. %artist% - %title%`和`%albumartist% - %albumtitle% (%year%)\ALAC\%tracknr2%. %artist% - %title%`以分别保存FLAC和ALAC文件。

`目录`可设置默认保存目录，也可每次抓轨前询问保存目录。
<img width="468" alt="PixPin_2024-04-05_10-58-17" src="https://github.com/KawausoJyou/KawausoJyou.github.io/assets/92703641/0cf99b20-8a3b-4b03-a7a5-b4897b9e3376">

#### 驱动器选项

- `抓取模式 - 安全模式`选择`带有下列驱动器特性的安全模式（推荐）`及勾选`驱动器具备[精确流]特性`和`驱动器可以缓冲音频数据`。
- `驱动器 - 驱动器读取指令`点击`现在自动检测读取指令`，然后勾选`驱动器支持CD-Text读取`。
- `偏移/速度`勾选`通读到Lead-In和Lead-Out`，选择`当前的速度选择`，勾选`抓取过程中允许降速`和`在驱动器上使用AccurateRip（数据库）`。
- `间隙检测`「间隙/索引寻获方式」选择`检测方法A`，「检测精确度」选择`安全`。

**以上步骤只需要首次抓轨配置一次，之后重复抓轨直接从下面步骤开始即可。**

### 抓轨

1. 「以管理员身份运行」Player，`File - Gracenote - Registration`注册Gracenote，然后`File - Gracenote - Retrieve Disc Info`即可获取放入到光驱的CD元数据，最后`File - Export Info - cdplayer.ini`输出CD元数据。
2. 回到EAC，`数据库 - 获取CD信息 - 从CDPLAYER.INI`导入CD元数据，警告弹窗`是`即可，警告内容翻译有误，不必理会。若CD元数据导入失败，可自行在EAC手动编辑元数据。
3. 确保已勾选`操作 - 追加间隙到上一轨（缺省）`，然后分别运行`检测间隙`和`检测静音间隙`，最后`创建CUE目录文件 - 多个带间隙的WAV文件（非规则）`保存CUE文件。
4. 最后正式开始抓轨。`操作 - 测试并抓取所选音轨 - 已压缩`选择好保存目录或默认保存目录即进入抓轨。等待时间视CD时长及光驱速度而定，抓轨完成后Log文件绝对不允许更改。

### 分享

我的自购CD抓轨后的文件都会分享在[此处](https://blog.kawauso.xyz/tag.html#%E5%88%86%E4%BA%AB)，感兴趣可自行下载（不会有人看这种无聊的Blog的

### 参考文章

- [EAC 配置与抓轨指南](https://zexwoo.blog/posts/tutorials/eac-ripping/) - 本教程旨在让你成为一名优秀的抓轨者，让你抓出 100% Log 的音轨。