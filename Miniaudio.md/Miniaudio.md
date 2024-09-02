# Miniaudio

Miniaudio 是一个用 C 语言编写的音频播放和录制库，所有功能都包含在一个源代码文件中。这个库的设计旨在简化，不需要除了标准 C 库以外的外部依赖，由于是Header Only Library库，所以集成编译非常简单,只需要指明所在路径,在C++实现文件中包含所需头文件即可。 Miniaudio支持所有主要的桌面和移动平台。它提供了简单的 API 层（包括低级的直接音频数据访问和高级的声音管理），内置支持多种音频格式如 WAV、FLAC 和 MP3，以及音频效果、混音和可选的 3D 立体声功能。

‍

当我们在用miniaudio时候，只需要在要编写的文件上引用以下内容

```c
#define MINIAUDIO_IMPLEMENTATION
#include "miniaudio.h"
```

即可

在miniaudio中，我们可以很方便的实现回调播放以及在回调播放中处理实时音频算法。

我们以简单播放功能举例。

‍

‍

```c
/*
Demonstrates how to load a sound file and play it back using the low-level API.

The low-level API uses a callback to deliver audio between the application and miniaudio for playback or recording. When
in playback mode, as in this example, the application sends raw audio data to miniaudio which is then played back through
the default playback device as defined by the operating system.

This example uses the `ma_decoder` API to load a sound and play it back. The decoder is entirely decoupled from the
device and can be used independently of it. This example only plays back a single sound file, but it's possible to play
back multiple files by simple loading multiple decoders and mixing them (do not create multiple devices to do this). See
the simple_mixing example for how best to do this.
*/
#define MINIAUDIO_IMPLEMENTATION
#include "../miniaudio.h"

#include <stdio.h>
int BUFFER_SIZE = 1024;//定义每一帧传1024个PCM采样点
// 定义回调函数，用于从解码器读取 PCM 数据并输出到音频设备
void data_callback(ma_device* pDevice, void* pOutput, const void* pInput, ma_uint32 frameCount)
{	  
	// 获取音频解码器
    ma_decoder* pDecoder = (ma_decoder*)pDevice->pUserData;
    if (pDecoder == NULL) {
        return;
    }
    // 从解码器读取 PCM 帧并写入到输出缓冲区
    ma_decoder_read_pcm_frames(pDecoder, pOutput, frameCount, NULL);

	//这部分可以写入一些实时信号处理的算法，如：
	//先读取
    // for (int i = 0; i < BUFFER_SIZE; i++) {
    // mydata[i] = *((short *)pOutput + i);
    // buffer[i] = mydata[i]/32767.0f;
    // }
    // algrithm_process(buffer, BUFFER_SIZE);

    (void)pInput;
}

int main(int argc, char** argv)
{
    ma_result result;
    ma_decoder decoder;
    ma_device_config deviceConfig;
    ma_device device;

    if (argc < 2) {
        printf("No input file.\n");
        return -1;
    }
	// 初始化解码器，加载音频文件
    result = ma_decoder_init_file(argv[1], NULL, &decoder);
    if (result != MA_SUCCESS) {
        printf("Could not load file: %s\n", argv[1]);
        return -2;
    }

    deviceConfig = ma_device_config_init(ma_device_type_playback);
    deviceConfig.playback.format   = decoder.outputFormat;  	//设置输出格式
    deviceConfig.playback.channels = decoder.outputChannels;	//设置声道数
    deviceConfig.sampleRate        = decoder.outputSampleRate;	//设置输出采样率
    deviceConfig.dataCallback      = data_callback;
    deviceConfig.pUserData         = &decoder;
	deviceConfig.periodSizeInFrames = BUFFER_SIZE;				//设置每一帧传值的数量
    // 初始化音频设备
    if (ma_device_init(NULL, &deviceConfig, &device) != MA_SUCCESS) {
        printf("Failed to open playback device.\n");
        ma_decoder_uninit(&decoder);
        return -3;
    }
    // 启动音频设备
    if (ma_device_start(&device) != MA_SUCCESS) {
        printf("Failed to start playback device.\n");
        ma_device_uninit(&device);
        ma_decoder_uninit(&decoder);
        return -4;
    }

    printf("Press Enter to quit...");
    getchar();
    // 关闭音频设备并释放解码器资源
    ma_device_uninit(&device);
    ma_decoder_uninit(&decoder);

    return 0;
}
```

miniaudio的优点如下

* **简单的构建系统方式**：只需将其添加到您的源代码树中即可，无需安装任何依赖或开发包，也无需复杂的构建系统。只需要在源码的第一行写入"#define MINIAUDIO_IMPLEMENTATION"和"#include "miniaudio.h"即可使用。
* **跨平台兼容性**：支持所有主要的桌面和移动平台，包括Windows、macOS、Linux、BSD、iOS、Android以及Web（通过Emscripten）。作者在Windows下，mac以及Linux（树莓派）环境下进行测试，播放效果良好。
* **简单的API**：提供简单、灵活且模块化的API，Miniaudio提供了极高的灵活性，允许通过低级API直接初始化设备并处理原始音频数据。其模块化设计确保使用低级API时不影响其他功能，如Node Graph和资源管理器的使用。
* **开源许可**：Miniaudio是开源的，提供公共领域或MIT No Attribution许可供选择。
* **详细文档**：提供优质的文档和示例，属于开源音频库中的佼佼者。
* **高级混音和效果处理**：Miniaudio的Node Graph系统简化了高级混音和效果的设置，通过节点间的连接实现复杂的音频效果。支持自定义节点，允许灵活的混音和复杂路由设置。
* **资源管理**：资源管理器优化了音频资源的加载和管理，使用引用计数来避免重复加载，支持流式传输大文件以节省内存，并可异步加载资源。
* **高级音频引擎**：封装了资源管理器和节点图，提供了一个简单的API来通过空间化技术管理3D场景中的声音，使每个声音成为场景中节点图的一部分，利用基于图的处理和路由优势。

‍

# MIT 协议简介：

‍
