# Site Status Checker Agent

### Coding Site Status Checker Agent in PHP

This agent gives you a status of following:

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
use app\public\include\classes\llmagents\salesanalysis\SalesAnalysisAgent;

// Usage example:
try {
    // Initialize the checker
    $checker = new AiAgentExecutor(
        aiAgent: SalesAnalysisAgent::class,
        apiKey: OPEN_AI_KEY,
        model: 'gpt-4o-mini',
        finalAnalysis: false,
        debug: true
    );

    $reportPath = 'public/pages/ai-agents/llm-agents/data/IC-Weekly-Sales-Activity-Report-11538.csv';

    // Generate report
    $result = $checker->execute(
        'Generate sales report from report path: ' . $reportPath
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
    echo "Sales Analysis:\n";

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
Sales Analysis:
Analysis Process:
Tool Called: generate_sales_report
Arguments: {"reportPath":"public/pages/ai-agents/llm-agents/data/IC-Weekly-Sales-Activity-Report-11538.csv"}

Tool Called: analyze_sales_data
Arguments: {"reportPath":"public/pages/ai-agents/llm-agents/data/IC-Weekly-Sales-Activity-Report-11538.csv"}

Tool Called: forecast_future_sales
Arguments: {"reportPath":"public/pages/ai-agents/llm-agents/data/IC-Weekly-Sales-Activity-Report-11538.csv","forecastMethod":"linear","forecastPeriods":4,"timeUnit":"weeks","confidence":95,"seasonality":1}

### AI: The analysis of the sales data extracted from the provided report indicates the following key insights:
---

### Summary of Sales Activity
- **Total Sales:** $5,880,400
- **Total Orders:** 95 (total count of records)
- **Total Units Sold:** 13,304 units
- **Average Order Value:** This information wasn't available directly, but can be calculated as Total Sales divided by Total Orders, yielding approximately $61,000 per order.
- **Unique Customers:** No specific data was provided to determine unique customers.

### Detailed Insights
1. **Sales Performance by Region:**
   - **Northeast:** $983,750 (Variance: -$16,250)
   - **Southeast:** $1,066,300 (Variance: +$116,300)
   - **Midwest:** $734,350 (Variance: +$94,350)
   - **Southwest:** $1,665,000 (Variance: +$65,000)
   - **West Coast:** $1,431,000 (Variance: +$231,000)

   The **West Coast** showed the highest revenue and positive variance from targets, indicating effective sales strategies in that region. The **Southeast** also performed well above its target.

2. **Sales Performance by Sales Representative:**
   - **David Wilson** (West Coast) had the highest individual sales revenue at $1,431,000.
    - **Amanda Rodriguez** (Southwest) followed closely with $1,665,000.
    - **John Smith** (Northeast) and **Sarah Johnson** (Southeast) had mixed results, showing both successes and challenges.

3. **Product Breakdown:**
   - **Enterprise Solutions** contributed the most to revenue at $2,125,000.
    - **Mid-Market Solutions** and **Small Business Package** also made significant contributions, highlighting a diverse product portfolio.

### Recommendations
1. **Focus on High-Performing Regions:** Increase sales efforts in the West Coast and Southwest regions by leveraging successful strategies used there.

2. **Product Strategy:** Given that Enterprise Solutions contribute significantly to sales, consider enhancing marketing and sales tactics for this category to further capitalize on its success.

3. **Sales Training:** For representatives in regions underperforming against their targets, consider additional training or resources to help improve their sales techniques.

4. **Customer Acquisition:** Investigate strategies to attract new customers, particularly in regions like the Northeast, where performance has lagged.

5. **Data Collection Improvement:** To enable future forecasting, it's crucial to ensure more granular data is collected consistently, particularly on unique customers and more detailed sales metrics.

Unfortunately, due to insufficient time series data, forecasting future sales trends was not possible. To assist with forecasting in the future, ensure that historical sales data is collected over a sufficient timeline. 
```
{% endcode %}

{% hint style="info" %}
To try this code yourself, install the example files from the official GitHub repository: [https://github.com/apphp/ai-with-php-examples](https://github.com/apphp/ai-with-php-examples)
{% endhint %}
