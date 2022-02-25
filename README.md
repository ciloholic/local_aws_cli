# AWS Vault + AWS CLI for Docker

# 事前インストール

- direnv
- docker
- aws-vault

# 独自関数を設定

```
$ vi ~/.zshrc
laws() {
    aws-vault exec ${AWS_PROFILE:-ap-northeast-1} -- env | \
    read envs <<< $(awk -v 'ORS= ' '/AWS_(REGION|ACCESS_KEY_ID|SECRET_ACCESS_KEY|SESSION_TOKEN)/ {print "-e " $0}'); \
    zsh -c "docker run --rm -it $envs amazon/aws-cli $*"
}
```

# 利用するAWSプロファイルを環境変数に設定

```
$ direnv edit .
export AWS_PROFILE=hogehoge
```

# 利用方法

```
$ laws --version
aws-cli/2.4.20 Python/3.8.8 Linux/5.10.76-linuxkit docker/x86_64.amzn.2 prompt/off
```

```
$ laws configure list
      Name                    Value             Type    Location
      ----                    -----             ----    --------
   profile                <not set>             None    None
access_key     ****************WRDS              env    
secret_key     ****************awrh              env    
    region           ap-northeast-1              env    ['AWS_REGION', 'AWS_DEFAULT_REGION']
```
