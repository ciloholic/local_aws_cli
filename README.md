# AWS Vault + Docker版AWS CLI

# 事前インストール

- [direnv](https://github.com/direnv/direnv)
- [docker](https://docs.docker.com/desktop/mac/install/)
- [aws-vault](https://github.com/99designs/aws-vault)

# 独自関数を定義

```
$ vi ~/.zshrc
# local aws
laws() {
    aws-vault exec $AWS_PROFILE -- env | \
    read envs <<< $(awk -v 'ORS= ' '/AWS_(REGION|ACCESS_KEY_ID|SECRET_ACCESS_KEY|SESSION_TOKEN)/ {print "-e " $0}'); \
    zsh -c "docker run --rm -it $envs amazon/aws-cli $*"
}
$ source ~/.zshrc
```

# AWSプロファイルを作成

```
$ aws-vault add jonsmith
Enter Access Key Id: ABDCDEFDASDASF
Enter Secret Key: %%%
$ aws-vault list
Profile                  Credentials              Sessions
=======                  ===========              ========
jonsmith                 jonsmith                 -
```

# 利用する名前付きプロファイルを環境変数に設定

```
$ direnv edit .
export AWS_PROFILE=jonsmith
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
access_key     ****************DASF              env    
secret_key     ****************awrh              env    
    region           ap-northeast-1              env    ['AWS_REGION', 'AWS_DEFAULT_REGION']
```
