### Shopware Version

6.7.8.1

### Affected area / extension

Platform(Default)

### Actual behaviour

When opening an order in the Administration, the order detail page automatically enters an edit mode. This triggers an order versioning process before any content is displayed in the UI. While this allows users to edit and subsequently save or discard changes to a versioned order, it creates a significant performance bottleneck for large orders.

When an order contains a high number of line items, this versioning process takes an excessive amount of time and can lead to request timeouts. **When the timeout occurs, the UI fails to load**, showing no error message or fallback content, which is highly confusing for the end user.

We encountered this with a B2B customer order containing approximately 400 line items. While other factors might contribute, the line item count is the primary driver of the performance degradation.

The stack trace suggests that EntityReader::loadOneToManyWithoutPagination is the root cause of the slowdown during the cloning/versioning process. ([source](https://github.com/shopware/shopware/blob/0b79c9a90b9b0251b0eef111fd4ae7d311d2c687/src/Core/Framework/DataAbstractionLayer/Dbal/EntityReader.php#L553))

```
PDOException: SQLSTATE[HY000]: General error: 3024 Query execution was interrupted, maximum statement execution time exceeded
#0 /var/www/vendor/doctrine/dbal/src/Driver/PDO/Statement.php(130): PDOStatement::execute
#1 /var/www/vendor/doctrine/dbal/src/Driver/PDO/Statement.php(130): Doctrine\DBAL\Driver\PDO\Statement::execute
#2 /var/www/vendor/doctrine/dbal/src/Connection.php(1109): Doctrine\DBAL\Connection::executeQuery
#3 /var/www/vendor/doctrine/dbal/src/Query/QueryBuilder.php(344): Doctrine\DBAL\Query\QueryBuilder::executeQuery
#4 /var/www/pickware-dependencies/pickware-shopware/src/Core/Framework/DataAbstractionLayer/Dbal/EntityReader.php(317): Shopware\Core\Framework\DataAbstractionLayer\Dbal\EntityReader::fetch
#5 /var/www/pickware-dependencies/pickware-shopware/src/Core/Framework/DataAbstractionLayer/Dbal/EntityReader.php(133): Shopware\Core\Framework\DataAbstractionLayer\Dbal\EntityReader::_read
#6 /var/www/pickware-dependencies/pickware-shopware/src/Core/Framework/DataAbstractionLayer/Dbal/EntityReader.php(499): Shopware\Core\Framework\DataAbstractionLayer\Dbal\EntityReader::loadOneToManyWithoutPagination
#7 /var/www/pickware-dependencies/pickware-shopware/src/Core/Framework/DataAbstractionLayer/Dbal/EntityReader.php(442): Shopware\Core\Framework\DataAbstractionLayer\Dbal\EntityReader::loadOneToMany
#8 /var/www/pickware-dependencies/pickware-shopware/src/Core/Framework/DataAbstractionLayer/Dbal/EntityReader.php(1266): Shopware\Core\Framework\DataAbstractionLayer\Dbal\EntityReader::fetchAssociations
#9 /var/www/pickware-dependencies/pickware-shopware/src/Core/Framework/DataAbstractionLayer/Dbal/EntityReader.php(137): Shopware\Core\Framework\DataAbstractionLayer\Dbal\EntityReader::_read
#10 /var/www/pickware-dependencies/pickware-shopware/src/Core/Framework/DataAbstractionLayer/Dbal/EntityReader.php(499): Shopware\Core\Framework\DataAbstractionLayer\Dbal\EntityReader::loadOneToManyWithoutPagination
#11 /var/www/pickware-dependencies/pickware-shopware/src/Core/Framework/DataAbstractionLayer/Dbal/EntityReader.php(442): Shopware\Core\Framework\DataAbstractionLayer\Dbal\EntityReader::loadOneToMany
#12 /var/www/pickware-dependencies/pickware-shopware/src/Core/Framework/DataAbstractionLayer/Dbal/EntityReader.php(1266): Shopware\Core\Framework\DataAbstractionLayer\Dbal\EntityReader::fetchAssociations
#13 /var/www/pickware-dependencies/pickware-shopware/src/Core/Framework/DataAbstractionLayer/Dbal/EntityReader.php(137): Shopware\Core\Framework\DataAbstractionLayer\Dbal\EntityReader::_read
#14 /var/www/pickware-dependencies/pickware-shopware/src/Core/Framework/DataAbstractionLayer/Dbal/EntityReader.php(499): Shopware\Core\Framework\DataAbstractionLayer\Dbal\EntityReader::loadOneToManyWithoutPagination
#15 /var/www/pickware-dependencies/pickware-shopware/src/Core/Framework/DataAbstractionLayer/Dbal/EntityReader.php(442): Shopware\Core\Framework\DataAbstractionLayer\Dbal\EntityReader::loadOneToMany
#16 /var/www/pickware-dependencies/pickware-shopware/src/Core/Framework/DataAbstractionLayer/Dbal/EntityReader.php(1266): Shopware\Core\Framework\DataAbstractionLayer\Dbal\EntityReader::fetchAssociations
#17 /var/www/pickware-dependencies/pickware-shopware/src/Core/Framework/DataAbstractionLayer/Dbal/EntityReader.php(137): Shopware\Core\Framework\DataAbstractionLayer\Dbal\EntityReader::_read
#18 /var/www/pickware-dependencies/pickware-shopware/src/Core/Framework/DataAbstractionLayer/Dbal/EntityReader.php(76): Shopware\Core\Framework\DataAbstractionLayer\Dbal\EntityReader::read
#19 /var/www/pickware-dependencies/pickware-shopware/src/Core/Framework/DataAbstractionLayer/VersionManager.php(244): Shopware\Core\Framework\DataAbstractionLayer\VersionManager::cloneEntity
#20 /var/www/pickware-dependencies/pickware-shopware/src/Core/Framework/DataAbstractionLayer/VersionManager.php(146): Shopware\Core\Framework\DataAbstractionLayer\VersionManager::createVersion
#21 /var/www/pickware-dependencies/pickware-shopware/src/Core/Framework/DataAbstractionLayer/EntityRepository.php(162): Shopware\Core\Framework\DataAbstractionLayer\EntityRepository::createVersion
```

Example Screenshot. Note that some regular `order` fetches (green arrow) work fine, while the versioning requests times out.

<img width="1122" height="1007" alt="Image" src="https://github.com/user-attachments/assets/2e37ffbd-edd9-4466-8bc8-6d4615cc1b1a" />
<img width="1079" height="532" alt="Image" src="https://github.com/user-attachments/assets/2fb2c564-9d6d-4203-a7bd-cb1d16a59247" />

### Expected behaviour

Order should open, regardless of the size (the number of order line items)

### How to reproduce

- create an order with "a lot" of product line items (try 500, 1000 or more)
- open the order in the Administration
