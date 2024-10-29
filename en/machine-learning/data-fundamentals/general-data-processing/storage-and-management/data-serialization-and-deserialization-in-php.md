# Data serialization and deserialization in PHP

### Data Serialization in PHP

#### JSON Processing

```php
phpCopy// Example of JSON handling with error checking
function safeJsonEncode($data) {
    $json = json_encode($data, JSON_THROW_ON_ERROR);
    return $json;
}

function safeJsonDecode($json) {
    $data = json_decode($json, true, 512, JSON_THROW_ON_ERROR);
    return $data;
}
```

#### XML Processing

```php
phpCopy// Example of SimpleXML usage
function parseXMLDocument($xmlString) {
    try {
        $xml = new SimpleXMLElement($xmlString);
        return $xml;
    } catch (Exception $e) {
        error_log("XML parsing error: " . $e->getMessage());
        return false;
    }
}
```

#### Custom Serialization

```php
phpCopy// Example of custom serialization for AI models
class ModelSerializer {
    public function serialize($model) {
        return base64_encode(serialize($model));
    }
    
    public function unserialize($serializedModel) {
        return unserialize(base64_decode($serializedModel));
    }
}
```

###
