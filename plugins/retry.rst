Retry Plugin
============

The ``RetryPlugin`` can automatically attempt to re-send a request that failed, to work around
unreliable connections and servers. It relies on errors to throw exceptions, so you need to
place something like the :doc:`ErrorPlugin <error>` later in the plugin chain::

    use Http\Discovery\HttpClientDiscovery;
    use Http\Client\Common\PluginClient;
    use Http\Client\Common\Plugin\ErrorPlugin;
    use Http\Client\Common\Plugin\RetryPlugin;

    $pluginClient = new PluginClient(
        HttpClientDiscovery::find(),
        [
            new RetryPlugin(),
            new ErrorPlugin(),
        ]
    );

.. warning::

    You should keep an eye on retried requests, as they add overhead. If a
    request fails due to a client side mistake, retrying is only a waste of
    time and resources.

Contrary to the :doc:`redirect`, the retry plugin does not restart the chain
but simply tries again from the current position.

Async
-----

This plugin is not fully compatible with asynchronous behavior, as the wait between retries is done 
with a blocking call to a sleep function.

Options
-------

``retries``: int (default: 1)

Number of retry attempts to make before giving up.
