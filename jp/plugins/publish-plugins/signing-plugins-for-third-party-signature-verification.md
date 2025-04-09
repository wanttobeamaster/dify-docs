# 第三者署名検証のためにプラグインに署名する

{% hint style="warning" %}
この機能はコミュニティ版でのみ利用できます。クラウドサービスでは、現時点では第三者署名検証はサポートされていません。
{% endhint %}

第三者署名検証により、Dify の管理者は、Dify Marketplace にリストされていないプラグインのインストールを、署名の検証を完全に無効化することなく、安全に許可できるようになります。これにより、例えば次のようなシナリオがサポートされます。

* Dify の管理者は、開発者から送信されたプラグインを承認したら、プラグインに署名を追加して公開できます
* プラグインの開発者は、署名の検証の無効化が許容されない Dify の管理者のために、プラグインに署名を追加し、公開鍵とともに公開できます

Dify の管理者やプラグインの開発者は、事前に準備した秘密鍵でプラグインに署名を追加できます。また、Dify の管理者は、プラグインのインストール時に、特定の公開鍵による署名の検証を強制するように Dify を構成できます。

## 署名の追加と検証に利用する鍵ペアの生成

次のコマンドで、プラグインの署名の追加と検証に使用する新しい鍵ペアを生成します：

```bash
dify signature generate -f your_key_pair
```

コマンド実行後、現在のパスに次の 2 つのファイルが生成されます：

* **秘密鍵**： `your_key_pair.private.pem`
* **公開鍵**： `your_key_pair.public.pem`

秘密鍵は、プラグインに署名を追加するために使用され、公開鍵はプラグインの署名を検証するために使用されます。

{% hint style="warning" %}
秘密鍵は充分に秘匿された状態で安全に保管してください。秘密鍵が漏洩すると、攻撃者が任意のプラグインに正規の署名を追加できるようになり、Dify の安全性が脅かされます。
{% endhint %}

## プラグインへの署名の追加と確認

次のコマンドで、プラグインに署名を追加します。引数で **署名したいプラグインファイル** と **秘密鍵** を指定していることに注意してください：

```bash
dify signature sign your_plugin_project.difypkg -p your_key_pair.private.pem
```

コマンド実行後、元のファイルと同じパスに、ファイル名に `signed` が追加された新しいプラグインファイルが生成されます： `your_plugin_project.signed.difypkg`

プラグインファイルが正しく署名されていることは、次のコマンドで確認できます。このコマンドでは、引数で **署名済みのプラグインファイル** と **公開鍵** を指定していることに注意してください：

```bash
dify signature verify your_plugin_project.signed.difypkg -p your_key_pair.public.pem
```

{% hint style="info" %}
公開鍵の指定を省略すると、Dify Marketplace 用の公開鍵での検証が行われます。この場合、Dify Marketplace からダウンロードしたプラグインファイル以外は、署名の検証に失敗します。
{% endhint %}

## 第三者署名検証の有効化

Dify の管理者は、プラグインのインストール前に、事前に許可した公開鍵よる署名の検証を強制できます。

### 公開鍵の配置

プラグインの署名に利用した秘密鍵に対応する **公開鍵** を、プラグインデーモンが参照できる場所に配置します。

例えば、`docker/volumes/plugin_daemon` の下に `public_keys` ディレクトリを作成し、公開鍵ファイルを配置します。

```bash
mkdir docker/volumes/plugin_daemon/public_keys
cp your_key_pair.public.pem docker/volumes/plugin_daemon/public_keys
```

### 環境変数の設定

コンテナ `plugin_daemon` で、次の環境変数を構成します。

* `THIRD_PARTY_SIGNATURE_VERIFICATION_ENABLED`
  * 第三者署名検証の有効化を行う環境変数です
  * `true` に設定すると、第三者署名検証が有効化されます
* `THIRD_PARTY_SIGNATURE_VERIFICATION_PUBLIC_KEYS`
  * 第三者署名検証に使用する公開鍵ファイルのパスを指定する環境変数です
  * カンマ区切りで複数の公開鍵ファイルを指定できます

これらを構成するための Docker Compose 用のオーバーライドファイル（`docker-compose.override.yaml`）の例を以下に示します：

```yaml
services:
  plugin_daemon:
    environment:
      FORCE_VERIFYING_SIGNATURE: true
      THIRD_PARTY_SIGNATURE_VERIFICATION_ENABLED: true
      THIRD_PARTY_SIGNATURE_VERIFICATION_PUBLIC_KEYS: /app/storage/public_keys/your_key_pair.public.pem
```

{% hint style="info" %}
`docker/volumes/plugin_daemon` は、コンテナ `plugin_daemon` の `/app/storage`にマウントされています。環境変数 `THIRD_PARTY_SIGNATURE_VERIFICATION_PUBLIC_KEYS` で指定するパスは、コンテナ内のパスであることに注意してください。
{% endhint %}

設定を反映させるため、最後に、Dify サービスを再起動します。

```bash
cd docker
docker compose down
docker compose up -d
```
