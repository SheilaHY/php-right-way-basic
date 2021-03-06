
# 整洁代码之道
## 有意义的命名
### 名副其实
变量名、函数名应该已经回答了所有的大问题，它告诉你  
它为什么存在、它做什么事、应该怎么用  
如果名称需要注释来补充，就不算是名副其实
```php
$d = 0; //消逝的时间，以日计
```
vs
```php
$elapseTimeInDays = 0;
$daysSinceCreation = 0;
```

以下代码的目的何在？
```java
public List<int[]> getThem() {
  List<int[]> list1 = new ArrayList<int[]>();
  for (int[] x : theList)
    if (x[0] == 4) list1.add(x);
  return list1; 
}
```
换个有意义的名字
```java
public List<int[]> getFlaggedCells() {
  List<int[]> flaggedCells = new ArrayList<int[]>(); 
  for (int[] cell : gameBoard)
    if (cell[STATUS_VALUE] == FLAGGED) 
      flaggedCells.add(cell);
  return flaggedCells; 
}
```
### 避免误导
* 提防使用不同之处较小的名称  
XYZControllerForEfficientHandlingOfStrings
XYZControllerForEfficientStorageOfStrings

* 避免使用误导性的名称  
```java
int a = l;
if ( O == l )
 a = O1;
else
 l = 01;
 ```
### 做有意义的区分
以下a1和a2有什么区别，它们的意图是什么？
```java
public static void copyChars(char a1[], char a2[]) {
 for (int i = 0; i < a1.length; i++) {
 a2[i] = a1[i];
 }
 }
```
如果把a1,a2分别改为source和distination，函数可以很容易被阅读者理解
```java
public static void copyChars(char source[], char distination[]) {
  for (int i = 0; i < a1.length; i++) {
    a2[i] = a1[i];
  }
}
```

* 无意义的区分示例
```
Product 与 ProductData 与 ProductInfo  
NameString 与 Name
Customer 与 CustomerObject
getAccount 与 getAccounts 与 getAccountInfo
moneyAmount 与 money
customer 与 customerInfo
```

### 使用读的出来的名称
```java
class DtaRcrd102 {
  private Date genymdhms; //出生年与日时分秒
  private Date modymdhms; //修改年与日时分秒
  private final String pszqint = "102";
  /* ... */
};
```
to
```java
class Customer {
  private Date generationTimestamp;
  private Date modificationTimestamp;;
  private final String recordId = "102";
  /* ... */
};
```
### 使用可搜索的名称
单字母名称和常量很难在一大篇文章中找出来，比如找到MAX_CLASSES_PER_STUDENT很容易，但要找出数字7就难了。
名称的长短应与其作用域大小相对应，若变量或常量可能在代码多出使用，则应赋予其便于搜索的名称
```java
for (int j=0; j<34; j++) {
  s += (t[j]*4)/5;
}
```
to
```java
int realDaysPerIdealDay = 4;
const int WORK_DAYS_PER_WEEK = 5;
int sum = 0;
for (int j=0; j < NUMBER_OF_TASKS; j++) {
  int realTaskDays = taskEstimate[j] * realDaysPerIdealDay;
  int realTaskWeeks = (realdays / WORK_DAYS_PER_WEEK);
  sum += realTaskWeeks;
}
```
### 避免使用编码
* 不必用m_前缀表明成员变量

### 避免思维映射
每个人的思维映射可能存在差异，名称要直接明了，避免取具有特殊含义的映射性名称

### 以名词或短语作为类名
* 类名尽量用名词或名词短语，比如Customer, WikiPage,Account, 或 AddressParser。

### 以动词或动词词组命名方法
* 方法名尽量用动词或动词短语。比如postPayment, deletePage, 或者save。

### 名称要一目了然，通俗易懂，避免使用具有隐晦含义的词
eatMyShorts =>abort

### 每个概念统一使用一个词
例如，使用fetch、retrieve和get来给在多个类中的同种方法命名，你怎么记得住哪个类中是哪个方法呢？  
如yii中查询数据库的方法findByPK, findByCondition, findById等均以find开头

### 避免将同一单词用于不同的目的

### 使用解决方案领域名词
* 专业术语、算法名、模式名等，如：JobQueue, AccountVisitor

### 使用源自所涉及问题领域的名称
* 开发用于零售业务的系统中使用零售业的专有名词：SKU代表库存量，DeadStock代表坏账等

### 添加有意义的语境
* 举个栗子，假如我们有名为firstName、lastName、street、houseNumber、city、state和zipcode的变量。当他们搁一块儿的时候，的确是构成了一个地址。不过，假如只是在某个方法中看到一个孤零零的state呢？我们会推断这个变量是地址的一部分吗？

我们可以添加前缀addrFirstName、addrLastName、addrState等，以此提供语境。至少，读者可以知道这些变量是某个更大变量的一部分。当然，更好的方案是创建名为Address的类。这样，即便是编译器也会知道这些变量是隶属于某个更大的概念了。
无上下文语境
```java
private void printGuessStatistics(char candidate, int count) {
  String number;
  String verb;
  String pluralModifier;
  if (count == 0) {
    number = "no";
    verb = "are";
    pluralModifier = "s";
  } else if (count == 1) {
    number = "1";
    verb = "is";
    pluralModifier = "";
  } else {
    number = Integer.toString(count);
    verb = "are";
    pluralModifier = "s";
  }
  String guessMessage = String.format(
  "There %s %s %s%s", verb, number, candidate, pluralModifier
  );
  print(guessMessage);
}
```

添加上下文后
```java
public class GuessStatisticsMessage {
  private String number;
  private String verb;
  private String pluralModifier;
  public String make(char candidate, int count) {
  
  createPluralDependentMessageParts(count);
   return String.format(
     "There %s %s %s%s",
     verb, number, candidate, pluralModifier );
 }
  private void createPluralDependentMessageParts(int count) {
    if (count == 0) {
      thereAreNoLetters();
    } else if (count == 1) {
      thereIsOneLetter();
    } else {
      thereAreManyLetters(count);
    }
  }
  private void thereAreManyLetters(int count) {
    number = Integer.toString(count);
    verb = "are";
    pluralModifier = "s";
  }
  private void thereIsOneLetter() {
    number = "1";
    verb = "is";
    pluralModifier = "";
  }
  private void thereAreNoLetters() {
    number = "no";
    verb = "are";
    pluralModifier = "s";
  }
}
```

### 别添加没有意义的语境
* 比如在开发一个GSD应用程序，类命名为GSDAccountAddress，其中GSD、Account都是与当前语境毫无关联的。

## 函数

```java
public static String testableHtml(
  PageData pageData,
  boolean includeSuiteSetup
  ) throws Exception {
  WikiPage wikiPage = pageData.getWikiPage();
  StringBuffer buffer = new StringBuffer();
  if (pageData.hasAttribute("Test")) {
    if (includeSuiteSetup) {
      WikiPage suiteSetup = PageCrawlerImpl.getInheritedPage(SuiteResponder.SUITE_SETUP_NAME, wikiPage);
        if (suiteSetup != null) {
          WikiPagePath pagePath = suiteSetup.getPageCrawler().getFullPath(suiteSetup);
          String pagePathName = PathParser.render(pagePath);
          buffer.append("!include -setup .")
                .append(pagePathName)
                .append("\n");
        }
    }
    WikiPage setup = PageCrawlerImpl.getInheritedPage("SetUp", wikiPage);
    if (setup != null) {
      WikiPagePath setupPath = wikiPage.getPageCrawler().getFullPath(setup);
      String setupPathName = PathParser.render(setupPath);
      buffer.append("!include -setup .")
            .append(setupPathName)
            .append("\n");
    }
  }
  buffer.append(pageData.getContent());
  if (pageData.hasAttribute("Test")) {
    WikiPage teardown = PageCrawlerImpl.getInheritedPage("TearDown", wikiPage);
    if (teardown != null) {
      WikiPagePath tearDownPath = wikiPage.getPageCrawler().getFullPath(teardown);
      String tearDownPathName = PathParser.render(tearDownPath);
      buffer.append("\n")
            .append("!include -teardown .")
            .append(tearDownPathName)
            .append("\n");
  }
  if (includeSuiteSetup) {
    WikiPage suiteTeardown = PageCrawlerImpl.getInheritedPage( SuiteResponder.SUITE_TEARDOWN_NAME, wikiPage );
    if (suiteTeardown != null) {
      WikiPagePath pagePath = suiteTeardown.getPageCrawler().getFullPath (suiteTeardown);
      String pagePathName = PathParser.render(pagePath);
      buffer.append("!include -teardown .")
            .append(pagePathName)
            .append("\n");
    }
 }
 }
 pageData.setContent(buffer.toString());
 return pageData.getHtml();
}
```
做的事情太多，不同层级的抽象放在了一个函数中  
程序逻辑脉络不直观清晰  
多重嵌套  
重复的逻辑判断  

重构后：
```java
 public static String renderPageWithSetupsAndTeardowns(
   PageData pageData, boolean isSuite
 ) throws Exception {
   boolean isTestPage = pageData.hasAttribute("Test");
   if (isTestPage) {
     WikiPage testPage = pageData.getWikiPage();
     StringBuffer newPageContent = new StringBuffer();
     includeSetupPages(testPage, newPageContent, isSuite);
     newPageContent.append(pageData.getContent());
     includeTeardownPages(testPage, newPageContent, isSuite);
     pageData.setContent(newPageContent.toString());
   }
   return pageData.getHtml();
 }
```

### 短小，不超过20行
### 一个函数只做一件事
### 每个函数一个抽象层级，自顶向下的阅读顺序
### switch语句的抽象
### 使用描述性的名词命名函数
* testableHtml => renderPageWithSetupsAndTeardowns
### 函数参数
* 参数个数尽量地少
* 参数顺序遵从自然规则，如equal()
### 无副作用
```java
public class UserValidator {
  private Cryptographer cryptographer;
  public boolean checkPassword(String userName, String password) {
    User user = UserGateway.findByName(userName);
    if (user != User.NULL) {
      String codedPhrase = user.getPhraseEncodedByPassword();
      String phrase = cryptographer.decrypt(codedPhrase, password);
        if ("Valid Password".equals(phrase)) {
          Session.initialize();
          return true;
      }
    }
    return false;
  }
}
```
### 分割指令与询问
函数的目的要么是做什么事，要么是回答什么事，二者不可兼得。
```java
public boolean set(String attribute, String value);
```
该函数设置某个指定的属性，如果成功返回true, 如果不存在返回false, 所以有以下用法：
```java
if (set("username", "unclebob"))...
```
从读者角度考虑一下这是什么意思？set成功？还是username以前的值==unclebob
```java
if (attributeExists("username")) {
  setAttribute("username", "unclebob");
... }
```
### 使用异常代替返回错误码
```java
if (deletePage(page) == E_OK) {
  if (registry.deleteReference(page.name) == E_OK) {
    if (configKeys.deleteKey(page.name.makeKey()) == E_OK){
      logger.log("page deleted");
    } else {
      logger.log("configKey not deleted");
    }
  } else {
    logger.log("deleteReference from registry failed");
  }
} else {
  logger.log("delete failed");
  return E_ERROR;
}
```
重构后：
```java
try {
  deletePage(page);
  registry.deleteReference(page.name);
  configKeys.deleteKey(page.name.makeKey());
} catch (Exception e) {
  logger.log(e.getMessage());
}
```
### 抽离try/catch
* 把try/catch代码块的主体抽离出来，另外形成函数。
```java
public void delete(Page page) {
  try {
    deletePageAndAllReferences(page);
  } catch (Exception e) {
    logError(e);
  }
}

private void deletePageAndAllReferences(Page page) throws Exception {
  deletePage(page);
  registry.deleteReference(page.name);
  configKeys.deleteKey(page.name.makeKey());
}

private void logError(Exception e) {
  logger.log(e.getMessage());
}
```
### 别重复自己(DRY原则)
### 结构化编程，单入单出
* 保持一个return
### 反复打磨代码
### 整洁代码示例
```c#
using System;  
  
public class SetupTeardownIncluder  
{  
    private PageData pageData;  
    private boolean isSuite;  
    private WikiPage testPage;  
    private StringBuffer newPageContent;  
    private PageCrawler pageCrawler;  
  
    public static String render(PageData pageData) throws Exception  
    {  
        return render(pageData, false);  
    }  
    
    public static String render(PageData pageData, boolean isSuite)throws Exception  
    {  
        return new SetupTeardownIncluder(pageData).render(isSuite);  
    }  
    
    private SetupTeardownIncluder(PageData pageData)  
    {  
        this.pageData = pageData;  
        testPage = pageData.getWikiPage();  
        pageCrawler = testPage.getPageCrawler();  
        newPageContent = new StringBuffer();  
    }  
  
    private String render(boolean isSuite) throws Exception  
    {  
        this.isSuite = isSuite;  
        if (isTestPage())  
        includeSetupAndTeardownPages();  
        return pageData.getHtml();  
    }  
  
    private boolean isTestPage() throws Exception  
    {  
        return pageData.hasAttribute("Test");  
    }  
  
    private void includeSetupAndTeardownPages() throws Exception  
    {  
        includeSetupPages();  
        includePageContent();  
        includeTeardownPages();  
        updatePageContent();  
    }  
  
    private void includeSetupPages() throws Exception  
    {  
        if (isSuite)  
            includeSuiteSetupPage();  
        includeSetupPage();  
    }  
  
    private void includeSuiteSetupPage() throws Exception  
    {  
        include(SuiteResponder.SUITE_SETUP_NAME, "-setup");  
    }  
  
    private void includeSetupPage() throws Exception  
    {  
        include("SetUp", "-setup");  
    }  
  
    private void includePageContent() throws Exception  
    {  
        newPageContent.append(pageData.getContent());  
    }  
  
    private void includeTeardownPages() throws Exception  
    {  
        includeTeardownPage();  
        if (isSuite)  
        includeSuiteTeardownPage();  
    }  
  
    private void includeTeardownPage() throws Exception  
    {  
        include("TearDown", "-teardown");  
    }  
  
    private void includeSuiteTeardownPage() throws Exception  
    {  
        include(SuiteResponder.SUITE_TEARDOWN_NAME, "-teardown");  
    }  
  
    private void updatePageContent() throws Exception  
    {  
        pageData.setContent(newPageContent.toString());  
    }  
  
    private void include(String pageName, String arg) throws Exception  
    {  
        WikiPage inheritedPage = findInheritedPage(pageName);  
        if (inheritedPage != null)   
        {  
            String pagePathName = getPathNameForPage(inheritedPage);  
            buildIncludeDirective(pagePathName, arg);  
        }  
    }  
  
    private WikiPage findInheritedPage(String pageName) throws Exception  
    {  
        return PageCrawlerImpl.getInheritedPage(pageName, testPage);  
    }  
  
    private String getPathNameForPage(WikiPage page) throws Exception  
    {  
        WikiPagePath pagePath = pageCrawler.getFullPath(page);  
        return PathParser.render(pagePath);  
    }  
  
    private void buildIncludeDirective(String pagePathName, String arg)  
    {  
        newPageContent  
        .append("\n!include ")  
        .append(arg)  
        .append(" .")  
        .append(pagePathName)  
        .append("\n");  
    }  
}  
```

## 注释
### 程序员不能坚持维护注释
### 注释不能美化糟糕的代码，写注释前先优化代码
### 用代码来阐述代替注释
### 好注释
### 坏注释
