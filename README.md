# zkm-encryptix
| zkm-encryptix 是为简化 JavaScript(node) 与 zkm 协议对接流程而开发的依赖库。通过封装内部逻辑，使用者无需深入理解底层对接细节，只需安装该依赖并调用简化后的 API，即可轻松实现与 zkm 协议的集成

## 快速开始
1. 下载依赖到本地项目文件夹
2. 安装依赖
    ```bash
    # 替换为依赖实际位置
    pnpm add ./path/zkm-encryptix-1.x.x.tgz
    # 或
    npm install ./path/zkm-encryptix-1.x.x.tgz
    ```
3. 观察到安装结束相关提示则代表成功
4. 使用
    ```typescript
    import { zkmEncryptix } from 'zkm-encryptix'
    console.log(zkmEncryptix.enableKeyMode) // output: Function
    ```
    补充：本项目支持 `commonJS`/ `esm` 两种模块规范。若需要使用指定模块规范，可直接使用依赖目录下以 `esm.js` 或 `cjs.js` 结尾的文件


## 函数
可划分为以下类型：发送/接收   

### 发送给设备的指令
- **async enableKeyMode()**   
获取开启按键测试模式（指令）   
返回值类型: Uint8Array   
- **async disibleKeyMode()**   
获取关闭按键测试模式（指令）   
返回值类型: Uint8Array   
- **async getConnectStatus()**   
获取连接状态以及信号强度（指令）   
返回值类型: Uint8Array   
- **async getVersionAndMode()**   
获取设备版本信息和型号（指令）   
返回值类型: Uint8Array   
- **async getBattery()**   
获取左/右电池信息（指令）   
返回值类型: Uint8Array   
- **async getMacAddress()**   
获取设备蓝牙的 mac 地址（指令）   
返回值类型: Uint8Array   

### 解析来自设备的指令
data: 接收的 Uint8Array 数组   
type: data[2]，枚举类型 *CmdEnum* 根据不同的类型值调用对应的函数   

- **async parseKeyModeStatus(data)**   
解析（返回的）按键测试模式状态   
返回值类型：TestKeySwitch   

- **async parseKey(data)**   
解析（返回的）按键信息   
返回值类型：TestKeyType   
- **async parseConnectStatus(data)**   
解析（返回的）连接状态以及信号强度   
返回值类型：ConnType   
- **async parseVersionAndMode(data)**   
解析（返回的）设备版本信息和型号   
返回值类型：VersionAndMode   
- **async parseBattery(data)**   
解析（返回的）左/右电池信息   
返回值类型：BatteryType   
- **async parseMacAddress(data)**   
解析（返回的）设备蓝牙的 mac 地址   
返回值类型：MacAddressType   

## 枚举
### CmdEnum
事件类型，上方与事件对应   
```typescript
TEST_MODE_CHANGE = 210,     // 测试模式状态
PARSE_KEY = 2,              // 按键信息
CONN_AND_DB = 250,          // 连接状态以及信号强度
VERSION_AND_MODE = 226,     // 设备版本信息和型号
BATTERY = 4,                // 左/右电池信息
MAC_ADDRESS = 25,           // 设备蓝牙的 mac 地址
ERROR = 0                   // 异常
```
## 类型
### ResultType
```typescript
{
    type: CmdEnum;          // 事件类型 
    rx_len: number;         // 数据包长度
    rx_buf: Uint8Array;     // USB 传输的数据
}
```