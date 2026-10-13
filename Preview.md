# 【CMSコンテンツ】コンポーネント仕様書_CDBSP0TBU_Web Content/ Page/ Object Preview

## 1. 表紙

<h1 style="text-align:center;">

ネット投票システム(RAISE)／コンポーネント仕様書
Web Content/ Page/ Object Preview

</h1>

<h3 style="text-align:center;">

```
2026年06月01日
第 00.01 版
オッズ・パーク株式会社
```

</h3>

## 2. 変更履歴


| 版数  | 変更日         | 変更ID | 変更箇所 | 変更内容 | 更新者 | 承認日付 | 承認者 |
| ------- | ---------------- | :------- | ---------- | ---------- | -------- | ---------- | -------- |
| 00.01 | 2026年06月23日 | -      | -        | 新規作成 | TuNA65 |          |        |

## 3. インプット一覧


| ドキュメント | 詳細                    | ID/No      | 名称                    |
| -------------- | ------------------------- | ------------ | ------------------------- |
| 要件定義書   | 業務フロー              | A06-050106 | キャンペーン_運用管理   |
| 要件定義書   | 業務処理定義書          | A06_006    | キャンペーン運用管理    |
| 要件定義書   | システム化業務説明書    | A06-161    | 投票促進・リテンション  |
| 要件定義書   | レコメンドカード画面API | AMDBCM500  | レコメンドカード画面API |

## 4. Author Configuration

Luồng hiển thị dữ liệu trong Liferay: từ Object/ Web Content đến Display Page Template và Static Page đến Page Template

1. Object → Display Page Template

Creator nhập dữ liệu tại Object Entries, với các field được định nghĩa trước trong Object Definition.
Để hiển thị dữ liệu, Admin có thể sử dụng Info Template (4.1.4.1 Màn hình Info Template) để định nghĩa cách render dữ liệu của Object.
Để hiển thị dưới dạng một trang hoàn chỉnh, cần sử dụng Display Page Template, trong đó dữ liệu Object được bind vào các component hoặc Info Template.

2. Web Content → Display Page Template

Creator nhập dữ liệu vào các field của Web Content. Các field này được định nghĩa trước bởi Admin thông qua Web Content Structure.
Để hiển thị dữ liệu, Admin cấu hình Web Content Structure Template (4.1.4.1 Màn hình Web Content Template) nhằm mapping các field dữ liệu sang giao diện hiển thị.
Web Content hỗ trợ preview trực tiếp tại màn hình tạo/chỉnh sửa, cho phép Creator xem trước nội dung đã nhập theo template.
Ngoài ra, khi cần hiển thị dưới dạng trang chi tiết đầy đủ, Admin có thể cấu hình Display Page Template để định nghĩa layout của trang.

3. Static Page → Page Template

Creator có thể tạo các trang (Page) thông qua Page Template hoặc trực tiếp bằng cách kéo thả các component (fragment, widget) có sẵn trong Liferay.
Các Page này không gắn với một data item cụ thể mà chủ yếu dùng để xây dựng các trang tĩnh hoặc bán động (ví dụ: Home, Landing Page).

![1782092356190.drawio](image/1782092356190.drawio.png)

Preview là chức năng cho phép người dùng xem trước nội dung hoặc trang dưới dạng hiển thị gần với thực tế trước khi publish. Chức năng này giúp kiểm tra dữ liệu, giao diện, layout và cách render của template/ component.

Nội dung (trang) được tạo và preview bởi Author trên Staging, sau đó gửi yêu cầu phê duyệt; System xác thực dữ liệu và kích hoạt workflow cho Reviewer kiểm tra. Sau khi được approve, nội dung được đẩy lên Staging FE và tiến hành publish ra môi trường Live.

Preview là chức năng  tùy chọn (optional) , cho phép người dùng kiểm tra nội dung trên môi trường Staging trước khi gửi phê duyệt, nhưng không phải là bước bắt buộc trong workflow.

![1780996438196.drawio](image/1780996438196.drawio.png)

### 4.1 画面レイアウト (Screen Layout)

#### 4.1.1 Màn hình Configuration

##### 4.1.1.1 Màn hình Web Content Configuration của Author/ Reviewer

* User select button Preview to view data. Tham khảo tài liệu common tại đây [Common](https://insight.fsoft.com.vn/confluence/spaces/OPNEXT88/pages/592004936/3.5.10.+CMS+team)

 ![1779868311080.drawio](image/1779868311080.drawio.png)

##### 4.1.1.2 Màn hình Page Configuration của Author/ Reviewer

* User select button Preview to view data. Tham khảo tài liệu common tại đây [Common](https://insight.fsoft.com.vn/confluence/spaces/OPNEXT88/pages/592004936/3.5.10.+CMS+team)
  ![1780042761823.drawio](image/1780042761823.drawio.png)

##### 4.1.1.3 Màn hình Object Configuration của Author/ Reviewer

* User select button Preview to view data. Tham khảo tài liệu common tại đây [Common](https://insight.fsoft.com.vn/confluence/spaces/OPNEXT88/pages/592004936/3.5.10.+CMS+team)

![1781081587203.drawio](image/1781081587203.drawio.png)

#### 4.1.2 Màn hình Login

Khi user click Preview:
-Nếu chưa login → hệ thống redirect đến màn hình Login

-Nếu đã login → chuyển trực tiếp đến màn hình Preview:

* Nếu User đang ở màn hình Web Content Configuration thì sẽ đến màn hình Preview Web Content (4.1.3.1 Màn hình Preview Web Content khi có nhận dữ liệu thành công)
* Nếu User đang ở màn hình Page Configuration thì sẽ đến màn hình Preview Page (4.1.3.2. Màn hình Preview Page khi nhận được dữ liệu thành công)
* Nếu User đang ở màn hình Object Configuration thì sẽ đến màn hình Preview Page (4.1.3.3. Màn hình Preview Object khi nhận được dữ liệu thành công)

-Sau khi login thành công, user được redirect lại flow preview và hiển thị dữ liệu dựa trên cấu hình trong CMS

![1779876066806.drawio](image/1779876066806.drawio.png)

#### 4.1.3 Màn hình Preview Web Content/ Page/ Object

##### 4.1.3.1 Màn hình Preview Web Content khi có nhận dữ liệu thành công

![1780018940594.drawio](image/1780018940594.drawio.png)

##### 4.1.3.2. Màn hình Preview Page khi nhận được dữ liệu thành công

![1779932920460.drawio](image/1779932920460.drawio.png)

##### 4.1.3.3 Màn hình Preview Object

![1780969181286.drawio](image/1780969181286.drawio.png)

##### 4.1.3.4 Component của màn hình Preview Web Content/ Page/ Object khi bị lỗi hệ thống/ không thể lấy dữ liệu từ BFF API

![1780021242193.drawio](image/1780021242193.drawio.png)

##### 4.1.3.5 Màn hình Preview: Mix Component (Liferay, API) + Scratch's Component khi Preview Ready

###### 4.1.3.5.1 Màn hình Preview: Mix Component (Liferay, API) + Scratch's Component có dữ liệu

![1781142008383.drawio](image/1781142008383.drawio.png)

###### 4.1.3.5.2 Màn hình Preview: Mix Component (Liferay, API) + Scratch's Component khi CMS (Liferay) không lấy được dữ liệu (không có dữ liệu từ API)

![1781605266733.drawio](image/1781605266733.drawio.png)

##### 4.1.3.6 Màn hình Preview: Mix Component (Liferay, API) + Scratch's Component khi Preview is not Ready

###### 4.1.3.6.1 Màn hình Preview: Mix Component (Liferay, API) + Scratch's Component không có dữ liệu và CMS (Liferay) lấy được dữ liệu từ API

![1781086000286.drawio](image/1781086000286.drawio.png)

###### 4.1.3.6.2 Màn hình Preview: Mix Component (Liferay, API) + Scratch's Component không có dữ liệu và CMS (Liferay) không lấy được dữ liệu (không có dữ liệu từ API)

 ![1782896829364.drawio](image/1782896829364.drawio.png)

#### 4.1.4 Màn hình Configuration Admin

##### 4.1.4.1 Màn hình Web Content Template

![1780997767449.drawio](image/1780997767449.drawio.png)

##### 4.1.4.2 Màn hình Info Template

![1780997877052.drawio](image/1780997877052.drawio.png)

### 4.2 項目説明 (Item Description)

* Other fields captured in the UI belong to OOTB of CMS and are not related to this requirement. Refer to official guide document from Liferay here: https://learn.liferay.com/w/dxp/content-management-system/web-content/web-content-articles/creating-web-content-articles.
* Web Content Preview được trigger từ button (Preview) của màn hình quản lý Web Content Configuration
* Page Preview được trigger từ button (Preview in a New Tab) của màn hình quản lý Page Configuration


| 項番 | 項目名 (業務)                                                                          | 種類      | 必須 | OOTB | 備考                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ------ | ---------------------------------------------------------------------------------------- | ----------- | ------ | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | Preview                                                                                | Button    | O    | O    | + Khi Preview thì domain sẽ đổi nhưng data sẽ vẫn luôn ở trạng thái Staging (Draft/ Unpublished)<br />- Đối với  4.1.1.3 Màn hình Object Configuration của Author:<br />+ Button Nút Preview (プレビュー)  sẽ ở bên dưới Permissions (権限の設定) trong Object Entry Action.<br />![1781146493915.drawio](image/1781146493915.drawio.png)<br />+ Button Preview cho Object được thực hiện bằng cách tạo một preview flow: khi user click Preview, hệ thống lấy objectEntryId và render dữ liệu thông qua <br />Info Template và hiển thi khi mapping với Display Page Template.                                                                                                                                                                                                                                                           |
| 2    | Author Configuration Page                                                              | Screen    | O    | O    | Cho phép Admin cấu hình Page bao gồm Global Components (Header, Footer), Content Area, Navigation và Domain URL để tạo thành Full Page Context.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| 2.1  | Config API/ Scratch's Component                                                       | Component | X    | O    | Các component sử dụng API/Scratch’s Component không được preview hoặc cấu hình dữ liệu trên CMS.<br />Dữ liệu sẽ được load động tại runtime thông qua Client Extension gọi BFF API.<br />+ Nếu  lấy API thành công/ Sratch's component có data: hiển thị component<br />+ Nếu API lỗi/ Scratch's Component không có data: Môi trường Preview sẽ không Ready cho User. Chi tiết: (4.6 Preview Mix Component + Scrach's Component (môi trường Preview is not Ready ))                                                                                                                                                                                                                                                                                                                                                                            |
| 3    | Login Screen                                                                           | Screen    | O    | O    | Màn hình này cho phép người dùng đăng nhập vào hệ thống bằng cách nhập Odds Park ID và Password, hoặc sử dụng các dịch vụ bên ngoài (Google, LINE, Facebook, X, Yahoo! Japan, Apple).<br />Ngoài ra, người dùng có thể chọn  ghi nhớ đăng nhập , truy cập chức năng  quên mật khẩu , hoặc chuyển sang đăng ký tài khoản mới nếu chưa có tài khoản.<br />Khi thực hiện  Preview , chế độ hiển thị sẽ phụ thuộc vào vai trò của User (Admin, Creator, End User).<br />+ Đối với End User, một số component có thể thay đổi tùy theo Rank<br />+ Nội dung và cách hiển thị trong Preview được quyết định dựa trên  cấu hình do Creator hoặc Admin cấu hình.<br />+ Nếu user không có quyền → hiển thị lỗi (403 – Forbidden)                                                           |
| 4    | Preview Web Content/ Page                                                              | Screen    | O    | O    | Cho phép user preview nội dung Web<br />Content/ Page được cấu hình từ CMS<br />- Dữ liệu hiển thị được kết hợp từ:<br />+ CMS configuration (static content)<br />+ API response (dynamic data)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| 4.1  | Preview Web Content (Success)                                                         | Screen    | O    | X    | Hiển thị nội dung preview khi:<br />+ API trả dữ liệu thành công<br />+ Không xảy ra lỗi hệ thống, UI hiển thị dữ liệu đầy đủ theo mapping giữa CMS và API                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| 4.2  | Preview Page (Success)                                                                | Screen    | O    | X    | Hiển thị dưới dạng 1 Page hoàn chỉnh giống Product<br />Bao gồm các thành phần do User config và các Scratch's component/ API<br />Dữ liệu hiển thị giống với Preview Web Content nhưng sẽ là Full Page Context                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| 4.3  | Preview Object (Success)                                                              | Screen    | O    | O    | Cho phép user preview dữ liệu của Object Entry dựa trên cấu hình tại  ObjectDefinition + Info Template.<br />Dữ liệu preview được lấy từ:<br />+ Các field đã được nhập tại Object Entry (Object instance)<br />+ Mapping hiển thị tại Info Template (HTML template)<br />UI hiển thị nội dung theo mapping giữa field và template, không bao gồm full page và không sử dụng dữ liệu từ external API.                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| 4.4  | Component của Preview Web Content/ Page/ Object<br /> (Error)                         | Screen    | O    | O    | Hiển thị message lỗi trong các trường hợp:<br />+ Lỗi không nhận được dữ liệu từ CMS <br />component<br />+ Lỗi không nhận được dữ liệu từ API<br />+ Lỗi hễ thống<br />UI sẽ chỉ hiện thị error message và không render content tại component đó theo template: {Code} - {Context of use in the Raise System}<br />Example: "400 - リクエスト構文エラー、フィルタ形式不正、必須項目不足"<br />Chi tiết tham khảo: [4. HTTP Status Codes (Status Codes)](https://insight.fsoft.com.vn/confluence/spaces/OPNEXT88/pages/594654849/CMS+Liferay+API+Requirement)                                                                                                                                                                                                                                                                                 |
| 4.5  | Preview Mix Component + Scrach's Component (môi trường Preview Ready )             | Screen    | O    | O    | Hiển thị mô hình kết hợp giữa Liferay standard components và  custom components (Scratch/Client Extension) .<br />Các thành phần UI tĩnh (layout, tabs, filter, label…) được cấu hình qua Liferay.<br />- Các CMS Components không lấy được dữ liệu (không có dữ liệu từ API) sẽ:<br />+Không có dữ liệu hiển thị<br />+Hiển thị message lỗi với component tương ứng (4.4 Component của Preview Web Content/ Page/Object (Error))                                                                                                                                                                                                                                                                                                                                                                                                           |
| 4.6  | Preview Mix Component + Scrach's Component<br />(môi trường Preview is not Ready ) | Screen    | O    | O    | Hiển thị mô hình kết hợp giữa Liferay standard components và custom components (Scratch/Client Extension).<br />UI tĩnh được cấu hình và hiển thị bởi Liferay.<br />- Môi trường Preview is not Ready, các Scratch Components không lấy được API sẽ:<br />+ Không có dữ liệu hiển thị <br />+ Khi một component không được render (do lỗi, thiếu dữ liệu hoặc chưa sẵn sàng), các component phía dưới sẽ tự động dịch chuyển lên để lấp khoảng trống, theo thứ tự từ trên xuống dưới, đảm bảo layout liên tục và không xuất hiện khoảng trống.<br />- Các CMS Components không lấy được dữ liệu (không có dữ liệu từ API):<br />+Không có dữ liệu hiển thị<br />+Hiển thị message lỗi với component tương ứng (4.4 Component của Preview Web Content/ Page/Object (Error)) |
| 5    | Info Template/ Web Content Template                                                    | Screen    | O    | X    | Cho phép Admin cấu hình Freemarker<br />hiển thị dữ liệu dựa trên các field đã được định nghĩa trước đó.<br />Freemarker sẽ được sử dụng để render dữ liệu thành HTML trong các trường hợp:<br />+ Author nhập dữ liệu vào Web Content<br />+ Author nhập dữ liệu vào Object Entry                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| 5.1  | Web Content Structure Template                                                        | Screen    | O    | X    | Cho phép Admin cấu hình template hiển thị cho Web Content bằng Freemarker, sử dụng các field đã được định nghĩa trong<br />Web Content Structure.<br />Tham khảo tài liệu common tại đây: [Web Content Template](https://learn.liferay.com/w/dxp/content-management-system/web-content/web-content-templates/creating-web-content-templates)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| 5.2  | Info Template                                                                          | Screen    | O    | X    | Cho phép Admin cấu hình template hiển thị cho Object bằng Freemarker, sử dụng các field đã được định nghĩa trong Object Definition.<br />Tham khảo tài liệu common tại đây: [Info Template](https://learn.liferay.com/w/dxp/sites/displaying-content/using-information-templates)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |

## 5. Frontend Display

なし (N/A)

## 6. 補足

### 6.1 Preview Staging Liferay Server gap LIVE Liferay Server

Khi thực hiện preview trên môi trường Staging, dữ liệu hiển thị có thể khác so với môi trường Production (Live).

Nguyên nhân là do dữ liệu giữa các môi trường (CMS content hoặc API data) không được đồng bộ hoàn toàn tại cùng một thời điểm.

Do đó, có khả năng xảy ra sự chênh lệch giữa kết quả Preview và dữ liệu thực tế sau khi public lên LIVE

### 6.2 Danh sách các page được preview của phần Object và Web Content


| **Major**              | **Minor JP**       | **Minor Eng**                      | **Output Type** | **Func**                          | **Preview Page**        | **Mockup Link**                                                                                                                                                                                                                                                                                                             |
| ------------------------ | -------------------- | ------------------------------------ | ----------------- | ----------------------------------- | ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Information System** | お知らせ           | Notice                             | Object          | Important Notice Priority         | Notice Details          | [https://mockup.opnextlab.click/proto-pages/info/cm_ann_dtl](https://mockup.opnextlab.click/proto-pages/info/cm_ann_dtl "https://mockup.opnextlab.click/proto-pages/info/cm_ann_dtl")                                                                                                                                       |
| **Information System** | お知らせ           | Notice                             | Object          | Notice                            | Notice Details          |                                                                                                                                                                                                                                                                                                                             |
| **Information System** | オッズパークライブ | Oddspark Live (youtube livestream) | Object          | Odds Park live URL                | có thể là ko preview | [https://mockup.opnextlab.click/proto-pages/info/cm_ann_list?tab=keiba](https://mockup.opnextlab.click/proto-pages/info/cm_ann_list?tab=keiba "https://mockup.opnextlab.click/proto-pages/info/cm_ann_list?tab=keiba")                                                                                                      |
| **Information System** | オッズパークライブ | Oddspark Live (youtube livestream) | Object          | Odds Park live URL Log Audit     | có thể là ko preview |                                                                                                                                                                                                                                                                                                                             |
| **Information System** | オッズパーク映像   | Race Video                         | Object          | URL setup                         | Race Video              | [https://mockup.opnextlab.click/proto-pages/video/kb_rcvd?isShowModal=true&amp;race=keiba&amp;isFiltered=true](https://mockup.opnextlab.click/proto-pages/video/kb_rcvd?isShowModal=true&race=keiba&isFiltered=true "https://mockup.opnextlab.click/proto-pages/video/kb_rcvd?isshowmodal=true&race=keiba&isfiltered=true") |
| **Information System** | オッズパーク映像   | Race Video                         | Object          | Horce racing venue                | Race Video              |                                                                                                                                                                                                                                                                                                                             |
| **Information System** | オッズパーク映像   | Race Video                         | Object          | Bicycle racing velodrome          | Race Video              |                                                                                                                                                                                                                                                                                                                             |
| **Information System** | オッズパーク映像   | Race Video                         | Object          | Auto racing venue                 | Race Video              |                                                                                                                                                                                                                                                                                                                             |
| **Information System** | オッズパーク映像   | Race Video                         | Object          | Video bitrate                     | Race Video              |                                                                                                                                                                                                                                                                                                                             |
| **Information System** | 予想情報           | Forecast information               | Object          | Prediction Mark Object            |                         |                                                                                                                                                                                                                                                                                                                             |
| **Information System** | 予想情報           | Forecast information               | Object          | Prediction Article Object         |                         |                                                                                                                                                                                                                                                                                                                             |
| **Information System** | 予想情報           | Forecast information               | Object          | Mark Details Object               |                         |                                                                                                                                                                                                                                                                                                                             |
| **Information System** | 予想情報           | Forecast information               | Object          | Prediction Article details        |                         |                                                                                                                                                                                                                                                                                                                             |
| **Information System** | 予想情報           | Forecast information               | Object          | Recommended purchase Object       |                         |                                                                                                                                                                                                                                                                                                                             |
| **Information System** | 予想情報           | Forecast information               | Object          | LOTO Object                       |                         |                                                                                                                                                                                                                                                                                                                             |
| **Information System** | 予想情報           | Forecast information               | Object          | Keirin Race Overview Object       |                         |                                                                                                                                                                                                                                                                                                                             |
| **Information System** | 予想情報           | Forecast information               | Object          | Keiba Race Overview Object        |                         |                                                                                                                                                                                                                                                                                                                             |
| **Information System** | 予想情報           | Forecast information               | Object          | Auto Race Overview Object         |                         |                                                                                                                                                                                                                                                                                                                             |
| **Information System** | 予想情報           | Forecast information               | Object          | Keirin Annual Race object         |                         |                                                                                                                                                                                                                                                                                                                             |
| **Information System** | 予想情報           | Forecast information               | Object          | Keiba Annual Race Object          |                         |                                                                                                                                                                                                                                                                                                                             |
| **Information System** | 予想情報           | Forecast information               | Object          | Auto Annual Race Object           |                         |                                                                                                                                                                                                                                                                                                                             |
| **Information System** | 静的コンテンツ     | Static content                     | Object          | GA summary info                   | có thể là ko preview |                                                                                                                                                                                                                                                                                                                             |
| **Information System** | 読み物             | Reading material information       | Object          | Read Later object                 | Read Later Listing      |                                                                                                                                                                                                                                                                                                                             |
| **Information System** | 静的コンテンツ     | Static content                     | Web Content     | Glossary Term details web content | Glossary Term Details   | [https://mockup.opnextlab.click/proto-pages/terms/kr_glss_art](https://mockup.opnextlab.click/proto-pages/terms/kr_glss_art "https://mockup.opnextlab.click/proto-pages/terms/kr_glss_art")                                                                                                                                 |
| **Information System** | 静的コンテンツ     | Static content                     | Web Content     | Stadium details                   | Stadium Details         | [https://mockup.opnextlab.click/proto-pages/location/kb_loc_art](https://mockup.opnextlab.click/proto-pages/location/kb_loc_art "https://mockup.opnextlab.click/proto-pages/location/kb_loc_art")                                                                                                                           |
| **Information System** | 静的コンテンツ     | Static content                     | Web Content     | Terms & Conditions web content    | có thể là ko preview |                                                                                                                                                                                                                                                                                                                             |
| **Information System** | 読み物             | Reading material information       | Web Content     | News Detail Web Content           | News Details            | [https://mockup.opnextlab.click/proto-pages/keiba/kb_news_art](https://mockup.opnextlab.click/proto-pages/keiba/kb_news_art "https://mockup.opnextlab.click/proto-pages/keiba/kb_news_art")                                                                                                                                 |
| **Membership**         | キャンペーン       | Campaign                           | Web content     | Campaign Detail Web Content       | Campaign Details        | [https://mockup.opnextlab.click/proto-pages/campaign/cm_cp_art](https://mockup.opnextlab.click/proto-pages/campaign/cm_cp_art "https://mockup.opnextlab.click/proto-pages/campaign/cm_cp_art")                                                                                                                              |
