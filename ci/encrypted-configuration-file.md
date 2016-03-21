# Encrypted Configuration File

When using CI services such as [Travis](https://travis-ci.org/) for open source projects there is usually a need for pulling in a bunch of secrets for a build script to use. Travis has a way to use secretes in a form of encrypted `secure` configuration [variables](https://docs.travis-ci.com/user/environment-variables/#Encrypted-Variables). However using this mechanism can be cumbersome when secrets frequently change or there are many to manage.

A really neat way to bundle all the secrets together is to version an encrypted file that gets decrypted with your [encryption keys](https://docs.travis-ci.com/user/encryption-keys/) upon build. I stumbled upon this when browsing [nebula-plugins](https://github.com/nebula-plugins), it would be interesting to know if this is an original idea from Netflix engineers or elsewhere. Here is a section from Travis configuration demonstrating the idea:

```yaml
before_install:
- test $TRAVIS_PULL_REQUEST = false && openssl aes-256-cbc -K $encrypted_514f58adac47_key -iv $encrypted_514f58adac47_iv
  -in gradle.properties.enc -out gradle.properties -d || true
```
