---
description: 为 meal-app 生成下月菜单的完整工作流
---

# 生成月度菜单

用户说"重新生成X月菜单"或"生成下月菜单"时，按此流程执行。

## 数据源

1. **灵感菜品**：从 GitHub 拉取 `inspirations.json`（`https://api.github.com/repos/zoexudesign/meal-app/contents/inspirations.json`）
2. **不爱吃食材**：从 GitHub 拉取 `dislikes.json`（`https://api.github.com/repos/zoexudesign/meal-app/contents/dislikes.json`）
3. **现有菜单**：`e:\Personal\meal-app\data.js`

## 菜单设计原则

### 早餐（固定，不需要设计）
- **希腊酸奶 + 格兰诺拉 + 水煮蛋**
- 唯一变量：水果种类（7种轮换：草莓→蓝莓→猕猴桃→橙子→苹果→香蕉→芒果）

### 午餐 + 晚餐
- **地中海饮食** ~35%（烤鸡胸沙拉、蒜虾、鹰嘴豆沙拉、烤鱼、意面等）
- **日式饮食** ~35%（三文鱼拌饭、亲子丼、味噌汤、照烧鱼、寿喜烧、乌冬面等）
- **东南亚菜** ~10%（冬阴功、泰式罗勒鸡、越南河粉/春卷等）
- **中餐** ~10%（白切鸡、清蒸鲈鱼、香菇鸡汤等）
- **韩餐** ~10%（拌饭、辣炒鸡胸、豆腐汤等）

### 健康约束
- 血糖血脂友好：多全谷物、优质蛋白、膳食纤维、Omega-3
- 每道菜做法 ≤ 15 分钟
- 不重复连续两天的主要蛋白质来源
- **必须排除** `dislikes.json` 中记录的食材
- **优先使用** `inspirations.json` 中记录的灵感菜品

### 数据格式
```js
"2026-04-01": {
    breakfast: {
        title: "希腊酸奶格兰诺拉 + 水煮蛋", items: [
            { i: "希腊酸奶+格兰诺拉", m: "拌匀", n: "钙·蛋白质·益生菌·膳食纤维·VB2" },
            { i: "水果（草莓）", m: "—", n: "VC·叶酸·花青素·膳食纤维" },
            { i: "水煮蛋", m: "煮8分钟", n: "蛋白质·VB12·VD·硒" }
        ]
    },
    lunch: { title: "菜名", items: [
        { i: "食材", m: "做法", n: "营养素1·营养素2·营养素3" }
    ]},
    dinner: { title: "菜名", items: [...] }
}
```

## 生成步骤

1. 读取 GitHub 上的 `inspirations.json` 和 `dislikes.json`
2. 确定目标月份的所有工作日（排除双休 + 公共节假日）
3. 中国公共假日参考：元旦、春节、清明、五一、端午、中秋、国庆
4. 生成菜单数据（由于数据量大，写到临时 JS 文件再用脚本合并到 `data.js`）
5. 更新 `data.js` 保留其他月份数据
6. `git add -A` → `git commit` → `git push`
7. 提醒用户刷新 app 查看

## 注意事项

- 三月和四月菜单不要完全相同，保持变化
- 每周内尽量不重复同一道主菜
- SW缓存版本号在 `sw.js` 第一行，大版本更新时需要 bump
