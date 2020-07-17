# PHP-Symfony 3.4 學習筆記

通常此指令為 FLEX 版本，FLEX 版本為最小話，採加法原理。
```bash
Symfony new project-name —version=3.4
```

### Bundle 整合，針對 Symfony ~3.4
_install_
```bash
composer req orm:~1.0 make server dump validator doctrine/annotations security orm-fixtures twig-bundle web-profiler-bundle:^3.4
```

### 指令縮寫
```bash
Php bin/console doctrine:generate:entities
# 可改成
php bin/console d:g:e
# 或給更多資訊
php bin/console doc:g:ent
```


### Doctrine();
_install_
```bash
composer require symfony/orm-pack
composer require --dev symfony/maker-bundle
```

_製作資料庫_
```bash
php bin/console doctrine:database:create
php bin/console make:migration
php bin/console doctrine:migrations:migrate
```

_Migrate 特定 migration_
```bash
# -e test 等於 --env=test 環境設定
php bin/console -e test doctrine:migrations:execute 'DoctrineMigrations\Version20200717040423'
```
_或是不製作 migration 直搗資料庫_
```bash
# -e test 等於 --env=test 環境設定
php bin/console -e test doctrine:schema:create
```

### Fixtures(); 製作假資料
_install_
```bash
composer req orm-fixtures
```
_製作假資料控制器_
```bash
php bin/console make:fixtures
```
_產生假資料_
```bash
php bin/console doctrine:fixtures:load
```

### STOF/Gedmo(); Doctrine 插件
_install_
```bash
composer require stof/doctrine-extensions-bundle
composer require gedmo/doctrine-extensions
```

有插件可以幫忙自動產時間[_參考_](https://youtu.be/19UjJCETTmc?t=635)

_相關設定_
```yaml
  stof_doctrine_extensions:
    orm:
        default:
              timestampable: true
```


### Server(); 內建伺服器
_install_
```bash
composer require --dev symfony/web-server-bundle
```

### dump(); 使用 dump()
_install_
```bash
composer require --dev symfony/var-dumper
```

### validator(); 驗證器
_install_
```bash
composer require symfony/validator doctrine/annotations
```

### Session()
[參考]([semantic_version.md](https://symfony.com/doc/current/components/http_foundation/sessions.html))

### Authentic 使用者系統
_install_
```bash
composer require symfony/security:^3.4
```

Security.yaml 相關設定 [_參考_](https://symfony.com/doc/current/reference/configuration/security.html)

臨時切換使用者 [_參閱_](https://symfony.com/doc/current/security/impersonating_user.html)

_臨時切換使用者- 參考設定_
```yaml
  switch_user: true
```

還有
_表格魔術設定_ [參閱](https://symfony.com/doc/current/security/form_login_setup.html)
與
_API 魔術設定_ [參閱](https://symfony.com/doc/current/security/guard_authentication.html)

### Login Form();
_install_
```bash
composer require symfony/form
```

### CSRF();
_install_
```bash
composer require symfony/security-csrf
```

### TWIG(); 有提供錯誤回應服務
```bash
composer require twig-bundle
```
