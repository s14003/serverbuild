

## 6-1 AWS EC2 + Ansible   

	$ scp -r コピーしたいもの諸々 学籍番号@172.16.40.2: 
	$ ssh 学籍番号@172.16.40.1  

scpしてきたplaybook.ymlを書き換える  
hostsも新しいIPアドレスに書き換える  

	ansible-playbook playbook.yml -i hosts  --private-key ~/.vagrant.d/insecure_private_key -u ec2-user -k

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
9. 自分のキーペアを作成  
10. インスタンスを起動する  
11. 左メニューのエラスティック IP を選択  
12. 新規のアドレスを割り当てるをクリック  
13. EIP とインスタンスの紐付けのため，アドレスの関連付けをクリック  
14. instanceで自分のインスタンスのインスタンスIDを入力  
15. インスタンスに戻ってパブリックDNSをコピーしてhttp://のあとに入れる  
16. 自分のインスタンスIDを入力  
17. Wordpressの画面がでたらおけー  
