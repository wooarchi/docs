# Ruby offline buildpack

# source code download
$ git clone https://github.com/cloudfoundry/ruby-buildpack.git

# version checkout
$ git checkout v1.7.15

# code build
$ source .envrc
$ cd src/ruby/vendor/github.com/cloudfoundry/libbuildpack/packager/buildpack-packager && go install
$ cd ~/ruby-buildpack

※ buildpack-packager build [ --cached ]
$ buildpack-packager build --cached
...

- Ruby offline buildpack upload
```
# check buildpacks
$ cf buildpacks
buildpack                          position   enabled   locked   filename
staticfile_buildpack-v1-4-16       1          true      false    staticfile-buildpack-v1.4.16.zip
java_buildpack-v4-5-1              2          true      false    java-buildpack-v4.5.1.zip
ruby_buildpack-v1-7-3              3          true      false    ruby-buildpack-v1.7.3.zip
dotnet_core_buildpack-v1-0-27      4          true      false    dotnet-core-buildpack-v1.0.27.zip
nodejs_buildpack-v1-6-7            5          true      false    nodejs-buildpack-v1.6.7.zip
go_buildpack-v1-8-8                6          true      false    go-buildpack-v1.8.8.zip
python_buildpack-v1-5-26           7          true      false    python-buildpack-v1.5.26.zip
php_buildpack-v4-3-42              8          true      false    php-buildpack-v4.3.42.zip
binary_buildpack-v1-0-14           9          true      false    binary-buildpack-v1.0.14.zip
egov_buildpack-v3-5                10         true      false    egov-buildpack-offline-egov3.5.zip
python_offline_buildpack-v1-6-11   11         true      false    python_buildpack-v1.6.11.zip

# create buildpack
$ cf create-buildpack ruby_offline_buildpack-v1-7-15 ruby_buildpack-cached-v1.7.15.zip 12 --enable

# disable java online buildpack
$ cf update-buildpack ruby_buildpack-v1-7-3 --disable

# buildpack check
$ cf buildpacks
buildpack                          position   enabled   locked   filename
staticfile_buildpack-v1-4-16       1          true      false    staticfile-buildpack-v1.4.16.zip
java_buildpack-v4-5-1              2          true      false    java-buildpack-v4.5.1.zip
ruby_buildpack-v1-7-3              3          false     false    ruby-buildpack-v1.7.3.zip
dotnet_core_buildpack-v1-0-27      4          true      false    dotnet-core-buildpack-v1.0.27.zip
nodejs_buildpack-v1-6-7            5          true      false    nodejs-buildpack-v1.6.7.zip
go_buildpack-v1-8-8                6          true      false    go-buildpack-v1.8.8.zip
python_buildpack-v1-5-26           7          true      false    python-buildpack-v1.5.26.zip
php_buildpack-v4-3-42              8          true      false    php-buildpack-v4.3.42.zip
binary_buildpack-v1-0-14           9          true      false    binary-buildpack-v1.0.14.zip
egov_buildpack-v3-5                10         true      false    egov-buildpack-offline-egov3.5.zip
python_offline_buildpack-v1-6-11   11         true      false    python_buildpack-v1.6.11.zip
ruby_offline_buildpack-v1-7-15     12         true      false    ruby_buildpack-cached-v1.7.15.zi
```
