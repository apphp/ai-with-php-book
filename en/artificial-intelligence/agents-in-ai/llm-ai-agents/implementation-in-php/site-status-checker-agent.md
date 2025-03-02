# Site Status Checker Agent

### Coding Site Status Checker Agent in PHP

* Check if a site is up and running
* Dig up DNS info
* Run ping tests
* Give you the lowdown on why a site might be offline

#### **Step 1: Create Agent class**

For this example, let’s create `SiteStatusCheckerAgent` class.

<details>

<summary>SiteStatusCheckerAgent class</summary>

```php
namespace app\public\include\classes\llmagents\sitestatuschecker;

use app\public\include\classes\llmagents\sitestatuschecker\tools\CheckSiteAvailabilityTool;
use app\public\include\classes\llmagents\sitestatuschecker\tools\GetDnsInfoTool;
use app\public\include\classes\llmagents\sitestatuschecker\tools\PerformPingTestTool;
use LLM\Agents\Agent\Agent;
use LLM\Agents\Agent\AgentAggregate;
use LLM\Agents\Solution\MetadataType;
use LLM\Agents\Solution\Model;
use LLM\Agents\Solution\SolutionMetadata;
use LLM\Agents\Solution\ToolLink;

final class SiteStatusCheckerAgent extends AgentAggregate
{
    public const DEFAULT_MODEL = 'gpt-4o-mini';
    public const NAME = 'site_status_checker';

    public static function create(string $model = self::DEFAULT_MODEL): self
    {
        $agent = new Agent(
            key: self::NAME,
            name: 'Site Status Checker',
            description: 'This agent specializes in checking the online status of websites. It can verify if a given URL is accessible, retrieve basic information about the site, and provide insights on potential issues if a site is offline.',
            instruction: 'You are a website status checking assistant. Your primary goal is to help users determine if a website is online and provide relevant information about its status. Use the provided tools to check site availability, retrieve DNS information, and perform ping tests when necessary. Always aim to give clear, concise responses about a site\'s status and offer potential reasons or troubleshooting steps if a site appears to be offline.',
        );

        $aggregate = new self($agent);

        $aggregate->addMetadata(
        // Instructions
            new SolutionMetadata(
                type: MetadataType::Memory,
                key: 'describe_decisions',
                content: 'Before calling any tools, describe the decisions you are making and why you are making them.',
            ),
            new SolutionMetadata(
                type: MetadataType::Memory,
                key: 'check_availability_first',
                content: 'Always start by checking the site\'s availability before using other tools.',
            ),
            new SolutionMetadata(
                type: MetadataType::Memory,
                key: 'don_not_repeat',
                content: 'Don\'t repeat yourself. If you have already provided something, don\'t repeat it unless necessary.',
            ),
            new SolutionMetadata(
                type: MetadataType::Memory,
                key: 'offline_site_checks',
                content: 'If a site is offline, consider checking DNS information and performing a ping test to gather more data.',
            ),
            new SolutionMetadata(
                type: MetadataType::Memory,
                key: 'explain_technical_terms',
                content: 'Provide clear explanations of technical terms and status codes for users who may not be familiar with them.',
            ),
            new SolutionMetadata(
                type: MetadataType::Memory,
                key: 'suggest_troubleshooting',
                content: 'Suggest common troubleshooting steps if a site appears to be offline.',
            ),

            // Prompts examples
            new SolutionMetadata(
                type: MetadataType::Prompt,
                key: 'google',
                content: 'Check if google.com is online.',
            ),

            new SolutionMetadata(
                type: MetadataType::Prompt,
                key: 'offline_site',
                content: 'Can you check why buggregator.dev is offline?',
            ),

            new SolutionMetadata(
                type: MetadataType::Configuration,
                key: 'max_tokens',
                content: 3000,
            ),
        );

        $aggregate->addAssociation(
            new Model(model: $model)
        );

        // Abbilities of the agent
        $aggregate->addAssociation(new ToolLink(name: CheckSiteAvailabilityTool::NAME));
        $aggregate->addAssociation(new ToolLink(name: GetDnsInfoTool::NAME));
        $aggregate->addAssociation(new ToolLink(name: PerformPingTestTool::NAME));

        return $aggregate;
    }

    public function getInputParamDescription(): array {
        $descriptions = [
            'url' => 'The URL of the website to check',
            'timeout' => 'Maximum time in seconds to wait for response',
            'method' => 'HTTP method to use for the request',
            'headers' => 'Additional HTTP headers to send with the request',
            'followRedirects' => 'Whether to follow HTTP redirects',
            'maxRedirects' => 'Maximum number of redirects to follow',
            'verifySSL' => 'Whether to verify SSL certificates'
        ];

        return $descriptions;
    }

    /**
     * Always require URL parameter for both tools
     * @param $properties
     * @param $required
     * @return void
     */
    public function addRequiredParams(&$properties, &$required) {
        // Always require URL parameter for both tools
        if (!isset($properties['url'])) {
            $properties['url'] = [
                'type' => 'string',
                'description' => 'The URL of the website to check'
            ];
            $required[] = 'url';
        }
    }

    public function getRequiredArgument():string {
        return 'url';
    }
}

```



</details>

#### **Step 2: Create Tools and A**ssociate Then With an **Agent**

Create `CheckSiteAvailabilityTool`. \
This tool checks if a given URL is accessible and returns its HTTP status code and response time.

<details>

<summary>CheckSiteAvailabilityTool class</summary>

```php
namespace app\public\include\classes\llmagents\sitestatuschecker\tools;

use LLM\Agents\Tool\PhpTool;

/**
 * @extends PhpTool<CheckSiteAvailabilityInput>
 */
final class CheckSiteAvailabilityTool extends PhpTool {
    public const NAME = 'check_site_availability';

    public function __construct() {
        parent::__construct(
            name: self::NAME,
            inputSchema: CheckSiteAvailabilityInput::class,
            description: 'This tool checks if a given URL is accessible and returns its HTTP status code and response time.',
        );
    }

    public function execute(object $input): string {
        $ch = \curl_init($input->url);
        \curl_setopt_array($ch, [
            CURLOPT_RETURNTRANSFER => true,
            CURLOPT_HEADER => true,
            CURLOPT_NOBODY => true,
            CURLOPT_FOLLOWLOCATION => true,
            CURLOPT_MAXREDIRS => 10,
            CURLOPT_TIMEOUT => 30,
        ]);

        $startTime = \microtime(true);
        $response = \curl_exec($ch);
        $endTime = \microtime(true);

        $statusCode = \curl_getinfo($ch, CURLINFO_HTTP_CODE);
        $finalUrl = \curl_getinfo($ch, CURLINFO_EFFECTIVE_URL);
        $redirectCount = \curl_getinfo($ch, CURLINFO_REDIRECT_COUNT);
        $responseTime = \round(($endTime - $startTime) * 1000, 2);

        \curl_close($ch);

        $isOnline = $statusCode >= 200 && $statusCode < 400;

        return \json_encode([
            'status_code' => $statusCode,
            'response_time_ms' => $responseTime,
            'is_online' => $isOnline,
            'final_url' => $finalUrl,
            'redirect_count' => $redirectCount,
        ]);
    }
}
```

</details>

Create `GetDnsInfoTool`. \
This tool retrieves DNS information for a given domain, including IP addresses and name servers.

<details>

<summary>GetDnsInfoTool class</summary>

```php
namespace app\public\include\classes\llmagents\sitestatuschecker\tools;

use LLM\Agents\Tool\PhpTool;

/**
 * @extends PhpTool<GetDnsInfoInput>
 */
final class GetDnsInfoTool extends PhpTool {
    public const NAME = 'get_dns_info';

    public function __construct() {
        parent::__construct(
            name: self::NAME,
            inputSchema: GetDnsInfoInput::class,
            description: 'This tool retrieves DNS information for a given domain, including IP addresses and name servers.',
        );
    }

    public function execute(object $input): string {
        // Implement the actual DNS info retrieval here
        // This is a placeholder implementation
        $dnsRecords = \dns_get_record(str_ireplace(['https://', 'http://'], '', $input->domain), DNS_A + DNS_NS);

        $ipAddresses = \array_column(array_filter($dnsRecords, fn($record) => $record['type'] === 'A'), 'ip');
        $nameServers = \array_column(array_filter($dnsRecords, fn($record) => $record['type'] === 'NS'), 'target');

        return \json_encode([
            'ip_addresses' => $ipAddresses,
            'name_servers' => $nameServers,
        ]);
    }
}

```

</details>

Create `PerformPingTestTool`. \
This tool performs a ping test to a specified host and returns the results, including response times and packet loss.

<details>

<summary>PerformPingTestTool class</summary>

```php
namespace app\public\include\classes\llmagents\sitestatuschecker\tools;

use LLM\Agents\Tool\PhpTool;

/**
 * @extends PhpTool<PerformPingTestInput>
 */
final class PerformPingTestTool extends PhpTool {
    public const NAME = 'perform_ping_test';

    public function __construct() {
        parent::__construct(
            name: self::NAME,
            inputSchema: PerformPingTestInput::class,
            description: 'This tool performs a ping test to a specified host and returns the results, including response times and packet loss.',
        );
    }

    public function execute(object $input): string {
        // Implement the actual ping test here
        // This is a placeholder implementation
        $command = \sprintf('ping -c %d %s', 4, \escapeshellarg($input->host));
        \exec($command, $output, $returnVar);

        $packetLoss = 0;
        $avgRoundTripTime = 0;

        foreach ($output as $line) {
            if (str_contains($line, 'packet loss')) {
                \preg_match('/(\d+(?:\.\d+)?)%/', $line, $matches);
                $packetLoss = $matches[1] ?? 0;
            }

            if (str_contains($line, 'rtt min/avg/max')) {
                \preg_match('/= [\d.]+\/([\d.]+)\/[\d.]+/', $line, $matches);
                $avgRoundTripTime = $matches[1] ?? 0;
            }
        }

        return \json_encode([
            'packet_loss_percentage' => (float)$packetLoss,
            'avg_round_trip_time_ms' => (float)$avgRoundTripTime,
            'success' => $returnVar === 0,
        ]);
    }
}

```

</details>

#### **Step 3: Associate Agent with Tools**

Associate  `SiteStatusCheckerAgent` with these tools.

```php
$aggregate->addAssociation(new ToolLink(name: CheckSiteAvailabilityTool::NAME));
$aggregate->addAssociation(new ToolLink(name: GetDnsInfoTool::NAME));
$aggregate->addAssociation(new ToolLink(name: PerformPingTestTool::NAME));
```

#### **Step 4: Run Code by Executor and Get Result**

Now we're ready to run agent and see the result.

```php
use app\public\include\classes\llmagents\AiAgentExecutor;
use app\public\include\classes\llmagents\sitestatuschecker\SiteStatusCheckerAgent;

// Usage example:
try {
    // Initialize the checker
    $checker = new AiAgentExecutor(
        aiAgent: SiteStatusCheckerAgent::class,
        apiKey: OPEN_AI_KEY,
        model: 'gpt-4o-mini',
        finalAnalysis: false,
        debug: true
    );

    $url = 'https://aiwithphp.org';

    // Check a specific site with a question
    $result = $checker->execute(
        'URL to check: ' . $url . '\nQuestion: What is the current status of this site and are there any performance concerns?'
    );

    // Output debug results
    $agentDebug ??= '';
    $debugResult = '--';
    if ($agentDebug) {
        $debugLog = $checker->getDebugLog();
        foreach ($debugLog as $key => $message) {
            $debugResult .= humanize($key);
            $debugResult .= "\n=================\n";
            $debugResult .= $message . "\n\n";
        }
    }

    // Output the results
    echo "Site Status Analysis:\n";
    echo "URL: {$url}\n\n";

    // Show conversation history
    echo "Analysis Process:\n";
    foreach ($result['conversation_history'] as $message) {
        if (isset($message->functionCall)) {
            echo "Tool Called: {$message->functionCall->name}\n";
            echo "Arguments: {$message->functionCall->arguments}\n";
        } elseif (!empty($message->content)) {
            echo "\n&nbsp;\nAI: {$message->content}\n";
        }
        echo "\n";
    }

    if (!empty($result['final_analysis'])) {
        echo "\nFinal Analysis:\n{$result['final_analysis']}\n";
    }

} catch (\Exception $e) {
    echo 'Error: ' . $e->getFile() . ' | ' . $e->getLine() . "\n";
    echo 'Error: ' . $e->getMessage() . "\n";
}
```

Here the result:&#x20;

{% code overflow="wrap" %}
```markup
Site Status Analysis:
URL: https://aiwithphp.org

Analysis Process:
Tool Called: check_site_availability
Arguments: {"url":"https://aiwithphp.org"}

Tool Called: get_dns_info
Arguments: {"domain":"aiwithphp.org","url":"https://aiwithphp.org"}

Tool Called: perform_ping_test
Arguments: {"host":"14.12.33.145","url":"https://aiwithphp.org"}

  
### AI: Here's the current status and analysis of the site **https://aiwithphp.org**:
---

1. **Availability**: The site is online and accessible, with an HTTP status code of **200** indicating that the request was successful. The response time was approximately **217.49 ms**, which is generally acceptable for a good user experience.

2. **DNS Information**:
   - **IP Address**: The site is hosted on the IP address **54.36.31.145**.
   - **Name Servers**: The DNS is managed by the name servers **dns101.ovh.net** and **ns101.ovh.net**.

3. **Ping Test**: Although the site is online, the ping test to the IP address returned **0% packet loss**, but it reported a failure in completing the ping test. This could be due to various reasons, such as firewall settings or server configurations that prevent ICMP requests from being responded to.

### Summary
- The website is functioning well with acceptable performance metrics.
- The unsuccessful ping test might indicate that the server is configured to ignore ping requests, which is common for security reasons.

### Recommendations
- If you experience any issues accessing the site, consider checking your internet connection or trying a different network.
- If performance concerns arise, monitoring the response times over different periods could provide insights into any potential issues.
```
{% endcode %}

{% hint style="info" %}
To try this code yourself, install the example files from the official GitHub repository: [https://github.com/apphp/ai-with-php-examples](https://github.com/apphp/ai-with-php-examples)
{% endhint %}
