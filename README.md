# yolov8n_rknn_Cplusplus_dfl

yolov8 瑞芯微 rknn 板端 C++部署，使用平台 rk3588，全网最简单、运行最快的部署方式。

yolov8 瑞芯微 rknn 板端 C++部署，使用平台 rk3588。模型转换参考 [onnx转rknn](https://blog.csdn.net/zhangqian_1/article/details/135523096) ， 仿真参考[PC仿真](https://github.com/cqu20160901/yolov8n_onnx_tensorRT_rknn_horizon_dfl) 。

## 编译和运行

1）编译

```
cd examples/rknn_yolov8_demo_dfl_open

bash build-linux_RK3588.sh

```

2）运行

```
cd install/rknn_yolov8_demo_dfl_open

./rknn_yolov8_demo

```

注意：修改模型、测试图像、保存图像的路径，修改文件为src下的main.cc

```

int main(int argc, char **argv)
{
    char model_path[256] = "/home/zhangqian/rknn/rknpu2_1.4.0/examples/rknn_yolov8_demo_dfl_open/model/RK3588/yolov8n_ZQ.rknn";
    char image_path[256] = "/home/zhangqian/rknn/rknpu2_1.4.0/examples/rknn_yolov8_demo_dfl_open/test.jpg";
    char save_image_path[256] = "/home/zhangqian/rknn/rknpu2_1.4.0/examples/rknn_yolov8_demo_dfl_open/test_result.jpg";

    detect(model_path, image_path, save_image_path);
    return 0;
}
```


# 测试效果


冒号“:”前的数子是coco的80类对应的类别，后面的浮点数是目标得分。（类别:得分）

![images](https://github.com/cqu20160901/yolov8n_rknn_Cplusplus_dfl/blob/main/examples/rknn_yolov8_demo_dfl_open/test_result.jpg)

（注：图片来源coco128）

说明：推理测试预处理没有考虑等比率缩放，激活函数 SiLU 用 Relu 进行了替换。由于使用的是coco128的128张图片数据进行训练的，且迭代的次数不多，效果并不是很好，仅供测试流程用。换其他图片测试检测不到属于正常现象，最好选择coco128中的图像进行测试。

把板端模型推理和后处理时耗也附上，供参考，使用的芯片rk3588，模型输入640x640，检测类别80类。

![image](https://github.com/cqu20160901/yolov8n_rknn_Cplusplus_dfl/assets/22290931/71e03ce6-df59-4746-855e-df77889d6ce5)

2024年1月12日：后处理代码有所优化，后处理时耗大幅度降低。（检测类别越多效果越明显，检测1个类别就没有优化效果，代码已同步到对应的代码仓中）

![image](https://github.com/cqu20160901/yolov8n_rknn_Cplusplus_dfl/assets/22290931/14683b96-11f3-4c2c-a71c-f79705577645)

