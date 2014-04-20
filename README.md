#railsの構築
- http://qiita.com/emadurandal/items/a60886152a4c99ce1017を参考に

```
git clone git://github.com/sstephenson/rbenv.git ~/.rbenv
mkdir -p ~/.rbenv/plugins
git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
```

- .bash_profileに以下を書く

```
export PATH="$HOME/.rbenv/bin:$PATH"
eval "$(rbenv init -)"

```

- brewでinstall

```
 brew install openssl
 brew install readline
 //これはやらず。 sudo port install libiconv
```

- rbenvでruby 2.1.1をインストール

- bundlerのインストール


```
rbenv exec gem install bundler
rbenv rehash
```

ここからは、repositoryに含まれているので不要
- rails(4.0.4)をローカルにインストール

```
$ cat << EOS > Gemfile
> source "http://rubygems.org"
> gem "rails", "4.0.4" # ←ローカルインストールしたいRailsのバージョンを指定。指定しなければ最新版が入る。
> EOS

$ bundle install --path vendor/bundle
$ bundle exec rails new WeddingCamera --skip-bundle
$ rm -f Gemfile
$ rm -f Gemfile.lock
$ rm -rf .bundle
$ rm -rf vendor/bundle
```

# mysqlセットアップ
## for mac
- http://tukaikta.blog135.fc2.com/blog-entry-197.html

```
$ brew install mysql
$ unset TMPDIR
$ mysql_install_db --verbose --user=`whoami` --basedir="$(brew --prefix mysql)" --datadir=/usr/local/var/mysql --tmpdir=/tmp
$ mysql.server start
$ mysqladmin -u root password 'new-password'
$ mysqladmin -u root -h kikaigyo.local password 'new-password'
```
- その他諸々上記URLの通り設定する。

### ユーザ作る

```
mysql> create user wedcam_user identified by 'wedcam_user';
mysql> grant all on *.* to wedcam_user@localhost;
mysql> set password for 'wedcam_user'@'localhost' = PASSWORD('wedcam_user');
mysql> create database wedcam_development;
mysql> create database wedcam_test;
mysql> create database wedcam_production;
```

### dbを作る
```
$ bundle exec rake db:migrate
```
