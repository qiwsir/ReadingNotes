# 数据准备类

## Clean Data

- 作者：Megan Squire

- 出版：Packt Publishing, 2015

### Preface

- Garbage in, garbage out.   - The United States iNTERNAL rEVENUE sERVICE

  **Note**

  对于此名言，维基百科有不同的解释说明。https://en.wikipedia.org/wiki/Garbage_in,_garbage_out

- All of thes helpful algorithms still require hight-quality data in order to learn properly in the first place, and we lament "there are no clean datasets".

### chapter 1: Why Do You Need Clean Data?

### page 2

- We recently read that The New York Times called data cleaning janitor work and said that 80 percent of a data scientist's time will be spendt doing this kind of cleaning.

### page 6

- Enron Data: http://www.ahschulz.de/enron-email-data/

  The tables/dataframes contain the following coloumns:

  **Employeelist**

  - *eid:* Employee-ID
  - *firstName:* First name
  - *lastName:* Last name
  - *Email_id:* Email address (primary). This one can be found in the other tables/dataframes and is useful for matching.
  - *Email2:* Additional email address that was replace by the primary one.
  - *Email3:* See above
  - *Email4:* See above
  - *folder:* The user’s folder in the original data dump.
  - *status:* Last position of the employee. “N/A” are unknown.

  **Message**

  - *mid:* Message-ID. Refers to the rows in recipientinfo and referenceinfo.
  - *sender:* Email address (updated)
  - *date:* Date.
  - *message_id:* Internal message-ID from the mailserver.
  - *subject:* Email subject
  - *body:* Email body. Can be truncated in the R-Version!
  - *folder:* Exact folder of the e-mail inclusing subfolders.

  **Recipientinfo**
   (Note: If an email is sent to multiple recipients, there is a new row for every recipient!)

  - *rid:* Reference-ID
  - *mid:* Message-ID from the message-table/-dataframe
  - *rtype:* Shows if the reciever got the email normally (“to”), as a carbon copy (“cc”) or a blind carbon copy (“bcc”).
  - *rvalue:* The recipient’s email address.

  **Referenceinfo**

  - *rfid:* referenceinfo-ID
  - *mid:* Message-ID
  - *reference:* Contains the whole email with shortend headers.

**Note**

下载数据集，并解压缩到本地之后，导入到MySQL数据库：

1. 在本地创建数据库：`mysql> create enron`

2. 将sql文件导入数据库：`mysql> source /Users/qiwsir/Documents/Codes/DataSet/Enron/enron-mysqldump_v5.sql;`

3. 使用sql语句查看

   ```
   mysql> show tables;
   +-----------------+
   | Tables_in_enron |
   +-----------------+
   | employeelist    |
   | message         |
   | recipientinfo   |
   | referenceinfo   |
   +-----------------+
   4 rows in set (0.01 sec)
   
   mysql> select count(1) from message;    //查看表中记录数量
   +----------+
   | count(1) |
   +----------+
   |   252759 |
   +----------+
   1 row in set (0.01 sec)
   ```

4. 应用举例：

   1. 统计message表中每天的记录数量

      ```
      mysql> desc message;
      +------------+--------------+------+-----+---------+-------+
      | Field      | Type         | Null | Key | Default | Extra |
      +------------+--------------+------+-----+---------+-------+
      | mid        | int(10)      | NO   | PRI | 0       |       |
      | sender     | varchar(127) | NO   | MUL |         |       |
      | date       | datetime     | YES  |     | NULL    |       |
      | message_id | varchar(127) | YES  |     | NULL    |       |
      | subject    | text         | YES  |     | NULL    |       |
      | body       | text         | YES  |     | NULL    |       |
      | folder     | varchar(127) | NO   |     |         |       |
      +------------+--------------+------+-----+---------+-------+
      7 rows in set (0.00 sec)
      
      // 将每天的记录进行统计，最终得到分类统计结果
      mysql> select date(date) as dateSent, count(mid) as numMsg from message GROUP BY dateSent ORDER BY dateSent;
      ```

   2. 分组统计每天的数据记录

      详见./book_clean_data/chapter1.ipynb

      （此数据集可以用来删除异常值或者缺失值，并绘制统计之后的数据分布）

      因为此公司2002年以后不存在了。所以可以用下面的方式删选统计。

      ```
      SELECT date(date) AS dateSent, count(mid) AS numMsg
      From message
      WHERE year(date) BETWEEN 1998 AND 2002
      GROUP BY dateSent ORDER BY datesent;
      ```

### chapter 2: Fundanmentals - Formats, Types, and Encodings

### page 37

【承接上一章的Enron数据，利用SQL语句进行数据格式转换】

### page 46

【区分：NULL、空（empty）、空格（blank）、零在数据中的状态和意义】

