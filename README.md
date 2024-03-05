# OpenSSLを利用して単一ファイルを公開鍵暗号化する

## アルゴリズム

- RSA 4096bit

実際にはデータをAESで暗号化し、AESのキーをRSAで暗号化する

## 使い方

- ビルドする

```shell
docker compose build
```

- 鍵を作る

```shell
docker compose run app keygen
ls my-keys
```

- 暗号化する
  - 暗号化する場合は送信する先に送信相手の公開鍵を入手しておく
  - 公開鍵は `pub-keys/` 配下に格納しておく
    - ここでは `pub-keys/foobar.pem` とする
  - 暗号化するファイルは `plain/` 配下に格納しておく
    - ここでは `plain/sample.zip` とする

```shell
docker compose run app encrypt foobar.pem sample.zip
ls cipher/
```

- 復号する
  - まず暗号化データを取得する
  - データを暗号化してもらうために公開鍵を通知しておく

  ```shell
  cat my-keys/public_key.pem
  ```

  - 暗号化データは `cipher/` 配下に格納しておく
    - ここでは `cipher/sample.zip.bin` とする

```shell
docker compose run app decrypt sample.zip.bin
ls plain/
```
