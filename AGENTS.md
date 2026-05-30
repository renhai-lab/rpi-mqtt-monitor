# RPI MQTT Monitor — 开发指南

## 技术栈

| 类别 | 技术 | 版本/备注 |
|------|------|-----------|
| 语言 | Python | 3.x |
| 包管理 | pip / venv | requirements.txt |
| MQTT | paho-mqtt | 1.6.1 |
| 系统监控 | psutil | CPU、内存、磁盘、网络等 |
| HTTP | requests | Home Assistant API / 更新检查 |
| 配置 | config.py | Python 配置文件 |
| 国际化 | translations.ini | 多语言支持（EN/DE/BG） |
| 服务 | systemd | rpi-mqtt-monitor.service |

## 项目结构

```
rpi-mqtt-monitor/
├── src/
│   ├── rpi-cpu2mqtt.py        # 主程序入口
│   ├── config.py               # 运行配置
│   ├── config.py.example       # 配置模板
│   ├── update.py               # 自动更新逻辑
│   └── translations.ini        # 多语言翻译
├── ext_sensor_lib/             # 外部传感器库
│   ├── ds18b20.py              # DS18B20 温度传感器
│   └── sht21.py                # SHT21 温湿度传感器
├── images/                     # README 截图
├── install.sh                  # 安装脚本
├── remote_install.sh           # 远程一键安装
├── rpi-mqtt-monitor.service    # systemd 服务文件
├── requirements.txt            # Python 依赖
├── LICENSE                     # MIT License
└── README.md
```

## 常用命令

```bash
# 安装依赖
pip install -r requirements.txt

# 运行监控（作为服务）
rpi-mqtt-monitor -s

# 运行监控（单次执行）
rpi-mqtt-monitor

# 通过 Home Assistant API 发送数据
rpi-mqtt-monitor --hass_api

# 显示屏幕输出
rpi-mqtt-monitor -d

# 更新脚本
rpi-mqtt-monitor --update

# 卸载
rpi-mqtt-monitor --uninstall

# 远程一键安装
bash <(curl -s https://raw.githubusercontent.com/hjelev/rpi-mqtt-monitor/master/remote_install.sh)
```

## CLI 参数

| 参数 | 说明 |
|------|------|
| `-s, --service` | 以服务模式运行，可配置间隔 |
| `-H, --hass_api` | 通过 Home Assistant API 发送数据 |
| `-d, --display` | 在屏幕上显示数值 |
| `-v, --version` | 显示版本号 |
| `-u, --update` | 更新脚本和配置 |
| `-w, --hass_wake` | 显示 Home Assistant Wake-on-LAN 配置 |
| `--uninstall` | 卸载并清理所有相关文件 |

## 代码规范

- Python 3.x 兼容，保持简洁
- 文件操作尽量使用 `pathlib`，不使用 `os.path`
- 日志输出使用 `logging` 或直接打印（项目原有风格）
- 配置集中在 `src/config.py`，通过 `config.py.example` 提供模板
- 外部传感器支持 DS18B20 和 SHT21，通过 `ext_sensor_lib/` 扩展
- MQTT 消息格式支持单条发送和 CSV 批量发送

## 写作约定

- 代码注释使用英文（保持与上游项目一致）
- 配置文件提供 `.example` 模板，实际配置不入库
- 本仓库为 hjelev/rpi-mqtt-monitor 的 fork，保持与上游兼容
- systemd 服务文件提供开箱即用的部署支持
