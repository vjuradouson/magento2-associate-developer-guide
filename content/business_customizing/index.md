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