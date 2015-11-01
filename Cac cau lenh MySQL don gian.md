##Các câu lệnh MySQL

Đăng nhập MySQL dùng lệnh: mysql -u root -p
####1. Thư mục chứa database

Toàn bộ file raw database được lưu trong thư mục `/var/lib/mysql`

####2. Quản lý tài khoản và phân quyền

Hiển thị toàn bộ users:
<br>`mysql> SELECT * FROM mysql.user;`

Xóa null user:
<br>`mysql> DELETE FROM mysql.user WHERE user = ' ';`

Xóa tất cả user mà không phải root:
<br>`mysql> DELETE FROM mysql.user WHERE NOT (host="localhost" AND user="root");`

Đổi tên tài khoản root (giúp bảo mật):
<br>`mysql> UPDATE mysql.user SET user="mydbadmin" WHERE user="root";`

Gán full quyền cho một user mới:
<br>`mysql> GRANT ALL PRIVILEGES ON *.* TO 'username'@'localhost' IDENTIFIED BY 'mypass' WITH GRANT OPTION;`

Phân quyền chi tiết cho một user mới:
<br>`mysql> GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, INDEX, ALTER, CREATE TEMPORARY TABLES, LOCK TABLES ON mydatabase.* TO 'username'@'localhost' IDENTIFIED BY 'mypass';`

* Trong đó:
    * **all** : tất cả các quyền
    * **create** : cho phép tạo mới table hoặc database
    * **drop** : cho phép xóa table hoặc database
    * **delete** : cho phép xóa dữ liệu trong bản ghi table
    * **insert** : cho phép thêm bản ghi mới vào bảng csdl
    * **select** : cho phép sử dụng lệnh select để tìm kiếm dữ liệu
    * **update** : cho phép cập nhật cơ sở dữ liệu
    * **grant option** : cho phép gán hoặc xóa quyền của người dùng khác.

Gán full quyền cho một user mới trên một database nhất định:
<br>`mysql> GRANT ALL PRIVILEGES ON mydatabase.* TO 'username'@'localhost' IDENTIFIED BY 'mypass' WITH GRANT OPTION;`

Thay đổi mật khẩu user:
<br>`mysql> UPDATE mysql.user SET password=PASSWORD("newpass") WHERE User='username';`

Xóa user:
<br>`mysql> DELETE FROM mysql.user WHERE user="username";`

####3. Các thao tác database

Hiển thị toàn bộ databases:
<br>`mysql> SHOW DATABASES;`

Tạo database:
<br>`mysql> CREATE DATABASE mydatabase;`

Sử dụng một database:
<br>`mysql> USE mydatabase;`

Xóa một database:
<br>`mysql> DROP DATABASE mydatabase;`

Tối ưu database:
<br>All Databases:
<br>`$ sudo mysqlcheck -o --all-databases -u root -p`
Single Database:
<br>`$ sudo mysqlcheck -o db_schema_name -u root -p`

####4. Các thao tác table

Tất cả các thao tác bên dưới bạn phải lựa chọn trước database bằng cách dùng lệnh: mysql> USE mydatabase;

Hiển thị toàn bộ table:
<br>`mysql> SHOW TABLES;`

Hiển thị dữ liệu của table:
<br>`mysql> SELECT * FROM tablename;`

Đổi tên table :
<br>`mysql> RENAME TABLE first TO second;`
hoặc
<br>`mysql> ALTER TABLE mytable rename as mynewtable;`
Xóa table:
<br>`mysql> DROP TABLE mytable;`

####5. Các thao tác cột và hàng

Tất cả các thao tác bên dưới bạn phải lựa chọn trước database bằng cách dùng lệnh: `mysql> USE mydatabase;`

Hiển thị các column trong table:
<br>`mysql> DESC mytable;`
hoặc
<br>`mysql> SHOW COLUMNS FROM mytable;`

Đổi tên column:
<br>`mysql> UPDATE mytable SET mycolumn="newinfo" WHERE mycolumn="oldinfo";`

Select dữ liệu:
<br>`mysql> SELECT * FROM mytable WHERE mycolumn='mydata' ORDER BY mycolumn2;`

Insert dữ liệu vào table:
<br>`mysql> INSERT INTO mytable VALUES('column1data','column2data','column3data','column4data','column5data','column6data','column7data','column8data','column9data');`

Xóa dữ liệu trong table:
<br>`mysql> DELETE FROM mytable WHERE mycolumn="mydata";`

Cập nhật dữ liệu trong table:
<br>`mysql> UPDATE mytable SET column1="mydata" WHERE column2="mydata";`

####6. Các thao tác sao lưu và phục hồi

Sao lưu toàn bộ database bằng lệnh (chú ý không có khoảng trắng giữa -p và mật khẩu):
<br>`mysqldump -u root -pmypass --all-databases > alldatabases.sql`

Sao lưu một database bất kỳ:
<br>`mysqldump -u username -pmypass databasename > database.sql`

Khôi phục toàn bộ database bằng lệnh:
<br>`mysql -u username -pmypass < alldatabases.sql (no space in between -p and mypass)`

Khôi phục một database bất kỳ:
<br>`mysql -u username -pmypass databasename < database.sql`

Chỉ sao lưu cấu trúc database:
<br>`mysqldump --no-data --databases databasename > structurebackup.sql`

Chỉ sao lưu cấu trúc nhiều database:
<br>`mysqldump --no-data --databases databasename1 databasename2 databasename3 > structurebackup.sql`

Sao lưu một số table nhất định:
<br>`mysqldump --add-drop-table -u username -pmypass databasename table_1 table_2 > databasebackup.sql`

