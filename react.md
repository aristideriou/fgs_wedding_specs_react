# FGS Wedding React App

## General notes

All values followed by comments (//) are just examples, and should be populated according to instruction in the comments.

Typing of values (string, integer, boolean) should strictly follow the specs.

If a key is not relevant in a given context, don't insert it in the dataLayer (no empty string, or key with no value...)

## Google Tag Manager + OneTrust bundle ==> No change with NOE

As high as possible in the ```<head>``` tag :

```html
<!-- Cookies Consent Notice, DO NOT REMOVE, contact: analytics@royalcanin.zendesk.com -->
<script src="https://cdn.cookielaw.org/scripttemplates/otSDKStub.js" data-document-language="true" type="text/javascript" charset="UTF-8" data-domain-script="eed53fb1-5baf-4bae-bff4-d2219117c884" ></script>
<script type="text/javascript">function OptanonWrapper() { }</script>
<!-- End Cookies Consent Notice -->

<!-- Global RC script, DO NOT REMOVE, contact: analytics@royalcanin.zendesk.com -->
<script type="text/javascript" src="https://rcdfcdn.mars.com/consent-management/global-script.js" id="global-script"></script>
<!-- End Global RC script -->
```

Anywhere in the ```<body>``` tag :

<!-- OneTrust Cookies Settings button start -->
<button id="ot-sdk-btn" class="ot-sdk-show-settings"> Cookie Settings</button>
<!-- OneTrust Cookies Settings button end -->"

## Previous implementation cleanup

Delete all calls to former specific FGS GTM Containers
Container IDs are GTM-PMW3VKL for FR,GTM-MH74J3M for RU, GTM-N8PCR2P for TR, GTM-TML5H48 for US.

Remove all previous dataLayer.push calls : the following specs are meant to 

## Generic React component tracking

Everytime a React component is loaded, populate the data layer the following way :

```javascript
dataLayer.push({
    'event':'reactComponentLoad',
    'reactStore': {
        'key1' : 'value1', //JS object with key / values of the apps state
        'key2' : 'value2' //JS object with key / values of the apps state
        //etc...
        } 
});
```

This instruction is meant to help us having more autonomy in GTM with to access easily React info. If it is too costy in terms of performance / reliability, feel free to reach us to find a tradeoff

### Checkout - Initial products array setup

When checkout step 1 is loaded, **before** any other dataLayer.push instruction (following points), feed the dataLayer with products information

```javascript
dataLayer.push({
    'products' : [
        {
            'price' : 40, //Product Price
            'productName' : '', //Coming from WeShare
            'productID' : '123456789' //Product SKU
            'recomandation' : 'recomended'
            'productBreed' : 
            'petAge' : 
            'neuteredFood' : 
            'pettype' : 
            'productSize' : 
            'specialNeeds' : 'diabetes', //Product special need if
            'productBrand' : 'Royal Canin', //Royal Canin or Eukanuba
            'guestOrder': 'yes' //'yes' or 'no' depending if guest transaction
            '
        },
        {

        }
        //One JS object for each product currently in a user basket
    ]
})
```

Word of caution : this object should never be overwritten or modified in the context of the checkout, as it is to be used through the entire process (checkout steps, c)

It also has to be populated **before** the checkout steps tracking ('event':'checkoutStep')

### Checkout - steps

When a checkoutStep is loaded

```javascript
dataLayer.push({
    'event':'checkoutStep',
    'checkoutStepName' : 'Email', //Following values possible : 'Email', 'Delivery', 'Payment', 'Confirmation'
});
```

### Checkout - Order confirmation

```javascript
dataLayer.push({
    'event':'orderConfirmation',
    'transaction' :{
        'id' : 123456789,
        'paymentMethod' : 'Credit Card', //Could be optionnal if no other payment method available

        'Currency' : 'euro',//cf. List Google doc
        'Amount' : 120, //Transaction amount
        'Taxes' : 12, //Taxes amount
        '' : 'One Shot' //One shot vs subscription vs Club 
        'quantity' : 1 //Number of products in the transaction
        'Delivery frequency (if subscription)' : 
        'Guest / logged transaction' : 
        'Promo code (yes / no)' : 
        'Transaction amount' : 
        'Recommendation?' : 
        'Guest vs logged' : 
    }
});
```

### Checkout - FAQ clicks

```javascript
dataLayer.push({
    'event':'faqClick',
    'faqClickItem': 'How to reach customer service' //Generic name in English for each item
});
```

---------------

### Product detail - Product info tabs clicks

```javascript
dataLayer.push({
    'event':'pdpTabsClick',
    'pdpTabsClickTabName': 'itemName'
});
```

### Product detail - Product page load

```javascript
dataLayer.push({
    'event':'pdpScreenLoad',
    'pdpScreenLoadProducts': {
        'productPrice'
    } 
});
```

---------------

### Product list page (PLP)

Product displayed in the page when PLP is loaded : 

```javascript
dataLayer.push({
    'event':'plpScreenLoad',
    'plpNbResults':50, //Number of total items from the request
    'pdpScreenLoadProducts': {
        'productPrice'
    } 
});
```

Filter clicks


### My account

Screens

```javascript
dataLayer.push({
    'event' : 'myAccountScreen',
    'myAccountScreenName' : 'Overview', //Values : 'Overview', 'Personal information', 'Pets', 'Orders & Subscriptions', 'Payment & Addresses', 'Security', 'Data & Settings'
})
```

Interactions

```javascript
dataLayer.push({
    'event' : 'myAccountAction',
    'myAccountActionName' : 'Add picture', 
    //Values : 'Add picture', 'Edit profile info', 'Edit contact info', 'Add pet', 'Remove pet', 'Download Invoice', 'Cancel Subscription', 'Add payment Method', 'Delet payment method', 'Add Address', 'Delete Address', 'Change email', 'Change password', 'Delete Account'
})
```

## Deployment

When immplemented and validated in a staging environment, these specs should be deployed in cunjunction with a Google Tag Manager deployment.

## Contact

For any questions regarding this tagging plan, please contact analytics@royalcanin.zendesk.com