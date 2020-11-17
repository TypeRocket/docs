Title: Widgets
Description: Add custom fields to widgets.

---

*Pro Only: This is a Pro only extension feature.*

## Getting Started

To create custom widgets with fields, you will need to extend the `\TypeRocketPro\Register\BaseWidget` class. 

```php
class TR_Widget extends \TypeRocketPro\Register\BaseWidget {

    /**
     * Sets up the widgets name etc
     */
    public function __construct() {
        parent::__construct( 'tr_widget', 'Tr Widget', [
            'classname' => 'Tr_Widget',
            'description' => 'Tr Widget is awesome'
        ] );
    }

    public function backend($fields)
    {
        echo $this->form->text('Title');
        echo $this->form->date('Date');
    }

    public function frontend($args, $fields)
    {
        // make frontend code
    }

    public function save($new_fields, $old_fields)
    {
        // You will want to sanitize your $new_fields data 
        return $new_fields;
    }
}
```

Then register it in WordPress.

```php
add_action( 'widgets_init', function(){
    register_widget( 'TR_Widget' );
});
```