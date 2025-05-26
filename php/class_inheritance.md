



```php
<?php
use App\Projects\Models\Project;

// Вывод всех методов класса Project
$methods = get_class_methods(Project::class);
print_r($methods);

```
