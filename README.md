# c_to_f
# c_to_f
# c_to_f
# c_to_f
# c_to_f
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

asm_P_AND_L= pd.read_excel('/Users/rebecca.tang/Desktop/asm_coding test.xlsx')
print(asm_P_AND_L)

# 將列名轉換為日期格式並去除時間部分
asm_P_AND_L.columns = asm_P_AND_L.columns.to_series().apply(lambda x: pd.to_datetime(x, errors='ignore').strftime('%Y-%m-%d') if ' ' in str(x) else x)
print(asm_P_AND_L.columns)

##################目前目標達成率 by region
# 計算一月到八月三日的累積總時數
asm_P_AND_L['一到八月累積'] = asm_P_AND_L['一到七月累積'] + asm_P_AND_L['2024-08-01'] + asm_P_AND_L['2024-08-02'] + asm_P_AND_L['2024-08-03']
# 計算每個region的目標達成率
region_agg = asm_P_AND_L.groupby('region').agg({ '一到八月累積': 'sum','一到八月目標': 'sum'})
# 計算達成率
region_agg['目標達成率'] = region_agg['一到八月累積'] / region_agg['一到八月目標'] *100

# 顯示結果
print(region_agg[['目標達成率']])

# 繪製柱狀圖
plt.figure(figsize=(10, 6))
plt.bar(region_agg.index, region_agg['目標達成率'], color='pink')

# 添加標題和標籤
plt.title('Target Achievement Rate for Each Region')
plt.xlabel('Region')
plt.ylabel('Target Achievement Rate(%)')

# 以百分比形式顯示達成率
plt.gca().yaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'{x:.0f}'))

# 在柱狀圖上方添加資料標籤
for i, v in enumerate(region_agg['目標達成率']):
    plt.text(i, v + 1, f'{v:.2f}', ha='center', va='bottom')

# 添加100%的目標線
plt.axhline(y=100, color='red', linestyle='--', label='目標 100%')

# 顯示圖表
plt.show()



##################目前季達成率 by region
