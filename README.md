# さくらのVPS(CentOS8)　初期セットアップPlaybook

## 概要

[さくらのVPS](https://vps.sakura.ad.jp/)のCentOS8にて初期セットアップをするためのAnsible Playbookを作成しました。

以下の設定に対応しています。

1. ホスト名設定
1. ユーザ作成
1. SSH設定
1. ネットワーク設定
1. ファイアウォール設定

## 設定方法

### group_vars/servers.yml

接続情報を記載します。  
パスワードを使用する場合はansible_ssh_passを、ポート番号を指定する場合は、ansible_ssh_portを使用してください。
```yaml
ansible_ssh_user: root
ansible_ssh_private_key_file: /path/to/id_rsa
```

作成するユーザリストを記載します。
```yaml
user_list:
- name: user01
  password: user01_P@ssw0rd
  groups: [wheel]
  pub_key: /path/to/id_rsa.pub
```
### host_vars/xxx.xxx.xxx.xxx.yml

ホスト名を設定する場合は、hostnameを記載します。  
(未定義の場合はスキップします。)
```yaml
hostname: server1.example.com
```

※ その他、ホストごとに固有の設定がある場合は、group_varsではなく、host_varsに記載してください。

### hosts

設定するサーバーのリストを記載します。  
IPアドレスまたは名前解決可能なホスト名を指定します。
```yaml
[servers]
# setup server list
11.22.33.44
22.33.44.55
```

## 使い方

設定が終わった後、以下のコマンドを実行します。

```bash
$ ansible-playbook site.yml
```
## ライセンス

[MIT License](https://opensource.org/licenses/mit-license.php)