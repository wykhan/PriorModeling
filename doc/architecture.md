# System Architecture

CenterNet + Prior Modeling Integration

## Repository Structure

PriorModeling_VibeCoding/
└── PriorModeling/                              # git clone 下来的项目根
    ├── CONTEXT/                                # 你已经建好：项目上下文
    ├── PROMPTS/                                # 你已经建好：多层任务prompt
    ├── doc/
    ├── CHANGELOG_DEV.md
    ├── debug.md

    ├── src/
    │   ├── main.py                              # 训练入口（少改：主要是加新arg/开关）
    │   ├── test.py                              # 测试入口（少改：主要是加新arg/开关）
    │   └── lib/
    │       ├── opts.py                          # ✅改动点A：新增Prior相关命令行参数
    │       ├── trains/
    │       │   ├── base_trainer.py              # 少改：日志/钩子/metric汇总
    │       │   └── ctdet.py                     # ✅改动点B：把 prior loss/正则项接入训练
    │       ├── models/
    │       │   ├── model.py                     # 少改：构建网络/forward出口（尽量不动骨架）
    │       │   └── networks/
    │       │       ├── dla.py / resnet.py       # ❌通常不改（保持baseline可比）
    │       │       └── ...
    │       ├── datasets/
    │       │   ├── dataset_factory.py           # 少改：注册你的dataset名
    │       │   ├── sample/
    │       │   │   └── ctdet.py                 # ✅改动点C：把 prior 需要的额外监督/统计打包进batch
    │       │   └── <your_dataset>.py            # ✅改动点D：适配你数据集/标注格式
    │       ├── detectors/
    │       │   └── ctdet.py                     # 少改：推理/后处理出口（必要时把prior用于score校正）
    │       ├── utils/
    │       │   ├── debugger.py                  # 少改：可视化输出
    │       │   ├── post_process.py              # 少改：decode/NMS（一般不动）
    │       │   └── ...
    │       └── logger.py / ...                  # 少改
    │
    ├── exp/                                     # 训练输出（运行产生）
    │   └── ctdet/
    │       ├── baseline_m0/
    │       ├── prior_m1/
    │       └── ...
    └── runs/                                    # ✅建议你新增：把日志/metrics/checkpoints再结构化一层
        ├── baseline_m0/
        │   ├── logs/
        │   ├── metrics/
        │   └── checkpoints/
        └── prior_m1/
            ├── logs/
            ├── metrics/
            └── checkpoints/

## Prior Integration Points

opts.py
datasets/sample/ctdet.py
trains/ctdet.py

## Data Flow

dataset
→ dataloader
→ network
→ loss
→ optimizer