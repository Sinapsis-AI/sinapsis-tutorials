# Sinapsis CLI

The sinapsis CLI offers a mechanism to easily execute Sinapsis Agent definitions from a unix shell. Additionally, it includes functionalities to display useful information such as a Template attribute names, example Agent defintions for specified Templates or the list of all the installed templates in the current workspace.


## Initial steps

Please follow the installation process described in the [Sinapsis Installation](https://github.com/Sinapsis-ai/sinapsis?tab=readme-ov-file#installation) section. 

## Available commands
A summary of the available CLI commands and usage can be consulted in the 
[Sinapsis Quickstart](https://github.com/Sinapsis-ai/sinapsis?tab=readme-ov-file#quickstart) section.


## Usage

To verify the installation process was successful, please execute the following command:

```bash
sinapsis
```
If everything was correctly installed, the below welcome message should be displayed in the unix console:
```bash
Welcome to Sinapsis CLI. Please run 'sinapsis -h' to obtain information about available commands.
```
Add the flag `-h` or `--help` after the `sinapsis` command to get detailed information about sinapsis usage and available options  
```bash
sinapsis -h
```
Output:
```bash
usage: sinapsis [-h] {run,info} ...

Sinapsis CLI

options:
  -h, --help  show this help message and exit

subcommands:
  CLI commands: run, help

  {run,info}
    run       Run a Sinapsis agent with a yaml config file
    info      Get information about one or all Sinapsis template
```

## Sinapsis run
Sinapsis CLI provide the command `sinapsis run` to execute Agent definitions declared in a config file. Detailed usage information can be obtained using the `--help` or `-h` flags as follows
```bash
sinapsis run -h
```
This will produce the following usage example 
```bash
usage: sinapsis run [-h] [--enable-profiler] agent_config

positional arguments:
  agent_config

options:
  -h, --help         show this help message and exit
  --enable-profiler  Run agent in profiling mode
```

For instance, to run the [hello_world_agent](https://github.com/Sinapsis-ai/sinapsis/blob/main/src/sinapsis/configs/hello_world_agent.yml) config file use
```bash
sinapsis run src/sinapsis/configs/hello_world_agent.yml
```
Output:
```bash
2025-02-24 @ 14:44:31:663 | DEBUG |  hello_world_agent:__instantiate_templates:113 - Initialized template: InputTemplate
2025-02-24 @ 14:44:31:665 | DEBUG |  hello_world_agent:__instantiate_templates:113 - Initialized template: HelloWorld
2025-02-24 @ 14:44:31:665 | DEBUG |  hello_world_agent:__instantiate_templates:113 - Initialized template: DisplayHelloWorld
2025-02-24 @ 14:44:31:665 | DEBUG |  hello_world_agent:_log_agent_execution_order:128 - Execution Order
2025-02-24 @ 14:44:31:665 | DEBUG |  hello_world_agent:_log_agent_execution_order:131 - Order: <<0>>, template name: <<InputTemplate>>
2025-02-24 @ 14:44:31:665 | DEBUG |  hello_world_agent:_log_agent_execution_order:131 - Order: <<1>>, template name: <<HelloWorld>>
2025-02-24 @ 14:44:31:665 | DEBUG |  hello_world_agent:_log_agent_execution_order:131 - Order: <<2>>, template name: <<DisplayHelloWorld>>
2025-02-24 @ 14:44:31:665 | INFO |  hello_world_agent:_lazy_init:66 - Agent and templates initialized
2025-02-24 @ 14:44:31:665 | INFO |  hello_world_agent:signal_block_if_needed:167 - Signaling block mode for HelloWorld no: 2/3
2025-02-24 @ 14:44:31:666 | INFO |  hello_world_agent:signal_block_if_needed:167 - Signaling block mode for DisplayHelloWorld no: 3/3

Hello, this is my first template!

2025-02-24 @ 14:44:31:666 | INFO |  hello_world_agent:all_templates_finished:208 - All templates returned finished, stopping execution...
2025-02-24 @ 14:44:31:666 | DEBUG | .../sinapsis_core/cli/run_agent_from_config.py:run_agent_from_config:47 - result: DataContainer(container_id=a360a..., images=[], audios=[], texts=[TextPacket(content='Hello, this is my first template!', id='e86...', source='HelloWorld', modified_by_templates=['HelloWorld'], embedding=[], generic_data={}, annotations=None)], time_series=[], binary_data=[], generic_data={})
```
### Enable profiler
Additionally, to get the profiling times of the executed Agent and Templates the `optional` flag `--enable-profiler` can be used. For example, the following command
```bash
sinapsis run src/sinapsis/configs/hello_world_agent.yml --enable-profiler
```
will display the following execution times and statistics in the output console 
```bash
2025-02-24 @ 14:49:07:502 | INFO |  hello_world_agent:print_stats:77 - template: HelloWorld, average_time: 0.030, median: 0.030, num_executions: 1
2025-02-24 @ 14:49:07:502 | INFO |  hello_world_agent:print_stats:84 - Agent times: 0.174 median: 0.174
```

## Sinapsis info
Sinapis CLI offers through the `sinapsis info` command, a funtionality to display detailed information regarding Template Attributes, example Agent configs for specified Templates and the list of all the installed Templates in the working environment. 

To display the `sinapsis info` command example of usage and all the available options use the `--help` or `-h` flags
```bash
sinapsis info -h
```
Output:
```bash
usage: sinapsis info [-h] [--template TEMPLATE_NAME] [--all] [--all-template-names] [--example-template-config TEMPLATE_NAME]

options:
  -h, --help            show this help message and exit
  --template TEMPLATE_NAME   Template name to display info for
  --all                 Get info about all Sinapsis templates
  --all-template-names  Display all template names
  --example-template-config TEMPLATE_NAME
                        Display an example config for the specified template
```
### Display Template information
Use 
```bash
sinapsis info --template TEMPLATE_NAME
```
to display the following information:
* Path to the installed Template.
* Brief description of Template.
* Description of Template attributes. 

For example, to display the `HelloWorld` Template information run

```bash
sinapsis info --template HelloWorld
```
Output:
```bash
/path/to/sinapsis/templates/hello_world.py

HelloWorld: This template simply adds a text packet to our data container. The data container
is `sent` to any subsequent templates in our Agent.

HelloWorld attributes: 
        display_text                  <class 'str'>: default: Hello World
```

### Display ALL Templates info 
Use the following command to display Template information for ALL the templates installed in the working environment:
```bash
sinapsis info --all
```
Output:
```bash
----------------------------------------------------------------------
/path/to/sinapsis/templates/display_hello_world.py

DisplayHelloWorld: This template simply logs all the text packets received in a data container.
It is meant to be used for illustration purposes and in combination with
HelloWorld template.

DisplayHelloWorld attributes: 

----------------------------------------------------------------------
/path/to/sinapsis/templates/hello_world.py

HelloWorld: This template simply adds a text packet to our data container. The data container
is `sent` to any subsequent templates in our Agent.

HelloWorld attributes: 
        display_text                  <class 'str'>: default: Hello World

----------------------------------------------------------------------
```
### Display ALL Template names
Use 
```bash
sinapsis info --all-template-names
```
to display a list with all the available Template names installed in the working environment. By default, when installing Sinapsis the following templates should be displayed: 
```bash
CopyDataContainer
DisplayHelloWorld
HelloWorld
InputTemplate
MergeDataFlow
OutputTemplate
SplitDataFlow
TransferDataContainer
```
### Display example template config
The ```sinapis info``` command offers the option to build and display an example Agent config for an specified Template name using the ```--example_template_config``` flag as follows:  
```bash
sinapsis info --example-template-config TEMPLATE_NAME
```
For example, to display an Agent config for the ```HelloWorld``` template use:
```bash
sinapsis info --example-template-config HelloWorld
```
Output:
```bash
agent:
  name: my_test_agent
templates:
- template_name: InputTemplate
  class_name: InputTemplate
  attributes: {}
- template_name: HelloWorld
  class_name: HelloWorld
  template_input: InputTemplate
  attributes:
    display_text: Hello World
```


