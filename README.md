# dev-kit (WIP)
Set of usefull configurations for my php / symfony projects

## Adding quality tools
### Phpstan
```bash
composer require phpstan/phpstan-doctrine phpstan/phpstan-phpunit phpstan/phpstan-symfony
```

### php-cs
```bash
composer require friendsofphp/php-cs-fixer squizlabs/php_codesniffer
```

## Automation with a Makefile
```make
reset-test-db: ## drop, create db and schema and load fixture for test env
	symfony console doctrine:database:drop --force --if-exists --env=test
	symfony console doctrine:database:create --if-not-exists --env=test
	symfony console doctrine:schema:update --force --env=test
	symfony console hautelook:fixtures:load --no-bundles --no-interaction --env=test

test: ## start test suite
	@make reset-test-db
	php bin/phpunit --stop-on-failure
lint: ## phpstan linter
	php -d memory_limit=4G vendor/bin/phpstan analyse src

cs: ## code style inspecter and fixer
	php vendor/bin/php-cs-fixer fix

quality:
	@make cs
	@make lint
	@make test
```




