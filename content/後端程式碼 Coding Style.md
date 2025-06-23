MOC:: [[乾淨程式碼.MOC|clean code]]

# 跟 commit 相關

-   commit 前請檢查自己的程式碼有沒有不必要的測試程式碼，盡量不要 commit 上去，就算不小心 commit 上去，事後再刪除也 OK
-   commit 前請檢查自己的程式碼有沒有在開發過程中後來發現不需要寫而註解掉的，請刪除它，不要留一大串無意義也根本不會再使用的程式碼。例如下方的註解就應該刪掉:

❌

```cs
public void GetNewCaseNo(string caseNo)
{
	string no = caseNo.Substring(5);
	//int noInt = int.Parse(no);
	bool tryParse = int.TryParse(no, out int noInt);
}
```

-   commit 前請檢查自己的程式碼有沒有不必要的 using，盡量不要 commit 上去
-   同一個 Service 超過 10 個函式方法時，請盡量使用 `region` 包住，增加程式碼的可讀性
-   在一個函式方法內，當寫到程式碼超過 100 行時，即使這個邏輯不一定有要共用的地方，也請考慮是否有邏輯可以單獨抽出來，以增加程式碼的可讀性。希望減少像林鐵那樣單一函式可以 500 多行甚至 1000 行的，維護起來非常不容易。
    -   如果真的無法抽出邏輯，也請盡量使用 `region` 將你函式中做的不同的事，各自包起來
-   現在公司新專案大多採用依賴注入的方式，請在寫 `interface` 時，至少將 summary 寫在 `interface` 檔案，因為如果只寫在實作的 service 檔案內的話，其他檔案在引用該函式時不會出現 summary 的內容，因為此時吃的是 `interface` 的 summary 而非實作檔案的 summary
-   函式外面的註解不要用 `//` ，請使用 `summary`

❌

```cs
// 使用者管理: 新增資料
public async Task<(bool Success, string Message)> Create(CreateModel model)
{
	// ...
}
```

✔️

```cs
/// <summary>
/// 使用者管理: 新增資料
/// </summary>
/// <param name="createModel"></param>
/// <returns></returns>
Task<(bool Success, string Message)> Create(CreateModel model);
```

# 寫共用方法

-   可能會共用的記得抽出來共用，共用方法盡量不要耦合太多東西，導致其他人難以使用。
-   如果真的需要很多其他參數的話，請用再包裝的方式。

範例:

-   原函式

```cs
public async Task SendMail(string to, string subject, List<string> replaceTemplate , string fileId, string level, List<string>? listFilePath = null)
{
	// 將 subject 與 templateName 混為一談
	// 預設一定顯示 LineQRCode
}
```

-   修改後

```cs
public async Task SendMail(string to, string templateName, List<string> replaceTemplate ,
    string? scheduleFileId = null, string? level = null,
    bool showLineQRCode = true, List<string>? listFilePath = null)
{
}
```

-   引用時的寫法

```cs
// 原函式寫法
await mailService.SendMail(email, "會員密碼重新設定", tempReplace, "", "");

// 新寫法
await mailService.SendMail(email, "會員密碼重新設定", tempReplace);
```

-   再包裝的方式

```cs
/// <summary>
/// 改變角色信件寄送
/// </summary>
/// <param name="to">收件人mail address</param>
/// <param name="subject"></param>
/// <returns></returns>
public async Task SendChangeRoleMail(string account, string role1, string role2)
{
    var before = role.Read(x => x.uid == role1)?.Name ?? "";
    var after = role.Read(x => x.uid == role2)?.Name ?? "";
    await SendMail(account, "帳號權限異動", new List<string> { before, after }, showLineQRCode: false);
}
```

# 以常用語法區分

## If 的寫法

-   假設判斷式後括號內的內容很短，只有一行，可以使用下方寫法 1 嗎，還是一定要使用大括號?
-   順便其他我看過的寫法也一併討論看看吧

```cs
string showStr = "";
// 寫法1.
if (hasAdmissionTicket)
	showStr = "有遊樂區門票";
else
	showStr = "沒有遊樂區門票";

// 寫法2.
if (hasAdmissionTicket)
{
	showStr = "有遊樂區門票";
}
else
{
	showStr = "沒有遊樂區門票";
}

// 寫法3.
if (hasAdmissionTicket) {
	showStr = "有遊樂區門票";
} else {
	showStr = "沒有遊樂區門票";
}

// 寫法4.
if (hasAdmissionTicket)
{
	showStr = "有遊樂區門票";
} else
{
	showStr = "沒有遊樂區門票";
}

// 寫法5.
if (hasAdmissionTicket) showStr = "有遊樂區門票";
else showStr = "沒有遊樂區門票";
```

## try catch 的寫法

```cs
// 寫法1.
try
{

}catch(Exception ex)
{
	throw ex;
}

// 寫法2.
try
{

}
catch(Exception ex)
{

}

// 寫法3.
try
{

}
catch (Exception ex)
{

}

// 寫法4.
try
{

}
catch (Exception ex)
{
	string message = ex.ToString();
	throw ex;
}
```

## using (某個物件)

-   到底左括號前要不要有空格?

```cs
// 寫法1.
using(TransactionScope tx = new TransactionScope())
{

}

// 寫法2.
using (TransactionScope tx = new TransactionScope())
{

}
```

## 抓資料的寫法(where)

```cs
// 寫法1.
.Where(x => searchModel.areaId == null || searchModel.areaId.Count == 0 ? true
    : searchModel.areaId.Contains(x.UserExt!.PrimaryTaiwanAreaId ?? -1) || searchModel.areaId.Contains(x.UserExt.SecondaryTaiwanAreaId ?? -1)
).Where(...)

// 寫法2.
.Where(x => searchModel.areaId == null || searchModel.areaId.Count == 0 ? true
    : searchModel.areaId.Contains(x.UserExt!.PrimaryTaiwanAreaId ?? -1) || searchModel.areaId.Contains(x.UserExt.SecondaryTaiwanAreaId ?? -1))
.Where(...)
```

![[Pasted image 20250114155029.png]]

# 變數命名

-   變數命名
    -   class / method / enum / 公開變數 / namespace：**Pascal Case**
    -   本地變數：camel Case
    -   private, protected, internal 變數：\_camel Case

*   檔案命名
    -   介面使用 I 開頭，例:ICaseInterface
    -   檔案命名：PascalCase，例:MyFile.cs
    -   檔案名稱盡量要跟 main class 一樣
        -   正常來說，偏好一個檔案一個核心 class

Modifiers (存取修飾詞)順序:

1. public
2. protected
3. internal
4. private
5. new
6. abstract
7. virtual
8. override
9. sealed
10. static
11. readonly
12. extern
13. unsafe
14. volatile
15. async

類別內容的順序：

1. 巢狀類別、enum、委派、事件(Nested classes, enums, delegates and events.)
2. Static, const and readonly fields.
3. Fields and properties.
4. Constructors and [[finalizers]].
5. Methods.

fields Vs. properties

| 特性     | Fields (欄位)      | Properties (屬性)              |
| -------- | ------------------ | ------------------------------ |
| 資料存取 | 直接存取           | 間接存取（透過 get 和 set）    |
| 邏輯控制 | 無法加入邏輯       | 可以加入邏輯（例如驗證或轉換） |
| 封裝性   | 通常對外公開不安全 | 提供封裝，保護內部結構         |
| 靈活性   | 靜態、簡單         | 動態，可擴展                   |
| 可測試性 | 無法對存取進行測試 | 可以測試 get 和 set 的邏輯     |

其每個內容內的順序是：

> 1. Public.
> 2. Internal
> 3. Protected internal.
> 4. Protected
> 5. Private

- 即使大括號是不必要的，還是要盡量使用。(Braces used even when optional.)
- if /for /while 與逗號等符號後加上 space (Space after if/for/while etc., and after commas.)


unary operator(一元運算符，例: +, -運算符號) 與 operand (運算數: 是指數學運算的對象)之間要加 space。

```cs
var k = 1 + 1;
var l = 5 / 2;
```

斷行後接續的使用 4 個 space 做縮排。(In general, line continuations are indented 4 spaces.)

```cs
if (celebrationThisYear != celebrationNextYear && celebration.type != "New Year" && celebration.Year >= DateTime.Now.Year &&
    (celebration.Attendance >= 100 || celebration.Attendance < 50)
)
{

}
```

方法帶入的參數如果太多，可以拆成好幾行，且每行要對齊第一個參數。

```cs
public class ExampleService
{
	private readonly IJsonMapService jsonMapService;
	private readonly IGetLastUpdateService getLastUpdate;
	private readonly IGetUserService getUserService;
	private readonly IMailService mail;
	private readonly IRepository<Case> usecase;
	private readonly IRepository<CaseTT> tap;
	private readonly IRepository<CaseDetail> detail;
	private readonly IRepository<CaseHistory> history;
	
    public ExampleService (
	    IJsonMapService _jsonMapService,
		IGetLastUpdateService _getLastUpdate,
		IGetUserService _getUserService,
		IMailService _mail,
		IRepository<Case> _case,
		IRepository<CaseTT> _tap,
		IRepository<CaseDetail> _detail,
		IRepository<CaseHistory> _history)
    {
	    jsonMapService = _jsonMapService;         // 轉換json
		getUserService = _getUserService;         // 轉換使用者
		getLastUpdate = _getLastUpdate;           // 轉換西元民國
		mail = _mail;                             // 寄信
		usecase = _case;                          // Case表的儲存體
		tap = _tap;                               // CaseTT表的儲存體
		detail = _detail;                         // CaseDetail表的儲存體
		history = _history;                       // CaseHistory表的儲存體
    }
}
```

