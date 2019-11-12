<h2 align="center">Magento 2 Associate Developer Certification Study Guide</h2>
	

<p align="center">
	A collection of notes for the Magento 2 Associate Developer Certification exam.
</p>

<br>

## 📝 Table of Contents
Information
- [Tips & Useful Information](/magento2-associate-developer-guide/tips)
- [Exam Structure](/magento2-associate-developer-guide/exam_structure)

Exam content
1. [Magento Architecture & Customisation Techniques](/magento2-associate-developer-guide/content/architecture)
1. [Request Flow Processing](/magento2-associate-developer-guide/content/request_flow)

---
### 3.0. Customising The Magento UI
This section covers **15%** of the exam.

#### 3.1. Customise The Magento UI Using Themes
> When would you create a new theme?

You would create a new theme when making design changes to the Magento frontend or admin. This typically involves copying and modifying layout files, templates, and styles to achieve your desired design.

---
> How do you define theme hierarchy for a project?

The `theme.xml` file can be used to specify the theme's parent using the `<parent />` node:
```XML
<theme xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:noNamespaceSchemaLocation="urn:magento:framework:Config/etc/theme.xsd">
    <title>Your Theme Name</title>
	<parent>Vendor/Theme</parent>
</theme>
```

Omitting the `<parent />` node dictates that the theme is the base, or default, theme.

---
#### 3.2. Blocks & Templates
> How do you assign a template to a block?

To assign a template to a block:
```XML
<block class="Namespace\Of\Your\Class"
       name="blockName"
       template="Module_Name::path/to/your/template.phtml"
/>
```

**NB:** The `path/to/your/template.phtml` starts from the `templates/` directory.

---
> How do you assign a different template to a native block?

To assign a template to an existing block, pass an argument to the block:
```XML
<referenceBlock name="blockName">
    <action method="setTemplate">
        <argument name="template" xsi:type="string">
            Module_Name::path/to/your/template.phtml
        <argument>
    </action>
</referenceBlock>
```

---
#### 3.3. Identify Different Types Of Blocks
> When would you use non-template block types?

| Block Type | Path | Use |
| :--------: | :--: | :-: |
| Text | `vendor/magento/framework/View/Element/Text.php` | Printing a string |
| ListText | `vendor/magento/framework/View/Element/Text/ListTest.php` | Output each of the child blocks |

---
#### 3.4. Magento Layout XML
> How do you use layout XML directives in your customizations?

Layout XML is what links templates to blocks and there are a number of directives available for use.

##### `<block />`
Defines a block. A block is a unit of page output that renders some distinctive content (anything visually tangible for the end-user), such as a piece of information or a user interface element. Blocks are a foundational building unit for layouts in Magento. They are the link between a PHP block class (which contains logic) and a template (which renders content). Blocks can have children and grandchildren (and so on).

| Attribute | Description | Values | Required |
| :-------: | :---------: | :----: | :------: |
| `class` | Name of a class that implements rendering of a particular block. An object of this class is responsible for actual rendering of block output. | Class name | No |
|` name` | Name that can be used to address the block to which this attribute is assigned. The name must be unique per generated page. If not specified, an automatic name will be assigned in the format `ANONYMOUS_n`. | 0-9, A-Z, a-z, underscore (`_`), period (`.`), dash (`-`). Should start with a letter. Case-sensitive. | No |
| `before` | Used to position the block before an element under the same parent. The element name or alias name is specified in the value. Use dash (`-`) to position the block before all other elements of its level of nesting. | Element name or dash (`-`). | No |
| `after` | Used to position the block after an element under the same parent. The element name or alias name is specified in the value. Use dash (`-`) to position the block after all other elements of its level of nesting. | Element name or dash (`-`). | No |
| `template` | A template that represents the functionality of the block to which this attribute is assigned. | Template file name | No |
| `as` | An alias name that serves as identifier in the scope of the parent element. | Same as `name` | No |
| `cachable` | Defines whether a block element is cacheable. This can be used for development purposes and to make needed elements of the page dynamic. | `true` or `false` | No |

```XML
@TODO: <block /> example
```

##### `<container />`
A structure without content that holds other layout elements such as blocks and containers.  A container renders child elements during view output generation. It can be empty or it can contain an arbitrary set of `<container>` and `<block>` elements. If the `<container>` is empty, and there is no child `<block>` available, it will not be displayed in the frontend source code.

| Attribute | Description | Values | Required |
| :-------: | :---------: | :----: | :------: |
|` name` | Name that can be used to address the block to which this attribute is assigned. The name must be unique per generated page. If not specified, an automatic name will be assigned in the format `ANONYMOUS_n`. | 0-9, A-Z, a-z, underscore (`_`), period (`.`), dash (`-`). Should start with a letter. Case-sensitive. | No |
| `label` | An arbitrary name to display in the web browser. | Any | No |
| `before` | Used to position the block before an element under the same parent. The element name or alias name is specified in the value. Use dash (`-`) to position the block before all other elements of its level of nesting. | Element name or dash (`-`). | No |
| `after` | Used to position the block after an element under the same parent. The element name or alias name is specified in the value. Use dash (`-`) to position the block after all other elements of its level of nesting. | Element name or dash (`-`). | No |
| `as` | An alias name that serves as identifier in the scope of the parent element. | Same as `name` | No |
| `output` | Defines whether to output the root element. If specified, the element will be added to output list. If not specified, the parent element is responsible for rendering its children. | Any value except the obsolete `toHtml`. Recommended value is `1`. | No |
| `htmlTag` | Output parameter. If specified, the output is wrapped into specified HTML tag. | Any valid HTML5 tag | No |
| `htmlID` | Output parameter. If specified, the value is added to the wrapper element. If there is no wrapper element, this attribute has no effect. | Any valid HTML5 `id` value | No |
| `htmlClass` | Output parameter. If specified, the value is added to the wrapper element. If there is no wrapper element, this attribute has no effect. | Any valid HTML5 `class` value | No |

```XML
@TODO: <container /> example
```

##### `<referenceBlock />` and `<referenceContainer />`
Updates in `<referenceBlock>` and `<referenceContainer>` are applied to the corresponding `<block>` or `<container>`. For example, if you make a reference by `<referenceBlock name="right">`, you are targeting the block `<block name="right">`.

| Attribute | Description | Values | Required |
| :-------: | :---------: | :----: | :------: |
| `remove` | Allows you to remove or cancel the removal of the element. When a container is removed, its child elements are removed as well. | `true` or `false` | No |
| `display` | Allows you to disable rendering of specific block or container with all its children (both set directly and by reference). The block’s/container’s and its children’ respective PHP objects are still generated and available for manipulation. | `true` or `false` | No |

The `remove` attribute is optional and its default value is `false` This implementation allows you to remove a block or container in your layout by setting the remove attribute value to `true`, or to cancel the removal of a block or container by setting the value to `false`.
```XML
<referenceBlock name="blockName" remove="true" />
```

The `display` attribute is optional and its default value is `true`. You are always able to overwrite this value in your layout. In situation when the remove value is `true`, the `display` attribute is ignored.
```XML
<referenceContainer name="containerName" display="false" />
```

##### `<arguments />`
`<arguments>` is a required container for `<argument>`. It does not have its own attributes.
```XML
<arguments>
    ...
</arguments>
```

##### `<argument />`
Information can be passed from layout XML files to blocks using the `<argument />` node. These directives must always be enclosed within `<arguments />` directives. Argument values set in a layout file are added to the block's `data` array and can be accessed in templates using the `getData('argumentName')` and `hasData('argumentName')` functions. The latter returns a boolean defining whether there’s any value set.

| Attribute | Description | Values | Required |
| :-------: | :---------: | :----: | :------: |
| `name` | Argument name. | Must be unique. | Yes |
| `xsi:type` | Argument type. | 	`string` / `boolean` / `object` / `number` / `null` / `array` / `options` / `url` / `helper` | Yes |
| `translate` |  | `true` or `false` | No |

```XML
<arguments>
    <!-- String -->
    <argument name="stringArgName" xsi:type="string">
        Some String
    </argument>
    
    <!-- Boolean -->
    <argument name="booleanArgName" xsi:type="boolean">
        true
    </argument>
    
    <!-- Object -->
    <argument name="objectArgName" xsi:type="object">
        <!-- Must implement `Magento\Framework\View\Element\Block\ArgumentInterface` -->
        Namespace\To\Your\Class
    </argument>
    
    <!-- Number -->
    <argument name="numberArgName" xsi:type="number">
        42
    </argument>
    
    <!-- Null -->
    <argument name="nullArgName" xsi:type="null" />
    
    <!-- Array -->
    <argument name="arrayArgName" xsi:type="array">
        <item name="arrayKeyOne" xsi:type="string">First Item</item>
        <item name="arrayKeyTwo" xsi:type="object">Namespace\Of\Your\Class</item>
        ...
    </argument>
    
    <!-- Options -->
    <argument name="optionsArgName" xsi:type="options">
        <!-- Must implement `Magento\Framework\Data\OptionSourceInterface` -->
        Namespace\Of\Your\Class
    </argument>
    
    <!-- URL -->
    <argument name="urlArgName"
              xsi:type="url"
              path="your/url/path"
    />
    
    <!-- Helper -->
    <argument name="helperArgName" 
              xsi:type="string"
              helper="Namespace\To\Your\Helper\Class::helperFunction">
        <param name="paramName">paramValue</param>
    </argument>
<arguments>
```

**NB:** The `helper` can only use public functions.

##### `<move />`
Sets the declared block or container element as a child of another element in the specified order. 

| Attribute | Description | Values | Required |
| :-------: | :---------: | :----: | :------: |
| `element` | Name of the element to move. | Element name | Yes |
| `destination` | Name of the target parent element. | Element name | Yes |
| `as` | Alias name for the element in the new location. | 0-9, A-Z, a-z, underscore (`_`), period (`.`), dash (`-`). Case-sensitive.	 | No |
| `after` or `before` | Specifies the element’s position relative to siblings. Use dash (`-`) to position the block before or after all other siblings of its level of nesting. If the attribute is omitted, the element is placed after all siblings. | Element name | No |

```XML
<move element="elementName"
      destination="parentElementName"
      as="newAlias"
      after="siblingElementName"
      before="anotherSiblingElementName"
/>
```

**NB:**
- `<move>` is skipped if the element to be moved is not defined.
- If the `as` attribute is not defined, the current value of the element alias is used. If that is not possible, the value of the `name` attribute is used instead.
- During layout generation, the `<move>` instruction is processed before the removal (set using the `remove` attribute). This means if any elements are moved to the element scheduled for removal, they will be removed as well.

##### `<remove />`
`<remove>` is used only to remove the static resources linked in a page `<head>` section. For removing blocks or containers, use the `remove` attribute for `<referenceBlock>` and `<referenceContainer>`.
```XML
<!-- Remove local resources -->
<remove src="css/styles-m.css" />
<remove src="my-js.js"/>
<remove src="Magento_Catalog::js/compare.js" />

<!-- Remove external resources -->
<remove src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.4/css/bootstrap-theme.min.css"/>
<remove src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.4/js/bootstrap.min.js"/>
<remove src="http://fonts.googleapis.com/css?family=Montserrat" />
```

##### `<update />`
Includes a certain layout file.
```XML
<update handle="{handleNameToInclude}" />
```

---
> How do you register a new layout file?

---
#### 3.5. Create & Add Code & Markup To A Page
> How do you add new content to existing pages using layout XML?

---
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

---
### 5.0. Developing With Adminhtml
This section covers **11%** of the exam.

#### 5.1. Admin Routes
> How would you create an admin controller?

---
> How do you ensure the right level of security for a new controller?

---
#### 5.2. System Configuration & Scopes
> How would you add a new system configuration option?

---
> What is the difference in this process for different option types (secret, file)?

---
#### 5.3. ACL
> How would you add a new ACL resource to a new entity?

---
> How do you manage the existing ACL hierarchy?

---
#### 5.4. Setup A Menu Item
> How do you add a new menu item to a given tab?

Adminhtml menu items are configured in `etc/adminhtml/menu.xml`. To add a new menu item, edit this file:
```XML
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="urn:magento:module:Magento_Backend:etc/menu.xsd">
   <menu>
       <add id="Your_Module::brief_module_description"
           title="Your Menu Tab Title"
           translate="title"
           module="Your_Module"
           sortOrder="10"
           
           <!-- No parent means the tab appears in the side bar -->
           parent="Magento_Catalog::inventory"
           
           <!-- No action means the item acts as a header -->
           action="path/to/your/controller"
           
           <!-- ID of the ACL entry that validates permissions -->
           resource="ACL_Entry_ID"
           />
   </menu>
</config>
```

---
> How do you add a new tab to the Admin menu?

Do not specify a `parent` attribute in the `<add />` node.

---
#### 5.5. Create User Permissions
> How are menu items related to ACL permissions?

Menu items are not shown to users with insufficient permissions to access them.

---
> How do you add a new user with given set of permissions?

Navigate to `System > Permissions > All Users` in Magento admin, add a new user, and set their role under `User Information > User Role`.

This can also be done programmatically:
```PHP
use Magento\User\Model\UserFactory;

// ...

protected $userFactory;

public function __construct(UserFactory $userFactory)
{
    $this->_userFactory = $userFactory;
}

public function execute()
{
    $user = $this->userFactory->create();
    $user->setData(
        [
            'username'          => 'user.name',
            'firstname'         => 'Forename',
            'lastname'          => 'Surname',
            'email'             => 'admin@test.com',
            'password'          => 'badpassword123',       
            'interface_locale'  => 'en_US',
            'is_active'         => 1
        ]
    );
    
    $user->setRoleId(1);
    $user->save(); 
}
```

---
### 6.0. Customising Magento Business Logic
This section covers **16%** of the exam.

#### 6.1. Product Types

| Product Type | Description |
| :----------: | :---------: |
| Simple | A simple product is a physical item with a single SKU. Simple products have a variety of pricing and of input controls which makes it possible to sell variations of the product. Simple products can be used in association with grouped, bundle, and configurable products. |
| Configurable | A configurable product appears to be a single product with lists of options for each variation. However, each option represents a separate, simple product with a distinct SKU, which makes it possible to track inventory for each variation.  |
| Grouped | A grouped product presents multiple, standalone products as a group. You can offer variations of a single product, or group them for a promotion. The products can be purchased separately, or as a group. |
| Bundle | A bundle product let customers “build their own” from an assortment of options. The bundle could be a gift basket, computer, or anything else that can be customized. Each item in the bundle is a separate, standalone product. |
| Virtual | Virtual products are not tangible products, and are typically used for products such as services, memberships, warranties, and subscriptions. Virtual products can be used in association with grouped and bundle products. |
| Downloadable | A digitally downloadable product that consists of one or more files that are downloaded. The files can reside on your server or be provided as URLs to any other server. |
| Gift Card | There are three kinds of gift cards: virtual gift cards which are sent by email, physical gift cards which are shipped to the recipient, and combined gift cards which are a combination of the two. Each has a unique code, that is redeemed during checkout. Gift cards can also be included in a grouped product. |

---
> How would you obtain a product of a specific type? 

To obtain a specific product type, build search criteria and retrieve them from the product repository:
```PHP
use Magento\Catalog\Api\Data\ProductInterface;
use Magento\Bundle\Model\Product\Type;

...

public function getBundleProducts()
{
    $searchCriteria = $this->searchCriteriaBuilder
        ->addFilter(ProductInterface::TYPE_ID, Type::TYPE_CODE)
        ->create();
    
    return $this->productRepository->getList($searchCriteria)->getItems();
}
```

---
> What tools (in general) does a product type model provide?

The product type model is responsible for:
- Loading and configuring product options
- Preparing the product for the cart
- Processing product data to/from the database
- Checking whether the product can be sold
- Loading child products, when applicable

---
#### 6.2. Category Properties
> How do you create and manage categories?

---
#### 6.3. Product/Category Relations
> How do you assign and unassign products to categories?

Navigate to `Catalog > Inventory > Products` in Magento Admin, select the product you wish to add to a category, and use the `Categories` drop down selection options to assign it to categories.

---
#### 6.4. Product Behaviour In The Cart
> How are configurable and bundle products rendered?

`view/frontend/layout/checkout_cart_item_renderers.xml` define how products are rendered in the cart.

Configurable products are shown as a single line item. It is shown with the parent product's title and the simple product option selected by the customer.

Each product in the Bundle product is rendered as a single line item.

---
> How can you create a custom shopping cart renderer?

---
#### 6.5. Native Shipping Functionality
> How do you customize the shipment step of order management?

---
#### 6.6. Customer Account Area Customisation
> How would you add another tab in the “My Account” section?

To add an additional menu tab in the customer "My Account" area create the layout file in `Your_Company/Your_Module/view/frontend/layout/customer_account.xml`:
```XML
<body>
    <referenceContainer name="content">
        <referenceBlock name="customer_account_navigation">
            <block class="Magento\Customer\Block\Account\SortLinkInterface" 
                   name="yourBlockName"
                   after="customer-account-navigation-address-link">
                <arguments>
                    <argument name="label" xsi:type="string" translate="true">
                        Your Label
                    </argument>
                    <argument name="path" xsi:type="string">
                        path/to/your/module/view/
                    </argument>
                    <argument name="sortOrder" xsi:type="number">
                        10
                    </argument>
                </arguments>
            </block>
        </referenceBlock>
    </referenceContainer>
</body
```

---
> How do you customize the order history page?

To customise the "Order History" page create the layout file in `Your_Company/Your_Module/view/frontend/layout/sales_order_history.xml`. The `sales.order.history.info` container is a common location for modifications to be made.

---
#### 6.7. Add/Modify Customer Attributes
> How do you add or modify customer attributes in a setup script?

Create `Your_Company/Your_Module/Setup/UpgradeData.php` with an `upgrade()` function:
```PHP
use Magento\Customer\Model\Customer;

// ...

$attribute = $this->eavConfig->getAttribute(Customer::ENTITY, Attribute::CUSTOMER_PROMOTION_PREFERENCE);
$attribute->setData('used_in_forms', ['adminhtml_customer', 'customer_account_edit']);
$attribute->save();
```

---
#### 6.8. Customising Customer Addresses
> How do you add another field to the customer address entity using a setup script?

---
## References
### Magento 2 Dev Docs
[Component Load Order](https://devdocs.magento.com/guides/v2.3/extension-dev-guide/build/module-load-order.html)

[Name Your Component](https://devdocs.magento.com/guides/v2.3/extension-dev-guide/build/create_component.html)

[Create Your Component File Structure](https://devdocs.magento.com/guides/v2.3/extension-dev-guide/build/module-file-structure.html)

[Plugins (Interceptors)](https://devdocs.magento.com/guides/v2.3/extension-dev-guide/plugins.html)

[About Magento Modes](https://devdocs.magento.com/guides/v2.3/config-guide/bootstrap/magento-modes.html)

[Layout Instructions](https://devdocs.magento.com/guides/v2.3/frontend-dev-guide/layouts/xml-instructions.html)

[Product Types](https://docs.magento.com/m2/ee/user_guide/catalog/product-types.html)

[Declarative Schema](https://devdocs.magento.com/guides/v2.3/extension-dev-guide/declarative-schema/)
