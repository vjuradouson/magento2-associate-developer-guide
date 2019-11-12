[ - - -  Go to Index - - - ](/magento2-associate-developer-guide)

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

[ - - -  Go to Index - - - ](/magento2-associate-developer-guide)