####Bước 1 — Tạo MySQL Database và User cho WordPress

Login vào mysql với lệnh:
>**mysql -u root -p**

![](http://imgur.com/MDNffG8.png)

Điền password vào để đăng nhập vào mysql. Sau đó đánh dòng lệnh sau để tạo cơ sở dữ liệu cho Wordpress

>**CREATE DATABASE wordpress;**

Tạo user để quản lí riêng cơ sở dữ liệu này ( để cho an toàn, bạn không nên dùng cùng user của CSDL khác như Koha, đặc biệt không nên dùng root user):

>**CREATE USER wordpressuser@localhost IDENTIFIED BY 'password';**

Thay wordpressuser và password theo ý bạn. Tiếp theo sẽ gán quyền quản lí cơ sở dữ liệu "wordpress" cho user này:

>**GRANT ALL PRIVILEGES ON wordpress.* TO wordpressuser@localhost;**

Thêm câu lệnh sau nữa là ok:

>**FLUSH PRIVILEGES;**

Và thoát khỏi mysql, về với Ubuntu:
**exit**


####Bước 2: Tải mã nguồn Wordpress
Nếu bạn dùng Ubuntu desktop, bạn có thể download mã nguồn wordpress về ( đây là lí do bạn không cần biết gì về web, code... mà vẫn có thể tạo ra đc website của riêng mình). Sau đó bạn giải nén file mới tải về, copy toàn bộ vào thư mục /var/www/wordpress

Bạn cùng Ubuntu server, bạn có thể chạy những lệnh sau ( cũng áp dụng cho Ubuntu desktop):

>**cd /var/www/**
<br>**sudo wget http://wordpress.org/latest.tar.gz**

Đánh password và đợi. Chú ý user của bạn phải có quyền của root ( hoặc root user) để được ghi vào thư mục **/var/www**. Tiếp theo giải nén file mới tải về:

>**sudo tar xzvf latest.tar.gz**

Vậy là bạn đã có thư mục wordpress tại địa chỉ: **/var/www/wordpress**. 

####Bước 3: Config Wordpress

Tiến vào thư mục gốc của wordpress

>**cd ~/wordpress**

Tạo ra file wp-config.php dành cho wordpress ( chứa user, password của cơ sở dữ liệu- cái mà bạn tạo ở bước 1):

>**sudo cp wp-config-sample.php wp-config.php**

Mở file mới tạo ra bằng lệnh vi:

>**sudo vi wp-config.php**

Copy đoạn code sau và paste vào file đó. Lệnh paste file bạn dùng **Ctrl+Shift+V** ( nếu copy từ terminal thì dùng **Ctrl+Shift+C**)

>// ** MySQL settings - You can get this info from your web host ** //
><br>/** The name of the database for WordPress */
><br>define('DB_NAME', 'wordpress');

>/** MySQL database username */
><br>define('DB_USER', 'wordpressuser');

>/** MySQL database password */
><br>define('DB_PASSWORD', 'password');

**Chú ý** thay user, pass thành của bạn ( lần đầu sử dụng, bạn có thể làm theo mặc định là hay nhất). Sau đó save và thoát. 


####Bước 4: Phân quyền, tạo thư mục chứa ảnh, tài liệu upload

Dùng lệnh sau để đảm bảo bạn có quyền truy cập mọi thứ trong thư mục từ internet qua giao thức web:

>**sudo chown -R www-data:www-data**  
( chú ý, bạn vẫn đang ở đường dẫn /var/www/wordpress). 

Hoặc nếu bạn đang ở nơi khác thì bạn có thể dùng lệnh sau:
><br>**sudo chown -R www-data:www-data /var/www/wordpress**

Tạo thư mục uploads để lưu trữ ( những thứ bạn sẽ tải lên website)

>**sudo mkdir /var/www/wordpress/wp-content/uploads**

Dùng câu lệnh sau để cho phép trình duyệt tải file lên thư mục này:

sudo chown -R :www-data /var/www/wordpress/wp-content/uploads


####Bước 5: Cài đặt Wordpress

http://server_domain_name_or_IP/wordpress

Theo mặc định, nếu bạn vào địa chỉ ip hoặc domain máy, bạn sẽ đến trang mặc định của apache2. Trên Ubuntu desktop, bạn chỉ việc vào http://localhost/wordpress là xong:

[​IMG]

Điền các thông số cần thiết nhé:

[​IMG]

Vậy là xong, bạn sẽ được đăng nhập vào giao diện admin của Wordpress tại địa chỉ:
http://localhost/wordpress/wp-admin ( Ubuntu desktop)
http://server_domain_name_or_IP/wordpress/wp-admin ( Ubuntu server- truy cập từ máy khác)

[​IMG]

Xem nào. Đầu tiên bạn có thể thử tạo 1 bài viết trong mục POSTS. Sau đó ra trang chủ wordpress xem thử nhé.

[​IMG]


Nếu bạn muốn tạo tên miền riêng cho wordpress, bạn làm như sau:

sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/wordpress.conf

Sau đó bạn edit file mới tạo ra:
sudo nano /etc/apache2/sites-available/wordpress.conf

Trong file này, chú ý dòng sau:

<VirtualHost *:80>
ServerAdmin webmaster@localhost
DocumentRoot /var/www/wordpress
ServerName dreamlib.vn ( thay bằng tên miền của bạn- đã trỏ đến ip máy chủ này)
<Directory /var/www/wordpress/>
AllowOverride All
</Directory>
. . .
Sau đó save file này lại, tiếp theo đánh các dòng lệnh sau:

sudo a2enmod rewrite
sudo a2ensite wordpress.conf
sudo service apache2 restart

Vậy giờ bạn có thể truy cập vào wordpress bằng tên miền, thay vì :
http://localhost/wordpress/wp-admin ( Ubuntu desktop)
http://server_domain_name_or_IP/wordpress/wp-admin ( Ubuntu server- truy cập từ máy khác)

####Conclusion
Xong rồi, chúc các bạn cài đặt Wordpress nhanh chóng, thành công và trải nghiệm thôi.
