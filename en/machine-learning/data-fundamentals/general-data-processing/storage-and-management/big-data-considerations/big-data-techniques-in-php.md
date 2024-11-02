# Big Data Techniques in PHP

## Comprehensive Guide to Big Data Techniques in PHP

### 1. Handling Large-scale Datasets

#### A. Chunked Processing

Chunked processing is crucial when dealing with datasets that are too large to fit in memory. This technique involves processing data in smaller, manageable pieces.

**Detailed Implementation:**

<details>

<summary>Example of Chunked Processor</summary>

```php
class ChunkedProcessor {
    private $chunkSize;
    private $maxMemoryUsage;
    private $logHandler;

    // Adjusted the constructor to avoid type errors
    public function __construct($chunkSize = 1000, $maxMemoryUsage = '256M', LogHandler $logHandler = null) {
        $this->chunkSize = $chunkSize;
        $this->maxMemoryUsage = $maxMemoryUsage;
        $this->logHandler = $logHandler ?? new LogHandler(); // Default if not provided
    }

    public function processLargeDataset($filename, callable $processor) {
        try {
            // Validate and set memory limit
            if (!ini_set('memory_limit', $this->maxMemoryUsage)) {
                throw new Exception("Failed to set memory limit.");
            }

            if (!file_exists($filename)) {
                throw new Exception("File not found: $filename");
            }

            $handle = fopen($filename, 'r');
            if ($handle === false) {
                throw new Exception("Failed to open file: $filename");
            }

            $stats = [
                'processed_rows' => 0,
                'failed_rows' => 0,
                'start_time' => microtime(true),
                'memory_peak' => 0
            ];

            // Process file in chunks
            while (!feof($handle)) {
                try {
                    $chunk = [];
                    $count = 0;

                    // Build chunk
                    while ($count < $this->chunkSize && ($line = fgets($handle)) !== false) {
                        $decodedLine = json_decode($line, true);
                        if (json_last_error() === JSON_ERROR_NONE) {
                            $chunk[] = $decodedLine;
                        } else {
                            $this->logHandler->warning("Failed to decode line: $line");
                            $stats['failed_rows']++;
                        }
                        $count++;
                    }

                    // Process current chunk
                    $processor($chunk);

                    // Update statistics
                    $stats['processed_rows'] += count($chunk);
                    $stats['memory_peak'] = max($stats['memory_peak'], memory_get_peak_usage(true));

                    // Log progress
                    $this->logProgress($stats);

                    // Clean up
                    unset($chunk);
                    if ($stats['processed_rows'] % ($this->chunkSize * 10) === 0) {
                        gc_collect_cycles();
                    }
                } catch (Exception $e) {
                    $stats['failed_rows'] += count($chunk);
                    $this->logHandler->error("Chunk processing failed: " . $e->getMessage());
                    continue;
                }
            }

            fclose($handle);
            return $this->generateReport($stats);
        } catch (Exception $e) {
            throw new Exception("Dataset processing failed: " . $e->getMessage());
        }
    }

    private function logProgress(array $stats) {
        $memoryUsage = memory_get_usage(true) / 1024 / 1024;
        $timeElapsed = microtime(true) - $stats['start_time'];
        $rowsPerSecond = $stats['processed_rows'] / $timeElapsed;

        $this->logHandler->info(sprintf(
            "Processed %d rows | Memory: %.2f MB | Speed: %.2f rows/sec",
            $stats['processed_rows'],
            $memoryUsage,
            $rowsPerSecond
        ));
    }

    private function generateReport(array $stats) {
        return [
            'total_processed' => $stats['processed_rows'],
            'total_failed' => $stats['failed_rows'],
            'memory_peak_mb' => $stats['memory_peak'] / 1024 / 1024,
            'time_taken_sec' => microtime(true) - $stats['start_time']
        ];
    }
}

// Example of usage
class LogHandler {
    public function info($message) {
        echo "INFO: $message\n";
    }
    public function error($message) {
        echo "ERROR: $message\n";
    }
    public function warning($message) {
        echo "WARNING: $message\n";
    }
}
```

</details>

**Example of Use:**

```php
$logHandler = new LogHandler();
$processor = new ChunkedProcessor(1000, '512M', $logHandler);

$result = $processor->processLargeDataset(dirname(__FILE__) . '/large_data.json', function($chunk) {
    // Custom processing logic for each chunk
    foreach ($chunk as $data) {
        // Simulate processing (e.g., database insert, API call, etc.)
        echo "Processing: " . json_encode($data) . "\n";
    }
});

echo "\n\n";
print_r($result);
```

**Key Benefits:**

* Memory efficient: Only loads a small portion of data at a time
* Fault tolerant: Errors in one chunk don't affect others
* Progress tracking: Enables monitoring of long-running processes
* Resource management: Regular garbage collection prevents memory leaks

#### B. Generator Functions

Generators provide a memory-efficient way to iterate over large datasets by yielding values one at a time.

<details>

<summary>Example of Dataset Generator</summary>

```php
class DatasetGenerator {
    private $bufferSize;
    
    public function __construct($bufferSize = 8192) {
        $this->bufferSize = $bufferSize;
    }
    
    public function readLargeFile($filename) {
        $handle = fopen($filename, 'r');
        
        while (!feof($handle)) {
            $buffer = fread($handle, $this->bufferSize);
            $lines = explode("\n", $buffer);
            
            foreach ($lines as $line) {
                if (trim($line) !== '') {
                    yield json_decode($line, true);
                }
            }
        }
        
        fclose($handle);
    }
    
    public function processBatchedData($filename, $batchSize = 100) {
        $batch = [];
        
        foreach ($this->readLargeFile($filename) as $item) {
            $batch[] = $item;
            
            if (count($batch) >= $batchSize) {
                yield $batch;
                $batch = [];
            }
        }
        
        if (!empty($batch)) {
            yield $batch;
        }
    }
}
```

</details>

Example of Use:

```php
// Instantiate the DatasetGenerator with an optional buffer size
// You can set the buffer size here or use the default
$generator = new DatasetGenerator(4096);

// Specify the path to the large file
$filename = dirname(__FILE__) . '/large_data.json';

// Process the file in batches of 100 items
foreach ($generator->processBatchedData($filename, 100) as $batch) {
    // Perform processing on each batch (e.g., save to a database or further transform data)
    foreach ($batch as $item) {
        // Example of handling each item in the batch
        echo 'Processing item: ' . json_encode($item) . PHP_EOL;

        // Add your specific logic here, such as inserting data into a database or validating items
    }
}
```

### 2. Distributed Storage Systems

#### A. HDFS Integration

Hadoop Distributed File System (HDFS) is designed for storing very large files with streaming data access patterns.

<details>

<summary>Example of HDFS Manager</summary>

```php
class HDFSManager {
    private $webhdfsClient;
    private $configuration;
    
    public function __construct(array $config) {
        $this->configuration = $config;
        $this->webhdfsClient = new WebHDFSClient($config['namenode_host'], 
                                                $config['namenode_port']);
    }
    
    public function writeFile($localPath, $hdfsPath, $replication = 3) {
        try {
            // Create file with specified replication
            $response = $this->webhdfsClient->create($hdfsPath, [
                'overwrite' => 'true',
                'replication' => $replication,
                'blocksize' => $this->configuration['block_size']
            ]);
            
            // Stream file content
            $handle = fopen($localPath, 'r');
            while (!feof($handle)) {
                $chunk = fread($handle, 8192);
                $this->webhdfsClient->append($hdfsPath, $chunk);
            }
            fclose($handle);
            
            return [
                'status' => 'success',
                'path' => $hdfsPath,
                'size' => filesize($localPath)
            ];
            
        } catch (Exception $e) {
            throw new HDFSException("Failed to write file: " . $e->getMessage());
        }
    }
    
    public function readFile($hdfsPath, $processor) {
        try {
            $metadata = $this->webhdfsClient->getFileStatus($hdfsPath);
            $blockSize = $metadata['blockSize'];
            $fileSize = $metadata['length'];
            
            // Read file in blocks
            for ($offset = 0; $offset < $fileSize; $offset += $blockSize) {
                $data = $this->webhdfsClient->read($hdfsPath, $offset, $blockSize);
                $processor($data);
            }
            
        } catch (Exception $e) {
            throw new HDFSException("Failed to read file: " . $e->getMessage());
        }
    }
}
```

</details>

#### B. Object Storage Implementation

Example of a robust object storage system integration with features like multipart uploads and retry logic.

<details>

<summary>Example of Object Storage Manager</summary>

```php
class ObjectStorageManager {
    private $client;
    private $retryAttempts;
    private $retryDelay;
    
    public function __construct(S3Client $client, $retryAttempts = 3, $retryDelay = 1) {
        $this->client = $client;
        $this->retryAttempts = $retryAttempts;
        $this->retryDelay = $retryDelay;
    }
    
    public function uploadLargeFile($bucket, $key, $filepath) {
        try {
            // Initialize multipart upload
            $upload = $this->client->createMultipartUpload([
                'Bucket' => $bucket,
                'Key'    => $key,
                'ServerSideEncryption' => 'AES256'
            ]);
            
            $uploadId = $upload['UploadId'];
            $parts = [];
            
            // Calculate optimal part size
            $filesize = filesize($filepath);
            $partSize = max(
                ceil($filesize / 10000), // Ensure we don't exceed max parts
                5 * 1024 * 1024         // Minimum 5MB part size
            );
            
            // Upload parts with retry logic
            $handle = fopen($filepath, 'r');
            $partNumber = 1;
            
            while (!feof($handle)) {
                $partData = fread($handle, $partSize);
                
                $result = $this->retryOperation(function() use ($bucket, $key, $uploadId, $partNumber, $partData) {
                    return $this->client->uploadPart([
                        'Bucket'     => $bucket,
                        'Key'        => $key,
                        'UploadId'   => $uploadId,
                        'PartNumber' => $partNumber,
                        'Body'       => $partData
                    ]);
                });
                
                $parts[] = [
                    'PartNumber' => $partNumber,
                    'ETag'       => $result['ETag']
                ];
                
                $partNumber++;
            }
            
            fclose($handle);
            
            // Complete upload
            $result = $this->client->completeMultipartUpload([
                'Bucket'          => $bucket,
                'Key'             => $key,
                'UploadId'        => $uploadId,
                'MultipartUpload' => ['Parts' => $parts]
            ]);
            
            return $result;
            
        } catch (Exception $e) {
            // Abort upload on failure
            if (isset($uploadId)) {
                $this->client->abortMultipartUpload([
                    'Bucket'   => $bucket,
                    'Key'      => $key,
                    'UploadId' => $uploadId
                ]);
            }
            
            throw new ObjectStorageException("Upload failed: " . $e->getMessage());
        }
    }
    
    private function retryOperation(callable $operation) {
        $lastException = null;
        
        for ($attempt = 1; $attempt <= $this->retryAttempts; $attempt++) {
            try {
                return $operation();
            } catch (Exception $e) {
                $lastException = $e;
                if ($attempt < $this->retryAttempts) {
                    sleep($this->retryDelay * $attempt);
                }
            }
        }
        
        throw $lastException;
    }
}
```

</details>

### 3. Sharding Strategies

#### A. Range-Based Sharding

This strategy distributes data based on ranges of a key value.

<details>

<summary>Example of Range Shard Manager</summary>

```php
class RangeShardManager {
    private $shardMap;
    private $ranges;
    
    public function __construct(array $shardConfig) {
        $this->initializeShards($shardConfig);
    }
    
    private function initializeShards(array $config) {
        // Example config:
        // [
        //     ['range' => [0, 1000000], 'connection' => 'shard1'],
        //     ['range' => [1000001, 2000000], 'connection' => 'shard2'],
        // ]
        
        foreach ($config as $shard) {
            $this->ranges[] = $shard['range'];
            $this->shardMap[] = $this->getConnection($shard['connection']);
        }
    }
    
    public function getShardForKey($key) {
        foreach ($this->ranges as $index => $range) {
            if ($key >= $range[0] && $key <= $range[1]) {
                return $this->shardMap[$index];
            }
        }
        
        throw new ShardingException("No shard found for key: $key");
    }
    
    public function executeAcrossShards(callable $operation) {
        $results = [];
        
        foreach ($this->shardMap as $shard) {
            try {
                $results[] = $operation($shard);
            } catch (Exception $e) {
                // Log error but continue with other shards
                error_log("Shard operation failed: " . $e->getMessage());
            }
        }
        
        return $results;
    }
}
```

</details>

#### B. Consistent Hashing

Implements consistent hashing for better distribution and rebalancing.

<details>

<summary>Example of Consistent Hash Ring</summary>

```php
class ConsistentHashRing {
    private $nodes = [];
    private $positions = [];
    private $replicas = 64;
    
    public function addNode($node) {
        for ($i = 0; $i < $this->replicas; $i++) {
            $hash = $this->hash($node . $i);
            $this->positions[$hash] = $node;
        }
        
        ksort($this->positions);
        $this->nodes[] = $node;
    }
    
    public function removeNode($node) {
        for ($i = 0; $i < $this->replicas; $i++) {
            $hash = $this->hash($node . $i);
            unset($this->positions[$hash]);
        }
        
        $index = array_search($node, $this->nodes);
        unset($this->nodes[$index]);
    }
    
    public function getNode($key) {
        if (empty($this->positions)) {
            return null;
        }
        
        $hash = $this->hash($key);
        
        foreach ($this->positions as $position => $node) {
            if ($hash <= $position) {
                return $node;
            }
        }
        
        // Wrap around to first node
        return reset($this->positions);
    }
    
    private function hash($key) {
        return crc32($key);
    }
}
```

</details>

### 4. Data Processing Patterns

#### A. Map-Reduce Implementation

Custom implementation of map-reduce pattern for PHP.

<details>

<summary>Example of Map Reduce Framework</summary>

```php
<?php

/**
 * A comprehensive MapReduce Framework implementation for PHP
 * Handles large-scale data processing with parallel execution support
 */
class MapReduceFramework {
    private $mappers = [];
    private $reducer;
    private $combiner;
    private $partitioner;
    private $maxWorkers;
    private $logger;
    private $errorHandler;
    
    /**
     * MapReduceFramework constructor.
     * @param int $maxWorkers Maximum number of parallel workers
     * @param LoggerInterface $logger Logger implementation
     */
    public function __construct(
        int $maxWorkers = 4,
        LoggerInterface $logger = null
    ) {
        $this->maxWorkers = $maxWorkers;
        $this->logger = $logger ?? new NullLogger();
        $this->errorHandler = function($error) {
            $this->logger->error("MapReduce error: " . $error->getMessage());
            throw $error;
        };
    }

    /**
     * Add a mapper function to the processing pipeline
     * @param callable $mapper Function that takes a single input and returns key-value pairs
     * @return self
     */
    public function addMapper(callable $mapper): self {
        $this->mappers[] = $mapper;
        return $this;
    }

    /**
     * Set the reducer function
     * @param callable $reducer Function that processes grouped values for a key
     * @return self
     */
    public function setReducer(callable $reducer): self {
        $this->reducer = $reducer;
        return $this;
    }

    /**
     * Set a combiner for local aggregation before shuffle
     * @param callable $combiner Function to combine mapper outputs locally
     * @return self
     */
    public function setCombiner(callable $combiner): self {
        $this->combiner = $combiner;
        return $this;
    }

    /**
     * Set a custom partitioner for data distribution
     * @param callable $partitioner Function to determine partition for a key
     * @return self
     */
    public function setPartitioner(callable $partitioner): self {
        $this->partitioner = $partitioner;
        return $this;
    }

    /**
     * Process the input data through the MapReduce pipeline
     * @param Iterator|array $data Input data
     * @return array Processed results
     * @throws MapReduceException
     */
    public function process($data): array {
        try {
            $this->validateConfiguration();
            
            // Map Phase
            $this->logger->info("Starting Map phase");
            $mappedData = $this->executeMapPhase($data);
            
            // Combine Phase (optional)
            if ($this->combiner) {
                $this->logger->info("Starting Combine phase");
                $mappedData = $this->executeCombinePhase($mappedData);
            }
            
            // Shuffle Phase
            $this->logger->info("Starting Shuffle phase");
            $shuffledData = $this->executeShufflePhase($mappedData);
            
            // Reduce Phase
            $this->logger->info("Starting Reduce phase");
            $result = $this->executeReducePhase($shuffledData);
            
            $this->logger->info("MapReduce job completed successfully");
            return $result;
            
        } catch (Exception $e) {
            ($this->errorHandler)($e);
        }
    }

    /**
     * Execute the map phase, potentially in parallel
     * @param Iterator|array $data Input data
     * @return array Mapped data
     */
    private function executeMapPhase($data): array {
        $mappedData = [];
        $chunks = $this->chunkData($data, $this->maxWorkers);
        
        if ($this->maxWorkers > 1 && function_exists('pcntl_fork')) {
            $mappedData = $this->parallelMap($chunks);
        } else {
            $mappedData = $this->serialMap($chunks);
        }
        
        return $mappedData;
    }

    /**
     * Execute mapping in parallel using process forking
     * @param array $chunks Chunked input data
     * @return array Mapped results
     */
    private function parallelMap(array $chunks): array {
        $sharedMemKey = ftok(__FILE__, 'm');
        $sharedMem = shmop_open($sharedMemKey, "c", 0644, 1024 * 1024);
        
        $pids = [];
        $chunkResults = [];
        
        foreach ($chunks as $i => $chunk) {
            $pid = pcntl_fork();
            
            if ($pid == -1) {
                throw new MapReduceException("Failed to fork process");
            } elseif ($pid) {
                // Parent process
                $pids[] = $pid;
            } else {
                // Child process
                try {
                    $result = $this->serialMap([$chunk]);
                    shmop_write($sharedMem, serialize($result), $i * 1024);
                    exit(0);
                } catch (Exception $e) {
                    exit(1);
                }
            }
        }
        
        // Wait for all child processes
        foreach ($pids as $pid) {
            pcntl_waitpid($pid, $status);
            if (!pcntl_wifexited($status) || pcntl_wexitstatus($status) != 0) {
                throw new MapReduceException("Child process failed");
            }
        }
        
        // Collect results
        $mappedData = [];
        for ($i = 0; $i < count($chunks); $i++) {
            $data = unserialize(shmop_read($sharedMem, $i * 1024, 1024));
            $mappedData = array_merge($mappedData, $data);
        }
        
        shmop_delete($sharedMem);
        shmop_close($sharedMem);
        
        return $mappedData;
    }

    /**
     * Execute mapping serially
     * @param array $chunks Chunked input data
     * @return array Mapped results
     */
    private function serialMap(array $chunks): array {
        $mappedData = [];
        
        foreach ($chunks as $chunk) {
            foreach ($chunk as $item) {
                foreach ($this->mappers as $mapper) {
                    $result = $mapper($item);
                    if (!empty($result)) {
                        $mappedData = array_merge($mappedData, $result);
                    }
                }
            }
        }
        
        return $mappedData;
    }

    /**
     * Execute the combine phase for local aggregation
     * @param array $mappedData Mapped data
     * @return array Combined data
     */
    private function executeCombinePhase(array $mappedData): array {
        $combinedData = [];
        
        foreach ($mappedData as $item) {
            foreach ($item as $key => $value) {
                if (!isset($combinedData[$key])) {
                    $combinedData[$key] = [];
                }
                $combinedData[$key][] = $value;
            }
        }
        
        $result = [];
        foreach ($combinedData as $key => $values) {
            $result[$key] = ($this->combiner)($key, $values);
        }
        
        return $result;
    }

    /**
     * Execute the shuffle phase to group data by key
     * @param array $mappedData Mapped/Combined data
     * @return array Shuffled data
     */
    private function executeShufflePhase(array $mappedData): array {
        $shuffled = [];
        
        foreach ($mappedData as $item) {
            foreach ($item as $key => $value) {
                $partition = $this->getPartition($key);
                if (!isset($shuffled[$partition][$key])) {
                    $shuffled[$partition][$key] = [];
                }
                $shuffled[$partition][$key][] = $value;
            }
        }
        
        return $shuffled;
    }

    /**
     * Execute the reduce phase
     * @param array $shuffledData Shuffled data
     * @return array Reduced results
     */
    private function executeReducePhase(array $shuffledData): array {
        $result = [];
        
        foreach ($shuffledData as $partition => $data) {
            foreach ($data as $key => $values) {
                $result[$key] = ($this->reducer)($key, $values);
            }
        }
        
        return $result;
    }

    /**
     * Get partition for a key using the partitioner
     * @param mixed $key
     * @return int Partition number
     */
    private function getPartition($key): int {
        if ($this->partitioner) {
            return ($this->partitioner)($key, $this->maxWorkers);
        }
        return abs(crc32(serialize($key))) % $this->maxWorkers;
    }

    /**
     * Chunk input data for parallel processing
     * @param Iterator|array $data Input data
     * @param int $numChunks Number of chunks
     * @return array Chunked data
     */
    private function chunkData($data, int $numChunks): array {
        if (is_array($data)) {
            return array_chunk($data, ceil(count($data) / $numChunks));
        }
        
        $chunks = array_fill(0, $numChunks, []);
        $i = 0;
        
        foreach ($data as $item) {
            $chunks[$i % $numChunks][] = $item;
            $i++;
        }
        
        return $chunks;
    }

    /**
     * Validate MapReduce configuration
     * @throws MapReduceException
     */
    private function validateConfiguration(): void {
        if (empty($this->mappers)) {
            throw new MapReduceException("No mappers configured");
        }
        
        if (!$this->reducer) {
            throw new MapReduceException("No reducer configured");
        }
    }
}

/**
 * Custom exception for MapReduce operations
 */
class MapReduceException extends Exception {}

/**
 * Example usage:
 */
// Word count example
$wordCount = new MapReduceFramework();

// Add mapper to split text into words
$wordCount->addMapper(function($text) {
    $words = str_word_count(strtolower($text), 1);
    $result = [];
    foreach ($words as $word) {
        $result[] = [$word => 1];
    }
    return $result;
});

// Set reducer to sum counts
$wordCount->setReducer(function($key, $values) {
    return array_sum($values);
});

// Optional: Add combiner for local aggregation
$wordCount->setCombiner(function($key, $values) {
    return array_sum($values);
});

// Optional: Custom partitioner
$wordCount->setPartitioner(function($key, $numPartitions) {
    return abs(crc32($key)) % $numPartitions;
});

// Process sample data
$texts = [
    "Hello world hello",
    "World of MapReduce",
    "Hello MapReduce world"
];

$result = $wordCount->process($texts);
print_r($result);

// Example output:
// Array
// (
//     [hello] => 3
//     [world] => 3
//     [of] => 1
//     [mapreduce] => 2
// )

/**
 * Advanced Example: Log Analysis
 */
class LogAnalyzer {
    public static function analyze(array $logs) {
        $mapReduce = new MapReduceFramework(8); // Use 8 workers
        
        // Map each log entry to [ip_address => response_size]
        $mapReduce->addMapper(function($logLine) {
            if (preg_match('/(\d+\.\d+\.\d+\.\d+).*?(\d+)$/', $logLine, $matches)) {
                return [$matches[1] => (int)$matches[2]];
            }
            return [];
        });
        
        // Combine local results
        $mapReduce->setCombiner(function($key, $values) {
            return [
                'count' => count($values),
                'total_size' => array_sum($values)
            ];
        });
        
        // Reduce to get final statistics
        $mapReduce->setReducer(function($key, $values) {
            $totalCount = 0;
            $totalSize = 0;
            
            foreach ($values as $value) {
                $totalCount += $value['count'];
                $totalSize += $value['total_size'];
            }
            
            return [
                'ip' => $key,
                'requests' => $totalCount,
                'total_size' => $totalSize,
                'avg_size' => $totalSize / $totalCount
            ];
        });
        
        return $mapReduce->process($logs);
    }
}

// Example usage of log analyzer:
$logs = [
    '192.168.1.1 - - [01/Jan/2024:00:00:01] "GET /index.html HTTP/1.1" 200 1024',
    '192.168.1.2 - - [01/Jan/2024:00:00:02] "GET /image.jpg HTTP/1.1" 200 2048',
    '192.168.1.1 - - [01/Jan/2024:00:00:03] "GET /style.css HTTP/1.1" 200 512'
];

$stats = LogAnalyzer::analyze($logs);
print_r($stats);

// Example output:
// Array
// (
//     [192.168.1.1] => Array
//         (
//             [ip] => 192.168.1.1
//             [requests] => 2
//             [total_size] => 1536
//             [avg_size] => 768
//         )
//     [192.168.1.2] => Array
//         (
//             [ip] => 192.168.1.2
//             [requests] => 1
//             [total_size] => 2048
//             [avg_size] => 2048
//         )
// )
```



</details>

{% hint style="info" %}
To try this code yourself, install the example files from the official GitHub repository: [https://github.com/apphp/ai-with-php-examples](https://github.com/apphp/ai-with-php-examples)
{% endhint %}
