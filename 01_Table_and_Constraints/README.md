### 📂 模块 01：数据表构建与完整性约束

> **实训环境**：SQL Server
> **核心技能**：`CREATE TABLE`、`ALTER TABLE`、主键/外键/唯一/检查约束
> **参考资料**：数据库原理与应用教程的3.3：数据表的操作

---

#### 一、 基础建表与默认值

*   **例 3-7**：使用 `DEFAULT` 关键字为性别字段设置默认值“男”。
    CREATE TABLE S ( 
        SNo CHAR(6), 
        SN VARCHAR(10), 
        Sex NCHAR(1) DEFAULT '男', 
        Age INT, 
        Dept NVARCHAR(20)
    );
<img width="528" height="496" alt="图片1" src="https://github.com/user-attachments/assets/4a5f53a1-7715-426d-9999-6d94cfc3c78c" />


#### 二、 数据完整性约束
*   **例 3-8**(NULL / NOT NULL)：规定 SNo 字段不允许为空。

CREATE TABLE S ( 
    SNo VARCHAR(6) CONSTRAINT S_CONST NOT NULL, 
    SN NVARCHAR(10), 
    Sex NCHAR(1), 
    Age INT, 
    Dept NVARCHAR(20)
);
<img width="555" height="528" alt="图片2" src="https://github.com/user-attachments/assets/99f1e602-c795-4ba7-9507-239a4e8c7db4" />

*   **例 3-9** (UNIQUE 列约束)：定义 SN 为唯一键。

CREATE TABLE S ( 
    SNo VARCHAR(6), 
    SN NVARCHAR(10) CONSTRAINT SN_UNIQ UNIQUE, 
    Sex NCHAR(1), 
    Age INT, 
    Dept NVARCHAR(20)
);
<img width="542" height="522" alt="图片3" src="https://github.com/user-attachments/assets/55a024df-23b4-4536-bcc3-b7d69546e8f9" />


*   **例 3-10** (UNIQUE 表约束)：定义 SN + Sex 组合为唯一键。

CREATE TABLE S ( 
    SNo VARCHAR(6), 
    SN NVARCHAR(10) UNIQUE, 
    Sex NCHAR(1), 
    Age INT, 
    Dept NVARCHAR(20), 
    CONSTRAINT S_UNIQ UNIQUE(SN, Sex)
);
<img width="557" height="510" alt="图片4" src="https://github.com/user-attachments/assets/3a378408-2740-4950-a8fa-8ee3b82728ae" />


*   **例 3-11** (PRIMARY KEY 主键)：定义 SNo 为主键。

CREATE TABLE S ( 
    SNo VARCHAR(6) CONSTRAINT S_Prim PRIMARY KEY, 
    SN NVARCHAR(10) UNIQUE, 
    Sex NCHAR(1), 
    Age INT, 
    Dept NVARCHAR(20)
);

CREATE TABLE C ( 
    CNo VARCHAR(6) CONSTRAINT C_Prim PRIMARY KEY, 
    CN NVARCHAR(20), 
    CT INT
);
<img width="692" height="455" alt="图片5" src="https://github.com/user-attachments/assets/229c5757-432c-47cf-9a18-5ef01d0efc18" />


*   **例 3-12**：定义 SNo + CNo 为联合主键。

CREATE TABLE SC ( 
    SNo VARCHAR(6) NOT NULL, 
    CNo VARCHAR(6) NOT NULL, 
    Score NUMERIC(4,1), 
    CONSTRAINT SC_Prim PRIMARY KEY(SNo, CNo)
);<img width="488" height="568" alt="图片6" src="https://github.com/user-attachments/assets/1f5616f6-2e0b-4f37-9310-3c1cdd502ba7" />

*   **例 3-12**(FOREIGN KEY 外键)：定义外部键引用。

CREATE TABLE SC ( 
    SNo VARCHAR(6) NOT NULL CONSTRAINT S_For FOREIGN KEY REFERENCES S(SNo), 
    CNo VARCHAR(6) NOT NULL CONSTRAINT C_For FOREIGN KEY REFERENCES C(CNO), 
    Score NUMERIC(4,1), 
    CONSTRAINT S_C_Prim PRIMARY KEY (SNo, CNO)
);
<img width="587" height="517" alt="图片7" src="https://github.com/user-attachments/assets/cbbf5aaa-3fac-4ee1-b398-39089a599be1" />


*   **例 3-14** (CHECK 约束)：定义 Score 取值范围。

CREATE TABLE SC ( 
    SNo VARCHAR(6), 
    CNo VARCHAR(6), 
    Score NUMERIC(4,1) CONSTRAINT Score_Chk CHECK(Score >= 0 AND Score <= 100)
);
<img width="570" height="517" alt="图片8" src="https://github.com/user-attachments/assets/a989df28-d1d9-43f8-8f66-1264d4dd9232" />


*   **例 3-15** (综合约束)：包含主键、非空、默认值、检查。

CREATE TABLE S ( 
    SNo VARCHAR(6) CONSTRAINT S_Prim PRIMARY KEY, 
    SN NVARCHAR(10) CONSTRAINT SN_Cons NOT NULL, 
    Sex NCHAR(1) CONSTRAINT Sex_Cons NOT NULL DEFAULT '男', 
    Age INT CONSTRAINT Age_Cons NOT NULL CONSTRAINT Age_Chk CHECK (Age BETWEEN 15 AND 50), 
    Dept NVARCHAR(20) CONSTRAINT Dept_Cons NOT NULL
);
<img width="665" height="517" alt="图片9" src="https://github.com/user-attachments/assets/58a0c01b-9a76-4c99-9bcf-22807a5144e5" />

#### 三、 修改数据表 (ALTER TABLE)
*   **例 3-16** (增加新列)：增加 Class_No 和 Address。

ALTER TABLE S 
ADD Class_No VARCHAR(6), Address NVARCHAR(20);
<img width="570" height="503" alt="图片10" src="https://github.com/user-attachments/assets/d2d32bdc-8646-461e-9045-6daec62a1689" />

*   **例 3-17**(增加约束)：增加 Score 检查约束。

ALTER TABLE SC 
ADD CONSTRAINT Score_Chk CHECK(Score BETWEEN 0 AND 100);
<img width="483" height="498" alt="图片11" src="https://github.com/user-attachments/assets/34224d34-de80-45d1-a8c5-21ec7fac2277" />

*   **例 3-18**(修改列)：加宽 SN 列。

ALTER TABLE S 
ALTER COLUMN SN NVARCHAR(12);
<img width="466" height="532" alt="图片12" src="https://github.com/user-attachments/assets/0d0cc825-1041-48bb-9529-72ea530c346a" />

*   **例 3-19**删除s表中的主键
  ALTER TABLE S
  DROP CONSTRAINT S_Prim
<img width="581" height="621" alt="图片13" src="https://github.com/user-attachments/assets/75fcfb75-d65a-48f7-bb5a-fe413da4985b" />

#### 删除数据库
DROP TABLE <表名>


