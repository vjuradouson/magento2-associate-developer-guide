[ - - -  Go to Index - - - ](/magento2-associate-developer-guide)

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

[ - - -  Go to Index - - - ](/magento2-associate-developer-guide)