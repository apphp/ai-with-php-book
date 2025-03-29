# Sales Analyst Agent

### Coding Sales Analyst Agent in PHP

This agent can provide you following:

* Generate sales report
* Get sales analysis
* Get recommendations

#### **Step 1: Create Agent class**

For this example, let’s create `SalesAnalysisAgent` class.

<details>

<summary>SalesAnalysisAgent class</summary>

```php
namespace app\public\include\classes\llmagents\salesanalysis;

use app\public\include\classes\llmagents\salesanalysis\tools\GenerateSalesReportTool;
use app\public\include\classes\llmagents\salesanalysis\tools\AnalyzeSalesDataTool;
use app\public\include\classes\llmagents\salesanalysis\tools\ForecastFutureSalesTool;
use LLM\Agents\Agent\Agent;
use LLM\Agents\Agent\AgentAggregate;
use LLM\Agents\Solution\MetadataType;
use LLM\Agents\Solution\Model;
use LLM\Agents\Solution\SolutionMetadata;
use LLM\Agents\Solution\ToolLink;

final class SalesAnalysisAgent extends AgentAggregate {
    public const DEFAULT_MODEL = 'gpt-4o-mini';
    public const NAME = 'sales_analysis';

    public static function create(string $model = self::DEFAULT_MODEL): self {
        $agent = new Agent(
            key: self::NAME,
            name: 'Sales Analysis Agent',
            description: 'This agent specializes in analyzing sales data, identifying trends, and providing actionable insights to improve sales performance. It can process historical sales data, generate comprehensive reports, and forecast future sales based on existing patterns.',
            instruction: 'You are a sales analysis assistant. Your primary goal is to help users analyze their sales data and extract valuable insights. Use the provided tools to analyze sales data, generate detailed reports, and forecast future sales trends when appropriate. Always aim to provide clear, data-driven insights that can help users make informed business decisions and improve their sales strategies.',
        );

        $aggregate = new self($agent);

        $aggregate->addMetadata(
        // Instructions
            new SolutionMetadata(
                type: MetadataType::Memory,
                key: 'data_driven_analysis',
                content: 'Base all your analyses on the provided data. Avoid making assumptions without supporting evidence.',
            ),
            new SolutionMetadata(
                type: MetadataType::Memory,
                key: 'identify_patterns',
                content: 'Look for meaningful patterns and trends in the sales data, such as seasonal fluctuations, growth rates, and customer behavior patterns.',
            ),
            new SolutionMetadata(
                type: MetadataType::Memory,
                key: 'contextual_insights',
                content: 'Provide insights that consider the specific industry and business context when analyzing sales data.',
            ),
            new SolutionMetadata(
                type: MetadataType::Memory,
                key: 'actionable_recommendations',
                content: 'Always include actionable recommendations based on your analysis that users can implement to improve their sales performance.',
            ),
            new SolutionMetadata(
                type: MetadataType::Memory,
                key: 'explain_technical_terms',
                content: 'Provide clear explanations of technical terms and metrics for users who may not be familiar with advanced sales analytics concepts.',
            ),
            new SolutionMetadata(
                type: MetadataType::Memory,
                key: 'comprehensive_analysis',
                content: 'Consider multiple factors in your analysis, including product performance, customer segments, regional variations, and time-based trends.',
            ),

            // Prompts examples
            new SolutionMetadata(
                type: MetadataType::Prompt,
                key: 'basic_analysis',
                content: 'Can you analyze my quarterly sales data from the last 2 years?',
            ),
            new SolutionMetadata(
                type: MetadataType::Prompt,
                key: 'forecast_request',
                content: 'Based on my historical sales data, what should I expect for the upcoming holiday season?',
            ),

            new SolutionMetadata(
                type: MetadataType::Configuration,
                key: 'max_tokens',
                content: 4000,
            ),
        );

        $aggregate->addAssociation(
            new Model(model: $model)
        );

        // Abilities of the agent
        $aggregate->addAssociation(new ToolLink(name: GenerateSalesReportTool::NAME));
        $aggregate->addAssociation(new ToolLink(name: AnalyzeSalesDataTool::NAME));
        $aggregate->addAssociation(new ToolLink(name: ForecastFutureSalesTool::NAME));

        return $aggregate;
    }

    public function getInputParamDescription(): array {
        $descriptions = [
            'reportPath' => 'The path to report file',
        ];

        return $descriptions;
    }

    /**
     * Always require URL parameter for both tools
     * @param $properties
     * @param $required
     * @return void
     */
    public function addRequiredParams(&$properties, &$required) {
        // Always require URL parameter for both tools
        if (!isset($properties['reportPath'])) {
            $properties['reportPath'] = [
                'type' => 'string',
                'description' => 'The path to report file'
            ];
            $required[] = 'url';
        }
    }

    public function getRequiredArgument():string {
        return 'reportPath';
    }
}
```

</details>

#### **Step 2: Create Tools and A**ssociate Then With an **Agent**

Create `GenerateSalesReportTool`.\
This tool generates comprehensive sales reports based on provided data, time period, and report type.

<details>

<summary>GenerateSalesReportTool class</summary>

```php
namespace app\public\include\classes\llmagents\salesanalysis\tools;

use LLM\Agents\Tool\PhpTool;

/**
 * @extends PhpTool<GenerateSalesReportInput>
 */
final class GenerateSalesReportTool extends PhpTool {
    public const NAME = 'generate_sales_report';

    public function __construct() {
        parent::__construct(
            name: self::NAME,
            inputSchema: GenerateSalesReportInput::class,
            description: 'This tool generates comprehensive sales reports based on provided data, time period, and report type.',
        );
    }

    public function execute(object $input): string {
        // Validate input data path
        if (!\file_exists(APP_PATH . $input->reportPath)) {
            return \json_encode([
                'success' => false,
                'error' => 'Sales data file not found',
                'message' => "The file at path '" . APP_PATH . $input->reportPath . "' does not exist.",
            ]);
        }

        // Generate the appropriate report based on type
        $report = file_get_contents(APP_PATH . $input->reportPath);

        return \json_encode([
            'success' => true,
            'report_type' => $input->reportType ?? 'standard',
            'report_data' => $report,
        ]);
    }
}

```

</details>

Create `AnalyzeSalesDataTool`.\
This tool analyzes sales data to identify trends, patterns, and key insights that can help improve business performance.

Create `ForecastFutureSalesTool`.\
This tool forecasts future sales based on historical data using various forecasting models.

#### **Step 3: Associate Agent with Tools**

Associate `SalesAnalysisAgent` with these tools.

```php
$aggregate->addAssociation(new ToolLink(name: GenerateSalesReportTool::NAME));
$aggregate->addAssociation(new ToolLink(name: AnalyzeSalesDataTool::NAME));
$aggregate->addAssociation(new ToolLink(name: ForecastFutureSalesTool::NAME));
```

#### **Step 4: Run Code by Executor and Get Result**

Now we're ready to run agent and see the result.
