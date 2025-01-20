# Unstructured Data Formats

Now let's check unstructured data formats commonly used in AI applications.

### Unstructured Data Formats

Unstructured data lacks a predefined data model or organization. It's often text-heavy but can also include dates, numbers, and facts. Here are some common types of unstructured data in AI:

#### Text Processing and Natural Language Data&#x20;

<div align="left"><figure><img src="../../../../.gitbook/assets/ml-data-type-txt-min (1).png" alt="" width="104"><figcaption></figcaption></figure></div>

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

#### Image Data Formats

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

#### Audio Data Considerations Formats

<div align="left"><figure><img src="../../../../.gitbook/assets/ml-data-type-aud-min.png" alt="" width="105"><figcaption></figcaption></figure></div>

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

<div align="left"><figure><img src="../../../../.gitbook/assets/ml-data-type-vid-min (1).png" alt="" width="102"><figcaption></figcaption></figure></div>

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

### Unstructured Data Formats

Unlike structured data (like databases or JSON), unstructured data doesn't follow a predefined model. Here are common types with examples:

#### 1. Plain Text Documents

```
To: Marketing Team
Subject: Q4 Campaign Ideas

Just wanted to jot down some quick thoughts about our upcoming holiday campaign. 
I think we should focus on user-generated content this year. Our customers' 
stories are compelling and authentic.

Key points to consider:
- Instagram engagement is up 23%
- Video content performs best between 2-3 minutes
- User testimonials get 3x more shares

Let's discuss this in tomorrow's meeting.

Best regards,
Sarah
```

#### 2. Email Data (Raw Format)

```
Received: from mail.server.com
        by mx.google.com with ESMTPS id a12bc3d4e5
        for <recipient@email.com>
        (version=TLS1_3 cipher=TLS_AES_128_GCM_SHA256);
        Wed, 23 Oct 2024 10:15:22 -0700 (PDT)
From: sender@email.com
To: recipient@email.com
Subject: Meeting Notes
Content-Type: text/plain; charset="UTF-8"
Content-Transfer-Encoding: quoted-printable

Meeting highlights:
1. Project timeline updated
2. New requirements added
3. Budget approved
```

#### 3. Log Files

```
[2024-10-23 08:12:33.123] [ERROR] server.js - Failed to connect to database
[2024-10-23 08:12:33.124] [INFO] app.js - Retrying connection (attempt 1 of 3)
[2024-10-23 08:12:34.231] [DEBUG] auth.js - User session expired: user_id=12345
[2024-10-23 08:12:34.232] [WARN] cache.js - Cache miss for key: "user_preferences"
[2024-10-23 08:12:35.410] [INFO] app.js - Database connection established
```

#### 4. Social Media Content

```
Tweet: Just discovered an amazing coffee shop in downtown! 😍 The barista made the
perfect latte art #CoffeeLife #DowntownVibes
Location: Downtown District
Timestamp: 2024-10-23 09:15 AM
Likes: 42
Retweets: 7
Replies: [
    "Where is this? I need to check it out! ☕️",
    "Their pastries are amazing too! 🥐",
    "Been going there for years, best coffee in town"
]
```

#### 5. Medical Records (Free Text)

```
Patient Progress Notes:
Date: 10/23/2024
Time: 14:30

Patient presents with persistent cough for past 3 days. Reports mild fever 
and fatigue. No shortness of breath. Current medications include antihistamine
for seasonal allergies. Lungs clear on auscultation. No sinus tenderness.

Assessment: Likely upper respiratory viral infection
Plan: Symptomatic treatment, rest, fluids. Return if symptoms worsen or fever
persists beyond 5 days.

Signed: Dr. Smith
```
