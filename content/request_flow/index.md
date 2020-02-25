[ - - -  Go to Index - - - ](/magento2-associate-developer-guide)

### 2.0 Request Flow Processing
This section covers **7%** of the exam.

- [2.1. Describe Magento 2 Modes](#21-describe-magento-2-modes)
- [2.2. Demonstrate Ability To Use Frontend Controllers](#22-demonstrate-ability-to-use-frontend-controllers)
- [2.3. URL Rewrites](#23-url-rewrites)

---
#### 2.1. Describe Magento 2 Modes
> Understand the pros and cons of using developer mode or production mode.

##### Production
This mode should be used on your live production server. Production mode leads to an increase in performance by providing all necessary static files at the time of deployment rather than requiring Magento to dynamically locate and create static files during run time.

In this mode Magento:
- Logs exceptions and **does not** show exceptions on the frontend
- Serves static view files from the cache **only**
- Prevents automatic code compilation meaning that new or updated files are **not** written to the file system
- Prevents you from enabling or disabled cache types via Magento Admin

##### Developer
As it's name suggests, this mode is intended to be used during development only. When in this mode, Magento:
- Disables static view file caching instead writing them to `pub/static` every time they are called
- Provides verbose logging in `var/report`
- Enables automatic code compilation
- Displays uncaught exceptions to the frontend
- Enables enhanced debugging
- Shows custom `x-magento-*` HTTP request and response headers
- Performs slower

---
> How do you enable/disable maintenance mode?

Maintenance mode can be enabled and disabled via the command line:
```
# enable maintenance mode
bin/magento maintenance:enable

# disable maintenance mode
bin/magento maintenance:disable
```

---
#### 2.2. Demonstrate Ability To Use Frontend Controllers with different response types (HTML / JSON / redirect)
The FrontController class class searches through a list of routers, provided by the RouterList class, until it matches one that can process a request. When the FrontController finds a matching router, it dispatches the request to an action class returned by the router.

If the FrontController cannot find a router to process a request, it uses the default router.
##### frontend area routers:

- 1 - robots
- 2 - urlrewrite	
- 3 - standard
- 4 - cms
- 5 - default

##### adminhtml area routers:
- 1 - admin
- 2 - default	


> How do you identify which module/controller corresponds to a given URL?

##### Frontend
Example url: `https://domain.com/index.php/frontname/controller/action`
File: `app/code/Vendor/ModuleName/etc/frontend/routes.xml`
````
<?xml version="1.0" ?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:App/etc/routes.xsd">
    <router id="standard">
        <!--frontName is the name in the front and the id is the layout handle reference -->
        <route frontName="helloworld" id="helloworld">
            <module name="Vendor_ModuleName"/>
        </route>
    </router>
</config>
````

##### Adminhtml
Example url: `https://domain.com/index.php/admin/frontname/controller/action`
File: `app/code/Vendor/ModuleName/etc/adminhtml/routes.xml`
````
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:App/etc/routes.xsd">
    <router id="admin">
        <route id="helloworld" frontName="helloworld">
            <module name="Vendor_ModuleName"/>
        </route>
    </router>
</config>
````
---
> What would you do to create a given URL?
##### From admin panel
Set the Category and Product `url_key` attributes. You can also create URL rewrites through the Magento Admin path `Marketing > SEO & Search > URL Rewrites`
#### See the point above

---
#### 2.3. URL Rewrites
URL rewrites provide a user-friendly URL to the customer in place of a cumbersome Magento URL. These values are stored in the `url_rewrite` table.

---
> How is the user-friendly URL of a product or category defined?

The product/category `url_key` attribute defines their URL. For each category a product belongs to, Magento will generate a URL based on the category tree before appending the product's `url_key`.

---
> How can you change a URL?

You can change a URL by creating a URL rewrite. The following actions also cause a URL to change:
- Updating a category's `url_key` attribute
- Updating a product's `url_key` attribute
- Modifying the categories a product belongs to

---
> How do you determine which page corresponds to a given user-friendly URL?

In the `url_rewrite` table you will find a row where the `request_path` value is the user-friendly URL. The corresponding `target_path` value is the internal Magento page. The `Magento_UrlRewrite` module contains a router that checks to see whether the given URL can be matched to a `request_path` in the `url_rewrite` table, redirecting to the `target_path` if a match is found.

[ - - -  Go to Index - - - ](/magento2-associate-developer-guide)
