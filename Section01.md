#Section01	
##05/10(火)
###virtualboxのインストール
公式ページに飛んでUbuntu用のものをダウンロード＆インストール

###vagrantのインストール

公式ページに飛んでダウンロード。dpkgで展開  
##CentOS7のインストール  

USBからCentOSをいれる。  
virtualboxのネットワーク設定からホストオンリーアダプターを指定。  
起動後、CentOSインストール。  

###yum・wgetの設定

sudo の権限がなかったのでrootユーザーになってvisudoで権限を追加する  
/etc/yum.conf にproxyの設定を追加  
viで/etc/sysconfig/network-scripts/ifcfg-enp0s3と/etc/sysconfig/network-scripts/ifcfg-enp0s8のONBOOTをyesに書き換える  
/etc/sysconfig/network-script/ifup enp0s3 を実行  
/etc/sysconfig/network-script/ifup enp0s8 を実行  

	$ sudo yum update    

* ip addr で自分のIPアドレスを確認  

###sshで接続  

	ssh s14003@192.168.56.101  

###Wordpressのインストール  

	$ yum -y install httpd mysql mysql-server mysql-devel mysql-utilities php php-mysql wget`  
	$ tar xzfv latest-ja.tar.gz`  

###Wordpress用のデータベースを作成  

``` mysql
	mysql -u root -p  
	create database databasename;
	grant all privileges on databasename.* to "username"@"localhost" identified by "password";
	flush pricileges;
	exit;
```

wp-config.phpにさっき作ったデータベースのデータを入力  
秘密鍵の値を入力  

	/etc/httpd/httpd.conf の中身を修正する  

	192.168.56.101/wp-admin/install.php に接続してWordpressをインストール

おしまい！！！！！！！