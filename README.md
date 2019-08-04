windows_desktop
===============

Windowsのデスクトップ環境をセットアップする。
自分で使うための個人用設定。

前提条件
--------

- 64 ビット オペレーティングシステム、x64 ベース プロセッサ
- Ansibleを実行するコントロールマシンで最新の`ubuntu_desktop`プレイブッ
  クを適用していること。

事前作業
--------

### 1. 管理者権限でPowerShellを開始

管理対象のMS-Windowsマシンで、管理者権限でWindows PowerShellを開始する。

```powershell
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

新しいクロスプラットフォームの PowerShell をお試しください https://aka.ms/pscore6

PS C:\WINDOWS\system32>
```

### 2. PowerShellスクリプトの実行の有効化

管理対象のMS-Windowsマシンで次のPowerShellコマンドを実行する。

```powershell
PS C:\WINDOWS\system32> Get-ExecutionPolicy
Restricted
PS C:\WINDOWS\system32> Set-ExecutionPolicy RemoteSigned

実行ポリシーの変更
実行ポリシーは、信頼されていないスクリプトからの保護に役立ちます。実行ポリシーを変更すると、about_Execution_Policies
のヘルプ トピック (https://go.microsoft.com/fwlink/?LinkID=135170)
で説明されているセキュリティ上の危険にさらされる可能性があります。実行ポリシーを変更しますか?
[Y] はい(Y)  [A] すべて続行(A)  [N] いいえ(N)  [L] すべて無視(L)  [S] 中断(S)  [?] ヘルプ (既定値は "N"): Y
PS C:\WINDOWS\system32> Get-ExecutionPolicy
RemoteSigned
PS C:\WINDOWS\system32>

```

### 3. WinRMの有効化

管理対象のMS-Windowsマシンで次のPowerShellコマンドを実行する。

```powershell
PS C:\Users\toki\Documents\tmp> Invoke-WebRequest -Uri https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1 -OutFile ConfigureRemotingForAnsible.ps1
PS C:\Users\toki\Documents\tmp> .\ConfigureRemotingForAnsible.ps1
Self-signed SSL certificate generated; thumbprint: DBE16617DB096A92EF18FE220AEBCFA86A1DC833


wxf                 : http://schemas.xmlsoap.org/ws/2004/09/transfer
a                   : http://schemas.xmlsoap.org/ws/2004/08/addressing
w                   : http://schemas.dmtf.org/wbem/wsman/1/wsman.xsd
lang                : ja-JP
Address             : http://schemas.xmlsoap.org/ws/2004/08/addressing/role/anonymous
ReferenceParameters : ReferenceParameters

OK



PS C:\Users\toki\Documents\tmp>
```

### 4. 疎通確認

Ansibleを実行するコントロールマシンで次のコマンドを実行する。

```sh
$ rake ping
ansible -ki inventory/hosts windows -m win_ping
SSH password:
win_local | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

構築作業
--------

### 1. プレイブックの実行

Ansibleを実行するコントロールマシンで次のコマンドを実行する。

```sh
$ rake run
ansible-playbook -ki inventory/hosts site.yml
SSH password:

PLAY [windows] ***************************************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************
ok: [win_local]

...省略...

PLAY RECAP *******************************************************************************************************************************************
win_local                  : ok=28   changed=2    unreachable=0    failed=0
```

### 2. 手動インストールの実施

管理対象のMS-Windowsマシンで`Documents\Installers`フォルダー
の各種インストーラを実行する。

### 3. 仕上げのプレイブックの実行

Ansibleを実行するコントロールマシンで次のコマンドを実行する。

```sh
$ rake fin
ansible-playbook -ki inventory/hosts final.yml
SSH password:

PLAY [windows] ***************************************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************
ok: [win_local]

...省略...

PLAY RECAP *******************************************************************************************************************************************
win_local                  : ok=2    changed=0    unreachable=0    failed=0
```

事後作業
--------

### 1. 高DPI環境のスケーリング無効化

管理対象のMS-Windowsマシンで次の設定を実施する。

#### 設定対象ファイル

- `C:\Program Files\VcXsrv\xlaunch.exe`
- `C:\Program Files (x86)\teraterm\ttermpro.exe`

#### プロパティの設定

- 互換性
    - 高DPI設定の変更
        - 高DPIスケール設定の上書き
            - 高いDPIスケールの動作を上書きします。: 有効にする
                - 拡大縮小の実行元
                    - アプリケーション : 選択する

### 2. TranspWndsの設定

管理対象のMS-Windowsマシンで`Documents\TranspWnds\TranspWnds.exe`を起
動し、次の設定を実施する。

- 通知領域
    - TrasnpWnds
        - 右クリック
            - Options
                - System
                    - AutoRun
                        - Run on Windows startup : 有効にする

### 3. 壁紙の設定

管理対象のMS-Windowsマシンで`Documents\Wallpapers`の画像から好みの壁紙
を設定する。壁紙の自動設定は難しかった。
