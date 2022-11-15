Title: Page Builder Plus for TypeRocket Pro
Description: Additional feature for the TypeRocket page builder. 

---

!!

## Page Builder Plus

! **IMPORTANT**: In beta phase, only works with the `App\Models\Page` model.

The Pro package includes a Page Builder Plus extension that adds additional capabilities to the basic Page Builder. You must have the Page Builder extension turned on to use this extension. The new features include:

- Blocks system and block component.
- Page builder revisions.
- Clone component to block.

You can [download the list of official page builder component thumbnail icons](https://typerocket.com/Page-Builder-Component-Thumbnails.zip).

### Installation

1. Update your App Config file adding to the extensions list: `\TypeRocketPro\Extensions\PageBuilderPlus\PageBuilderPlus`
2. Run the Pro package galaxy publish script: `php galaxy extension:publish typerocket/professional page-builder-plus --mode=publish`

And you're almost done installing.

The above steps implements the required `Page` model trait and runs the needed migrations so your admin is exposed to the additional functionalities and blocks.

### Block Component

The Page Builder Plus package comes with an additional builder Block component. Modify your component's config file and add the following to the list of components `'block' => TypeRocketPro\Extensions\PageBuilderPlus\Components\Block::class`. Next, add the `block` option to the builder component settings list just below the registry.

### Builder Revisions Usage

The Page Builder Plus package comes with builder revisions. With it, you can see the history of your component changes. There is additional steps involved to get the revisions included.

1. You need to include the BuilderRevisions trait in your applicable `Page` model class. `use TypeRocketPro\Extensions\PageBuilderPlus\Traits\BuilderRevisions;`