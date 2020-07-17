# PHP-Symfony 3.4 學習筆記

## 一. 安裝

通常以下指令創造出來的專案版本為 FLEX 版本，FLEX 版本為最小化，採加法原理。
```bash
symfony new project-name —version=3.4
```

## 二. Bundle 下載，依賴下載
以下指令為，針對 Symfony ~3.4
```bash
composer req orm:~1.0 make server dump validator doctrine/annotations security orm-fixtures twig-bundle web-profiler-bundle:^3.4
```

## 三. 小技巧
指令縮寫
```bash
Php bin/console doctrine:generate:entities
# 可改成
php bin/console d:g:e
# 或給更多資訊
php bin/console doc:g:ent
```

## 四. 資料庫

### Symfony 建議 ORM- Doctrine();

> _安裝_
```bash
composer require symfony/orm-pack
composer require --dev symfony/maker-bundle
```

> _製作資料庫_
```bash
php bin/console doctrine:database:create
# 寫好 entity 之後
php bin/console make:migration
php bin/console doctrine:migrations:migrate
```

> _執行特定 migration_
```bash
# -e test 等於 --env=test 環境設定
php bin/console -e test doctrine:migrations:execute 'DoctrineMigrations\Version20200717040423'
```
> _或是不製作 migration 直搗資料庫_
```bash
# -e test 等於 --env=test 環境設定
php bin/console -e test doctrine:schema:create
```

### 製作假資料- Fixtures();
> _安裝_
```bash
composer req orm-fixtures
```
> _製作假資料控制器_
```bash
php bin/console make:fixtures
```
> _產生假資料_
```bash
php bin/console doctrine:fixtures:load
```

### Doctrine 插件- STOF/Gedmo();
> _安裝_
```bash
composer require stof/doctrine-extensions-bundle
composer require gedmo/doctrine-extensions
```

> _相關設定_，此處針對自動產時間
```yaml
# config/pakcages/stof_doctrine_extensions.yaml

stof_doctrine_extensions:
  orm:
    default:
      timestampable: true
```
[_影片參考_](https://youtu.be/19UjJCETTmc?t=635)


## 五. 伺服器
### Server();
> _安裝_
```bash
composer require --dev symfony/web-server-bundle
```
>跑起來！內建伺服器
```bash
php bin/console server:run
```

## 六. 偵錯
### dump();
_安裝_
```bash
composer require --dev symfony/var-dumper
```

在 php 檔案內就可以任意使用 dump 方法
```php
dump(123, 'anything);
```

## 七. 驗證
### 套件- validator();
_安裝_
```bash
composer require symfony/validator doctrine/annotations
```



## 八. 授權與認證
### 認識 Symfony 指定套件- Security();
_安裝_
```bash
composer require symfony/security:^3.4
```

>config/packages/security.yaml 相關設定

[_參考_](https://symfony.com/doc/current/reference/configuration/security.html)

>臨時切換使用者
```yaml
# config/packages/security.yaml
  switch_user: true
```
 [_參閱_](https://symfony.com/doc/current/security/impersonating_user.html)

>_表格魔術設定_

[參閱](https://symfony.com/doc/current/security/form_login_setup.html)


>_API 魔術設定_

[參閱](https://symfony.com/doc/current/security/guard_authentication.html)

### 表格登入- Login Form();
>_安裝_
```bash
composer require symfony/form
```
> 重要參考

https://symfony.com/doc/3.4/security.html

>相關設定，以下秀出最必要的設定
```yaml
# config/packages/security.yaml
security:
  providers:
    my_provider:
      entity:
        class: App\Entity\User
        property: username
  firewalls:
    main:
      anonymous: true
      form_login:
        login_path: login
        check_path: login
        provider: my_provider
      logout:
        path: /logout
        target: /
  encoders:
    App\Entity\User:
      algorithm: bcrypt
  access_control:
    - { path: ^/login, roles: IS_AUTHENTICATED_ANONYMOUSLY }
    - { path: ^/, roles: ROLE_USER }
```

### CSRF();
_安裝_
```bash
composer require symfony/security-csrf
```
>相關設定，以下秀出最必要的設定
```yaml
# config/packages/security.yaml
security:
  main:
    form_login:
      csrf_token_generator: security.csrf.token_manager
```


### RESTful API 登入- Json Login();
```yaml
security:
  providers:
    my_provider:
      entity:
        class: App\Entity\User
        property: username
  firewalls:
    main:
      anonymous: true
      json_login:
        check_path: /login
  encoders:
    App\Entity\User:
      algorithm: bcrypt
  access_control:
    - { path: ^/login, roles: IS_AUTHENTICATED_ANONYMOUSLY }
    - { path: ^/先找到先贏, roles: ROLE_USER }
    - { path: ^/, roles: 自訂角色 }
```

## 九. 畫面
### 有提供錯誤回應服務- TWIG();
> 安裝
```bash
composer require twig-bundle
```

> 設定回應格式，以下以 Json 為範例，預設是 HTML
```php
/**
 * @Route(defaults = {"_format": "json"})
 */
```

## 十. Session
[參考]([semantic_version.md](https://symfony.com/doc/current/components/http_foundation/sessions.html))

## 十一.寫測試
### 初始化
> 安裝
```bash
composer require --dev phpunit/phpunit ^8
composer require --dev symfony/browser-kit symfony/css-selector
composer require dama/doctrine-test-bundle liip/test-fixtures-bundle
```

>PHP Storm 相關設定

[影片](https://www.youtube.com/watch?v=I7aGWO6K3Ho)

>額外提醒

`config/bundles.php`
```php
// 有時候 make 在 env=test 無法使用，記得要開啟權限
return [
  Symfony\Bundle\MakerBundle\MakerBundle::class => ['dev' => true, 'test' => true],
]
```

### liip/test-fixtures-bundle() 製作假資料;

Fixture [參考](https://medium.com/@ger86/symfony-improving-your-tests-with-doctrinefixturesbundle-1a37b704ac05)

步驟
1. _安裝_
```bash
composer require liip/test-fixtures-bundle
```

2. _改變設定 config/packages/test/framework.yaml_
```yaml
liip_test_fixtures:
  cache_db:
    sqlite: liip_test_fixtures.services_database_backup.sqlite
```

3. 測試檔，再開 setUp 階段，產假資料
```php

use Liip\TestFixturesBundle\Test\FixturesTrait; // 注意這裡

class TradeControllerTest extends WebTestCase
{
  private $client = null;

  use FixturesTrait; // 注意這裡

  public function setUp(): void
  {
    $this->client = static::createClient();

    // 注意下面三行，引用檔案
    $this->loadFixtures([
      'App\DataFixtures\UserFixture'
    ]);
  }
```

### 測試依賴登入時，若使用 security 登入

_設定 security.yaml_
```yaml
# 記得加 http_basic
    main:
      http_basic: ~
```
_測試檔內_
```php
  public function setUp(): void
  {
    $this->client = static::createClient([], [
      'PHP_AUTH_USER' => 'adminTest1',
      'PHP_AUTH_PW' => '0000',
    ]);
  }
```

### 取得 doctrine 方法
```php
  public function setUp(): void
  {
    $kernel = self::bootKernel();

    $this->em = $kernel->getContainer()
      ->get('doctrine')
      ->getManager();

    $this->tradeRepository = $this->em->getRepository(TradeDetail::class);
  }
```
