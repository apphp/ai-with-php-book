# Additional Notes

### Ready to Use Environment

You may install ready-to-use docker environment with examples:\
[https://github.com/apphp/ai-with-php-examples](https://github.com/apphp/ai-with-php-examples)

### Additional Notes on PHP-ML and Rubix ML

1. PHP-ML:
   * Provides a wide range of machine learning algorithms and preprocessing tools.
   * Suitable for smaller datasets and simpler machine learning tasks.
   * Easy to use with a straightforward API.
2. Rubix ML:
   * Offers a more comprehensive set of machine learning algorithms and tools.
   * Better performance for larger datasets and more complex tasks.
   * Provides more advanced features like cross-validation and hyperparameter tuning.

When choosing between the two libraries, consider the complexity of your tasks, the size of your datasets, and the specific algorithms you need. For many projects, you may find yourself using both libraries to leverage their respective strengths.

### Troubleshooting

If you encounter any issues with the installation or use of these libraries, here are a few steps you can take:

1. Ensure all required PHP extensions are installed. You may need to modify the Dockerfile to include additional extensions required by Rubix ML.
2. Check the versions of the libraries in your `composer.json` file and update them if necessary.
3. If you're working with large datasets, you might need to increase the memory limit in your PHP configuration.
4. For Rubix ML, you might need to install additional system libraries. Add the following if you encounter issues:

For direct installation (run in terminal)

```
apt-get install -y libopenblas-dev liblapacke-dev && docker-php-ext-install gd
```

For Docker (in Dockerfile)

```dockerfile
RUN apt-get install -y libopenblas-dev liblapacke-dev && docker-php-ext-install gd
```
