# Get Config Value In Js File


![logo](https://docsify.js.org/_media/icon.svg ':size=50x100')

**Need to add config value in phtml**


- *In phtml file*


 ```bash
<script>
    window.customConfig = <?php /* @escapeNotVerified */ echo \Zend_Json::encode($block->getCustomConfig()); ?>;
</script>

 ```

 - *In Block file*

  ```bash
	public function getCustomConfig()
	    {
	        return $config = [
	    		'name' => 'John Doe',
	    		'Email' => 'john@webkul.com',
	    		'DOB' => '08/05/1990',
	    	];
	    }
 ```

  - *result in js file*


 ```bash

	console.log(window.customConfig);

 ```

 -*open console you will get the value like this*

 > {name: "John Doe", Email: "john@webkul.com", DOB: "08/05/1990"}


  -*also here a reference link*

  > https://webkul.com/blog/create-custom-js-window-configproviders-magento-2/