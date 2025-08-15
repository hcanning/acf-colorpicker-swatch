# acf-colorpicker-swatch
Create a Swatch only field for use in ACF

Create a Radio or Select Field in ACF called "brand_color"

Add your palette e.g:
```
#1A3C5A : Navy Blue
#8B1E3F : Burgundy
#D4A017 : Gold
#C91732 : Red
#E8ECEF : Light Gray
#FFFFFF : White
#333333 : Dark Gray
```

In your site css e.g. custom.css add:
```

/* Swatch styling for ACF radio field */
.acf-field-brand_color .acf-radio-list label {
  display: inline-block;
  cursor: pointer;
}

.acf-field-brand_color .acf-radio-list input {
  display: none; /* hide radio button circles */
}

.acf-field-brand_color .acf-radio-list label span {
  display: inline-block;
  width: 32px;
  height: 32px;
  border-radius: 50%;
  margin: 4px;
  border: 2px solid transparent;
  box-sizing: border-box;
}

/* Selected state */
.acf-field-brand_color .acf-radio-list input:checked + span {
  border-color: #000;
}
```

Add this to your `functions.php` in WP
```
// Add swatch class to any field whose name starts with a prefix
add_action('acf/input/admin_footer', function() { ?>
<style>
.acf-swatch-field .acf-radio-list-wrapper {
    display: flex;
    align-items: center;
    gap:16px;
}
.acf-swatch-field .acf-radio-list {
    display:flex; flex-wrap:wrap; gap:8px; margin:0; padding:0; list-style:none;
}
.acf-swatch-field .acf-radio-list li { display:inline-flex; margin:0; padding:0; }
.acf-swatch-field .acf-radio-list input[type="radio"] { display:none; }
.acf-swatch-field .acf-radio-list label {
    display:block; width:32px; height:32px; border-radius:50%;
    border:1px solid #dedede; cursor:pointer; text-indent:-9999px;
    overflow:hidden; box-sizing:border-box;
}
.acf-swatch-field .selected-color {
    display:inline-flex; align-items:center; gap:6px; font-weight:bold; font-size:13px;
}
.acf-swatch-field .selected-color .swatch {
    width:20px; height:20px; border-radius:50%; border:1px solid #dedede; display:inline-block;
}
</style>

<script>
(function($){
acf.add_action('ready append', function($el){
    $('.acf-swatch-field', $el).each(function(){
        var $field = $(this);
        var $radioList = $field.find('.acf-radio-list');

        if(!$field.find('.acf-radio-list-wrapper').length){
            $radioList.wrap('<div class="acf-radio-list-wrapper"></div>');
        }
        if(!$field.find('.selected-color').length){
            $field.find('.acf-radio-list-wrapper')
                  .append('<div class="selected-color">Selected Color: <span class="swatch"></span></div>');
        }
        var $selected = $field.find('.selected-color .swatch');

        $radioList.find('li').each(function(){
            var $li = $(this);
            var $input = $li.find('input[type="radio"]');
            var $label = $li.find('label');
            var val = $input.val() ? $input.val().trim() : '';
            if(!val) return;

            $label.css('background-color', val);

            if($input.is(':checked')) $selected.css('background-color', val);

            $input.off('change.swatch').on('change.swatch', function(){
                var selectedVal = $radioList.find('input:checked').val();
                if(selectedVal) $selected.css('background-color', selectedVal.trim());
                else $selected.css('background-color', 'transparent');
            });
        });
    });
});
})(jQuery);
</script>
<?php });
```


See Screenshot: (https://github.com/hcanning/acf-colorpicker-swatch/blob/main/swatch-circles.png)

![alt text](https://github.com/hcanning/acf-colorpicker-swatch/blob/main/swatch-circles.png "Screenshot")

Clone Field: https://www.advancedcustomfields.com/resources/clone/

![alt text](https://github.com/hcanning/acf-colorpicker-swatch/blob/main/clone-fields.png "Clone Field")


