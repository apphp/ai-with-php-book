# Unstructured Data Formats ?...

Now let's check unstructured data formats commonly used in AI applications.

### Unstructured Data Formats

Unstructured data lacks a predefined data model or organization. It's often text-heavy but can also include dates, numbers, and facts. Here are some common types of unstructured data in AI:

#### Text Processing and Natural Language Data&#x20;

This includes free-form text like articles, social media posts, or customer reviews.

Considerations:

* Tokenization and text normalization
* Handling multiple languages
* Dealing with misspellings and informal language

PHP example (basic text processing):

```php
$text = "Hello, world! This is a sample text.";
$words = str_word_count($text, 1);
$word_count = count($words);
```

#### Image Data Formats&#x20;

Common formats include JPEG, PNG, TIFF, and raw image data.

Considerations:

* Color space (RGB, CMYK, grayscale)
* Resolution and file size
* Metadata (EXIF)

PHP example (using GD library):

```php
$image = imagecreatefromjpeg('image.jpg');
$width = imagesx($image);
$height = imagesy($image);
```

#### Audio Data Considerations Formats&#x20;

Common formats include WAV, MP3, AAC, and raw audio data.

Considerations:

* Sampling rate and bit depth
* Mono vs. stereo
* Compression (lossy vs. lossless)

PHP example (using getID3 library):

```php
require_once 'getid3/getid3.php';
$getID3 = new getID3;
$file_info = $getID3->analyze('audio.mp3');
$duration = $file_info['playtime_seconds'];
```

#### Video Data Considerations&#x20;

Common formats include MP4, AVI, MOV, and raw video frames.

Considerations:

* Frame rate and resolution
* Codec (e.g., H.264, VP9)
* Handling large file sizes

PHP example (using FFmpeg):

```php
$ffmpeg = FFMpeg\FFMpeg::create();
$video = $ffmpeg->open('video.mp4');
$duration = $video->getStreams()->first()->get('duration');
```

When working with unstructured data in AI applications, it's often necessary to preprocess the data to extract meaningful features or convert it into a structured format that machine learning algorithms can work with.

In conclusion, choosing the right data format depends on your specific use case, the nature of your data, and the requirements of your AI application. PHP provides various built-in functions and third-party libraries to work with these different data formats, allowing you to effectively manage and process both structured and unstructured data in your AI projects.



To help you better understand and visualize the differences between various data formats,\
json

```
[
  {"name": "Alice", "age": 30, "city": "New York"},
  {"name": "Bob", "age": 25, "city": "San Francisco"},
  {"name": "Charlie", "age": 35, "city": "London"}
]
```

csv

```
name,age,city
"Alice",30,"New York"
"Bob",25,"San Francisco"
"Charlie",35,"London"
```

