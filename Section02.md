#Section02
##2-1Vagrantを利用したCentOS7環境の起動  
USBストレージにあるVagrant用のCentOSをコピー  

	$ vagrant box add CentOS7 コピーしたbox ファイル --force  
###Vagrantの初期設定  

作業用ディレクトリを作成し、その中で初期設定を行う  

	$ vagrant init
コマンドを実行するとVagrantfileというファイルが作成される。  
viでVagrantfileを開いて  

	config.vm.box = "base" と書かれているのを  
	config.vm.box = "CentOS7" に変更  
###ホストオンリーアダプタの設定  

Vagrantfileの中身を書き換える  
		
	Vagrant.configure(2) do |config|     
から一番最後の  

	end 
の間に  

	config.vm.network "private_network", ip:"192.168.56.129"  
と書いて仮想マシンのIPアドレスを設定  

###Vagrantfileの設定変更の反映  

	vagrant reload   

###仮想マシンの起動  

	vagrant ssh  

##2-2Wordpressを動かす  
### nginxのインストール

	yum install --enablerepo=epel nginx  
でnginxをインストール  

### phpのインストール 

	sudo yum install --enablerepo=epel,remi-php70 php php-mbstring php-pear php-fpm php-mcrypt php-mysql  

### php-fpmの設定  

	www.conf の中身のユーザ・グループをnginxに変更  
	systemctl start php-fpm でphp-fpmのサービスを起動  
	systemctl enable php-fpm で自動起動の設定  

### MariaDBのインストール 

	yum -y install mariadb mariadb-server  

### MariaDBの起動  

	systemctl start mariadb でサービスを起動   
	systemctl enable mariadb で自動起動設定  

### MariaDBの設定  

	mysql_secure_installation

### MariaDBの再起動  

	systemctl restart mariadb  

### Wordpress用のデータベースを作成  

``` mysql
	mysql -u root -p  
	create database databasename;
	grant all privileges on databasename.* to "username"@"localhost" identified by "password";
	flush pricileges;
	exit;
```

	
### Wordpressのダウンロード  
	wget http://wordpress.org/latest.tar.gz    
でダウンロード開始  

	tar xzfv latest-ja.tar.gz  
で解凍  
所有者とグループをnginxに変更  

	chown -R nginx:nginx wordpress  

wordpressのディレクトリを/var/www/の中に移動  

### default.confの中身を変更  

	vi /etc/nginx/conf.d/default.conf  
rootをwordpressを置いた場所に変更  

	fastcgi_param SCRIPT_FILENAME に $document_root$fastcgi_script_name; を入れる  

	systemctl restart nginx  

でnginxを再起動  

### wp-config.phpの編集  

wordpressの中のwp-config-sample.phpをwp-config.phpに変更して中身を編集  
MariaDBで作った内容を入力、ユニークキーを入れて:wq  

	192.168.56.129/wp-admin/install.php に繋いでインストールが開始されたら終了  

## 2-3Wordpressを動かす  

### 2-3-1 apacheをダウンロード・コンパイル
#### ダウンロード
公式サイトからvarsion2.2のものをwgetを使ってダウンロード  
#### ソースツリーの設定 
デフォルトオプションを使ってソースツリーを全て設定するなら  

	$ ./configre  
#### ビルド 

	$ make
####インストール  

	$ make install   

### 2-3-2 phpをダウンロード・コンパイル  
#### ダウンロード・展開  

	$ wget http://jp2.php.net/get/php-7.0.6.tar.bz2/from/this/mirror  
でphpのソースをダウンロード  

	$ tar -xvf mirror  
で展開  
#### ソースツリーの設定 
デフォルトオプションを使ってソースツリーを全て設定するなら 

	$ ./configure --with-apxs2=/usr/local/apache2/bin/apxs --with-mysqli  

#### ビルド  

	$ make  

#### インストール  

	$ make install      
#### Apacheのサービス起動  

	$ /usr/local/apache2/bin/apachectl start  
#### Apacheのサービス停止  

	$ /usr/local/apache2/bin/apachectl stop  
#### Apacheのサービス再起動  

	$ /usr/local/apache2/bin/apachectl restart  
### 2-3-3 MySQLのインストール  

	$ wget https://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm で、公式リポジトリをインストール  
	$ tar mysql57-community-release-el7-8.noarch.rpm で、展開。  
	$ sudo yum -y install mysql mysql-devel mysql-server  でmysqlをインストール  


### Wordpress用のデータベース作成  

``` mysql
	mysql -u root -p  
	create database databasename;
	grant all privileges on databasename.* to "username"@"localhost" identified by "password";
	flush pricileges;
	exit;
```

### 2-3-4 Wordpressのダウンロード・インストール  
#### ダウンロード  
	
	$ wget http://wordpress.org/latest.tar.gz  
#### 展開  
	$ tar -xvf latest.tar.gz`  

####wp-configの設定  
mysqlで作ったデータをこっちに書き込む  

### 2-3-5httpd.confの設定  
DocumentRootをWordpressがある位置に変更する  
PHPが動いてないっぽかったらFilesMatchを確認する  


	$ 192.168.56.129/wp-admin/install.php に繋いでインストールが開始されたら終了  
