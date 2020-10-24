## Laravel 規則

### Migrate 與 Model
Migrate 可以操作控制資料庫的結構！類似 adminer, phpadmin 的CLI 轉圖形化概念。只是 它是 CLI 轉檔案化概念，利用檔案去控制、讀寫。
Model 使用者介面，就如同 ORM 的 Model，決定要寫入資料庫的模組。但他不會操作資料庫的結構等，可以決定寫入的資料。

但是 Node.js ORM 利用了 Model 是包含了 laravel(migrate, model)，而 Laravel 把它分開了

### One to One
在 migration 的A檔案，寫入了 $table->unsignedBigInteger('user_id'); ，給個 B 檔案的 FK 欄位。
然後在 model 的檔案，寫入了 public function 並用到了 belongsTo(User::class)，
這裡有個慣用法，我們並沒有指定 A 表的 user_id 欄位關聯至 B 檔案的 id 欄位，
但是 user + _ + id 就間接代表了 User Model 的 id 欄位。
另外 belongsTo 還有幾個參數可以輸入，例如第二個就可以指定 FK

有時候，Http\Request->hasFile === false
明明就有上傳檔案，卻跟我說沒有，
因為 php.ini 的 upload_max_filesize 是預設 2M，超過了所以變成 false
One thing important to note is I also had to change post_max_size to be greater than upload_max_filesize.

### Flash session
閃存 session 可以在當前製作 session 直到回應完下個 request 。
Request -> controller 某行執行 session 儲存 -> return view() check has session: true 這時候還可以取得 session 並且不會刪除
下一個 request -> 可以取得 session -> 還是可以取得 session -> 等到結束回應後 -> session 這時就被刪除了
