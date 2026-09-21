Git 与 GitHub 使用指南

心音辅助检测系统项目 · 团队协作工具

仓库地址：https://github.com/Star-xiaotu/heart-sound-ai

适用范围：刘文鑫、李政洪、郝静、王鹏辉

2026年8月

# 一、这是什么？为什么用？

## 1.1 一句话解释

Git 是代码版本管理工具，GitHub 是存放代码的网站。它们让你可以：随时回到代码的历史版本、四个人同时改代码不会互相覆盖、清楚地看到每个人改了什么东西。

## 1.2 不用 Git 的后果

微信传代码 → "你改的是旧版本" → "这个bug我明明修过了" → 最终不知道哪个文件是最新的

U盘拷文件 → 版本混乱 → 谁改了什么都不清楚 → 出了问题没法追溯

## 1.3 用 Git 之后

每个人的修改独立记录，互不干扰

出问题可以一键回退到"昨天那个能跑的版本"

所有修改历史完整可查

结题时可以直接在 GitHub 上展示完整代码，比交一堆 .py 文件专业得多

# 二、安装与配置（每人只需做一次）

## 2.1 安装 Git

Windows: 去 https://git-scm.com/download/win 下载安装，一路默认选项即可

Mac: 终端运行 brew install git

安装后检查：打开终端或 Git Bash，输入 git --version，看到版本号就是成功了

## 2.2 配置身份

每个组员都要在 Git Bash 中运行（把名字和邮箱换成自己的）：

`git config --global user.name "你的姓名"`

`git config --global user.email "你的邮箱@qq.com"`

## 2.3 获取项目代码

每个组员在电脑上找一个合适的文件夹，右键 → Git Bash Here，运行：

`git clone https://github.com/Star-xiaotu/heart-sound-ai.git`

这会在当前目录创建一个 heart-sound-ai 文件夹，里面就是项目的全部代码。

# 三、日常使用（每人每天就这三条命令）

## 3.1 工作流程总览

每次开始写代码前 → 拉取最新代码；写完一段代码后 → 保存提交；一天结束时 → 推送到云端。

## 3.2 开始工作前：拉取

每天打开电脑后的第一件事：

`git pull`

这条命令会把仓库里别人的最新修改下载到你电脑上。如果不先 pull 就直接改代码，后面必定冲突。

## 3.3 完成一段修改后：提交

每完成一个小功能（比如"修了一个bug"、"加了一个函数"），就提交一次：

`git add .`

`git commit -m "[模块名] 做了什么改动"`

提交说明格式示例：

[数据] 完成元数据清单，新增文件哈希与排除日志

[模型] 轻量CNN 改用全局平均池化替代Flatten

[系统] 修复检测结果页面概率显示错误

写得越清楚，以后回头看越容易理解。

## 3.4 一天结束时：推送

`git push`

把今天所有的提交推送到 GitHub 云端。推送后其他人就能看到你的修改了。

## 3.5 三步总结

每天就是：pull → 写代码 → add → commit → push。反复循环。

# 四、常见问题处理

## 4.1 提交被拒绝："Updates were rejected"

说明别人在你之前推送了新的修改，你需要先拉取再推送：

`git pull`

`git push`

如果 pull 时出现冲突，看 4.2。

## 4.2 合并冲突怎么办

如果两个人改了同一个文件的同一行代码，Git 不知道用哪个版本，就会产生冲突。Git 会在文件中标记：

<<<<<<< HEAD

你修改的内容

=======

别人修改的内容

>>>>>>>

你需要手动编辑这个文件，保留正确的版本，删掉 <<< === >>> 这些标记，然后：

`git add .`

`git commit -m "解决冲突"`

`git push`

最好的策略是避免冲突——改代码前先 pull，尽量不和其他人同时改同一个文件。

## 4.3 想放弃今天的修改，回到上次提交的状态

`git checkout -- 文件名    # 恢复单个文件`

`git reset --hard          # 恢复所有文件（谨慎！）`

## 4.4 想看谁改了什么

`git log --oneline     # 查看提交历史摘要`

`git log --stat        # 查看每次提交改了哪些文件`

# 五、项目仓库规范

## 5.1 目录结构

项目重构后，目录结构统一为下面这样，依据是《智能心音辅助检测系统详细项目计划书》第 16 节。

**新建文件请放到对应目录，不要在根目录乱丢。**

HeartSoundAI/

├── configs/                   # 实验配置与模型参数（每次正式实验留一份）

├── data_manifest/             # 元数据、数据划分清单和排除日志（重要！）

├── src/                       # 全部源码

│   ├── preprocessing/         # 重采样、滤波、切片和频谱生成

│   ├── features/              # 传统特征提取

│   ├── models/                # 传统模型与深度模型

│   ├── training/              # 训练、验证和日志

│   ├── inference/             # 片段预测与录音级聚合

│   └── evaluation/            # 指标、置信区间和鲁棒性实验

├── app/                       # 前端与系统接口

├── tests/                     # 单元测试和系统一致性测试

├── checkpoints/               # 冻结模型与标签映射（权重文件不上传）

├── reports/                   # 实验报告、图表和错误案例

├── docs/                      # 使用说明、接口文档和教学材料

│   └── archive/               # 旧方案文档归档（只读，不要改）

├── environment/               # 软件依赖与运行环境（requirements.txt 放这里）

├── data/                      # 原始数据集（不上传Git，太大）

├── README.md                  # 项目说明（必读）

└── .gitignore                 # 哪些文件不上传Git

> **和旧结构的区别**：源码不再平铺在根目录，旧的 `models/`、`features/`、`evaluation/`
> 一律放进 `src/` 下；前端由 `frontend/` 改名为 `app/`；原方案里的 `llm_module/`
> （LLM 分析模块）**已取消**，不要再建。

## 5.2 提交信息规范

每次 commit 必须写清楚改了什么。格式：

[模块名] 一句话描述改动

可用模块名：数据 | 预处理 | 特征 | 模型 | 训练 | 推理 | 评测 | 系统 | 文档 | 测试 | 其他

（对照上面的目录结构：数据=`data_manifest/`，预处理=`src/preprocessing/`，
特征=`src/features/`，模型=`src/models/`，训练=`src/training/`，
推理=`src/inference/`，评测=`src/evaluation/`，系统=`app/`，文档=`docs/`，测试=`tests/`）

错误示例：

`git commit -m "修改"           # 不知道改了啥`

`git commit -m "更新"           # 太笼统`

正确示例：

`git commit -m "[模型] 轻量CNN 引入深度可分离卷积，参数量下降 42%"`

`git commit -m "[系统] 修复历史记录页面日期排序错误"`

`git commit -m "[数据] 完成受试者级划分，三集合交集检查通过"`

## 5.3 不上传 Git 的文件

以下文件已经配置在 .gitignore 中，不会被上传：

模型权重文件（.pth, .h5, .pkl）—— 太大，几百MB

数据集中的音频文件（.wav, .dat）—— 太大，几个GB

虚拟环境（venv/）—— 每台电脑自己装

临时文件（.tmp, .log）—— 没必要

# 六、组员加入步骤（给李政洪、郝静、王鹏辉）

## 6.1 首次加入

① 去 https://github.com 注册一个 GitHub 账号

② 把账号名发给刘文鑫，刘文鑫在仓库 Settings → Collaborators 中添加

③ 接受邮箱里的邀请

④ 安装 Git（见第二章）

⑤ 配置身份：

`git config --global user.name "你的姓名"`

`git config --global user.email "你的GitHub注册邮箱"`

⑥ 下载项目：

`git clone https://github.com/Star-xiaotu/heart-sound-ai.git`

⑦ 进入项目文件夹，创建虚拟环境并安装依赖（参考项目 README.md）

## 6.2 日常工作流

加入后每天的三步：

开始工作：git pull

完成修改：git add . → git commit -m "[模块] 说明"

一天结束：git push

# 七、刘文鑫（组长）额外职责

## 7.1 管理仓库

定期检查 GitHub Issues，看有没有人提问题

Review 组员的提交，确保代码质量

维护 README.md，保持项目说明是最新的

## 7.2 添加组员

① 打开仓库页面 → Settings → Collaborators and teams

② 点击 Add people

③ 搜索组员的 GitHub 用户名

④ 发送邀请

## 7.3 解决重大冲突

如果出现复杂冲突无法自动解决，可以：

在群里发冲突截图，召集相关人员一起讨论用哪个版本

或者保留两个版本的功能，在交流后手动合并

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Git 不复杂，关键是养成习惯。遇到问题不要慌，先在群里问，不要自己乱试。
