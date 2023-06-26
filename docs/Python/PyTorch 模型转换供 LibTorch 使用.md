# PyTorch 模型转换供 LibTorch 使用

1. 复制下面代码替换模型路径和输入数据即可完成转换

    ```python
    import torch
    # 权重文件
    weights = '权重路径'
    # 输入数据格式
    data = torch.zeros((16, 3, 640, 640))
    # 加载已经存在的模型
    model = torch.load(weights, map_location=torch.device('cpu')).float()
    # 替换成预测模式
    # model.eval()
    # 模拟运行
    y = model(data)
    ts = torch.jit.trace(model, data)
    # 保存转换完成的权重
    ts.save('model.pt')
    ```
