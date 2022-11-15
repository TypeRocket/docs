Title: Upgrade Guide v5 to v6
Description: Upgrading TypeRocket v5 to v6

---

## Upgrading From v5 to v6

If you are coming from TypeRocket Pro v5 upgrading should take 15 minutes depending on your project size. When upgrading you will need to [access your account](/account/) and upgrade your license once you have completed the upgrade process.

**Only once you have a working upgrade for all of your sites** go to "Account -> View Licenses -> View Upgrades" and upgrade to a **TypeRocket Pro v6 – Antennae** license of the correct size from there for free.

After and before upgrading, you will have access to both `v5` and `v6`. However, your license will only give you automatic updates for your current license type.

## TypeRocket Pro Namespace Change

Update all instances of `TypeRocketPro` to `TypeRocket\Pro`. For example, if you are accessing the class `TypeRocketPro\Elements\Fields\Gallery` you must now call the class `TypeRocket\Pro\Elements\Fields\Gallery`. It is NOT recommended that you run a find and replace. Find and replace will not account for escapping needed withing strings. For example, a find and replace of `TypeRocketPro` to `TypeRocket\Pro` on these strings will cause errors:

```
# Targets
"TypeRocketPro\\Elements\\Fields\\Gallery"
'TypeRocketPro\Elements\Fields\Gallery'

# Results
"TypeRocket\Pro\\Elements\\Fields\\Gallery" 
    -> TypeRocketPro\Elements\Fields\Gallery
    
'TypeRocket\Pro\Elements\Fields\Gallery' 
    -> TypeRocket\Pro\Elements\Fields\Gallery
```

## Composer Install Changes

If you are using other packages ensure they are PHP 8 compatible. Next, update the PHP version to `^8.0.2`. Next, update `typerocket/core` and `typerocket/professional` to `^6.0`.

```
"require": {
    "php": "^8.0.2",
    "typerocket/core": "^6.0",
    "typerocket/core": "^6.0",
    ....
}
```

## Other Package Updates

- Symfony PHP Console have been update from `5.1` to `6.0`.
- Whoops PHP has been update from to `2.5` to `2.14`.

