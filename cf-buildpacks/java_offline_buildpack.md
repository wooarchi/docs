# Java Offline buildpack

- Java offline buildpack build
```
# source code download
$ git clone https://github.com/cloudfoundry/java-buildpack.git

# version checkout
$ git checkout v4.8

# code build
$ bundle install
$ bundle exec rake clean package OFFLINE=true PINNED=true
...
Creating build/java-buildpack-offline-v4.8.zip
```

- Java offline buildpack upload
```
# check buildpacks
$ cf buildpacks
buildpack                                   position   enabled   locked   filename
test-sample-builpack-1234_buildpack-1_4_5   1          true      false
test-sample-builpack-1234_buildpack-43      2          true      false
staticfile_buildpack-v1-4-20                3          true      false    staticfile-buildpack-v1.4.20.zip
java_buildpack-v4-7                         4          false     false    java-buildpack-v4.7.zip
ruby_buildpack-v1-7-7                       5          true      false    ruby-buildpack-v1.7.7.zip
dotnet_core_buildpack-v1-0-32               6          true      false    dotnet-core-buildpack-v1.0.32.zip
nodejs_buildpack-v1-6-12                    7          true      false    nodejs-buildpack-v1.6.12.zip
go_buildpack-v1-8-14                        8          true      false    go-buildpack-v1.8.14.zip
python_buildpack-v1-6-3                     9          true      false    python-buildpack-v1.6.3.zip
php_buildpack-v4-3-44                       10         true      false    php-buildpack-v4.3.44.zip
binary_buildpack-v1-0-15                    11         true      false    binary-buildpack-v1.0.15.zip

# create buildpack
$ cf create-buildpack java_offline_buildpack-v4-8 java-buildpack-offline-v4.8.zip 12  --enable

# diable java online buildpack
$ cf update-buildpack java_buildpack-v4-7 --disable

# buildpack check
$ cf buildpacks
buildpack                                   position   enabled   locked   filename
test-sample-builpack-1234_buildpack-1_4_5   1          true      false
test-sample-builpack-1234_buildpack-43      2          true      false
staticfile_buildpack-v1-4-20                3          true      false    staticfile-buildpack-v1.4.20.zip
java_buildpack-v4-7                         4          false     false    java-buildpack-v4.7.zip
ruby_buildpack-v1-7-7                       5          true      false    ruby-buildpack-v1.7.7.zip
dotnet_core_buildpack-v1-0-32               6          true      false    dotnet-core-buildpack-v1.0.32.zip
nodejs_buildpack-v1-6-12                    7          true      false    nodejs-buildpack-v1.6.12.zip
go_buildpack-v1-8-14                        8          true      false    go-buildpack-v1.8.14.zip
python_buildpack-v1-6-3                     9          true      false    python-buildpack-v1.6.3.zip
php_buildpack-v4-3-44                       10         true      false    php-buildpack-v4.3.44.zip
binary_buildpack-v1-0-15                    11         true      false    binary-buildpack-v1.0.15.zip
java_offline_buildpack-v4-8                 12         true      false    java-buildpack-offline-v4.8.zip
```
