# Site Status Checker Agent

### Coding Site Status Checker Agent in PHP

* Check if a site is up and running
* Dig up DNS info
* Run ping tests
* Give you the lowdown on why a site might be offline

#### **Step 1: Create Agent class**

For this example, let’s create SiteStatusCheckerAgent class.

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

Create `GetDnsInfoTool`.&#x20;

\>>>>>>>>>>>



{% hint style="info" %}
To try this code yourself, install the example files from the official GitHub repository: [https://github.com/apphp/ai-with-php-examples](https://github.com/apphp/ai-with-php-examples)
{% endhint %}
