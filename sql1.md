# 基本指令

-- 使用 root 權限進入 MySQL

```sql
sudo mysql 
```



-- 在 MySQL 內建立資料庫

```sql
CREATE DATABASE mydb;
```



-- 因為有啟用安全設置，無法使用 root 權限登入 MySQL，因此需要建立一個新的使用者

```sql
CREATE USER 'uriel'@'uriel-BM6875-BM6675-BP6375' IDENTIFIED BY '密碼(111111)';
```



-- 授予 uriel 使用 mydb 的所有權限

```sql
GRANT ALL PRIVILEGES ON mydb.* TO 'uriel'@'uriel-BM6875-BM6675-BP6375';
```



-- 刷新權限

```sql
FLUSH PRIVILEGES;
```



-- 檢查目前使用者 howard 的權限

```sql
SHOW GRANTS FOR 'uriel'@'uriel-BM6875-BM6675-BP6375';
```



有設定才需要



- 使用 uriel 這個 MySQL 帳號登入 MySQL，需要輸入 MySQL 密碼 可以不用

```sql
mysql -u uriel -p
```



- 查詢 MySQL 所有使用者的認證方式

```sql
SELECT user, host, plugin FROM mysql.user;
```



-- 選擇要使用的資料庫

```sql
USE mydb;
```



-- 在資料庫中建立資料表

```sql
CREATE TABLE test (

  id INT AUTO_INCREMENT PRIMARY KEY,

  name VARCHAR(100) NOT NULL,

  age INT NOT NULL

);
```



-- 插入單筆資料

```sql
INSERT INTO test(name, age) VALUES('Mary', 18);
```



-- 插入多筆資料

```sql
INSERT INTO test (name, age) VALUES

('Bob', 30),

('Charlie', 22),

('David', 35),

('Eve', 28);
```



-- 查詢所有資料庫

```sql
SHOW DATABASES;
```



-- 查詢該資料庫內所有的資料表

```sql
USE mydb;

SHOW TABLES;
```



-- 刪除資料表

```sql
DROP TABLE test;
```



-- 查詢目前正在使用的資料庫

```sql
SELECT DATABASE();
```



-- 查詢資料表中的欄位類型

```sql
SHOW COLUMNS FROM test;
```



- 到BookDAO.java.將"jdbc:mysql://localhost:3306/bookdb?useUnicode=true&characterEncoding=UTF-8&serverTimezone=UTC";中bookdb改為自己資料庫名稱

錯誤訊息如下

The virtual machine 'linux' has terminated unexpectedly during startup with exit code -1073740791 (0xc0000409). More details may be available in 'C:\Users\ellen\VirtualBox VMs\linux\Logs\VBoxHardening.log'.

解決方法 

1. 重啟virtual box

2. 進bios，選項可能各家bios文字略有出入。關鍵字為virtual，該選項需設置為enable



## sql補充

### **🔍 问题根源：**

你的 MySQL 用户 'uriel' 的 **主机（host）绑定** 是：

- 'uriel'@'uriel-BM6875-BM6675-BP6375'

但是你尝试登录时的主机是：

- 'uriel'@'localhost'

🛑 **这两个不是同一个用户**，所以你使用密码登录 'uriel'@'localhost' 是无效的！



------



## **✅ 解决方法：创建** **'uriel'@'localhost'** **用户**

你需要在 MySQL 中 **显式地创建** 'uriel'@'localhost' 用户，并设置密码。

请按照以下步骤操作：



------



### **1️⃣ 使用** **sudo** **登录 MySQL（你已经可以登录）：**

```sql
sudo mysql
```





------



### **2️⃣ 创建** **'uriel'@'localhost'** **用户并赋权：**

```sql
CREATE USER 'uriel'@'localhost' IDENTIFIED BY '111111';

GRANT ALL PRIVILEGES ON *.* TO 'uriel'@'localhost' WITH GRANT OPTION;

FLUSH PRIVILEGES;
```





------



### **3️⃣ 退出 MySQL：**

```sql
EXIT;
```





------



### **4️⃣ 测试登录是否成功：**

```sql
mysql -u uriel -p
```



输入你刚设置的密码：111111
