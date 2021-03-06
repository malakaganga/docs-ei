# Sample 150: Introduction to Proxy Services


``` 
    <definitions xmlns="http://ws.apache.org/ns/synapse">
        <proxy name="StockQuoteProxy">
            <target>
                <endpoint>
                    <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
                </endpoint>
                <outSequence>
                    <send/>
                </outSequence>
            </target>
            <publishWSDL uri="file:samples/service-bus/resources/proxy/sample_proxy_1.wsdl"/>
        </proxy>
    </definitions>
```

## Prerequisites

-   Start the Axis2 server and deploy the
    `          SimpleStockQuoteService         ` if not already done.

Once ESB starts, you could go to
<http://localhost:8280/services/StockQuoteProxy?wsdl> and view the WSDL
generated for the Proxy Service defined in the configuration. This WSDL
is based on the source WSDL supplied in the Proxy Service definition and
is updated to reflect the Proxy Service EPR.

Execute the stock quote client by requesting for a stock quote on the
Proxy Service as follows:

1\. Stock Quote Client

This is a simple SOAP client that could send stock quote requests, and
receive and display the last sale price for a stock symbol.

The client is able to operate in the following modes, and send the
payloads listed below as SOAP messages:

-   `          quote         ` - sends a quote request for a single
    stock as follows. The response contains the last sales price for the
    stock which would be displayed.

```
    <m:getQuote xmlns:m="http://services.samples/xsd">
        <m:request>
            <m:symbol>IBM</m:symbol>
        </m:request>
    </m:getQuote>
```

-   `          customquote         ` - sends a quote request in a custom
    format. The ESB would transform this custom request into the
    standard stock quote request format and send it to the service. Upon
    receipt of the response it would be transformed again to a custom
    response format and returned to the client, which will then display
    the last sales price.

``` 
    <m0:checkPriceRequest xmlns:m0="http://services.samples/xsd">
        <m0:Code>symbol</m0:Code>
    </m0:checkPriceRequest>
```

-   `          fullquote         ` - gets quote reports for the stock
    over a number of days (i.e. last 100 days of the year).

``` 
    <m:getFullQuote xmlns:m="http://services.samples/xsd">
        <m:request>
            <m:symbol>IBM</m:symbol>
        </m:request>
    </m:getFullQuote>
```

-   `          placeorder         ` - places an order for stocks using a
    one way request

``` 
    <m:placeOrder xmlns:m="http://services.samples/xsd">
        <m:order>
            <m:price>3.141593E0</m:price>
            <m:quantity>4</m:quantity>
            <m:symbol>IBM</m:symbol>
        </m:order>
    </m:placeOrder>
```

-   `          marketactivity         ` - gets a market activity report
    for the day (i.e. quotes for multiple symbols)

```
    <m:getMarketActivity xmlns:m="http://services.samples/xsd">
        <m:request>
            <m:symbol>IBM</m:symbol>
            ...
            <m:symbol>MSFT</m:symbol>
        </m:request>
    </m:getMarketActivity>
```

!!! Info
    See `         samples/axis2Client/src/samples/common/StockQuoteHandler.java        ` for sample responses expected by the clients.


``` java
ant stockquote -Daddurl=http://localhost:8280/services/StockQuoteProxy
```

An `         inSequence        ` or `         endpoint        ` or both
of these would decide how the message would be handled after the Proxy
Service receives the message. In the above example the request received
is forwarded to the sample service hosted on Axis2. The
`         outSequence        ` defines how the response is handled
before it is sent back to the client. By default, a Proxy Service is
exposed over all transports configured for ESB, unless these are
specifically mentioned through the `         transports        `
attribute.
