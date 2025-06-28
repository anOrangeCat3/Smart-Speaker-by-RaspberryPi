# 基于树莓派的智能音箱

[English](README.md) | 简体中文 |

**_2022.10~2023.1_**

_二人团队项目_

**_项目描述:_**  
本项目是微型计算机原理课程的课设项目。我们以树莓派为开发板，连接多个硬件组件，开发了一个智能音箱。我们主要完成了以下功能：
- 利用 Wukong 开源项目实现了 **唤醒功能**
- 集成百度智能云 API，实现语音识别与合成，完成了 **语音交互**
- 使用网络爬虫获取天气信息，实现 **语音天气播报**
- 接入图灵机器人，实现了 **基础对话功能**（当时 ChatGPT 尚未发布）
- 连接多种硬件组件，建立通信网络，实现 **智能家居控制**，如语音控制开关灯、远程控制空调等

**_个人贡献:_**  
我完成了主程序框架的搭建，语音唤醒功能的实现，语音交互的开发，天气播报功能的集成，图灵机器人的接入，以及灯光控制功能。

以下图片展示了语音交互模块的实物（左）与系统结构图（右）：

<div align=center>
<img src="https://github.com/anOrangeCat1/projects_sustech/assets/99580008/45a4b741-42db-4953-b83c-d8d44e5561a9" width="31%" height="31%"/>
  <img src="https://github.com/anOrangeCat1/projects_sustech/assets/99580008/c6c27131-5520-4af8-a51e-7ec29d0a8496" width="27%" height="27%"/>
</div>

下列图片展示了智能家居部分的实物（左、中）及其系统结构图（右）：

<div align=center>
<img src="https://github.com/anOrangeCat1/projects_sustech/assets/99580008/d28cf865-8e07-4ad6-82dd-a178dae5e6db" width="26%" height="26%" />
  <img src="https://github.com/anOrangeCat1/projects_sustech/assets/99580008/70992164-7c35-4ffb-a6ff-266bcd2ee28c" width="15%" height="15%"/>
   <img src="https://github.com/anOrangeCat1/projects_sustech/assets/99580008/c463842a-6c0b-4b7f-81fd-25363f971efe" width="35%" height="35%"/>
</div>

在本项目中，我基于树莓派开发了一个类似智能语音助手的微系统，完成了主程序架构搭建、语音唤醒、语音交互、天气播报、接入图灵机器人、灯光控制等功能。

**项目视频链接：**

youtube: https://www.youtube.com/watch?v=xWRJxR4BXoA

bilibili: https://www.bilibili.com/video/BV1WC4y157by

**_以下是相关文件:_**

|文件名|内容与功能|
|------|-----|
|_demo.py_|**唤醒功能代码**|
|_easy.py_|**语音交互功能代码**|
|_main.py_|**项目完整代码**|
|_project_pre.pdf_|**最终项目展示PPT**|
|_system_frame.png_|**系统结构图图片**|

**_项目整体效果如下图所示:_**

<div align=center>
<img src="https://github.com/anOrangeCat1/projects_sustech/assets/99580008/09866319-dd67-4345-868a-65e39d49b617"  /> 
</div>

**_功能清单:_**
- 唤醒与语音交互  
- 语音控制开关灯  
- 查询天气信息  
- 与图灵机器人对话  
- 远程/本地服务器  
- 红外设备控制  
- 室内温湿度查询  
- 手机端控制家居设备  

**_整个项目开发过程中的一些关键点:_**

---

- **声卡配置**  

在使用外接 USB 音频设备之前，需要提前配置声卡。在树莓派配置中指定设备的内建声卡（而不是树莓派板载声卡）。

<div align=center>
<img src="https://github.com/anOrangeCat1/projects_sustech/assets/99580008/ec32270d-5587-414a-8f16-c3960b6aaec8"  />
  <img src="https://github.com/anOrangeCat1/projects_sustech/assets/99580008/40a3e33c-1175-4d90-9a12-aa164201e251"  />
</div>

---

- **音频录制参数**

配置完成后可以使用系统自带命令进行录音：

``arecord -d 3 -r 16000 -c 1 -f S16_LE listen_word.wav``

说明：

- `-d 3`：录音3秒  
- `-r 16000`：采样率16000  
- `-c 1`：单通道  
- `-f S16_LE`：16位格式  

百度智能云强制要求音频格式为16000Hz、单声道、16位，否则语音识别将无法使用。虽然一开始以为32位音质更好，但多次实验发现无法识别。百度文档并未明确写出16位限制，但多次调参验证了这一点。

---

- **语音唤醒与 Snowboy**

常见产品如 Siri、小爱同学等都具有唤醒功能。  
1. Snowboy 是 KITT.AI 开发的语音唤醒工具包  
2. 支持树莓派、Linux、MacOS 等系统，可离线持续监听  
3. Snowboy 已开放源码，项目中用于唤醒功能框架

---

- **Snowboy 的回调函数（call_back）**

主要的任务执行函数写在 Snowboy 的回调函数中。每次检测到唤醒词，就执行主函数，实现人机交互。

<div align=center>
<img src="https://github.com/anOrangeCat1/projects_sustech/assets/99580008/abdaf5c6-a990-404d-b7d7-66f115e14c2f"  />
  <img src="https://github.com/anOrangeCat1/projects_sustech/assets/99580008/de21d8e1-01aa-45c0-bcbd-3f988deea76f"  />
</div>

---

- **语音识别**

语音识别（ASR）即自动将语音内容转化为文本。在深度学习兴起后，循环神经网络语言模型（RNNLM）被广泛应用。  

语音识别的早期应用主要是语音转写，现在已成为智能交互的重要组成部分。  
本项目中使用的是百度智能云的 ASR API。

语音识别的实际效果如下图所示：

<div align=center>
<img src="https://github.com/anOrangeCat1/projects_sustech/assets/99580008/ff4ded00-0dd2-414b-acfd-d16574e03ff0"  />
  <img src="https://github.com/anOrangeCat1/projects_sustech/assets/99580008/2b95a6f5-c9c4-4e58-b661-3ed4129f5865" width="45%" height="45%" />
  <img src="https://github.com/anOrangeCat1/projects_sustech/assets/99580008/95c08da6-6d4f-4764-bd13-d019de7f7a27" width="45%" height="45%" />
</div>

---

- **语音合成**

语音合成技术指的是通过电子或机械方式生成可理解的语音。TTS（Text-to-Speech）属于语音合成的一种。  
TTS 将文本内容转换为流畅可理解的语音输出。

本项目中使用了百度智能云的语音合成 API 来实现该功能。具体效果请参考项目演示视频。

---

- **云边协同架构**

由于树莓派计算能力有限，无法本地完成深度学习语音识别，因此本项目使用百度云的 API 实现识别，然后返回结果给树莓派做进一步处理。

这种方式体现了“边缘-云计算”架构的优势，展示了轻量嵌入式系统的灵活性。

<div align=center>
<img src="https://github.com/anOrangeCat1/projects_sustech/assets/99580008/61792e10-50d5-47d0-97ed-899e03825490" width="45%" height="45%" />
  <img src="https://github.com/anOrangeCat1/projects_sustech/assets/99580008/1740ab2f-bc60-49a1-be60-1c60104ec86d" width="45%" height="45%" />
</div>
