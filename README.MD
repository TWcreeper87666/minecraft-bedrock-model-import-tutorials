# 匯入任何 3D 模型到 Minecraft（基岩版）

> 原本只是在玩 FMBE（用 Blender 把模型轉成方塊）
> 但是太卡了！
> 不如走正經一點的路線（？）

實際嘗試、又跟 AI 聊了一下後……
慢慢摸出了一條**相對穩定又好控的流程**。

這篇會一步一步教你，**從任意 3D 模型 → Minecraft 基岩版實體**。

[![介紹影片](https://img.youtube.com/vi/2SSS5OO_jm8/maxresdefault.jpg)](https://youtu.be/2SSS5OO_jm8)

---

## 會用到的工具

（⭐ 代表插件）

* [**Blender**](https://www.blender.org/download/)：導入模型、處理比例、輸出 `OBJ` 
* [**Blender ⭐ MatCombiner**](https://github.com/Grim-es/material-combiner-addon/releases/tag/2.1.2.9)：合併材質，產生完整 `PNG` 貼圖
* [**Blockbench**](https://www.blockbench.net/downloads)：製作 / 編輯 Minecraft 模型
* **Blockbench ⭐ Minecraft Entity Wizard**：快速建立實體插件
* **Blockbench ⭐ Meshy**：導入 `OBJ`、轉成基岩版模型格式

---

## Step 1：準備 Blender 與插件

> 本教學以 **Blender 3.6** 為例，介面可能略有不同

1. 開啟 Blender
2. 前往：`Edit > Preferences > Add-ons`
3. 點擊 **Install**
4. 安裝 `material-combiner-addon-master.zip`
5. 安裝完成後，**記得啟用插件** ✅

---

## Step 2：導入 3D 模型

1. 找一個你喜歡的模型（我用地球示範 🌍）
2. **確認授權**（尊重著作權）

   * 搜尋時建議選 `CC BY`
   * 使用時標註原作者即可
3. 下載檔案大小不要太誇張，格式 Blender 能吃就行
4. 在 Blender 中導入：

   ```
   File > Import > 模型格式
   ```

切換到 **材質預覽（Material Preview）**，確認貼圖是否正常顯示。

---

## Step 3：調整比例與位置

* `S` + 滑鼠：縮放模型
* 使用 **預設 Cube** 當作比例尺（1 格 Minecraft 方塊）
* 調整模型位置，盡量貼齊座標軸平面
* 調整完成後，把 Cube 刪掉

---

## Step 4：處理材質（MatCombiner）

1. 按 `N` 開啟右側面板
2. 找到 **MatCombiner**
3. 點擊「更新材質列表」

> 如果模型只有一個材質，其實不用合併

目標只有一個：

> **拿到最終的貼圖 PNG**

把貼圖存成 `png` 即可。

---

## Step 5：輸出 OBJ

* `File > Export > Wavefront (.obj)`
* 匯出後你會得到：

  * `model.obj`
  * `model.mtl`

👉 到這裡，Blender 的工作完成！

---

## Step 6：準備 Blockbench

### 安裝插件

在 Blockbench 中安裝：

* **Minecraft Entity Wizard**
* **Meshy**

---

## Step 7：建立 Minecraft 實體插件

1. 使用 **Minecraft Entity Wizard**
2. 選擇「建立 Minecraft 插件」
3. 照精靈一步步走即可

建議設定：

* 行為：可套用其他實體（先別亂改）
* 掉落物：可略過
* 生怪蛋：不需要（直接用 `/summon`）
* 輸出位置：資料夾
* 幫插件取個名字

點擊 **Export** 🎉

接著按 **Open in VS Code**，它會自動幫你打開：

* 行為包（BP）
* 材質包（RP）

---

## Step 8：導入 OBJ 成模型

1. 回到 Blockbench
2. 點擊 **Edit Model**
3. 刪除預設模型
4. 使用 **Meshy → Import OBJ**
5. 匯入時：

   * Scale 設為 `8`
6. 匯入貼圖，確認顯示正常

沒問題就存檔吧（對，就是存檔 😅）

---

## Step 9：替換貼圖並測試

1. 回到 VS Code
2. 把材質包中的貼圖，換成你剛剛的 `png`
3. 安裝行為包與材質包

🎉 你現在已經能在遊戲裡看到模型了！

---

## 進階：多材質模型怎麼辦？

有些模型會有 **很多材質**，這時就需要合併。

### MatCombiner 建議設定

* Size：`Automatic`
* 不需要勾選 PBR Maps

生成後你會得到：

> 一張**超大的貼圖** 😅

解析度太大的話：

* 自行縮小
* 或重新合併時降低解析度

---

## 模型方向問題（超常見）

如果模型方向怪怪的：

* 在 Blender 匯出 OBJ 時
* 把 **Forward Axis** 改成 `Z`

這樣模型在 Minecraft 中會朝 **北方（實體正面）**。

---

## 不用 Wizard 的手動方式（補充）

1. Blockbench 新建 **Bedrock Model**
2. 匯入 OBJ
3. 存成 `.geo.json`
4. 放到：

   ```
   RP/models/entity/
   ```
5. 貼圖放到：

   ```
   RP/textures/entity/
   ```
6. 其他檔案直接複製現有實體來改

⚠️ 確保：

* 名稱正確
* 路徑完全一致

---

## 效能優化（檔案太大怎麼辦）

### 使用 Decimate Modifier

* 加入 `Decimate`
* 調整 **Ratio** 降低面數

如果模型是多物件：

1. 全選物件
2. `Ctrl + L`
3. 選 **Copy Modifiers**

👉 記得匯出 OBJ 時：

* 勾選 **Apply Modifiers**

貼圖也可以一併降低解析度，

> 視需求使用，不要無腦壓。

---

## 成果檢查

* 如果畫面怪怪的，先把 **RTX 材質包關掉**
* 重新進世界測試

**真香。**

---

冰淇淋 🍦
再見 👋
