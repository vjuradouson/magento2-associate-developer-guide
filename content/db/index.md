### 4.0. Working With Databases In Magento
This section covers **18%** of the exam.

#### 4.1. Models, Resource Models, And Collections
> What are the responsibilities of each of the ORM object types?

The Magento ORM elements are:
- **Models:** Define entities, their data, and their behaviour.
- **Resource Models:** Data mappers for storage structures.
- **Collections:** Stores sets of Models and related functionality including filtering, sorting, and pagination.
- **Resources:** Maintain database connections through adapters.

---
> How do they relate to one another?

---
#### 4.2. Entity Loading And Saving
> How do you use the native Magento save/load process in the development process?

---
#### 4.3. Filter, Sort, And Select From Collections & Repositories
> How do you select a subset of records from the database?

##### Collections
**Filter:** `$collection->addFieldToFilter()`
**Sort:** `$collection->addOrder()`
**Select Column:** `$collection->addFieldToSelect()`
**Pagination:** `$collection->setPageSize()` and `$collection->setCurPage()`

##### Repositories
https://devdocs.magento.com/guides/v2.2/extension-dev-guide/searching-with-repositories.html

---
#### 4.4. Declarative Schema
Declarative schema in Magento 2.3 allows developers to declare the final desired state of the database and has the system adjust to it automatically, without performing redundant operations. Developers are no longer forced to write scripts for each new version. Additionally, this approach allows data be deleted when a module is uninstalled - something that previously had to be done separately.

##### Terminology
Data Patch
- A class that contains data modification instructions. It can have dependencies on other data or schema patches.
- Some data patches can be reverted as a module or patch is uninstalled or deleted. Revertable operations are Data Query Language (DQL) and Data Manipulation Language (DML) operations: `INSERT`, `UPDATE`.

Schema Patch
- A class that contains custom schema modification instructions. Schema patches are used along with declarative schema, but these patches allow complex operations such as:
    - Adding triggers, stored procedures, or functions
    - Performing data migration with inside DDL operations
    - Renaming tables, columns, and other entities
    - Adding partitions and options to a table

---
> How do you add a column using declarative schema?

You can create/modify the `etc/db_schema.xml` file, specifying a `<table />` node for the table you are editing and adding a `<column />` node inside to create the new column.
```XML
<schema xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
        xsi:noNamespaceSchemaLocation="urn:magento:framework:Setup/Declaration/Schema/etc/schema.xsd">
    <table name="table_name">
        <column xsi:type="varchar"
                name="title"
                padding="10"
                comment="Page Title"
        />
    </table>
</schema>
```

You can also create a schema patch file in the `YourCompany\YourModule\Setup\Patch\Schema` namespace:
```php
<?php
namespace YourCompany\YourModule\Setup\Patch\Schema;

use Magento\Framework\DB\Ddl\Table;
use Magento\Framework\Setup\Patch\SchemaPatchInterface;
use Magento\Framework\Setup\SchemaSetupInterface;

class YourSchemaPatch implements SchemaPatchInterface
{
    /** @var SchemaSetupInterface */
    private $moduleSchemaSetup;

    /**
     * YourSchemaPatch Constructor.
     *
     * @param SchemaSetupInterface $moduleSchemaSetup
     */
    public function __construct(SchemaSetupInterface $moduleSchemaSetup)
    {
        $this->moduleSchemaSetup = $moduleSchemaSetup;
    }

    /**
     * {@inheritdoc}
     */
    public function apply()
    {
        $this->schemaSetup->startSetup();
        $table = $this->schemaSetup->getTable('your_table_name');
        $connection = $this->schemaSetup->getConnection();
        
        if ($connection->isTableExists($table)) {
            // declare new columns to add
            $newColumns = [
                'shiny_new_column' => [
                    'type' => Table::TYPE_TEXT,
                    'nullable' => true,
                    'comment' => 'Shiny New Column',
                ],
                'another_shiny_column' => [
                    'type' => Table::TYPE_TEXT,
                    'nullable' => true,
                    'comment' => 'OMG More Shinies! :O',
                ],
            ];

            foreach ($newColumns as $newColumn => $columnDescription) {
                $connection->addColumn($table, $newColumn, $columnDescription);
            }
        }

        $this->schemaSetup->endSetup();
    }

    /**
     * {@inheritDoc}
     */
    public static function getDependencies()
    {
        // return [dependencies]
    }
    
    /**
     * {@inheritDoc}
     */
    public function getAliases()
    {
        // return [aliases]
    }
}
```

**NB:** When adding new columns to a table, you also need to generate the `db_schema_whitelist.json` file.

---
> How do you modify a table added by another module?

To modify a table added by another module, create `Your_Company/Your_Module/etc/db_schema.xml` and specify the table name in the `<table />` node:
```XML
<schema xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="urn:magento:framework:Setup/Declaration/Schema/etc/schema.xsd">
    <table name="table_create_by_other_module">
        ...
    </table>
</schema>
```

---
> How do you delete a column?

To remove a column from a table created by your module you can simply remove the corresponding `<column />` node in `Your_Company/Your_Module/etc/db_schema.xml`. To drop a column declared in another module, you must redeclare it and set the `disabled` attribute to `true`:
```XML
<schema xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="urn:magento:framework:Setup/Declaration/Schema/etc/schema.xsd">
    <table name="table_create_by_other_module">
        ...
        <column xsi:type="int"
                name="new_id"
                padding="10"
                unsigned="true"
                nullable="false"
                comment="New ID"
                disabled="true"
        />
        ...
    </table>
</schema>
```

---
> How do you add an index or foreign key using declarative schema?

##### Index
To add an index, declare an `<index />` node in `Your_Company/Your_Module/etc/db_schema.xml`:
```XML
<schema xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="urn:magento:framework:Setup/Declaration/Schema/etc/schema.xsd">
    <table name="table_create_by_other_module">
        ...
        <index referenceId="INDEX_NAME" indexType="btree">
            <column name="column_name" />
        </index>
        ...
    </table>
</schema>
```

##### Foreign Key
To add a foreign key, declare a `<constraint />` node with an `xsi:type` of `foreign` in `Your_Company/Your_Module/etc/db_schema.xml`:
```XML
<schema xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="urn:magento:framework:Setup/Declaration/Schema/etc/schema.xsd">
    <table name="table_create_by_other_module">
        ...
        <constraint xsi:type="foreign"
                    referenceId="YOUR_REFERENCE_ID"
                    
                    <!-- The current table -->
                    table="table_name"
                    
                    <!-- The column in the current table that refers to a column in another table -->
                    column="column_name"
                    
                    <!-- The table being referenced -->
                    referenceTable=""
                    
                    <!-- The column being referenced -->
                    referenceColumn=""
                    
                    <!-- Foreign key trigger (CASCADE/SET/NULL/NO ACTION) -->
                    onDelete="CASCADE"
        />
        ...
    </table>
</schema>
```

---
> How do you manipulate data using data patches?

---
> What is the purpose of schema patches?

Schema patches are used to make custom database schema modifications:
- Renaming tables
- Adding/removing columns
- Setting primary & foreign keys
