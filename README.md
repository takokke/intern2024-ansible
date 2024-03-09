# 導入手順
## brew経由でansibleをインストール
```
$ brew install ansible
```
インストールできたか確認します
```
$ ansible --version
    出力例=>
    ansible [core 2.12.7]
```

## 仮想マシンを建てる
```
$ multipass launch --name 名前
```
建てるまで少し時間がかかります

## 建てた仮想マシンにローカルの公開鍵を登録
### ローカルの公開鍵をコピー
以下のコマンドをローカルで実行してください。

・公開鍵ファイルの中身をターミナルに出力
```
$ cat ~/.ssh/id_ed25519.pub
```
`ssh-ed25519`で始まる文字列をコピーしてください。

### ローカルの公開鍵を仮想マシンに渡す
以下のコマンドは仮想マシン上で実行してください。

・ファイルを開きます
```
$ vim ~/.ssh/authorized_keys
```
先ほどコピーした公開鍵の中身をペーストして登録完了です。

## Ansibleの設定を仮想マシンに流し込む
playbook.ymlを以下の通りに編集します
```
[local]
192.168.xx.xx-local ansible_host=192.168.xx.xx local_ipv4=127.0.0.1 global_ipv4=192.168.xx.xx

[local:vars]
ansible_ssh_host=192.168.xx.xx // 自分の環境のプライベートipアドレス
ansible_ssh_port=22
ansible_ssh_user=ubuntu
ansible_ssh_private_key_file=｛private_keyのパス｝
```
これで準備完了です。以下のコマンドを実行してansibleを実行しましょう
```
$ ansible-playbook -i hosts playbook.yml
```