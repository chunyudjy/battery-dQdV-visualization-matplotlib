# battery-dQdV-visualization-matplotlib （Dash 版）

本工具是一个基于 Dash 框架的网页应用，支持上传多个 CSV 数据文件，自动完成 dQ/dV 分析、SoC/DoD 与电压关系图绘制，以及平滑滤波处理。

## 🚀 启动方式

```bash
pip install dash pandas numpy matplotlib scipy
python app.py
```

然后访问浏览器中的：`http://localhost:8050`

---

## 📂 数据要求（非常重要）

上传的 CSV 文件必须包含以下列名（**区分大小写**）：

| 列名                  | 含义                        | 是否必须   |
| ------------------- | ------------------------- | ------ |
| `Voltage[V]`        | 电压值                       | ✅ 必须   |
| `Capacity[mAh]`     | 累积容量（用于 dQ/dV）            | ✅ 必须   |
| `Condition`         | 工况编号（判断 charge/discharge） | ✅ 必须   |
| `SoC[%]` 或 `DoD[%]` | 状态百分比（任一即可）               | ✅ 至少一项 |

> ⚠️ 文件应为 UTF-8 编码的标准 CSV 格式，不能含有空行或标题行之外的非数值内容。

---

## 🔧 功能概览

* 📤 上传多个 CSV 文件
* 📉 自动绘制：

  * 充电/放电 dQ/dV 曲线（平滑后）
  * SoC/DoD vs Voltage 曲线
* 🧹 使用 Savitzky-Golay 滤波自动平滑
* 🧾 显示处理信息（窗口长度、数据点数、处理时间等）

---

## 📌 处理逻辑说明

* 根据 `Condition` 判断工况类型：

  * `1`, `10` → `charge`
  * `2`, `20` → `discharge`
* 对每个文件提取对应工况数据，计算 `dQ/dV = ΔQ / ΔV`，再进行平滑处理
* 自动识别 SoC 或 DoD 列作为横坐标绘图

---

## 📤 输出说明

应用生成四张图表：

1. **每文件的 charge dQ/dV 曲线**
2. **每文件的 discharge dQ/dV 曲线**
3. **每文件的 SoC/DoD vs Voltage 图**
4. **所有数据的综合图（charge + discharge）**
