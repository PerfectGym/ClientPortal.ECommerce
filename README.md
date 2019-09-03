# E-commerce embedding guide

You can embedd E-commerce module to be a part of your own site.

## DEMO

[Basic](https://perfectgym.github.io/ClientPortal.ECommerce/)

[Redirecting from payment gate back to your page after payment](https://perfectgym.github.io/ClientPortal.ECommerce/returnUrl)

[Narrowing flow config to single club](https://perfectgym.github.io/ClientPortal.ECommerce/singleClub)

[Basic demo without usage of external liblaries, that shows only code required for embeding)](https://perfectgym.github.io/ClientPortal.ECommerce/plain)

## Find ecommerce flow you want to embed
Go the the list of configured flows:

![ECommerce Flow List](./public/img/ECommerceFlowList.png)

Go to the URL of the flow you want to embed.

## Open ecommerce HTML document
Open the document and check HTML that we generated (with devtools or some other way).


You should get something like this (comments are for the sake of this guide):
```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">

    <!-- That's our internal monitoring, please ignore -->
    <script type="text/javascript">window.NREUM||(NREUM={});NREUM.info = {"...</script>

    <title>Client Portal ECommerce</title>
    <meta name="description" content="">
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <!-- Styles -->
    <link href="https://thunder.perfectgym.pl/ClientPortal2/Assets/D65A30004CA147ECD3A8/common.css" rel="stylesheet">
    <link href="https://thunder.perfectgym.pl/ClientPortal2/Assets/D65A30004CA147ECD3A8/ecommerce.css" rel="stylesheet">
</head>

<body>
    <!-- Container -->
    <div id="main"></div>

    <!-- Scripts -->
    <script src="https://thunder.perfectgym.pl/ClientPortal2/Assets/D65A30004CA147ECD3A8/vendor.js" type="text/javascript"></script>    
    <script src="https://thunder.perfectgym.pl/ClientPortal2/Assets/D65A30004CA147ECD3A8/pl/common.js" type="text/javascript"></script>
    <script src="https://thunder.perfectgym.pl/ClientPortal2/Assets/D65A30004CA147ECD3A8/pl/ecommerce.js" type="text/javascript"></script>

    <!-- Initialization script -->
    <script>
        PerfectGym.ECommerce.init({
            flowId: 'ed65ff14c',
            element: '#main'
        });
    </script>
</body>

</html>
```

You need to include styles and scripts as in the HTML.
Those hashes are important and they are unique for your database, so please check what are your asset URLs.

There are 2 styles (`common`, `ecommerce`) and 3 scripts (`vendor`, `common`, `ecommerce`). You need to include all of them.
In the top of the page there is also some generated script, that you should simply ignore - that's our app monitoring scripts.

Two of the 3 scripts are localizable, so you need to choose which language do you want to display.
It's not possible to change language after loading a page, you need to include different scripts. This is a tradeoff to achieve best performance.
As you can see in the example above, we load scripts in polish here (pl). Change it to language of your choice, that is enabled for your company.
In case of doubts, contact our support.

## Initialize the ecommerce
Run the initialization script. It has several parameters:

* `flowId` (required) - id of the e-commerce flow, this is always a string, you can find it in URL
* `element` (required) - html element or CSS selector to render the ecommerce
* `clubId` (optional)- if you have multiple clubs, you can force to choose a specific one
* `returnUrl` (optional) - URL that you want to redirect to after the transaction is complete successfully or failed

## Usage in SPA applications
If you have SPA application, and you don't reload the page when navigating, you will need to remember to destroy the ecommerce instance when is not needed anymore.

`PerfectGym.ECommerce.init` returns a promise, which resolves with initailized instance of ecommerce flow. To destroy it invoke `$destroy()` method.

For example, for AngularJS application it would look something like this:
```js
 window.PerfectGym.ECommerce.init({
    flowId: 'ed65ff14c',
    element: '#main'
}).then(vm => {
    $scope.$on('$destroy', () => vm.$destroy());
});
```

## Add your domain to allowed domains
Embedding e-commerce uses CORS. We have adapted proper security measures to make it safe, so we have to add your domain to the list of allowed ones.

To do that, please contact our support team.

The setting responsible for that is `ClientPortal.AllowedDomains`.

## Remarks
Do not try to change styles of e-commerce. Class names are not deterministic and may change, so you can break your application.
We are constantly developing the application and make big changes at regular basis, so we cannot provide you a way to use your own CSS without risk of breaking the design.

Because we want to achieve best user experience and performance, we decided to not use IFRAMEs or anything like that, but to render the component directly on client site.
This is best for resposiveness, animations and overall experience.

But this approach causes that, you can by accident override our styles, which may result in breaking the appereance of the application once in a while.
We make our best to secure our styles against such interferences, but it's possible that we miss something.

In case you would see some UI errors, please contact our support, we will fix it ASAP.