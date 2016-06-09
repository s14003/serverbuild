## 6-0 AWSコマンドラインインターフェイスのインストール  

[詳細はこちら](http://docs.aws.amazon.com/ja_jp/cli/latest/userguide/installing.html)  
To install pip on Linuxの2までできてればおけーなはず  

## 6-1 AWS EC2 + Ansible   

	$ scp -r コピーしたいもの諸々 学籍番号@172.16.40.2: 
	$ ssh 学籍番号@172.16.40.1  

scpしてきたplaybook.ymlを書き換える  
hostsも新しいIPアドレスに書き換える  

	ansible-playbook playbook.yml -i hosts  --private-key 学籍番号.pemのパス -u ec2-user -k

あとはエラーログ見ながら権限とかソケットとか直していけばおけー  
パブリックDNSに書かれているのを、http://のあとに付け足してWordpressの画面が出たらおけー  


### AMI(Amazon Machine Image)を作る

EC2の中のAMIにいって作成ボタンを押す。AMI作る  
インスタンスの作成でマイ AMIを選択、さっき作ったAMIを選択  
あとは6-1でインスタンスを作ったのと同じやり方でインスタンスを作る  
パブリックDNSを投げるとさっき作ったWordpressと同じ画面がでたらおけー  

## 6-2 AWS EC2(AMIMOTO)  
1. EC2の選択  
2. インスタンスを作成をクリック  
3. AWS Marketplace マーケットプレイスをクリック  
4. AMIMOTOと入力します  
5. WordPress powered by AMIMOTO (HVM)を選択  
6. インスタンスを選択  
7. “確認して起動” をクリック  
8. 起動をクリック  
9. 自分のキーペアを設定    
10. インスタンスを起動する  
11. 左メニューのエラスティック IP を選択  
12. 新規のアドレスを割り当てるをクリック  
13. EIP とインスタンスの紐付けのため，アドレスの関連付けをクリック  
14. instanceで自分のインスタンスのインスタンスIDを入力  
15. インスタンスに戻ってパブリックDNSをコピーしてhttp://のあとに入れる  
16. 自分のインスタンスIDを入力  
17. Wordpressの画面がでたらおけー  

## 6-4 CloudFront  
1. マイAMIを使ってインスタンスを作る  
2. CloudFrontに行ってCreate Distributionをクリック  
3. WebとRTMPがあるが、Webを選択  
4. Origin Domain Nameに1で作ったインスタンスのパブリックDNSを指定。  
5. Create Distributionを押す  
6. Domain nameをhttp://のあとに追加して実行  

# 6-5 ELB  

1. 普通にインスタンスを作成  
2. 新規イメージの作成からAMIを作る  
3. AMIを使ってインスタンスを2つ作る  
4. ロードバランサー を新規作成→  [詳細](http://qiita.com/hiroshik1985/items/ffda3f2bdb71599783a3#elb%E3%81%AE%E4%BD%9C%E6%88%90)  
5. grusからsshでそれぞれに接続  
6. /var/log/nginx/access.logにアクセス  
7. IPアドレスが分散されているか確認  
8. ロードバランサーに行って、説明の一番上にあるDNSのアドレスに繋いで、最初に作ったワードプレスが表示されたら終了  

