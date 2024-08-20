---
title: PHP
icon: /docs/icons/php.svg
---

Run PHP code inside of your flow.

There isn't an official PHP plugin but we can use the `Shell` `Commands` task to execute arbitrary commands inside of a Docker container. We can also specify a container image that contains the necessary libraries to run the specific programming language.

In this example, we're using the Docker Task Runner with the `php:8.4-rc-alpine` image so that PHP can be executed.

```yaml file=public/examples/commands_php.yml
```

The contents of the `main.php` file contains a simple print statement:

```php
<?php
echo "Hello, World!";
?>
```

You'll need to add your PHP code using the Editor or [sync it using Git](../08.developer-guide/04.git.md) so Kestra can see it. You'll also need to set the `enabled` flag for the `namespaceFiles` property to `true` so Kestra can access the file.

You can also have the PHP code written inline using the `inputFiles` property.

```yaml file=public/examples/commands_php_inline.yml
```

You can read more about the Shell Commands type in the [Plugin documentation](/plugins/plugin-script-shell/tasks/io.kestra.plugin.scripts.shell.commands).

## Handling Outputs

If you want to get a variable or file from your PHP code, you can use an [output](../04.workflow-components/06.outputs.md).

### Variable Output

You can get the JSON outputs from the PHP script using the `::{}::` pattern. Here is an example:

```yaml file=public/examples/outputs_php.yml
```

All the output variables can be viewed in the Outputs tab of the execution.

![php_outputs](/docs/how-to-guides/php/outputs.png)

You can refer to the outputs in another task as shown in the example below:

```yaml file=public/examples/outputs_php_usage.yml
```

### File Output

Inside of your PHP code, write a file to the system. You'll need to add the `outputFiles` property to your flow and list the files you're trying to put out. In this case, we want to output `output.txt`. More information on the formats you can use for this property can be found [here](../08.developer-guide/07.scripts/08.output-directory.md).

The example below writes a `output.txt` file containing the "Hello World" text. We can then refer the file using the syntax `{{ outputs.{task_id}.outputFiles['<filename>'] }}`, and read the contents of the file using the `read()` function.

```yaml file=public/examples/scripts_output-files-php.yml
```

## Handling Metrics

You can also get [metrics](../08.developer-guide/07.scripts/07.outputs-metrics.md#outputs-and-metrics-in-script-and-commands-tasks) from your PHP code. We use the same pattern for defining metrics as we had used for outputs `::{}::`. In this example, we will demonstrate both the counter and timer metrics.

```yaml file=public/examples/metrics_php.yml
```

Once this has executed, both the metrics can be viewed under **Metrics**.

![php_metrics](/docs/how-to-guides/php/metrics.png)
