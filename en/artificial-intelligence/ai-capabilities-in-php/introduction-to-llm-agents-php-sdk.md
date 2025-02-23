# Introduction to LLM Agents PHP SDK

### What is LLM Agents PHP SDK?

The **LLM Agents PHP SDK** is a powerful library designed to help PHP developers create and manage **Language Model (LLM)-based agents**. These agents can perform complex tasks, interact with external tools, and respond intelligently to user input. The library provides an efficient framework to integrate AI capabilities into PHP applications, allowing for the development of **autonomous, AI-driven systems**.

Official documentation: [https://github.com/llm-agents-php/agents](https://github.com/llm-agents-php/agents)

<div align="left"><figure><img src="../../.gitbook/assets/ai-introduction-to-llm-agents-php-sdk-min.png" alt="" width="375"><figcaption><p>Introduction to LLM Agents PHP SDK</p></figcaption></figure></div>

This SDK is designed to work with any **LLM service or API**, making it highly flexible and adaptable to various AI use cases.

### Key Features

* **Agent Creation** – Build LLM-powered agents with customizable behaviors.
* **Tool Integration** – Connect agents with external APIs and services.
* **Memory Management** – Store and retrieve agent memory across interactions.
* **Prompt Handling** – Efficiently manage prompts and responses.
* **Extensible Architecture** – Easily add new agent types and capabilities.
* **Multi-Agent Support** – Enable collaboration between multiple agents.

### Installation

To install the SDK, use **Composer**:

```bash
composer require llm-agents/agents
```

### Creating an Agent

Here’s a basic example of defining an agent:

```php
use LLM\Agents\Agent\Agent;
use LLM\Agents\Agent\AgentAggregate;

class MyAgent extends AgentAggregate {
    public static function create(): self {
        $agent = new Agent(
            key: 'my_agent',
            name: 'My Custom Agent',
            description: 'An AI-powered agent that performs specific tasks.',
            instruction: 'Follow the provided guidelines strictly.'
        );

        return new self($agent);
    }
}
```

### Implementing a Custom Tool

Agents can use tools to perform specific actions. Here’s an example of a **site status checker tool**:

```php
use LLM\Agents\Tool\PhpTool;

class CheckSiteAvailabilityTool extends PhpTool {
    public const NAME = 'check_site_availability';

    public function execute(object $input): string {
        $ch = curl_init($input->url);
        curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
        curl_setopt($ch, CURLOPT_NOBODY, true);

        $startTime = microtime(true);
        curl_exec($ch);
        $endTime = microtime(true);

        $statusCode = curl_getinfo($ch, CURLINFO_HTTP_CODE);
        curl_close($ch);

        return json_encode([
            'status_code' => $statusCode,
            'response_time_ms' => round(($endTime - $startTime) * 1000, 2),
            'is_online' => $statusCode >= 200 && $statusCode < 400,
        ]);
    }
}
```

### Executing an Agent

To run an agent, use the **AgentExecutor**:

```php
use LLM\Agents\AgentExecutor\ExecutorInterface;

class AgentRunner {
    public function __construct(private ExecutorInterface $executor) {}

    public function run(string $input): string {
        return (string) $this->executor->execute('my_agent', $input)->result->content;
    }
}
```

### Conclusion

The **LLM Agents PHP SDK** simplifies the process of integrating AI into PHP applications. With its flexible architecture, developers can build intelligent agents that interact with tools, retain memory, and process complex tasks efficiently. Whether you're automating workflows or enhancing user experiences, this SDK provides a robust foundation for AI-driven applications in PHP.
