Title: Custom Fields
Description: You can create your own custom fields.

---

## Make a Custom Field

To make a custom field make the directory `app/Fields`. This is where you will place all of your custom fields. Here is a custom field named `Age`.

```php
<?php
namespace App\Fields;

use TypeRocket\Elements\Fields\Field;
use TypeRocket\Html\Html;

class Age extends Field
{
    protected function init() {
        $this->setType( 'number' );
    }

    public function getString() {
        $name = $this->getNameAttributeString();
        $value = (int) $this->getValue();
        $attr = $this->getAttributes();

        return (string) Html::input('number', $name, $value, $attr );
    }
}
```

## Applying Custom Fields to Forms

Then to use the new field with a `Form` object pass it into the constructor of a new instantiated field. You can pass the `Form` object anywhere in the constructor you like - typically as the last parameter regardless of the number of other arguments you use. TypeRocket knows exactly how to sync the form to the field no matter where it is placed in the constructor.

```php
$form = tr_form();
$age = new \App\Fields\Number('Age');
echo $form->custom($age); // Outputs field
```

