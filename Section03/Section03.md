# Section 3 Ansibleによる自動化とテスト  
## 3-0 Ansibleのインストール  
Ubuntuにapt-getを使ってインストール  

```markdown
$ sudo apt-get install software-properties-common  
$ sudo apt-add-repository ppa:ansible/ansible  
$ sudo apt-get update  
$ sudo apt-get install ansible
```  

## 3-1 AnsibleでWordpressを動かす   
echoを使ってhostsファイルを作る  

$ echo 自分のサーバーのIPアドレス > hosts  

pingで確認する

	$ ansible 自分のIPアドレス -i hosts -m ping --private-key ~/.vagrant.d/insecure_private_key -u vagrant -k

playbook.ymlに設定を書いていく  

playbook.ymlと同じ場所にnginx,php-fpm,wordpressディレクトリを作成する  
	
	$ mkdir nginx php-fpm wordpress  

それぞれのディレクトリにWordpressの設定ファイルを作って記述していく  

Syntaxを確認  

	$ ansible-playbook -i hosts playbook.yml --syntax-check

コマンドでplaybookを実行  

	$ ansible-playbook playbook.yml -i hosts  --private-key ~/.vagrant.d/insecure_private_key -u vagrant -k

playbook実行中にどこがエラーなのか見たい場合は  

	 $ ansible-playbook playbook.yml -i hosts  --private-key ~/.vagrant.d/insecure_private_key -u vagrant -k -vv   

を実行すればいいんじゃないかな  

