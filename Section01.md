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
/etc/wgetrc にもproxyの設定を追加(80行目あたりにあるよ)  

###ネットワークアダプター1/2へのIPアドレスの設定とssh接続の確認  

viで/etc/sysconfig/network-scripts/ifcfg-enp0s3と  
/etc/sysconfig/network-scripts/ifcfg-enp0s8のONBOOTをyesに書き換える  

	$ ./etc/sysconfig/network-script/ifup enp0s3   
	$ ./etc/sysconfig/network-script/ifup enp0s8   

	$ sudo yum update    

* `$ ip a` で自分のIPアドレスを確認  

###sshで接続  

	ssh s14003@192.168.56.101  

### wget,httpd,phpのインストール  

	$ yum -y install httpd mysql mysql-server mysql-devel mysql-utilities php php-mysql wget  

###Wordpressのインストール  

	$ wget http://wordpress.org/latest.tar.gz  
	$ tar xzfv latest-ja.tar.gz`  

###Wordpressの移動  

	$sudo cp -r /var/www  

###Wordpress用のデータベースを作成  

``` mysql
	mysql -u root -p  
	create database databasename;
	grant all privileges on databasename.* to "username"@"localhost" identified by "password";
	flush pricileges;
	exit;
```

wp-config.phpにうえで作ったデータベースのデータを入力  
[秘密鍵の値](https://api.wordpress.org/secret-key/1.1/salt/)を入力  

`/etc/httpd/httpd.conf` の中身を修正する  

	DocumentRoot と htmlになっているところをwordpressに変更  

`http://192.168.56.101/wp-admin/install.php` に接続してWordpressをインストール

おしまい！！！！！！！