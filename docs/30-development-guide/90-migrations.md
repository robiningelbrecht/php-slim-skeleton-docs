# Database migrations

To manage database migrations, the [doctrine/migrations](https://www.doctrine-project.org/projects/doctrine-migrations/en/3.6/) 
package is used.

> The Doctrine Migrations project offers additional functionality on top of the DBAL and ORM 
> for versioning your database schema. It makes it easy and safe to deploy changes to it in a way 
> that can be reviewed and tested before being deployed to production.

All migration files will be added under `/migrations`.

## Set up an entity

First, you'll want to create an entity (or aggregate) and add the necessary ORM attributes to it:

```php showLineNumbers title="User.php"
#[Entity]
class User extends AggregateRoot
{
    private function __construct(
        #[Id, Column(type: 'string', unique: true, nullable: false)]
        private readonly UserId $userId,
        #[Column(type: 'string', nullable: false)]
        private readonly Name $name,
    ) {
    }

    // ...
}
```

## Create a migration

Next you can have Doctrine generate a migration for you by comparing the current state of your
database schema to the mapping information that is defined by using the ORM:

```bash
docker-compose run --rm php-cli vendor/bin/doctrine-migrations diff
```

This should generate a new file located in `/migrations`:

```php showLineNumbers title="migrations/Version20220805142337.php"
/**
 * Auto-generated Migration: Please modify to your needs!
 */
final class Version20220805142337 extends AbstractMigration
{
    public function getDescription(): string
    {
        return '';
    }

    public function up(Schema $schema): void
    {
        // this up() migration is auto-generated, please modify it to your needs
        $this->addSql('CREATE TABLE User (userId VARCHAR(255) NOT NULL, name VARCHAR(255) NOT NULL PRIMARY KEY(userId)) DEFAULT CHARACTER SET utf8 COLLATE `utf8_unicode_ci` ENGINE = InnoDB');
    }

    public function down(Schema $schema): void
    {
        // this down() migration is auto-generated, please modify it to your needs
        $this->addSql('DROP TABLE User');
    }
}
```

## Run the migration

Finally, you can update your database schema to the latest version of your ORM by running

```bash
docker-compose run --rm php-cli vendor/bin/doctrine-migrations migrate
```
