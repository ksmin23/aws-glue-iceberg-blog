# aws-glue-iceberg-blog

- [Apache Iceberg Connector for AWS Glue를 이용하여 데이터레이크 CRUD 하기](https://aws.amazon.com/ko/blogs/tech/transactional-datalake-using-apache-iceberg-connector-for-aws-glue/) 포스팅 내용 실습 프로젝트
- Postgresql 대신 MySQL을 사용함
- sample-data 디렉터리에 MySQL에서 S3로 덤프한 parquet 파일이 있음 -> S3에 데이터를 업로드해서 Glue Job 테스트 할 때 사용하면 됨

## MySQL 테이블

- Full-Load

<pre>
CREATE TABLE human_resources.employee_details (emp_no BIGINT(20) AUTO_INCREMENT, name varchar(30), department varchar(30), city varchar(50), salary int, m_time DATETIME DEFAULT CURRENT_TIMESTAMP, PRIMARY KEY(emp_no) ) ENGINE=InnoDB AUTO_INCREMENT=0;

INSERT INTO human_resources.employee_details (emp_no, name, department, city, salary) VALUES (1, 'Adam', 'IT', 'SFO', 50000);
INSERT INTO human_resources.employee_details (emp_no, name, department, city, salary) VALUES (2, 'Susan', 'Sales', 'NY', 60000);
INSERT INTO human_resources.employee_details (emp_no, name, department, city, salary) VALUES (3, 'Jeff', 'Finance', 'Tokyo', 55000);
INSERT INTO human_resources.employee_details (emp_no, name, department, city, salary) VALUES (4, 'Bill', 'Manufacturing', 'New Delhi', 70000);
INSERT INTO human_resources.employee_details (emp_no, name, department, city, salary) VALUES (5, 'Joe', 'IT', 'Chicago', 45000);
INSERT INTO human_resources.employee_details (emp_no, name, department, city, salary) VALUES (6, 'Steve', 'Finance', 'NY', 60000);
</pre>

- CDC-Load

<pre>
UPDATE human_resources.employee_details SET city='New Delhi' WHERE emp_no = 2;
UPDATE human_resources.employee_details SET salary=70000 WHERE emp_no = 5;
INSERT INTO human_resources.employee_details (emp_no, name, department, city, salary) VALUES (8, 'John', 'Sales', 'SFO', 90000);
INSERT INTO human_resources.employee_details (emp_no, name, department, city, salary) VALUES (9, 'Eli', 'Purchasing', 'Chicago', 90000);
DELETE FROM human_resources.employee_details WHERE emp_no = 3;
</pre>

## Sample data

- full-load: 처음 Glue Job을 이용해서 Apache Iceberg 테이블을 생성할 때 사용하는 데이터
- cdc-load: RDS에서 Insert/Update/Delete를 수행한 데이터로 full-load로 만든 Apache Iceberg 테이블을 업데이트 하는데 사용하는 데이터

