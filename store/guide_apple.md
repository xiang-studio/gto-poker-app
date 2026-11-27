# Apple TestFlight：照順序做（用電腦瀏覽器）

依據：Apple App Store Connect 說明（2026/11/27 查）。iOS 沒有「12 人 14 天」的規定。

## 事前準備
- [ ] 收到 Apple 的會員啟用信（付款後最多 48 小時）
- [ ] 公開聯絡信箱、隱私權政策網址

## A. 註冊 App ID（developer.apple.com/account）
1. **Certificates, Identifiers & Profiles → Identifiers → ＋**
2. 選 **App IDs → App**
3. Description：`GTO Poker Lab`；Bundle ID 選 **Explicit**，填 **`com.xiang.pokergto`**（必須一字不差）
4. Capabilities 勾 **In-App Purchase**（預設已勾）→ Continue → Register

## B. 建立 App（appstoreconnect.apple.com）
1. **Apps → ＋ → New App**
2. Platforms：iOS；Name：`GTO 德撲研究所`；Primary Language：Traditional Chinese
3. Bundle ID：選剛剛的 `com.xiang.pokergto`；SKU：`pokergto001`；User Access：Full Access → Create

## C. 產生 API 金鑰（讓雲端可以幫你打包上傳）
1. App Store Connect → **Users and Access → Integrations**（第一次要按 Request Access 同意條款）
2. **Team Keys → Generate API Key**；Name：`github`；Access：**App Manager**
3. 記下頁面上的 **Issuer ID** 和這把金鑰的 **Key ID**
4. 按 **Download API Key** 下載 `.p8` 檔。**只能下載一次**，存好並備份
5. Team ID：developer.apple.com/account → Membership details 裡的 **Team ID**（10 碼）

## D. 把四個值填進 GitHub（你自己填，我不經手金鑰）
github.com/xiang-studio/gto-poker-app → **Settings → Secrets and variables → Actions → New repository secret**，建四個：
| Name | 值 |
|---|---|
| `ASC_KEY_ID` | Key ID |
| `ASC_ISSUER_ID` | Issuer ID |
| `ASC_KEY_P8` | 用記事本打開 .p8，把全部內容（含 BEGIN／END 那兩行）貼上 |
| `APPLE_TEAM_ID` | Team ID |

## E. 打包並上傳
1. GitHub → **Actions → iOS build → Run workflow**
2. mode 選 **testflight**；build_number 填 `1`（之後每次上傳都要比上次大）→ Run
3. 約 10–15 分鐘。成功後到 App Store Connect → 你的 App → **TestFlight**，幾分鐘到半小時內會出現這個版本（狀態 Processing → 可測試）

## F. 邀請測試者
- **自己先測（Internal）**：TestFlight → Internal Testing → ＋ 建群組 → 加你自己 → 手機裝 TestFlight App 即可安裝，不用審查
- **朋友（External）**：TestFlight → External Testing → ＋ 建群組 → Add Builds 選版本 → 填 Test Information（Beta App Description、Feedback Email）→ 送出。**第一個版本要經過 Beta App Review**（通常一兩天），通過後可用 Email 邀請或開啟 **Public Link** 直接把連結丟給朋友（上限 10,000 人）

## G. App 內購買（完整版）
1. App Store Connect → 你的 App → **Monetization → In-App Purchases → ＋**
2. Type：**Non-Consumable**；Reference Name：`Pro Unlock`；**Product ID：`pro_unlock`**（必須一字不差）
3. 設定價格、加上繁體中文的顯示名稱與說明
4. 第一次要先到 **Business（協議、稅務與銀行）** 簽 Paid Apps 協議並填銀行與稅務資料，內購才會生效。TestFlight 裡的購買不會真的扣款

## H. 正式上架（之後）
App Store 分頁填商店資訊、截圖（6.9 吋與 6.5 吋）、隱私權「App Privacy」選 **Data Not Collected**、年齡分級問卷（模擬賭博：有）、選版本 → Submit for Review。
