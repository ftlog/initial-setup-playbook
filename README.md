# さくらのVPS(CentOS8)　初期セットアップPlaybook

## 概要

[さくらのVPS](https://vps.sakura.ad.jp/)のCentOS8にて初期セットアップをするためのAnsible Playbookを作成しました。

以下の設定に対応しています。

1. ホスト名設定
1. ユーザ作成
1. SSH設定
1. ネットワーク設定
1. ファイアウォール設定
1. 日本語パックインストール
1. システムアップデート

※SSH設定でrootユーザのリモートログインを禁止しています。  
　ユーザ作成で作成したユーザでのみ接続、sudoができるようになります。  
　システムアップデート後は、必要に応じて再起動を実施してください。

## 動作確認環境

<コントロールノード>  
ansible 2.10.2  
Python 3.7.3  
$ ansible-galaxy collection install ansible.posix  

<ターゲットノード>  
さくらのVPS 標準OS CentOS8 x86_64  

## 設定方法

### group_vars/servers.yml

接続情報を記載します。  
パスワードを使用する場合はansible_ssh_passを、ポート番号を指定する場合は、ansible_ssh_portを使用してください。  
rootユーザ以外の場合、becomeされるため、ansible_become_passを指定してください。
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
ネットワーク設定を行う場合は、network_listを記載します。

```yaml
network_list:
- conn_name: System ens4
  ifname: ens4
  ip4: 192.168.0.10/24
  autoconnect: yes
```
ファイアウォールの設定を行う場合は、port_list、service_listを記載します。  
使用しない場合は、コメントアウトしてください。

```yaml
# port_list:

service_list:
- ssh
```

※ ホストごとに固有の設定がある場合は、group_varsではなく、host_varsに記載してください。

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

git cloneをします。

```
$ git clone https://github.com/ftlog/sakuravps-initial-setup-centos8
$ cd sakuravps-initial-setup-centos8
```

設定が終わった後、以下のコマンドを実行します。

```
$ ansible-playbook site.yml
```
## ライセンス

[MIT License](https://opensource.org/licenses/mit-license.php)
